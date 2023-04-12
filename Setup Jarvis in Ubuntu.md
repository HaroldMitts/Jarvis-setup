# Some commands to setup HuggingGPT [Jarvis] on a clean Ubuntu install 
Last Updated: April 9, 2023

This not a script, just a commented version of the history of my terminal in Ubuntu 22. Use at your own risk. 
Read the comments before running any commands.
Check with Bing or GPT if you are unsure or unfamiliar with anything. 
If you have suggestions for improvements, please let me know by creating an Issue.

Note: The requirements.txt file provided by Jarvis team has errors. This guide also has errors. I would like to be able to just run the requirements.txt method, but unfortunately it fails on pesq, asteroid, and espnet.

I am working through it to attempt to resolve and will update this guide as I solve each requirement.

## The following section is only needed if you need to expand your disk partition. 

Here's the complete sequence of commands to resize the partition and filesystem in your Ubuntu virtual machine:

1. Check current disk space and list the block devices:

```
df -h 
lsblk
```

2. Rescan the SCSI bus to recognize the new disk space:

`echo 1 | sudo tee /sys/class/block/sda/device/rescan`

3. Check the current partition table:

`sudo fdisk -l`

4. Run fdisk to modify the partition table:

`sudo fdisk /dev/sda`

Inside fdisk, follow these steps:

a. Press `p` to print the current partition table.

b. Note the starting sector of the partition you want to resize (usually `/dev/sda1`).

c. Press `d` to delete the partition.

d. Press `n` to create a new partition.

e. Choose the primary partition type.

f. Enter the partition number (usually `1`).

g. Enter the starting sector from the previous partition (noted in step b).

h. Press `Enter` to use the default value for the last sector, which will utilize the full available disk space.

i. When prompted with the warning about the existing filesystem signature, press `N` and then `Enter` to keep the existing signature.

j. Press `w` to write the changes and exit fdisk.


5. Reboot the system:

`sudo reboot`

6. After the system has rebooted, check the block devices again:

`lsblk`

7. Install cloud-guest-utils package to use growpart:

```
sudo apt-get update
sudo apt-get install cloud-guest-utils
```

8. Resize the partition (if necessary) to use the new disk space:

`sudo growpart /dev/sda 1`

This step might not be needed if the partition has already been resized to use the new disk space during the fdisk process.

9. Resize the filesystem on the partition:

`sudo resize2fs /dev/sda1`

10. Verify that the partition has been resized and is using the new disk space:

`df -h`

With these steps, you should be able to resize the partition and filesystem in your Ubuntu virtual machine to use the new disk space you've added.

### beginning of Ubuntu clean installation Jarvis setup. 

Some of this may not be needed if you are not configuring a clean install and have set these up before.

`python3` check the python install

`sudo apt update`

`sudo apt install curl`

`curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

`sudo apt install -y nodejs`

`node -v`

`npm -v`

`sudo apt update`

`sudo apt upgrade nodejs`

### Download Conda and use it to install requirements

https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html

`bash Miniconda3-py310_23.1.0-1-Linux-x86_64.sh` Run the installer from the downloads folder. Press Enter to proceed, then spacebar about 8x, then Enter to select the default installation location.

`conda update conda`

`conda config --add channels conda-forge`

`conda install -c conda-forge pydub`

`conda install -c conda-forge ffmpeg`

`conda install numpy`

`conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia`

### Make sure you have Jarvis downloaded at this point.

`conda create -n jarvis python=3.8`

`conda activate jarvis`

`pip install diffusion`

`sudo apt-get install git`

`git --version`

### Make sure you activate the appropriate virtual environment before running this command, if you have one. Most of the requirements are probaly installed already if you used the list above. In which case, you wont need to run `pip` with `requirements.txt`

`pip install -r requirements.txt` read the label above before running this command.


### This command creates a new Conda environment named flask_env and installs the flask, waitress, flask-cors, and tiktoken packages from the Conda Forge channel in that environment. 

`conda create -n flask_env`

`conda activate flask_env`

`conda create -n flask_env -c conda-forge flask waitress flask-cors tiktoken`

`sudo apt update`

`apt list --upgradeable`



### Install nodejs and npm, and verify they installed

`sudo apt update`

`sudo apt install nodejs`

`sudo apt install npm`

`node -v`

`nvm --version`

### install diffusers, transformers, and controlnet_aux

```
git clone https://github.com/huggingface/diffusers.git
cd diffusers
git checkout 8c530fc2f6a76a2aefb6b285dce6df1675092ac6
pip install .
```

```
cd ..
git clone https://github.com/huggingface/transformers.git
cd transformers
git checkout c612628045822f909020f7eb6784c79700813eda
pip install .
```

```
cd ..
git clone https://github.com/patrickvonplaten/controlnet_aux
cd controlnet_aux
git checkout 78efc716868a7f5669c288233d65b471f542ce40
pip install .

cd ..
```

### Optional: create a file named environment.yaml in the current working directory and write a list of all the packages installed in the current Conda environment to that file in YAML format

`conda list --export > environment.yaml`

recreate the environment on another machine using the `conda create --name <env> --file <file>` command.

`conda env create --name my_env --file environment.yaml` 

### Configure the config.yaml and lite.yaml 
  
Be sure to enclose your keys on double-quotes - not mentioned in official documentation

### run Jarvis

`python models_server.py --config config.yaml`
  
`python awesome_chat.py --config config.yaml --mode server`

### run Jarvis web

`npm run dev` #this opens the browser and magics will happen if you have setup everything right.