# Lab 03b - Roteiro: O Guardião do Código (CI/CD Simulado)

Neste laboratório, vamos aprender a estruturar uma esteira de automação (Pipeline) utilizando a sintaxe do GitHub Actions e validar a execução dos comandos em um ambiente isolado.

---

### **Contexto do Lab**
Devido às restrições de segurança do ambiente corporativo (GHE), dividiremos este lab em duas partes:
1. **Design:** Escrita do Pipeline no GitHub (onde o código mora).
2. **Execução:** Simulação dos comandos no Killercoda (onde o motor roda).

---

### **Parte 1: Desenhando o Pipeline (GitHub Web IDE)**

1. No seu repositório corporativo, pressione a tecla **`.` (ponto)** no teclado. Isso abrirá o editor visual no seu navegador.
2. Crie a seguinte estrutura de pastas e arquivo: `.github/workflows/frontend-ci.yml`
3. Cole o conteúdo abaixo:

```yaml
name: Frontend Quality Gate

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  security-check:
    runs-on: ubuntu-latest
    steps:
      - name: 1. Checkout do Código
        uses: actions/checkout@v4

      - name: 2. Auditoria de Segurança
        # Este comando busca vulnerabilidades conhecidas em bibliotecas
        run: npm audit --audit-level=high

      - name: 3. Verificação de Lint (Estilo)
        run: echo "Executando ng lint..." && sleep 2
```

4. No menu lateral esquerdo, vá em **Source Control**, digite a mensagem `ci: add quality gate pipeline` e clique em **Commit & Push**.

---

### **Parte 2: Simulando o "Runner" no Killercoda**

Como os *Runners* (máquinas que executam o código) podem estar restritos no banco, vamos agir como se fôssemos o motor do GitHub dentro do Killercoda.

1. Acesse o [Killercoda Ubuntu Playground](https://killercoda.com/playgrounds/scenario/ubuntu).
2. No terminal, vamos simular os passos do arquivo YAML que você acabou de escrever:

```bash
# 1. Simulação do Step "Checkout" (Vamos criar um projeto fake para testar)
mkdir meu-app-caixa && cd meu-app-caixa
apt install npm
npm init -y

# 2. Simulação do Step "Auditoria de Segurança"
# Vamos instalar uma biblioteca propositalmente insegura para testar o "Gate"
npm install lodash@4.17.15

# 3. Executando o comando que estaria no Pipeline
npm audit --audit-level=high
```

---

### **Missão: O "Quality Gate" na Prática**

No terminal do Killercoda, observe a saída do comando `npm audit`. 
*   **Pergunta:** O comando terminou com erro ou sucesso? 
*   **Conceito:** Se o `npm audit` falhar no Pipeline real, o GitHub bloqueia o seu código de seguir adiante. Isso é o que chamamos de **Quality Gate** (Portão de Qualidade). No ambiente da CAIXA, isso garante que nenhuma vulnerabilidade crítica suba para produção.

---

### **O que aprendemos hoje?**
1. **Sintaxe YAML:** Entendemos o que são `Triggers` (on push), `Jobs` (tarefas) e `Steps` (passos).
2. **Segurança Proativa:** Vimos como automatizar a checagem de bibliotecas inseguras.
3. **Independência de Ambiente:** Mesmo sem *Runners* ativos, agora você sabe testar a lógica da sua esteira localmente.

---

**Dica para o Chat:** Cole um print do seu terminal do Killercoda mostrando o resultado do `npm audit` para o instrutor validar!
