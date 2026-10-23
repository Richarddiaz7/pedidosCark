# 📌 CONTEXTO MAESTRO – PROYECTO DISTRIBUIDORA CJTA

> Este documento debe pegarse al inicio de cualquier chat nuevo para retomar el proyecto exactamente donde quedó.
> Última actualización: 23 de octubre de 2026
> Versión del proyecto: 2.0 (nueva lista de precios + condiciones + lógica de descuentos)

---

## 🎯 OBJETIVO GENERAL

Crear una aplicación de pedidos/notas de entrega para **Distribuidora CJTA** (lubricantes CARK), instalable como PWA en móviles, con generación de PDF, precios multimoneda (USD/BCV), firma digital, sello fotográfico, código QR de ubicación, lógica de descuentos escalonados, opción con/sin IVA, modo de hoja imprimible en blanco, y posibilidad de monetizarla como servicio freelance.

---

## 👤 DATOS DEL USUARIO

- **Nombre**: Richard Díaz
- **GitHub**: Richarddiaz7
- **Repo**: pedidos-cjta
- **URL de la app**: https://Richarddiaz7.github.io/pedidos-cjta/
- **Trabaja en/comercializa para**: Distribuidora CJTA (lubricantes CARK)
- **Aprende rápido**, trabaja desde móvil, prefiere respuestas completas y código listo para copiar
- **Interesado en**: trabajo remoto freelance (Workana/Fiverr), monetización de automatizaciones
- **Aprendiendo**: Dart/Flutter (vio ejemplos en DartPad), Python (bots de Telegram)

---

## ✅ ESTADO ACTUAL DEL PROYECTO

### 1. App PWA publicada en GitHub Pages

**Archivos en el repo:**
- `index.html` (app completa)
- `manifest.json` (con `?v=2` en iconos)
- `service-worker.js` (caché-primero, `cjta-pedidos-v3`)
- `icon-192.png`
- `icon-512.png`

**Instalable como ícono** en pantalla de inicio.

### 2. Funcionalidades ya implementadas (versión anterior)

- Catálogo con precios USD/BCV (obsoleto, se reemplaza)
- Búsqueda por nombre/código/categoría
- Firma digital (SignaturePad)
- Captura de sello (cámara/galería, Base64)
- QR de ubicación (Google Maps)
- PDF con html2canvas + jsPDF
- Logo SVG integrado

### 3. Problemas ya resueltos

| Problema | Solución |
|----------|----------|
| PDF en blanco | Contenedor temporal con `z-index: -1` y `opacity: 1` |
| Firma se agranda en móvil | Altura fija + `touch-action: none` + media query |
| Icono gris en Chrome | Limpieza de service worker + `?v=2` en manifest |
| Archivos temporales en descargas | Eliminado al usar jsPDF directamente |
| 404 en GitHub Pages | Renombrar `pedidosx.html` a `index.html` |
| Logo no visible sin archivo | SVG integrado en el código |

---

## 🔄 NUEVA VERSIÓN 2.0 (EN DESARROLLO)

### Cambios respecto a la versión anterior

1. **Un solo precio de lista en USD** (se elimina `priceBCV` como segundo precio editable)
2. **Descuentos escalonados automáticos** según forma de pago y volumen
3. **Descuento manual ajustable** (mixto: la app calcula, el vendedor puede sobreescribir)
4. **Selector Con IVA / Sin IVA** (Factura vs Nota de Entrega)
5. **PDF rediseñado** para parecerse a la Hoja de Pedido oficial de CJTA
6. **Modo "Hoja imprimible en blanco"** para llenar a mano
7. **Catálogo actualizado** con lista de octubre 2026
8. **Códigos corregidos** (errores de tipeo del PDF)
9. **Base de datos externa en JSON** (Fase 1) para que gerencia actualice sin tocar código

---

## 💰 LÓGICA DE PRECIOS Y DESCUENTOS

### Precio base
- **Un solo precio de lista en USD** (el del PDF oficial)
- **IVA 16%** se suma aparte (opcional según cliente)

### Descuentos (en escalera, uno sobre otro)

Se aplican en este orden:
1. **Divisas** (efectivo USD, Zelle, USDT, transferencia divisas): **20%**
2. **Contado** (paga el 100% el mismo día): **5%**
3. **Volumen**:
   - 40–99 cajas: **5%**
   - 100+ cajas: **10%**

**Ejemplo del PDF:**
- Lista: $100
- −20% divisas → $80
- −5% contado → $76
- −5% volumen → $72.20
- + IVA 16% → $83.75

