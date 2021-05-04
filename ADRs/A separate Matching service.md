## A separate matching service

###Status
Accepted

### Context
The tickets created by the users need to be matched to an expert based on the locality, skill set and availability of
 experts. 

### Decision
Create a new matching service that reads the state of tickets and available experts and matches them. This matching 
service consumes _ticket-created_ events from the ticket service and matches them to the right expert. 

#### Reasons:
1. Different matching algorithms (like JIT-Just In Time matching, or batch) could be experimented easily without making
 any changes to the user facing ticket-service.
2. Consuming events from a _ticket-created_ topic lets the matching service match experts independently of the ticket 
creation process and have more relaxed availability constraints focusing on optimising the match rather than on
 availability of the system.

### Consequences
1. We have to maintain a new service that needs to be available to take requests from different services.
2. We need to handle distributed transactions involving ticket and experts across multiple systems. 