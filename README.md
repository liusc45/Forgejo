# Forgejo - Git Hosting Platform
Forgejo es una solución de hosting de Git autohospedada, un fork de Gitea con desarrollo comunitario.

## 🚀 Despliegue con Docker Compose

Este proyecto utiliza Docker Compose para desplegar una instancia completa de Forgejo con:

- **Forgejo**: Servidor Git principal (puerto 3000)
- **MySQL**: Base de datos (puerto 3306)
- **Caddy**: Proxy inverso con HTTPS automático (puertos 80/443)

## 📋 Requisitos Previos

- Docker y Docker Compose instalados
- 2GB+ de RAM disponible
- 10GB+ de espacio en disco

## 🛠️ Configuración y Uso

### 1. Iniciar los servicios
```bash
docker-compose up -d
```

### 2. Acceso a la aplicación
- **Forgejo**: http://git.localhost:3000
- **Con HTTPS**: https://git.localhost (via Caddy)

### 3. Configuración inicial
1. Accede a http://git.localhost:3000
2. Crea la cuenta de administrador
3. Configura las opciones del repositorio

### 4. Detener los servicios
```bash
docker-compose down
```

## 📁 Estructura de Directorios

```
.
├── docker-compose.yaml    # Configuración de servicios
├── Caddyfile             # Configuración del proxy inverso
├── forgejo/              # Datos de Forgejo (repositorios, configuración)
├── mysql/                # Base de datos Percona Server (persistente)
├── caddy_data/           # Certificados SSL, logs de Caddy
├── caddy_config/         # Configuración dinámica de Caddy
└── README.md            # Esta documentación
```

### Volumenes Importantes:
- **forgejo/**: Repositorios Git, datos de usuarios, configuración
- **mysql/**: Toda la base de datos, incluye tablas, índices, usuarios
- **caddy_data/**: Certificados SSL automáticos, logs de accesos
- **caddy_config/**: Configuración en tiempo de ejecución de Caddy

## 🔧 Configuración

### Variables de Entorno Principales
- `USER_UID`/`USER_GID`: IDs de usuario para permisos
- `FORGEJO__database__*`: Configuración de base de datos
- `FORGEJO__server__*`: Configuración del servidor

### Base de Datos
- **Motor**: Percona Server 8.0 (drop-in replacement mejorado de MySQL)
- **Base de datos**: forgejo
- **Usuario**: forgejo
- **Contraseña**: forgejo

#### Ventajas de Percona Server sobre MySQL:
- **Mejor rendimiento**: 2x más rápido en consultas complejas
- **Mayor escalabilidad**: Soporte para más conexiones concurrentes
- **XtraDB**: Motor de almacenamiento mejorado basado en InnoDB
- **Mejor diagnóstico**: Herramientas avanzadas de monitoreo y análisis
- **Hot backups**: Backups sin detener el servicio
- **Compatible 100%**: Sustitución directa de MySQL
- **Enterprise features**: Características corporativas disponibles gratuitamente

#### Volumenes y Persistencia:
- **mysql/**: Datos persistentes de la base de datos (`/var/lib/mysql`)
- **Importante**: Los datos se conservan al reiniciar o actualizar contenedores
- **Backups recomendados**: Copiar el directorio `mysql/` regularmente

### Proxy Inverso (Caddy)
- **Dominio**: git.localhost
- **HTTPS**: Automático con certificados locales
- **Compression**: gzip activado
- **Límite de subida**: 100MB

## 🔒 Seguridad

- Las contraseñas están configuradas para desarrollo
- En producción, cambiar todas las credenciales
- Configurar dominios reales para HTTPS válido
- Considerar backups regulares de los volúmenes

## 🐛 Troubleshooting

### Problemas comunes:
1. **Permisos**: Asegurar que los directorios tienen permisos correctos
2. **Puertos**: Verificar que los puertos 3000, 80, 443 estén disponibles
3. **Base de datos**: Esperar a que MySQL inicie completamente antes de acceder a Forgejo

### Logs útiles:
```bash
# Ver logs de todos los servicios
docker-compose logs

# Ver logs de un servicio específico
docker-compose logs forgejo
docker-compose logs db
docker-compose logs caddy
```

## 📚 Enlaces Útiles

- **Forgejo**: https://forgejo.org/
- **Documentación**: https://forgejo.org/docs/latest/
- **Repositorio Codeberg**: https://codeberg.org/forgejo/forgejo
- **Caddy**: https://caddyserver.com/

## 📄 Licencia

Este proyecto sigue la misma licencia que Forgejo.


