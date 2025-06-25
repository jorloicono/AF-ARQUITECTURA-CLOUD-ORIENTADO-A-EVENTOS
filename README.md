# AF-ARQUITECTURA-CLOUD-ORIENTADO-A-EVENTOS

## Módulo 1. Introducción a la Computación en la Nube y a los Sistemas Orientados a Eventos

- **Concepto de computación en la nube:**
  - Qué es y cómo funciona la nube
  - Comparación entre infraestructura local y cloud
  - Ventajas del cloud: escalabilidad, elasticidad, pago por uso, disponibilidad

- **Modelos de servicios cloud:**
  - IaaS (Infraestructura como servicio): ejemplos y casos de uso
  - PaaS (Plataforma como servicio): simplificación del desarrollo
  - SaaS (Software como servicio): software listo para usar

- **Modelos de despliegue:**
  - Nube pública, privada e híbrida: definición, diferencias y escenarios típicos

- **Arquitecturas orientadas a eventos:**
  - Introducción conceptual
  - Contraste con arquitectura monolítica o basada en llamadas directas (sincronía vs asincronía)
  - Situaciones donde tiene sentido usar eventos (alta concurrencia, desacoplamiento, tiempo real)

- **Ejemplos reales del uso de eventos:**
  - Compras online (pedido → evento → preparación → envío)
  - Dispositivos IoT (sensor → evento → procesamiento → alerta)
  - Aplicaciones móviles con notificaciones push

---

## Módulo 2. Fundamentos de Eventos y Mensajería en Cloud

- **Qué es un evento:**
  - Definición
  - Estructura de un evento (cabecera + cuerpo)
  - Datos típicos transportados (identificadores, marcas de tiempo, tipos de evento)

- **Roles en un sistema orientado a eventos:**
  - Productor de eventos: qué lo genera
  - Consumidor de eventos: qué los escucha y actúa
  - Broker o intermediario: canal de distribución del evento

- **Modelos de mensajería asincrónica:**
  - Envío directo (point-to-point)
  - Publicación y suscripción (publish-subscribe): varios consumidores, suscripciones temáticas
  - Diferencias clave entre modelos
  - Ventajas del desacoplamiento temporal

- **Formato de eventos:**
  - Uso de JSON y XML
  - Estructuras simples, jerárquicas y ejemplos reales
  - Consideraciones para el diseño de eventos comprensibles y reutilizables

---

## Módulo 3. Servicios Cloud para Arquitecturas Orientadas a Eventos

- **Servicios en AWS, Azure y Google Cloud:**

  - **AWS:**
    - SNS (notificaciones simples)
    - SQS (colas)
    - EventBridge (orquestador de eventos)

  - **Azure:**
    - Event Grid (distribución de eventos)
    - Service Bus (mensajería empresarial)

  - **Google Cloud:**
    - Pub/Sub (modelo publish-subscribe escalable)

- **Conceptos comunes entre proveedores:**
  - Tópicos, colas y suscripciones
  - Eventos estructurados vs no estructurados
  - Limitaciones y ventajas técnicas de cada enfoque

- **Escenarios de uso con cada servicio:**
  - Procesamiento de órdenes, actualizaciones de stock, logs en tiempo real, alertas de seguridad

- **Diferencias clave entre proveedores:**
  - Costes aproximados y criterios para elegir
  - Integraciones disponibles (cloud-native y externas)
  - Facilidad de uso e interfaces de gestión

---

## Módulo 4. Diseño de Arquitectura Cloud Basada en Eventos

- **Principios de diseño de arquitectura orientada a eventos:**
  - Desacoplamiento entre servicios
  - Persistencia e idempotencia
  - Enrutamiento basado en eventos
  - Reintentos y tolerancia a fallos

- **Patrones comunes:**
  - Event Notification (avisar y olvidar)
  - Event-Carried State Transfer (el evento contiene todo lo necesario)
  - Event Sourcing (el sistema guarda todos los eventos para reconstruir el estado)

- **Relación con microservicios:**
  - Cómo los microservicios se comunican mediante eventos
  - Beneficios frente a la comunicación directa (REST/API)

- **Diseño de flujos de eventos:**
  - Identificación de eventos clave en un proceso
  - Determinación de emisores y receptores
  - Trazabilidad y monitorización de eventos

- **Gobernanza y seguridad:**
  - Autenticación/autorización de emisores y receptores
  - Auditoría de eventos
  - Riesgos de duplicación, pérdida de eventos o malformación

---

## Módulo 5. Automatización de Procesos con Eventos y Funciones Serverless

- **Introducción al concepto de automatización basada en eventos:**
  - Por qué automatizar
  - Tipos de tareas comunes que se benefician de esta automatización

- **Triggers y funciones serverless:**
  - Qué son los triggers (activadores)
  - Cuándo usar una función sin servidor
  - Ventajas del modelo serverless: bajo coste, escalado automático, mantenimiento mínimo

- **Proveedores y ejemplos:**
  - AWS Lambda: responder a un evento de S3 o SNS
  - Azure Functions: procesar eventos de Event Grid
  - Google Cloud Functions: acciones tras Pub/Sub o subida de archivos

- **Integración en arquitecturas:**
  - Cómo una función puede actuar como consumidor de eventos
  - Encadenamiento de eventos con lógica de negocio
  - Buenas prácticas para definir funciones pequeñas y reutilizables

- **Consideraciones de diseño:**
  - Tiempo de ejecución, límites y costes
  - Logs y trazabilidad
  - Gestión de errores y reintentos
