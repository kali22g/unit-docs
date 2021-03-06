.. |app| replace:: Guillotina
.. |mod| replace:: Python 3.7+

##########
Guillotina
##########

To run apps built with the `Guillotina
<https://guillotina.readthedocs.io/en/latest/>`_ web framework using Unit:

#. .. include:: ../include/howto_install_unit.rst

#. Create a virtual environment to install |app|'s `PIP package
   <https://guillotina.readthedocs.io/en/latest/training/installation.html>`_:

   .. code-block:: console

      $ cd /path/to/app/
      $ python3 -m venv venv
      $ source venv/bin/activate
      $ pip install guillotina
      $ deactivate

#. Let's try a version of the `tutorial app
   <https://guillotina.readthedocs.io/en/latest/#build-a-guillotina-app>`_,
   saving it as :file:`/path/to/app/asgi.py`:

   .. code-block:: python

      from guillotina import configure
      from guillotina import content
      from guillotina import schema
      from guillotina.factory import make_app
      from zope import interface


      class IMyType(interface.Interface):
          foobar = schema.TextLine()


      @configure.contenttype(
          type_name="MyType",
          schema=IMyType,
          behaviors=["guillotina.behaviors.dublincore.IDublinCore"],
      )
      class MyType(content.Resource):
          pass


      @configure.service(
          context=IMyType,
          method="GET",
          permission="guillotina.ViewContent",
          name="@foobar",
      )
      async def foobar_service(context, request):
          return {"foobar": context.foobar}


      :nxt_term:`application <Callable name that Unit looks for>` = make_app(
              settings={
                  "applications": ["__main__"],
                  "root_user": {"password": "root"},
                  "databases": {
                      "db": {"storage": "DUMMY_FILE", "filename": "dummy_file.db",}
                  },
              }
          )

   Note that all server calls and imports are removed.

#. .. include:: ../include/howto_change_ownership.rst

#. Next, :ref:`put together <configuration-python>` the |app| configuration for
   Unit:

   .. code-block:: json

      {
          "listeners": {
              "*:80": {
                  "pass": "applications/guillotina"
              }
          },

          "applications": {
              "guillotina": {
                  "type": "python 3",
                  ":nxt_term:`user <User and group values must have access to path and home directories>`": "app_user",
                  "group": "app_group",
                  ":nxt_term:`path <Path to the ASGI module>`": "/path/to/app/",
                  ":nxt_term:`home <Path to the virtual environment, if any>`": "/path/to/app/venv/",
                  ":nxt_term:`module <ASGI module filename with extension omitted>`": "asgi",
                  ":nxt_term:`protocol <Protocol hint for Unit, required to run Guillotina apps>`": "asgi"
              }
          }
      }

#. .. include:: ../include/howto_upload_config.rst

#. After a successful update, your app should be available on the listener’s IP
   address and port:

   .. code-block:: console

      $ curl -XPOST --user root:root http://localhost/db \
             -d '{ "@type": "Container", "id": "container" }'

            {"@type":"Container","id":"container","title":"container"}

      $ curl --user root:root http://localhost/db/container

            {
                "@id": "http://localhost/db/container",
                "@type": "Container",
                "@name": "container",
                "@uid": "84651300b2f14170b2b2e4a0f004b1a3",
                "@static_behaviors": [
                ],
                "parent": {
                },
                "is_folderish": true,
                "creation_date": "2020-10-16T14:07:35.002780+00:00",
                "modification_date": "2020-10-16T14:07:35.002780+00:00",
                "type_name": "Container",
                "title": "container",
                "uuid": "84651300b2f14170b2b2e4a0f004b1a3",
                "__behaviors__": [
                ],
                "items": [
                ],
                "length": 0
            }
