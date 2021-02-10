# Monk Bison 🦬
A bison trails like stack built on top of Monk for a 1000th of the time and cost.

1. Secrets such as validator keys are stored in Hashicorp Vault
2. Monk stack is extendable to other staking nodes of your choice (see below)
3. Use Monk's cloud provider abstraction for a multi cloud setup

## Usage
This should be as easy as editing `stack.yaml` to contain the right default keys. Those will be transferred into the Vault on deployment and used by the staking nodes.
```
    dot_validator_sk: 0x3a37407ee54935b75b723a303e301c2ca23c24625b37b1089fc5b27bc50431c3
    dot_validator_sr25519_pk: 0x3acfd15c0ef5516a4eeeff48537e08a329c09a603dfa480086707ba9e4c7e628
    dot_validator_ed25519_pk: 0x258295389473ef1b9f8d2ef9d556e5fde25c11e1fc45aaad36c41e2c611aa394
    # use the same keys for DOT
    ksm_validator_sk: <- `${dot_validator_sk}`
    ksm_validator_sr25519_pk: <- `${dot_validator_sr25519_pk}`
    ksm_validator_ed25519_pk: <- `${dot_validator_ed25519_pk}`
```
Once ready run:
```
🦬 monk load polkadot/* vault.yaml stack.yaml
🦬 monk run bison/stack
```
Once deployed you should see your workloads details.
```
➜  monk run bison/stack
? Select tag to run on: staking-cluster
✔ Starting the job... DONE
✔ Preparing nodes DONE
✔ Preparing containers DONE
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Started bison/stack

✨ All done! 

🔩 bison/stack
 ├─🧊 Peer QmSKr5pQR37dcS3cfyPaX8bXZUirSn4P7i5mUrpbRVWwrm
 │  └─📦 templates-local-bison-vault_pandora-vault
 │     ├─🧩  vault:latest
 │     ├─💾 /Users/toymachine/.monk/volumes/vault/file -> /vault/file
 │     └─💾 /Users/toymachine/.monk/volumes/vault/logs -> /vault/logs
 ├─🧊 Peer QmUWgTPsJFcLPUgkjsj5Gm5WEP3VkLngvCQ9qNXs4ETYkx
 │  └─📦 templates-local-bison-dot_validator-node
 │     ├─🧩  docker.io/eteissonniere/polkadot:latest
 │     ├─💾 /Users/toymachine/.monk/volumes/monk-overlord -> /data
 │     ├─🔌 open 35.204.247.113:9944 (0.0.0.0:9944) -> 9944
 │     ├─🔌 open 35.204.247.113:30333 (0.0.0.0:30333) -> 30333
 │     └─🔌 open 35.204.247.113:9615 (0.0.0.0:9615) -> 9615
 └─🧊 Peer Qmf7ggHMC99Xe4rECcfmsgsHfyJkPtcpTVhtSbQd5qLqjo
    └─📦 templates-local-bison-ksm_validator-node
       ├─🧩  docker.io/eteissonniere/polkadot:latest
       ├─💾 /Users/toymachine/.monk/volumes/monk-overlord -> /data
       ├─🔌 open 34.91.127.200:30333 (0.0.0.0:30333) -> 30333
       ├─🔌 open 34.91.127.200:9944 (0.0.0.0:9944) -> 9944
       └─🔌 open 34.91.127.200:9615 (0.0.0.0:9615) -> 9615
```
## Interactions
You can easily inspect your cluster in various ways.
### Logs
```
➜  monk logs -l 1000 -f bison/ksm_validator
2021-02-10 17:24:50  ----------------------------    
2021-02-10 17:24:50  👤 Role: AUTHORITY    
2021-02-10 17:24:50       KUSAMA FOUNDATION          
2021-02-10 17:24:50  This chain is not in any way    
2021-02-10 17:24:50        endorsed by the           
2021-02-10 17:24:50  ----------------------------    
2021-02-10 17:24:50  Parity Polkadot    
2021-02-10 17:24:50  ✌️  version 0.8.28-c3c739ab-x86_64-linux-gnu    
2021-02-10 17:24:53  🔍 Discovered new external address for our node: /ip4/10.25.18.1/tcp/30333/p2p/12D3KooWNQmHJhZx9XVmkAKsRmcmoxMoEdcaEFKu7hmsoiaWhctL    
2021-02-10 17:24:50  ❤️  by Parity Technologies <admin@parity.io>, 2017-2021    
2021-02-10 17:24:50  📋 Chain specification: Kusama    
2021-02-10 17:24:50  🏷 Node name: monk-overlord    
2021-02-10 17:24:50  💾 Database: RocksDb at /data/chains/ksmcc3/db    
2021-02-10 17:24:50  ⛓  Native runtime: kusama-2029 (parity-kusama-0.tx4.au2)    
2021-02-10 17:24:50  🔨 Initializing Genesis block/state (state: 0xb000…ef6b, header-hash: 0xb0a8…dafe)    
2021-02-10 17:24:50  👴 Loading GRANDPA authority set from genesis on what appears to be first startup.    
2021-02-10 17:24:52  Listening for new connections on 127.0.0.1:9944.   
```
### Workloads & Cluster status
```
➜  monk ps -a                              
✔ Got containers
Template             Container                                  Uptime  Peer ID                                         Status   
bison/ksm_validator  templates-local-bison-ksm_validator-node   4m5s    Qmf7ggHMC99Xe4rECcfmsgsHfyJkPtcpTVhtSbQd5qLqjo  running  
bison/vault_pandora  templates-local-bison-vault_pandora-vault  4m53s   QmSKr5pQR37dcS3cfyPaX8bXZUirSn4P7i5mUrpbRVWwrm  running  
bison/dot_validator  templates-local-bison-dot_validator-node   4m22s   QmUWgTPsJFcLPUgkjsj5Gm5WEP3VkLngvCQ9qNXs4ETYkx  running 

➜  monk cluster peers
✔ Got the list of peers
ID                                              Name     Tag            Provider  Containers  IP              Started At  Active  
QmSKr5pQR37dcS3cfyPaX8bXZUirSn4P7i5mUrpbRVWwrm  instance-1  staking-cluster  gcp       1           35.204.57.113   9m38s       true    
QmUWgTPsJFcLPUgkjsj5Gm5WEP3VkLngvCQ9qNXs4ETYkx  instance-3  staking-cluster  gcp       1           35.204.247.113  7m48s       true    
local                                           local                   unknown   0           127.0.0.1       14m36s      true    
Qmf7ggHMC99Xe4rECcfmsgsHfyJkPtcpTVhtSbQd5qLqjo  instance-2  staking-cluster  gcp       1           34.91.127.200   8m43s       true 
```
### Actions
Retrieving existing secret keys from vault.
```
➜  monk-bison git:(main) ✗ monk do bison/vault_pandora/get path=validators/ksm/validator_sr25519_pk key=key
✔ Got action parameters
✔ Parse parameters success
✔ Running action: 
0x3acfd15c0ef5516a4eeeff48537e08a329c09a603dfa480086707ba9e4c7e628
✨ Took: 2s
```
## Expandable Stack
A sample bison stack is defined as such in `stack.yml`:
```yaml
stack:
  defines: process-group
  runnable-list:
    - /bison/vault_pandora
    - /bison/dot_validator
    - /bison/ksm_validator
```
Adding or removing new validators is as easy as inheriting their Monk template and doing something similar to:
```yaml
stack:
  defines: process-group
  runnable-list:
    - /bison/vault_pandora # <- always keep this if you use vault for secrets
    - /your_stack/eth2_validator
    - /your_stack/acala_validator
    - /your_stack/cosmos_validator
```