# ETL no Databricks com Arquitetura Medalhão (Bronze → Silver → Gold)

> Repositório de estudos práticos para construção de um pipeline de dados em **Delta Lake** no **Databricks**, seguindo a **arquitetura Medalhão**. Os notebooks `.ipynb` estão **numerados** e contêm explicações passo a passo dentro de cada arquivo.

---

## 🧭 Objetivo

Demonstrar, de ponta a ponta, um fluxo de **ingestão, padronização, enriquecimento e consumo analítico** de dados usando **Delta Tables** e boas práticas de modelagem **Bronze/Silver/Gold**.

---

## 🏗️ Arquitetura (visão geral)

```
[Fonte(s) de Dados]
       │
       ▼
   BRONZE (raw landing) ──► dados crus, esquema flexível, auditoria
       │
       ▼
   SILVER (curated) ──────► limpeza, padronização, chaves, qualidade
       │
       ▼
   GOLD (business) ───────► métricas, dimensões, fatos, consumo BI/ML
```

* **Bronze**: armazenamento quase bruto (raw) com mínimas transformações.
* **Silver**: tratamento, junções entre entidades, validação de qualidade.
* **Gold**: modelagem analítica (fatos e dimensões) e KPIs prontos para consumo.

---

## 📁 Estrutura dos Notebooks

> Execute na ordem numérica para reproduzir o pipeline.

| Ordem | Notebook                            | Camada | Propósito (resumo)                                                |
| ----: | ----------------------------------- | ------ | ----------------------------------------------------------------- |
|    01 | `01_Primeira_Camada.ipynb`          | Bronze | Inicializa a estrutura/lakehouse e configurações base do projeto. |
|    02 | `02_Tabela_Bronze_Clientes.ipynb`   | Bronze | Ingestão de **Clientes** (raw → Delta), schema/particionamento.   |
|    03 | `03_Tabela_Bronze_Produtos.ipynb`   | Bronze | Ingestão de **Produtos** (raw → Delta), validações básicas.       |
|    04 | `04_Tabela_Bronze_Transacoes.ipynb` | Bronze | Ingestão de **Transações** (raw → Delta), controles de carga.     |
|    05 | `05_Segunda_Camada.ipynb`           | Silver | Orquestra a passagem Bronze → Silver (bases e dependências).      |
|    06 | `06_Tabela_Silver_Clientes.ipynb`   | Silver | Padroniza clientes (tipos, dedup, chaves naturais/surrogates).    |
|    07 | `07_Tabela_Silver_Produtos.ipynb`   | Silver | Padroniza produtos (normalização, catálogos, SKUs).               |
|    08 | `08_Tabela_Silver_Transacoes.ipynb` | Silver | Consolida transações (FKs, regras de negócio e qualidade).        |
|    09 | `09_Camada_Final.ipynb`             | Gold   | Monta visão de negócio (fatos/dimensões, métricas e agregações).  |
|    10 | `10_Tabela_Gold.ipynb`              | Gold   | Publica tabelas **Gold** finais para consumo por BI/ML.           |

> **Obs.:** Cada notebook contém comentários/explicações adicionais sobre decisões de modelagem e qualidade de dados.

---

## ⚙️ Pré‑requisitos

* **Conta Databricks** (Community Edition ou Workspace da sua organização)
* **Configure os caminhos conforme nomenclatura dos arquivos na pasta Dados**

---

## ▶️ Como executar

1. **Importe** os notebooks para um **Workspace Databricks**.
2. **Anexe** todos ao **mesmo cluster** (ou compatíveis) e **configure** widgets/paths.
3. **Execute em ordem** (01 → 10), verificando os resultados ao final de cada etapa.
4. **Valide** as tabelas Delta criadas em cada camada (SQL UI do Databricks ajuda!).

---

## 🗃️ Tabelas esperadas (exemplo)

> Adapte os nomes conforme seus notebooks criam.

**Bronze**

* `bronze.clientes_raw`
* `bronze.produtos_raw`
* `bronze.transacoes_raw`

**Silver**

* `silver.clientes` (limpo/deduplicado)
* `silver.produtos` (padronizado)
* `silver.transacoes` (consolidado + FKs)

**Gold**

* `gold.fato_vendas`
* `gold.dim_cliente`, `gold.dim_produto`
* `gold.kpi_vendas_diarias` (ex.: por dia, UF, canal, etc.)

---

## 📊 Consumo (BI/ML)

* Conecte ferramentas de **BI** (Power BI, Databricks SQL, Tableau) diretamente às tabelas **Gold**.
* Para **ML**, a camada **Silver** costuma ser melhor para feature stores; a **Gold** serve para scoring/monitoramento.


---

## 🗂️ Estrutura sugerida do repositório

```
.
├── notebooks/
│   ├── 01_Primeira_Camada.ipynb
│   ├── 02_Tabela_Bronze_Clientes.ipynb
│   ├── 03_Tabela_Bronze_Produtos.ipynb
│   ├── 04_Tabela_Bronze_Transacoes.ipynb
│   ├── 05_Segunda_Camada.ipynb
│   ├── 06_Tabela_Silver_Clientes.ipynb
│   ├── 07_Tabela_Silver_Produtos.ipynb
│   ├── 08_Tabela_Silver_Transacoes.ipynb
│   ├── 09_Camada_Final.ipynb
│   └── 10_Tabela_Gold.ipynb
├── dados/ (opcional: amostras .csv/.json/.parquet)
└── README.md
```
