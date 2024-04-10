### ANSIBLE


<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/aedf78c7-ec45-47b7-a086-2bd1ebc6d612" width="150">

___________

###  Project: 

  * Automate Node.js application Deployment 
 ### Technologiesused: 
  * Ansible, Node.js, DigitalOcean, Linux

### Project Description:
  ### Create Server on DigitalOcean: 
   * Create a server on DigitalOcean, take the IP address, In Ansible, add the hosts file, add the IP address, and configure it with a ssh private key and user as root.


  ### Ansible Playbook that installs necessary technologies, creates Linux user for an application and deploys a NodeJS application with that user
  ### Written Play “Install node and npm”

   1. Install Node.js and npm

      * Target host: 157.245.131.45.
   2. Update APT Repository and Cache

      * Uses the ```apt``` module to update the APT package repository and cache.
      *  ```update_cache=yes``` updates the cache.
      * ```force_apt_get=yes``` ensures that the ```apt-get``` command is used.
      * ```cache_valid_time=3600``` sets the cache validity time to 3600 seconds (1 hour).
   3. Install Node.js and npm

      * Uses the ```apt``` module to install Node.js and npm packages.
      * The packages to install are specified in the ```pkg``` section, which includes 'nodejs' and 'npm'.

   4. Create New User for Node.js App

      *  Target host: 157.245.131.45.
      * Uses variables from a file named ```project-vars```.
   5. Create New User

      * Creates a new user with the following properties:
         * Name is defined by the variable ```name```.
         * Comment is set to "node User."
         * The user is added to the 'admin' group.
      * Registers the result of the user creation in the ```user_creation_result``` variable.
      * Displays the output of the ```user_creation_result``` for debugging.
     
   6. Deploy Node.js App

      * Target host: ```157.245.131.45```.
      * Uses variables from a file named ```project-vars```.
      * Executes tasks with elevated privileges (```become: True```) as the user specified by the variable ```name```.
  7. Unpack Node.js App

      * Unarchives the Node.js application package (specified by ```location``` and ```version```) from a ```.tgz``` file to the destination directory (destination).
      * Registers the result in the ```user_creation_result``` variable.
      * Displays the output of the ```user_creation_result``` for debugging.
  8. Install Dependencies
      * Uses the ```npm```Ansible module to install the dependencies of the Node.js application.
      * The path to the application package is specified as ```{{destination}}/package```.
  9. Start the Application

      * Changes the current directory to ```{{destination}}/package/app```.
      * Executes the Node.js application using the ```node server``` command.
      * Runs the command asynchronously with a timeout of 1000 seconds (1,000 seconds).
      * Does not poll for the command completion (```poll: 0```).
 10 . Ensure App is Running

      * Checks if the Node.js application is running by using the ```ps aux | grep node``` command.
      * Registers the result in the ```app_status``` variable.
      * Displays the output of the ```app_status``` variable for debugging.


### Execute the ansible-play-book.

<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/a1018a74-985f-4349-8063-76c0b627a71e" width="700">


------------------------------------------------------------------------------------------------------


### Project: 
   * Automate Nexus Deployment
### Technologiesused: 
   * Ansible, Nexus, DigitalOcean, Java, Linux

### Project Description:
 * Create Server on DigitalOcean Write Ansible Playbook that creates Linux user for Nexus, configure server, installs and deploys Nexus and verifies that it is running successfully

### Create Server on DigitalOcean: 
   * Create a server on DigitalOcean, take the IP address, In Ansible, add the hosts file, add the IP address, and configure it with a ssh private key and user as root.


### Ansible Playbook that creates Linux user for Nexus, configure server, installs and deploys Nexus and verifies that it is running successfully

1. Install Java and Network Tools

   * Target host: ```nexus_server```(using a common name because we dont want to pass dirctly ip address in the host file).
2. Update APT Repository and Cache

   * Uses the ```apt``` module to update the APT package repository and cache.
   * ```update_cache=yes``` updates the cache.
   * ```force_apt_get=yes``` ensures that the ```apt-get``` command is used.
   * ```cache_valid_time=3600``` sets the cache validity time to 3600 seconds (1 hour).
