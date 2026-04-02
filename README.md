# io.github.jgomezbau.iawrapper

Repositorio de empaquetado Flatpak para **IAWrapper**.

Este proyecto contiene el manifest y los archivos necesarios para construir y distribuir **IAWrapper** como paquete **Flatpak**, incluyendo su futura publicación en **Flathub**.

> El código fuente principal de la aplicación está en:  
> https://github.com/jgomezbau/IAWrapper

---

## ¿Qué es este repositorio?

Este repositorio **no contiene el desarrollo principal de la aplicación**, sino el empaquetado Flatpak de distribución.

Su función es:

- definir el manifest de build
- fijar el commit exacto del repositorio fuente
- descargar la versión correspondiente de Electron
- incluir los metadatos necesarios para Flatpak / Flathub
- permitir builds reproducibles

---

## Repositorios relacionados

### Código fuente principal
- https://github.com/jgomezbau/IAWrapper

### Repositorio de empaquetado Flatpak
- https://github.com/jgomezbau/io.github.jgomezbau.iawrapper

---

## Archivos principales

- `io.github.jgomezbau.iawrapper.yml` → manifest principal de Flatpak
- `generated-sources.json` → fuentes auxiliares generadas para el proceso de build

---

## Build local

Desde la raíz de este repositorio:

```bash
flatpak-builder --user --install --force-clean build-dir io.github.jgomezbau.iawrapper.yml
```

---

## Ejecutar la aplicación instalada

```bash
flatpak run io.github.jgomezbau.iawrapper
```

También pueden utilizarse los accesos directos instalados para cada proveedor soportado.

---

## Proceso de actualización

Cuando se publica una nueva versión de **IAWrapper**, el flujo recomendado es:

### 1. Actualizar el repositorio fuente
En el repo principal:

```bash
git add .
git commit -m "Nueva versión X.Y.Z"
git push
git rev-parse HEAD
```

Copiar el hash resultante.

### 2. Actualizar este repositorio de packaging
Editar `io.github.jgomezbau.iawrapper.yml` y reemplazar el valor de `commit:` por el nuevo hash del repo fuente.

Ejemplo:

```yaml
sources:
  - type: git
    url: https://github.com/jgomezbau/IAWrapper.git
    commit: <nuevo_hash_aqui>
```

### 3. Si cambia la versión de Electron
Actualizar también:

- la URL del archivo `electron-vX.Y.Z-linux-x64.zip`
- el valor `sha256`

### 4. Si cambian dependencias npm
Regenerar `generated-sources.json` desde el repo fuente y copiarlo aquí.

### 5. Probar nuevamente el build
```bash
flatpak-builder --user --install --force-clean build-dir io.github.jgomezbau.iawrapper.yml
```

---

## Publicación en Flathub

Este repositorio está preparado como base para el empaquetado y validación previa a su publicación en Flathub.

Antes de enviar una nueva versión, conviene verificar:

- que el manifest pase el linter
- que el build local finalice correctamente
- que el metainfo esté actualizado
- que las capturas y metadatos sean válidos

---

## Licencia

El empaquetado se distribuye bajo la misma licencia del proyecto principal:

**MIT**

---

## Autor

**Juan Bau**  
jgomezbau@gmail.com
