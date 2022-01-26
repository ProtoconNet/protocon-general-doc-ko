.. _seal command:

===================================================
Seal Command
===================================================

| ``seal`` 은 seal에 포함된 다양한 operation들을 실행하는데 도움을 주는 명령어입니다.

| operation과 관련된 명령어는 다음과 같습니다.

* ``create-account``
* ``transfer``
* ``key-updater``
* ``currency-register``
* ``currency-policy-updater``
* ``suffrage-inflation``

| ``seal``은 서명을 만들고 seal을 전송하도록 지원합니다.

| signature generation과 transmission에 관련된 명령어는 다음과 같습니다.

* ``send``
* ``sign``
* ``sign-fact``

| operation이 제대로 처리되었는지는 api를 통해 확인할 수 있습니다.

| 더 많은 정보는 :ref:`confirm success` 를 참고하세요.

---------------------------------------------------
create-account
---------------------------------------------------

| ``create-account`` 로 계정을 생성하세요.

.. code-block:: shell

    $ ./mc seal create-account --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency,amount> --key=KEY@... --threshold= 

* KEY: <pub key, weight>

| **EXAMPLE**

| ``ac0`` 와 ``ac1`` 두 계정을 생성하는 과정을 예제로 설명합니다.

| 키페어를 생성하는 방법은 :ref:`key command` 을 참고하세요.

| ``ac0`` 를 생성하는 operation은 다음과 같습니다.

1. **Create Single Sig Account**

| 생성할 계정의 정보는 다음과 같습니다.

.. code-block:: none

    sender's account - who create new account
    private key: L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr
    address: Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    ac0
    public key - weight: cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu - 100
    threshold: 100
    initial balance: 500 MCC


.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ SENDER_PRV=L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr

    $ SENDER_ADDR=Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    $ AC0_PUB=cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu

    $ ./mc seal create-account --network-id=$NETWORK_ID $SENDER_PRV $SENDER_ADDR MCC,500 --key=$AC0_PUB,100 --threshold=100 | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "Xr7HS7rnbfxTrNbr6qRJ64on6KFuMzvJf5Z6BGqVZsX",
        "body_hash": "EJ93htxhUh2edJhBujMCHhpvGGHQoBic8KQ7VzggxKw1",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZUffVp3gmKD2WJA6756SeDy16d3PF6Ym15HBL89rs1YhT1cW4zVnWD17mhBdhfhutu3848GPd9zTMDqUFmkE8rUWmCs",
        "signed_at": "2021-06-10T14:06:17.60152Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "8ezjZDuC44U2ZFPDkebMyLEYNQBPUUnRjHyfSTeQs9gk",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "F1o51xXWnnQYUVV6JA44beJeKKxuJi3Tv8DzvREodHhA",
                    "token": "MjAyMS0wNi0xMFQxNDowNjoxNy41OTczMDNa",
                    "sender": "Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "500",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "381yXYyRo91cqu5gFp5GtHWCiYmsssbFxx95MaL8gH4koBCZ5AfnRqYEpWMxcxgKmeEWsRPVJ8zWytAMLiA9zQes9qGnbcj8",
                        "signed_at": "2021-06-10T14:06:17.601089Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| 위 json 메시지가 seal에 담겨 전송됩니다.

2. **Create Multi Sig Account**

.. note::

    * Mitum Currency에서 한 계정에 의해 서명된 다수의 operation은 한 블록에서 처리될 수 없습니다.
    * 예를 들어 ``ac0`` 에서 각각 ``ac1``, ``ac2`` 로 5 amount를 전송하는 두 별개의 operation은 한 번에 처리될 수 없습니다.
    * 이 경우 처음 도착한 operation만이 처리되며 나머지는 무시됩니다.

| sender가 두 계정 ``ac0`` 와 ``ac1`` 을 하나의 seal로 한 번에 생성하려 한다고 가정해보세요. 그러면 sender는 ``ac0`` 와 ``ac1`` 각각에 대한 item을 생성하여 operation에 추가해야 합니다.

| 즉, sender는 두 게정을 생성하는 오직 하나의 operation을 생성하고 seal에 담아 전송해야 합니다. 이 seal은 성공적으로 처리될 것입니다. sender가 같은 여러 개의 operation을 생성할 필요가 없습니다.

.. code-block:: none

    sender's account - who create new account
    private key: L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr
    address: Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    ac0
    public key - weight: cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu - 100
    threshold: 100
    initial balance: 50 MCC

    ac1
    public key - weight: sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu - 100
    threshold: 100
    initial balance: 50 MCC

| 다음과 같은 명령어를 실행하세요.

