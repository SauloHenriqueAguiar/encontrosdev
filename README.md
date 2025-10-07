# Encontros Tech

Aplicação web completa para gerenciamento de eventos de tecnologia, desenvolvida com Python/Flask e infraestrutura cloud-native.

## Sobre o Projeto

Encontros Tech é uma plataforma que permite gerenciar eventos técnicos, meetups e encontros da comunidade de tecnologia. O sistema oferece uma interface web intuitiva e uma API REST completa para criar, visualizar, buscar e editar eventos de forma simples e segura.

A aplicação foi desenvolvida seguindo boas práticas de engenharia de software, com foco em observabilidade, escalabilidade e pronta para produção em ambientes Kubernetes. Cada evento possui um token único de edição, permitindo que organizadores gerenciem seus eventos sem necessidade de autenticação tradicional.

## Funcionalidades Principais

- Criação de eventos via interface web ou API REST
- Listagem de eventos com interface moderna em cards
- Sistema de busca que pesquisa em título, descrição e localização
- Visualização detalhada de eventos específicos
- Edição de eventos via token único (sem necessidade de autenticação)
- Gerenciamento seguro através de UUID token por evento
- API REST completa para integração com outros sistemas
- Interface responsiva com Bootstrap 5
- Validação robusta de dados com Pydantic
- Sistema de logging estruturado com níveis configuráveis
- Instrumentação completa com métricas Prometheus
- Health checks para monitoramento
- Suporte a múltiplos workers com Gunicorn

## Tecnologias Utilizadas

**Backend**
- Python 3.11+
- Flask 3.0.0 (framework web)
- SQLAlchemy 2.0.43 (ORM)
- Pydantic 2.11.7 (validação de dados)
- Gunicorn 21.2.0 (servidor WSGI)
- psycopg2-binary 2.9.10 (driver PostgreSQL)
- python-dotenv 1.1.1 (gerenciamento de variáveis)

**Frontend**
- Jinja2 3.1.6 (template engine)
- Bootstrap 5.3.0 (framework CSS)
- Bootstrap Icons
- JavaScript Vanilla
- CSS customizado

**Banco de Dados**
- PostgreSQL (managed database)
- SQLAlchemy ORM

**DevOps e Infraestrutura**
- Docker (containerização)
- Kubernetes (orquestração)
- GitHub Actions (CI/CD)
- Digital Ocean (cloud provider)
- Docker Hub (registry)

**Monitoramento e Observabilidade**
- Prometheus (coleta de métricas)
- prometheus-flask-exporter 0.23.0
- prometheus-client 0.21.1
- Grafana (visualização)
- Kube State Metrics
- Node Exporter

**Qualidade**
- pytest 8.3.4
- Logging estruturado customizado

## Estrutura do Projeto

```
encontrosdev/
├── .github/
│   └── workflows/
│       └── main.yml              # Pipeline CI/CD
├── k8s/                          # Manifestos Kubernetes
│   ├── deployment.yaml           # Deploy da aplicação + Service
│   ├── prometheus.yaml           # Stack Prometheus + Grafana
│   └── secret.yaml               # Secrets do banco
├── src/                          # Código-fonte
│   ├── core/                     # Módulos principais
│   │   ├── database.py           # Configuração do banco
│   │   ├── logging.py            # Sistema de logging
│   │   └── settings.py           # Configurações e variáveis
│   ├── models/                   # Modelos SQLAlchemy
│   │   └── event.py              # Modelo Event
│   ├── routers/                  # Rotas Flask (Blueprints)
│   │   ├── api_router.py         # API REST
│   │   └── page_router.py        # Rotas Web
│   ├── schemas/                  # Schemas Pydantic
│   │   └── event.py              # Validação Event
│   ├── services/                 # Lógica de negócio
│   │   └── event_service.py      # Serviço de eventos (CRUD)
│   ├── static/                   # Arquivos estáticos
│   │   ├── css/
│   │   │   └── style.css         # Estilos customizados
│   │   └── js/
│   │       └── main.js           # JavaScript
│   ├── templates/                # Templates Jinja2
│   │   ├── base.html             # Template base
│   │   └── events/               # Templates de eventos
│   │       ├── create.html
│   │       ├── detail.html
│   │       ├── edit.html
│   │       ├── list.html
│   │       └── not_found.html
│   ├── tests/                    # Testes automatizados
│   │   └── services/
│   │       └── test_event_service.py
│   ├── Dockerfile                # Imagem Docker
│   ├── main.py                   # Ponto de entrada
│   └── requirements.txt          # Dependências Python
└── README.md
```

## Configuração

### Pré-requisitos

**Desenvolvimento Local**
- Python 3.11 ou superior
- PostgreSQL 12 ou superior
- pip (gerenciador de pacotes Python)
- virtualenv ou venv
- Git

