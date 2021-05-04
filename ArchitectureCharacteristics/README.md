# How the proposed system tackles the shortcomings of the current system

### Change is difficult and risky
- The proposed system is designed as a distributed micro-service architecture.
- Each component has a small set of responsibilities.
- Changed in each component can be made and tested without affecting other components.

### No proper accounting for tickets/ticket-SLA
- Each ticket creation will emit a message in the queue, this will minimize the risk of losing the ticket creation requests.
- Each ticket creation will also set a timer in the timer service to track the ticket-SLA. If there has been no action on the ticket, the ticket will be again put for matching.
- The timer-service will similarly track the SLA of tickets at each stage in the ticketing workflow.

### Wrong matching of experts
- With matching as a isolated component, different strategies for matching can be tested easily. A/B testing can be performed on matching service instances for further improvement.
- With increased reliability in notification-delivery through Message-queues, there will be far less chances of experts not receiving correct notifications.

### Reliability & Performance issues on scale
- With the micro-service architecture, we can scale each of the component horizontally without needing to scale other components.
- The user-facing services can be scaled horizontally to handle the increased traffic.
- Tier-1 storages will have read-replicas that will be used for read operations.

### Availability
- All ciritcal storage will have read-replicas to serve the read traffic and to reduce load on the write instances.
- Services requiring high write traffic will have will have multi-master replication to divide the write load among multiple instances.

