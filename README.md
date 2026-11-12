# homelab insfrastructure below:

This is a small homelab I'm running on my Raspberry Pi 4B. Services are ran on Docker containers via Portainer.

## Tech Stack:
* **Hardware:** Raspberry Pi 4B 
* **Containerization:** Docker & Docker Compose
* **CI-CD:** Portainer
* **Zero-Trust Networking:** Tailscale (for remote access)

## Observability
| **Homarr** | Unified landing page and GitOps service dashboard | `3002` | Active |
| **Pi-hole** | Network-wide DNS sinkhole and ad-blocker | `53`, `80` | Active |
| **Uptime Kuma** | Uptime monitoring for internal services and network latency | `3001` | Active |
| **Prometheus** | Time-series database for hardware and container metrics | `9090` | Active |
| **Grafana** | Visual observability interface for system telemetry | `3000` | Active |
| **Node Exporter** | Background agent scraping Raspberry Pi hardware metrics | `9100` | Active |

## Security
The homelab employs several strict security layers:

1. Private information is stripped from the `docker-compose.yml` files and replaced with `${VARIABLE}` placeholders
3. Docker networking is restricted to prevent services from listening on `0.0.0.0`.

## Deployment
This homelab no longer relies on manual terminal deployments. 
1. Infrastructure changes are committed and pushed to the `main` branch of this repository.
2. Portainer periodically polls the repository.
3. Portainer clones the latest `docker-compose.yml`, injects local secrets, and automatically rebuilds the containers to match the declared state.

## Disaster Recovery
In the event of hardware failure, the entire software stack can be restored in minutes:
1. Flash a new OS and install Docker.
2. Spin up Portainer and connect it to this GitHub repository.
3. Redeploy stacks using Portainer's repository build method, reattaching to the persistent local volumes (e.g., `pihole_data`).
