.. |app| replace:: FastAPI
.. |mod| replace:: Python 3.6+

#######
FastAPI
#######

To run apps built with the `FastAPI
<https://fastapi.tiangolo.com>`_ web framework using Unit:

#. .. include:: ../include/howto_install_unit.rst

#. Create a virtual environment to install |app|'s `PIP package
   <https://fastapi.tiangolo.com/tutorial/#install-fastapi>`_:

   .. code-block:: console

      $ cd /path/to/app/
      $ python3 -m venv venv
      $ source venv/bin/activate
      $ pip install fastapi
      $ deactivate

#. Let's try a version of a `tutorial app
   <https://fastapi.tiangolo.com/tutorial/first-steps/>`_,
   saving it as :file:`/path/to/app/asgi.py`:

   .. code-block:: python

      from fastapi import FastAPI

      app = FastAPI()

      @app.get("/")
      async def root():
          return {"message": "Hello, World!"}

   .. note::

      For something more true-to-life, try the
      `RealWorld example app
      <https://github.com/nsidnev/fastapi-realworld-example-app>`_; just
      install all its dependencies in the same virtual environment where you've
      installed |app| and add the app's :samp:`environment` :ref:`variables
      <configuration-apps-common>` like :samp:`DB_CONNECTION` or
      :samp:`SECRET_KEY` directly to the app configuration in Unit instead of
      the :file:`.env` file.

#. .. include:: ../include/howto_change_ownership.rst

#. Next, :ref:`put together <configuration-python>` the |app| configuration for
   Unit:

   .. code-block:: json

      {
          "listeners": {
              "*:80": {
                  "pass": "applications/fastapi"
              }
          },

          "applications": {
              "fastapi": {
                  "type": "python 3",
                  "user": ":nxt_term:`app_user <User and group values must have access to path and home directories>`",
                  "group": "app_group",
                  "path": ":nxt_term:`/path/to/app/ <Path to the ASGI module>`",
                  "home": ":nxt_term:`/path/to/app/venv/ <Path to the virtual environment, if any>`",
                  "module": ":nxt_term:`asgi <ASGI module filename with extension omitted>`",
                  "callable": ":nxt_term:`app <Name of the callable in the module to run>`"
              }
          }
      }

#. .. include:: ../include/howto_upload_config.rst

#. After a successful update, your app should be available on the listener’s IP
   address and port:

   .. code-block:: console

      $ curl http://localhost

            Hello, World!

   Alternatively, try |app|'s nifty self-documenting features:

.. image:: ../images/fastapi.png
   :width: 100%
   :alt: FastAPI in Unit - Swagger Screen

