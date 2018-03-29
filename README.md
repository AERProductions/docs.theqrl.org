# QRL Documentation Page

> Quantum Resistant Ledger


This is the documentation for the QRL project. The main project can be found at [https://theQRL.org](https://theqrl.org)

This Github project is hosted at [https://docs.theqrl.org](https://docs.theqrl.org)

## Contribute

> We welcome contributors and believe in **open source development**

If you have good information to add to the documentation, please submit a PR.
Anything from a typo to creating a complete write-up, the project and community will be better for it.


* * * 

[![PyPI version](https://badge.fury.io/py/qrl.svg)](https://badge.fury.io/py/qrl)
[![Build Status](https://travis-ci.org/theQRL/QRL.svg?branch=master)](https://travis-ci.org/theQRL/QRL) 
[![Build Status](https://img.shields.io/travis/theQRL/integration_tests/master.svg?label=Integration_Tests)](https://travis-ci.org/theQRL/integration_tests) 
[![Codacy Badge](https://api.codacy.com/project/badge/Coverage/37ffe8d11be94eb5aeb5d29379dd3543)](https://www.codacy.com/app/jleni/QRL?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=theQRL/QRL&amp;utm_campaign=Badge_Coverage) 
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/37ffe8d11be94eb5aeb5d29379dd3543)](https://www.codacy.com/app/jleni/QRL?utm_source=github.com&utm_medium=referral&utm_content=theQRL/QRL&utm_campaign=badger) [![codebeat badge](https://codebeat.co/badges/9a0c8cad-bfa0-4ea7-89bf-bcb80859ce43)](https://codebeat.co/projects/github-com-theqrl-qrl-master)
[![Snyk Vulnerability Analysis](https://snyk.io/test/github/theQRL/QRL/badge.svg)](https://snyk.io/test/github/theQRL/QRL)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/theQRL/qrllib/master/LICENSE)

# QRL - Quantum Resistant Ledger 

> Python-based blockchain ledger utilizing hash-based one-time merkle tree signature scheme (XMSS) instead of ECDSA. Proof-of-work block selection via the cryptonight algorithm. Late 2018 planned transition to POS with signed iterative hash chain reveal scheme which is both probabilistic and random (https://github.com/theQRL/pos).
>
> Hash-based signatures means larger transactions (2.4Kb per tx, binary), longer keypair generation times and the need to record 'state' of transactions as each keypair can only be used once safely. Merkle tree usage enables a single address to be used for signing numerous transactions (up to 2^14 computationally easily enough). Transactions have an incremented nonce to allow wallets to know which MSS keypair to use - currently XMSS/W-OTS+ are natively supported.

More information:
 * [theqrl.org](https://theqrl.org)
 * [Blog (Medium)](https://medium.com/the-quantum-resistant-ledger)
 * [Original Whitepaper (English)](https://github.com/theQRL/Whitepaper/blob/master/QRL_whitepaper.pdf) or [other languages](https://github.com/theQRL/Whitepaper/blob/master) [These documents are subject to change]
 * [Original Proof of Stake document](https://github.com/theQRL/pos) [This document is subject to change]
 * [Discord Chat](https://discord.gg/RcR9WzX)
 
* * *

# QRL Testnet (Instructions for beta testers)

You are welcome to install the beta version and join the testnet. Be aware that work is in progress and there might be frequent breaking changes. It's best to start with a fresh install of Ubuntu 16.04.

> Note if you build on a small VPS or other light weight hardware you may run into issues building the package. Make sure you have enough *RAM* and enable *SWAP* if needed.

### Minimum Hardware Requirements

* Most Linux / Unix based systems
* Any x86 or x64 based processor
* Support for AES-NI
* Enough space for the blockchain growth.

## Update and Dependencies 

```bash
# Issue the following command to update software

sudo apt update && sudo apt upgrade -y
```

### Dependencies

```bash
# Install the required packages for QRL
sudo apt-get -y install swig3.0 python3-dev python3-pip build-essential cmake pkg-config libssl-dev libffi-dev libhwloc-dev libboost-dev
```
* * * 

### OSX

To build in OSX Please install `brew` if you have not already.

```bash
# Install brew with
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 
```

This will prompt you through a few questions while it installs.

Having Issues? Please follow the instructions found at the brew main page: [https://brew.sh/](https://brew.sh/)

#### Update brew

```bash
brew update
brew install cmake python3 swig boost hwloc
```

* * * 


### Install QRL BETA-NET

```bash
# Install the qrl Package.
pip3 install -U qrl
```
If you need to add logging and troubleshoot issues enter:

```
# Add Logging for pip3 
pip3 install -U qrl --log ~/pip3-Qrl.log
```

* * *

## Wallet

Create a new wallet using the `qrl` command. After you create a wallet you can create a new `slaves.json` file and use it to mine with.

### Create New Wallet

```bash
# Create a new wallet
qrl wallet_gen
```
### Get Mnemonic/hexseed

```bash
# Get mnemonic phrase and hexseed
qrl wallet_secret
```  
### Recover QRL Wallet

```bash
# Options:
#	--seed-type	[hexseed|mnemonic]
#  	--help   	Show this message and exit.

qrl wallet_recover --seed-type mnemonic
```

### Generate a slaves.json file

Using the wallet you just created generate the slaves.json file against a know working node. You can switch the ip-address with a trusted open node.

```
# QRL Nodes

104.237.3.185
104.237.3.184
104.251.219.145
104.251.219.40
```

```bash
# Generate slaves.json file
qrl -r --host 104.237.3.185 slave_tx_generate 
```
This will prompt with some questions:

```bash
Src []:                 # The wallet you generated by #ID
Addr from []:           # (Leave blank in case same as source)
Number of slaves [0]:   # Enter 4
Access type [0]:        # Mining only or transfer coins [0],[1]
                        # 0 Gives all permissions from the master wallet to slave wallet
                        # 1 Only mining permissions to slave wallet
                            # This setting allows incoming only transfers (SAFE)
Fee [0.0]: 0            # How much fee
```

This will generate a `slaves.json` in the directory you are in. 

If you have saved the **Mnemonic** or **hexseed** somewhere safe you can delete the open `wallet.qrl` file and use the `slaves.json` file to mine with. 

Move this fie into your `~/.qrl` directory. If needed, create the directory.

* * * 

## Start QRL Node

Now that we have a `slaves.json` file in our ~/.qrl folder we can start qrl.

```bash
# start qrl
start_qrl --miningCreditWallet YOUR_QRL_ADDRESS 
```

Check out all the options with a simple `start_qrl --help`

Using `screen` you can run in the background and reconnect later. You may need to install screen.

```bash
# run in background
screen -d -m start_qrl --miningCreditWallet YOUR_QRL_ADDRESS
```
You can see the progress in the `~/.qrl/qrl.log` file.

```bash
# watch the action with 
tail -f ~/.qrl/qrl.log
```

* * * 

## Config File

You can alter the default settings of the node by simply adding a file to your `~/.qrl` folder 

```bash
# Create and edit the config.yml file
nano ~/.qrl/config.yml
```

Add the following to the file. These are all default settings, uncomment to edit the parameters.

```bash
# ======================================
#    Mining Configuration
# ======================================
# mining_enabled: True
# mining_thread_count: 0  # 0 to auto detect thread count based on CPU/GPU number of processors

# ======================================
#    Ephemeral Configuration
# ======================================

# accept_ephemeral: True
# outgoing_message_expiry: 90 

# Cache Size
# lru_state_cache_size: 10
# max_state_limit: 10


# ======================================
#    P2P Configuration
# ======================================
# enable_peer_discovery: True  # Allows to discover new peers from the connected peers
# peer_rate_limit: 500  # Max Number of messages per minute per peer

# ntp_servers: pool.ntp.org
# ban_minutes: 20              # Allows to ban a peer's IP who is breaking protocol

# max_peers_limit: 100  # Number of allowed peers
# chain_state_timeout: 180
# must be less than ping_timeout
# chain_state_broadcast_period: 30


# ======================================
#    Directory Configuration
# ======================================
#  
## The users ~/.qrl/ directory  
#qrl_dir : /home/{USER}/.qrl  
#  
## The users ~/.qrl/data/ directory  
#data_dir : /home/{USER}/.qrl/data  
#  
# 
## QRL Loging location ~/.qrl/qrl.log  
#log_path : /home/{USER}/.qrl/qrl.log  
#  
## The users ~/.qrl/wallet/ directory  
#wallet_staking_dir : /home/{USER}/.qrl/wallet 

# qrl_dir: os.path.join(expanduser("~"), ".qrl")
# data_dir: os.path.join(qrl_dir, "data")
# config_path: os.path.join(qrl_dir, "config.yml")
# log_path: os.path.join(qrl_dir, "qrl.log")
# wallet_staking_dir: os.path.join(qrl_dir, "wallet")

# mining_pool_payment_wallet_path: os.path.join(qrl_dir, 'payment_slaves.json')  # Only for mining Pool

# ======================================
#    MINING WALLET CONFIGURATION
# ======================================
# slaves_filename: 'slaves.json'

# wallet_dir: os.path.join(qrl_dir)

```

* * *

Please add any issues found here in GitHub. Thanks for helping run QRL Beta-Net!

If you need help jump into the [Discord Chat](https://discord.gg/RcR9WzX)

* * *

## Windows :seedling:

*Windows support in the current version is limited. An alternative is to use an Ubuntu VM (virtualbox), or install Ubuntu using the Linux Subsystem for Windows.

### Ubuntu on Linux Subsystem for Windows

It is possible to run a QRL node using an Ubuntu Bash shell through the Linux Subsystem for Windows. 

Follow [these instructions](https://msdn.microsoft.com/en-us/commandline/wsl/install-win10) to install Ubuntu using Linux Subsystem, start the Ubuntu bash shell, and then follow the previous instructions on setting up QRL.

*We are working on a solution to native Windows support*

* * *
