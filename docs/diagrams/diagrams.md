# Diagramas Técnicos - SIGSALUD

Este documento contiene los diagramas requeridos por la actividad, construidos en formato Mermaid para facilitar su visualización y trazabilidad con el código.

## 1. Diagrama de Componentes

Muestra la estructura del sistema y cómo se comunican los servicios.

```mermaid
graph TD
    User([Usuario/Navegador]) -->|HTTP| Auth[Servicio Auth :8007]
    User -->|HTTP| HIS[Servicio HIS :8001]
    User -->|HTTP| LIS[Servicio LIS :8002]
    User -->|HTTP| RIS[Servicio RIS :8003]

    Auth -->|PostgreSQL| DB_Auth[(sigsalud_auth)]
    HIS -->|PostgreSQL| DB_HIS[(sigsalud_his)]
    LIS -->|PostgreSQL| DB_LIS[(sigsalud_lis)]
    RIS -->|PostgreSQL| DB_RIS[(sigsalud_ris)]

    HIS <-->|API REST / JSON| LIS
    HIS <-->|API REST / JSON| RIS
    RIS -->|API REST| Orthanc[(Orthanc PACS)]
```

---

## 2. Diagrama de Actividades (Flujo Principal)

Muestra el flujo de una orden desde su creación hasta la visualización de resultados.

```mermaid
sequenceDiagram
    actor Medico
    actor Bacteriologo
    participant HIS
    participant LIS

    Medico->>HIS: Crea Orden de Laboratorio
    HIS->>HIS: Guarda Orden (Estado: CREADA)
    HIS->>LIS: Envía Orden por API REST
    activate LIS
    LIS->>LIS: Crea Orden en LIS (Estado: RECIBIDA)
    LIS-->>HIS: Confirmación (OK)
    deactivate LIS
    HIS->>HIS: Actualiza Estado: RECIBIDA

    Note over LIS: El bacteriólogo procesa la muestra...
    Bacteriologo->>LIS: Registra y Valida Resultados
    LIS->>HIS: Envía Resultados Validados por API REST
    activate HIS
    HIS->>HIS: Guarda en resultados_diagnosticos
    HIS-->>LIS: Confirmación (OK)
    deactivate HIS

    Medico->>HIS: Consulta Historia Clínica
    HIS-->>Medico: Muestra Resultados
```

---

## 3. Diagrama de Estados (Orden Diagnóstica)

Muestra los estados por los que pasa una orden en el sistema.

```mermaid
stateDiagram-v2
    [*] --> CREADA: Médico crea orden en HIS
    CREADA --> RECIBIDA: HIS envía a LIS/RIS y confirma
    RECIBIDA --> EN_PROCESO: LIS/RIS toma muestra o inicia estudio
    EN_PROCESO --> INFORMADA: Resultados registrados
    INFORMADA --> VALIDADA: Profesional valida resultados
    VALIDADA --> [*]: Enviada a HIS y archivada
```

---

## 4. Diagrama de Entidades (Modelo de Datos Simplificado)

Muestra las relaciones principales inferidas del código y consultas SQL.

```mermaid
erDiagram
    PACIENTE ||--o{ ORDEN_DIAGNOSTICA : tiene
    ORDEN_DIAGNOSTICA ||--o{ DETALLE_ORDEN : contiene
    DETALLE_ORDEN }|--|| PROCEDIMIENTO_CUPS : referencia
    ORDEN_DIAGNOSTICA ||--o{ RESULTADO_DIAGNOSTICO : produce

    subgraph HIS_DB
        PACIENTE
        ORDEN_DIAGNOSTICA
        DETALLE_ORDEN
        PROCEDIMIENTO_CUPS
        RESULTADO_DIAGNOSTICO
    end

    subgraph LIS_DB
        ORDEN_LABORATORIO {
            int id
            int his_order_id
            string numero_orden_his
        }
        RESULTADO_LABORATORIO {
            int id
            int orden_id
            string resultado
        }
        ORDEN_LABORATORIO ||--o{ RESULTADO_LABORATORIO : contiene
    end

    subgraph RIS_DB
        ORDEN_RADIOLOGIA {
            int id
            int his_order_id
        }
        ESTUDIO_RADIOLOGICO {
            int id
            int orden_id
            string descripcion
        }
        INFORME_RADIOLOGIA {
            int id
            int estudio_id
            string conclusion
        }
        ORDEN_RADIOLOGIA ||--o{ ESTUDIO_RADIOLOGICO : genera
        ESTUDIO_RADIOLOGICO ||--|| INFORME_RADIOLOGIA : produce
    end
```
