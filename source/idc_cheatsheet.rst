
IDC Cheat Sheet 
================

IDC or Intel® Tiber™ Developer Cloud is Intel's production cloud for AI applications. During hackathons, participants can access IDC in various ways as outlined below. 🚀

.. image:: ./images/idc.png
   :alt: Intel® Tiber™ Developer Cloud

General Information 📜
----------------------

Participants have access to two primary systems on the Intel® Tiber™ Developer Cloud (IDC):

- **For Development (Jupyter Environment):** 👨‍💻
  
  - Equipped with an **Intel® Data Center GPU Max Series 1100 GPU** `GPU <https://www.intel.com/content/www/us/en/products/details/discrete-gpus/data-center-gpu/max-series.html>`_ (48 GB VRAM), Xeon CPU, and 30 GB disk space.
  - This setup is ideal for AI development and fine-tuning models but cannot be exposed with a public IP.
  - Utilize the ``Pytorch GPU`` environment available in Jupyter Notebook for GPU-supported PyTorch operations. Similar environments are available for TensorFlow.

- **For Deployment (CPU VMs):** 🚀

  - Participants need to request cloud credits to launch a CPU VM and should consult any Intel resource at the hackathon for assistance. Below is an image showing where to add credits to your account:
  
  - You can select the desired VM from the hardware catalog:
  
    - **Medium CPU VMs:** 8 cores, 16 GB RAM, 20 GB Disk.
    - **Large CPU VMs:** 16 cores, 32 GB RAM, 32 GB Disk.
    - These are suitable for deploying applications that require a public IP. Tools like ngrok or reverse tunneling are recommended for public exposure or showcasing the app on your laptop.

Accessing IDC 🌍
----------------

To access IDC, the first step is to register for a **standard account** at the `Intel® Tiber™ Developer Cloud <https://cloud.intel.com/hackdavis>`_.

Developing AI Apps Using the Jupyter Environment 📓
---------------------------------------------------

After creating a standard account, you can launch a Jupyter Notebook directly from the homepage. Each Jupyter notebook has access to an Intel Data Centre GPU with 48 GB of VRAM. Explore any of the example `Gen AI Essentials notebooks` (available on your IDC home page) to learn how to utilize Intel GPUs effectively.

For instance, if you are using PyTorch, select the `Pytorch GPU` Jupyter kernel after launching your notebook. Below is a sample code snippet to verify if GPUs are available in your environment:

.. code-block:: python

   import torch
   import intel_extension_for_pytorch as ipex  # this adds a new namespace `xpu` to torch

   print(f"Number of GPU (XPU) devices: {torch.xpu.is_available()}")
   print(f"GPU is: {torch.xpu.get_device_name()}")


Deploying Your Solution 🚀 🖥️
-------------------------------

You can launch a **Small or Large CPU VM** by going to the hardware catalog section of IDC and use `SSH` to connect to the machine. Once you have a working prototype and are ready to deploy it for others to use, come talk to us. We can provide you with cloud credits to launch a dedicated **CPU VM** to host your deployed app.

**Configure SSH for VM Access** 🔐
----------------------------------

To connect to the VM, you need to generate an SSH key, here is a GitHub doc on generating `SSH keys<https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>_` .

- Once you launch a VM from the `Hardware Catalog`, navigate to the `Compute Instances` tab and click on your instance name. Next to your instance name, there is a "How to Connect" button; click on it to see detailed connection instructions based on the OS you have.
- The guest IP address (``146.152.X.X``) provided in the “How to Connect” tab will vary; use the specific IP given for your session.
- Navigate to your ``~/.ssh`` folder and create a config file::

    Host 146.152.*.*
    IdentityFile "~/.ssh/id_rsa"

- Replace ``146.152.*.*`` with the actual guest IP address you receive.
- ``id_rsa`` is your private key. Adding a passphrase is optional.

**Connect via SSH** ⌨️
-----------------------

Command format::

    ssh -i id_rsa -J guest@GUEST_IP ubuntu@VM_IP

Replace ``GUEST_IP`` and ``VM_IP`` with the actual IP addresses.


Public Exposure via ngrok 🌐
-----------------------------

Use **ngrok** to expose your VM's server to the internet when a public URL is needed to share your application:

.. code-block:: bash

   snap install ngrok
   ngrok http 8080

Example of Exposing a Simple HTTP Server Using ngrok 🖥️➡️🌐
-----------------------------------------------------------

1. Start a simple HTTP server on your VM (ensure you have Python installed):

.. code-block:: bash

   python3 -m http.server 8080

2. Open a new terminal and ssh into the machine, then run ngrok to expose the HTTP server:

.. code-block:: bash

   ngrok http 8080

Local Port Forwarding Example 🔄
---------------------------------

To expose a web application running on your VM through your local machine, assuming the username of the VM is `ubuntu`:

.. code-block:: bash

   ssh -i id_rsa -L 8080:localhost:8080 guest@GUEST_IP ubuntu@VM_IP

Other Useful Commands After Logging In to VMs 🛠️
-------------------------------------------------

Update the repositories and install Python:

.. code-block:: bash

   sudo apt update
   sudo apt-get install python3.10
   sudo apt-get install pip

Local Port Forwarding for JupyterLab (Applicable for CPU VMs) 🔄
-----------------------------------------------------------------

Use local port forwarding to securely access the JupyterLab session from your VM on your local machine:

.. code-block:: bash

   ssh -i id_rsa -J guest@GUEST_IP -L 8888:VM_IP:8888 ubuntu@VM_IP

Start JupyterLab:

.. code-block:: bash

   pip install notebook
   jupyter notebook --ip 0.0.0.0 --port 8888 --allow-root

Access at localhost:8888 using the provided token.

Uploading and Managing Files to VMs 📤📥
-----------------------------------------

Assuming the username for the VM is `ubuntu`:

To upload files to the remote VM:

.. code-block:: bash

   scp -i id_rsa -o ProxyJump:guest@GUEST_IP FILE ubuntu@VM_IP:/home/ubuntu/

To download files from the remote VM:

.. code-block:: bash

   scp -i id_rsa -o ProxyJump:guest@GUEST_IP ubuntu@VM_IP:/path/to/remote/file /local/destination

Choosing the Right Platform 🧭
-------------------------------

- Development: Use the Jupyter environment for quick AI development, model fine-tuning, or modifying existing LLM notebooks.
- Deployment: Utilize CPU VMs for deploying applications with frontend components, using local port forwarding or ngrok for external access.

Connecting VSCode to IDC Environments 🖥️🔗
-------------------------------------------

You can enhance your development experience by connecting Visual Studio Code (VSCode) to either the Jupyter environment or a CPU VM using Microsoft's VSCode Remote Tunnels extension. This allows you to develop directly on the IDC resources using VSCode's interface.

**Follow the detailed guide here:**
`VSCode Remote Development on IDC <https://console.cloud.intel.com/docs/tutorials/vs_code.html>`_
