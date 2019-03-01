KubeSphere base images
========================================

This repository contains Dockerfiles which serve as base images for various KubeSphere images.

Versions
---------------------------------
s2i image versions currently provided are:
* [core](core/README.md) - rhel7 base + s2i settings
* [base](base/README.md) - s2i-core + development libraries + npm

RHEL versions currently supported are:
* RHEL7

CentOS versions currently supported are:
* CentOS7

Installation
---------------
To build a S2I base image, choose either the CentOS or RHEL based image:

*  **Based image**

    This image is available on DockerHub. To download it run:

    ```
    $ docker pull kubespheredev/s2i-base-centos7
    ```

    Or

    ```
    $ docker pull kubespheredev/s2i-core-centos7
    ```

    To build a S2I base image from scratch run:

    ```
    $ git clone --recursive https://github.com/kubesphere/s2i-base-container.git
    $ cd s2i-base-container
    $ git submodule update --init
    $ make build TARGET=centos7 VERSIONS=base
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of S2I base image.**


