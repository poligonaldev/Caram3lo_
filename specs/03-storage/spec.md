# Especificação de Requisitos: Motor Pote de Ração (03-storage)

## 📌 Visão Geral (`#especificacao-pote-de-racao`)

O módulo **Pote de Ração** (`03-storage`) é a camada de banco de dados e persistência de dados relacional. Ele é responsável por armazenar com segurança todos os editais validados, registrar histórico de atualizações e prorrogações, manter fixtures de exemplo e gerenciar o status dos portais monitorados.

---

## 🎯 Requisitos Funcionais

1. **Persistência Relacional FOSS (SQLite)**:
   - Salvar dados de editais validados sem dependência de serviços externos pagos.
   - Armazenar arquivo local de banco `.sqlite` (em produção ou container).

2. **Gerenciamento de Histórico de Versões**:
   - Manter histórico de alterações de prazos e termos quando um edital for prorrogado.

3. **Gerenciamento de Fontes Monitoradas**:
   - Manter registros da tabela de fontes (`sources`), incluindo URL base, nome do portal e status de funcionamento (ativo, inativo, com erro).

4. **Registro de Notificações Enviadas**:
   - Registrar no banco se determinado edital já foi notificado no Discord, WhatsApp ou Website para evitar disparos duplicados em spam.

---

## ⚡ Requisitos Não-Funcionais

- **Migrations e Schema Versionado**: Migrações de banco rastreáveis via ORM (Prisma / Kysely).
- **Export / Dump em Fixtures JSON**: Capacidade de exportar a base para arquivos JSON de backup em `pote/` (substituindo o antigo arquivo estático).
