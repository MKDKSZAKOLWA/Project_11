# PROJECT 11: ANSIBLE CONFIGURATION MANAGEMENT - AUTOMATE PROJECT 7-10

- In project 7-10, we had to perform alot of manual operations to set up virtual servers, install and configure required software, and deploy the web application.
- This project will allow us to appreciate more the DevOps tools by making most of the routine tasks automated with *Ansible Configuration Management*. At the same time we will be confident in writing code using declarative language such as *YAML*.


## Ansible Client as a Jump Server (Bastion Host)

A *Jump Server* (sometimes also referred as *Bastion Host*) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.


![Virtual Private Network](./Images/Virtual%20Private%20Network.png)


When you reach Project 15, you will see a Bastion host in proper action. But for now, we will develop Ansible scripts to simulate the use of a Jump box/Bastion host to access our Web Servers.

## Tasks
- Install and configure Ansible client to act as a Jump Server/Bastion Host
- Create a simple Ansible playbook to automate servers configuration



## STEP 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE


1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

   Select the EC2 Jenkins server that you used in Project 9 and update it to Jenkins_Ansible.

   ![Jenkins_Ansible EC2](./Images/Jenkins_Ansible%20EC2.png)

- Connect to the Jenkins_Ansible terminal using SSH.


  ![jenkins_Ansible Login](./Images/Jenkins_Ansible%20Login.png)


2. In your GitHub account create a new repository and name it ansible-config-mgt.

   Login to your github.

   ![Github Login](./Images/Github%20Login.png)

   Click on New repository (Green icon with New).

   ![New Repo](./Images/New%20Repo.png)

   Name the repository as *ansible-config-mgt* , c check *add a README file, and then click *Create repository*.

   ![Create repository](./Images/Create%20repository.png)

   The screen below will be generated.

   ![Created repository](./Images/Created%20repository.png)



3. Instal Ansible.

   `sudo apt update`

   ![Sudo apt update](./Images/Sudo%20apt%20update.png)
   
   `sudo apt install ansible`. Click Y when prompted.

   ![Install ansible](./Images/Install%20ansible.png)
   
   Check your Ansible version by running;
   
   `sudo ansible --version`

   ![Ansible version](./Images/Ansible%20version.png)

