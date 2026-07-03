# MM-010 — Knowledge Object Taxonomy

## НАЗНАЧЕНИЕ

Документ определяет официальную таксономию всех типов Knowledge Objects, существующих в платформе CIS Data Commons.
Он не описывает атрибуты объектов (это задача MM-000), а отвечает на вопрос:
Какие виды объектов существуют в платформе и как они классифицируются?

Структура документа
1. Purpose
2. Scope
3. Normative References
4. Taxonomy Principles
5. Root Taxonomy
6. Domain Taxonomies
7. Object Classification Rules
8. Extension Rules
9. Compliance Requirements
10. Future Evolution

## ПРИНЦИПЫ ТАКСОНОМИИ

Зафиксировано несколько обязательных правил:

- каждый объект принадлежит хотя бы одному типу;
- каждый тип наследуется от Knowledge Object;
- множественное наследование запрещено (если не будет специально обосновано);
- объект может принадлежать нескольким доменам (MM-011);
- тип определяется семантикой, а не способом хранения.

## КОРНЕВАЯ ТАКСОНОМИЯ

Knowledge Object
│
├── Governance Object
├── Architecture Object
├── Requirement Object
├── Standard Object
├── Decision Object
├── Information Object
├── Metadata Object
├── Semantic Object
├── API Object
├── Service Object
├── Portal Object
├── Analytics Object
├── AI Object
├── Infrastructure Object
└── Operational Object

## Information Object

Information Object
│
├── Dataset
├── Statistical Indicator
├── Time Series
├── Observation
├── Cube
├── Table
├── Dimension
├── Measure
├── Attribute
├── Classification
├── Code List
├── Geography
├── Organization
├── Publication
└── Statistical Programme

## Metadata Object

Metadata Object
│
├── DDI Study
├── DDI Variable
├── SDMX Data Structure
├── SDMX Dataflow
├── SDMX Codelist
├── SIMS Metadata
├── ESMS Metadata
├── ESQRS Metadata
├── Schema.org Entity
├── DCAT Dataset
├── RDF Resource
└── JSON-LD Context

## Architecture Object

Architecture Object
│
├── Architecture Principle
├── Architecture Decision
├── Reference Architecture
├── Architecture View
├── Architecture Building Block
├── Architecture Pattern
└── Architecture Constraint

## AI Object

AI Object
│
├── AI Agent
├── MCP Server
├── Prompt
├── Embedding Collection
├── Vector Index
├── RAG Collection
├── AI Tool
└── AI Workflow

## 5Extension Rules

Важное определение:
новые типы объектов создаются только через Architecture Decision Record (ADR).

Это защитит платформу от хаотичного роста.