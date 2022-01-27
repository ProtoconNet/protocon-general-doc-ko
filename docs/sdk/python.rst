===================================================
Python
===================================================

| 이 페이지는 Python으로 작성된 SDK에 관한 문서입니다.

| 지원하는 모델은 다음과 같습니다

* Mitum Currency
* Mitum Blocksign

| 이 문서는 Mitum Currency의 operation에 대해서만 소개하고 있습니다.

| Mitum Blocksign의 operation 생성 방법 혹은 Mitum Currency operation 생성에 관한 자세한 내용은 `mitum-py-util <https://github.com/ProtoconNet/mitum-py-util>`_ 을 참고해주세요.
---------------------------------------------------
Get Started
---------------------------------------------------

Prerequisite and Requirements
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 이 패키지는 다음 환경에서 개발되었습니다.

.. code-block:: shell

    $ python --version
    Python 3.9.2

| ``python 3.9 or later`` 이 요구됩니다.

Installation
'''''''''''''''''''''''''''''''''''''''''''''''''''

* git을 사용하면,

.. code-block:: shell

    $ git clone https://github.com/ProtoconNet/mitum-py-util.git

    $ cd mitum-py-util

    $ python setup.py install

| setup.py이 제대로 작동하지 않는다면, setup.py 실행 전 필요한 패키지를 requirements.txt를 사용하여 설치해주세요.

.. code-block:: shell

    $ pip install -r requirements.txt

---------------------------------------------------
Make Your First Operation
---------------------------------------------------

| 이 예제는 **mitum-py-util** 로 ``create-account`` operation을 생성하는 방법에 대해 설명합니다. 

| ``key-updater`` 와 ``transfer`` operation 생성 방법은 이 파트 마지막의 **Support Operations** 에서 확인하세요.

Get Available Account
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 시작하기 전에, 네트워크에 등록된 계정을 가지고 있어야 합니다.

| Mitum Currency에서는 오직 이미 존재하는 계정만이 블록에 저장될 수 있는 새로운 operation을 만들 수 있습니다.

| 계정은 다음과 같은 요소들로 이루어져있습니다.

.. code-block:: none

    1. pairs of (public key, weight); aka `keys`
    - public key has suffix `mpu`
    - The range of each weight should be in 1 <= weight <= 100
    - If an account have single public key, the account is called 'single-sig account', or it's called 'multi-sig account'
    
    2. threshold
    - The range of threshold should be in 1 <= threshold <= 100
    - The sum of all weights of the account should be over or equal to threshold

| 아직 아무 계정도 가지고 있지 않다면 다른 계정에 당신의 첫 계정을 만들어 달라 요청해야 합니다.
| **Details - Get Mitum Keypair** 파트에서 계정을 위한 키페어를 생성할 수 있습니다.
| (public key, weight)쌍과 threshold를 새 계정 생성을 도와줄 계정 보유자에게 전달하세요.

| 서명을 위해 계정의 각 공개키에 상응하는 개인키를 기억하고 있어야 합니다. 다른 사람에게 개인키를 알려주지 마세요!
| 물론 계정 주소 또한 ``sender`` 로 사용해야 하기 때문에 기억하고 있어야 합니다.

| 등록되지 않은 계정으로도 operation을 생성할 수는 있지만 해당 operation들은 브로드캐스팅 이후 처리 거부될 것입니다.

| 이제 첫 operation을 만들기 위해 다음 장으로 이동하세요.

Create Generator
'''''''''''''''''''''''''''''''''''''''''''''''''''

| operation의 대부분의 요소는 ``Generator`` 로 생성합니다.
| Mitum Currency에 대해서는 ``Generator.currency`` 를 사용하세요.

| ``Generator`` 를 선언할 때, ``network id`` 가 필요합니다.
| ``network id`` 는 네트워크에 따라 다릅니다.

| 이 페이지에서는 ``mitum`` 을 네트워크 id로 가정합니다.

.. code-block:: python

    from mitumc import Generator

    networkId = 'mitum'
    generator = Generator('mitum')
    currencyGenerator = generator.currency

| ``Generator`` 에 대한 더 자세한 내용은 Details - Major Classes 로 이동하여 Generator를 참고하세요.

