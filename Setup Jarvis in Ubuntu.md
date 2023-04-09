# Some commands to setup HuggingGPT [Jarvis] on a clean Ubuntu install 
Last Updated: April 7, 2023

This not a script, just a commented version of the history of my terminal in Ubuntu 22. Use at your own risk. 
Read the comments before running any commands.
Check with Bing or GPT if you are unsure or unfamiliar with anything. 
If you have suggestions for improvements, please let me know by creating an Issue.

## The following section is only needed if you need to expand your disk partition. 

Usually needed only in a Hyper-v quick create scenario.

`df -h`		displays information about the available disk space on the file system, including the file system name, total size, used space, available space, and utilization percentage, in gigabytes (GB).

`echo 1 | sudo tee /sys/class/block/sda/device/rescan`	This command writes the value "1" to the rescan file in the block device directory using administrative privileges.

`sudo gdisk /dev/sda`	Starts the gdisk utility for the specified disk, allowing you to create, modify, and delete disk partitions using the GUID Partition Table (GPT) format.

`sudo apt-get install cloud-guest-utils`	Installs the "cloud-guest-utils" package, which provides utilities for managing virtual machines in cloud environments.

`echo 1 | sudo tee /sys/class/block/sda/device/rescan`	The rescan process will cause the system to detect any new or modified disks and update its device list

`sudo growpart /dev/sda 1`	Resizes the partition table of the specified disk to occupy all available space, using the growpart utility.

`sudo resize2fs /dev/sda1`	Resizes the file system on the specified partition to match the new size of the partition, using the resize2fs utility.

end of disk configs

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

`conda install -c conda-forge transformers`

`conda install -c conda-forge diffusion`

`sudo apt-get install git`

`git --version`

### Make sure you activate the appropriate virtual environment before running this command, if you have one. Most of the requirements are probaly installed already if you used the list above. In which case, you wont need to run `pip` with `requirements.txt`

`pip install -r requirements.txt` read the label above before running this command.

`#sh download.sh`  #probably wont need this

### When you run this command, Conda will create a new environment named flapip and install the latest version of PyTorch available in the default Conda channels. Once the environment is created, you can activate it using the conda activate flapip command, and then use PyTorch in that environment without affecting your other Python environments.

`conda create -n flapip install pytorch`

### This command creates a new Conda environment named flask_env and installs the flask, waitress, flask-cors, and tiktoken packages from the Conda Forge channel in that environment.

`flask_env -c conda-forge flask waitress flask-cors tiktoken`

`conda activate flask_env` When you create a new Conda environment and install packages in that environment, the environment is not active by default

### This code sets up the environment variables required to use Node Version Manager (NVM), a tool used to manage different versions of Node.js on a single machine.

Overall, this code sets up the environment required to use NVM, which is used to manage different versions of Node.js on the same machine. By running this code in your shell environment, you can then use the nvm command to install and switch between different versions of Node.js as needed.

Note: this is actually 2 commands, so you can run as 2 separate commmands if needed.

`export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"; [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"`

`nvm --version`

### create a file named environment.yaml in the current working directory and write a list of all the packages installed in the current Conda environment to that file in YAML format

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