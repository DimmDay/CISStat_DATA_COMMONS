# ADR-005

Document ID: ADR-005
Title: AI Platform as Dedicated Architecture Layer
Status: Proposed
Date: 2026-07-08
Authors: Chief Architect, Architecture Review Board
Classification: Enterprise Architecture Decision (Draft)
Version: 0.1.0

Related Documents:

ARC-008 AI Native
ARC-016 Semantics Before Implementation
ARC-017 Metadata Before Data
ARC-018 Interoperability by Design (§11 AI Interoperability)
MM-010 Knowledge Object Taxonomy (AI Object)
MM-011 Knowledge Domains (AI Domain)
ADR-003 API Platform Decomposition (планируется)
ADR-004 Event Platform (планируется)

## 1. СТАТУС

Proposed.
Настоящий ADR подготовлен как черновая заготовка по итогам анализа экспертного фидбека к ТЗ на цифровую реконструкцию платформы CISStat. Решение подлежит рассмотрению и утверждению Architecture Review Board в соответствии с GOV-003.

## 2. КОНТЕКСТ

Таксономия платформы (MM-010) уже содержит категорию **AI Object** (AI Agent, MCP Server, Prompt, Embedding Collection, Vector Index, RAG Collection, AI Tool, AI Workflow), а MM-011 определяет **AI Domain** (LLM, AI Agents, Prompt Library, Embeddings, RAG). Однако в текущем ТЗ работа с ИИ ограничена упоминанием MCP как единственного механизма и не выделена в самостоятельный сервисный архитектурный слой.

Отсутствие единого AI-слоя приводит к тому, что каждый AI-сценарий (поиск, суммаризация, генерация ответов, агентные workflow) потенциально реализуется отдельно, без единого управления моделями, промптами и качеством.

## 3. ПРОБЛЕМА

Ограничение AI-функциональности рамками MCP приводит к следующим рискам:

- отсутствие единой точки управления доступом к языковым моделям (разные сервисы обращаются к LLM напрямую и по-разному);
- отсутствие централизованного реестра промптов, что приводит к дублированию и рассинхронизации формулировок;
- отсутствие единого сервиса эмбеддингов и, как следствие, несовместимые векторные представления между сервисами;
- отсутствие формализованной оценки качества (Evaluation) и защитных механизмов (Guardrails), что создаёт риск несогласованного поведения ИИ-агентов;
- отсутствие единого Model Registry, что затрудняет замену или обновление моделей без риска для потребителей.

## 4. РЕШЕНИЕ

Принято решение выделить самостоятельный архитектурный слой — **AI Platform**, обеспечивающий единообразную работу всех интеллектуальных сервисов платформы поверх существующей таксономии AI Object (MM-010).

```
AI Platform
   │
   ├── LLM Gateway
   ├── RAG (Retrieval-Augmented Generation)
   ├── Prompt Registry
   ├── Embedding Service
   ├── Model Registry
   ├── Evaluation
   └── Guardrails
```

Соответствие компонентов AI Platform существующей таксономии MM-010 (AI Object):

| Компонент AI Platform | Соответствующий тип MM-010          |
|------------------------|--------------------------------------|
| LLM Gateway            | AI Tool / AI Agent (точка доступа)   |
| RAG                    | RAG Collection                       |
| Prompt Registry        | Prompt                               |
| Embedding Service      | Embedding Collection, Vector Index   |
| Model Registry         | (новый тип, требует расширения MM-010 через ADR — см. §12) |
| Evaluation             | AI Workflow                          |
| Guardrails             | AI Tool / AI Workflow                |

## 5. АРХИТЕКТУРНЫЕ ПРИНЦИПЫ РЕШЕНИЯ

### 5.1 Single Point of Model Access
Любое обращение любого сервиса платформы к языковой модели осуществляется исключительно через LLM Gateway. Прямые обращения сервисов к внешним или внутренним LLM в обход Gateway не допускаются.

### 5.2 Prompts as Governed Knowledge Objects
Промпты являются самостоятельными Knowledge Objects (тип Prompt, MM-010) и управляются через Prompt Registry с версионированием, а не хранятся в коде отдельных сервисов.

