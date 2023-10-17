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

 - Download the latest release of Prometheus for your platform, then extract and run it:
        ```
        tar xvfz prometheus-*.tar.gz
        cd prometheus-*
        ```
 - Configure prometheus.yml 
        ```
        global:
            scrape_interval: 15s

        scrape_configs:
            - job_name: 'combined_monitoring'
        static_configs:
            - targets: ['localhost:8000']

        rule_files:
            - 'alert-rules.yml'

        alerting:
            alertmanagers:
                - static_configs:
                - targets: ['localhost:9093']
      ```
