# Entorno de desarrollo Docker para Portal Open Data

Entorno de desarrollo con Docker Compose para portales Open Data basados en Drupal y CKAN.

## Estructura de directorios

* En el directorio `.docker` tenemos todo lo relacionado con Docker, dentro de esta misma encontramos:
  * El archivo default de variables de entorno `dist.env`
  * El YAML de definición de servicios `docker-compose.yml`
  * La carpeta `build` con los Dockerfile custom
  * La carpeta `data` con los volúmenes persistentes
* Dentro del directorio `drupal` estará el proyecto Drupal. Este proyecto lo podemos inicializar desde cero desde el propio Dockerfile, utilizando la variables de entorno como `CREATE_DRUPAL_PROJECT=True`, aunque lo recomendado es inicializarlo desde el exterior clonando el contenido del proyecto en la carpeta `drupal`, teniendo en cuenta que el DOCROOT de Apache apuntará a `drupal/web`.

## Levantar proyecto

Clonar el repositorio:
```sh
git clone git@github.com:managume/opendata-docker.git
````

Configurar variables de entorno:
```sh
cd .docker
cp dist.env .env
```

Levantar proyecto con Docker Compose:
```sh
# Dentro del directorio .docker
docker-compose up -d --build
```