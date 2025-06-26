# Configuración de Google Cloud Pub/Sub

Esta guía te ayudará a configurar Google Cloud Pub/Sub para enviar y recibir mensajes usando una **Cloud Function como suscriptor**.

---

## Paso 1: Crear y Configurar un Proyecto en Google Cloud

1. Accede a la [Consola de Google Cloud](https://console.cloud.google.com/).
2. Crea un nuevo proyecto o selecciona uno existente.
3. Ve al menú **"API y servicios" > "Biblioteca"**.
4. Busca y habilita la API **"Pub/Sub"**.

---

## Paso 2: Crear un Tema (Topic)

Un **tema** es el canal al que los publicadores envían mensajes.

1. Ve a la sección **Pub/Sub** en el menú de navegación.
2. Haz clic en **"Crear tema"**.
3. Escribe un nombre para tu tema, por ejemplo: `mi-tema`.
4. Haz clic en **"Crear"** para guardarlo.

---

## Paso 3: Crear una Suscripción

Una **suscripción** conecta el tema con un destino que recibirá los mensajes.

1. En **Pub/Sub**, selecciona tu tema (`mi-tema`).
2. Haz clic en **"Crear suscripción"**.
3. Define un nombre, por ejemplo: `mi-suscripcion`.
4. En **Tipo de entrega**, selecciona **"Push"**.
5. En **Push Endpoint**, pega la URL de tu **Google Cloud Function**.
6. Haz clic en **"Crear"**.

---

## Paso 4: Publicar Mensajes

Puedes publicar mensajes a un tema desde la consola o usando código.
Aquí tienes un ejemplo en **Python**:

```python
from google.cloud import pubsub_v1

# Inicializar el publicador
publisher = pubsub_v1.PublisherClient()
topic_path = publisher.topic_path('YOUR_PROJECT_ID', 'mi-tema')

# Publicar un mensaje
message = "¡Hola, mundo!"
future = publisher.publish(topic_path, message.encode('utf-8'))
print(f'ID del mensaje: {future.result()}')
```

---

## Paso 5: Crear una Google Cloud Function para Escuchar Mensajes

Una función puede actuar como **suscriptor Push** y ejecutarse automáticamente cuando se publique un mensaje.

### 1. Crear la Función

1. En la consola de Google Cloud, ve a **"Cloud Functions"**.
2. Haz clic en **"Crear función"**.
3. Asigna un nombre, por ejemplo: `mi-funcion`.
4. En el campo **"Tipo de activador"**, selecciona **HTTP**.
5. Selecciona el entorno de ejecución, por ejemplo: `Python 3.9`.

### 2. Código para la Función

Esta función recibe mensajes de Pub/Sub, los decodifica y los imprime:

```python
import base64

def pubsub_listener(event, context):
    # Verificar y decodificar el mensaje
    if 'data' in event:
        message_data = base64.b64decode(event['data']).decode('utf-8')
        print(f'Mensaje recibido: {message_data}')
    else:
        print('No se encontraron datos del mensaje')
```

> **Nota:** Los mensajes de Pub/Sub vienen codificados en base64. Este código los decodifica para imprimir el texto real.

### 3. Despliega la Función

Una vez implementada, Google generará automáticamente una **URL pública**, que usarás como **Push Endpoint** en tu suscripción.

---

## Paso 6: Probar la Configuración

1. Vuelve a la sección **Pub/Sub** en la consola.
2. Selecciona el tema `mi-tema`.
3. Haz clic en **"Publicar mensaje"** y escribe un texto, por ejemplo: `Mensaje de prueba`.
4. Verifica los registros:

   * Entra a **Cloud Logging** o a los registros de tu función.
   * Deberías ver una línea como: `Mensaje recibido: Mensaje de prueba`.

---

Ya tienes una integración completa entre **Google Cloud Pub/Sub y Cloud Functions**. Puedes escalar este flujo para enviar eventos entre servicios de forma asincrónica y robusta.

---
