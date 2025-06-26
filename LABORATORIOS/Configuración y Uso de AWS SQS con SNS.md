## Objetivo del Ejemplo

Vamos a construir un sistema donde:

1. Un **servicio publica mensajes en un tema SNS** (por ejemplo, una nueva orden de compra).
2. SNS **envía el mensaje a múltiples suscriptores**, incluyendo:

   * Una **cola SQS**, que usará un microservicio de procesamiento.
   * Un **correo electrónico** como notificación al administrador.

---

## Requisitos

* Cuenta en [AWS](https://aws.amazon.com/)
* IAM user con permisos para SNS y SQS
* AWS CLI (opcional, para comandos)
* Python (para código del consumidor, opcional)

---

## Paso 1: Crear el Tema SNS

1. Ve a la consola de AWS.
2. Abre **SNS** > **Topics** > **Create topic**.
3. Elige **Standard** como tipo.
4. Nombre: `NewOrderTopic`
5. Haz clic en **Create topic**.

---

## Paso 2: Crear una Cola SQS

1. Ve a la consola de **SQS**.
2. Crea una nueva cola:

   * Tipo: **Standard**
   * Nombre: `OrderQueue`
   * Otras configuraciones por defecto.
3. Crea la cola.

---

## Paso 3: Suscribir la Cola SQS al Tema SNS

1. Vuelve a SNS > `NewOrderTopic`.
2. Ve a **Subscriptions** > **Create subscription**.
3. **Protocol**: `Amazon SQS`
4. **Endpoint**: pega la **ARN** de la cola SQS (`OrderQueue`)
5. Haz clic en **Create subscription**

---

## Paso 4: Añadir Suscripción por Correo Electrónico (opcional)

1. En el mismo tema SNS > **Create subscription**.
2. **Protocol**: `Email`
3. **Endpoint**: tu correo (ej: [admin@ejemplo.com](mailto:admin@ejemplo.com))
4. Haz clic en **Create subscription**
5. Revisa tu correo y **confirma la suscripción**.

---

## Paso 5: Configurar Permisos SQS para recibir de SNS

1. Ve a SQS > `OrderQueue` > **Access policy**
2. Asegúrate de tener una política como esta:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "SQS:SendMessage",
    "Resource": "arn:aws:sqs:REGION:ACCOUNT_ID:OrderQueue",
    "Condition": {
      "ArnEquals": {
        "aws:SourceArn": "arn:aws:sns:REGION:ACCOUNT_ID:NewOrderTopic"
      }
    }
  }]
}
```

---

## Paso 6: Publicar un Mensaje de Prueba en SNS

1. Ve a SNS > `NewOrderTopic`
2. Haz clic en **Publish message**
3. Subject: `New Order`
4. Message:

```json
{
  "order_id": "12345",
  "customer": "Juan Pérez",
  "items": ["Producto A", "Producto B"]
}
```

5. Haz clic en **Publish message**

Esto enviará el mensaje a:

* La cola SQS (`OrderQueue`)
* Tu correo electrónico (si lo configuraste)

---

## Paso 7: Procesar Mensajes desde la Cola (Python)

```python
import boto3
import json

# Configura tus credenciales con AWS CLI o variables de entorno
sqs = boto3.client('sqs', region_name='us-east-1')
queue_url = 'https://sqs.us-east-1.amazonaws.com/TU_ID/OrderQueue'

def receive_messages():
    response = sqs.receive_message(
        QueueUrl=queue_url,
        MaxNumberOfMessages=1,
        WaitTimeSeconds=10
    )
    
    messages = response.get('Messages', [])
    for message in messages:
        body = json.loads(message['Body'])
        # Si viene de SNS, el mensaje está dentro de 'Message'
        sns_message = json.loads(body['Message'])
        print(f"Orden recibida: {sns_message}")
        
        # Eliminar mensaje después de procesar
        sqs.delete_message(
            QueueUrl=queue_url,
            ReceiptHandle=message['ReceiptHandle']
        )

while True:
    receive_messages()
```

---

## Paso 8: Probar Todo

1. Publica un nuevo mensaje en SNS.
2. Verifica que:

   * El mensaje aparece en la cola SQS.
   * Llega al correo electrónico.
   * El script Python lo consume correctamente.

---

## Paso 9: Limpieza (si es un test)

* Elimina el tema SNS y las suscripciones.
* Elimina la cola SQS.

---



