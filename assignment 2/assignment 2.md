# Assignment 2:

## 1. Now the organisation wants to ingest data from a PostgreSQL database hosted on AWS RDS into Kafka to support real-time analytics and downstream processing.

## 2. You are required to design and implement a solution using the Confluent Postgres CDC (Debezium) Kafka Connector as a native integration.

---

**Flow**

`AWS RDS Postgres → CFK Connect (Debezium Postgres connector) → Kafka topics (Avro) → Schema Registry → Downstream consumers`

Components:

- **AWS RDS Postgres** with logical replication enabled
- **CFK Kafka** (KRaft) as in Assignment 1
- **CFK Connect** cluster with Debezium Postgres plugin
- **Schema Registry** (CFK) for Avro schemas
- **Connector CR** for Postgres CDC, including SMTs for:
  - Debezium envelope unwrapping
  - PII masking

Connect Cluster (CFK) with Debezium Plugin

Extend the existing Connect CR from Assignment 1 to install **Datagen** and **Debezium Postgres** from Confluent Hub.
<img width="569" height="504" alt="image" src="https://github.com/user-attachments/assets/566a4611-a716-49b8-b1ca-43b8d5ad1988" />


---

## 3. The PostgreSQL source tables contain sensitive fields (PII). Therefore, your solution must include a mechanism to mask, redact, or hide sensitive columns before the data is published to Kafka topics.

- Uses Debezium Postgres CDC
- Unwraps the Debezium envelope so values are just the row
- Masks PII columns (e.g. email, phone_number, ssn)
- Connector SMT <img width="603" height="299" alt="image" src="https://github.com/user-attachments/assets/9f0c7fc6-49e7-4ff7-8f16-e9a947bfc19f" />
- source table contains sensitive fields (email, phone_number, ssn) <img width="1810" height="99" alt="image" src="https://github.com/user-attachments/assets/01422381-752e-40a8-918e-56c848a178de" />
- the message appeared masked in topic <img width="1454" height="593" alt="image" src="https://github.com/user-attachments/assets/d6c4bf3c-cc23-40bc-aa3b-79f525bdd5ad" />



## 4. The connector deployment must be compatible with CI/CD pipelines, and therefore must be deployed using kubectl as Kubernetes connector resources.

---
<img width="658" height="343" alt="image" src="https://github.com/user-attachments/assets/9c999251-c237-4ea1-a505-16c55a5fc6b6" />
<img width="508" height="74" alt="image" src="https://github.com/user-attachments/assets/de9eba72-06ed-4121-ba82-a37fb9019ab5" />



## 5. Additionally, the solution should ensure that downstream applications are able to consume the ingested data in Avro format, leveraging Confluent Schema Registry.

---
- The topic schema is Avro format and using backward compatibility in Schema Registry.
<img width="1755" height="1236" alt="image" src="https://github.com/user-attachments/assets/e23bdad8-a074-4819-b2d5-d9d78b30a202" />
