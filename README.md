sudo apt install nodejs sudo apt install npm

npm install

node app.js




                       ##CICD_TODO_App jusing AWS, Jenkins and Docker.

1. Instance Created on AWS and connected on Ubuntu with public ip of the same ec2 instance.
2. After the connection, java is installed first as Jenkins is built on java then Jenkins is installed.
3. Then, Jenkins is started, enabled and status was checked and it was running successfully. cmd: sudo systemctl start/enable/status
4. As jenkins run on PORT:8080 ---> AWS --> Security Groups --> Inbound Rule --> PORT 8080 --> anywhere with IPV4 enabled.
5. Again enable PORT:8000 as the application will run on this port: ---> AWS --> Security Groups --> Inbound Rule --> PORT 8000 --> anywhere with IPV4 enabled.

                                    ##Jenkins
Error: While installing plugins for Jenkins - faced plugin dependency cache already corruped error.

Actions to resolve this issue: i. Restarted the Jenkins but it doesn't work as the restart only fixes service issues.
                               ii.  Cache cleanup fixes dependency/ installation issues ---so firstly stopped jenkins --> removed broken plugin cache ---> started jenkins --> then only installed suggested plugins such as pipeline, Git, Github, Credentials, SSH, Workspace Cleanup.

Installed git, docker --> and Jenkins added to docker group using - sudo usermod -aG dokcer Jenkins



                                ##Creating Jobs to Jenkins

a. Clicked on add new item (MyFirst CICD Todo App)
b. Select an item type: Freestyle
c. OK
d. Source Code Management --> Repo --> Credentials --> SSH username with private key --> ssh-keygen
e. cd ssh --> ls --> authorized keys : id_ed25519 and id_ed25519.pub
f. Public key should be pasted in Github --> Settings > SSH and GPG key --> New SSH key, private should be paste in jenkins.
g. Go to the path like : cd /var/lib/jenkins/workspace/MyFirst CICD TodoApp
h. give command: node app.js --> As a result, the application will run on port 8000.
But, when we exit - the app will not run --> This is where the Docker comes into the picture.



                                   ##DOCKER
The workflow is: Dockerfile ---> Docker image ---> Container ---> run applications.

Dockerfile:- It contains instructions for build docker image such as Specifying base image, copying files
Docker Image:-
Container:

cmd to build image: docker build -t <image_name>:>tag> .  OR docker build -t <image_name>:<tag> -f <directoryname> .  

- build means initiate the version , . represents the current directory lai build context ko lagi use garne (mostly indicate path or directory where the  dockerfile is ) and also build context means yo directory wa folder ma rahe ko sab kura lai liyear docker engine or docker dameon lai pathau build ko lagi.

                    - Once the build is complete:
  docker ps --> shows all running containers
  docker ps -a --> shows all running and stopped container as well.
  docker run -d -p 8000:8000 todo-cicd
  where -d: detached mode (run in background)
        -p: port mapping , 8000(left):8000(right) --> left represents our system or ec2 or local machine where as right one represents inside the container

  Result: This time also app  run successfully  and we can easily add our work in our todo app. But, the interesting part is - this time app doesn't closed or stopped running even with exit the path of workspace in ec2. The app runs continuously.


  Till now, we are doing the things manually like pushing the code to github, the building , creating docker image and deploying the app in container manually. But now, we will create the webhooks in the github and it will trigger the build whenever the code is pushed to github. the build will trigger the docker image and the app will run continuously.

                                       ##Automatic Jenkins

  1. For that first we removed the created container ; docker rm -f <container_id>
  2. Webhook created on Github and the header of todo.ejs is changed.
  3. As the change is commited - it triggerd jenkins for build ---> docker image > App run succesfully.


###SUMMARY
1. firstly, on jenkins console we manually started the build after the code is pushed and run the app but the app has stopped running when we exit the path.
2. Secondly, docker make the app run continuously but we went through manual docker image build and container creation.
3. Lastly, we created the webhook of Jenkins with Github which automated all the build process and container creation. That makes the TODO APP run continuously.
  
