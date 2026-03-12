<img width="665" height="283" alt="image" src="https://github.com/user-attachments/assets/9e9780f5-8f4e-4a87-bebf-fd30a309b509" /> Here is Assignment 3:

Deploy Confluent Kafka on Kubernetes with SASL/PLAIN over PLAINTEXT to add an authentication layer between clients and brokers. Configuring SASL/PLAIN enforces username/password authentication for producers, consumers and inter-broker communication, which reduces the risk of unauthorised access inside the cluster and simplifies integration with Kubernetes Secrets for credential management.

Also make changes in your respective producers and consumers.

Why this assignment?  This requirement is essential for the startup as this use case requires approval from the IS before moving the application to production.

---

## 1. Create secret and apply updated Kafka CR to turn on SASL/PLAIN on the external listener (for client traffic)
<img width="665" height="522" alt="image" src="https://github.com/user-attachments/assets/03ae9385-9408-4a8b-9c66-2dfdbc5abc11" />
<img width="665" height="283" alt="image" src="https://github.com/user-attachments/assets/68d3cadb-ec0d-4071-9880-873152dbfbdc" />



## 2. Create a python client to use as producer/consumer with SASL mechanism:
<img width="667" height="551" alt="image" src="https://github.com/user-attachments/assets/3a2d99a6-77d2-416e-8b0a-5319e0d5cd39" />
<img width="665" height="569" alt="image" src="https://github.com/user-attachments/assets/5a3da0f2-2413-4698-bb05-46734e558bff" />
<img width="667" height="477" alt="image" src="https://github.com/user-attachments/assets/2fd61fbf-80c9-42dc-8322-f987beb37043" />

Produce event to topic:
<img width="571" height="161" alt="image" src="https://github.com/user-attachments/assets/d4eebc1d-9c79-4527-a099-cb39768d53d6" />


Poll event from topic:
<img width="422" height="156" alt="image" src="https://github.com/user-attachments/assets/de9627c7-150f-4340-8330-d79fbd738d57" />

