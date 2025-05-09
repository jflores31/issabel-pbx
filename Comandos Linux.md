# 🐧 Comparativa de Comandos Antiguos vs Comandos Modernos en Linux

Una lista extensa y actualizada de comandos tradicionales de Linux y sus reemplazos modernos más potentes, rápidos y visuales.

---

## 📁 Gestión de Archivos y Directorios

| Comando Clásico | Comando Moderno      | Ventajas                                         |
|-----------------|----------------------|--------------------------------------------------|
| `ls`            | `exa`, `lsd`         | Colores, íconos, vista de árbol, info de Git     |
| `cd`            | `zoxide`, `autojump` | Navegación basada en uso frecuente               |
| `find`          | `fd`                 | Sintaxis sencilla, más veloz, respeta .gitignore |
| `du`            | `dust`, `ncdu`       | Visualización gráfica del uso del disco          |
| `df`            | `dysk`               | Salida más clara y estética                      |
| `tree`          | `broot`              | Árbol interactivo de directorios                 |
| `cp`, `mv`, `rm`| `rip`, `vivid`       | Alternativas más seguras y visuales              |

---

## 📄 Lectura y Visualización

| Comando Clásico | Comando Moderno  | Ventajas                                 |
|-----------------|------------------|------------------------------------------|
| `cat`           | `bat`            | Colores, numeración, integración Git     |
| `less`          | `most`, `moar`   | Mejor scroll, más amigable               |
| `head` / `tail` | `bat`            | Soporte parcial de archivos con estilo   |

---

## 📊 Procesos y Sistema

| Comando Clásico | Comando Moderno   | Ventajas                                        |
|-----------------|-------------------|-------------------------------------------------|
| `top`           | `htop`, `btop`    | Gráfico, interactivo, visual                    |
| `ps`            | `procs`           | Información legible, estructurada               |
| `time`          | `hyperfine`       | Benchmarking preciso y comparativo              |
| `uptime`        | `bpytop`          | Vista de sistema detallada con estilo moderno   |

---

## 🌐 Redes y Conectividad

| Comando Clásico | Comando Moderno | Ventajas                                           |
|-----------------|------------------|----------------------------------------------------|
| `ping`          | `gping`          | Muestra gráfica del ping                          |
| `ifconfig`      | `ip`, `nmcli`    | Reemplazos oficiales con más funciones            |
| `netstat`       | `ss`, `nethogs`  | Más detallado, más veloz                          |
| `traceroute`    | `mtr`            | Análisis en tiempo real de la ruta                |
| `curl`, `wget`  | `httpie`, `xh`   | Solicitudes HTTP legibles y modernas              |
| `scp`           | `rsync`, `rclone`| Transferencias eficientes y seguras               |

---

## 🧠 Búsqueda y Procesamiento de Texto

| Comando Clásico | Comando Moderno | Ventajas                                      |
|-----------------|------------------|-----------------------------------------------|
| `grep`          | `ripgrep (rg)`   | Más rápido, busca recursiva y colores         |
| `awk`           | `xsv`            | Procesamiento de archivos CSV más potente     |
| `sed`           | `sd`             | Reemplazos de texto más simples y directos    |
| `diff`          | `delta`          | Comparaciones con colores y mejor formato     |

---

## 🧰 DevOps y Desarrollo

| Clásico     | Moderno       | Ventajas                                      |
|-------------|---------------|-----------------------------------------------|
| `man`       | `tldr`, `cheat`| Ejemplos prácticos, manuales resumidos       |
| `git log`   | `gitui`, `lazygit` | Interfaces interactivas para Git          |
| `history`   | `atuin`, `zsh-autosuggestions` | Historial mejorado con búsqueda inteligente |

---

## 🧪 Herramientas Especiales

| Herramienta | Descripción                                   |
|-------------|-----------------------------------------------|
| `glow`      | Visualiza Markdown con estilo en terminal     |
| `dog`       | Cliente DNS moderno (mejor que `dig`)         |
| `choose`    | Selección de columnas simplificada            |
| `navi`      | Snippets interactivos de comandos             |
| `dua`, `gdu`| Análisis de uso de disco visual y rápido      |

---

## 🧬 Shells Modernas

| Shell       | Ventaja                                       |
|-------------|-----------------------------------------------|
| `bash`      | Tradicional, robusta                          |
| `zsh`       | Plugins, temas, autocompletado                |
| `fish`      | Configuración simple, sugerencias inteligentes|
| `nushell`   | Shell estructurada con salidas en tabla       |

---

# 🛠️ Guía de Instalación de Comandos Modernos de Linux

Una colección de herramientas modernas para mejorar la experiencia en terminal. Aquí te mostramos cómo instalarlas en Debian/Ubuntu, Arch/Manjaro, Fedora y mediante Homebrew (recomendado si usas WSL o macOS también).

## 📦 Debian / Ubuntu
```bash
sudo apt update
sudo apt install exa bat fd-find ripgrep zoxide dust procs btop tldr hyperfine lsd ncdu mtr nethogs httpie
```
## 🧱 Arch / Manjaro

```bash
sudo pacman -S exa bat fd ripgrep zoxide dust procs btop tldr hyperfine lsd ncdu mtr nethogs httpie
```
## 🐂 Fedora

```bash
sudo dnf install exa bat fd-find ripgrep zoxide dust procs btop tldr hyperfine lsd ncdu mtr nethogs httpie
```

## 🍺 Homebrew (Linux & macOS)

```bash
brew install exa bat fd ripgrep zoxide dust procs btop tldr hyperfine lsd ncdu mtr nethogs httpie
```

## 📚 Complementos Opcionales

```bash
brew install cheat navi
brew install fish starship
brew install dua-cli gdu
brew install glow
brew install dog
```