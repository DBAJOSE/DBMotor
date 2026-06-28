# DBMotor - GitHub Pages

Sitio estatico publicado con GitHub Pages para presentar publicaciones y laboratorios interactivos sobre motores de bases de datos, con foco inicial en SQL Server.

URL publica esperada:

```text
https://dbajose.github.io/DBMotor/
```

## Contexto Rapido Para IA

Este repositorio no usa framework, build step ni backend. Todo funciona con archivos estaticos servidos por GitHub Pages:

- HTML5 para estructura.
- CSS embebido o `styles.css` para estilos.
- JavaScript vanilla para interacciones.
- Los laboratorios se cargan desde `laboratorios.html` dentro de un `iframe`.

La rama publicada es `main` y GitHub Pages debe apuntar a `main` / `root`.

## Estructura Principal

| Archivo | Proposito |
|---|---|
| `index.html` | Portada del sitio DBMotor. Contiene hero, cards principales y enlaces a laboratorios. |
| `styles.css` | Estilos globales de la portada y elementos comunes. |
| `script.js` | Interacciones basicas de la portada. |
| `laboratorios.html` | Panel principal de laboratorios. Tiene menu lateral izquierdo y visor central/derecho con `iframe`. |
| `01 Recuros.html` | Laboratorio Recursos del Sistema: carga HammerDB, log writer, Buffer Pool, WRITELOG y delayed durability. |
| `02 Recuros.html` | Laboratorio de I/O SQL Server con drag and drop para discos y archivos `.mdf` / `.ldf`. |
| `02 Recuros - cuellos.html` | Simulador de cuellos de botella: CPU, log, datos, memoria y bloqueos. |
| `Simulador_Performance_Avanzado.html` | Simulador avanzado de hardware vs logica: indices, estadisticas, red/app y disco. |
| `Simulador_SQL_Enterprise.html` | Simulador Enterprise de concurrencia: blocking, deadlock, TempDB spill y parameter sniffing. |
| `juego_memory.html` | Laboratorio de Juegos: Juego de memoria (memofichas) con conceptos de bases de datos. |
| `DBMOTOR.txt` | Nota/artifacto auxiliar existente. Revisar antes de borrar. |

## Funcionamiento De Laboratorios

`laboratorios.html` contiene el menu lateral. Cada opcion llama a `loadLab(file, element)` y reemplaza el contenido del visor con:

```html
<iframe src="archivo-del-laboratorio.html"></iframe>
```

Cada laboratorio esta disenado como una mini aplicacion independiente con su propio HTML, CSS y JS embebidos. Esto permite abrirlos directamente, pero la experiencia principal es desde:

```text
https://dbajose.github.io/DBMotor/laboratorios.html
```

## Laboratorios Actuales

### 1. Recursos Del Sistema

Archivo: `01 Recuros.html`

Simula una carga tipo TPC-C/HammerDB y muestra el flujo:

```text
HammerDB -> Buffer Pool -> Log Writer -> Data File
```

Controles importantes:

- Usuarios virtuales.
- Latencia del log `.ldf`.
- Fragmentacion VLF.
- Delayed Durability.
- Reinicio de metricas.

Notas tecnicas:

- Usa `requestAnimationFrame` para el loop visual.
- Calcula TPM con base en tiempo transcurrido desde el inicio de prueba.
- Limpia particulas al reiniciar metricas.

### 2. I/O SQL Server Drag And Drop

Archivo: `02 Recuros.html`

Permite arrastrar discos HDD/SSD/NVMe a ranuras y luego ubicar archivos `.mdf` y `.ldf` sobre discos instalados.

Mejoras actuales:

- Header con metricas WRITELOG, PAGEIOLATCH y TPM.
- Boton Pausa/Play.
- Boton Reiniciar.
- Reasignacion de `.mdf` / `.ldf` sin recargar.
- Layout responsive.

### 3. Cuellos De Botella

Archivo: `02 Recuros - cuellos.html`

Simula waits y escenarios DBA frecuentes:

