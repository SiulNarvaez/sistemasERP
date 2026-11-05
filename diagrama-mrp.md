```mermaid
graph TD
    %% Evento de Inicio
    START((Inicio: Corrida MRP))
    
    %% Actividades Principales
    MRP_RUN(Ejecutar explosión de materiales MRP)
    CALC_DATE(Calcular fecha de inicio restando Lead Time a Fecha de Necesidad)
    
    %% Compuerta de Decisión
    DECISION{¿Fecha calculada es retroactiva?}
    
    %% Subprocesos / Actividades de Bifurcación
    SUB_YES(Reprogramar fecha de inicio al día actual)
    ALERT(Generar alerta y etiquetar orden como urgente/retraso)
    SUB_NO(Mantener fecha calculada y escalonar Lista de Materiales)
    
    %% Evento Final
    END_YES((Fin: Orden Urgente))
    END_NO((Fin: Orden Estándar))
    
    %% Relaciones y Flujo
    START --> MRP_RUN
    MRP_RUN --> CALC_DATE
    CALC_DATE --> DECISION
    
    %% Flujo si es pasado
    DECISION -->|Sí, es fecha pasada| SUB_YES
    SUB_YES --> ALERT
    ALERT --> END_YES
    
    %% Flujo si es válido
    DECISION -->|No, es fecha futura| SUB_NO
    SUB_NO --> END_NO
    
    %% Estilos BPMN
    classDef startEvent fill:#81C784,stroke:#2E7D32,stroke-width:2px;
    classDef endEvent fill:#E57373,stroke:#C62828,stroke-width:3px;
    classDef activity fill:#BBDEFB,stroke:#1565C0,stroke-width:2px;
    classDef gateway fill:#FFE082,stroke:#FF8F00,stroke-width:2px;
    
    class START startEvent;
    class END_YES,END_NO endEvent;
    class MRP_RUN,CALC_DATE,SUB_YES,ALERT,SUB_NO activity;
    class DECISION gateway;
