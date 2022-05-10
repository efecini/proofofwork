# Proof-of-Code Server

This is a Flask application which implements a JSON API for
consumption by the frontend React application.

The main libraries used are Flask-SQLAlchemy and psycopg2 which
together allow using PostgreSQL for application data.

Since the application's specs are quite simple, the application only
requires a few tables to store its data model, see the
[models](./poc/models) directory for the corresponding SQLAlchemy
model definitions.

Alembic is used for defining [migrations](./migrations) to these data
models.

This application follows a Model-View-Controller (MVC) pattern, and has the
following structure:

 * poc/controllers: contains the "controller" part, server side.   While,
   conceptually, controller code is also in the frontend javascript side,
   here  is located the server-side controller code.  Specifically, any code
   that  deals with web framework directly (Flask) is here, leaving the rest
   of the code base completely web framwork agnostic;

 * poc/validation: this contains simple utility functions to help validate
   that the untrusted data received from the web brower is actually valid and
   neither a user error, frontend programming error, or even a security
   attack;

 * poc/models: here are the SQL-Alchemy based database models.  Not only
   models of the DB tables, but it may occasionally get some simple helper
   functions or methods to facilitate working with the DB.

 * poc/services: here is the main "business logic" part of the code base.
   This code is kept clean by not having to deal with web requests directly.


```
	[browser] --> [controllers] --(validation)--> [services] --> [models]
```

There are only a handful of [controller routes](./poc/controllers)
which together implement the simple API the application requires.

Testing is done through `pytest`.  This code base follows the
[Tests as part of application code](https://docs.pytest.org/en/6.2.x/goodpractices.html#tests-as-part-of-application-code)
layout.
