.. _key command:

===================================================
Key Command
===================================================

| ``key``는 키페어 생성, keys로부터 계정 주소 연산, 서명을 지원합니다.

| ``key``의 하위 명령어는 다음과 같습니다.

* ``new``
* ``address``
* ``sign``

.. note::

    Keypair
    
    * *개인키*와 *공개키*는 키페어 생성에 의해 만들어집니다.
    * 생성된 키페어는 *게정 생성*, 노드의 *키페어 등록*, operation과 seal의 *서명 생성*에 사용됩니다.

---------------------------------------------------
new
---------------------------------------------------

| ``new``를 사용해 **새로운 키페어를 생성**합니다.

Random Keypair
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 시드 없이 키페어를 생성합니다.

.. code-block:: shell

    $ ./mc key new

| **EXAMPLE**

.. code-block:: shell

    $ ./mc key new 
          hint: mpr
    privatekey: L1ZERchoY53vC5TJQ3WnZEWmg97L2Utw5rgFrCwM7ekTu9zJkZYjmpr
     publickey: 28nFxuC5ETygieSGEYTkewwnCZseB4TNYGMRtxz31bvxzmpu

Keypair from Seed
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 시드로 키페어를 생성합니다. 문자열 시드의 길이는 36 이상이어야 합니다.

.. code-block:: shell

    $ ./mc key new --seed <string seed>

| **EXAMPLE**

.. code-block:: shell

    $ ./mc key new --seed abcdefghijklmnopqrstuvwxyzABCDEFGHIJ
          hint: mpr
    privatekey: KypAAGYtVFdTFLS8muPJhwfJBFCFHKSe594yYmKK3FPteh7sie4Dmpr
     publickey: 25BcZrcyiE3TD2BZEqkdDuaYB9zHxpdW82BNn8HkCLTijmpu

---------------------------------------------------
address
---------------------------------------------------

| ``address`` 명령어로 keys로부터 주소를 계산합니다.

| 계정의 (public key, weight) 쌍과 threshold를 준비해야 합니다. 아래의 *Multi Sig Account*를 참고하세요.

.. code-block:: shell

    $ ./mc key address <threshold> [<publickey>,<weight>]

| **EXAMPLE**

| 계정 정보가 아래와 같을 때,

+---------------+------------------------------------------------------------------+
| threshold     | 100                                                              |
+---------------+------------------------------------------------------------------+
| keys          | {key: 21Sn1o…, weight: 50}, {key: utzCef…, weight: 50}           |
+---------------+------------------------------------------------------------------+

.. code-block:: shell

    $ ./mc key address 100 21Sn1owHXRx336aaerU1WbbKjiZXMcrJsnxBHP9etNx6zmpu,50 utzCefA1Szmmt3rAwqW5yEhxK1x3hG3Y3yThEK3gZmv3mpu,50
    37x8YoAGA93B3HmDVNterRf1NTgz9tfN1gQn4jYuBYCHmca

| **하지만, key-updater 명령어로 계정의 key, weight, threshold 구성이 달라진 경우에는 올바른 계정 주소를 얻을 수 없습니다.** :ref:`key updater`를 참고하세요. 

.. _multi sig:

Multi Sig Account
'''''''''''''''''''''''''''''''''''''''''''''''''''

* Mitum Currency에서 계정은 *currency*와 *balance*를 가진 데이터 구조체입니다.
* 계정은 *address*라는 고유값을 가지고 있으며 이 값을 통해 식별할 수 있습니다.
* 사용자의 *Account authentication*을 위해 공개키를 등록하세요.
* Mitum Currency 계정은 **multi signature**가 가능하므로 *multiple public keys*를 가질 수 있습니다. 

| 예를 들어, 다음 조건의 계정이 유효합니다.

+---------------+------------------------------------------------------------------+
| address       | HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca                  |
+---------------+------------------------------------------------------------------+
| threshold     | 100                                                              |
+---------------+------------------------------------------------------------------+
| keys          | {key: rd89Gx…, weight: 50}, {key: skRdC6…, weight: 50}           |
+---------------+------------------------------------------------------------------+
| balance       | {currency: MCC, amount: 10000}, {currency: MCC2, amount: 20000}  |
+---------------+------------------------------------------------------------------+

.. note::

    각 계정이 따라야 할 조건은 다음과 같습니다.

    * ``threshold``의 범위는 1 <= threshold <= 100입니다.
    * 각 ``weight``의 범위는 1 <= weight <= 100입니다.
    * 계정의 모든 ``weight``의 합은 ``threshold`` 이상이어야 합니다.
    * 각 공개키는 ``mpu`` 접미사가 붙은 BTC compressed public key여야 합니다.
    * 주소값에는 ``mca``가 접미사로 따라옵니다.

    다음은 유효한 계정의 예시입니다.

    CASE1 (single)

    * threshold: 100
    * keys: {key: rd89Gx…, weight: 100}

    CASE2 (single)

    * threshold: 50
    * keys: {key: rd89Gx…, weight: 60}

    CASE3 (multi)
    
    * threshold: 100
    * keys: {key: rd89Gx…, weight: 40}, {key: skRdC6…, weight: 30}, {key: mymMwq…, weight: 30}

    CASE4 (multi)

    * threshold: 50
    * keys: {key: rd89Gx…, weight: 20}, {key: skRdC6…, weight: 20}, {key: mymMwq…, weight: 10}

| 같은 공개키 조합이라도 ``weight``나 ``threshold``의 조합이 다르면 다른 계정 주소를 가질 수 있습니다.

---------------------------------------------------
sign
---------------------------------------------------

| ``sign`` 명령어로 특정 메시지에 개인키로 서명하세요.

.. code-block:: shell

    $ ./mc key sign <privatekey> <signature base>

| **EXAMPLE**

.. code-block:: shell

    $./mc key sign L5nDx2QtZVBPtJvUQ13cj3bMhC487JdxrwXTdS6JgzTvnSHestCxmpr bWVzc2FnZQ=
    381yXZHrm73kGD8z7FAksBjxy49wPRWn3WRdP22befdbFff6WYSdK8rz9TLpFWuEW7rmmphF3rHkrvTPvhVQ5kXNGLmELBwZ

| signature base는 *base64* 인코딩된 문자열이어야 합니다. 