.. code-block:: shell

    $ NETWORK_ID=mitum

    $ NODE=https://127.0.0.1:54321

    $ SENDER_PRV=L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr

    $ SENDER_ADDR=Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    $ CURRENCY_ID=MCC

    $ AC0_PUB=cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu

    $ AC1_PUB=sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu

    $ ./mc seal create-account --network-id=$NETWORK_ID \
        $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 \
            --key=$AC0_PUB,100 |
        ./mc seal create-account --network-id=$NETWORK_ID \
            $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 \
            --key=$AC1_PUB,100 --seal=- | \
        ./mc seal send --network-id="$NETWORK_ID" \
            $SENDER_PRV --seal=- --node=$NODE --tls-insecure | jq -R '. as $line | try fromjson catch $line'
    {
        "_hint": "seal-v0.0.1",
        "hash": "HV1tT3D639TiYe6bmamXtesvNjAN8tJ7AmgmeB6STrwz",
        "body_hash": "Gg5KQzzNPAt5PiLrcE5kjMbd4jB7Vk4ooBmN81yWDqYv",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZ1szjaYdxsznCpSvg19yS1tKUw1yPmgXBX6Ehf5ZcKNaMCRkJ8PaNS34rUwLSZ88EPh8vFq1FfRncHiTfo1v9adHCSH",
        "signed_at": "2021-06-10T15:01:13.080144Z",
        "operations": [
            {
                "memo": "",
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "AhqQMGZHDCeJDp74aQJ8rEXMC6GgQtpxP3rXnjjP41ui",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE",
                    "token": "MjAyMS0wNi0xMFQxNTowMToxMy4wNDA0OTZa",
                    "sender": "Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "50",
                                    "currency": "MCC"
                                }
                            ]
                        },
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "50",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "AN1rKvthtCymTu7gv2fSrMhGwqVuK3o24FrDe6GGLzRU8N5SWF62nPs3iKcEjuzwHya6P9JmrNLRF95ri8QTE4NBc66TxhCHm",
                        "signed_at": "2021-06-10T15:01:13.053303Z"
                    }
                ]
            }
        ]
    }
    "2021-06-10T15:01:13.083634Z INF trying to send seal module=command-send-seal"
    "2021-06-10T15:01:13.171266Z INF sent seal module=command-send-seal"

| operation이 블록에 저장되었는지는 operation의 ``fact.hash`` 를 digest API에 요청해 확인할 수 있습니다.

.. code-block:: shell

    $ FACT_HASH=3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE

    $ DIGEST_API="https://127.0.0.1:54320"
    
    $ curl --insecure -v $DIGEST_API/block/operation/$FACT_HASH | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-operation-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-operation-value-v0.0.1",
            "hash": "3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE",
            "operation": {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "AhqQMGZHDCeJDp74aQJ8rEXMC6GgQtpxP3rXnjjP41ui",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE",
                    "token": "MjAyMS0wNi0xMFQxNTowMToxMy4wNDA0OTZa",
                    "sender": "Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "50",
                                    "currency": "MCC"
                                }
                            ]
                        },
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "50",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "AN1rKvthtCymTu7gv2fSrMhGwqVuK3o24FrDe6GGLzRU8N5SWF62nPs3iKcEjuzwHya6P9JmrNLRF95ri8QTE4NBc66TxhCHm",
                        "signed_at": "2021-06-10T15:01:13.053Z"
                    }
                ],
                "memo": ""
            },
            "height": 13,
            "confirmed_at": "2021-06-10T15:01:13.354Z",
            "reason": null,
            "in_state": true,
            "index": 0
        },
        "_links": {
            "block": {
                "href": "/block/13"
            },
            "manifest": {
                "href": "/block/13/manifest"
            },
            "new_account:8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu": {
                "href": "/account/8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                "address": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                "key": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu"
            },
            "new_account:EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL": {
                "href": "/account/2S252hnemi1oA3UZqEA7dvMSvbd3RA9ut1mgJNxoGW1Pmca",
                "key": "EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL",
                "address": "2S252hnemi1oA3UZqEA7dvMSvbd3RA9ut1mgJNxoGW1Pmca"
            },
            "operation:{hash}": {
                "templated": true,
                "href": "/block/operation/{hash:(?i)[0-9a-z][0-9a-z]+}"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "self": {
                "href": "/block/operation/3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE"
            }
        }
    }

---------------------------------------------------
transfer
---------------------------------------------------

| ``transfer`` 명령어를 사용해 계정 사이에 토큰을 전송하세요.

.. code-block:: shell

    $ ./mc seal transfer --network-id=NETWORK-ID-FLAG <privatekey> <sender> <receiver> <currency,amount> ...

| **EXAMPLE**

| 다음은 10 *MCC* token을 ``ac0`` 에서 ``ac1`` 로 전송하는 예제입니다.

