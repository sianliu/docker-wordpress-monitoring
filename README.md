# Project Title 

Ansible installs wordpress mysql, docker, docker-compose, prometheus, and grafana. Docker-compose is then used to setup the wordpress and monitoring environment. 

# Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

# Prerequisites

Install Ansible on EC2 instance. 

```
sudo apt install ansible
```

## Monitoring Applications
Prometheus along with grafana is configured to monitor docker and show it in the dashboard. Instance is configured using [ansible playbook](https://github.com/sianliu/docker-wordpress-monitoring/tree/develop/playbook/monitoring) which install docker and docker compose. Applications are launched inside docker container using [docker compose](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/docker/monitoring).

### Prometheus docker compose config
``` prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - 9090:9090
```

### Grafana docker compose config
```  grafana:
    image: grafana/grafana
    depends_on:
      - prometheus
    ports:
      - 3000:3000
```

### [Prometheus](https://prometheus.io)
Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud. Prometheus is used to monitor and collect and store it in time series format. Prometheus configuration found [here](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/docker/monitoring/prometheus)

### [Grafana](https://grafana.com/)
Grafana is an open source metric analytics & visualization suite. It is most commonly used for visualizing time series data for infrastructure and application analytics. Grafana dashboard found [here](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/docker/monitoring/dashboards)

## Sourcecode reference
```
+--playbook
|  +--instance-launch #launch aws ec2 instance via asg and lc
|  +--monitoring #Install docker and run docker compose for monitoring app
|  +--wordpress #Install docker and run docker compose for wordpress app
+--docker
|  +--monitoring #docker compose for prometheus, grafana, cadvisor
|  +--wordpress #docker compose for worpress and mysql
+--README.md
```

## Reference
* http://docs.ansible.com/ansible/latest/list_of_cloud_modules.html
* http://docs.ansible.com/ansible/latest/list_of_commands_modules.html
* https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
* https://docs.docker.com/compose/install/
* https://docs.docker.com/samples/wordpress/
* https://prometheus.io/docs/introduction/overview/
* http://docs.grafana.org/
* https://grafana.com/dashboards/893
