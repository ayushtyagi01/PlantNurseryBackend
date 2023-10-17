# Prometheus web application monitoring and triggering jenkins job

Prometheus will monitor the 3 services
    - MySQL (Database)
    - Nginx
    - Spring

If any of the 3 services is down then it will send alert through alert manager and it will trigger a jenkins job which will restart the service which was down.

## Table of Content



## Introduction

Prometheus is configured to monitor three services: MySQL (Database), Nginx, and Spring. If any of these services goes offline, Prometheus will trigger an alert through Alert Manager. This alert, in turn, activates a Jenkins job responsible for restarting the specific service that experienced the outage.

## Prerequisites

 - Azure VM [Size - Standard D2s v3 (2 vcpus, 8 GiB memory)]
 - Web application should be running in the VM
    - Install all the dependencies inside the vm like jdk,node,npm,maven,mysql,nginx and configure the neccessary components[ In our case we  have used Spring backend, React Frontend , MysQL Database]
    - Clone your web application inside the VM.
 - Install and set up Jenkins
 - Install Python [To scrape custom metrics].

## Steps
 ### Defining Custom Metrics
    - We have used a Python Script to define custom metrics
    - Define Custom metrics to expose the status of MySQL, Nginx, Spring on port 8000
 
 ### Define Alert-Rules
    -  The alert-rules file contains Prometheus alert rules for monitoring critical services and applications:
        -  MySQLServiceDown: Alerts when the MySQL service is unresponsive.
        -  NginxServiceDown: Alerts when the Nginx service encounters issues.
        -  SpringAppServiceDown: Alerts when the Spring application is not responding.

 ### Alert Manager 
    - This configuration file for AlertManager defines how alerts are routed and handled based on specific service names. It ensures timely responses to service disruptions

    - Service-specific routes direct alerts to distinct Jenkins webhook endpoints for handling. For instance, 'jenkins-webhook-mysql' manages MySQL service alerts, and 'jenkins-webhook-nginx' handles Nginx alerts.

    - Start alert manager
    ```
    ./alertmanager --config.file=alertmanager.yml
    ```
   
 ### Prometheus Setup
    - Download the latest release of Prometheus for your platform, then extract and run it:
            ```
            tar xvfz prometheus-*.tar.gz
            cd prometheus-*
            ```
    - Configure prometheus.yml 
    - Start the prometheus
    ```
    ./prometheus --config.file=prometheus.yml
    ```

 ### Setup Jenkins 
    - Create a new Job in Jenkins
    - Configure the Job 
        - Select the option "The project is parameterized"
        image.png
        
