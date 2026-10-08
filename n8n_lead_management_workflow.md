{
  "name": "Automação de Captura e Qualificação de Leads - Comercial",
  "nodes": [
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMinute"
            }
          ]
        },
        "documentId": {
          "__rl": true,
          "value": "SEU_SPREADSHEET_ID_AQUI",
          "mode": "list",
          "cachedResultName": "Respostas do Formulário"
        },
        "sheetName": {
          "__rl": true,
          "value": "Respostas ao formulário 1",
          "mode": "list",
          "cachedResultName": "Respostas ao formulário 1"
        },
        "event": "rowAdded"
      },
      "id": "11a1a2b3-c4d5-4e6f-8a9b-0c1d2e3f4a5b",
      "name": "Gatilho: Novo Form",
      "type": "n8n-nodes-base.googleSheetsTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "credentials": {
        "googleSheetsTriggerOAuth2Api": {
          "id": "CREDENTIAL_ID_SHEETS",
          "name": "Conta Google Sheets"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": ""
          },
          "conditions": [
            {
              "id": "e9876543-21ba-4321-9876-543210fedcba",
              "leftValue": "={{ $json[\"E-mail\"] }}",
              "rightValue": "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$",
              "operator": {
                "type": "string",
                "operation": "regex"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "id": "22b2b3c4-d5e6-4f7a-8b9c-0d1e2f3a4b5c",
      "name": "Validar E-mail Válido",
      "type": "n8n-nodes-base.filter",
      "typeVersion": 2,
      "position": [
        470,
        300
      ]
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "SEU_SPREADSHEET_ID_OFICIAL",
          "mode": "list",
          "cachedResultName": "Base Oficial de Leads"
        },
        "sheetName": {
          "__rl": true,
          "value": "Leads Comercial",
          "mode": "list",
          "cachedResultName": "Leads Comercial"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Nome": "={{ $json[\"Nome Completo\"] }}",
            "Email": "={{ $json[\"E-mail\"] }}",
            "Telefone": "={{ $json[\"Telefone / WhatsApp\"] }}",
            "Empresa": "={{ $json[\"Nome da Empresa\"] }}",
            "DataCadastro": "={{ $now.format('yyyy-MM-dd HH:mm:ss') }}",
            "Status": "Novo Lead"
          },
          "matchingColumns": [],
          "schema": []
        },
        "options": {}
      },
      "id": "33c3c4d5-e6f7-4a8b-9c0d-1e2f3a4b5c6d",
      "name": "Salvar na Planilha Oficial",
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.5,
      "position": [
        690,
        300
      ],
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "CREDENTIAL_ID_SHEETS",
          "name": "Conta Google Sheets"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "={{ $json[\"Email\"] }}",
        "subject": "Recebemos seu contato! - Equipe Comercial",
        "emailType": "text",
        "message": "=Olá, {{ $json[\"Nome\"] }}!\n\nAgradecemos o seu interesse. Recebemos seus dados com sucesso e nossa equipe comercial entrará em contato em breve.\n\nAtenciosamente,\nEquipe Comercial",
        "options": {
          "ccAddresses": "comercial@suaempresa.com"
        }
      },
      "id": "44d4d5e6-f7a8-4b9c-0d1e-2f3a4b5c6d7e",
      "name": "Notificar Lead e Vendas",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        910,
        300
      ],
      "credentials": {
        "gmailOAuth2": {
          "id": "CREDENTIAL_ID_GMAIL",
          "name": "Conta Gmail"
        }
      }
    }
  ],
  "connections": {
    "Gatilho: Novo Form": {
      "main": [
        [
          {
            "node": "Validar E-mail Válido",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Validar E-mail Válido": {
      "main": [
        [
          {
            "node": "Salvar na Planilha Oficial",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Salvar na Planilha Oficial": {
      "main": [
        [
          {
            "node": "Notificar Lead e Vendas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "settings": {
    "executionOrder": "v1"
  }
}