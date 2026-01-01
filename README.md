# 🤖 Chatbot Web com IA, Banco de Dados e RAG

## 📌 Descrição do Projeto

Este projeto implementa um **chatbot web inteligente para uma loja virtual**, integrado a **automação de processos**, **banco de dados** e **RAG (Retrieval-Augmented Generation)**.

O objetivo é **automatizar o atendimento ao cliente**, permitindo:
- Respostas inteligentes sobre produtos
- Consultas seguras de pedidos via CPF
- Atualização dinâmica de conhecimento sem alterar o fluxo principal

---

## 🏗️ Arquitetura Geral

A arquitetura do projeto é composta por **dois pipelines principais**, desenvolvidos no **n8n**:

1. **Pipeline de Ingestão de Conhecimento (Google Drive → RAG)**
2. **Pipeline de Atendimento ao Cliente (Chatbot Web)**

Cada fluxo possui responsabilidades bem definidas, garantindo **segurança, escalabilidade e manutenibilidade**.

---

## 🔴 Pipeline 1 – Ingestão de Contexto e Produtos (RAG)

### 🎯 Objetivo

Transformar documentos em uma **base de conhecimento vetorial**, utilizada pelo chatbot para responder dúvidas gerais.

### 🔄 Funcionamento

1. O fluxo inicia com um **Google Drive Trigger**, acionado quando um novo arquivo é criado.
2. O arquivo é baixado automaticamente via **Download File**.
3. O conteúdo é processado por um **Data Loader**, preparando o texto.
4. O texto é dividido em partes menores utilizando o **Recursive Character Text Splitter**.
5. Os textos são convertidos em **embeddings**.
6. Os embeddings são armazenados no **Pinecone Vector Store**.
7. A geração dos embeddings é realizada com **OpenAI Embeddings**.

### 📄 Tipos de Documentos Suportados

- Descrições de produtos  
- Catálogos  
- Informações institucionais  

Os documentos podem ser atualizados dinamicamente **sem necessidade de alterar o fluxo principal**.

---

## 🟢 Pipeline 2 – Chatbot Web e Atendimento ao Cliente

### 🎯 Objetivo

Gerenciar a **interação direta com o usuário final**, respondendo dúvidas e realizando consultas seguras.

### 🔄 Funcionamento

1. O usuário interage com uma **interface web criada no Lovable**.
2. As mensagens são enviadas ao **n8n via Webhook**.
3. Um **Agente de IA** analisa a mensagem e decide a ação adequada:
   - 🔐 Consultar o banco de dados (quando o CPF é informado)
   - 🧠 Consultar o RAG (quando a dúvida é geral sobre produtos)
4. O agente utiliza:
   - **Google Gemini Chat Model**
   - **Simple Memory** para manter o contexto da conversa
   - **Node Think** para organização da tomada de decisão
5. A resposta é enviada ao usuário via **Respond to Webhook**.

---

## 🔐 Regras de Segurança – Uso de CPF

O acesso ao banco de dados segue uma regra de segurança rígida:

### ❌ Sem CPF informado
- O agente orienta educadamente o usuário a fornecer o CPF.
- Nenhuma consulta sensível é realizada.

### ✅ Com CPF informado
- O agente pode consultar:
  - Produtos comprados
  - Produtos cancelados
  - Status dos pedidos associados ao CPF

---

## 🧠 Uso de RAG (Retrieval-Augmented Generation)

O RAG é utilizado **exclusivamente para dúvidas gerais sobre produtos**.

- As respostas são baseadas **somente** nos documentos armazenados no **Pinecone**.
- O agente **não utiliza conhecimento externo**.
- O sistema **não gera informações fora da base de conhecimento**.

---

## 🛠️ Tecnologias Utilizadas

- **n8n** – Orquestração de automações  
- **Lovable** – Interface web do chatbot  
- **Google Drive API** – Gerenciamento de documentos  
- **Pinecone** – Vector Store para RAG  
- **OpenAI Embeddings** – Geração de embeddings  
- **Google Gemini** – Modelo de linguagem (LLM)  
- **Banco de Dados** (Supabase / PostgreSQL) – Dados de pedidos e produtos  

---

## 🎯 Principais Funcionalidades

- Atendimento automatizado ao cliente  
- Consulta segura de pedidos via CPF  
- Respostas inteligentes baseadas em RAG  
- Atualização dinâmica de conteúdo via Google Drive  
- Arquitetura modular e escalável  

---

## 🚀 Próximos Passos

- Integração com CRM (Salesforce, HubSpot)  
- Implementação de logs e monitoramento  
- Controle de permissões por perfil  
- Versionamento dos fluxos no GitHub  
- Deploy em ambiente de produção  

---

## 👤 Autor: Franklin Mendonça

Foco em Automação de Processos, Inteligência Artificial e Análise de Dados
