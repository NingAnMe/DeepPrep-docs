.. include:: links.rst

============
Installation
============

Run with Docker (step-by-step)
---------------------------------

DeepPrep provides a Docker image as the recommended way to get started.

.. note::
    **Required Environment**
        + Graphics Driver VRAM: >= 24GB
        + Disk: >= 20G
        + RAM: >= 16GB
        + Swap space: >=16G
        + Ubuntu:  >= 20.04
        + NVIDIA Driver: >= 11.8

1. Install Docker if you don't have one (`Docker Installation Page`_).

.. _Docker Installation Page: https://www.docker.com/get-started/


2. Test Docker with the ``hello-world`` image::

    $ docker run -it --rm hello-world

The following message should appear:

::

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
     https://hub.docker.com/

    For more examples and ideas, visit:
     https://docs.docker.com/get-started/

3. Please make sure GPUs are accessible by adding the flag ``--gpus all``::

    $ docker run -it --gpus all --rm hello-world

The same output as before is expected.

4. Pull the Docker image::

    $ docker pull ninganme/deepprep:v23.1.0


