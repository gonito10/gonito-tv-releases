GoNiTo TV — Actualización de la app por internet (OTA)
======================================================

Ahora la app avisa cuando publicás una versión nueva, la descarga
sola y abre el instalador. No hace falta pasar el APK a mano.


═══ CARPETA pasalink/ → repo gonito10/pasalink ═══
  server.js    (reemplazar)
  index.html   (reemplazar)

Se agregó el bloque "appUpdate" DENTRO del /gonito/config que ya
existía — no hay endpoint nuevo. Probado: al publicar solo la
actualización, las fuentes de agenda quedan intactas.

En el admin, abajo de las fuentes, aparece "🚀 Actualización de la
app Android" con: versionCode · versionName · URL del APK ·
changelog · checkbox de obligatoria.


═══ CARPETA android/ → proyecto gonitotv-android ═══

ARCHIVOS NUEVOS (agregar al proyecto):
  java/com/gonito/tv/data/UpdateRepository.kt
  java/com/gonito/tv/ui/UpdateDialog.kt
  res/xml/file_paths.xml          ← la carpeta res/xml no existía

A REEMPLAZAR:
  java/com/gonito/tv/ui/SettingsActivity.kt
  java/com/gonito/tv/ui/MainActivity.kt
  res/layout/activity_settings.xml
  raiz/AndroidManifest.xml   → va en app/src/main/
  raiz/build.gradle.kts      → va en app/


═══ CÓMO PUBLICAR UNA ACTUALIZACIÓN ═══

1. En build.gradle.kts SUBÍ versionCode (1 → 2 → 3…) y versionName.
   Este es el paso que más se olvida: si no lo subís, nadie se entera.

2. Generá el APK firmado.
   ⚠ CRÍTICO: usá SIEMPRE el mismo keystore. Android rechaza la
   instalación si el APK viene firmado con otra clave — el usuario
   tendría que desinstalar y perdería la licencia activada.

3. Subilo a GitHub Releases:
   repo → Releases → Draft a new release → tag v1.1 →
   arrastrás el APK → Publish release.
   Después, botón derecho sobre el APK → copiar dirección del enlace.
   Queda algo así:
   https://github.com/gonito10/REPO/releases/download/v1.1/gonito-tv.apk

   El repo puede ser privado, pero entonces el link de descarga NO
   es público y la app no va a poder bajarlo. Para que funcione,
   el repo tiene que ser público (o usar otro hosting).

4. En el admin de PasaLink completá versionCode (el mismo del paso 1),
   versionName, la URL del APK y qué cambió. Publicar.

5. Los dispositivos avisan al abrir la app.


═══ QUÉ VE EL USUARIO ═══
· Al abrir: diálogo con la versión nueva y el changelog.
  Botones Actualizar / Después / Omitir esta versión.
· Si la marcaste obligatoria, no puede posponerla.
· En Configuración → "🚀 Buscar actualizaciones" para chequear
  a mano (ahí no aparece "Omitir", ya la pidió él).
· La descarga va con el DownloadManager: se ve el progreso en la
  barra de notificaciones y sigue si sale de la app.
· Al terminar se abre el instalador solo.

Android 8+ pide permiso "Instalar apps desconocidas" la primera vez.
La app lo detecta y lleva al usuario directo a esa pantalla.

Para desactivar el aviso: poné versionCode en 0 en el admin.
