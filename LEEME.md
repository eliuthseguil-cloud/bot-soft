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

## 1. Estado — faltan tres datos de contacto

En `index.html`, sección Contacto, quedan marcados con comentario:

- `+51 TELEFONO` — el número real, en formato internacional
- `DISTRITO` — basta el distrito, no hace falta calle ni número
- `botsoft@krovax.site` — hay que crear el buzón (ver más abajo)

Meta pide correo corporativo, dirección física **o** teléfono. Con cualquiera
de los tres alcanza, pero los tres juntos dan señal de negocio real, que es lo
que se está evaluando.

Lo demás está declarado:

| Dato | Valor |
|---|---|
| Contacto | `david_seguil@hotmail.com` (el mismo de la cuenta de Meta) |
| Alojamiento | servidor propio, Contabo GmbH (Alemania) |
| Atención y almacenamiento | Chatwoot autohospedado |
| Modelo de lenguaje | Google (Gemini) |
| Retención | 2 años desde el último mensaje |

Si alguno de esos cambia —cambiás de proveedor de modelo, movés el servidor,
ajustás el plazo— hay que actualizar `privacidad.html`. Una política que dejó
de describir la realidad es peor que no tener ninguna: declara algo que no se
cumple.

---

## 2. Publicar en GitHub Pages

El repositorio local ya está iniciado, con el primer commit hecho y la
identidad de este repo apuntando a `david_seguil@hotmail.com` (el `user.name`
global de esta máquina es la cuenta de ISIL; acá se sobreescribe solo para
este proyecto).

**Paso 1 — crear el repositorio vacío en GitHub.** En
<https://github.com/new>:

- Owner: `eliuthseguil-cloud`
- Repository name: `bot-soft`
- Visibility: **Public** (Pages no funciona en privado con cuenta gratuita)
- **No** marcar «Add a README», ni .gitignore, ni licencia: el repo local ya
  tiene contenido y GitHub rechazaría el push.

**Paso 2 — subirlo:**

```bash
cd sitio-bot-whatsapp
git remote add origin https://github.com/eliuthseguil-cloud/bot-soft.git
git push -u origin main
```

Git va a pedir usuario y contraseña. **La contraseña de GitHub no sirve**:
hay que usar un token. Se crea en Settings → Developer settings → Personal
access tokens → Tokens (classic) → Generate new token, con el permiso `repo`.
Ese token se pega donde pide la contraseña.

**Paso 3 — encender Pages.** En el repositorio: **Settings → Pages →
Source: Deploy from a branch → Branch: `main` / `(root)` → Save**.

En uno o dos minutos queda publicado:

```
https://eliuthseguil-cloud.github.io/bot-soft/
https://eliuthseguil-cloud.github.io/bot-soft/privacidad.html
https://eliuthseguil-cloud.github.io/bot-soft/terminos.html
```

> **Alternativa:** si nombrás el repositorio `eliuthseguil-cloud.github.io`
> en vez de `bot-soft`, el sitio queda en la raíz
> (`https://eliuthseguil-cloud.github.io/`). Se ve más como el sitio de un
> negocio, que es lo que Meta espera. La contra es que esa dirección es única
> por cuenta: si después querés publicar otra cosa, ya está ocupada.

**Para actualizar el sitio más adelante:**

```bash
git add .
git commit -m "qué cambió"
git push
```

Pages reconstruye solo en un minuto.

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


---

## Dominio propio — por qué y cómo

Meta desconfía de los sitios en subdominios gratuitos de plataformas
(`.wix.com`, `.blogspot.com`, y también `.github.io`). El sitio sigue alojado
en GitHub Pages, pero se sirve bajo **botsoft.krovax.site**, que es un dominio
propio registrado en Namecheap.

El archivo `CNAME` de este repositorio ya declara ese dominio. Falta el DNS.

### Paso 1 — en Namecheap

Panel de `krovax.site` → **Advanced DNS** → Add New Record:

| Campo | Valor |
|---|---|
| Type | `CNAME Record` |
| Host | `botsoft` |
| Value | `eliuthseguil-cloud.github.io.` |
| TTL | Automatic |

El punto final en el Value no es un error de tipeo: cierra el nombre.

### Paso 2 — en GitHub

Settings → Pages → **Custom domain**: `botsoft.krovax.site` → Save.

Ahora sí ese campo tiene sentido: es exactamente para esto. Cuando GitHub
valide el DNS (unos minutos, a veces hasta una hora), marcá **Enforce HTTPS**.

### Paso 3 — verificar

```
https://botsoft.krovax.site/
https://botsoft.krovax.site/privacidad.html
https://botsoft.krovax.site/terminos.html
```

Esas son las URLs que van en Meta. La vieja de `github.io` sigue funcionando
y redirige sola.

---

## Correo con el dominio propio

Meta pide un correo «corporativo»; un Gmail cuenta como débil. **Zoho Mail**
tiene un plan gratuito que alcanza de sobra:

1. <https://www.zoho.com/mail/> → Pricing → **Forever Free Plan**
2. Agregar el dominio `krovax.site`
3. Zoho da unos registros TXT y MX para poner en Namecheap → Advanced DNS
4. Crear el buzón `botsoft@krovax.site`

Hasta que esté listo, ese correo del sitio **no recibe**. Si Meta escribe
antes de configurarlo, el mensaje se pierde: conviene hacerlo antes de mandar
la verificación.

Alternativa más rápida: un *alias de reenvío* en Namecheap (Domain →
Redirect Email), que manda lo de `botsoft@krovax.site` a tu Gmail. No sirve
para responder desde esa dirección, pero al menos nada se pierde.

---

## La API key NO va en este repositorio

La clave de Gemini se pasa al servicio por variable de entorno en el VPS, nunca
en el código ni en un archivo versionado. Si una clave de Google llega a un
repositorio público, GitHub la detecta y Google la revoca automáticamente — con
el bot caído hasta que se genere otra.

En el VPS:

```bash
# en el .env del servicio, con chmod 600
GEMINI_API_KEY=...
```

Conviene además restringirla en Google Cloud Console (APIs y servicios →
Credenciales): limitarla a la API de Generative Language, para que si se filtra
no sirva para nada más.

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
