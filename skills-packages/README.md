# EPA Digital Skills - Installation Guide

Archivos `.skill` para instalar en Claude Code y Cowork.

## 📦 Skills Disponibles

```
✅ go-api-scaffold.skill              (1.4 KB)
✅ go-client-scaffold.skill           (1.4 KB)
✅ nextjs-scaffold.skill              (1.3 KB)
✅ validate-pr-format.skill           (872 B)
✅ generate-openapi.skill             (904 B)
✅ git-flow-guide.skill               (1.2 KB)
```

**Total:** 24 KB | 6 skills

---

## 🚀 Instalación en Cowork

### Opción 1: Instalar desde Archivo

1. Abre Cowork
2. Ve a **Settings** → **Skills**
3. Click en **"+ Add Skill"** o **"Install from File"**
4. Selecciona uno de los archivos `.skill`
5. Click **Install**

Repite para los 6 skills.

### Opción 2: Instalar desde Carpeta

Si tu instalación de Cowork busca skills en carpetas locales:

```bash
# Copiar a carpeta de skills de Cowork
cp *.skill ~/.cowork/skills/
```

---

## 🛠️ Instalación en Claude Code

### Opción 1: Instalar desde Archivo

1. Abre Claude Code
2. Ve a **Settings** → **Skills** (o **Plugins**)
3. Click en **"+ New Skill"** o **"Upload Skill"**
4. Selecciona uno de los archivos `.skill`
5. Click **Install**

Repite para los 6 skills.

### Opción 2: Copiar a Directorio de Usuario

```bash
# Crear directorio de skills
mkdir -p ~/.claude/skills

# Copiar archivos .skill
cp *.skill ~/.claude/skills/

# Claude Code buscará automáticamente aquí
```

### Opción 3: Copiar a Directorio del Proyecto

```bash
# En tu proyecto de Claude Code
mkdir -p .claude/skills
cp *.skill .claude/skills/

# Los skills estarán disponibles solo en este proyecto
```

---

## ✅ Verificación

Después de instalar, verifica que los skills estén disponibles:

### En Cowork
- Ve a **Ayuda** → **Skills Disponibles**
- Deberías ver los 6 skills listados

### En Claude Code
- Escribe uno de los trigger keywords:
  - `"Create a new Go API"` → `go-api-scaffold`
  - `"Create a new NextJS app"` → `nextjs-scaffold`
  - `"Validate my PR"` → `validate-pr-format`
  - `"What branch should I create?"` → `git-flow-guide`
  - `"Update OpenAPI docs"` → `generate-openapi`
  - `"Create a Go client"` → `go-client-scaffold`

---

## 📋 Contenido de Cada .skill

Cada archivo es un ZIP que contiene:

```
{skill-name}/
└── SKILL.md
```

Por ejemplo, `go-api-scaffold.skill` contiene:
```
go-api-scaffold/
└── SKILL.md (con toda la documentación y prompt)
```

---

## 🔄 Actualizar Skills

Si actualizas los SKILL.md en el repo:

1. **En epa-standards/skills/{skill}/SKILL.md** - Edita el archivo
2. **Regenera los .skill:**
   ```bash
   # Vuelve a crear los archivos .skill
   # (Ver sección de desarrollo abajo)
   ```
3. **Reinstala en Cowork/Claude Code:**
   - Desinstala la versión anterior
   - Instala la nueva versión

---

## 🛠️ Para Desarrolladores

### Regenerar los Archivos .skill

Si necesitas regenerar los archivos después de cambiar SKILL.md:

```bash
#!/bin/bash

SKILLS=(
  "go-api-scaffold"
  "go-client-scaffold"
  "nextjs-scaffold"
  "validate-pr-format"
  "generate-openapi"
  "git-flow-guide"
)

for skill in "${SKILLS[@]}"; do
  # Crear directorio temporal
  mkdir -p /tmp/$skill/$skill
  
  # Copiar SKILL.md
  cp ../skills/$skill/SKILL.md /tmp/$skill/$skill/
  
  # Crear ZIP con nombre .skill
  cd /tmp/$skill
  zip -r $skill.skill $skill/
  
  # Mover a skills-packages
  mv $skill.skill /path/to/skills-packages/
  
  # Limpiar
  cd -
  rm -rf /tmp/$skill
done
```

---

## 📞 Soporte

- ¿Los skills no aparecen? 
  - Reinicia Cowork/Claude Code
  - Verifica que el archivo .skill sea válido (es un ZIP)
  - Comprueba los logs en Settings → Debug

- ¿Problemas al instalar?
  - Asegúrate de tener permisos de lectura en los archivos
  - Intenta instalar uno a la vez
  - Contacta a Eddye si persisten los problemas

---

## 📚 Documentación de Cada Skill

Para ver la documentación completa de cada skill:

### Go API Scaffold
- **Ubicación:** `../skills/go-api-scaffold/SKILL.md`
- **Propósito:** Crear nuevas APIs Go con hexagonal architecture
- **Triggers:** "Create a new Go API", "hexagonal scaffold"

### Go Client Scaffold
- **Ubicación:** `../skills/go-client-scaffold/SKILL.md`
- **Propósito:** Crear clientes Go reutilizables
- **Triggers:** "Create a Go client", "new client library"

### NextJS Scaffold
- **Ubicación:** `../skills/nextjs-scaffold/SKILL.md`
- **Propósito:** Crear nuevas apps NextJS con estándares EPA
- **Triggers:** "Create a new NextJS app", "scaffold Next app"

### Validate PR Format
- **Ubicación:** `../skills/validate-pr-format/SKILL.md`
- **Propósito:** Validar PRs antes de submitear
- **Triggers:** "Validate my PR", "check PR format"

### Generate OpenAPI
- **Ubicación:** `../skills/generate-openapi/SKILL.md`
- **Propósito:** Generar/actualizar OpenAPI specs
- **Triggers:** "Update OpenAPI", "generate Postman"

### Git Flow Guide
- **Ubicación:** `../skills/git-flow-guide/SKILL.md`
- **Propósito:** Guía interactiva de branching
- **Triggers:** "What branch should I create?", "git workflow"

---

**¡Listo para instalar!** 🚀
