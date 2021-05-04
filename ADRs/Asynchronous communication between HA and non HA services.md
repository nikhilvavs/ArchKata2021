## Asynchronous communication between HA and non HA services

###Status
Accepted

### Context
The services that interact with user flow directly will have high availability requirements. If the other systems are
 down, this should not affect the functionality of user flows. 

### Decision
When a HA system needs to talk to a non HA system/events that are not critical to be executed right away, asynchronous 
communication methods like kafka will be used.

The ticket state change events will be published to different topics like _ticket-created_, _ticket-completed_. Different 
services will be listening to these topics and take actions based on them (For e.g. matching service could listen to the 
_ticket-created_ topic and then pick the expert to be assigned to this topic.)

#### Reasons:
1. All the user interacting systems will be expected to be highly available. If it depends on a non HA system,
 it will affect the overall performance of the user interacting system. 
2. If a non-user interacting system is down, there should not be any impact on the user flows.   
3. In the event of a consuming service degradation, backpressure is automatically applied instead of overloading the system.

### Consequences
1. Because, there could be significant time delta between an event happening and the event being consumed, we need to 
ensure the validity of the event before processing it.
2. Additional infrastructure (like kafka) needs to be supported.  