
# Helm Charts for Morpheus Lumerin

This repository hosts Helm charts for deploying Morpheus Lumerin-related services. You can add this repository, update it, and install charts with the commands below.

## Adding the Repository

To add the Morpheus Lumerin Helm chart repository:

```bash
helm repo add morpheus-lumerin https://aether-rise.github.io/charts
helm repo update
```

## Installing a Chart

Once the repository is added, you can install a specific chart. Replace `<chart-name>` with the name of the chart you wish to install:

```bash
helm install mlc morpheus-lumerin/morpheus-lumerin-node --namespace mlc --create-namespace
```

## Available Configuration Values

Below are the main configurable values for this chart. You can set them inline with `--set` or in a `values.yaml` file.

### Proxy Router Configuration
- `proxyRouter.image.repository`: Docker image repository for the proxy router (default: `bsord/mln`).
- `proxyRouter.image.tag`: Image tag (default: `latest`).
- `proxyRouter.image.pullPolicy`: Image pull policy (default: `IfNotPresent`).

### Environment Variables for Proxy Router
- `proxyRouter.env.WALLET_PRIVATE_KEY`: Private key from your wallet (for signing transactions).
- `proxyRouter.env.ETH_NODE_ADDRESS`: ETH node WebSocket address (recommended to use a private node for better performance).
- `proxyRouter.env.ETH_NODE_CHAIN_ID`: Chain ID for the Ethereum testnet (default: `421614`).
- `proxyRouter.env.DIAMOND_CONTRACT_ADDRESS`: Diamond contract address (as of 10/30/2024).
- `proxyRouter.env.MOR_TOKEN_ADDRESS`: MOR token address (as of 10/30/2024).
- `proxyRouter.env.WEB_ADDRESS`: Address and port for the Proxy-router API (default: `0.0.0.0:8082`).
- `proxyRouter.env.WEB_PUBLIC_URL`: Public URL for accessing the API (should match `WEB_ADDRESS` port).
- `proxyRouter.env.OPENAI_BASE_URL`: URL for local OpenAI API (default: `http://ollama:11434/v1`).
- Additional environment variables are available to configure proxy, model, and system settings.

### Ports
- `proxyRouter.ports`: List of ports exposed by the proxy router (default: `[8080, 8082, 3333]`).

### Volumes
- `proxyRouter.volumes.logs`: Path for log storage (default: `/app/logs`).
- `proxyRouter.volumes.goPkgMod`: Go package module storage (default: `/go/pkg/mod`).

### Service Configuration
- `proxyRouter.service.type`: Type of Kubernetes service (default: `LoadBalancer`).

### Config Map
- `proxyRouter.configMap.modelsconfig`: JSON configuration for model settings.

### Ollama Configuration
- `ollama.image.repository`: Docker image repository for Ollama (default: `ollama/ollama`).
- `ollama.image.tag`: Image tag (default: `latest`).
- `ollama.image.pullPolicy`: Image pull policy (default: `IfNotPresent`).
- `ollama.env.OLLAMA_NUM_PARALLEL`: Number of parallel processes (default: `4`).
- `ollama.ports`: Port for Ollama (default: `11434`).
- `ollama.resources.limits.gpu`: GPU limit (default: `1`).
- `ollama.volumes.config`: Path for configuration (default: `/root/.ollama`).
- `ollama.volumes.startupScript`: Path to the startup script (default: `/docker-compose-startup.sh`).
- `ollama.readinessProbe.path`: Readiness probe path (default: `/`).
- `ollama.readinessProbe.port`: Readiness probe port (default: `11434`).

## Updating the Repository

To update your local Helm repository cache with the latest charts:

```bash
helm repo update
```

This ensures you have access to the most recent versions of the charts in this repository.

## Example of Using a `values.yaml` File

You can specify these configurations in a `values.yaml` file:

```yaml
replicaCount: 1

proxyRouter:
  image:
    repository: bsord/mln
    tag: "latest"
    pullPolicy: IfNotPresent
  env:
    WALLET_PRIVATE_KEY: "YOUR_PRIVATE_KEY"
    ETH_NODE_ADDRESS: "wss://arb-sepolia.g.alchemy.com/v2/YOUR_ALCHEMY_ID"
    ETH_NODE_CHAIN_ID: 421614
    DIAMOND_CONTRACT_ADDRESS: "0x10777866547c53cbd69b02c5c76369d7e24e7b10"
    MOR_TOKEN_ADDRESS: "0x34a285a1b1c166420df5b6630132542923b5b27e"
  ports:
    - 8080
    - 8082
    - 3333

ollama:
  image:
    repository: ollama/ollama
    tag: "latest"
    pullPolicy: IfNotPresent
  env:
    OLLAMA_NUM_PARALLEL: "4"
  ports:
    - 11434
  resources:
    limits:
      gpu: 1
```

Then use it with:

```bash
helm install mlc morpheus-lumerin/morpheus-lumerin-compute -f values.yaml --namespace mlc --create-namespace
```

For more configuration options, refer to the `values.yaml` file provided in each chart directory.
