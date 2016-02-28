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

# Docker Install

In this first part we're going to check you have the right version of Docker.

At your terminal, type:

```
docker version
```

and you should get some output that looks like this:

```
Client:
 Version:      1.9.1
 API version:  1.21
 Go version:   go1.4.2
 Git commit:   a34a1d5
 Built:        Fri Nov 20 13:16:54 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.9.1
 API version:  1.21
 Go version:   go1.4.2
 Git commit:   a34a1d5
 Built:        Fri Nov 20 13:16:54 UTC 2015
 OS/Arch:      linux/amd64
```

If you get output that looks like this:

```
docker: command not found
```

Then you need to install docker first.

If your version is older than 1.8, then you may come across problems later.

See [here](https://docs.docker.com/engine/installation/) for how to install the latest version of Docker.

<!-- @section -->

# Docker-Compose Install

Next we're going to check you have the correct version of Docker-Compose.

At your terminal, type:

```
docker-compose version
```

and you should get output that looks like this:

```
docker-compose version 1.5.2, build 7240ff3
docker-py version: 1.5.0
CPython version: 2.7.9
OpenSSL version: OpenSSL 1.0.1e 11 Feb 2013
```

You should have at least version 1.5+ of docker-compose.

See [here](https://docs.docker.com/compose/install/) for how to install the latest version of Docker Compose.

<!-- @section -->

# Get the code

Now we will get the code we need to run this example.

Go to an appropriate folder to download a git repository, and type:

```
git clone https://github.com/ianmiell/jenkins-phoenix
cd jenkins-phoenix
git checkout 1.0
```

After the last git command, you should see output similar to:

```
Switched to branch '1.0'
```

You are now ready to run your first docker-compose command!


<!-- @section -->

# Run docker-compose

In the same folder as before, run:

```
docker-compose up
```

Watch the output as it builds the containers to run your Jenkins setup. At first some of it won't make sense, but it's good to watch docker-compose go through its paces.

```
Creating jenkins
Attaching to jenkins
jenkins | Running from: /usr/share/jenkins/jenkins.war
jenkins | webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
[...]
jenkins | --> setting agent port for jnlp
jenkins | --> setting agent port for jnlp... done
```

When you see the last two lines you know it has started up ok.

If you now navigate to http://localhost:8080 on the same machine you will be presented with your own Jenkins instance, provisioned by docker-compose!

![jenkins](https://raw.githubusercontent.com/ianmiell/outlearn-modules/master/modules/jenkins_basic.png)

Take a look at the docker-compose file [here](https://raw.githubusercontent.com/ianmiell/jenkins-phoenix/1.0/docker-compose.yml)

The first line:

```
jenkins:
```

defines a container that runs within this compose cluster. The next line:

```
  image: jenkins
```

is indented, and tells docker-compose to pull down and run the default jenkins image from the Docker Hub. Next, we give the container a name when it runs:

```
  container_name: jenkins
```

and finally, expose the port 8080 

```
  ports:
    - "8080:8080"
```

Note: if port 8080 is used on your machine already, you may want to change the first 8080 to another port, and point your browser at that port instead.


<!-- @end -->

