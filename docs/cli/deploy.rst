.. _deploy command:

===================================================
Deploy Command
===================================================

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

---------------------------------------------------
new
---------------------------------------------------

| ``new`` 를 사용해 새로운 deploy key를 생성하고 노드에 등록하세요.

.. code-block:: shell

    $ ./mc deploy key new <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr

    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ ./mc deploy key new $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    {"key":"d-fc4179e7-2ff3-4372-bd83-f70526bed476","added_at":"2021-06-09T09:31:22.321675852Z"}
    2021-06-09T09:31:22.320055Z INF new deploy key module=command-deploy-key-new

---------------------------------------------------
keys
---------------------------------------------------

| ``keys`` 를 사용해 노드에 등록된 deploy key 리스트를 확인하세요.

.. code-block:: shell

    $ ./mc deploy key keys <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr
    
    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ ./mc deploy key keys $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    [{"key":"d-974702df-89a7-4fd1-a742-2d66c1ead6cd","added_at":"2021-06-09T03:14:33.9Z"},{"key":"d-2897ced4-ceb5-4e11-be81-3139350c9c55","added_at":"2021-06-09T03:56:49.393Z"},{"key":"d-fc4179e7-2ff3-4372-bd83-f70526bed476","added_at":"2021-06-09T09:31:22.321675852Z"}]

---------------------------------------------------
key
---------------------------------------------------

| ``key`` 를 사용해 노드에 deploy key가 존재하는지 확인하세요.

.. code-block:: shell

    $ ./mc deploy key key <deploy key> <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr

    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ ./mc deploy key key $DEPLOY_KEY $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    {"key":"d-974702df-89a7-4fd1-a742-2d66c1ead6cd","added_at":"2021-06-09T03:14:33.9Z"}

---------------------------------------------------
revoke
---------------------------------------------------

| ``revoke`` 를 사용해 노드에서 deploy key를 취소하세요.

.. code-block:: shell

    $ ./mc deploy key revoke <deploy key> <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr
    
    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ ./mc deploy key revoke $DEPLOY_KEY $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    2021-06-09T09:36:19.763339Z INF deploy key revoked deploy_key=d-974702df-89a7-4fd1-a742-2d66c1ead6cd module=command-deploy-key-revoke