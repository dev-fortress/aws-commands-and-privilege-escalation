# IAM
## Enumeración de Usuarios
aws sts get-caller-identity  
aws iam list-users  
aws iam list-groups-for-user --user-name usuario  
aws iam list-virtual-mfa-devices  
aws iam list-attached-user-policies --user-name usuario  
aws iam list-user-policies --user-name usuario  
		
## Listado de Grupos IAM
aws iam list-groups  
aws iam list-attached-group-policies --group-name Developers  
aws iam list-group-policies --group-name Developers  
		
## Listado de Roles IAM	aws iam list-roles
aws iam list-attached-role-policies --role-name AWSServiceRoleForAmazonGuardDuty  
aws iam list-role-policies --role-name AWSServiceRoleForAmazonGuardDuty  
aws iam get-role --role-name AWSServiceRoleForAmazonGuardDuty  
		
## Listado de Todas las Políticas IAM	
aws iam list-policies --profile perfile-comprometido  
aws iam get-policy --policy-arn arn:aws:iam::583318501385:policy/Buckets-Test --profile perfile-comprometido  
aws iam list-policy-versions --policy-arn arn:aws:iam::583318501385:policy/Buckets-Test --profile perfile-comprometido  
aws iam get-policy-version --policy-arn arn:aws:iam::583318501385:policy/Buckets-Test --version-id v1 --profile perfile-comprometido  
		
# S3 Buckets	Listar un s3 con credenciales comprometidas
aws s3 ls --profile perfile-comprometido  

## Listar un s3 sin credenciales
aws s3 ls s3://cpna-test-public/ --no-sign-request  

## Listar un s3 sin credenciales y de manera recursiva
aws s3 ls s3://cpna-test-public/ --recursive  --no-sign-request  
		
# EC2	
## Obtener información de las conexiones de una VPC
aws ec2 describe-vpc-peering-connections  

## enumeran las subredes de dichos VPC	
aws ec2 describe-subnets --filters "Name=vpc-id, Values=vpc-075cb81f5ac4a4d20"  

## identificar tablas de enrutamiento networking configuradas en cada VPC	
aws ec2 describe-route-tables --filters "Name=vpc-id, Values=vpc-075cb81f5ac4a4d20"  

## ACL para el VPC	
aws ec2 describe-network-acls --filters "Name=vpc-id, Values=vpc-075…4d20"  

## identificar que EC2 hace parte de dichos VPC	
aws ec2 describe-instances --filters "Name=vpc-id, Values=vpc-075cb81f5ac4a4d20"  

## Listar Todas las Instancias	
aws ec2 describe-instances  

## Detalles de Instancias Específicas
aws ec2 describe-instances --instance-ids i-1234567890abcdef0  

## Atributos de Instancia	
aws ec2 describe-instance-attribute --attribute userData --instance-id i-1234567890abcdef0  

## Asociaciones de Perfiles de IAM	
aws ec2 describe-iam-instance-profile-associations  

## Grupos de Seguridad Configurados	
aws ec2 describe-security-groups  

## Modificar Reglas de Grupo de Seguridad	
aws ec2 authorize-security-group-ingress --group-id sg-0a4c90b45f6200f2e --protocol tcp --port 2222 --cidr 181.206.25.59/32  


