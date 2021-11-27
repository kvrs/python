# python

``Create a Dockerfile for Python``
Now that our application is running properly, let’s take a look at creating a Dockerfile.

A Dockerfile is a text document that contains the instructions to assemble a Docker image. When we tell Docker to build our image by executing the docker build command, Docker reads these instructions, executes them, and creates a Docker image as a result.

Let’s walk through the process of creating a Dockerfile for our application. In the root of your project, create a file named Dockerfile and open this file in your text editor.

What to name your Dockerfile?

The default filename to use for a Dockerfile is Dockerfile (without a file- extension). Using the default name allows you to run the docker build command without having to specify additional command flags.

Some projects may need distinct Dockerfiles for specific purposes. A common convention is to name these Dockerfile.<something> or <something>.Dockerfile. Such Dockerfiles can then be used through the --file (or -f shorthand) option on the docker build command. Refer to the “Specify a Dockerfile” section in the docker build reference to learn about the --file option.

We recommend using the default (Dockerfile) for your project’s primary Dockerfile, which is what we’ll use for most examples in this guide.

The first line to add to a Dockerfile is a # syntax parser directive. While optional, this directive instructs the Docker builder what syntax to use when parsing the Dockerfile, and allows older Docker versions with BuildKit enabled to upgrade the parser before starting the build. Parser directives must appear before any other comment, whitespace, or Dockerfile instruction in your Dockerfile, and should be the first line in Dockerfiles.

# syntax=docker/dockerfile:1
We recommend using docker/dockerfile:1, which always points to the latest release of the version 1 syntax. BuildKit automatically checks for updates of the syntax before building, making sure you are using the most current version.

Next, we need to add a line in our Dockerfile that tells Docker what base image we would like to use for our application.

# syntax=docker/dockerfile:1

FROM python:3.8-slim-buster
Docker images can be inherited from other images. Therefore, instead of creating our own base image, we’ll use the official Python image that already has all the tools and packages that we need to run a Python application.

Note

To learn more about creating your own base images, see Creating base images.

To make things easier when running the rest of our commands, let’s create a working directory. This instructs Docker to use this path as the default location for all subsequent commands. By doing this, we do not have to type out full file paths but can use relative paths based on the working directory.

WORKDIR /app
Usually, the very first thing you do once you’ve downloaded a project written in Python is to install pip packages. This ensures that your application has all its dependencies installed.

Before we can run pip3 install, we need to get our requirements.txt file into our image. We’ll use the COPY command to do this. The COPY command takes two parameters. The first parameter tells Docker what file(s) you would like to copy into the image. The second parameter tells Docker where you want that file(s) to be copied to. We’ll copy the requirements.txt file into our working directory /app.

COPY requirements.txt requirements.txt
Once we have our requirements.txt file inside the image, we can use the RUN command to execute the command pip3 install. This works exactly the same as if we were running pip3 install locally on our machine, but this time the modules are installed into the image.

