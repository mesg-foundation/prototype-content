# Etherstellar

## Introduction

In this document we will present Etherstellar, a echosystem of tools to create and manage all your application related to the Ethereum blockchain.

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

List all the events of your contract. An event is a emitted directly by your contract to notify some informations. From the dashboard you can so see all the data your contract emit and you will be able to connect any actions to this event (more details in the **Ethereum Blockchain Real Time Actions/Notification** section)

#### Functions

Visualize all the functions of your contract, all the arguments to call it and what this function returns. When you work with some contract created by someone else (teammate, open source contract or an already deployed contract) this is really usefull to have a clear idea of what this contract can do and how.

Also from any function you will be able to create a specific private API endpoint that will sign your transaction for you and execute the function and you don't have to worry to connect to the blockchain and do this yourself (more details on **Ethereum Blockchain Transactions** section)

### Ethereum Blockchain Real Time Actions/Notification

### Ethereum Blockchain Transactions

### Ethereum Contract Marketplace

### Etherstellar CLI
can create triggers directly from command line, run them, log etc...