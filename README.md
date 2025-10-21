# Práctica 2: Sociograma DAW (MP07)

Este proyecto es una aplicación web sencilla. El objetivo es simular un sociograma para una clase de DAW, recogiendo las preferencias de colaboración de los alumnos a través de un formulario web.

Implemento un backend básico en PHP que recibe datos, los valida en el servidor y los persiste en un archivo JSON, que actúa como una base de datos ligera.

## ⚙️ Cómo Funciona

1.  **Formulario (`index.php`)**: Es el punto de entrada. Carga la cabecera, el formulario (`_form.php`) y el pie de página. El formulario está preparado para la "rehidratación", es decir, para mostrar datos antiguos y errores si los hay.
2.  **Envío (`POST`)**: Cuando el usuario pulsa "Enviar", el navegador empaqueta los datos y los envía por `POST` al archivo `process.php`.
3.  **Procesador (`process.php`)**: Es el cerebro de la aplicación.
    * Recibe y limpia los datos de `$_POST`.
    * Valida cada campo obligatorio (nombre, grupo, preferencias, etc.).
4.  **La Decisión (Dos Caminos)**:
    * **A) Error de Validación**: Si falta un campo o es incorrecto, `process.php` **no guarda nada**. En su lugar, define las variables `$errors` (con mensajes de error) y `$old` (con los datos que el usuario escribió) y vuelve a incluir el `_form.php`. El formulario usa estas variables para mostrar los errores en rojo y rellenar los campos que ya estaban correctos.
    * **B) Éxito**: Si todos los datos son correctos, `process.php` lee el `data/sociograma.json` (con `load_json()`), añade la new respuesta al array, y lo vuelve a guardar (con `save_json()`). Finalmente, muestra una página de confirmación.
5.  **Almacén (`data/sociograma.json`)**: Actúa como nuestra base de datos. Es un simple archivo de texto que almacena un gran array con todas las respuestas.
6.  **API de Lectura (`api.php`)**: Es un *endpoint* de solo lectura que devuelve el contenido de `sociograma.json`. Es muy útil para depurar y probar con Postman.

## 🛠️ Herramientas Utilizadas

* **PHP**: Lenguaje del *backend* para toda la lógica de servidor (validación, lectura/escritura de archivos).
* **HTML**: Maquetación del formulario (`_form.php`, `header.php`, `footer.php`).
* **CSS**: Estilos para el diseño y la usabilidad (`assets/css/styles.css`).
* **JSON**: Formato de persistencia de datos (nuestra "base de datos" en `data/sociograma.json`).
* **XAMPP (Apache)**: Entorno de servidor local para ejecutar PHP.
* **Postman**: (Opcional) Herramienta para probar los *endpoints* (`api.php` y `process.php`) sin usar el navegador.

## 🚀 Cómo Empezar (con XAMPP)

1.  **Iniciar XAMPP**: Abre el XAMPP Control Panel e inicia el módulo **Apache**.
2.  **Buscar `htdocs`**: En el panel de XAMPP, haz clic en "Explorer". Navega hasta la carpeta `htdocs` (normalmente en `C:\xampp\htdocs\`).
3.  **Copiar Proyecto**: Copia esta carpeta (`sociograma`) completa y pégala **dentro** de `htdocs`.
4.  **Crear el JSON**: Asegúrate de que la carpeta `data/` exista y contenga el archivo `sociograma.json`. Este archivo debe contener un array vacío para empezar: `[]`.
5.  **Abrir en Navegador**: Abre tu navegador y ve a la siguiente URL:
    ```
    http://localhost/sociograma/
    ```
    Apache cargará automáticamente `index.php` y verás el formulario.

## 🧪 Pruebas y Endpoints

Puedes probar los *endpoints* directamente con Postman:

### Leer todas las respuestas

* **Método**: `GET`
* **URL**: `http://localhost/sociograma/api.php`
* **Descripción**: Muestra un JSON con el contador (`count`) y la lista (`items`) de todas las respuestas guardadas.

### Enviar una nueva respuesta

* **Método**: `POST`
* **URL**: `http://localhost/sociograma/process.php`
* **Body**: Selecciona `form-data`
* **Descripción**: Simula el envío del formulario. Añade en "KEY" los `name` de los inputs (ej. `nombre`, `grupo`, `preferido_1`) y en "VALUE" los datos que quieras enviar.
