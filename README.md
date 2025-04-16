
## **GUÍA DE SAP PARA ALMACENAMIENTO (Módulo WM/MM)**

---

### **1. Recepción de Mercancía (Goods Receipt - GR)**  
**Transacción: `MIGO`**  
**Propósito:** Registrar la entrada de materiales al almacén (ej: desde proveedores o producción).  

**Pasos:**  
1. **Abrir transacción:**  
   - Escribe `/nMIGO` en la barra de comandos y pulsa **Enter**.  
2. **Seleccionar parámetros:**  
   - En **"Material Document"**, elige:  
     - **Movement Type:** `101` (Entrada de compra).  
     - **Document Date:** Fecha actual.  
     - **Posting Date:** Fecha de contabilización.  
   - Haz clic en **Purchase Order** y escribe el número de pedido (ej: `4500001234`).  
3. **Verificar datos:**  
   - SAP cargará automáticamente los materiales y cantidades del pedido.  
   - Revisa que coincidan con la mercancía recibida (ej: 100 botellas de vino).  
4. **Especificar ubicación:**  
   - En la columna **"Storage Location"**, ingresa la ubicación de destino (ej: `0001` para almacén principal).  
   - Si el material requiere un lote, usa **F4** para seleccionarlo.  
5. **Confirmar y guardar:**  
   - Pulsa **Enter** y luego **"Post"** (icono de disco).  
   - **¡Listo!** Anota el **Material Document Number** generado (ej: `4900005678`).

**Errores comunes:**  
- Si aparece **"Material X no existe en la ubicación Y"**, verifica que el material esté asignado a la ubicación en el maestro de materiales (transacción `MM02`).  

---

### **2. Preparar Envío (Goods Issue - GI)**  
**Transacción: `MIGO`**  
**Propósito:** Registrar la salida de materiales (ej: ventas, devoluciones).  

**Pasos:**  
1. **Abrir transacción:**  
   - Ejecuta `/nMIGO`.  
2. **Configurar movimiento:**  
   - Selecciona **"Goods Issue"** → **Movement Type:** `601` (Entrega a cliente).  
   - Ingresa el **número de entrega** (Delivery) o el material directamente.  
3. **Detallar la salida:**  
   - En **"Material"**, escribe el código (ej: `VINO_ROJO_001`).  
   - En **"Quantity"**, ingresa la cantidad a despachar (ej: `50` unidades).  
   - En **"Storage Location"**, confirma la ubicación de salida (ej: `0001`).  
4. **Validar y guardar:**  
   - Usa **F8** para simular el movimiento antes de guardar.  
   - Si todo está correcto, haz clic en **"Post"**.  

**Ejemplo práctico:**  
- **MovType 261**: Para retirar material dañado (ej: 10 botellas rotas).  
  - En **"Movement Type"**, usa `261` y selecciona la razón (ej: `RA1` para "Daño en almacén").  

---

### **3. Transferir Materiales Internamente (Orden de Transporte - TO)**  
**Transacciones: `LT01` (crear) y `LT03` (confirmar)**  
**Propósito:** Mover stock entre ubicaciones (ej: de zona de recepción a estanterías).  

**Pasos:**  
1. **Crear orden de transporte (`LT01`):**  
   - Ejecuta `/nLT01`.  
   - Completa los campos:  
     - **Material:** `VINO_BLANCO_002`.  
     - **Cantidad:** `200`.  
     - **Ubicación origen:** `RECEPCION`.  
     - **Ubicación destino:** `ESTANTERIA_A1`.  
   - Pulsa **Enter** para generar el **TO Number** (ej: `TO123456`).  

2. **Confirmar movimiento (`LT03`):**  
   - Ejecuta `/nLT03`.  
   - Ingresa el **TO Number** y haz clic en **"Confirmation"**.  
   - Verifica que la cantidad y ubicaciones sean correctas.  
   - Guarda con **"Save"**.  

**Nota:** Si el movimiento requiere varios pasos (ej: picking → packing), usa `LT0C` para confirmaciones parciales.  

---