.. code-block:: shell

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ AC1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ CURRENCY_ID=MCC

    $ NETWORK_ID="mitum"

    $ ./mc seal transfer --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $AC1_ADDR $CURRENCY_ID,10 | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "EJDzHbusvvcknN9NWaK1wjuvSTav2TVfnDmtRnqVjEVn",
        "body_hash": "FWLTyQePguo6CFxH8SgEHesoLL8ab3FofEw9nXHDDLMp",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZMbRqwMgfWwJNk4rWNuaJenJMHZU3HBufz7Uo4Yj3zo944oeJeGoKjUDyCJXuL4pZLt49gqW2FHV3YuB5zBR24h96ZH",
        "signed_at": "2021-06-14T03:42:11.969679Z",
        "operations": [
            {
                "_hint": "mitum-currency-transfers-operation-v0.0.1",
                "hash": "F3WZYRgcwwYENiVXx6J6zKPqkiDjSZcuF2vUUPiyR3n9",
                "fact": {
                    "_hint": "mitum-currency-transfers-operation-fact-v0.0.1",
                    "hash": "7xzioXfnkKU1qrFvgeWK1KrhR71RMHMSBZdpWRVK3MUD",
                    "token": "MjAyMS0wNi0xNFQwMzo0MjoxMS45NjUyNjNa",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-transfers-item-single-amount-v0.0.1",
                            "receiver": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "10",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtRQeMWcFQ9oPLqgakgW33fed4mCcxxfQwi3icWLyn19AKJ3XpYehA8njvAi7qzgGSVpv23JXBDcXbwiZvQkHBj6T8jw",
                        "signed_at": "2021-06-14T03:42:11.96891Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| operation을 네트워크로 바로 전송하고 싶다면,

.. code-block:: shell

    $ ./mc seal transfer --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $AC1_ADDR $CURRENCY_ID,3 | jq \
        ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --seal=-

.. _key updater:

---------------------------------------------------
key-updater
---------------------------------------------------

| ``key-updater`` 로 계정 keys를 업데이트하세요.

| 새로운 공개키로 계정 keys를 업데이트하여도 주소는 변경되지 않습니다.

.. code-block:: shell

    $ ./mc seal key-updater --network-id=NETWORK-ID-FLAG <privatekey> <target> <currency> --key=KEY@... --threshold=THRESHOLD

* KEY: <pub key, weight>

| 계정 keys에 대한 더 자세한 정보는 :ref:`multi sig` 를 참고하세요.

| **EXAMPLE**

| 다음은 ``key-updater`` 의 예제입니다. 예제에서는 ``ac0`` 의 keys를 교체하려고 하고 있습니다.

.. code-block:: none

    ac0 - target account
    private key: KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr
    public key: 2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu
    address: FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    ac1 - new key
    public key: 247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ NODE=https://127.0.0.1:54321

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_PUB=2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ AC1_PUB=247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu

    $ CURRENCY_ID=MCC

    $ ./mc seal key-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR --key $AC1_PUB,100 $CURRENCY_ID | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "GvuGxKCTKWqXzgzxk3iWVGkSPAMn1nBNbAu7qgzHB8y6",
        "body_hash": "8gyB4eE7yQvneA463ZnM8LEWKDCthm8mKEFcfvAmk2pg",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZWCaZy3G5VLse9NCBMmJg8bPWoY4rmyAWMTRVjLKZP9WkexgJfN8EP4G2P64MPchFKtsYZ2QsNyu31rrjKQN4THtEtz",
        "signed_at": "2021-06-14T03:45:21.821896Z",
        "operations": [
            {
                "_hint": "mitum-currency-keyupdater-operation-v0.0.1",
                "hash": "4fFKpjDBmSrka3C3Q62fz5JYGZstZmkQTe27vgyNj4A9",
                "fact": {
                    "_hint": "mitum-currency-keyupdater-operation-fact-v0.0.1",
                    "hash": "5yaMz2aSKS5H1wtd4YVcU4q5awbaxu7bhhswX3ss8XCb",
                    "token": "MjAyMS0wNi0xNFQwMzo0NToyMS44MTczNjNa",
                    "target": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "GmUiuEbsoTVLSirRWMZ2WcxT69enhEXNfskAnRJby8he",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "currency": "MCC"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtPv6CuiW36Q4g1wtmsGNy2Fc3ierpHgfnjXjdqjDE3wvSH293FVDYy9Yf9VTNadfMGJ38WC39hthZuGkau3vBGq7ijP",
                        "signed_at": "2021-06-14T03:45:21.821399Z"
                    }
                ],
                "memo": ""
            }
        ]
    }    

| operation을 바로 전송하고 싶다면,

.. code-block:: shell

    $ ./mc seal key-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR \
        --key $AC1_PUB,100" $CURRENCY_ID \
        | ./mc seal send --network-id=$NETWORK_ID \
        $AC0_PRV --seal=- --node=$NODE --tls-insecure

| 또한, 계정 keys가 정말로 바뀌었는지 확인할 수 있습니다.

.. code-block:: shell

    $ find blockfs -name "*-states-*" -print | sort -g | xargs -n 1 gzcat |  grep '^{' | jq '. | select(.key == "'$AC0_ACC_KEY'") | [ "height: "+(.height|tostring),   "state_key: " + .key, "key.publickey: " + .value.value.keys.keys[0].key, "key.weight: " + (.value.value.keys.keys[0].weight|tostring), "threshold: " + (.value.value.keys.threshold|tostring)]'
    [
        "height: 3",
        "state_key: GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca:account",
        "key.publickey: 2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "key.weight: 100",
        "threshold: 100"
    ]
    [
        "height: 104",
        "state_key: GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca:account",
        "key.publickey: 247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu",
        "key.weight: 100",
        "threshold: 100"
    ]

---------------------------------------------------
currency-register
---------------------------------------------------

| ``currency-register`` 를 사용해 새로운 currency 토큰을 등록하세요.

.. code-block:: shell

    $ ./mc seal currency-register --network-id=NETWORK-ID-FLAG --feeer=STRING <privatekey> <currency-id> <genesis-amount> <genesis-account>

| 새로운 currency 등록 시, 설정해야할 요소는 다음과 같습니다.

* ``genesis account``: 새로운 currency 등록과 함께 발행될 토큰이 입금될 계정
* ``genesis amount``: 새롭게 발행될 토큰의 양
* ``–policy-new-account-min-balance=<amount>`` 을 설정해야 합니다.
* ``feeer``: feeer는 세 정책 중 선택될 수 있습니다; {nil, fixed, ratio}.

    * ``nil`` - 수수료를 지급하지 않습니다.
    * ``fixed`` - 고정 수수료를 지급합니다.
    * ``ratio`` - operation amount의 일정 비율로 책정한 수수료를 지급합니다.

    * 수수료 정책이 fixed인 경우, ``–feeer-fixed-receiver=<fee receiver account address>``, ``–feeer-fixed-amount=<fee amount>`` 를 설정합니다.
    * 수수료 정책이 ratio인 경우, ``–feeer-ratio-receiver=<fee receiver account address>``, ``–feeer-ratio-ratio=<fee ratio, multifly by operation amount>``, ``–feeer-ratio-min=<minimum fee>``, ``–feeer-ratio-max=<maximum fee>`` 을 설정합니다.

| 새로운 currency를 등록할 때, 합의에 참여하는 노드들의 서명이 threshold(67%)를 넘겨야 operation이 처리됩니다.

| **EXAMPLE**

| 새로운 currency MCC2를 다음과 같은 조건에 따라 등록한다고 가정해봅시다.

.. code-block:: none

    genesis-account : ac1
    genesis-amount : 9999999999999
    currency-id : MCC2
    feeer : fixed
    feeer-fixed-receiver : ac1
    feeer-fixed-amount : 3
    seal sender : ac1
    suffrage node : n0, n1, n2, n3

| 다음과 같은 명령어를 통해 등록합니다.

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ AC1_ADDR="HWXPq5mBSneSsQis6BbrNT6nvpkafuBqE6F2vgaTYfAC-a000:0.0.1"

    $ AC1_PRV="792c971c801a8e45745938946a85b1089e61c1cdc310cf61370568bf260a29be-0114:0.0.1"

    $ N0_PRV=<n0 private key>

    $ N1_PRV=<n1 private key>

    $ N2_PRV=<n2 private key>

    $ N3_PRV=<n3 private key>

    $ ./mc seal currency-register --network-id=$NETWORK_ID --feeer=fixed --feeer-fixed-receiver=$AC1_ADDR \
        --feeer-fixed-amount=3 --policy-new-account-min-balance=10 $N0_PRV MCC2 9999999999999 $AC1_ADDR \
        | ./mc seal sign-fact $N1_PRV --network-id="$NETWORK_ID" --seal=- \
        | ./mc seal sign-fact $N2_PRV --network-id="$NETWORK_ID" --seal=- \
        | ./mc seal sign-fact $N3_PRV --network-id="$NETWORK_ID" --seal=- \
        | ./mc seal send --network-id="$NETWORK_ID" $AC1_PRV --seal=-

| 각 currency는 예금만 가능한 zero account를 가지고 있습니다. zero account는 token을 태우는데 사용됩니다. zero account는 공개키가 등록되어있지 않기 때문에 예금만 가능합니다.

| zero account의 주소는 모두 ``<currency id>-Xmca`` 형식을 가지고 있습니다. 예를 들어, PEN의 zero account의 주소는 ``PEN-Xmca`` 입니다.

.. code-block:: shell

    $ curl --insecure http://localhost:54320/account/PEN-Xmca | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-account-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-account-value-v0.0.1",
            "hash": "EJvkxncxfVQNncdKZtjQTH2XuT5ECRiqSZA7LLE14zqi",
            "address": "PEN-Xmca",
            "keys": {
                "_hint": "mitum-currency-keys-v0.0.1",
                "hash": "",
                "keys": [],
                "threshold": 0
            },
            "balance": [
                {
                    "_hint": "mitum-currency-amount-v0.0.1",
                    "amount": "100000000000000000000000000",
                    "currency": "PEN"
                }
            ],
            "height": 41,
            "previous_height": 0
        },
        "_links": {
            "block": {
                "href": "/block/41"
            },
            "previous_block": {
                "href": "/block/0"
            },
            "self": {
                "href": "/account/PEN-Xmca"
            },
            "operations": {
                "href": "/account/PEN-Xmca/operations"
            },
            "operations:{offset}": {
                "href": "/account/PEN-Xmca/operations?offset={offset}",
                "templated": true
            },
            "operations:{offset,reverse}": {
                "templated": true,
                "href": "/account/PEN-Xmca/operations?offset={offset}&reverse=1"
            }
        }
    }

