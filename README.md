# HuaLo apt — repositorio de paquetes

[![Pipeline apt](https://github.com/komodo1110/hualo-apt/actions/workflows/repo-apt.yml/badge.svg)](https://github.com/komodo1110/hualo-apt/actions/workflows/repo-apt.yml)
[![Licencia MIT](https://img.shields.io/badge/licencia-MIT-blue.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/Pages-activo-brightgreen)](https://komodo1110.github.io/hualo-apt/)

Repositorio de paquetes `.deb` (aarch64 / arm64) para la app **HuaLo**, un
emulador de terminal profesional para Android. Los paquetes se firman con GPG
y se distribuyen públicamente mediante GitHub Pages.

## Instalación rápida

Desde la terminal de HuaLo (o cualquier entorno Debian/Ubuntu arm64):

```sh
# 1. Añade el repositorio (URL pública de GitHub Pages)
echo "deb [signed-by=/etc/apt/trusted.gpg.d/hualo.gpg] https://komodo1110.github.io/hualo-apt/ stable main" \
  | sudo tee /etc/apt/sources.list.d/hualo.list

# 2. Instala la clave pública de firma
sudo install -m 644 https://komodo1110.github.io/hualo-apt/pubkey.asc \
  /etc/apt/trusted.gpg.d/hualo.gpg

# 3. Actualiza e instala
sudo apt-get update
sudo apt-get install <paquete>   # p. ej.: sudo apt-get install nano
```

> En la app HuaLo, el comando `pkg` ya trae estas operaciones integradas:
> `pkg update`, `pkg install <paquete>`, `pkg remove <paquete>`, `pkg upgrade`,
> `pkg docs`, `pkg doctor`. La clave pública y la configuración del repo se
> instalan automáticamente al iniciar la app por primera vez.

## Estructura del repositorio

```
.
├── .github/workflows/repo-apt.yml   # CI: regenera índices, firma y publica en Pages
├── pools/main/…                     # donde se suben los .deb (fuente de los índices)
├── dists/stable/…                   # índices apt (generados: Packages, Release, InRelease)
├── pubkey.asc                       # clave pública de firma (para añadir a trust.gpg.d)
├── docs/index.txt                   # documento de ayuda (comando `pkg docs`)
├── README.md
├── CONTRIBUTING.md                  # cómo enviar paquetes o reportar problemas
└── SECURITY.md                      # cómo reportar vulnerabilidades
```

## Cómo funciona

1. Un `.deb` nuevo se añade en `pools/main/<letra>/<paquete>/`.
2. Al hacer push a `main`, el workflow regenera:
   - `Packages` / `Packages.gz` (índice de paquetes) con `dpkg-scanpackages`.
   - `Release`, `Release.gpg` e `InRelease` (índices firmados) con
     `apt-ftparchive` + GPG.
3. El resultado se publica automáticamente en la rama `gh-pages`, servida por
   GitHub Pages en `https://komodo1110.github.io/hualo-apt/`.

Toda la cadena está firmada: cada `.deb`, el `Packages.gz` dentro del
`Release` firmado y el `InRelease`. El secreto de firma de la clave privada
(`REPO_GPG_KEY`) vive solo en el secret de GitHub Actions; nunca se sube al
repositorio.

## Verificación de firma

La clave pública del repositorio:

```
pub   ed25519 2026-09-21 [SC]
uid   HuaLo Apt <hualo@termhub>
```

Puedes comprobar que los índices descargados están firmados por esta clave:

```sh
gpg --show-keys pubkey.asc          # muestra la huella de la clave pública
gpg --verify dists/stable/InRelease # comprueba la firma del índice
```

Cualquier cambio en esta clave se anuncia en los releases del repositorio y en
`SECURITY.md`.

## Publicar un paquete nuevo

1. Sube el `.deb` a `pools/main/<letra>/<paquete>/`.
2. Haz push a `main`. El workflow regenera y firma los índices y despliega en
   Pages de forma automática (solo paquetes `arm64`).

Guías detalladas para contribuidores en [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Licencia

El contenido de este repositorio (workflows, documentación y estructura) se
distribuye bajo la **licencia MIT** (ver [`LICENSE`](LICENSE)). Los paquetes
`.deb` conservan la licencia de cada uno de sus proyectos upstream; compruébala
en cada paquete antes de redistribuirlo.

## Seguridad

Para reportar una vulnerabilidad o un incidente con la clave de firma, consulta
[`SECURITY.md`](SECURITY.md). Usa el canal privado (Security Advisory); **no**
publiques detalles sensibles en issues abiertos.