# MicroServicesDocumentation

[![Documentation Status](https://readthedocs.org/projects/microservicesdocumentation/badge/?version=latest)](https://readthedocs.org/projects/microservicesdocumentation/?badge=latest)


The docs for the MicroServicesProject which is hosted at [http://microservicesdocumentation.readthedocs.org/en/latest/](http://microservicesdocumentation.readthedocs.org/en/latest/)

## Setting Up

1. Start and activate environment
		
		virtualenv [location to local envs]
		source [location to local envs]/bin/activate

1. Run the requirements 

		pip install -r requirements.txt

## Build the Docs

    cd docs     
    make html

## Auto Build the Docs as you Edit

	cd docs
	sphinx-autobuild source build/html -p3000