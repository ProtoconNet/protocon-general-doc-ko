===================================================
Javascript
===================================================

| This is SDK written in Javascript.

| Support models are,

* Mitum Currency
* Mitum Blocksign

| Note that this document introduces how to create operations only for Mitum Currency.

| If you would like to check the way to create operations for Mitum Blocksign and the detail explanation for Mitum Currency, please refer to README of `mitum-js-util <https://github.com/ProtoconNet/mitum-js-util>`_.

---------------------------------------------------
Get Started
---------------------------------------------------

Prerequisite and Requirements
'''''''''''''''''''''''''''''''''''''''''''''''''''

| To use **mitum-js-util** and build it, ``npm`` or ``yarn`` should be installed.

| Especially, this package has been developed by,

.. code-block:: sh

    $ npm --version
    v16.10.0

    $ node --version
    7.24.0

| ``npm version 16.10.0 or later`` is recommended,

Installation
'''''''''''''''''''''''''''''''''''''''''''''''''''

* Using **npm**,

.. code-block:: sh

    $ npm install mitumc

* Using **yarn**,

.. code-block:: sh

    $ yarn add mitumc

* Using **Git**,

.. code-block:: sh

    $ git clone https://github.com/ProtoconNet/mitum-js-util.git

    $ cd mitum-js-util

    $ sudo npm install -g

    $ cd YOUR_PACKAGE

    $ npm link mitumc

---------------------------------------------------
Make Your First Operation
---------------------------------------------------

| This tutorial explains how to ``create account`` by **mitum-js-util**.

| If you want to check how to create ``key-updater`` and ``transfer`` operation, go **Support Operations** at the end of this section.

Get Available Account
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Before start, you must hold the account registered in the network.

| In Mitum Currency, only already existing account can create operations able to be stored in block.

| An account consists of following factor.

.. code-block:: none

    1. pairs of (public key, weight); aka `keys`
    - public key has suffix `mpu`
    - The range of each weight should be in 1 <= weight <= 100
    - If an account have single public key, the account is called 'single-sig account', or it's called 'multi-sig account'
    
    2. threshold
    - The range of threshold should be in 1 <= threshold <= 100
    - The sum of all weights of the account should be over or equal to threshold

| If you haven't have any account yet, ask some other account to create your account first.
| You can get keypairs for your account in **Details - Get Mitum Keypair** section.
| Hand your (public key, weight) pairs and threshold to the account holder who helps make your new account.

| For signing, private keys corresponding each public key of the account must be remembered. **Don't let not allowed users to know your private key!**
| Of course, you must know your account address because you should use the address as ``sender``.

| You are able to create operations with unathorized account(like fake keys and address) but those operations will be rejected after broadcasting.

| Now, go to next part to start to create your first operation!

Create Generator
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Most of elements and factors for an operation are created by ``Generator``.
| For Mitum Currency, use ``Generator.currency``.

| When declare a ``Generator``, ``network id`` should be provided.
| ``network id`` is up to each network.

| Let's suppose that the network id of the network is ``mitum``.

.. code-block:: js

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

| For details about ``Generator``, go to **Details - Major Classes** and refer to **Generator**.

| In addition, you must have available account on the network.

| Now, it's done to create operations.

Create Operation Item
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Everything to do by an operation is contained in *operation fact*, not in *operation*.
| *Fact* have the basic information such that ``sender``, ``token``, etc...

| Actually, real constuctions for the operation are contained in *Item*.
| That means you must create items for the operation.

| Let's suppose that you want to create an account following below conditions.

.. code-block:: none

    1. The keys and threshold of the account will be,
        - keys(public key, weight): (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 50), (pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu, 50) 
        - threshold: 100

    2. The initial balance of the account will be,
        - balance(currency id, amount): (MCC, 10000), (PEN, 20000)

| Since the number of keys contained in the account is 2, new account will be *multi-sig account*.

| If every factor of new account have been decided, create an item!

.. code-block:: js

    const key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50) // key(pub, weight)
    const key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50) // key(pub, weight)
    
    const keys = currencyGenerator.createKeys([key1, key2], 100) // createKeys([key1, key2], threshold)

    const amount1 = currencyGenerator.amount("10000", "MCC") // amount(amount, currencyId)
    const amount2 = currencyGenerator.amount("20000", "PEN") // amount(amount, currencyId)
    const amounts = currencyGenerator.createAmounts([amount1, amount2]); // createAmounts([amount1, amount2])

    const createAccountsItem = currencyGenerator.createCreateAccountsItem(keys, amounts); // createCreateAccountsItem(keys, amounts)

