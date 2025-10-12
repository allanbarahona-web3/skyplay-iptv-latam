# 📋 Documentación - Integración VPS PostgreSQL

## 🎯 Estado Actual
- ✅ **Formulario funcionando** con email: `abarahonag@barmentech.com`
- ✅ **Preparado para webhook** al VPS
- ✅ **Estructura de datos** definida

## 📊 Estructura de Datos PostgreSQL

```sql
-- Tabla para clientes mayoristas
CREATE TABLE wholesale_clients (
    id SERIAL PRIMARY KEY,
    nombre_completo VARCHAR(255) NOT NULL,
    codigo_pais VARCHAR(10) NOT NULL,
    telefono VARCHAR(50) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    comentarios TEXT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    source VARCHAR(100) DEFAULT 'skyplay-wholesale-form',
    status VARCHAR(50) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Índices para optimización
CREATE INDEX idx_wholesale_email ON wholesale_clients(email);
CREATE INDEX idx_wholesale_status ON wholesale_clients(status);
CREATE INDEX idx_wholesale_created ON wholesale_clients(created_at);
```

## 🔗 Endpoint VPS Requerido

```javascript
// POST /api/webhook/wholesale-registration
{
    "nombre_completo": "Juan Pérez García",
    "codigo_pais": "+506",
    "telefono": "12345678",
    "email": "juan@email.com",
    "comentarios": "Interesado en plan para empresa",
    "timestamp": "2025-10-11T22:25:00.000Z",
    "source": "skyplay-wholesale-form"
}
```

## ⚙️ Activación del Webhook

Cuando el VPS esté listo, ejecutar en la consola del navegador:

```javascript
enableVPSIntegration(
    'https://tu-vps-domain.com/api/webhook/wholesale-registration',
    'tu-api-key-segura'
);
```

## 📧 Estado Actual del Email

- **Receptor**: abarahonag@barmentech.com
- **Asunto**: "Nueva Solicitud - Cliente Mayorista Skyplay IPTV"
- **Formato**: Tabla HTML con todos los datos
- **Auto-respuesta**: Mensaje personalizado al cliente
- **Redirección**: Vuelta a la página principal

## 🔄 Flujo Híbrido (Futuro)

1. **Usuario completa formulario**
2. **Datos van a email** (inmediato)
3. **Datos van a VPS** (simultáneo)
4. **Base PostgreSQL** se actualiza
5. **Notificaciones** automáticas

## 🛡️ Seguridad Implementada

- ✅ Honeypot anti-spam
- ✅ Validación HTML5
- ✅ HTTPS FormSubmit
- ✅ API Key para VPS
- ✅ Rate limiting preparado

## 📱 Países Soportados

21 países latinoamericanos con códigos telefónicos:
- 🇺🇸 Estados Unidos (+1)
- 🇲🇽 México (+52)
- 🇨🇷 Costa Rica (+506)
- [Y 18 más...]

## 🚀 Próximos Pasos

1. **Configurar VPS PostgreSQL**
2. **Crear endpoint webhook**
3. **Activar integración dual**
4. **Monitoreo y análisis**