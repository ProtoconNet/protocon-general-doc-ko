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
send
---------------------------------------------------

| ``send`` 명령어를 통해 seal을 전송하세요.

.. code-block:: shell

    $ ./mitum seal send  <sender privatekey> --network-id=<network id> --seal=<data file path> --node=<node https url>

| Mitum Currency에서 생성된 operation들은 seal 단위로 전송됩니다.

| seal을 전송하기 위해 서명이 필요합니다. 서명 생성 시 필요한 키페어와 관련된 것은 :ref:`seal` 를 참고하세요.

| **EXAMPLE**

| data.json은 json으로 작성된 seal 파일입니다.

.. code-block:: shell

    $ NETWORK_ID="mitum" 

    $ NODE="https://127.0.0.1:54321"

    $ AC0_PRV=L1jPsE8Sjo5QerUHJUZNRqdH1ctxTWzc1ue8Zp2mtpieNwtCKsNZmpr

    $ ./mitum seal send --network-id=$NETWORK_ID $AC0_PRV --seal=data.json --node=$NODE jq -R '. as $line | try fromjson catch $line'
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

    $ ./mitum seal send --network-id=$NETWORK_ID $AC0_PRV --tls-insecure=true --seal=data.json --node=$NODE

---------------------------------------------------
sign
---------------------------------------------------

| ``sign`` 으로 seal에 서명하세요.

.. code-block:: shell

    $ ./mitum seal sign --network-id=NETWORK-ID-FLAG <privatekey>

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
    $ ./mitum seal sign --seal=data.json  --network-id=mitum $SIGNER_PRV --pretty
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

    $ ./mitum seal sign-fact --network-id=NETWORK-ID-FLAG <privatekey>

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
    $ ./mitum seal sign-fact $SIGNER2_PRV_KEY --seal data.json --network-id=$NETWORK_ID --pretty

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