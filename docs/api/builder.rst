===================================================
Using Operation Builder
===================================================

| **Digest API**는 operation 메시지 작성을 도와주는 **Operation Builder**를 가지고 있습니다. **Operation Builder**를 사용하면 SDK 없이 operation 메시지를 만들 수 있습니다.

---------------------------------------------------
Get Operation Fact Template
---------------------------------------------------

| *Operation Fact Template*을 요청해 각 operation 타입에 따른 템플릿을 받을 수 있습니다. 템플릿의 필드값을 수정 해 fact 메시지를 만들 수 있습니다.

+---------------+-----------------------------------------------+
| METHOD        | GET                                           |
+---------------+-----------------------------------------------+
| PATH          | /builder/operation/fact/template/{fact_type}  |
+---------------+-----------------------------------------------+

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "3Zdg5ZVdNFRbwX5WU7Nada3Wnx5VEgkHrDLVLkE8FMs1",
            "token": "cmFpc2VkIGJ5",
            "sender": "mothermca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "2TQ8Xn5tdowqkJt8kHWcNj2QKhuNRnnCwiXxFbRbwBWY",
                        "keys": [
                            {
                                _hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "-333",
                            "currency": "xXx"
                        }
                    ]
                }
            ]
        },
        "_links": {
            "self": {
                "href": "/builder/operation/fact/template/create-accounts"
            }
        },
        "_extra": {
            "default": {
                "items.keys.keys.key": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu"
                "items.big": "-333",
                "currency": "xXx",
                "token": "cmFpc2VkIGJ5",
                "sender": "mothermca",
            }
        }
    }

* 응답 템플릿 내용 중 ``_embedded`` 객체는 *fact*를 나타냅니다. fact의 내용을 편집해 *Build Operation Message*에 사용하세요.

| **create-accounts FACT EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "hash": "3Zdg5ZVdNFRbwX5WU7Nada3Wnx5VEgkHrDLVLkE8FMs1",
        "token": "cmFpc2VkIGJ5",
        "sender": "mothermca",
        "items": [
            {
                "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                "keys": {
                    "_hint": "mitum-currency-keys-v0.0.1",
                    "hash": "2TQ8Xn5tdowqkJt8kHWcNj2QKhuNRnnCwiXxFbRbwBWY",
                    "keys": [
                        {
                            _hint": "mitum-currency-key-v0.0.1",
                            "weight": 100,
                            "key": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu"
                        }
                    ],
                    "threshold": 100
                },
                "amounts": [
                    {
                        "_hint": "mitum-currency-amount-v0.0.1",
                        "amount": "-333",
                        "currency": "xXx"
                    }
                ]
            }
        ]
    }

* ``hash``값은 builder에 의해 자동적으로 완성됩니다. 그러므로 따로 편집할 필요가 없습니다.
* ``token``는 *base64*로 인코딩된 문자열을 사용합니다.
* ``_hint``는 그대로 사용하세요.

| ``keys``와 key 등록에 대한 자세한 내용은 :ref:`key command`를 참고하세요.

---------------------------------------------------
Build Operation Message
---------------------------------------------------

| 생성된 fact 메시지는 json 형식으로 request body로 전달되며 완성된 fact 메시지를 응답값으로 돌려받습니다.

| 예제에서, ``keys hash``, ``token``, ``fact hash`` 값이 변경된 fact 메시지를 돌려받을 것입니다.

+---------------+-----------------------------------------------+
| METHOD        | POST                                          |
+---------------+-----------------------------------------------+
| PATH          | /builder/operation/fact                       |
+---------------+-----------------------------------------------+

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "_embedded": {
            "hash": "92FXbSdm46iuA7kQuC6ENfi5pd64G1Uiu49A3VmaA8Tu",
            "fact": {
                "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                "hash": "9ttqrz1bkFNCySVnrhYrxewcVB6mkZWWvBpSPS2fShip",
                "token": "MjAyMS0wNi0xNSAwODo0OTozOS45NDggKzAwMDAgVVRD",
                "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                                "amount": "333",
                                "currency": "MCC"
                            }
                        ]
                    }
                ]
            },
            "fact_signs": [
                {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu",
                    "signature": "22UZo26eN",
                    "signed_at": "2020-10-08T07:53:26Z"
                }
            ],
            "memo": "",
            "_hint": "mitum-currency-create-accounts-operation-v0.0.1"
        },
        "_links": {
            "self": {
                "href": "/builder/operation/fact"
            }
        },
        "_extra": {
            "default": {
                "fact_signs.signer": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu",
                "fact_signs.signature": "22UZo26eN"
            },
            "signature_base": "hCi8MFOChFusqKx6v0zrsJ8u3tppYUOewadYjwTvDUFtaXR1bQ=="
        }
    }

| 응답 데이터의 ``fact.hash`` 값을 확인하세요. ``fact.hash`` 값은 *fact_sign*을 생성하기 위해 사용됩니다.

| ``fact_signs``의 한 *fact_sign*에서, 

* ``signer``는 서명을 생성하는데 사용된 키페어의 public key입니다.
* ``signature``는 ``signer``에 의해 만들어진 서명입니다.
* ``signed_at``는 서명 생성 시각입니다.

---------------------------------------------------
Sign Operation Message
---------------------------------------------------