| 또한, 네트워크 상에서 사용할 수 있는 등록된 계정을 가지고 있어야 합니다.

| 이제 새로운 operation을 만들기 위한 준비가 끝났습니다.

Create Operation Item
'''''''''''''''''''''''''''''''''''''''''''''''''''

| operation이 실행해야할 모든 것은 operation이 아닌 operation fact에 들어있습니다.
| fact는 ``sender``, ``token`` 등의 기본적인 정보를 담고 있습니다.

| 사실, 실제 operation의 지시 사항은 그 중에서도 Item에 들어있습니다.
| 한 마디로, operation을 위해 item들을 먼저 생성해야 한다는 뜻입니다.

| 아래 조건에 따라 계정을 생성하려 하는 상황이라고 가정해봅시다.

.. code-block:: none

    1. The keys and threshold of the account will be,
        - keys(public key, weight): (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 50), (pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu, 50) 
        - threshold: 100

    2. The initial balance of the account will be,
        - balance(currency id, amount): (MCC, 10000), (PEN, 20000)

| 계정이 가지고 있는 키의 수가 2 개이기 때문에, 새로운 계정은 multi-sig 계정이 될 것입니다.

| 새 계정에 대한 모든 조건이 결정되었으면 아래와 같이 item을 생성하세요.

.. code-block:: python

    key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50) # key(public key, weight)
    key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50)
    keys = currencyGenerator.createKeys([key1, key2], 100) # createKeys(keyList, threshold)

    amount1 = currencyGenerator.amount(10000, 'MCC') # amount(amount, currency id)
    amount1 = currencyGenerator.amount(20000, 'PEN')
    amounts = currencyGenerator.createAmounts([amount]) # createAmounts(amountList)

    createAccountsItem = currencyGenerator.createCreateAccountsItem(keys, amounts)

* 우선, ``Generator.currency.key(public key, weight)`` 를 사용해 각 key를 생성합니다..
* 다음으로 모든 키와 계정 threshold를 ``Generator.currency.createKeys(key list, threshold)`` 로 결합합니다.
* 그리고, ``Generator.currency.amount(amount, currencyId)`` 를 사용해 각 amount를 생성합니다..
* 다음 ``Generator.currency.createAmounts(amount list)`` 로 모든 amount를 결합합니다.
* 마지막으로, ``Generator.currency.createCreateAccountsItem(keys, amounts)`` 를 사용해 item을 생성하세요.

| 물론 각 item의 내용을 다음 조건 하에서 사용자화 할 수 있습니다.

.. code-block:: none

    - `createKeys`를 사용하여 생성하는 `Keys`는 key를 10개까지 포함할 수 있습니다.
    - item 당 최대 10개의 amount를 가질 수 있기 때문에 `createAmounts`의 amount list에는 amount를 10개까지 넣을 수 있습니다.
    - 게다가, `fact`는 item을 여러 개 포함할 수 있습니다. fact 당 item 개수는 최대 10 개입니다.

Create Operation Fact
'''''''''''''''''''''''''''''''''''''''''''''''''''

| *fact*는 반드시 ``items``, ``sender``, ``token``, ``fact hash`` 를 가져야 합니다.

| ``token`` 와 ``fact hash`` 는 SDK가 자동적으로 생성해주므로 걱정하지 않아도 됩니다.
| 반드시 제공해야할 정보는 ``items`` 와 ``sender`` 에 대한 것입니다.

| item을 생성하는 방법은 바로 위에서 설명하였습니다.

| 아래 조건을 만족할 수 있는 계정만 ``sender`` 로 사용할 수 있다는 것을 명심하세요.

.. code-block:: none

    1. 이미 생성되어 등록된 계정.
    2. item의 각 amount에 대해 충분한 잔액을 보유한 계정.
    3. 계정의 공개키에 상응하는 개인키(멀티 시그 계정인 경우 모든 개인키들 중 일부)를 알고 있는 계정.

| 그리고 다음과 같이 fact를 생성하세요!

