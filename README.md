# Introducción a Herramientas DevOps
## Evaluación Parcial N°3

# Automatización de Despliegue en AWS EKS mediante GitHub Actions

## Integrantes

- Nombre Integrante 1
- Nombre Integrante 2

---

# Descripción

Este proyecto corresponde a la Evaluación Parcial N°3 de la asignatura **Introducción a Herramientas DevOps**.

El objetivo fue implementar un entorno de despliegue automatizado utilizando Amazon EKS y GitHub Actions, permitiendo ejecutar una aplicación contenedorizada de forma escalable y automatizada.

La solución contempla:

- Orquestación de contenedores mediante Amazon EKS.
- Despliegue de Frontend y Backend.
- Almacenamiento de imágenes Docker en Amazon ECR.
- Automatización del despliegue mediante GitHub Actions.
- Escalamiento automático utilizando Horizontal Pod Autoscaler (HPA).
- Comunicación entre servicios dentro del clúster.

---

# Objetivos

- Implementar un clúster Kubernetes en AWS.
- Automatizar el proceso de Build, Push y Deploy.
- Ejecutar la aplicación utilizando contenedores.
- Implementar escalamiento automático.
- Aplicar buenas prácticas DevOps.

---

# Arquitectura

El flujo de despliegue implementado es el siguiente:

```
GitHub

   │
Git Push

   │
GitHub Actions

   │
Docker Build

   │
Push hacia Amazon ECR

   │
Deploy en Amazon EKS

   │
Frontend ─────────► Backend
```

---

# Tecnologías utilizadas

| Tecnología | Uso |
|------------|-----|
| Amazon EKS | Orquestación |
| Amazon ECR | Registro de imágenes Docker |
| Docker | Contenedores |
| Kubernetes | Orquestación |
| GitHub Actions | CI/CD |
| kubectl | Administración del clúster |
| AWS CLI | Administración de AWS |

---

# Estructura del proyecto

```
.
├── backend/
├── frontend/
├── k8s/
│   ├── backend-deployment.yaml
│   ├── frontend-deployment.yaml
│   ├── services.yaml
│   └── hpa.yaml
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
└── README.md
```

---

# Configuración del Clúster

Se configuró un clúster utilizando Amazon EKS.

La infraestructura considera:

- Clúster Kubernetes.
- Node Group.
- Roles IAM.
- VPC.
- Security Groups.
- Subredes.

Todo el entorno fue configurado para permitir el despliegue automático de los servicios mediante GitHub Actions.

---

# Despliegue de Servicios

La aplicación está dividida en dos componentes.

## Frontend

- Imagen Docker almacenada en Amazon ECR.
- Deployment Kubernetes.
- Acceso público mediante Load Balancer.

## Backend

- Imagen Docker almacenada en Amazon ECR.
- Deployment Kubernetes.
- Comunicación interna con el Frontend mediante Kubernetes.

---

# Load Balancer

Se utilizó un **Load Balancer** para exponer el Frontend y permitir el acceso desde Internet.

Su función es distribuir las solicitudes hacia los Pods disponibles, mejorando la disponibilidad del servicio.

---

# Autoscaling

Se implementó un Horizontal Pod Autoscaler (HPA).

Configuración:

- Réplicas mínimas: 2
- Réplicas máximas: 5
- Umbral de CPU: 50%

El objetivo fue aumentar automáticamente la cantidad de Pods cuando el consumo de CPU supera el valor definido.

---

# Pipeline CI/CD

El pipeline automatiza completamente el despliegue de la aplicación.

Flujo de ejecución:

```
Git Push

↓

GitHub Actions

↓

Docker Build

↓

Push hacia Amazon ECR

↓

Deploy en Amazon EKS
```

Cada vez que se realiza un **Push** al repositorio:

1. Se construye la imagen Docker.
2. Se publica en Amazon ECR.
3. Se actualiza el Deployment.
4. Kubernetes despliega automáticamente la nueva versión.

---

# GitHub Secrets

Para proteger las credenciales de AWS se utilizaron GitHub Secrets.

Variables utilizadas:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_SESSION_TOKEN

Gracias a ello las credenciales nunca quedan expuestas dentro del repositorio.

---

# Logs y Validación

Durante las pruebas se verificó el correcto funcionamiento del clúster mediante:

```bash
kubectl get pods
```

Este comando permitió comprobar:

- Estado de los Pods.
- Correcto despliegue.
- Disponibilidad de los servicios.

---

# Evaluación y Mejora

## Problema encontrado

Durante las primeras pruebas el Deployment presentó el error **ImagePullBackOff**, ya que Kubernetes intentaba descargar una imagen que aún no existía en Amazon ECR.

## Solución encontrada

Se reorganizó el pipeline para ejecutar las siguientes etapas en orden:

1. Build de la imagen Docker.
2. Push hacia Amazon ECR.
3. Actualización del Deployment.
4. Rollout Restart para forzar la descarga de la nueva imagen.

Con esta modificación el despliegue comenzó a ejecutarse correctamente.

---

# Resultados

Se verificó correctamente:

- Frontend accesible públicamente.
- Backend funcionando dentro del clúster.
- Comunicación Frontend → Backend.
- Despliegue automático tras cada Push.
- Pods recreados correctamente.
- Escalamiento automático.

---

# Conclusiones

La implementación permitió automatizar completamente el proceso de despliegue utilizando Amazon EKS y GitHub Actions.

Se consiguió una solución funcional que integra contenedores, Kubernetes y automatización CI/CD, permitiendo reducir la intervención manual durante las actualizaciones y facilitando el despliegue continuo de la aplicación.

Además, la utilización de Horizontal Pod Autoscaler permitió mejorar la disponibilidad del servicio frente a aumentos de carga, aplicando conceptos fundamentales de DevOps y orquestación de contenedores.