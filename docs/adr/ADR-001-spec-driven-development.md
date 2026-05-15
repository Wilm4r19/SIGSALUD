# ADR-001: Adopción de Spec-Driven Development para la revisión

## Estado
Aceptado

## Contexto
El equipo recibió el proyecto SIGSALUD sin documentación de diseño detallada más allá del `README_SIGSALUD.md`. Para evaluar si el sistema es mantenible y comprensible, se debe aplicar un enfoque sistemático.

## Decisión
Adoptar el enfoque de **Spec-Driven Development Review** para realizar la ingeniería inversa y contrastar la especificación con la implementación. Esto implica usar el README como la "especificación" de referencia y validar el código contra ella.

## Alternativas consideradas
- **Análisis de código libre:** Leer el código sin una especificación de referencia. Se descartó porque no permite medir la brecha entre lo que se dice que hace y lo que hace.
- **Pruebas dinámicas:** Ejecutar el sistema y probarlo. Se descartó por la restricción de "no ejecutar código" dada por el usuario.

## Consecuencias
- **Positivas:** Permite identificar inconsistencias y documentación desactualizada.
- **Negativas:** La calidad del análisis depende de qué tan completa sea la especificación inicial (README).

## Riesgos
- Alucinaciones de la IA si se usa para inferir comportamiento sin validar con el código.
