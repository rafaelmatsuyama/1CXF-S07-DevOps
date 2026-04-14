# Lab 09: Frontend na Nuvem (Azure App Service)

Neste laboratório, vamos tirar nossos artefatos do playground local e colocá-los em um Data Center real da Microsoft. Escolha um dos "sabores" abaixo para realizar o seu deploy.

---

### **Opção 09A: IT-Tools (O Canivete Suíço do Dev)**
*   **O que é:** Uma coleção de ferramentas úteis (Formatador JSON, Gerador de UUID, etc).
*   **Imagem:** `corentinth/it-tools`
*   **Porta Padrão:** 80 (Caminho Feliz).

### **Opção 09B: Swagger UI (O Padrão Corporativo)**
*   **O que é:** Interface visual para documentação e teste de APIs.
*   **Imagem:** `swaggerapi/swagger-ui`
*   **Porta Padrão:** 8080 (**Atenção:** Exige configuração manual de porta na Azure).

### **Opção 09C: 2048 Game (O Clássico Retrô)**
*   **O que é:** Um jogo de quebra-cabeça funcional rodando via Web.
*   **Imagem:** `alexwhen/docker-2048`
*   **Porta Padrão:** 80 (Caminho Feliz).

---

### **Passo 1: Criando o Web App no Portal**
1. Acesse o [Portal Azure](https://portal.azure.com).
2. Clique em **"Criar um recurso"** e pesquise por **"Web App"**.
3. No botão **Criar**, selecione **"Web App"**.
4. Configure os **Basics**:
   *   **Resource Group:** Clique em "Create new" e use `rg-imersao-frontend`.
   *   **Name:** Escolha um nome único (ex: `app-caixa-seu-nome`).
   *   **Publish:** Selecione **Container**.
   *   **Operating System:** Linux.
   *   **Region:** Brazil South (ou a mais próxima).
   *   **App Service Plan:** Deixe o padrão sugerido (Nível B1 ou F1).

### **Passo 2: Configurando o Docker (Pull-based)**
Na aba **Container**, preencha:
*   **Options:** Single Container.
*   **Image Source:** Other Container Registries.
*   **Access Type:** Public.
*   **Image and tag:** (Insira a imagem da sua opção escolhida: `corentinth/it-tools`, `swaggerapi/swagger-ui` ou `alexwhen/docker-2048`).

Clique em **Review + create** e depois em **Create**. Aguarde o deploy (pode levar 2-3 minutos).

---

### **Passo 3: O Desafio do Troubleshooting (Obrigatório para Opção 09B)**
Após o deploy, clique em **Go to resource** e acesse a URL do seu site.
*   Se você escolheu o **Swagger UI**, o site vai falhar ou carregar infinitamente.
*   **Ajuste:** No menu lateral do seu Web App, vá em **Settings > Environment Variables**.
*   Clique em **+ Add**:
    *   **Name:** `WEBSITES_PORT`
    *   **Value:** `8080` (Ou `80` se as outras opções falharem).
*   Clique em **Apply** e depois em **Apply** no topo da página.
*   Aguarde o App reiniciar e atualize o navegador. **O Frontend deve nascer!**

---

### **Passo 4: Observabilidade (Espiando o Container)**
1. No menu lateral, procure por **Monitoring > Log Stream**.
2. Veja as linhas de comando do container subindo. 
3. Tente acessar seu app e veja se novos logs aparecem (especialmente se houver erros 404/500).

---

### **Higiene de Nuvem (Importante!)**
Para não consumir todo o seu crédito de $100:
1. Vá no menu superior e procure por **Resource Groups**.
2. Selecione o seu `rg-imersao-frontend`.
3. Clique em **Delete resource group**, digite o nome para confirmar e dê adeus aos recursos.
