# Iteraciones de Prompts - SIGSALUD

Este documento detalla los prompts específicos utilizados durante las diferentes fases del análisis.

## 1. Prompt para Entender la Estructura (Momento 1)
**Propósito:** Obtener una visión general de los archivos y carpetas del proyecto.
**Prompt:**
```markdown
Muestra la estructura de directorios del proyecto en la raíz y dentro de la carpeta `src`. ¿Qué tipo de arquitectura sugiere esta estructura?
```

## 2. Prompt para Analizar el Servicio Auth (Momento 1)
**Propósito:** Entender cómo funciona la autenticación centralizada.
**Prompt:**
```markdown
Lee el archivo `src/auth/app.js` y `src/auth/services/authService.js`. Explica cómo se valida el usuario y cómo se redirige a los otros sistemas.
```

## 3. Prompt para Analizar la Integración HIS -> LIS (Momento 1)
**Propósito:** Entender el flujo de envío de órdenes.
**Prompt:**
```markdown
Busca en el código de HIS cómo se envía una orden al LIS. ¿Qué protocolo usa y qué datos se envían?
```

## 4. Prompt para Identificar Gaps (Momento 2)
**Propósito:** Encontrar diferencias entre el README y el código.
**Prompt:**
```markdown
Compara las rutas listadas en la sección 13 del `README_SIGSALUD.md` con los archivos de rutas en `src/his/routes/`. ¿Falta alguna ruta o hay rutas adicionales?
```

## 5. Prompt para Generar Diagramas (Momento 3)
**Propósito:** Crear código Mermaid para los diagramas requeridos.
**Prompt:**
```markdown
Genera un diagrama de secuencia en Mermaid que muestre el flujo completo desde que un médico crea una orden en HIS hasta que el LIS envía el resultado validado de vuelta.
```
