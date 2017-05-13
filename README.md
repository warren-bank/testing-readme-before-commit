### [dapp-deploy](https://github.com/warren-bank/dapp-deploy)

#### Description:

Command-line tool to deploy Ethereum contracts to a blockchain

#### Usage:

```bash
$ dapp-deploy --help

Tool to use in combination with 'dapphub/dapp'.
Uses artifacts generated by "dapp build".
Deploys compiled contract(s) to an Ethereum blockchain.

Each deployed contract address is saved to a JSON data file.
Format is a hash table:
    Ethereum network ID => array of addresses

Ethereum network ID represents a unique Ethereum blockchain.
Enables deployment to multiple chains.

A frontend Dapp can determine the current Ethereum network ID.
For each contract, the Dapp can perform the necessary lookup
in the corresponding hash table of deployment addresses.
Using web3.js, only 2 data files are needed per contract:
    ./out/CONTRACTNAME.abi
    ./out/CONTRACTNAME.deployed

Usage: dapp-deploy [options]


Options:
  --all                         Deploy all contracts   [boolean] [default: true]
  -c, --contract                Deploy only specified contract(s)        [array]
  --params                      Parameter(s) to pass to contract constructor(s)
                                note: In most situations, each contract having a
                                constructor that accepts input parameters should
                                be deployed individually, rather than in a
                                batch. Please be careful.  [array] [default: []]
  --value, --wei                Value (wei) to pass to contract constructor(s)
                                note: In most situations, each contract having a
                                payable constructor should be deployed
                                individually, rather than in a batch. Please be
                                careful.                   [number] [default: 0]
  --gas                         Gas to send with each transaction
                                note: In most situations, it would be better to
                                not use this option. By default, the amount of
                                gas sent is an estimate.                [number]
  -h, --host                    Ethereum JSON-RPC server hostname
                                                 [string] [default: "localhost"]
  -p, --port                    Ethereum JSON-RPC server port number
                                                        [number] [default: 8545]
  --tls, --https, --ssl         Require TLS handshake (https:) to connect to
                                Ethereum JSON-RPC server
                                                      [boolean] [default: false]
  -a, --aa, --account_address   Address of Ethereum account to own deployed
                                contracts                               [string]
  -A, --ai, --account_index     Index of Ethereum account to own deployed
                                contracts.
                                note: List of available/unlocked accounts is
                                determined by Ethereum client.
                                                           [number] [default: 0]
  -i, --input_directory         Path to input directory. All compiled contract
                                artifacts are read from this directory.
                                note: The default path assumes that the current
                                directory is the root of a compiled "dapp"
                                project.             [string] [default: "./out"]
  -o, --od, --output_directory  Path to output directory. All
                                "contract.deployed" JSON files will be written
                                to this directory.   [string] [default: "./out"]
  -O, --op, --output_pattern    Pattern to specify absolute output file path.
                                The substitution pattern "{{contract}}" will be
                                interpolated.
                                note: The substitution pattern is required.
                                                                        [string]
  --help                        Show help                              [boolean]

Examples:
  dapp-deploy                               deploy all contracts via:
                                            "http://localhost:8545" using
                                            account index #0
  dapp-deploy -A 1                          deploy all contracts via:
                                            "http://localhost:8545" using
                                            account index #1
  dapp-deploy -h                            deploy all contracts via:
  "mainnet.infura.io" -p 443 --ssl -a       "https://mainnet.infura.io:443"
  "0xB9903E9360E4534C737b33F8a6Fef667D5405  using account address
  A40"                                      "0xB9903E9360E4534C737b33F8a6Fef667D
                                            5405A40"
  dapp-deploy -c Foo                        deploy contract: "Foo"
  dapp-deploy -c Foo --params bar           deploy contract: "Foo"
  baz 123 --value 100                       call: "Foo('bar', 'baz', 123)"
                                            pay to contract: "100 wei"
  dapp-deploy -c Foo Bar Baz                deploy contracts:
                                            ["Foo","Bar","Baz"]
  dapp-deploy -c Foo -o                     generate:
  "~/Dapp_frontend/contracts"               "~/Dapp_frontend/contracts/Foo.deplo
                                            yed"
  dapp-deploy -c Foo -O                     generate:
  "~/Dapp_frontend/contracts/{{contract}}.  "~/Dapp_frontend/contracts/Foo.deplo
  deployed.json"                            yed.json"
  dapp-deploy -c Foo -i                     deploy contract:
  "~/Dapp_contracts/out" -O                 "~/Dapp_contracts/out/Foo.bin"
  "./contracts/{{contract}}.deployed.json"  and generate:
                                            "./contracts/Foo.deployed.json"

copyright: Warren Bank <github.com/warren-bank>
license: GPLv2
```

#### Notes:

* This tool is standalone.
* It is intended to complement the [`dapphub/dapp`](https://github.com/dapphub/dapp) toolchain,<br>
  but it can also work directly with Solidity compiler (solc) output.
* When `dapp` is installed, this tool can be invoked by the command: `dapp deploy [options]`
* When used standalone, it can be invoked by the command: `dapp-deploy [options]`

#### Commentary:

* I'm fairly new to the Ethereum ecosystem.
* I've inspected various tools that assist with the development and testing of Solidity contracts
  * [Truffle](https://github.com/trufflesuite/truffle)
  * [Dapple](https://github.com/dapphub/dapple)
  * [Dapp](https://github.com/dapphub/dapp)
* I prefer `dapp` for the ability of its testing harness to debug deeply nested throws
* After a Solidity contract (or system of contracts) is ready for a front-end UI
  * the contracts need to be deployed to an Ethereum blockchain
    * most likely, this blockchain is a short-lived instance of `testrpc`;<br>
      in which case, redeployment is necessary each time the RPC server is restarted.
  * the addresses of these deployed contracts need to be saved,<br>
    and made available to the Dapp as it is being developed and tested.
* I've found the existing tools to be somewhat lacking for this purpose.<br>
  It's very possible that I'm mistaken and have re-invented the wheel.
  * `dapp` provides [`seth`](https://github.com/dapphub/seth) as its command-line tool to perform RPC calls.<br>
    It has many more features than this tool, but I find it lacking (and difficult to use) in a lot of ways.<br>
    Hopefully, it will continue to improve over time.
  * `dapple` provides [`DappleScript`](http://dapple.readthedocs.io/en/master/dapplescript/) as its Solidity-like language in which to write "migrations".<br>
    These migration scripts can be run on the command-line.<br>
    My impression is that it's a really nice system.<br>
    My only issue is that the scripting environment is very limiting.
    * can't work with complex data types
    * can't parse (or stringify) JSON data
    * can't write to the file system
  * `truffle` provides [`migrations`](http://truffleframework.com/docs/getting_started/migrations).<br>
    I haven't spent much time looking at this.<br>
    Initial impressions:
    * Pros:
      * scripting is done in a nodeJS javascript environment
      * can use a chain of promises to inspect the "instance" of each deployed contract
      * supports linking (which is something I may need to add)
    * Cons:
      * "Truffle requires you to have a Migrations contract in order to use the Migrations feature."
      * Personally, I don't plan to use Truffle to either test Solidity contracts or write front-end Dapps.<br>
        Though it could be used solely for generating deployment metadata that is consumed elsewhere,<br>
        I don't think that makes for a pleasant (or efficient) workflow.

#### Legal:

* copyright: [Warren Bank](https://github.com/warren-bank)
* license: [GPLv2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.txt)