---------------------------------------------------
currency-policy-updater
---------------------------------------------------

| ``currency-policy-updater`` 명령어를 사용하여, currency와 관련된 정책을 업데이트하세요.

.. code-block:: shell

    $ ./mc seal currency-policy-updater --network-id=NETWORK-ID-FLAG --feeer=STRING <privatekey> <currency-id>

| 우선 API를 통해 등록된 currency의 정보를 확인하세요.

| 정책 업데이트 시, 합의에 참여하는 노드들의 서명이 threshold(67%)를 넘겨야 operation이 처리됩니다.

.. code-block:: shell

    $ curl --insecure -v https://localhost:54320/currency/MCC2 | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-currency-design-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-currency-design-v0.0.1",
            "amount": {
                "_hint": "mitum-currency-amount-v0.0.1",
                "amount": "9999999999999",
                "currency": "MCC2"
            },
            "genesis_account": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
            "policy": {
                "_hint": "mitum-currency-currency-policy-v0.0.1",
                "new_account_min_balance": "10",
                "feeer": {
                    "_hint": "mitum-currency-fixed-feeer-v0.0.1",
                    "type": "fixed",
                    "receiver": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "amount": "3"
                }
            }
        },
        "_links": {
            "self": {
                "href": "/currency/MCC2"
            },
            "currency:{currencyid}": {
                "templated": true,
                "href": "/currency/{currencyid:.*}"
            },
            "block": {
                "href": "/block/10"
            },
            "operations": {
                "href": "/block/operation/goNANpmA1BcnXA6TVL6AKkoxsmiaT2F5ss5zoSh7Wdt"
            }
        }
    }

