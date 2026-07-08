# ADR-002

Document ID: ADR-002
Title: Enterprise Data Fabric as Data Integration Layer
Status: Proposed
Date: 2026-07-08
Authors: Chief Architect, Architecture Review Board
Classification: Enterprise Architecture Decision (Draft)
Version: 0.1.0

Related Documents:

ARC-004 Knowledge Graph First
ARC-007 Infrastructure Agnostic
ARC-013 Interoperability by Design
ARC-014 No Vendor Lock-In
ARC-016 Semantics Before Implementation
ARC-017 Metadata Before Data
ARC-018 Interoperability by Design
MM-000 Knowledge Object Metamodel
MM-011 Knowledge Domains
ADR-001 Repository as Enterprise Knowledge System

## 1. СТАТУС

Proposed.
Настоящий ADR подготовлен как черновая заготовка по итогам анализа экспертного фидбека к ТЗ на цифровую реконструкцию платформы CISStat. Решение подлежит рассмотрению и утверждению Architecture Review Board в соответствии с GOV-003.
До утверждения ARB положения настоящего документа не являются обязательными.

## 2. КОНТЕКСТ

В текущей редакции архитектуры платформы упоминается Data Lakehouse, однако отсутствует самостоятельный архитектурный слой, отвечающий за управление потоками данных: их извлечение, преобразование, доставку и контрактное описание взаимодействия между источниками и потребителями.

Knowledge Graph в текущей архитектуре (ARC-004) выполняет функцию семантического ядра платформы, но не предназначен для решения задач физического перемещения и первичной интеграции данных из национальных статистических систем государств — участников СНГ.

При подключении новой страны СНГ или новой национальной статистической системы отсутствие выделенного интеграционного слоя приводит к разработке точечных, невоспроизводимых интеграционных решений для каждого нового источника.

## 3. ПРОБЛЕМА

Отсутствие самостоятельного слоя управления данными приводит к следующим рискам:

- разработка уникального интеграционного решения для каждой новой национальной статистической системы;
- отсутствие единого контракта данных между источником и платформой;
- смешение ответственности между семантическим слоем (Knowledge Graph) и слоем перемещения данных;
- отсутствие единого реестра метаданных для CDC/ETL/ELT-процессов;
- риск публикации данных без обязательных метаданных в обход ARC-017;
- сложность масштабирования при росте числа источников и потоков.

## 4. РЕШЕНИЕ

Принято решение ввести самостоятельный архитектурный слой — **Enterprise Data Fabric**, отвечающий за интеграцию, перемещение и контрактное описание потоков данных, отдельно от семантического слоя Knowledge Graph.

Целевая архитектурная последовательность слоёв:

```
Enterprise Data Fabric
        │
        ▼
Knowledge Graph
        │
        ▼
API Platform
        │
        ▼
Presentation Layer
```

Enterprise Data Fabric включает следующие функциональные компоненты:

- Change Data Capture (CDC);
- ETL (Extract-Transform-Load);
- ELT (Extract-Load-Transform);
- Streaming Ingestion;
- Data Contracts;
- Master Data Management;
- Metadata Registry (интеграционные метаданные, во взаимодействии с Metadata Service — см. отдельное предложение фидбека);
- Data Catalog;
- Data Lineage.

Подключение новой национальной статистической системы государства — участника СНГ должно осуществляться через типовой **Data Contract**, а не через разработку индивидуального интеграционного решения.

## 5. АРХИТЕКТУРНЫЕ ПРИНЦИПЫ РЕШЕНИЯ

### 5.1 Data Fabric as Integration Layer, Not Semantic Authority
Enterprise Data Fabric отвечает за перемещение и техническую интеграцию данных. Семантическим авторитетом платформы остаётся Knowledge Graph (ARC-004). Data Fabric не подменяет и не дублирует семантический слой.

