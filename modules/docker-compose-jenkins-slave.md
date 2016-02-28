<!--
{
"name": "docker-compose-jenkins-slave",
"version" : "0.1",
"title" : "Docker Compose Jenkins Slave",
"description" : "Set up a Jenkins SSH slave in Docker Compose",
"homepage" : "https://github.com/ianmiell/outlearn-docker-compose-jenkins-slave",
"freshnessDate" : 2016-02-28,
"license" : "CC BY 4.0"
}
-->
<!-- @section -->

# Jenkins Slave Intro

In this module we're going to take things to the next level by adding a Jenkins slave to our Jenkins master. The slave will run as a separate container, and will be able to run jobs we send to it.

First we need to get the right code checked out:

```
git checkout 2.0
```

If you type the above you will get the code we need checked out. The files we have are:

```
docker-compose.yml
jenkinssetup
jenkinsslave1
phoenix.sh
```

The docker-comose.yml we already know about (although it has been altered here). The phoenix.sh file is a helper file to clean and rebuild your docker-compose environment if you want to be sure you have a stable build. The remaining directories (jenkinssetup, and jenkinsslave1) contain the configuration and code for the containers we want to run alongside the original Jenkins server container.

Let's look at the new docker-compose.yml file first.


<!-- @section -->

## The 'docker-compose.yml' File

In our new docker-compose.yml we have three sections (jenkinsslave1, jenkins and jenkinssetup), each representing a container that will run in our docker-compose cluster.

```
jenkinsslave1:
  container_name: jenkinsslave1
  build: jenkinsslave1
jenkins:
  image: jenkins
  container_name: jenkins
  ports:
    - "8080:8080"
  links:
    - jenkinsslave1
jenkinssetup:
  container_name: jenkinssetup
  build: jenkinssetup
  links:
    - jenkins
```

The containers are brought up in the order they are described, so the 'jenkinsslave1' container is run first. Then the 'jenkins' server container is run, and finally the 'jenkinssetup' container is run.

In the previous module, we had a single container (the Jenkins server) pulled directly from the Docker Hub and run. In this module we're building up two new containers locally. Hence the 'image:' entry has been replaced by a 'build:' entry in the 'jenkinsslave1' and 'jenkinssetup' cases. The docker-compose invocation will look at the directory set in the 'build:' attribute and run a docker build to create the container that is eventually run.

We also use the 'links:' attribute to indicate which of the other containers are depended on by this container. So the 'jenkins' server container depends on the 'jenkinsslave1' container being up to run, and the 'jenkinssetup' container needs the jenkins server to be up to run. The 'links:' attribute can serve another purpose, which is to specify how the container will refer to the other container over the network (eg we could change the line: '- jenkinsslave1' to '- myslave', and refer to the slave over the network from the jenkins container as 'myslave' instead of 'jenkinsslave1'. However, here we simply use the container names as they are in the file.

<!-- @section -->

## The 'jenkinssetup' Container

The purpose of the 'jenkinssetup' container is to interact with the Jenkins server to get configure it into the state we want it in.

It sets up the server with:

- credentials information
- node information

The credentials information sets up the username and password information so that the server can ssh into the slave. It does this by calling the 'credentials/jenkins_credentials.sh' file, which uses curl to POST the information about the username password.

The node information uses the 'node/jenkins_node.py' script to set up a Jenkins slave. The code is listed here:

```
from jenkinsapi.jenkins import Jenkins
from jenkinsapi.credential import UsernamePasswordCredential, SSHKeyCredential

api = Jenkins('http://jenkins:8080')
# Get a list of all global credentials
creds = api.credentials
credentialsId = creds.credentials.keys()[0]

import jenkins

j = jenkins.Jenkins('http://jenkins:8080')

# jenkins slave
params = {
    'port': '22',
    'username': 'jenkins',
    'credentialsId': credentialsId,
    'host': 'jenkinsslave1'
}
create = True
for node in j.get_nodes():
    if node['name'] == 'jenkinsslave1':
        create = False
if create:
    j.create_node(
        'jenkinsslave1',
        nodeDescription='my test slave',
        remoteFS='/tmp',
        labels='jenkinsslave',
        launcher=jenkins.LAUNCHER_SSH,
        launcher_params=params
    )
```

In brief, this script retrieves the credentials set up on the server, specifies the parameters of the slave we will create and calls 'create_node' to create an SSH slave node. It uses the 'jenkins' and 'jenkinsapi' modules to achieve this.

<!-- @section -->

## The 'jenkinsslave1' Container

The 'jenkinsslave1' is very simple: it contains a Dockerfile which sets up git on a standard slave image:

```
FROM evarga/jenkins-slave
RUN apt-get update -y && apt-get install -y git
```

Bear in mind that you could configure this to run any kind of weird and wonderful configuration of software that works for your Jenkins job, eg an old version of Java, or a particular version of Python.


<!-- @section -->

# Build and run

The phoenix.sh script is used as a time

```
#!/bin/bash
docker-compose rm -f && docker-compose build && docker-compose up
```

It removes any left-over containers from previous builds, builds the containers (using the Docker cache to save time) and then brings the docker-compose cluster up.

If you run this script and wait until you see the lines we saw in the previous module:

```
jenkins       | --> setting agent port for jnlp
jenkins       | --> setting agent port for jnlp... done
```

your Jenkins server is ready to roll on http://localhost:8080.

Now if you want to create a Jenkins job to run in your Jenkins server you can! Remember to specify the 'jenkinsslave' tag if you want the job to run in the 'jenkinsslave1' container.


<!-- @section -->

# Wrap-up

We've covered a lot of ground in this module, so it's worth going over what we've learned a little!

We've:

- created multiple containers in a cluster
- set up dynamic interactions between these containers to configure the Jenkins server from a separate container
- used this to create a new isolated environment to run Jenkins jobs in

Finally our somewhat complex requirement for a Jenkins cluster with different slaves has been fulfilled using code rather than manual set-up!

From here you can iterate a solution that works for your codebase, creating slaves with differing configurations for different types of Jenkins jobs.

Moreover, by separating out the different pieces into different containers, you avoid the 'ball of string' problem of monolithic Jenkins builds by ensuring each container is only as complex as it needs to be, and no more, making refactoring or changes easier to both test and develop.

<!-- @end -->

