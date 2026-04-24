# Internet Monitor

Ferramenta para monitorar a qualidade da sua internet ao longo do tempo. Mede velocidade de download, upload e ping a cada hora, salva o histórico e exibe tudo em gráficos.

## Como funciona

Três serviços rodam juntos via Docker:

- **Speedtest Tracker** — faz o teste de velocidade automaticamente a cada hora e guarda os resultados
- **Prometheus** — coleta as métricas do Speedtest Tracker a cada 5 minutos e armazena por 90 dias
- **Grafana** — exibe os dados em dashboards com gráficos

## Portas

| Serviço            | Endereço                    |
| ------------------ | --------------------------- |
| Speedtest Tracker  | http://localhost:9100       |
| Prometheus         | http://localhost:9090       |
| Grafana            | http://localhost:9200       |

## Pré-requisitos

- [Docker](https://docs.docker.com/get-docker/) com Docker Compose instalado

## Instalação

**1. Clone o repositório**

```bash
git clone <url-do-repositorio>
cd internet-monitor
```

**2. Crie o arquivo `.env` a partir do exemplo**

```bash
cp .env.example .env
```

**3. Gere uma chave para o Speedtest Tracker**

```bash
echo "base64:$(openssl rand -base64 32)"
```

Cole o resultado no campo `APP_KEY` dentro do `.env`.

**4. Defina uma senha forte para o Grafana**

Edite o `.env` e coloque um valor seguro em `GRAFANA_PASSWORD` (mínimo 16 caracteres).

**5. Proteja o arquivo `.env`**

```bash
chmod 600 .env
```

**6. Suba os containers**

```bash
docker compose up -d
```

Aguarde alguns segundos e acesse http://localhost:9100 para ver o Speedtest Tracker e http://localhost:9200 para o Grafana.

## Uso

- O teste de velocidade roda sozinho a cada hora — não é necessário fazer nada.
- Acesse o **Grafana** (http://localhost:9200) com o usuário e senha definidos no `.env` para ver os gráficos.
- Os resultados são mantidos por **30 dias** no Speedtest Tracker e por **90 dias** no Prometheus.

## Parar e iniciar

```bash
# Parar
docker compose down

# Iniciar novamente
docker compose up -d

# Ver logs
docker compose logs -f
```

## Segurança

- Nunca suba o arquivo `.env` para o git — ele já está no `.gitignore`.
- Use senhas fortes para o Grafana.
- Estes serviços são feitos para uso local/doméstico; evite expô-los diretamente na internet sem proteção adicional.
