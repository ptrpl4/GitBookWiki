# Monitoring

process of collecting, analyzing, and acting on logs and metrics data in real-time

Monitoring (the overall process)

- Logs - application logs
- Metrics - technical events with system data and info
- Analysis
	- Visualisation for tech data
	- Visualisation for Business metrics and goals
- Action
	- Alerting based on data
	- Alerting based on data analyse (anomaly)

## Data Strategy
### Pull Monitoring Data Strategy

In a pull monitoring data strategy, the monitoring system actively requests data from the monitored systems or applications at regular intervals. This approach is also known as "polling".

#### Disadvantages

1. **Latency**: There can be a delay between the time the data is generated and the time it is collected by the monitoring system.
2. **Inaccuracy**: If the monitoring system is not polling frequently enough, it may miss important data or events.
3. **Scalability**: As the number of monitored systems or applications increases, the monitoring system may become overwhelmed with requests.

### Push Monitoring Data Strategy

In a push monitoring data strategy, the monitored systems or applications actively send data to the monitoring system as it is generated. This approach is also known as "event-driven" or "streaming" monitoring.

#### Disadvantages

1. **Complexity**: Push monitoring can be more complex to implement, especially in large-scale environments.
2. **Overhead**: The monitored system or application may incur additional overhead in sending data to the monitoring system.
3. **Noise**: Push monitoring can generate a high volume of noise or irrelevant data, which can make it difficult to identify important events.

## Data Analysis 

### Anomaly detector

Changes in data that are not critical but may be symptoms of problems.