| ``currency-policy-updater`` 통해 업데이트할 수 있는 정책은 fee-related policy와 계정 생성 시의 minimum balance value입니다.

| **EXAMPLE**

| 다음 조건에 따라 MCC2의 정책을 업데이트한다고 가정해봅시다.

.. code-block:: none

    currency-id : MCC2

    Policy to be updated
    - feeer : ratio
    - feeer-ratio-receiver : ac1
    - feeer-ratio-ratio : 0.5
    - feeer-ratio-min : 3
    - feeer-ratio-max : 1000
    - policy-new-account-min-balance : 100
    
    suffrage node : n0, n1, n2, n3

| 명령어를 실행하면,

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ AC1_ADDR="HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca"

    $ AC0_PRV="KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr"

    $ N0_PRV=<n0 private key>

    $ N1_PRV=<n1 private key>

    $ N2_PRV=<n2 private key>

    $ N3_PRV=<n3 private key>

    $ ./mc seal currency-policy-updater --network-id=$NETWORK_ID --feeer="ratio" --feeer-ratio-receiver=$AC1_ADDR \
        --feeer-ratio-ratio=0.5 --feeer-ratio-min=3 --feeer-ratio-max=1000 --policy-new-account-min-balance=100 $N0_PRV MCC2 \
        | ./mc seal sign-fact $N1_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N2_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N3_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --seal=-

| 결과를 확인하면,

.. code-block:: shell

    $ curl --insecure https://localhost:54320/currency/MCC2 | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-currency-design-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-currency-design-v0.0.1",
            "amount": {
                "_hint": "mitum-currency-amount-v0.0.1",
                "amount": "9999999999999",
                "currency": "MCC2"
            },
            "genesis_account": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
            "policy": {
                "_hint": "mitum-currency-currency-policy-v0.0.1",
                "new_account_min_balance": "100",
                "feeer": {
                    "_hint": "mitum-currency-ratio-feeer-v0.0.1",
                    "type": "ratio",
                    "receiver": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                    "ratio": 0.5,
                    "min": "3",
                    "max": "1000"
                }
            }
        },
        "_links": {
            "currency:{currencyid}": {
                "href": "/currency/{currencyid:.*}",
                "templated": true
            },
            "block": {
                "href": "/block/13"
            },
            "operations": {
                "href": "/block/operation/3HxC5VP5Fjzent7uVVLsK44i1tp8ooH4f2Vh4c4uWM4e"
            },
            "self": {
                "href": "/currency/MCC2"
            }
        }
    }

