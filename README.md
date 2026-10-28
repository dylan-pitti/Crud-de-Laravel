# GUÍA TÉCNICA — CRUD de Productos en Laravel

> **Proyecto:** `crud_rapido`  
> **Módulo:** Creación de un CRUD para administrar productos  
> **Entorno:** Laravel, PHP, Composer, Node.js/NPM y WampServer  
> **Directorio de trabajo:** `C:\wamp64\www`

---

## 📌 Objetivo

Documentar, en el orden de las capturas, el proceso de creación de un proyecto Laravel llamado `crud_rapido` y la preparación de un CRUD para gestionar productos. La guía explica qué muestra cada captura y los comandos utilizados o visibles en ellas.

## 📜 Índice

1. [Creación del proyecto Laravel](#1-creación-del-proyecto-laravel)
2. [Configuración de AppServiceProvider](#2-configuración-de-appserviceprovider)
3. [Creación del modelo y la migración](#3-creación-del-modelo-y-la-migración)
4. [Definición de la tabla products](#4-definición-de-la-tabla-products)
5. [Generación del CRUD](#5-generación-del-crud)
6. [Migración de la base de datos](#6-migración-de-la-base-de-datos)
7. [Instalación de dependencias y ejecución de Vite](#7-instalación-de-dependencias-y-ejecución-de-vite)
8. [Comprobación de la interfaz](#8-comprobación-de-la-interfaz)
9. [Resumen de comandos](#9-resumen-de-comandos)

---

## 1. Creación del proyecto Laravel

![Captura 1: creación del proyecto](capturas/01_creacion_proyecto.png)

La primera captura muestra la creación del proyecto desde PowerShell, ubicado en la carpeta de proyectos de WampServer:

```powershell
cd C:\wamp64\www
laravel new crud_rapido
```

Durante el asistente de instalación se selecciona **`none`** para el starter kit y **`blade`** como frontend stack. Esto crea el proyecto Laravel sin un kit de autenticación inicial y con vistas Blade.

Después de crearlo, entra en la carpeta del proyecto:

```powershell
cd C:\wamp64\www\crud_rapido
```

## 2. Configuración de AppServiceProvider

![Captura 2: configuración de AppServiceProvider](capturas/02_configuracion_appserviceprovider.png)

En la segunda captura se observa el archivo `app/Providers/AppServiceProvider.php`. En el método `boot()` se configura la longitud predeterminada de los campos de texto para los índices de la base de datos:

```php
use Illuminate\Support\Facades\Schema;

// Dentro de la clase AppServiceProvider:
public function boot(): void
{
    Schema::defaultStringLength(191);
}
```

Esta configuración puede ayudar a evitar problemas de longitud de índices en determinados entornos MySQL. Verifica que la línea `use Illuminate\Support\Facades\Schema;` esté en la parte superior del archivo y que el código se encuentre dentro de la clase.

## 3. Creación del modelo y la migración

![Captura 3: generación del modelo Product y migración](capturas/03_creacion_modelo_migracion.png)

La tercera captura muestra la ejecución del comando:

```powershell
php artisan make:model Product -m
```

La opción `-m` solicita a Laravel que cree, además del modelo `Product`, un archivo de migración asociado. La terminal confirma que se generaron el modelo y la migración.

- **Modelo:** `app/Models/Product.php`
- **Migración:** `database/migrations/` con un nombre que termina en `create_products_table.php`

El modelo representa los productos dentro de la aplicación; la migración define la estructura de su tabla en la base de datos.

## 4. Definición de la tabla products

![Captura 4: estructura de la tabla products](capturas/04_definicion_tabla_products.png)

En la cuarta captura se ve la migración de `products`. Los campos que aparecen definidos son:

| Campo | Tipo / función |
|---|---|
| `id` | Identificador del producto |
| `description` | Descripción del producto, tipo texto |
| `price` | Precio del producto, decimal con 8 dígitos y 2 decimales |
| `stock` | Cantidad disponible, entero |
| `timestamps` | Fechas de creación y actualización |

La estructura visible en la captura corresponde a un esquema similar a este:

```php
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('description');
    $table->double('price', 8, 2);
    $table->integer('stock');
    $table->timestamps();
});
```

Asegúrate de guardar la migración antes de ejecutarla.

## 5. Generación del CRUD

![Captura 5: generación del CRUD de products](capturas/05_generacion_crud_products.png)

La quinta captura muestra el comando:

```powershell
php artisan make:crud products
```

Laravel solicita elegir el stack de interfaz. En la captura está seleccionada la opción **Bootstrap**. Este comando depende de que el generador CRUD correspondiente esté disponible en el proyecto; no es un comando estándar incluido en todas las instalaciones de Laravel.

Si tu terminal no reconoce `make:crud`, revisa qué paquete o extensión proporciona ese comando y sigue las instrucciones de instalación de dicho generador. No continúes suponiendo que el comando existe si Artisan informa que no está definido.

## 6. Migración de la base de datos

![Captura 6: ejecución de las migraciones](capturas/06_migracion_products.png)

La sexta captura muestra una migración ejecutada correctamente:

```powershell
php artisan migrate
```

El mensaje `DONE` indica que Laravel aplicó la migración `create_products_table`. Para que el proceso funcione, la base de datos debe estar creada y los parámetros de conexión del archivo `.env` deben ser correctos.

Revisa como mínimo las variables `DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME` y `DB_PASSWORD`. Los valores exactos dependen de la configuración local de MySQL.

## 7. Instalación de dependencias y ejecución de Vite

![Captura 7: instalación de dependencias y Vite](capturas/07_instalacion_dependencias_vite.png)

La séptima captura muestra la instalación de paquetes de JavaScript y el inicio del servidor de desarrollo:

```powershell
npm install
npm run dev
```

`npm install` instala las dependencias declaradas por el proyecto. `npm run dev` inicia Vite para procesar y servir los recursos del frontend durante el desarrollo. Mantén abierta la terminal mientras trabajas con esos recursos.

## 8. Comprobación de la interfaz

![Captura 8: interfaz del CRUD de productos](capturas/08_interfaz_crud_products.png)

La última captura presenta la interfaz web de productos. Se observa una página con el listado de productos, una acción para crear un producto y controles de acción en la tabla.

Esta vista sirve como comprobación visual de que la interfaz CRUD se está mostrando. Para validar las operaciones completas, prueba en el navegador:

1. Crear un producto con descripción, precio y stock.
2. Confirmar que el registro aparece en el listado.
3. Editar el registro y comprobar que los cambios se guardan.
4. Eliminar un registro de prueba y verificar que desaparece.

La captura por sí sola no demuestra que todas las operaciones estén funcionando; es necesario probarlas en la aplicación.

## 9. Resumen de comandos

Ejecuta los comandos desde PowerShell, según el punto del proceso en el que te encuentres:

```powershell
cd C:\wamp64\www
laravel new crud_rapido
cd C:\wamp64\www\crud_rapido

php artisan make:model Product -m
# Editar la migración de products y AppServiceProvider.php
php artisan make:crud products
php artisan migrate

npm install
npm run dev
```

> **Importante:** `php artisan make:crud products` solo funcionará si está instalado el generador que registra ese comando. La guía describe lo que aparece en las capturas; no incluye archivos de código que no se muestran en ellas.

---

## ✅ Resultado esperado

Al completar el procedimiento, el proyecto `crud_rapido` debe contar con el modelo `Product`, una migración para la tabla `products`, los recursos generados por el CRUD y una interfaz para administrar productos. La base de datos debe reflejar la tabla migrada y Vite debe poder ejecutar los recursos frontend.

## 🖼️ Índice de capturas

Las imágenes incluidas en la carpeta `capturas/` corresponden a las ocho capturas del documento de Word, renombradas para que sea más sencillo identificar cada etapa.
