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
  - show the proces of
    - creating a python-poetry project
    - setting up a FastAPI "Hello World" example in this python-poetry project

### Prerequisites

- VirtualBox
- Vagrant

### To use

- clone this repo in a new directory
- enter this new directory
- run
      vagrant up


## Vagrant SSH

Setting up the Poetry project can be done inside your Vagrant machine.  
Access this via:

      vagrant ssh

### Poetry project

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
      copy ~/data/main.py.forFastAPI to ~/code/demo/demo/main.py

Run your FastAPI webserver

      uvicorn main:app --reload --host 0.0.0.0 --port 8012
