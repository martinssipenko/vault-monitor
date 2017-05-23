# Vault monitor

This small program can be used to monitor the seal status of [Hashicorp Vault](https://www.vaultproject.io). It will
periodically checks [`/sys/seal-status`](https://www.vaultproject.io/api/system/seal-status.html) endpoint and push
results to [AWS CloudWatch](https://aws.amazon.com/cloudwatch/).

We use this tool to monitor the number of unsealed Vault instances in our Vault cluster. Because Vaults nature is
that it gets sealed after process restart, and that the unseal process should be done manually, we wanted to get
notified when we need to perform Vault unsealing. Pushing number of unsealed instances to CloudWatch, together with
CoudWatch alarm allowed us to get notifications.

## Usage

Environment Variables

- `CHECK_INTERVAL` - Time interval of how often to run the check (in seconds). (default: 60)
- `VAULT_ADDR` - The address of the Vault server. (default: `https://127.0.0.1:8200`)
- `VAULT_NAME` - The name of the Vault (cluster). This value will be used as CloudWatch dimension value. (default: `Vault`)
- `METRIC_NAMESPACE` - AWS CloudWatch metric namespace. (default: Vault)
- `AWS_REGION` - AWS CloudWatch region. (default: us-east-1)

Alternatively CLI flags can be used and will override the value specified in environment variables.

- interval
- address
- name
- namespace
- region

### Docker

This can also be used with docker

```sh
docker run --rm --network=host kasko/vault-monitor vault-monitor -address=http://127.0.0.1:8200 -region=eu-west-1
```

## Build

``sh
# Install dependencies
make tools

# Compile binary for linux
make

# Create Docker image
make container

# Simply run without compiling (you can also applie arguments)
go run vault-monitor.go [-interval=60]
```