4. Configure Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

    - Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.
    
      Login to ansible through the browser. Use your Public IP address for Jenkins_Ansible server. 

      http://18.220.231.43:8080

      The screen below will be displayed.

      ![Jenkins_Ansible Login2](./Images/Jenkins_Ansible%20Login2.png)


      Login with your credentials.

      Username : Project9_Admin

      Password: password


      Click New Item.


      ![New Item](./Images/New%20Item.png)

      Enter item name as *ansible*, click on *Frestyle project* and click ok.

      ![Freestyle project](./Images/Freestyle%20project.png)


      Change Source Code to *Git*.


      ![Source Code_Git](./Images/Source%20Code_Git.png)


      To connect your GitHub repository, you will need to provide its URL. You can copy from the repository itself.
      
      Go to the repository that you created in your Github and then under code , copy the https url as shown below.


      ![Ansible repo url](./Images/Ansible%20repo%20url.png)

      https://github.com/MKDKSZAKOLWA/ansible-config-mgt.git

      Copy it under repository url.

      ![Repository url](./Images/Repository%20url.png)

      Click Add under Credentials, then click on Jenkins.


      ![Add Credentials](./Images/Add%20Credentials.png)


      Get your Github credentials (Username and Password) and input in the below page, then click Add.


      ![Add Credential_2](./Images/Add%20Credentials_2.png)


      Save.

      ![Save Add Credentials](./Images/Save%20Add%20Credentials.png)


      The screen below will be displayed.

      ![Save Add Credentials](./Images/Add%20Credentials_2.png)


      Click "Configure"  and add these two configurations. 

      ![Configure ansible](./Images/Configure%20ansible.png)


      Configure triggering the job from GitHub webhook:

      Under Build Triggers select Github hook trigger for GITscm polling.


      ![Build Triggers](./Images/Build%20Triggers.png)


      Under Source Code scroll down to branches to build, replace *master* with *main* under Branch specifier.


      ![Branch Specifier](./Images/Branch%20Specifier.png)


      Click Save.

      ![Save Branch Specifier](./Images/Save%20Branch%20Specifier.png)




    - Configure a Post-build job to save all (**) files, like you did it in Project 9. – files resulted from a build are called "artifacts".

      ![Archive the artifacts](./Images/Archive%20the%20artifacts.png)

      Put ** under Files to Archive and then save.


      ![Files to archive](./Images/Files%20to%20archive.png)


      Save.


      ![Save archive files](./Images/Save%20archive%20files.png)


      The screen below will be displayed.

      ![Archive the artifacts_2](./Images/Archive%20the%20artifacts_2.png)


    - Configure Webhook in GitHub and set webhook to trigger ansible build.

      This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

      Enable webhooks in your GitHub repository settings.

      Go back to your Github account.

      ![Ansible repo](./Images/Ansible%20repo.png)


       Click on Settings and then Webhooks.


       ![Settings_Webhooks](./Images/Settings_Webhooks.png)

       Add Webhook.

       ![Add Webhooks](./Images/Add%20Webhooks.png)


       You wil be prompted to confirm access.


       ![Confirm Access](./Images/Confirm%20Access.png)


       The screen below will be displayed once you have successfully logged in.


       ![Add Webhooks_1](./Images/Add%20Webhooks_1.png)


        Copy the Jenkins link and paste it under "Payload URL" in Github. Ensure you add github-webhook/ as shown below.

       `http://18.220.231.43:8080/github-webhook/`

       Under Content Type, select application/json

       Leave everything as defaulted with Just the push event and Active selected.


       ![Add Webhook_2](./Images/Add%20Webhook_2.png)


       Click Add Webhook.


       ![Add Webhook_3](./Images/Add%20Webhook_3.png)


       The screen below will be displayed.


       ![Add Webhoo_4](./Images/Add%20Webhook_4.png)


       Go back to jenkins and  click Build Now.

      ![Build Now](./Images/Build%20Now.png)


      It will show a successful Build for # 5.

      ![Buld Now_5](./Images/Build%20Now.png)



5. Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder.


   ![ansible-config-mgt](./Images/ansible-config-mgt.png)

   Click the READ.ME.md file to edit.


   ![README.md file](./Images/README.md%20file.png)


   Click edit on the top right side of the screen to display the screen below.

   ![README.md file_2](./Images/README.md%20file_1.png)

   At the bottom of the file add the statement "Testing the setup of Jenkins in Project 11 by editing the README.md file" . This can be anything .


   ![Edit README.md file](./Images/Edit%20README.md%20file.png)

   Then click "Commit Changes".


   ![Edit README.md file_2](./Images/Edit%20README.md%20file_2.png)


   Go back to jenkins and it will show a green check mark for Build # 7 at the bottom.

   ![Build_7](./Images/Build_7.png)


   Click on # 7

   ![Build_7](./Images/Build_7.png)


   Click on Console Output.

   ![Console Output_7](./Images/Console%20Output_7.png)


   It will show that it finished successfully.


   ![Console Output Success_7](./Images/Console%20Output%20Success_7.png)


   We have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub).

    By default, the artifacts are stored on Jenkins server locally.


    `ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`


    To do this;
    `cd /var/lib`
    `cd jenkins`
    `cd jobs`
    `cd ansible`
    `cd builds`
    `cd 7`
    `cd archive`

    `ll`


    ![Artifacts Storage](./Images/Artifacts%20Storage.png)


    Note: Trigger Jenkins project execution only for /main (master) branch.

    Now your setup will look like this:


    ![Virtual Private Network Updated](./Images/Virtual%20Private%20Network%20Updated.png)

Tip Every time you stop/start your *Jenkins_Ansible* server – you have to reconfigure GitHub webhook to a new IP address. In order to avoid that, it makes sense to allocate an Elastic IP to your *Jenkins_Ansible* server (you have done it before to your LB server in *Project 10*). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.


