# 📸 CameraX Application – Documentación

## 📑 Índice

1. 🎯 Creación e Inicialización del Proyecto
2. 🚀 Lógica de la App – MainActivity


---

## 🎯 Creación e Inicialización del Proyecto

### 🛠️ Creación del Proyecto en Android Studio

1. Abre Android Studio.
2. Ve a `File > New > New Project`.
3. Selecciona **Empty Activity** y haz clic en **Next**.
4. Asigna un nombre al proyecto (ej. `CameraXAplication`).
5. Asegúrate de seleccionar **Java** como lenguaje.
6. Establece el **Minimum SDK** en `API 21 (Lollipop)` o superior.
7. Haz clic en **Finish** y espera a que el proyecto se cree.

---

### 📦 Dependencias necesarias

Agrega las siguientes líneas en tu archivo `app/build.gradle` dentro de la sección `dependencies`:

```gradle
implementation "androidx.camera:camera-core:1.3.0"
implementation "androidx.camera:camera-camera2:1.3.0"
implementation "androidx.camera:camera-lifecycle:1.3.0"
implementation "androidx.camera:camera-view:1.3.0"
implementation "androidx.camera:camera-extensions:1.3.0"
implementation "androidx.camera:camera-video:1.3.0"
```

---

### 🔐 Permisos requeridos

Agrega lo siguiente en tu archivo `AndroidManifest.xml` dentro del `<manifest>`:

```xml
<uses-feature
    android:name="android.hardware.camera"
    android:required="false" />

<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

---

### 🎨 Diseño de la Interfaz XML

Reemplaza el contenido de tu archivo `res/layout/activity_main.xml` con lo siguiente:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.camera.view.PreviewView
        android:id="@+id/previewView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <Button
        android:id="@+id/captureButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="📸 Foto"
        app:layout_constraintBottom_toTopOf="@id/videoButton"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/videoButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="🎥 Grabar"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginBottom="32dp"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

---

## 🚀 Lógica de la App - `MainActivity` - Flujo y casos de uso

### ✅ Métodos clave en MainActivity

La clase `MainActivity` contiene **7 métodos principales** que controlan el flujo de la aplicación:

- `onCreate`
- `startCamera`
- `takePhoto`
- `startRecording`
- `LuminosityAnalyzer` (clase interna con método `analyze`)
- `allPermissionsGranted`
- `onRequestPermissionsResult`

---

### 🔁 Flujo general de la aplicación

#### 1. `onCreate()`

- Punto de entrada de la actividad.
- Se inicializa el layout (`activity_main.xml`) y los botones de foto y video.
- Se verifica si los permisos han sido concedidos mediante `allPermissionsGranted()`.
- Si faltan permisos, se solicitan.
- Si están concedidos, se llama a `startCamera()` para iniciar la cámara.

#### 2. `startCamera()`

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

#### ⤵️ Diagrama

![Descripción del diagrama](diagrama.png)

---

### 📸 Métodos que ejecutan los *use cases*

#### `takePhoto()`
- Relacionado con el caso de uso **ImageCapture**.
- Captura una foto y la guarda como archivo local.
- Usa `ImageCapture.takePicture(...)`.
- Muestra mensaje de confirmación con `Toast` y `Log`.

#### `startRecording()`
- Relacionado con el caso de uso **VideoCapture**.
- Inicia o detiene la grabación de video con audio.
- Usa `Recorder.prepareRecording().start(...)`.
- Al finalizar, se guarda el archivo y se notifica al usuario.

#### `LuminosityAnalyzer()`
- Relacionado con el caso de uso **ImageAnalysis**
- Accede al primer plano Y de la imagen (`YUV`).
- Calcula la **luminosidad promedio** de la imagen.
- Muestra el valor en el log.
- Se puede extender para detectar objetos, códigos QR, etc.

---

### 🔐 Gestión de permisos

#### `allPermissionsGranted()`
- Verifica que todos los permisos requeridos estén concedidos.

#### `onRequestPermissionsResult(...)`
- Llamado cuando el usuario responde a la solicitud de permisos.
- Si se conceden, inicia la cámara.
- Si se niegan, muestra un mensaje y cierra la app.

---

### 🧠 Resumen Final

Esta app se basa en CameraX y aprovecha sus principales funcionalidades con un enfoque modular y guiado por el ciclo de vida, ideal para quienes quieren aprender a usar la cámara en Android.

#### 🗺️ Mapa conceptual

| Caso de uso       | Método relacionado     | Función principal                     |
|------------------|------------------------|--------------------------------------|
| Preview           | Dentro de `startCamera` | Mostrar la vista previa en pantalla |
| ImageCapture      | `takePhoto()`           | Capturar y guardar fotos             |
| VideoCapture      | `startRecording()`      | Grabar y guardar video               |
| ImageAnalysis     | `LuminosityAnalyzer`    | Analizar cada fotograma (luminosidad) |

---

### 📂 Código fuente con comentarios detallados

A continuación se presenta el código completo de `MainActivity.java`, organizado con secciones claramente delimitadas y comentarios explicativos. Esta estructura sirve como guía didáctica para desarrolladores principiantes que desean comprender cómo utilizar CameraX en una aplicación Android. Cada bloque del código está documentado para mostrar su propósito, uso y relación con los *Use Cases* de CameraX: **Preview**, **ImageCapture**, **VideoCapture** e **ImageAnalysis**.

```java
package com.example.cameraxaplication;

