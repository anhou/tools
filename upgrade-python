# Upgrade Python 3.10 on Ubuntu 20.04
* it's simmilar for other Ubuntu OS version and pytho version.
* Ref: ###### https://gist.github.com/rutcreate/c0041e842f858ceb455b748809763ddb

## Upgrade 3.10(Default 3.8)

```
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.10 python3.10-venv python3.10-dev
python3 --version
```
please don't change OS's default python version 3.8 to 3.10 as below. maybe it will case `apt update` fail.

```
ls -la /usr/bin/python3
rm /usr/bin/python3
ln -s python3.10 /usr/bin/python3
ln -s python3.10-config /usr/bin/python3-config
ln -s python3 /usr/bin/python
```
## Create Virtual environment with virutalenv
* After install 3.10(don't need set it to be default)
```
# after below commands, myenv's python, python3, pip, pip3 will all be for 3.10
python3.10 -m virtualenv myenv
source myenv/bin/activate
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10

# NOTE: Don't need below commands
#python3.10 -m pip --version
#python3.10 -m pip install --upgrade pip
#pip install virtualenv
#sudo apt install virtualenv
```
