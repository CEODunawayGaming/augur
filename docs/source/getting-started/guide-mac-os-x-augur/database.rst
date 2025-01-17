Database setup
===============

One of the reasons that Augur is so powerful is because of its `unified data model <../schema/data-model.html>`_.
In order to ensure this data model remains performant with large amounts of data, we use PostgreSQL as our database engine. 
We'll need to set up a PostgreSQL instance and create a database, after which Augur can take care of the rest.
Make sure to save off the credentials you use when you create the database, you'll need them again to configure Augur.

PostgreSQL Installation
~~~~~~~~~~~~~~~~~~~~~~~~

Before you can install our schema, you will need to make sure you have write access to a PostgreSQL 10 or later database. If you're looking for the fastest possible way to get Augur started, we recommend use our `database container <../docker/docker.html>`_. If you're looking to collect data long term, we recommend following the rest of this tutorial and setting up a persistent PostgreSQL installation.

.. warning::

    If you want to collect data over the long term, we strongly advise against `using a Docker container for your database <https://vsupalov.com/database-in-docker/>`_.

If you're a newcomer to PostgreSQL, you can follow their excellent instructions `here <https://www.postgresql.org/download/macosx/>`_ to set it up for macOS. We recommend using ``Postgres.app`` on macOS which can be installed `with this link <https://postgresapp.com/>`_. Alternatively, if `homebrew <https://brew.sh/>`_ is installed, the following command can be used to install PostgreSQL.

.. code-block:: bash 
    
    $ brew install postgres

Creating a Database
~~~~~~~~~~~~~~~~~~~~~

Once PostgreSQL is installed on your machine you can set up an instance by using the command below.

.. code-block:: bash 
    
    $ psql postgres

Once inside the instance you'll need to create a database and user with the correct permissions. You can do this with the SQL commands below, but be sure to change the password!

.. code-block:: postgresql 
    
    postgres=# CREATE DATABASE augur;
    postgres=# CREATE USER augur WITH ENCRYPTED PASSWORD 'password';
    postgres=# GRANT ALL PRIVILEGES ON DATABASE augur TO augur;

For example, if you were using ``psql`` to connect to an instance on your machine ``localhost`` under the default user ``postgres`` on the default PostgreSQL port ``5432``, you might run something like this to connect to the server:

.. code-block:: bash

    $ psql -h localhost -U augur -p 5432

Once you've got the database setup, Augur will take care of installing the schema for you. You're now ready to `install Augur <installation.html>`_!
