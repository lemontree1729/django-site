# How to set up server

## 0. Stacks

Linux(Ver: AWS VM Ubuntu 20.04.2 LTS, UsedTo: Server computer)  
Windows(Ver: 10 Home, UsedTo: Local computer)  
MobaXterm(Ver: Personal Edition v21.4, UsedTo: Connect server and local via SSH and SFTP)  
Python(Ver: 3.10.0, UsedTo: Used django)  

## 1. Make a virtual machine

* [Create a new aws account](https://aws.amazon.com/ko/free/)
* *(optional)* [Make admin account with IAM](https://console.aws.amazon.com/iam/home) ([more details on here](https://extsdd.tistory.com/76?category=853192))
* [Make EC2 instance with key pairing](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:)
* *(optional)* [Make elastic ip adress](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#Addresses:)

## 2. Connect your computer(local) with VM(server)

* [Download MobaXterm](https://mobaxterm.mobatek.net/download.html)
* Make SSH session and add ip address and private key with pem extension
* Type `ubuntu`(default user name for AWS VM) to enter VM(Type `exit` to exit)

## 3. Set your server

* type `sudo passwd root` to set root password
* *(optional)* type `sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime` to change server time as KST
* type `sudo apt-get update` and `sudo apt-get upgrade` to update and upgrade modules
* type `python` or `python3` to check python is installed already
* type `sudo apt-get install python3-pip` to install pip
* type `sudo apt install python3-venv` to install venv

## 4. Make python virtual environment with django

### 1) Initialize (once only)

* install [Python](https://www.python.org/downloads/)
* type `cd $project-path$` on PowerShell to move to the path
* type `mkdir $project-name$` and `cd $project-name$` to prepare folder
* type `py -3.10 -m venv $virt-name$` on PowerShell
* type `$virt-name$\Scripts\Activate`(for only PowerShell) on PowerShell to activate virtual environment(type `deactivate` to deactivate) ([more details on here](https://docs.python.org/ko/3.7/library/venv.html))
  * if you have an Security error, you should type `Set-ExecutionPolicy RemoteSigned -force` on PowerShell **run as administrator** ([more details on here](https://dreamlog.tistory.com/603))
* if you could see the commandlines like `($virt-name$) $project-path$\$project-name$`, it works!
* (from now on, you should install used modules every time you entered virtual environment!)
* type `py -3.10 -m pip install --upgrade pip` on PowerShell to upgrade pip
* type `py -3.10 -m pip install django` on PowerShell to install django
* type `django-admin startproject config .` on PowerShell to make a config
* type `py -3.10 manage.py startapp $app-name$` on PowerShell to make new app
* type `pip freeze > requirements.txt` to collect package you used

### 2) After initialize

* add `$project-path$\$virt-name$` on environment variable `Path`
* make openvenv.ps1 and closevenv.ps1 file in `$virt-name$`
* add below text on openvenv.ps1

```
cd $project-path$\$project-name$
$virt-name$\Scripts\Activate
py -3.10 -m pip install --upgrade pip
py -3.10 -m pip install -r requirements.txt
```

* add below text on closevenv.ps1

```
deactivate
cd $project-path$\$project-name$
pip freeze > requirements.txt
```

* now we could open and close virtual environment with just typing `openvenv` and `closevenv`!

### 3) File structure

```
  $project-name$
   ├──$virt-name$
   │   ├──openvenv.ps1
   │   └──closevenv.ps1
   ├──config
   │   ├──setting.py
   │   └──...etc
   ├──myapp
   │   └──...etc
   ├──requirement.txt
   └──manage.py
```

### 4) Reference  

> <https://wikidocs.net/70588>  
> <https://nerogarret.tistory.com/45?category=800142>
