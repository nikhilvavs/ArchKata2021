## A timer service

###Status
Accepted

### Context
There are multiple use cases in the system for events to be executed without any human driven triggers.
A few use cases for this are listed below.
1. Call ChargeUser at the end of billing cycle. (Timer created after customer buys a plan)
2. Call VerifyTicketSLA 7 days after ticket create. (Timer created after a ticket is created in our system to verify 
that the ticket is closed within 7 days(actual business logic may be different)
4. Call NotifyUser a few hours before the slot (Potential Future usecase - if the matching process is changed) 


### Decision
Create a timer service which can be configured to call an endpoint at the requested time. 

#### Reasons:
1. Timers need to be fired reliably without losing state.
2. A Cron will be lost if the instance restarts.
3. Storing these in each service and ensuring that they are called reliably duplicates effort in multiple services. 

### Alternatives
1. This can managed manually through admins, but a systematic way helps identify/potentially prevent any losses.

### Consequences
1. We have to maintain a new service that needs to be available to take requests from different services.
2. This service also needs to be call different services, so the service-mesh should be able to support calling a service by name. 