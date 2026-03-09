# VaultX Quick Start

Minimal steps to run a Vault inventory with VaultX.

**Prerequisites:** `vault` and `jq` in PATH, and a Vault token (or auth method) with enough permissions to create policies and tokens.

### 1. Set Vault address

```bash
export VAULTX_ADDR=<url>
```

Replace `<url>` with your Vault server (e.g. `https://vault.example.com`).

### 2. Bootstrap (1st run)

```bash
vaultx bootstrap
```

VaultX emits a bootstrap policy and exits.

### 3. Apply bootstrap policy

```bash
vault policy write vaultx <vault_host>-vaultx-bootstrap.hcl
```

Use the generated file from step 2. The prefix comes from the Vault host (e.g. `https://vault.example.com` → `vault_example_com-vaultx-bootstrap.hcl`).

### 4. Create a token and export it

```bash
export VAULTX_TOKEN=$(vault token create -policy=vaultx -ttl=1h -format=json | jq -r '.auth.client_token')
```

### 5. Bootstrap (2nd run)

```bash
vaultx bootstrap
```
or (here and below)

```bash
vaultx bootstrap --namespace <name_of_your_namespace>
```

Run again with a token that has the bootstrap policy. VaultX will generate a targeted policy for the next step.

### 6. Apply the policy generated in step 5

```bash
vault policy write vaultx <vault_host>-vaultx-policy.hcl
```

### 7. Run inventory

```bash
vaultx inventory
```

