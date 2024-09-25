# Herramientas de AWS

## Descripción
Este archivo contiene una lista de herramientas y comandos utilizados para gestionar recursos en AWS. Cada herramienta incluye una breve descripción y ejemplos de uso.

## Herramientas

### AWS CLI
La interfaz de línea de comandos de AWS (AWS CLI) es una herramienta unificada para gestionar sus servicios de AWS. Con una sola herramienta para descargar e instalar, puede controlar varios servicios de AWS desde la línea de comandos y automatizarlos a través de scripts.
### En Linux
```bash
sudo apt install awscli
```


### En MacOS
```bash
brew install awscli
```

### S3scanner  
**Descripción:** S3Scanner es una herramienta de seguridad diseñada para buscar y detectar buckets de Amazon S3 mal configurados. Permite identificar buckets públicos o accesibles sin autorización adecuada, ayudando a prevenir la exposición de datos sensibles en entornos de almacenamiento en la nube.  
https://github.com/sa7mon/S3Scanner.git  

### ScoutSuite  
**Descripción:** ScoutSuite es una herramienta de auditoría de seguridad multi-nube que permite revisar configuraciones y políticas de seguridad en entornos de AWS, Azure, Google Cloud y otros proveedores. Proporciona reportes detallados sobre posibles vulnerabilidades y malas configuraciones para ayudar a mejorar la postura de seguridad en la nube.  
https://github.com/nccgroup/ScoutSuite.git  

### enumerate-iam  
**Descripción:** enumerate-iam es una herramienta que facilita la enumeración de políticas de IAM y permisos asociados a usuarios, grupos y roles en AWS. Proporciona una visión detallada de los permisos efectivos que tiene cada entidad de IAM.  
https://github.com/andresriancho/enumerate-iam.git  

### cliam  
**Descripción:** cliam es una herramienta que ayuda a realizar análisis de políticas de IAM en AWS. Permite verificar y visualizar permisos, ayudando a identificar configuraciones incorrectas y posibles escalaciones de privilegios.  
https://github.com/securisec/cliam  

### IAMFinder  
**Descripción:** IAMFinder es una herramienta diseñada para encontrar y reportar posibles problemas de seguridad en las configuraciones de IAM. Ayuda a identificar cuentas y roles con permisos excesivos que podrían ser explotados.  
https://github.com/prisma-cloud/IAMFinder  

### IAMPrivesc  
**Descripción:** IAMPrivesc es una herramienta que facilita la identificación de posibles vectores de escalación de privilegios en las políticas de IAM de AWS. Ayuda a los evaluadores de seguridad a descubrir rutas de privilegios potencialmente explotables.  
https://github.com/kriko69/IAMprivesc  

### prowler  
**Descripción:** prowler es una herramienta de auditoría y evaluación de seguridad para AWS. Realiza verificaciones basadas en las mejores prácticas de seguridad de AWS y en los controles de seguridad del CIS (Center for Internet Security).  
https://github.com/prowler-cloud/prowler  

### Cloudsplaining  
**Descripción:** Cloudsplaining es una herramienta que analiza políticas de IAM en AWS para identificar riesgos de seguridad. Genera informes que destacan permisos excesivos y recomendaciones para mitigar estos riesgos.  
https://github.com/salesforce/cloudsplaining  

### CloudMapper  
**Descripción:** CloudMapper es una herramienta de visualización y auditoría de seguridad para AWS. Permite mapear la arquitectura de una cuenta de AWS y generar gráficos interactivos que ayudan a entender mejor la infraestructura y sus posibles vulnerabilidades.  
https://github.com/duo-labs/cloudmapper  

### PACU  
**Descripción:** PACU es una herramienta de evaluación de seguridad para AWS desarrollada por Rhino Security Labs. Es una suite completa que permite realizar pruebas de penetración en entornos AWS, automatizando tareas comunes y permitiendo la explotación de vulnerabilidades conocidas.  
https://github.com/RhinoSecurityLabs/pacu  

### Gitleaks
https://github.com/gitleaks/gitleaks
