===================================================
Seal and Operation
===================================================

.. _operation:

---------------------------------------------------
Operation
---------------------------------------------------

| Mitum 블록체인 네트워크에서 operation은 데이터를 변경하는 명령의 단위입니다.

| 예를 들어, Mitum Currency는 ``create-account``, ``transfer``, ``key-updater``, ``currency-register``, ``currency-policy-updater``, ``suffrage-infration`` operation을 지원합니다.

| 각 operation은 그 내용에 따라 개인키로 서명한 서명이 필요합니다.

| operation의 fact는 실행할 내용을 담고 있으며 fact는 내용을 요약하는 hash 값이 포함되어 있습니다.

.. _fact:
.. _token:

---------------------------------------------------
Fact and token
---------------------------------------------------

| 모든 operation은 fact를 포함하고 있습니다. 한 마디로, operation의 내용은 실재로는 fact에 들어있습니다.

| fact는 Mitum 블록체인 네트워크에서 중요한 역할을 합니다.

* fact hash는 처리된 operation을 대표하는 값입니다.
* fact hash 블록체인에서 고유값을 가집니다.
* operation이 블록에 저장되었는지는 fact hash를 검색해 확인할 수 있습니다.

| 이때, fact의 내용은 중복될 수 있습니다. 

| 예를 들어, 동일한 내용의 두 트랜잭션은 ``token`` 이라는 요소의 도움 없이는 자연스럽게 같은 fact hash를 가지게 되며 Mitum 블록체인은 이를 다음과 같이 처리합니다.

    .. code-block:: none
        
        - `sender A가 receiver B에게 토큰 100을 보낸다`는 항상 같은 fact 내용을 가집니다.
        - 그러므로 같은 fact 내용만으로 만든 fact의 hash 값은 중복될 가능성이 있습니다.
        - 만약 fact hash가 중복된 여러 개의 operation이 있을 경우 오직 첫 번째 operation만이 처리되며 나머지는 무시됩니다.

| 그렇다면, 같은 내용을 가진 fact는 다시 보낼 수 없을까요?

| 걱정하지 마세요. 각 fact에는 fact를 고유하게 만들어주는 token 값이 포함되어 있습니다.
| token은 operation에 추가되어야 하는 필수적인 값입니다.

.. code-block:: json
    
    {
        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "hash": "3Zdg5ZVdNFRbwX5WU7Nada3Wnx5VEgkHrDLVLkE8FMs1",
        "token": "cmFpc2VkIGJ5",
        "sender": "8PdeEpvqfyL3uZFHRZG5PS3JngYUzFFUGPvCg29C2dBnmca",
        "items": [
            {
                "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                "keys": {
                    "_hint": "mitum-currency-keys-v0.0.1",
                    "keys": [
                        {
                            "_hint": "mitum-currency-key-v0.0.1",
                            "weight": 100,
                            "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                        }
                    ],
                    "threshold": 100
                },
                "amounts": [
                    {
                        "_hint": "mitum-currency-amount-v0.0.1",
                        "amount": "333",
                        "currency": "MCC"
                    }
                ]
            }
        ]
    }

| token은 memo와 비슷하지만, 같은 fact 내용에 대해 고유한 token을 사용함으로써 fact를 고유하게 만들어주는 특성이 있습니다.

| 모든 operation의 fact를 고유하게 만드는 것은 많은 방향으로 사용성을 확장시킵니다.

* 가장 큰 이점은 token과 fact의 정확한 내용을 알고 있으면 operation이 처리되었는지 간단히 확인할 수 있다는 것입니다.
* sender, receiver, currencyID, amount, 그리고 특정 token 값을 알고 있으면 누구나 fact hash를 계산할 수 있습니다.
* 그러므로 fact hash에 상응하는 operation이 처리되었는지 누구나 알 수 있습니다.

| fact hash는 블록체인에 기록되는 공개 증명과 같습니다. 만약 블록체인에 게시된 증명을 잘 사용하면 다양하게 응용할 수 있습니다.
| 예를 들자면 블록체인에 직접 계정을 가지지 않은 외부인도 operation이 처리되었는지를 나타내는 유일한 값인 fact hash를 알아내고 조건에 맞게 구현할 수 있습니다.

| 게다가 fact와 token들은 송금뿐만이 아닌 다양한 데이터를 다루는 모델에 유용하게 사용될 수 있습니다.

.. _seal:

---------------------------------------------------
Seal
---------------------------------------------------

| Seal은 operation의 모음으로 네트워크에 전송됩니다. 즉, operation이 seal에 담겨 전송됩니다.

* seal을 전송하기 위해, 개인키로 만든 서명이 필요합니다.
* 서명을 생성하기 위해 Mitum의 키페어 패키지로부터 생성한 개인키가 필요합니다.
* seal은 최대 100 개의 operation을 담을 수 있습니다.

| seal 서명에 사용되는 개인키는 블록체인 상에서 아무것도 할 필요가 없습니다. 즉, 해당 개인키는 등록된 계정의 키일 필요가 없습니다.

---------------------------------------------------
Send
---------------------------------------------------

| operation 생성 후, 클라이언트는 서명을 생성 해 seal에 붙입니다.

* seal에 담을 수 있는 최대 operation 수 내에서 필요한 가능한 많은 operation을 만들어 seal에 담으세요.
* seal 서명을 생성해 추가하세요.
* Mitum 노드에 seal을 전송하세요.

---------------------------------------------------
Stored in Block
---------------------------------------------------

| 블록체인 네트워크에 전송된 operation은 그 operation이 정상적이고 블록에 쌓인 경우 계정의 상태를 변화시킵니다.
| operation이 블록에 저장되었는지 :ref:`rest api` 를 통해 확인할 수 있습니다.