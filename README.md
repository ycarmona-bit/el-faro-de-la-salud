# El Faro de la Salud

Periódico interno diario de Mercadeo Nueve Once: noticias del sector salud en Venezuela, competencia, aseguradoras, entes públicos y agenda de eventos médicos y de tecnología, clasificados en Relevante, Quizás y No. Se publica con GitHub Pages y se actualiza solo cada mañana.

## Cómo funciona

1. Todos los días a las 6:30 a. m. (hora de Caracas), GitHub Actions ejecuta `scripts/generar-edicion.mjs`.
2. Claude investiga la web, clasifica todo para Nueve Once y arma la edición.
3. El script visita cada reportaje y toma su foto de portada.
4. La edición se guarda en `data/latest.json` y una copia en `data/archivo/AAAA-MM-DD.json`.
5. El mismo proceso publica la página en GitHub Pages.

## Puesta en marcha (una sola vez)

1. **Crear el repositorio:** en GitHub, *New repository*. Nombre sugerido: `el-faro-de-la-salud`.
   - Con cuenta gratuita, GitHub Pages solo funciona si el repositorio es **público** (la página quedará visible para quien tenga el enlace).
   - Con GitHub Pro, Team o Enterprise puede ser **privado**.
2. **Subir los archivos:** en el repositorio, *Add file → Upload files* y arrastra todo el contenido de la carpeta `faro` (index.html, README.md, package.json, .gitignore y las carpetas `data` y `scripts`). Pulsa *Commit changes*.
3. **Crear la automatización:** *Add file → Create new file*. En el nombre escribe exactamente `.github/workflows/edicion-diaria.yml`, pega el contenido de `workflow-para-copiar.yml` y pulsa *Commit changes*. (Se hace así porque la carpeta `.github` es oculta y el navegador no siempre la sube al arrastrar.)
4. **Clave de Claude:** *Settings → Secrets and variables → Actions → New repository secret*. Nombre: `ANTHROPIC_API_KEY`. Valor: tu API key de Claude (console.anthropic.com).
5. **Permisos:** *Settings → Actions → General → Workflow permissions* → **Read and write permissions** → *Save*.
6. **Activar Pages:** *Settings → Pages → Build and deployment → Source* → **GitHub Actions**.
7. **Primera edición:** *Actions → Edición diaria de El Faro de la Salud → Run workflow*. Al terminar, el enlace de la página aparece en el paso *publicar* y en *Settings → Pages* (algo como `https://tu-usuario.github.io/el-faro-de-la-salud/`).

## Uso diario

- No hay que hacer nada: cada mañana sale una edición nueva en el mismo enlace.
- Para correrla a mano: *Actions → Run workflow*.
- Si cambias el diseño (`index.html`), la página se vuelve a publicar sola.

## Ajustes

- **Hora:** línea `cron` del workflow (está en UTC; Caracas es UTC-4).
- **Qué se vigila y cómo se clasifica:** texto `SYSTEM` en `scripts/generar-edicion.mjs`.
- **Modelo:** variable `FARO_MODEL` (por defecto `claude-sonnet-5-5`).

## Si algo falla

Si la investigación falla, no se publica nada y la edición anterior sigue en pie. Revisa el error en *Actions* (clic en la ejecución en rojo).

## Costos

GitHub Actions y Pages son gratuitos para este uso. Cada edición consume tokens de la API de Claude y hasta 30 búsquedas web.
