

# k8-docker-desktop-kafka


![enter image description here](https://miro.medium.com/max/1400/0*z3nQB8zQjQCRhrDG.png)

### What is this repo about? 
---
Need to test Kafka and want to do it the good way? do it with a docker container!

The goal is to have a fully working Kafka ecosystem up and runnning in a blink of an eye (maybe a few blinks actually) without makeing any significant installation on your local computer, and in the case you need to start from scrach.. is easy and run a few commands.

### What does the docker image include? 
---
When you run the docker compose, the followinng items will be deployes:

- Zookeeper (**localhost:2181**)
- Kafka Broker (**localhost:9092**)

**feature/confluent-tools** branch include:
- Schema Registry (**localhost:8081**)
- Kafka Connect (**localhost:8083**)
- Kafka Proxy
- Kafka KSQL

## Notes
- There are many other methods to run Kubernetes locally including minikube (kinda big resource footprint), Kind (pretty awesome but very light), or many others. This is probably the quickest way to get Kubernetes going with Kafka.
- This is meant to be run on MacOS

# Prerequisites

1. Docker ([link](https://docs.docker.com/get-docker/))
2. Kubectl (`brew install kubectl`) 

# Pre-checks

Confirm Kubernetes is enabled:
- Click on the Docker icon on the top right menu bar click and select preferences. 
- From the preferences menu, choose Kubernetes and confirm the checkbox next to Enable Kubernetes is check, if not... check it

![enter image description here](https://i.imgur.com/5uJoQzI.jpg)
- Apply and Restart

# Instructions

- Download/Unzip or clone this repo to you computer
- From a terminal, navigate to the root directory of this project run the commnand `docker-compose pull`
![enter image description here](https://i.imgur.com/0r0x8sn.jpg)
- Deploy the docker-compose stack to Kubernetes like by running the command:
```docker stack deploy --compose-file docker-compose.yml kafka-stack```
![enter image description here](https://i.imgur.com/0a9sFuu.jpg)

- Run this commands to confirm the **services** are up and running:  ```kubectl get svc ```
![enter image description here](https://i.imgur.com/BU4yGQv.jpg)
- Run this commands to confirm the **pods** are up and running: ```kubectl get pods ```
![enter image description here](https://i.imgur.com/ZbsJ0cq.jpg)
---
At this point you can start using the broker on the ports 9092 for Kafka and 2181 for Zookeeper, there is a cool tool called [Conduktor](https://www.conduktor.io/) that can ease the kafka broker management with a nice GUI, you can also setup the schema registry and kafka connectgive it a try:
![enter image description here](https://i.imgur.com/9pCnFru.jpg)


# I want moar!



if you want to dig into the Kubernetes dashboard, please do the following:

- Start the Kubernetes dashboard: ```kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml```
![enter image description here](https://i.imgur.com/rLnT1IF.jpg)

- Start the the web server: ``` kubectl proxy ```
![enter image description here](https://i.imgur.com/xh183vY.jpg)

- Open a new terminal and get the token that we will use to authenticate on the web server: ``` kubectl -n kube-system describe secret default | grep "token:"```
![enter image description here](https://i.imgur.com/KmKBfJq.jpg)

- Open the web server and sign in with the token we got on the step above: 
``` http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/ ```
![enter image description here](https://i.imgur.com/KDJd2Fm.jpg)


- Congratulations, you are good to go!
![enter image description here](https://i.imgur.com/Oe3SwqX.png)

# A fresh start

In the case you want or need to clean up and start from scratch. You have 3 options:
- Remove the stack: ``` docker stack rm kafka-stack```
- Manually:
	- Open docker preferences
	- Go to the Kubernetes section
	- Click on "Reset kuberneters cluster"
	- Confirm the operation
- Reset all (this will remove *everything*):  `docker system prune -a --volumes`  