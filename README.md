# 📘 Manual de Uso – Sistema de Renombrado Inteligente

<img src="https://daiderd.com/nix-darwin/images/nix-darwin.png" width="200px" alt="logo" />

## 📌 ¿Qué hace este sistema?

El **Sistema de Renombrado Inteligente** automatiza el cambio de nombre de los archivos de tus proveedores usando la información de los registros contables del mes.
En lugar de renombrar archivo por archivo, el sistema lo hace por ti.

---

## 📂 Requisitos previos

Antes de comenzar, asegúrate de tener:

### 1. **Archivo `proveedores.xlsx`**

Debe contener:

| Columna | Contenido                   | Ejemplo           |
| ------- | --------------------------- | ----------------- |
| A       | Carpeta padre               | `Banco`, `Hotel`  |
| B       | Nombre exacto del proveedor | `BANCO BBVA PERU` |
| C       | Código de proveedor         | `270-VEN00001476` |
| E       | Procesar (1=Sí, 0=No)       | `1`               |

---

### 2. **Archivo de registros del mes**

Excel con movimientos del mes:

| Columna | Contenido                                             | Ejemplo           |
| ------- | ----------------------------------------------------- | ----------------- |
| A       | Código de proveedor (igual al archivo de proveedores) | `270-VEN00001476` |
| B       | Nº de voucher / comprobante                           | `270-PIV00015211` |
| C       | Nº de factura (formato estándar)                      | `01F001-00006429` |

---

### 3. **Carpetas organizadas**

Tus carpetas deben tener el **mismo nombre exacto** que en el archivo `proveedores.xlsx`.

---

## 🛠 Cómo usar el sistema

### **PASO 1 – Iniciar el proceso**

Haz clic en **"Renombrado Inteligente con Data"** y sigue las instrucciones.

---

### **PASO 2 – Verificar archivo de proveedores**

* El sistema buscará `proveedores.xlsx` en la carpeta del programa.
* Si no lo encuentra, pedirá que lo selecciones.
* Confirmará cuántos proveedores fueron cargados.

---

### **PASO 3 – Seleccionar registros del mes**

* Elige el archivo Excel con los registros contables.
* El sistema mostrará cuántos registros se cargaron.

---

### **PASO 4 – Seleccionar carpeta de trabajo**

* Elige la carpeta principal donde están todas las subcarpetas de proveedores.
* El sistema analizará todo el contenido automáticamente.

---

### **PASO 5 – Revisión automática**

El sistema:

* Localiza carpetas de proveedores.
* Revisa todos los archivos.
* Compara nombres con los registros del mes.
* Genera una **lista preliminar** de archivos a renombrar.

---

### **PASO 6 – Reporte de resultados**

Se abrirá un archivo de texto con:

* 📊 Archivos que se pueden renombrar.
* ❌ Archivos sin coincidencia.
* 🗂 Tabla: **nombre actual → nuevo nombre**.
* ⚠ Lista de errores o motivos por los que no se procesaron ciertos archivos.

---

### **PASO 7 – Confirmación**

Si estás de acuerdo con el reporte, responde **"Sí"** para continuar.

---

### **PASO 8 – Ejecución**

El sistema:

* Renombra automáticamente los archivos.
* Muestra un resumen de la operación.
* Pregunta si quieres abrir la carpeta de resultados.

---

## 📄 Tipos de archivos compatibles

* **PDF, XML, ZIP, RAR, JPG, JPEG, PNG**

---

## 📝 Formato del nuevo nombre

```
Número_de_Voucher-Número_de_Factura Nombre_del_Proveedor.extensión
```

**Ejemplo:**

```
270-PIV00015211-01F001-00006429 BANCO BBVA PERU.pdf
```

---

## 💡 Consejos importantes

> **💡 TIP:** Siempre revisa el reporte antes de confirmar los cambios.
> **💡 TIP:** Haz una copia de seguridad de tu carpeta antes de iniciar.
> **⚠ ADVERTENCIA:** Si existe un archivo con el nuevo nombre, se omitirá para evitar sobreescribirlo.
> **🔒 Seguridad:** El sistema solo renombra, **nunca borra archivos**.

---

## 🛠 En caso de problemas

* Los errores se muestran en el reporte final.
* Archivos sin coincidencia mantienen su nombre original.
* Puedes repetir el proceso si es necesario.

---

## 📅 Cuándo usarlo

Ideal para el cierre mensual, cuando ya tienes todos los registros contables listos y quieres aplicar una **nomenclatura uniforme** a todos tus archivos.

---

Si quieres, puedo prepararte esta misma documentación con **iconos, bloques de color y estilos amigables** para que en GitHub Pages se vea más visual y moderna, incluyendo *cajas de advertencia y tips*. ¿Quieres que la maquete así?

---

$$ Developed-with-♥️-by-arnoldbgm 