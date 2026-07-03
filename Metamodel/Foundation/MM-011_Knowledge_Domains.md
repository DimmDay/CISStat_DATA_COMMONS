# MM-011 — Knowledge Domains

Документ отвечает уже не на вопрос: «Какие существуют объекты?»
а на вопрос: «В каких предметных областях живут эти объекты?»

## Структура
1. Purpose
2. Scope
3. Domain Principles
4. Domain Model
5. Domain Catalogue
6. Domain Relationships
7. Ownership
8. Governance
9. Evolution

## Каталог доменов
1. Governance Domain
Управление проектом.

2. Architecture Domain
Все архитектурные документы.

3. Requirements Domain
Все требования.

4. Information Domain
Статистические данные.

5. Metadata Domain
- SDMX
- DDI
- SIMS
- ESMS
- ESQRS
- Semantic Domain
- Ontology
- RDF
- OWL
- Knowledge Graph
- Schema.org
- Integration Domain
- OpenAPI
- MCP
- External APIs
- ETL
- Streaming
- Analytics Domain
- TS Analysis
- Dashboards
- Forecasts
- Machine Learning
- Visualization
- AI Domain
- LLM
- AI Agents
- Prompt Library
- Embeddings
- RAG
- Portal Domain
- UX
- CMS
- Search
- Personalization
- Navigation
- Infrastructure Domain
- Kubernetes
- Docker
- PostgreSQL
- Neo4j
- Monitoring
- Deployment
- Domain Relationships

Например:

Portal
        │
        ▼
Integration
        │
        ▼
Information
        │
        ▼
Metadata
        │
        ▼
Semantic
        │
        ▼
Knowledge Graph
        │
        ▼
AI
Ownership

## Каждый домен имеет:

Domain Owner;
Architecture Owner;
Metadata Owner;
Data Steward;
Review Board.

Даже если сейчас все роли выполняет один человек, модель должна быть определена сразу.