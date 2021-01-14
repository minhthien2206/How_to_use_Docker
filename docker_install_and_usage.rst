.. raw:: html
   
    <h1 align="center"> Docker: Install and Usage on UBUNTU 18.04 </h1>
    

**Author: Truong Dong Do** | Updated: January 14, 2021

Introduction
------------
`Docker <https://www.docker.com/>`__ is an application that simplifies the process of managing application processes in containers. Containers let you run your applications in resource-isolated processes. They’re similar to virtual machines, but containers are more portable, more resource-friendly, and more dependent on the host operating system.

Prerequisites
------------
- Ubuntu 18.04

0. Install GPU Driver:
--------------------
- `Check the suitable GPU Driver <https://www.nvidia.com/download/index.aspx?lang=en-us>`_ | `GPU support <https://www.tensorflow.org/install/gpu#software_requirements>`_

.. code:: bash

   # Add NVIDIA package repositories
   wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
   sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
   sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
   sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"
   sudo apt-get update
   
   # Install NVIDIA driver
   sudo apt-get install --no-install-recommends nvidia-driver-450
   # Reboot. Check that GPUs are visible using the command: nvidia-smi

- Verify NVIDIA Driver:

.. code:: bash

    nvidia-smi

1. Docker 2.0 installation Procedures:
----------------------------------------
- 1.0. If there is nvidia-docker 1.0 already, it needs to be removed. (Skip it if you do not have it.)

.. code:: bash

   docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 
   docker ps -q -a -f volume={} | xargs -r docker rm -f
   sudo apt-get purge nvidia-docker
   
- 1.1. Set the repository and update. 
 
.. code:: bash

   curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
     sudo apt-key add -
   distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
   curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
     sudo tee /etc/apt/sources.list.d/nvidia-docker.list
   sudo apt-get update
  
.. image:: 1.1.png
  :width: 400
  :align: center
  :alt: Image 1.1

- 1.2. Install nvidia-docker 2.0. 

.. code:: bash

   sudo apt-get install nvidia-docker2
   sudo pkill -SIGHUP dockerd

If you get the successfully installation. Verify it by run the nvidia-docker 2.0:

.. code:: bash
   
   sudo docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
   
.. image:: 1.2_2.png
  :width: 400
  :align: center
  :alt: Image 1.2_2  
The nvidia image can be pulled. And we can check the GPU information by nvidia-smi.

If you get the error messenger as in image bellow (Please follow step 1.3 to 1.5).

.. image:: 1.2_1.png
  :width: 400
  :align: center
  :alt: Image 1.2_1 
  
- 1.3. Check the versions nvidia-docker 2.0 on github.

.. code:: bash
   
   apt-cache madison nvidia-docker2 nvidia-container-runtime


References:
-----------
- https://sh-tsang.medium.com/docker-tutorial-5-nvidia-docker-2-0-installation-in-ubuntu-18-04-cb80f17cac65
- https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)
- `Create Docs with RST <https://sublime-and-sphinx-guide.readthedocs.io/en/latest/index.html>`__


    
