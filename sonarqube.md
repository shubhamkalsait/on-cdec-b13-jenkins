# Prerequesists for installation
yum install epel-release unzip vim -y
yum install wget -y
wget https://download.bell-sw.com/java/11.0.4/bellsoft-jdk11.0.4-linux-amd64.rpm
yum install  java -y
rpm -ivh bellsoft-jdk11.0.4-linux-amd64.rpm

# Mysql Configuration

'''shell
rpm -ivh http://repo.mysql.com/mysql57-community-release-el7.rpm
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
yum install mysql-server -y
echo 'vm.max_map_count=262144' >/etc/sysctl.conf
sysctl -p
echo '* - nofile 80000' >>/etc/security/limits.conf
sed -i -e '/query_cache_size/ d' -e '$ a query_cache_size = 15M' /etc/my.cnf
systemctl start mysqld
grep 'password' /var/log/mysqld.log
mysql_secure_installation
mysql -p -u root
create database sonarqube;
create user 'sonarqube'@'localhost' identified by 'Redhat@123';
grant all privileges on sonarqube.* to 'sonarqube'@'localhost';
flush privileges;
'''

# Installation of sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.9.1.zip
unzip sonarqube-7.9.1.zip
mv sonarqube-7.9.1 /opt/sonarqube
cd /opt/
mv sonarqube sonar
sed -i -e '/^sonar.jdbc.username/ d' -e '/^sonar.jdbc.password/ d' -e '/^sonar.jdbc.url/ d' -e '/^sonar.web.host/ d' -e '/^sonar.web.port/ d' /opt/sonar/conf/sonar.properties
sed -i -e '/#sonar.jdbc.username/ a sonar.jdbc.username=sonarqube' -e '/#sonar.jdbc.password/ a sonar.jdbc.password=Cloudblitz@123' -e '/InnoDB/ a sonar.jdbc.url=jdbc.mysql://localhost:3306/sonarqube?useUnicode=true&characterEncoding=utf&rewriteBatchedStatements=true&useConfigs=maxPerformance' -e '/#sonar.web.host/ a sonar.web.host=0.0.0.0' /opt/sonar/conf/sonar.properties
useradd sonar
chown sonar:sonar /opt/sonar/ -R
sed -i -e '/^#RUN_AS_USER/ c RUN_AS_USER=sonar' /opt/sonar/bin/linux-x86-64/sonar.sh
vim /opt/sonar/bin/linux-x86-64/sonar.sh
vim /opt/sonar/conf/wrapper.conf
alternatives --config java
vim /opt/sonar/conf/wrapper.conf

# to start sonarqube
/opt/sonar/bin/linux-x86-64/sonar.sh start

# to show status
/opt/sonar/bin/linux-x86-64/sonar.sh status

# to get logs
less logs/sonar.log



## New Sonarqube

### Prerequisites Istallatio.
1. Install Java 11
- apt update -y
- apt install openjdk-11-jdk -y

2. Install Postgresql
- apt install postgresql -y
- systemctl start postgresql
- systemctl enable postgresql

3. Cofigure Linux
- sysctl -w vm.max_map_count=524288
- sysctl -w fs.file-max=131072
- ulimit -n 131072
- ulimit -u 8192

### Deploy Schema
- sudo -u postgres psql template1
    * CREATE USER sonarqube;
    * ALTER USER sonarqube with encrypted password 'Redhat123';
    * CREATE DATABASE sonarqube;
    * GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonarqube;
    * GRANT ALL ON SCHEMA public TO sonarqube;

### Install Sonarqube server
- wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.8.100196.zip
- apt install unzip -y
- unzip sonarqube-9.9.8.100196.zip
- mv sonarqube-9.9.8.100196 /opt/sonarqube
- vi conf/sonar.properties
    sonar.jdbc.username=sonarqube
    sonar.jdbc.password=mypassword
    sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
- apt install openjdk-17-jdk
- useradd sonar
- chown -R sonar:sonar /opt/sonarqube
- su sonar
- bin/linux/sonar.sh start
