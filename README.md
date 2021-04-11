# task_12.2


Follow the steps below to build the setup :

**Step 1:** download ec2.py and ec2.ini files from this link

https://github.com/ansible/ansible/tree/stable-2.9/contrib/inventory

use "wget" command to download these files

wget https://github.com/ansible/ansible/tree/stable-2.9/contrib/inventory/ec2.py

wget https://github.com/ansible/ansible/tree/stable-2.9/contrib/inventory/ec2.ini
also make these file executable, use command chmod

chmod +x ec2.py

chmod +x ec2.ini

after this color of both the files changes as shown in the image below.


**Step 2 :** Install ec2 and boto python libraries.

use pip to isntall these libraries

pip3 install ec2

pip3 install boto
**Step 3 :** Create an IAM User as we need Access Key and Secret Key

Create a user in AWS IAM service as we need the Access and Secret key of the user to login. also while creating the user give the user some power.

To create a user goto IAM > Add user > enter details > give power > store secret and access key some where as we need these keys.

NOTE : Never share your Access and Secret keys with anyone.

**Step 4:** Set your IAM Access and Secret Key

Goto the directory where we download the ec2.py and ec2.ini files, here we need to set the keys here

“ export AWS_ACCESS_KEY_ID=’XXXXXXXXXXXXXXXX’ ”

“ export AWS_SECRET_ACCESS_KEY=’XXXXXXXXXXXXXXXXXXXXXX’ ”

“ export AWS_REGION= 'ap-south-1' ”

**Step 5 :** Setup ansible configuration file

There are few details we have to provide in this file

Give path to the dynamic inventory file i.e the directory where we install ec2.py and ec2.ini.
disable host_key_checking
Set remote user
Give path of the private key (We need to secure the private key as well so execute the chmod 400 aws_cool.pem)
set privileges escalation


**Step 6 :** Create an ansible vault to store Access Key and Secret Key

To safe keep the keys we need to put these keys inside the vault so that no one can see them.

ansible-vault create <vault_name>.yml
after executing this command ansible asks to set a password for this vault in the next line. So, write the password and confirm it and the vault is ready.

We can create, edit, view, delete the vault using the command above.

NOTE : If you forget the password there is no way to recover the data in the vault so careful about it.

Now write the keys inside the vault in key-value pair format so that we can use them in code file as variables.

for the safe keep of the keys execute command " chmod 400 key.yml "

chmod 400 <file_name> - Gives the user read permission, and removes all other permission.



**Step 7 : **Now write the code.

we need to write code for ec2 instance provisioning configure then as Load Balancer and Backend Server.

Get the code and all the other files from my Github the link is in the end of the article.




**Step 8 :** launch instances

here I am launching 2 instances as backend server and 1 instance as load_balancer

Command : ansible-playbook --ask-vault-pass EC2.yml

--ask-vault-pass : it will ask the vault password that contains the keys as it needs the keys to login to AWS.

after the file successfully executed try to ping the all the hosts to confirm if we have the connectivity or not

use commmand : ansible all -m ping 



**Step 9 :** Execute haproxy.yml file as it contains reverse proxy configuration for load balancer.

Command : ansible-playbook haproxy.yml

after executing the file check the aws instance that we launched as load balancer for the configuration file.





**Step 10 :** Access the website using public IP of load balancer.

the port number that I specify for Haproxy is 8080 so the format in which we need to access the backend server is

<IP_address_of_loadbalancer>:<port_number>/<page_name>

for example : 15.207.115.249:8080/index.php
