# Cleaning the house contiene los comandos aws para deshacer cambios como eliminar politicas creadas, eliminar llaves, etc.  
## Quitar una politica a un usuario  
```bash
aws iam list-attached-user-policies --user-name privesc4-CreateAccessKey-user  

aws iam detach-user-policy --user-name privesc4-CreateAccessKey-user --policy-arn arn:aws:iam::aws:policy/AdministratorAccess  
```
	
## Eliminar access key creadas  
```bash
aws iam list-access-keys --user-name privesc4-CreateAccessKey-user  

aws iam delete-access-key --user-name privesc4-CreateAccessKey-user --access-key-id AKIAYPUD57AE5ANQGKU3  
```
	
## Eliminar una Key pairs  
```bash
aws ec2 delete-key-pair --key-name my-iam-vulnerable-key  
```