### 5.3 Consistent Embeddings Across Platform
Все векторные представления платформы формируются единым Embedding Service, что обеспечивает совместимость векторных индексов между Knowledge Graph, поиском и RAG-сценариями.

### 5.4 Every Model Change is Traceable
Замена, обновление или дообучение модели фиксируется в Model Registry и подлежит трассируемости в соответствии с ARC-010 (Everything is Traceable).

### 5.5 Evaluation Before Production
Ни одна модель, промпт или AI-агент не могут быть переведены в промышленную эксплуатацию без прохождения этапа Evaluation.

### 5.6 Guardrails as Mandatory Layer
Guardrails являются обязательным компонентом для всех AI-сценариев, взаимодействующих с внешними пользователями или публикующих данные, и реализуют ограничения, согласованные с ARC-012 (Security by Design).

### 5.7 AI Platform Consumes, Does Not Bypass, Semantic Layer
AI Platform использует Knowledge Graph и Metadata как источник истины (ARC-016, ARC-017) и не создаёт параллельного, несогласованного представления знаний.

## 6. КОНЦЕПТУАЛЬНАЯ МОДЕЛЬ

```
        Knowledge Graph / Metadata (источник истины)
                        │
                        ▼
                 Embedding Service
                        │
                        ▼
                  RAG Collection
                        │
                        ▼
   Prompt Registry ──▶ LLM Gateway ◀── Model Registry
                        │
                        ▼
                    Guardrails
                        │
                        ▼
                    Evaluation
                        │
                        ▼
        MCP API / AI Agents / Portal AI-сценарии
```

## 7. ПОСЛЕДСТВИЯ ПРИНЯТИЯ РЕШЕНИЯ

Положительные:
- единообразная работа всех AI-сценариев платформы;
- возможность контролируемой замены моделей без изменения потребителей;
- централизованное управление качеством и безопасностью ИИ;
- согласованность с уже принятой таксономией AI Object (MM-010).

Потенциальные ограничения:
- необходимость миграции существующих точечных AI-интеграций (если таковые есть) на единый Gateway;
- дополнительная операционная нагрузка на этапе Evaluation перед каждым релизом модели или промпта;
- необходимость расширения MM-010 новым типом объекта (Model Registry Entry), что требует отдельного ADR согласно правилу Extension Rules MM-010.

## 8. ВЛИЯНИЕ НА АРХИТЕКТУРУ

| Домен                  | Влияние      |
|-------------------------|--------------|
| AI Architecture          | Критическое  |
| Application Architecture | Высокое      |
| Security Architecture     | Высокое (Guardrails) |
| Knowledge Graph            | Среднее      |
| Integration Architecture   | Среднее (MCP API, см. ADR-003) |

## 9. РЕАЛИЗАЦИЯ

- LLM Gateway (единая точка доступа к моделям, внутренним и внешним);
- Prompt Registry с версионированием и связью с MM-010 (Prompt);
- Embedding Service с единым форматом векторных представлений;
- Model Registry (новый тип объекта, оформляется отдельным ADR на расширение MM-010);
- Evaluation Pipeline (метрики качества, регресс-тесты промптов и моделей);
- Guardrails Engine (фильтрация, ограничение действий агентов).

## 10. СООТВЕТСТВИЕ СТАНДАРТАМ

- Model Context Protocol (MCP);
- JSON Schema / JSON-LD (описание промптов и метаданных моделей);
- Open Standards First (ARC-011);
- согласованность с ARC-008 (AI Native).

## 11. СВЯЗАННЫЕ АРХИТЕКТУРНЫЕ РЕШЕНИЯ

- ADR-003 — API Platform Decomposition (MCP API как интерфейс AI Platform);
- ADR-004 — Event Platform (обновление LLM Cache по событию);
- будущий ADR на расширение MM-010 типом Model Registry Entry.

## 12. ОТКРЫТЫЕ ВОПРОСЫ (для рассмотрения ARB)

- Необходимость формального расширения MM-010 новым типом объекта для Model Registry.
- Разграничение терминологии между ТЗ и MM-010 (например, «Prompt Registry» в ТЗ и «Prompt» как тип объекта в MM-010) для соблюдения единого словаря.
- Политика Guardrails для сценариев, где AI-агент действует от имени пользователя через MCP.
