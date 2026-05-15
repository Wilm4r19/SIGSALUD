# Reporte de Validación de IA - SIGSALUD

Este documento registra los errores detectados en las respuestas de la IA durante el análisis y cómo fueron corregidos por el equipo humano.

## Error 1: Asunción de Archivos de Prueba inexistentes
- **Prompt usado:** ¿Qué framework de pruebas se utiliza en el proyecto según el `package.json`?
- **Resultado IA:** La IA respondió que se usaba `jest` porque vio un proyecto Node.js estándar.
- **Error encontrado:** En el `package.json` no hay ninguna dependencia de pruebas (`jest`, `mocha`, etc.) ni scripts de test.
- **Corrección humana:** Se corrigió a la IA indicando que el proyecto carece de pruebas automatizadas, lo que se marcó como Deuda Técnica Alta.

## Error 2: Confusión en la relación con Orthanc
- **Prompt usado:** ¿Cómo se integra el LIS con Orthanc?
- **Resultado IA:** La IA afirmó que el LIS consulta imágenes en Orthanc para validar las muestras.
- **Error encontrado:** Según el README y el código, es el **RIS** (Radiología) el que se integra con Orthanc, no el LIS.
- **Corrección humana:** Se aclaró que la integración con PACS/Orthanc es exclusiva del módulo RIS para estudios radiológicos.

## Error 3: Alucinación sobre la base de datos compartida
- **Prompt usado:** ¿Cómo comparten datos los servicios si están en el mismo repositorio?
- **Resultado IA:** La IA sugirió que los servicios hacían consultas `JOIN` entre las bases de datos porque estaban en el mismo servidor PostgreSQL.
- **Error encontrado:** El README prohíbe explícitamente consultas directas entre bases de datos ("No se deben hacer consultas directas..."). El código usa API REST para la integración.
- **Corrección humana:** Se instruyó a la IA a buscar las llamadas `fetch` en el código que demuestran la integración por API REST, respetando el aislamiento de bases de datos.
