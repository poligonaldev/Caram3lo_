# Plano de Desenvolvimento Granular (devplan.md) — Caram3l0

Este documento reúne a quebra de tarefas atômicas para a execução do **Caram3l0**, compilado a partir dos diagnósticos de repositório ([`LLMReports.md`](.agents/LLMReports.md)) e do documento de decisões de design ([`DDD.md`](docs/DDD.md)).

---

## 🚀 Visao Geral das Fases de Execucao

```text
Fase 0: Higienização & Governança (Sanitization)
  └── Fase 1: Especificação de Módulos (/specs - Spec-Driven)
        └── Fase 2: Scaffolding TypeScript & Infra FOSS
              └── Fase 3: Piloto Farejando (Scraper Mapa da Cultura)
                    └── Fase 4: Validação & Pote de Ração (SQLite)
                          └── Fase 5: Latidos (Discord, WhatsApp & Website)
                                └── Fase 6: Orquestração Horária & Observabilidade
```

---

## 📑 Mapeamento Granular de Tarefas Atomicas

### 🧹 Fase 0: Higienizacao do Repositorio e Governanca (`#fase-0`)
- [ ] **Task 0.1**: Criar `package.json` válido em formato JSON (Node 20+, `"type": "module"`, scripts de `dev`, `build`, `test`, `lint`).
- [ ] **Task 0.2**: Substituir `pote/mapacultura.json` por um fixture/schema JSON válido representando um edital capturado.
- [ ] **Task 0.3**: Atualizar o arquivo `LICENSE` para Creative Commons CC BY-NC-SA 4.0.
- [ ] **Task 0.4**: Criar arquivo `.env.example` contendo as variáveis `NODE_ENV`, `DATABASE_URL`, `DISCORD_WEBHOOK_URL`, `WHATSAPP_API_URL`.
- [ ] **Task 0.5**: Corrigir links de imagem e comando de `git clone` no `README.md`, adicionando o guia passo a passo de instalação.
- [ ] **Task 0.6**: Criar arquivo `CONTRIBUTING.md` padronizando guias de contribuição open-source.

---

### 📝 Fase 1: Especificacao Spec-Driven dos Modulos (`#fase-1`)
- [x] **Task 1.1**: Criar pasta [`/specs/01-scraper-engine/`](specs/01-scraper-engine/spec.md) contendo `spec.md`, `design.md` e `tasks.md` (Motor *Farejando*).
- [x] **Task 1.2**: Criar pasta [`/specs/02-validation-engine/`](specs/02-validation-engine/spec.md) contendo `spec.md`, `design.md` e `tasks.md` (Motor *Rabo Levantado*).
- [x] **Task 1.3**: Criar pasta [`/specs/03-storage/`](specs/03-storage/spec.md) contendo `spec.md`, `design.md` e `tasks.md` (Motor *Pote de Ração*).
- [x] **Task 1.4**: Criar pasta [`/specs/04-notifications/`](specs/04-notifications/spec.md) contendo `spec.md`, `design.md` e `tasks.md` (Motor *Latidos*).
- [x] **Task 1.5**: Criar pasta [`/specs/05-scheduler/`](specs/05-scheduler/spec.md) contendo `spec.md`, `design.md` e `tasks.md` (Motor de Orquestração).

---

### 🏗️ Fase 2: Scaffolding TypeScript e Tooling (`#fase-2`)
- [ ] **Task 2.1**: Inicializar projeto TypeScript (`npm install typescript @types/node tsx -D`) e gerar `tsconfig.json` otimizado para ESNext.
- [ ] **Task 2.2**: Criar estrutura de diretórios do código-fonte em `src/`:
  - `src/scrapers/`
  - `src/validation/`
  - `src/storage/`
  - `src/notifiers/`
  - `src/scheduler/`
  - `src/types/`
  - `src/utils/`
- [ ] **Task 2.3**: Configurar framework de testes **Vitest** (`vitest.config.ts`).
- [ ] **Task 2.4**: Configurar linter e formatador de código (ESLint + Prettier).
- [ ] **Task 2.5**: Criar workflow básico de CI no GitHub Actions (`.github/workflows/ci.yml`) para validar lint e testes a cada PR.

---

### 🐕 Fase 3: Piloto do Motor Farejando (`#fase-3`)
- [ ] **Task 3.1**: Definir interface TypeScript unificada `BaseScraper` e tipo de dado `RawEdital`.
- [ ] **Task 3.2**: Implementar o scraper piloto para o portal **Mapa da Cultura** (`src/scrapers/mapa-cultura.scraper.ts`).
- [ ] **Task 3.3**: Escrever suíte de testes com mocks HTTP para validar a resiliência da extração do Mapa da Cultura.

---

### 🥣 Fase 4: Validacao e Pote de Racao em SQLite (`#fase-4`)
- [ ] **Task 4.1**: Configurar ORM/Query Builder leve (Prisma ou Kysely) com driver SQLite FOSS.
- [ ] **Task 4.2**: Criar migração do esquema do banco de dados (tabelas `editais`, `fontes_monitoradas`, `historico_notificacoes`).
- [ ] **Task 4.3**: Implementar motor de validação *Rabo Levantado* (algoritmo de deduplicação por hash/URL, detecção de prorrogação e validação de documentos PDF/DOCX).
- [ ] **Task 4.4**: Conectar o validador à persistência do SQLite.

---

### 📢 Fase 5: Motor Latidos — Notificacoes Gratuitas (`#fase-5`)
- [ ] **Task 5.1**: Criar formatador universal de alertas com suporte a Rich Embeds (título, órgão, prazo, valor, links e badges de urgência).
- [ ] **Task 5.2**: Implementar cliente de envio via **Discord Webhook** (`src/notifiers/discord.notifier.ts`).
- [ ] **Task 5.3**: Implementar cliente de envio via gateway FOSS de **WhatsApp** (`src/notifiers/whatsapp.notifier.ts`).
- [ ] **Task 5.4**: Implementar gerador de feed/API para o **Website do App** (`src/notifiers/web-feed.notifier.ts`).
- [ ] **Task 5.5**: Criar script de teste integrado de notificações apontando para canal sandbox no Discord.

---

### ⏰ Fase 6: Orquestracao Horaria e Observabilidade (`#fase-6`)
- [ ] **Task 6.1**: Implementar o orquestrador de ciclo horário (`src/scheduler/cron.ts`).
- [ ] **Task 6.2**: Desenvolver scrapers adicionais por site (Caixa Cultural, MinC, PROSAS, Funjope, SECULT-PB, JP Cultura, Prefeitura JP).
- [ ] **Task 6.3**: Criar sistema de logs estruturados e alertas em caso de alteração de layout HTML dos sites monitorados.
