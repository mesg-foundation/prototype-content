## Connector

The connector will have the responsability to send some events to the dispatcher.

### Docker image

The application of the connector needs to run on a docker and have a `docker-compose` config. This ensure that the connector can connect to other services (like a Ethereum node, a specific database etc...). We also need to provide to the application a really easy way to communicate between the dispatcher and itself. Also the connector will need to (un)register itself. All thoses steps will be common to every connectors so we actually can create and plug this architecture to the actual connector.

As communication system we can use [RabbitMQ](https://www.rabbitmq.com) which is a famous open source protocol to exchange messages. We also need to provide the business model to manage all the autentication for the connector when contacting the `dispatcher`. In order to do that we will create a specific docker image that will need to implement the following features :

- Create the channel to communicate with the connector
- Start the connector
- Notify the dispatcher that the connector is running with all the meta informations (address to access to it, kind of connector ...)
- Listen the channel from the connector for all incoming events
- For every events, ask the matching triggers to the dispatcher
- Filter the events with the triggers list
- Send the valid events to the dispatcher
- Receive actions from the dispatcher and send them to the connector (like decode some parameters for exemple)
- Unregister the connector to the dispatcher when needed

There is two ways to do that:

#### Docker in Docker

This is the solution where we will run this application and this application will run inside it the docker of the connector. This can be a good thing like that every connectors are really separated and we could maybe run multiple connectors at the same time. The only problem is that it's not sure that we can send some messages between the docker parent and the docker children

#### Parallel Dockers

In this case we will execute the docker of the connector but also our own application in a docker and the protocol for communication and just link to the connector container. In this case we will have the same network and the communication is possible but in this solution we have to update the docker configuration of the connector so maybe have some conflicts with some existings services. The solution to fix this is to let directly the developer of the connector to add our applications directly into it's config. In this case we would not have any risk of conflicts.