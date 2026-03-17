# Assignment 4: Confluent Platform 8.0+ Upgrade and Modern Observability Stack
This assignment focuses on adopting the new features of the Confluent Platform latest release, version 8.0+, which includes significant cloud-native and operational enhancements.

Your task is to execute an upgrade of our core components to the latest version, specifically focusing on two key initiatives: Optimisation and Enhanced Monitoring.

## 1. For optimisation, you must implement and test Tiered Storage, configuring Kafka topics to automatically move older, cold data segments to cost-effective object storage to maximize cost efficiencies.

Update Kafka CR to use tiered storage.

<img width="739" height="267" alt="image" src="https://github.com/user-attachments/assets/dbbbb388-5d94-4f7c-9eba-10a95cc14a22" />

Verify the configuration in Control Center

<img width="468" height="302" alt="image" src="https://github.com/user-attachments/assets/dc591956-ee53-4757-9e32-ec166b8c9f59" />

## 2. Also, enable cluster self balancing and verify how the cluster will behave if you scale broker replicas while new topics are being added with continuous throughput.

Verify self balancing has enabled

<img width="468" height="351" alt="image" src="https://github.com/user-attachments/assets/a42f4bb6-679f-4fce-a2b4-405ae7b2ebdd" />

## 3. For enhanced monitoring, you will utilize the latest Control Center UI with its native Prometheus integration, and then extend this observability by configuring Grafana Dashboards to ingest the Prometheus metrics, providing our operations team with rich, centralized, and enhanced real-time views of the cluster performance.

## 4. Also make sure the control center should be accessible by a load balancer and not port-forward.

<img width="468" height="246" alt="image" src="https://github.com/user-attachments/assets/ae5b01f9-8b71-4340-a545-b5159ca79d67" />

<img width="1383" height="1290" alt="image" src="https://github.com/user-attachments/assets/86b0c91d-8b4a-4427-adb3-377b929f16e3" />
