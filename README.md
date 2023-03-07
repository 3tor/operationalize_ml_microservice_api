[![CircleCI](https://dl.circleci.com/status-badge/img/gh/3tor/operationalize_ml_microservice_api/tree/main.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/3tor/operationalize_ml_microservice_api/tree/main)

## Project Overview

In this project, you will apply the skills you have acquired in this course to operationalize a Machine Learning Microservice API.

You are given a pre-trained, `sklearn` model that has been trained to predict housing prices in Boston according to several features, such as average rooms in a home and data about highway access, teacher-to-pupil ratios, and so on. You can read more about the data, which was initially taken from Kaggle, on [the data source site](https://www.kaggle.com/c/boston-housing). This project tests your ability to operationalize a Python flask app—in a provided file, `app.py`—that serves out predictions (inference) about housing prices through API calls. This project could be extended to any pre-trained machine learning model, such as those for image recognition and data labeling.

### Project Tasks

Your project goal is to operationalize this working, machine learning microservice using [kubernetes](https://kubernetes.io/), which is an open-source system for automating the management of containerized applications. In this project you will:

- Test your project code using linting
- Complete a Dockerfile to containerize this application
- Deploy your containerized application using Docker and make a prediction
- Improve the log statements in the source code for this application
- Configure Kubernetes and create a Kubernetes cluster
- Deploy a container using Kubernetes and make a prediction
- Upload a complete Github repo with CircleCI to indicate that your code has been tested

**The final implementation of the project will showcase your abilities to operationalize production microservices.**

---

## Setup the Environment

- Create a virtualenv with Python 3.7 and activate it. Refer to this link for help on specifying the Python version in the virtualenv.

```bash
python3 -m pip install --user virtualenv
# You should have Python 3.7 available in your host.
# Check the Python path using `which python3`
# Use a command similar to this one:
python3 -m virtualenv --python=<path-to-Python3.7> .devops
source .devops/bin/activate
```

- Run `make install` to install the necessary dependencies

### Running `app.py`

1. Standalone: `python app.py`
2. Run in Docker: `./run_docker.sh`
3. Run in Kubernetes: `./run_kubernetes.sh`

### Kubernetes Steps

- Setup and Configure Docker locally

  ```bash
  - To install Docker on your machine, go to [Docker ](https://docs.docker.com/engine/install/ "docker")
  ```

- Setup and Configure Kubernetes locally

  ```bash
  - To install Kubernetes on linux:
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  ```

  or go [here](https://kubernetes.io/docs/tasks/tools/) for other OS

  ```bash
    #Verify the installation with:`
    kubectl version --client
  ```

- Create Flask app in Container

  ```bash
      docker build --tag=yourdockerhubusername/imagename:tag`
      docker run -p 8000:80 imagename
  ```

- Run via kubectl

  ```bash
      #To execute via Kubectl
      kubectl run preferredname --image=yourdockerhubusername/imagename:tag --port=80
      #Verify kubernetes pods started
      kubectl get pods
      #Forward the container port to a host
      kubectl port-forward pods/preferedname --address 0.0.0.0 8000:80
      #Verify port is forwarded by opening another terminal and execute
      curl localhost:8000
      #or
      run "make_prediction.sh"
  ```

### Files In the Project

`.circleci/config.yml`: Has the circleci workflow configurations for testing and building our code.

`output_txt_files`: Files containing outputs from the predictor. Docker_out.txt contains docker run outputs and kubernetes_out.txt contains output from kubernetes

`app.py`: Main flask entry app that accepts input requests and makes predictions based on input.

`Dockerfile`: Our docker image setup file

`make_prediction.sh`: This file when executed makes an http request to the /predict route and the prediction response is outputted.

`Makefile`: The Makefile includes instructions on environment setup and lint tests. It also creates and activates a virtual environment and installs dependencies in requirements.txt

`requirements.txt`: List of libraries needed for our app to run

`run_docker.sh`: Build image with a descriptive tag to run the flask app

`run_kubernetes.sh`: Run the Docker Hub container with kubernetes and forward the container port to a host

`upload_docker.sh`: The file tags and uploads an image to Docker Hub
