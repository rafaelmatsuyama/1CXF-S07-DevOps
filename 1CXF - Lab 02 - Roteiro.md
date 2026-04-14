# Lab 02 - Roteiro: O Cirurgião de Código (Git Avançado)

Este laboratório foca em operações que "reescrewem o histórico" para manter um repositório limpo e profissional.

---

### **Etapa 0: Preparação (Subindo o Codespaces)**

Como as máquinas locais podem ter restrições, usaremos o **GitHub Codespaces** (um VS Code completo no seu navegador).

1. Acesse seu [GitHub](https://github.com/) e crie um **Novo Repositório** (New Repository).
2. Nomeie como: `imersao-frontend-devops` (Marque como **Public**).
3. **Não** precisa adicionar README ou .gitignore agora. Clique em **Create repository**.
4. Na tela que abrir, procure o botão verde **[ <> Code ]**.
5. Clique na aba **Codespaces** e depois no botão **Create codespace on main**.
6. Aguarde alguns segundos. When o editor abrir, procure a aba **Terminal** na parte inferior.

---

### **Cenário Inicial**
No seu terminal do Codespaces, vamos preparar o ambiente de teste:
```bash
# 1. Configurar identidade (Obrigatório para o primeiro commit)
git config --global user.email "seu-email@exemplo.com"
git config --global user.name "Seu Nome"

# 2. Criar pasta do lab e iniciar o Git
mkdir lab-git-avancado && cd lab-git-avancado
git init

# 3. Criar o primeiro arquivo e o primeiro commit
echo "app-base" > app.txt
git add . && git commit -m "feat: initial commit"

# 4. Forçar o nome da branch para 'main' (Evita erro de 'master')
git branch -M main

# 5. Criar branch de feature e adicionar commits "sujos"
git checkout -b feature/ui
echo "ui-v1" >> app.txt
git add . && git commit -m "fix: ajuste visual 1"
echo "ui-v2" >> app.txt
git add . && git commit -m "fix: ajuste visual 2"
echo "ui-v3" >> app.txt
git add . && git commit -m "fix: ajuste visual final"
```

---

### **Missão 1: O Squash (Limpeza de Histórico)**
Você tem 3 commits de "fix" que poluem o histórico. Vamos transformá-los em um único commit `feat: add new ui`.

1. Execute: `git rebase -i HEAD~3`
2. No editor que abrir (Vim ou VS Code):
   - Mantenha o primeiro como `pick`.
   - Mude os outros dois de `pick` para `squash` (ou apenas `s`).
3. Salve e feche.
4. Na tela de mensagem, digite: `feat: complete ui redesign`.
5. **Validação:** Digite `git log --oneline` e veja que agora só existem 2 commits no histórico.

---

### **Missão 2: Resolução de Conflito**
1. Volte para a main: `git checkout main`
2. Altere o arquivo na main para gerar conflito:
   ```bash
   echo "main-change" > app.txt
   git add . && git commit -m "fix: urgent main fix"
   ```
3. Tente trazer sua feature: `git merge feature/ui`
4. **O Conflito surgiu!** 
   - Abra o arquivo `app.txt` no VS Code.
   - Escolha o conteúdo que deve ficar (ou aceite ambos).
   - Salve o arquivo.
5. Finalize: `git add . && git commit -m "chore: resolve conflicts"`

---

### **Missão 3: O Salvador Reflog**
1. Delete sua branch de feature (simulando um erro): `git branch -D feature/ui`
2. Como recuperar? Digite: `git reflog`
3. Ache o hash do commit onde a branch estava (ex: `abc1234`).
4. Ressuscite a branch: `git checkout -b feature/ui abc1234`

---
**Dica para o Chat:** Cole seu `git log --oneline --graph --all` ao final da Missão 2 para o instrutor validar!
---

### **Instruções para Descomissionar o Ambiente**

Após concluir o laboratório, siga estes passos para limpar seu ambiente do Codespaces e evitar confusão nos próximos exercícios:

#### **1. Limpeza do Laboratório Específico**
Para remover apenas a pasta do exercício atual:
```bash
cd ..
rm -rf lab-git-avancado
```

#### **2. "Reset Nuclear" do Repositório (Limpeza Total)**
Se você deseja apagar **todo** o histórico de commits, branches e arquivos criados para começar um novo projeto do zero na raiz do Codespaces:
```bash
# CUIDADO: Isso apaga TODO o histórico do Git permanentemente
rm -rf .git
rm app.txt  # Remove o arquivo físico criado
```
