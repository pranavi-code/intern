4 questions 25 each
Jenkins
pre-requisites
1)local host 8080--tomcat //when u go in this u should know userid and pass
2)plugins section 
	email-extension
3)tools and configuration
	maven, git, jdk
Questions:
1) two pipeline
2) 3 pipeline
3) build triggers -- webhooks
4) email-notification
5) scripted pipeline

what to do:
install ngrok
sign up
open cmd of ngrok
in that page there will be ngrok config copy that and paste in cmd
ngrok .exe http 8080

6)Kubernetes---minikube
--------------------------------------------------------------------------
pre-requisites
docker- desktop should be running
u should have docker image

cmds
	minikube version
	minikube status
	kubectl create deployment mynginx --image=nginx
	minikube status
	kubectl get deployment
	kubectl get pods
	kubectl describe pods
	kubectl scale deployment mynginx --replicas=4
	kubectl get deployment
	kubectl get pods
	kubectl describe pod <pod-name>   # copy–paste a pod name
	kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80 --name=mynginx
	kubectl get service mynginx
	kubectl port-forward service/mynginx 30088:80
	http://localhost:30088
	minikube service mynginx --url
	minikube dashboard
	kubectl delete deployment mynginx
	kubectl delete service mynginx
	minikube stop




7)nagios
-------------------------------------------------------------------
docker pull jasonrivers/nagios:latest 
docker run --name nagios4 -p 8090:80 -d jasonrivers/nagios:latest
docker ps
http://localhost:8090/nagios


8)aws
-----------------------------------------------------------------------
 create ec2 instance
 connect it
 cmds:
	docker --version	
	sudo su //it moves to admin
	sudo apt-get update
	sudo apt-get install docker.io
	sudo docker images
	sudo docker ps
	git clone url.git
	ls
	cd that folder
	sudo docker build -t img1 . 
	sudo docker images
	sudo docker run -d -p 80:8080 img1

9)UML-- use case diagram
	class
	sequence
	component
case study-- 	online reservation 
		hospital management
		book banking systems	
each set will be having one diagram 






email
-------------------------------------------------------------------
install email extension in plugins
manage account security 2 step verification
app pass-Jenkins generate
in jenkins- in tools:
configure system
last email- smtp.gmail.com
advanced- username- gmail
	  pass- generated one
use ssl
port 465
click on test
past gmail username 
click test configuration







script:
----------------------------------------------------------------
pipeline { 
agent any 
tools{ 
maven 'MAVEN-HOME' 
} 
stages { 
stage('git repo & clean') { 
steps { 
//bat "rmdir  /s /q mavenjava" 
bat "git clone provide your github link" 
bat "mvn clean -f mavenjava" 
} 
} 
stage('install') { 
steps { 
bat "mvn install -f mavenjava" #project name# 
} 
} 
stage('test') { 
steps { 
bat "mvn test -f mavenjava" 
} 
} 
stage('package') { 
steps { 
bat "mvn package -f mavenjava" 
} 
} 
} 
} 



ORRRRRRRR

pipeline {
    agent any
    tools{
        maven 'MAVEN-HOME'
    }
    stages {
        stage('git repo & clean') {
            steps {
                //bat "rmdir  /s /q mavenjava"
                bat "git clone https://github.com/savram674/MavenJavaDemo.git"
                bat "mvn clean -f MavenJavaDemo"
            }
        }
        stage('install') {
            steps {
                bat "mvn install -f MavenJavaDemo"
            }
        }
        stage('test') {
            steps {
                bat "mvn test -f MavenJavaDemo"
            }
        }
        stage('package') {
            steps {
                bat "mvn package -f MavenJavaDemo"
            }
        }
    }
}


