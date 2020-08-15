# MLops_DevOpsAL_Task2

## Task:

1. Create a container image that’s has Jenkins installed using DockerFile.

2. When we launch this image, it should automatically start the Jenkins service in the container.

3. Create a job chain of job1, job2, job3, and job4 using the build pipeline plugin in Jenkins.

4. Job1: Pull the Github repo automatically when some developers push the repo to Github.

5. Job2: By looking at the code or program file, Jenkins should automatically start the respective language interpreter install image container to deploy code ( eg. If code is of PHP, then Jenkins should start the container that has PHP already installed ).

6. Job3: Test your app if it is working or not. If the app is not working, then send an email to the developer with error messages.

7. Job4: If the container where the app is running. fails due to any reason then this job should automatically start the container again.

## Pre-requisite :

* OS: Base OS is Windows 10. Server OS is RedHat Enterprise Linux 8 (RHEL8) in Virtual Box.

* In RHEL8 some of the software needed is Docker (also need the PHP image downloaded in it), Jenkins (also Github, build pipeline and email extension plugin should be installed in it).

* In Windows, we need git bash software.

* At the start, stop the firewalld in RHEL8 and start the docker and Jenkins services.

## Performing the task!

***Step 1:*** Creating Dockerfile and Building Personal Image using it:

Here I create one Dockerfile and run the build command. Remember one thing you have to run this command particularly on that folder where your Dockerfile exits.

Here we have three important noticeable points:

* FROM: the image to be used for container

* RUN: commands to be executed while building the modified container

* CMD: the command used here will keep the Jenkins live till the container is on and will start on container boot.

We will be installing wget for downloading the files from URL, and before moving further we will set up the yum repository for Jenkins from URLs using wget command. Then we will first install java as it is a prerequisite for the smooth functioning of Jenkins.

We will make a Jenkins entry in the sudoers file to give it root powers without the requirement for any password. We will also require Git installed to be used in Jenkins as a plugin.

Here is the Dockerfile :

Here is the command to build. To build the image we will use: ** . is to be replaced with the path of Dockerfile

```
docker build -t taskjenkins:v1 .

```
***Step 2:*** Running the container and collecting needed pieces of information:

Here is the command of docker run : 

```
docker run -it --privileged -P -v /:/host --name task taskjenkins:v1
```

Here we have three important noticeable points:

* "-P" means it will go inside the image's configuration and will look for which port is exposed. It will then allocate one random port and link that with that exposed port.
* "-v /:/host " this command will attach our base os absolute directory to the container.
* " -- privileged " this will allow our docker to go inside base os and from the inside container, we will be able to run any commands in base os.

Now observe these two below mentioned images :

Here you will find the initial password of Jenkins. This is needed to unlock Jenkins for the 1st time.
Now type your IP address along with the port number and then copy-paste the initial password on the starting page of Jenkins.

***Step 3:*** Jenkins Job1:

Go to the 'configure' of your job and fill these data. First, give the URL of your project repository. Next in SCM select git and fill the repo URL and select the branch to master.

And in build trigger just select Build Trigger Remotely and give a desired token name. This will help us to trigger Jenkins as soon as we push our code to GitHub remotely.

Next, this is the code which gonna help to copy our files from Jenkins Workspace to our desired folder.

***Step 4:*** Jenkins Job2:

In this job, we are deploying our code. Here we select one different trigger. Means as soon as job1 will complete job2 will start automatically. Also, this job at first check if in our folder have any PHP or HTML code exist or not. If yes then it will start the container accordingly.

**chroot** command in the Linux system is used to change the root directory. Every process/command in Linux like systems has a current working directory called the root directory. It changes the root directory for currently running processes as well as its child processes.

A process/command that runs in such a modified environment cannot access files outside the root directory. This modified environment is known as “chroot jail” or “jailed directory”. Some root users and privileged processes are allowed to use the chroot command.

**“chroot”** command can be very useful:

* To create a test environment.
* To recover the system or password.
* To reinstall the bootloader.
* So firstly I have created a new environment using chroot command. Then checked whether the file is of HTML or PHP then accordingly launched the container.

***Step 5:*** Jenkins Email Configuration:

Goto The Manage Jenkins, then Configuration and scroll down and fill up the email notification portion.

***Step 6:*** Jenkins Job3:

Here again, use the previous trigger. This code will check if our PHP or HTML code is working fine or not.

So firstly I have stored the status code of my webpage in the status variable. Generally, status code 200 represents that the website is running fine. So If status == 200 then the webpage is running absolutely fine and the job will terminate with exit status 0 (Success), else it will terminate with exit status 1 (Fail).

If our PHP or HTML code has any error then it will send a notification to the developer. Follow the below picture.

***Step 7:*** Jenkins Job4:
This job will keep on checking each minute if my docker container is running or not. These 5 start means each minute this job will automatically trigger itself.

***Step 8:*** Creating Build Pipeline View:
Just simply create one new My view in Jenkins and select Build Pipeline. Then at the end only mention job1 and you are done.

# Testing Time!

