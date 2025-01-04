- Mock interview video - https://youtu.be/i7YJesoeWFI
- Mock interview Answers - https://youtu.be/5w8qVukxXXY 

GIT
---------------------------------------------------------------------------------------------------------------------------------
### 1. Why we need git? What makes git unique from other tools like SVN?

   Git is the most commonly used Version control system. It will track the changes that you made to a file, so you have a record of what you have been done and 
   you can revert back to the specified versions. \
   Git also makes collobaration easier, allowing changes by multiple people to all be merged into one sources. \
   Git is a Decentralized Version Control tool and commits are possible even if offline. Push/pull operations are faster when compared to SVN.

------------------------------------------------------------------------------------------------------------------------------------
### 2. Let's say i have maven repo cloned on to my local, did some changes and i have build the code now target folder will be generated. So now when i do git operations like git add, git commit or any other git operations target folder should not be considered, how would you achieve the same?

   We can achieve this by using .gitignore \
   Create one file .gitignore and add the folder to .gitignore
   ```sh
      $ vi .gitignore
        target/
      $ git add .
      $ git commit -m "target added to .gitignore"
      $ git push
   ```
---------------------------------------------------------------------------------------------------------------------------
### 3. difference between git pull and git fetch?
   ```sh
      git pull = git fetch + git merge
   ```

   **git pull**
      git pull performs two functions using a single command. It fetches all the changes that were made to the remote branch and then merges those changes into our             localbranch.

   **git fetch**
      git fetch just brings the remote changes into your local repo but doesnt apply them onto our branches. We need to explicitly apply those changes.

---------------------------------------------------------------------------------------------------------------------------------
### 4. How to clone specific branch in git?
   ``` sh
       git clone -b branchname --single-branch gitcloneurl
   ```

Maven
--------------------------------------------------------------------------------------------------------------------------
### 5. When we issue mvn install what all things happen in background?
   
   There are three built in build lifecycles: default, clean and site \
   **clean**: cleans up artifacts created by prior builds. \
   **default**: used to build the application. \
   **site**: generates site documentation for the project.
   
   **Phases of default Lifecycle**
   **validate** − Validate the project and check if everything is correct and all necessary information is available. \
   **compile** − This phase compiles the source code of your project. \
   **test** − Tests the compiled source code by using a suitable unit testing framework. These tests should not require the code to be packaged or deployed \
   **package** − Takes the compiled code and packages it in its distributable format. \
   **integration-test** − Processes and deploys the package if possible into an environment where integration tests can be run. \
   **verify** − Runs any checks to verify the package is valid and meets the required quality criteria. \
   **install** − installation of the package into the local repository. This is done to use it as a dependency in other projects locally. \
   **deploy** − done in an integration environment or release environment. Here the final package is copied to the remote repository for sharing with other developers        and projects.
 
---------------------------------------------------------------------------------------------------------------------------
### 6. what are the settings you need to do before running mvn deploy?

------------------------------------------------------------------------------------------------------------------
### 7. why maven takes much time for 1st execution and from 2nd execution it will take less time?
 
   For the first time maven will download all the dependencies we have mentioned and will place it in .m2 \
   For the second time if you run again it will refer from .m2 so it will take very less time

Unix and Shell Scripting 
--------------------------------------------------------------------------------------------------------
### 8. How to get present working folder?
   ```sh
   basename "$PWD"
   ```
   or
   ```sh
   pwd | rev | cut -d '/' -f 1 | rev
   ```
------------------------------------------------------------------------------------------------
### 9. How to copy files from local windows machine to cloud based Linux machine?

   Goto Powershell in Windows 
   ```sh
   pscp ./filename root@192.168.43.32:/home
   ```
   viceversa
   ```sh
   pscp root@192.168.43.32:/home C:\Temp
   ```
   