Script harshith:
---------------------------------------------------------
pipeline{
  agent any
  tools{
    jdk 'JAVA_HOME'
    maven 'MAVEN_HOME'
  }
  stages{
    stage('git repo and clean'){
      steps{
        sh 'rm -rf java_se_lab || true'
        sh 'git clone https://github.com/harshith-kulkarni/java_se_lab'
        echo 'cloned repo'
      }
    }
    stage('clean'){
      steps{
        dir('java_se_lab'){
          sh 'mvn clean compile'
        }
        echo 'compiled'
      }
    }
    stage('test'){
      steps{
        dir('java_se_lab'){
          sh 'mvn test'
        }
        echo 'tested'
      }
    }
    stage('package'){
      steps{
        dir('java_se_lab'){
          sh 'mvn package'
          archiveArtifacts 'target/*.jar'  idid kuda push chey ra anduloed'
      }
    }
  chey ra anduloalways{ cleanWs() }
    success{ echo 'build done' }
    failure{ echo 'build failed' }
  }
}




https://github.com/savram674/MavenJavaDemo.git
https://github.com/archanareddyse/mavenweb.git





webhook build triggers:
-------------------------------------------
ngrok http 8080
copy .dev link
Go to your GitHub repository. 
2. Navigate to Settings → Webhooks. 
3. Click “Add webhook”. 
4. In the Payload URL field: 
o Enter the Jenkins webhook 
Add url example-------<https://unhired-stormily-alaine.ngrok-free.dev/github-webhook/>

Open Jenkins Dashboard. 
2. Select the job (freestyle or pipeline) you’ve already created.
3. Click Configure. 
4. Scroll down to the Build Triggers section. 
5. Check the box: ✅GitHub hook trigger for GITScm polling

1. Make any code update in your local repo and push it to GitHub. 

git add .
git commit -m "testing github webhook"
git push origin main

2. Once pushed, GitHub will trigger the webhook. 
3. Jenkins will automatically detect the change and start the build pipeline.









jenkins deployment using tomcat:
----------------------------------------------------------------
└── Step 1: Open Jenkins (localhost:8080) 
├── Click on "New Item" (left side menu) 
└── Step 2: Create Freestyle Project (e.g., MavenWeb_Build) 
├── Enter project name (e.g., MavenWeb_Build) 
├── Click "OK"
└── Configure the project: 
├── Description: "Web Build demo" 
├── Source Code Management: 
└── Git repository URL: [GitMavenWeb repo URL] 
├── Branches to build: */Main or master
└── Build Steps: 
├── Add Build Step -> "Invoke top-level Maven targets" 
└── Maven version: MAVEN_HOME 
└── Goals: clean 
├── Add Build Step -> "Invoke top-level Maven targets" 
└── Maven version: MAVEN_HOME 
└── Goals: install
└── Post-build Actions: 
├── Add Post Build Action -> "Archive the artifacts" 
└── Files to archive: **/* 
├── Add Post Build Action -> "Build other projects" 
└── Projects to build: MavenWeb_Test 
└── Trigger: Only if build is stable 
└── Apply and Save
└── Step 3: Create Freestyle Project (e.g., MavenWeb_Test) 
├── Enter project name (e.g., MavenWeb_Test) 
├── Click "OK"
└── Configure the project: 
├── Description: "Test demo" 
├── Build Environment: 
└── Check: "Delete the workspace before build starts" 
├── Add Build Step -> "Copy artifacts from another project" 
└── Project name: MavenWeb_Build 
└── Build: Stable build only 
└── Artifacts to copy: **/*
├── Add Build Step -> "Invoke top-level Maven targets" 
└── Maven version: MAVEN_HOME 
└── Goals: test 
└── Post-build Actions: 
├── Add Post Build Action -> "Archive the artifacts" 
└── Files to archive: **/*
├── Add Post Build Action -> "Build other projects" 
└── Projects to build: MavenWeb_Deploy 
└── Apply and Save 
└── Step 4: Create Freestyle Project (e.g., MavenWeb_Deploy) 
├── Enter project name (e.g., MavenWeb_Deploy) 
├── Click "OK"
└── Configure the project: 
├── Description: "Web Code Deployment" 
├── Build Environment: 
└── Check: "Delete the workspace before build starts" 
├── Add Build Step -> "Copy artifacts from another project" 
└── Project name: MavenWeb_Test 
└── Build: Stable build only 
└── Artifacts to copy: **/*
└── Post-build Actions: 
├── Add Post Build Action -> "Deploy WAR/EAR to a container" 
└── WAR/EAR File: **/*.war 
└── Context path: Webpath 
└── Add container -> Tomcat 9.x remote 
└── Credentials: Username: admin, Password: 1234 
── Tomcat URL: https://localhost:8085/
└── Apply and Save

└── Step 5: Create Pipeline View for MavenWeb 
├── Click "+" beside "All" on the dashboard 
├── Enter name: MavenWeb_Pipeline 
├── Select "Build pipeline view"
└── Pipeline Flow: 
├── Layout: Based on upstream/downstream relationship 
├── Initial job: MavenWeb_Build 
└── Apply and Save

└── Step 6: Run the Pipeline and Check Output 
├── Click on the trigger “RUN” to run the pipeline 
Note:  
1. After Click on Run -> click on the small black box to open the console to check if the build is success 
2. Now we see all the build has success if it appears in green color


extended email
copy artifact
deploy to
build pipeline
pipeline stage view
maven


------------------------------------------------------------------------------------------
multi module

1. Creating a Multi-Module Maven Project (Parent Project)
Step 1: Create a new Maven Project

Open File → New → Other → Maven → Maven Project

Click Next

Select Create a simple project (skip archetype selection)

Click Next

Step 2: Fill Parent Project Details
Group ID: KMIT
Artifact ID: MultiModule
Packaging: pom
Name: MultiModule Creation
Description: Sample multi-module project with parent-child hierarchy

This creates the parent project containing the main pom.xml.

2. Creating Child Module 1 (JAR Module)
Steps

Right-click on the parent project → New → Maven Module

Enable Create simple project

Click Next

Module Details
Module Name: KMultimodule
Parent Project: MultiModule
Group ID: KMIT
Packaging: jar
Name: Simple MultiModule
Description: Basic JAR module

This generates child module 1 with its own pom.xml.

3. Creating Child Module 2 (Web Module)
Steps

Right-click on the parent project → New → Maven Module

Click Next

Module Details

Module Name: KMultiModule1
Parent Project: MultiModule
Choose maven-archetype-webapp (version 1.1 or 1.5)
Click Next
Group ID: KMIT
Package: org.KMultiModule1
Select Run archetype

Click Finish

If console asks for confirmation → type Y

This creates a web application module with WAR packaging.

4. Adding Dependency (Child2 depends on Child1)

In child module 2's pom.xml, add:

<dependency>
    <groupId>KMIT</groupId>
    <artifactId>KMultimodule</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>


This allows the web module to use classes from the JAR module.

5. Building Modules
A. Build the JAR Module

Right-click on Child 1 (KMultimodule)

Select Run As → Maven Build

Goal: package

Run

B. Build the WAR Module

Ensure folder structure exists:

src/main/webapp/WEB-INF/web.xml


In child module 2 pom.xml, ensure:

<packaging>war</packaging>


Run mvn package on the module.








-----------------------------------------------------------
COLLABORATIVE

1. Creating and Configuring a GitHub Organization
1.1 Create a New Organization

Go to GitHub → New Organization

Choose Create a free organization

Fill required fields:

Organization name

Email

(Billing info if needed)

Click Next to complete setup.

1.2 Configure Organization Permissions

Navigate to:
Organization → Settings → Member privileges

Set the following:

Base Permissions

Write

Project Permissions

Projects base permissions: Write

1.3 Create a Repository in the Organization

Go to the Organization

Click New Repository

Enter:

Repository name

Description (optional)

Choose:

Private (recommended for internal projects)

or Public

Click Create Repository

1.4 Add Members or Collaborators

You can add people at org or repo level:

A. Add Members to Organization

Org → People → Invite member

B. Add Access to Specific Repo

Repo → Settings → Manage access → Invite teams or people

1.5 (Optional) Create a Dedicated Deployment User

Useful for CI/CD or Tomcat deployment

Not required for general GitHub workflow

2. Collaborator Workflow (What Developers Do)

Prerequisite: User accepted the invite and has Git installed.

2.1 Configure Git (One-time Setup)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

2.2 Clone the Repository
git clone https://github.com/<org-or-username>/<repo-name>.git
cd <repo-name>

2.3 Create a Feature Branch (Never Work on Main)
git checkout -b feature/your-feature-name

2.4 Make Changes → Stage → Commit
# edit files
git add .
git commit -m "Short, descriptive message explaining what & why"

2.5 Push Branch to GitHub
git push origin feature/your-feature-name

2.6 Open a Pull Request (PR)

GitHub → Repository → Pull Requests → New Pull Request

Select:

Source: feature/your-feature-name

Target: main

Add:

Title

Description

Click Create Pull Request

2.7 Keep Feature Branch Updated with Latest Main
git checkout main
git pull origin main

git checkout feature/your-feature-name
git merge main


Resolve conflicts if any → then:

git add .
git commit -m "Merge main into feature branch"
git push origin feature/your-feature-name

3. Owner / Maintainer Workflow (Reviewing & Merging PRs)

Review code on GitHub (add comments as needed)

Approve PR → Click Merge pull request

Confirm merge

Delete the feature branch on GitHub (button appears after merge)

Local cleanup
git checkout main
git pull origin main
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name

4. Forking Workflow (For External Public Repos)

Use this when you are not a collaborator on the original repo.

4.1 Fork the Repository

Go to the repo → Click Fork

A copy is created under your GitHub account.

4.2 Clone Your Fork
git clone https://github.com/your-username/awesome-project.git
cd awesome-project

4.3 Create Branch → Commit → Push
git checkout -b feature/your-change
git add .
git commit -m "Explain what you changed"
git push origin feature/your-change

4.4 Create Pull Request to Original Repo

Your fork → Pull Request
Target: Person1/awesome-project → Branch: main

4.5 Sync Your Fork With Upstream Repo
Add upstream remote (one-time)
git remote add upstream https://github.com/Person1/awesome-project.git

Sync
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

5. Resolving Merge Conflicts
5.1 Attempt Merge
git checkout main
git pull origin main

git checkout feature/your-feature
git merge main


If conflicts occur → Git shows which files need attention.

5.2 Conflict Markers Look Like:
<<<<<<< HEAD
your current branch content
=======
incoming branch content
>>>>>>> feature/other-branch


Edit the file → keep correct content → remove markers.

5.3 Mark As Resolved
git add conflicted-file.txt
git commit      # completes merge
git push origin feature/your-feature

To abort merge:
git merge --abort

6. Creating and Applying Patch Files
6.1 Create Patch For Single Commit
git format-patch -1 <commit-hash>
# file example: 0001-Your-commit-message.patch

6.2 Create Patch For Last N Commits
git format-patch -3

6.3 Maintainer Applies Patch

For patch from format-patch:

git am 0001-Your-commit-message.patch


For a simple .patch file:

git apply my-changes.patch
git add .
git commit -m "Apply patch: description"

------------------------------------------------------------------------------------------------------
DOCKER CLI

docker --version
docker pull ubuntu:latest
docker run -it -p 9090:80 --name myubuntu1 ubuntu
apt update
ls
cd usr
ls
cd share
apt install nginx -y
ls
cd nginx
ls
cd html
ls
nano index.html ctrl - x
nginx
localhost:9090

-------------------------------------------------------------------------------------------------
GIT

STEP 10 — Check Status
git status

STEP 11 — Create a new file and add it
echo "Hello" > test1.txt
git add test1.txt
git commit -m "Added test1 file"

STEP 12 — Check commit history
git log --oneline

STEP 13 — Create and switch to new branch
git branch feature-branch
git branch
git checkout feature-branch
# OR
# git switch feature-branch

STEP 14 — Make changes in this branch
echo "Feature work" > feature.txt
git add .
git commit -m "Feature work added"

STEP 15 — Push new branch
git push --set-upstream origin feature-branch

STEP 16 — View remote branches
git branch -a
git branch -r

STEP 17 — Fetch updates (no merge)
git fetch origin

STEP 18 — Switch back to main
git checkout main
# or
git switch main

STEP 19 — Pull latest changes
git pull origin main

STEP 20 — Merge feature branch into main
git merge feature-branch

STEP 21 — See merged branches
git branch --merged

STEP 22 — Delete the feature branch
git branch -d feature-branch

STEP 23 — Rename remote
git remote rename origin upstream
git remote -v
git remote rename upstream origin

STEP 24 — Change remote URL
git remote set-url origin https://github.com/yourusername/new-repo.git
git remote -v

STEP 25 — Remove a remote
git remote remove origin


Add back:

git remote add origin https://github.com/yourusername/your-private-repo.git

STEP 26 — Undo staging
git reset test1.txt

STEP 27 — Restore file before staging
git restore test1.txt

STEP 28 — Show commit details
git show HEAD

STEP 29 — Show differences

Before staging:

git diff


After staging:

git diff --staged

STEP 30 — Simulate Merge Conflict

Create 2 branches:

git checkout -b conflict-1
echo "AAA" > conflict.txt
git add .
git commit -m "AAA added"
git push -u origin conflict-1


Another branch:

git checkout main
git checkout -b conflict-2
echo "BBB" > conflict.txt
git add .
git commit -m "BBB added"
git push -u origin conflict-2


Merge:

git checkout conflict-1
git merge conflict-2

NOTEPAD CONFLICT.TXT
You will get conflict markers.

Resolve manually:

<<<<<<< HEAD
AAA
=======
BBB
>>>>>>> conflict-2


Then:

git add conflict.txt
git commit


📸 Screenshot of conflict markers

STEP 31 — Use git blame
git blame conflict.txt

STEP 32 — Use stash
echo "Temporary" > temp.txt
git stash
git stash list
git stash apply

STEP 33 — Delete multiple branches
git branch -D conflict-1 conflict-2

STEP 34 — Reflog (recover lost commit or branch)
git reflog


Recover:

git checkout -b recovered-branch <commit_hash>
