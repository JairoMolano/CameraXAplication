
# 📷 Análisis de `MainActivity` - App con CameraX

Este documento explica detalladamente el funcionamiento del archivo `MainActivity.java` de una aplicación Android que utiliza CameraX para capturar fotos, grabar videos y analizar imágenes en tiempo real.

---

## ✅ Métodos clave en MainActivity

La clase `MainActivity` contiene **7 métodos principales** que controlan el flujo de la aplicación:

- `onCreate`
- `startCamera`
- `takePhoto`
- `startRecording`
- `LuminosityAnalyzer` (clase interna con método `analyze`)
- `allPermissionsGranted`
- `onRequestPermissionsResult`

---

## 🔁 Flujo general de la aplicación

### 1. `onCreate()`

- Punto de entrada de la actividad.
- Se inicializa el layout (`activity_main.xml`) y los botones de foto y video.
- Se verifica si los permisos han sido concedidos mediante `allPermissionsGranted()`.
- Si faltan permisos, se solicitan.
- Si están concedidos, se llama a `startCamera()` para iniciar la cámara.

### 2. `startCamera()`

Configura y gestiona los **use cases (casos de uso)** de CameraX. Estos son:

| Caso de uso       | Descripción |
|------------------|-------------|
| `Preview`         | Muestra en pantalla lo que ve la cámara (vista previa). |
| `ImageCapture`    | Permite capturar fotos. |
| `VideoCapture`    | Permite grabar videos. |
| `ImageAnalysis`   | Permite analizar imágenes en tiempo real. Ejemplo: luminosidad. |

Además:
- Se obtiene una instancia de `ProcessCameraProvider` para gestionar el ciclo de vida de la cámara.
- Se vinculan los use cases al ciclo de vida de la actividad con `bindToLifecycle(...)`.

---

## 📸 Métodos que ejecutan los *use cases*

### `takePhoto()`
- Relacionado con el caso de uso **ImageCapture**.
- Captura una foto y la guarda como archivo local.
- Usa `ImageCapture.takePicture(...)`.
- Muestra mensaje de confirmación con `Toast` y `Log`.

### `startRecording(Button videoButton)`
- Relacionado con el caso de uso **VideoCapture**.
- Inicia o detiene la grabación de video con audio.
- Usa `Recorder.prepareRecording().start(...)`.
- Al finalizar, se guarda el archivo y se notifica al usuario.

---

## 🔬 Análisis en tiempo real: `LuminosityAnalyzer`

Clase interna que implementa `ImageAnalysis.Analyzer`.

- Accede al primer plano Y de la imagen (`YUV`).
- Calcula la **luminosidad promedio** de la imagen.
- Muestra el valor en el log.
- Se puede extender para detectar objetos, códigos QR, etc.

---

## 🔐 Gestión de permisos

### `allPermissionsGranted()`
- Verifica que todos los permisos requeridos estén concedidos.

### `onRequestPermissionsResult(...)`
- Llamado cuando el usuario responde a la solicitud de permisos.
- Si se conceden, inicia la cámara.
- Si se niegan, muestra un mensaje y cierra la app.

---

## 🧠 Resumen Final

Esta app se basa en CameraX y aprovecha sus principales funcionalidades con un enfoque modular y guiado por el ciclo de vida, ideal para quienes quieren aprender a usar la cámara en Android.

### 🗺️ Mapa conceptual

| Caso de uso       | Método relacionado     | Función principal                     |
|------------------|------------------------|--------------------------------------|
| Preview           | Dentro de `startCamera` | Mostrar la vista previa en pantalla |
| ImageCapture      | `takePhoto()`           | Capturar y guardar fotos             |
| VideoCapture      | `startRecording()`      | Grabar y guardar video               |
| ImageAnalysis     | `LuminosityAnalyzer`    | Analizar cada fotograma (luminosidad) |
