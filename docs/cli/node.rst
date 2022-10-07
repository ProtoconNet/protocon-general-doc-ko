.. _node management:

===================================================
Node Management
===================================================

.. _node command:

---------------------------------------------------
node
---------------------------------------------------

| ``node`` 는 노드를 초기화하고 운용합니다.

| ``node`` 의 하위 명령어는 다음과 같습니다.

* ``init``
* ``run``
* ``start-handover``
* ``info``

init
''''''''''''''''''''''''''''''''''''''''''''''''''''

| ``init`` 명령어로 노드 구성을 가지고 있는 노드 디자인 파일을 사용해 노드를 초기화하세요.

| ``init`` command에 대한 자세한 설명은 :ref:`node init` 를 참고하세요.

.. code-block:: shell

    $ ./mitum node init <node design file>

run
''''''''''''''''''''''''''''''''''''''''''''''''''''

| ``run`` 을 사용해 노드 구성을 가지고 있는 노드 디자인 파일에 따라 노드를 운용하세요.

| See :ref:`node run` for a detailed explanation of ``run`` command.

.. code-block:: shell

    $ ./mitum node run <node design file>

| 만약 노드가 suffrage 노드라면 다른 살아있는 suffrage 노드의 주소들을 Node discovery 프로토콜로 알아낼 수 있습니다. node discovery 특성은 노드가 suffrage 노드일 때만 제공됩니다.

* suffrage 노드가 시작되면 publish url 정보를 제외한 모든 suffrage 노드들의 네트워크 정보를 결정하는 것이 가능합니다.
* node discovery를 위해 노드는 시작 시 discovery url에 하나 이상의 suffrage 노드의 주소를 설정해야 합니다.

| discovery url을 설정하기 위해 ``–discovery`` 명령줄 옵션을 사용하세요.

.. code-block:: shell
    
    $ ./mitum node run <n0 config file> --discovery <n1 publish url> --discovery <n2 publish url>

* 노드가 discovery url을 스스로 설정하지 않더라도 다른 suffrage 노드가 이 노드를 discovery 노드로 지정하면 다른 노드들의 publish url은 gossip protocol에 의해 알려집니다. 만약 discovery로 지정된 노드들이 discovery 운용중이지 않다면, 성공할때까지 시도합니다.
* 또한 node discovery는 suffrage 노드와만 작동합니다. suffrage 노드 리스트에 추가되지 않은 노드들은 다른 suffrage 노드들의 url들을 노드 설정에 지정합니다.
* 만약 log level를 info로 설정하면 쉽게 새롭게 생성되는 블록의 정보를 확인할 수 있습니다.

| ``–log`` 명령줄 옵션은 특정 파일들에 로그를 수집할 수 있습니다.

| Mitum은 다음과 같은 *quic* (http) 요청 메시지를 포함한 방대한 디버깅 로그 메시지를 덤프합니다.

.. code-block:: json
    
    "l":"debug","module":"http2-server","ip":"127.0.0.1","user_agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0.3 Safari/605.1.15","req_id":"c30q3kqciaejf9nj79c0","status":200,"size":2038,"duration":0.541625,"content-length":0,"content-type":"","headers":{"Accept-Language":["en-us"],"Connection":["keep-alive"],"Upgrade-Insecure-Requests":["1"]},"host":"127.0.0.1:54320","method":"GET","proto":"HTTP/1.1","remote":"127.0.0.1:55617","url":"/","t":"2021-06-10T05:23:31.030086621Z","caller":"/Users/soonkukkang/go/pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210609043008-298f37780037/network/http.go:61","m":"request"

| ``–network-log`` 명령줄 옵션은 특정 파일들에 이 요청 메시지들을 수집할 수 있습니다.

.. code-block:: shell

    $ ./mitum node run \
        --log-level debug \
        --log-format json \
        --log ./mitum.log \
        --network-log ./mitum-request.log \
        ./tutorial.yml

| 여러 파일을 ``–network-log``와 ``–log``로 설정할 수 있습니다.
 
