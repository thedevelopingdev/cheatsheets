# Vault

- Capabilities list: https://developer.hashicorp.com/vault/docs/concepts/policies

## Initialization

```bash
# inform vault cli of the vault server's url or IP
export VAULT_ADDR=
export VAULT_SKIP_VERIFY=true

# initialize vault's unseal keys
vault operator init -key-shares=5 -key-threshold=3
vault status

# begin vault unseal process
vault operator unseal
```

## Key-value store

```bash
# check which secrets engines are available
vault secrets list

# enable a v2 key-value store at mount point kv_secrets/
vault secrets enable -version=2 -path=kv_secrets kv

# set key=value pairs in secret located at kv_secrets/data/sample_creds
vault kv -mount=kv_secrets put sample-creds key1=value1 key2=value2

# read key=value pairs stored in kv_secrets/data/sample_creds
vault kv -mount=kv_secrets get sample-creds
```