## Step 2 – Prepare your development environment using Visual Studio Code.

1.  part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an *Integrated development environment (IDE) or Source-code Editor*. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – *Visual Studio Code (VSC)*, you can get it here.

2. After you have successfully installed VSC, configure it to *connect to your newly created GitHub repository*. We already have VSC code installed so we do not have to reinstall it.

   We need to install am extension called *Remote Development*. Go to VS Code and search for *Remote Development*. This helps to SSH and open folders to remote server.

   ![Install Remote Development](./Images/Install%20Remote%20Development.png)


   Click Install.



3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

   git clone <ansible-config-mgt repo link>


   Go to github under your account/ansible-config-mgt.

   ![Ansible-config-mgt clone](./Images/Ansible-config-mgt%20clone.png)

   Open Code (green) and copy the SSH link.

   ![Ansible-config-mgt clone_1](./Images/Ansible-config-mgt%20clone_1.png)

   `git@github.com:MKDKSZAKOLWA/ansible-config-mgt.git`

   In VS Code at the top click on Terminal > New Terminal.

   The screen below will open up under your Workspace.

   ![Ansible-config-mgt_2](./Images/Ansible-config-mgt%20clone_2.png)

   Run the command below. 

   `git clone <ansible-config-mgt repo link>`

   Replace it with your link from github.


   `git clone git@github.com:MKDKSZAKOLWA/ansible-config-mgt.git`

   ![Ansible-config-mgt_3](./Images/Ansible-config-mgt%20clone_3.png)


   The screen below will be displayed showing that the clone has been completed.

   ![Ansible-config-mgt_4](./Images/Ansible-config-mgt%20clone_4.png)

   You can confirm the clone by seeing the repository cloned under Explorer as shown below.

   ![Ansible-config-mgt_5](./Images/Ansible-config-mgt%20clone_5.png)


## BEGIN ANSIBLE DEVELOPMENT

1. In your *ansible-config-mgt* GitHub repository, create a new branch that will be used for development of a new feature.

   ### Tip
   Give your branches descriptive and comprehensive names, for example, if you use Jira or Trello as a project management tool – include ticket number (e.g. *PRJ-145*) in the name of your branch and add a topic and a brief description what this branch is about – *a bugfix, hotfix, feature, release (e.g. feature/prj-145-lvm)*.

   If you closed the terminal in VS Code then open it by Terminal > New Terminal. The screen below will be displayed.

   ![Open Terminal](./Images/Open%20Terminal.png)

   Run the command below to change the directory to *ansible-config-mgt* =.

   `cd ansible-config-mgt`

   ![Directory change](./Images/Directory%20change.png)

   Create a new directory by running the command below. We will call it prj-11.

   `git checkout -b prj-11`


   ![Create a new branch](./Images/Create%20a%20new%20branch.png)

   Run `git push origin prj-11`. This allows for the created branch to be pushed to Github as it is just currently in the local machine.

   ![Push prj-11](./Images/Push%20prj-11.png)


   To confirm the branch run;

   `git branch`

   ![Confirm branch](./Images/Confirm%20branch.png)

   Note: to switch branch you can run the command below. In our case we dont have to at this time.

   `git switch <the branch you want to switch to>`

   Example

   `git switch main`



2. Checkout the newly created feature branch to your local machine and start building your code and directory structure.

3. Create a directory and name it playbooks – it will be used to store all your playbook files. You can perform the step directly from the terminal in VS Code or opening a new terminal from file explorer as explained below.

   Open file explorer

   Manuever to your workspace: 

   Documents > DevOps Training > DK_WORKSPACE

   Right click DK_WORKSPACE and open in Terminal

   `cd ansible-config-mgt`

   The screen below will be displayed.

   ![Change directory](./Images/Change%20directory.png)

   Corfirm your branch.

   `git branch`

   ![Confirm branch_prj-11](./Images/Confirm%20branch_prj-11.png)

   Create playbooks directory

   `cd mkdir playbooks`

   ![Create directory_playbooks](./Images/Create%20directory_playbooks.png)

   The screen below will show the created playbooks directory.

   ![Playbooks directory](./Images/Playbooks%20directory.png)

   

