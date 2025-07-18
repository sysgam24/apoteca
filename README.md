# 🏥 API Artículos - Apoteca

API REST para consultar archivos de artículos de Apoteca. Esta API proporciona acceso a la información más reciente de productos farmacéuticos de forma automática y eficiente.

## 📋 Tabla de Contenidos

- [Características](#-características)
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

- ✅ **Obtención automática** del archivo más reciente de artículos
- ✅ **Respuestas JSON** estructuradas y consistentes
- ✅ **Sin autenticación** requerida (versión actual)
- ✅ **Alta disponibilidad** con servidor de producción
- ✅ **Documentación completa** con ejemplos prácticos

## 🚀 Instalación

Esta API está desplegada en producción y no requiere instalación local. Puedes acceder directamente a través de:

```
https://wsapi.innoverse.es/articulos
```

## 📖 Uso

### Endpoint Principal

```http
GET https://wsapi.innoverse.es/articulos
```

### Ejemplos de Petición

**Obtener todos los artículos:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos" \
     -H "Accept: application/json"
```

**Obtener artículos de una farmacia específica:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos?idFarm=2" \
     -H "Accept: application/json"
```

**Obtener un artículo específico:**
```bash
curl -X GET "https://wsapi.innoverse.es/articulos?idFarm=1&idArticu=654571" \
     -H "Accept: application/json"
```

### Ejemplo con JavaScript

**Obtener todos los artículos:**
```javascript
fetch('https://wsapi.innoverse.es/articulos')
  .then(response => response.json())
  .then(data => {
    console.log('Artículos:', data.articulo);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**Obtener un artículo específico:**
```javascript
fetch('https://wsapi.innoverse.es/articulos?idFarm=1&idArticu=654571')
  .then(response => response.json())
  .then(data => {
    console.log('Artículo específico:', data.articulo[0]);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### Ejemplo con Python

**Obtener todos los artículos:**
```python
import requests

response = requests.get('https://wsapi.innoverse.es/articulos')
if response.status_code == 200:
    articulos = response.json()['articulo']
    for articulo in articulos:
        print(f"ID: {articulo['idArticu']} - {articulo['descripcion']}")
else:
    print(f"Error: {response.status_code}")
```

**Obtener un artículo específico:**
```python
import requests

response = requests.get('https://wsapi.innoverse.es/articulos?idFarm=1&idArticu=654571')
if response.status_code == 200:
    data = response.json()
    if data['articulo']:
        articulo = data['articulo'][0]
        print(f"ID: {articulo['idArticu']} - {articulo['descripcion']}")
    else:
        print("Artículo no encontrado")
else:
    print(f"Error: {response.status_code}")
```

## 🔗 Endpoints

### GET / - Obtener Artículos

Obtiene el archivo más reciente de artículos con información completa.

**Parámetros:**
- `idFarm` (opcional): ID de la farmacia (por defecto: 1)
- `idArticu` (opcional): ID específico del artículo a consultar

**Respuesta exitosa (200):**

**Sin parámetros (todos los artículos):**
```json
{
  "idFarm": 1,
  "nombreFarm": "Farmacia Prueba1",
  "articulo": [
    {
      "idArticu": "000000",
      "descripcion": "SPD 2 SEMANAS",
      "stockActual": 0,
      "EnRobot": "T",
      "stockRobot": 45
    },
    {
      "idArticu": "000001",
      "descripcion": "ALCOHOL BORICADO A SATURACION (5%) 30 ML SOLUCION OTICA (PO)",
      "stockActual": 0,
      "EnRobot": "F",
      "stockRobot": 0
    }
  ]
}
```

**Con parámetros específicos:**
```json
{
  "idFarm": 2,
  "nombreFarm": "Farmacia Prueba2",
  "articulo": [
    {
      "idArticu": "654571",
      "descripcion": "ARTÍCULO ESPECÍFICO",
      "stockActual": 10,
      "EnRobot": "T",
      "stockRobot": 25
    }
  ]
}
```

## 📊 Estructura de Respuesta

### Artículo

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| `idFarm` | integer | ID de la farmacia | `1` |
| `nombreFarm` | string | Nombre de la farmacia | `"Farmacia Prueba1"` |
| `idArticu` | string | ID único del artículo (6 dígitos) | `"000000"` |
| `descripcion` | string | Descripción del producto | `"SPD 2 SEMANAS"` |
| `stockActual` | integer | Cantidad en stock | `0` |
| `EnRobot` | string | Indica si está en robot ("T" o "F") | `"T"` |
| `stockRobot` | integer | Stock disponible en robot | `45` |

### Validaciones

- `idFarm`: Debe ser un número entero mayor a 0 (por defecto: 1)
- `idArticu`: Debe ser exactamente 6 dígitos numéricos
- `stockActual`: Debe ser un número entero mayor o igual a 0
- `EnRobot`: Debe ser "T" o "F"
- `stockRobot`: Debe ser un número entero mayor o igual a 0

## ⚠️ Manejo de Errores

### Error 500 - No se encontraron archivos

```json
{
  "error": true,
  "message": "No se encontraron archivos de artículos en la carpeta"
}
```

### Error 500 - Error de token

```json
{
  "error": true,
  "message": "Error al renovar access token"
}
```

### Respuesta cuando no se encuentra un artículo específico

```json
{
  "idFarm": 1,
  "nombreFarm": "Farmacia Prueba1",
  "articulo": [],
  "message": "Artículo no encontrado"
}
```

## 🚦 Rate Limiting

**Recomendación:** No hacer más de **10 peticiones por minuto** para evitar sobrecargar el servidor.

## 🔐 Autenticación

### Versión Actual (1.0.0)
- **No requiere autenticación** para las consultas públicas
- Acceso libre a todos los endpoints

### Versión Futura (1.1.0)
- Se implementará validación por token de seguridad
- Se recomienda preparar las aplicaciones para esta futura implementación

## 🛠️ Ejemplos de Uso Avanzados

### Filtrar artículos con stock

```javascript
fetch('https://wsapi.innoverse.es/articulos')
  .then(response => response.json())
  .then(data => {
    const articulosConStock = data.articulo.filter(
      articulo => articulo.stockActual > 0
    );
    console.log('Artículos con stock:', articulosConStock);
  });
```

### Buscar artículo por ID

**Método 1: Usando parámetro de URL (recomendado)**
```javascript
function buscarArticulo(id, idFarm = 1) {
  return fetch(`https://wsapi.innoverse.es/articulos?idFarm=${idFarm}&idArticu=${id}`)
    .then(response => response.json())
    .then(data => {
      return data.articulo.length > 0 ? data.articulo[0] : null;
    });
}

// Uso
buscarArticulo('654571', 1).then(articulo => {
  if (articulo) {
    console.log('Artículo encontrado:', articulo);
  } else {
    console.log('Artículo no encontrado');
  }
});
```

**Método 2: Filtrando en el cliente**
```javascript
function buscarArticulo(id) {
  return fetch('https://wsapi.innoverse.es/articulos')
    .then(response => response.json())
    .then(data => {
      return data.articulo.find(
        articulo => articulo.idArticu === id
      );
    });
}

// Uso
buscarArticulo('000031').then(articulo => {
  if (articulo) {
    console.log('Artículo encontrado:', articulo);
  } else {
    console.log('Artículo no encontrado');
  }
});
```

### Obtener estadísticas de stock

```javascript
fetch('https://wsapi.innoverse.es/articulos')
  .then(response => response.json())
  .then(data => {
    const stats = data.articulo.reduce((acc, articulo) => {
      acc.totalArticulos++;
      acc.stockTotal += articulo.stockActual;
      acc.stockRobotTotal += articulo.stockRobot;
      if (articulo.stockActual > 0) acc.conStock++;
      if (articulo.EnRobot === "T") acc.enRobot++;
      return acc;
    }, { totalArticulos: 0, stockTotal: 0, stockRobotTotal: 0, conStock: 0, enRobot: 0 });
    
    console.log('Estadísticas:', stats);
  });
```

### Consultar artículos por farmacia

```javascript
function obtenerArticulosPorFarmacia(idFarm) {
  return fetch(`https://wsapi.innoverse.es/articulos?idFarm=${idFarm}`)
    .then(response => response.json())
    .then(data => {
      return data.articulo;
    });
}

// Uso
obtenerArticulosPorFarmacia(2).then(articulos => {
  console.log(`Artículos de la farmacia 2:`, articulos);
});
```

## 📞 Soporte

Para soporte técnico y consultas:

- **Email:** sistemas@innoverse.es
- **Sistema:** Sistema de Apoteca

## 🤝 Contribución

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto es propiedad de Innoverse. Todos los derechos reservados.

---

**Desarrollado con ❤️ por el equipo de sistemas de Innoverse** 
