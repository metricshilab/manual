## Access GPU of SCRP Cluster in VS Code Jupyter Notebook

Currently, if one wants to use GPU of SCRP cluster, the typical way is to use the Slurm server through broswer and apply for a computing node with GPU. However, typing code in the browser is not as convenient as using VS Code. This note will introduce how to use GPU in VS Code Jupyter Notebook.

### Step 1: Launch a Jupyter server on computing server

First, we need to access the SCRP cluster through SSH: (replace `username` with your own username)

```bash
ssh -Y username@scrp-login.econ.cuhk.edu.hk
```

Then, we launch a Jupyter server on the computing server by typing the following command in the terminal:

```bash
compute --gpu-per-tasks=rtx3090 private-jupyter
```

This line will launch a Jupyter server on a computing node with RTX3090 GPU. If you want to use other GPUs, you can use other options. Please refer to the [documentation](https://scrp.econ.cuhk.edu.hk/guide/slurm#compute) for more details. This command will return some information. In the first line, you will see the name of the computing node, which is something like `scrp-node-9`. Remember this name, as we will use it in the next step. In the bottom, you will see a URL like `http://localhost:xxxx/?token=...`, which is the URL of the Jupyter server, and we also need it in the next steps.

### Step 2: Forward the port to your local machine

To access the Jupyter server on your local machine, you need to forward the port. Open a new terminal and type the following command:

```bash
ssh -t -L xxxx:localhost:xxxx username@scrp-login.econ.cuhk.edu.hk ssh -L xxxx:localhost:xxxx compute-node-name
```

where `xxxx` is the port number in the URL, `username` is your username, and `compute-node-name` is the name of the computing node. For example, if the URL is `http://localhost:8888/?token=...` and the name of the computing node is `scrp-node-9`, you should type:

```bash
ssh -t -L 8888:localhost:8888 username@scrp-login.econ.cuhk.edu.hk ssh -L 8888:localhost:8888 scrp-node-9
```

This will forward the port to your local machine so that your can access the Jupyter server running on the computing node from your local machine.

### Step 3: Access the Jupyter server in VS Code

Now open your Jupyter notebook file in VS Code. On the right upper corner, you will see `Select Kernel`. Click it and choose `Select Another Kernel` > `Exsting Jupyter Server`. Then, type the URL `http://localhost:8888/?token=...` from Step 1 in the input box. This means that your Jupyter notebook in VS Code will connect to the Jupyter server running on the computing node. After that, you can choose a kernel of the Jypyter server. For example, you can choose `Python [env:pytorch-2.2]`. Please be reminded not to choose the Anaconda kernel, as it cannot access the GPU correctly. To see wheter the GPU is available, you can type the following code in a cell:

```python
import torch
torch.cuda.is_available()
```

If the output is `True`, then you have successfully accessed the GPU.
