# vcard-sergio — Tarjeta digital · INTERCEPCOM

GitHub Pages: **https://tarjeta.intercepcom.co/** · Repo: `ArdaTX/vcard-sergio` · Rama `main` / `/`.

## Estructura

| Archivo | Propósito |
|---|---|
| `index.html` | Tarjeta single-file (CSS/JS inline, sin build). Rutas relativas `./` para funcionar en dominio custom y en `ardatx.github.io/vcard-sergio/`. |
| `avatar.jpg` / `avatar-512.jpg` | Foto real 800/512px optimizada desde `intercepcom2/public/team/sergio-molinares.jpg`. OG usa `og-card.jpg` 1200×630. |
| `og-card.jpg` | Imagen social 1200×630 (foto + navy `#0A1628` + franja teal). |
| `apple-touch-icon.jpg` | 180px para iOS. |
| `favicon.svg` | Monograma `S` navy/cream/teal (sin dependencias). |
| `sergio.vcf` | vCard 3.0 con `TEL CELL`, `PHOTO;VALUE=URI`, `UID`, `REV`, `LABEL`. |
| `manifest.webmanifest` / `robots.txt` / `sitemap.xml` / `404.html` | Instalabilidad, SEO y error page con misma marca. |
| `CNAME` | `tarjeta.intercepcom.co`. No borrar. |
| `.nojekyll` | Desactiva Jekyll para que `_`/`.` se sirvan tal cual. |

## Decisiones senior

1. **Rutas relativas**: antes `/avatar.png` y `/sergio.vcf` rompían fuera del dominio custom. Ahora `./`.
2. **Label corregido**: `Empresa` → `Teléfono` con formato `+57 323 433 0185` + hint `Llamar o WhatsApp`. Se añadió botón WhatsApp `wa.me/573234330185` (estándar en CO).
3. **Foto real**: `avatar.png` (3 KB, 59 colores, placeholder) reemplazado por foto del equipo. `avatar.png` legacy se elimina.
4. **SEO/social**: `lang es-CO`, `canonical https`, OG `profile` + `og:image:width/height/alt`, `twitter summary_large_image`, JSON-LD `Person`, `robots.txt` + `sitemap.xml`.
5. **A11y**: skip-link, landmarks, `aria-label` ES, focos visibles, contraste subtitle `#D4DDDC` sobre navy, targets ≥48px, `prefers-reduced-motion`, `print` que oculta hero/acciones.
6. **Seguridad**: Pages no envía `CSP/HSTS/X-Frame`. Mitigado con `referrer strict-origin-when-cross-origin`, `rel me` para verificación, enlaces externos con `noopener noreferrer`, fila PGP → `intercepcom.co/pgp-key.txt` y footer → `security.txt` canónico del dominio principal (no duplicar).
7. **Performance**: 0 dependencias JS, CSS inline, `display=swap`, `fetchpriority=high` solo avatar, `decoding=async`, `srcset` 512/800, JPG progresivos (~48/21/37 KB).

## DNS + HTTPS (pendiente operativo)

- DNS: `tarjeta CNAME ardatx.github.io` (DNS-only, sin proxy naranja durante emisión). Verifica con `dig CNAME tarjeta.intercepcom.co`.
- GitHub → Settings → Pages → Custom domain `tarjeta.intercepcom.co` debe mostrar `DNS check successful` y emitir cert Let's Encrypt. Luego activar **Enforce HTTPS**. Hoy `https_enforced:false` y `html_url:http://...` con cert `*.github.io` → `https://` falla con `ERR_CERT_COMMON_NAME_INVALID`. Es esperado hasta que emita (creado 2026-10-01, puede tardar hasta 24h).
- CAA del apex permite `letsencrypt.org` — OK.
- Tras emitir, verificar: `curl -sI https://tarjeta.intercepcom.co/` → `200` + `curl --resolve` cert con `subjectAltName: tarjeta.intercepcom.co`.

## Local

```bash
python3 -m http.server 8000
# http://localhost:8000/
python3 -c "import html.parser" # smoke
curl -sI http://localhost:8000/sergio.vcf
```
