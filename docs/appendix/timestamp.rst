===================================================
About Time Stamp
===================================================

Expression of Time Stamp
'''''''''''''''''''''''''''''''''''''''''''''''''''

| For blocks, seals, signatures and etc, mitum uses ``yyyy-MM-dd HH:mm:ss.* +0000 UTC`` expression and ``yyyy-MM-ddTHH:mm:ss.*Z`` as standard.
| All other timezones are not allowed! You must use only ``+0000 timezone`` for mitum.

| For example,

* When converting timestamp to byte format for generating block/seal/fact_sign hash
| ``convert the string 2021-11-16 01:53:30.518 +0000 UTC to bytes format``

* When putting timestamp in block, seal, fact_sign or etc
| ``convert the timestamp to 2021-11-16T01:53:30.518Z and put it in json``

| To generate operation hash, mitum concatenates byte arrays of network id, fact hash and byte arrays of fact_signs.
| And to generate the byte array of a fact_sign, mitum concatenates byte arrays of signer, signature digest and signed_at.

| Be careful that the format of signed_at when converted to bytes is like ``yyyy-MM-dd HH:mm:ss.* +0000 UTC`` but it will be expressed as ``yyyy-MM-ddTHH:mm:ss.*Z`` when putted in json.

How many decimal places to be expressed?
'''''''''''''''''''''''''''''''''''''''''''''''''''

| There is one more thing to note.

| First at all, you don't have to care about decimal points of second(ss.*) in timestamp.
| Moreover, you can write timestamp without . and any number under ..
| However, you should not put any unnecessary zeros(0) in the float expression of second(ss.*) when converting timestamp to bytes format.

| For example,

* ``2021-11-16T01:53:30.518Z`` is converted to ``2021-11-16 01:53:30.518 +0000 UTC`` without any change of the time itself.
* ``2021-11-16T01:53:30.510Z`` must be converted to ``2021-11-16 01:53:30.51 +0000 UTC`` when generating hash.
* ``2021-11-16T01:53:30.000Z`` must be converted to ``2021-11-16T01:53:30 +0000 UTC`` when generating hash.

| Any timestamp with some unnecessary zeros putted in json doesn't affect to effectiveness of the block, seal, or operation.

| Just pay attention when convert the format.