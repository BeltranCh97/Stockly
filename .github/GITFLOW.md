# 🌳 Gitflow - Estructura de Ramas para Stockly

## 📌 Visión General

Stockly implementa **Gitflow** como modelo de versionamiento. Este modelo permite mantener un flujo ordenado de desarrollo con ramas especializadas para features, releases y hotfixes.

---

## 🎯 Ramas Principales

### 1. **main** (Rama de Producción)
- **Propósito**: Contiene código en producción
- **Permanente**: Sí
- **Se crea desde**: `release/` o `hotfix/`
- **Se mergea a**: Nunca (solo recibe merges)
- **Tags**: Todas las versiones de producción (v1.0.0, v1.0.1, etc.)

### 2. **develop** (Rama de Desarrollo)
- **Propósito**: Rama de integración para features y bugfixes
- **Permanente**: Sí
- **Se crea desde**: `main`
- **Se mergea a**: `release/` y `hotfix/`
- **Tags**: No tiene tags

---

## 🚀 Ramas de Soporte (Temporales)

### 3. **feature/** (Nuevas Funcionalidades)
- **Patrón**: `feature/nombre-descriptivo`
- **Ejemplos**:
  - `feature/autenticacion-usuarios`
  - `feature/dashboard-inventario`
  - `feature/reportes-pdf`
- **Se crea desde**: `develop`
- **Se mergea a**: `develop` (vía Pull Request)
- **Duración**: Días/Semanas
- **Convención**: usar kebab-case (palabras separadas por guiones)

### 4. **bugfix/** (Correcciones de Bugs)
- **Patrón**: `bugfix/descripcion-bug`
- **Ejemplos**:
  - `bugfix/validacion-cantidad-negativa`
  - `bugfix/error-reportes-excel`
  - `bugfix/timeout-conexion-bd`
- **Se crea desde**: `develop`
- **Se mergea a**: `develop` (vía Pull Request)
- **Duración**: Horas/Días
- **Convención**: usar kebab-case

### 5. **release/** (Preparación de Versiones)
- **Patrón**: `release/X.Y.Z` (seguir Semantic Versioning)
- **Ejemplos**:
  - `release/1.0.0`
  - `release/1.1.0`
  - `release/2.0.0`
- **Se crea desde**: `develop`
- **Se mergea a**: `main` y `develop`
- **Duración**: Días
- **Propósito**: 
  - Ajustes finales
  - Corrección de bugs de última hora
  - Actualización de versión y CHANGELOG
  - No se aceptan features nuevas

### 6. **hotfix/** (Parches de Producción)
- **Patrón**: `hotfix/X.Y.Z` o `hotfix/descripcion-rapido`
- **Ejemplos**:
  - `hotfix/1.0.1-seguridad-critica`
  - `hotfix/correccion-perdida-datos`
  - `hotfix/fallo-autenticacion`
- **Se crea desde**: `main`
- **Se mergea a**: `main` y `develop`
- **Duración**: Horas
- **Propósito**: Correcciones críticas en producción

---

## 🔐 Reglas de Protección de Ramas

### Regla 1: Protección de `main`
```
Nombre: Main Production Protection
Patrón: main
```

**Configuración:**
- ✅ Require a pull request before merging
  - Number of approvals: **2**
  - Dismiss stale pull request approvals: ✅
  - Require resolution of conversations: ✅
  - Require review from Code Owners: ✅
- ✅ Require status checks to pass before merging
  - Require branches to be up to date: ✅
- ✅ Restrict who can push to matching branches
  - Allow: Maintainers/Release managers
- ✅ Block force pushes
- ✅ Block deletions

---

### Regla 2: Protección de `develop`
```
Nombre: Develop Integration Protection
Patrón: develop
```

**Configuración:**
- ✅ Require a pull request before merging
  - Number of approvals: **1**
  - Dismiss stale pull request approvals: ✅
  - Require resolution of conversations: ✅
- ✅ Require status checks to pass before merging
  - Require branches to be up to date: ✅
- ✅ Restrict who can push to matching branches
  - Allow: All developers
- ✅ Block force pushes
- ✅ Block deletions

---

### Regla 3: Protección de ramas `release/*`
```
Nombre: Release Branch Protection
Patrón: release/*
```

**Configuración:**
- ✅ Require a pull request before merging
  - Number of approvals: **1**
  - Require resolution of conversations: ✅
- ✅ Require status checks to pass before merging
- ✅ Restrict who can push
  - Allow: Release managers
- ✅ Block deletions

---

### Regla 4: Protección de ramas `feature/*`
```
Nombre: Feature Branch Protection (Opcional)
Patrón: feature/*
```

**Configuración:**
- ✅ Dismiss stale pull request approvals
- ✅ Require resolution of conversations
- ❌ No requiere protección estricta (son ramas personales de desarrollo)

---

## 📊 Diagrama del Flujo

```
                        Tag v1.0.0
                             ↓
    ◄──────────────────── main ◄─────────────────┐
    │                         ▲                   │
    │                         │ (2 aprobs)        │
    │                    release/1.0.0            │
    │                     ▲     │                 │
    │                     │     └──────┐          │
    │                     │            ▼          │
    ├──────────────── develop ◄─────── develop ──┤
    │                  ▲    ▲                     │
    │          (1 aprob)│    │                    │
    │               │   │    └──────┬─────────┐   │
    │               │   │           │         │   │
    │           feature/  bugfix/   │    release/ │
    │           feature/  bugfix/   │         │   │
    │           feature/  bugfix/   │    release/ │
    │                              │         │   │
    └──────────────── hotfix/ ───────────────┘   │
                   ▲                              │
                   └──────────────────────────────┘
              (merge de hotfix/X.Y.Z)
```

