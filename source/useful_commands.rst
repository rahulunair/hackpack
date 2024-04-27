
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


Connecting VSCode to IDC Environments 🖥️🔗
-------------------------------------------

You can enhance your development experience by connecting Visual Studio Code (VSCode) to either the Jupyter environment or a CPU VM using Microsoft's VSCode Remote Tunnels extension. This allows you to develop directly on the IDC resources using VSCode's interface.

**Follow the detailed guide here:**
`VSCode Remote Development on IDC <https://console.cloud.intel.com/docs/tutorials/vs_code.html>`_
