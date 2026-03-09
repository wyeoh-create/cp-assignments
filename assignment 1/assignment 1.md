# Assignment 1: From Local to Cloud (Kafka on Kubernetes with CFK + KRaft)

## 1. Architecture Overview

- **Cloud provider:** EKS / GKE / AKS
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
2. Deploy KRaft controller, Kafka, Connect, Schema Registry, ksqlDB, REST Proxy, and Control Center using:
   - `confluent-platform-c3++.yaml`
3. Verify all pods are running:
   - `kubectl get pods -n confluent`
4. Port-forward Control Center:
   - `kubectl port-forward controlcenter-0 9021:9021`
5. (Optional) Create topic `pageviews` via Control Center if auto-create is disabled.
6. Deploy Datagen connector:
   - `kubectl apply -f connector-datagen-pageviews.yaml`
7. Verify connector status:
   - `kubectl get connector -n confluent`
8. Verify data flow:
   - Console consumer from `kafka-0` pod **or**
   - Control Center → Topics → `pageviews` → Messages

## 4. Issues Encountered and Resolutions

- Issue 1: _e.g. Connect pod crashloop due to image pull / permissions_
  - Root cause:
  - Fix:
- Issue 2: _e.g. topic auto-creation disabled_
  - Root cause:
  - Fix:

## 5. Verification Screenshots

- Control Center overview showing Kafka, Connect, and Control Center deployed
- `pageviews` topic in Control Center with messages produced
- Datagen connector (`pageviews`) in Running state
- Optional: Terminal screenshot of `kafka-console-consumer` showing messages

## 6. Included Deployment Files

- `confluent-platform-c3++.yaml` – KRaft controller, Kafka, Connect, Control Center, etc.
- `connector-datagen-pageviews.yaml` – Datagen source connector
- (Optional) Additional CRs (e.g. KafkaTopic definitions)
