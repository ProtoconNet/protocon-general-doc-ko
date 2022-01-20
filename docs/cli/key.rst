===================================================
Key Command
===================================================

| ``key`` command helps create a keypair, get address from keys, and get signature.

| The subcommands of the ``key`` command are as follows.

* ``new``
* ``address``
* ``sign``

.. note::

    Keypair
    
    * *Private key* and *public key* are created through keypair generation.
    * The generated keypair is used to *create an account*, *register a keypair* of a node, and *create a signature* of operation and seal.

---------------------------------------------------
new
---------------------------------------------------

| By ``new`` command, **create a new keypair**.

Random Keypair
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Create random keypair without any seed.

.. code-block:: shell

    $ ./mc key new

| **EXAMPLE**

.. code-block:: shell

    $ ./mc key new 
          hint: mpr
    privatekey: L1ZERchoY53vC5TJQ3WnZEWmg97L2Utw5rgFrCwM7ekTu9zJkZYjmpr
     publickey: 28nFxuC5ETygieSGEYTkewwnCZseB4TNYGMRtxz31bvxzmpu

Keypair from Seed
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Keypair from seed. The length of string seed must be longer than or equal to 36.

.. code-block:: shell

    $ ./mc key new --seed <string seed>

| **EXAMPLE**

.. code-block:: shell

    $ ./mc key new --seed abcdefghijklmnopqrstuvwxyzABCDEFGHIJ
          hint: mpr
    privatekey: KypAAGYtVFdTFLS8muPJhwfJBFCFHKSe594yYmKK3FPteh7sie4Dmpr
     publickey: 25BcZrcyiE3TD2BZEqkdDuaYB9zHxpdW82BNn8HkCLTijmpu

---------------------------------------------------
address
---------------------------------------------------

| By ``address`` command, generate address from keys.

| You should prepare (public key, weight) pairs and threshold for the account. Refer to below *Multi Sig Account* for details.

.. code-block:: shell

    $ ./mc key address <threshold> [<publickey>,<weight>]

| **EXAMPLE**

| When the information of the account is like below,

+---------------+------------------------------------------------------------------+
| threshold     | 100                                                              |
+---------------+------------------------------------------------------------------+
| keys          | {key: 21Sn1o…, weight: 50}, {key: utzCef…, weight: 50}           |
+---------------+------------------------------------------------------------------+

.. code-block:: shell

    $ ./mc key address 100 21Sn1owHXRx336aaerU1WbbKjiZXMcrJsnxBHP9etNx6zmpu,50 utzCefA1Szmmt3rAwqW5yEhxK1x3hG3Y3yThEK3gZmv3mpu,50
    37x8YoAGA93B3HmDVNterRf1NTgz9tfN1gQn4jYuBYCHmca

| **However, you can't get correct address if the keys of the account have updated by key-updater command.** Refer to `key-updater <https://protocon-general-doc.readthedocs.io/en/latest/docs/cli/seal.html#key-updater>`_. 

Multi Sig Account
'''''''''''''''''''''''''''''''''''''''''''''''''''

* Account is a data structure that has *currency* and *balance* in Mitum Currency.
* Account has a unique value called *address* and can be identified through this.
* Register a public key for user’s *Account authentication*.
* Mitum Currency accounts can register *multiple public keys* because **multi signatures are possible**.

| For example, an account under following condition is available.

+---------------+------------------------------------------------------------------+
| address       | HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca                  |
+---------------+------------------------------------------------------------------+
| threshold     | 100                                                              |
+---------------+------------------------------------------------------------------+
| keys          | {key: rd89Gx…, weight: 50}, {key: skRdC6…, weight: 50}           |
+---------------+------------------------------------------------------------------+
| balance       | {currency: MCC, amount: 10000}, {currency: MCC2, amount: 20000}  |
+---------------+------------------------------------------------------------------+

.. note::

    There are several conditions that each account should follow.

    * The range of ``threshold`` should be 1 <= threshold <= 100.
    * The range of each ``weight`` should be 1 <= weight <= 100.
    * The sum of every weight of the account should be greater than or equal to ``threshold``.
    * Each key must be a BTC compressed public key with suffix ``mpu``.
    * ``mca`` follows the address as a suffix.

    These are examples of available account states.

    CASE1 (single)

    * threshold: 100
    * keys: {key: rd89Gx…, weight: 100}

    CASE2 (single)

    * threshold: 50
    * keys: {key: rd89Gx…, weight: 60}

    CASE3 (multi)
    
    * threshold: 100
    * keys: {key: rd89Gx…, weight: 40}, {key: skRdC6…, weight: 30}, {key: mymMwq…, weight: 30}

    CASE4 (multi)

    * threshold: 50
    * keys: {key: rd89Gx…, weight: 20}, {key: skRdC6…, weight: 20}, {key: mymMwq…, weight: 10}

| Even in the same publickey combination, address will have different values if ``weight`` or ``threshold`` are different.

---------------------------------------------------
sign
---------------------------------------------------

| By ``sign`` command, get the signature of the private key for a specific message.

.. code-block:: shell

    $ ./mc key sign <privatekey> <signature base>

| **EXAMPLE**

.. code-block:: shell

    $./mc key sign L5nDx2QtZVBPtJvUQ13cj3bMhC487JdxrwXTdS6JgzTvnSHestCxmpr bWVzc2FnZQ=
    381yXZHrm73kGD8z7FAksBjxy49wPRWn3WRdP22befdbFff6WYSdK8rz9TLpFWuEW7rmmphF3rHkrvTPvhVQ5kXNGLmELBwZ

| Note that signature base is string type encoded by *base64*. 