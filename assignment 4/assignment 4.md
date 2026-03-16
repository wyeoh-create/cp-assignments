# Assignment 4: Confluent Platform 8.0+ Upgrade and Modern Observability Stack
This assignment focuses on adopting the new features of the Confluent Platform latest release, version 8.0+, which includes significant cloud-native and operational enhancements.

Your task is to execute an upgrade of our core components to the latest version, specifically focusing on two key initiatives: Optimisation and Enhanced Monitoring.

## 1. For optimisation, you must implement and test Tiered Storage, configuring Kafka topics to automatically move older, cold data segments to cost-effective object storage to maximize cost efficiencies.

Update Kafka CR to use tiered storage.


Verify the configuration in Control Center


## 2. Also, enable cluster self balancing and verify how the cluster will behave if you scale broker replicas while new topics are being added with continuous throughput.

## 3. For enhanced monitoring, you will utilize the latest Control Center UI with its native Prometheus integration, and then extend this observability by configuring Grafana Dashboards to ingest the Prometheus metrics, providing our operations team with rich, centralized, and enhanced real-time views of the cluster performance.

## 4. Also make sure the control center should be accessible by a load balancer and not port-forward.