4. Create a directory and name it inventory – it will be used to keep your hosts organised.

   Another way to create a directory is go straight to VS Code under ansible-config-mgt and click on the folder (second box). Name it inventory.

   ![Create inventory directory](./Images/Create%20inventory%20directory.png)


   ![Create inventory directory_1](./Images/Create%20inventory%20directory_1.png)





5. Within the playbooks folder, create your first playbook, and name it common.yml.

   Put your curson on playbooks and create a file (first icon).

   ![Create file common.yml](./Images/Create%20file%20common.yml.png)

   The screen below will be displayed.

   ![Create file common.yml_2](./Images/Create%20file%20common.yml_2.png)



6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

   `cd inventory`

   ![Directory change inventory](./Images/Directory%20change%20inventory.png)

   Run the command below to create inventory files as mentioned above. Touch does not work in Powershel but in Git bash. So open Git bash and navigate to the directory below by running `cd` to each directory.

   /OneDrive/Documents/DevOps Training/DK-Workspace/ansible-config-mgt


   ![Directory Change_1](./Images/Directory%20change_2.png)

   Move to inventory directory.

   `cd inventory`

   ![Directory change inventory_2](./Images/Directory%20change%20inventory_1.png)



   `touch dev.yml staging.yml uat.yml prod.yml`

   ![Create inventory files](./Images/Create%20inventory%20files.png)

   The screen below shows that the inventory files have been created.

   ![Created inventory files](./Images/Created%20inventory%20files.png)




### Step 4 – Set up an Ansible Inventory


   An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

   - Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

   - Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:

   - To learn how to setup SSH agent and connect VS Code to your Jenkins-Ansible instance, please see this video:

     For Windows users – ssh-agent on windows

     https://www.youtube.com/watch?v=OplGrY74qog

     For Linux users – ssh-agent on linux

     https://www.youtube.com/watch?v=OplGrY74qog

     On VS Code click Terminal > New Terminal if you done have it open yet.

     ![Open Terminal_1](./Images/Open%20Terminal_1.png)

     On the right side of the terminal open Git Bash.

     ![Open Git Bash](./Images/Open%20Git%20Bash.png)

     Go back to the inial directory by using CD.

     ![Initial directory](./Images/Initial%20directory.png)
     
     
     CD into downloads to confirm the location where the pem key (PBL_All_Projects.pem).

     Run `ls`

     ![Contents of downloads](./Images/Contents%20of%20downloads.png)


     Run the code below. You wiill need to repeat the steps below if you turned off your systems.

     ```
     eval `ssh-agent -s`
     ```

     It will show the agent PID

     ![ssh agent](./Images/ssh%20agent.png)
   
     

     Now you need to add the private key.

     ` ssh-add <path-to-private-key>`

     Run;

     `ssh-add PBL_All_Projects.pem`


     ![Add private key](./Images/Add%20private%20key.png)


     Confirm the key has been added with the command below, you should see the name of your key

      `ssh-add -l`

     ![Confirm added key](./Images/Confirm%20added%20key.png)

     Now, ssh into your Jenkins-Ansible server using ssh-agent. If connecting for the forst time click Yes when prompted.


     Run;

     `ssh -A ubuntu@public IP address for jenkins-ansible`

     `ssh -A ubuntu@3.134.112.165`

     ![Connect to Jenkins Ansible Server](./Images/Connect%20to%20Jenkins%20Ansible%20Server.png)

     We will be able to connect to the Jenkins Ansible server through any instances that we would create as long as the same pem key is used.

- Create the ec2 servers below in AWS.

   - 1 NFS Server - RHEL(Red Hat)
   - 2 Web Servers - RHEL(Red Hat)
   - 1 Database Server -RHEL(Red Hat)
   - 1 Load Balancer Server - Ubuntu

