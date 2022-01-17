===================================================
Installation
===================================================

* Please download the source code of `Mitum Currency <https://github.com/spikeekips/mitum-currency>`_.

* Using **Git**,

.. code-block:: sh

    $ git clone https://github.com/spikeekips/mitum-currency.git

* Build exe file.

.. code-block:: sh

    $ cd mitum-currency
    
    $ go build -ldflags="-X 'main.Version=v0.0.1-tutorial'" -o ./mc ./main.go
    
    $ ./mc version
    v0.0.1