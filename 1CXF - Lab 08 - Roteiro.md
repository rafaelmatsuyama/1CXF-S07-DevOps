# Lab 08 - Roteiro: Orquestração e Alta Disponibilidade (Visual)

Neste laboratório, vamos aprender a escalar e expor uma aplicação no Kubernetes, vendo o balanceamento de carga agir visualmente.

---

### **Etapa 0: Preparação do Ambiente**

1. Acesse o [Killercoda Kubernetes Playground](https://killercoda.com/playgrounds/scenario/kubernetes).
2. Aguarde o terminal carregar (status `Ready`).

---

### **Etapa 1: Criando o Deployment (Escala)**

1. No terminal do Killercoda, crie o arquivo `angular-deploy.yaml`:
   ```bash
   nano angular-deploy.yaml
   ```

2. Cole o conteúdo abaixo:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: angular-caixa
spec:
  replicas: 5 
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: angular-container
        image: paulbouwer/hello-kubernetes:1.10 
        ports:
        - containerPort: 8080
        env:
        - name: MESSAGE
          value: "Imersao Frontend CAIXA - Escala Ativa!"
```

3. Aplique o manifesto:
   ```bash
   kubectl apply -f angular-deploy.yaml
   ```

---

### **Etapa 2: Expondo o App (Service)**

1. Crie o Service:
   ```bash
   kubectl expose deployment angular-caixa --type=NodePort --port=8080 --name=angular-service
   ```

2. Descubra a porta de acesso:
   ```bash
   kubectl get svc angular-service
   ```

3. **Visualização:** Vá no menu **"Traffic Control"** do Killercoda e acesse a porta encontrada. 
   *A página mostrará qual Pod está respondendo. Ao dar F5, você verá o balanceamento de carga entre os 5 Pods.*

---

### **Etapa 3: Simulando Falha e Autocura**

1. Delete todos os Pods: `kubectl delete pods -l app=frontend`
2. Verifique o Kubernetes recriando-os: `kubectl get pods -w`
3. A aplicação permanece disponível no navegador.
