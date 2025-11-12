# 📦 Projeto de Engenharia de Dados - LogisticHub

<div align="center">

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

**Sistema completo de análise de dados para operações logísticas**

[Sobre](#-sobre) • [Funcionalidades](#-funcionalidades) • [Instalação](#-instalação) • [Uso](#-uso) • [Relatórios](#-relatórios) • [Arquitetura](#-arquitetura)

</div>

---

## 📖 Sobre

Este projeto é um pipeline completo de ETL (Extract, Transform, Load) desenvolvido para análise de dados de operações logísticas. Foi construído utilizando **PostgreSQL**, **Python** e **Docker** para facilitar a execução em qualquer ambiente.

O sistema processa dados de entregas, motoristas, veículos, transportadoras, centros de distribuição e clientes, gerando insights valiosos através de 10 relatórios analíticos focados em KPIs operacionais.

### 🎯 Objetivo

Fornecer à equipe de operações uma ferramenta robusta para:
- Monitorar performance operacional
- Identificar gargalos e oportunidades de melhoria
- Otimizar custos e recursos
- Melhorar a qualidade do serviço prestado

---

## ✨ Funcionalidades

### 🐳 Serviços Containerizados
- **PostgreSQL 15** - Banco de dados relacional
- **Pipeline ETL Python** - Processamento automatizado de dados
- **Gerador de dados sintéticos** - Dataset realista para testes

### 📊 Análises Disponíveis
- ⏱️ Tempo médio de entrega por rota
- 📈 Taxa de pontualidade por região
- 💰 Custo de frete por kg transportado
- 🏆 Ranking de motoristas por performance
- 🚚 Taxa de ocupação de veículos
- 🏭 Performance de centros de distribuição
- 📊 Crescimento mensal de entregas
- ⚠️ Análise de problemas operacionais
- 💼 Rentabilidade por cliente
- ⛽ Eficiência operacional e consumo

---

## 📁 Estrutura do Projeto
```
logistichub_container/
├── briefing/                      # Documentação do projeto
│   └── solicitacao_projeto.md     # Briefing detalhado
├── dataset/                       # Dados sintéticos (CSV)
│   ├── transportadoras.csv
│   ├── centros_distribuicao.csv
│   ├── veiculos.csv
│   ├── motoristas.csv
│   ├── clientes.csv
│   └── entregas.csv
├── reports/                       # Relatórios SQL (10 KPIs)
│   ├── 01_tempo_medio_entrega/
│   ├── 02_taxa_pontualidade/
│   ├── 03_custo_medio_frete/
│   ├── 04_top_motoristas/
│   ├── 05_taxa_ocupacao_veiculos/
│   ├── 06_ranking_centros/
│   ├── 07_crescimento_mensal/
│   ├── 08_taxa_problemas/
│   ├── 09_rentabilidade_cliente/
│   └── 10_eficiencia_operacional/
├── schema/                        # Estrutura do banco de dados
│   └── schema.sql                 # DDL das tabelas
├── docker-compose.yaml            # Orquestração dos containers
├── Dockerfile                     # Imagem do serviço ETL
├── requirements.txt               # Dependências Python
├── generate_data.py               # Gerador de dados sintéticos
├── etl.py                         # Pipeline de ETL
└── README.md                      # Este arquivo
```

---

## 🗄️ Modelo de Dados

O sistema trabalha com **6 tabelas relacionadas**:

### Tabelas Dimensão
- **transportadoras** - Empresas de transporte parceiras (10 registros)
- **centros_distribuicao** - CDs espalhados pelo Brasil (10 registros)
- **veiculos** - Frota de veículos (150 registros)
- **motoristas** - Cadastro de motoristas (200 registros)
- **clientes** - Base de clientes (1.500 registros)

### Tabela Fato
- **entregas** - Registro completo de entregas (10.000 registros)

### Relacionamentos
```
transportadoras ──┬── veiculos ──── entregas
                  └── motoristas ─┘
centros_distribuicao ──────────────┘
clientes ──────────────────────────┘
```

---

## 🚀 Instalação

### Pré-requisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop) instalado
- 4GB+ de RAM alocada para o Docker
- 3GB de espaço em disco disponível
- Git (para clonar o repositório)

### Passo 1: Clonar o Repositório
```bash
git clone https://github.com/matheus-felipe-soares/logistichub_container.git
cd logistichub_container
```

### Passo 2: Gerar os Dados Sintéticos
```bash
# Criar ambiente virtual Python (opcional mas recomendado)
python3 -m venv venv
source venv/bin/activate  # No Windows: venv\Scripts\activate

# Instalar dependências
pip install -r requirements.txt

# Gerar os dados
python3 generate_data.py
```

Você verá uma mensagem de sucesso e os arquivos CSV serão criados em `dataset/`.

### Passo 3: Iniciar os Containers
```bash
docker-compose up --build
```

**Aguarde alguns minutos** para:
- Download das imagens Docker
- Build do container ETL
- Criação do banco de dados
- Execução do schema SQL
- Carregamento dos dados via ETL

---

## 💻 Uso

### Acessar o Banco de Dados

Você pode conectar ao PostgreSQL usando qualquer cliente SQL:

| Parâmetro | Valor |
|-----------|-------|
| **Host** | `localhost` |
| **Porta** | `5433` |
| **Database** | `logistica` |
| **Usuário** | `postgres` |
| **Senha** | `postgrespass` |

#### Clientes SQL Recomendados
- [DBeaver](https://dbeaver.io/) - Gratuito e multiplataforma
- [pgAdmin](https://www.pgadmin.org/) - Interface oficial do PostgreSQL
- [DataGrip](https://www.jetbrains.com/datagrip/) - IDE paga (JetBrains)
- **psql** - Cliente de linha de comando

#### Exemplo via Terminal
```bash
# Acessar via psql
docker exec -it logistica_db psql -U postgres -d logistica

# Comandos úteis no psql:
\dt                    # Listar tabelas
\d entregas            # Descrever tabela
SELECT COUNT(*) FROM entregas;  # Contar registros
\q                     # Sair
```

### Executar Relatórios

#### Opção 1: Via Cliente SQL (GUI)
1. Conecte-se ao banco usando DBeaver/pgAdmin
2. Abra um dos arquivos `.sql` da pasta `reports/`
3. Execute a query

#### Opção 2: Via Terminal
```bash
# Exemplo: executar relatório de tempo médio de entrega
docker exec -it logistica_db psql -U postgres -d logistica \
  -f /reports/01_tempo_medio_entrega/query.sql
```

---

## 📊 Relatórios Disponíveis

### 1️⃣ Tempo Médio de Entrega
📂 `reports/01_tempo_medio_entrega/query.sql`

Analisa o tempo médio de entrega por rota e transportadora, identificando gargalos operacionais.

**Métricas:**
- Tempo médio, mínimo e máximo de entrega
- Agrupamento por transportadora e estado
- Análise de rotas mais rápidas e mais lentas

---

### 2️⃣ Taxa de Pontualidade
📂 `reports/02_taxa_pontualidade/query.sql`

Mede entregas no prazo vs atrasadas por região e transportadora.

**Métricas:**
- Percentual de pontualidade
- Comparativo no prazo vs atrasados
- Evolução mensal da pontualidade

---

### 3️⃣ Custo Médio de Frete
📂 `reports/03_custo_medio_frete/query.sql`

Calcula o custo por kg transportado e identifica rotas mais econômicas.

**Métricas:**
- Custo por kg por tipo de veículo
- Análise de eficiência (custo/km/kg)
- Taxa de ocupação de carga

---

### 4️⃣ Top Motoristas
📂 `reports/04_top_motoristas/query.sql`

Ranking dos melhores motoristas baseado em pontualidade e ausência de problemas.

**Métricas:**
- Top 5 por performance geral
- Top 10 por volume de entregas
- Bottom 5 com mais problemas

---

### 5️⃣ Taxa de Ocupação de Veículos
📂 `reports/05_taxa_ocupacao_veiculos/query.sql`

Analisa aproveitamento da capacidade de carga dos veículos.

**Métricas:**
- Ocupação por peso e volume
- Identificação de subutilização
- Veículos com melhor aproveitamento

---

### 6️⃣ Ranking de Centros de Distribuição
📂 `reports/06_ranking_centros/query.sql`

Performance dos CDs por volume de saída e receita gerada.

**Métricas:**
- Ranking por entregas e receita
- Estados atendidos por CD
- Taxa de pontualidade por centro

---

### 7️⃣ Crescimento Mensal
📂 `reports/07_crescimento_mensal/query.sql`

Variação percentual de entregas mês a mês por estado.

**Métricas:**
- Crescimento/queda mensal
- Estados com maior crescimento
- Tendências e sazonalidades

---

### 8️⃣ Taxa de Problemas
📂 `reports/08_taxa_problemas/query.sql`

Análise de avarias, extravios, devoluções e outros problemas.

**Métricas:**
- Tipos de problemas mais comuns
- Taxa de problemas por transportadora
- Impacto financeiro de problemas
- Relação entre distância e problemas

---

### 9️⃣ Rentabilidade por Cliente
📂 `reports/09_rentabilidade_cliente/query.sql`

Análise de receita gerada vs custo operacional por cliente.

**Métricas:**
- Top 20 clientes mais lucrativos
- Ticket médio por cliente
- Segmentação por faixa de receita
- Análise de recorrência

---

### 🔟 Eficiência Operacional
📂 `reports/10_eficiencia_operacional/query.sql`

Análise de consumo de combustível e margem operacional.

**Métricas:**
- Consumo estimado por tipo de veículo
- Custo de combustível vs receita
- Margem bruta por tipo de veículo
- Rotas mais eficientes

---

## 🏗️ Arquitetura Técnica
```
┌─────────────────────┐
│  generate_data.py   │  Gera dados sintéticos realistas
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   dataset/ (CSV)    │  6 arquivos CSV com relacionamentos
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Docker Compose     │  Orquestra os serviços
└──────────┬──────────┘
           │
      ┌────┴─────┐
      │          │
      ▼          ▼
┌──────────┐  ┌────────────┐
│   ETL    │  │ PostgreSQL │
│  Python  │→ │     DB     │
└──────────┘  └─────┬──────┘
                    │
                    ▼
              ┌──────────┐
              │ Reports  │
              │   SQL    │
              └──────────┘
```

### Fluxo de Dados

1. **Geração** → Script Python cria dados sintéticos em CSV
2. **Extração** → ETL lê os arquivos CSV
3. **Transformação** → Pandas processa e valida os dados
4. **Carregamento** → Inserção no PostgreSQL respeitando relacionamentos
5. **Análise** → Queries SQL geram insights de negócio

---

## 🔧 Comandos Úteis

### Docker
```bash
# Iniciar serviços
docker-compose up

# Iniciar em background
docker-compose up -d

# Parar serviços
docker-compose down

# Rebuild completo
docker-compose up --build

# Ver logs
docker-compose logs -f

# Remover volumes (limpar dados)
docker-compose down -v
```

### PostgreSQL
```bash
# Acessar banco
docker exec -it logistica_db psql -U postgres -d logistica

# Backup do banco
docker exec logistica_db pg_dump -U postgres logistica > backup.sql

# Restaurar backup
docker exec -i logistica_db psql -U postgres logistica < backup.sql
```

### Python (ETL)
```bash
# Reexecutar apenas o ETL
docker-compose restart etl

# Ver logs do ETL
docker logs logistica_etl
```

---

## ⚠️ Troubleshooting

### Problema: Porta 5432 em uso

**Erro:** `bind: address already in use`

**Solução:** A porta foi alterada para 5433 no `docker-compose.yaml`. Se ainda houver conflito, mude para outra porta livre.

---

### Problema: Tabelas vazias

**Causa:** ETL pode ter falhado

**Solução:**
```bash
# Ver logs do ETL
docker logs logistica_etl

# Reexecutar ETL
docker-compose restart etl
```

---

### Problema: Dados não foram gerados

**Causa:** Script `generate_data.py` não foi executado

**Solução:**
```bash
python3 generate_data.py
```

---

### Problema: Erro de permissão no Docker

**Solução Linux:**
```bash
sudo usermod -aG docker $USER
# Fazer logout e login novamente
```

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Uso |
|------------|--------|-----|
| **Python** | 3.11 | Pipeline ETL e geração de dados |
| **PostgreSQL** | 15 | Banco de dados relacional |
| **Docker** | Latest | Containerização |
| **Docker Compose** | Latest | Orquestração |
| **Pandas** | 2.1.4 | Manipulação de dados |
| **Faker** | 22.0.0 | Geração de dados sintéticos |
| **psycopg2** | 2.9.9 | Conexão Python-PostgreSQL |
| **SQLAlchemy** | 2.0.23 | ORM para Python |

---

## 📈 Métricas do Projeto

- **10.000** entregas processadas
- **1.500** clientes cadastrados
- **200** motoristas ativos
- **150** veículos na frota
- **10** centros de distribuição
- **10** transportadoras parceiras
- **10** relatórios analíticos
- **6** tabelas relacionadas

---

## 🎓 Aprendizados e Boas Práticas

### Engenharia de Dados
✅ Modelagem dimensional (Star Schema)  
✅ Pipeline ETL robusto com tratamento de erros  
✅ Normalização e integridade referencial  
✅ Otimização de queries com índices  

### DevOps
✅ Containerização completa com Docker  
✅ Ambiente reproduzível em qualquer máquina  
✅ Separação de serviços (PostgreSQL + ETL)  
✅ Health checks e dependências entre containers  

### Análise de Dados
✅ KPIs relevantes para negócio  
✅ Queries otimizadas e documentadas  
✅ Análises bônus com insights adicionais  
✅ Visualização clara de métricas  

---

## 🚧 Roadmap Futuro

- [ ] Dashboard interativo com Streamlit/Metabase
- [ ] API REST para consulta de dados
- [ ] Alertas automáticos para KPIs críticos
- [ ] Integração com sistemas externos
- [ ] Testes automatizados (pytest)
- [ ] CI/CD com GitHub Actions
- [ ] Documentação das APIs
- [ ] Monitoramento com Prometheus/Grafana

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

## 👤 Autor

<div align="center">

**Matheus Soares**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/matheusfssoares/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/matheus-felipe-soares)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:matheusfsilvasoares@gmail.com)

---

⭐ **Se este projeto foi útil, deixe uma estrela no repositório!**

</div>
