# Lab 06 - Roteiro: O Maestro com Docker Compose (Full Stack Local)

Neste laboratório final da aula, vamos subir um ambiente completo com um único comando. Teremos o nosso **App Angular** conversando com uma **API de Mock (JSON Server)** em uma rede isolada, garantindo a persistência dos dados com **Volumes**.

---

### **Etapa 1: Preparando a API de Mock**

Para simular o backend da CAIXA, usaremos o `json-server`.

1. Crie uma pasta para o projeto full stack no **Killercoda**:
   ```bash
   mkdir ambiente-caixa && cd ambiente-caixa
   ```

2. Crie o arquivo de banco de dados (`db.json`):
   ```bash
   echo '{"usuarios": [{"id": 1, "nome": "Rafael"}]}' > db.json
   ```

3. **Ajuste de Permissão:** No ambiente Killercoda, execute o comando abaixo para garantir que o container consiga salvar dados no arquivo:
   ```bash
   chmod 777 db.json
   ```

---

### **Etapa 2: O Orquestrador (Docker Compose)**

Agora vamos escrever o arquivo que descreve como o Frontend e o Backend devem interagir.

1. Crie o arquivo `docker-compose.yml`:
   ```bash
   nano docker-compose.yml
   ```

2. Cole o conteúdo abaixo:

```yaml
services:
  # Nosso app Angular que criamos no Lab 05
  frontend:
    image: frontend-caixa:v1
    ports:
      - "8081:80"
    networks:
      - rede-caixa
    depends_on:
      - backend

  # API de Mock que simula o Backend
  backend:
    image: clue/json-server
    ports:
      - "3000:80"
    volumes:
      # Montamos o diretório atual na pasta /data do container. 
      # Isso garante que o Docker sincronize as alterações no db.json corretamente.
      - .:/data
    networks:
      - rede-caixa

networks:
  # Criamos uma rede privada para os containers conversarem entre si
  rede-caixa:
    driver: bridge
```

---

### **Etapa 3: Subindo o Ambiente**

1. Execute o comando para subir tudo em background:
   ```bash
   docker-compose up -d
   ```

2. **Validando a Orquestração:**
   ```bash
   docker-compose ps
   ```
   *Você deverá ver os dois serviços (frontend e backend) como "Up".*

---

### **Etapa 4: Testando a Persistência (Volume)**

1. Vamos adicionar um dado novo na API Mock via terminal:
   ```bash
   curl -X POST -H "Content-Type: application/json" -d '{"nome": "Dev CAIXA"}' http://localhost:3000/usuarios
   ```

2. Agora vamos destruir o ambiente e subir de novo:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

3. **A Prova Real:** O dado "Dev CAIXA" ainda deve estar lá porque usamos um Volume e ajustamos as permissões:
   ```bash
   curl http://localhost:3000/usuarios
   ```

---

### **O que aprendemos no Lab 06?**
1. **Docker Compose:** Como gerenciar múltiplos containers sem digitar comandos gigantescos.
2. **Networks:** Como os containers se enxergam (o frontend consegue acessar o backend pelo nome do serviço: `http://backend`).
3. **Volumes (Persistência):** Como evitar a perda de dados ao deletar containers (essencial para bancos de dados e mocks).

---
**Dica para o Chat:** Se você conseguiu ver os usuários "Rafael" e "Dev CAIXA" no terminal, mande um ✅✅ no chat!
