# Práctica: tu perfil en el repo del curso (fork + pull request)

**Objetivo:** al final de la clase el repo principal tendrá un menú (`index.html`) con el perfil de cada estudiante.
Vas a trabajar como en un equipo real: **fork → rama → commit → sincronizar → pull request**.

Repo principal (upstream): `https://github.com/CristopherLarios/Proyecto-CI-V2`

---

## 1. Cómo está organizado el repo

```
index.html                         ← MENÚ (archivo compartido: solo tocas TU slot)
css/main.css                       ← estilos compartidos (NO se toca)
estudiantes/
├── _plantilla/index.html          ← plantilla (NO se toca, se copia)
├── cristopherlarios/index.html    ← ejemplo del docente
└── TU-USUARIO-GITHUB/             ← TU carpeta (la creas tú)
    ├── index.html
    ├── foto.jpg                   (opcional)
    └── estilo.css                 (opcional)
```

## 2. Reglas para que NO haya conflictos

Seremos 10 personas trabajando al mismo tiempo. Git solo genera conflicto cuando **dos personas cambian las mismas líneas**. Por eso:

| ✅ Sí puedes | ❌ No puedes |
|---|---|
| Crear y editar todo dentro de `estudiantes/TU-USUARIO/` | Editar la carpeta de otro compañero |
| Cambiar **una sola línea** en `index.html`: la `<li>` de **tu slot** | Mover, borrar o reordenar slots o comentarios `<!-- SLOT -->` |
| Poner tus estilos en `estudiantes/TU-USUARIO/estilo.css` | Modificar `css/main.css` o `_plantilla/` |
| `git add` de tus archivos por nombre | `git add .` sin revisar `git status` |

> 🎯 **El docente te asigna un número de slot (01–10).** Anótalo: ____

Tu carpeta usa **tu usuario de GitHub en minúsculas** (ej. `estudiantes/mariaperez/`). Así nunca chocan dos nombres.

---

## 3. Paso a paso

> En Windows usa **Git Bash** para que todos los comandos funcionen igual.
> Reemplaza `TU-USUARIO` por tu usuario de GitHub y `NN` por tu número de slot.

### Paso 1 · Hacer fork (en GitHub)
1. Abre `https://github.com/CristopherLarios/Proyecto-CI-V2`
2. Clic en **Fork** → **Create fork**. Ahora tienes `https://github.com/TU-USUARIO/Proyecto-CI-V2`.

### Paso 2 · Clonar TU fork
```bash
git clone https://github.com/TU-USUARIO/Proyecto-CI-V2.git
cd Proyecto-CI-V2
```

### Paso 3 · Conectar el repo original como `upstream`
```bash
git remote add upstream https://github.com/CristopherLarios/Proyecto-CI-V2.git
git remote -v
```
Debes ver dos remotos:
- `origin` → tu fork (aquí haces **push**)
- `upstream` → repo del docente (de aquí **traes** cambios)

### Paso 4 · Crear tu rama de trabajo
```bash
git switch -c perfil/TU-USUARIO
```
Nunca trabajes directo en `main`.

### Paso 5 · Crear tu perfil desde la plantilla
```bash
mkdir -p estudiantes/TU-USUARIO
cp estudiantes/_plantilla/index.html estudiantes/TU-USUARIO/index.html
```
Abre `estudiantes/TU-USUARIO/index.html` y reemplaza todo lo que está entre `[CORCHETES]`:
- **Datos:** nombre, carrera, usuario de GitHub
- **Sobre mí:** quién eres, qué te gusta (hobbies, música, videojuegos…)
- **Habilidades:** lo que sabes hacer
- **Lo que aprendí en el curso:** resumen con tus palabras y al menos 4 comandos

Mira el ejemplo del docente: `estudiantes/cristopherlarios/index.html`.

Primer commit:
```bash
git status
git add estudiantes/TU-USUARIO
git commit -m "Agrega perfil de TU-USUARIO"
```

### Paso 6 · Enlazar tu perfil en el menú (¡solo tu línea!)
Abre `index.html` y busca **tu** slot. Ejemplo para el slot 03:

```html
<!-- ===== SLOT 03 ===== -->
<li class="pendiente">Slot 03 · disponible</li>
<!-- ===== FIN SLOT 03 ===== -->
```

Cambia **solo la línea del medio** por:

```html
<!-- ===== SLOT 03 ===== -->
<li><a href="estudiantes/TU-USUARIO/">Tu Nombre</a> · Tu rol</li>
<!-- ===== FIN SLOT 03 ===== -->
```