------------------------------------------------------------------------------------------------
### 10. A shell script named test.sh can accept 4 parameters i.e, a,b,c,d. the parameters wont be supplied in order always and number of parameters might also vary( only 2 parameters user might supply sometimes), how to identify position of letter c?
   **script.sh**
   ```sh
   i=0;
   for p in "$@" ; do
       i=$((i+1))
           if [ "$p" = "c" ]; then 
           echo "User supplied C has a parameter, in $i position"
           fi
   done
   ```

Ansible
---------------------------------------------------------------------------------------------------------------------
### 11. Why we need ad-hoc ansible commands, scenario where you have used ansible ad-hoc command?
Ad-hoc commands are simple one-line commands used to perform a certain task. It is an alternative to writing playbooks. An example of an Adhoc command is as follows:
   ```sh
   ansible all -m ping
   ```
   
------------------------------------------------------------------------------------------------
### 12. When i need detailed logs on executing ansible playbook what option i need to use?
We can get the detailed logs by providing -v flag
   ```sh
   ansible-playbook -i hosts play.yml -vvv
   ```
------------------------------------------------------------------------------------------------
### 13. what is ansible.cfg file?

This is the brain and the heart of Ansible. The file that governs the behavior of all interactions performed by the control node. In Ansible's case that default         configuration file is **ansible.cfg** located in /etc/ansible/

------------------------------------------------------------------------------------------------
### 14. what are the modules have you worked on? which module will you use for getting the file from node to master?
I have worked on copy, fetch, yum, debug, Get_url, expect, Template, file, apt, Command, Shell. \
Fetch module is used to get the file from node to master
   ```sh
   ---
   - hosts: dev
     become: yes
     gather_facts: false
     tasks:
       - name: Install application tree
         fetch:
           src: src_path
           dest: dest_path
   ```

------------------------------------------------------------------------------------------------
### 15. Lets say i have a playbook which has 5 tasks in playbook, first 2 tasks should run on local machine and other 3 tasks should run on node?
We can achieve this by using multiple play \
   **multiple_play.yml**
   ```sh
   ---
   - hosts: localhost
     become: yes
     gather_facts: false
     tasks:
       - name: installing wget 
         apt:
           name: wget
           state: present 
       - name: download Jenkins
         get_url:
           url: https://updates.jenkins-ci.org/download/war/2.248/jenkins.war
           dest: /home/spovedd
   - hosts: dev
     become: yes
     gather_facts: false
     tasks:
       - name: copy jenkins war to host machines
         copy:
           src: /home/spovedd/jenkins.war
           dest: /home/jenkins.war
       - name: creating folder structure and running jenkins in the background 
         shell: |
           mkdir -p /home/spovedd/jenkins
           mv /home/jenkins.war /home/spovedd/jenkins
           nohup java -jar /home/spovedd/jenkins/jenkins.war &
   ```

Jenkins
-----------------------------------------------------------------------------------------------------------------------
### 16. How to save only last 5 builds of jenkins job?

   Goto configure >> Discard Old Builds >> Max no of build to keet we need to set 5. Then we can save only last 5 build

--------------------------------------------------------------------------------------------------------------------------
### 17. Have you worked on Jenknsfile? can we use docker container as a node in Jenkinsfile? Who will handle docker container creation and deletion? If i am building a maven project always docker container is fresh instance it will try to download dependency from repository, what measures you will take to reduce build time?

   We can copy .m2 to the container and we can reduce the build time

--------------------------------------------------------------------------------------------------
### 18. Why we need multi branch pipeline?

   The multi branch pipeline project type enables you to implement different Jenkinsfiles for different branches of the same project. \
   In a multi branch pipeline project, Jenkins automatically discovers, manages and executes Pipelines for branches which contain a Jenkinsfile in source control\
   Please visit https://www.jenkins.io/doc/book/pipeline/multibranch/ 

-------------------------------------------------------------------------------------------------------------
### 19. If you forget Jenkins password, how would you login back?

   Please goto config.xml under jenkins_home and then make false as below
   ```sh
   <userSecurity>false</useSecurity>
   ```

