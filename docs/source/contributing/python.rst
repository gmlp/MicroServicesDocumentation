Writing a MicroService with Python (Django)
===========================================

Typical Project Layout
-----------------------

Toolset
--------

* Documentation: Sphinx + `ReadTheDocs <https://readthedocs.org/>`_.
* Django Rest Framework    

Project Setup
-------------

**Instructions**

1. Setup and activate your virtual environment

  ::

    virtualenv env
    source env/bin/activate

2. Create a requirements.txt file with the following and run it

  ::

    django==1.7
    gunicorn
    requests
    djangorestframework==3
    django-rest-swagger
    django-filter

    ## dev requirements
    sphinx
    sphinx_rtd_theme
    mock
    responses
    ipdb
    ipython

    ## Test and quality analysis

    pylint
    coverage
    django-jenkins
    django-extensions
    django-cors-headers

    ## custom libs:
    -e git://github.com/TangentMicroServices/PythonAuthenticationLib.git#egg=tokenauth

Run the requirements file using::

    pip install -r requirements.txt

3. Create the python project

  ::

    django-admin.py startproject projectservice .

  NOTE:

  * projectservice all lowercase 
  * note that . at the end: so it creates it in the current directory

4. Check that your structure is as follows::

    LICENSE     
    README.md   
    manage.py   
    requirements.txt
    projectservice    
      __init__.py 
      settings.py 
      urls.py   
      wsgi.py

5. Create an API app::

    python manage.py startapp api

6. Create api.py in the api app::

    touch api/api.py

7. Add the following to settings.py::

    # CUSTOM AUTH
    AUTHENTICATION_BACKENDS = (
        'django.contrib.auth.backends.ModelBackend',
        'tokenauth.authbackends.TokenAuthBackend'
    )

    ## REST
    REST_FRAMEWORK = {
        'DEFAULT_PERMISSION_CLASSES': (
            'rest_framework.permissions.IsAuthenticated',
        ),
        'DEFAULT_AUTHENTICATION_CLASSES': (
            'tokenauth.authbackends.RESTTokenAuthBackend',        
        )
    }

    # Services:

    ## Service base urls without a trailing slash:
    USER_SERVICE_BASE_URL = 'http://staging.userservice.tangentme.com'

    JENKINS_TASKS = (
        'django_jenkins.tasks.run_pylint',
        'django_jenkins.tasks.with_coverage',
        # 'django_jenkins.tasks.run_sloccount',
        # 'django_jenkins.tasks.run_graphmodels'
    )

    PROJECT_APPS = (
        'api',
    )

8. Update INSTALLED_APPS in settings.py::

    INSTALLED_APPS = (

        ...

        ## 3rd party
        'rest_framework',

        ## custom
        'tokenauth',
        'api',

        # testing etc:
        'django_jenkins',
        'django_extensions',
        'corsheaders',
    )

9. Update MIDDLEWARE_CLASSES in setttings.py::

    MIDDLEWARE_CLASSES = (

        ## add this:
        'tokenauth.middleware.TokenAuthMiddleware',
        'corsheaders.middleware.CorsMiddleware',
        'django.middleware.common.CommonMiddleware',
    )

>Note that CorsMiddleware needs to come before Django's CommonMiddleware if you are using Django's USE_ETAGS = True setting, otherwise the CORS headers will be lost from the 304 not-modified responses, causing errors in some browsers.

10. Update settings.py with the following setting at the bottom

    ::

        CORS_ORIGIN_ALLOW_ALL = True

11. Create some end points 

- `Django REST Framework <http://www.django-rest-framework.org/>`_.


12. Build the documentation

  ::

    sphinx-quickstart

This will create a folder called /docs and the structure should like this this::

    Makefile  
    make.bat
    build/    
    source/
      _static   
      _templates  
      conf.py   
      index.rst

13. Add /docs/build/ to .gitignore file


14. Write your own documentation as you go - `RST Docs <http://docutils.sourceforge.net/docs/user/rst/quickref.html>`_.

Authentication
--------------

Documenting
------------

Continious Integration with Jenkins
----------------------------------------

**Requirements**

* pip install pylint
* pip install coverage
* pip install django-jenkins
* pip install django-extensions

**Instructions**

1. Install requirements::

    pip install -r requirements.txt
    pip install pylint
    pip install coverage
    pip install django-jenkins
    pip install django-extensions

2. Configure settings.py::

    JENKINS_TASKS = (
      'django_jenkins.tasks.run_pylint',
      'django_jenkins.tasks.with_coverage',
      # 'django_jenkins.tasks.run_sloccount',
      # 'django_jenkins.tasks.run_graphmodels'
    )

    ## Apps to run analysis over:
    PROJECT_APPS = (
        'api',
    )

3. Run:: 

    `./manage.py jenkins`

This will:

* Run tests (build junit report)
* Generate coverage report (cobertura)
* Run pylint (generate checkstyle report)

All files are generated in the `reports` directory
