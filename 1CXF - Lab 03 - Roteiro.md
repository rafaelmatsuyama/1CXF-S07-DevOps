# Lab 03 - Roteiro: Minha Primeira GitHub Action

Neste laboratório, vamos automatizar a verificação de qualidade do nosso código usando o motor de CI/CD do GitHub.

---

### **Etapa 0: Preparação do Projeto (Obrigatório)**
Para que os comandos de CI/CD funcionem, o repositório precisa ser reconhecido como um projeto Node.js. 
No terminal do seu Codespaces, execute:

```bash
# 1. Inicializa o projeto (Cria o package.json)
npm init -y

# 2. Cria a estrutura de pastas do GitHub
mkdir -p .github/workflows
```

---

### **Etapa 1: Configurando o Pipeline (YAML)**
Crie o arquivo `.github/workflows/ci.yml` e cole o conteúdo abaixo. 
*Nota: Atualizamos para Node 22 para evitar avisos de depreciação do GitHub.*

```yaml
name: Frontend CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    # Forçamos o uso do Node 24 para as Actions internas
    env:
      FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

    steps:
      - name: 1. Checkout do Código
        uses: actions/checkout@v4

      - name: 2. Setup do Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22' # Versão estável em 2026

      - name: 3. Auditoria de Segurança
        # O '|| true' evita que o lab trave se não houver vulnerabilidades/dependências
        run: npm audit --audit-level=high || echo "Nenhuma vulnerabilidade crítica encontrada."

      - name: 4. Instalação de Dependências
        run: npm install

      - name: 5. Verificação de Build (Simulada)
        run: echo "Executando build do Angular 19..." && sleep 5
```

---

### **Etapa 2: Disparando o Pipeline**
Envie as alterações para o GitHub:
```bash
git add .
git commit -m "ci: add stable github actions workflow"
git push origin main
```

---

### **Etapa 3: Monitorando a Execução**
1. Vá para a aba **"Actions"** no seu repositório no GitHub.com.
2. Clique no workflow **"Frontend CI"**.
3. Veja o pipeline rodando e explore os logs do passo **"Auditoria de Segurança"**.

---

### **Desafio Extra: O "Quality Gate"**
1. No seu arquivo `ci.yml`, remova o `|| echo ...` do passo 3.
2. No terminal, instale uma biblioteca propositalmente vulnerável:
   `npm install lodash@4.17.15`
3. Faça o `push`.
4. **O Pipeline deve falhar (Vermelho).** Isso é um "Quality Gate": impedimos que uma brecha de segurança chegue ao ambiente da CAIXA.

---

### **Limpeza do Ambiente (Descomissionamento)**

Como este é o último laboratório da aula, siga os passos abaixo para encerrar seu ambiente com segurança:

#### **1. Encerrar o Codespace (2 caminhos possíveis)**

*   **Opção A (Atalho):** 
    1. Pressione `Ctrl + Shift + P` (ou `F1`) para abrir a Paleta de Comandos.
    2. Digite: **Codespaces: Stop Current Codespace** e dê Enter.
*   **Opção B (Interface):**
    1. No canto inferior esquerdo, clique no botão azul/verde que diz **"Codespaces: [nome-do-repo]"**.
    2. No menu que abrir no topo, selecione **"Stop Current Codespace"**.

Isso interrompe o consumo de horas gratuitas imediatamente.

#### **2. Gestão do Repositório (Opcional)**
- Se você deseja manter esse aprendizado na sua conta do GitHub, não precisa fazer nada.
- Se deseja deletar o repositório: 
  - Vá em **Settings** (dentro do repo no GitHub.com).
  - Role até o final (**Danger Zone**).
  - Clique em **Delete this repository**.

#### **3. Até amanhã!**
- Amanhã focaremos em **Docker e Containers**, trazendo este mesmo projeto para dentro de uma imagem isolada.
