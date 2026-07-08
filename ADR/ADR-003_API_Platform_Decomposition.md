# ADR-003

Document ID: ADR-003
Title: API Platform Decomposition
Status: Proposed
Date: 2026-07-08
Authors: Chief Architect, Architecture Review Board
Classification: Enterprise Architecture Decision (Draft)
Version: 0.1.0

Related Documents:

ARC-005 API First
ARC-008 AI Native
ARC-011 Open Standards First
ARC-018 Interoperability by Design
MM-010 Knowledge Object Taxonomy
ADR-001 Repository as Enterprise Knowledge System
ADR-002 Enterprise Data Fabric (планируется)

## 1. СТАТУС

Proposed.
Настоящий ADR подготовлен как черновая заготовка по итогам анализа экспертного фидбека к ТЗ на цифровую реконструкцию платформы CISStat. Решение подлежит рассмотрению и утверждению Architecture Review Board в соответствии с GOV-003.

## 2. КОНТЕКСТ

В текущей архитектуре API представлен единым компонентом — API Gateway. Такое представление не отражает разнородность потребителей платформы: пользовательские интерфейсы, системы государств-участников СНГ, семантические клиенты, статистические информационные системы и интеллектуальные агенты предъявляют различные требования к протоколу доступа.

ARC-018 §6 уже фиксирует канонические стандарты по категориям (SDMX для статистических данных, OpenAPI 3.1 для API, GraphQL как допустимый на техническом уровне интерфейс), однако архитектурно эти интерфейсы не выделены в самостоятельные, специализированные компоненты API-слоя.

## 3. ПРОБЛЕМА

Единый недифференцированный API Gateway приводит к следующим ограничениям:

- смешение протоколов и контрактов внутри одного компонента усложняет сопровождение;
- невозможность независимого развития каждого типа интерфейса (REST, семантический доступ, статистический обмен, доступ ИИ-агентов);
- усложнение версионирования при разнородных потребителях;
- затруднённая интеграция с MCP и AI-агентами без выделенного интерфейса;
- отсутствие явного канонического владельца для SPARQL-доступа к Knowledge Graph.

## 4. РЕШЕНИЕ

Принято решение декомпозировать единый API Gateway на специализированные интерфейсы в рамках самостоятельного архитектурного компонента — **API Platform**.

```
REST API
   │
   ▼
GraphQL
   │
   ▼
SPARQL
   │
   ▼
SDMX API
   │
   ▼
MCP API
   │
   ▼
Webhooks
```

Каждый интерфейс закрепляется за конкретным Canonical Authority (владельцем домена данных), а не существует как независимый от домена технический компонент:

| Интерфейс   | Canonical Authority (домен-владелец)         |
|-------------|-----------------------------------------------|
| REST API    | Data Models (DM-*), прикладные сервисы         |
| GraphQL     | Data Models (DM-*), композитные представления  |
| SPARQL      | Knowledge Graph (KG-*)                         |
| SDMX API    | Metadata / Information Domain (статистический обмен) |
| MCP API     | AI Platform (см. ADR-005), Knowledge Graph      |
| Webhooks    | Event Platform (см. ADR-004)                    |

## 5. АРХИТЕКТУРНЫЕ ПРИНЦИПЫ РЕШЕНИЯ

### 5.1 One Interface — One Canonical Authority

Каждый специализированный API-интерфейс имеет единственного владельца домена, ответственного за его контракт, в соответствии с принципом Single Source of Truth (GOV-002 §2.3).

### 5.2 OpenAPI as Baseline Contract

REST API и GraphQL обязаны иметь машиночитаемую спецификацию (OpenAPI 3.1 / GraphQL Schema) в соответствии с ARC-018 §8.

### 5.3 SPARQL as Native Knowledge Graph Access

SPARQL-интерфейс предоставляет прямой семантический доступ к Knowledge Graph и не дублирует функциональность REST/GraphQL.

### 5.4 SDMX API as Statistical Exchange Standard

SDMX API реализует канонический стандарт статистического обмена (ARC-018 §6) для взаимодействия с национальными статистическими системами и международными организациями.

### 5.5 MCP API as AI-Native Interface

MCP API проектируется в соответствии с ARC-008 (AI Native) и является основным интерфейсом доступа интеллектуальных агентов к платформе, дополняя, а не заменяя REST/GraphQL/SPARQL.

### 5.6 Webhooks as Asynchronous Notification Channel

Webhooks обеспечивают асинхронное уведомление внешних потребителей о событиях платформы и являются точкой интеграции с Event Platform (ADR-004).

## 6. КОНЦЕПТУАЛЬНАЯ МОДЕЛЬ

```
                    API Platform
                          │
   ┌──────────┬───────────┼────────────┬───────────┬───────────┐
   ▼          ▼           ▼            ▼           ▼           ▼
 REST API  GraphQL     SPARQL       SDMX API     MCP API    Webhooks
   │          │           │            │           │           │
   ▼          ▼           ▼            ▼           ▼           ▼
Data      Data        Knowledge    Metadata /    AI          Event
Models    Models       Graph       Information   Platform    Platform
                                    Domain
```

## 7. ПОСЛЕДСТВИЯ ПРИНЯТИЯ РЕШЕНИЯ

Положительные:
- независимое развитие каждого интерфейса без взаимного влияния;
- явное закрепление владельцев контрактов;
- упрощение подключения ИИ-агентов через MCP без перегрузки REST-слоя;
- соответствие сложившейся практике интероперабельности (ARC-018).

Потенциальные ограничения:
- увеличение количества API-контрактов, подлежащих сопровождению;
- необходимость единой политики версионирования и аутентификации для всех интерфейсов (ARC-018 §12);
- риск дублирования функциональности между REST и GraphQL при недостаточно чётком разграничении сценариев использования.

## 8. ВЛИЯНИЕ НА АРХИТЕКТУРУ

| Домен                    | Влияние      |
|--------------------------|--------------|
| Application Architecture | Критическое  |
| Integration Architecture | Высокое      |
| AI Architecture           | Высокое      |
| Knowledge Graph            | Среднее      |
| Security Architecture      | Высокое (единая схема аутентификации по всем интерфейсам) |

## 9. РЕАЛИЗАЦИЯ

- API-000 Canonical Resource Model (уже запланирован в PROJECT_STATE §15);
- отдельные API-* спецификации для каждого интерфейса;
- единая политика версионирования и схемы ошибок для всех интерфейсов (ARC-018 §8);
- единая схема аутентификации (OAuth 2.1 / OpenID Connect) для REST, GraphQL, SPARQL, SDMX API, MCP API.

## 10. СООТВЕТСТВИЕ СТАНДАРТАМ

- OpenAPI 3.1;
- GraphQL Specification;
- SPARQL 1.1 Protocol;
- SDMX 3.x;
- Model Context Protocol (MCP);
- OAuth 2.1 / OpenID Connect (ARC-018 external_standards).

## 11. СВЯЗАННЫЕ АРХИТЕКТУРНЫЕ РЕШЕНИЯ

- ADR-002 — Enterprise Data Fabric (источник данных для REST/GraphQL/SDMX API);
- ADR-004 — Event Platform (интеграция с Webhooks);
- ADR-005 — AI Platform (владелец MCP API совместно с Knowledge Graph).

## 12. ОТКРЫТЫЕ ВОПРОСЫ (для рассмотрения ARB)

- Разграничение сценариев использования REST и GraphQL во избежание дублирования.
- Единая схема аутентификации и авторизации для всех шести интерфейсов.
- Порядок версионирования MCP API по мере эволюции протокола MCP.
