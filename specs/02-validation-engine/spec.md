# Especificação de Requisitos: Motor Rabo Levantado (02-validation-engine)

## 📌 Visão Geral (`#especificacao-rabo-levantado`)

O módulo **Rabo Levantado** (`02-validation-engine`) é a camada inteligente de validação, filtragem e saneamento de dados. Ele analisa os editas brutos fornecidos pelo módulo *Farejando* e determina a vigência, originalidade, integridade de links e extração de metadados antes que a oportunidade seja armazenada e notificada.

---

## 🎯 Requisitos Funcionais

1. **Deduplicação & Alteração de Prazos**:
   - Verificar se o edital já foi processado anteriormente comparando Hash de Conteúdo e URL de Origem.
   - Se o edital já existir, verificar se a data final de inscrição ou retificação mudou. Se sim, marcar como **Prorrogado / Atualizado**.

2. **Validação de Vigência & Prazos**:
   - Parsear strings de data para objetos de data UTC.
   - Descartar editais cujos prazos de inscrição já tenham sido encerrados no momento da coleta.

3. **Verificação de Documentos & Links Oficiais**:
   - Validar se há links diretos para arquivos de edital e anexos oficiais (`.pdf`, `.docx`, `.doc`, `.csv`, `.zip`).
   - Testar o código de resposta HTTP (ex: HTTP 200 OK) dos links antes de aprová-los.

4. **Extração de Metadados Estruturados**:
   - Identificar perfil de elegibilidade: Pessoa Física (PF), Pessoa Jurídica (PJ) ou Ambos.
   - Extrair valores de financiamento (orçamento total do edital e valor teto por projeto).
   - Classificar escopo geográfico (João Pessoa, Paraíba, Nordeste, Nacional).

---

## ⚡ Requisitos Não-Funcionais

- **Precisão na Extração de Datas**: Suporte a múltiplos formatos de data em português (ex: "31/12/2026", "31 de Dezembro de 2026", "Até 18h de 15/09/26").
- **Validação com Zod**: Uso de schemas estritos de validação de runtime com biblioteca Zod.
