# Bot Soft — sitio para Meta / WhatsApp Business

Sitio estático de tres páginas. No hay build, ni npm, ni framework: GitHub
Pages sirve los archivos tal cual.

```
index.html       el servicio y quién lo desarrolla
privacidad.html  política de privacidad          <- la que revisa Meta
terminos.html    términos del servicio           <- la otra que revisa Meta
estilo.css       todo el diseño, un solo archivo
```

---

## 1. Antes de publicar — queda UN dato

Ya están puestos el correo (`eliuthseguil@gmail.com`), el alojamiento
(servidor propio en Contabo, Alemania), Chatwoot autohospedado y la retención
de 2 años.

Falta una sola cosa, marcada con recuadro en `privacidad.html`, **sección 6**:

> `[NOMBRE DEL PROVEEDOR DEL MODELO DE LENGUAJE]`

Poné cuál usás (OpenAI, Anthropic, Google…). Si el modelo corre en el mismo
VPS y el texto no sale hacia afuera, borrá esa línea y decilo explícitamente:
es un punto a favor.

Es el único dato que no se puede dejar genérico. Enumerar a quién le llegan
los datos es un requisito, y es lo que se contrasta contra lo que la app hace
de verdad.

---

## 2. Publicar en GitHub Pages

```bash
cd sitio-bot-whatsapp
git init
git add .
git commit -m "Sitio de Bot Soft"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/bot-soft.git
git push -u origin main
```

En GitHub: **Settings → Pages → Source: Deploy from a branch → main / (root)
→ Save**. En un par de minutos queda en:

```
https://TU-USUARIO.github.io/bot-soft/
https://TU-USUARIO.github.io/bot-soft/privacidad.html
https://TU-USUARIO.github.io/bot-soft/terminos.html
```

El repositorio debe ser **público** para que Pages funcione en una cuenta
gratuita.

> Si en vez de `bot-soft` nombrás el repositorio `TU-USUARIO.github.io`, el
> sitio queda en la raíz: `https://TU-USUARIO.github.io/`. Más limpio si no
> pensás publicar otra cosa ahí.

---

## 3. Dónde van esas URLs en Meta

En **developers.facebook.com → tu app Bot Soft → Configuración → Básica**:

| Campo | URL |
|---|---|
| URL de la política de privacidad | `.../privacidad.html` |
| URL de las condiciones del servicio | `.../terminos.html` |
| Sitio web | `.../` (la raíz) |

Y en **Meta Business Suite → Configuración del negocio → Información del
negocio**, el sitio web del negocio.

---

## 4. Ver cómo queda antes de subirlo

Abrí `index.html` con doble clic — basta, es estático. Si preferís servirlo:

```bash
python -m http.server 8000
# http://localhost:8000
```

---

## Dos cosas que conviene saber

**HTTPS lo pone GitHub Pages solo**, y Meta lo exige. No hay que hacer nada,
pero verificá que la URL arranque con `https://` antes de pegarla.

**El sitio se adapta al tema del visitante** (claro u oscuro) y al tamaño de
pantalla. Probalo en el teléfono: la revisión de Meta la hace una persona, y
un sitio roto en móvil es un motivo de rechazo barato de evitar.

**Si cambia algo del servicio, actualizá la política.** El plazo de retención,
el proveedor del modelo, dónde está alojado: son justamente los datos que
pueden quedar desactualizados sin que nadie lo note, y una política que ya no
describe la realidad vale menos que ninguna.
