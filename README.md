# Monk Bison
A bison trails like stack built on top of Monk for a 1000th of the time and cost.

1. Secrets such as validator keys are stored in Hashicorp Vault
2. Monk stack is extendable (see below)
3. Use Monk's cloud provider abstraction for a multi cloud setup

## Extandable Stack
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