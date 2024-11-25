# Running AWX on Docker

**REQUIRED OS:**
- Ubuntu 22
- 2 core = atlease
- 4 ram = atlease

## 1 - Update the system:
```
sudo apt update -y
sudo apt upgrade -y
```

## 2 - Install the ansible and the packages required to install ansible
- **A - Install Python tools:**
```
sudo apt install python-setuptools -y
Sudo apt install python3-setuptools -y
```

**B - Install Python and Pip:**
```
sudo apt install python3-pip -y
```

**C - Create Python env:**
```
sudo apt install python3.12-venv
```

ls /home/ubuntu    = to check if you will see myenv then continue the following cmds.
```
python3 -m venv myenv
source /myenv/bin/activate
```

 **D - Install Ansible:**
```
sudo pip3 install ansible
ansible --version
```

## 3 - Install docker: Refer to Docker page for proper installation:
```
sudo apt install docker docker.io -y
docker version
sudo pip3 install docker-compose 
docker-compose version
```

## 4 - If you are running from other user, I tried with ROOT, did not ran this:

```
sudo usermod -aG docker $USER
```

## 5 - Install the required package like pwgen, vim and git to install and setup awx
```
sudo apt install git vim pwgen -y
```

## 6 - Clone the ansible awx repo (it should be less then version 17 to install it using docker compose)
```
git clone https://github.com/ansible/awx.git --branch 17.0.1 --depth 1
```

## 7 - Change the directory to awx/installer 
- (you will not see this folder in new version, so clone the old version i.e 17)
```
cd awx/installer
```

- **Generate a secret key (will be used later)**
```
$ apt install pwgen
$ pwgen -N 1 -s 30
```

- Exp: UB3kzjNXfLwPNXTAcwuG2LSYqYCPqu
 
## 8 - Now we need to modify the inventory file with a text editor(nano): 
- Replacing admin_password and secret_key. And make sure it's uncommented.
- Remember admin_password as it is needed to login in AWX web interface later. 
```
$ sudo nano inventory
```

- Modify the admin_password and secret_key and also username if you want. 
- Example:
- admin_password=admin
- secret_key=UB3kzjNXfLwPNXTAcwuG2LSYqYCPqu

## 9 - After that implement yaml playbook:
-  It will  downloads docker container images and sets them up accordingly. 
- Run the following command to apply ansible playbook.
- Make sure you are in this directory awx/installer running the below command.
```
$ ansible-playbook -i inventory install.yml
```

- If you want to see the containers that is running then run:
```
sudo docker ps
```

- You can go to your browser and type http://your-server-ip to access AWX GUI. 
- It will run on port 80
- Sign in by using the admin_user, admin_password in the UI
