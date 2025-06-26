## Copia de objetos S3 con una función Lambda

Este laboratorio te guiará a través de la creación y el uso de un servicio sin servidor de AWS, AWS Lambda, para copiar objetos de un bucket S3 de origen a uno de destino cuando se cargan nuevos objetos.

### Detalles de la Tarea

1. Iniciar sesión en la Consola de administración de AWS.
2. Crear dos buckets S3: uno para el origen y otro para el destino.
3. Crear una función Lambda para copiar objetos entre los buckets.
4. Probar la función Lambda.

---

### Paso 1: Crear buckets S3

Necesitarás crear dos buckets S3: uno que actuará como bucket de origen y otro como bucket de destino.

#### 1.1 Crear el bucket de origen (`your_source_bucket_name`)

* Inicia sesión en la Consola de administración de AWS.
* Navega a los servicios de S3.
* Haz clic en "Create bucket" (Crear bucket).
* **Nombre del bucket:** Ingresa un nombre único globalmente (ej. `your_source_bucket_name`).
* **Región:** Selecciona la región deseada (ej. `US East (N. Virginia)`).
* Deja las demás configuraciones por defecto y haz clic en "Create bucket".
* Una vez creado el bucket, selecciónalo y copia su ARN. Guárdalo para usarlo más tarde.

#### 1.2 Crear el bucket de destino (`your_destination_bucket_name`)

* En la Consola de S3, haz clic nuevamente en "Create bucket".
* **Nombre del bucket:** Ingresa un nombre único globalmente (ej. `your_destination_bucket_name`).
* **Región:** Selecciona la misma región que el bucket de origen.
* Deja las demás configuraciones por defecto y haz clic en "Create bucket".
* Una vez creado el bucket, selecciónalo y copia su ARN. Guárdalo para usarlo más tarde.

---

### Paso 2: Crear una política de IAM

Para que tu función Lambda pueda interactuar con S3, necesitarás crear una política personalizada con permisos adecuados.

#### 2.1 Crear la política de IAM

* Navega a los servicios de IAM.
* En el menú izquierdo, haz clic en "Policies" (Políticas).
* Haz clic en "Create policy".
* Ve a la pestaña **JSON** y pega la siguiente política:

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action":[
        "s3:GetObject"
      ],
      "Resource":[
        "arn:aws:s3:::your_source_bucket_name/*"
      ]
    },
    {
      "Effect":"Allow",
      "Action":[
        "s3:PutObject"
      ],
      "Resource":[
        "arn:aws:s3:::your_destination_bucket_name/*"
      ]
    }
  ]
}
```

> **Importante:** Reemplaza `your_source_bucket_name` y `your_destination_bucket_name` con los nombres exactos de tus buckets.

* Haz clic en "Review policy".
* **Nombre de la política:** `mypolicy`
* Haz clic en "Create policy".

---

### Paso 3: Crear un rol de IAM

Ahora crearás un rol de IAM para que Lambda pueda asumirlo y usar la política anterior.

#### 3.1 Crear el rol

* En el menú de IAM, haz clic en "Roles".
* Haz clic en "Create role".
* **Tipo de entidad confiable:** Selecciona "AWS service" > "Lambda".
* Haz clic en "Next: Permissions".
* Busca y selecciona la política `mypolicy`.
* Haz clic en "Next: Tags" (puedes omitir las etiquetas o usar por ejemplo: `Name: myrole`).
* Haz clic en "Next: Review".
* **Nombre del rol:** `myrole`
* Haz clic en "Create role".

---

### Paso 4: Crear una función Lambda

#### 4.1 Crear la función

* Navega al servicio AWS Lambda.
* Haz clic en "Create function".
* Selecciona "Author from scratch".
* **Nombre de la función:** `mylambdafunction`
* **Runtime:** `Node.js 12.x` o superior.
* **Permisos:** Selecciona "Use an existing role" y elige `myrole`.
* Haz clic en "Create function".

#### 4.2 Agregar el código

* En la página de la función, baja a "Function code".
* Reemplaza el contenido del archivo `index.js` por:

```javascript
var AWS = require("aws-sdk");
exports.handler = (event, context, callback) => {
  var s3 = new AWS.S3();
  var sourceBucket = "your_source_bucket_name";
  var destinationBucket = "your_destination_bucket_name";
  var objectKey = event.Records[0].s3.object.key;
  var copySource = encodeURI(sourceBucket + "/" + objectKey);

  var copyParams = {
    Bucket: destinationBucket,
    CopySource: copySource,
    Key: objectKey
  };

  s3.copyObject(copyParams, function(err, data) {
    if (err) {
      console.log(err, err.stack);
    } else {
      console.log("S3 object copy successful.");
    }
  });
};
```

> **Importante:** Sustituye los nombres de bucket correctamente.
> Luego, haz clic en **Deploy**.

---

### Paso 5: Agregar un disparador

#### 5.1 Crear el disparador S3

* En el "Designer", haz clic en **+ Add trigger**.
* Selecciona **S3**.
* **Bucket:** selecciona `your_source_bucket_name`.
* **Tipo de evento:** `All object create events`.
* Marca "Recursive invocation".
* Haz clic en **Add**.

---

### Paso 6: Probar la función

#### 6.1 Subir una imagen al bucket de origen

* Prepara una imagen localmente.
* Ve al bucket `your_source_bucket_name` en S3.
* Haz clic en "Upload".
* Selecciona el archivo y haz clic en "Upload".

#### 6.2 Verificar la copia

* Ve a tu bucket `your_destination_bucket_name`.
* Verifica que la imagen fue copiada correctamente.


