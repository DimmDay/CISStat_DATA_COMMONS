# 1. Структура MM-000

1. Purpose
2. Scope
3. Normative References
4. Position of the Metamodel
5. Core Concepts
6. Knowledge Object Definition
7. Knowledge Object Identity
8. Knowledge Object Structure
9. Lifecycle
10. Relationships
11. Metadata
12. Semantic Layer
13. Physical Representations
14. Serialization
15. Compliance Rules
16. Extension Rules
17. Traceability
18. Future Evolution

# Определение Knowledge Object

Knowledge Object — фундаментальная архитектурная сущность CIS Data Commons, представляющая любой объект знаний, обладающий устойчивой идентичностью, семантическим значением, управляемым жизненным циклом, машиночитаемыми метаданными и формализованными отношениями с другими объектами знаний независимо от формы хранения, технологии реализации или способа представления.

# Минимальная структура объекта

Knowledge Object
│
├── Identity
├── Metadata
├── Semantic Definition
├── Lifecycle
├── Version
├── Relationships
├── Manifest
├── Representations
├── Provenance
└── Governance

# Важнейшее положение
Любой объект платформы обязан наследоваться от Knowledge Object.

Knowledge Object
│
├── Architecture Document
│
├── Requirement
│
├── ADR
│
├── Standard
│
├── API
│
├── Ontology
│
├── Dataset
│
├── Indicator
│
├── Time Series
│
├── Classification
│
├── Metadata Scheme
│
├── Organization
│
├── Statistical Program
│
├── Publication
│
├── Dashboard
│
├── Portal Page
│
├── AI Agent
│
└── Service

# Identity / Подлинность

Здесь фиксируется принцип:
У объекта существует одна неизменяемая идентичность.

Она независима от:
- версии;
- имени файла;
- расположения;
- URI представления;
- формата сериализации.

# Relationships / Отношения

Стандартный словарь отношений:
- implements
- extends
- references
- dependsOn
- derivedFrom
- describes
- contains
- ownedBy
- publishedBy
- validatedBy
- governedBy
- uses
- defines
- conformsTo
- versionOf
- replaces
- supersedes

# Представления

Один объект может существовать одновременно в разных формах.

Knowledge Object
        │
        ├── Markdown
        ├── YAML Manifest
        ├── JSON-LD
        ├── RDF
        ├── OWL
        ├── OpenAPI
        ├── SDMX
        ├── DDI
        ├── XBRL
        └── HTML
Это означает, что Markdown-файл — лишь одно из представлений объекта, а не сам объект.

# Метамодель как основа графа знаний

Каждый Knowledge Object должен автоматически становиться узлом графа знаний.
Минимальные свойства узла:

- Identity
- Type
- Title
- Status
- Version
- URI
- Owner
- Relations
- Metadata
- Semantic Definition

# Правила соответствия

Каждый Knowledge Object обязан:

- иметь уникальную идентичность;
- иметь обязательный манифест (STD-001);
- соответствовать правилам идентификации (STD-002);
- поддерживать трассируемость;
- иметь определенный жизненный цикл;
- быть пригодным для машинной обработки;
- иметь семантическое описание.

# Knowledge Object Contract

Каждый объект должен удовлетворять контракту, включающему:

- идентичность;
- тип;
- обязательные метаданные;
- правила версионирования;
- допустимые отношения;
- жизненный цикл;
- сериализации;
- требования к трассируемости;
- требования к управлению.

Тогда все последующие типы объектов (ADR, REQ, DM, ONT, API, KG, Dataset, Indicator, Time Series) будут не просто документами или файлами, а специализированными реализациями единого Knowledge Object Contract. Это сделает архитектуру CIS Data Commons чрезвычайно последовательной и подготовленной к автоматической обработке, интеграции с графом знаний и работе ИИ-агентов через MCP.

# Standards Alignment / Согласование стандартов

| Knowledge Object Type      | External Standard | Mapping                  |
| -------------------------- | ----------------- | ------------------------ |
| Classification             | SKOS              | skos:ConceptScheme       |
| Classification Item        | SKOS              | skos:Concept             |
| Statistical Classification | XKOS              | xkos:ClassificationLevel |
| Dataset                    | DCAT              | dcat:Dataset             |
| Distribution               | DCAT              | dcat:Distribution        |
| Metadata Record            | ADMS              | adms:Asset               |
| Vocabulary                 | SKOS              | skos:ConceptScheme       |
| API                        | OpenAPI           | OpenAPI Document         |
| Ontology                   | OWL               | owl:Ontology             |

Каждый объект знаний может объявлять одно или несколько внешних семантических представлений посредством conformsTo.

# Versioning Policy / Политика версионирования

Правила версионирования major.minor.patch:

## PATCH

Исправления.
Никакие ссылки не ломаются.
1.0.1

## MINOR

Добавлены новые свойства.
Все старые объекты продолжают работать.
1.2.0

## MAJOR

Изменена семантика объекта.
Требуется миграция.
2.0.0

## Replacement Knowledge Object / Замена объектов знаний

Knowledge Object может заменить Knowledge Object только через отношение supersedes, а не изменение существующего объекта.

## Dataset dependency / Зависимость датасета

Если существует зависимость Dataset от Classification v1, то Classification v2 не должна автоматически заменить v1.
Иначе нарушится воспроизводимость статистики.

## AI Agent

Agent:

  имеет возможности:
    - create/создавать
    - classify/классифицировать
    - annotate/аннотировать
    - summarize/кратко излагать
    - recommend/рекомендовать
    - validate/проверять
    - infer/делать вывод

  имеет ограничения:

    mayModify/вносить изменения: false
    mayDelete/удалять: false
    mayApprove/утверждать: false
    mayPublish/публиковать: false
    modifies Canonical Classification/изменять классификации: false
    modifies Ontology/изменять онтологии: false
    deletes Knowledge Object/удалять объект знаний: false
    requiresHumanApproval/требовать одобрение человека: true

  допустимые отношения:
    creates/создавать: Dataset, Metadata, Report/отчет
    reads/читать: Classification, Ontology

  уровень доверия:
    Advisory/консультации
       ↓
    Assisted/оказание помощи
       ↓
    Operational/действие
       ↓
    Autonomous/автономность
