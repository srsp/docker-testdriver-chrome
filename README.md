# What? 
The idea is to run end-to-end tests in a Docker container, that contains Chrome, the chromedriver, mvn and the source for the tests. The tests run against an instance of your application outside of the container. This can be done on a CI server like Jenkins, so you do not need to hassle with the installation of these tools on the CI server itself.

Published on [Docker Hub](https://hub.docker.com/r/srsp/testdriver-chrome/).

# How? 

```
docker pull srsp/testdriver-chrome
```

You can run this image with 

```docker run -v "$(pwd)":/home/tester/code -v"$HOME"/.m2:/home/tester/.m2 -w /home/tester/code/someOtherDir srsp/testdriver-chrome ./execute-mvn-test.sh```

where 

* `-v "$(pwd)":/home/tester/code` mounts your source code into the container
* `-v"$HOME"/.m2:/home/tester/.m2` mounts a directory (empty or containing your mvn .m2 folder) into the container
* `./execute-mvn-test.sh` is the script that runs your tests.

A typical `execute-mvn-test.sh` could look like this:

```
#!/usr/bin/env bash
xvfb-run mvn verify
```

The important thing here is the `xvfb-run`, which will wrap the `mvn` execution into the virtual framebuffer, so Selenium or Serenity tests can use the provided chromedriver to start the provided Chrome browser.

# Versions

| testdriver-chrome | Chrome        | Chromedriver | OpenJDK   | mvn   | 
|---		        |---            |---           |---        |--- 
| 2.46.0-jdk11      | 72.0.3626.109 | 2.46         | 11.0.2.9  | 3.6.0 |
| 2.46.0            | 72.0.3626.109 | 2.46         | 1.8.0_202 | 3.6.0 |
| 2.45.0            | 71.0.3578.98  | 2.45         | 1.8.0_191 | 3.6.0 |
| 2.43.1            | 70.0.3538.77  | 2.43         | 1.8.0_181 | 3.6.0 |
| 2.43.0            | 70.0.3538.77  | 2.43         | 1.8.0_181 | 3.5.4 |
| 2.42.0            | 69.0.3497.92  | 2.42         | 1.8.0_181 | 3.5.4 |
| 2.41.0            | 69.0.3497.92  | 2.41         | 1.8.0_181 | 3.5.4 |


# Acknowledgments
Big thanks to [Hronom](https://github.com/Hronom/chromedriver-docker-example) for the Chrome, Chromedriver and Xvfb stuff.

# Development

### Find out Chrome version

`docker run srsp/testdriver-chrome /opt/google/chrome/chrome --no-sandbox --version`

### Find out Java and mvn version

`docker run srsp/testdriver-chrome mvn --version`