| ``–network-log`` 옵션은 digest API(http2)로부터 요청 로그를 수집할 것입니다.
| ``–network-log`` 옵션은 오직 ``node run`` 명령어와 함께 사용될 수 있습니다.

start-handover
''''''''''''''''''''''''''''''''''''''''''''''''''''

| ``start-handover`` 명령어로 운용중인 노드를 다른 노드로 교체하세요.

| ``start-handover`` 에 대한 자세한 설명은 :ref:`node handover` 를 확인하세요.

.. code-block:: shell

    $ ./mitum node start-handover <node address> <private key of node> <network-id> <new node url>

info
''''''''''''''''''''''''''''''''''''''''''''''''''''

| ``info`` 명령어로 노드 url을 사용해 원격 노드 정보를 확인하세요.

.. code-block:: shell

    $ ./mitum node info <node url>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum node info https://127.0.0.1:54321 --tls-insecure --pretty
    {
        "_hint": "mitum-currency-node-info-v0.0.1",
        "node": {
            "_hint": "base-node-v0.0.1",
            "address": "mc-nodesas",
            "publickey": "27P4S2FdDALmg4QzShCDTDne1pe8y1H2bE2uQCVpnqWpumpu",
            "url": "https://127.0.0.1:54321"
        },
        "network_id": "bWl0dW0=",
        "state": "CONSENSUS",
        "last_block": {
            "_hint": "block-manifest-v0.0.1",
            "hash": "5Z2SFA6DqYg8KdRPAD4uXAM7wpPE6vjyQ5iWqu4sc1yP",
            "height": 421,
            "round": 0,
            "proposal": "3H5wmRqvnburtEMqvkLh11vetbbdsdvHAkJRM6L6nu3Z",
            "previous_block": "J3if3xYD1wUQxUnm52UpddHT4Dipsd35bYGQxurMGnXm",
            "block_operations": null,
            "block_states": null,
            "confirmed_at": "2021-06-10T07:04:31.378699784Z",
            "created_at": "2021-06-10T07:04:31.390856784Z"
        },
        "version": "v0.0.0",
        "url": "https://127.0.0.1:54321",
        "policy": {
            "network_connection_timeout": 3000000000,
            "max_operations_in_seal": 10,
            "max_operations_in_proposal": 100,
            "interval_broadcasting_init_ballot": 1000000000,
            "wait_broadcasting_accept_ballot": 1000000000,
            "threshold": 100,
            "interval_broadcasting_accept_ballot": 1000000000,
            "timeout_waiting_proposal": 5000000000,
            "timespan_valid_ballot": 60000000000,
            "interval_broadcasting_proposal": 1000000000,
            "suffrage": "{\"type\":\"\",\"cache_size\":10,\"number_of_acting\":1}"
        },
        "suffrage": [
            {
                "_hint": "base-node-v0.0.1",
                "address": "mc-nodesas",
                "publickey": "27P4S2FdDALmg4QzShCDTDne1pe8y1H2bE2uQCVpnqWpumpu",
                "url": "https://127.0.0.1:54321"
            }
        ]
    }

.. _storage command:

---------------------------------------------------
storage
---------------------------------------------------

| ``storage`` 명령어는 블록 데이터의 내려받기, 검증, 복구를 도와줍니다.

| ``storage`` 의 하위 명령어는 다음과 같습니다.

* ``download``
* ``verify-blockdata``
* ``verify-database``
* ``clean``
* ``clean-by-height``
* ``restore``
* ``set-blockdatamaps``

download
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``download`` 를 통해 특정 block height의 블록 데이터를 내려받으세요.

