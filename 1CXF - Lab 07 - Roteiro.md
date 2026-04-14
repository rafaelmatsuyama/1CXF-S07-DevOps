# Lab 07 - Roteiro: A Unidade Fundamental (PODs e kubectl)

Neste laboratório, vamos aprender a operar um cluster Kubernetes real. Vamos entender a diferença entre um Container e um Pod, e como o `kubectl` atua como o nosso "controle remoto" para a infraestrutura.

---

### **Etapa 0: Preparação do Ambiente**

1. Acesse o [Killercoda Kubernetes Playground](https://killercoda.com/playgrounds/scenario/kubernetes).
2. Aguarde o terminal carregar e o nó ficar pronto (status `Ready`).
3. Verifique a saúde do cluster:
   ```bash
   # Listar os nós (máquinas) do cluster
   kubectl get nodes
   ```
   *Você deverá ver pelo menos um nó com o status "Ready".*

---

### **Etapa 1: O Nascimento de um Pod**

No Docker, usávamos `docker run`. No Kubernetes, usamos `kubectl run` para criar a menor unidade de execução: o **Pod**.

1. Vamos criar um Pod com um servidor Nginx (simulando nosso servidor de arquivos estáticos):
   ```bash
   kubectl run meu-frontend --image=nginx:alpine
   ```

2. **Verificando o status:**
   ```bash
   kubectl get pods
   ```
   *Note que o status passará de "ContainerCreating" para "Running".*

---

### **Etapa 2: O Detetive do Cluster (Troubleshooting)**

Diferente do Docker local, no K8s o Pod pode estar em outra máquina. Precisamos de ferramentas para "enxergar" lá dentro.

1. **Descrevendo o Pod (O Raio-X):**
   Este comando mostra TUDO o que aconteceu com o Pod (IP, Imagem, Eventos de erro).
   ```bash
   kubectl describe pod meu-frontend
   ```

2. **Vendo os Logs:**
   Igual ao Docker, essencial para debugar falhas no Angular.
   ```bash
   kubectl logs meu-frontend
   ```

3. **Interatividade (Entrando no Pod):**
   ```bash
   kubectl exec -it meu-frontend -- sh
   # Digite 'exit' para sair
   ```

---

### **Etapa 3: O Problema da Efemeridade**

O que acontece se um Pod morre? Vamos testar a resiliência (ou a falta dela) de um Pod isolado.

1. Delete o Pod:
   ```bash
   kubectl delete pod meu-frontend
   ```

2. Verifique se ele voltou:
   ```bash
   kubectl get pods
   ```
   *Resultado: No Pods found. No K8s, um Pod sozinho é frágil. Se ele morre, ninguém o traz de volta automaticamente.*

---

### **O que aprendemos no Lab 07?**
1. **kubectl:** Aprendemos os verbos principais (`get`, `describe`, `logs`, `run`, `delete`).
2. **Pod vs Container:** O Pod é a "caixa" que envolve um ou mais containers no K8s.
3. **Fragilidade:** Entendemos que rodar Pods isolados não é seguro para a CAIXA, pois eles não possuem **Autocura** (Self-healing).

---
**Dica para o Chat:** Se você conseguiu ver o status "Running" no seu primeiro Pod, mande um ☸️ no chat do Teams!
