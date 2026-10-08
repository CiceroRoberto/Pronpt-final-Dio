# 🚀 Automação de Captura e Processamento de Leads (n8n)

Solução automatizada para qualificação e registro de leads em tempo real desenvolvida para a **Equipe Comercial**. O fluxo realiza a recepção de entradas vindas de formulários web (Google Forms), executa a validação de formato e integridade dos dados, persiste o registro na base comercial do Google Sheets e realiza o disparo automatizado de e-mails via Gmail.

---

## 📌 Visão Geral do Fluxo no n8n

Below is the layout of the workflow inside the n8n editor:

```
┌────────────────────┐     ┌──────────────────────┐     ┌────────────────────────┐     ┌─────────────────────────┐
│ Google Sheets      │ ──► │ Filter               │ ──► │ Google Sheets          │ ──► │ Gmail                   │
│ Gatilho: Novo Form │     │ Validar E-mail Válido│     │ Salvar Planilha Oficial│     │ Notificar Lead e Vendas │
└────────────────────┘     └──────────────────────┘     └────────────────────────┘     └─────────────────────────┘
                                      │ (Invalid)
                                      └─► [ Ignorado ]
```

> **Dica para o repositório:** Adicione uma captura de tela do seu canvas no n8n salvando a imagem em `assets/preview.png` e desmarcando a linha abaixo no seu README:
> `![Preview do Workflow no n8n](assets/preview.png)`

---

## 🛠️ Tecnologias Utilizadas

* **[n8n](https://n8n.io/)** — Orquestrador de automações no-code/low-code.
* **Google Forms / Google Sheets** — Captura inicial e persistência dos leads.
* **Gmail API** — Envio automatizado de confirmações e alertas.

---

## 📋 Regras de Negócio Implementadas

1. **Monitoramento Ativo:** O gatilho escuta novas entradas na planilha de respostas a cada minuto.
2. **Validação de Sintaxe (Regex):** O filtro valida a existência e a formatação do endereço de e-mail (`^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$`). Registros inválidos ou sem e-mail são silenciosamente descartados.
3. **Enriquecimento do Registro:** Adiciona carimbo de data/hora (`DataCadastro`) no formato ISO e atribui o status padrão de `"Novo Lead"`.
4. **Comunicação Comercial:** Dispara mensagem de confirmação para o lead com cópia (`CC`) automatizada para a caixa da equipe comercial (`comercial@suaempresa.com`).

---

## ⚙️ Instruções de Configuração e Instalação

### 1. Importando o Workflow para o n8n
1. Faça o download ou copie o conteúdo do arquivo [`workflow.json`](./workflow.json).
2. Abra o seu painel do **n8n**.
3. Crie um novo fluxo (*Create Workflow*).
4. Clique no canvas do n8n e pressione `Ctrl + V` (Windows) ou `Cmd + V` (Mac), ou acesse o menu superior direito `...` > **Import from File / Clipboard**.

### 2. Configurando Credenciais
* **Google Sheets OAuth2:** Adicione ou atribua sua conta Google autorizada para leitura/escrita nas planilhas.
* **Gmail OAuth2:** Configure a autenticação para permitir o envio automatizado de e-mails.

### 3. Ajuste dos IDs das Planilhas
1. Abra o nó `Gatilho: Novo Form` e selecione a planilha vinculada ao Google Forms.
2. Abra o nó `Salvar na Planilha Oficial` e defina o ID da planilha final da equipe comercial.

---

## 📄 Licença

Este projeto está sob a licença [MIT](LICENSE).