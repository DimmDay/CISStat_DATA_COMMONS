# CIS Data Commons
## Architecture Session Summary
Version: 0.1
Status: Active
Last Updated: 2026-07-06

---

# 1. Цель проекта

Создается цифровая платформа нового поколения **CIS Data Commons** для Межгосударственного статистического комитета СНГ.
Проект НЕ является реконструкцией существующего сайта.
Целевая архитектура представляет собой Knowledge-Centric Platform — единую цифровую экосистему официальной статистики, объединяющую:

- официальные статистические данные;
- метаданные;
- статистическую методологию;
- документы;
- API;
- Knowledge Graph;
- онтологии;
- AI-сервисы;
- платформу анализа временных рядов;
- международные стандарты обмена статистическими данными.

Главная цель проекта — создать "единую точку входа в мировую статистику".

---

# 2. Архитектурная концепция

Принято решение отказаться от использования Liferay в качестве целевой платформы пользовательского интерфейса.
Liferay сохраняется исключительно как источник миграции контента.
Целевой портал строится как:

Headless Platform

Frontend:
Next.js (SSR/ISR)

Backend:
- API First
- Knowledge Graph
- Metadata First
- Semantic Architecture
- Cloud Neutral
- AI Native
- Infrastructure Agnostic

---

# 3. Принятые архитектурные решения

Утверждены следующие ключевые положения.

## Repository First

Весь проект разрабатывается как единый GitHub Repository.
Repository является "Single Source of Truth".

---

## Everything is a Knowledge Object (EKO)

Любая сущность платформы рассматривается как Knowledge Object.

Это относится к:

- документам;
- требованиям;
- архитектуре;
- API;
- статистическим показателям;
- наборам данных;
- временным рядам;
- онтологиям;
- классификаторам;
- AI Agent;
- сервисам;
- Dashboard;
- Portal Components.

---

## Knowledge Object Contract

Каждый объект обязан иметь:

- Identity
- Metadata
- Lifecycle
- Version
- Relations
- Manifest
- URI
- Semantic Description
- Governance
- Provenance

---

## Semantics Before Implementation

Сначала определяется семантика.
Потом модель данных.
Потом API.
Потом код.

---

## Metadata Before Data

Любые статистические данные публикуются только при наличии обязательных машиночитаемых метаданных.

---

## Cloud Neutral Architecture

Платформа должна одинаково работать:

- локально;
- в частном облаке;
- в Kubernetes;
- в государственных ЦОД;
- без зависимости от конкретного облачного провайдера.

---

# 4. Структура репозитория

Сформирована базовая структура проекта.

Созданы разделы:

Governance/
Architecture/
Metamodel/
Development/
Infrastructure/
OpenAPI/
Ontology/
Knowledge Graph/
Data Models/
Documentation/
Portal/
TS Analysis/
Design/

---

# 5. Разработанные документы

## Governance

README
Repository Governance
ADR Framework
GitHub Governance

---

## Standards

STD-001
Document Manifest Standard

STD-002
Knowledge Object Identity Standard

---

## Architecture

ARC-000
Architecture Principles

ARC-001
Architecture Vision
(создан)

ARC-002
Reference Architecture
(создан)

ARC-003
Architecture Repository Model
(создан)

ARC-004
Architecture Compliance
(создан)

ARC-016
Semantics Before Implementation

ARC-017
Metadata Before Data

---

## Metamodel

MM-000
Knowledge Object Metamodel

MM-010
Knowledge Object Taxonomy

MM-011
Knowledge Domains

---

# 6. GitHub

Настроены:

Git
GitHub Repository
GitHub Actions
CODEOWNERS
Branch Strategy
Repository Governance

Первый push выполнен успешно.

---

# 7. Основные архитектурные принципы

Проект строится как:

- Knowledge-Centric Platform
- Repository as Single Source of Truth
- Metadata First
- Semantics First
- API First
- AI Native
- Infrastructure Agnostic
- Open Standards
- Interoperability by Design
- Knowledge Graph Native

---

# 8. Международные стандарты

В архитектуру закладывается поддержка:

- SDMX
- DDI Lifecycle
- SIMS
- ESMS
- ESQRS
- Schema.org
- DCAT
- RDF
- OWL
- SKOS
- PROV-O
- XBRL
- OpenAPI
- JSON-LD

---

# 9. Интегрируемые подсистемы

- ПАК ИАП
- ЕИАС
- СУЗ-СОД
- TS Analysis
- Будущий MCP Server
- Knowledge Graph
- AI Agents

---

# 10. Следующий этап разработки

Далее начинается проектирование семантического уровня платформы.

План работ:

1.ARC-018
Interoperability by Design

2.Ontology/
README

3.ONT-000
Upper Ontology

4.ONT-001
Core Concepts

5.DM-000
Canonical Knowledge Model

6.KG-000
Knowledge Graph Architecture

7.API-000
Canonical Resource Model

8.REQ-000
Requirements Taxonomy

После этого начинается проектирование Portal и сервисной архитектуры.

---

# 11. Долгосрочная цель

Создать платформу мирового уровня, объединяющую официальную статистику, метаданные, документы, методологию и интеллектуальные сервисы в едином Knowledge Space.

Архитектура должна быть рассчитана минимум на 15–20 лет развития без смены концепции.
