# Crear, Consultar y Eliminar una Tabla NoSQL en Amazon DynamoDB

Esta guía te ayudará a trabajar con **Amazon DynamoDB**, un servicio de base de datos NoSQL totalmente administrado que ofrece un rendimiento rápido y predecible con escalado automático.

---

## Crear una Tabla NoSQL en DynamoDB

1. Accede a la [Consola de administración de AWS](https://aws.amazon.com/console/), busca "DynamoDB" y abre la consola correspondiente.
2. Haz clic en **"Crear tabla"**.
3. En **Nombre de la tabla**, escribe `Música`. Este será el contenedor de tus registros.
4. Configura las claves primarias:

   * **Clave de partición**: `Artista`

     * Se utiliza para distribuir los datos entre particiones de forma escalable.
     * Es recomendable que tenga valores diversos y distribuidos uniformemente.
   * **Clave de ordenación**: `Título de la canción`

     * Permite almacenar múltiples canciones por artista y facilita las consultas ordenadas.
5. Habilita el **escalado automático** seleccionando **"Personalizar la configuración"**:

   * Esto ajustará automáticamente la capacidad de lectura y escritura según la carga.
   * Si es la primera vez que habilitas esta función, se creará el rol `DynamoDBAutoscaleRole` en IAM.
6. Haz clic en **"Crear tabla"**. Cuando la tabla esté lista, aparecerá listada con una casilla de selección.

---

## Añadir Datos a la Tabla

1. En el panel izquierdo de DynamoDB, selecciona **"Explorar elementos"**.
2. Selecciona la tabla `Música` y haz clic en **"Crear elemento"**.
3. Introduce los siguientes valores:

   * **Artista**: `No One You Know`
   * **Título de la canción**: `Call Me Today`
4. Haz clic en **"Crear elemento"**.
5. Repite el proceso para añadir más canciones:

   | Artista         | Título de la canción    |
   | --------------- | ----------------------- |
   | No One You Know | My Dog Spot             |
   | No One You Know | Somewhere Down The Road |
   | The Acme Band   | Still in Love           |
   | The Acme Band   | Look Out, World         |

---

## Consultar Datos

DynamoDB permite dos métodos para recuperar datos:

* **Consulta**: Filtra por claves primarias, más eficiente.
* **Escaneo**: Recorre toda la tabla (menos eficiente).

### Realizar una Consulta:

1. Haz clic en **"Escanear/consultar elementos"** y selecciona **"Consulta"**.

2. Consulta por artista:

   * **Artista**: `No One You Know`
   * Clic en **"Ejecutar"** para ver todas sus canciones.

3. Otra consulta:

   * **Artista**: `The Acme Band`
   * Clic en **"Ejecutar"** para ver sus canciones.

4. Consulta más específica:

   * **Artista**: `The Acme Band`
   * **Título de la canción**: seleccionar **"Empieza por"**, ingresar `S`
   * Clic en **"Ejecutar"**
   * Resultado: `Still in Love`

---

## Eliminar un Elemento

1. Cambia la vista de **"Consulta"** a **"Escaneo"**.
2. Busca y selecciona el elemento deseado (por ejemplo, `The Acme Band - Look Out, World`).
3. Haz clic en **"Acciones" > "Eliminar elementos"**.
4. Confirma la eliminación haciendo clic en **"Eliminar"**.

---

## Eliminar la Tabla

1. En la consola de DynamoDB, selecciona la tabla `Música`.
2. Haz clic en **"Eliminar"**.
3. En la ventana emergente, escribe `eliminar` para confirmar.
4. Haz clic en **"Eliminar tabla"**.

> **Recomendación**: Elimina las tablas que ya no uses para evitar cargos innecesarios.

---
