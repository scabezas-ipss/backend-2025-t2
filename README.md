# backend-2025-t2
Clase repaso antes de la evaluación 3 y exámen del curso

# Requerimientos

# R01
Accesos directos a información municipal permanente (Deptos. Municipales, contactos, servicios, transparencia, números de emergencia)

# R02
Botones permanentes a redes sociales (Instagram, Facebook, youtube)

# R03
Banner inicial personalizable para destacar eventos, servicios temporales, etc.

# R04
Enlazar un reproductor de radio isla dentro de la página (https://radioisla.cl/)

# R05
Formulario de contacto para consultas, que se enlace a correo municipal.

# R06
Sección Marca Destino. Debería contener un pdf descargable con el manual y aparte un kit (archivo comprimido) para descargar también.

# Wordpress consumo de Endpoint

Sí, se puede. Para consumir un endpoint JSON y cargar el contenido en una página de WordPress, tenés que utilizar código personalizado. No existe una función nativa en WordPress que te permita hacer esto directamente sin programación.

-----

## Opciones para lograrlo

Existen varias formas de conseguirlo, dependiendo del nivel de complejidad y de dónde querés mostrar el contenido.

### 1\. Usando el archivo `functions.php` del tema

Esta es la forma más común. Podés crear una función que consuma el endpoint y luego usarla para mostrar el contenido.

**Pasos:**

  * **Crear una función para consumir el endpoint:** Utilizá la función de WordPress `wp_remote_get()` para hacer la solicitud HTTP al endpoint. `wp_remote_get()` es la forma segura y recomendada de hacer peticiones externas en WordPress.
  * **Decodificar el JSON:** Una vez que obtengas la respuesta, usá `json_decode()` para convertir el string JSON en un objeto o array de PHP.
  * **Mostrar el contenido:** Utilizá la función que creaste dentro de un shortcode o directamente en una plantilla de página para mostrar el contenido.

**Ejemplo de código simple para `functions.php`:**

```php
function obtener_datos_endpoint() {
    $response = wp_remote_get('URL_DE_TU_ENDPOINT');
    if (is_wp_error($response)) {
        return 'Error al obtener los datos.';
    }
    $body = wp_remote_retrieve_body($response);
    $datos = json_decode($body, true);
    if ($datos) {
        $html = '<ul>';
        foreach ($datos as $item) {
            $html .= '<li>' . esc_html($item['titulo']) . '</li>';
        }
        $html .= '</ul>';
        return $html;
    }
    return 'No se encontraron datos.';
}
add_shortcode('mostrar_contenido_json', 'obtener_datos_endpoint');
```

Luego, en cualquier página o entrada, podés usar el shortcode `[mostrar_contenido_json]` para mostrar la lista.

### 2\. Usando JavaScript y la API de WordPress REST

Esta opción es ideal si querés cargar el contenido de forma dinámica después de que la página haya cargado, o si preferís trabajar con **JavaScript**.

**Pasos:**

  * **Crear una plantilla o bloque personalizado:** Podés crear un bloque de Gutenberg personalizado o una plantilla de página específica.
  * **Usar la API `fetch` de JavaScript:** Con la API `fetch`, hacé una solicitud al endpoint desde el lado del cliente (el navegador del usuario).
  * **Manipular el DOM:** Una vez que recibas los datos, usá JavaScript para crear elementos HTML y agregarlos al DOM de tu página.

**Ejemplo de código JavaScript:**

```javascript
document.addEventListener('DOMContentLoaded', function() {
    fetch('URL_DE_TU_ENDPOINT')
        .then(response => response.json())
        .then(data => {
            const contenedor = document.getElementById('contenedor-json');
            let html = '<ul>';
            data.forEach(item => {
                html += `<li>${item.titulo}</li>`;
            });
            html += '</ul>';
            contenedor.innerHTML = html;
        })
        .catch(error => {
            console.error('Error:', error);
        });
});
```

En tu plantilla de WordPress, solo necesitás un `div` con el ID `contenedor-json` para que el script inyecte el contenido.

### 3\. Usando un plugin

Si no querés programar, podés buscar un plugin que te permita consumir endpoints y mostrar el contenido. Sin embargo, puede ser difícil encontrar uno que se ajuste perfectamente a tus necesidades sin requerir una configuración compleja. Algunos plugins de formularios o de importación de datos pueden tener esta funcionalidad. Una búsqueda en el repositorio de WordPress por "consume json", "json importer" o "fetch json" podría darte algunas opciones.

-----

## Consideraciones importantes

  * **Seguridad:** Siempre validá y sanitizá los datos que recibas del endpoint para prevenir vulnerabilidades XSS.
  * **Rendimiento:** Cargar datos de un endpoint externo puede ralentizar la carga de tu página. Considerá usar un **sistema de caché** (como la API de transitorios de WordPress) para almacenar los datos temporalmente y evitar hacer una nueva solicitud a cada carga de página.
  * **Mantenimiento:** Si la estructura del JSON cambia, tendrás que actualizar tu código para que siga funcionando correctamente.