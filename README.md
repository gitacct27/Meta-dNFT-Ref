# Meta-dNFT-Ref

# Initial Setup for execution 
We employed Bionic 18.04.4, Pandas, Python 3.6, Jupyter Notebook, and Hyperledger AnonCreds 1.0 to create and confirm Dynamic Identities (DID), Wallet addresses, and Verifiable Credentials (VC) for marketplace participants. AnonCreds 1.0 operates with the Hyperledger Indy platform to generate an Indy System Pool, assigning verifier roles like Trust Anchor (role '101') or Trustee (role '0') using Nym Transactions in our main21.py program.

 ### 1. Start Docker with the following command

docker-compose up-d

### to stop the docker after experiment 
docker-compose down

### 2. Installation 
AnonCreds 1.0 collaborates with the Hyperledger Indy platform, creating an Indy System Pool that assigns predefined verifiers, such as Trust Anchors (role '101') and Trustees (role '0'), through Nym Transactions in our main21.py program.

Please go inside path # cd fabric

./ install.sh

####  during Installation of Fabric, also please confirm that the following commands will run successfully
/network down

./network up

Add mychannel:

./network.sh createChannel -c mychannel

Add simple chaincode to channel:

./network.sh deployCC -ccn simple -ccp /home/suman/Downloads/caliper-benchmarks/src/fabric/samples/marbles/go -ccl go

Invoke chain code:

peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile
${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n simple --peerAddresses localhost:7051 --tlsRootCertFiles 

${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles 

${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"InitLedger","Args":[]}'

 
in test-networkfolder (onetime thing): 

export FABRIC_CFG_PATH=$PWD/../config/

export CORE_PEER_TLS_ENABLED=true

export CORE_PEER_LOCALMSPID="Org1MSP"

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

export CORE_PEER_ADDRESS=localhost:7051

### 3. Installation of Ethereum platform 
cd ethereum

sudo ./ install.sh

### 4. Installation of Hyperledger Indy platform for starting the system pool for AnonCreds and run the AnonCreds funcionality under Hyperledger platform

a.First clone Indy node repository for starting the repository using the commands- 

git clone https://github.com/hyperledger/indy-node.git 

Then go inside the directory using the command - cd indy-sdk

b. starting with a pre-configured docker image to build and run it for the pool:
   
docker build -f ci/indy-pool.dockerfile -t indy_pool.

docker run -itd -p 9701-9708: 9701-9708 ghoshbishakh/indy_pool

This creates an Indy container housing a pool of system-validated authenticators, each with a unique identity, pool number, and assigned port between 9701-9708.

c.Then run-  docker ps , to get the container identity forexample in our case it is 351k39691g56. 
Then go inside the indy pool docker container  using the command - docker exec -it 351k39691g56 bash

d. Now go inside the container 351k39691g56 and run the command - cat /var/lib/indy/sandbox/pool_transactions_genesis                                              
to get the details information of each validator nodes.

e. Now open a terminal and copy the information of all the information of validators nodes in to an text editotor that is opened using the command -'code.'.
and past all the information into the text editor and save it as 'pool1.txn' that is basically a type of JSON file for communication with the AnonCreds main code.

f. Import "main21.py" into the folder containing 'pool1.txn' to integrate Hyperledger AnonCreds for generating dynamic IDs, wallet addresses, and verifiable credentials for anonymous marketplace participant validation.

g. Now Import "main30.py" and "Registrationcontract.sol" into the editor's folder containing 'pool1.txn', then execute "main30.py" to validate dynamic identities and wallet addresses before registering marketplace participants.

All such coding related the aforesaid procedure is already uploaded under the path -  ### smajumder/did_wallet_management/indy-sdk/

### 5. Installation of python 3.6, Jupiter Note book and Pandas for running the graph generated from the dNFT Tokens 

a. Initially, install Python 3.6, Jupyter Notebook, and Pandas to efficiently generate reputation graphs using dNFT token data from wallet addresses.

b. The relevant information is uploaded in separate CSV files, detailing 'bidding and pricing,' linked to dynamically generated DID and Wallet Addresses, 'listing and current dNFT prices,' as well as 'dNFT owner, creator, and minting information,' to facilitate efficient data exchange (located at smajumder/Meritrank/NFT/data/).

c. Afterwards, a .JSON file is created from the .CSV files, shaping a multi-directed weighted graph (MDG) with nodes V linked to unique dynamic wallet addresses and edges E depicting token transfers from node Vi to Vj, using scripts like 'hitting_time.py', 'random_walks.py', located at smajumder/Meritrank/NFT/Trust/.

d. Create 'read_data.py' and the 'NFT_mail_file.ipynb' as found in the directory smajumder/Meritrank/NFT/, then execute 'NFT_mail_file.ipynb' to produce outputs depicted from Figures 2 to 10 in the manuscript.

### 6. Installation of Hyperledger Caliper platform, please follow the steps as mentioned in the detailed link

a.Please follow the installation procedure as mentioned in the link below:
https://hyperledger.github.io/caliper/v0.5.0/installing-caliper/

### 7. For running the Caliper sut configuration of thr benchmark and generate the report in tabular format using Prometheus, run the following command in Hyperledger caliper platform 

a. Please the upload the files as mentioned in the paths ### - smajumder/Caliper/caliper-benchmarks/ 
and smajumder/fabric/ .

b. Run the following command for generation of reports in 'Prometheus' as shown in Figures 11 and Figure 12 respectively in the manuscript.

npx caliper launch manager --caliper-bind-sut fabric:2.2 --caliper-workspace . --caliper-benchconfig benchmarks/scenario/simple/config.yaml --caliper-networkconfig networks/fabric/test-network.yaml

### 8. Run the commands for Comparative-Positioning-with-Extensions for Quantitative Evidence Analysis of proposed scheme using p50/p75/p90/p95/p99, for tht uploafd the file "nftbench25_synthetic_summary.csv" and run the file "nftbench25.py" using the following commands sequentially.
Public
 

Synthetic:

python nftbench25.py --mode synthetic --chain "Polygon (Amoy)" --concurrency 64 --duration 120
python nftbench25.py --mode synthetic --chain "Solana (devnet)" --concurrency 64 --duration 120


Real EVM (after fill the adapter):

sudo python nftbench25.py --mode evm --chain "Polygon (Amoy)" \
  --rpc http://127.0.0.1:8545 --private-key $DEV_KEY \
  --iface eth0 --rtt-ms 50 --jitter-ms 5 --concurrency 64 --duration 600


sudo python nftbench25.py --mode solana --chain "Solana (devnet)" \
  --rpc https://api.devnet.solana.com --iface eth0 --rtt-ms 50 --jitter-ms 5 \
  --concurrency 64 --duration 600

A runnable reference implementation of the event-driven reputation workflow in
*Meta-dNFT-Rep: Metaverse-assisted Dynamic NFT Reputation Scheme with
Low-Memory Off-Chain AnonCreds Service*. The repository provides a local
workflow, a synthetic benchmark, an audit command, and optional smart-contract
execution in an in-memory Ethereum virtual machine.

## Requirements

- Python 3.12 recommended. The verified environment used Python 3.12 on Linux x86-64.
- The [AnonCreds Python package](https://pypi.org/project/anoncreds/) uses a
  platform-specific native wheel. Version 0.2.3 has wheels for Linux x86-64
  and AArch64, macOS universal2, and Windows x86-64.
- The optional local EVM requires `requirements-evm.txt`. Node.js and npm are
  needed only if you want to recompile the bundled Solidity artifact.

Extract the archive and run the commands from the directory containing
`README.md`.

## Quick start

Create and activate a virtual environment:

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux or macOS**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the pinned dependency and run the demo:

```text
python -m pip install -r requirements.txt
python -m meta_dnft_rep demo --feedback examples/feedback.json --out runs/demo
python -m meta_dnft_rep audit --db runs/demo/private/offchain.sqlite3 --commitments runs/demo/public/commitments.jsonl
python -m unittest discover -s tests -v
```

The demo processes **5 events**, creates **5 commitments**, and leaves
**2 token states**. The audit verifies the commitment chain against the
private database and exported public records. Choose an empty output
directory for a new run. The default test run skips the two EVM tests
when the optional EVM dependencies are absent.

If a configured package mirror does not carry the native wheel, install
with `python -m pip install --index-url https://pypi.org/simple -r requirements.txt`.

## Run the benchmark

```text
python -m meta_dnft_rep benchmark --events 200 --seed 42 --out runs/benchmark
```

The seed determines the prices of synthetic marketplace events.
Native credential proofs, salts, and timing observations vary across runs.
Each latency sample covers challenge creation, proof generation and
verification, and the SQLite transaction. Issuer setup, credential
issuance, export, and EVM execution occur outside the timed loop.

`metrics.json` reports p50, p95, and p99 using linear interpolation over
the observed samples, and throughput over the sequential loop. The
SQLite file size and serialized commitment size are storage measures.
Peak process RSS on Linux and macOS is reported in bytes for the entire
run, including setup. On other systems that field is `null`.

These figures describe this reference implementation and workload.
The manuscript’s Caliper, Indy, Fabric, and distributed testbed results
require their original configurations, workloads, and measurement logs.
The local benchmark makes no prediction of production gas cost, network
finality, or end-to-end transaction latency on a deployed network.

## Anchor the commitments on a local EVM

```text
python -m pip install -r requirements-evm.txt
python -m meta_dnft_rep anchor-local --commitments runs/demo/public/commitments.jsonl --out runs/demo/evm_receipts.json
python -m unittest discover -s tests -v
```

The command deploys the bundled Solidity contract on an ephemeral
Py-EVM chain, submits the five commitments from the demo, and checks
the contract’s final sequence count and head. The receipt file records
the actual local gas used, transaction hashes, and block numbers.
No external RPC, wallet key, faucet, or live token is required.

The contract restricts writes to its deployer and checks the predecessor
and commitment uniqueness. Its input consists of salted commitments;
the AnonCreds proof and token details are processed off-chain. The token
metadata in this implementation is application state in SQLite. The
Solidity contract exposes an audit anchor for this state.

To rebuild the included ABI and bytecode:

```text
cd contracts
npm ci
npm run compile
cd ..
```

The compiler is pinned to Solidity 0.8.30. The Python runner checks the
Solidity source hash against the compiled artifact.

## Supply a local trace

```text
python -m meta_dnft_rep demo --trace examples/events.jsonl --graph examples/graph.json --feedback examples/feedback.json --out runs/custom
```

Each line of the JSONL trace is one event object:

| Field | Meaning |
| --- | --- |
| `event_id` | Unique event identifier. An exact retry with a fresh proof returns the original commitment. |
| `kind` | `Listed` or `PriceChanged`. |
| `token_id`, `wallet` | Token identifier and locally enrolled wallet alias. |
| `price_wei` | Positive integer price in wei. |
| `expected_version` | Zero for a new listing, then the current state version for each update. |
| `timestamp` | Nonnegative Unix time in seconds. Updates advance time for the token. |
| `name` | Required for `Listed`; omit for `PriceChanged`. |

The graph JSON supplies directed weighted edges `[source, target, weight]`,
trusted wallet aliases, the finite walk horizon, and the restart
probability. The optional feedback file maps event IDs to curator-supplied
values in `[0, 5]`. The CLI enrolls the aliases in the input trace as
synthetic sellers for the local run. A deployment needs its own verified
enrollment and authenticated graph and feedback sources.

## Implementation and evidence

1. The local issuer creates genuine revocable AnonCreds credentials.
   For each event, the holder presents the scoped wallet alias and seller role.
   The presentation request uses a single-use nonce bound to the event digest
   and the current revocation status list. The credential’s private profile
   attribute remains undisclosed. The disclosed alias can link presentations
   at this verifier.
2. The service authorizes a new listing or an owner’s price update and
   checks the expected version. It uses a bounded first-hitting-time signal
   for listings and finite-horizon random walk with restart for price changes.
3. The reference score starts at 0.50. Hitting time is converted to proximity
   as `max(0, 1 - H / steps)`, so nearer trusted wallets contribute more.
   For a new token the smoothed signal equals the event signal, then it uses
   EMA weights 0.75 and 0.25 for previous and current signals. Event weights are
   0.30 and 0.40 for listing and price change, respectively. The update adds
   `0.10 * feedback / 5` and subtracts
   `0.05 * (1 - exp(-elapsed_seconds / 86400))` before clipping to `[0, 1]`.
   These choices operationalize the manuscript workflow for this dataset and
   do not recover the manuscript’s per-token scores.
4. The SQLite transaction stores the event, token snapshot, salt, and
   commitment together. The SHA-256 commitment covers the domain string,
   sequence, predecessor, event, state, and 32-byte random salt in canonical
   JSON. The export contains sequence, predecessor, and commitment.
   A trusted external copy or an anchored head is needed to detect a full
   rewrite by someone with control of the local database.
5. The issuer keys, holder link secrets, credentials, and proofs are created
   for the run and kept out of the output. The private database and state
   exports contain the event data and salts. Share the public commitment
   export when sharing examples of audit records.

| Location | Contents |
| --- | --- |
| `meta_dnft_rep/` | CLI, credential authority, graph scoring, SQLite marketplace and EVM adapter |
| `examples/` | Synthetic trace, graph, and feedback fixtures |
| `contracts/` | Solidity source, pinned build toolchain, ABI, and creation bytecode |
| `tests/` | Local tests of event policy, proof authorization, revocation, audit and EVM anchoring |
| `runs/<run>/private/` | Off-chain SQLite database, token state and per-event timings |
| `runs/<run>/public/commitments.jsonl` | Public audit commitment export |
| `runs/<run>/metrics.json` | Observed run summary |

## Put the project on GitHub

Create an empty repository in your GitHub account and run the following
from the extracted project directory. Replace the uppercase placeholders
with your account or organization and repository name.

```text
git init -b main
git add .
git commit -m "Add Meta-dNFT-Rep reference implementation"
git remote add origin https://github.com/OWNER/REPOSITORY.git
git push -u origin main
```

The included `.gitignore` excludes local runs, SQLite files, virtual
environments, Python caches and Node modules.



## The output Screen shots of the Tables VII, TABLE VIII and Table IX of the Manuscript are attached Below those are generated using  Hyperledger Caliper Platform## 


### Output Screenshot of TABLE VII: Dynamic reputation calculation using the ’Listed Event’ from the Listed Event DataFrame###
![Listed](https://github.com/user-attachments/assets/91e9815d-3373-4f07-bb91-bd3bd92d5479)

### Output Screenshot of TABLE VIII: Dynamic reputation calculation using the ‘PriceChange’ event from the Price Changed Events DataFrame###
![PriceChange_](https://github.com/user-attachments/assets/daeac683-5e46-43a8-8af1-13cfe63ede09)


### Output Screenshot of TABLE IX: Summarized Report obtained from Hyperledger Caliper on ’Average Memory Consumption’, ’Average Latency’, and ’Average TPS’ and summarized report ###

![Memory](https://github.com/user-attachments/assets/0c920125-29a3-4477-8193-86bdb8dfe40e)

![summarised report ](https://github.com/user-attachments/assets/9e295e27-e5dc-4cf8-9de7-573f5e9875d5)
Solana devnet (after adapter):
