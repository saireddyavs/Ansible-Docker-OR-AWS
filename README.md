
## Details

Deploy any number of EC2 Sample flask web servers with  mysql  and Extra EC2 instance which monitor all web servers.

 you can also deploy application using Docker Containers in your computer.



## Required

* You need to provide the git hub repo link where you flask application has present.
* make sure that there is a ```.sql``` file for ```Tables``` creation.
* The name of python file should be ```app.py```

# For AWS

## AWS credintials

* You can store them in ```.boto``` file
* Or export to the environment variable using ```export AWS_ACCESS_KEY=XXXXXXX  ``` and ```export AWS_SECRET_KEY=XXXXXXX``` **as of now this step is implemented in code** 
* Or you can directly declare them as variables, in ```group_vars``` or in ```playbook vars```.

> So modify    ```ec2_create``` role according to your usage
* ssh setup is required. for making chnages in EC2 instances.

* ```ssh-keygen```  for creating new ssh keys. Accept the default values, and no passphrase, if it prompts you to add the new keys to the home directory, without overwriting existing ones.
* Now in AWS EC2 keypairs section create a new keypair using ```import key pair``` option which is present in ```Actions``` and copy paste the content in ```cat .ssh/id_rsa.pub```.
* Change key-pair name accordingly in ```roles/vars/ec2_create``` the default key pair name i have used is ```jenkins_ubuntu```.
* or else if you are familiar with AWS-CLI you can use ```aws ec2 import-key-pair --key-name "your'e key name" --public-key-material fileb://~/.ssh/id_rsa.pub```

## Nagios

* you are free to add Nagios notification alerts.
* The default nagios credintials are:
    * user_name: nagiosadmin and password is password itslef.
* you can add any name and password, and you can change monitoring thresholds.

### steps to run
* clone the repo:  ```git clone https://github.com/saireddyavs/Ansible-Docker-OR-AWS.git```.
* make changes to default variables like key-pair etc..
* The code works for all linux Distributions of your'e choice in AWS AMI like Ubuntu,CentOS, RedHat etc..,
* For running the code in AWS EC2 instances: ``` ansible-playbook run_setup.yml -e env_to_run=AWS``` or ```ansible-playbook run_setup.yml``` as the code AWS by default.
* you can see your web-application using, copy pasting ```public_ip ``` of your any of your web-servers as the URL in browser.
* for Nagios , copy pasting ```public_ip/nagios``` as the URL in browser,here   ```public_ip``` refers to ip of nagios-server.
 You can login  with ```username:nagiosadmin and password:password```  to monitor all your web-servers.


# For Docker
* For running code in Docker Containers in your computer :```ansible-playbook run_setup.yml -e env_to_run=Docker```
* As of now monitoring support is not yet added, The code spins up three different  containers for MySQL,Flask,Nginx.



## For Jenkins(testing)
* make sure you have ssh setup fot jenkins, if not follow below procedure.
* ```su -s /bin/bash jenkins``` then recheck the user using ```whoami```  and ```echo $HOME```
* ```ssh-keygen``` for creating new ssh keys. Accept the default values, and no passphrase, if it prompts you to add the new keys to the home directory, without overwriting existing ones.
* Now in AWS EC2 keypairs section create a new keypair using ```import key pair``` option which is present in ```Actions```.
* Change key-pair name accordingly in ```roles/vars/ec2_create``` the default key pair name i have used is ```jenkins_ubuntu```.
* In future SonarQube suppourt will be added.






> Your'e free to change the code and any contributions and suggestions are welcomed.

## References

* sample flask application is from [here](https://github.com/EswarGitHub/GymManagementSystem) where i have modified and fixed some errors, the updatd version is [here](https://github.com/saireddyavs/Gym-appication).
* for monitoring i used help from [Sam-Darwins repo](https://github.com/sdarwin/Ansible-Nagios). 