import ...

public class MainActivity extends AppCompatActivity {

    // ------ Variables y constantes principales ------------------------------------------------------------------------------------------------------------

    private static final int REQUEST_CODE_PERMISSIONS = 10;
    private static final String[] REQUIRED_PERMISSIONS = new String[]{Manifest.permission.CAMERA, Manifest.permission.RECORD_AUDIO};
    private static final String TAG = "CameraXApp";

    private PreviewView previewView;
    private ImageCapture imageCapture;
    private VideoCapture<Recorder> videoCapture;
    private Recording recording;

    // -------------------------------------------------------------------------------------------------------------------------------------------------------


    // --- Método que se llama al iniciar la actividad -------------------------------------------------------------------------------------------------------

    //  - Infla el layout activity_main.xml.
    //  - Inicializa la vista de la cámara y los botones.
    //  - Verifica si los permisos están concedidos. Si no, los solicita.
    //  - Configura el botón de foto y el botón de video con sus respectivos eventos onClick.
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        previewView = findViewById(R.id.previewView);
        Button captureButton = findViewById(R.id.captureButton);
        Button videoButton = findViewById(R.id.videoButton);


        if (allPermissionsGranted()) {
            startCamera();
        } else {
            ActivityCompat.requestPermissions(this, REQUIRED_PERMISSIONS, REQUEST_CODE_PERMISSIONS);
        }

        captureButton.setOnClickListener(v -> takePhoto());

