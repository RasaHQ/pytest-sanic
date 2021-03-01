.. pytest-sanic documentation master file, created by
   sphinx-quickstart on Wed Aug 17 13:50:33 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

pytest-sanic
============

.. image:: https://travis-ci.org/yunstanford/pytest-sanic.svg?branch=master
    :alt: build status
    :target: https://travis-ci.org/yunstanford/pytest-sanic

A pytest plugin for `Sanic <http://sanic.readthedocs.io/en/latest/>`_. It helps you to test your code asynchronously.

This plugin provides:

* very easy testing with async coroutines
* common and useful fixtures
* asynchronous fixture support
* test_client/sanic_client for Sanic application
* test_server for Sanic application


-----------
Quick Start
-----------

You don't have to load ``pytest-sanic`` explicitly. ``pytest`` will do it for you. Just write tests like,

.. code-block:: python

    async def test_sanic_db_find_by_id(app):
        """
        Let's assume that, in db we have,
            {
                "id": "123",
                "name": "Kobe Bryant",
                "team": "Lakers",
            }
        """
        doc = await app.db["players"].find_by_id("123")
        assert doc.name == "Kobe Bryant"
        assert doc.team == "Lakers"


Here's much more realistic & useful example,

.. code-block:: python

    from .app import create_app

    @pytest.yield_fixture
    def app():
        app = create_app(test_config, **params)
        yield app

    @pytest.fixture
    def test_cli(loop, app, sanic_client):
        return loop.run_until_complete(sanic_client(app))

    async def test_index(test_cli):
        resp = await test_cli.get('/')
        assert resp.status_code == 200

    async def test_player(test_cli):
        resp = await test_cli.get('/player')
        assert resp.status_code == 200


Contents:

.. toctree::
   :maxdepth: 2

   installation
   example
   fixtures
   tips
   development
   reference


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
