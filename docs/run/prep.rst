===================================================
Prerequisite
===================================================

---------------------------------------------------
Database
---------------------------------------------------

| `Mitum Currency` uses ``MongoDB`` as its main storage engine.

| To run the Mitum currency node, you need to prepare mongodb first.

Installation and Setup
'''''''''''''''''''''''''''''''''''''''''''''''''''

* `Manual Installation Guide <https://docs.mongodb.com/manual/installation/>`_

* Using `Docker Container <https://hub.docker.com/_/mongo>`_,

.. code-block:: sh

    $ docker run --name mc-test-mongo-27017 -it -p 27017:27017 -d mongodb

* About DB setup, refer to `Configuration <https://protocon-general-doc.readthedocs.io/en/develop/docs/run/config.html>`_.

---------------------------------------------------
Golang
---------------------------------------------------

| `Mitum Currency` is developed using the programming language `Go <https://golang.org>_`.

| To create an executable binary, you need to build it from the source code.
| We do not provide detailed instructions for installing the Go language here.
| You must have the Golang installed with at least version 1.16 to build Mitum currency.

| For more information, refer to `How to Install Go <https://go.dev/doc/install>`_.