# Fase 1: Diseño del Flujo y Toma de Decisiones

# 1. Justificación de Calidad de Datos

En un entorno bancario, la calidad de los datos es un requisito fundamental para garantizar la confiabilidad de los análisis de fraude y la correcta toma de decisiones de negocio. Los modelos analíticos y las alertas operativas dependen directamente de la consistencia de la información procesada.
![Transferencias Bancarias](assets/img/transferencia.jpg)
La eliminación de registros duplicados evita que una misma transacción sea contabilizada múltiples veces, lo que podría generar falsos positivos en los análisis de comportamiento financiero y distorsionar métricas operativas.

La corrección de valores faltantes en transacciones rechazadas permite mantener la integridad semántica de los datos. Dado que una transacción rechazada no representa un movimiento financiero efectivo, asignar un monto de 0.0 estandariza el tratamiento de estos registros y evita inconsistencias posteriores.

La clasificación de montos inusuales facilita la identificación temprana de patrones de riesgo. Las transacciones internacionales de alto valor representan un escenario común de monitoreo dentro de los sistemas de prevención de fraude y cumplimiento normativo.

Finalmente, el aislamiento exclusivo de transacciones aprobadas para el análisis de anomalías garantiza que los modelos trabajen únicamente sobre movimientos financieros reales. Incluir operaciones rechazadas o pendientes podría introducir ruido analítico y afectar la precisión de los resultados.
![transferencia electronica](assets/img/transferencia.jpg)

# 2. Decisiones de Arquitectura

La solución propuesta implementa una arquitectura ETL ligera orientada a procesamiento batch diario.

Aunque el archivo suministrado para la prueba es pequeño, el diseño considera un contexto bancario real donde diariamente pueden procesarse cientos de miles o incluso millones de transacciones. Diversos reportes del sector financiero salvadoreño indican que la banca digital del país concentra entre aproximadamente 725,000 y 1.5 millones de usuarios activos diarios considerando el ecosistema bancario nacional.

Por esta razón, la arquitectura incorpora componentes ampliamente utilizados en entornos empresariales:

- Apache Airflow para la orquestación y programación de pipelines.
- Pandas para la transformación y limpieza de datos.
- PostgreSQL (Supabase) como repositorio persistente de datos procesados.
- SQL para consultas analíticas y generación de reportes.

Apache Airflow permite definir DAGs que automatizan la ejecución diaria del proceso, proporcionando monitoreo, reintentos automáticos, trazabilidad y escalabilidad operativa. Estas características son especialmente relevantes en sistemas financieros donde la disponibilidad y confiabilidad del procesamiento son críticas.

Pandas ofrece una implementación eficiente para el volumen esperado de la prueba técnica y simplifica la aplicación de reglas de calidad de datos mediante operaciones vectorizadas.

PostgreSQL fue seleccionado por ser un motor relacional maduro, ampliamente utilizado en entornos empresariales, con soporte para transacciones ACID, índices, particionamiento y capacidades analíticas avanzadas. La utilización de Supabase permite disponer de una instancia PostgreSQL administrada reduciendo la complejidad operativa del despliegue.

La arquitectura también permite una evolución futura hacia tecnologías de procesamiento distribuido como Apache Spark si el volumen de datos creciera significativamente.

# Tecnologías Utilizadas

- Apache Airflow
- Python 3.x
- Pandas
- SQLAlchemy
- PostgreSQL (Supabase)

# Justificación de las Librerías

Pandas:
Aplicación de reglas de calidad, transformación y enriquecimiento de datos mediante operaciones eficientes sobre DataFrames.

SQLAlchemy:
Abstracción segura y mantenible para la conexión y carga de información hacia PostgreSQL.

Apache Airflow:
Orquestación, monitoreo, programación y observabilidad del pipeline de datos.

PostgreSQL (Supabase):
Persistencia transaccional y soporte para consultas analíticas posteriores.
![diagrama de flujo](assets/img/diagrama_flujo.png)
