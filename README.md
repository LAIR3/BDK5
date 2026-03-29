# LAIR3-BDK5 layer 3 Blockchain Deployment Kit v5
Layered Architecture for Innovative Resilience (Layer 3) Blockchain Deployment Kit v5<br />
lair: a place where a wild animal, especially a fierce or dangerous one, lives<br />
pronounced: layer three Blockchain Deployment Kit<br />


A [Kurtosis](https://github.com/kurtosis-tech/kurtosis) package that deploys a private, portable, and modular Blockchain Deployment Kit layer3 BDK as devnet<br />
LAIR3-BDK5 is derived from modest improvements to Polygon-SDK and Kurtosis-CDK dual licenced with inspiration from<br /><br />
<a href="http://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Proof%20Systems/The_Knowledge_Complexity_Of_Interactive_Proof_Systems.pdf">The Knowledge Complexity of Interactive Proof Systems</a><br /><br />


instructions are somewhat specific to Ubuntu 22.04LTS<br />
docker container is using Alpine Linux<br />
# reference links<br />
<a href="https://docs.kurtosis.com/">kurtosis-docs</a><br />
<a href="https://docs.kurtosis.com/how-to-local-eth-testnet">kurtosis ETH local testnet</a><br />
<a href="https://docs.kurtosis.com/how-to-compose-your-own-testnet">kurtosis local custom testnet</a>
<a href="https://lighthouse-book.sigmaprime.io/">lighthouse</a><br />
<a href="https://docs.polygon.technology/cdk/">polygon-cdk</a><br />
<a href="https://docs.polygon.technology/cdk/agglayer/overview/">agglayer-docs</a><br />
<a href="https://rollupjs.org/configuration-options/#output-manualchunks">manualchunks</a><br />
<a href="https://docs.timescale.com/self-hosted/latest/install/installation-docker/">pgvectorscale timescale</a><br />
<a href="https://docs.docker.com/engine/install/">docker engine install</a><br />
<a href="https://training.github.com/downloads/github-git-cheat-sheet/">github quick reference</a><br />


to begin install the following requirements<br />

#  go1.21.6 install on Ubuntu Linux 22.04LTS for amd64 using bash<br />
```bash
#get wget
sudo apt install wget
#wget go
wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz
#remove existing go
sudo rm -rf /usr/local/go
# possibly necessary
sudo apt remove golang $$ autoremove
#extract verbosely with force
sudo tar -xvf go1.21.6.linux-amd64.tar.gz -C /usr/local/
#add go path to ./bashrc current user with default Ubuntu 22 bash shell
echo 'export PATH="$PATH:/usr/local/go/bin"' | sudo tee -a ./bashrc
#refresh your bash shell
source $HOME/.bashrc
# verify correct version install
go version
```

# nodejs install<br />
```bash
nodejs -v
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install nodejs -y
nodejs -v
```

# docker install<br />
```bash
# docker install https://docs.docker.com/engine/install/ubuntu/
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
docker ps
```

# kurtosis install
https://docs.kurtosis.com/install/
```bash
echo "deb [trusted=yes] https://apt.fury.io/kurtosis-tech/ /" | sudo tee /etc/apt/sources.list.d/kurtosis.list
sudo apt update
sudo apt install kurtosis-cli
```

# kurtosis-engine upgrade
```bash
sudo apt update && sudo apt install --only-upgrade kurtosis-cli
kurtosis engine restart
```


# polygon-cli install
blockchain swiss army knife source build<br />

```bash
snap install yq
sudo apt install bc protoc
git clone https://github.com/maticnetwork/polygon-cli.git
cd polygon-cli
make install
```
# foundry install 
```bash
curl -L https://foundry.paradigm.xyz | bash
source $HOME/.bashrc
foundryup
cd BDK5
git config --global user.email "codephreak@pythai,net"
git config --global user.name "Professor-Codephreak"
forge init --force
```
To begin make sure you have installed the above requirements including [Docker](https://docs.docker.com/get-docker/) and [Kurtosis](https://docs.kurtosis.com/install/)

run tool_check to test for version requirements

```bash
sh scripts/tool_check.sh
```
wallet creation
```
polycli wallet inspect --mnemonic "$seed" --addresses 9 | \
    jq -r '.Addresses[] | [.ETHAddress, .HexPrivateKey] | @tsv' | \
    awk 'BEGIN{split("sequencer,aggregator,claimtxmanager,timelock,admin,loadtest,agglayer,dac,proofsigner",roles,",")} {print "zkevm_l2_" roles[NR] "_address: \"" $1 "\""; print "zkevm_l2_" roles[NR] "_private_key: \"0x" $2 "\"\n"}'
```

Once that is good and installed on your system, you can run the following command to locally deploy the complete BDK stack

this will take between 5 and 20 minutes depending on your hardware

```bash
kurtosis clean --all
kurtosis run --enclave bdk-v5 --args-file params.yml --image-download always .
```

![Architecture Diagram](./docs/img/starlark.png)

To begin install [Docker](https://docs.docker.com/get-docker/) and [Kurtosis](https://docs.kurtosis.com/install/).

run tool_check to test for version requirements

```bash
sh scripts/tool_check.sh
```

Once that is good and installed on your system, you can run the following command to locally deploy the complete BDK stack

this will take between 5 and 20 minutes depending on your hardware

```bash
kurtosis clean --all
kurtosis run --enclave bdk-v5 --args-file params.yml --image-download always .
```

The command above deploys a BDK stack using [zkevm-node](https://github.com/0xPolygonHermez/zkevm-node) as the sequencer. Alternatively, to launch a CDK stack using [cdk-erigon](https://github.com/0xPolygonHermez/cdk-erigon) as a sequencer, you can run the following command.

```bash
kurtosis run --enclave bdk-v5 --args-file cdk-erigon-sequencer-params.yml --image-download always .
```

# simple L2 RPC test call.

First, you will need to figure out which port Kurtois uses for the RPC. You can get a general feel for the entire network layout by running the following command:

```bash
kurtosis enclave inspect bdk-v5
```

# view port mapping
within enclave `bdk-v2` for `zkevm-node-rpc` service and `trusted-rpc`storing the RPC URL as an environment variable:<br />

```bash
export ETH_RPC_URL="$(kurtosis port print bdk-v2 zkevm-node-rpc-001 http-rpc)"
```

That is the same environment variable that `cast` uses, so you should now be able to run this command. Note that the steps below will assume you have the [Foundry toolchain](https://book.getfoundry.sh/getting-started/installation) installed.

```bash
cast block-number
```

By default, the CDK is configured in `test` mode, which means there is some pre-funded value in the admin account with address `0xE34aaF64b29273B7D567FCFc40544c014EEe9970`.

```bash
cast balance --ether 0xE34aaF64b29273B7D567FCFc40544c014EEe9970
```

# sending transaction

```bash
export PK="0x12d7de8621a77640c9241b2595ba78ce443d05e94090365ab3bb5e19df82c625"
cast send --legacy --private-key "$PK" --value 0.01ether 0x0000000000000000000000000000000000000000
```

# multisend transactions with polygon-cli [polygon-cli](https://github.com/maticnetwork/polygon-cli) installed.

```bash
polycli loadtest --rpc-url "$ETH_RPC_URL" --legacy --private-key "$PK" --verbosity 700 --requests 500 --rate-limit 5 --mode t
polycli loadtest --rpc-url "$ETH_RPC_URL" --legacy --private-key "$PK" --verbosity 700 --requests 500 --rate-limit 10 --mode t
polycli loadtest --rpc-url "$ETH_RPC_URL" --legacy --private-key "$PK" --verbosity 700 --requests 500 --rate-limit 10 --mode 2
polycli loadtest --rpc-url "$ETH_RPC_URL" --legacy --private-key "$PK" --verbosity 700 --requests 500 --rate-limit 3  --mode uniswapv3
```
```

```bash
sudo apt install postgresql-client-common
```

foundry creates script and src folders

The LAIR3-BDK5 Layer 3 Blockchain Development Kit is designed to facilitate the deployment and management of advanced blockchain solutions specifically supporting zkEVM Rollup and Validium technologies. Merging the best of the Kurtosis SDK, polygon-cli, avalanche subnet-evm and ignite with Starlark the Blockchain Deployment Kit facilitates the creation and deployment of customized blockchain environments with enhanced observability and testing capabilities.

The primary goal is to to make blockchain deployment a sane operation accessiable to regular developers interested in dapplications development.

# set PostgreSQL master password
```bash
export POSTGRES_MASTER_PASSWORD="your_secure_password"
```

# Set Validator PostgreSQL Password

Log into PostgreSQL
```bash
sudo -u postgres psql
ALTER USER validator1 WITH PASSWORD 'new_validator1_password';
\q
```

# check postgresql installation
```bash
sudo apt install postgresql-client-common
psql -U master_user -h 127.0.0.1 -p 5432 -d master
\dx
SELECT * FROM pg_extension WHERE extname = 'vectorscale';
```

# optional upggrade PostgreSQL modify pgvectorscale as docker build using toolbox.Dockerfile
```bash
docker build -t blockchaindeploymentkit/bdk-toolbox-postgres-pgvectorscale -f docker/toolbox.Dockerfile .
docker login
docker tag blockchaindeploymentkit/bdk-toolbox-postgres-pgvectorscale blockchaindeploymentkit/bdk-toolbox-postgres-pgvectorscale
docker push blockchaindeploymentkit/bdk-toolbox-postgres-pgvectorscale
```

# remove bdk-toolbox-postgres-pgvectorscale
```bash
docker rmi blockchaindeploymentkit/bdk-toolbox-postgres-pgvectorscale
docker system prune -a
docker compose version
```

# troubleshoot ###################################################################

# kurtosis logs directory error

Create Missing Directories:
If directories /var/log/kurtosis/ and similar are missing you can manually create these directories to resolve the issue

```bash
sudo mkdir -p /var/log/kurtosis/2024/31
sudo chown -R $USER:docker /var/log/kurtosis
```
Verify Docker Permissions:
```bash
sudo chmod -R 755 /var/log/kurtosis
````
Restart Kurtosis and Docker:
```bash
kurtosis engine restart
sudo systemctl restart docker
```
```bash
docker ps -a
docker images
```
requirements include but not limited to<br />

# zkevm_bridge logs ./lib/zkevm_bridge.star<br />
```bash
kurtosis service logs cdk-v1 zkevm-bridge-ui-001
```

# run the bridge UI standalone<br />
```bash
docker run -it --rm -p 8080:80 leovct/zkevm-bridge-ui:multi-network
```
generic instruction
```bash
docker run --name zkevm-bridge-ui -p 80:8080 -v /path/to/.env:/app/.env leovct/zkevm-bridge-ui:multi-network
```

# erigon standalone

```bash
docker --version
docker network create erigon-net
docker run -d --name cdk-erigon-sequencer --network erigon-net -p 8123:8123 -p 6900:6900 -p 9091:9091 -p 6060:6060 -v erigon-data:/home/erigon/data -e CDK_ERIGON_SEQUENCER=1 hermeznetwork/cdk-erigon:2.0.0-beta15 sh -c "sleep 10 && cdk-erigon --pprof=true --pprof.addr 0.0.0.0 --config /etc/cdk-erigon/config.yaml"
docker logs -f cdk-erigon-sequencer
```

# recomended diagnostics
```bash
sudo apt install netstat hardinfo
```

# run the zkevm-prover standalone
```bash
docker run -it --rm -p 50071:50071 -p 50061:50061 hermeznetwork/zkevm-prover:v6.0.2 /bin/bash
```
# ENCLAVES

# list enclaves
```bash
kurtosis enclave ls
```

# Inspect the Kurtosis enclave
```bash
kurtosis enclave inspect bdk-v5
```

# Check the logs of the failing service
```bash
kurtosis service logs bdk-v5 zkevm-bridge-ui-001
```
# Open a service shell
enter the service container to manually inspect and debug example brige-ui<br />
```bash
kurtosis service shell bdk-v5 zkevm-bridge-ui-001
```

# Environment Setup
Clone the Repository and Install Dependencies

```bash
git clone https://github.com/LAIR3/BDK5.git
cd BDK5
sh scripts/tool_check.sh
```

# Clean Up Previous Environments

```bash
kurtosis clean --all
```

# docker remove
stop all running containers
```bash
docker stop $(docker ps -aq)
```
Remove all containers:
```bash
docker rm $(docker ps -aq)
```

Remove all images:
```bash
docker rmi -f $(docker images -q)
```

Remove all volumes:
```bash
docker volume rm $(docker volume ls -q)
```
Remove all networks:
```bash
docker network rm $(docker network ls -q)
```
Prune the system:
```bash
docker system prune -a --volumes
```

```bash
docker network ls
docker network inspect NAME
```

############ START ############
# BDK-v5 as Kurtosis Enclave

```bash
kurtosis run --enclave BDK-v5 --args-file params.yml --image-download always .
```

# Inspect Enclave for Dashboard Details

```bash
kurtosis enclave ls
```

```bash
kurtosis enclave inspect BDK-v5
```

# Fetch Service Logs

```bash
kurtosis service logs bdk-v5 zkevm-agglayer-001
```

# Add Permissionless Node

```bash
yq -Y --in-place 'with_entries(if .key == "deploy_zkevm_permissionless_node" then .value = true elif .value | type == "boolean" then .value = false else . end)' params.yml
kurtosis run --enclave cdk-v1 --args-file params.yml --image-download always .
```

# Sync External Permissionless Node

```bash
cp /path/to/external/genesis.json templates/permissionless-node/genesis.json
yq -Y --in-place 'with_entries(if .key == "deploy_zkevm_permissionless_node" then .value = true elif .value | type == "boolean" then .value = false else . end)' params.yml
kurtosis run --enclave bdk-v5 --args-file params.yml --image-download always .
```

# Simple RPC Calls

```bash
export ETH_RPC_URL="$(kurtosis port print cdk-v1 zkevm-node-rpc-001 http-rpc)"
cast block-number
cast balance --ether 0xE34aaF64b29273B7D567FCFc40544c014EEe9970
```

# chain interactions
# Send Transactions

```bash
cast send --legacy --private-key 0x12d7de8621a77640c9241b2595ba78ce443d05e94090365ab3bb5e19df82c625 --value 0.01ether 0x0000000000000000000000000000000000000000
```

# Load Testing with Polygon CLI
```bash
polycli loadtest --requests 500 --legacy --rpc-url $ETH_RPC_URL --verbosity 700 --rate-limit 5 --mode t --private-key 0x12d7de8621a77640c9241b2595ba78ce443d05e94090365ab3bb5e19df82c625
```

# Observability Tools

# <a href="https://prometheus.io/">Prometheus</a>
Prometheus captures all the metrics for the running services.
Accessible via: http://127.0.0.1:49678

# Prometheus Targets

```
open http://127.0.0.1:49651/targets
```

# <a href="https://grafana.com/">Grafana</a>
Dashboards for visualizing metrics.
Accessible via: http://127.0.0.1:49701

# run grafana as a standalone
```bash
docker run -d --name=grafana-001 -p 49701:3000 grafana/grafana:latest

docker run -d --name=grafana -p 3000:3000 grafana/grafana:latest
docker start grafana
open http://localhost:49701/login
docker logs grafana
docker logs grafana-001
docker stop graphana
```

# View Grafana Dashboard

```bash
open http://127.0.0.1:49701/dashboards
open http://127.0.0.1:49701/login
```

# Panoptichain
Monitors on-chain metrics such as blocks, transactions, and smart contract calls.
Accessible via: http://127.0.0.1:49651



