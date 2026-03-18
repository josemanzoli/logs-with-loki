# Observability Lab: Loki, Grafana, Prometheus & RabbitMQ

Laboratório de observabilidade e mensageria para uso em aulas de **Arquitetura de Microsserviços**. Este projeto demonstra na prática conceitos como logs centralizados, métricas, comunicação assíncrona, backpressure, escalabilidade horizontal e muito mais.

## Stack

| Serviço | Função |
|---|---|
| **Loki** | Agregação e armazenamento de logs |
| **Promtail** | Coleta de logs dos containers e encaminha ao Loki |
| **Grafana** | Visualização de logs e métricas |
| **Prometheus** | Coleta e armazenamento de métricas |
| **cAdvisor** | Exporta métricas de hardware/CPU/RAM dos containers |
| **RabbitMQ** | Message broker (comunicação assíncrona) |
| **Python API** | API REST Flask — produz mensagens e expõe `/metrics` |

## Pré-requisitos

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Como executar

```sh
git clone https://github.com/josemanzoli/logs-with-loki
cd logs-with-loki

# Copie e configure as variáveis de ambiente
cp .env.example .env   # ajuste as senhas se necessário

docker compose up -d
```

## Acessos

| Interface | URL | Credenciais |
|---|---|---|
| **Grafana** | http://localhost:3000 | admin / `GRAFANA_PASSWORD` do `.env` |
| **Prometheus** | http://localhost:9090 | — |
| **RabbitMQ Management** | http://localhost:15672 | admin / `RABBITMQ_PASSWORD` do `.env` |
| **cAdvisor** | http://localhost:8080 | — |
| **API Health** | http://localhost:5000/health/live | — |
| **API Metrics** | http://localhost:5000/metrics | — |

## Tópicos cobertos na aula

- **Logs centralizados** com Loki + Promtail + Grafana
- **Métricas de aplicação** com `prometheus_client` (Flask)
- **Métricas de infraestrutura** com cAdvisor (CPU/RAM por container)
- **Comunicação síncrona** via REST (`POST /message`)
- **Comunicação assíncrona** via RabbitMQ (fanout exchange)
- **Backpressure & Load Shifting** com filas duráveis
- **Escalabilidade horizontal** — `docker compose up --scale api-rest=N`
- **Health Checks** — Liveness (`/health/live`) e Readiness (`/health/ready`)
- **Dead Letter Queue (DLQ)** — mensagens com erro vão para `logs_dlq`
- **Idempotência** — consumer descarta duplicatas via `correlationId`
- **Distributed Tracing** — rastreie uma mensagem de ponta a ponta pelo `correlationId` no Grafana

## Estrutura do projeto

```
logs-with-loki/
├── config/
│   ├── grafana/provisioning/datasources/   # Datasources automáticos (Loki + Prometheus)
│   ├── loki/                               # Configuração do Loki
│   ├── promtail/                           # Configuração do Promtail
│   └── prometheus/                         # Configuração do Prometheus + cAdvisor
├── docker-compose.yml
└── .env                                    # Variáveis de ambiente (não commitado)
```

## Repositórios Relacionados

- **[josemanzoli/python-api](https://github.com/josemanzoli/python-api)** — API REST Flask com RabbitMQ producer e consumer, instrumentada com Prometheus. Usada como serviço de aplicação neste laboratório.

## Créditos

Este projeto foi baseado no trabalho original de **Bruno Paz**:
- Repositório: [brpaz/logs-with-loki](https://github.com/brpaz/logs-with-loki)
- Artigo: [Better docker logs with Loki](https://brunopaz.dev/blog/better-docker-logs-with-loki)

Modificado e expandido por **Jose Manzoli** com modernização da stack, adição de RabbitMQ, Python API, Prometheus, cAdvisor e exemplos de arquitetura de microsserviços.

## License

[MIT](LICENSE)