---------------------------------------------------
suffrage-inflation
---------------------------------------------------

| ``suffrage-inflation`` 를 사용해 존재하는 currency 토큰에 인플레이션을 발생시킵니다.

.. code-block:: shell

    $ ./mc seal suffrage-inflation --network-id=NETWORK-ID-FLAG <privatekey> <inflation item> ...

* ``inflation item``: <receiver-account>,<currency-id>,<inflation-amount>

| Mitum Currency에 currency를 등록하기 위한 두 가지 방법이 있습니다.

* 초기 genesis currency 생성을 통해 
* 네트워크가 살아있을 때 새로운 currency를 등록함으로써

| 등록된 currency에는 총 공급량이 있습니다. Mitum Currency는 기존 총 공급량에 일정량의 토큰을 추가할 수 있습니다.

| 새로운 계정을 생성할 때, 설정해야 할 요소에는 다음과 같은 것들이 있습니다.

* 추가로 발행되는 토큰을 입금할 ``receiver-account``.

| currency에 인플레이션을 일으킬 때, 합의에 참여하는 노드들의 서명이 threshold(67%)를 넘겨야 operation이 처리됩니다.

| **EXAMPLE**

| 우리는 다음 조건에 따라 MCC에 인플레이션을 일으키고자 합니다.

.. code-block:: none

    operation-sender-account : ac1
    receiver-account : ac2
    inflation-amount : 9999999999999
    currency-id : MCC
    seal sender : ac1
    suffrage node : n0, n1, n2, n3

| 이를 실행하면,

.. code-block:: shell

    $ NETWORK_ID="mitum"
    
    $ AC1_PRV="L2Q4PqxrhgS39jgGoXsV92LaCHRF2SqTLRwMhCC6Q6in9Vb19aDLmpr"
    
    $ AC2_ADDR="HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca"
    
    $ N0_PRV=<n0 private key>
    
    $ N1_PRV=<n1 private key>
    
    $ N2_PRV=<n2 private key>
    
    $ N3_PRV=<n3 private key>
    
    $ ./mc seal suffrage-inflation --network-id=$NETWORK_ID $N0_PRV MCC 9999999999999 $AC2_ADDR \
        | ./mc seal sign-fact $N1_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N2_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N3_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal send --network-id=$NETWORK_ID $AC1_PRV --seal=-

---------------------------------------------------
send
---------------------------------------------------

| ``send`` 명령어를 통해 seal을 전송하세요.

.. code-block:: shell

    $ ./mc seal send  <sender privatekey> --network-id=<network id> --seal=<data file path> --node=<node https url>

| Mitum Currency에서 생성된 operation들은 seal 단위로 전송됩니다.

| seal을 전송하기 위해 서명이 필요합니다. 서명 생성 시 필요한 키페어와 관련된 것은 :ref:`seal` 를 참고하세요.

| **EXAMPLE**

| data.json은 json으로 작성된 seal 파일입니다.

.. code-block:: shell

    $ NETWORK_ID="mitum" 

    $ NODE="https://127.0.0.1:54321"

    $ AC0_PRV=L1jPsE8Sjo5QerUHJUZNRqdH1ctxTWzc1ue8Zp2mtpieNwtCKsNZmpr

    $ ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --seal=data.json --node=$NODE jq -R '. as $line | try fromjson catch $line'
    {
        "_hint": "seal-v0.0.1",
        "hash": "6nLRWj5hGQ7va9gxpAJCBxNDKvgFnms9jaa913uWgsx1",
        "body_hash": "32ZEf8V9fV41JHVWbbqQdYWtrw5T255XN8fSXhBAhGFD",
        "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
        "signature": "381yXZ4LFY5HnK211gpG3W22V52vMLqix4SysXEeMnqcXUk5eEYGM1JfFaX5UE86EF6qog5jUScPqZo6UkiaAFocUhwtSsjx",
        "signed_at": "2021-06-10T09:17:51.236729Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "7YvcA6WAcKEag1Z4Jv1bQ2wYxAZix5sNB6u8MUXDM44D",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3equMRJAVHk8WdVanffzEWkHfwnBDqF2cFwmmcv8MzDW",
                    "token": "MjAyMS0wNi0xMFQwOToxNzo1MS4yMDgwOTVa",
                    "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
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
                                    "amount": "100000",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                        "signature": "AN1rKvtPEX4MRu6kWRYDJ6WtsSnwxwJsYXiVi2Qujx8sF6SJzsZZKj7anCd9cmUZ175FSYLkkWkpDRj3fVgZFDxLFSnos3szz",
                        "signed_at": "2021-06-10T09:17:51.211816Z"
                    }
                ],
                "memo": ""
            }
        ]
    }
    2021-06-10T09:17:51.240066Z INF trying to send seal module=command-send-seal
    2021-06-10T09:17:51.345243Z INF sent seal module=command-send-seal

