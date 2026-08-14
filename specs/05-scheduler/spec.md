# Especificação de Requisitos: Motor de Orquestração (05-scheduler)

## 📌 Visão Geral (`#especificacao-orquestracao`)

O módulo **Orquestração** (`05-scheduler`) é o maestro central do Caram3l0. Ele executa o ciclo de vida periódico (a cada 1 hora) conectando ordenadamente os 4 módulos: **Farejando** ➔ **Rabo Levantado** ➔ **Pote de Ração** ➔ **Latidos**.

---

## 🎯 Requisitos Funcionais

1. **Ciclo Periodico Horário**:
   - Executar a varredura completa dos scrapers a cada hora (ex: cron `0 * * * *`).

2. **Fluxo Integrado de Execução**:
   - Etapa 1: Executar `ScraperRegistry.runAll()`.
   - Etapa 2: Passar os `RawEdital` obtidos para o `ValidationEngine`.
   - Etapa 3: Persistir os `ValidatedEdital` no `EditalRepository`.
   - Etapa 4: Disparar `NotificationDispatcher` apenas para oportunidades com status `NOVO` ou `PRORROGADO` que ainda não tenham sido notificadas no canal.

3. **Observabilidade & Relatório de Saúde**:
   - Registrar métricas da execução: quantidade de sites varridos, número de editais extraídos, validados e notificados, tempo total de execução.
   - Em caso de quebra isolada em um scraper (ex: mudança de layout HTML de um portal), emitir um log de alerta sem derrubar a aplicação.

---

## ⚡ Requisitos Não-Funcionais

- **Execução CLI ou Background Daemon**: Possibilidade de rodar como tarefa agendada (node-cron, GitHub Actions ou container).
- **Graceful Shutdown**: Permitir encerramento seguro de tarefas sem corromper o banco de dados SQLite.