### **4. Realizar Inventario Físico**  
**Transacciones: `MI31` (crear), `MI32` (registrar), `MI33` (ajustar)**  
**Propósito:** Asegurar que el stock físico coincida con SAP.  

**Proceso completo:**  
1. **Crear documento de inventario (`MI31`):**  
   - Ejecuta `/nMI31`.  
   - Selecciona **"Plant"** (ej: `PL001`) y **"Storage Location"** (ej: `0001`).  
   - Elige el tipo de conteo (ej: `01` para conteo cíclico).  
   - Genera el **Documento de Inventario** (ej: `INV_202310`).  

2. **Contar y registrar (`MI32`):**  
   - Ejecuta `/nMI32`.  
   - Ingresa el **Documento de Inventario** y el material.  
   - Anota la cantidad física contada (ej: `950` botellas).  
   - Guarda los resultados.  

3. **Analizar diferencias (`MI33`):**  
   - Ejecuta `/nMI33`.  
   - Ingresa el documento y revisa las diferencias.  
   - Si hay discrepancias, ajusta el stock con **"Post Difference"**.  

**Consejo:** Usa **MB52** antes del conteo para ver el stock según SAP y comparar.  

---

### **5. Consultar Stock en Tiempo Real**  
**Transacción: `MB52`**  
**Propósito:** Verificar disponibilidad de materiales por ubicación.  

**Pasos:**  
1. Ejecuta `/nMB52`.  
2. Completa los filtros:  
   - **Material:** `VINO_TINTO_003`.  
   - **Planta:** `PL001`.  
   - **Storage Location:** `0001`.  
3. Pulsa **F8** para ejecutar el reporte.  
4. **Resultado:** Verás columnas como:  
   - **Unrestricted Stock:** Stock disponible.  
   - **Blocked Stock:** Material bloqueado (ej: en cuarentena).  

---

### **6. Movimiento Rápido de Stock (Transfer Posting)**  
**Transacción: `MB1B`**  
**Propósito:** Transferir material entre ubicaciones sin orden de transporte.  

**Pasos:**  
1. Ejecuta `/nMB1B`.  
2. Selecciona **Movement Type:** `311` (Transferencia entre ubicaciones).  
3. Ingresa:  
   - **Material:** `VINO_ROSADO_004`.  
   - **Cantidad:** `100`.  
   - **Ubicación origen:** `ESTANTERIA_A1`.  
   - **Ubicación destino:** `ZONA_PROMO`.  
4. Guarda con **"Post"**.  

**Nota:** Este movimiento no genera una orden de transporte (TO), por lo que es ideal para correcciones rápidas.  

---

### **7. Gestionar Errores con COGI**  
**Transacción: `COGI`**  
**Propósito:** Resolver inconsistencias en el almacén (ej: diferencias entre stock físico y SAP).  

**Pasos:**  
1. Ejecuta `/nCOGI`.  
2. Filtra por ubicación o material.  
3. Selecciona la inconsistencia y haz clic en **"Process"**.  
4. Corrige el error (ej: ajustar stock o eliminar entradas duplicadas).  

---

### **8. Atajos y Recomendaciones Clave**  
- **F1 + Click en campo:** Muestra ayuda detallada sobre un campo específico.  
- **MB51:** Histórico de movimientos (ej: ver todas las salidas de un material).  
- **LS26:** Monitor de stock en tiempo real por ubicación.  
- **Siempre:**  
  - Valida **Movement Type** antes de guardar.  
  - Usa **F8** para simular transacciones críticas.  

---

### **Ejemplo Integrado: Recepción + Almacenamiento**  
1. **Recibir 500 botellas de vino:**  
   - Usa `MIGO` (MovType `101`) con PO `4500005678`.  
2. **Transferir a estantería:**  
   - Crea TO en `LT01` desde `RECEPCION` a `ESTANTERIA_B2`.  
3. **Verificar stock:**  
   - Ejecuta `MB52` para confirmar que las 500 unidades están en `ESTANTERIA_B2`.  

---

### **Diagrama de Flujo de Operaciones**  
```
Recepción (MIGO) → Transferir (LT01/LT03) → Inventario (MI31/MI33) → Envío (MIGO)
```