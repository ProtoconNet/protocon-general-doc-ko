.. _node handover:

===================================================
Node Handover
===================================================

| **node handover** 과정을 설명합니다.

---------------------------------------------------
What is Handover?
---------------------------------------------------

| Handover는 운용중인 노드를 멈추지 않고 교체하도록 도와줍니다.

* 전체 네트워크에서 합의 과정에 참여하는 노드들은 여러 상황에 의해 교체되어야 할 수 있습니다.
* 노드를 운용하던 프로그램이 업데이트 되어야 하는 경우, 다른 클라우드 서비스로 노드를 옮겨야 하는 경우, 노드가 실패하는 경우 등의 상황이 있을 수 있습니다. 그럴 때, 이미 운용중인 노드를 멈춤 없이 다른 대체 노드로 교체할 수 있습니다.

| Handover는 향후 공개 네트워크를 지원할 수 있는 주요 특징 중 하나입니다.

---------------------------------------------------
Handover Scenario
---------------------------------------------------

* **Node A**: 운용중인 노드
* **Node A-sub**: 새롭게 교체될 대체 노드
* Condition: *A-sub* 는 *A* 와 같은 구성을 가져야 합니다. 하지만, ``network.url`` 값은 달라야 합니다.

| Node *A*'s Configuration

.. code-block:: none

    address: Asas
    network-id: mitum
    network:
        url: https://172.17.0.1:54321
    privatekey: KyfqCLSEyfUhskZ63WtVH3m3pGgnurFHuTgkgu73Pgyjf8sxbp8Rmpr    

| Node *A-sub*'s Configuration

.. code-block:: none

    address: Asas
    network-id: mitum
    network:
        url: https://172.17.0.2:54321
    privatekey: KyfqCLSEyfUhskZ63WtVH3m3pGgnurFHuTgkgu73Pgyjf8sxbp8Rmpr

---------------------------------------------------
How to Run
---------------------------------------------------

| 위 시나리오 하에 다음과 같은 순서로 진행됩니다.

* *A* 는 suffrage 노드로서 운용되고 있으며 이를 *A-sub* 로 교체하려고 합니다.
* *A* 가 **CONSENSUS** 상태로 들어간 것을 확인한 후, 위 config를 사용한 ``node run`` 으로 *A-sub* 를 실행합니다.
* *A-sub* 는 이전 블록 데이터를 수집하여 동기화를 수행하고 **SYNCING** 상태로 진입합니다.

* *A-sub* 가 **SYNCING** 상태가 된 것을 확인하고, *A-sub* 노드에 대해 ``start-handover`` 를 실행합니다.

.. code-block:: shell

    $ ./mitum node start-handover \
        "Asas" \
        "KyfqCLSEyfUhskZ63WtVH3m3pGgnurFHuTgkgu73Pgyjf8sxbp8Rmpr" \
        "mitum" \
        "https://172.17.0.2:54321"

* *B* 가 동기화를 마칠 때 *Handover* 가 시작됩니다.
* *A* 는 **CONSENSUS** 에서 **SYNCING** 로 변경되며 *A-sub* 는 *A* 대신 합의 과정에 참여합니다.
* 다른 suffrage 노드들은 *A-sub* 를 *A* 대신 suffrage 노드로 추가하며 함께 합의 과정을 수행합니다.
*  *A* 에 도달한 Operation seal이 다른 노드에도 전달됩니다.

What If a start-handover is sent to A after the Handover is over?
------------------------------------------------------------------

* *A-sub* 로 교체된 *A* 는 **SYNCING** 상태가 됩니다.
* handover가 끝난 후, *A* 는 내부적으로 ``start-handover`` 명령 이전의 *A-sub* 의 상태와 같아집니다.
* ``start-handover`` 명령어가 이 상태의 *A* 에 전달되면, 그 다음, *A* 는 *A-sub* 를 교체하려고 시도합니다.

How can I check that the start-handover is finished?
-----------------------------------------------------

* *A-sub* 의 *NodeInfo* 를 확인하면 **CONSENSUS** 상태인 것을 확인할 수 있습니다.
* *A* 의 *NodeInfo* 가 확인되었을 때 **SYNCING** 상태라면 handover가 정상적으로 완료된 것입니다.