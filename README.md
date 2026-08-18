# astrapi-builders

Dockerfiles für die Builder-Images von astrapi-packages. `images.yaml` listet
alle Images mit `tag`/`module`/`subdir`; der Git-basierte Builder in
astrapi-packages klont dieses Repo bei jedem Bau frisch und erwartet je Image
einen eigenen Ordner (Name = Image-ID, siehe `subdir`) mit einer Datei
`Dockerfile` darin. Weitere vom Dockerfile per `COPY`/`ADD` eingebundene
Dateien (Skripte etc.) liegen im selben Ordner.

```
arch-builder/
  Dockerfile
  build.sh          # von Dockerfile per COPY eingebunden
debian-builder/
  Dockerfile
debian-builder-rust/
  Dockerfile
debian-builder-python/
  Dockerfile
debian-builder-php/
  Dockerfile
images.yaml
```

Neues Image hinzufügen: Ordner anlegen, `Dockerfile` (+ ggf. weitere Dateien)
hineinlegen, Eintrag in `images.yaml` ergänzen (`subdir` = Ordnername).

Herkunft: 1:1 übernommen aus astrapi-packages
(`modules/{debian,archlinux}/dockerfiles/`, main-Branch-Stand vom 2026-08-18),
wo sie bisher statisch im App-Repo lagen.
