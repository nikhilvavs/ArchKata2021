## Migration

Follow the links for ADRs.

## Approach

Our approach is to minimise the changes in monolith for migration so we reduce the risk. Due to reliability issues in the current monolith system we will refrain from making major changes in the existing code. We will gradually create a new system around the edges of the old system. We will complete the migration of the existing system to the [new system](./FinalArchitecture/) in below phases. To gradually cut over to a new system, we will begin by identifying a subset of assets that we'll start with the new system. We will start with the simple assets because they are quick to get going.

To make this work smoothly we'll need a mechanism to migrate assets from the old application to the new one and back again. Reverse migration will reduce the risk and to handle cases where an asset may dynamically change in such a way that the new application can no longer handle it e.g: A new feature implementation in an old system.

### Phase 1: New API Gateway & UI components

We will start by creating an api-gateway that will act as a routing proxy between the new system and the old system. This routing layer will be able to resolve the regions and routing components based on the configurations. To decouple user interface aspects from existing monolith, new UI components will be built (link ADR). We will introduce simple APIs in existing monoliths to expose the existing UI interactions. The new UI components will proxy to these new APIs via api-gateway.

Along with API-Gateway, below components for the new system will aid in smooth migration.
- _Routing Component_ - Will handle routing of user traffic between old and new system based on the configurations and constraints such as locality (geo-location), api, features etc..
- _Region Resolver_ - The new system will support multi-zone multi-region deployment capability. Region resolver will resolve the region/zone during failover scenarios.
- _CI-CD_ - We will incorporate CI-CD workflows in order to ensure safe code/config rollout.

### Phase 2: Identity service

We will build Identity service in the second phase. Existing data will be back filled into the new system. We will have a proper validation pipeline built in to identify the gaps in the new and old components and rectify the same. With this change in place both UI components and new Identity systems can start to evolve independently.

### Phase 3: Ticketing, Matching and Notification

New ticketing system will provide a generic ticket state machine and matching stack. The new system will start with a simple ticketing system catering existing business use-cases but with abstractions built in so that any new system can easily onboard. Similarly we will introduce a matching stack as a separate component so that we can try different types of matching strategies with ease. The system will provide easy integration of new ML algorithm integrations to improve the matching efficiency. 
Notification will be a new component that will support not only SMS text service but will be able to cater in-app andd native notifications to users in future. Also it can integrate with different third-party vendors to optimise cost and scale on demand dynamically to improve overall notifications delivery efficiency.


### Phase 4: Payments and Billing, Knowledge base & Reporting

Payments and Billing gateway will be a new component which will be PCI compliant. The new billing system will have the ability to integrate with different payment methods in the future. In the new system all the Knowledge base related data will be kept in a searchable database. Reporting system will leverage data lake of the new system.
