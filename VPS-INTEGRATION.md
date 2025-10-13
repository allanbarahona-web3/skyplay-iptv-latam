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

### **Opción 1: n8n Webhook (Recomendado)**
```bash
# URL del webhook n8n
https://tu-n8n-instance.com/webhook/skyplay-wholesale

# Método: POST
# Content-Type: application/json
# Headers: X-Source: skyplay-frontend
```

### **Opción 2: API Directo Nest.js**
```bash
# URL del API directo
https://tu-vps-domain.com/api/v1/wholesale/register

# Headers requeridos:
# Content-Type: application/json
# Authorization: Bearer tu-api-key
# X-API-Source: skyplay-frontend
```

### **Estructura JSON enviada:**
```javascript
{
    "fullName": "Juan Pérez García",
    "countryCode": "+506", 
    "phone": "12345678",
    "email": "juan@email.com",
    "comments": "Interesado en plan para empresa",
    "timestamp": "2025-10-13T15:30:00.000Z",
    "source": "skyplay-wholesale-modal"
}
```

## ⚙️ Configuración del Webhook

### **Paso 1: Configurar URL en el código**
Editar `index.html` línea ~3950:

```javascript
const webhookConfig = {
    enabled: true,
    url: 'https://tu-n8n-real.com/webhook/skyplay-wholesale', // 👈 CAMBIAR
    apiKey: 'tu-api-key-si-necesitas', // Opcional
    timeout: 10000
};
```

### **Paso 2: Ejemplo configuración n8n**
```javascript
// Para n8n
const webhookConfig = {
    enabled: true,
    url: 'https://n8n.tudominio.com/webhook/skyplay-wholesale',
    apiKey: '', // n8n no requiere API key por defecto
    timeout: 10000
};
```

### **Paso 3: Ejemplo configuración API directo**
```javascript
// Para tu Nest.js API
const webhookConfig = {
    enabled: true,
    url: 'https://api.tudominio.com/api/v1/wholesale/register',
    apiKey: 'bearer-token-secreto',
    timeout: 15000
};
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

## 🤖 Configuración n8n (Recomendada)

### **Flujo n8n sugerido:**
1. **Webhook Trigger** → Recibe datos del formulario
2. **PostgreSQL Node** → Inserta en base de datos
3. **Email Node** → Notifica al equipo (opcional)
4. **HTTP Response** → Confirma recepción

### **Nodos n8n necesarios:**
```bash
1. Webhook (Trigger)
   - Method: POST
   - Path: /webhook/skyplay-wholesale

2. PostgreSQL (Database)
   - Operation: Insert
   - Table: wholesale_clients
   - Columns: Map from webhook data

3. HTTP Response (Optional)
   - Status: 200
   - Body: {"status": "success", "message": "Registration received"}
```

## 🚀 Próximos Pasos

1. **Instalar n8n** o configurar Nest.js API
2. **Crear workflow** en n8n o endpoint
3. **Actualizar URL** en el código frontend
4. **Probar integración** completa
5. **Monitoreo y análisis**

## 🧪 Testing

### **URL de prueba:**
```bash
curl -X POST https://tu-webhook-url.com/webhook/skyplay-wholesale \
  -H "Content-Type: application/json" \
  -H "X-Source: skyplay-frontend" \
  -d '{
    "fullName": "Test User",
    "countryCode": "+506",
    "phone": "12345678", 
    "email": "test@example.com",
    "comments": "Test registration",
    "timestamp": "2025-10-13T15:30:00.000Z",
    "source": "skyplay-wholesale-modal"
  }'
```