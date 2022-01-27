.. _config:

===================================================
Configuration
===================================================

| 노드 설정을 위한 구성은 YAML으로 작성됩니다.

---------------------------------------------------
address
---------------------------------------------------

| 로컬 노드의 주소 (url 주소 별칭)

.. code-block:: none

    address: n0sas

---------------------------------------------------
genesis-operations
---------------------------------------------------

| ``genesis-operation``은 네트워크 초기화 시 실행되는 제네시스 operation에 대한 설정입니다.
| ``genesis-operation``은 최초로 생성되는 블록의 내용을 담고 있습니다.

| currency 모델에서는 메인 currency와 제네시스 계정의 정보가 설정되어야 합니다.

| 등록하는 정보는 다음과 같습니다.

* *genesis account*의 키 (key, weight, threshold)
* 최초 잔액
* Currency ID
* 생성될 currency의 수수료 정책

| 예시는 다음과 같습니다.

.. code-block:: none

    genesis-operations:
        - account-keys:
            keys:
                - publickey: rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu
                  weight: 100
            threshold: 100
        currencies:
            - balance: "99999999999999999999"
              currency: MCC
              new-account-min-balance: "33"
        type: genesis-currencies

---------------------------------------------------
network
---------------------------------------------------

| 네트워크에 사용되는 노드의 *domain address* 혹은 *IP address* 를 설정합니다.
| network는 quic communication protocol을 사용하여 노드나 클라이언트로부터 메시지를 받기 위한 주소입니다.
| 테스트 노드를 구성하기 위해 Self-signed certificates가 사용될 수 있습니다. 보안이 크게 문제가 되지 않는 테스트 및 개발 노드에만 사용할 수 있습니다.

.. code-block:: shell
    
    $ openssl genrsa -out mitum.key 4096

    $ openssl req -x509 -new -nodes -key mitum.key -sha256 -days 1024 -out mitum.crt
    
| 예시는 다음과 같습니다.

.. code-block:: none

    network:
        bind: https://0.0.0.0:54321
        url: https://127.0.0.1:54321
        cert-key: mitum.key
        cert: mitum.crt

rate-limit
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 기본적으로, 인터넷 서비스의 API 인터페이스는 제한 없이 클라이언트로부터의 연결을 허용합니다.
| 하지만 서비스에 지나치게 많은 요청이 들어오면 서비스의 성능을 해칠 수 있습니다.
| 서비스를 안정적으로 유지하기 위해 API 서비스에 ``rate limit`` 가 적용될 수 있습니다.

| `Rate limiting <https://en.wikipedia.org/wiki/Rate_limiting>`_ 에 대해 확인하세요.

| Mitum은 none-suffrage 노드들을 포함한 노드들 간의 통신을 위해 *quic* 기반 API 인터페이스를 제공합니다.
| 또한, Mitum Currency는 digest라 불리는 *http2* 기반 API 서비스를 제공합니다.

| ``rate-limit`` 가 이런 API 서비스에 적용됩니다.

.. code-block:: none

    network:
        bind: https://0.0.0.0:54321
        url: https://127.0.0.1:54321

        rate-limit:
            cache: "memory:?prefix=showme"
            preset:
                bad-nodes:
                    new-seal: 3/2m
                    blockdata: 4/m
            3.3.3.3:
                preset: bad-nodes
            4.4.4.4/24:
                preset: bad-nodes
                blockdata: 5/m
            127.0.0.1/24:
                preset: suffrage

* ``cache``: 요청에 대한 캐시. 이때, “memory:”와 “redis://<redis server>”를 지원합니다.

  * **memory**: memory cache
  * **redis://<redis server>**: cached in redis server

* ``preset``: 사전 정의된 rate limit 설정.

    * Mitum에대한 ``suffrage`` 와 ``world`` 프리셋은 이미 정의되어 있습니다. `launch/config/ratelimit.go <https://github.com/spikeekips/mitum/blob/master/launch/config/ratelimit.go>`_ 소스코드에서 확인하세요.
    * bad-nodes와 같은 자신만의 rate limit 설정을 만들수도 있습니다.