### Equivalencias para contar cajas
- 1 paila = 1,5 cajas
- 1 bidón = 1,5 cajas
- 1 tambor = 15 cajas
- Grasas y granel **no cuentan** para volumen

### Modo mixto
- La app **calcula automáticamente** los descuentos
- El vendedor puede **sobreescribir** el descuento total si negoció algo especial
- Queda registrado en el PDF que hubo ajuste manual

### IVA (selector)
- **Con IVA (Factura)**: Subtotal + IVA 16% + Total
- **Sin IVA (Nota de Entrega)**: Solo subtotal
- **Precio BCV**: El precio de lista en USD se convierte a Bs. según tasa BCV del día de pago (mostrado como referencia)

---

## 📋 CATÁLOGO DE PRODUCTOS (octubre 2026)

Organizado en 10 categorías:

1. **CAJAS 12x0,946 L** (`CJ-*`) — 14 productos
2. **BIDONES 19 L** (`BD-*`) — 10 productos
3. **PAILAS 19 L** (`PA-*`) — 23 productos
4. **GRASA TARRO 6x250g** (`TR-*`) — 2 productos
5. **GRASA CARTUCHO 12x400g** (`CT-*`) — 1 producto
6. **GRASA MINI PAILA 3,5kg** (`MP-*`) — 3 productos
7. **GRASA PAILA 15,9kg** (`PL-*`) — 3 productos
8. **GRASA TAMBOR 181,5kg** (`TBG-*`) — 3 productos
9. **TAMBORES 208 L** (`TB-*`) — 40 productos
10. **GRANEL Precio/Litro** (`GR-*`) — 27 productos

**Total: ~126 productos**

### Correcciones aplicadas a errores de tipeo del PDF
- `CJ-MAT-SL20W50` → `CJ-MOT-SL20W50`
- `CJ-MZT-JASO` → `CJ-M2T-JASO`
- `CJ-MAT-SNL5W50` → `CJ-MOT-SN15W50`
- `CJ-CF-DXIIICaja` → `CJ-CF-DXIII` (nombre "Caja Fluido Dexron III")
- `BD-OP-C14-15W40` → `BD-OP-CI4-15W40`
- `PA-OP-C14-15W40` → `PA-OP-CI4-15W40`
- `TB-CF-DIIICaja` → `TB-CF-DXIII`
- `GR-SST-28W50` → `GR-SST-20W50`
- `GR-TC-80W50` → `GR-TC-80W90`
- `GR-CF-DIIICaja` → `GR-CF-DXIII`
- `TB-HA-AW32` (sin tipeo, correcto)
- `TB-TM-*` → `TB-TR-*` (Termo Cark, según lista anterior)

⚠️ **Nota**: los códigos se pueden editar desde la app si es necesario.

---

## 📄 ESTRUCTURA DEL PDF FINAL

Basado en la **Hoja de Pedido oficial de CJTA**:

1. **Encabezado**: Logo CJTA + título "HOJA DE PEDIDO"
2. **Datos**: Cliente, RIF, Vendedor, Forma de pago (con checkboxes)
3. **Tablas por categoría** (como el PDF original):
   - CÓDIGO · PRODUCTO · PRECIO · CANT. · SUBTOTAL
4. **Resumen de descuentos aplicados** (divisas, contado, volumen, ajuste manual)
5. **Totales**: Subtotal · IVA (opcional) · Total USD · Total Bs. (con tasa BCV)
6. **Observaciones / Condiciones**
7. **Firma y sello**
8. **Nota**: *"Pedido sujeto a disponibilidad de inventario. No constituye factura."*

### Modo "Hoja imprimible en blanco"
Botón adicional que genera la hoja de pedido vacía (formato oficial) para imprimir y llenar a mano.

---

## 📄 CONDICIONES DE VENTA (octubre 2026)

Extraídas del PDF "CJTA - Condiciones de ventas octubre":

- **Lista única en dólares**, pagaderos en bolívares a tasa BCV del día de pago
- **IVA aparte** (16%)
- **Descuentos**:
  - Divisas: 20%
  - Contado: 5%
  - Volumen 40–99 cajas: 5%
  - Volumen 100+ cajas: 10%
- **Reglas**:
  - Pago mixto: el 20% aplica solo a la parte pagada en divisas
  - Crédito: 30 a 45 días según el cliente. Si paga tarde, pierde el volumen; conserva el 20% por divisas hasta 60 días de atraso. Con más de 60 días paga el precio de lista
  - Deudas en bolívares: se pagan a la tasa BCV del día de pago
  - Ningún otro descuento sin aprobación de gerencia, por escrito y firmado

---

## 🗂️ ESTRUCTURA DE ARCHIVOS
