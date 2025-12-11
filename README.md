# Desafio Feel - Emissão de Certificados via N8N (Jeferson Junior) 
# Automação de Disparo de WhatsApp via Planilha (n8n + Evolution API)

Este projeto consiste em um fluxo de automação desenvolvido no **n8n** que recebe uma planilha Excel, trata os dados dos contatos e realiza o envio de mensagens via WhatsApp utilizando a **Evolution API**.

O projeto conta também com uma interface de upload desenvolvida no **Lovable**.
Você pode acessar ela neste link: Entregáveis:

Lovable: https://certiprep-hub.lovable.app

## 🚀 Funcionalidades

- **Leitura de Arquivo Excel (.xlsx)**: Processamento automático de linhas e colunas.
- **Tratamento Inteligente de Telefones (JavaScript)**:
  - Higienização de caracteres não numéricos (traços, parênteses, espaços).
  - Normalização do DDI (55) para o Brasil.
  - Lógica de prevenção de duplicação de DDI (remove "55" se já existir na planilha antes de padronizar).
- **Integração via API**: Conexão com a Evolution API para envio das mensagens.
- **Tratamento de Erros**: Verificação de campos vazios antes do envio.

## 🛠️ Tecnologias Utilizadas

- **n8n**: Orquestração do fluxo de trabalho.
- **JavaScript**: Lógica de manipulação e limpeza de dados (Nodes *Code*).
- **Evolution API**: Gateway para envio de mensagens WhatsApp.
- **Lovable**: Interface frontend para upload do arquivo.

## 📋 Pré-requisitos

Para rodar este fluxo, você precisará de:

1. Uma instância do **n8n** (pode ser local, cloud ou auto-hospedada).
2. Uma instância da **Evolution API** configurada e com um WhatsApp conectado.

## ⚙️ Como Importar e Usar

1. Baixe o arquivo `workflow.json` deste repositório.
2. No seu n8n, vá em **Workflows** > **Import from File** e selecione o arquivo.
3. Ajuste as credenciais e configurações:
   - **Node HTTP Request (Evolution API)**: Atualize a URL da sua API e a `apikey` no Header.
   - **Node Webhook/Trigger**: Verifique o caminho (path) se necessário.

## 🧩 Lógica de Tratamento de Dados

O diferencial deste fluxo é o tratamento robusto do número de telefone utilizando JavaScript, garantindo que o envio não falhe independentemente de como o usuário digitou na planilha.

```javascript
// Exemplo da lógica utilizada no node "Code":
if (apenasNumeros.startsWith("55") && apenasNumeros.length > 11) {
    apenasNumeros = apenasNumeros.slice(2); // Remove duplicidade
}
let numeroFinal = "55" + apenasNumeros; // Garante padrão internacional
