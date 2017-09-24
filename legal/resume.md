# Etherstellar

## Introduction

In this document we will present Etherstellar, a ecosystem of tools to create and manage all your application related to the Ethereum blockchain.

Blockchain technology is now more than ever a really important and future-proof technology. Before Bitcoin this technology has been used for a single purpose, finance. Ethereum blockchain brings some new advantages and allow people to code "smart contracts" on the blockchain and this opened the door to millions of possibilities.

We saw many projects emerge, from lottery to authenticity network to ensure copyright or proof of authenticity, or even insurance companies or even banks. All thoses projects have one common thing, Ethereum Blockchain.

Today it's already possible to create amazing applications with the blockchain but it's also really complicated and time consuming to create one.

Etherstellar will reduce the complexity and bring all the necessary tools for all the developper to create there applications and all the companies who wants to start using Blockchain technology.

## Services

Our plan is to release different tools for developers and companies that may help them to create easily and quickly there applications.

To do so we had to think about the basics of every applications, what a good applications needs, what a Blockchain brings and what is missing from it and we realised that it was still a lot of tools to create to make Ethereum Blockchain even more amazing. 

For now you can store your data in a way that you can be sure it will be perfectly safe, no possible alteration of your data, no possibility to delete them or "hide" them, everything on the blockchain stays on the blockchain and will not be changed. This is definitely the good point and that is why so many companies are interested about blockchain technologies. But, there is also many things missing. It's actually quite complicated to visualise your different applications on the blockchain (smart contract), visualise the data the contains, interact with thoses data or even know when thoses data or contracts are called. Isn't it normal to know when your user just registered or when you just received a payment in your application.

With all that in mind we decided to create some tools to fix those problems

### Ethereum Blockchain Contract Management

The first tool is to manage your smart contracts. When you create a smart contract you have no places to get all the informations about it except your codebase but you will not have your deployed adress on your codebase usually so it can be a bit messy to know what addresses your contracts are deployed to.

When you access to Etherstellar dashboard you can now have access to all the contracts you deployed and you can also add some contract you are interested in even you are not the one developing it, this can be really usefull if you want to use thoses contracts.

On this dashboard you will be able to do more than just store your contracts with the associated addresses, you can in a simple click access to the the [etherscan](https://etherscan.io) data and more important, have the details of your contracts.

This dashboard also comes with a view of all your contract data and functions and can act as a documentation to share the knowledge to your team in a single place.

#### Constructor

Display the constructor of your contract to remind you all the arguments to create this contract and remind you that you have to test that this constructor can be called only one time

#### Constants

Check all the available data of your contract and see the actual value, we connect to the blockchain for you and you can see the value of your contract data just in the dashboard with nothing else to do

#### Events

List all the events of your contract. An event is a emitted directly by your contract to notify some informations. From the dashboard you can so see all the data your contract emit and you will be able to connect any actions to this event (more details in the **Blockchain Real Time Actions/Notification** section)

#### Functions

Visualize all the functions of your contract, all the arguments to call it and what this function returns. When you work with some contract created by someone else (teammate, open source contract or an already deployed contract) this is really usefull to have a clear idea of what this contract can do and how.

Also from any function you will be able to create a specific private API endpoint that will sign your transaction for you and execute the function and you don't have to worry to connect to the blockchain and do this yourself (more details on **Blockchain Transactions** section)

### Blockchain Real Time Actions

Based on the Ethereum contracts we offer the possibility to connect your contract events to any actions you needs for your applications. On Ethereum, when a contract function is called, this function can (if written in the source code) emit some events. Thoses events are really usefuls and will send some parameters defined by the developer of the contract.

With this your contract can notify that something happened (user registered, payment done, data written or whatever action the contract does) but like any event system, it's really useful when you can connect on it. It's actually quite complicated to connect to thoses events, you need to have a full copy of the blockchain, develop some specific code to listen to the actions of your contract and configure your connection between your node and your listener... this is some extra work your business don't need to do.

As a platform we provide this service and we let you easily connect any event of your contract with the action you need for your business.

#### Webhooks

This is the most generic way to listen about your event. Every time an even is triggered you will receive all the data from this event directly on the webhook you defined. This webhook will be private and only visible by you and your team (if you defined one in the dashboard). Of course endpoints are not 100% safe, some people can still guess the endpoint URL so a common practice is to define some specific private parameters between the service that send the webhook notification and the one that receive it. You can so define all the headers you want in order to valide them on your server and ensure that the event is relevant.

With this feature you can connect any existing application backend.

#### Cloud Functions

Sometimes you don't need or have a proper backend to manage thoses events or just don't want to point directly to your application to have a more microservices architecture. Also you don't want to manage the deployment of new cloud functions. If you match one of thoses criteria, the cloud functions are for you.

With our cloud functions you can directly, from the dashboard create your own functions that will receive data from your contract event. Those functions will be automatically deploy and you don't have to worry about the infrastructure to manage and scale them, everything is all included and you can just code your action.

#### Slack notifications

If you just need a notification of any action, many company use Slack wich is an awesome too and so we decided to use it too and to let you receive your notifications directly in your Slack. Simply follow the steps from Slack to create your endpoint and connect it in the dashboard and you are ready to receive notifications of your contract in real time in your slack with all the details that your contract gives.

#### Emails with Sendgrid

Emails are important part of a business, for you customers but also for you to know when something happen on your application and interact accordingly. From the dashboard you can connect any contract event to an email from your own Sendgrid account. This email is fully customizable, subject, receiver, emitter and of course the body, all those email information can be edited with a template engine that let you replace some content with the actual value of your contract. 

For exemple, your contract emit the email of your user when he pay with the price he paid (with the parameters email and amount), you can so send a email with the receiver `{{ email }}` and a body like `Thanks for paying {{ amount }}`. Like that your user will receive the email with the confirmation of his payment. Of course you can send any kind or transactional emails you want

#### Important part 1

All those services are really extendable and we plan to add more of them according to your needs. For now this is base on **Ethereum contract** but we plan really soon to support other events such as **Ethereum transactions**, **Bitcoin transactions** and much more. With this feature you will be able to execute the same services just with any events from any blockchain.

#### Important part 2

Because of this list of service will grow as the product evolve we think about the possibility for you to create your own connectors. We plan to provide a documentation for that and a testing environment like that you will be able to develop any connector you like and submit it directly from the dashboard or from the open sourced git repository. This kind of connectoy may lead to a marketplace where the developer can even get some money from the sells of his connector

### Blockchain Transactions

### Ethereum Contract Marketplace

### Etherstellar CLI
can create triggers directly from command line, run them, log etc...