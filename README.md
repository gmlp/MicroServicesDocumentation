# MicroServicesDocumentation

[![Documentation Status](https://readthedocs.org/projects/microservicesdocumentation/badge/?version=latest)](https://readthedocs.org/projects/microservicesdocumentation/?badge=latest)


The docs for the MicroServicesProject

## Setting Up

1. Start and activate environment
		
		Virtualenv env
		source env/bin/activate

1. Run the requirements 

		pip install -r requirements.txt

## Build the Docs

    cd docs     
    make html

## Auto Build the Docs as you Edit

	sphinx-autobuild source build/html -p3000