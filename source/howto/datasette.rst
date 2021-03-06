.. |app| replace:: Datasette
.. |mod| replace:: Python 3.6+

#########
Datasette
#########

To run the `Datasette
<https://docs.datasette.io/en/stable/>`_ data exploration tool using Unit:

#. .. include:: ../include/howto_install_unit.rst

#. Create a virtual environment to install |app|'s `PIP package
   <https://docs.datasette.io/en/stable/installation.html#using-pip>`_:

   .. code-block:: console

      $ cd /path/to/app/
      $ python3 -m venv venv
      $ source venv/bin/activate
      $ pip install datasette
      $ deactivate

#. Running |app| in Unit requires a wrapper to expose the `application object
   <https://github.com/simonw/datasette/blob/4f7c0ebd85ccd8c1853d7aa0147628f7c1b749cc/datasette/app.py#L169>`_
   as the ASGI callable. Let's use the following basic version, saving it as
   :file:`/path/to/app/asgi.py`:

   .. code-block:: python

      import glob
      from datasette.app import Datasette

      application = Datasette(glob.glob('*.db')).app()

#. .. include:: ../include/howto_change_ownership.rst

#. Next, :ref:`put together <configuration-python>` the |app| configuration for
   Unit:

   .. code-block:: json

      {
          "listeners": {
              "*:80": {
                  "pass": "applications/datasette"
              }
          },

          "applications": {
              "datasette": {
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

#. After a successful update, |app| should be available on the listener’s IP
   address and port:

   .. image:: ../images/datasette.png
      :width: 100%
      :alt: Datasette in Unit - Query Screen
