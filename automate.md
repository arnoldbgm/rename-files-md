# 🔧 Guía completa: Automatización para unir archivos PDF con Power Automate

## 📋 Tabla de contenido
- [Introducción](#introducción)
- [Requisitos previos](#requisitos-previos)
- [PARTE I: IMPLEMENTACIÓN DEL FLUJO](#parte-i-implementación-del-flujo)
- [PARTE II: CÓMO USAR LA AUTOMATIZACIÓN](#parte-ii-cómo-usar-la-automatización)
- [Solución de problemas](#solución-de-problemas)

---

## 🎯 Introducción

Esta guía te permitirá crear una automatización que une múltiples archivos PDF en un solo documento. La automatización funciona seleccionando una carpeta con PDFs ordenados alfabéticamente y genera un archivo llamado "pdf_unido.pdf" en la misma ubicación.

**¿Qué lograrás?**
- 🤖 Automatizar la unión de múltiples PDFs
- 🚫 Evitar software de terceros
- 📁 Procesar archivos directamente desde OneDrive/SharePoint
- 📬 Recibir confirmación automática al completarse

---

## ✅ Requisitos previos

✅ **Cuenta Microsoft 365** con acceso a Power Automate  
✅ **Archivos PDF** guardados en OneDrive o SharePoint  
✅ **Navegador web** actualizado  
✅ **Permisos de escritura** en las carpetas donde están tus PDFs  

> [!NOTE]
> No necesitas licencia Premium de Power Automate para esta automatización

---

# 🔧 PARTE I: IMPLEMENTACIÓN DEL FLUJO

*Esta sección es para crear la automatización. Solo necesitas hacerlo una vez. ⚙️*

> [!WARNING]
> Sigue estos pasos exactamente como se describe. Un error en la configuración puede hacer que la automatización no funcione correctamente.

## Paso 1: Crear el flujo base 🎯

### 1.1 Acceder a Power Automate
1. Ve a https://powerautomate.microsoft.com
2. Inicia sesión con tu cuenta Microsoft 365
3. En el menú izquierdo, haz clic en **"+ Crear"**

### 1.2 Configurar flujo instantáneo
1. Selecciona **"Flujo de nube instantáneo"**
2. Configura:
   - **Nombre:** `Unir archivos PDF`
   - **Desencadenador:** Selecciona "Desencadenar un flujo manualmente"
3. Haz clic en **"Crear"**

## Paso 2: Configurar el desencadenador de entrada

1. En el desencadenador "Manually trigger a flow", haz clic en **"+ Agregar una entrada"**
2. Selecciona **"Archivo"**
3. Configura los campos:
   - **Título:** `Archivo de referencia de la carpeta`
   - **Descripción:** `Selecciona cualquier PDF de la carpeta que quieres procesar`

## Paso 3: Obtener información de la carpeta

### 3.1 Agregar acción de propiedades
1. Haz clic en **"+ Nuevo paso"**
2. Busca y selecciona **"Obtener propiedades del archivo"** (OneDrive o SharePoint)
3. Configura:
   - **Dirección del sitio:** Selecciona tu ubicación (OneDrive personal o sitio SharePoint)
   - **Identificador del archivo:** Del menú dinámico, selecciona **"Archivo"**

### 3.2 Extraer ruta de carpeta
1. Haz clic en **"+ Nuevo paso"**
2. Busca y selecciona **"Componer"**
3. En **"Entradas"**, del contenido dinámico selecciona **"Carpeta"**

## Paso 4: Listar todos los PDFs de la carpeta 📋

1. Haz clic en **"+ Nuevo paso"**
2. Busca **"Obtener archivos (propiedades únicamente)"**
3. Configura:
   - **Dirección del sitio:** Misma que el paso anterior
   - **Identificador de carpeta:** Del contenido dinámico, selecciona **"Salidas"** (del paso Componer)
   - **Filtro de consulta ODATA:** `endswith(name,'.pdf')`
   - **Ordenar por:** `name asc`

> [!TIP]
> El filtro ODATA asegura que solo se procesen archivos PDF y que se mantengan en orden alfabético automáticamente.

## Paso 5: Inicializar variable para combinar contenido

1. Haz clic en **"+ Nuevo paso"**
2. Busca **"Inicializar variable"**
3. Configura:
   - **Nombre:** `contenidoPDFs`
   - **Tipo:** `Matriz`
   - **Valor:** Dejar vacío

## Paso 6: Procesar cada archivo PDF

### 6.1 Crear bucle de procesamiento
1. Haz clic en **"+ Nuevo paso"**
2. Busca **"Aplicar a cada uno"**
3. En **"Seleccionar una salida"**, del contenido dinámico selecciona **"valor"** (de Obtener archivos)

### 6.2 Obtener contenido de cada PDF
*Dentro del bucle "Aplicar a cada uno":*

1. Haz clic en **"Agregar una acción"**
2. Busca **"Obtener contenido del archivo"**
3. Configura:
   - **Dirección del sitio:** Misma ubicación que antes
   - **Identificador del archivo:** Del contenido dinámico, selecciona **"Id"**

### 6.3 Agregar contenido a la variable
*Aún dentro del bucle:*

1. Haz clic en **"Agregar una acción"**
2. Busca **"Anexar a la variable de matriz"**
3. Configura:
   - **Nombre:** `contenidoPDFs`
   - **Valor:** Del contenido dinámico, selecciona **"Contenido del archivo"**

## Paso 7: Crear el archivo PDF unido 📄

*Este paso debe estar FUERA del bucle:*

> [!IMPORTANT]
> Es crucial que este paso esté fuera del bucle "Aplicar a cada uno". Si lo colocas dentro del bucle, se creará un archivo por cada PDF individual en lugar de un solo archivo combinado.

1. Haz clic en **"+ Nuevo paso"** (fuera del bucle "Aplicar a cada uno")
2. Busca **"Crear archivo"**
3. Configura:
   - **Dirección del sitio:** Misma ubicación
   - **Ruta de la carpeta:** Del contenido dinámico, selecciona **"Salidas"** (del paso Componer)
   - **Nombre del archivo:** `pdf_unido.pdf`
   - **Contenido del archivo:** Del contenido dinámico, selecciona **"contenidoPDFs"**

## Paso 8: Configurar notificación de éxito

1. Haz clic en **"+ Nuevo paso"**
2. Busca **"Enviar una notificación de inserción"**
3. En **"Texto"** escribe: `PDF unido exitosamente`

## Paso 9: Guardar la automatización

1. Haz clic en **"Guardar"** (esquina superior derecha)
2. Espera el mensaje de confirmación "Flujo guardado"

### Verificación final del flujo

Tu flujo debe tener esta estructura:
```
1. Manually trigger a flow (con entrada de archivo)
2. Obtener propiedades del archivo
3. Componer (extraer carpeta)
4. Obtener archivos (filtrar PDFs)
5. Inicializar variable
6. Aplicar a cada uno
   ├── Obtener contenido del archivo
   └── Anexar a variable de matriz
7. Crear archivo
8. Enviar notificación
```

---

# 👤 PARTE II: CÓMO USAR LA AUTOMATIZACIÓN

*Esta sección es para usuarios finales que van a utilizar la automatización ya creada. 📱💻*

## Preparación previa 📁

### Organizar tus archivos PDF
1. **Coloca todos los PDFs** que quieres unir en una sola carpeta
2. **Renombra los archivos** para que aparezcan en el orden deseado alfabéticamente:
   - ✅ Correcto: `01_introduccion.pdf`, `02_desarrollo.pdf`, `03_conclusion.pdf`
   - ❌ Incorrecto: `introduccion.pdf`, `desarrollo.pdf`, `conclusion.pdf`
3. **Verifica que todos sean archivos .pdf** válidos

> [!TIP]
> Si tus PDFs ya tienen nombres que se ordenan correctamente de forma alfabética, no necesitas renombrarlos. La automatización respetará el orden natural de los nombres de archivo.

## Ejecutar la automatización 🚀

### Método 1: Desde el listado de flujos (💻 Navegador)
1. Ve a https://powerautomate.microsoft.com
2. En el menú izquierdo, haz clic en **"Mis flujos"**
3. Busca tu flujo **"Unir archivos PDF"**
4. Haz clic en el botón **"Ejecutar"**

### Método 2: Desde la aplicación móvil (📱 Mobile)
1. Abre la app **"Power Automate"** en tu móvil
2. Ve a la pestaña **"Botones"**
3. Toca **"Unir archivos PDF"**

## Proceso de ejecución

### Paso 1: Seleccionar archivo de referencia 🎯
1. Se abrirá una ventana para seleccionar archivo
2. **Navega hasta la carpeta** que contiene tus PDFs
3. **Selecciona cualquier PDF** de esa carpeta (no importa cuál)
4. Haz clic en **"Seleccionar"**

> [!NOTE]
> El archivo que selecciones solo sirve como "referencia" para identificar la carpeta. No afecta el orden final de los PDFs unidos.

### Paso 2: Iniciar procesamiento ⚡
1. Haz clic en **"Ejecutar flujo"**
2. Verás el mensaje: *"Su flujo se ejecutará en unos instantes"*
3. Puedes cerrar la ventana o esperar a ver el progreso

### Paso 3: Confirmación de finalización ✅
- **Notificación móvil:** Recibirás "PDF unido exitosamente"
- **En el navegador:** Verás el estado "Correcto" en el historial de ejecuciones

## Encontrar tu archivo unido 🔍

1. **Ve a la carpeta original** donde estaban tus PDFs
2. **Busca el archivo:** `pdf_unido.pdf`
3. **Si no aparece:** Actualiza la vista (F5 en navegador o deslizar hacia abajo en móvil)

## Ejemplo de uso completo 💡

**Situación:** Tienes 5 PDFs de un informe que quieres unir

**Preparación:**
```
📁 Informe_Mensual/
├── 01_portada.pdf
├── 02_resumen_ejecutivo.pdf
├── 03_analisis_datos.pdf
├── 04_conclusiones.pdf
└── 05_anexos.pdf
```

**Ejecución:**
1. Ejecutar flujo "Unir archivos PDF" 🚀
2. Seleccionar cualquier archivo (ej: `01_portada.pdf`) 🎯
3. Clic en "Ejecutar flujo" ▶️
4. Esperar notificación de éxito ✅

**Resultado:**
```
📁 Informe_Mensual/
├── 01_portada.pdf
├── 02_resumen_ejecutivo.pdf
├── 03_analisis_datos.pdf
├── 04_conclusiones.pdf
├── 05_anexos.pdf
└── 📄 pdf_unido.pdf ← ¡Tu archivo final!
```

---

# 🛠️ Solución de problemas

## Problemas durante la implementación ⚙️

**Error: "No se puede encontrar el conector"**
- Solución: Verifica que estés usando OneDrive o SharePoint según tu configuración

**Error: "Acceso denegado al crear el flujo"**
- Solución: Contacta a tu administrador de IT para verificar permisos de Power Automate

**El flujo no se guarda**
- Solución: Revisa que todos los campos obligatorios estén completados

## Problemas durante el uso 👤

> [!CAUTION]
> Si el flujo falla repetidamente, evita ejecutarlo múltiples veces seguidas. Esto puede crear archivos duplicados o generar errores adicionales.

**Error: "No se encontraron archivos PDF"**
- ✅ Verifica que los archivos tengan extensión `.pdf` (en minúsculas)
- ✅ Confirma que estás seleccionando un archivo de la carpeta correcta
- ✅ Asegúrate de que la carpeta no esté vacía

**El flujo se ejecuta pero no aparece el archivo final**
- ✅ Espera 2-3 minutos y actualiza la carpeta
- ✅ Revisa si ya existe un archivo llamado "pdf_unido.pdf" (se sobrescribirá)
- ✅ Verifica permisos de escritura en la carpeta

**Notificación: "Error en el flujo"** ⚠️
- ✅ Ve al historial de ejecuciones en Power Automate
- ✅ Revisa qué paso falló específicamente
- ✅ Verifica que todos los archivos sean PDFs válidos

**Los PDFs se unen en orden incorrecto**
- ✅ Renombra los archivos para que el orden alfabético coincida con el deseado
- ✅ Usa números al inicio: `01_`, `02_`, `03_`, etc.

## Contacto y soporte 📞

Si experimentas problemas técnicos más complejos:
- Revisa el **historial de ejecuciones** en Power Automate para detalles específicos del error
- Consulta la **documentación oficial de Microsoft Power Automate**
- Contacta al **administrador de IT** de tu organización para problemas de permisos

---

🎉 **¡Tu automatización está lista para usar!** Ahora puedes unir archivos PDF de forma rápida y eficiente sin necesidad de software adicional.

_Desarrollado con ♥️ arnoldbgm_
