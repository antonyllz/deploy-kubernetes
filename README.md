# 📘 Projeto Kubernetes – Deploy de Aplicação Full Stack  
**Autor:** *Antony César Pereira de Araújo – Matrícula 20231380013*  
**Curso:** Redes de Computadores – IFPB  
**Disciplina:** Virtualização
**Ano:** 2025  

---

# 🚀 Descrição Geral do Projeto

Este repositório contém uma aplicação completa composta por:

- **Frontend:** React + Vite  
- **Backend:** Python Flask  
- **Banco de Dados:** PostgreSQL  
- **Orquestração:** Kubernetes (Minikube)

O objetivo deste projeto é demonstrar um ambiente realista de deploy utilizando Kubernetes, abordando:

- Deployments  
- Services (ClusterIP e NodePort)  
- PersistentVolumeClaims  
- ConfigMaps e Secrets  
- Ingress Controller  
- Build local de imagens via Minikube

A aplicação originalmente funciona com Docker Compose, porém aqui criamos toda a estrutura para rodar 100% no Kubernetes.

---

# 📁 Estrutura do Repositório

```
Projeto01/
├── backend/
│   ├── app.py
│   ├── Dockerfile
│   ├── models.py
│   └── requirements.txt
├── frontend/
│   ├── Dockerfile
│   ├── public/
│   ├── src/
│   ├── package.json
│   └── vite.config.js
├── docker-compose.yml
├── generate_k8s.sh
└── k8s/
```

A pasta **k8s/** é criada automaticamente pelo script.

---

# ⚙️ 1. Pré-requisitos

### Debian / Ubuntu

```bash
sudo apt update
sudo apt install -y curl wget apt-transport-https ca-certificates gnupg
```

---

## ✔ Instalar Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

## ✔ Instalar kubectl

```bash
sudo apt install -y kubectl
```

---

# 🚀 2. Clonar o Repositório

```bash
git clone https://github.com/pedrofilhojp/kube-students-projects.git
cd kube-students-projects/Projeto01
```

---

# 🏗 3. Gerar todos os arquivos YAML do Kubernetes

O script `generate_k8s.sh` cria automaticamente:

- configmap  
- secrets  
- deployments  
- services  
- pvc  
- ingress  
- estrutura completa em `k8s/`

### Executar:

```bash
chmod +x generate_k8s.sh
./generate_k8s.sh
```

---

# 🐳 4. Preparar o Minikube para build local

```bash
minikube start --driver=docker
eval $(minikube -p minikube docker-env)
```

---

# 🏗 5. Build das imagens

### Backend:

```bash
docker build -t api-flask:1.0 ./backend
```

### Frontend:

```bash
docker build -t react-frontend:1.0 ./frontend
```

---

# ⚡ 6. Aplicar tudo no Kubernetes

```bash
kubectl apply -f k8s/
```

---

# 🔎 7. Verificar status dos recursos

```bash
kubectl get pods
kubectl get svc
kubectl get deployments
```

---

# 🌐 8. Acessar via Ingress

Habilitar o ingress:

```bash
minikube addons enable ingress
```

Obter o IP:

```bash
minikube ip
```

Acessar no navegador:

```
http://<MINIKUBE_IP>
```

Exemplo:

```
http://192.168.49.2
```

---

# 🛠 9. Estrutura dos Serviços

| Serviço     | Tipo       | Porta | Caminho |
|-------------|------------|-------|---------|
| Frontend    | NodePort   | 30000 | `/`     |
| Backend     | ClusterIP  | 5000  | `/api`  |
| PostgreSQL  | ClusterIP  | 5432  | —       |
| Ingress     | Ingress    | 80    | `/`     |

---

# 🧪 10. Testar a API

```bash
curl http://<MINIKUBE_IP>/api
```