.. code-block:: shell

    $ ./mitum storage download --node=quic://localhost:54321 <data type> <height> ...

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage download --tls-insecure --node=https://127.0.0.1:54321  --save=data all -- -1 0 1 2 3 4 5
    2021-06-08T10:50:08.018561Z INF saved file=data/000/000/000/000/000/000/0_1/-1-manifest-48cfbadd18b892bfd0a6fa230ff0c5f719bd517d37f594012aeca7244ef12599.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.018531Z INF saved file=data/000/000/000/000/000/000/000/0-manifest-307ffa78d4ce5e32e25347f5ec8ee626e44d41e55f565c2082ac00f8f128dbd9.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.058628Z INF saved file=data/000/000/000/000/000/000/0_1/-1-operations-0fedf0c3ccb08aea5694e04a382ca04fb1338dfc9c2c408fe6296c93c0931124.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.068871Z INF saved file=data/000/000/000/000/000/000/000/0-operations-d17d5b941aec3c100a43e2c228bca4134473bb9c78dcf567bdd8b9e12e5cc928.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.12423Z INF saved file=data/000/000/000/000/000/000/000/0-operations_tree-45aff89f7084384fdecfac9689b75168a33f03bf6ba677ad085a6ac8fdf2bd12.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.130027Z INF saved file=data/000/000/000/000/000/000/0_1/-1-operations_tree-d0c45c5292593853052aba6d3f410c93f6cc4473e7873ded2d623069adfc0025.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.162735Z INF saved file=data/000/000/000/000/000/000/000/0-states-73ac164e67fb49877b132aaaae2f7adf92cc237ef0e63db30f3013c283fb7100.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.172536Z INF saved file=data/000/000/000/000/000/000/0_1/-1-states-0fedf0c3ccb08aea5694e04a382ca04fb1338dfc9c2c408fe6296c93c0931124.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.215233Z INF saved file=data/000/000/000/000/000/000/000/0-states_tree-7155e9c9f393943429f9341f22cba749203eaa2effd51bbbdb9b97c899cac62e.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.217385Z INF saved file=data/000/000/000/000/000/000/0_1/-1-states_tree-d0c45c5292593853052aba6d3f410c93f6cc4473e7873ded2d623069adfc0025.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.278019Z INF saved file=data/000/000/000/000/000/000/000/0-init_voteproof-dab53369d715fc74ad750d95f1ceb859d62009165a76ea3368399da2b16bf4d7.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.287794Z INF saved file=data/000/000/000/000/000/000/0_1/-1-init_voteproof-812c550f7595c4c949d2255217a343864bdd878b09d124235d7db07758620bc7.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.319642Z INF saved file=data/000/000/000/000/000/000/000/0-accept_voteproof-09fd08050476a5d0a343154aaa0325809d721004b49cba303a58300b7415235e.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.334284Z INF saved file=data/000/000/000/000/000/000/0_1/-1-accept_voteproof-812c550f7595c4c949d2255217a343864bdd878b09d124235d7db07758620bc7.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.399426Z INF saved file=data/000/000/000/000/000/000/000/0-suffrage_info-038aa59ed7db04c96d11405336c7a2d1cb8ad6df5a18d66f8f3bf2919c6767f8.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.591648Z INF saved file=data/000/000/000/000/000/000/0_1/-1-suffrage_info-038aa59ed7db04c96d11405336c7a2d1cb8ad6df5a18d66f8f3bf2919c6767f8.jsonld.gz height=-1 module=command-block-download
    2021-06-08T10:50:08.613875Z INF saved file=data/000/000/000/000/000/000/000/0-proposal-81c03f9c912591796ae5f3dbaab85bc91d7ca4031413787abb3068c5efa78360.jsonld.gz height=0 module=command-block-download
    2021-06-08T10:50:08.750795Z INF saved file=data/000/000/000/000/000/000/0_1/-1-proposal-812c550f7595c4c949d2255217a343864bdd878b09d124235d7db07758620bc7.jsonld.gz height=-1 module=command-block-download

map
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``download map`` 을 사용해 blockdata map을 내려받으세요.

| 자세한 내용은 :ref:`block data` 을 참고하세요.

