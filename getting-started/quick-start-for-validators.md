# Quick Start For Validators

## Set Up a Node

The first step is to run a node. Find a number of different resources in [Running a Node](../resources/running-a-node.md) if you don't already have one running.

## Run the Reti Node Daemon

Validators need to run the [Reti Node Daemon](../technical-implementation/reti-node-daemon/) alongside their node.

## Define Validator

Validators can define a number of parameters - some of which are immutable. More information on each parameter can be found in [Validators](../core-concepts/validators.md).

**Mandatory Parameters**

* Owner address (preferably a cold wallet)
* Manager address (hot wallet)
* Epoch Length (Payout frequency) in rounds \[blocks]
* Validator commission rate
* Validator fee address (account which receives commission)
* Minimum entry amount
* Number of pools per node (participation keys) - Maximum of 3
* Max number of nodes - Maximum of 8
  * This means a maximum of 24 pools can be created.

**Optional Parameters**

* Link an NFD to the Validator - this enables linking certain data to their Validator details page like a profile picture and bio.
* Reward token / reward rate&#x20;
* Sunsetting information
* Token / NFD Gating:
  * Supported gating options include:
    * Tokens/NFTs by creator and min amount
    * Specific ASA ID(s) \[up to 4]
    * Tokens/NFTs created by any address linked within a particular NFD.
    * Owning a segment of a particular NFD Root

