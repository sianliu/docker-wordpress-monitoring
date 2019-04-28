# WordPress, Prometheus and Grafana on docker using Ansible

Here's is the quick start on wordpress application setup using docker compose and setup on monitoring tools such as prometheus, grafana, cadvisor on docker container using docker compose. AWS instances are used and configured with help of ansible.

```
## Here WordPress is hosted on docker container inside aws instance. Instance is configured using [ansible playbook](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/playbook/wordpress) which install docker and docker compose. [Docker compose](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/docker/wordpress) uses the latest wordpress and mysql image from docker hub and build the container with configuration and then start it.

### WordPress docker compose config
```  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    volumes:
      - ./wp-app:/var/www/html # Full wordpress project
    depends_on:
      - db
```

### Mysql docker compose config
```  db:
    image: mysql:latest
    ports:
      - 3306:3306
    volumes:
      - ./wp-data:/docker-entrypoint-initdb.d
```

## Monitoring Applications
Prometheus along with grafana is configured to monitor docker and show it in the dashboard. Instance is configured using [ansible playbook](https://github.com/kavinksm/docker-wordpress-monitoring/tree/develop/playbook/monitoring) which install docker and docker compose. Applications are launched inside docker container using [docker compose](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/docker/monitoring).

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

#### Steps to configure grafana dashboard
1. Select the datasource type to prometheus
2. Provide the prometheus URL default - http://localhost:9090
3. Set the datasource access type to Proxy
4. Import dashboard from [json](https://github.com/sianliu/docker-wordpress-monitoring/tree/master/docker/monitoring/dashboards/docker_monitoring.json) and select the prometheus datasource.

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
* http://docs.aws.amazon.com/autoscaling/latest/userguide/LaunchConfiguration.html
* http://docs.aws.amazon.com/autoscaling/latest/userguide/asg-in-vpc.html
* https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
* https://docs.docker.com/compose/install/
* https://docs.docker.com/samples/wordpress/
* https://prometheus.io/docs/introduction/overview/
* http://docs.grafana.org/
* https://grafana.com/dashboards/893
