
Step 1: Installing Remi Repository
-------------------------------------------

    sudo rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    sudo rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

Step 2: Installing Node.js / NPM / Grunt / Bower
-------------------------------------------

    sudo yum -y --enablerepo=remi,remi-test install gcc-c++ openssl-devel git
    sudo cd /usr/local/src
    sudo wget http://nodejs.org/dist/node-latest.tar.gz
    sudo tar zxvf node-latest.tar.gz
    cd node-v* #tab
    sudo ./configure
    sudo make
    sudo make install

#### Create symlinks

    sudo ln -s /usr/local/bin/node /usr/bin/node
    sudo ln -s /usr/local/lib/node /usr/lib/node
    sudo ln -s /usr/local/bin/npm /usr/bin/npm
    sudo ln -s /usr/local/bin/node-waf /usr/bin/node-waf

#### Check node and npm version

    node --version
    npm --version

#### Installing grunt-cli and bower

    sudo npm install -g bower
    sudo npm install -g grunt-cli

Step 3 - Configurations for RHEL SELinux properties
-------------------------------------------
#### 1.

    cd /home/ec2-user/
    sudo git clone https://github.com/appirio-tech/arena-web
    cd /arena-web
    sudo chcon -R -t httpd_sys_content_t /home/ec2-user/arena-web

#### 2.
By default the RedHat firewall blocks ports such as port 80. This is in addition to the Firewall configured on Amazon AWS through the Security Group. From the menu, you can customize the firewall to allow your HTTP traffic.

    sudo system-config-firewall-tui

Console window will open - Disable firewall
If you are able to scroll on the console window - scroll down and enable WWW (HTTP)

#### 3.
Disable SELinux - Use the getenforce or sestatus commands to check the status of SELinux. The getenforce command returns Enforcing, Permissive, or Disabled. We need to set the state to "Permissive" and we do this by:

    sudo getenforce # to see current status
    sudo setenforce 0 # to set to permissive

Step 4: Finding out what is your public hostname on ec2
-------------------------------------------
To get your public hostname use:

    curl http://169.254.169.254/latest/meta-data/public-hostname

Response for my case: ec2-54-148-88-233.us-west-2.compute.amazonaws.com
We access our php files on apache using http://ec2-54-148-88-233.us-west-2.compute.amazonaws.com:3000

Step 5: Installing Grunt-cli, Bower
-------------------------------------------

    sudo npm install -g bower
    sudo npm install -g grunt-cli

    sudo git clone https://github.com/appirio-tech/arena-web
    cd /arena-web
    sudo npm install
    sudo source config/dev-local.sh
    sudo grunt

    sudo grunt release
    sudo grunt jslint
    sudo npm start

