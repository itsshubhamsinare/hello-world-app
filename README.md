Microservice architected services

This is project of creating a simple infrastructure using Docker container, Ansible, Jenkins and AWS Cloud Provider.
Link to access Web-Application: http://34.248.174.139:8090/webapp/ 
Link to access GitHub: https://github.com/itsshubhamsinare/hello-world-app.git

Brief Description:
1.	Make sure that Jenkins and Tomcat server is installed on AWS ec2 instance.
2.	Clone Git repository with Jenkins, as when any developer makes changes to application, the Jenkins will take and build that code and create .war file.
3.	After creating .war file Jenkins will pass it to Ansible.
4.	Ansible will build the image and push it into DockerHub.
5.	Docker node will take image and build docker container and display latest web application to end-user.
Prerequisites: 
Apache Tomcat Server, Jenkins, Ansible, Docker Container.
How to create infrastructure:
1.	Deploy Jenkins, Tomcat, Ansible and Docker on ec2-instance.
2.	Login into Jenkins console.
3.	Make sure to install Maven plugin before build. 
4.	Create Jenkins Job as follows:
1.	In source code Management add repository as: https://github.com/itsshubhamsinare/hello-world-app.git 
2.	Branches to Build: */master
3.	Build Triggers -> Poll SCM -> Schedule: */2 * * * *
4.	Goals and options: clean install package, 
5.	Post steps: In steps or execute commands over SSH execute as follows:
-Name: ansible-server
-Source Files: webapp/target/*.war
-Remote directory: //opt//docker
6.	Send files or execute commans over SSH:
-Name: ansible-server
-Source files: Dockerfile
-Remote directory: //opt//docker
-Exec Command:
      - cd/opt/docker
      - docker build -t name.demo .
      - docker tag namo/namo.demo
      - docker push name/name.demo
      - docker rmi name/name.demo

5.	Integrate Apache Tomcat server with Jenkins and make sure to set default port to :8090, as Jenkins uses port :8080.
6.	Test the build whether working properly.
7.	After build is successfully completed, integrate it with Ansible server.
8.	After whole setup is completed, ensure whole CI/CD pipeline is working properly and final changes and visible on web-application.
9.	After ensuring whole process, integrate Docker with Ansible as Ansible will build the Image and push to DockerHub.
10.	Then, Docker will pull the image and start building Docker container.
11.	The latest container will be successfully deployed and new build will be generated on Jenkins after 4mins time and latest web-application will be visible.
 
Sample .json script to copy .war file on Tomcat server: 
[
  {
    "hosts": "web-servers",
    "become": true,
    "tasks": [
      {
        "name": "copy war onto tomcat server",
        "copy": {
          "src": "/opt/playbooks/webapp/target/webapp.war",
          "dest": "/opt/apache-tomcat-8.5.69/webapps"
        }
      }
    ]
  }
]

