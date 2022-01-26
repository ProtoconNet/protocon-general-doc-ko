===================================================
Mitum Protocol
===================================================

---------------------------------------------------
What is MITUM?
---------------------------------------------------

| Mitum은 유연하고 탄력적인 범용 프라이버시 블록체인입니다.

| Mitum은 다양한 목적으로 사용될 수 있습니다/.

* 암호화폐 네트워크와 같은 public과 private 블록체인
* 임의의 데이터를 위한 데이터 중심 블록체인
* 안전한 익명 투표 시스템

| MITUM에 대해 더 알고 싶다면, `Mitum Document <https://mitum-doc.readthedocs.io/en/proto2/index.html>`_ 을 방문하세요.

---------------------------------------------------
Mitum Technical SPEC
---------------------------------------------------

* Mitum(blockchain core framework)는 PBFT에 기초한 ISAAC+ 합의 프로토콜을 사용합니다.
* 네트워크 전송 프로토콜은 `quic <https://en.wikipedia.org/wiki/QUIC>`_ (udp 기반)입니다.
* *Gossip-Based* Node Discovery Protocol.
* 블록체인의 메인 스토리지 엔진은 *MongoDB*를 사용하며 로컬 파일 시스템이 블록 스토리지로 사용됩니다.
* operation 병렬 처리
* Main hash algorithm: `Keccak <https://keccak.team>`_ 256, SHA-3
* 지원하는 hash algorithm: ``Keccak 256``, ``Keccak 512``, ``Raw bytes.``
* 지원하는 message serialization format: *JSON*, *BSON*
* 적은 양의 코드.
* *JSON logging*