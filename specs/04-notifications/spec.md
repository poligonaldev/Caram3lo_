# Especificação de Requisitos: Motor Latidos (04-notifications)

## 📌 Visão Geral (`#especificacao-latidos`)

O módulo **Latidos** (`04-notifications`) é a camada de comunicação e notificação automatizada do Caram3l0. Ele recebe os editais validados e prorrogados do banco de dados e os formata em mensagens otimizadas para transmissão nos 3 canais oficiais do projeto: **Discord**, **WhatsApp** e **Website do App**.

---

## 🎯 Requisitos Funcionais

1. **Formatador Único de Alertas**:
   - Gerar mensagens com emojis e estrutura clara: órgão responsável, título do edital, prazo final, perfil elegível (PF/PJ), valores e link direto para os documentos.
   - Adicionar badges visuais de urgência:
     - 🚨 `NOVO EDITAL DETECTADO`
     - ⏳ `ÚLTIMOS DIAS DE INSCRIÇÃO`
     - 🔄 `EDITAL PRORROGADO`

2. **Notificador Discord Webhook (Canal Principal de Entrada)**:
   - Formatar o alerta utilizando **Discord Rich Embeds** com cores dinâmicas (ex: verde para novo edital, amarelo para retificação, vermelho para prazo crítico).

3. **Notificador WhatsApp (Gateway FOSS)**:
   - Suporte a envio de mensagens formatadas em Markdown WhatsApp (`*negrito*`, `_itálico_`) via integração com gateway FOSS (Evolution API ou Baileys self-hosted).

4. **Gerador de Feed / API para o Website do App**:
   - Disponibilizar arquivo de feed JSON estático ou endpoint de API com as últimas oportunidades ativas para consumo pelo website.

---

## ⚡ Requisitos Não-Funcionais

- **Idempotência**: Garantir que o mesmo alerta não seja enviado mais de uma vez para o mesmo canal.
- **Formatação Resiliente**: Se o limite de caracteres do Discord ou WhatsApp for atingido, truncar a descrição de forma legível sem quebrar o link principal.
