# Documento de Decisões de Design (DDD) — Caram3l0

Este documento registra cronologicamente as decisões arquiteturais, técnicas e de produto adotadas no projeto **Caram3l0**.

---

## 📌 Registro de Decisões de Arquitetura (ADR)

### ADR-001: Adocao do TypeScript como Linguagem Padrao

* **Data**: 13/08/2026
* **Status**: Aprovado
* **Contexto**: O projeto necessita manipular contratos de dados complexos vindos de múltiplos sites de editais (datas, prazos, orçamentos, perfis de proponente PF/PJ, anexos PDF).
* **Decisão**: Adotar **TypeScript** em ambiente Node.js (ES Modules).
* **Consequências**:
  * Maior segurança de tipos na passagem de dados entre os módulos *Farejando*, *Rabo Levantado*, *Pote de Ração* e *Latidos*.
  * Prevenção de erros de runtime ao raspar metadados incompletos ou malformados dos sites.

---

### ADR-002: Banco de Dados 100% FOSS e Gratuito (SQLite / Embedded FOSS)

* **Data**: 13/08/2026
* **Status**: Aprovado
* **Contexto**: O projeto é de utilidade pública open-source e não possui financiamento ou orçamento para infraestrutura paga de banco de dados cloud.
* **Decisão**: Adotar **SQLite** como banco de dados relacional FOSS principal, utilizando ORM/Query Builder leve (como Prisma ou Kysely).
* **Consequências**:
  * Custo de hospedagem e armazenamento: **R$ 0,00** (arquivo `.sqlite` local ou persistido em container).
  * Portabilidade total: o banco roda em qualquer máquina, servidor gratuito ou container sem necessidade de provisionamento de clusters.
  * Facilidade de backup (versão compactada do arquivo `.sqlite` ou export em JSON).

---

### ADR-003: Estrategia de Canais de Notificacao Gratuitos (Latidos)

* **Data**: 13/08/2026
* **Status**: Aprovado
* **Contexto**: O envio de alertas precisa atingir artistas e produtores sem gerar custos operacionais por mensagem (evitando APIs pagas de SMS ou WhatsApp oficial de negócios).
* **Decisão**:
  1. **Discord Webhook** (Canal Principal de Entrada): 100% gratuito, suporte nativo a Rich Embeds (cards formatados com cores, links de editais e botões), rápido de implementar e testar.
  2. **WhatsApp via Gateway FOSS**: Integração com bibliotecas FOSS (Baileys / Evolution API self-hosted) via QR Code, sem custos de API oficial por mensagem.
  3. **Website do App / Feed API**: Geração de feeds estáticos ou API leve hospedável em plataformas gratuitas (Vercel, Render, Cloudflare Pages).
* **Consequências**:
  * Garantia de notificação sem custos recorrentes nos 3 canais oficiais do projeto (Discord, WhatsApp e Website).
  * Facilidade de teste rápido utilizando o Discord como *sandbox* de validação visual.

---

### ADR-004: Modelo de Licenciamento Creative Commons (CC BY-NC-SA 4.0)

* **Data**: 13/08/2026
* **Status**: Aprovado
* **Contexto**: O software deve ter o uso incentivado para a comunidade artística, porém protegendo o projeto contra exploração comercial de terceiros sem repasse e exigindo a atribuição ao **Poligonal Hub**.
* **Decisão**: Adotar a licença **Creative Commons Atribuição-NãoComercial-CompartilhaIgual 4.0 Internacional (CC BY-NC-SA 4.0)**.
* **Consequências**:
  * Obriga a atribuição de autoria ao Poligonal Hub (`[Caram3l0](#caram3lo)`).
  * Proíbe o uso comercial direto ou venda de acesso ao software por terceiros.
  * Exige que melhorias e edições derivadas sejam compartilhadas sob a mesma licença, fortalecendo o ecossistema do app principal.

---

### ADR-005: Arquitetura Modular Baseada na Metafora Canina e Spec-Driven Development

* **Data**: 13/08/2026
* **Status**: Aprovado
* **Contexto**: Facilidade de comunicação com desenvolvedores e não-desenvolvedores mantendo isolamento de escopo por funcionalidade.
* **Decisão**: Dividir a base de código em 5 módulos independentes sob a pasta `/specs` e `src/`:
  * `01-scraper-engine` (**Farejando**): Motores de coleta por portal cultural.
  * `02-validation-engine` (**Rabo Levantado**): Validação de prazos, vigência, links e duplicidade.
  * `03-storage` (**Pote de Ração**): Modelo de dados e persistência relacional SQLite.
  * `04-notifications` (**Latidos**): Formatadores e provedores de envio (Discord, WhatsApp, Website).
  * `05-scheduler` (**Orquestração**): Cron jobs horários e saúde do sistema.
* **Consequências**:
  * Nenhuma funcionalidade é implementada sem especificação prévia (`spec.md`, `design.md`, `tasks.md`).
  * Falha em um scraper isolado não compromete os demais scrapers nem os motores de notificação e persistência.
