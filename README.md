[![Go Report Card](https://nkn.org/badge/nkn.svg)](https://goreportcard.com/report/github.com/nknorg/nkn)
[![Build Status](https://travis-ci.org/nknorg/nkn.svg?branch=master)](https://travis-ci.org/nknorg/nkn)

[![NKN](https://github.com/nknorg/nkn/wiki/img/nkn_logo.png)](https://nkn.org)

# NKN: a Scalable Self-Evolving and Self-Incentivized Decentralized Network

> NKN, short for New Kind of Network, is a project aiming to rebuild the
> Internet that will be truly open, decentralized, dynamic, safe, shared and
> owned by the community.

d官方网站：d[ 
https://nkn.org/ ]（https://nkn.org/） 
dfadsf
＃＃ 介绍

The core of the NKN network consists of many connected nodes distributed
globally. Every node is only connected to and aware of a few other nodes called
neighbors. Packets can be transmitted from any node to any other node in an
efficient and verifiable route. Data can be sent to any clients without public
or static IP address using their permanent NKN address with end-to-end
encryption.

The relay workload can be verified using our Proof of Relay (PoR) algorithm. A
small and fixed portion of the packets will be randomly selected as proof. The
random selection can be verified and cannot be predicted or controlled. Proof
will be sent to other nodes for payment and rewards.

A node in our network is both relayer and consensus participant. Consensus among
massive nodes can be reached efficiently by only communicating with neighbors
using our consensus algorithm based on Cellular Automata. Consensus is reached
for every block to prevent fork.

More details can be found in [our wiki](https://github.com/nknorg/nkn/wiki).

## Technical Highlights

* Transmit any data to any node/client without any centralized server. [Related tech design doc](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Distributed-Data-Transmission-Network-%28DDTN%29)
* Proof-of-Relay, a useful proof of work: mining is relaying data. [Related tech design doc](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Proof-of-Relay-%28PoR%29)
* Extremely scalable consensus algorithm (millions or even billions of nodes). [Related tech design doc](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Consensus-and-Blockchain)
* Strong consistency rather than eventual consistency. [Related tech design doc](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Consensus-and-Blockchain)
* Dynamic, large-scale network.
* Verifiable topology and routes. [Related tech design doc](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-Relay-Path-Validation)
* Next generation address scheme with public key embedded. [Related tech design doc](https://github.com/nknorg/nkn/wiki/Tech-Design-Doc%3A-NKN-Address-Scheme)

## Building

To build from source, you need a properly configured Go environment (Go 1.8+,
$GOROOT and $GOPATH set up, see [Go Official Installation
Documentation](https://golang.org/doc/install) for more details).

Create directory $GOPATH/src/github.com/nknorg/ if not exists

In directory $GOPATH/src/github.com/nknorg/ clone the repository

```shell
$ git clone https://github.com/nknorg/nkn.git
```

Install package management tool `glide` if it's not installed on your system.

```shell
$ cd nkn
$ make glide
```

Download dependencies for building
```shell
$ make vendor
```

Build the source code with make

```shell
$ make all
```

After building is successful, you should see two executables:

* `nknd`: the nkn node program
* `nknc`: command line tool for nkn node control

## Deployment

**Note: this repository is in the early development stage and may not
have all functions working properly. It should be used only for testing
now.**

Create several directories to save exectuable files `nknd` `nknc` and
config file `config.json`. Create new wallet in each directory

``` shell
$ ./nknc wallet -c
Password:
Re-enter Password:
Address                            Public Key
-------                            ----------
NjCWGM1EfJeopJopSQGC6aLEkuug5GiwLM 03d45f701e7e330e1fd1c7cce09ffb95f7b1870e5c429ad8e8c950ddb879093f52
```

Config the same bootstrap node address and public key to each
`config.json` file, for example:

```shell
{
  "Magic": 99281,
  "Version": 1,
  "ChordPort": 30000,
  "NodePort": 30001,
  "HttpWsPort": 30002,
  "HttpJsonPort": 30003,
  "Hostname": "127.0.0.1",
  "LogLevel": 1,
  "IsTLS": false,
  "ConsensusType": "ising",
  "SeedList": [
    "127.0.0.1:30000"
  ],
  "GenesisBlockProposer": [
    "03d45f701e7e330e1fd1c7cce09ffb95f7b1870e5c429ad8e8c950ddb879093f52"
  ]
}
```

Note that ports in different `config.json` does not need to be different,
conflict in ports will be resolved automatically.

Start bootstrap node by creating a network

```shell
$ ./nknd -test create
```

Start other nodes by joining the network

```shell
$ ./nknd -test join
```

When the network contains enough nodes (usually 8+), stop the node that created
the network in order for the relay service to work properly. Nodes joining the
network later should use a live node as seed.

## Docker

Build and tag Docker image

```shell
$ docker build -t nkn .
```

When starting the container, a local directory containing `config.json` and
`wallet.dat` (if exists) should be mapped to `/nkn` directory in the container.
This local directory will also be used for wallet, block data and logs storage.
Assuming the local directory is the current directory, creating a wallet can be
done by

```shell
$ docker run -v $PWD:/nkn -it nkn nknc wallet -c
```

Start bootstrap node by creating a network

```shell
$ docker run -p 30000-30003:30000-30003 -v $PWD:/nkn -it nkn nknd -test create
```

Start other nodes by joining the network

```shell
$ docker run -p 30000-30003:30000-30003 -v $PWD:/nkn -it nkn nknd -test join
```

## Contributing

**Can I submit a bug, suggestion or feature request?**

Yes. Please open an issue for that.

**Can I contribute patches to NKN project?**

Yes, we appreciate your help! To make contributions, please fork the repo, push
your changes to the forked repo with signed-off commits, and open a pull request
here.

Please follow our [Golang Style Guide](https://github.com/nknorg/nkn/wiki/NKN-Golang-Style-Guide)
for coding style.

Please sign off your commit. This means adding a line "Signed-off-by: Name
<email>" at the end of each commit, indicating that you wrote the code and have
the right to pass it on as an open source patch. This can be done automatically
by adding -s when committing:

```shell
git commit -s
```

## Community

* [Telegram](https://t.me/nknorg)
* [Reddit](https://www.reddit.com/r/nknblockchain/)
* [Twitter](https://twitter.com/NKN_ORG)
* [Facebook](https://www.facebook.com/nkn.org)
