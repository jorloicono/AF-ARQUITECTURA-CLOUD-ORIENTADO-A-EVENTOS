# Enviar correos electrónicos con Amazon EventBridge cuando se crean objetos en S3

Este tutorial te guía paso a paso para recibir un **correo electrónico automáticamente** cuando se cree un nuevo objeto en un bucket de **Amazon S3**, utilizando **Amazon EventBridge** y **Amazon SNS**.

---

## Requisitos Previos

Antes de comenzar, asegúrate de tener lo siguiente:

- Una cuenta activa de **AWS**.
- Permisos para gestionar **S3**, **SNS** y **EventBridge**.
- Un bucket de S3 existente (o crea uno).
- Habilitada la integración de **Amazon S3 con EventBridge** (ver paso siguiente).

---

## Habilitar Amazon EventBridge en un bucket de S3

> Por defecto, los buckets no envían eventos a EventBridge hasta que tú lo activas.

### Pasos:

1. Abre la consola de Amazon S3:  
   [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/)

2. Selecciona el bucket donde deseas recibir eventos.

3. Ve a la pestaña **Propiedades**.

4. Desplázate hasta **Notificaciones de eventos**.

5. En la sección **Eventos de Amazon EventBridge**, haz clic en **Editar**.

6. Marca la opción:  
   **Habilitar eventos de Amazon EventBridge para este bucket**

7. Haz clic en **Guardar cambios**.

---

## Pasos del tutorial

### Paso 1: Crear un tema de Amazon SNS

Este tema será el canal donde se publicarán los eventos que se enviarán por correo.

1. Abre la consola de Amazon SNS:  
   [https://console.aws.amazon.com/sns/](https://console.aws.amazon.com/sns/)

2. En el menú lateral, haz clic en **Temas**.

3. Haz clic en **Crear tema**.

4. En **Tipo de tema**, elige `Estándar`.

5. Asigna el nombre: `eventbridge-test`.

6. Haz clic en **Crear tema**.

---

### Paso 2: Crear una suscripción de Amazon SNS

1. En la misma consola de SNS, ve a **Suscripciones**.

2. Haz clic en **Crear suscripción**.

3. En **ARN del tema**, selecciona el tema `eventbridge-test`.

4. En **Protocolo**, elige `Correo electrónico`.

5. En **Punto de conexión**, introduce tu dirección de correo electrónico.

6. Haz clic en **Crear suscripción**.

7. Revisa tu correo electrónico y **confirma la suscripción** mediante el enlace que recibirás de AWS.

---

### Paso 3: Crear una regla en Amazon EventBridge

Esta regla enviará eventos de tipo `Object Created` de S3 al tema SNS.

1. Abre la consola de Amazon EventBridge:  
   [https://console.aws.amazon.com/events/](https://console.aws.amazon.com/events/)

2. En el menú lateral, haz clic en **Reglas** > **Crear regla**.

3. Nombra la regla, por ejemplo: `s3-test`.

4. En **Bus de eventos**, selecciona `default`.

5. En **Tipo de regla**, selecciona: `Regla con un patrón de evento`.

6. Haz clic en **Siguiente**.

7. En **Fuente del evento**, elige `Servicios de AWS`.

8. En **Servicio de AWS**, selecciona `Simple Storage Service (S3)`.

9. En **Tipo de evento**, elige `Amazon S3 Event Notification`.

10. En **Eventos específicos**, selecciona `Object Created`.

11. (Opcional) Especifica el bucket.

12. Haz clic en **Siguiente**.

13. En **Tipo de destino**, elige `Servicio de AWS`.

14. En **Seleccionar destino**, elige `Tema de SNS`.

15. Selecciona el tema `eventbridge-test`.

16. Haz clic en **Siguiente**, revisa los detalles y finalmente en **Crear regla**.

---

### Paso 4: Probar la regla

1. Ve al bucket de S3 para el que configuraste la integración.

2. Sube cualquier archivo (puede ser un `.txt` simple).

3. Espera unos minutos.

4. Revisa tu correo: deberías recibir una notificación del evento.

---

### Paso 5: Eliminar recursos (opcional)

Si solo estabas haciendo pruebas, puedes eliminar los recursos para evitar cargos.

#### Eliminar el tema SNS:

1. Ve a **SNS > Temas**.

2. Selecciona `eventbridge-test`.

3. Haz clic en **Eliminar** > Escribe `delete me` > Confirmar.

#### Eliminar la suscripción:

1. Ve a **SNS > Suscripciones**.

2. Selecciona la suscripción creada.

3. Haz clic en **Eliminar**.

#### Eliminar la regla de EventBridge:

1. Ve a **EventBridge > Reglas**.

2. Selecciona `s3-test`.

3. Haz clic en **Eliminar**.

---

## Resumen

| Recurso AWS        | Propósito                                  |
|--------------------|---------------------------------------------|
| Amazon S3          | Fuente del evento (archivo creado)         |
| Amazon EventBridge | Detecta el evento y lo dirige al destino   |
| Amazon SNS         | Envía el correo electrónico                |
| Correo electrónico | Recibe notificación del evento             |

---

## Recursos adicionales

- [Documentación de SNS](https://docs.aws.amazon.com/sns/)
- [Documentación de EventBridge](https://docs.aws.amazon.com/eventbridge/)
- [Documentación de S3](https://docs.aws.amazon.com/s3/)