Verifica que cambiaste exactamente **1 línea**:
```bash
git diff index.html
```
Debe mostrar una línea con `-` (la de "disponible") y una con `+` (la tuya). Nada más.

```bash
git add index.html
git commit -m "Enlaza perfil de TU-USUARIO en el slot NN"
```

### Paso 7 · Revisar en el navegador
Abre `index.html` con doble clic → haz clic en tu nombre → revisa tu perfil → "Volver al menú" debe funcionar.

### Paso 8 · Sincronizar con el repo original (OBLIGATORIO antes del PR)
Mientras trabajabas, el docente pudo aceptar PRs de tus compañeros. Trae esos cambios a tu rama:
```bash
git fetch upstream
git merge upstream/main
```
- Si respetaste las reglas, el merge se hace **solo, sin conflictos** (cada quien cambió líneas distintas).
- Si se abre el editor con un mensaje de merge, guarda y cierra (en vim: `:wq`).
- Vuelve a abrir `index.html` en el navegador: ahora verás también a tus compañeros.

<details>
<summary>⚠️ ¿Te salió un conflicto? Ábreme</summary>

Casi siempre significa que tocaste algo fuera de tu slot.

```bash
git status            # muestra el archivo en conflicto
```
Abre el archivo y busca las marcas:
```
<<<<<<< HEAD
(tu versión)
=======
(versión del repo original)
>>>>>>> upstream/main
```
1. Deja **las líneas de tus compañeros tal como vienen de upstream**.
2. Deja **solo tu línea** en tu slot.
3. Borra las marcas `<<<<<<<`, `=======`, `>>>>>>>`.

```bash
git add index.html
git commit -m "Resuelve conflicto con upstream"
```
Si te perdiste, cancela y pide ayuda: `git merge --abort`
</details>

### Paso 9 · Subir tu rama a TU fork
```bash
git push -u origin perfil/TU-USUARIO
```

### Paso 10 · Crear el Pull Request
1. Entra a tu fork en GitHub → aparecerá **Compare & pull request**.
2. Verifica la dirección:
   - **base repository:** `CristopherLarios/Proyecto-CI-V2` · **base:** `main`
   - **head repository:** `TU-USUARIO/Proyecto-CI-V2` · **compare:** `perfil/TU-USUARIO`
3. Título: `Perfil de Tu Nombre (slot NN)`
4. En la pestaña **Files changed** solo deben aparecer:
   - `estudiantes/TU-USUARIO/...`
   - `index.html` con **1 línea** cambiada
5. Clic en **Create pull request** y completa el checklist.

### Paso 11 · Si el docente te pide cambios
No crees otro PR. Corrige en la misma rama y vuelve a subir:
```bash
git add estudiantes/TU-USUARIO
git commit -m "Corrige perfil según revisión"
git push
```
El PR se actualiza solo.

### Paso 12 · Después de que acepten tu PR (dejar tu fork al día)
```bash
git switch main
git fetch upstream
git merge upstream/main
git push origin main
```

---

## 4. Resumen de comandos

```bash
git clone https://github.com/TU-USUARIO/Proyecto-CI-V2.git
cd Proyecto-CI-V2
git remote add upstream https://github.com/CristopherLarios/Proyecto-CI-V2.git
git switch -c perfil/TU-USUARIO
mkdir -p estudiantes/TU-USUARIO
cp estudiantes/_plantilla/index.html estudiantes/TU-USUARIO/index.html
# ...editar perfil...
git add estudiantes/TU-USUARIO && git commit -m "Agrega perfil de TU-USUARIO"
# ...editar SOLO tu slot en index.html...
git add index.html && git commit -m "Enlaza perfil de TU-USUARIO en el slot NN"
git fetch upstream && git merge upstream/main
git push -u origin perfil/TU-USUARIO
# → abrir Pull Request en GitHub
```

---

## 5. Notas para el docente

- **Antes de clase:** asigna un slot (01–10) a cada estudiante, por ejemplo en orden de lista.
- **Revisión de cada PR:** en *Files changed* solo deben estar la carpeta del alumno y 1 línea de `index.html`. Si hay más, pide cambios.
- **Merge:** acepta los PR en cualquier orden. Como cada slot está separado por comentarios que nadie toca, los PR no se bloquean entre sí.
- **Ver el resultado:** *Settings → Pages → Deploy from a branch → `main` / root*. El menú queda en `https://cristopherlarios.github.io/Proyecto-CI-V2/`.
- **¿Más de 10 alumnos?** Agrega slots nuevos en `main` **antes** de que hagan fork (o pídeles sincronizar con `upstream` después).
