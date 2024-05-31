# Configuration

Very few configuration parameters are actually required for running the node daemon (running with the 'daemon' command). The only requirements are:

* The Algorand node to connect to for 'this' node and its ADMIN key token.\
  Each Reti node MUST connect to a distinct Algorand node as the participation keys per-node are 1:1 mapped to specific pools you've assigned for your validator.
  * Set through **ALGO\_ALGOD\_URL** environment variable, or will also use **ALGORAND\_DATA** env variable and read settings from the algod.net, algod.admin.token files
  * If running on same machine, this will likely be:\
    ALGO\_ALGOD\_URL=http://localhost:8080
  * **ALGO\_ALGOD\_TOKEN**: (if not using ALGORAND\_DATA) Set this to the value contained in the algod.admin.token file inside your node's data directory.
* Validator ID (**RETI\_VALIDATORID** env var or -validator cmd line arg)
  * The validator ID assigned to you when you added yourself as a validator. This is simply a sequential number in the protocol.
* Node Number (**RETI\_NODENUM** env var, -node cmd line arg, or -usehostname flag)
  * A number from 1 - X (the maximum number of nodes allowed in the protoclo)
  * This can be set automatically for kubernetes installations using the (--usehostname flag)
* Mnemonics for the owner or manager address.
  * The manager address is always preferred as it has fewer rights. Because the node daemon needs the keys to an address it can issue transactions to the validator/pool contracts with, and is thus a 'hot wallet', it needs to be an account with minimal rights. The manager address can only trigger epoch payouts, add new pools, and having a pool going online or offline.
  * The owner of a validator can change the manager address at any time.

{% hint style="warning" %}
The manager account **MUST** be initially funded with at least a few ALGO ! The manager account is what the node daemon uses to issue transactions on behalf of the validator. It issues transactions to have pools go online/offline, and most importantly, the Epoch update calls which pays the validator their commission as well compounding staker balances.

Validator pay outs will send up to 1 ALGO to the manager account if its spendable balance drops below 1 ALGO. This ensures the manager automatically stays funded for epoch updates.
{% endhint %}

When starting, the specified mnemonics are checked and must correspond to an account that is either the owner or manager of the specified Validator.

#### Environment loading

There are a number of environment variables that can be specified. Any of these can be specified as **environment variables,** **command line flags** (in many cases), or in **environment files**.

The following files are checked and loaded (if found in working directory) in the following order:

.env.local\
.env\
.env.{network} (the name of the specified network - defaulting to mainnet)\
{envfile} (if an environment file override was speciied on command line or via RETI\_ENVFILE env. var)

#### Mnemonics

Mnemonics for the manager address at the moment are read from the environment (in any manner defined above), by reading all environment variables with '\_MNEMONIC' as part of the environment variable name. The value is expected to be a space-delimited 25 word Algorand mnemoic.

If none of the addresses loaded via these mnemonics match the owner or manager of the specified validator ID, the node daemon will refuse to run.

#### Global Command Line options

<table><thead><tr><th>Command Line Option</th><th width="192">Environment Variable</th><th></th></tr></thead><tbody><tr><td>--envfile {file}</td><td>RETI_ENVFILE</td><td>Specify an additional file to process like an .env file.</td></tr><tr><td>--validator</td><td>RETI_VALIDATORID</td><td>The validator ID of the validator running commands like the daemon</td></tr><tr><td>--node</td><td>RETI_NODENUM</td><td>The node number (1 - 6)</td></tr><tr><td>--usehostname</td><td></td><td>When running in Kubernetes environments, this sets the node number assuming the node daemon is running as a sidecar alongside an algod container. The suffix of the (presumed statefulset) hostname is used for the node number. Since stateful sets are sequentially numbered starting from 0, the node number will be the hostname suffix + 1.</td></tr><tr><td>--network</td><td>ALGO_NETWORK</td><td>Override the network to use (sandbox, betanet, testnet, mainnet, voitestnet)</td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

#### Options for 'Daemon' mode

| Command Line Option |                                                                                                                                                      |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| --port              | <p>Defaults to 6260.<br>Specifies the port the Node Daemon listens on for its HTTP endpoint.<br>The paths:<br>/ready<br>/metrics<br>are exposed.</p> |

#### Environment only overrides (.env files as well):

Some options are only overridden through environment variables as they're more low-level.

* ALGORAND\_DATA
  * If detected, the address to connect to will be read from $ALGORAND\_DATA/algod.net and the admin token will be read from $ALGORAN\_DATA/algod.admin.token.
* ALGO\_ALGOD\_URL
  * The Algorand node address (ie: http://localhost:8080) to connect to for this daemon's node. Each node daemon should connect to its own independent algorand node.
* ALGO\_ALGOD\_TOKEN
  * The ADMIN token to use when making calls to the Algorand node for this daemon
* ALGO\_ALGOD\_HEADERS
  * Comma delimiter key:value pairs to set in the headers passed in calls to the algod API. Seldom needed but may be required if you have additional policies defined in some type of fronting load-balancer or proxy.
