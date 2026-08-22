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
