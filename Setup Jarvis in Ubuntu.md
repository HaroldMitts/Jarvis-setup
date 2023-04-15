# Some commands to setup HuggingGPT [Jarvis] on a clean WSL install 
Last Updated: April 14, 2023

This not a script, just a commented version of the history of my terminal in Ubuntu 22. Use at your own risk. 

Read the comments before running any commands.

Check with Bing or GPT if you are unsure or unfamiliar with anything. 
If you have suggestions for improvements, please let me know by creating an Issue.

Make sure disk space is available before setting up Jarvis.

```
sudo apt update -y
apt list --upgradeable
sudo apt install git -y
git --version
pip install --upgrade pip
```

```
sudo apt install --only-upgrade python3
python3 -V
```

```
sudo apt update && sudo apt upgrade -y
source ~/.bashrc
```

```
sudo apt install nodejs npm -y
# might need to use curl to install node
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
nodejs -v
npm -v
source ~/.bashrc
```

download Conda from this URL: https://docs.conda.io/en/latest/miniconda.html

```
bash Miniconda3-py310_23.1.0-1-Linux-x86_64.sh
source ~/.bashrc
cd /home/
pwd
cd ~
```

You might want to clone Jarvis manually. Run this command, from the home folder:

```
sudo git clone --depth=1 --branch=main https://github.com/microsoft/JARVIS.git && sudo chown -R $USER:$USER JARVIS/
sudo git clone https://github.com/lumaku/ctc-segmentation.git && sudo chown -R $USER:$USER ctc-segmentation
source ~/.bashrc
```
sudo apt-get install python3-pip

```
which conda
conda create -y -n jarvis python=3.8
conda activate jarvis
conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
cd JARVIS/server
source ~/.bashrc
pip install -r requirements.txt
source ~/.bashrc
```

## Download models. 

It will take a long time to download all the models, there are many gigabytes. This process truely puts the large in LLMs.

**Important** : First, make sure that `git-lfs` is installed. Don't let your pc go to sleep.

In my experience, you probably need to manually install git-lfs.

If you get errors when running download.sh, due to git-lfs missing, you need to rerun download.sh

```
sudo apt-get install git-lfs
cd models
bash download.sh 
```

Run the server

```
cd ..
python models_server.py --config config.yaml 
python awesome_chat.py --config config.yaml --mode server 
source ~/.bashrc
```

```
cd web
npm install
npm run dev
```