### 5.2 No Publication Without Metadata
Ни один поток данных, проходящий через Enterprise Data Fabric (CDC, ETL, ELT, Streaming), не может быть опубликован или передан в Knowledge Graph без сопровождающих обязательных метаданных, определённых ARC-017 §7. Это ограничение является обязательным и не может быть обойдено техническими средствами интеграции.

### 5.3 Contract-Driven Onboarding
Подключение нового источника данных (страны, ведомства, статистической программы) осуществляется исключительно через формализованный Data Contract, соответствующий единому шаблону.

### 5.4 Infrastructure Agnostic Implementation
Компоненты Enterprise Data Fabric (CDC, ETL/ELT-движки, потоковая обработка) реализуются на основе открытых стандартов и переносимых технологий в соответствии с ARC-007 и ARC-014. Конкретные продукты рассматриваются как референсные реализации, а не как обязательные архитектурные компоненты.

## 6. КОНЦЕПТУАЛЬНАЯ МОДЕЛЬ

```
National Statistical System (страна СНГ)
        │
        ▼
   Data Contract
        │
        ▼
Enterprise Data Fabric
   (CDC / ETL / ELT / Streaming)
        │
        ▼
  Metadata Validation (ARC-017 Gate)
        │
        ▼
   Knowledge Graph
        │
        ▼
   API Platform
        │
        ▼
 Presentation Layer
```

## 7. ПОСЛЕДСТВИЯ ПРИНЯТИЯ РЕШЕНИЯ

Положительные:
- стандартизация подключения новых стран и источников через Data Contract;
- разделение ответственности между интеграционным и семантическим слоями;
- снижение стоимости и сроков подключения новых источников;
- централизованный Data Lineage и Data Catalog для всех потоков;
- предсказуемость архитектуры при масштабировании.

Потенциальные ограничения:
- необходимость разработки типового шаблона Data Contract до начала подключения новых источников;
- дополнительные затраты на проектирование Metadata Validation Gate между Data Fabric и Knowledge Graph;
- необходимость согласования границ ответственности между Data Fabric и будущим Metadata Service.

## 8. ВЛИЯНИЕ НА АРХИТЕКТУРУ

| Домен                     | Влияние      |
|---------------------------|--------------|
| Data Architecture          | Критическое  |
| Application Architecture   | Высокое      |
| Integration Architecture   | Критическое  |
| Knowledge Graph             | Среднее (уточнение границ) |
| Metadata Domain             | Высокое      |
| Security Architecture       | Среднее      |

## 9. РЕАЛИЗАЦИЯ

Для реализации решения предполагается создание следующих артефактов и подсистем:

- DM-* Data Contract Canonical Model;
- шаблон Data Contract (машиночитаемый, версионируемый);
- Metadata Registry для интеграционных метаданных;
- Data Catalog;
- Data Lineage Service;
- Metadata Validation Gate (реализация правила ARC-017 §7 на границе Data Fabric → Knowledge Graph).

## 10. СООТВЕТСТВИЕ СТАНДАРТАМ

- SDMX 3.x (обмен статистическими данными);
- DDI Lifecycle;
- DCAT (каталогизация наборов данных);
- PROV-O (происхождение данных);
- Open Standards First (ARC-011).

## 11. СВЯЗАННЫЕ АРХИТЕКТУРНЫЕ РЕШЕНИЯ

Настоящий ADR является основанием для последующих решений, включая:

- ADR-003 — API Platform Decomposition (Enterprise Data Fabric как источник данных для API-слоя);
- будущее решение о Metadata Service как едином владельце методологических метаданных.

## 12. ОТКРЫТЫЕ ВОПРОСЫ (для рассмотрения ARB)

- Разграничение ответственности между Metadata Registry внутри Data Fabric и предлагаемым отдельным Metadata Service.
- Выбор референсных технологий CDC/ETL/ELT/Streaming с обоснованием отсутствия vendor lock-in.
- Формат и версия шаблона Data Contract.
