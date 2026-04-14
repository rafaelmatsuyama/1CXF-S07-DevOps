# Lab 04 - Roteiro: Hello Docker World (O Primeiro Contato)

Neste laboratório, vamos dar nossos primeiros passos com containers utilizando o **Killercoda**. Vamos aprender a baixar imagens, rodar containers, mapear portas e interagir com um servidor web "vivo".

---

### **Etapa 0: Preparação do Ambiente**

1. Acesse o [Killercoda Ubuntu Playground](https://killercoda.com/playgrounds/scenario/ubuntu).
2. Aguarde o terminal carregar no lado direito da tela. 
3. Verifique se o Docker está pronto:
   ```bash
   docker --version
   ```

---

### **Etapa 1: Rodando seu Primeiro Container (Nginx)**

Vamos subir um servidor web Nginx (muito usado para servir apps Angular) com um único comando.

1. Execute o comando abaixo:
   ```bash
   # -d: Roda em background (detached)
   # -p 8080:80: Mapeia a porta 8080 do host para a 80 do container
   # --name meu-servidor: Dá um nome amigável ao container
   docker run -d -p 8080:80 --name meu-servidor nginx:alpine
   ```

2. **Validando a execução:**
   ```bash
   docker ps
   ```
   *Você verá o container `meu-servidor` com o status "Up".*

3. **Visualizando o Site:**
   No Killercoda, clique no menu **"Traffic"** (ícone de rede no topo ou lateral) e selecione a porta **8080**. Uma nova aba abrirá com a mensagem padrão: *"Welcome to nginx!"*.

---

### **Etapa 2: O Cirurgião de Containers (Interatividade)**

Containers não são "caixas pretas". Podemos entrar neles e alterar arquivos em tempo real.

1. Vamos entrar no terminal do container que está rodando:
   ```bash
   docker exec -it meu-servidor sh
   ```
   *Note que o seu prompt mudou. Você agora está "dentro" do container Linux.*

2. Vamos alterar a página inicial do Nginx:
   ```bash
   echo "<h1>Ola Time CAIXA - Container Rodando!</h1>" > /usr/share/nginx/html/index.html
   exit
   ```

3. **Validação:** Volte à aba do navegador onde o Nginx estava aberto e dê um **F5 (Refresh)**. O texto mudou!

---

### **Etapa 3: Ciclo de Vida e Faxina**

A agilidade do Docker vem de criar e destruir ambientes rapidamente.

1. Parar o servidor:
   ```bash
   docker stop meu-servidor
   ```

2. Tentar rodar novamente com o mesmo nome (vai dar erro!):
   ```bash
   docker run -d -p 8080:80 --name meu-servidor nginx:alpine
   ```
   *O Docker dirá que o nome já está em uso.*

3. Remover o container antigo e limpar tudo:
   ```bash
   docker rm meu-servidor
   # Comando de faxina geral (remove containers parados e imagens sem uso)
   docker system prune -f
   ```

---

### **O que aprendemos no Lab 04?**
1. **Imagens vs Containers:** A imagem é o "projeto", o container é o "prédio construído".
2. **Port Mapping (`-p`):** Como conectar o mundo exterior ao interior do container.
3. **Execução Interativa (`exec`):** Como realizar manutenções rápidas em containers ativos.

---
**Dica para o Chat:** Quando conseguir ver a frase "Ola Time CAIXA" no seu navegador, mande um ✅ no chat do Teams!
