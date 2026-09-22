# Notas de mantenimiento (uso interno)

Este archivo no es documentación de usuario. Contiene las notas de
configuración del repositorio para quien lo mantenga.

## Configuración inicial (ya realizada)

1. Repo público `hualo-apt` en la cuenta `komodo1110`.
2. GitHub Pages activado: **Settings > Pages > Deploy from a branch = gh-pages**.
3. Clave GPG del repositorio (ed25519). La clave privada se exporta y se
   guarda **solo localmente**, nunca en este repositorio. Se sube como
   secreto de GitHub Actions:
   **Settings > Secrets and variables > Actions > REPO_GPG_KEY**
   (contenido: la clave privada en formato ASCII armor).
4. La clave pública correspondiente vive en [`pubkey.asc`](./pubkey.asc)
   en la raíz del repo.

## Publicar un paquete nuevo

1. Sube el `.deb` en `pools/main/<letra>/<paquete>/`.
2. Haz push a `main`.
3. El workflow `.github/workflows/repo-apt.yml` regenera los índices,
   firma con `REPO_GPG_KEY` y despliega a `gh-pages` automáticamente.
4. Si `pools/main` está vacío, el workflow genera un índice vacío sin
   fallar (para que el esqueleto se publique igual).

## Rotar la clave GPG (si se compromete)

1. Genera un par de claves nuevo.
2. Reemplaza el secreto `REPO_GPG_KEY` en Settings > Secrets.
3. Reemplaza `pubkey.asc` en la raíz del repo.
4. Revoca la clave anterior y publícalo en las notas de la próxima release.
5. Avisa a los usuarios de que deben reimportar la clave pública.