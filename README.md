# Práctica: Git en entornos distribuidos

Practicamos con comandos de Git cómo se trabaja entre varias personas con un repositorio remoto: traer los cambios de otro, subir los tuyos cuando otro ya ha subido los suyos, resolver un conflicto con `merge`, reorganizar con `rebase`, compartir una rama y proponer cambios con un *fork* y un *pull request*. Como la práctica es individual, **harás de dos personas**: tú y un compañero imaginario, cada uno con su propia copia del repositorio.

Sigue este guion paso a paso, sin saltarte ninguno. Las partes 1 a 6 llevan alrededor de una hora; si lo necesitas, puedes parar al final de una parte y seguir otro día. La parte 0 es solo para el profesor.

## ⚠️ Antes de empezar

- 💻 Todo lo de Git se hace con **comandos de Git** en la terminal **Git Bash** de VS Code, como en la práctica de ramas: **Terminal > Nuevo terminal** (*Terminal > New Terminal*) y, a la derecha del panel de la terminal, flecha **∨** junto al **+** > **Git Bash**. Las carpetas y los archivos se crean, se abren y se editan desde VS Code.
- 👥 **Vas a ser dos personas.** Tendrás dos copias del repositorio, en dos carpetas: `yo` (tú) y `companero` (tu compañero). Cada paso dice en cuál va. Antes de escribir un comando, mira en la línea de Git Bash en qué carpeta estás: termina en `practica_remotos/yo (master)` o en `practica_remotos/companero (master)`. Para pasar de una carpeta a otra usarás `cd`, el único comando de la práctica que no es de Git.
- 🔢 Cada paso tiene **como mucho un comando**. Escríbelo o cópialo, pulsa **Intro** y no pases al siguiente hasta ver el resultado. Como en la práctica de ramas, debajo de cada comando tienes:
  - 📝 qué hace el comando y cada una de sus partes;
  - 👀 qué verás al ejecutarlo;
  - 💡 la idea importante, cuando la hay;
  - 🖱️ cómo se hace lo mismo con los botones de VS Code, la primera vez que sale cada comando nuevo.
- 👀 El panel de **control de código fuente** (**Ctrl + Mayús + G**) se mira, pero no se tocan sus botones (**Confirmar**, **Publicar Branch**, **Sincronizar cambios**…). Verás en él los dos repositorios, `yo` y `companero`. El historial lo miraremos con `git log`.
- 🗂️ Los archivos se abren desde el **Explorador** (**Ctrl + Mayús + E**), dentro de la carpeta que diga el paso: hay un `index.html` en `yo` y otro en `companero`. Después de editar, guarda con **Ctrl + S** y **cierra la pestaña** (la **x**), para no confundirte de archivo en el paso siguiente.
- 📄 No edites este `README.md`: solo se cambia `index.html`.
- ✍️ Escribe los mensajes de commit tal como vienen en cada paso.
- 🔢 Los códigos de commit (*hashes*, como `eb0ebcd`) de los ejemplos no coincidirán con los tuyos.
- 🌐 Git puede mostrar sus mensajes en inglés o en español, según el ordenador. El guion da el inglés y, en los mensajes importantes, el español. Las líneas de resumen de `git push` (`[new branch]`, `[rejected]`, `[deleted]`) salen siempre en inglés.
- 🚫 **No uses `git pull`**, aunque Git te lo sugiera en sus mensajes. `git pull` es un `git fetch` seguido de un `git merge` (Pro Git 3.5, «Traer y fusionar»): aquí haremos los dos comandos por separado, para ver qué pasa entre uno y otro.
- 📖 Si al final de la terminal aparece `:`, Git te está mostrando la salida por páginas: pulsa **q** para salir.

---

## 0️⃣ Parte 0. Solo el profesor: crear el repositorio base

> Si eres alumno, salta a la Parte 1: este repositorio ya está creado y vas a hacer un *fork* de él.

**0.1.** Dentro de `vs_projects`, crea una carpeta `practica_remotos_base` y copia en ella `index.html` y este `README.md`. Ábrela con **Archivo > Abrir carpeta** y abre una terminal de **Git Bash**.

**0.2.** Convierte la carpeta en un repositorio con la rama `master`:

```
git init -b master
```