| *signature*는 *fact*의 ``hash`` 값을 사용해 만들어지며 이에 대한 *fact_sign*가 ``fact_signs``에 추가됩니다.

| 생성된 fact 메시지는 json 형식으로 request body에 전달되며 *operation hash*가 추가된 완성된 operation 메시지를 돌려받습니다.

+---------------+-----------------------------------------------+
| METHOD        | POST                                          |
+---------------+-----------------------------------------------+
| PATH          | /builder/operation/sign                       |
+---------------+-----------------------------------------------+

| **REQUEST BODY EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "fact": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
            "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
            "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                            "amount": "333",
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
                "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                "signed_at": "2021-06-16T01:56:14.124268Z"
            }
        ],
        "memo": "",
    }

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "_embedded": {
            "fact": {
                "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
                "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
                "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                                "amount": "333",
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
                    "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                    "signed_at": "2021-06-16T01:56:14.124268Z"
                }
            ],
            "memo": "",
            "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
            "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT"
        },
        "_links": {
            "self": {
                "href": "/builder/operation/sign"
            }
        }
    }

---------------------------------------------------
Broadcast Message to Network
---------------------------------------------------

| request body를 통해 *Operation*나 *Seal* 메시지를 전송하면 네트워크에 이를 브로드캐스팅 할 수 있습니다.

| 이때, seal의 *signer*는 digest 노드가 됩니다.

* **operation**를 전송한 경우, digest 노드가 서명한 새로운 seal이 생성됩니다.
* **seal**을 전송한 경우, digest 노드가 seal에 서명합니다.

+---------------+-----------------------------------------------+
| METHOD        | POST                                          |
+---------------+-----------------------------------------------+
| PATH          | /builder/send                                 |
+---------------+-----------------------------------------------+

| **REQUEST BODY EXAMPLE**

.. code-block:: json

    {
        "fact": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
            "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
            "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                            "amount": "333",
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
                "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                "signed_at": "2021-06-16T01:56:14.124268Z"
            }
        ],
        "memo": "",
        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT"
    }

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "seal-v0.0.1",
        "_embedded": {
            "_hint": "seal-v0.0.1",
            "hash": "4UvusVw9RYdqxHQz2EzDb6gW6CgoZGPayD1yZBcdSSHW",
            "body_hash": "9AFx2gAqeMveV6ojwUi6HKx19GfbZZggPTGhTS3dDih5",
            "signer": "uGnKHNfh8EtNVXsL4Qu1a655oQuzibK8Tc41TZUHzHqkmpu",
            "signature": "381yXZAzT6LcYUXfTG9Fifc6neDfXDqpjzuGzfqr1LXPMvvtseJKzGSRwdL6jvkHBaVRdGPD4YfrHnp2rbpZEEWRNAePiJBt",
            "signed_at": "2021-06-16T03:06:33.649190888Z",
            "operations": [
                {
                    "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                    "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT",
                    "fact": {
                        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                        "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
                        "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
                        "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                                        "amount": "333",
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
                            "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                            "signed_at": "2021-06-16T01:56:14.124268Z"
                        }
                    ],
                    "memo": ""
                }
            ]
        },
        "_links": {
            "self": {
                "href": ""
            },
            "operation:0": {
                "href": "/block/operation/CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE"
            }
        }
    }

.. _confirm success:

---------------------------------------------------
Confirming the Success of the Operation
---------------------------------------------------

| operation이 성공적으로 처리되었는지 api에 *fact hash*값을 요청하여 확인할 수 있습니다.

+---------------+-----------------------------------------------+
| METHOD        | GET                                           |
+---------------+-----------------------------------------------+
| PATH          | /block/operation/{operation_fact_hash}        |
+---------------+-----------------------------------------------+

* 만약 응답 메시지에서 ``_embedded.in_state``가 *true*라면, operation은 블록에 저장됩니다.
* 만약``_embedded.in_state``가 false라면, operation은 블록에 저장되지 않습니다.

* 만약 **operation이 실패한 경우**, 원인은 다음과 같을 수 있습니다.
    
    1. 토크느 전송 시 *sender의 불충분한 잔액*
    2. *부정확한 signature*
    3. *create-account의 amount가 currency의 new-account-min-balance보다 낮을 경우*
    4. 기타 등등...

| 실패한 이유는 ``_embedded.reason.msg``에서 찾을 수 있습니다.

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-operation-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-operation-value-v0.0.1",
            "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
            "operation": {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
                    "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
                    "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                                    "amount": "333",
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
                        "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                        "signed_at": "2021-06-16T01:56:14.124Z"
                    }
                ],
                "memo": ""
            },
            "height": 108674,
            "confirmed_at": "2021-06-16T02:26:55.75Z",
            "reason": {
                "_hint": "base-operation-reason-v0.0.1",
                "msg": "state, \"9g4BAB8nZdzWmrsAomwdvNJU2hA2psvkfTQ5XdLn4F4r-mca:account\" does not exist",
                "data": null
            },
            "in_state": false,
            "index": 0
        },
        "_links": {
            "manifest": {
                "href": "/block/108674/manifest"
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
                "href": "/block/operation/CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE"
            },
            "block": {
                "href": "/block/108674"
            }
        }
    }