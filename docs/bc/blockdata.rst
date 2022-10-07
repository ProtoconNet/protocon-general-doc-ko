.. _block data:

===================================================
Block Data
===================================================

---------------------------------------------------
Block data in Mitum Node
---------------------------------------------------

| Mitum에서 블록 데이터는 두 공간에 저장됩니다: 데이터베이스, 파일 시스템

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

* 데이터베이스에 저장된 블록 데이터는 mitum 노드를 실행하고 정상적으로 네트워크에 참여하기 위해 필요합니다.
* 파일 시스템의 블록 데이터는 런타임에 사용되지 않으며 syncing 노드에 블록 데이터를 제공하는데 사용됩니다.

| 온전한 노드는 블록 데이터를 동기화하길 원하는 다른 노드들을 위해 블록 데이터를 제공해야 합니다.

---------------------------------------------------
BlockDataMap
---------------------------------------------------

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
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Mitum는 노드의 로컬 파일 시스템이 아닌 외부 스토리지에 블록 데이터를 저장하는 것을 지원합니다.

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

* mitum 노드의 새로운 deploy key를 얻습니다.
* ``storage download map`` 을 사용해 현재 blockdatamap을 내려받습니다.
* block height 10의 모든 블록 데이터를 외부 스토리지에 업로드합니다. (example : AWS S3)
* 내려받은 BlockDataMap의 ``url`` 필드값을 새로운 외부 스토리지 url로 교체합니다.
* ``storage set-blockdatamaps`` 명령어를 실행하여 노드의 blockdatamap을 업데이트합니다.
* 새롭게 업데이트 된 blockdatamap을 ``storage download map`` 명령어로 확인하세요.

| blockdatamap을 성공적으로 업데이트하면 mitum 노드는 30초뒤 자동적으로 height 10의 모든 파일을 지웁니다.

.. code-block:: shell

    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ NODE=https://127.0.0.1:54321
    
    $ ./mitum storage download map 10 --tls-insecure --node=$NODE > mapData
    
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

    $ ./mitum storage set-blockdatamaps $DEPLOY_KEY mapData $NODE --tls-insecure

    $ ./mitum storage download map 2 --tls-insecure --node=$NODE
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