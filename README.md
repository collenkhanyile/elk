# elk
#####Logstash#####
APT

Download and install the Public Signing Key:

$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

You may need to install the apt-transport-https package on Debian before proceeding:


$  sudo apt-get install apt-transport-https

Save the repository definition to /etc/apt/sources.list.d/elastic-7.x.list:

$  echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

Use the echo method described above to add the Logstash repository. Do not use add-apt-repository as it will add a deb-src entry as well, but we do not provide a source package. If you have added the deb-src entry, you will see an error like the following:

Unable to find expected entry 'main/source/Sources' in Release file (Wrong sources.list entry or malformed file)
Just delete the deb-src entry from the /etc/apt/sources.list file and the installation should work as expected.

Run sudo apt-get update and the repository is ready for use. You can install it with:

$  sudo apt-get update && sudo apt-get install logstash

See Running Logstash for details about managing Logstash as a system service.

---------------------------------------------------------------------

#YUM

Download and install the public signing key:

$  sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

Add the following in your /etc/yum.repos.d/ directory in a file with a .repo suffix, for example logstash.repo

$ sudo nano /etc/yum.repos.d/logstash.repo

[logstash-7.x]
name=Elastic repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

And your repository is ready for use. You can install it with:

$  sudo yum install logstash

#####

Add ELK repository to RHEL 8 / CentOS 8 / Amazon linux

# elasticsearch depends on JAVA, therefore we need Java to be installed.

$ sudo yum install java -y

$ sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

Add the following in your /etc/yum.repos.d/ directory in a file with a .repo suffix, for example logstash.repo

[logstash-7.x]
name=Elastic repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
And your repository is ready for use. You can install it with:

$ sudo yum install logstash || elasticsearch || kibana

Dont forget to enable and start the services.

###logstash commands###

EKL folders are located here>><>
$ cd /usr/share/
$ cd bin

#logstash configs are here!
cd /etc/logstash/
cd conf.d/

#pipeline commands example<<>>
1. 
--------------
$ cd /usr/share/logstash/bin/
./logstash -e 'input {stdin {}} output{stdout{}}'
--------------

2. 
##create sample file for input and output

#file location 
$ cd /etc/logstash/conf.d

#file name - sample.logstash.conf
EOF >>
input {
   file {
      path => "/home/ec2-user/input.log"
   }
}

output {
   file {
      path => "/home/ec2-user/output.log"
   }
}

3.
#now we need to call the file we created in step (2) lets nagivate to the folder. Copy the content of the file you created ei 'sample.logstash.conf'
$ cd /usr/share/logstash/bin/

#after the copy, lets run this command.

$ ./logstash -f sample.logstash.conf

# add random data on the input file, the pipeline will generate an output file based on what you have definded.

# #################################################################################################################
#
# 
#                        https://www.elastic.co/guide/en/logstash/current/index.html
#
#
# #################################################################################################################

###testing kibana
$ ps -ef | grep -i kibana
$ sudo lsof -i -P -n | grep 5601
$ curl -I http://localhost:5601/status

#to configure kibana change the context of this file: kibana.yml
[root@i-036186b0b1b218c0 kibana]# pwd
/etc/kibana
[root@i-036186b0b1b218c0 kibana]# ls
kibana.keystore  "kibana.yml"  node.options
[root@i-036186b0b1b218c0 kibana]#

# #################################################################################################################
#
# 
#                       VERY VERY IMPORTANT NOTE!!!!!!!!!!!!!!!
#  To remotely connect to Kibana, set server.host to a non-loopback address. 
   for instance the configs should look like this:
     server.port: 5601    
     server.host: "0.0.0.0"
#   you make any changes to this file  kibana.yml stop and start the server. YOU MUST STOP and START or you will TROUBLESHOOL until Piet KOM!!
# #################################################################################################################

# https://www.elastic.co/guide/en/kibana/current/access.html