- CPU al 100% / `SOS_SCHEDULER_YIELD`.
- Latencia en log / `WRITELOG`.
- Latencia en datos / `PAGEIOLATCH`.
- Presion de memoria.
- Bloqueos / `LCK_M_X`.

### 4. Performance Avanzado

Archivo: `Simulador_Performance_Avanzado.html`

Explica la diferencia entre problemas de hardware y problemas logicos de diseno:

- Disco lento.
- Fragmentacion de indices.
- Falta de indice.
- Estadisticas viejas.
- Aplicacion/red lenta.

Notas tecnicas:

- Al cambiar escenario se eliminan particulas anteriores para evitar acumulacion visual.
- El panel informativo tiene scroll controlado.

### 5. SQL Enterprise

Archivo: `Simulador_SQL_Enterprise.html`

Simula escenarios de concurrencia e internals:

- Blocking.
- Deadlock con victima.
- TempDB spill.
- Parameter sniffing.
- `KILL Session 52` para el escenario de blocking.
- Reset general.

Notas tecnicas:

- Maneja temporizadores por escenario para evitar que eventos viejos se disparen despues de un reset.
- El monitor inferior muestra SPID, estado, wait type y comando.

### 6. Laboratorio de Juegos (DBA Memory)

Archivo: `juego_memory.html`

Juego interactivo de memofichas diseñado para reforzar conceptos técnicos de bases de datos de forma lúdica.

Mecánica del juego:
- Tablero de 16 cartas (8 parejas).
- Animación CSS 3D (flip cards).
- Sistema de puntuación: cantidad de intentos y tiempo transcurrido.
- Conceptos incluidos: ACID, Index, Deadlock, Sharding, Replica, Backup, Query, Transaction.

Notas técnicas:
- Lógica de barajado aleatorio (Fisher-Yates) al iniciar o reiniciar.
- Bloqueo de interacción durante la comparación de cartas para evitar clics múltiples.
- Totalmente responsive para jugar en móviles.

## Convenciones Importantes

- Mantener los nombres actuales de los laboratorios, aunque tengan espacios o el texto `Recuros`, porque ya estan referenciados desde `laboratorios.html` y publicados en GitHub Pages.
- Si se renombran archivos, actualizar obligatoriamente las llamadas `loadLab(...)` en `laboratorios.html`.
- Evitar dependencias que requieran build. GitHub Pages esta sirviendo contenido estatico directo.
- Si se agregan librerias externas, preferir CDN confiables y validar que funcionen bajo HTTPS.
- No convertir los laboratorios en rutas dinamicas ni dependientes de servidor.
- Mantener cada laboratorio autocontenido, salvo que se haga una refactorizacion planeada para CSS/JS compartido.

## Flujo De Publicacion

Desde la raiz del repo local:

```powershell
git status --short
git add .
git commit -m "Descripcion breve del cambio"
git push origin main
```

Despues del push, GitHub Pages suele tardar entre 1 y 3 minutos en reflejar cambios. Para validar en navegador usar recarga fuerte:

```text
Ctrl + F5
```

## Recomendaciones Para Futuras Mejoras

- Normalizar nombres de archivos en una fase controlada, por ejemplo `lab-recursos-sistema.html`, actualizando enlaces y probando GitHub Pages.
- Extraer estilos comunes de laboratorios a un CSS compartido si empiezan a crecer mas.
- Agregar una tarjeta descriptiva previa por laboratorio en `laboratorios.html`.
- Agregar capturas o miniaturas por laboratorio para que el menu sea mas visual.
- Crear seccion de publicaciones con Markdown o HTML estatico si el sitio evoluciona hacia blog tecnico.

## Ultima Nota Operativa

El proyecto esta pensado para ser simple, portable y mantenible: abrir archivos HTML, editar, probar, hacer commit y publicar. Si una IA entra al repositorio, debe comenzar por `laboratorios.html` para entender la navegacion y luego revisar cada laboratorio autocontenido segun el cambio solicitado.
