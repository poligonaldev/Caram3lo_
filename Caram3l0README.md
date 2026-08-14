# 🐶 Caram3l0 app

> O melhor amigo do artista que sofre com editais culturais.

![Caram3l0V1](/assets/caramelov1.png)

[![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)
[![JavaScript](https://img.shields.io/badge/javascript-%23F7DF1E.svg?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript)
[![Puppeteer](https://img.shields.io/badge/puppeteer-%2340B5A4.svg?style=for-the-badge&logo=puppeteer&logoColor=white)](https://pptr.dev/)

---

## 📌 O que é o Caram3l0?

**Caram3l0** é a força-tarefa de editais da ![Poligonal Hub](https://poligonalhub/) Pra facilitar a vida dessa força-tarefa o Caram3l0 app foi idealizado. Notando que essa ideia teria impacto de utilidade pública, o projeto Caram3l0 ingressou no movimento de código aberto. O Caram3l0 app é um assistente automatizado (bot) desenvolvido para encontrar, validar e disparar atualizações sobre oportunidades de patrocínio e financiamento para projetos culturais. O foco principal da ferramenta está na região de **João Pessoa (Paraíba)** e no **Norte-Nordeste** do Brasil, ajudando artistas locais a não perderem prazos importantes.

### 🎯 Funções Principais
* **Busca Ativa:** Procura editais culturais em múltiplos portais nacionais e regionais.
* **Validação Inteligente:** Analisa se o edital possui documentos válidos, prazos visíveis e categorias especificadas.
* **Armazenamento Seguro:** Guarda as oportunidades validadas para evitar duplicidade.
* **Notificação Automatizada:** Envia alertas de novos editais e lembretes de prazos (ex: *"Última semana!"* ou *"Último dia!"*).

---

## ⚙️ Como Funciona? (Arquitetura do Projeto)

O ciclo de vida do Caram3l0 roda a cada hora e é dividido em 4 etapas principais:

### 1. 🐕 Farejando (Coleta de Dados)
A cada hora, o sistema realiza uma raspagem de dados (*web scraping*) nos sites cadastrados no seu **Pote de Ração**. Ele busca termos relevantes como:
* `edital`, `chamada pública`, `oportunidade`, `patrocínio`, `seleção`, `incentivo`, `lei de fomento`.

### 2. 🐕 Rabo Levantado (Validação)
Ao encontrar uma possível oportunidade, o Caram3l0 aplica filtros rigorosos:
* **Verificação de Duplicidade:** Se o edital já existe no banco, ele checa se a data mudou (prorrogamento). Se sim, dispara um alerta de prorrogação; se não, entra em modo de espera (*Rabo Abaixado* com `async/await`).
* **Checagem de Links:** Verifica se há links diretos para arquivos oficiais (`.pdf`, `.docx`, `.doc`, `.csv`, `.pptx`, `.ppt`, `.txt`, `.html`).
* **Análise de Vigência:** Valida se o prazo de inscrição não expirou.
* **Extração de Metadados:** Confirma se o cronograma, valores (orçamento total/por projeto) e categorias participantes estão explícitos.

### 3. 🥣 Pote de Ração (Banco de Dados e Scrapers)
Os editais aprovados recebem uma **ID única** e são salvos no banco de dados com suas informações estruturadas (prazos, público-alvo, perfil PF/PJ, etc.).
> 💡 **Nota de Design:** Para garantir a escalabilidade, cada site possui um arquivo de raspagem (`.js`) independente baseado em um *template* comum. Se a estrutura de um site mudar, apenas o arquivo correspondente quebra, mantendo o restante do sistema operacional.

#### Fontes Iniciais Monitoradas:
* [Caixa Cultural - Seleção](https://www.selecaocaixacultural.com.br/)
* [Ministério da Cultura - Editais Abertos](https://www.gov.br/cultura/pt-br/assuntos/editais/inscricoes-abertas)
* [PROSAS - Plataforma de Editais](https://prosas.com.br/editais)
* [Mapa da Cultura – Oportunidades](https://mapa.cultura.gov.br/oportunidades/)
* [Funjope – Fundação Cultural de João Pessoa](https://www.joaopessoa.pb.gov.br/secretarias/funjope/)
* [SECULT-PB – Secretaria de Estado da Cultura da Paraíba](https://www.paraiba.pb.gov.br/diretas/secretaria-da-cultura)
* [JP Cultura – Oportunidades](https://jpcultura.joaopessoa.pb.gov.br/oportunidades/)
* [Prefeitura de João Pessoa – Notícias](https://www.joaopessoa.pb.gov.br/categoria/noticias/)

### 4. 📢 Latidos (Agitação e Propagação)
Após filtrar e armazenar, o Caram3l0 formata os dados e envia notificações integradas para:
* 👾 **Discord** (Canal oficial via Webhook)
* 💬 **WhatsApp**
* 🌐 **Website do App**

---

## 🛠️ Tecnologias Utilizadas (Tech Stack)

| Tecnologia | Descrição / Função no Projeto |
| :--- | :--- |
| **JavaScript (ES6+)** | Linguagem base do projeto. |
| **Node.js** | Ambiente de execução do código no servidor. |
| **Puppeteer** | Navegação automatizada para raspagem de SPA/sites complexos. |
| **Cheerio** | Extração e manipulação de HTML de forma rápida (parsing). |
| **node-fetch / get-urls** | Requisições HTTP e extração de links de textos. |
| **IndexedDB** | Armazenamento estruturado no lado do cliente. |
| **CORS** | Mecanismo de segurança para requisições entre origens. |

---

## 🚀 Como Executar o Projeto

### Pré-requisitos
* Ter o [Node.js](https://nodejs.org/) instalado na sua máquina.

### Instalação
1. Clone este repositório:
   ```bash
   git clone [https://github.com/seu-usuario/caram3l0.git](https://github.com/seu-usuario/caram3l0.git)
