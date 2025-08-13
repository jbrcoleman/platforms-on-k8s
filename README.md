# Platform Engineering on Kubernetes

This repository contains examples and resources for building platform APIs on Kubernetes, following concepts from the "Platform Engineering on Kubernetes" book. The focus is on creating self-service development environments using Crossplane and vcluster.

## Overview

This platform demonstrates how to:
- Build platform APIs using Kubernetes Custom Resources
- Provision on-demand development environments
- Simplify complex infrastructure through abstractions
- Enable self-service capabilities for development teams

## Prerequisites

- Kubernetes cluster (local or remote)
- kubectl configured
- Helm 3.x installed

## Quick Start

### 1. Create Kubernetes Cluster

```bash
./create-cluster.sh
```

### 2. Install Crossplane

```bash
./install-crossplane.sh
```

### 3. Apply Platform Resources

```bash
# Install environment definitions and compositions
kubectl apply -f resources/definitions/
kubectl apply -f resources/compositions/

# Configure Crossplane providers
kubectl apply -f crossplane/
```

### 4. Request a Development Environment

```bash
# Simple environment
kubectl apply -f team-a-dev-env-simple.json

# Full environment with additional features
kubectl apply -f team-a-dev-env.yaml
```

### 5. Install Conference Application (Optional)

```bash
./conference-app-install.sh
```

## Repository Structure

```
.
├── README.md                       # This file
├── create-cluster.sh              # Cluster creation script
├── install-crossplane.sh          # Crossplane installation
├── conference-app-install.sh      # Sample application installer
├── crossplane/                    # Crossplane provider configurations
│   ├── helm-provider.yaml
│   └── helm-provider-config.yaml
├── resources/
│   ├── definitions/               # Custom Resource Definitions
│   │   └── env-resource-definition.yaml
│   └── compositions/              # Crossplane Compositions
│       └── composition-devenv.yaml
├── team-a-dev-env.yaml           # Environment request example
└── team-a-dev-env-simple.json    # Simple environment example
```

## Key Components

### Environment API
- **Custom Resource Definition**: Defines the API for requesting environments
- **Composition**: Implements the logic for provisioning resources
- **Provider Configs**: Configure external providers (Helm, etc.)

### Development Environments
- Built using vcluster for isolation
- Includes necessary development tools
- Self-contained and easily disposable

## Usage Examples

### Check Environment Status
```bash
kubectl get environments
kubectl describe environment team-a-dev-env
```

### Access Environment
```bash
# Get vcluster connection info
kubectl get secret vc-team-a-dev-env --template={{.data.config}} | base64 -d > team-a-kubeconfig
export KUBECONFIG=team-a-kubeconfig
kubectl get pods
```

### Clean Up
```bash
kubectl delete -f team-a-dev-env.yaml
kubectl delete -f resources/
```

## Learning Resources

This repository accompanies the "Platform Engineering on Kubernetes" book and demonstrates:
- Chapter 6: Building platform APIs
- Self-service infrastructure patterns
- Kubernetes operator patterns
- Multi-tenancy with vcluster
