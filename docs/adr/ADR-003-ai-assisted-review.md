# ADR-003: Uso de IA con validación humana estricta

## Estado
Aceptado

## Contexto
La actividad exige el uso de IA para el análisis, pero advierte sobre el riesgo de alucinaciones y la necesidad de criterio técnico propio.

## Decisión
Utilizar la IA para acelerar la lectura de código y la generación de borradores de documentos y diagramas (Mermaid), pero someter cada afirmación a validación contra el código fuente real antes de incluirla en los entregables.

## Alternativas consideradas
- **No usar IA:** No cumple con el requerimiento de la actividad.
- **Confiar ciegamente en la IA:** Alto riesgo de incluir errores (como los documentados en el reporte de validación).

## Consecuencias
- **Positivas:** Mayor velocidad de entrega y consistencia en los formatos.
- **Negativas:** Requiere tiempo adicional para verificar cada respuesta.

## Riesgos
- Que el equipo pase por alto una alucinación sutil de la IA.
