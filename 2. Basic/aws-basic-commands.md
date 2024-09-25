# Comandos Básicos de AWS

## Descripción
Este archivo contiene una lista de comandos básicos para interactuar con AWS utilizando AWSCLI. Estos comandos cubren operaciones comunes como la gestión de instancias EC2, buckets de S3, usuarios de IAM, y más.

## Comandos Básicos

## Configurar un nuevo perfil para usar con awscli.
```bash
aws configure --profile perfil-nuevo
```

## Crear Access Key.
```bash
aws iam create-access-key --user-name privesc6-UpdateLoginProfile-user
```

## Listar buckets.
```bash
aws s3 ls
```

## Listar un bucket especifico.
```bash
aws s3 ls s3://nombre-del-bucket/  

aws s3 ls s3://nombre-del-bucket/ --recursive  
```

## Descarga de Datos de un Bucket.
```bash
aws s3 cp s3://nombre-del-bucket/Importante/README.txt ./
```

## Subida de Archivos a un Bucket.
```bash
aws s3 cp .\ejemplo.txt s3://nombre-del-bucket/
```

## Identificar la politica de un bucket.
```bash
aws s3api get-bucket-policy --bucket Nombre-del-Bucket --profile Perfil-de-Prueba
```

## Confirmar si el bucket es publico.
```bash
aws s3api get-public-access-block --bucket Nombre-del-Bucket --profile Perfil-de-Prueba
```

## Consulta de metadatos.
```bash
curl http://169.254.169.254/latest/meta-data/  

curl http://169.254.169.254/latest/meta-data/iam/security-credentials/  

curl http://169.254.169.254/latest/meta-data/iam/security-credentials/perfil-usuario  
```

## Listar las instancias de Lightsail  
```bash
aws lightsail get-instances --profile cpna-9151 --region us-east-1
```

## Crea una regla firewall de entrada en Lightsail  
```bash
aws lightsail put-instance-public-ports --instance-name <nombre-de-la-instancia> --port-infos fromPort=22,toPort=22,protocol=TCP,cidrs=<tu-ip>/32 --profile <tu-perfil> --region us-east-1
```


# AWS Security Token.

## Solicitud de un Token desde una instancia ec2:
```bash
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
```

## Impresión del Token:	
```bash
echo $TOKEN
```

## Recuperación del Documento de Identidad de la Instancia:
```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/dynamic/instance-identity/document
```

## Listado de Información de IAM:
```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/info
```

## Solicitud de Credenciales de Seguridad de IAM:
```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/ServerManager
```

## Almacenamiento de Credenciales en la Configuración de AWS CLI:
Finalmente, las credenciales obtenidas se pueden almacenar en el archivo de configuración de AWS CLI, permitiendo la ejecución de comandos CLI de AWS con los permisos otorgados.
	
## Crear un Keypair para acceso ssh:
```bash
aws ec2 create-key-pair --key-name my-iam-vulnerable-key --query 'KeyMaterial' --output text > keys.pem
```
