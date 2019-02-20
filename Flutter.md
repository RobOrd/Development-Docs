# Flutter

### Instalación
Descargar el instalador de la página oficial https://flutter.io/docs/get-started/install/windows. Este es un zip, el cual contiene una carpeta Flutter, descomprimirla en alguna ruta diferente a Program Files.

**Ejecutar el programa flutter_console.bat**
Este programa indicará que herramientas o sistemas se requieren para poder correr las aplicaciones creadas con flutter, por ejemplo Andriod SDK y Android Studio. En esta misma página estan las instrucciones para instalar todos los componentes requeridos.

Instalar **Andriod Studio**. Aunque se use otro IDE flutter requiere que se instale la versión completa de Android Studio. https://developer.android.com/studio/install

> Flutter relies on a full installation of Android Studio to supply its Android platform dependencies

Luego de realizar las instalaciones correspondientes volver a ejecutar el bat para verificar que no falte ninguna dependencia. Posiblemente este bat nos indique que se deben revisar las licencias usando el comando 

```
flutter doctor --android-licenses
```

Instalar **VS Code**.

