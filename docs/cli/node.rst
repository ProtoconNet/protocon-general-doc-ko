===================================================
Node Command
===================================================

| ``node``는 노드를 초기화하고 운용합니다.

| ``node``의 하위 명령어는 다음과 같습니다.

* ``init``
* ``run``
* ``start-handover``
* ``info``

---------------------------------------------------
init
---------------------------------------------------

| ``init`` 명령어로 노드 구성을 가지고 있는 노드 디자인 파일을 사용해 **노드를 초기화**하세요.

| ``init`` command에 대한 자세한 설명은 :ref:`node init`를 참고하세요.

.. code-block:: shell

    $ ./mc node init <node design file>

---------------------------------------------------
run
---------------------------------------------------

| ``run``을 사용해 노드 구성을 가지고 있는 노드 디자인 파일에 따라 **노드를 운용**하세요.

| See :ref:`node run` for a detailed explanation of ``run`` command.

.. code-block:: shell

    $ ./mc node run <node design file>

---------------------------------------------------
start-handover
---------------------------------------------------

| ``start-handover`` 명령어로 **운용중인 노드를 다른 노드로 교체**하세요.

| ``start-handover``에 대한 자세한 설명은 :ref:`node handover`를 확인하세요.

.. code-block:: shell

    $ ./mc node start-handover <node address> <private key of node> <network-id> <new node url>

---------------------------------------------------
info
---------------------------------------------------

| ``info`` 명령어로 노드 url을 사용해 **원격 노드 정보를 확인**하세요.

.. code-block:: shell

    $ ./mc node info <node url>

| **EXAMPLE**

.. code-block:: shell

    $ ./mc node info https://127.0.0.1:54321 --tls-insecure | jq
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