        videoButton.setOnClickListener(v -> {
            if (recording != null) {
                recording.stop();
                recording = null;
                videoButton.setText("🎥 Grabar");
            } else {
                startRecording(videoButton);
            }
        });
    }

    // -----------------------------------------------------------------------------------------------------------------------------------------------------------



   //  --- Método que inicia y configura los distintos casos de uso de CameraX ------------------------------------------------------------------------------------

    private void startCamera() {
        ListenableFuture<ProcessCameraProvider> cameraProviderFuture =
                ProcessCameraProvider.getInstance(this);

        cameraProviderFuture.addListener(() -> {
            try {
                ProcessCameraProvider cameraProvider = cameraProviderFuture.get();

                // Preview
                Preview preview = new Preview.Builder().build();
                preview.setSurfaceProvider(previewView.getSurfaceProvider());

                // ImageCapture
                imageCapture = new ImageCapture.Builder().build();

                // VideoCapture
                Recorder recorder = new Recorder.Builder()
                        .setQualitySelector(
                                QualitySelector.from(
                                        Quality.HD,
                                        FallbackStrategy.lowerQualityOrHigherThan(Quality.SD)))
                        .build();
                videoCapture = VideoCapture.withOutput(recorder);

                // ImageAnalysis
                ImageAnalysis imageAnalysis = new ImageAnalysis.Builder()
                        .setBackpressureStrategy(ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST)
                        .build();
                imageAnalysis.setAnalyzer(ContextCompat.getMainExecutor(this), new LuminosityAnalyzer());

                // Cámara trasera
                CameraSelector cameraSelector = new CameraSelector.Builder()
                        .requireLensFacing(CameraSelector.LENS_FACING_BACK)
                        .build();

                // Reemplaza cualquier uso previo
                cameraProvider.unbindAll();

                // Enlaza todos los use cases
                cameraProvider.bindToLifecycle(
                        this,
                        cameraSelector,
                        preview,
                        imageCapture,
                        videoCapture,
                        imageAnalysis
                );

            } catch (ExecutionException | InterruptedException e) {
                Log.e(TAG, "Error al iniciar la cámara: ", e);
            }
        }, ContextCompat.getMainExecutor(this));
    }

    // -------------------------------------------------------------------------------------------------------------------------------------------------------------


    // --- Método captura una foto -> Caso de uso: ImageCapture ----------------------------------------------------------------------------------------------------

    private void takePhoto() {
        if (imageCapture == null) return;

        File photoFile = new File(getExternalFilesDir(null),
                new SimpleDateFormat("yyyy-MM-dd-HH-mm-ss-SSS", Locale.US)
                        .format(new Date()) + ".jpg");

        ImageCapture.OutputFileOptions outputOptions =
                new ImageCapture.OutputFileOptions.Builder(photoFile).build();

        imageCapture.takePicture(outputOptions, getExecutor(),
                new ImageCapture.OnImageSavedCallback() {
                    @Override
                    public void onImageSaved(@NonNull ImageCapture.OutputFileResults outputFileResults) {
                        String msg = "Foto guardada: " + photoFile.getAbsolutePath();
                        Toast.makeText(getBaseContext(), msg, Toast.LENGTH_SHORT).show();
                        Log.d(TAG, msg);
                    }

                    @Override
                    public void onError(@NonNull ImageCaptureException exception) {
                        Log.e(TAG, "Error al capturar la foto: ", exception);
                    }
                });
    }

    // ------------------------------------------------------------------------------------------------------------------------------------------------------------



    // --- método inicia la grabación de video -> Caso de uso: VideoCapture ---------------------------------------------------------------------------------------

    private void startRecording(Button videoButton) {
        if (videoCapture == null) return;

        if (ContextCompat.checkSelfPermission(this, Manifest.permission.RECORD_AUDIO)
                != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.RECORD_AUDIO},
                    REQUEST_CODE_PERMISSIONS);
            return;
        }

        String name = new SimpleDateFormat("yyyy-MM-dd-HH-mm-ss", Locale.US)
                .format(new Date());

        ContentValues contentValues = new ContentValues();
        contentValues.put(MediaStore.MediaColumns.DISPLAY_NAME, name);
        contentValues.put(MediaStore.MediaColumns.MIME_TYPE, "video/mp4");

        MediaStoreOutputOptions mediaStoreOutputOptions =
                new MediaStoreOutputOptions.Builder(
                        getContentResolver(),
                        MediaStore.Video.Media.EXTERNAL_CONTENT_URI
                ).setContentValues(contentValues).build();

        PendingRecording pendingRecording = videoCapture.getOutput()
                .prepareRecording(this, mediaStoreOutputOptions)
                .withAudioEnabled();

        recording = pendingRecording.start(ContextCompat.getMainExecutor(this),
                videoRecordEvent -> {
                    if (videoRecordEvent instanceof VideoRecordEvent.Finalize) {
                        Uri uri = ((VideoRecordEvent.Finalize) videoRecordEvent)
                                .getOutputResults().getOutputUri();
                        String msg = "Video guardado: " + uri;
                        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
                        Log.d(TAG, msg);
                    }
                });
        videoButton.setText("⏹️ Detener");
    }

    // --------------------------------------------------------------------------------------------------------------------------------------------------------------


    //  --- Realiza análisis en tiempo real del brillo (luminosidad) -> Caso de uso: ImageAnalysis -------------------------------------------------------------------

    public class LuminosityAnalyzer implements ImageAnalysis.Analyzer {
        private static final String TAG = "LuminosityAnalyzer";

        @Override
        public void analyze(@NonNull ImageProxy image) {
            ByteBuffer buffer = image.getPlanes()[0].getBuffer();
            byte[] data = new byte[buffer.remaining()];
            buffer.get(data);

            int sum = 0;
            for (byte b : data) {
                sum += (b & 0xFF);
            }

            double luma = sum / (double) data.length;
            Log.d(TAG, "Luminosidad promedio: " + luma);

            image.close();
        }
    }

    private Executor getExecutor() {
        return ContextCompat.getMainExecutor(this);
    }

    // -------------------------------------------------------------------------------------------------------------------------------------------------------------------



    //  --- Metodo que Verifica que todos los permisos requeridos (cámara y audio) hayan sido concedidos por el usuario -------------------------------------------------

    private boolean allPermissionsGranted() {
        for (String permission : REQUIRED_PERMISSIONS) {
            if (ContextCompat.checkSelfPermission(this, permission)
                    != PackageManager.PERMISSION_GRANTED) {
                return false;
            }
        }
        return true;
    }

    // --------------------------------------------------------------------------------------------------------------------------------------------------------------------


    // --- Metodo que maneja la respuesta del usuario cuando se solicitan permisos. Si los concede, se inicia la cámara. Si no, se muestra un mensaje y se cierra la app ---

    @Override
    public void onRequestPermissionsResult(int requestCode,
                                           @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == REQUEST_CODE_PERMISSIONS) {
            if (allPermissionsGranted()) {
                startCamera();
            } else {
                Toast.makeText(this,
                        "Permisos no concedidos por el usuario.",
                        Toast.LENGTH_SHORT).show();
                finish();
            }
        }
    }

    // -----------------------------------------------------------------------------------------------------------------------------------------------------------------------

}
```
