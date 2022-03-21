# How to deploy FastAPI on an AWS EC2 instance.
This README will direct you on how to deploy fastAPI on a AWS in less than 10 steps. We are going to use PM2 (PM2 is a nodejs daemon process manager that will help you manage and keep your application online 24/7). [Click here to read more about PM2](https://pm2.keymetrics.io/)
#
#### Prerequisites
1.  Have a AWS EC2 instance up an running. In this example i am running on an Ubuntu 20.04 LTS
#
#### 1. Connect into you EC2 instance.
Connect to the new intance you just created. There are various ways in which you can connect to the instance. Syntax for SSH into the instance is below:

bash
```
ssh -i <your-key-pair-here>.pem <public-IPv4-DNS-here>
```

#### 2. Update all system packages.
First we are going to run the command below to update all your packages.
```
sudo apt update -y
```
alternatively you can run
```
sudo apt-get update -y
```

#### 3. Install nodejs.
Run the following commands
```
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
then
```
sudo apt-get install -y nodejs
```
This will install nodejs on your machine. We will need this to install PM2 later.

#### 4. Install Python3 and NGINX.
Run the following command.
```
sudo apt-get install -y python3-pip python3-venv nginx
```

#### 5. Install fastAPI, uvicorn and gunicorn
We will first start with creating a directory where all our files will be hosted. We do this using the following command
```
mkdir <file-name>
```
then we are going to jump into the directory we just created above and we will create a virtual environment for our python packages.
```
cd <file-name>
```
```
python3 -m venv venv
```
If you check the current directory now you will see a venv directory has been added to your project.
We will then proceed to activating the virtual environment using the following command
```
. venv/bin/activate
```
We can now run
```
pip install fastapi "uvicorn[standard]" gunicorn
```
which will install fastapi, uvicorn and gunicorn in the virtual environment we are in.
To confirm that all 3 packages are installed run
```
pip freeze
```

#### 6. Install PM2.
We are now ready to install PM2. Run the following command.
```
sudo npm install -g pm2
```
To start a gunicorn deamon using PM2 run.
```
pm2 start "gunicorn -w 4 -k uvicorn.workers.UvicornWorker <api-file>:<api-class-name>" --name <process-name>
```
To check the status of your process run
```
pm2 list
```
alternatively
```
pm2 monit
```

#
[Read more about PM2 and how they work here](https://pm2.keymetrics.io/)
