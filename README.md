# Jenkins Continuous Integration and Continuous Delivery Pipeline

## Stage 1:
In the first stage named as checkout source code, the code was checked out from the github repository from the master branch. Then the code was read from the pom.xml file to find the artifact id and version and these are stored as environment variables "app name" and "version-number".

def pom = readMavenPom file: 'pom.xml'
env.appname = "${pom.artifactId}"
env.version-number = "${pom.version}"


## Stage 2:
The <AppName> value was replaced with the artifactId and <Version> was replaced with <version> from the pom.xml by using environment variables defined earlier.
The code was built by maven clean package command.

 "mvn clean package"

## Stage 3:
On successful built process, an input step was defined seeking the timeout for approval which was set to 5 mins. This allows the process to go on to the next step only once it is approved or else gives a times out error.
The stage was named as "Awaiting approval for deploying <AppName> and <Version>. Are you ready to deploy?"

## Stage 4:
In the deployment stage, the test.pem was used for connecting to the Ubuntu EC-2 instance and then the war file was copied onto the EC2 instance by using the following command:
"scp -i Test.pem target/JenkinsAssignment-1.0-SNAPSHOT.war ubuntu@ec2–compute-1.amazonaws.com:/home/ubuntu/ JenkinsAssignment-1.0-SNAPSHOT.war"
The war file was added to webapp folder in ec2 and the tomcat server was restarted by providing the stop and start scripts.


## Step 5:
Once the application is deployed, in the executing tests stage, the output was echoed by using curl command.
http://localhost:8080/jenkinsAssignment/webapps"
