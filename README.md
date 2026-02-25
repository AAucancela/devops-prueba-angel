 README – Prueba DevOps (Ángel Aucancela)
🚀 Descripción del Proyecto
Este proyecto implementa un pipeline CI/CD completo utilizando Azure DevOps, Docker, Docker Hub, un agente local Windows, y despliegue en Kubernetes (Minikube).
El objetivo es demostrar la capacidad de automatizar el ciclo de vida de una aplicación Node.js desde el build hasta el despliegue.

🧩 Arquitectura General
1. CI/CD en Azure DevOps
- Pipeline YAML automatizado.
- Agente local autoalojado.
- Build de Node.js.
- Construcción y publicación de imagen Docker.
- Despliegue a Kubernetes.
2. Contenedores
- Dockerfile optimizado basado en node:18-alpine.
- Imagen publicada en Docker Hub:
- aucancelaa/prueba-devops:latest
- aucancelaa/prueba-devops:<BuildId>
3. Kubernetes
- Despliegue mediante Deployment + Service.
- Exposición del servicio en puerto 3000.
- Verificación mediante kubectl get pods y kubectl get svc.

🛠️ Tecnologías Utilizadas
- Azure DevOps Pipelines
- Agente Local Windows
- Docker Desktop
- Docker Hub
- Node.js 18
- Kubernetes (Minikube)
- YAML para CI/CD

📦 Pipeline CI/CD (YAML Final)
trigger:
  - main

pool:
  name: PoolCompilacion
  demands:
  - agent.name -equals AgentVR

variables:
  DOCKER_HOST: tcp://127.0.0.1:2375
  NODE_VERSION: '18.x'
  IMAGE_NAME: 'prueba-devops'
  REGISTRY: 'docker.io'
  DOCKER_REPO: 'aucancelaa/prueba-devops'

steps:
  - task: NodeTool@0
    inputs:
      versionSpec: $(NODE_VERSION)
    displayName: "Instalar Node.js"

  - script: npm install
    displayName: "Instalar dependencias"

  - task: Docker@2
    displayName: "Build & Push Docker image"
    inputs:
      command: buildAndPush
      repository: $(DOCKER_REPO)
      dockerfile: '**/Dockerfile'
      containerRegistry: 'DockerHub-Service-Connection'
      tags: |
        latest
        $(Build.BuildId)



🧪 Cómo funciona el pipeline
1. Instalación de Node.js
Usa NodeTool@0 para instalar la versión 18.x.
2. Instalación de dependencias
Ejecuta npm install.
3. Build & Push de Docker
- Construye la imagen usando el Dockerfile.
- Etiqueta la imagen con:
- latest
- $(Build.BuildId)
- Publica la imagen en Docker Hub.

🖥️ Agente Local – Configuración Clave
El agente local se configuró para permitir que Azure DevOps ejecute comandos Docker.
Para ello se habilitó:
- Docker Desktop con daemon TCP (tcp://127.0.0.1:2375)
- Contexto Docker personalizado (tcp-context)
- Variable DOCKER_HOST en el pipeline
Esto permite que el agente ejecute:
- docker build
- docker push
- docker inspect
sin errores de permisos.

☸️ Despliegue en Kubernetes
Archivos utilizados:
deployment.yaml
- ReplicaSet con 2 pods.
- Imagen tomada desde Docker Hub.
service.yaml
- Tipo NodePort.
- Expone la aplicación en el puerto 3000.
Comandos utilizados:
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
kubectl get svc



📝 Notas Importantes
❌ Escaneo de vulnerabilidades (Trivy)
El escaneo con Trivy fue considerado, pero se omitió en la versión final debido a limitaciones del agente local Windows (PATH y permisos).
En un agente Linux funcionaría sin ajustes adicionales.
Esto se documenta para transparencia técnica.

🎯 Resultado Final
El proyecto cumple con:
✔ CI/CD completo
✔ Build automatizado
✔ Publicación de imagen Docker
✔ Agente local configurado correctamente
✔ Despliegue funcional en Kubernetes
✔ Pipeline estable y reproducible
✔ Documentación clara y profesional

🙌 Autor
Ángel Aucancela
Prueba DevOps – 2026
