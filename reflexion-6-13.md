# Reflexión sobre git worktree

## 1. Ventajas de los worktrees

**Frente a cambiar de rama en el mismo directorio:**
La principal ventaja es que no necesitas interrumpir tu trabajo actual. Si tienes cambios sin guardar, no es necesario hacer `git stash` o crear commits temporales/sucios para poder saltar a otra rama. Cada worktree es un directorio independiente, por lo que puedes tener el código de varias ramas abierto en tu editor al mismo tiempo sin conflictos.

**Frente a clonar el repositorio varias veces:**
Clonar un repositorio varias veces ocupa mucho más espacio en el disco duro, ya que se duplica toda la carpeta `.git` con todo el historial. Además, los clones son repositorios independientes: si haces un `git fetch` o cambias algo, tendrías que sincronizarlos a través del servidor remoto (haciendo push y pull). Con `git worktree`, comparten la misma base de datos `.git` local, por lo que cualquier rama nueva o commit creado en un worktree está disponible inmediatamente en los demás, ahorrando tiempo y espacio.

## 2. Dos situaciones reales de uso

1. **Revisión urgente (Hotfix o PR):** Estás a mitad de desarrollar una funcionalidad compleja (feature branch) y tu código ni siquiera compila. De repente, surge un bug crítico en producción o te piden revisar un Pull Request urgentemente. Puedes crear un worktree con la rama `main` o la del PR, arreglar el bug o probar el código de tu compañero, hacer commit, borrar el worktree y volver a tu editor original donde tu código inacabado sigue exactamente igual.
2. **Ejecutar o comparar dos versiones a la vez:** Necesitas probar cómo se veía una funcionalidad en la versión anterior (por ejemplo, para comparar si un bug ya existía). Con worktrees, puedes tener el servidor de desarrollo ejecutándose desde el worktree A (versión nueva) en el puerto 3000, y otro servidor desde el worktree B (versión antigua) en el puerto 3001, permitiéndote interactuar y comparar ambas versiones lado a lado en tu navegador.

## 3. Buenas prácticas para nombrar y organizar

- **Ubicación:** Crear los worktrees siempre fuera o paralelos al directorio del repositorio principal (por ejemplo usando `../wt-nombre`) para evitar que Git los rastree accidentalmente o que ensucien tu entorno de trabajo principal.
- **Nomenclatura:** Usar prefijos estándar como `wt-` (worktree) seguido del nombre de la rama o ticket. Ejemplos: `wt-hotfix-login`, `wt-pr-123`, `wt-feature-dashboard`. Así es fácil identificarlos en el explorador de archivos.
- **Limpieza periódica:** Acostumbrarse a eliminar el worktree usando `git worktree remove` inmediatamente después de terminar la tarea. Si se deja acumulando, se pueden llegar a bloquear ramas (no puedes hacer checkout de una rama si ya está asignada a un worktree abierto). Además, ejecutar `git worktree prune` de forma periódica por si alguna carpeta se borró manualmente por error.
