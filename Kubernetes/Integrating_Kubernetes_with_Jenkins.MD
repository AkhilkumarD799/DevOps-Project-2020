## Integration Kubernetes with Jenkins

# *Jenkins CI Job:* `Deploy_on_Kubernetes-CI`

### Pre-requisites

1. Jenkins server 
1. Ansible server
1. Kubernetes cluster
 
### Steps to create "Deploy_on_Kubernetes_CI" Jenkin job
#### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Kubernetes_CI`
     - Copy from: `Deploy_on_Docker_Container_using_Ansible_playbooks`
     
   - *Source Code Management:*
      - Repository: `https://github.com/akhilkumard799/hello-world.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name: `ansible-control-node`
       - `Transfers` >  `Transfer set`
           - Source files: `webapp/target/*.war`
	       - Remove prefix: `webapp/target`
	       - Remote directory: `//opt//kubernetes`
	       - Exec command: 
                ```sh 
                ansible-playbook -i /opt/kubernetes/hosts /opt/kubernetes/push-simple-devops-image.yml;
                ```

Save and run the job.

# *Jenkins CD Job:* `Deploy_on_Kubernetes-CD`

### Steps to create "Deploy_on_Kubernetes_CI" Jenkin job
#### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Kubernetes_CI`
     - Freestyle Project
	 
  - *Post-build Actions*  
    - Send build artifacts over SSH  
      - *SSH Publishers*  
	       - Exec command: 
                ```sh 
                ansible-playbook -i /opt/docker/hosts /opt/docker/kubernetes-akhil-deployment.yml;
                ansible-playbook -i /opt/docker/hosts /opt/docker/kubernetes-akhil-service.yml;
                ```
Save and run the job.
