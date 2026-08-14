# Especificação de Requisitos: Motor Farejando (01-scraper-engine)

## 📌 Visão Geral (`#especificacao-farejando`)

O módulo **Farejando** (`01-scraper-engine`) é a camada responsável pela raspagem de dados (*web scraping*) e coleta ativa de oportunidades de financiamento cultural em portais governamentais e plataformas parceiras.

---

## 🎯 Requisitos Funcionais

1. **Interface Comum de Scraper**:
   - Todo scraper de portal deve estender a interface base `BaseScraper`.
   - O método principal `scrape()` deve retornar uma promessa com uma lista de objetos `RawEdital`.

2. **Fontes de Coleta Iniciais**:
   - Mapa da Cultura (`mapa.cultura.gov.br`) - Piloto.
   - Caixa Cultural (`selecaocaixacultural.com.br`).
   - Ministério da Cultura (`gov.br/cultura`).
   - PROSAS (`prosas.com.br`).
   - Funjope - João Pessoa (`joaopessoa.pb.gov.br/secretarias/funjope`).
   - SECULT-PB (`paraiba.pb.gov.br/diretas/secretaria-da-cultura`).
   - JP Cultura (`jpcultura.joaopessoa.pb.gov.br`).
   - Prefeitura de João Pessoa — Notícias (`joaopessoa.pb.gov.br/categoria/noticias`).

3. **Filtragem por Palavras-Chave**:
   - Cada scraper deve buscar termos como `edital`, `chamada pública`, `oportunidade`, `patrocínio`, `seleção`, `incentivo`, `lei de fomento`.

---

## ⚡ Requisitos Não-Funcionais

- **Isolamento de Falhas**: A falha no scraper de uma fonte específica não pode interromper a execução dos demais scrapers.
- **Resiliência e Timeout**: Cada requisição deve possuir um timeout máximo ajustável (ex: 15 segundos) e estratégia de retentativas (retry logic).
- **Polidez de Coleta (Rate Limiting)**: Intervalo mínimo entre requisições ao mesmo servidor para evitar bloqueios de IP.

---

## 🚫 Fora de Escopo

- Validação de expiração ou deduplicação (responsabilidade do módulo *Rabo Levantado*).
- Persistência direta em banco de dados (responsabilidade do módulo *Pote de Ração*).
- Disparo de notificações (responsabilidade do módulo *Latidos*).