Docker
------------------------------------------------------------------------------------------------------------------------------
### 20. Any 3 best practices of docker?

   => ALways keep Dockerfile in an empty directory or make sure directory where Dockerfile present with required files \
   => Use official images when possible \
   => Use more specific tags for the images \
   => Look for minimal flavours (Using a different base image will reduce the image size \
   => Multi-Stage builds to remove build deps \
   
----------------------------------------------------------------------------------------------------------------------
### 21. Difference between docker stop and docker kill?
   **docker stop**
   Stops a running container (send SIGTERM and then SIGKILL after grace period) \
   The main process inside the container will receive SIGTERM and after a grace period SIGKILL
   
   **docker kill**
   Killing a running container (Send SIGKILL or specified signal) \
   The main process inside the container will be sent SIGKILL or any signal specified with option --signal
   
   We can see the events by docker events

-----------------------------------------------------------------------------------------------------------------------
### 22. Command to list conatiners which state is exited?
   ```sh
   docker ps -a -f status=running
   docker ps -a -f status=exited
   docker ps -a -f status=created
   ```
   Here -f is a filter \
   To remove a stopped container
   ```sh
   docker rm $(docker ps -a -f status=exited -q)
   ```
   
----------------------------------------------------------------------------------------------------------------------------
### 23. command to clean-up docker host ( deleting stopped conatiners, dangling images and unused networks)?
   ```sh
   docker system prune
   ```
   
----------------------------------------------------------------------------------------------------------------------------
### 24. What version of docker you have used? Specific reason to use that particular version?
   Please visit https://docs.docker.com/release-notes/

-----------------------------------------------------------------------------------------------------------------------------
### 25. Can we have multiple CMD in Dockerfile?
   We can have multiple CMD's but it wont take into account. It will take the lase CMD. So better to take last CMD

---------------------------------------------------------------------------------------------------------------------------
### 26. Have you worked on docker swarm and docker compose?
   **Docker Swarm**
   Docker swarm is a container orchestration tool, meaning that it allows the user to manage multiple containers deployed across multiple host machines. One of the key      benefits associated with the operation of a docker swarm is the high level of availability offered for applications.
   
   **Docker Compose**
   Docker Compose is a tool that was developed to help define and share multi-container applications. With Compose, we can create a YAML file to define the services and    with a single command, can spin everything up or tear it all down.

Kubernetes
------------------------------------------------------------------------------------------------------------------------------
### 27. Can we have multiple conatiners in a pod? Can we have similar conatiners in a pod? Lets say i have 4 conatiners, one of them has failed how would you check which container has failed?
   Yes we can have multiple containers in a Pod. \
   Similar containers can be there in a single Pod. \
   We can check the failed Pod status by
   ```sh
   kubectl describe pod/podname -n namespace --kubeconfig config
   ```
 
 ---------------------------------------------------------------------------------------------------
### 28. What is liveness and readiness probe? Why we need them?

   **Liveness Probe:** \
   Kubernetes uses liveness probes to know when to restart a container \
   If a container is unresponsive -- perhaps the application is deadlocked due to multi-threading defect - restarting the container make the application more available, despite    the defect. It certainly beats paging someone in the middle of the night to restart a container.
   
   **Rediness Probe:** \
   Kubernetes uses readiness probes to decide when the container is available for accepting the traffic. \
   The readiness probe is used to control which pods are used as the backends for a service. \
   A pod is considered ready when all of its containers are ready. If a pod is not ready, it is removed from service load balancers. \
   
   **For example:** If a container loads a large cache at startups and takes a minute to start, you do not want to start requests to this container untill it is ready, or the      request will fail -- you want to route requests to other pods, which are capable of servicing requests.
   
-------------------------------------------------------------------------------------------------------
### 29. Have you worked on kubernetes monitoring? Which tools you have used?
   
### 30. Can we deploy a pod on particular node?
   We can do this as we have so many approaches like Node selctor, Node affinity, taints, tolerations \
   For now we will go through node selector. In this we neet to mention labels of node in the selector in pod.yml \
   ```sh
   kubectl label nodes node-name size-medium
   ```
