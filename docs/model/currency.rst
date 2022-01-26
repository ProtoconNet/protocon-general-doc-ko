===================================================
Mitum Currency
===================================================

---------------------------------------------------
What is Mitum Currency
---------------------------------------------------

* Mitum Currency는 Mitum 네트워크 위에서 운용되는 currency 모델입니다.
* Mitum 모델은 Mitum main chain을 확장하는 확장층으로서 다양한 서비스를 제공할 수 있는 솔루션입니다.
* Mitum Currency에서, 화폐 발행 및 운용과 관련한 유연한 정책 설정이 가능합니다.
* Mitum Currency는 Mitum(blockchain core framework)을 기반으로 구현되었습니다.


.. image:: ../images/model.currency/mitum_blockchain_layer.png
    :height: 570
    :scale: 50 
    :alt: Mitum Blockchain Layer


---------------------------------------------------
Feature of Mitum Currency
---------------------------------------------------

* Mitum Currency가 제공하는 주요 특징은 토큰과 관련한 다양한 분야의 비즈니스 요구를 충족합니다.
* 새 계정 생성 시 다중 키가 등록될 수 있으며 관련 키들은 key update operation으로 교체될 수 있습니다.
* Mitum Currency는 새로운 currency를 발행하고 정책을 사용자화 할 수 있습니다.
* currency 정책은 필요할 때 언제나 업데이트 될 수 있습니다.
* Mitum Currency는 블록 생성에 보상이 없으며 인플레이션도 없습니다.
* Mitum Currency 네트워크의 노드 구성은 Mitum의 노드 operation 정책에 따르며 자세한 내용은 :ref:`build network` 에서 찾아볼 수 있습니다.

---------------------------------------------------
Digest Service
---------------------------------------------------

* Digest Service는 Mitum에 저장된 블록 데이터를 저장하며 HTTP-based API로 서비스되는 내부 서비스입니다.
* Digest Service에 대한 자세한 내용은 :ref:`rest api` 를 참고해주세요.

---------------------------------------------------
Seal and Operation
---------------------------------------------------

Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Mitum 블록체인 네트워크에서 operation은 데이터를 변경하는 명령의 단위입니다.

| Mitum Currency가 지원하는 operation은 다음과 같습니다.

* ``create-account``
* ``transfer``
* ``key-updater``
* ``currency-register``
* ``currency-policy-updater``
* ``suffrage-infration``

| 각 operation은 그 내용에 따라 개인키로 서명한 서명이 필요합니다.

| operation의 fact는 실행할 내용을 담고 있으며 fact는 내용을 요약하는 hash 값이 포함되어 있습니다.

Fact and token
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 모든 operation은 fact를 포함하고 있습니다. 한 마디로, operation의 내용은 실재로는 fact에 들어있습니다.

| fact는 Mitum Currency에서 중요한 역할을 합니다.

* fact hash는 처리된 operation을 대표하는 값입니다.
* fact hash 블록체인에서 고유값을 가집니다.
* operation이 블록에 저장되었는지는 fact hash를 검색해 확인할 수 있습니다.

| 사실, fact의 내용은 중복될 수 있습니다. 

| 예를 들어, 

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

Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Seal은 operation의 모음으로 네트워크에 전송됩니다. 즉, operation이 seal에 담겨 전송됩니다.

* seal을 전송하기 위해, 개인키로 만든 서명이 필요합니다.
* 서명을 생성하기 위해 Mitum의 키페어 패키지로부터 생성한 개인키가 필요합니다.
* seal은 최대 100 개의 operation을 담을 수 있습니다.

| seal 서명에 사용되는 개인키는 블록체인 상에서 아무것도 할 필요가 없습니다. 즉, 해당 개인키는 등록된 계정의 키일 필요가 없습니다.