3. Install Java 8

   * Uses the ```apt``` module to install the 'openjdk-8-jre-headless' package, which corresponds to Java 8.
4. Install Network Tools

   * Uses the ```apt``` module to install the 'net-tools' package, which provides a set of networking tools.

5. Download and Unpack Nexus Installer

   * Target host: ```nexus_server```.
6. Check Nexus Folder Stat

   * Uses the stat module to check the status of the /opt/nexus directory and registers the result in the stat_result variable.
7. Download Nexus Installer

   * Uses the ```get_url``` module to download the latest Nexus installer (tar.gz archive) from the specified URL (https://download.sonatype.com/nexus/3/latest-unix.tar.gz) and saves it to the /opt/ directory.
   * Registers the result in the ````download_result```` variable.
8. Untar the Nexus Installer

   * Uses the ```unarchive``` module to extract the downloaded Nexus installer from ```{{download_result.dest}}``` to the ```/opt/ directory```.
   * The extraction is done remotely since ```remote_src: yes``` is specified.
   * This task only runs if the ```/opt/nexus``` directory doesn't exist, as indicated by ```when: not stat_result.stat.exists```.
9.Find Nexus Folder

   * Uses the ```find``` module to search for directories in the ```/opt``` path that match the pattern "nexus-*".
   * Registers the result in the ```find_result``` variable.
10. Rename Nexus Folder

    * Uses the ```shell``` module to rename the first directory found in the previous step to ```/opt/nexus```.
    * This task runs only if the ```/opt/nexus``` directory doesn't exist, as indicated by ```when: not stat_result.stat.exists```.
   
11. Create Nexus User and Set Ownership

     * Target host: ```nexus_server```.

     * This section creates a 'nexus' user, ensures the 'nexus' group exists, and sets ownership of certain directories.

     *  ```Ensure   group nexus exists```

          * Uses the ```group``` module to ensure that a group named 'nexus' is present.
     * ```Create nexus user```

        * Uses the ```user``` module to create a user named 'nexus' and adds them to the 'nexus' group.
     * ```Make nexus user owner of nexus folder```

        * Uses the ```file``` module to set the owner and group of the '/opt/nexus' directory to 'nexus' and 'nexus,' respectively.
        * The` ```recurse: yes``` option ensures that ownership changes are applied recursively.
    * ```Make nexus user owner of sonatype-work folder```

        * Similar to the previous task, this task sets the owner and group of the '/opt/sonatype-work' directory to 'nexus' and 'nexus' respectively, with recursive application.
12. ```Start Nexus with Nexus User```

     * Target host: ```nexus_server```.

     * This section sets up and starts the Nexus service as the 'nexus' user with elevated privileges.

     * ```Set run_as_user in Nexus Configuration```

         * Uses the ```lineinfile``` module to modify the '/opt/nexus/bin/nexus.rc' file by uncommenting the line ```run_as_user="nexus"```.
     * ```Start Nexus```

         * Uses the ```command``` module to execute the '/opt/nexus/bin/nexus start' command to start the Nexus service.
13. Verify Nexus Running

     * Target host: ```nexus_server```.

     * This section checks and verifies that the Nexus service is running.

     * ```Check with ps```
       
         * Uses the ```shell``` module to execute the 'ps aux | grep nexus' command to check for the Nexus process and registers the result in the ```app_status``` variable.
       * Displays the output of the ps command for debugging.
     * ```Wait for one Minutes```
  
       * Pauses the playbook execution for 1 minute to allow time for the Nexus service to start.
     * ```Check with netstat```

       * Uses the ```shell``` module to execute the 'netstat -lnpt' command to check the network status and 
       * registers the result in the ```app_status ```variable.
       * Displays the output of the netstat command for debugging.


### Execute the ansible-play-book.

<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/a469a230-70ff-4f8c-8992-d824e3f02edd" width="700">
<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/8535a5ed-9513-4a99-bff7-bb7d658eb8ba" width="700">

------------------------------------------------------------------------------------------------------


###  Project: 
   * Ansible & Docker
###  Technologiesused: 
   * Ansible, AWS, Docker, Terraform, Linux

 ### Project Description:

###  Create AWS EC2 Instance with Terraform 
### Written Ansible Playbook that installs necessary technologies like Docker and Docker Compose, copies docker-compose file to the server and starts the Docker containers configured inside the docker- compose file

1. Install Pip3, Docker, and Docker-Compose

   * Target hosts: ```docker_server```.

   * This section installs Python 3 (pip3), Docker, and Docker-Compose on the  hosts.

   * Ensure Pip3 and Docker Installed

      * Uses the ```yum``` module to install the 'python3-pip' and 'docker' packages.
      * Updates the package cache and ensures that these packages are installed.
   * Install Docker-Compose

      * Downloads the Docker-Compose binary from the official GitHub releases based on the host's architecture.
      * Sets the binary to be executable and places it in '/usr/local/bin'.
   * Start Docker Daemon

      * Uses the ```systemd``` module to ensure the Docker service is started.
2. Install Docker Python Modules

   * Target hosts: ```docker_server```.

   * This section installs Docker-related Python modules using the ```ansible.builtin.pip``` module.

   * Install Docker Python Module

      * Installs the 'docker' and 'docker-compose' Python modules.
3. Add ec2-user to Docker Group

   * Target hosts: ```docker-server```.

   * This section adds the 'ec2-user' to the 'docker' group, granting them access to Docker.

   * ```Add ec2-user to Docker Group``` 

     * Uses the ```user``` module to add the 'ec2-user' to the 'docker' group, ensuring that the 'append' option is set to 'yes'.
     * Reconnects to the server session for the changes to take effect.
4. Start Docker Container

   * Target hosts: ```docker_server```.

   * Uses variables from a file named ```project-vars``` to set the dockerhub password as variable.

   * ```Copy Docker Compose File```

     * Copies a Docker Compose file from the source directory to the 'ec2-user' home directory.
   * ```Docker Login```

     * Uses the ```community.docker.docker_login``` module to log in to the Docker registry using the specified username and password.
   * ```Start Container from Compose```

     * Uses the ```docker_compose``` module to start a Docker container using the Docker Compose file.
     * The ```project_src``` parameter specifies the location of the Docker Compose file.



### Execute the ansible-play-book.


<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/2a5ba88b-1fd4-410f-80cd-76a0e9638656" width="750">
<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/11e69785-6d69-438b-a4b1-8b721949a466" width="750">


<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/9e6fe2ac-0a3f-4eb7-8063-46b70ca2fc26" width="800">


-----------------------------------------------------------------------------




### Project: 
  * Ansible Integration in Terraform
### Technologiesused: 
  * Ansible, Terraform, AWS, Docker, Linux
###  Project Desscription:

###  Create Ansible Playbook for Terraform integration
  * Name: "Wait for ssh connection"

  * ```Target Hosts```: "all" (it will be executed on all hosts defined in the inventory).

  * ```Gather Facts```: Set to "False," which means Ansible won't collect facts about the target hosts before running tasks.

  * ```Tasks```:

  1. Ensure SSH Port is Open:
     * Uses the ```ansible.builtin.wait_for``` module to wait for the SSH port (port 22) to become open.

     * The ```port``` parameter specifies the port to wait for, in this case, 22 (the default SSH port).

     * ```delay``` specifies a delay of 10 seconds before starting to check.

     * ```timeout``` sets a timeout of 100 seconds, after which the task will fail if the SSH port is not open.

     * ```search_regex``` is used to search for the "OpenSSH" string in the response from the target host, indicating that the SSH port is open.

     * The ```host``` parameter dynamically gets the host's SSH address and defaults to "ansible_host" or "inventory_hostname" as needed.

     * ```Vars```:

        * ```ansible_connection``` is set to "local," which means the task is executed on the control machine (where Ansible is being run), not on the target hosts.
        * ```ansible_python_interpreter``` is set to "/usr/bin/python" to specify the Python interpreter to use for this local execution.



* Continue the use of docker-compose file for terraform intergration [link] 



###  Adjust Terraform configuration to execute Ansible Playbook automatically, so once Terraform provisions a server, it executes an Ansible playbook that configures the server.


### Used “null_resource” in Terraform configuration file.

* Resource Type: ```null_resource``` is used to represent a resource that doesn't create any infrastructure but can be used for running local-exec provisioners, which execute commands on the local machine.

* Resource Name: The resource is named ```"configure_server"```.

* ```Triggers```: The triggers block specifies a trigger that depends on the public IP address of an AWS EC2 instance created elsewhere in the Terraform configuration. This means that when the public IP of the ```aws_instance.myapp-server``` instance changes, it triggers the local-exec provisioner to run again.

* ```Provisioner```: It defines a local-exec provisioner. This provisioner allows you to execute a local command, in this case, an Ansible playbook, on the local machine running Terraform.

    * ```working_dir```: Specifies the working directory where the command is executed. it's "/home/rajib/ansible-playbooks."

   * ```command```: Specifies the command to run. It runs an Ansible playbook on the remote EC2 instance using the specified inventory, private key, and user. The ```${aws_instance.myapp-server.public_ip}``` interpolation is used to get the public IP of the EC2 instance, and ```${var.ssh_key_private}``` is used to specify the private SSH key to use. The Ansible playbook being executed is "deploy-docker-new-user.yaml."


### Execute the terraform-apply.
![main tf - terraform  WSL_ Ubuntu  - Visual Studio Code 24-10-2023 14_03_25](https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/efcdf73d-16cd-4646-8070-5a1754f4a4b6)


![ec2-user@ip-10-0-1-235_~ 24-10-2023 14_02_23](https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/a162b278-28cd-49d4-80d7-1df818579565)



-----------------------------------------------


###  Project: 
  * Configure Dynamic Inventory 
### Technologiesused: 
  * Ansible, Terraform, AWS


### Project Description:
  *  Create EC2 Instance with Terraform
  
* Ansible AWS EC2 Plugin to dynamically sets inventory of EC2 servers that Ansible should manage, instead of hard-coding server addresses in Ansible inventory file

  ### plugin configuration for aws_ec2

  1. ```Plugin```: Specifies that the plugin to be used is the ```aws_ec2``` plugin. This plugin interacts with the AWS API to discover EC2 instances.

2. ```Regions```: Lists the AWS regions where the dynamic inventory should look for EC2 instances.  it specifies "eu-central-1". 

3. ```Keyed Groups```: This section configures groups that are created based on specific attributes of EC2 instances. The purpose of these groups is to organize instances in the inventory.

   * ```Tags```: Instances are grouped based on their tags. A group is created for each unique tag key with the prefix "tag."

   * ```Instance Type```: Instances are grouped based on their instance type. A group is created for each unique instance type with the prefix "instance_type."

### Adjusted the Ansible playbook[docker-compose-user link] and ansible.cfg to use dynamic hosts from AWS.

### Execute the ansible-playbook

<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/1e027a04-bdb3-4d13-85dd-1fc77ae5bf2d" width="700">
<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/f8f5cd03-22e3-4dd5-b4a3-991a0092dbc1" width="700">
<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/35d8cb4a-d792-4779-a19b-6e23086827e6" width="700">


<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/029d0f7d-c094-4117-8a34-6a542456920c" width="700">

------------------------------

### Project: 
   * Automate Kubernetes Deployment
### Technologiesused: 
   * Ansible, Terraform, Kubernetes, AWS EKS, Python, Linux

### Project Description:

### Create EKS cluster with Terraform

 *  Set kubeconfig environment variable


 ### Install the python module in local machine to use the kubernetes modules for ansible.
   * kubernetes 
   * PyYAML
   * jsonpatch

### Ansible Play to deploy application in a new K8s namespace
  
1. Name: "Deploy app in new namespace"

2. ```Hosts```:  "localhost," which means that these tasks are executed on the machine where Ansible is being run, not on a remote server.

3. Tasks:

a. Create a Kubernetes Namespace:

   * Uses the ```kubernetes.core.k8s``` Ansible module to create a new Kubernetes namespace named "my-app."
   * The ```api_version``` is set to "v1," indicating it's a core Kubernetes resource.
   * The ```kind``` is set to "Namespace," specifying the type of resource to create.
   * The ```state``` is set to "present," indicating that the namespace should be created if it doesn't already exist.
b. Deploy nginx App in the Namespace:

   * Uses the ```kubernetes.core.k8s``` Ansible module to deploy a Kubernetes resource defined in the YAML file located at "~/nginx-config.yaml."
   * The ```state``` is set to "present," indicating that the specified resource should be created in the Kubernetes cluster.
   * The ```namespace``` is set to "my-app," specifying that the resource should be deployed in the "my-app" namespace created in the previous task.



5. ### execute the ansible-playbook



<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/89d0f3e9-4334-48b9-8d28-45f901967ffc" width="800">



<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/937e7f41-8f5f-4e20-ab9b-0c468c618c80" width="800">


![Welcome to nginx! - Google Chrome 25-10-2023 19_11_19](https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/c31f96e7-214d-4062-8329-d608f87b8ef2)


--------------------------------------------------------------------------------------

 ### Project: 
   * Ansible Integration in Jenkins 
 ### Technologiesused: 
   * Ansible, Jenkins, DigitalOcean, AWS, Boto3, Docker, Java, Maven, Linux, Git

### Project Description:

### Create and configure a dedicated server for Jenkins 

   * create a DigitalOcean droplet ssh into the server ,  Install docker and run jenkins as docker container.

### Create and configure a dedicated server for Ansible Control Node
  * create a DigitalOcean droplet
  * ssh into the server
    * Configure Droplet as Ansible Control Node(Install Ansible).
       *  ``` apt install ansible``` command
    * Install pip3
       * ```apt install python3-pip``` command 
    * Install boto3 and botocore
      
       * ``` apt install python3-boto3```
       * ```apt install python3-botocore```
    *  Configure the aws credentials in the ansible-server.
     * create  .aws credentials  in the home directory as root user.
     *  ```mkdir .aws```
     *   Create the credentials file and use a text editor vim to edit it. Copy and paste the AWS credentials you have locally in your home directory:
    
    
### Create two  EC2 Instances (for Ansible Managed Nodes) manually and download the pem file.

* written   the Ansible Playbook that installs  the  Docker and Docker Compose in the Ec2 server with ansible.cfg file , and inventory file



### Add ssh key file credentials in Jenkins for Ansible Control Node server(droplet server) and Ansible Managed Node servers(ec2 instance)

  * Install the ```ssh agent``` Plugin and  ```ssh pipeline steps plugin``` in jenkins.
      
### Configure Jenkins to execute the Ansible Playbook on remote Ansible Control Node server as part of the CI/CD pipeline

* ```Pipeline Agent```: The pipeline is set to execute on any available agent.

* ```Environment```: It defines an environment variable ```ANSIBLE_SERVER``` with the value "142.93.4.168," which represents the IP address of the Ansible control node.

* ```Stages```:

1. Copy Files to Ansible Server:

   * This stage is responsible for copying necessary files to the Ansible control node.
   * It uses the ```scp``` command to transfer files from the Jenkins agent to the Ansible control node, including Ansible playbooks and an SSH key.
   * The SSH key is obtained from Jenkins credentials (credentialsId: 'ec2-server-key').
     
2. Execute Ansible Playbook:

   * This stage triggers the execution of an Ansible playbook on the Ansible control node.
   * It defines a ```remote``` object that specifies the remote server (```ANSIBLE_SERVER```) to connect to and execute commands.
   * The SSH key for authentication is obtained from Jenkins credentials (credentialsId: 'ansible-server-key').
   * It executes the ```prepare-ansible-server.sh``` script and the ```ansible-playbook my-playbook.yaml``` command on the Ansible control node.
  
 ### execute the pipeline

   * execute the playbook remotely on that Control Node that will configure the 2 EC2 Managed Nodes.

<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/26a74bce-503b-4fd7-94ae-974fc4270ef3" width="700">



<img src="https://github.com/Rajib-Mardi/ansible-projects/assets/96679708/db1727fa-73a8-4a32-b3c9-b0bd17c5bb60" width="700">
