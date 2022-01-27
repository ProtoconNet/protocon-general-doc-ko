===================================================
Get Started
===================================================

| 이 장에서 **Mitum Currency** 의 노드를 어떻게 실행하는지 소개합니다.
| Mitum Currency를 사용하기 위해 docker 와 golang 를 먼저 설치하세요.

| Mitum Blocksign의 사용 방법에 대해서는 `mitum-data-blocksign <https://github.com/ProtoconNet/mitum-data-blocksign>`_ 에서 확인하세요.

---------------------------------------------------
About Mitum Currency Node
---------------------------------------------------

| Mitum Currency를 운용하는 Mitum Blockchain는 PBFT-based ISAAC+ 합의 프로토콜을 사용합니다.
| *ISAAC+* 합의 프로토콜에서, 모든 노드들은 같은 역할을 하며 블록 생성에 참여합니다.

| 네트워크에 참여하는 노드들은 다음 임무를 수행합니다.

* Make Proposal
* Block Verification
* Voting
* Store Block
* Provide Digest API Service
* Transaction Request Collection

| Mitum Blockchain 네트워크에 대한 더 많은 정보는 `Mitum Document <https://mitum-doc.readthedocs.io/en/proto2/>`_ 에서 확인하세요.

---------------------------------------------------
Prerequisite
---------------------------------------------------

Database
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Mitum Currency는 MongoDB를 메인 스토리지 엔진으로 사용합니다.

| Mitum Currency 노드를 운용하기 위해 mongodb를 먼저 준비해야 합니다.

Installation and Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* `Manual Installation Guide <https://docs.mongodb.com/manual/installation/>`_

* `Docker Container <https://hub.docker.com/_/mongo>`_ 를 사용해,

.. code-block:: shell

    $ docker run --name <db name> -it -p <host port>:<container port> -d mongo

* DB setup와 관련해서는 :ref:`config` 를 참고하세요.

Golang
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Mitum Currency는 `Go <https://golang.org>`_ 언어로 개발되었습니다.

| 실행가능한 바이너리를 생성하기 위해 소스코드를 빌드해야 합니다.
| Go 언어를 설치하기 위한 자세한 방법은 제공하지 않습니다.
| Mitum Currency를 빌하기 위해 최소 버전 1.16의 golang이 설치되어야 합니다.

| 더 자세한 사항은 `How to Install Go <https://go.dev/doc/install>`_ 을 참고하세요.

---------------------------------------------------
Installation
---------------------------------------------------

* `Mitum Currency <https://github.com/spikeekips/mitum-currency>`_ 의 소스코드를 다운로드 해주세요.

* **Git** 을 사용하면,

.. code-block:: shell

    $ git clone https://github.com/spikeekips/mitum-currency.git

* exe 파일을 빌드하세요.

.. code-block:: shell

    $ cd mitum-currency
    
    $ go build -ldflags="-X 'main.Version=v0.0.1-tutorial'" -o ./mc ./main.go
    
    $ ./mc version
    v0.0.1

| Mitum Currency의 모든 명령어는 :ref:`cli` 에서 확인할 수 있습니다.