Send
'''''''''''''''''''''''''''''''''''''''''''''''''''

| operation 생성 후, 클라이언트는 서명을 생성 해 seal에 붙입니다.

* seal에 담을 수 있는 최대 operation 수 내에서 필요한 가능한 많은 operation을 만들어 seal에 담으세요.
* seal 서명을 생성해 추가하세요.
* Mitum 노드에 seal을 전송하세요.

Stored in Block
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 블록체인 네트워크에 전송된 operation은 그 operation이 정상적이고 블록에 쌓인 경우 계정의 상태를 변화시킵니다.
| operation이 블록에 저장되었는지 :ref:`rest api` 를 통해 확인할 수 있습니다.

.. _block data:

---------------------------------------------------
Block Data
---------------------------------------------------

Block data in Mitum Currency Node
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Mitum Currency Node에서 블록 데이터는 두 공간에 저장됩니다: 데이터베이스, 파일 시스템

* 데이터베이스는 합의에 사용되는 다음과 같은 정보를 저장합니다.

.. code-block:: none

    blockdata_map
    info
    manifest: block header
    operation: operation fact
    operation
    proposal
    seal
    state: state data by each block
    voteproof

* 파일 시스템은 다음과 같은 모든 블록 데이터를 저장합니다.

.. code-block:: none

    manifest
    operations of block
    states of block
    proposal
    suffrage information
    voteproofs(and init and accept ballots)

* 데이터베이스에 저장된 블록 데이터는 mitum currency 노드를 실행하고 정상적으로 네트워크에 참여하기 위해 필요합니다.
* 파일 시스템의 블록 데이터는 런타임에 사용되지 않으며 syncing 노드에 블록 데이터를 제공하는데 사용됩니다.

| 온전한 노드는 블록 데이터를 동기화하길 원하는 다른 노드들을 위해 블록 데이터를 제공해야 합니다.

BlockDataMap
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 기본적으로 블록 데이터는 로컬 파일 시스템에 저장됩니다.

| blockdatamap은 실제 블록 데이터가 어디에 위치해있는지에 대한 정보를 포함합니다.

.. code-block:: json

    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "file:///000/000/000/000/000/000/002/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
        },
        "writer": "blockdata-writer-v0.0.1"
    }

| 이 BlockDataMap 예제에서, ``operation_tree`` 의 데이터는 ``file:///000/000/000/000/000/000/002/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz`` 에 위치해있습니다.

BlockDataMap for block data stored in external storage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Mitum Currency는 노드의 로컬 파일 시스템이 아닌 외부 스토리지에 블록 데이터를 저장하는 것을 지원합니다.

| 블록 데이터를 외부에 저장하기 위한 몇 가지 과정을 거치고 나면 blockdatamap은 다음과 같아집니다.

.. code-block:: json
    
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "fhttps://aws/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
        },
        "writer": "blockdata-writer-v0.0.1"
    }

| 보이는 것처럼 ``url`` 은 외부 스토리지 서버로 교체됩니다.

How to update BlockDataMap for external Storage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 예를 들어 block height 10의 블록 데이터가 외부 스토리지로 옮겨졌다고 가정해봅시다.

| 노드의 deploy key를 사용하겠습니다.
| 노드의 이 deploy key는 노드의 개인키 대신 사용되는 키입니다.

| deploy key를 만드는 방법은 :ref:`deploy command` 의 ``deploy key`` 를 참고하세요.

| 블록 데이터 이동과 blockdatamap 업데이트 과정은 다음과 같습니다.

* mitum currency 노드의 새로운 deploy key를 얻습니다.
* ``storage download map`` 을 사용해 현재 blockdatamap을 내려받습니다.
* block height 10의 모든 블록 데이터를 외부 스토리지에 업로드합니다. (example : AWS S3)
* 내려받은 BlockDataMap의 ``url`` 필드값을 새로운 외부 스토리지 url로 교체합니다.
* ``storage set-blockdatamaps`` 명령어를 실행하여 노드의 blockdatamap을 업데이트합니다.
* 새롭게 업데이트 된 blockdatamap을 ``storage download map`` 명령어로 확인하세요.

| blockdatamap을 성공적으로 업데이트하면 mitum currency 노드는 30초뒤 자동적으로 height 10의 모든 파일을 지웁니다.

.. code-block:: shell

    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ NODE=https://127.0.0.1:54321
    
    $ ./mc storage download map 10 --tls-insecure --node=$NODE > mapData
    
    $ cat mapData | jq
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "file:///000/000/000/000/000/000/002/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
            "manifest": {
                "type": "manifest",
                "checksum": "6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42",
                "url": "file:///000/000/000/000/000/000/002/2-manifest-6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42.jsonld.gz"
            },
            "suffrage_info": {
                "type": "suffrage_info",
                "checksum": "e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02",
                "url": "file:///000/000/000/000/000/000/002/2-suffrage_info-e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02.jsonld.gz"
            },
            "states": {
                "type": "states",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "file:///000/000/000/000/000/000/002/2-states-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "operations": {
                "type": "operations",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "file:///000/000/000/000/000/000/002/2-operations-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "proposal": {
                "type": "proposal",
                "checksum": "dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe",
                "url": "file:///000/000/000/000/000/000/002/2-proposal-dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe.jsonld.gz"
            },
            "init_voteproof": {
                "type": "init_voteproof",
                "checksum": "705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64",
                "url": "file:///000/000/000/000/000/000/002/2-init_voteproof-705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64.jsonld.gz"
            },
            "accept_voteproof": {
                "type": "accept_voteproof",
                "checksum": "0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34",
                "url": "file:///000/000/000/000/000/000/002/2-accept_voteproof-0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34.jsonld.gz"
            },
            "states_tree": {
                "type": "states_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "file:///000/000/000/000/000/000/002/2-states_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            }
        },
        "writer": "blockdata-writer-v0.0.1"
    }

    $ aws s3 cp ./blockdata/000/000/000/000/000/000/002 s3://destbucket/blockdata/000/000/000/000/000/000/002 --recursive
    # update mapData blockdata url from "file:///000/000/000/000/000/000/002/" to https://aws/"

    $ ./mc storage set-blockdatamaps $DEPLOY_KEY mapData $NODE --tls-insecure

    $ ./mc storage download map 2 --tls-insecure --node=$NODE
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "fhttps://aws/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
            "manifest": {
                "type": "manifest",
                "checksum": "6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42",
                "url": "fhttps://aws/2-manifest-6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42.jsonld.gz"
            },
            "suffrage_info": {
                "type": "suffrage_info",
                "checksum": "e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02",
                "url": "fhttps://aws/2-suffrage_info-e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02.jsonld.gz"
            },
            "states": {
                "type": "states",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "fhttps://aws/2-states-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "operations": {
                "type": "operations",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "fhttps://aws/2-operations-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "proposal": {
                "type": "proposal",
                "checksum": "dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe",
                "url": "fhttps://aws/2-proposal-dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe.jsonld.gz"
            },
            "init_voteproof": {
                "type": "init_voteproof",
                "checksum": "705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64",
                "url": "fhttps://aws/2-init_voteproof-705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64.jsonld.gz"
            },
            "accept_voteproof": {
                "type": "accept_voteproof",
                "checksum": "0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34",
                "url": "fhttps://aws/2-accept_voteproof-0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34.jsonld.gz"
            },
            "states_tree": {
                "type": "states_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "fhttps://aws/2-states_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            }
        },
        "writer": "blockdata-writer-v0.0.1"
    }

---------------------------------------------------
Support Operations
---------------------------------------------------

+------------------------------------+------------------------------------+
| Operations for Currency                                                 | 
+====================================+====================================+
| currency-register                  | Register new currency id           |
+------------------------------------+------------------------------------+
| currency-policy-updater            | Update currency policy             |
+------------------------------------+------------------------------------+
| suffrage-infration                 | Increase amount of tokens          |
+------------------------------------+------------------------------------+

+------------------------------------+------------------------------------+
| Operations for Account                                                  |
+====================================+====================================+
| create-account                     | Create new account                 | 
+------------------------------------+------------------------------------+
| key-updater                        | Update account keys                | 
+------------------------------------+------------------------------------+
| transfer                           | Transfer amount of tokens          | 
+------------------------------------+------------------------------------+

| 명령어로 위 operation을 생성하는 방법은 :ref:`seal command` 을 참고하세요.