- You can create 4 EC2 servers (RHEL) all at once and rename them as shown below. 

   ![Created Servers](./Images/Created%20servers.png)


   For Security group, you can open SSH port 22, HTTP port 80 and Custom TCP port 8080. I created Project 11 Security group and then added it to the 4 servers.


   ![Project 11 Security Group](./Images/Project%2011%20Security%20Group.png)


   Create ubuntu server which is the load balancer. Select Project 11 Security Group.


   ![All Servers Created](./Images/All%20Servers%20Created.png)



- Lets make sure that Jenkins_Ansible server will have access to the Load balancer and the NFS servers we have just created.

   Get the Private IP for the Load Balancer.

   172.31.35.198

   Run the command below;

   `ssh ubuntu@172.31.35.198`

   ![Connect Load Balancer to Jenkins_Ansible](./Images/Connect%20Load%20Balancer%20to%20Jenkins_Ansible.png)

   Exit and clear.

   ![Exit](./Images/Exit.png)


   Repeat the same process with the NFS server.


   Get the Private IP for the NFS server and then run the command below.

   172.31.42.206

   `ssh ec2-user@172.31.42.206`

   ![Connect NFS Jenkins_Ansible](./Images/Connect%20NFS%20to%20Jenkins_Ansible.png)

   Exit and clear.

   ![Exit_1](./Images/Exit_1.png)



- Also notice, that your Load Balancer user is ubuntu and user for  RHEL-based servers is ec2-user.


  ![Ubuntu user](./Images/Ubuntu%20user.png)

  ![ec2 user](./Images/ec2%20user.png)

  Update your inventory/dev.yml file with this snippet of code:

  ```
  [nfs]
  <NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

  [webservers]
  <Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
  <Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

  [db]
  <Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

  [lb]
  <Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
  ```

  Click on dev.yml

  ![dev.yml](./Images/dev.yml.png)

  Copy and paste the code provided above.

  ![Code_1](./Images/Code_1.png)

  Get the Private IP addresses and paste them as shown below.


  ![Updated inventry dev.yml file](./Images/Updated%20inventry%20dev.yml%20file.png)


  The reason why we have ansible_ssh user is because we need to tell ansible which user would access which host.


### CREATE A COMMON PLAYBOOK

   - Step 5 – Create a Common Playbook
   It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in *inventory/dev.*

   - In *common.yml* playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

   - Update your *playbooks/common.yml* file with following code:

     ```
     ---
     name: update web, nfs and db servers
     hosts: webservers, nfs, db
     remote_user: ec2-user
     become: yes
     become_user: root
     tasks:
      - name: ensure wireshark is at the latest version
         yum:
           name: wireshark
           state: latest

     - name: update LB server
       hosts: lb
       remote_user: ubuntu
       become: yes
       become_user: root
       tasks:
         - name: Update apt repo
         apt: 
           update_cache: yes

       - name: ensure wireshark is at the latest version
         apt:
           name: wireshark
           state: latest
     ```

   
   
     ![common.yml](./Images/common.yml.png)

- The servers are grouped separately. We have 4 redhat servers that are grouped together and one ubuntu grouped separately.

- Note that we are using *yum* for redhat installion and *apt* for ubuntu.

- Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

- Feel free to update this playbook with following tasks:

   - Create a directory and a file inside it
   - Change timezone on all servers
   - Run some shell script

   For a better understanding of Ansible playbooks – watch this video from RedHat and read this article.

   https://www.youtube.com/watch?v=ZAdJ7CdN7DY&feature=youtu.be

   https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook


- Step 6 – Update GIT with the latest code
Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

- In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes – it is also called "Four eyes principle".

- Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

Confirm the branch that you are working on.

`cd ansible-config-mgt`

![Directory change ansible_config](./Images/Directory%20change_ansible_config.png)

`git branch`

It will show that you are working on prh-11.

![Branch prj-11](./Images/Branch%20prj-11.png)

Make sure you are inventory directory. If you are not then;

`cd inventory`

