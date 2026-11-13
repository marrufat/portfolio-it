# Caso de Estudio: Auditoría de Salud, Telemetría y Optimización de Portátil HP 14

## 1. Resumen del Equipo y Objetivo
* **Equipo analizado:** Portátil HP 14 (`hp14`)
* **Procesador y Gráficos:** Intel Core Ultra (i7) con NPU integrada e Intel(R) Graphics (arquitectura Arc/Xe)
* **Memoria RAM:** 16 GB
* **Sistema Operativo:** Windows 11 (Build 26300)
* **Objetivo de la intervención:** Realizar una auditoría preventiva de hardware, verificar la integridad térmica y del almacenamiento, y resolver un conflicto de telemetría a nivel de controlador gráfico durante el diagnóstico.

---

## 2. Diagnóstico de Hardware y Estado de Salud

### Almacenamiento (Análisis S.M.A.R.T. con CrystalDiskInfo)
* **Estado de salud general:** Bueno (100%)
* **Temperatura de operación en reposo:** 36 °C
* **Horas de encendido totales:** 945 horas
* **Conclusión del almacenamiento:** La unidad SSD no presenta sectores reasignados ni errores críticos de lectura/escritura.

### Control Térmico y Rendimiento (Telemetría con HWiNFO64)
* **Temperatura CPU en reposo (Idle):** 46 °C (valores estables en los núcleos)
* **Temperatura CPU bajo carga:** 54 °C
* **Frecuencias de reloj:** Tras la monitorización, el equipo se mantiene ampliamente alejado de los umbrales críticos de temperatura (habitualmente entre 90 °C y 100 °C). Esto demuestra que el sistema de disipación interna opera de manera eficiente, evitando cualquier mecanismo de protección por estrangulamiento térmico que reduzca las frecuencias de reloj (GHz) de la CPU.

---

## 3. Resolución de Incidencias Técnicas Durante la Auditoría
Durante la fase de recolección de datos con **HWiNFO64 (v8.60)**, el software experimentaba un cierre inesperado (*crash*) exactamente durante la fase de **Detección de GPU**.

1. **Diagnóstico:** Mediante PowerShell (`Get-CimInstance Win32_VideoController`) se verificó que el controlador gráfico OEM de Intel (`32.0.101.8424`) estaba actualizado y operativo, descartando un fallo físico. Se aisló el conflicto en la inicialización de la librería de telemetría **Intel oneAPI** sobre la GPU integrada moderna.
2. **Solución aplicada:** Se desactivó el sondeo de **Soporte oneAPI** y del **Embedded Controller (EC)** en los ajustes de seguridad (*Safety*) de HWiNFO, permitiendo una lectura limpia y estable de todos los sensores sin alterar el controlador gráfico OEM del fabricante.

---

## 4. Optimización del Sistema y Conclusiones
* **Gestión de arranque:** Se auditaron las aplicaciones de inicio en Windows 11, deshabilitando servicios innecesarios en segundo plano para agilizar el tiempo de carga del escritorio y reducir el consumo base de memoria RAM.
* **Veredicto técnico:** Equipo en óptimas condiciones operativas, con sistema de disipación eficiente y almacenamiento saludable.