* Rules:

    * 특정 IP에 대한 Rate-limit 설정
    * 규칙은 *IP address* (또는 IP address 범위), ``preset`` 그리고 자세한 ``rate-limit`` 설정으로 구성되어 있습니다.
    * IP 주소는 단일 값이나 *CIDR* 표기법으로 표현된 IP 주소의 범위일 수 있습니다.
      * example : 3.3.3.3, 4.4.4.4/24, 127.0.0.1/24
    * Rate limit는 ``preset`` 과 추가적인 ``limits`` 으로 설정될 수 있습니다.
    * ``preset``는 ``suffrage``, ``world`` 와 같은 사전 정의된 프리셋이나 ``bad-nodes`` 와 같은 사용자화 프리셋일 수 있습니다.
    * ``blockdata: 5/m`` 과 같은 추가적인 limit이 ``preset`` 에 추가될 수 있습니다.
    * 규칙은 정의된 순서대로 확인됩니다. 상위 규칙이 먼저 확인됩니다.

* Detailed limit:

    * limit 설정에 사용되는 new-seal과 같은 Mitum API 인터페이스의 이름은 RateLimitHandleMap(launch/config/ratelimit.go)에서 확인할 수 있습니다..
    * Mitum Currency API 인터페이스의 이름은 RateLimitHandlerMap(digest/handler.go)에서 확인할 수 있습니다.
    * new-seal: 3/2m은 new-seal 인터페이스가 특정 IP, IP 주소 범위에 대해 2분에 3 요청을 허용한다는 뜻입니다.
    * time duration의 방법을 확인하세요.

* 다른 규칙이 설정되지 않으면 기본적으로 rate limit이 없습니다.

| 0보다 작은 late limit은 unlimited를 뜻합니다.

| 다음 예시입니다,

.. code-block:: none

    4.4.4.4/24:
    preset: bad-nodes
    blockdata: -1/m

| 0 limit 값은 요청을 차단한다는 뜻입니다.

| For example,

.. code-block:: none

    4.4.4.4/24:
        preset: bad-nodes
        blockdata: 0/m

---------------------------------------------------
network-id
---------------------------------------------------

| ``network id`` 는 네트워크를 식별하는 식별자의 역할을 합니다.
| 같은 네트워크의 모든 노드들은 같은 ``network id`` 값을 가집니다.

| 다음은 예시입니다.

.. code-block:: none

    network-id: mitum

---------------------------------------------------
keypair
---------------------------------------------------

| 노드의 개인키를 기입하세요.

| 예시입니다.

.. code-block:: none

    privatekey: Kxt22aSeFzJiDQagrvfXPWbEbrTSPsRxbYm9BhNbNJTsrbPbFnPAmpr

| 키페어 생성 방법은 :ref:`key command` 을 참고하세요.

---------------------------------------------------
storage
---------------------------------------------------

| 블록체인 데이터 스토리지의 파일 시스템 경로와 mongodb 데이터베이스 주소를 설정하세요.
| 블록 데이터 설정이 없으면, ``blockdata > path`` 가 현재 경로의 blockdata라고 불리는 폴더로 기본 설정됩니다.

| 다음은 예시입니다. 

.. code-block:: none

    storage:
    blockdata:
        path: ./mc-blockfs
    database:
        uri: mongodb://127.0.0.1:27017/mc

| ``port number`` 는 docker를 실행할 때의 것과 같아야합니다.

---------------------------------------------------
suffrage
---------------------------------------------------

nodes
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 합의에 참여하는 suffrage 노드의 주소를 설정하세요.

| 로컬 노드의 별칭은 ``n0sas`` 입니다.
| 만약 ``n0``, ``n1``, ``n2``, ``n3`` 노드가 suffrage 노드로 추가되면 설정은 다음과 같아집니다.

.. code-block:: none

    suffrage:
        nodes:
            - n0sas
            - n1sas
            - n2sas
            - n3sas

| 만약 로컬 노드인 ``n0`` 가 suffrage 노드로 추가되지 않으면 로컬 노드는 *None-suffrage* 노드가 되며 *syncing node* 로서만 운용되게 됩니다.

* *Syncing node* 는 합의에 참여하지 않으며 오직 블록 데이터를 동기화하기만 합니다.
* *None-suffrage* 노드는 operation을 담은 seal만 다룹니다.
* *None-suffrage* 노드는 노드 사이의 voting과 관련된 ballot과 proposal을 처리하지 않습니다.
* *None-suffrage* 노드가 operation seal을 저장할 때, 이를 suffrage 노드에 브로드캐스팅합니다.

| 만약 *None-suffrage* 노드가 다른 노드들을 suffrage 노드에 추가하지 않으면, 혹은 다른 suffrage 노드를 구성하지 않으면 operation seal이 처리될 수 없습니다.