.. code-block:: shell

    $ ./mitum storage download map --node=https://localhost:54321 <height> ...

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage download map --tls-insecure --node=https://127.0.0.1:54321 0 --pretty
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "DvYK11jZ8KWafAGPssypdNMRwwXwJJTKeyzTAx4JNnwc",
        "height": 10,
        "block": "AnjD39fpP6cJKVhnSfJxPfQ8sxrVwCrKhm1zWjb38dUS",
        "created_at": "2021-06-10T06:37:42.251Z",
        "items": {
            "accept_voteproof": {
            "type": "accept_voteproof",
            "checksum": "03dd3c2ce852729ff52ec7dcd31a2a1532656fbcea12a28438c3e84c8146c753",
            "url": "file:///000/000/000/000/000/000/010/10-accept_voteproof-03dd3c2ce852729ff52ec7dcd31a2a1532656fbcea12a28438c3e84c8146c753.jsonld.gz"
            },
            "init_voteproof": {
            "type": "init_voteproof",
            "checksum": "70d59dc3e84ddd06d319e9d38d68a976b09a816fbe5a5fdef42f5b80908b0fa0",
            "url": "file:///000/000/000/000/000/000/010/10-init_voteproof-70d59dc3e84ddd06d319e9d38d68a976b09a816fbe5a5fdef42f5b80908b0fa0.jsonld.gz"
            },
            "states": {
            "type": "states",
            "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
            "url": "file:///000/000/000/000/000/000/010/10-states-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "proposal": {
            "type": "proposal",
            "checksum": "ccd31f6627aa3cc6e9768b318f8cfd8e7f371b907f329fb89d692c7aea2ef465",
            "url": "file:///000/000/000/000/000/000/010/10-proposal-ccd31f6627aa3cc6e9768b318f8cfd8e7f371b907f329fb89d692c7aea2ef465.jsonld.gz"
            },
            "suffrage_info": {
            "type": "suffrage_info",
            "checksum": "f8955c57fb4a7dc48e71973af01852008c76ae4bb5487f8d6fccebcc10e5412e",
            "url": "file:///000/000/000/000/000/000/010/10-suffrage_info-f8955c57fb4a7dc48e71973af01852008c76ae4bb5487f8d6fccebcc10e5412e.jsonld.gz"
            },
            "manifest": {
            "type": "manifest",
            "checksum": "1f21552b0d7a11c0397c7429849a0f611d9681f70cecd5165e21fcbd5276a880",
            "url": "file:///000/000/000/000/000/000/010/10-manifest-1f21552b0d7a11c0397c7429849a0f611d9681f70cecd5165e21fcbd5276a880.jsonld.gz"
            },
            "operations": {
            "type": "operations",
            "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
            "url": "file:///000/000/000/000/000/000/010/10-operations-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "states_tree": {
            "type": "states_tree",
            "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
            "url": "file:///000/000/000/000/000/000/010/10-states_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
            "operations_tree": {
            "type": "operations_tree",
            "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
            "url": "file:///000/000/000/000/000/000/010/10-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            }
        },
        "writer": "blockdata-writer-v0.0.1"
    }

verify-blockdata
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``verify-blockdata`` 을 사용해 로컬 스토리지의 블록 데이터를 검증하세요.

.. code-block:: shell

    $ ./mitum storage verify-blockdata <blockdata path>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage verify-blockdata data --network-id=mitum --verbose
    2021-06-08T10:52:03.249204Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/cmd.go:86 > maxprocs: Leaving GOMAXPROCS=8: CPU quota undefined module=command-blockdata-verify
    2021-06-08T10:52:03.250015Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/cmd.go:95 > flags parsed flags={"CPUProf":"mitum-cpu.pprof","EnableProfiling":false,"LogColor":false,"LogFile":null,"LogFormat":"terminal","LogLevel":"info","LogOutput":{},"MemProf":"mitum-mem.pprof","NetworkID":"bWl0dW0=","Path":"data","TraceProf":"mitum-trace.pprof","Verbose":true} module=command-blockdata-verify
    2021-06-08T10:52:03.250188Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:38 > trying to verify blockdata module=command-blockdata-verify path=data
    2021-06-08T10:52:03.250315Z INF ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:107 > last height found last_height=5 module=command-blockdata-verify
    2021-06-08T10:52:03.250607Z INF ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/verify_storage.go:53 > checking manifests module=command-blockdata-verify
    2021-06-08T10:52:03.255675Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/verify_storage.go:109 > manifests loaded heights=[-1,6] module=command-blockdata-verify
    2021-06-08T10:52:03.255766Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/verify_storage.go:121 > manifests checked heights=[-1,6] module=command-blockdata-verify
    2021-06-08T10:52:03.258293Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=0 module=command-blockdata-verify
    2021-06-08T10:52:03.257947Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=1 module=command-blockdata-verify
    2021-06-08T10:52:03.259131Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=4 module=command-blockdata-verify
    2021-06-08T10:52:03.257772Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=5 module=command-blockdata-verify
    2021-06-08T10:52:03.260384Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=2 module=command-blockdata-verify
    2021-06-08T10:52:03.260419Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=-1 module=command-blockdata-verify
    2021-06-08T10:52:03.260606Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:257 > block data files checked height=3 module=command-blockdata-verify
    2021-06-08T10:52:03.274069Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=-1 module=command-blockdata-verify
    2021-06-08T10:52:03.279165Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=3 module=command-blockdata-verify
    2021-06-08T10:52:03.279179Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=2 module=command-blockdata-verify
    2021-06-08T10:52:03.279223Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=1 module=command-blockdata-verify
    2021-06-08T10:52:03.279267Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=4 module=command-blockdata-verify
    2021-06-08T10:52:03.279344Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=5 module=command-blockdata-verify
    2021-06-08T10:52:03.281481Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:187 > block checked height=0 module=command-blockdata-verify
    2021-06-08T10:52:03.281569Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/blockdata_verify.go:87 > blockdata verified module=command-blockdata-verify
    .....

verify-database
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``verify-database`` 명령어는 블록데이터와의 비교를 통해 database를 검증합니다.

.. code-block:: shell

    $ ./mitum storage verify-database <database uri> <blockdata path>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage verify-database mongodb://127.0.0.1:27017/n0_mc blockfs --network-id=mitum --verbose
    2021-06-08T10:56:20.879671Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/cmd.go:86 > maxprocs: Leaving GOMAXPROCS=8: CPU quota undefined module=command-database-verify
    2021-06-08T10:56:20.879921Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/cmd.go:95 > flags parsed flags={"CPUProf":"mitum-cpu.pprof","EnableProfiling":false,"LogColor":false,"LogFile":null,"LogFormat":"terminal","LogLevel":"info","LogOutput":{},"MemProf":"mitum-mem.pprof","NetworkID":"bWl0dW0=","Path":"data","TraceProf":"mitum-trace.pprof","URI":"mongodb://127.0.0.1:27017/mc","Verbose":true} module=command-database-verify
    2021-06-08T10:56:20.880018Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:310 > processed from_process= module=process-manager process=init
    2021-06-08T10:56:20.880066Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:310 > processed from_process=time-syncer module=process-manager process=config
    2021-06-08T10:56:21.038454Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/util/localtime/time_sync.go:67 > started interval=120000 module=time-syncer server=time.google.com
    2021-06-08T10:56:21.042330408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:310 > processed from_process=init module=process-manager process=time-syncer
    2021-06-08T10:56:21.042835408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:359 > hook processed from=encoders hook=add_hinters module=process-manager
    2021-06-08T10:56:21.042884408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:310 > processed from_process=init module=process-manager process=encoders
    2021-06-08T10:56:21.203404408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:310 > processed from_process=init module=process-manager process=database
    2021-06-08T10:56:21.203608408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:359 > hook processed from=blockdata hook=check_blockdata_path module=process-manager
    2021-06-08T10:56:21.203899408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/database_verify.go:207 > block found block={"hash":"CzF6t6ePyBaz6RnSjw6YRhwKsxA5sRnhHwQJvK8xVgMR","height":0,"round":0} module=command-database-verify
    2021-06-08T10:56:21.204001408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:359 > hook processed from=blockdata hook=check_storage module=process-manager
    2021-06-08T10:56:21.204054408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/pm/processes.go:310 > processed from_process=init module=process-manager process=blockdata
    2021-06-08T10:56:21.204357408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/database_verify.go:74 > trying to verify database module=command-database-verify path=data uri=mongodb://127.0.0.1:27017/mc
    2021-06-08T10:56:21.204424408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/database_verify.go:100 > verifying database module=command-database-verify
    2021-06-08T10:56:21.204941408Z INF ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/verify_storage.go:53 > checking manifests module=command-database-verify
    2021-06-08T10:56:21.210215408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/verify_storage.go:109 > manifests loaded heights=[-1,1] module=command-database-verify
    2021-06-08T10:56:21.210355408Z DBG ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/verify_storage.go:121 > manifests checked heights=[-1,1] module=command-database-verify
    2021-06-08T10:56:21.210456408Z INF ../../../../pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210605063447-f720096b150d/launch/cmds/database_verify.go:105 > database verified module=command-database-verify

clean
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``clean`` 명령어는 블록데이터와 데이터베이스를 정리합니다.

.. code-block:: shell

    $ ./mitum storage clean <node design file>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage clean node.yml

clean-by-height
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``clean-by-height`` 는 특정 height 위로 블록데이터와 데이터베이스를 정리합니다.

.. code-block:: shell

    $ ./mitum storage clean-by-height <node design file> <height>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage clean-by-height node.yml 54234

restore
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``restore`` 명령어로 내려받은 블록데이터로부터 전체 데이터베이스를 복구하세요.

| ``restore`` 사용 시, 블록데이터 뿐만 아니라 digest API에 사용되는 데이터도 생성됩니다. yml 파일 설정의 ``network id`` 이 내려받은 노드의 것과 같은지 확인하세요.

* ``–concurrency`` 옵션으로 여러 개의 블록 데이터가 동시에 복구될 수 있습니다.
* 존재하는 mongodb 데이터를 삭제하고 복구하고 싶다면 ``–clean`` 을 사용하세요.
* 실제 복구하지 않고 오직 blockdata를 확인하려면 ``–dryrun`` 를 사용하세요.
* ``–one`` 옵션으로 특정 blockdata 디렉토리를 특정하면, 그것들을 하나씩 복구할 수 있습니다.

.. code-block:: shell

    $ ./mitum storage restore <node design file>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum storage restore node.yml --concurrency 10
    2021-06-08T11:00:34.304594Z INF prepare to run module=command-restore
    2021-06-08T11:00:34.304656Z INF prepared module=command-restore
    2021-06-08T11:00:34.743477729Z INF block restored height=-1 module=command-restore
    2021-06-08T11:00:34.828859729Z INF block restored height=0 module=command-restore
    2021-06-08T11:00:34.829060729Z INF restored module=command-restore
    2021-06-08T11:00:35.833206729Z INF stopped module=command-restore

set-blockdatamaps
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``set-blockdatamaps`` 는 여러 BlockDataMap을 업데이트합니다.

| 자세한 내용은 :ref:`block data` 를 참고하세요.

.. code-block:: shell

    $ ./mitum storage set-blockdatamaps <deploy key> <maps file> [<node url>]

.. _deploy command:

---------------------------------------------------
deploy
---------------------------------------------------

| 노드의 deploy key를 생성하고 관리하기 위해 ``deploy key`` 를 사용하세요.

| ``deploy key`` 의 하위 명령어는 다음과 같습니다.

* ``new``
* ``keys``
* ``key``
* ``revoke``

.. note::

    **What is deploy key?**

    (BlockDataMap 변경과 같은)노드의 업데이트는 반드시 노드 소유자에 의해 허용되어야 합니다.
    노드 소유자는 노드를 관리할 때 자신을 증명하기 위한 key를 사용합니다.
   
    하지만 노드 관리에 노드의 개인키를 직접 사용하는 것은 위험할 수 있습니다.
    따라서 우리는 노드 관리 등을 위한 교체 가능하고 관리 가능한 키가 필요합니다.
    
    ``deploy key`` 는 이런 목적으로 사용됩니다.

new
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``new`` 를 사용해 새로운 deploy key를 생성하고 노드에 등록하세요.

.. code-block:: shell

    $ ./mitum deploy key new <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr

    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ ./mitum deploy key new $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    {"key":"d-fc4179e7-2ff3-4372-bd83-f70526bed476","added_at":"2021-06-09T09:31:22.321675852Z"}
    2021-06-09T09:31:22.320055Z INF new deploy key module=command-deploy-key-new

keys
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``keys`` 를 사용해 노드에 등록된 deploy key 리스트를 확인하세요.

.. code-block:: shell

    $ ./mitum deploy key keys <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr
    
    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ ./mitum deploy key keys $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    [{"key":"d-974702df-89a7-4fd1-a742-2d66c1ead6cd","added_at":"2021-06-09T03:14:33.9Z"},{"key":"d-2897ced4-ceb5-4e11-be81-3139350c9c55","added_at":"2021-06-09T03:56:49.393Z"},{"key":"d-fc4179e7-2ff3-4372-bd83-f70526bed476","added_at":"2021-06-09T09:31:22.321675852Z"}]

key
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``key`` 를 사용해 노드에 deploy key가 존재하는지 확인하세요.

.. code-block:: shell

    $ ./mitum deploy key key <deploy key> <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr

    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ ./mitum deploy key $DEPLOY_KEY $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    {"key":"d-974702df-89a7-4fd1-a742-2d66c1ead6cd","added_at":"2021-06-09T03:14:33.9Z"}

revoke
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``revoke`` 를 사용해 노드에서 deploy key를 취소하세요.

.. code-block:: shell

    $ ./mitum deploy key revoke <deploy key> <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr
    
    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ ./mitum deploy key revoke $DEPLOY_KEY $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    2021-06-09T09:36:19.763339Z INF deploy key revoked deploy_key=d-974702df-89a7-4fd1-a742-2d66c1ead6cd module=command-deploy-key-revoke

.. _version command:

---------------------------------------------------
version
---------------------------------------------------

| ``version`` 을 사용해 Mitum Currency의 버전을 확인하세요.

.. code-block:: shell

    $ ./mitum version

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum version
    v0.0.1

.. _quic-client command:

---------------------------------------------------
quic-client
---------------------------------------------------

| ``quic-client`` 의 응답은 API의 *node info* 응답과 형태가 같습니다.

.. code-block:: shell

    $ ./mitum quic-client <node-url>

| **EXAMPLE**

.. code-block:: shell

    $ ./mitum quic-client https://3.35.171.179:54321/
    {
        "_hint": "node-info-v0.0.1",
        "node": {
            "_hint": "base-node-v0.0.1",
            "address": "node4sas",
            "publickey": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu"
        },
        "network_id": "bWl0dW0=",
        "state": "CONSENSUS",
        "last_block": {
            "_hint": "block-manifest-v0.0.1",
            "hash": "GBQqKbR6pAs8gWzNmf5mrHGUYUmjs829NVX4WuYz7uzf",
            "height": 994024,
            "round": 0,
            "proposal": "HbxL38mNX8NGTqErNE3Hw5w639qKpbEwC4SkkCDZvrYB",
            "previous_block": "5rPQHEunbAw15YG3GaZneYKQpxsKRgQuThW6Yd7KBZb",
            "block_operations": null,
            "block_states": null,
            "confirmed_at": "2022-01-19T05:58:14.623577286Z",
            "created_at": "2022-01-19T05:58:14.631963244Z"
        },
        "version": "v0.0.1-stable-383cf0c-20211224",
        "policy": {
            "timespan_valid_ballot": 60000000000,
            "network_connection_timeout": 3000000000,
            "threshold": 100,
            "max_operations_in_seal": 10,
            "max_operations_in_proposal": 100,
            "interval_broadcasting_proposal": 1000000000,
            "wait_broadcasting_accept_ballot": 1000000000,
            "timeout_waiting_proposal": 5000000000,
            "interval_broadcasting_init_ballot": 1000000000,
            "interval_broadcasting_accept_ballot": 1000000000,
            "suffrage": "{\"type\":\"\",\"cache_size\":10,\"number_of_acting\":1}"
        },
        "suffrage": [
            {
                "address": "node4sas",
                "publickey": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu",
                "conninfo": {
                    "_hint": "http-conninfo-v0.0.1",
                    "url": "https://3.35.171.179:54321",
                    "insecure": true
                }
            }
        ],
        "conninfo": {
            "_hint": "http-conninfo-v0.0.1",
            "url": "https://3.35.171.179:54321",
            "insecure": true
        }
    }