.. code-block:: python

    senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca" # sender's account address; replace with your address
    createAccountsFact = currencyGenerator.createCreateAccountsFact(senderAddress, [createAccountsItem]) # createCreateAccountsFact(sender's address, item list)

| 만약 다수의 item을 가진 fact를 생성하고 싶다면 ``Generator.currency.createCreateAccountsFact(sender's address, item list)`` 의 item list에 item을 모두 넣으세요.

Create Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 드디어 operation을 생성하기 위한 단계에 도달하였습니다!

| 준비해야 하는 것은 오직 sender의 개인키입니다. 개인키는 fact에 서명하기 위해 필요합니다.
| 개인키의 서명은 fact 서명으로서 ``fact_signs``에 추가됩니다.
| ``fact_signs``의 모든 signer의 weight들의 총합이 ``sender`` 의 threshold 이상이어야 합니다.

| fact_sign에는 오직 ``sender`` 의 개인키의 서명만이 유효합니다. 

| operation에는 ``memo`` 값이 존재하지만 필수적이지는 않습니다. 필요한 내용을 넣어도 괜찮지만 ``memo`` 또한 ``operation hash`` 값에 영향을 미치기 때문에 주의해야 합니다.

| 이 예제에서는 ``sender`` 가 single-sig 계정이라고 가정합니다. 즉, sender의 계정에는 오직 하나의 키 밖에 없습니다.
| 만약 ``sender`` 가 multi-sig 계정이라면 ``fact_signs`` 에 여러 개의 서명을 추가해야 할 수 있습니다.
| 어떤 키들이 반드시 서명해야 하는지는 계정의 threshold와 각 key의 weight에 달렸습니다.

.. code-block:: python

    senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr" # sender's private key; replace with your private key
    
    createAccounts = generator.createOperation(createAccountsFact, "") # createOperation(fact, memo)
    createAccounts.addFactSign(senderPrivateKey); # addFactSign(private key) add fact signature to fact_signs 

| operation을 생성하기 위해 ``Generator.currency.createOperation(fact, memo)`` 가 아닌 ``Generator.createOperation(fact, memo)`` 을 사용해야 한다는 점에 주의하세요.

| 아쉽지만 하나의 operation에는 하나의 fact만 넣을 수 있습니다.

Create Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 사실 ``operation`` 자체로도 계정을 생성하는 데는 충분합니다.

| 하지만 종종 여러 개의 operation을 seal로 감싸 전송해야 할 일이 있을 수 있습니다. - 여러 개의 각각 다른 계정으로부터 하나의 계정으로 동시에 송금하는 경우 등

| 위에 언급한대로 하나의 seal은 여러 개의 operation을 가질 수 있습니다.

| seal에 넣을 수 있는 operation의 최대 개수는 노드 정책에 따라 다를 수 있습니다.
| 따라서 seal을 생성하기 전 하나의 seal에 몇 개의 operation을 넣을 수 있는지 확인해야 합니다.

| 어쨌든 mitum-py-util을 사용해 seal을 생성하는 것은 간단합니다.

| 준비해야 하는 것은 Mitum 키 패키지로부터 얻은 아무 개인키입니다.
| *mpr* 타입 접미사가 붙은 어떤 *btc compressed wif* 형식 키라도 가능합니다.

.. code-block:: python

    signKey = "L1V19fBjhnxNyfuXLWw6Y5mjFSixzdsZP4obkXEERskGQNwSgdm1mpr"

    operations = [createAccounts]
    seal = generator.createSeal(signKey, operations)

| ``createOperation`` 의 경우와 같이, 단순히 ``Generator.createSeal(signer, operation list)`` 를 사용하세요.

| 감싸길 원하는 모든 operation을 operation list에 추가하세요.

Support Operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 이 파트에서는 각 operation에 대한 코드 예제를 제공합니다.

| mitum-py-util가 지원하는 Mitum Currency operation은 다음과 같습니다.

* Create Account
* Key Updater
* Transfer

Create Account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``create-account`` 의 예제는 이미 설명했으나 여기서 하나의 코드 블록으로 다시 한 번 소개합니다.

| 새 계정을 생성하기 위해 다음과 같은 것을 준비해야 합니다.

* 새로운 계정의 정보: (public key, weight)쌍과 threshold로 이루어진 계정 keys, (currency id, amount) 쌍으로 이루어진 계정 초기 잔액
* 이미 존재하는 sender의 계정 - 특히 계정 주소와 개인키를 알아야 합니다.

| 이전에 설명한대로 어떤 개인키가 서명해야 하는지는 threshold와 weight들의 구성에 달렸습니다.

.. code-block:: python

    from mitumc import Generator

    senderPrivateKey = "L1V19fBjhnxNyfuXLWw6Y5mjFSixzdsZP4obkXEERskGQNwSgdm1mpr"
    senderAddress = "5fbQg8K856KfvzPiGhzmBMb6WaL5AsugUnfutgmWECPbmca"

    generator = Generator('mitum')
    gn = generator.currency

    key = gn.key("2177RF13ZZXpdE1wf7wu5f9CHKaA2zSyLW5dk18ExyJ84mpu", 100)
    keys = gn.createKeys([key], 100)

    amount = gn.amount(100, 'MCC')
    amounts = gn.createAmounts([amount])

    createAccountsItem = gn.createCreateAccountsItem(keys, amounts)
    createAccountsFact = gn.createCreateAccountsFact(srcAddr, [createAccountsItem])

    createAccounts = generator.createOperation(createAccountsFact, "")
    createAccounts.addFactSign(srcPriv)

| 자세한 설명은 생략합니다. 'Make Your First Operation'의 시작 부분을 확인하세요.

Key Updater
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 이 operation은 말 그대로 계정의 키를 업데이트 하기 위한 것입니다.

| 예를 들어,

.. code-block:: none

    - I have an single sig account with keys: (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 100), threshold: 100
    - But I want to replace keys of the account with keys: (22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu, 50), (22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu, 50), threshold: 100
    - Then you can use key-updater operation to reach the goal!

| single-sig 계정을 multi-sig로 바꾸거나 반대로 multi-sig에서 single-sig로 바꿀 수 있을까요?

| 물론 가능합니다!

| 계정 키를 업데이트하기 위해서 다음과 같은 것을 준비해야 합니다.

* 키를 교체하고자 하는 계정(target)의 정보 - 계정 주소와 개인키; 어떤 개인키가 필요한지는 threshold와 키 weight들에 따라 다를 수 있습니다.
* 새로운 keys: (public key, weights)쌍들과 threshold
* 수수료를 지불하려는 currency의 충분한 잔액

| ``create-account`` 와 ``transfer`` 는 ``item`` operation 생성을 위해 item을 만들어야 하지만 ``key-updater`` 는 item이 필요하지 않습니다.
| 바로 fact를 만드세요.

.. code-block:: python

    from mitumc import Generator

    targetPrivateKey = "KzejtzpPZFdLUXo2hHouamwLoYoPtoffKo5zwoJXsBakKzSvTdbzmpr"
    targetAddress = "JDhSSB3CpRjwM8aF2XX23nTpauv9fLhxTjWsQRm9cJ7umca"

    generator = Generator('mitum')
    gn = generator.currency

    key1 = gn.key("22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu", 50)
    key2 = gn.key("22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu", 50)
    keys = gn.createKeys([key1, key2], 100)

    keyUpdaterFact = gn.createKeyUpdaterFact(targetAddress, keys, "MCC") # createKeyUpdaterFact(target address, new keys, currency id for fee)

    keyUpdater = generator.createOperation(keyUpdaterFact, "")
    keyUpdater.addFactSign(targetPrivateKey)

* 계정의 키를 업데이트한 후에는 이전의 키를 사용할 수 없게 됩니다. 계정의 새로운 키페어의 개인키로 서명해야 합니다.
* 따라서 네트워크에 key-updater operation을 전송하기 전, 새로운 키들을 기록해두세요.
* 
Transfer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 드디어 다른 계정으로 토큰을 송금할 수 있습니다!

| 다른 operation들과 같이, 다음과 같은 것들을 준비해야 합니다.

* sender의 계정 정보 - 계정 주소와 개인키
* 송금할 (currency id, amount) 쌍

| ``create-account`` 처럼 fact 생성 전 item을 먼저 만들어야 합니다.

| operation을 전송하기 전 전송하려는 토큰의 잔액이 충분한지 먼저 확인하세요.

| 시작하기 전, 다음과 같이 토큰을 전송하려 한다고 가정해 봅시다.

* 1000000 MCC token
* 15000 PEN token

| 그리고 receiver는,

* CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca

| 최대 10 (currency id, amount) 쌍이 item 하나에 들어갈 수 있습니다.
| 또한 최대 10개의 item이 한 fact에 들어갈 수 있습니다. 하지만 각 item의 receiver는 달라야 합니다.

.. code-block:: python

    from mitumc import Generator

    generator = Generator('mitum')
    gn = generator.currency

    senderPrivateKey = "KzdeJMr8e2fbquuZwr9SEd9e1ZWGmZEj96NuAwHnz7jnfJ7FqHQBmpr"
    senderAddress = "2D5vAb2X3Rs6ZKPjVsK6UHcnGxGfUuXDR1ED1hcvUHqsmca"
    receiverAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"

    amount = gn.amount(1000000, 'MCC')
    amount = gn.amount(15000, 'PEN')
    amounts = gn.createAmounts([amount1, amount2])

    transfersItem = gn.createTransfersItem(receiverAddress, amounts) # createTransfersItem(receiver address, amounts)
    transfersFact = gn.createTransfersFact(senderAddress, [transfersItem]) # createTransfersFact(sender addrewss, item list)

    transfers = generator.createOperation(transfersFact, "")
    transfers.addFactSign(senderPrivateKey)  

| Mitum Blocksign 등 mitum-py-util이 지원하는 다른 operation이 더 있지만 이 문서에서는 설명하지 않습니다.
| 필요하다면 `README <https://github.com/ProtoconNet/mitum-py-util/blob/master/README.md>`_ 을 참고하세요.

---------------------------------------------------
Sign
---------------------------------------------------

| operation이 정상적으로 블록에 저장되기 위해서는 operation의 서명들이 특정 조건을 만족해야 합니다.

| 주의해야할 점은,

* 모든 서명이 계정의 개인키의 서명인가요?
* 각 signer의 weight들을 모두 합한 값이 계정의 threshold 이상인가요?

| 물론, 각 operation이 지켜야 할 다른 조건들이 더 있습니다. 하지만 여기서는 (fact)서명에만 집중하겠습니다.

| 각 키의 weight가 30이고 threshold가 50인 멀티 시그 계정이 있다고 가정해봅시다.

| 즉, 다음과 같습니다. 

* (pub1, 30)
* (pub2, 30)
* (pub3, 30)
* threshold: 50

| 이 계정이 operation을 전송하길 원할 때, operation은 서로 다른 signer의 최소 2 개의 fact 서명을 가지고 있어야 합니다.

1. CASE1: fact signatures signed by pub1's private key and pub2's private key

   1. the sum of pub1's weight and pub2's weight: 60
   2. the sum of weights = 60 > threshold = 50
   3. So the operation with these two fact signatures is available

2. CASE2: fact signatures signed by pub2's private key and pub3's private key

   1. the sum of pub2's weight and pub3's weight: 60
   2. the sum of weights = 60 > threshold = 50
   3. So the operation with these two fact signatures is available

3. CASE3: fact signatures signed by pub1's private key and pub3's private key

   1. the sum of pub1's weight and pub3's weight: 60
   2. the sum of weights = 60 > threshold = 50
   3. So the operation with these two fact signatures is available

4. CASE4: fact signatures signed by pub1's private key, pub2's private key, pub3's private key

   1. the sum of pub1's weight, pub2's weight and pub3's weight: 90
   2. the sum of weights = 90 > threshold = 50
   3. So the operation with these two fact signatures is available

| 그러므로 조건을 만족하기 위해 각 operation에 여러 개의 signature를 추가해야 합니다. (``Operation.addFactSign(private key)`` 를 사용하세요.)
| CASE4의 경우와 같이 weight들의 총합 >= threshold 조건이 지켜지는 한 모든 개인키로 서명하는 것도 가능합니다.

Add Fact Sign to Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| operation 생성 시 fact 서명을 추가하는 방법 외에 fact 서명을 추가하는 다른 방법이 하나 더 있습니다.

| operation에 새 서명을 추가하기 위해 준비해야 할 것은 다음과 같습니다.

* 서명할 개인키 - 이 개인키는 계정의 키여야 합니다.
* python dictionary 객체 혹은 외부 JSON 파일 형태의 operation
* Network ID

| 우선 ``Generator`` 처럼 ``network id`` 와 함께 ``Signer`` 를 생성합니다.

.. code-block:: python

    from mitumc import Signer

    networkId = 'mitum'
    signKey = 'L1V19fBjhnxNyfuXLWw6Y5mjFSixzdsZP4obkXEERskGQNwSgdm1mpr'
    signer = Signer(networkId, signKey)

| 그리고, 서명하세요!

.. code-block:: python

    signed = signer.signOperation('operation.json') # signOperation(filePath)

| 아웃풋인 signed는 mitum-js-util의 ``Operation`` 객체가 아닙니다. 단지 dictionary 객체입니다.
| 한 번에 여러 개의 서명을 추가하길 원한다면 signed - dictionary object에 다른 개인키로 Signer를 다시 만들어 서명해야 합니다.

---------------------------------------------------
Details
---------------------------------------------------

Get Mitum Keypair
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Mitum 키페어 생성 방법을 소개합니다!

| 시작 전, 중요한 것을 설명하겠습니다.

| Mitum의 계정의 주소, 개인키, 공개키는 각자 특별한 타입 접미사를 가지고 있습니다. 그것은 다음과 같습니다.

* Account Address: ``mca``
* Private Key: ``mpr``
* Public Key: ``mpu``

| 예를 들어, 한 single sign 계정은 다음과 같은 형태를 가집니다.

* Account Address: ``9XyYKpjad2MSPxR4wfQHvdWrZnk9f5s2zc9Rkdy2KT1gmca``
* Private Key: ``L11mKUECzKouwvXwh3eyECsCnvQx5REureuujGBjRuYXbMswFkMxmpr``
* Public Key: ``28Hhy6jwkEHx75bNLmG66RQu1LWiZ1vodwRTURtBJhtPWmpu``

| 키페어를 생성하는 세 가지 방법이 있습니다.

Just Create New Keypair
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| mitum-py-util가 무작위의 키페어를 생성해줍니다.

| ``getNewKeypair()`` 를 사용하세요.

.. code-block:: python

    from mitumc.key import getNewKeypair

    # get new Keypair
    kp = getNewKeypair() # returns BTCKeyPair
    kp.privateKey # KzafpyGojcN44yme25UMGvZvKWdMuFv1SwEhsZn8iF8szUz16jskmpr
    kp.publicKey # 24TbbrNYVngpPEdq6Zc5rD1PQSTGQpqwabB9nVmmonXjqmpu

Get Keypair From Your Private Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 이미 개인키를 가지고 있다면 해당 키로부터 키페어를 생성할 수 있습니다.

.. code-block:: python

    from mitumc.key import getKeypairFromPrivateKey

    # get Keypair from your private key
    pkp = getKeypairFromPrivateKey("L2ddEkdgYVBkhtdN8HVXLZk5eAcdqXxecd17FDTobVeFfZNPk2ZDmpr")

Get Keypair From Your Seed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 시드로부터 키페어를 생성할 수도 있습니다. 키페어의 개인키를 기억하지 못하더라도 시드를 통해 복구할 수 있습니다.
| 문자열 시드 길이는 36 이상이어야 합니다.

.. code-block:: python

    from mitumc.key import getKeypairFromSeed

    # get Keypair from your seed
    skp = getKeypairFromSeed("Thisisaseedforthisexample.len(seed)>=36.")

Get Account Address with Keys
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 계정 주소를 threshold와 계정의 모든 (public key, weight)쌍을 사용해 알아낼 수 있습니다.

| 하지만 이 방법은 계정의 threshold나 키가 업데이트 되지 않은 경우에만 사용할 수 있습니다.

| 예제의 계정 정보는 다음과 같습니다.

* key1: (vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu, 40)
* key2: (29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu, 30)
* key3: (uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu, 30)
* threshold: 100

.. code-block:: python

    from mitumc import Generator

    gn = Generator('mitum').currency

    pub1 = "vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu"
    pub2 = "29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu"
    pub3 = "uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu"

    key1 = gn.key(pub1, 40)
    key2 = gn.key(pub2, 30)
    key3 = gn.key(pub3, 30)

    keys = gn.createKeys([key1, key2, key3], 100)
    address = keys.address # your address

Major Classes
'''''''''''''''''''''''''''''''''''''''''''''''''''

Generator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Generator`` 는 Mitum Currency operation 생성을 도와줍니다.

| ``Generator`` 를 사용하기 전 ``network id`` 를 설정해야 합니다.

* Mitum Currency에 대해 ``Generator.currency``를 사용하세요.
* Mitum Blocksign에 대해 ``Generator.blockSign``를 사용하세요.

| Mitum Blocksign operation 생성을 위한 자세한 내용은 `README <https://github.com/ProtoconNet/mitum-py-util/blob/master/README.md>`_ 을 참고하세요.

.. code-block:: python

    from mitumc import Generator

    generator = Generator('mitum')
    currencyGenerator = generator.currency
    blockSignGenerator = generator.blockSign

| ``Generator`` 가 제공하는 모든 메서드는 다음과 같습니다.

.. code-block:: python

    /* For Mitum Currency */
    Generator.currency.key(key, weight)
    Generator.currency.amount(amount, currencyId)
    Generator.currency.createKeys(keys, threshold)
    Generator.currency.createAmounts(amounts) 
    Generator.currency.createCreateAccountsItem(keys, amounts)
    Generator.currency.createTransfersItem(receiver, amoutns)
    Generator.currency.createCreateAccountsFact(sender, items)
    Generator.currency.createKeyUpdaterFact(target, cid, keys)
    Generator.currency.createTransfersFact(sender, items)

    /* For Mitum Blocksign */
    Generator.blockSign.createCreateDocumentsItem(filehash, did, signcode, title, size, cid, signers, signcodes)
    Generator.blockSign.createSignDocumentsItem(owner, documentid, cid)
    Generator.blockSign.createTransferDocumentsItem(owner, receiver, documentid, cid)
    Generator.blockSign.createBlockSignFact(operationType, sender, itemList)

    /* Common */
    Generator.createOperation(fact, memo)
    Generator.createSeal(signKey, operations)

Signer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Signer`` 는 이미 생성된 operation에 새로운 fact 서명을 추가할 때 사용합니다.

| ``Generator`` 와 같이 ``network id`` 가 설정되어야 합니다.

| 서명에 사용할 개인키도 준비해야 합니다.

| ``Signer`` 는 오직 하나의 메서드를 제공합니다.

.. code-block:: python

    Signer.signOperation(operation)

| ``Signer`` 의 정확한 사용 방법은 'Make Your First Operation - Sign'로 돌아가서 확인하세요.

JSONParser
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 이 클래스는 편의를 위해 개발되었습니다.
| ``Operation`` 을 내보내거나 JSON 형식으로 출력하기 위해 다른 패키지를 사용하길 원한다면 굳이 mitum-py-util의 ``JSONParser`` 를 사용할 필요는 없습니다.

.. code-block:: python

    from mitumc import JSONParser

    # ... omitted
    # ... create operations
    # ... refer to above `Make Your First Operation`
    # ... suppose you have already made operations - createAccount, keyUpdater, transfer and a seal - seal

    JSONParser.toJSONString(createAccount.dict()) # print operation createAccount in JSON
    JSONParser.toJSONString(keyUpdater.dict()) # print operation keyUpdater in JSON
    JSONParser.toJSONString(transfer.dict()) # print operation transfer in JSON
    JSONParser.toJSONString(seal) # print seal seal in JSON

    JSONParser.generateFile(createAccount.dict(), 'createAccount.json') # generateFile(dict object, file path)
    JSONParser.generateFile(keyUpdater.dict(), 'keyUpdater.json')
    JSONParser.generateFile(transfer.dict(), 'transfer.json')
    JSONParser.generateFile(seal, 'seal.json')