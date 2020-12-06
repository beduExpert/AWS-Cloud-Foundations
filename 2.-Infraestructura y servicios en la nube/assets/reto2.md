# Sesión 2 - Reto 2


# 1. Objetivo 🎯
- Explorar las opciones avanzadas de AWS CLI con S3

# 2. Requisitos 📌
- AWS CLI instalado y configurado.
- Tener presente la documentación de [AWS CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html) como referencia.
- Un bucket con archivos precargados.
- Un bucket vacío.

# 3. Desarrollo  📑
Se desea copiar el contenido de un bucket a otro bucket en la misma cuenta de AWS, una forma es descargar archivos a local y luego subirlos al nuevo bucket.

¿Qué comando o comandos sería el usado para realizar dicha acción?

No es óptimo en costos  y tiempo seguir ese esquema, al descargar los datos a local genera consto de transferencia, sin contar que el ancho de banda hacia local es menor, una mejor opción es transferir los archivos de bucket a bucket, con lo que no habrá costos de transferencia de datos y será mucho mas rápido pues AWS tiene conexiones de baja latencia.
¿Qué comando se requiere ejecutar para copiar todos los archivos de un bucket a otro?

Una vez copiados los archivos:
¿Con qué comando se puede eliminar el bucket de origen?