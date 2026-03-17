# 42 VS Code Installer Script

![Shell Script](https://img.shields.io/badge/Shell_Script-121011?style=for-the-badge&logo=gnu-bash&logoColor=white)
![Zsh](https://img.shields.io/badge/Zsh-4EAA25?style=for-the-badge&logo=Zshell&logoColor=white)
![VS Code](https://img.shields.io/badge/VS_Code-007ACC?style=for-the-badge&logo=visual-studio-code&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![42 School](https://img.shields.io/badge/42_School-000000?style=for-the-badge&logo=42&logoColor=white)

> Instalador automatizado de Visual Studio Code para el entorno de 42 Network. Descarga, configura e integra VS Code en el sistema sin requerir permisos de administrador.

---

## Características Principales

- Instalación automatizada de VS Code desde el servidor oficial (última versión estable)
- Integración completa con el entorno de escritorio Linux (GNOME/KDE)
- Acceso directo en el escritorio y menú de aplicaciones
- Comando `code` disponible en terminal
- Desinstalación limpia y completa
- Funciona sin permisos root en espacios de usuario (`/sgoinfre`)

## Stack Tecnológico

| Categoría | Tecnología |
|-----------|------------|
| Shell Scripting | Zsh |
| Descarga de binarios | GNU Wget |
| Descompresión | tar |
| Integración Desktop | XDG Desktop Entry Standard |

## Decisiones Técnicas / Arquitectura

Este proyecto resuelve un problema específico del entorno 42 Network: las máquinas de los clusters se reinician periódicamente, eliminando cualquier software instalado localmente. Además, los estudiantes no tienen permisos de administrador.

La arquitectura aprovecha el directorio `/sgoinfre/students/$USER`, un espacio de almacenamiento persistente entre reinicios, para instalar VS Code de forma portable. El uso de `nohup` con redirección a `/dev/null` garantiza que el editor se ejecuta como proceso independiente del terminal, evitando bloqueos de sesión. La creación dinámica del archivo `.desktop` cumple con los estándares XDG para máxima compatibilidad entre entornos de escritorio Linux.

```mermaid
flowchart TB
    subgraph Instalación["Instalación (install.sh)"]
        A[Usuario ejecuta install.sh] --> B[Descarga VS Code<br/>wget]
        B --> C[Descomprimir tar.gz]
        C --> D[Guardar en /sgoinfre/code]
        D --> E[Crear vscode.desktop]
        E --> F[Copiar a Desktop y Applications]
        F --> G[Añadir función code a .zshrc]
    end

    subgraph Componentes["Componentes Creados"]
        G --> H[Binarios del editor<br/>/sgoinfre/code/]
        F --> I[Acceso directo GUI<br/>~/Desktop/]
        F --> J[Menú aplicaciones<br/>~/.local/share/applications/]
        G --> K[CLI wrapper function<br/>~/.zshrc]
    end

    subgraph Desinstalación["Desinstalación (uninstall.sh)"]
        L[ejecuta uninstall.sh] --> M[Eliminar /sgoinfre/code]
        M --> N[Eliminar .desktop files]
        N --> O[Limpiar función en .zshrc]
    end
```

## Guía de Instalación

```bash
# Clonar el repositorio
git clone https://github.com/samuelhm/42_VscodeInstallerScript.git
cd 42_VscodeInstallerScript

# Dar permisos de ejecución
chmod +x install.sh uninstall.sh

# Instalar VS Code
./install.sh
```

### Verificar Instalación

```bash
# Verificar que está instalado
code --version

# Abrir VS Code desde terminal
code .
```

### Desinstalar

```bash
./uninstall.sh
```

---

## Contacto

[![GitHub](https://img.shields.io/badge/GitHub-samuelhm-181717?style=flat-square&logo=github)](https://github.com/samuelhm/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-shurtado--m-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/shurtado-m/)