# Assignment 5:

Hybrid Cloud Adoption with Cluster Linking & Self Managed Connectors

## 1. HA/DR with Cluster Linking 
Now the company wants to explore Active-Passive DR strategy where the on-premises CP cluster replicates data to the CC cluster using Cluster Linking. You must create a simple Cluster Link between CP (Source) and CC (Destination) on a confluent cloud and configure it to replicate critical topics and Consumer Offsets. Finally, you'll perform a simulated failover by using promote/failover to switch consumption to the mirror topic on CC.

Create CC dedicated cluster for DR

<img width="1286" height="535" alt="image" src="https://github.com/user-attachments/assets/2a4f04c5-82e9-4d77-88e4-a36b40b2483a" />

Create cluster linking on CC

<img width="1376" height="1021" alt="image" src="https://github.com/user-attachments/assets/4578f20b-b585-4952-ae52-a649854a8890" />


Create source-initiated cluster linking

```bash
kubectl exec -it kafka-0 -n confluent -c kafka -- bash

cat >/tmp/cluster-link.properties <<'EOF'
link.mode=SOURCE
connection.mode=OUTBOUND
bootstrap.servers=pkc-9prz1y.us-east-1.aws.confluent.cloud:9092
ssl.endpoint.identification.algorithm=https
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
  username="ZOCB75HEOY57XDG2" password="cflt2/5Sb3oOj3M9PCZA3VAiyMpbvJCCsMiemT3IKw6TvDZN9ZmTa9mazSRRvVog";
auto.create.mirror.topics.enable=true
consumer.offset.sync.enable=true
consumer.offset.sync.ms=10000
local.listener.name=INTERNAL
local.sasl.mechanism=PLAIN
EOF

/bin/kafka-cluster-links \
  --bootstrap-server kafka.confluent.svc.cluster.local:9071 \
  --create \
  --link cp-to-cloud-dr \
  --config-file /tmp/cluster-link.properties \
  --cluster-id lkc-73v8vp \
  --command-config /tmp/client.properties
```
<img width="678" height="344" alt="image" src="https://github.com/user-attachments/assets/7fc51a02-7e37-4f1a-88e9-94db7a694e10" />

<img width="544" height="144" alt="image" src="https://github.com/user-attachments/assets/9bdb66f6-96e9-4f75-9e39-469f7ce89361" />

Validate the mirror topic in CC

<img width="1384" height="1292" alt="image" src="https://github.com/user-attachments/assets/e3dce053-e5ec-43c3-9c43-17e864128429" />

## 2. Data Ingestion with Self-Managed Connectors 
One of the use cases follows strict security and networking, hence database connection details are not shareable on fully managed connectors. So the team wants to utilise the Self Managed Kafka Connect cluster to ingest data into a confluent cloud. You need to configure the Connect workers with the necessary Bootstrap Servers and Authentication details for CC. Then, deploy a Source Connector (e.g., SQL Server) on the self-managed cluster to read local data and write it to a CC topic, validating that the data flows correctly.


