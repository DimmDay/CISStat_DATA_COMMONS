---
Repository Governance

1.1 ЦЕЛЬ РЕПОЗИТОРИЯ

Репозиторий CIS-Data-Commons является главным (master) репозиторием программы цифровой трансформации Статкомитета СНГ.

Он хранит:
- архитектуру платформы;
- технические требования;
- модели данных;
- онтологии;
- API-контракты;
- UX/UI-дизайн;
- инфраструктуру;
- документацию;
- исходный код сервисов;
- архитектурные решения (ADR).

Репозиторий не является исключительно репозиторием разработки — он является архитектурным репозиторием проекта.

1.2 АРХИТЕКТУРНЫЕ ПРИНЦИПЫ РЕПОЗИТОРИЯ

Все содержимое репозитория должно соответствовать следующим принципам.

     Принцип	                    Описание
Documentation First	     | Архитектура документируется до реализации.
API First	             | Любой сервис начинается с контракта OpenAPI/GraphQL/AsyncAPI.
Architecture First	     | Архитектурное решение принимается до начала кодирования.
Data First	             | Модель данных проектируется раньше интерфейсов.
Metadata First	         | Любые данные сопровождаются метаданными.
Open Standards First	 | Приоритет открытым международным стандартам (SDMX, DDI, RDF, Schema.org, XBRL, SKOS и др.).
Headless by Design	     | UI не зависит от CMS и потребляет только API.
AI Ready	             | Любой компонент проектируется с учетом использования ИИ-агентами и MCP.
Cloud Neutral	         | Развертывание возможно как в облаке, так и on-premise.
Vendor Neutral	         | Исключается зависимость от конкретного поставщика ПО.

1.3 РЕПОЗИТОРИЙ КАК АРХИТЕКТУРНЫЙ АКТИВ

**Пять уровней зрелости:**

- Level 0  Governance
- Level 1  Architecture
- Level 2  Specifications
- Level 3  Implementation
- Level 4  Operations

**Level 0 — Governance**
.github/
ADR/
Governance/
Standards/
LICENSE
README.md
CHANGELOG.md

Назначение:
- правила проекта;
- процессы;
- стандарты;
- архитектурные решения.

**Level 1 — Architecture**
Architecture/
Vision/
Business/
Enterprise/
Application/
Data/
Technology/
Security/
AI/
Roadmap/

Здесь не должно быть исходного кода.
Только архитектура.

**Level 2 — Specifications**
OpenAPI/
Ontology/
Data Models/
Schemas/
Contracts/
Documentation/

Это слой спецификаций.

**Level 3 — Implementation**
Portal/
Services/
Libraries/
Infrastructure/
DevOps/
Tests/

Это слой реализации.

**Level 4 — Operations**
Deployment/
Monitoring/
Runbooks/
Operations/

1.4 ЦЕЛЕВАЯ СТРУКТУРА РЕПОЗИТОРИЯ

CIS-Data-Commons
│
├── Governance/
├── Architecture/
├── Standards/
├── Documentation/
├── Requirements/
├── ADR/
├── RFC/
├── Decisions/
├── Data Models/
├── Metadata/
├── Ontology/
├── Knowledge Graph/
├── OpenAPI/
├── Infrastructure/
├── Development/
├── Portal/
├── TS Analysis/
├── AI/
├── Traceability/
├── Quality/
├── Security/
└── Operations/

1.5 ПРАВИЛА ВЕДЕНИЯ РЕПОЗИТОРИЯ

**Правило №1**
- Ни один Pull Request не может содержать код без документации.
- Каждый PR должен содержать минимум одно из:

ADR;
OpenAPI;
обновление документации;
UML;
ERD.

**Правило №2**
- Каждый сервис начинается с OpenAPI.
- Порядок разработки:
OpenAPI
↓
Review
↓
Implementation

**Правило №3**
- Архитектура хранится отдельно от реализации.
- Никакой UML внутри исходников.
- Вся архитектура хранится только в Architecture/.

**Правило №4**
- Любое архитектурное изменение оформляется ADR.

Например:
ADR-0001
Отказ от Liferay
Причина
Следствия
Альтернативы

**Правило №5**
- Все модели данных являются независимыми артефактами.

Data Models/
Indicator.yaml
Country.yaml
Dataset.yaml
Observation.yaml

**Правило №6**
- Никаких бинарных файлов.
- Все документы:
Markdown
PlantUML
Mermaid
YAML
JSON
XML

Это обеспечит возможность ревью изменений через Git.

1.6 СТРАТЕГИЯ ВЕТВЛЕНИЯ

main
Только стабильная архитектура.

develop
Текущая разработка.

