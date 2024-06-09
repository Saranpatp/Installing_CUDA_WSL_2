# Installing CUDA on WSL 2

A detailed walkthrough for installing CUDA on WSL 2 for use with TensorFlow and GPU-accelerated applications.

### Requirements for Installing CUDA on WSL 2

* WSL 2 (Ubuntu for this demonstration)
* NVIDIA GPU
* Latest NVIDIA Drivers ([Download here](https://www.nvidia.com/Download/index.aspx#))

## Instructions

1. **Open Windows PowerShell**

2. **Access the Linux Subsystem (Ubuntu)**
    ```sh
    wsl
    ```

3. **Install Miniconda in the Linux Subsystem**

    We need to install Anaconda in our Linux subsystem environment to create virtual environments and execute Python files. Install the mini-version of Anaconda using the following commands:

    ```sh
    curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh
    ```

4. **Create a Conda Virtual Environment**

    After installing Miniconda, create a virtual environment in the Linux subsystem:

    ```sh
    conda create --name tf python=3.11
    ```

    *Note: You can change the Python version to the latest version that supports pandas 2.0.*

5. **Activate the Virtual Environment**

    ```sh
    conda activate tf
    ```

6. **Install CUDA and cuDNN**

    ```sh
    conda install -c conda-forge cudatoolkit=12.1.0
    pip install nvidia-cudnn-cu11==8.9.0.131
    ```

7. **Configure the Path**

    Configure the path for cuDNN:

    ```sh
    CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))
    export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:$LD_LIBRARY_PATH
    ```

    To make this configuration persistent every time you activate the environment, use the following commands:

    ```sh
    mkdir -p $CONDA_PREFIX/etc/conda/activate.d
    echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
    echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
    ```

8. **Install PyTorch**

    Install PyTorch according to your CUDA version and OS by running:

    ```sh
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
    ```

    For more information and options, visit the [PyTorch installation guide](https://pytorch.org/get-started/locally/).

By following these steps, you will have CUDA installed on WSL 2 and be ready to run TensorFlow and GPU-accelerated applications.