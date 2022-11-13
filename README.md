# Vagrant - Python - Poetry - for FastAPI

This is a repository for setting up a FastAPI site. This is set up using Vagrant and Python Poetry.

These links have been visited during this journey:

- [Vagrant](https://developer.hashicorp.com/vagrant)
- [Python Poetry](https://python-poetry.org/docs/)
- [FastAPI](https://fastapi.tiangolo.com/)
- [FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/first-steps/)

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

I have done this setup on my home Linux laptop, running Linux Mint LMDE

    [bart@thinkpad-t480s][~] $ screenfetch
                                           bart@thinkpad-t480s
     MMMMMMMMMMMMMMMMMMMMMMMMMmds+.        OS: Linuxmint 5 elsie
     MMm----::-://////////////oymNMd+`     Kernel: x86_64 Linux 5.10.0-19-amd64
     MMd      /++                -sNMd:    Uptime: 16d 14h 48m
     MMNso/`  dMM    `.::-. .-::.` .hMN:   Packages: 2285
     ddddMMh  dMM   :hNMNMNhNMNMNh: `NMm   Shell: bash 5.1.4
         NMm  dMM  .NMN/-+MMM+-/NMN` dMM   Resolution: 4480x1440
         NMm  dMM  -MMm  `MMM   dMM. dMM   DE: GNOME
         NMm  dMM  -MMm  `MMM   dMM. dMM   WM: Muffin
         NMm  dMM  .mmd  `mmm   yMM. dMM   WM Theme: Mint-Y-Dark (Mint-Y)
         NMm  dMM`  ..`   ...   ydm. dMM   GTK Theme: Mint-Y [GTK2/3]
         hMM- +MMd/-------...-:sdds  dMM   Icon Theme: Mint-Y
         -NMm- :hNMNNNmdddddddddy/`  dMM   Font: Ubuntu 10
          -dMNs-``-::::-------.``    dMM   Disk: 331G / 465G (75%)
           `/dMNmy+/:-------------:/yMMM   CPU: Intel Core i7-8650U @ 8x 4.2GHz [44.0°C]
              ./ydNMMMMMMMMMMMMMMMMMMMMM   GPU: Mesa Intel(R) UHD Graphics 620 (KBL GT2)
                 \.MMMMMMMMMMMMMMMMMMM     RAM: 10479MiB / 15755MiB


- VirtualBox
  - in my setup I used VirtualBox 6.1
- Vagrant
  - in my setup I used Vagrant 2.2.14

### Let's start

- clone this repo in a new directory
- enter this new directory
- fire up the Vagrant VirtualBox machine

      vagrant up


## Vagrant SSH

Setting up the Poetry project can be done inside your Vagrant machine.  
Access this via:

    vagrant ssh

Your prompt should look like this now:

    [vagrant@python-poetry][~]$


### Poetry project

The following only describes the happy path. Hope all goes well for you too.  
If not, let the internet help you.

Create a new project in the code directory:

    cd ~/code
    poetry new demo
    cd demo
    poetry shell

Your prompt should look like this now:

    (demo-py3.9) [vagrant@python-poetry][~/code/demo]$

Add the python modules needed for your FastAPI project

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


Run your FastAPI webserver (in the ~/code/demo/demo/ directory)

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
