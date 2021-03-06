===============================
manila-ui
===============================

Manila Management Dashboard

* Free software: Apache license

.. Uncomment these bullet items when the project is integrated into OpenStack
.. item * Documentation: http://docs.openstack.org/developer/manila-ui
.. item * Source: http://git.openstack.org/cgit/openstack/manila-ui

* Bugs: http://bugs.launchpad.net/manila-ui

Installation instructions
-------------------------

Begin by cloning the Horizon and Manila UI repositories::

    git clone https://github.com/openstack/horizon
    git clone https://github.com/hp-storage/manila-ui

Create a virtual environment and install Horizon dependencies::

    cd horizon
    python tools/install_venv.py

Set up your ``local_settings.py`` file::

    cp openstack_dashboard/local/local_settings.py.example openstack_dashboard/local/local_settings.py

Open up the copied ``local_settings.py`` file in your preferred text
editor. You will want to customize several settings:

-  ``HORIZON_CONFIG`` requires, an entry, ``customization_module``,
   that refers to ``manila_ui.overrides``::

    HORIZON_CONFIG = {
        ...
        'js_spec_files': [],
        'customization_module': 'manila_ui.overrides',
    }

-  ``OPENSTACK_HOST`` should be configured with the hostname of your
   OpenStack server. Verify that the ``OPENSTACK_KEYSTONE_URL`` and
   ``OPENSTACK_KEYSTONE_DEFAULT_ROLE`` settings are correct for your
   environment. (They should be correct unless you modified your
   OpenStack server to change them.)


Install Manila UI with all dependencies in your virtual environment::

    tools/with_venv.sh pip install -e ../manila-ui/

And enable it in Horizon::

    cp ../manila-ui/manila_ui/enabled/_90_manila_*.py openstack_dashboard/local/enabled


Starting the app
----------------

If everything has gone according to plan, you should be able to run::

    ./run_tests.sh --runserver 0.0.0.0:8080

and have the application start on port 8080. The horizon dashboard will
be located at http://localhost:8080/

Unit testing
------------

The unit tests can be executed directly from within this Manila UI plugin
project directory by using::

    cd ../manila-ui
    ./run_tests.sh

This is made possible by the dependency in test-requirements.txt upon the
horizon source, which pulls down all of the horizon and openstack_dashboard
modules that the plugin uses.
