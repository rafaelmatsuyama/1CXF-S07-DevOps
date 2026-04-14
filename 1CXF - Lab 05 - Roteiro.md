# Lab 05 - Roteiro: Dockerizing Angular 19 (O Padrão de Mercado)

Neste laboratório, vamos transformar um projeto Angular 19 real em uma imagem Docker profissional utilizando o conceito de **Multi-stage Build**.

---

### **Etapa 0: Criando o Projeto Angular 19**

No terminal do **Killercoda**, vamos gerar um novo projeto Angular.
*Nota: Este passo pode levar de 2 a 3 minutos dependendo da rede do Killercoda.*

1. Execute o comando de criação:
   ```bash
   apt install npm
   npx @angular/cli@19 new app-caixa --style=css --routing=false --skip-git
   cd app-caixa
   ```

---

### **Etapa 1: O Dockerfile "Mestre" (Multi-stage)**

Agora vamos escrever a receita para transformar esse código em um artefato de produção leve e seguro.

1. Crie o arquivo `Dockerfile` na raiz da pasta `app-caixa`:
   ```bash
   nano Dockerfile
   ```

2. Cole o conteúdo abaixo (Atenção ao caminho da pasta `/dist/` no Angular 19):

```dockerfile
# ESTAGIO 1: Build (Onde a magica do Node acontece)
FROM node:22-alpine AS build-step
WORKDIR /app

# Copiamos as definicoes de dependencias primeiro (Cache optimization)
COPY package*.json ./
RUN npm install

# Copiamos o coodigo fonte e geramos o build de producao
COPY . .
RUN npx ng build --configuration production

# ESTAGIO 2: Runtime (Onde o Nginx serve o app)
FROM nginx:alpine

# No Angular 19, o build vai para dist/app-caixa/browser
# Copiamos APENAS os arquivos estaticos para o Nginx
COPY --from=build-step /app/dist/app-caixa/browser /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

### **Etapa 2: O Build da Imagem**

Vamos transformar o código em uma imagem pronta para o ambiente da CAIXA.

1. Execute o comando de build (isso pode levar uns instantes devido à compilação do Angular):
   ```bash
   docker build -t frontend-caixa:v1 .
   ```

2. **Validando a Imagem:**
   ```bash
   docker images | grep frontend-caixa
   ```
   *Note como a imagem final é pequena (~60MB), pois o Node e os node_modules ficaram para trás no estágio de build.*

---

### **Etapa 3: Execução e Validação**

1. Rode o seu app Angular 19 "dockerizado":
   ```bash
   docker run -d -p 8081:80 --name meu-app-angular frontend-caixa:v1
   ```

2. **Visualização:** Vá no menu **"Traffic"** do Killercoda e acesse a porta **8081**. 
   *Você verá a página padrão do Angular 19 rodando de dentro do seu container!*
   
3. Limpeza do ambiente e Preparoo para o Lab 06, pare o container:
   ```bash
   docker stop meu-app-angular
   docker ps
   ```
   *Note que a imagem ainda continua no ambiente, utilizaremos ela no Lab 06.
---

### **O que aprendemos no Lab 05?**
1. **Multi-stage Build Real:** Como compilar um projeto complexo e gerar um artefato leve.
2. **Otimização de Camadas:** Como o Docker reutiliza o cache do `npm install` se o `package.json` não mudar.
3. **Padrão Cloud Native:** Criamos um artefato idêntico ao que seria usado em um cluster Kubernetes na nuvem.

---
**Dica para o Chat:** Se o ícone do Angular apareceu na porta 8081, mande um 🚀 no chat!
