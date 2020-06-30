

# kubernetes Kafka Playground


![enter image description here](https://miro.medium.com/max/1400/0*z3nQB8zQjQCRhrDG.png)

### What is this repo about?
---
Need to test Kafka and some of its extra tools? do it with a docker container!

The goal is to have a fully working Kafka ecosystem up and running in a blink of an eye (maybe a few blinks actually) without making any significant installation on your local computer, and in the case you need to start from scratch.. is easy and run a few commands.

### What does the docker container include?
---
When you deploy this container, the following items will be available to you:

Main cluster:

- Kafka Broker on port 9092
- zookeeper on port 2181

Extras:

- schema-registry, port 8081 // UI o port 8086
- Rest Proxy on port 8082 // Topics UI on port 8085
- Kafka connect on port 8083 // UI on port 8084
- KSQL on port 8088
- PostgreSQL on port 5432

## Notes
- This is meant to be run on MacOS

# Software requirements:

1. Docker Desktop ([link](https://docs.docker.com/get-docker/))
2. Kubectl ([link](https://kubernetes.io/docs/tasks/tools/install-kubectl/))

# Pre-checks

Confirm Kubernetes is enabled:
- Click on the Docker icon on the top right menu bar click and select preferences.
- From the preferences menu, choose Kubernetes and confirm the checkbox next to Enable Kubernetes is check, if not... check it
- Apply and Restart

# Instructions (part1): Stack deployment

- Download/Unzip or clone this repo to you computer
- From a terminal, navigate to the root directory of this project
- Run the command `docker-compose pull` to get all the images needed
- Deploy the docker-compose stack to Kubernetes like by running the command:
`docker stack deploy --compose-file docker-compose.yml myStackName`

# Instructions (part2): Kubernetes Management

if you want to dig into the Kubernetes dashboard, please do the following:

- Start the Kubernetes dashboard:

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml`

- Start the the web server:

`kubectl proxy`

- Open a new terminal and get the token that we will use to authenticate on the web server:

`kubectl -n kube-system describe secret default | grep "token:"`

- Open the web server and sign in with the token we got on the step above:

`http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/`

- Congratulations, you are good to go!

At this point you can start using the main cluster, schema registry and kafka connect with a cool tool called [Conduktor](https://www.conduktor.io/) that can ease the kafka broker management with a nice GUI:

# A fresh start

In the case you want or need to clean up and start from scratch. You have 3 options:
- Remove the stack: ``` docker stack rm kafka-stack```
- Manually:
	- Open docker preferences
	- Go to the Kubernetes section
	- Click on "Reset kuberneters cluster"
	- Confirm the operation
- Reset all (this will remove *everything*):  `docker system prune -a --volumes`  