* First, create each key by ``Generator.currency.key(public key, weight)``.
* Second, combine all keys with account threshold by ``Generator.currency.createKeys(key list, threshold)``.
* Third, create each amount by ``Generator.currency.amount(amount, currencyId)``.
* Forth, combine all amounts by ``Generator.currency.createAmounts(amount list)``.
* Finally, create an item by ``Generator.currency.createCreateAccountsItem(keys, amounts)

| Of course you can customize the content of items by following constrains.

.. code-block:: none

    - `Keys` created by `createKeys` can contain up to 10 key pairs.
    - `Amounts` created by `createAmounts` can contain up to 10 amount pairs.
    - Moreover, a `fact` can contain multiple items. The number of items in a fact is up to 10, either.

Create Operation Fact
'''''''''''''''''''''''''''''''''''''''''''''''''''

| *Fact* must have not empty ``items``, ``sender``, ``token``, and ``fact hash``.

| Don't worry about ``token`` and ``fact hash`` because they will be filled automatically by SDK.
| The information you must provide is about ``items`` and ``sender``.

| The way to create items has been introduced above section.

| Just be careful that only the account under below conditions can be used as ``sender``.

.. code-block:: none

    1. The account which has been created already.
    2. The account which has sufficient balance of currencies in items.
    3. The account that you(or owners of the account) know its private keys corresponding account public keys.

| Then, create *fact*!

.. code-block:: js

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca" // sender's account address; replace with your address
    const createAccountsFact = currencyGenerator.createCreateAccountsFact(senderAddress, [createAccountsItem]); // createCreateAccountsFact(sender's address, item list)

| If you want to create fact with multiple items, put them all in item list as an array of ``Generator.currency.createCreateAccountsFact(sender's address, item list)``

Create Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Finally, you are in the step to create operation!

| Only thing you need to prepare is **sender's private key**. It is used for signing fact.
| The signature of a private key is included to ``fact_signs`` as a **fact signature**.
| The sum of weights of all signers in ``fact_signs`` should exceeds or be equal to ``sender``'s threshold.

| **Only the signatures of sender account's keys are available to fact_signs!**

| There is ``memo`` in operation but it is not necessary. You can enter something if you need, but be careful because that ``memo`` also affect to ``operation hash``.

| In this example, supposed that ``sender`` is *single-sig account*. That means, only one key exist in the sender's account.
| If ``sender`` is *multi-sig account*, you may add multiple signatures to ``fact_signs``.
| What key must sign is decided by the account's threshold and keys' weights.

.. code-block:: js

    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr" // sender's private key; replace with your private key
    
    const createAccounts = generator.createOperation(createAccountsFact, ""); // createOperation(fact, memo)
    createAccounts.addSign(senderPrivateKey); // addSign(private key); add fact signature to fact_signs 

| Use just ``Generator.createOperation(fact, memo)`` for create operations, not ``Generator.currency.createOperation(fact, memo)``.

| Be sad, an operation can contain only one fact.

Create Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In fact, ``operation`` itself is enough to create an account.

| However, sometimes you may need to wrap multiple operations with a seal.

| Mentioned above, one seal can contain multiple operations.

| The maximum of the number of operations in a seal is decided by the policy of nodes.
| So check how many operations you can include in a seal before create seals.

| Anyway, it is simple to create a seal with **mitum-js-util**.

| What you have to prepare is *private key* from Mitum key package without any conditions.
| Any *btc compressed wif* with suffix *mpr* is okay.

.. code-block:: js

    const anyPrivateKey = "KyK7aMWCbMtCJcneyBZXGG6Dpy2jLRYfx3qp7kxXJjLFnppRYt7wmpr";

    const operations = [createAccounts];
    const seal = generator.createSeal(anyPrivateKey, operations);

| Like ``createOperation``, use ``Generator.createSeal(signer, operation list)``.

| Put all operations to wrap in *operation list*.

Support Operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

---------------------------------------------------
Sign
---------------------------------------------------

Add Fact Sign to Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

---------------------------------------------------
Details
---------------------------------------------------

Get Mitum Keypair
'''''''''''''''''''''''''''''''''''''''''''''''''''

Get Account Address with Keys
'''''''''''''''''''''''''''''''''''''''''''''''''''

Major Classes
'''''''''''''''''''''''''''''''''''''''''''''''''''