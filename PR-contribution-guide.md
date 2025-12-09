# Guía de Contribución Open Source

Guía personal para contribuir a proyectos open source.

---

## Checklist Pre-Contribución

Ejecutar antes de empezar a trabajar en un nuevo dispositivo o proyecto.

### 1. Verificar Git Config

```bash
# Verificar identidad
git config --global user.name
git config --global user.email

# Debe mostrar tu email de GitHub (ej: 17481958+ronaldmego@users.noreply.github.com)
```

### 2. Verificar SSH Signing

```bash
# Verificar que está configurado
git config --global gpg.format          # Debe decir: ssh
git config --global user.signingkey     # Debe apuntar a tu llave
git config --global commit.gpgsign      # Debe decir: true

# Si falta alguno, configurar:
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_rsa.pub
git config --global commit.gpgsign true
```

### 3. Verificar Llave SSH en GitHub

1. Ir a https://github.com/settings/keys
2. Verificar que existe una **Signing Key** (no solo Authentication)
3. Si no existe:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   Copiar y agregar en GitHub como "Signing Key"

### 4. Test Rápido de Firma

```bash
# En cualquier repo local
echo "test" > test.txt && git add test.txt
git commit -S -m "test signature"
git cat-file commit HEAD | grep "BEGIN SSH SIGNATURE"
# Si aparece la firma, está OK
git reset --hard HEAD~1  # Limpiar el test
rm test.txt
```

---

## Proceso de Contribución

### Paso 1: Fork y Clone

```bash
# En GitHub: click en "Fork" en el repo original

# Clonar tu fork
git clone https://github.com/TU_USUARIO/REPO.git
cd REPO

# Agregar upstream (repo original)
git remote add upstream https://github.com/OWNER_ORIGINAL/REPO.git
```

### Paso 2: Crear Branch

```bash
# Asegurar que main está actualizado
git checkout main
git pull upstream main

# Crear branch para tu fix/feature
git checkout -b fix/nombre-descriptivo
```

### Paso 3: Hacer los Cambios

```bash
# Editar archivos...
# Probar que funciona...

# Ver cambios
git status
git diff
```

### Paso 4: Commit Firmado

```bash
# Agregar cambios
git add archivo1 archivo2

# Commit con firma (la -S es automática si configuraste gpgsign=true)
git commit -m "fix: descripción breve del cambio

- Detalle 1 de qué se hizo
- Detalle 2 de qué se hizo"
```

### Paso 5: Push y PR

```bash
# Push a tu fork
git push origin fix/nombre-descriptivo

# En GitHub: aparecerá botón "Compare & pull request"
# Click y llenar:
# - Título descriptivo
# - Descripción de qué y por qué
```

### Paso 6: Responder Review

- Esperar review de maintainers
- Si piden cambios:
  ```bash
  # Hacer cambios...
  git add .
  git commit -m "fix: address review feedback"
  git push origin fix/nombre-descriptivo
  ```

### Paso 7: Si Necesitas Squash

Si el repo requiere un solo commit o quieres limpiar historia:

```bash
# Squash todos los commits de tu branch en uno
git reset --soft origin/main
git commit -S -m "fix: mensaje final unificado"
git push --force origin fix/nombre-descriptivo
```

---

## Situaciones Comunes

### Bloqueo por Firma

**Síntoma:** "Commits must have verified signatures"

**Solución:**
```bash
git reset --soft origin/main
git commit -S -m "mensaje"
git push --force origin tu-branch
```

### CLA (Contributor License Agreement)

- Se firma una vez por proyecto
- Comentar en el PR: "I have read the CLA Document and I hereby sign the CLA"
- Queda asociado a tu cuenta de GitHub

### Conflictos con Main

```bash
git checkout main
git pull upstream main
git checkout tu-branch
git rebase main
# Resolver conflictos si hay...
git push --force origin tu-branch
```

### PR Rechazado

- No lo tomes personal
- Lee el feedback
- Pregunta si no entiendes
- Itera o crea nuevo PR

---

## Verificar tu Contribución

Después del merge:

1. **Commit en main:** `https://github.com/OWNER/REPO/commits/main`
2. **Tu perfil:** Aparecerá en tu historial de contribuciones
3. **Contributors:** Puede tardar 24h en actualizar `https://github.com/OWNER/REPO/graphs/contributors`

---

## Tips

1. **Lee CONTRIBUTING.md** del proyecto antes de empezar
2. **Issues primero:** Comenta en un issue existente o crea uno antes de un PR grande
3. **PRs pequeños:** Más fáciles de revisar y aprobar
4. **Tests:** Si el proyecto tiene tests, asegúrate de que pasen
5. **Linting:** Ejecuta el linter del proyecto antes de hacer PR
6. **Paciencia:** Los maintainers son voluntarios, puede tardar en revisar

---

## Comandos de Referencia Rápida

```bash
# Ver remotes
git remote -v

# Actualizar desde upstream
git fetch upstream
git checkout main
git merge upstream/main

# Ver commits pendientes vs main
git log --oneline origin/main..HEAD

# Ver si un commit está firmado
git cat-file commit HEAD | grep -A2 "gpgsig"

# Cambiar mensaje del último commit
git commit --amend -m "nuevo mensaje"

# Ver estado del PR (si tienes gh cli)
gh pr status
```
