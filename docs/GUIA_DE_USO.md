# Guía de uso — Actualización masiva en AS400

## Qué necesitas
- Windows con **Power Automate Desktop** (viene gratis en Windows 10 y 11).
- **Microsoft Excel**.
- Un emulador de terminal **IBM i Access / Client Access** con soporte HLLAPI (`ehlapi32.dll`) y acceso a un sistema AS400.

## 1. Prepara el Excel
Usa la plantilla [`datos_ejemplo/plantilla_actualizacion.xlsx`](../datos_ejemplo/plantilla_actualizacion.xlsx). La primera fila son los encabezados y deben llamarse exactamente así:

| CUENTA | CODIGO | RESULTADO |
|---|---|---|
| 100245 | A12 | *(vacío)* |
| 100318 | B07 | *(vacío)* |

- **CUENTA:** la cuenta que se va a modificar.
- **CODIGO:** el código nuevo.
- **RESULTADO:** déjala vacía. El robot escribe ahí `OK` u `OMITIDA`.

> Formatea CUENTA y CODIGO como **texto** para que Excel no borre los ceros a la izquierda.

## 2. Crea el flujo en Power Automate Desktop
1. Abre Power Automate Desktop y crea un **flujo nuevo**.
2. Abre [`flujos/actualizacion_masiva_as400.txt`](../flujos/actualizacion_masiva_as400.txt), copia **todo** el texto y pégalo (Ctrl+V) en el área de acciones del diseñador. PAD convierte el texto en acciones.
3. En la sección **1. Configuración**, cambia:
   - `RutaExcel`: dónde guardaste tu Excel.
   - `RutaSesion`: tu archivo de sesión `.ws` del emulador.
   - `MenuDestino`, `PantallaDestino`, `TextoMenuPrincipal` y `TextoPantallaCuenta`: los nombres reales de tu sistema.
4. Si tu sistema usa otras posiciones de pantalla, ajusta las filas y columnas de las acciones **Mover cursor** (`MoveCursor`).

## 3. Ejecuta
1. Pulsa **Ejecutar**.
2. Cuando abra el emulador, **inicia sesión tú**. El robot espera hasta 2 minutos a que aparezca el menú principal.
3. No uses el teclado ni el mouse mientras trabaja.
4. Al final verás un resumen con cuántas cuentas se actualizaron, cuántas se omitieron y cuántas ya estaban hechas.

## Si algo falla
- **Se detuvo a la mitad:** abre el Excel. Las filas con `OK` ya están hechas. Corrige el problema y vuelve a ejecutar: el robot **salta** las filas con `OK`.
- **"Text not found" o tiempo agotado:** el texto que espera el robot no coincide con tu pantalla. Revisa `TextoMenuPrincipal` y `TextoPantallaCuenta`, o sube `SegundosEspera`.
- **Escribe en el lugar equivocado:** ajusta la fila y la columna del `MoveCursor` correspondiente.

## Buenas prácticas
- Prueba primero con **2 o 3 cuentas** en un ambiente de pruebas.
- Guarda una copia del Excel original antes de ejecutar.
- Nunca pongas usuario ni contraseña dentro del flujo.
