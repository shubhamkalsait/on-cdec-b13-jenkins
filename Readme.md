# Jenkins

## What is Jenkins
SCM: Git, Github, Gitlab
Containerisation: Docker
Orchestration: Kubernetes
IAC: Terraform
CICD: Jenkins

SDLC: Software development lifecycle

Req. Gathering / Analysis
Plan / Design
Coding / Development
Testing
Deploy / deliver
Maintainance / monitoring

DevOps Eng - Process Dev and Ops will work same environment 


Monolithic | Microservice
amazon.war |    cloth.war
                mobile.war
                home.war

cloth.war
    main.java
    demo.java
    app.java

Continuous Integration - Process of automating the integration of code files wrote by multiple developers
java integrate 
java validate

Continous Building
java compile
java package .war

cloth.war

Continuous Testing

Contniuous deployment / delivery



Continuois integration / Continuous Build

GitHub -> { PULL(git clone) -> Intergrate (java integrate) -> 
java validate -> javac compile -> java package } -> cloth.war (tomcat)

Automation ->

It reduced time
reduced effort
reduced repeatative tasks


Deployment   OR    Delivered
  Server             Store



Jenkins - Build Tool / CICD tool - Bamboo | plugins | Community 
Java based application
weekly - unstable
stable - 3 month

gitlab-ci, actions, code pipeline, cloud build, azure pipeline


## Installing Jenkins
```
apt install 

```

student.war  -> 

HW.
1. Create a job to pull the source code from GITLAB's private repository
2. EMAIL Notification, Configure email notification if jobs get failed/pass

gmail - two way auth (dissable) - low security app (enable)



## Pipeline
-----------

Automation of the JOBs

4 Jobs - Pipeline
PULL -> BUILD -> TEST -> DEPLOYMENT (CICD)
Plugins (Pipeline)

Pipeline -> Groovy Language (DSL)
Scripted Pipeline .groovy | node
Declarative Pipeline .groovy/.jdp | More easy to Use | pipeline

pipeline {
    agent any
    stages {
        stage('Pull') {
            steps {
                git 'https://github.com/shubhamkalsait/studentapp-ui.git'
            }
        }
        stage('Build') {
            steps {
                sh 'echo \'Application is Build\''
                integrate, validate
                compile, package
            }
        }
        stage('Test') {
            steps { 
                sonarqube test
                echo 'Test Success'
            }
        }
        stage('Deploy') {
            steps {
                cp cloth.war webapps/
                echo 'Deploy Success'
            }
        }
    }
}

HW.
- Run pipeline on slave node


Build Stage:

code integrate
code validate
code compile
code package

Build tool - Apache Maven(pom.xml), gradle(gradlew)

Developers, Project 

Repositories, Maven Project


### Maven


1. Basics of Maven
2. Installation of maven
3. How to create Maven Project
4. How to read Pom.xml


Maven Lifecylce - Maven Phases - Maven Goals

1. Default = 8 Phases - 
2. Clean 
3. Site

studentapp, mvn package 

studentapp - Development - java 1.0.8 (lib)

java17  (lib)

BUILD - Compile


### Testing

Code test
Use cases test
API Test
Quality Test - Pass - 
UAT 

Sonarqube - Sonar - Java - 9000 - Server Setup - webapp
Sonarcloud

Vulnerbailities, bugs, Understanding, duplicate 



### Deployment

HW.
- Create a wordpress server
- Deploy application on Wordpress (CLI + Console)
- Add delivery stage in pipeline

