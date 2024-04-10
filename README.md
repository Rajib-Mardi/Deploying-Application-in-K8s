
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


