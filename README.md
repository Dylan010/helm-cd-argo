# helm-cd-argo

# Desafío 15 - Helm Chart & GitOps

Este proyecto demuestra el uso de Helm Charts y GitOps con ArgoCD para desplegar una aplicación en Kubernetes.

## Prerrequisitos

- Kubernetes cluster
- kubectl configurado para tu cluster
- Helm 3 instalado
- ArgoCD instalado en tu cluster

## Estructura del Proyecto

```
.
├── app.py
├── Dockerfile
├── helm-chart
│   └── desafio-15-chart
│       ├── Chart.yaml
│       ├── templates
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
├── manifiestos
│   ├── deployment.yaml
│   └── service.yaml
├── README.md
├── requirements.txt
└── templates
    └── index.html
```

## Pasos para el Despliegue

1. Clonar el repositorio:
   ```
   git clone https://github.com/Dylan010/helm-cd-argo.git
   cd helm-cd-argo
   ```

2. Crear los namespaces necesarios:
   ```
   kubectl create namespace helm-namespace
   kubectl create namespace manifests-namespace
   ```

3. Configurar ArgoCD:
   
   a. Acceder a la interfaz de ArgoCD (asegúrate de tener acceso):
   ```
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```
   
   b. Abrir en el navegador: https://localhost:8080

4. Crear aplicaciones en ArgoCD:

   a. Para el Helm Chart:
   - Nombre: helm-app
   - Proyecto: default
   - Sync Policy: Automatic
   - Repository URL: https://github.com/Dylan010/helm-cd-argo.git
   - Path: helm-chart/desafio-15-chart
   - Cluster: https://kubernetes.default.svc
   - Namespace: helm-namespace

   b. Para los manifiestos:
   - Nombre: manifests-app
   - Proyecto: default
   - Sync Policy: Manual
   - Repository URL: https://github.com/Dylan010/helm-cd-argo.git
   - Path: manifiestos
   - Cluster: https://kubernetes.default.svc
   - Namespace: manifests-namespace

5. Sincronizar las aplicaciones:
   
   Puedes hacerlo desde la UI de ArgoCD o usando la CLI:
   ```
   argocd app sync helm-app
   argocd app sync manifests-app
   ```

6. Verificar el despliegue:
   ```
   kubectl get pods -n helm-namespace
   kubectl get pods -n manifests-namespace
   ```

## Solución de Problemas

- Si encuentras errores de sincronización, verifica los logs de la aplicación en ArgoCD.
- Asegúrate de que los namespaces existan antes de sincronizar.
- Verifica que la URL del repositorio y los paths sean correctos en la configuración de ArgoCD.
