# Comandos Básicos de AWS

## Descripción
Este archivo contiene una lista de comandos básicos para interactuar con AWS utilizando AWSCLI. Estos comandos cubren operaciones comunes como la gestión de instancias EC2, buckets de S3, usuarios de IAM, y más.

## Comandos Básicos

### Configuración Inicial

**Configurar AWS CLI**
aws configure

## Configurar un nuevo perfil para usar con awscli.
aws configure --profile perfil-nuevo

## Crear Access Key.
aws iam create-access-key --user-name privesc6-UpdateLoginProfile-user

## Listar buckets.
aws s3 ls

## Listar un bucket especifico.
1. aws s3 ls s3://nombre-del-bucket/
2. aws s3 ls s3://nombre-del-bucket/ --recursive

## Descarga de Datos.
aws s3 cp s3://nombre-del-bucket/Importante/README.txt ./

## Subida de Archivos.
aws s3 cp .\ejemplo.txt s3://nombre-del-bucket/
	
## Consulta de metadatos.
**curl http://169.254.169.254/latest/meta-data/**
**curl http://169.254.169.254/latest/meta-data/iam/security-credentials/**
**curl http://169.254.169.254/latest/meta-data/iam/security-credentials/perfil-usuario**
	
# AWS Security Token.

## Solicitud de un Token desde una instancia ec2:
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

## Impresión del Token:	
echo $TOKEN

## Recuperación del Documento de Identidad de la Instancia:
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/dynamic/instance-identity/document

## Listado de Información de IAM:
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/info

## Solicitud de Credenciales de Seguridad de IAM:
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/ServerManager

## Almacenamiento de Credenciales en la Configuración de AWS CLI:
Finalmente, las credenciales obtenidas se pueden almacenar en el archivo de configuración de AWS CLI, permitiendo la ejecución de comandos CLI de AWS con los permisos otorgados.
	
## Crear un Keypair para acceso ssh:
aws ec2 create-key-pair --key-name my-iam-vulnerable-key --query 'KeyMaterial' --output text > keys.pem
