# Comparación Spec vs Implementación - SIGSALUD

Este documento detalla las diferencias encontradas entre la especificación disponible (principalmente el `README_SIGSALUD.md`) y la implementación real del sistema.

## 1. Tabla Comparativa de Diferencias

| Elemento | Especificación esperada | Implementación real | Gap identificado |
| :--- | :--- | :--- | :--- |
| **Arquitectura** | Cuatro servicios Node.js independientes. | Monorepo con un solo `package.json` y dependencias compartidas. | No se especifica que es un monorepo, lo que cambia la estrategia de despliegue. |
| **Configuración** | URLs fijas en el README (ej. `http://localhost:8001`). | El código usa variables de entorno (`process.env.HIS_PORT`). | El README sugiere que los puertos son fijos, pero el código es dinámico. |
| **Integración Orthanc** | Orthanc es opcional para pruebas RIS/PACS. | El código de RIS intenta conectar a Orthanc y falla si no responde. | No hay manejo de contingencia para cuando Orthanc no está disponible. |
| **Manejo de Errores** | El flujo de integración debe ser confiable. | En HIS, si falla el envío de la orden, solo se loguea el error. | Riesgo de inconsistencia de datos entre sistemas si falla la red. |
| **Pruebas** | Debe validarse que funciona correctamente. | No hay archivos de pruebas (unitarias, integración, etc.). | No hay forma automatizada de validar el sistema según la especificación. |

---

## 2. Tabla de Deuda Técnica

| Problema | Descripción | Impacto | Severidad |
| :--- | :--- | :--- | :--- |
| **Falta de Pruebas** | No existen pruebas automatizadas en el proyecto. | Dificulta la validación de cambios y la integración continua. | Alta |
| **Manejo de Errores Silencioso** | En `orderDispatchService.js`, los errores de red se capturan y solo se loguean, sin detener el flujo ni reintentar. | Puede resultar en órdenes creadas en HIS pero no enviadas a LIS/RIS. | Alta |
| **Acoplamiento por Monorepo** | Aunque los servicios son independientes en código, comparten el mismo `node_modules` y `package.json`. | Dificulta el despliegue independiente de cada servicio en contenedores separados. | Media |
| **Dependencia de Orthanc** | El flujo de RIS asume que Orthanc está disponible si se usa la funcionalidad de búsqueda. | Fallos en la interfaz de usuario si el servicio externo no responde. | Media |
| **Falta de Documentación de API** | No hay Swagger ni documento de diseño de API REST más allá de las tablas en el README. | Dificulta que otros equipos consuman las APIs sin leer el código fuente. | Media |
