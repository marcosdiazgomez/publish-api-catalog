# API de Aseguradora - Usuarios

**Versión:** 1.0.1

Una API para consultar la información de los usuarios (asegurados) de una compañía de seguros.

* **Contacto:** [Equipo de Desarrollo API](https://api.ejemploaseguradora.com/support) (devteam@ejemploaseguradora.com)
* **Licencia:** [Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)

---

## Servidores

La API está disponible en los siguientes entornos:

* **Servidor de Producción**: `https://api.ejemploaseguradora.com/v1`
* **Servidor de Pruebas (Sandbox)**: `https://sandbox-api.ejemploaseguradora.com/v1`

---

## Endpoints

### **`GET /usuarios`**

**Resumen:** Consultar lista de usuarios asegurados.

Obtiene una lista de todos los usuarios asegurados. Permite filtrar los resultados por nombre y/o número de póliza para refinar la búsqueda.

#### Parámetros de Consulta

| Nombre | En | Descripción | Requerido | Tipo | Por Defecto |
| --- | --- | --- | --- | --- | --- |
| `nombre` | `query` | Filtrar la lista de usuarios por su nombre. La búsqueda no distingue mayúsculas de minúsculas y busca coincidencias parciales. | No | `string` | N/A |
| `numero_poliza` | `query` | Filtrar usuarios que tengan una póliza específica activa. | No | `string` | N/A |
| `limit` | `query` | Número máximo de usuarios a devolver (Mín: 1, Máx: 100). | No | `integer` | 20 |
| `offset` | `query` | Número de usuarios a omitir para la paginación (Mín: 0). | No | `integer` | 0 |

#### Respuestas

<details>
<summary><strong><code>200 OK</code></strong> - Búsqueda exitosa. Devuelve una lista de usuarios.</summary>

**Cuerpo de la Respuesta (`application/json`)**
```json
[
  {
    "idUsuario": "123e4567-e89b-12d3-a456-426614174000",
    "nombreCompleto": "Ana Sofía García López",
    "fechaNacimiento": "1985-05-20",
    "email": "ana.garcia@email.com",
    "telefono": "+52 55 1234 5678",
    "direccion": {
      "calle": "Av. de la Reforma 222",
      "ciudad": "Ciudad de México",
      "estado": "CDMX",
      "codigoPostal": "06600",
      "pais": "México"
    },
    "polizas": [
      {
        "numeroPoliza": "POL-987654321",
        "tipo": "Gastos Medicos",
        "estado": "Activa"
      },
      {
        "numeroPoliza": "POL-112233445",
        "tipo": "Auto",
        "estado": "Activa"
      }
    ]
  }
]
```
</details>

<details>
<summary><strong><code>400 Bad Request</code></strong> - Solicitud incorrecta debido a parámetros inválidos.</summary>

**Cuerpo de la Respuesta (`application/json`)**
```json
{
  "codigo": "INVALID_PARAMETER",
  "mensaje": "El parámetro 'limit' excede el valor máximo permitido de 100.",
  "detalles": "El valor proporcionado fue 150."
}
```
</details>

<details>
<summary><strong><code>404 Not Found</code></strong> - No se encontraron usuarios que coincidan con los criterios de búsqueda.</summary>

**Cuerpo de la Respuesta (`application/json`)**
```json
{
  "codigo": "NOT_FOUND",
  "mensaje": "No se encontraron usuarios con el número de póliza especificado."
}
```
</details>

<details>
<summary><strong><code>500 Internal Server Error</code></strong> - Error interno del servidor.</summary>

**Cuerpo de la Respuesta (`application/json`)**
```json
{
  "codigo": "INTERNAL_ERROR",
  "mensaje": "Ocurrió un error inesperado al procesar la solicitud."
}
```
</details>

---

## Modelos de Datos (Schemas)

### `Usuario`

| Propiedad | Tipo | Descripción | Requerido | Ejemplo |
| --- | --- | --- | --- | --- |
| `idUsuario` | `string` (uuid) | Identificador único universal para el usuario. | Sí | `123e...` |
| `nombreCompleto` | `string` | Nombre y apellidos del asegurado. | Sí | `Ana Sofía García López` |
| `fechaNacimiento` | `string` (date) | Fecha de nacimiento del usuario. | Sí | `1985-05-20` |
| `email` | `string` (email) | Dirección de correo electrónico de contacto. | Sí | `ana.garcia@email.com` |
| `telefono` | `string` | Número de teléfono de contacto del usuario. | No | `+52 55 1234 5678` |
| `direccion` | `object` ([Direccion](#direccion)) | Dirección postal del usuario. | No | |
| `polizas` | `array` ([PolizaResumen](#polizaresumen)) | Lista de pólizas asociadas al usuario. | Sí | |

### `PolizaResumen`

| Propiedad | Tipo | Descripción | Requerido | Ejemplo |
| --- | --- | --- | --- | --- |
| `numeroPoliza` | `string` | Número identificador de la póliza. | No | `POL-987654321` |
| `tipo` | `string` | Tipo de seguro. | No | `Gastos Medicos` |
| `estado` | `string` | Estado actual de la póliza. | No | `Activa` |

### `Direccion`

| Propiedad | Tipo | Descripción | Requerido | Ejemplo |
| --- | --- | --- | --- | --- |
| `calle` | `string` | | No | `Av. de la Reforma 222` |
| `ciudad` | `string` | | No | `Ciudad de México` |
| `estado` | `string` | | No | `CDMX` |
| `codigoPostal` | `string` | | No | `06600` |
| `pais` | `string` | | No | `México` |

### `Error`

| Propiedad | Tipo | Descripción | Requerido | Ejemplo |
| --- | --- | --- | --- | --- |
| `codigo` | `string` | Un código de error legible por máquina. | Sí | `INVALID_PARAMETER` |
| `mensaje` | `string` | Una descripción del error legible por humanos. | Sí | `El parámetro 'limit' excede...` |
| `detalles` | `string` | Información adicional sobre el error. | No | `El valor proporcionado fue 150.` |
