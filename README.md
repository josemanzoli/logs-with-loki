# Observability Lab: Loki, Grafana, Prometheus, Tempo & RabbitMQ

Laboratório de observabilidade e mensageria para uso em aulas de **Arquitetura de Microsserviços**. Este projeto demonstra na prática conceitos como logs centralizados, métricas, distributed tracing, comunicação assíncrona, resiliência e persistência.

## Stack

| Serviço | Função |
|---|---|
| **Loki** | Agregação e armazenamento de logs |
| **Grafana** | Visualização de logs, métricas e traces |
| **Prometheus** | Coleta e armazenamento de métricas |
| **Tempo** | Armazenamento de traces distribuídos (OTLP) |
| **PostgreSQL** | Persistência de dados das mensagens processadas |
| **cAdvisor** | Exporta métricas de hardware/CPU/RAM dos containers |
| **RabbitMQ** | Message broker (comunicação assíncrona) |
| **Python API** | API REST Flask — produz mensagens e expõe métricas/traces |
| **Python Consumer** | Worker assíncrono — consome, persiste e gera traces |

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
| **Tempo (API)** | http://localhost:3200 | — |
| **RabbitMQ Management** | http://localhost:15672 | admin / `RABBITMQ_PASSWORD` do `.env` |
| **cAdvisor** | http://localhost:8080 | — |
| **API Endpoints** | http://localhost:5000/messages | — |

## Tópicos cobertos na aula

- **Logs centralizados** com Loki + Promtail + Grafana
- **Distributed Tracing** com **Grafana Tempo** — rastreie o fluxo completo (API → RabbitMQ → Consumer)
- **Trace-to-Logs** — Pule de um trace no Tempo diretamente para os logs no Loki usando o `correlationId`
- **Métricas de aplicação** com `prometheus_client` (Flask)
- **Métricas de infraestrutura** com cAdvisor (CPU/RAM por container)
- **Persistência SQL** — Mensagens salvas em **PostgreSQL** via worker assíncrono
- **Comunicação assíncrona** via RabbitMQ (DLQ, Retry com Backoff, Idempotência)
- **Resiliência (Circuit Breaker)** — Proteção de falhas em cascata com `pybreaker`
- **Health Checks Profundos** — Liveness e Readiness probes integradas ao estado do circuito

## Estrutura do projeto

```
logs-with-loki/
├── config/
│   ├── grafana/provisioning/  # Datasources automáticos (Loki, Prometheus, Tempo)
│   ├── loki/                  # Configuração do Loki
│   ├── promtail/              # Configuração do Promtail
│   ├── prometheus/            # Scrape config p/ API e cAdvisor
│   └── tempo/                 # Configuração de armazenamento de traces
├── docker-compose.yml
└── .env                       # Variáveis de ambiente (não commitado)
```

## Repositórios Relacionados

- **[josemanzoli/python-api](https://github.com/josemanzoli/python-api)** — Repositório da aplicação usada neste laboratório. Contém a lógica de negócio, instrumentação OTel e modelos SQL.

## Créditos

Este projeto foi baseado no trabalho original de **Bruno Paz**:
- Repositório: [brpaz/logs-with-loki](https://github.com/brpaz/logs-with-loki)
- Artigo: [Better docker logs with Loki](https://brunopaz.dev/blog/better-docker-logs-with-loki)

Modificado e expandido por **Jose Manzoli** com modernização da stack, adição de RabbitMQ, PostgreSQL, Grafana Tempo, OpenTelemetry e exemplos práticos de padrões de microsserviços.

## License

[MIT](LICENSE)