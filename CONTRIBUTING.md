# Cómo contribuir

Gracias por querer ayudar con el repositorio apt de **HuaLo**. Este repositorio
publica paquetes `.deb` (aarch64) firmados con GPG y los sirve vía GitHub
Pages. Toda contribución debe mantener el pipeline reproducible y los
paquetes autenticados.

## Formas de contribuir

### 1. Reportar un problema (issues)

- **Bugs del pipeline o de los índices**: abre un issue en la pestaña
  *Issues*. Describe qué esperabas y qué pasó, y si puedes, pega el comando
  `apt-get update`/`pkg update` que falla y su salida.
- **Vulnerabilidades o claves comprometidas**: **no** las escribas en un issue
  público. Usa el flujo privado descrito en
  [`SECURITY.md`](SECURITY.md) (Security Advisory).

### 2. Proponer un paquete `.deb`

Los paquetes van en:

```
pools/main/<letra>/<paquete>/<paquete>_<version>_arm64.deb
```

(a) **Normas del paquete**
- Solo arquitectura `arm64` (aarch64) y solo paquetes Ubuntu/Debian.
- El `.deb` debe ser reproducido por su propia fuente (debian/ + upstream);
  adjunta en la descripción del PR el enlace a la fuente y la verificación
  (`debsig-verify`, hashes, etc.).
- No subas binarios precompilados sin fuente. Si es un paquete propiedad de
  terceros, asegúrate de tener derecho a redistribuirlo y documéntalo.
- No subas claves privadas, credenciales ni datos personales. Nada que no
  deba ser público.

(b) **Cómo enviarlo**
1. Clona el repositorio.
2. Coloca el `.deb` en la ruta correcta de `pools/main/`.
3. (Opcional pero recomendado) Verifica que el índice regenera bien:
   ```bash
   dpkg-scanpackages --multiversion pools/main > dists/stable/main/binary-arm64/Packages
   gzip -9kf dists/stable/main/binary-arm64/Packages
   ```
4. Crea una rama, haz commit y abre un **Pull Request**. En la descripción del
   PR, indica: paquete, versión, fuente upstream, licencia y cómo verificaste
   el binario.
5. El mantenedor revisa, y al hacer merge, el workflow de GitHub Actions
   regenera los índices y los firma automáticamente.

### 3. Mejorar el pipeline (workflows/scripts)

- Trabaja en ramas, mantén los cambios pequeños y legibles.
- Si cambias `.github/workflows/repo-apt.yml`, prueba el pipeline en tu fork
  (con un secret `REPO_GPG_KEY` de pruebas) antes de pedir el merge.
- No añadas secretos nuevos sin actualizar este documento y `SECURITY.md`.

## Estándares

- Texto: español (o añade la variante en inglés en `README.en.md` si es
  necesario).
- Sin rutas locales, sin datos personales, sin secretos en el historial.
- Los commits: prefijos descriptivos (`apt:`, `fix:`, `docs:`, `ci:`).

## Código de conducta

Sé respetuoso y constructivo. El proyecto pretende ser una experiencia de
distribución profesional para una app de terminal; la calidad y la seguridad
van primero.