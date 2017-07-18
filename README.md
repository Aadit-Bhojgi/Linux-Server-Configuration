# Linux Server Configuration

### Overview
>This project includes setting up the Linux Server, configuring database and then deploying Item Catalog Application on it and securing it with any      possible security threats.

* Public IP ADRESS: `13.126.30.40`
* The cloud server is on <a href="https://amazonlightsail.com/">Amazon Lightsail</a>, Amazon Lightsail is the easiest way to launch and manage a virtual private server with AWS.
* To view the deployed Application on the cloud server following is the URL to the website.<br>
  ## <a href="http://ec2-13-126-30-40.ap-south-1.compute.amazonaws.com/">http://ec2-13-126-30-40.ap-south-1.compute.amazonaws.com/</a>
  
#### NOTE
>The above given domain name is converted on the given below link through the Public IP ADDRESS.<br>
<a href="http://www.nmonitoring.com/ip-to-domain-name.html?ip=13.126.30.40&pingsub=1&ln=en">Convert your IP</a>

### Project Execution
###### Connecting to the Cloud Server
* Paste the content given in the notes in a Private-Key.pub file
* Then open CLI(GIT Bash: recommanded) in your system and run the following command<br>
```$ ssh grader@ip-address -p 2200 -i Private-Key.pub```