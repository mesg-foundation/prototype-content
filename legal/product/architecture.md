The idea is to work on the infrastructure to have something more flexible to switch in full decentralised in the futur and something easier to maintain and evolve.

As discussed we have 3 main parts on the project:

- **Connector**: This is the part in charge of listening some event from any source we need, we can listen for Ethereum transactions, Ethereum contract, Neo transactions or even a HTTP event or Redis or whatever. Those connectors will be developed by us but also by the community and every connector will receive a reward when it's event is processed

- **Runner**: The runner is an instance of code that is executed in a specific context on any computed. This will have the responsibility to execute the service the user want to run, exemple call a webhook, write the event in a database or even trigger a transaction on a blockchain. For every execution the runner will receive a reward.

- **Dispatcher**: The dispatcher will be in charge to receive all the events from the different **connectors**, then select one. Once the connector is selected and verified, it will match all the configurations that match this specific event and select a **runner** to execute the action. From the result of this action, the dispatcher will send a reward to the selected connector and the selected runner (maybe also a small reward to every others tiers that were participating but didn't get choose). The rewarding is not necessarily done for every event processed and can be done at the end of the day or the month etcc... When all this is done it will log the result of all this somewhere to be able to replay everything that happen.


The worflow will be as follow:

- Event from the connector
- Send the event through one protocol (http, p2p, amqp, ...)
- The dispatcher receive the event(s)
- The dispatcher check if the/which event is valid
- The dispatcher get all the matching config for this event
- The dispatcher ask the event provider (connector) to decode the event with the config retrieve
- The dispatcher select a runner that can execute the action in the config
- The dispatcher send the event + some configuration to the runner
- The runner execute the correct service and return the logs
- The dispatcher save the result of this execution
- The dispatcher calculate the price of the execution
- The dispatcher reward the connecter (or maybe all)
- The dispatcher reward the service (or maybe all)


## Connectors

Every connector should be able to emit an event with some specific data. It should also connect a source of event (ethereum transaction, ethereum contract event, Neo transaction, Http transaction, SNS transaction... whatever).
Everytime the connector receive an event it should convert it in data that the dispatcher will be able to process. It should also be able to be developped by anyone with any language/techno. For that it will provide a `docker-compose` config that will run all the required part of the software needed and will need to define the format of the data it will send

One exemple of configuration for a connector file could be like that 
```yml
connectorX:
    type: ETHEREUM_MAINNET_CONTRACT
    propagation: all | none # the connector will be propagated and possibly executed for every computer of the network
    private: true | false # the connector will be private, nobody will be able to connect it except the creator of the connector or anyone he gives access to
    data:
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

## Runner/Service

The runner will need to listen some events from the dispatcher and need to be able to be written in any language. For that docker will be again used. The runner will have the possibility to run one service (or multiple) and service will need to specify what kind of data they need to be able to process the event they will receive. This will be usefull to be able to connect an event from a connector to a service. If the connector define all the data it send and the service all the data it needs we can match and connect them.
One service definition could be something like that:
```yml
serviceX:
    title: Webhook
    propagation: all | none # the service will be propagated and possibly executed for every computer of the network
    private: true | false # this service will be private, nobody will be able to connect it except the creator of the service or anyone he gives access to
    data:
        arguments: JSON
        endpoint: String
        headers:
            - key: String
              value: String
    run:
        version: '2'
        app:
            image: https://repository.etherstellar.com/service-webhook
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
