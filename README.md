**guía rápida de SAP para almacenamiento** (módulo **WM - Warehouse Management** o **MM - Materials Management**), con comandos clave y ejemplos de uso:

---

### **1. Transacciones clave para almacenamiento**

| **Transacción** | **Descripción** | **Uso común** |
|------------------|-----------------|----------------|
| **MIGO**         | Gestión de movimientos de mercancía | Entradas/salidas de mercancía, transferencias. |
| **MB1A**         | Retirada de material (consumo) | Retirar stock por daños o mermas. |
| **MB1B**         | Transferencia de stock | Movimientos entre almacenes o ubicaciones. |
| **LT01**         | Crear orden de transporte (TO) | Mover mercancía dentro del almacén. |
| **LT03**         | Confirmar orden de transporte | Registrar la ejecución de una TO. |
| **LS01N**        | Gestión de ubicaciones | Consultar o gestionar ubicaciones de almacén. |
| **MI31**         | Crear documento de inventario físico | Iniciar un conteo físico. |
| **MI32**         | Introducir resultados de inventario | Registrar cantidades contadas. |
| **MI33**         | Analizar diferencias de inventario | Comparar conteo físico vs. sistema. |
| **VL06O**        | Monitor de entregas pendientes | Verificar órdenes de entrega pendientes. |
| **MB51**         | Lista de movimientos de material | Histórico de entradas/salidas. |
| **MB52**         | Stock en almacén | Consultar stock disponible por ubicación. |

---

### **2. Pasos básicos para operaciones comunes**

#### **A. Recepción de mercancía (Goods Receipt)**
1. **Transacción: `MIGO`**  
   - Seleccionar **"Goods Receipt"** → **"Purchase Order"** (si hay orden de compra).  
   - Ingresar **número de pedido (PO)** y validar.  
   - Confirmar cantidad y ubicación de almacén (campo **"Storage Location"**).  
   - Guardar con **Enter** y luego **"Post"** (se genera un **Material Document Number**).

#### **B. Preparar envío (Goods Issue)**
1. **Transacción: `MIGO`**  
   - Seleccionar **"Goods Issue"** → **"A01: Delivery to Customer"**.  
   - Ingresar **número de entrega (Delivery)** o material.  
   - Especificar cantidad y ubicación de salida.  
   - **Post** para registrar la salida.

#### **C. Movimiento interno (Transferencia)**
1. **Transacción: `LT01`**  
   - Ingresar **material**, **cantidad**, **ubicación origen** y **ubicación destino**.  
   - Guardar para generar la **orden de transporte (TO Number)**.  
2. **Transacción: `LT03`**  
   - Ingresar el **TO Number** y confirmar el movimiento.

#### **D. Inventario físico**
1. **Transacción: `MI31`** → Crear documento de conteo.  
2. **Transacción: `MI32`** → Ingresar resultados del conteo.  
3. **Transacción: `MI33`** → Analizar y ajustar diferencias.

---

### **3. Comandos rápidos y atajos**
- **/n + código de transacción**: Abrir una transacción nueva (ej: `/nMIGO`).  
- **/o**: Abrir una nueva sesión sin cerrar la actual.  
- **/nex**: Salir de SAP inmediatamente.  
- **F4**: Ayuda para buscar datos (ej: códigos de material).  
- **F1**: Ayuda contextual (información sobre un campo).  
- **F8**: Ejecutar un reporte (ej: en MB51 o MB52).  
- **Ctrl + Y**: Borrar una línea en una tabla.  

---

### **4. Consejos para evitar errores**
- **Verifica siempre**:  
  - **Número de material**, **cantidad** y **ubicación**.  
  - **MovType** (tipo de movimiento: 101 = entrada, 261 = salida, etc.).  
- Usa **F1** para entender el significado de los campos.  
- Si hay un error, revisa los **mensajes en la barra inferior** (ej: "Material X no existe en la ubicación Y").  

---

### **5. Ejemplo práctico: Registrar entrada de mercancía**
1. Ejecuta `/nMIGO`.  
2. Selecciona **"Goods Receipt"** → **"Purchase Order"**.  
3. Ingresa el **PO Number** y haz clic en **"Enter"**.  
4. Verifica los datos automáticos (material, cantidad).  
5. Completa la **Storage Location** (ej: "0001").  
6. Haz clic en **"Post"** (✓).  
7. Anota el **Material Document Number** generado (ej: "4900001234").  

---

### **6. Documentación adicional**
- **MMBE**: Consultar niveles de stock por material.  
- **COGI**: Gestión de inconsistencias en almacén.  
- **LS24**: Lista de órdenes de transporte pendientes.  

---

### **Nota importante**  
Los comandos pueden variar según la versión de SAP y la configuración de la empresa.  
¡Siempre verifica con tu supervisor o manual interno los procesos específicos de tu organización!