**Ambiente Docker**
- Docker 20.10 ou superior
- Docker Compose (opcional)

**Ambiente Kubernetes**
- Kubernetes 1.24 ou superior
- kubectl configurado
- Cluster Kubernetes (local ou cloud)
- Acesso ao Docker Hub

### Variáveis de Ambiente

| Variável | Descrição | Valor Padrão |
|----------|-----------|--------------|
| `DATABASE_URL` | String de conexão PostgreSQL | `postgresql://encontros_tech:encontros_tech@localhost:5432/encontros_tech` |
| `APP_TITLE` | Título da aplicação | `Encontros Tech` |
| `DEBUG` | Modo de debug | `False` |
| `HOST` | Host do servidor | `0.0.0.0` |
| `PORT` | Porta do servidor | `8000` |
| `LOG_LEVEL` | Nível de log | `INFO` |
| `LOG_FORMAT` | Formato do log | `colored` |
| `SERVICE_NAME` | Nome do serviço | `encontros-tech` |
| `SERVICE_VERSION` | Versão do serviço | `1.0.0` |
| `PROMETHEUS_MULTIPROC_DIR` | Diretório para métricas | `/tmp/prometheus_multiproc` |

**Formato do DATABASE_URL:**
```
postgresql://[usuario]:[senha]@[host]:[porta]/[database]?sslmode=[require|disable]
```

## Instalação e Execução

### Execução Local

**1. Clonar o repositório**
```bash
git clone <url-do-repositorio>
cd encontrosdev
```

**2. Criar e ativar ambiente virtual**
```bash
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows
```

**3. Instalar dependências**
```bash
cd src
pip install --upgrade pip
pip install -r requirements.txt
```

**4. Configurar variáveis de ambiente**
```bash
cat > .env << EOF
DATABASE_URL=postgresql://encontros_tech:encontros_tech@localhost:5432/encontros_tech
DEBUG=True
LOG_LEVEL=DEBUG
LOG_FORMAT=colored
HOST=0.0.0.0
PORT=8000
EOF
```

**5. Configurar banco de dados**
```bash
psql -U postgres
```
```sql
CREATE DATABASE encontros_tech;
CREATE USER encontros_tech WITH PASSWORD 'encontros_tech';
GRANT ALL PRIVILEGES ON DATABASE encontros_tech TO encontros_tech;
\q
```

**6. Executar aplicação**
```bash
# Desenvolvimento
python main.py

# Produção local
gunicorn --bind 0.0.0.0:8000 --workers 4 main:app
```

**7. Acessar aplicação**
```
http://localhost:8000
```

**8. Executar testes**
```bash
pytest tests/ -v
```

### Execução com Docker

**1. Build da imagem**
```bash
cd src
docker build -t encontros-tech:local .
```

**2. Executar container**
```bash
docker run -d \
  -p 8000:8000 \
  -e DATABASE_URL="postgresql://user:pass@host:5432/db" \
  -e DEBUG=False \
  -e LOG_LEVEL=INFO \
  --name encontros-tech \
  encontros-tech:local
```

**3. Verificar logs**
```bash
docker logs -f encontros-tech
```

**4. Acessar aplicação**
```
http://localhost:8000
```

### Deploy no Kubernetes

**1. Criar namespace**
```bash
kubectl create namespace tech-prod
```

**2. Criar secret com DATABASE_URL**
```bash
kubectl create secret generic encontros-tech-db-secret \
  --from-literal=DATABASE_URL="postgresql://user:pass@host:5432/db" \
  --namespace=tech-prod
```

**3. Aplicar manifests**
```bash
kubectl apply -f k8s/deployment.yaml -n tech-prod
```

**4. Verificar deploy**
```bash
# Verificar pods
kubectl get pods -n tech-prod

# Verificar services
kubectl get svc -n tech-prod

# Verificar logs
kubectl logs -f deployment/encontros-tech -n tech-prod
```

**5. Obter IP externo**
```bash
kubectl get svc encontros-tech-service -n tech-prod
```

**6. Deploy do Prometheus + Grafana (opcional)**
```bash
kubectl apply -f k8s/prometheus.yaml

# Obter senha do Grafana
kubectl get secret grafana -o jsonpath="{.data.admin-password}" | base64 --decode
echo
```

## Monitoramento

### Sistema de Logging

A aplicação possui sistema de logging estruturado com:
- Logs coloridos para desenvolvimento
- Níveis configuráveis: DEBUG, INFO, WARNING, ERROR, CRITICAL
- Logs de negócio para eventos importantes
- Logs de operações de banco de dados
- Logs de requisições HTTP com método, path e status

