# RPA_Garantias_ECommerce

Proceso UiPath para consolidar y validar solicitudes de garantía de comercio electrónico.

## Flujo

1. Lee solicitudes desde Google Sheets.
2. Filtra filas pendientes según `Estado_Bot`.
3. Consulta PostgreSQL para validar factura, producto y fecha de compra.
4. Calcula el estado y porcentaje de reembolso.
5. Actualiza las columnas de resultado en Google Sheets.
6. Envía una notificación por correo al destinatario de la solicitud, excepto cuando el estado sea `ERROR_SISTEMA`.
7. Registra `No hay solicitudes pendientes` cuando no existen filas para procesar.

## Requisitos

- UiPath Studio para proyectos Windows.
- UiPath.System.Activities.
- UiPath.Database.Activities.
- UiPath.GSuite.Activities.
- UiPath.Mail.Activities.
- Driver PostgreSQL ODBC de 64 bits.
- PostgreSQL accesible desde el robot.
- Activos de credenciales en Orchestrator:
  - `Credential_PostgresERP`
  - `Credential_Gmail`

## Configuración local

1. Copiar `Data/Config.template.json` como `Data/Config.json`.
2. Completar los valores reales de Google Sheets y PostgreSQL en `Data/Config.json`.
3. No publicar `Data/Config.json`.
4. En UiPath Studio, volver a seleccionar la conexión de Google Sheets para `ReadQueueSheets.xaml` y `UpdateAndNotify.xaml`.
5. Configurar los activos de credenciales en la carpeta de Orchestrator que utilizará el robot.
6. Revisar la configuración SMTP antes de ejecutar el proceso.

## Ejecución

El punto de entrada es `Main.xaml`. El proyecto no incluye un trigger local; puede ejecutarse manualmente desde Studio/Assistant o mediante un proceso y trigger configurados en UiPath Orchestrator.

## Seguridad

- No se incluyen credenciales, contraseñas ni `Data/Config.json`.
- Los identificadores de Google Sheets, conexiones y carpetas de Orchestrator del entorno original se sustituyeron por placeholders.
- Verifica los permisos del repositorio antes de publicar cambios adicionales.

## Estructura principal

- `Main.xaml`: orquestación principal.
- `ReadQueueSheets.xaml`: lectura y filtro de solicitudes.
- `ProccessTransactionSQL.xaml`: validación contra PostgreSQL.
- `UpdateAndNotify.xaml`: actualización de Sheets y notificación.
- `InitAllSettings.xaml`: carga de configuración y credenciales.
- `Data/Config.template.json`: plantilla de configuración local.
