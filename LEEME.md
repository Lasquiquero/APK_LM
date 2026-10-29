# LM Palabras para Android

Proyecto Kivy + Buildozer. Contiene:

| Archivo | Para qué sirve |
|---|---|
| `main.py` | Pantallas de la app (Kivy) |
| `lm_core.py` | Lógica: listas, cuestionario, progreso, historial, estadísticas, rachas |
| `buildozer.spec` | Configuración del APK |
| `.github/workflows/build-apk.yml` | Construye el APK automáticamente en GitHub |

## Opción A (recomendada): obtener el APK con GitHub, sin instalar nada

1. Crea una cuenta en github.com y un repositorio nuevo (por ejemplo `lm-palabras`).
2. Sube `main.py`, `lm_core.py` y `buildozer.spec` (botón **Add file > Upload files**).
3. Sube también el workflow. Las carpetas que empiezan por punto a veces no se suben
   arrastrando, así que créalo a mano: **Add file > Create new file**, escribe exactamente
   `.github/workflows/build-apk.yml` como nombre y pega el contenido del archivo del mismo nombre.
4. Ve a la pestaña **Actions**. La construcción empieza sola (o pulsa **Run workflow**).
   La primera vez tarda entre 20 y 40 minutos (descarga el SDK y el NDK de Android).
5. Cuando salga el check verde, entra en esa ejecución y descarga **LM-Palabras-apk**
   (abajo, en *Artifacts*). Descomprime el zip: dentro está el `.apk`.
6. Pásalo al móvil e instálalo. Android pedirá permitir "instalar apps desconocidas" para
   la app desde la que lo abras (Archivos, Chrome...). Es un APK de prueba (debug).

## Opción B: construirlo tú en Linux (o Windows con WSL)

    sudo apt update
    sudo apt install -y git zip unzip openjdk-17-jdk python3-pip autoconf libtool pkg-config \
         zlib1g-dev libncurses5-dev libncursesw5-dev libtinfo5 cmake libffi-dev libssl-dev
    pip install --user buildozer "cython<3"
    cd carpeta_del_proyecto
    buildozer android debug

El APK queda en la carpeta `bin/`.

## Probarla antes en el ordenador

    pip install kivy openpyxl
    python main.py

(Los datos se guardan en la carpeta de usuario de Kivy; el selector de archivos
es el propio de Kivy en lugar del de Android.)

## Cómo funciona en el móvil

- **Importar listas:** abre el selector de archivos de Android (puedes elegir varios a la vez:
  CSV, TXT o Excel `.xlsx`). La app guarda una **copia** dentro de sí misma, así no hay que
  volver a importarlas y tu archivo original nunca se modifica. Las listas se recuerdan
  entre sesiones.
- **Editar** modifica la copia de la app. **Quitar** borra solo esa copia.
- **Escribir mi lista:** si le pones un nombre en "Guardar como", se guarda en la app (y se intenta
  copiar también a la carpeta Documentos del móvil).
- Los ajustes (tema oscuro, tamaño de letra, temporizador y objetivo diario), el historial y
  el progreso por palabra se guardan en la carpeta privada de la app. Si la desinstalas, se pierden.
- Botón **atrás** de Android: cierra ventanas, vuelve al menú o pide terminar la lista.

## Si la construcción falla o la app se cierra al abrirla

- En GitHub: abre el paso *Construir con Buildozer* y copia las últimas 30 líneas del error.
- En el móvil (con el cable y depuración USB): `adb logcat -s python` muestra el error de Python.

Con ese texto se puede corregir rápido.

## Para móviles antiguos (32 bits)

En `buildozer.spec`, cambia `android.archs = arm64-v8a` por `android.archs = arm64-v8a, armeabi-v7a`
(la construcción tarda más).