**0.3.** Prepara los dos archivos:

```
git add .
```

**0.4.** Guarda el primer commit:

```
git commit -m "Punto de partida"
```

**0.5.** En [github.com](https://github.com) crea el repositorio **vacío** `practica_remotos_base`: **+ > New repository**, **Public**, **Add README** desactivado, sin `.gitignore` ni licencia, y **Create repository**.

**0.6.** Añade el remoto:

```
git remote add origin https://github.com/victorrpf/practica_remotos_base.git
```

**0.7.** Sube la rama `master`:

```
git push origin master
```

**0.8.** **No lo archives**, a diferencia de `practica_ramas_base`: en un repositorio archivado no se pueden abrir *pull requests*, y la Parte 6 termina con uno. Los alumnos tampoco pueden subir nada aquí, porque no tienen permiso de escritura: por eso hacen el fork.

**0.9.** GitHub no deja hacer un fork de un repositorio propio en la misma cuenta. Para ensayar la práctica, haz el fork en una organización tuya (**+ > New organization**, plan gratuito) o con una segunda cuenta.

**0.10.** Los *pull requests* de los alumnos llegan a la pestaña **Pull requests** de este repositorio. No los fusiones (**Merge pull request**): solo se revisan.

---

## 1️⃣ Parte 1. Tu fork y dos copias (8 min)

**1.** En GitHub, con tu cuenta, abre el repositorio del profesor: [github.com/victorrpf/practica_remotos_base](https://github.com/victorrpf/practica_remotos_base). Pulsa **Fork**, arriba a la derecha. En la página que aparece:
- En **Repository name**, cambia `practica_remotos_base` por `practica_remotos`.
- Deja marcado **Copy the `master` branch only**.
- Pulsa **Create fork**.

👀 Se abre tu repositorio, `TU_USUARIO/practica_remotos`, y debajo del nombre pone `forked from victorrpf/practica_remotos_base`.

💡 Un **fork** es una copia del repositorio de otra persona en tu cuenta de GitHub. En el del profesor no puedes subir nada; en tu fork, sí. Al final le propondrás tus cambios con un *pull request* (Pro Git 5.2, «Proyecto público bifurcado», y 6.2).

⚠️ En tu fork, no pulses **Sync fork** ni **Contribute**.

**2.** Abre VS Code. Entra en **Archivo > Abrir carpeta** (*File > Open Folder*), ve a tu carpeta `vs_projects`, pulsa **Nueva carpeta** y llámala `practica_remotos`. Entra en ella y pulsa **Seleccionar carpeta**. Si VS Code te pregunta si confías en los autores, pulsa **Sí, confío en los autores**. Abre una terminal de **Git Bash**.

👀 La línea de Git Bash termina en `vs_projects/practica_remotos`, sin `(master)`: esta carpeta no es un repositorio. Solo va a guardar las dos copias.

**3.** Clona tu fork en una carpeta llamada `yo`. Escribe en la terminal `git clone `, con un espacio al final, y **no pulses Intro**. En la página de tu fork, pulsa el botón verde **Code**, pestaña **HTTPS**, y el botón de copiar que hay junto a la dirección. Pega la dirección en la terminal (**Ctrl + V**), escribe un espacio y `yo`, y pulsa **Intro**. Tiene que quedar así, con tu usuario:

```
git clone https://github.com/TU_USUARIO/practica_remotos.git yo
```

📝 Como en la práctica de ramas, `git clone` descarga una copia completa del repositorio y guarda su dirección como remoto `origin`. La última palabra, `yo`, es el nombre de la carpeta que se crea.

👀 `Cloning into 'yo'...` (en español, `Clonando en 'yo'...`). En el Explorador aparece la carpeta `yo`.

**4.** Clona otra vez tu fork, ahora en la carpeta `companero`. Pulsa la flecha **↑** del teclado para recuperar el comando anterior y cambia `yo` por `companero`:

```
git clone https://github.com/TU_USUARIO/practica_remotos.git companero
```

👀 `Cloning into 'companero'...`.

💡 Ahora hay tres copias del mismo repositorio: la de GitHub y dos en tu portátil. En el trabajo real, cada una estaría en el ordenador de una persona.

**5.** Entra en la copia de tu compañero:

```
cd companero
```

📝 `cd` (de *change directory*, «cambiar de carpeta») te lleva a la carpeta que escribes.

👀 La línea de Git Bash termina en `practica_remotos/companero (master)`.

**6.** Pon el nombre de tu compañero, solo en esta copia:

```
git config user.name "Compañero"
```

📝 Sin `--global`, el ajuste se guarda solo en este repositorio y manda sobre tu configuración global (Pro Git 1.6). Los commits que hagas en esta copia aparecerán como de «Compañero».

👀 No muestra nada.

**7.** Pon también su correo:

```
git config user.email "companero@example.com"
```

👀 No muestra nada. `example.com` es un dominio reservado para ejemplos: no es el correo de nadie.

**8.** Comprueba el nombre:

```
git config user.name
```

👀 `Compañero`.

**9.** Vuelve a tu copia:

```
cd ../yo
```

📝 `..` es la carpeta de arriba, `practica_remotos`: el comando sube a ella y entra en `yo`.

👀 La línea de Git Bash termina en `practica_remotos/yo (master)`.

**10.** Comprueba el nombre en tu copia:

```
git config user.name
```

👀 Tu nombre: en esta copia sigue valiendo tu configuración global.

---

## 2️⃣ Parte 2. Traer los cambios de otro (7 min)

Tú subes un cambio y tu compañero lo trae a su copia.

**11.** *(yo)* En el Explorador, abre `yo` > `index.html`. Haz clic al final de la línea `<li>Leche</li>` y pulsa **Intro**. Escribe `<li>` (VS Code añade `</li>`) y, entre las dos etiquetas, `Huevos`:

```
    <li>Huevos</li>
```

Guarda con **Ctrl + S** y cierra la pestaña.

**12.** Guarda el cambio en un commit:

```
git commit -a -m "Añade huevos"
```

📝 `-a` prepara antes todos los archivos que Git ya sigue y que han cambiado: te ahorra el `git add` (Pro Git 2.2, «Saltar el área de preparación»). Los archivos nuevos sí necesitan `git add`.

👀 `[master eb0ebcd] Añade huevos` y `1 file changed, 1 insertion(+)`.

**13.** Súbelo a tu fork:

```
git push origin master
```

👀 La última línea es `e29255e..eb0ebcd  master -> master`. Si te pide iniciar sesión en GitHub, hazlo como en la práctica de ramas.

**14.** Pasa a la copia de tu compañero:

```
cd ../companero
```

👀 `practica_remotos/companero (master)`.

**15.** Mira su estado:

```
git status
```

👀 `Your branch is up to date with 'origin/master'.` (en español, `Tu rama está actualizada con 'origin/master'.`).

💡 ¡Pero no lo está! En GitHub ya está «Añade huevos». Git no se conecta al remoto por su cuenta: `origin/master` es donde estaba `master` en GitHub la última vez que esta copia habló con él, al clonar.

**16.** Pregunta a GitHub qué hay de nuevo:

```
git fetch origin
```

📝 `git fetch` se conecta al remoto, descarga los commits que no tienes y mueve `origin/master` hasta donde está ahora en GitHub. No cambia tus archivos ni tu rama `master` (Pro Git 3.5).

👀 `e29255e..eb0ebcd  master     -> origin/master`. El `index.html` del compañero no ha cambiado.

🖱️ Botón **···** de arriba del panel de control de código fuente > **"Pull", "Push"** > **Capturar** (*Fetch*).

**17.** Mira el estado otra vez:

```
git status
```

👀 `Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.` (en español, `Tu rama está detrás de 'origin/master' por 1 commit, y puede ser avanzada rápido.`). Git sugiere `git pull`: no lo uses.

**18.** Trae el commit a la rama `master` del compañero:

```
git merge origin/master
```

📝 `git merge` fusiona en tu rama la rama remota `origin/master`, igual que en la práctica de ramas fusionabas `libros` en `master`.

👀 `Updating e29255e..eb0ebcd` (en español, `Actualizando…`) y `Fast-forward`. Abre `companero` > `index.html`: ya están los huevos. Cierra la pestaña.

💡 `git fetch` más `git merge` es lo que hace `git pull` de una vez.

🖱️ **···** > **Rama** (*Branch*) > **Combinar...** (*Merge...*), y elige `origin/master`.

---

## 3️⃣ Parte 3. Los dos cambiáis lo mismo: push rechazado y conflicto (12 min)

Ahora tú y tu compañero añadís a la vez un elemento en el mismo sitio de la lista.

**19.** Pasa a tu copia:

```
cd ../yo
```

**20.** *(yo)* En `yo` > `index.html`, añade debajo de `<li>Huevos</li>` esta línea. Guarda y cierra la pestaña.

```
    <li>Pimientos</li>
```

**21.** Guárdalo en un commit:

```
git commit -a -m "Añade pimientos"
```

👀 `[master a64238d] Añade pimientos`.

**22.** Súbelo:

```
git push origin master
```

👀 `eb0ebcd..a64238d  master -> master`.

**23.** Pasa a la copia de tu compañero:

```
cd ../companero
```

**24.** *(companero)* En `companero` > `index.html`, añade debajo de `<li>Huevos</li>` esta otra línea. Guarda y cierra la pestaña.

```
    <li>Ciruelas</li>
```

💡 Tu compañero no sabe nada de los pimientos: no ha traído los cambios de GitHub.

**25.** Guárdalo en un commit:

```
git commit -a -m "Añade ciruelas"
```

👀 `[master 6030b65] Añade ciruelas`.

**26.** Intenta subirlo:

```
git push origin master
```

👀 Falla:

```
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/TU_USUARIO/practica_remotos.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. ...
```

En español, `error: falló el empuje de algunas referencias a…` y `ayuda: Actualizaciones fueron rechazadas porque el remoto contiene trabajo que no existe localmente…`.

💡 GitHub tiene un commit que esta copia no tiene, «Añade pimientos». Si aceptara tu push, ese commit se perdería. Primero tienes que traerlo y juntarlo con el tuyo en tu copia; después ya podrás subir (Pro Git 5.2, «Pequeño equipo privado»).

**27.** Trae los cambios:

```
git fetch origin
```

👀 `eb0ebcd..a64238d  master     -> origin/master`.

**28.** Mira el historial de las dos ramas como un dibujo:

```
git log --oneline --graph --all
```

📝 `--graph` dibuja las ramas, como en la práctica de ramas. `--all` muestra también las ramas en las que no estás, como `origin/master` (Pro Git 3.1).

👀 Verás algo así:

```
* 6030b65 (HEAD -> master) Añade ciruelas
| * a64238d (origin/master, origin/HEAD) Añade pimientos
|/
* eb0ebcd Añade huevos
* e29255e Punto de partida
```

💡 El historial se ha bifurcado: `master` (tu compañero) y `origin/master` (GitHub) salen del mismo commit por dos caminos. `origin/HEAD` solo indica cuál es la rama principal de GitHub: ignóralo.

**29.** Fusiona los cambios de GitHub:

```
git merge origin/master
```

👀 Verás:

```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

(en español: `Auto-fusionando index.html`, `CONFLICTO (contenido): Conflicto de fusión en index.html` y `Fusión automática falló; arregle los conflictos y luego realice un commit con el resultado.`). La línea de Git Bash termina en `(master|MERGING)`.

💡 Los dos commits añaden una línea en el mismo sitio, y Git no sabe cuál va primero.

**30.** Abre `companero` > `index.html`. Git ha escrito las dos versiones, separadas por marcas:

```
<<<<<<< HEAD
    <li>Ciruelas</li>
=======
    <li>Pimientos</li>
>>>>>>> origin/master
```

💡 Arriba está la versión de esta copia (para VS Code, el **cambio actual**); abajo, la que viene de GitHub (el **cambio entrante**).

Aquí queremos las dos. Justo encima de `<<<<<<< HEAD` hay una línea de texto pequeño, en gris: haz clic en **Aceptar ambos cambios** (*Accept Both Changes*). Quedan `Ciruelas` y `Pimientos`, sin marcas. No uses el botón **Resolver en el Editor de combinación** (*Resolve in Merge Editor*). Comprueba que no queda ninguna marca (`<<<<<<<`, `=======`, `>>>>>>>`), guarda con **Ctrl + S** y cierra la pestaña.

**31.** Marca el conflicto como resuelto:

```
git add index.html
```

📝 Como en la práctica de ramas: durante un conflicto, `git add` le dice a Git que el archivo ya está resuelto.

**32.** Termina la fusión con un commit:

```
git commit -m "Fusiona pimientos y ciruelas"
```

👀 `[master 8dd5bf2] Fusiona pimientos y ciruelas`. La línea de Git Bash vuelve a `(master)`.

**33.** Mira el historial:

```
git log --oneline --graph --all
```

👀 Verás algo así:

```
*   8dd5bf2 (HEAD -> master) Fusiona pimientos y ciruelas
|\
| * a64238d (origin/master, origin/HEAD) Añade pimientos
* | 6030b65 Añade ciruelas
|/
* eb0ebcd Añade huevos
* e29255e Punto de partida
```

💡 Los dos caminos se juntan en el commit de fusión: el historial cuenta que dos personas trabajaron a la vez.

**34.** Ahora sí, súbelo:

```
git push origin master
```

👀 `a64238d..8dd5bf2  master -> master`.

**35.** Pasa a tu copia:

```
cd ../yo
```

**36.** *(yo)* Trae los cambios de tu compañero:

```
git fetch origin
```

👀 `a64238d..8dd5bf2  master     -> origin/master`.

**37.** Fusiónalos:

```
git merge origin/master
```

👀 `Updating a64238d..8dd5bf2` y `Fast-forward`. Tu copia, la de tu compañero y GitHub tienen ahora lo mismo.

---

## 4️⃣ Parte 4. Lo mismo, con rebase (8 min)

Volvéis a trabajar a la vez, ahora en sitios distintos del archivo. Esta vez tu compañero no va a fusionar: va a **reorganizar** su commit con `git rebase`.

**38.** *(yo)* En `yo` > `index.html`, en la línea `<h1>Lista de la compra</h1>`, haz clic justo antes de `</h1>` y escribe un espacio y `de la semana`. Debe quedar así; guarda y cierra la pestaña:

```
  <h1>Lista de la compra de la semana</h1>
```

**39.** Guárdalo en un commit:

```
git commit -a -m "Cambia el título"
```

👀 `[master 07d9257] Cambia el título`.

**40.** Súbelo:

```
git push origin master
```

👀 `8dd5bf2..07d9257  master -> master`.

**41.** Pasa a la copia de tu compañero:

```
cd ../companero
```

**42.** *(companero)* En `companero` > `index.html`, añade debajo de `<li>Pimientos</li>`, al final de la lista, esta línea. Guarda y cierra la pestaña:

```
    <li>Naranjas</li>
```

**43.** Guárdalo en un commit:

```
git commit -a -m "Añade naranjas"
```

👀 `[master d81e45a] Añade naranjas`. Fíjate en el código de tu commit: lo compararás en el paso 47.

**44.** Intenta subirlo:

```
git push origin master
```

👀 Falla otra vez con `! [rejected]        master -> master (fetch first)`: GitHub tiene «Cambia el título» y esta copia no.

**45.** Trae los cambios:

```
git fetch origin
```

👀 `8dd5bf2..07d9257  master     -> origin/master`.

**46.** En lugar de fusionar, reorganiza:

```
git rebase origin/master
```

📝 `git rebase` (reorganizar) toma los commits de tu rama que no están en `origin/master`, aquí «Añade naranjas», y los vuelve a aplicar uno a uno encima de `origin/master` (Pro Git 3.6).

👀 `Successfully rebased and updated refs/heads/master.` (en español, `Rebase aplicado satisfactoriamente y actualizado refs/heads/master.`).

🖱️ **···** > **Rama** > **Fusionar la rama mediante "rebase"...** (*Rebase Branch...*), y elige `origin/master`.

**47.** Mira el historial:

```
git log --oneline --graph --all
```

👀 Verás algo así:

```
* 0e0d0ae (HEAD -> master) Añade naranjas
* 07d9257 (origin/master, origin/HEAD) Cambia el título
*   8dd5bf2 Fusiona pimientos y ciruelas
|\
| * a64238d Añade pimientos
* | 6030b65 Añade ciruelas
|/
* eb0ebcd Añade huevos
* e29255e Punto de partida
```

💡 Compáralo con el paso 33: esta vez no hay commit de fusión y el historial sigue en línea recta, como si tu compañero hubiera empezado a trabajar después del cambio de título. Además, «Añade naranjas» tiene otro código (en el ejemplo, `d81e45a` antes y `0e0d0ae` ahora): `rebase` no mueve los commits, crea otros nuevos con los mismos cambios. Por eso **nunca se reorganizan commits que ya se han subido** (Pro Git 3.6, «Los peligros de reorganizar»). Este no lo estaba: el push del paso 44 falló.

**48.** Súbelo:

```
git push origin master
```

👀 `07d9257..0e0d0ae  master -> master`.

### 🔀 ¿Merge o rebase?

Las partes 3 y 4 resuelven el mismo problema: alguien ha subido commits que tú no tienes y, antes de hacer push, tienes que juntarlos con los tuyos. Hay dos maneras de hacerlo:

| | `git merge origin/master` (Parte 3) | `git rebase origin/master` (Parte 4) |
|---|---|---|
| Qué hace | Crea un commit de fusión que une los dos caminos. | Vuelve a aplicar tus commits, uno a uno, encima de los del remoto. |
| Cómo queda el historial | Con una bifurcación que se vuelve a juntar: cuenta lo que pasó de verdad, que dos personas trabajaron a la vez. | En línea recta, como si hubieras empezado a trabajar después. |
| Tus commits | No cambian. | Se crean otros nuevos con los mismos cambios y otro código. |
| Cuándo se puede usar | Siempre. | Solo con commits que todavía no has subido. |
| Si hay conflicto | Se resuelve una vez y se termina con `git add` y `git commit`, como en la Parte 3. | Se resuelve en cada commit que choca y se termina con `git add` y `git rebase --continue` (no lo usamos en esta práctica). |

¿Cuál es mejor? Pro Git no elige: cada equipo decide según cómo quiere que se lea su historial (3.6, «Reorganizar vs. Fusionar»). Su consejo es reorganizar tu trabajo local antes de subirlo, pero nunca reorganizar nada que ya hayas compartido. Si dudas, usa `merge`: nunca cambia lo que ya existe.

---

## 5️⃣ Parte 5. Una rama en el remoto (10 min)

Tu compañero prepara las bebidas en una rama aparte y la sube a GitHub para que tú la revises y la fusiones.

**49.** *(companero)* Crea la rama `bebidas` y salta a ella:

```
git checkout -b bebidas
```

👀 `Switched to a new branch 'bebidas'` (en español, `Cambiado a nueva rama 'bebidas'`).

**50.** En `companero` > `index.html`, añade debajo de `<li>Naranjas</li>` esta línea. Guarda y cierra la pestaña:

```
    <li>Zumo de naranja</li>
```

**51.** Guárdalo en un commit:

```
git commit -a -m "Añade zumo"
```

👀 `[bebidas 4063041] Añade zumo`: el commit está en la rama `bebidas`.

**52.** Sube la rama a GitHub:

```
git push -u origin bebidas
```

📝 `git push origin bebidas` sube la rama `bebidas`. `-u` (de *upstream*) además asocia tu rama `bebidas` con la rama remota `origin/bebidas`, para que Git sepa con cuál comparar (Pro Git 5.2 y 3.5, «Hacer seguimiento a las ramas»).

👀 `* [new branch]      bebidas -> bebidas` y `branch 'bebidas' set up to track 'origin/bebidas'.` (en español, `rama 'bebidas' configurada para rastrear 'origin/bebidas'.`). GitHub añade unas líneas `remote:` con un enlace para crear un *pull request*: no lo abras.

🖱️ El botón **Publicar Branch** (*Publish Branch*) del panel hace lo mismo.

**53.** Pasa a tu copia:

```
cd ../yo
```

**54.** *(yo)* Pregunta a GitHub qué hay de nuevo:

```
git fetch origin
```

👀 Dos líneas:

```
   07d9257..0e0d0ae  master     -> origin/master
 * [new branch]      bebidas    -> origin/bebidas
```

(en español, `[nueva rama]`).

💡 Tu copia se entera de dos cosas: del commit «Añade naranjas» de la Parte 4, que aún no habías traído, y de la rama nueva.

**55.** Salta a la rama `bebidas`:

```
git checkout bebidas
```

📝 Tu copia no tiene ninguna rama `bebidas`, pero sí `origin/bebidas`. Git crea una rama local con ese nombre, asociada a la remota, y salta a ella (Pro Git 3.5, «Hacer seguimiento a las ramas»).

👀 `Switched to a new branch 'bebidas'` y `branch 'bebidas' set up to track 'origin/bebidas'.`. En `yo` > `index.html` ya están las naranjas y el zumo.

🖱️ Clic en el nombre de la rama, abajo a la izquierda en la barra de estado, y elige `origin/bebidas`.

**56.** Mira tus ramas y sus ramas remotas:

```
git branch -vv
```

📝 `-vv` lista tus ramas con su rama remota asociada entre corchetes y dice si van por delante (*ahead*) o por detrás (*behind*) de ella (Pro Git 3.5).

👀 Verás algo así:

```
* bebidas 4063041 [origin/bebidas] Añade zumo
  master  07d9257 [origin/master: behind 1] Cambia el título
```

(en español, `detrás 1`). Tu `master` va un commit por detrás de `origin/master`: le falta «Añade naranjas».

**57.** Vuelve a `master`:

```
git checkout master
```

👀 `Switched to branch 'master'` y `Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.`

**58.** Fusiona la rama de tu compañero:

```
git merge bebidas
```

👀 `Updating 07d9257..4063041`, `Fast-forward` y `1 file changed, 2 insertions(+)`.

💡 Dos líneas nuevas: `bebidas` traía las naranjas y el zumo. Con una sola fusión, tu `master` se pone al día.

**59.** Sube `master`:

```
git push origin master
```

👀 `0e0d0ae..4063041  master -> master`.

**60.** La rama `bebidas` ya está fusionada. Bórrala de GitHub:

```
git push origin --delete bebidas
```

📝 `--delete` borra una rama del remoto (Pro Git 3.5, «Eliminar ramas remotas»).

👀 `- [deleted]         bebidas`.

🖱️ **···** > **Rama** > **Eliminar rama remota...** (*Delete Remote Branch...*).

**61.** Bórrala también de tu copia:

```
git branch -d bebidas
```

👀 `Deleted branch bebidas (was 4063041).` (en español, `Eliminada la rama bebidas (era 4063041).`).

💡 En tu fork ya solo queda `master`. La copia de tu compañero aún tiene su rama `bebidas`: no importa.

---

## 6️⃣ Parte 6. Propón tus cambios al profesor: pull request (8 min)

**62.** *(yo)* Crea la rama `firma` y salta a ella:

```
git checkout -b firma
```

👀 `Switched to a new branch 'firma'`.

**63.** En `yo` > `index.html`, en la línea `<p>Autor: (tu nombre)</p>`, cambia `(tu nombre)` por tu nombre, sin apellidos ni paréntesis: por ejemplo, `<p>Autor: Laura</p>`. Guarda y cierra la pestaña.

**64.** Guárdalo en un commit:

```
git commit -a -m "Firma la práctica"
```

👀 `[firma 1161f15] Firma la práctica`.

**65.** Sube la rama a tu fork:

```
git push -u origin firma
```

👀 `* [new branch]      firma -> firma` y `branch 'firma' set up to track 'origin/firma'.`.

**66.** Abre tu fork en GitHub (`https://github.com/TU_USUARIO/practica_remotos`) y recarga la página. Aparece un aviso amarillo, `firma had recent pushes…`: pulsa **Compare & pull request**. En la página **Open a pull request**:
- Comprueba la línea de arriba: **base repository** `victorrpf/practica_remotos_base`, **base** `master` ← **head repository** `TU_USUARIO/practica_remotos`, **compare** `firma`.
- En el título, escribe `Práctica de remotos de` y tu nombre, sin apellidos: por ejemplo, `Práctica de remotos de Laura`.
- Pulsa **Create pull request**.

👀 La página de tu *pull request*, en el repositorio del profesor, con la etiqueta verde **Open** y la frase `TU_USUARIO wants to merge 8 commits into victorrpf:master from TU_USUARIO:firma`.

💡 Un **pull request** pide al dueño de un repositorio que traiga a su proyecto los commits de una rama tuya. El profesor verá tus commits y tus cambios sin que tengas permiso para subir nada a su repositorio (Pro Git 6.2). Son 8 commits: los 7 de `master` que no están en el repositorio del profesor y el de `firma`.

⚠️ No subas nada más a la rama `firma`: cada push a esa rama cambia el *pull request*.

**67.** Copia la dirección de tu *pull request*, de la barra del navegador: `https://github.com/victorrpf/practica_remotos_base/pull/` y un número.

**68.** Entrega en Aules la dirección de tu fork (`https://github.com/TU_USUARIO/practica_remotos`) y la de tu *pull request*.

---

## 🆘 Si algo sale mal

| Mensaje o síntoma | Qué ha pasado y qué hacer |
|---|---|
| `fatal: not a git repository` (`no es un repositorio git`) | Estás en `practica_remotos`, fuera de las dos copias. Entra en la del paso con `cd yo` o `cd companero`. |
| `fatal: destination path 'yo' already exists` (`la ruta de destino 'yo' ya existe…`) al clonar | Ya habías clonado esa copia. Sigue en el paso siguiente. |
| `git commit -a` dice `nothing to commit, working tree clean` (`nada para hacer commit, el árbol de trabajo está limpio`) | No has guardado, o has editado el `index.html` de la otra copia. Si es lo segundo, en esa otra copia deshaz el cambio (**Ctrl + Z**) y guarda; después haz el cambio en la copia correcta y repite el commit. |
| Has hecho un commit o un push en la copia que no tocaba | Lo más sencillo es empezar de cero: en GitHub, en tu fork, **Settings** > abajo del todo, **Delete this repository**, y sigue las confirmaciones; borra la carpeta `practica_remotos` desde el explorador de archivos y vuelve al paso 1. |
| `! [rejected]` con `(fetch first)` o `(non-fast-forward)` en un paso en el que no tocaba | Esta copia no tiene algo que ya está en GitHub. Haz `git fetch origin` y `git merge origin/master` y repite el push. |
| Después de `git merge origin/master` la terminal se convierte en un editor de texto | No ha habido conflicto (seguramente pusiste la línea en otro sitio) y Git te pide el mensaje del commit de fusión. Pulsa **Esc**, escribe `:wq` y pulsa **Intro**: se guarda el mensaje que propone Git. En la Parte 3, sigue en el paso 33. |
| Has usado `git pull` | Si responde `fatal: Need to specify how to reconcile divergent branches.` (`Necesita especificar cómo reconciliar las ramas divergentes.`), solo ha hecho el `fetch`: sigue con el `git merge origin/master` del guion. Si ha fusionado (en Windows suele venir configurado así), sigue como si hubieras hecho el `git merge`. |
| `error: Your local changes to the following files would be overwritten by merge` (`Los cambios locales a los archivos siguientes serían sobreescritos al fusionar`) | Te has saltado el commit. Haz el `git commit -a -m "…"` del paso que faltaba y repite el `git merge`. |
| `error: cannot rebase: You have unstaged changes.` (`no se puede realizar rebase: Tienes cambios fuera del área de stage.`) | Te has saltado el commit del paso 43. Hazlo y repite el `git rebase`. |
| `error: pathspec 'bebidas' did not match any file(s) known to git` | Te has saltado el `git fetch origin` del paso 54, o no estás en la copia `yo`. |
| `error: unable to delete 'bebidas': remote ref does not exist` | Ya la habías borrado de GitHub. Sigue en el paso 61. |
| Después del commit de fusión todavía hay marcas `<<<<<<<` en `index.html` | Bórralas y deja `Ciruelas` y `Pimientos`. Guarda y haz `git commit -a -m "Corrige el conflicto"` y `git push origin master`. |
| `git push` pide usuario y contraseña en la terminal | Tu ordenador no tiene configurado el inicio de sesión de GitHub, y GitHub no acepta la contraseña de tu cuenta. Escribe al profesor antes de seguir. |
| No cambiaste el nombre del fork en el paso 1 | Tu fork se llama `TU_USUARIO/practica_remotos_base`. Funciona igual: usa y entrega esa dirección. |
| Has creado un *pull request* de la rama `bebidas`, o uno que no va al repositorio del profesor | Ábrelo y pulsa **Close pull request**, al final de la página. Después repite el paso 66. |
| La terminal termina en `:` | Pulsa **q**. |
