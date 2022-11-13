# Vagrant - Python - Poetry - for FastAPI

This is a repository for setting up a FastAPI site.

## Vagrant UP

### General

This Vagrantfile will
- use VirtualBox
- install debian (bento/debian-11.4)
- set up network 192.168.80.12
- forward port 8012
- synch folder data (containing some handy files)
- synch folder code (for setting up poetry project and FastAPI)
- configure data/dot-bashrc.fordebian as the .bashrc
- configure data/dot-tmux.conf.fordebian as the .tmux.conf (for colored prompt)
- provision (as root)
  - python3-disutils (needed for Poetry)
  - curl, tmux, tree, jq (some handy little tools)
- provision (as user)
  - python-poetry
  - show the proces of (yes, you need to do this part yourself)
    - creating a python-poetry project
    - setting up a FastAPI "Hello World" example in this python-poetry project

### Prerequisites

- VirtualBox
- Vagrant

### To start

- clone this repo in a new directory
- enter this new directory
- fire up the Vagrant VirtualBox machine

      vagrant up


## Vagrant SSH

Setting up the Poetry project can be done inside your Vagrant machine.  
Access this via:

    vagrant ssh

### Poetry project

The following only describes the happy path. Hope all goes well for you too.  
If not, let the internet help you.

Create a new project:

    cd ~/code
    poetry new demo

Add your python modules needed for your FastAPI project

    cd demo
    poetry shell
    poetry add fastapi
    poetry add uvicorn

Create your FastAPI app

    cd demo
    cp ~/data/main.py.forFastAPI ~/code/demo/demo/main.py

Ready? It should be, if your setup looks like this:

(demo-py3.9) [vagrant@python-poetry][~/code/demo/demo]$ tree ~/code

    /home/vagrant/code
    ├── demo
    │   ├── demo
    │   │   ├── __init__.py
    │   │   └── main.py
    │   ├── poetry.lock
    │   ├── pyproject.toml
    │   ├── README.md
    │   └── tests
    │       └── __init__.py
    └── someinfo.txt


Run your FastAPI webserver

    uvicorn main:app --reload --host 0.0.0.0 --port 8012

The following output indicates that all went well, and your api is ready to be served:

    (demo-py3.9) [vagrant@python-poetry][~/code/demo/demo]$ uvicorn main:app --reload --host 0.0.0.0 --port 8012
    INFO:     Will watch for changes in these directories: ['/home/vagrant/code/demo/demo']
    INFO:     Uvicorn running on http://0.0.0.0:8012 (Press CTRL+C to quit)
    INFO:     Started reloader process [12518] using StatReload
    INFO:     Started server process [12520]
    INFO:     Waiting for application startup.
    INFO:     Application startup complete.


Surf to the FastAPI site

- Inside your Vagrant SSH shell:

      curl -s 'http://127.0.0.1:8012' | jq

- Via your browser on your host:

      http://192.168.80.12:8012/
      http://192.168.80.12:8012/docs
      http://192.168.80.12:8012/redoc
