Prueba DevOps – Node.js + Docker + Kubernetes + Azure DevOps
Este proyecto es una aplicación Node.js sencilla expuesta mediante Express.
La aplicación se empaqueta en Docker, se despliega en un clúster de Kubernetes y cuenta con un pipeline CI/CD configurado en Azure DevOps para automatizar el build, push y despliegue.
Incluye:
- Dockerfile funcional
- Manifiestos Kubernetes (Deployment + Service)
- Pipeline CI/CD (azure-pipelines.yml)
- Integración con Docker Hub
- Actualización automática del deployment

🧱 Arquitectura del Proyecto
devops-prueba-angel/
│
├── azure-pipelines.yml        # Pipeline CI/CD
├── Dockerfile                 # Imagen Docker
├── index.js                   # App principal
├── package.json
├── shared/database/database.js
├── users/router.js
└── k8s/
    ├── deployment.yaml        # Deployment Kubernetes
    └── service.yaml           # Service Kubernetes



🐳 Docker
Construir la imagen
docker build -t aucancelaa/prueba-devops:latest .


Probar localmente
docker run -p 8000:8000 aucancelaa/prueba-devops:latest


Subir a Docker Hub
docker push aucancelaa/prueba-devops:latest



☸️ Kubernetes
Aplicar los manifiestos
kubectl apply -f k8s/deployment.yaml -n devops-demo
kubectl apply -f k8s/service.yaml -n devops-demo


Ver pods
kubectl get pods -n devops-demo


Reiniciar el deployment
kubectl rollout restart deployment prueba-devops-deployment -n devops-demo



🔧 Endpoints
|  |  |  | 
|  | /api/users |  | 


Ejemplo de respuesta:
{ "message": "Users endpoint funcionando" }



🔄 CI/CD con Azure DevOps
El pipeline CI/CD realiza:
- Construcción de la imagen Docker
- Push a Docker Hub
- Actualización del deployment en Kubernetes
Archivo: azure-pipelines.yml
Incluye:
- Build de imagen
- Push a Docker Hub
- Actualización del deployment
Service Connections necesarias
|  |  |  | 
| docker-hub-connection |  |  | 
| k8s-connection |  |  | 



⚠️ Nota sobre Azure DevOps Parallelism
Las organizaciones nuevas requieren solicitar el agente gratuito.
Mensaje típico:
No hosted parallelism has been purchased or granted.


Solución oficial:
https://aka.ms/azpipelines-parallelism-request
Una vez aprobado, el pipeline corre sin cambios.

🧪 Cómo probar el despliegue
Obtener IP del servicio:
kubectl get svc -n devops-demo


Probar endpoint:
curl http://<EXTERNAL-IP>/api/users



✅ Estado Final del Proyecto
- Aplicación funcionando en Kubernetes ✔
- Imagen Docker estable ✔
- Rutas corregidas ✔
- CI/CD configurado ✔ (pendiente de activación de parallelism)
- README completo ✔