![Directory change inventory1](./Images/Directory%20change_inventory2.png)


  Commit your code into GitHub:

  1. use git commands to add, commit and push your branch to GitHub.


     `git status`

     ![Git status](./Images/Git%20status.png)

     `git add .`

     ![Git add](./Images/Git%20add.png)



     `git commit -m "Made changes to the code"`

     ![Git commit](./Images/Git%20commit.png)


     Now add the playbooks;

     `git add ../playbooks/`

     
     `git commit -m "Added playbooks folder"`

     ![Git add Git commit](./Images/Git%20add%20Git%20commit.png)


     Run the command below to push the changes to the main branch.

     `git push origin prj-11`

     ![Git push](./Images/Git%20push.png)





   2. Create a Pull request (PR)

      Go to your github. It will show prj-11 has some recent changes.

      ![Code changes_1](./Images/Code%20Changes_1.png)

      Click compare & pull request.

      ![Code changes_1](./Images/Code%20Changes_1.png)

      The screen below will be displayed.

      ![Code Changes_2](./Images/Code%20Changes_2.png)

      Click Create a pull request.

      ![Code Changes_2](./Images/Code%20Changes_2.png)

      The screen below will be displayed.


      ![Code Changes_3](./Images/Code%20Changes_3.png)


      Click Mearge pull request.

      ![Code Changes_3](./Images/Code%20Changes_3.png)


      Confirm merge.

      ![Code Changes_4](./Images/Code%20Changes_4.png)


      It will show that the pull request has been successfully merged and closed.

      ![Code Changes_5](./Images/Code%20Changes_5.png)

      Click Code on the top left side and the screen below will be displayed.

      ![Code Changes_6](./Images/Code%20Changes_6.png)

      You can confirm that the files you created are there under inventory and playbooks repectively.

      ![Code Changes_7](./Images/Code%20Changes_7.png)


      ![Code Changes_8](./Images/Code%20Changes_8.png)

      When i was doing the project  `git pull` did not properly work so I performed the steps below;

         - Run `git branch` to confirm the branch that you are working on. 
         - If you are on prj-11 and not main then run the command below to ensure that you are on main. make sure that in ansible-config-mgt directory.

           `git checkout main`

           ![git checkout main_1](./Images/git%20checkout%20main_1.png)

           Run `git pull`

           ![git pull](./Images/git%20pull.png)

           ![git pull](./Images/git%20pull_1.png)



   3. Wear a hat of another developer for a second, and act as a reviewer.

   4. If the reviewer is happy with your new feature development, merge the code to the master branch.

      `git checkout main`

      ![Git checkout main](./Images/Git%20checkout%20main.png)


   5. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.


      Login to Jenkins and confirm that the latest build was completed successfully.

      ![Build 13](./Images/Build%2013.png)

      You can click view under dev .

      ![Build 13 view dev](./Images/Build%2013%20view%20dev.png)

      Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to */var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/* directory on *Jenkins-Ansible server.*


      Go back to the git bash terminal and run  the command below. Remember our last build was # 13.

      sudo ls /var/lib/jenkins/jobs/ansible/builds/13/archive/

      It will confirm that the folders are there.

      ![Inventory playbooks](./Images/Inventory%20playbooks.png)


### RUN FIRST ANSIBLE TEST

   ### Step 7 – Run first Ansible test

   - Now, it is time to execute *ansible-playbook* command and verify if your playbook actually works:

   - One you have executed the command (sudo ls /var/lib/jenkins/jobs/ansible/builds/13/archive/), make sure you stay in the same directory.

   - Run the command below to execute ansible test.


     `ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/playbooks/common.yml`

      Replace your build number and the run the command below.

     `ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/13/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/13/archive/playbooks/common.yml`


     ![Run Ansible Test](./Images/Run%20Ansible%20Test.png)




   - You can go to each of the servers and check if wireshark has been installed by running *which wireshark* or *wireshark  --version*

     I logged through the terminal for Web Server 1 and ran `wireshark --version`. The screen below shows that wireshark version 3.4.10 has been installed. This should be the same for all the rest of the servers.

     ![Wireshark installed_web 1](./Images/Wireshark%20installed_web%201.png)


     Your updated with Ansible architecture now looks like this:


     ![Updated Architecture](./Images/Updated%20Architecture.png)

     Complete




























     



















