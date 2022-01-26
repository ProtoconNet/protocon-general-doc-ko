===================================================
About Time Stamp
===================================================

Expression of Time Stamp
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 블록, seal, 서명 등에서 mitum은 ``yyyy-MM-dd HH:mm:ss.* +0000 UTC`` 표현과 ``yyyy-MM-ddTHH:mm:ss.*Z``을 표준으로 사용합니다.
| 다른 timezone은 허용되지 않습니다! 오직 ``+0000 timezone``을 사용하세요.

| 예를 들어,

* timestamp를 block/seal/fact_sign hash 생성을 위한 byte 포맷으로 변환할 경우, 
| ``문자열 '2021-11-16 01:53:30.518 +0000 UTC'을 byte 포맷으로 변환합니다.``

* timestamp를 block, seal, fact_sign나 기타 json value에 추가하여 파일로 출력할 경우,
| ``문자열 '2021-11-16T01:53:30.518Z'을 json에 추가합니다.``

| operation hash를 생성하기 위해, mitum은 network id, fact hash의 byte array와 각 fact_sign의 byte array를 연결합니다.
| fact_sign의 byte array를 생성하기 위해, mitum은 signer, signature digest, signed_at의 byte array를 연결합니다.

| signed_at이 byte array로 변환될 때 문자열 ``yyyy-MM-dd HH:mm:ss.* +0000 UTC``를 byte 형식으로 변환한다는 사실에 주의하세요. 반면 이 문자열은 json의 signed_at 키에 추가 시 ``yyyy-MM-ddTHH:mm:ss.*Z`` 형태가 됩니다.

How many decimal places to be expressed?
'''''''''''''''''''''''''''''''''''''''''''''''''''

| 한 가지 더 주의해야 할 점이 있습니다.

| 우선, timestampe의 '초'자리의 소수점 자리수에 대해 걱정할 필요는 없습니다.
| 게다가, ``.``와  그 아래의 소수점 자리를 적지 않아도 되는 경우에는 없이 사용할 수도 있습니다.
| 하지만 timestamp를 byte 형식으로 바꿀 때에는 불필요한 소수점 ``0``가 없는 문자열을 사용해야 합니다.

| For example,

* ``2021-11-16T01:53:30.518Z``는 아무런 차이가 없는 ``2021-11-16 01:53:30.518 +0000 UTC``로 변환됩니다.
* ``2021-11-16T01:53:30.510Z``는 hash 생성 시에는 반드시 ``2021-11-16 01:53:30.51 +0000 UTC``로 변환하여 사용해야 합니다.
* ``2021-11-16T01:53:30.000Z``는 hash 생성 시에는 반드시 ``2021-11-16T01:53:30 +0000 UTC``로 변환하여 사용해야 합니다.

| json 값으로 포함되는 불필요한 ``0``이 있는 모든 문자열 timestamp들은 block, seal, operation 처리에 영향을 주지 않습니다.

| 단지 timestamp의 byte array가 필요한 경우에만 주의하십시오.