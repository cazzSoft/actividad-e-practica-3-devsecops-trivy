# Informe Tecnico - Practica DevSecOps con Jenkins y Trivy

## 1. Objetivo

Documentar la ejecucion del pipeline Jenkins configurado para el proyecto de practica, incluyendo construccion de backend y frontend, validacion Docker, analisis de vulnerabilidades con Trivy y generacion de evidencias tecnicas.

## 2. Evidencias recopiladas

### Evidencia 1. Ejecucion inicial del pipeline Jenkins

**Archivo:** `evidencias/jenkins/01_pipeline_ejecucion_inicial.png`

**Descripcion:**
Se ejecuto el job `actividad-e-practica-3-devsecops-trivy` desde Jenkins. El pipeline tomo el codigo fuente desde la rama `main` del repositorio GitHub y comenzo la ejecucion de las etapas definidas en el `Jenkinsfile`.

En la captura se observa que finalizaron correctamente las siguientes etapas iniciales:

- `Checkout`
- `Metadata`
- `Backend - Install`
- `Backend - Prisma`
- `Backend - Test`
- `Frontend - Install`
- `Frontend - Lint`

Tambien se observa el commit ejecutado: `9bd3488`, con la condicion `main=true`.

## 3. Pendiente de evidencia

- Resultado final del pipeline completo.
- Etapa `Docker - Build`.
- Etapa `Docker - Verify Images`.
- Etapa `Trivy - Backend Scan`.
- Etapa `Trivy - Frontend Scan`.
- Artefactos archivados en Jenkins.

### Evidencia 2. Error en etapa Docker - Validate

**Archivo:** `evidencias/jenkins/02_error_docker_compose_validate.png`

**Descripcion:**
Durante la ejecucion del build `#4`, el pipeline fallo en la etapa `Docker - Validate` al ejecutar el comando `docker compose config --quiet`.

El mensaje reportado por Jenkins fue:

```text
unknown flag: --quiet
See 'docker --help'.
```

**Analisis tecnico:**
El contenedor Jenkins tiene instalado el cliente Docker, pero no cuenta con el plugin `docker compose` ni con el binario clasico `docker-compose`. Por esta razon, Jenkins no pudo ejecutar la validacion basada en Docker Compose.

**Accion correctiva:**
Se ajusto el `Jenkinsfile` para validar la existencia de los Dockerfile y la disponibilidad del cliente Docker, y para construir las imagenes mediante `docker build` directo, manteniendo los nombres de imagen requeridos por las etapas posteriores de Trivy.

### Evidencia 3. Ejecucion exitosa de Docker y Trivy con fallo posterior en publicacion

**Archivo:** `evidencias/jenkins/03_pipeline_trivy_ok_publish_error.png`

**Descripcion:**
En el build `#5` se observa que el pipeline ejecuto correctamente las etapas principales de la practica: instalacion, pruebas, construccion frontend, validacion Docker, construccion de imagenes, verificacion de imagenes, analisis Trivy para backend y analisis Trivy para frontend.

Las etapas completadas correctamente incluyen:

- `Docker - Build`
- `Docker - Verify Images`
- `Trivy - Backend Scan`
- `Trivy - Frontend Scan`
- `Evidence - Image Metadata`

**Resultado tecnico:**
Los reportes de Trivy fueron generados y archivados como artefactos de Jenkins:

- `backend-trivy.json`
- `frontend-trivy.json`
- `backend-image-inspect.json`
- `frontend-image-inspect.json`
- `docker-images.txt`

**Observacion:**
El pipeline fallo posteriormente en la etapa `Docker - Publish` debido a que no existe en Jenkins la credencial requerida con ID `jenkins-u3`.

Mensaje identificado:

```text
ERROR: Could not find credentials entry with ID 'jenkins-u3'
```

Este fallo corresponde a una configuracion externa de credenciales para publicar en Docker Hub y no afecta la evidencia principal de analisis de vulnerabilidades con Trivy, ya que los escaneos se ejecutaron correctamente antes de la publicacion.

### Ajuste de alcance: Railway no se modifica

Para evitar afectar servicios de practicas anteriores desplegados en Railway, se decidio mantener Railway sin cambios. El pipeline conserva las etapas de Railway, pero quedan omitidas mediante la variable `SKIP_RAILWAY_DEPLOY = 'true'`.

La publicacion en Docker Hub si se mantiene activa, utilizando la credencial existente en Jenkins con ID `dockerhub-cazzsoft`.

Con este ajuste, el alcance de la practica queda centrado en Jenkins, construccion de imagenes Docker, analisis Trivy y publicacion de imagenes en Docker Hub, sin redeploy sobre Railway.

### Evidencia 4. Pipeline final exitoso con publicacion en Docker Hub

**Archivo:** `evidencias/jenkins/04_pipeline_publish_ok_railway_skipped.png`

**Descripcion:**
En el build `#6` se evidencia la ejecucion completa de las etapas principales del pipeline Jenkins. Las etapas de construccion, verificacion de imagenes, analisis Trivy y publicacion en Docker Hub finalizaron correctamente.

**Resultados destacados:**

- `Docker - Build`: correcto.
- `Docker - Verify Images`: correcto.
- `Trivy - Backend Scan`: correcto.
- `Trivy - Frontend Scan`: correcto.
- `Evidence - Image Metadata`: correcto.
- `Docker - Publish`: correcto.
- `Railway`: omitido para no afectar servicios de practicas anteriores.
- Resultado final del pipeline: `SUCCESS`.

**Artefactos generados:**

- `backend-trivy.json`
- `frontend-trivy.json`
- `backend-image-inspect.json`
- `frontend-image-inspect.json`
- `docker-images.txt`
- `docker-publish-metadata.txt`

Con esta evidencia se cierra la validacion Jenkins + Docker + Trivy, quedando lista la continuacion hacia la guia de despliegue con GitHub Actions, Docker Hub, AWS EC2 y K3s.
