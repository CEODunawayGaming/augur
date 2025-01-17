Database setup
===============

This database setup is specific to an Amazon Linux 2 instance. It should also work for Amazon Linux.

One of the reasons that Augur is so powerful is because of its `unified data model <../schema/data-model.html>`_.
To ensure this data model remains performant with large amounts of data, we use PostgreSQL as our database engine. 
We'll need to set up a PostgreSQL instance and create a database, after which Augur can take care of the rest.
Make sure to save off the credentials you use when creating the database; you'll need them again to configure Augur.

PostgreSQL Information
~~~~~~~~~~~~~~~~~~~~~~~~

Before you can install our schema, you will need to make sure you have **write access** to a PostgreSQL 10 or later database. If you're looking for the fastest possible way to get Augur started, we recommend use our `database container <../docker/docker.html>`_. If you're looking to collect data long-term, we recommend following the rest of this tutorial and setting up a persistent PostgreSQL installation.

.. warning::

    If you want to collect data over the long term, we strongly advise against `using a Docker container for your database <https://vsupalov.com/database-in-docker/>`_.

If you're a newcomer to PostgreSQL, you can follow their excellent instructions `here <https://www.postgresql.org/docs/12/tutorial-install.html>`_ to set it up for your machine of choice.

PostgreSQL Quick Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, you must actually install PostgreSQL on your machine. One of the necessary packages (*postgresql-client*) does not technically exist on Amazon Linux servers, but installing *amazon-linux-extras* allows for the same functionality.

.. code-block:: bash

	$ sudo yum install postgres postgresql-server
	$ sudo amazon-linux-extras install postgresql-server

Don't forget to activate and start the service!

.. code-block:: bash
	
	$ sudo service postgresql initdb
	$ sudo service postgresql start

You should then be able to access the user `postgres` and the `psql` service.

.. code-block:: bash

        $ sudo su - postgres
        $ psql

Creating a Database
~~~~~~~~~~~~~~~~~~~~~

After you set up your PostgreSQL instance, you'll need to create a database and user with the correct permissions. You can do this with the SQL commands below, but be sure to change the password!

.. code-block:: postgresql 
    
    postgres=# CREATE DATABASE augur;
    postgres=# CREATE USER augur WITH ENCRYPTED PASSWORD 'password';
    postgres=# GRANT ALL PRIVILEGES ON DATABASE augur TO augur;

To be able to long into the database, you'll probably have to change some permissions in a configuration file *pg_hba.conf*. This file could be installed in a few different places, so searching for it is probably the best way to find it.

.. code-block:: bash

	$ find / -iname 'pg_hba.conf'
	
Within the file, there are two sections that are uncommented. Within the first section, change any mention of "*peer*" and "*ident*" to "*md5*". Everywhere else, change "*peer*" and "*ident*" to "*trust*". This should change the login specifications for your database so that augur can actually use the database you create.

Then restart the service so the changes to the configuration file take place.

.. code-block:: bash

	$ sudo service postgresql restart

For example, if you were using ``psql`` to connect to an instance on your machine ``localhost`` under the default user ``postgres`` on the default PostgreSQL port ``5432``, you might run something like this to connect to the server:

.. code-block:: bash

    $ psql -h localhost -U augur -p 5432

Once you've got the database setup, Augur will install the schema for you. You're now ready to `install Augur <installation.html>`_!