Exemplo de log:
```
2025-10-07 20:30:45 | INFO | encontros-tech | setup_logging:78 | Criando tabelas no banco de dados
2025-10-07 20:30:46 | INFO | encontros-tech.event_service | create_event:14 | Criando novo evento: Python Meetup
```

### Métricas Prometheus

A aplicação expõe métricas no endpoint `/metrics`:

**Métricas HTTP:**
- `flask_http_request_duration_seconds`: Histograma de duração das requisições
- `flask_http_request_total`: Total de requisições por método, path e status

**Métricas da aplicação:**
- `app_info`: Informações da aplicação (versão, nome)

**Métricas do processo:**
- `process_cpu_seconds_total`: CPU utilizado
- `process_resident_memory_bytes`: Memória RAM utilizada

**Acessar métricas:**
```bash
curl http://localhost:8000/metrics
```

### Prometheus Server

O Prometheus está configurado no Kubernetes com:
- Retenção de dados: 15 dias
- Intervalo de coleta: 10 segundos
- Scrape de pods, services e nós do cluster

**Queries úteis:**
```promql
# Taxa de requisições por segundo
rate(flask_http_request_total[5m])

# Latência média
rate(flask_http_request_duration_seconds_sum[5m]) / rate(flask_http_request_duration_seconds_count[5m])

# Requisições por status
sum by (status) (flask_http_request_total)
```

### Grafana

**Configuração:**
- Usuário: `admin`
- Senha: obtida via secret do Kubernetes
- Data Source: Prometheus (`http://prometheus-server`)

**Obter senha:**
```bash
kubectl get secret grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```

**Acessar:**
```bash
kubectl get svc grafana
# Acessar http://<EXTERNAL-IP>
```

## Modelo de Dados

### Tabela: events

| Coluna | Tipo | Constraints | Descrição |
|--------|------|-------------|-----------|
| `id` | Integer | PRIMARY KEY, AUTO INCREMENT | Identificador único |
| `title` | String | INDEX | Título do evento |
| `description` | Text | NULLABLE | Descrição detalhada |
| `date` | DateTime | DEFAULT utcnow() | Data e hora do evento |
| `location` | String | - | Local do evento |
| `edit_token` | String | UNIQUE, INDEX | Token UUID para edição |

### Schema de Requisição (POST/PUT)

```json
{
  "title": "Python Meetup SP",
  "description": "Encontro mensal de Pythonistas",
  "date": "2025-10-15T19:00:00",
  "location": "São Paulo - SP",
  "technologies": ["Python", "Flask", "SQLAlchemy"]
}
```

### Schema de Resposta

```json
{
  "id": 1,
  "title": "Python Meetup SP",
  "description": "Encontro mensal de Pythonistas",
  "date": "2025-10-15T19:00:00",
  "location": "São Paulo - SP",
  "technologies": ["Python", "Flask", "SQLAlchemy"],
  "edit_token": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
}
```

## API REST

### Endpoints

**Criar evento**
```http
POST /api/events/
Content-Type: application/json

{
  "title": "Python Meetup",
  "description": "Encontro de Pythonistas",
  "date": "2025-10-15T19:00:00",
  "location": "São Paulo - SP"
}
```

**Listar eventos**
```http
GET /api/events/?q=python&limit=10&offset=0
```

**Buscar por token**
```http
GET /api/events/by-token/{token}
```

**Atualizar evento**
```http
PUT /api/events/by-token/{token}
Content-Type: application/json

{
  "title": "Novo título",
  "description": "Nova descrição",
  "date": "2025-10-16T20:00:00",
  "location": "Novo local"
}
```

## CI/CD

O projeto possui pipeline completo no GitHub Actions com três jobs:

**CI (Integração Contínua):**
- Checkout do código
- Setup Python 3.13
- Instalação de dependências
- Execução de testes
- Build e push da imagem Docker

**CD-Staging:**
- Deploy automático no namespace `tech-staging`
- Executado após sucesso do CI

**CD-Prod:**
- Deploy no namespace `tech-prod`
- Requer aprovação manual
- Executado após sucesso do Staging

**Secrets necessários:**
- `DOCKERHUB_TOKEN`: Token Docker Hub
- `KUBE_CONFIG`: Kubeconfig em base64
- `DATABASE_URL`: String de conexão

## Arquitetura

O projeto segue arquitetura em camadas:

1. **Routers** (Controladores): Recebem requisições HTTP
2. **Services** (Serviços): Lógica de negócio
3. **Models** (Modelos): Representação do banco
4. **Schemas** (DTOs): Validação e serialização

**Características:**
- Aplicação stateless (sem estado local)
- Horizontal scaling (2 réplicas no K8s)
- Load Balancer configurado
- Multiprocessing com Gunicorn (4 workers)
- Usuário não-root no Docker
- Secrets gerenciados via Kubernetes
- SSL/TLS no banco de dados