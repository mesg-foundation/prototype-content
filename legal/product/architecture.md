The idea is to have an architecture that is flexible enough to be able to switch to full decentralised in the futur and easier to maintain and evolve.

## Parts

- **Connector**: This is the part in charge of listening events from any source (eg:  Ethereum transactions, Ethereum contract, Neo transactions, HTTP event, Redis, etc.) and send them to the **dispatcher**. The goal is to have multiple connectors that are listening from the same source in order not to really on a single point of connection. Each individual will be able to run a connector on its computer (like miner). Every connector will receive a reward when its event is processed by the rest of the system. Those connectors will be developed by developers who will receive a reward when its connector is used (even if he doesn't run the actual connector on its own computer).

- **Runner**: This is the part in charge of executing the event's action that the **dispatcher** will send. The runner will be able to execute a lot of different **services**. Each individual will be able to run a runner on its computer (like miner) and will receive a reward when used.

- **Dispatcher**: The dispatcher will be in charge to receive all the events from the different **connectors**. The dispatcher will match events against existing **triggers** and select **runners** to execute the actions. From the result of these actions, the dispatcher will send a reward to the connectors that provided the events and to the runners that executed the actions from the user's balance and also to itself. When all of this is done, the dispatcher will save the logs and the result of the execution.

- **Service**: The service is a specific piece of code that will execute an action from an event on a runner. For example, post to an HTTP endpoint, send an email, save in a database, etc. The services will be developed by developers who will receive a reward when its service is used (even if he doesn't run the actual runner on its own computer).

- **Trigger**: The trigger is a configuration users provide in order to connect a connector's events to a specific service and thus executing action from events.


## Workflow

- The user provides tokens to the dispatcher to be used to pay futur executions
- The user creates a trigger with that uses a connector and a service
- The connector receives a event
- The connector sends the event to the dispatcher
- The dispatcher receives the event
- The dispatcher matchs the event against existing triggers
- If no trigger matchs the event, stop execution
- [optional] The dispatcher asks the connector to decode the event with the trigger's data (in the case of Ethereum Contract, we need to decode the event with the contract ABI)
- The dispatcher selects one runner that can execute the trigger's service
- The dispatcher sends the event + trigger's data to the selected runner
- The runner executes the service set in the trigger
- The runner sends the execution's logs and result to the dispatcher
- The dispatcher saves the logs and result of this execution
- The dispatcher calculates the price of the execution
- The dispatcher rewards the connector from the user's tokens
- The dispatcher rewards the connector's developer from the user's tokens
- The dispatcher rewards the service from the user's tokens
- The dispatcher rewards the service's developer from the user's tokens
- The dispatcher rewards itself from the user's tokens


## Connectors

Every connector should be able to emit an event with some specific data. It should also connect a source of event (ethereum transaction, ethereum contract event, Neo transaction, Http transaction, SNS transaction... whatever).
Everytime the connector receive an event it should convert it in data that the dispatcher will be able to process. It should also be able to be developped by anyone with any language/techno. For that it will provide a `docker-compose` config that will run all the required part of the software needed and will need to define the format of the data it will send

One exemple of configuration for a connector file could be like that 
```yml
type: ETHEREUM_MAINNET_CONTRACT
propagation: all | none # the connector will be propagated and possibly executed for every computer of the network
private: true | false # the connector will be private, nobody will be able to connect it except the creator of the connector or anyone he gives access to
outputs:
    from: String
    to: String
    transactionId: String
    fees: Int
    logs:
        - String
    executedAt: Datetime
    arguments: JSON
run: # docker compose file or maybe just a command line
    version: '2'
    services:
        app:
            image: https://repository.etherstellar.com/ethereum-mainnet-contract
            link:
                - ethereum-node
        ethereum-node:
            image: parity/parity
```

## Runner

The runner will need to listen some events from the dispatcher and will be written in Javascript for start but can be extended to any languages. For that docker will be again used, we will provide a docker image "runner" that can automatically register to the dispatcher and then receive the list of service it needs to run. Once it knows all the services it will fetch the source code of every services. The runner will have the possibility to run one service (or multiple) and service will need to specify what kind of data they need to be able to process the event they will receive. This will be usefull to be able to connect an event from a connector to a service. If the connector define all the data it send and the service all the data it needs we can match and connect them.
One service definition could be something like that:
```yml
title: Webhook
propagation: all | none # the service will be propagated and possibly executed for every computer of the network
private: true | false # this service will be private, nobody will be able to connect it except the creator of the service or anyone he gives access to
inputs:
    arguments: JSON
    endpoint: String
    headers:
        - key: String
            value: String
handler: index.js
```

## The config for the triggers

Those configs will be set by the final user in order to connect an event to a service. It will have 3 parts :
- The connector that it wants to connect to
- The filter on the event he want to process (only transaction for this address, only event of this contract from this address...) based on the data provided by the connector
- The service to connect with all the environment needed for the service (this environment will be merged with the data sent from the connector)

Let's say we want to have the following trigger:
"For every `Transfert` on the `Augur contract` after the `October 20th, 2017` where the amount of the transfer is greater than `10` send a webhook notification to `https://webhook.site/aer1i3h413iheqfuif` with the headers `foo: bar`" we would have the following file:

```yml
augur-transfer-after-12-oct-with-more-than-10-token:
  connector:
    type: ETHEREUM_MAINNET_CONTRACT
  event-filters:
    - to:
        eq: 0xe94327d07fc17907b4db788e5adf2ed424addff6
    - logs:
        include: 134014335782478591349648913289461...
    - executedAt:
        gt: '2017-10-12T00:00:00.000Z'
    - arguments.Amount:
        gt: 10
  service:
    service: webhook
    data:
      endpoint: https://webhook.site/aer1i3h413iheqfuif
      headers:
        foo: bar
```
