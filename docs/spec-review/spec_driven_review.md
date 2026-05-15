# Documento Principal de Revisión - SIGSALUD

## 1. Introducción y Respuesta a la Pregunta Central

Este documento presenta la revisión del proyecto SIGSALUD bajo el enfoque de Spec-Driven Development Review.

**Pregunta Central:** ¿La especificación de este sistema es suficientemente clara, completa y trazable para que yo, sin hablar con nadie del equipo autor, pueda entender lo que hace, validar que funciona correctamente y continuar su desarrollo?

**Respuesta:** **Sí, en gran medida.** La documentación disponible en `README_SIGSALUD.md` es excelente y proporciona una visión clara de la arquitectura, los servicios, los flujos de integración y la configuración necesaria. La estructura del código es consistente y sigue patrones estándar de Express.js. Sin embargo, existen áreas de mejora en cuanto a la robustez del manejo de errores en la integración y la falta de pruebas automatizadas, lo que dificulta la validación sin ejecución.

---

## 2. Propósito y Tecnologías del Sistema

**Propósito:** SIGSALUD es una plataforma académica que simula la integración de tres sistemas de información en salud (HIS, LIS, RIS) y un servicio central de autenticación, utilizando una arquitectura híbrida basada en API REST.

**Tecnologías:**
- **Backend:** Node.js, Express.js.
- **Base de Datos:** PostgreSQL (4 bases de datos independientes).
- **Vistas:** EJS, Bootstrap 5.
- **Seguridad:** JWT, BcryptJS, Cookie-parser, Helmet.
- **Integración Externa:** Orthanc (PACS) para RIS.

---

## 3. Arquitectura Identificada

El sistema sigue una arquitectura de **Microservicios (o servicios independientes)** con comunicación síncrona vía API REST:
1.  **Auth/Login:** Centraliza la autenticación y redirige a los sistemas específicos con un token JWT.
2.  **HIS (Hospital Information System):** Sistema central que gestiona historias clínicas y crea órdenes.
3.  **LIS (Laboratory Information System):** Recibe órdenes de laboratorio, procesa muestras y envía resultados al HIS.
4.  **RIS (Radiology Information System):** Recibe órdenes radiológicas, gestiona agenda, vincula imágenes (Orthanc) y envía informes al HIS.

La base de datos está segregada (una por servicio), lo que evita el acoplamiento a nivel de datos.

---

## 4. Diferencias entre Especificación e Implementación (Resumen)

Se han identificado las siguientes diferencias clave (se detallan en `spec_vs_implementation.md`):
- **Monorepo:** El README los describe como servicios independientes, pero están contenidos en un solo repositorio y comparten `package.json`.
- **Brackets de Puertos:** Los puertos están fijos en el README, pero se leen de variables de entorno en el código.
- **Opcionalidad de Orthanc:** El README dice que es opcional, pero el código de RIS arroja error si no está disponible al realizar búsquedas.

---

## 5. Problemas Encontrados y Deuda Técnica

- **Falta de Pruebas Automatizadas:** No se encontraron pruebas unitarias ni de integración.
- **Manejo de Errores en Integración:** En HIS, si el envío de la orden a LIS/RIS falla, el error se captura y se registra, pero el flujo continúa y la orden queda creada en HIS sin estar en el sistema destino.
- **Dependencia de Orthanc:** El RIS depende fuertemente de que Orthanc esté activo para ciertas rutas, sin un manejo de contingencia claro si está caído.

---

## 6. Recomendaciones de Mejora

1.  **Implementar Pruebas Unitarias e Integración:** Para validar el sistema sin necesidad de levantarlo completo.
2.  **Mejorar el Manejo de Errores en la Integración:** Implementar un sistema de reintentos (Retry) o notificar al usuario si la integración falla.
3.  **Documentar el Enfoque de Monorepo:** Aclarar que es un monorepo para desarrollo y cómo se desplegaría en producción.
4.  **Hacer la integración con Orthanc realmente opcional:** Usar flags de configuración para habilitar/deshabilitar la búsqueda si no se cuenta con el servicio.
