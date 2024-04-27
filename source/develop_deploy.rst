

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

Configure SSH for VM Access 🔐
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To connect to the VM, you need to generate an SSH key, here is a GitHub doc on generating `SSH keys<https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>_` .

- Once you launch a VM from the `Hardware Catalog`, navigate to the `Compute Instances` tab and click on your instance name. Next to your instance name, there is a "How to Connect" button; click on it to see detailed connection instructions based on the OS you have.
- The guest IP address (``146.152.X.X``) provided in the “How to Connect” tab will vary; use the specific IP given for your session.
- Navigate to your ``~/.ssh`` folder and create a config file::

    Host 146.152.*.*
    IdentityFile "~/.ssh/id_rsa"

- Replace ``146.152.*.*`` with the actual guest IP address you receive.
- ``id_rsa`` is your private key. Adding a passphrase is optional.

Connect via SSH ⌨️
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command format::

    ssh -i id_rsa -J guest@GUEST_IP ubuntu@VM_IP

Replace ``GUEST_IP`` and ``VM_IP`` with the actual IP addresses.


Exposing your Application via ngrok 🌐
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use **ngrok** to expose your VM's server to the internet when a public URL is needed to share your application:

.. code-block:: bash

   snap install ngrok
   ngrok http 8080

Example of Exposing a Simple HTTP Server Using ngrok 🖥️➡️🌐
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

1. Start a simple HTTP server on your VM (ensure you have Python installed):

.. code-block:: bash

   python3 -m http.server 8080

2. Open a new terminal and ssh into the machine, then run ngrok to expose the HTTP server:

.. code-block:: bash

   ngrok http 8080

Local Port Forwarding Example 🔄
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To expose a web application running on your VM through your local machine, assuming the username of the VM is `ubuntu`:

.. code-block:: bash

   ssh -i id_rsa -L 8080:localhost:8080 guest@GUEST_IP ubuntu@VM_IP

Choosing the Right Platform 🧭
---------------------------------------------------

- **Development** : Use the Jupyter environment for quick AI development, model fine-tuning, or modifying existing LLM notebooks.
- **Deployment** : Utilize CPU VMs for deploying applications with frontend components, using local port forwarding or ngrok for external access.