| 테스트를 위해 로컬 노드로 전송할 때, tls 인증과 관련한 에러가 발생할 수 있습니다.

| 이 경우, seal 전송 시 ``–tls-insecure=true`` 옵션을 추가하세요.

.. code-block:: shell

    $ ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --tls-insecure=true --seal=data.json --node=$NODE

---------------------------------------------------
sign
---------------------------------------------------

| ``sign`` 으로 seal에 서명하세요.

.. code-block:: shell

    $ ./mc seal sign --network-id=NETWORK-ID-FLAG <privatekey>

| **EXAMPLE**

| ``sign`` 사용 전, 서명 생성을 위해 opeation이 들어있는 seal의 json 파일을 준비하세요.

| 예를 들어,

.. code-block:: json

    {
        "_hint": "seal-v0.0.1",
        "hash": "5W39B2mmtc4KK9THiRdoF6F5UMZPSxjzedPePojVhqyV",
        "body_hash": "5yGtCzJiPRRbZkeLawQev4dvdYgYuKHXe6TP6x2VLSt4",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZHsyzbc8qTD7BJgmGoM8ncSrUcyDZiSNanARp9h84tvcj6HkGXzpFyck9arJTCQDmPGzT5UFq1coHv7wijusgynSfgr",
        "signed_at": "2021-06-10T06:50:26.903245Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "9mFHaqd66pv7RjoAbKScUucJLKW7KVSkWqN1WXnzMrxQ",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3CpL1MgD1TPejUmVxPKSgiUu6LCR7FhFrDehSjSogavZ",
                    "token": "MjAyMS0wNi0xMFQwNjo1MDoyNi44NzQyNzVa",
                    "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "Dut3WiprEo1BRcx2xRvh6qbBgxaTLXQDris7SihDTET8",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "27tMvbSpajF1VSnrn3xRQESpPAsmA7KZEfUz9ZuTZEemumpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "100000",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "AN1rKvtfRrgY15owfURsNyfWnYtZ7syuRafWa637tkWB1HyxDCD2tWZUhySTg6mnZWQKpP3i6Dmf96fw9TUWb8rrbsetHJciH",
                        "signed_at": "2021-06-10T06:50:26.877954Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| 이 json 파일과 함께 ``seal sign`` 을 실행하세요.

| 그러면 새로운 seal 서명이 추가된 seal을 얻을 수 있습니다. 

.. code-block:: shell

    $ SIGNER_PRV=KxmWM4Zj5Ln8bbDwVZEKrYQY8N51Uk3UVq5GNQAeb2KW8JqHmsgmmpr
    $ ./mc seal sign --seal=data.json  --network-id=mitum $SIGNER_PRV | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "5dLCySkPrFtc8SnbjzELBK5GR7VQocrK7cXswEnhEa1S",
        "body_hash": "3Ah7J2q4HhFXSgV3c4EQWeZtpi1nFY7be2nmL4X6qDxa",
        "signer": "224ekkhrax6EpekzfLTv9See1hNDZW3LAjWBRuzTMpgnrmpu",
        "signature": "AN1rKvtFhZfDzyLLXtK3PtZ8P1jSTqZy6gC8WooBjWRhzwLrXjCcVTeo4juzdMg83he2emJ3SVkCNZssiB1pTtAPtx753P5CT",
        "signed_at": "2021-06-10T07:12:41.992205Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "9mFHaqd66pv7RjoAbKScUucJLKW7KVSkWqN1WXnzMrxQ",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3CpL1MgD1TPejUmVxPKSgiUu6LCR7FhFrDehSjSogavZ",
                    "token": "MjAyMS0wNi0xMFQwNjo1MDoyNi44NzQyNzVa",
                    "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "Dut3WiprEo1BRcx2xRvh6qbBgxaTLXQDris7SihDTET8",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "27tMvbSpajF1VSnrn3xRQESpPAsmA7KZEfUz9ZuTZEemumpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                "_hint": "mitum-currency-amount-v0.0.1",
                                "amount": "100000",
                                "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "AN1rKvtfRrgY15owfURsNyfWnYtZ7syuRafWa637tkWB1HyxDCD2tWZUhySTg6mnZWQKpP3i6Dmf96fw9TUWb8rrbsetHJciH",
                        "signed_at": "2021-06-10T06:50:26.877954Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

---------------------------------------------------
sign-fact
---------------------------------------------------

| ``sign-fact`` 를 사용해 opeation fact에 서명을 추가하세요.

| 이 명령어는 operation을 가진 seal에 fact 서명을 추가하기 위해 사용됩니다. operation을 가진 seal 파일을 넘겨주어야 합니다.

| 멀티 시그나 멀티 노드를 사용할 경우 operation fact에 여러 개의 서명을 추가하는 것이 목적입니다.

.. code-block:: shell

    $ ./mc seal sign-fact --network-id=NETWORK-ID-FLAG <privatekey>

| **EXAMPLE**

| 다음은 멀티 시그 계정으로부터 토큰을 전송하는 transfer operation을 담은 seal의 예제입니다. operation은 두 개의 fact_sign을 가져야 하지만 현재는 하나밖에 없습니다.

.. code-block:: json

    {
        "_hint": "seal-v0.0.1",
        "hash": "CgFaHkJEP966xRQjzPtXBUwzqgQYWB53RHwjBqyvmKHs",
        "body_hash": "Akjx1kJZKzyYMo2eVbqcUvtEfivDEGsK4yeUUuNwbGmu",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZ8qZBYQXDBaGr1KyAcsMJyB9HZLo1aQQRsxhx854aMYm5n7nh3NXzsJHpEhiYHgWUYnCtbAZaVsQ8pe6nEnLaHCXizY",
        "signed_at": "2021-06-10T09:54:35.868873Z",
        "operations": [
            {
                "hash": "Eep8SJH7Vkqft3BcvKYd9NY14Zgzmhyp7Uts2GmpaS5N",
                "fact": {
                    "_hint": "mitum-currency-transfers-operation-fact-v0.0.1",
                    "hash": "Eu1b4gr528Xy4u2sg97DsEo5uj9BuQEMjHzJxdsLgH48",
                    "token": "MjAyMS0wNi0xMFQwOTo1NDozNS44NjQwOTha",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-transfers-item-single-amount-v0.0.1",
                            "receiver": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                            "amounts": [
                                {
                                "_hint": "mitum-currency-amount-v0.0.1",
                                "amount": "100",
                                "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtZFkx5e4NexvBSjjJkuzUj45UKau8DL2JZx5d1htnbnkmPmHnNbgwqfvUnz8KHpUR72Z9YxD4JVQhdh4JCzGv9zMDDG",
                        "signed_at": "2021-06-10T09:54:35.868223Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-currency-transfers-operation-v0.0.1"
            }
        ]
    }

| fact_sign 추가 후 위 json은 다음과 같아집니다.

.. code-block:: shell

    $ SIGNER1_PUB_KEY=2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu
    $ SIGNER2_PUB_KEY=sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu
    $ SIGNER2_PRV_KEY=L5AAoEqwnHCp7WfkPcUmtUX61ppZQww345rEDCwB33jVPud4hzKJmpr
    $ NETWORK_ID=mitum
    $ ./mc seal sign-fact $SIGNER2_PRV_KEY --seal data.json --network-id=$NETWORK_ID | jq

    {
        "_hint": "seal-v0.0.1",
        "hash": "GiADUurx7qVwyeu8XUNQgmNpqmtN9UDzockhLNKXzYN6",
        "body_hash": "Ci7yzpahGtXqpWs3EGfoqnmUhTgbRhdkgb2GupsJRvgB",
        "signer": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu",
        "signature": "381yXYnDDMYrZ4asLpAYgD7AHDAGMsVih11S3V2jCwNdvJJxeA96whPnth4DxXoJ3RiK8vBpvVKRvXJsPpDpZZ2GMagAmaBi",
        "signed_at": "2021-06-10T10:01:27.690429Z",
        "operations": [
            {
                "_hint": "mitum-currency-transfers-operation-v0.0.1",
                "hash": "AduowWC9mHTCeRp8aqN4dQxHjKGH8xdm8vqxcMj7SfUZ",
                "fact": {
                    "_hint": "mitum-currency-transfers-operation-fact-v0.0.1",
                    "hash": "Eu1b4gr528Xy4u2sg97DsEo5uj9BuQEMjHzJxdsLgH48",
                    "token": "MjAyMS0wNi0xMFQwOTo1NDozNS44NjQwOTha",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-transfers-item-single-amount-v0.0.1",
                            "receiver": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "100",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtZFkx5e4NexvBSjjJkuzUj45UKau8DL2JZx5d1htnbnkmPmHnNbgwqfvUnz8KHpUR72Z9YxD4JVQhdh4JCzGv9zMDDG",
                        "signed_at": "2021-06-10T09:54:35.868223Z"
                    },
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu",
                        "signature": "381yXZ9yqzCSzUZZUuQvU3ZMHgM9Pa5MQUo2hKGhPFW4ZuMCC3eK2iGYvx3gwQD3LCfELuUXejAQiMmeKaNAEoZVPDf1gpkE",
                        "signed_at": "2021-06-10T10:01:27.690034Z"
                    }
                ],
                "memo": ""
            }
        ]
    }