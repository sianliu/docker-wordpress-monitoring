# Project Title 

Ansible installs wordpress mysql, docker, docker-compose, prometheus, and grafana. Docker-compose is then used to setup the wordpress and monitoring environment.

# Getting Started

These instructions will get you a copy of the project up and running on your local machine running Linux for development and testing purposes.

# Environment
* OS: Ubuntu 16.04 Xenial LTS
* Ansible: 2.0.0.2
* Docker: 18.09.5
* docker-compose: 1.24
* mysql: 5.7
* grafana: 5.4.4

# Prerequisites

1) Install Ansible on EC2 instance.

```
sudo apt install ansible
```
2) Manage docker as a non-root user

```
sudo usermod -aG docker ubuntu
```

# Initial Installation

1) git clone https://github.com/sianliu/docker-wordpress-monitoring.git
2) chdir: docker-wordpress-monitoring
3) run: ansible-playbook playbook/wordpress/site.yml 
4) chdir: docker/monitoring 
5) run: docker-compose up -d 

# Subsequent Launches 
1) chdir: docker/wordpress docker-compose up -d
2) chdir: docker/monitoring docker-compose up -d

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

## Intent
For Interview Technical Assessment Purposes and not for production use. 

## Credit
https://github.com/kavinksm/docker-wordpress-monitoring
