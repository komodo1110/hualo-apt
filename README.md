# HuaLo apt repository (GitHub Pages)

Repositorio de paquetes de la app HuaLo. Los paquetes son `.deb` Ubuntu/Debian
aarch64 con su firma GPG. Al subir un `.deb` nuevo a `pools/`, un GitHub Action
regenera los indices y publica en GitHub Pages.

## Estructura

```
apt-repo/
├── pools/main/<letra>/<paquete>/<paquete>_<version>_arm64.deb   <- sube aqui los .deb
├── dists/stable/main/binary-arm64/                              <- indices (generados)
├── pubkey.asc                                                   <- clave publica del repo
├── docs/index.txt                                               <- comando `pkg docs`
└── .github/workflows/repo-apt.yml                               <- publica automaticamente
```

## Como crear el repositorio (komodo1110/hualo-apt)

1. Crea el repo publico `hualo-apt` en la cuenta `komodo1110` y activa
   **Settings > Pages > Deploy from a branch = gh-pages**.
2. Sube el contenido de esta carpeta a la rama `main`.
3. La clave GPG YA esta generada (ed25519, fingerprint
   `1A1AF140C7966EA1A27167A6CA17AE5D01B61EDE`): publica en `pubkey.asc`;
   privada exportada en `/root/HuaLo/dist/REPO_GPG_KEY.asc` (en el dispositivo,
   NO subir nunca al repo).
4. En el repo: **Settings > Secrets and variables > Actions**, crea el secreto
   `REPO_GPG_KEY` con el contenido de esa clave privada exportada (ASCII armor).
5. En la app/entorno el `sources.list` ya apunta a
   `deb [signed-by=/etc/apt/trusted.gpg.d/hualo.gpg] https://komodo1110.github.io/hualo-apt/ stable main`
   e instala `pubkey.asc` en `/etc/apt/trusted.gpg.d/hualo.gpg`.

Nota: si no hay `.deb` en `pools/main`, el Action genera un indice vacio sin
fallar (para que el primer push publique el esqueleto igualmente).

## Comandos

- `pkg update` (consulta el indice)
- `pkg install <paquete>` / `pkg remove` / `pkg upgrade`
- `pkg docs` (abre `docs/index.txt`)
- `pkg doctor`

## Firma

Toda la cadena esta firmada: los paquetes `.deb` (campos de control verificados
por dpkg), el indice `Packages.gz` (dentro de `Release` firmado) y `InRelease`.

## Publicar un paquete nuevo

Sube el `.deb` en `pools/main/...` y haz push a `main`. El Action (ver
`.github/workflows/repo-apt.yml`) regenera indices y firma con `REPO_GPG_KEY`,
y despliega en `gh-pages` (Pages) automaticamente.