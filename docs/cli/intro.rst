.. _CLI:

===================================================
Command Line Interface
===================================================

| 이 장에서 Mitum 및 Mitum 모델이 제공하는 명령어들과 그 사용 방법을 소개합니다.

| 기본 Mitum 프로그램이 제공하는 주요 명령어는 다음과 같습니다.

* ``version``
* ``node``
* ``key``
* ``seal``
* ``storage``
* ``deploy``
* ``quic-client``

| ``Run`` 페이지를 거쳐왔다면 이미 ``node`` 명령어는 익숙할 것입니다.

| 각 모델의 operation은 ``seal`` 명령어를 통해 생성할 수 있습니다.

| ``version`` 와 ``quic-client`` 를 사용하는 방법은 간단합니다. 각 명령어에 대해 하나씩 설명합니다.

---------------------------------------------------
Summary
---------------------------------------------------

| 예를 들어, 다음은 Mitum Currency가 제공하는 모든 명령어입니다.

| 특히 아래 명령어들은 Mitum Currency 모델을 확장한 모델에서만 사용가능합니다. 그 외 다른 명령어들은 일반적으로 모든 모델에서 사용가능합니다.

* ``seal create-account``
* ``seal transfer``
* ``seal key-updater``
* ``seal currency-register``
* ``seal currency-policy-updater``
* ``seal suffrage-inflation``

.. code-block:: shell

    $ ./mitum --help

    Usage: mitum-currency <command>

    mitum-currency tool

    Flags:
        -h, --help    Show context-sensitive help.

    Commands:
        version                         version

        node                                node
            init                            initialize node
                <node design file>          node design file
            run                             run node
                <node design file>          node design file
            info                            node information
                <node url>                  remote mitum url
            start-handover                  start handover
                <node address>
                <private key of node>
                <network-id>
                <new node url>              new node url

        key                                 key
            new                             new keypair
            verify                          verify key
                <key>                       key
            address                         generate address from key
                [<threshold>]               threshold for keys (default: 100)
                [<key> ...]                 key for address (ex: "<public key>,<weight>")
            sign                            signature signing
                <privatekey>                privatekey
                <signature base>            signature base for signing

        seal                                seal
            send                            send seal to remote mitum node
                <privatekey>                privatekey for sign
            create-account                  create new account
                <privatekey>                privatekey to sign operation
                <sender>                    sender address
                <currency-amount> ...       amount (ex: "<currency>,<amount>")
            transfer                        transfer big
                <privatekey>                privatekey to sign operation
                <sender>                    sender address
                <receiver>                  receiver address
                <currency-amount> ...       amount (ex: "<currency>,<amount>")
            key-updater                     update keys
                <privatekey>                privatekey to sign operation
                <target>                    target address
                <currency>                  currency id
            currency-register               register new currency
                <privatekey>                privatekey to sign operation
                <currency-id>               currency id
                <genesis-amount>            genesis amount
                <genesis-account>           genesis-account address for genesis balance
            currency-policy-updater         update currency policy
                <privatekey>                privatekey to sign operation
                <currency-id>               currency id
            suffrage-inflation              suffrage inflation operation
                <privatekey>                privatekey to sign operation
                <inflation item> ...        ex: "<receiver address>,<currency>,<amount>"
            sign                            sign seal
                <privatekey>                sender's privatekey
            sign-fact                       sign facts of operation seal
                <privatekey>                sender's privatekey

        storage                             storage
            download                        download block data
                <data type>                 data type of block data,
                                            {manifest,operations,operations_tree,states,states_tree,init_voteproof,accept_voteproof,suffrage_info,proposal all}
                <height> ...                heights of block
            verify-blockdata                verify block data
                <blockdata path>
            verify-database                 verify database
                <database uri>
                <blockdata path>
            clean                           clean storage
                <node design file>          node design file
            clean-by-height                 clean storage by height
                <node design file>          node design file
                <height>                    height of block
            restore                         restore blocks from blockdata
                <node design file>          node design file
            set-blockdatamaps               set blockdatamaps
                <deploy key>
                <maps file>                 set blockdatamap file
                [<node url>]                remote mitum url; default: quic://localhost:54321

        deploy                              deploy
            key                             deploy key
                new                         request new deploy key
                    <private key of node>
                    <network-id>
                    [<node url>]            remote mitum url; default: quic://localhost:54321
            keys                            deploy keys
                <private key of node>
                <network-id>
                [<node url>]                remote mitum url; default: quic://localhost:54321
            key                             deploy key
                <deploy key>
                <private key of node>
                <network-id>
                [<node url>]                remote mitum url; default: quic://localhost:54321
            revoke                          revoke deploy key
                <deploy key>
                <private key of node>
                <network-id>
                [<node url>]                remote mitum url; default: quic://localhost:54321

        quic-client                         quic-client
            <node url>                      remote mitum url

    Run "mitum-currency <command> --help" for more information on a command.