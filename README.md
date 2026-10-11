# php-project-s3

Versión estática del proyecto original `marmeow/php-project` adaptada para desplegarse en un bucket de Amazon S3.

Objetivo:
- eliminar la dependencia de PHP en el frontend
- dejar un sitio totalmente estático
- preparar el proyecto para subirlo directamente a un bucket S3 con hosting estático
- mantener la lógica de base de datos fuera del navegador

## Qué se ha convertido

Se ha creado una versión estática con:
- landing page principal
- sección de ofertas
- listado de productos renderizado con JavaScript
- contacto y estructura visual del proyecto original

## Importante sobre la base de datos

En una web alojada en S3 no se debe guardar ni exponer información sensible de base de datos en el frontend.

Si necesitas variables de base de datos, deben ir en un backend separado (por ejemplo, API Gateway + Lambda, ECS, EC2 o una función serverless), no en `index.html`, `script.js` ni en archivos públicos del bucket.

Regla recomendada:
- S3: solo sirve archivos estáticos
- Base de datos: siempre en backend privado

## Estructura del proyecto

```text
.
├── index.html
├── 404.html
├── styles.css
├── script.js
├── README.md
```

## Despliegue en S3

### Opción 1: AWS Console
1. Crear un bucket S3
2. Habilitar "Static website hosting"
3. Configurar el nombre del bucket igual al dominio o utilizar un dominio alternativo
4. Subir todos los archivos del proyecto a la raíz del bucket
5. Establecer permisos públicos de lectura del bucket
6. Visitar la URL pública del bucket o la del CloudFront si se usa

### Opción 2: AWS CLI

```bash
aws s3 mb s3://mi-bucket-estatico --region eu-west-1
aws s3 cp . s3://mi-bucket-estatico --recursive --acl public-read
```

### Política recomendada del bucket

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mi-bucket-estatico/*"
    }
  ]
}
```

## Recomendación adicional

Para producción lo ideal es:
- S3 para servir el contenido estático
- CloudFront para caché y HTTPS
- backend separado para cualquier dato dinámico o base de datos

## Nota

Esta es una versión estática de demostración, pensada para alojamiento ligero y simple en S3. Si quieres, en el siguiente paso puedo convertir también la parte de productos a un formato más parecido al original con más páginas, carrito y datos en JSON local.

