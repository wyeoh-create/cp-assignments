# Assignment 1: From Local to Cloud (Kafka on Kubernetes with CFK + KRaft)

## 1. Architecture Overview

- **Cloud provider:** EKS (AWS)
- **Kubernetes namespace:** `confluent`
- **Control plane:** Confluent for Kubernetes (CFK)
- **Data plane components:**
  - KRaft Controller
  - Kafka brokers
  - Kafka Connect
  - Confluent Control Center
- **Data flow:** Datagen connector → `pageviews` topic → Kafka console consumer / Control Center message browser

## 2. Prerequisites

- Cloud Kubernetes cluster (3+ nodes, CNCF-conformant)
- `kubectl` and `helm` configured
- Namespace `confluent` created and set as current context

## 3. Deployment Steps

1. Install CFK operator with Helm.
<img width="666" height="257" alt="image" src="https://github.com/user-attachments/assets/fe4d1551-fda9-41ba-a63e-347207b657b7" />

2. Deploy KRaft controller, Kafka, Connect, Schema Registry, ksqlDB, REST Proxy, and Control Center using:
   - `confluent-platform-c3++.yaml`
   <img width="673" height="213" alt="image" src="https://github.com/user-attachments/assets/ba6afd6a-003f-4ea7-b927-a994afc8d539" />

3. Verify all pods are running:
   - `kubectl get pods -n confluent`
   <img width="668" height="255" alt="image" src="https://github.com/user-attachments/assets/61f431d5-98fb-40ce-bc62-b8242fa4bb88" />

4. Port-forward Control Center:
   - `kubectl port-forward controlcenter-0 9021:9021`
   <img width="673" height="235" alt="image" src="https://github.com/user-attachments/assets/9ea13381-a0e4-417b-b71e-04c1591e27b9" />

5. (Optional) Create topic `pageviews` via Control Center if auto-create is disabled.
6. Deploy Datagen connector:
   - `kubectl apply -f connector-datagen-pageviews.yaml`
   <img width="584" height="248" alt="image" src="https://github.com/user-attachments/assets/ab67f014-4fe9-4e72-b441-2048d884a1c0" />

7. Verify connector status:
   - `kubectl get connector -n confluent`
   <img width="476" height="80" alt="image" src="https://github.com/user-attachments/assets/8cd77f97-679f-4c07-aef9-7a4d757861b5" />

8. Verify data flow:
   - Console consumer from `kafka-0` pod **or**
   - Control Center → Topics → `pageviews` → Messages
   <img width="1733" height="903" alt="image" src="https://github.com/user-attachments/assets/67ee5b2b-c199-458c-8295-b598e725015e" />
   <img width="584" height="356" alt="image" src="https://github.com/user-attachments/assets/bb1ba02d-529b-4635-a4f4-d3241ffd1cc7" />



## 4. Issues Encountered and Resolutions

- Issue 1: 0/3 nodes are available: pod has unbound immediate PersistentVolumeClaims.
  - Root cause: 
  The primary issue is that the PVCs (data0-controlcenter-0 and prometheus-data-controlcenter-0) are in a Pending state because:
    - Missing StorageClass specification: The PVCs don't specify a storageClassName, so they cannot bind to any storage provisioner
    - No matching PersistentVolumes: There are no pre-provisioned PersistentVolumes that match the PVC requirements
    - Storage provisioning failure: Without a StorageClass, dynamic provisioning cannot occur
  The cluster has a gp2 StorageClass available, but the PVCs aren't configured to use it.
  - Fix: Set gp2 as default StorageClass
  <img width="1677" height="260" alt="image" src="https://github.com/user-attachments/assets/4b6ad6bf-0aa1-44a3-98af-134e4341f052" />

- Issue 2: Error when provisioning datagen connector.
  - Root cause: Connect cluster doesn’t have the Datagen plugin installed, so the DatagenConnector class doesn’t exist. 
  - Fix: Install the Datagen plugin into Connect, then Create the Connector CR.

## 5. Verification Screenshots

- Control Center overview showing Kafka, Connect, and Control Center deployed
<img width="2559" height="1136" alt="image" src="https://github.com/user-attachments/assets/ad6bbcb9-297c-4ee8-ac05-45b044b309fc" />

- `pageviews` topic in Control Center with messages produced
<img width="2559" height="1292" alt="image" src="https://github.com/user-attachments/assets/bc32287c-82cb-43e7-b6cf-b4a4e32b4506" />

- Datagen connector (`pageviews`) in Running state
<img width="1792" height="892" alt="image" src="https://github.com/user-attachments/assets/2b248b6a-f4a0-4167-8bb8-5290958ed705" />

- Optional: Terminal screenshot of `kafka-console-consumer` showing messages
<img width="586" height="359" alt="image" src="https://github.com/user-attachments/assets/cbfb0cdd-d82f-4487-84b6-0689559372c0" />


## 6. Included Deployment Files

- `confluent-platform-c3++.yaml` – KRaft controller, Kafka, Connect, Control Center, etc.
- `connector-datagen-pageviews.yaml` – Datagen source connector
- (Optional) Additional CRs (e.g. KafkaTopic definitions): Install Datagen into your Connect cluster
```
apiVersion: platform.confluent.io/v1beta1
kind: Connect
metadata:
  name: connect
  namespace: confluent
spec:
  replicas: 1
  image:
    application: confluentinc/cp-server-connect:7.9.0
    init: confluentinc/confluent-init-container:2.11.0

  build:
    type: onDemand
    onDemand:
      plugins:
        locationType: confluentHub
        confluentHub:
          - name: kafka-connect-datagen
            owner: confluentinc
            version: 0.5.2

  dependencies:
    kafka:
      bootstrapEndpoint: kafka:9071
```
