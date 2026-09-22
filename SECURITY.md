# Seguridad

## Reportar una vulnerabilidad

Este repositorio distribuye paquetes `.deb` firmados para la app **HuaLo**. Si
encuentras una vulnerabilidad de seguridad en el contenido, el pipeline de
publicación o los índices:

1. **No abras un issue público** con los detalles si la vulnerabilidad aún no
   está corregida. Usa el flujo privado de GitHub:
   **Issues → New issue → "Report a security vulnerability"** (o el formulario
   de GitHub Security Advisories del repositorio).
2. Incluye:
   - Descripción breve y reproducible del problema.
   - Pasos para reproducirlo o, si es un paquete malicioso, el nombre/versión
     del `.deb` y su hash SHA-256.
   - Impacto estimado (¿qué puede lograr un atacante?).
   - Contacto opcional para seguimiento.

Las vulnerabilidades se tratan de forma privada hasta tener una corrección y/o
aviso coordinado, y se reconocerá al autor en el advisory si lo desea.

## Clave GPG comprometida

Los paquetes se firman con una clave GPG cuyo **secreto nunca vive en este
repositorio**: solo se inyecta como secret `REPO_GPG_KEY` de GitHub Actions
para firmar los índices. Si sospechas que la clave privada se ha filtrado o
comprometido:

1. **Reporta** el incidente por el canal privado indicado arriba lo antes
   posible.
2. La clave pública del repositorio (`pubkey.asc`) muestra una huella
   dactilar; si esta cambia, **los usuarios deben migrar** a la nueva clave:
   la vieja deja de ser de confianza.
3. El mantenedor regenerará la clave, actualizará el secret `REPO_GPG_KEY` y
   el `pubkey.asc`, y publicará un aviso.

## Buenas prácticas de este repositorio

- El secret de firma (`REPO_GPG_KEY`) es de solo escritura en GitHub Actions y
  **no** tiene acceso de lectura ni aparece en logs.
- Los permisos del workflow se limitan a lo necesario
  (`contents: write`, `pages: write`, `id-token: write`).
- No se suben rutas locales, configuraciones del dispositivo ni credenciales.
- Al detectar cualquier fuga, se trata como incidente: revocar, regenerar,
  purgar del historial y avisar.