.. code-block:: none

    suffrage:
        nodes:
            - n1sas
            - n2sas
            - n3sas

---------------------------------------------------
sync-interval
---------------------------------------------------

| *None-suffrage* 노드는 주기적으로 블록데이터를 동기화합니다.

| 기본 주기는 10초입니다.
| ``sync-interval`` 설정을 통해 주기를 변경할 수 있습니다.

.. code-block:: none

    sync-interval: 3s

---------------------------------------------------
nodes
---------------------------------------------------

| 블록체인 네트워크의 알려진 노드들의 ``address``, ``public key``, ``url`` 를 입력합니다.

* 작성하지 않으면 **standalone node** 로서 운용되게 됩니다.
* 노드가 suffrage 노드이거나 node discovery 기능이 사용되면, 노드의 ``url`` 는 필요하지 않습니다.
* 하지만 노드가 suffrage 노드가 아닌 경우 suffrage 노드들의 ``url`` 입력되어야 합니다.

| Mitum 노드들은 기본적으로 *CA signed certificate* (public certificate)를 사용합니다.

* 만약 설정과 관련된 certificate가 *Network config* 에 설정되지 않으면 노드는 *self-signed certificate* 를 사용합니다.
* 다른 Mitum 노드들이 self-signed certificate를 사용하면, self-signed certificate를 사용하는 모든 노드들에 ``tls-insecure: true`` 가 설정되어야 합니다.

.. code-block:: none

    (In case of suffrage node)

    nodes:
        - address: n1sas
        publickey: ktJ4Lb6VcmjrbexhDdJBMnXPXfpGWnNijacdxD2SbvRMmpu
        tls-insecure: true
        - address: n2sas
        publickey: wfVsNvKaGbzB18hwix9L3CEyk5VM8GaogdRT4fD3Z6Zdmpu
        tls-insecure: true
        - address: n3sas
        publickey: vAydAnFCHoYV6VDUhgToWaiVEtn5V4SXEFpSJVcTtRxbmpu
        tls-insecure: true

.. code-block:: none

    (If it is not a suffrage node)

    nodes:
        - address: n1sas
        publickey: ktJ4Lb6VcmjrbexhDdJBMnXPXfpGWnNijacdxD2SbvRMmpu
        url: https://127.0.0.1:54331
        tls-insecure: true
        - address: n2sas
        publickey: wfVsNvKaGbzB18hwix9L3CEyk5VM8GaogdRT4fD3Z6Zdmpu
        url: https://127.0.0.1:54341
        tls-insecure: true
        - address: n3sas
        publickey: vAydAnFCHoYV6VDUhgToWaiVEtn5V4SXEFpSJVcTtRxbmpu
        url: https://127.0.0.1:54351
        tls-insecure: true

---------------------------------------------------
digest
---------------------------------------------------

| API 접근 시의 *API* 및 *IP address* 에서 제공하는 데이터를 저장하는 *mongodb address* 를 지정하세요.

.. code-block:: none

    digest:
        network:
            bind: https://localhost:54320
            url: https://localhost:54320
            cert-key: mitum.key
            cert: mitum.crt

---------------------------------------------------
tutorial.yml
---------------------------------------------------

| 다음은 **standalone** 노드 구성의 한 예제입니다.

.. code-block:: none

    address: mc-nodesas
    privatekey: Kxt22aSeFzJiDQagrvfXPWbEbrTSPsRxbYm9BhNbNJTsrbPbFnPAmpr
    storage:
        database:
            uri: mongodb://127.0.0.1:27017/mc
        blockdata:
            path: ./mc-blockfs
    network-id: mitum
    network:
        bind: https://0.0.0.0:54321
        url: https://127.0.0.1:54321
        cert-key: mitum.key
        cert: mitum.crt
    genesis-operations:
        - type: genesis-currencies
        account-keys:
            keys:
                - publickey: rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu
                    weight: 100
            threshold: 100
        currencies:
            - balance: "99999999999999999999"
                currency: MCC
                new-account-min-balance: "33"
                feeer:
                    type: fixed
                    amount: 1
    policy:
        threshold: 100
    suffrage:
        nodes:
            - mc-nodesas

    digest:
        network:
            bind: https://0.0.0.0:54320
            url: https://127.0.0.1:54320
            cert-key: mitum.key
            cert: mitum.crt