# HuaLo apt repository

Repositorio de paquetes `.deb` (Ubuntu/Debian, arm64) para la app HuaLo,
publicado en GitHub Pages y firmado con GPG.

## Instalación rápida

Agrega el repositorio a tu `sources.list`:

```
deb [signed-by=/etc/apt/trusted.gpg.d/hualo.gpg] https://komodo1110.github.io/hualo-apt/ stable main
```

Importa la clave pública:

```
curl -fsSL https://komodo1110.github.io/hualo-apt/pubkey.asc \
  -o /etc/apt/trusted.gpg.d/hualo.gpg
```

Actualiza e instala:

```
pkg update
pkg install <paquete>
```

## Comandos disponibles

| Comando | Descripción |
|---|---|
| `pkg update` | Consulta el índice del repositorio |
| `pkg install <paquete>` | Instala un paquete |
| `pkg remove <paquete>` | Elimina un paquete |
| `pkg upgrade` | Actualiza los paquetes instalados |
| `pkg docs` | Abre la documentación |
| `pkg doctor` | Revisa el estado del entorno |

## Estructura del repositorio

```
apt-repo/
├── pools/main/<letra>/<paquete>/<paquete>_<version>_arm64.deb
├── dists/stable/main/binary-arm64/
├── pubkey.asc
├── docs/
└── .github/workflows/
```

## Firma y seguridad

Toda la cadena de confianza está firmada: los paquetes `.deb` (verificados
por dpkg), el índice `Packages.gz` y `Release`/`InRelease`. La clave pública
está disponible en [`pubkey.asc`](./pubkey.asc).

## Contribuir

Consulta [CONTRIBUTING.md](./CONTRIBUTING.md) para saber cómo proponer un
paquete nuevo o reportar un problema.

## Seguridad

Para reportar una vulnerabilidad, consulta [SECURITY.md](./SECURITY.md).

## Licencia

Distribuido bajo la licencia [MIT](./LICENSE).