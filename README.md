# Trino Community Helm Chart

This Helm chart installs Trino, an open-source distributed SQL query engine.

## Prerequisites

- Kubernetes 1.10+ with Beta APIs enabled
- Helm 3.x installed

## Installation

To install Trino using this Helm chart, follow these steps:

1. Install the Helm repository in your cluster:
   ```bash
    helm upgrade -i trino-community https://raw.githubusercontent.com/dview-io/trino-community/main/charts/trino-community-0.1.0.tgz -f values.yaml
   ```