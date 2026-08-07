# Propuesta de Estructura Wiki para Stockly

Para llevar la formalización del repositorio al siguiente nivel, se propone migrar toda la documentación estática en formato Markdown hacia la funcionalidad nativa de **GitHub Wiki**. Esto permitirá una mejor navegación, indexación y escalabilidad de la base de conocimiento del proyecto.

## 🗂️ Arquitectura de la Información (Sitemap Propuesto)

La Wiki se estructurará mediante una barra lateral (Sidebar) personalizada (`_Sidebar.md`) con las siguientes categorías principales:

### 🏠 Home
*   **Inicio:** (Página principal `Home.md` equivalente al actual `README.md` resumido). Presentación del proyecto Stockly, objetivo, tecnologías base y links rápidos.

### 📐 Arquitectura y Diseño
*   **Arquitectura de la Aplicación:** Diagramas de componentes, modelo de base de datos y tecnologías core (Java, Tomcat).
*   **Infraestructura (Kubernetes):** Definición del clúster, explicación de los manifiestos (pods, services, ingress) y recursos desplegados.

### ⚙️ Gestión de Configuración (SCM)
*   *(Contenido migrado de `SCM.md` y dividido en subpáginas)*
*   **Elementos de Configuración (CIs):** Identificación de lo que se versiona.
*   **Baselines y Versionamiento:** Explicación de los hitos y uso de Semantic Versioning.
*   **Control de Cambios y Trazabilidad:** Reglas para ramas, Pull Requests y auditoría.

### 🚀 Integración y Entrega Continua (CI/CD)
*   *(Contenido migrado de `CI_CD_PIPELINE.md`)*
*   **GitHub Actions (CI):** Explicación del workflow, pruebas unitarias y análisis de código estático (SonarCloud, Snyk).
*   **Jenkins (CD):** Orquestación del despliegue en Kubernetes y actualización de imágenes Docker.

### 🛠️ Guías Operativas
*   **Guía para Desarrolladores:** Cómo configurar el entorno local, ejecutar `mvn clean package` y lanzar pruebas localmente.
*   **Guía de Despliegue Manual:** Procedimiento en caso de fallo del CD (rollback, comandos `kubectl` y `docker`).

---

## 🔄 Proceso de Migración Sugerido
1.  **Habilitar Wiki:** En los ajustes del repositorio de GitHub (`Settings > Features`), activar la casilla de *Wikis*.
2.  **Poblar Contenido:** Mover el contenido de `SCM.md` y `CI_CD_PIPELINE.md` creando las páginas correspondientes en la interfaz de la Wiki para facilitar la lectura.
3.  **Configurar Sidebar:** Crear un archivo `_Sidebar.md` para establecer la navegación jerárquica mostrada arriba.
4.  **Actualizar README.md del Repositorio:** Simplificar el actual `README.md` de la raíz del código fuente para que actúe únicamente como una landing page breve, que envíe al desarrollador a la pestaña de *Wiki* para encontrar cualquier detalle técnico, arquitectónico o de SCM.