feature/*
Новые функции.

adr/*
Архитектурные изменения.

docs/*
Документация.

1.7 ПРАВИЛА ИМЕНОВАНИЯ

Папки:
- Portal
- Services
- OpenAPI
- Knowledge-Graph
PascalCase без пробелов.

Файлы:
- Vision.md
- Architecture.md
- README.md
- ADR-0001.md
- OpenAPI.yaml

1.8 КОДИРОВКА

Единые требования:

UTF-8;
LF;
UTF-8 без BOM;
Markdown CommonMark;
YAML 1.2;
OpenAPI 3.1.

1.9 ЖИЗНЕННЫЙ ЦИКЛ ИЗМЕНЕНИЙ

Каждое изменение проходит следующие стадии:

Идея
    ↓
RFC (при необходимости)
    ↓
ADR
    ↓
Архитектурное ревью
    ↓
OpenAPI / Data Model
    ↓
Разработка
    ↓
Тестирование
    ↓
Документация
    ↓
Merge

Это гарантирует прослеживаемость архитектурных решений.

_____________________________________________________________________

Каталог ADR

ADR/
│
├── README.md
├── ADR-000_TEMPLATE.md
├── ADR-001_Repository_as_Enterprise_Knowledge_System.md
├── ADR-002_Headless_Architecture.md
├── ADR-003_API_First.md
├── ADR-004_Metadata_First.md
├── ADR-005_Knowledge_Graph.md
├── ADR-006_Everything_is_a_Knowledge_Object.md
...


#############################################################################################
# СПЕЦИФИКАЦИЯ
#############################################################################################

| Поле      | Тип     | Обязательное | Описание                                          |
| --------- | ------- | ------------ | ------------------------------------------------- |
| id        | String  | Да           | Уникальный идентификатор документа                |
| title     | String  | Да           | Полное название документа                         |
| version   | SemVer  | Да           | Версия документа                                  |
| status    | Enum    | Да           | Draft / Review / Approved / Deprecated / Archived |
| owner     | Object  | Да           | Владелец документа                                |
| author    | Array   | Да           | Автор(ы)                                          |
| related   | Object  | Да           | Связанные артефакты                               |
| tags      | Array   | Нет          | Теги                                              |
| graph_uri | URI     | Да           | URI узла графа знаний                             |
| ai_ready  | Boolean | Да           | Готовность к индексации AI                        |

##############################################################################################
# СТАНДАРТИЗАЦИЯ ИСПОЛЬЗУЕМЫХ ПОЛЕЙ
##############################################################################################

status:
  - Draft
  - Review
  - Approved
  - Accepted
  - Implemented
  - Deprecated
  - Superseded
  - Archived

classification:
  - Public
  - Internal
  - Confidential

document_class:
  - Standard
  - Specification
  - Architecture
  - Governance
  - ADR
  - Requirement
  - DataModel
  - API
  - Ontology
  - Guideline
  - Procedure

language:
  - en
  - ru
##############################################################################################
_________________________________________________________________________________________

CIS Data Commons
│
├── Governance
│   ├── GOV-001 Governance Framework
│   ├── GOV-002 Repository Governance
│   └── README.md
│
├── Standards
│   ├── STD-001 Knowledge Object Manifest Standard
│   └── STD-002 Knowledge Object Identifier & PID Standard
│
├── ADR
│   ├── README.md
│   └── ADR-001 Repository as Enterprise Knowledge System
│
└── (далее будут)
    ├── Architecture
    ├── Requirements
    ├── Data Models
    ├── Ontology
    ├── Knowledge Graph
    ├── OpenAPI
    └── …

__________________________________________________________________________________________

Разделение понятий:
| Понятие             | Пример                                     | Изменяемость   |
| ------------------- | ------------------------------------------ | -------------- |
| **Object Identity** | `ADR-001`                                  | Никогда        |
| **Persistent URI**  | `https://knowledge.cisstat.org/id/ADR-001` | Никогда        |
| **Version**         | `v1.2.0`                                   | Меняется       |
| **Location**        | путь к файлу в Git                         | Может меняться |

Knowledge Object
        │
        ├── Identity
        │
        ├── URI
        │
        ├── Metadata
        │
        ├── Versions
        │
        └── Representations

Architecture — устройство и принципы платформы;
Metamodel — фундаментальные понятия и объекты знаний;
Data Models — прикладные модели данных;
Ontology — формальная семантика этих объектов.
_______________________________________________________________________________________

что получится:

MM-000 говорит

существует Knowledge Object

↓

ONT-000 говорит

Knowledge Object —
это класс

↓

DM-000 говорит

Knowledge Object
имеет такие атрибуты

↓

API говорит

Knowledge Object
передается вот так

↓

KG говорит

Knowledge Object —
это вершина графа

То есть появляется идеальная архитектурная цепочка.
________________________________________________________________________________

В будущем ИИ сможет выполнять запросы вроде:

«Покажи все архитектурные решения, влияющие на показатели рынка труда, опубликованные после внедрения новой версии SDMX и использующие API v2».

Без отдельной поисковой логики — только за счет графа знаний и семантических связей.
---