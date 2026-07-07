# Sunk

Slurm on Kubernetes via [Slinky](https://github.com/SlinkyProject).

## Prerequisites

- [k3d](https://k3d.io)
- [Tilt](https://tilt.dev)
- [Helm](https://helm.sh)
- [Task](https://taskfile.dev)

## Getting Started

```bash
task cluster-start
task dev
```

## Connection

| Service        | URL                                                |
|----------------|----------------------------------------------------|
| HTTP (Traefik) | http://localhost:8080                              |
| HTTPS          | https://localhost:8443                             |
| SSH (login)    | `ssh -p 2222 research@localhost` (password: `pass` |

## Teardown

```bash
task dev-down
task cluster-stop
```
