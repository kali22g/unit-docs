.. |app| replace:: Review Board
.. |mod| replace:: Python 2.7
.. |app-preq| replace:: prerequisites
.. _app-preq: https://www.reviewboard.org/docs/manual/dev/admin/installation/linux/#before-you-begin
.. |app-link| replace:: core files
.. _app-link: https://www.reviewboard.org/docs/manual/dev/admin/installation/linux/#installing-review-board

############
Review Board
############

To run the `Review Board
<https://www.reviewboard.org>`_ code review tool using Unit:

#. .. include:: ../include/howto_install_unit.rst

#. .. include:: ../include/howto_install_prereq.rst

   .. note::

      We'll use Unit as the web server, so you can skip the corresponding step.

#. Install the |app-link|_ and create a `site
   <https://www.reviewboard.org/docs/manual/dev/admin/installation/creating-sites/>`_.
   Here, it's :samp:`/path/to/app/`; use a real path in your configuration:

   .. code-block:: console

      $ rb-site install /path/to/app/

            * Welcome to the Review Board site installation wizard

                This will prepare a Review Board site installation in:

                /path/to/app

                We need to know a few things before we can prepare your site for
                installation. This will only take a few minutes.
                ...

#. Add the :file:`.py` extension to the WSGI module's name to make it
   discoverable by Unit, for example:

   .. code-block:: console

      $ mv /path/to/app/htdocs/reviewboard.wsgi /path/to/app/htdocs/wsgi.py

#. .. include:: ../include/howto_change_ownership.rst

   Also, make sure the following directories are `writable
   <https://www.reviewboard.org/docs/manual/dev/admin/installation/creating-sites/#changing-permissions>`_:

   .. code-block:: console

      $ chmod u+w /path/to/app/htdocs/media/uploaded/
      $ chmod u+w /path/to/app/data/

#. Next, :ref:`put together <configuration-python>` the |app| configuration for
   Unit:

   .. code-block:: json

      {
          "listeners": {
              "*:80": {
                  "pass": "routes/rb"
              }
          },

          "routes": {
              "rb": [
                  {
                      "match": {
                          ":nxt_term:`uri <Static file directories>`": [
                              "/media/*",
                              "/static/*",
                              "/errordocs/*"
                          ]
                      },

                      "action": {
                          "share": "/path/to/app/htdocs/"
                      }
                  },
                  {
                      "action": {
                          "pass": "applications/rb"
                      }
                  }
              ]
          },

          "applications": {
              "rb": {
                  "type": "python 2",
                  "user": ":nxt_term:`app_user <User and group values must have access to path directory>`",
                  "group": "app_group",
                  "path": ":nxt_term:`/path/to/app/htdocs/ <Path to the WSGI module>`",
                  "module": ":nxt_term:`wsgi <WSGI module filename with extension omitted>`"
              }
          }
      }

#. .. include:: ../include/howto_upload_config.rst

   After a successful update, browse to http://localhost and `set up
   <https://www.reviewboard.org/docs/manual/dev/admin/#configuring-review-board>`_
   your |app| installation:

   .. image:: ../images/reviewboard.png
      :width: 100%
      :alt: Review Board on Unit - Dashboard Screen