---

## 🔄 Flujo de Trabajo Detallado

### ✨ Workflow 1: Desarrollar una Nueva Feature

```bash
# 1. Asegurar que develop está actualizado
git checkout develop
git pull origin develop

# 2. Crear rama feature desde develop
git checkout -b feature/nombre-descriptivo

# 3. Hacer commits regularmente
git commit -m "feat: descripción del cambio"

# 4. Hacer push a la rama
git push -u origin feature/nombre-descriptivo

# 5. Crear Pull Request en GitHub hacia develop
# - Título: Descripción clara de la feature
# - Descripción: Explicar qué hace, cómo probarlo
# - Reviewers: Asignar al menos 1 revisor

# 6. Una vez aprobado, mergear en GitHub

# 7. Eliminar rama local y remota
git branch -d feature/nombre-descriptivo
git push origin --delete feature/nombre-descriptivo
```

---

### 🐛 Workflow 2: Corregir un Bug

```bash
# 1. Crear rama bugfix desde develop
git checkout develop
git pull origin develop
git checkout -b bugfix/descripcion-bug

# 2. Hacer correcciones y commit
git commit -m "fix: descripción de la corrección"

# 3. Hacer push
git push -u origin bugfix/descripcion-bug

# 4. Crear Pull Request hacia develop
# - Explicar el bug y la solución
# - Incluir pasos para reproducir y verificar

# 5. Una vez aprobado, mergear
```

---

### 📦 Workflow 3: Preparar una Release

```bash
# 1. Crear rama release desde develop
git checkout develop
git pull origin develop
git checkout -b release/1.0.0

# 2. Actualizar versión en archivos
# - package.json (versión)
# - CHANGELOG.md (nuevas features)
# - Otros archivos de configuración

git commit -m "chore: bump version to 1.0.0"
git push -u origin release/1.0.0

# 3. Crear Pull Request en GitHub
# - Hacia: main
# - Título: "Release 1.0.0"
# - Descripción: Resumen de cambios

# 4. Hacer pruebas finales en esta rama
# - Solo bugfixes críticos permitidos
git commit -m "fix: corrección crítica en release"

# 5. Una vez aprobado, mergear a main
# - En GitHub: Merge Pull Request
# - Crear Tag: v1.0.0

# 6. Mergear de vuelta a develop
git checkout develop
git pull origin develop
git merge --no-ff release/1.0.0
git push origin develop

# 7. Eliminar rama release
git branch -d release/1.0.0
git push origin --delete release/1.0.0
```

---

### 🚨 Workflow 4: Hotfix para Producción

```bash
# 1. Crear rama hotfix desde main
git checkout main
git pull origin main
git checkout -b hotfix/1.0.1-descripcion

# 2. Hacer la corrección
git commit -m "fix: corrección crítica de seguridad"

# 3. Hacer push
git push -u origin hotfix/1.0.1-descripcion

# 4. Crear Pull Request hacia main
# - Título: "Hotfix: descripción de la corrección"
# - Explicar el impacto en producción

# 5. Una vez aprobado, mergear a main
# - Crear Tag: v1.0.1

# 6. Mergear de vuelta a develop
git checkout develop
git pull origin develop
git merge --no-ff hotfix/1.0.1-descripcion
git push origin develop

# 7. Eliminar rama hotfix
git branch -d hotfix/1.0.1-descripcion
git push origin --delete hotfix/1.0.1-descripcion
```

---

## 📝 Convenciones de Commits

Para mayor claridad, usa el formato de Conventional Commits:

```
<tipo>(<scope>): <sujeto>

<cuerpo (opcional)>

<footer (opcional)>
```

**Tipos:**
- `feat`: Nueva funcionalidad
- `fix`: Corrección de bug
- `docs`: Cambios en documentación
- `style`: Cambios de formato (sin lógica)
- `refactor`: Refactorización de código
- `perf`: Mejoras de performance
- `test`: Agregar o modificar tests
- `chore`: Tareas de mantenimiento

**Ejemplos:**
```
feat(auth): agregar autenticación con JWT
fix(inventory): corregir cálculo de stock negativo
docs: actualizar README con instrucciones de setup
refactor(api): simplificar lógica de validación
```

---

## ✅ Checklist para Implementar Gitflow

- [ ] Crear ramas base (`main`, `develop`)
- [ ] Configurar regla de protección para `main`
- [ ] Configurar regla de protección para `develop`
- [ ] Configurar regla de protección para `release/*`
- [ ] Agregar CODEOWNERS (opcional)
- [ ] Crear primera rama `develop` si no existe
- [ ] Comunicar a todo el equipo sobre el nuevo flujo
- [ ] Crear documentación en el equipo
- [ ] Configurar rama por defecto en `develop` (Settings → Default branch)

---

## 🎓 Recursos Útiles

- [Gitflow Cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)
- [Semantic Versioning](https://semver.org/es/)
- [Conventional Commits](https://www.conventionalcommits.org/es)
- [GitHub Flow vs Gitflow](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow)
