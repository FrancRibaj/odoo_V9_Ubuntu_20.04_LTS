## odoo_V9_Ubuntu_20.04_LTS
## setting up framework

##From Requirements:
##-CentOS 7 (64 Bit)
##-Python 2.7.5
##-Postgres 9.2.18
##-Odoo 9

##To Requirements:
##-Ubuntu 20.04 LTS
##-PYTHON 2.7.18
##-Postgres 9.2.18
##-Odoo 9jo

##STEP-1.Installation of Postgres 9.2.18
Download and install:

##Note remove old version of POSTGRES unless important database stored,if not continue to the next step
sudo apt-get --purge remove postgresql postgresql-*

wget https://ftp.postgresql.org/pub/source/v9.2.18/postgresql-9.2.18.tar.gz
tar xvfz postgresql-9.2.18.tar.gz
cd postgresql-9.2.18

##dependancy

sudo apt libreadline-dev

./configure
sudo su -
make install
adduser odoo
mkdir /usr/local/pgsql/data
chown odoo /usr/local/pgsql/data
sudo su - odoo
/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
/usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
/usr/local/pgsql/bin/createdb test
/usr/local/pgsql/bin/psql test

##STEP-2.Open and modify "postgresql.conf":

vim /usr/local/pgsql/data/postgresql.conf
#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------
# - Connection Settings -
listen_addresses = '*'     # what IP address(es) to listen on;
port = 5432


nano /usr/local/pgsql/data/pg_hba.conf
##or
vim /usr/local/pgsql/data/pg_hba.conf

# TYPE  DATABASE        USER            ADDRESS                 METHOD
# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
# Allow replication connections from localhost, by a user with the
# replication privilege.
local   replication     odoo                                trust
host    replication     odoo        127.0.0.1/32            trust
host    replication     odoo        ::1/128                 trust
host    all             all         0.0.0.0/0               md5

##STEP-3.Edit ".bash_profile" for odoo user and export psql path:
sudo su odoo
vim ~/.bash_profile
##or 
nano ~/.bash_profile
# Below exports are added by developer after installation:
export PATH="/usr/local/pgsql/bin/:$PATH"
export PGHOME=/usr/local/pgsql
export PGDATA=/usr/local/pgsql/data
export PGHOST=localhost

Edit ".bash_profile" for postgres user and export psql path:
sudo su postgres
touch ~/.bash_profile
vim ~/.bash_profile
##or 
nano ~/.bash_profile

# Below exports are added by developer after installation:
export PATH="/usr/local/pgsql/bin/:$PATH"

export PGHOME=/usr/local/pgsql
export PGDATA=/usr/local/pgsql/data
export PGHOST=localhost

##Restart the PostgreSQL service:
source ~/.bash_profile
sudo /usr/local/pgsql/bin/pg_ctl restart 
sudo passwd postgres
echo $PG_DATA
ls -l /usr/local/pgsql/data
sudo chown postgres /usr/local/pgsql/data
nano ~/.bash_profile

##Installation of Odoo 9.0
##Download and install complete odoo repository:
cd
git clone https://github.com/odoo/odoo.git

## Checkout to branch *9.0:
cd odoo
git checkout 9.0

## Install required 3d-party python packages:
sudo apt install python3-dev libxml2-dev libxslt1-dev libldap2-dev libtiff-dev libjpeg-dev libzip-dev libfreetype-dev liblcms2-dev libwebp-dev tcl-dev tk-dev python3-pip wkhtmltopdf

'''
yum install python-pip
pip install setuptools==1.4.1
pip install beautifulsoup4==4.9.3
pip install pillow
'''

python2 -m pip
apt search python-pip
python2 -m ensurepip
sudo apt install curl
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
nano get-pip.py
sudo python2 get-pip.py
pip2 --version
sudo apt install python2.7-dev
sudo apt install g++
sudo apt install gcc
uname -a
sudo apt install libsasl2-dev
pip2 install --update setuptools
pip2 install setuptools
pip2 install setuptools --upgrade
sudo apt install nodejs
nano ~/.bash_profile
pip2 install pyscopg --upgrade
pip2 install pyscopg2 --upgrade
pip2 install psycopg2 --upgrade

```
 
```
Wkhtmltopdf  - is also required, add it to requirements above
```
 
### Install requirements:
```
cd
cd odoo
pip install -r requirements.txt
```
 
### Install NodeJs and "less":
```
sudo yum install nodejs
npm install -g less
```
 
 
 
## Running Odoo
 
 
### Open access to port-8069:
 
```
firewall-cmd --get-active-zones
```
It will say either public, dmz, or something else. You should only apply to the zones required. In the case of 'public' try:
 
```
firewall-cmd --zone=public --add-port=8069/tcp --permanent
firewall-cmd --reload
```
 
Otherwise, substitute public for your zone, for example, if your zone is 'dmz':
```
firewall-cmd --zone=dmz --add-port=8069/tcp --permanent
```
 
 
### Edit .bash_profile:
 
Add at the end this line:
```
export PGHOST=localhost
```
 
### Run odoo.py
```
cd odoo
python odoo.py -w odoo -r odoo
```
 
Stop the running odoo instance before continuing with adding of new modules.
 
## Add custom modules:
```
cd odoo
cd addons
git clone `~git repo clone link~`
```
 
## Install tring module requirements:
```
pip install openpyxl==2.6.4
pip install luhn
yum install gmp-devel
pip install paramiko==1.7.7.2
pip install python2-secrets
```
 
These packages were missing in runtime:
```
pip install cffi
pip install pysftp==0.2.8
```
 
Note that the path of 'addons' above might be differen on your installation, usually it is inside python library, in our case it is directly inside odoo folder. After cloning the module repository should login to odoo as administrator and activate developer mode to then update apps list and new module will show on the list. Then can be proceeded with the installation and using of the module.
 
## ERRORS
 
```
Depends that might be needed:
 
-Technical Name: marketing
-Technical Name: mass_mailing
```
 
 
```
File "/root/odoo/openerp/tools/convert.py", line 907, in convert_csv_import
    raise Exception(_('Module loading %s failed: file %s could not be processed:\n %s') % (module, fname, warning_msg))
Exception: Module loading tring failed:
 
file tring/security/ir.model.access.csv could not be processed:
Line 158 : No matching record found for external id 'model_tring_mass_mail' in field 'Object'
 
Possible solutions:
https://www.odoo.com/it_IT/forum/help-1/why-does-this-security-rule-not-work-no-matching-record-found-for-external-id-model-project-project-97522
https://github.com/Yenthe666/auto_backup/commit/faa3e75d13244766a992f81abdaf73457e403bfa
```
