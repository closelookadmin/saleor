### Server Setup

# install packages
sudo yum update
sudo yum install gcc gcc-c++ make openssl-devel bzip2-devel libffi-devel git mlocate perl-devel redhat-rpm-config


# Install Python 3
cd /opt
sudo wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
sudo tar xzf Python-3.7.4.tgz
#make altinstall is used to prevent replacing the default python binary file /usr/bin/python.
cd Python-3.7.4
sudo ./configure --enable-optimizations
sudo make altinstall
sudo rm /usr/bin/python
sudo rm /usr/bin/pip
sudo ln -s /usr/local/bin/python3.7 /usr/bin/python
sudo cp /usr/local/bin/pip3.7 /usr/bin/pip
sudo yum install python-devel python-pip python-setuptools python-wheel python3-devel mysql-devel -y


# Docker
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user

sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose


# Install nodeJS 10
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
sudo yum install -y nodejs


# Clone the repo
git clone https://github.com/closelookadmin/saleor.git
cd saleor


# Build Docker image
sudo docker-compose build
sudo docker-compose run web python3 manage.py migrate
sudo docker-compose run web python3 manage.py collectstatic
# If you need example data and default admin user
- sudo docker-compose run web python3 manage.py populatedb --createsuperuser
sudo docker-compose up



# Node modules 
cd into saleor
npm i
npm run-script build -assets


# to rebuild and restart docker containers
sudo docker-compose stop
sudo docker-compose build
sudo docker-compose up


# Old Steps, we dont need for Docker
# Local Postgres
yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-6-x86_64/pgdg-redhat-repo-latest.noarch.rpm
yum install postgresql96
yum install postgresql96-server
yum install postgresql96-contrib

su - postgres
createdb saleor
#CREATE DATABASE saleor
createuser -P -s -e saleor
#CREATE ROLE saleor PASSWORD 'md52e0873c6883e46aeab1904c374580dc9' SUPERUSER CREATEDB CREATEROLE INHERIT LOGIN;
modify the pg_hba.conf in /var/lib/pgsql96/data/pg_hba.conf to be trust for ipv4 connections


git clone https://github.com/closelookadmin/saleor.git
cd saleor/
pythoon -m venv venv
python -m pip install -r requirments.txt
pip install django-debug-toolbar