# Las Villas de Bayamón · Portal comunitario

MVP estático en español para explorar un portal de residentes de Las Villas de Bayamón. Incluye el logo compartido por la comunidad, la foto de la entrada como portada de login y una paleta inspirada en los edificios crema y el terracota del logo. Incluye vistas de residente y una vista de ejemplo para Junta/administración. Todo lo mostrado es ficticio; esta aplicación sirve para revisión de diseño y flujo, no para operación comunitaria real.

## Ejecutar localmente

No requiere instalación de paquetes ni compilación. Desde la carpeta del proyecto:

```powershell
python -m http.server 8000
```

Abre `http://localhost:8000` en el navegador. Si Python no está instalado, cualquier servidor estático local puede servir `index.html`, `styles.css` y `app.js`. Abrir `index.html` directamente también permite recorrer la mayoría de las vistas, aunque un servidor local es preferible.

## Incluye

- Navegación adaptable a móvil y escritorio.
- Pantalla inicial de acceso con email/usuario y contraseña, más selector de perfil: Junta, Administración, Titular, Residente y Proveedor. Para recorrer la demo se acepta cualquier texto en ambos campos; no se valida ni crea una cuenta.
- Formulario de alta de demostración: acepta emails del padrón ficticio con su rol correspondiente o un código de invitación de demostración. Códigos de prueba: `DEMO-TITULAR-2026`, `DEMO-RESIDENTE-2026`, `DEMO-JUNTA-2026`, `DEMO-ADMIN-2026`, `DEMO-PROVEEDOR-2026`. Son públicos en el código y no sirven para autenticar usuarios reales.
- Menú y paneles de ejemplo distintos por rol. Titular puede ver pagos y balance; Residente no ve finanzas; Junta y Administración tienen vistas de gestión; Proveedor ve tareas, contratos y calendario de trabajo asignados.
- Cola de tareas ordenada por urgencia, cambios de estado simulados, balances por apartamento para Junta/Administración, formulario de publicación de anuncios y foro con publicaciones/comentarios locales.
- Registro de titulares y residentes por apartamento con contactos, ocupación alquilada/inquilino, proxy o apoderado, notas privadas, búsqueda y altas/ediciones de ejemplo; solo aparece en el menú de Junta y Administración. Incluye datos ficticios y no representa el padrón real.
- Catálogo de servicios y contratos para Junta/Administración con contactos, dirección, horario, estado de pago, documento de referencia, fechas de vigencia/renovación y notas. Permite asignar tasks a un proveedor. El portal de proveedor muestra solo los contratos/tareas asociados a su perfil de demostración.
- El rol Proveedor no tiene acceso al Foro ni a Contactos y ayuda. Tiene un canal separado para comunicarse únicamente con Junta y Administración.
- Biblioteca documental filtrada por rol: Residentes ven solo solicitudes y reglamentos; Titulares, Junta y Administración pueden ver categorías adicionales. Los proveedores consultan únicamente los contratos asignados.
- Preferencias de notificación con checklist por tipo y selección de email, SMS o ambos. Solo guarda la preferencia en el estado local de demostración; no envía notificaciones.
- Votaciones comunitarias accesibles solo para Titulares y Junta; solo el perfil Titular puede emitir un voto. Administración, Residentes y Proveedores no tienen acceso a la sección. Los totales son de muestra y cada perfil titular puede votar una vez por encuesta durante la sesión.
- Contactos, enlaces útiles y preguntas frecuentes.
- Panel de residente con resumen de cuenta, avisos y actividades.
- Historial ficticio de cargos, pagos y recibos; el botón de pago abre un aviso simulado.
- Calendario navegable de ejemplo para reservar el gazebo.
- Formulario de mantenimiento de demostración, con campo de foto que no carga ni conserva archivos.
- Biblioteca de documentos de muestra, avisos, calendario comunitario y directorio optativo ficticio.
- Panel de Junta/administración con totales agregados de ejemplo, supervisión de suplidores/contratos y una muestra de registro de auditoría.
- Ventana informativa para transferencia de titularidad.

## Arquitectura

MVP sin dependencias externas de JavaScript: `index.html` contiene el esqueleto; `styles.css` y `roles.css` implementan la interfaz responsiva; `app.js` contiene datos de demostración y navegación/login simulado; `roles.js` define paneles e interacciones de cada rol; `assets/logo-vdb.png` y `assets/villas-entrance.png` son copias de los recursos visuales suministrados. Se puede reemplazar esta capa de presentación por una aplicación React/Next.js y conectar servicios de backend cuando se definan requisitos y responsables.

## Datos, roles y seguridad

Los datos de demostración viven en el código del navegador. El flujo de alta comprueba una lista y códigos ficticios, pero el login de demostración todavía acepta cualquier email/usuario y contraseña para poder previsualizar roles. No hay cuentas ni autenticación real; esos límites no son controles de acceso y se pueden evadir desde el navegador. No hay base de datos, API, almacenamiento de imágenes, envío de mensajes ni procesamiento de pagos. Publicaciones, comentarios, preferencias, votos y altas son temporales de la sesión. Los permisos deben implementarse y comprobarse en el servidor antes de usar información real.

Para una versión real, cada cuenta debe asociarse a una unidad tras validar la titularidad. El servidor debe aplicar autorización por recurso y rol (residente, Junta, administración), de modo que el residente solo pueda consultar información autorizada de su unidad. No se debe confiar en filtros ni permisos implementados solo en el navegador. Deben definirse retención, minimización de datos, consentimiento del directorio, recuperación de cuenta, MFA para personal privilegiado, políticas de respaldo, registros protegidos contra alteración y procedimientos de respuesta a incidentes. Las fotos y documentos requieren almacenamiento privado, URLs de acceso temporal y validación de contenido.

## Antes de producción

1. Acordar requisitos, gobierno de datos, políticas de acceso, flujos de aprobación y retención con la Junta y administración.
2. Elegir e implementar autenticación y recuperación de cuenta; verificar identidad y asignar titulares a apartamentos mediante un proceso administrativo.
3. Crear una API y una base de datos con permisos en servidor, migraciones, copias de seguridad y separación de entornos.
4. Configurar almacenamiento privado de documentos/fotos con permisos y auditoría.
5. Integrar un procesador de pagos aprobado por la comunidad. La aplicación no debe recolectar ni almacenar números de tarjeta; usar página alojada o tokenización del proveedor. Confirmar requisitos comerciales, liquidación, conciliación y recibos con Banco Popular/proveedor elegido.
6. Añadir validación, límites de carga, protección contra abuso, monitoreo, manejo de secretos, cifrado en tránsito y en reposo, y revisión de accesibilidad.
7. Completar revisión de seguridad independiente, pruebas de privacidad y autorización, revisión legal/contable, plan de recuperación y aprobación formal de la Junta antes de usar datos reales.
8. Elegir hosting, dominio, certificados TLS, observabilidad, soporte, responsables y proceso de actualización.

La plataforma no está conectada a Banco Popular, Placetopay, MultiPago Popular ni otro procesador. Cualquier integración futura requiere evaluación y credenciales contratadas por la comunidad.
