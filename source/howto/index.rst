.. meta::
   :og:description: Run a variety of frameworks and applications, use Unit with Docker, build custom modules, and resolve other issues.

#####
Howto
#####

This section describes various real-life situations and issues that you may
experience with Unit.

.. toctree::
   :hidden:
   :maxdepth: 1

   docker
   integration
   certbot
   Language Modules <modules>
   samples
   walkthrough

- :doc:`docker`: Configure standalone Unit or a Unit-run app in a Docker
  container.

- :doc:`integration`: Front or secure Unit with NGINX.

- :doc:`certbot`: Use EFF's Certbot with Unit to simplify certificate
  manipulation.

- :doc:`modules`: Build new modules or prepare custom packages for
  Unit.

- :doc:`samples`: Reuse sample app configurations for all languages
  supported by Unit.

- :doc:`walkthrough`: Follow an end-to-end guide to application configuration
  in Unit.


.. _howto-frameworks:

**********
Frameworks
**********

With Unit, you can configure a diverse range of applications based on the
following frameworks:

.. toctree::
   :maxdepth: 1

   cakephp
   catalyst
   codeigniter
   django
   djangochannels
   express
   fastapi
   flask
   guillotina
   laravel
   pyramid
   quart
   responder
   sanic
   starlette
   symfony
   yii

************
Applications
************

You can also make use of detailed setup instructions for popular web apps such
as:

.. toctree::
   :maxdepth: 1

   bugzilla
   datasette
   drupal
   grafana
   jira
   joomla
   mediawiki
   mercurial
   moin
   nextcloud
   opengrok
   phpbb
   redmine
   reviewboard
   trac
   wordpress

If you are interested in a specific use case not yet listed here, please `post
a feature request <https://github.com/nginx/unit-docs/issues>`_ on GitHub.
