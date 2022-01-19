===================================================
Run
===================================================

| Here we will explain the process for running the node.

.. note::

    * A node can find out the addresses of all nodes by using the node discovery protocol.
    * *Digest API* is included in Mitum Currency, so API service is provided by default.
    * Please check `Configuration <https://protocon-general-doc.readthedocs.io/en/develop/docs/run/config.html>`_ for Digest setting.
    * If Digest is not set, data for API service must be processed separately.

---------------------------------------------------
Running the Standalone Node
---------------------------------------------------

| Before running a node, please prepare tutorial.yml by refering to `Configuration <https://protocon-general-doc.readthedocs.io/en/develop/docs/run/config.html>`_.

node init
'''''''''''''''''''''''''''''''''''''''''''''''''''

| First, the *genesis block* and *genesis account* must be created. The main currency is issued through the creation of the *genesis block* and stored in the balance of the *genesis account*.

* tutorial.yml : config file

.. code-block:: shell

    $ ./mc node init --log-level info ./tutorial.yml
    2021-06-10T05:13:09.232802Z INF dryrun? dryrun=false module=command-init
    2021-06-10T05:13:09.235942Z INF prepare to run module=command-init
    2021-06-10T05:13:09.236013Z INF prepared module=command-init
    2021-06-10T05:13:09.780335419Z INF genesis block created block={"hash":"6HjkXEhTNhPzUTG167jsTEany3dHebDQ5cKGNTNEzcgh","height":0} module=command-init
    2021-06-10T05:13:10.786661419Z INF stopped module=command-init
    ...

    $ echo $?
    0

.. note::

    If already saved block data is found, an error ``environment already exists: block=0`` occurs. To reset the error and ignore it, run it by adding the ``--force`` option.    
    
    ``$ ./mc --log-level info init ./tutorial.yml --force``

node run
'''''''''''''''''''''''''''''''''''''''''''''''''''

| When the node is run, the blockchain’s storage status and consensus participation status are changed to *SYNC*, *JOIN*, and *CONSENSUS* modes, and block creation starts.

.. code-block:: shell

    $ ./mc node run --log-level info ./tutorial.yml
    2021-06-10T05:14:08.225487Z INF dryrun? dryrun=false module=command-run
    2021-06-10T05:14:08.228797Z INF prepare to run module=command-run
    2021-06-10T05:14:08.228869Z INF prepared module=command-run
    2021-06-10T05:14:09.706271049Z INF new blocks found to digest last_block=-2 last_manifest=0 module=command-run
    2021-06-10T05:14:09.827980049Z INF digested new blocks module=command-run
    2021-06-10T05:14:09.828967049Z INF trying to start http2 server for digest API bind=https://localhost:54320 module=command-run publish=https://localhost:54320
    2021-06-10T05:14:11.894638049Z INF new block stored block={"hash":"CC57VpSKPozBRABPnznyMk6QY4GHn7CiSH4zSZBs8Rri","height":1,"round":0} elapsed=17.970959 module=basic-consensus-state proposal_hash=DJBgmoAJ4ef7h7iF6E3gTQ83AjWxbGDGQrmDSiQMrfya voteproof_id=BAg2HCNfBenFebuCM4P4HkDfF1off8FCBcSejdK1j7w6
    2021-06-10T05:14:11.907600049Z INF block digested block=1 module=digester

| If the node is a suffrage node, the addresses of other live suffrage nodes can be found using the *Node discovery protocol*. The node discovery feature is only supported when the node is a suffrage node.

* When the suffrage node starts up, it is possible to determine the network information of all suffrage nodes without publish url information of all suffrage nodes.
* For node discovery, a node must set the address of one or more suffrage nodes it knows to a discovery url at startup.

| To specify the discovery url, use the ``–discovery`` command line option.

.. code-block:: shell
    
    $ ./mc node run n0.yml --discovery "https://n1#insecure" --discovery "https://n2#insecure"

* Even if a node does not set the discovery url by itself, if another suffrage node designates this node as a discovery node, the publish url of other nodes is known by the gossip protocol. If the nodes specified by discovery are not running, it keeps trying until it succeeds.
* Again, node discovery only works with suffrage nodes. For nodes not included in the suffrage node list, the urls of other suffrage nodes are still specified in the node settings.
* If you set the log level to info, you can easily check the information of the newly created block.

| ``–log`` command line option can collect logs to the specific files.

| Mitum dumps huge debugging log messages, including *quic*(http) request message like this,

.. code-block:: json
    
    "l":"debug","module":"http2-server","ip":"127.0.0.1","user_agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0.3 Safari/605.1.15","req_id":"c30q3kqciaejf9nj79c0","status":200,"size":2038,"duration":0.541625,"content-length":0,"content-type":"","headers":{"Accept-Language":["en-us"],"Connection":["keep-alive"],"Upgrade-Insecure-Requests":["1"]},"host":"127.0.0.1:54320","method":"GET","proto":"HTTP/1.1","remote":"127.0.0.1:55617","url":"/","t":"2021-06-10T05:23:31.030086621Z","caller":"/Users/soonkukkang/go/pkg/mod/github.com/spikeekips/mitum@v0.0.0-20210609043008-298f37780037/network/http.go:61","m":"request"

| ``–network-log`` command line option can collect these request messages to the specific files.

.. code-block:: shell

    $ ./mc node run \
        --log-level debug \
        --log-format json \
        --log ./mitum.log \
        --network-log ./mitum-request.log \
        ./tutorial.yml

| Multiple file can be set to ``–network-log`` and ``–log``.

| In mitum Currency, ``–network-log`` option will also collect the requests log from *digest API*(http2)
| ``–network-log`` option is only available in ``node run`` command.
