# 🏥 API Artículos - Apoteca

API REST segura para consultar archivos de artículos de Apoteca. Esta API proporciona acceso a la información más reciente de productos farmacéuticos con autenticación por token y control de permisos.

## 📋 Tabla de Contenidos

- [Características](#-características)
- [Registro y Autenticación](#-registro-y-autenticación)
- [Instalación](#-instalación)
- [Uso](#-uso)
- [Endpoints](#-endpoints)
- [Ejemplos de Uso](#-ejemplos-de-uso)
- [Estructura de Respuesta](#-estructura-de-respuesta)
- [Manejo de Errores](#-manejo-de-errores)
- [Rate Limiting](#-rate-limiting)
- [Autenticación](#-autenticación)
- [Soporte](#-soporte)
- [Contribución](#-contribución)
- [Licencia](#-licencia)

## ✨ Características

- ✅ **Autenticación segura** con Bearer Token y JWT
- ✅ **Control de permisos** por farmacia y artículo
- ✅ **Paginación avanzada** con límites configurables
- ✅ **Filtrado de artículos** bloqueados por usuario
- ✅ **Logging detallado** de accesos y errores
- ✅ **Respuestas JSON** estructuradas y consistentes
- ✅ **Alta disponibilidad** con servidor de producción
- ✅ **Documentación completa** con ejemplos prácticos
- ✅ **Métricas de rendimiento** con tiempo de respuesta

## 🔐 Registro y Autenticación

### Registro de Usuario

Para obtener acceso a la API, debes registrarte en el sistema:

**URL de Registro:** https://wsapi.innoverse.es/registro_usuario.php

**Proceso de Registro:**
1. Accede a la URL de registro
2. Completa el formulario con:
   - Nombre Completo (requerido)
   - Email (requerido)
   - Teléfono (requerido)
   - Empresa (opcional)
   - Motivo de Uso (opcional)
3. **Espera a que un administrador active tu cuenta** (puede tomar hasta 24 horas)
4. Una vez activado, recibirás un token JWT único por email
5. Tu información será visible en el panel de administración y podra copiar y guardar el token generado.

**⚠️ Importante:** Tu cuenta permanecerá inactiva hasta que un administrador la apruebe. Este proceso puede tomar hasta 24 horas hábiles.

### Obtención del Token

Una vez que tu cuenta sea activada por un administrador, podras consumir la api :

```http
Authorization: Bearer tu_token_jwt_aqui
```

**📧 Notificación:** El token será enviado automáticamente a tu email registrado una vez que tu cuenta sea activada. (Proximamente)

## 🚀 Instalación

Esta API está desplegada en producción y no requiere instalación local. Puedes acceder directamente a través de:

```
https://wsapi.innoverse.es/articulos
```

**Nota:** Ahora requiere autenticación con token Bearer.

## 📖 Uso

### Endpoint Principal

```http
GET https://wsapi.innoverse.es/articulos
Authorization: Bearer tu_token_jwt_aqui
```

### Ejemplos de Petición

**Obtener todos los artículos:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos" \
     -H "Authorization: Bearer tu_token_jwt_aqui" \
     -H "Accept: application/json"
```

**Obtener artículos con paginación:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos?limit=50&offset=0" \
     -H "Authorization: Bearer tu_token_jwt_aqui" \
     -H "Accept: application/json"
```

**Obtener todos los artículos sin límite:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos?limit=all" \
     -H "Authorization: Bearer tu_token_jwt_aqui" \
     -H "Accept: application/json"
```

**Obtener artículos de una farmacia específica:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos?idFarm=2" \
     -H "Authorization: Bearer tu_token_jwt_aqui" \
     -H "Accept: application/json"
```

**Obtener un artículo específico:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos?idFarm=1&idArticu=654571" \
     -H "Authorization: Bearer tu_token_jwt_aqui" \
     -H "Accept: application/json"
```

### Ejemplo con JavaScript

**Obtener todos los artículos:**
```javascript
const token = 'tu_token_jwt_aqui';

fetch('https://wsapi.innoverse.es/articulos', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'Accept': 'application/json'
  }
})
.then(response => response.json())
.then(data => {
  console.log('Artículos:', data.articulo);
  console.log('Total:', data.total);
  console.log('Tiempo de respuesta:', data.tiempo_respuesta_ms + 'ms');
})
.catch(error => {
  console.error('Error:', error);
});
```

**Obtener artículos con paginación:**
```javascript
const token = 'tu_token_jwt_aqui';

function obtenerArticulos(limit = 100, offset = 0) {
  return fetch(`https://wsapi.innoverse.es/articulos?limit=${limit}&offset=${offset}`, {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/json'
    }
  })
  .then(response => response.json())
  .then(data => {
    return {
      articulos: data.articulo,
      total: data.total,
      limit: data.limit,
      offset: data.offset,
      tiempoRespuesta: data.tiempo_respuesta_ms
    };
  });
}

// Uso
obtenerArticulos(50, 0).then(resultado => {
  console.log('Artículos:', resultado.articulos);
  console.log('Total disponible:', resultado.total);
});
```

### Ejemplo con Python

**Obtener todos los artículos:**
```python
import requests

token = 'tu_token_jwt_aqui'
headers = {
    'Authorization': f'Bearer {token}',
    'Accept': 'application/json'
}

response = requests.get('https://wsapi.innoverse.es/articulos', headers=headers)
if response.status_code == 200:
    data = response.json()
    articulos = data['articulo']
    print(f"Total de artículos: {data['total']}")
    print(f"Tiempo de respuesta: {data['tiempo_respuesta_ms']}ms")
    for articulo in articulos:
        print(f"ID: {articulo['IdArticu']} - {articulo['Descripcion']}")
else:
    print(f"Error: {response.status_code}")
    print(response.json())
```

**Obtener artículos con paginación:**
```python
import requests

def obtener_articulos_paginados(token, limit=100, offset=0):
    headers = {
        'Authorization': f'Bearer {token}',
        'Accept': 'application/json'
    }
    
    params = {
        'limit': limit,
        'offset': offset
    }
    
    response = requests.get('https://wsapi.innoverse.es/articulos', 
                          headers=headers, params=params)
    
    if response.status_code == 200:
        return response.json()
    else:
        raise Exception(f"Error {response.status_code}: {response.json()}")

# Uso
try:
    resultado = obtener_articulos_paginados('tu_token_jwt_aqui', 50, 0)
    print(f"Artículos obtenidos: {len(resultado['articulo'])}")
    print(f"Total disponible: {resultado['total']}")
except Exception as e:
    print(f"Error: {e}")
```

## 🔗 Endpoints

### GET / - Obtener Artículos

Obtiene el archivo más reciente de artículos con información completa.

**Headers requeridos:**
- `Authorization: Bearer tu_token_jwt_aqui`

**Parámetros:**
- `idFarm` (opcional): ID de la farmacia (por defecto: 1)
- `idArticu` (opcional): ID específico del artículo a consultar
- `limit` (opcional): Número de artículos por página (por defecto: 100, máximo: 10000)
- `offset` (opcional): Número de artículos a saltar para paginación (por defecto: 0)
- `limit=all` o `limit=todos`: Obtener todos los artículos sin límite

**Respuesta exitosa (200):**

**Con paginación:**
```json
{
  "idFarm": 1,
  "nombreFarm": "Farmacia Prueba1",
  "articulo": [
    {
      "IdArticu": "000000",
      "idfarm": 1,
      "Descripcion": "SPD 2 SEMANAS",
      "StockActual": 0,
      "EnRobot": "T",
      "StockRobot": 45,
      "fecha_creacion": "2024-01-15 10:30:00",
      "fecha_actualizacion": "2024-01-15 10:30:00"
    }
  ],
  "total": 1500,
  "limit": 100,
  "offset": 0,
  "timestamp": "2024-01-15 10:30:00",
  "tiempo_respuesta_ms": 45.123
}
```

**Sin límite (todos los artículos):**
```json
{
  "idFarm": 1,
  "nombreFarm": "Farmacia Prueba1",
  "articulo": [...],
  "total": 1500,
  "limit": "todos",
  "offset": 0,
  "info": "Se obtuvieron todos los artículos disponibles",
  "timestamp": "2024-01-15 10:30:00",
  "tiempo_respuesta_ms": 125.456
}
```

**Artículo específico:**
```json
{
  "idFarm": 2,
  "nombreFarm": "Farmacia Prueba2",
  "articulo": [
    {
      "IdArticu": "654571",
      "idfarm": 2,
      "Descripcion": "ARTÍCULO ESPECÍFICO",
      "StockActual": 10,
      "EnRobot": "T",
      "StockRobot": 25,
      "fecha_creacion": "2024-01-15 10:30:00",
      "fecha_actualizacion": "2024-01-15 10:30:00"
    }
  ],
  "timestamp": "2024-01-15 10:30:00",
  "tiempo_respuesta_ms": 12.789
}
```

## 📊 Estructura de Respuesta

### Artículo

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| `idFarm` | integer | ID de la farmacia | `1` |
| `nombreFarm` | string | Nombre de la farmacia | `"Farmacia Prueba1"` |
| `IdArticu` | string | ID único del artículo (6 dígitos) | `"000000"` |
| `idfarm` | integer | ID de la farmacia del artículo | `1` |
| `Descripcion` | string | Descripción del producto | `"SPD 2 SEMANAS"` |
| `StockActual` | integer | Cantidad en stock | `0` |
| `EnRobot` | string | Indica si está en robot ("T" o "F") | `"T"` |
| `StockRobot` | integer | Stock disponible en robot | `45` |
| `fecha_creacion` | string | Fecha de creación del registro | `"2024-01-15 10:30:00"` |
| `fecha_actualizacion` | string | Fecha de última actualización | `"2024-01-15 10:30:00"` |
| `total` | integer | Total de artículos disponibles | `1500` |
| `limit` | mixed | Límite aplicado (número o "todos") | `100` |
| `offset` | integer | Offset aplicado | `0` |
| `timestamp` | string | Timestamp de la respuesta | `"2024-01-15 10:30:00"` |
| `tiempo_respuesta_ms` | float | Tiempo de respuesta en milisegundos | `45.123` |

### Validaciones

- `idFarm`: Debe ser un número entero mayor a 0 (por defecto: 1)
- `IdArticu`: Debe ser exactamente 6 dígitos numéricos
- `StockActual`: Debe ser un número entero mayor o igual a 0
- `EnRobot`: Debe ser "T" o "F"
- `StockRobot`: Debe ser un número entero mayor o igual a 0
- `limit`: Máximo 10000 artículos por petición
- `offset`: Debe ser un número entero mayor o igual a 0

## ⚠️ Manejo de Errores

### Error 401 - Token inválido

```json
{
  "error": true,
  "message": "Token inválido",
  "code": "UNAUTHORIZED"
}
```

### Error 403 - Sin permisos

```json
{
  "error": true,
  "message": "No tienes permisos para consultar esta farmacia",
  "code": "FORBIDDEN"
}
```

### Error 500 - Error interno

```json
{
  "error": true,
  "message": "Error interno del servidor",
  "timestamp": "2024-01-15 10:30:00",
  "tiempo_respuesta_ms": 12.789
}
```

### Respuesta cuando no se encuentra un artículo específico

```json
{
  "idFarm": 1,
  "nombreFarm": "Farmacia Prueba1",
  "articulo": [],
  "message": "Artículo no encontrado o bloqueado"
}
```

## 🚦 Rate Limiting

**Recomendación:** No hacer más de **10 peticiones por minuto** para evitar sobrecargar el servidor.

**Límites de seguridad:**
- Máximo 10000 artículos por petición
- Logging automático de todas las peticiones
- Control de acceso por usuario y farmacia

## 🔐 Autenticación

### Versión Actual (2.0.0)
- **Requiere autenticación** con Bearer Token o JWT
- **Control de permisos** por farmacia y artículo
- **Registro obligatorio** en https://wsapi.innoverse.es/registro_usuario.php
- **Logging detallado** de accesos y errores
- **Filtrado automático** de artículos bloqueados

### Tipos de Token Soportados

1. **Bearer Token:** Token de acceso directo
2. **JWT Token:** Token JWT con payload de usuario

### Proceso de Autenticación

1. **Registro:** Accede a https://wsapi.innoverse.es/registro_usuario.php
2. **Activación:** Espera a que un administrador active tu cuenta (hasta 24 horas)
3. **Obtención de Token:** Recibe tu token JWT único por email
4. **Uso:** Incluye el token en el header `Authorization: Bearer tu_token`
5. **Validación:** El sistema valida automáticamente tu token y permisos

## 🛠️ Ejemplos de Uso Avanzados

### Implementar paginación completa

```javascript
class ArticulosAPI {
  constructor(token) {
    this.token = token;
    this.baseUrl = 'https://wsapi.innoverse.es/articulos';
  }

  async obtenerTodosArticulos(idFarm = 1) {
    const articulos = [];
    let offset = 0;
    const limit = 1000; // Máximo por petición
    
    while (true) {
      const response = await fetch(`${this.baseUrl}?idFarm=${idFarm}&limit=${limit}&offset=${offset}`, {
        headers: {
          'Authorization': `Bearer ${this.token}`,
          'Accept': 'application/json'
        }
      });
      
      const data = await response.json();
      
      if (data.error) {
        throw new Error(data.message);
      }
      
      articulos.push(...data.articulo);
      
      // Si obtenemos menos artículos que el límite, hemos terminado
      if (data.articulo.length < limit) {
        break;
      }
      
      offset += limit;
    }
    
    return articulos;
  }

  async obtenerArticulosConStock(idFarm = 1) {
    const todosLosArticulos = await this.obtenerTodosArticulos(idFarm);
    return todosLosArticulos.filter(articulo => articulo.StockActual > 0);
  }
}

// Uso
const api = new ArticulosAPI('tu_token_jwt_aqui');
api.obtenerTodosArticulos(1).then(articulos => {
  console.log(`Total de artículos: ${articulos.length}`);
});
```

### Monitorear rendimiento de la API

```javascript
async function medirRendimientoAPI(token, iteraciones = 10) {
  const tiempos = [];
  
  for (let i = 0; i < iteraciones; i++) {
    const inicio = performance.now();
    
    const response = await fetch('https://wsapi.innoverse.es/articulos?limit=100', {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/json'
      }
    });
    
    const data = await response.json();
    const fin = performance.now();
    
    tiempos.push({
      iteracion: i + 1,
      tiempoCliente: fin - inicio,
      tiempoServidor: data.tiempo_respuesta_ms,
      totalArticulos: data.total
    });
  }
  
  const tiempoPromedio = tiempos.reduce((sum, t) => sum + t.tiempoCliente, 0) / tiempos.length;
  const tiempoServidorPromedio = tiempos.reduce((sum, t) => sum + t.tiempoServidor, 0) / tiempos.length;
  
  console.log('Estadísticas de rendimiento:');
  console.log(`Tiempo promedio cliente: ${tiempoPromedio.toFixed(2)}ms`);
  console.log(`Tiempo promedio servidor: ${tiempoServidorPromedio.toFixed(2)}ms`);
  console.log('Detalles por iteración:', tiempos);
  
  return tiempos;
}

// Uso
medirRendimientoAPI('tu_token_jwt_aqui', 5);
```

### Manejo de errores robusto

```javascript
class ArticulosAPISegura {
  constructor(token) {
    this.token = token;
    this.baseUrl = 'https://wsapi.innoverse.es/articulos';
    this.maxRetries = 3;
  }

  async hacerPeticion(url, opciones = {}) {
    let intentos = 0;
    
    while (intentos < this.maxRetries) {
      try {
        const response = await fetch(url, {
          headers: {
            'Authorization': `Bearer ${this.token}`,
            'Accept': 'application/json',
            ...opciones.headers
          },
          ...opciones
        });
        
        const data = await response.json();
        
        if (response.ok) {
          return data;
        } else {
          throw new Error(`HTTP ${response.status}: ${data.message || 'Error desconocido'}`);
        }
      } catch (error) {
        intentos++;
        console.warn(`Intento ${intentos} fallido:`, error.message);
        
        if (intentos >= this.maxRetries) {
          throw new Error(`Error después de ${this.maxRetries} intentos: ${error.message}`);
        }
        
        // Esperar antes del siguiente intento
        await new Promise(resolve => setTimeout(resolve, 1000 * intentos));
      }
    }
  }

  async obtenerArticulo(idArticu, idFarm = 1) {
    return this.hacerPeticion(`${this.baseUrl}?idFarm=${idFarm}&idArticu=${idArticu}`);
  }

  async obtenerArticulosPaginados(limit = 100, offset = 0, idFarm = 1) {
    return this.hacerPeticion(`${this.baseUrl}?idFarm=${idFarm}&limit=${limit}&offset=${offset}`);
  }
}

// Uso con manejo de errores
const api = new ArticulosAPISegura('tu_token_jwt_aqui');

api.obtenerArticulo('654571', 1)
  .then(data => {
    console.log('Artículo encontrado:', data.articulo[0]);
  })
  .catch(error => {
    console.error('Error al obtener artículo:', error.message);
  });
```

## 📞 Soporte

Para soporte técnico y consultas:

- **Email:** sistemas@innoverse.es
- **Sistema:** Sistema de Apoteca
- **Registro:** https://wsapi.innoverse.es/registro_usuario.php


## 📄 Licencia

Este proyecto es propiedad de Innoverse. Todos los derechos reservados.

---

**Desarrollado con ❤️ por el equipo de sistemas de Innoverse** 
