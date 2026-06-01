<div align="center">
  <img src="assets/logo.png" alt="NewUX OS" width="180"/>

# NewUX OS

**Una distro Linux moderna basada en LFS, con experiencia desktop pulida.**

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![LFS](https://img.shields.io/badge/base-LFS-orange)](https://www.linuxfromscratch.org/)
[![Kernel](https://img.shields.io/badge/kernel-7.1--rc5%20%2F%207.0.10%20%2F%206.18.33-green)]()
[![Desktop](https://img.shields.io/badge/desktop-XFCE-cyan)]()

</div>

---

## ¿Qué es NewUX OS?

NewUX OS es una distribución GNU/Linux construida **desde cero** sobre [Linux From Scratch](https://www.linuxfromscratch.org/),
con foco en una experiencia de escritorio moderna, distintiva y simple. Trae:

- **3 kernels** a elegir desde GRUB: Mainline RC (7.1-rc5), Stable (7.0.10), LTS (6.18.33), más un modo Recovery.
- **Ricing pro out-of-the-box**: tema Greybird-Dark + Papirus-Dark + acento teal `#16c3a5`, fuentes Noto Sans + JetBrains Mono, layout con top-bar + dock inferior estilo elementary.
- **`pnu`** (Packaging for NewUX Users), un gestor de paquetes universal con backends Flathub + AppImage + source-compile, y comandos al estilo `apt`/`dnf`.
- **Instalador propio** (`install-newux-gui.py`) basado en GTK3, con flow de particionado automático/alongside/cifrado.
- **Repositorio de paquetes propio** publicado en GitHub Pages.

## Componentes del proyecto

Cada parte vive en su propio repositorio dentro de la organización:

| Repo | Descripción |
|------|-------------|
| [**NewUX-OS**](https://github.com/tomymartingrinberg-cpu/NewUX-OS) | Meta-repo: documentación general, branding, roadmap (este repo). |
| [**newux-os-pnu**](https://github.com/tomymartingrinberg-cpu/newux-os-pnu) | `pnu` — gestor de paquetes universal estilo apt/dnf, ~1.2k LOC bash. |
| [**newux-os-installer**](https://github.com/tomymartingrinberg-cpu/newux-os-installer) | Instalador GTK3 + wrapper de elevación + .desktop entry. |
| [**newux-os-ricing**](https://github.com/tomymartingrinberg-cpu/newux-os-ricing) | Configs XFCE, GTK CSS, wallpaper, theme de Calamares, script de aplicación. |
| [**pnu-packages**](https://github.com/tomymartingrinberg-cpu/pnu-packages) | Manifests YAML del repo oficial NewUX (sirvido en GitHub Pages). |

## Especificaciones técnicas

| Característica | Detalle |
|---|---|
| Base | LFS (12.x) |
| Kernels disponibles | 7.1-rc5 (Mainline RC), 7.0.10 (Stable), 6.18.33 (LTS) |
| Bootloader | GRUB 2.12 con entradas: Mainline / Stable / LTS / Recovery |
| Init | systemd 256 |
| Window manager | xfwm4 (XFCE 4.18+) con compositing |
| Display manager | LightDM con greeter GTK |
| Display server | Xorg + virtio-gpu KMS (Wayland WIP) |
| Theme | Greybird-Dark + Papirus-Dark |
| Acento | `#16c3a5` (teal) |
| Fuentes | Noto Sans 10 (UI) / JetBrains Mono 10 (mono) |
| Paquetería | pnu (Flathub + AppImage + source) |
| Audio | PipeWire + WirePlumber |
| Toolchain | GCC 14.2, glibc 2.40, binutils 2.43 |
| Versión actual | 1.0 |

## Quick start

### Bootear una VM con NewUX OS (QEMU)

```bash
# Live OS (descargá la imagen pre-construida o construila con jhalfs)
qemu-system-x86_64 \
  -enable-kvm -cpu host -m 4096 -smp 2 \
  -drive if=pflash,format=raw,readonly=on,file=/usr/share/OVMF/OVMF_CODE_4M.fd \
  -drive if=pflash,format=raw,file=OVMF_VARS_4M.fd \
  -drive file=newux-os-1.0.qcow2,if=virtio,format=qcow2 \
  -vga virtio -display gtk \
  -device qemu-xhci -device usb-tablet -device usb-kbd
```

### Instalar `pnu` sobre un sistema existente

```bash
sudo curl -sL https://raw.githubusercontent.com/tomymartingrinberg-cpu/newux-os-pnu/main/pnu \
  -o /usr/local/bin/pnu && sudo chmod +x /usr/local/bin/pnu
sudo pnu update
pnu search firefox
```

## Diseño visual

El proyecto sigue una paleta consistente en todos los componentes:

| Token | Color | Uso |
|---|---|---|
| `--bg` | `#0a1018` → `#0e2f3a` | Fondo navy con gradiente |
| `--surface` | `#1a1f2e` | Paneles, ventanas |
| `--surface-deep` | `#10131c` | Sidebar, dock |
| `--accent` | `#16c3a5` | Botones, links, current step |
| `--accent-hi` | `#1ee0bd` | Hover |
| `--text` | `#e8ecef` | Texto principal |
| `--text-dim` | `#97a3b0` | Texto secundario |

## Estado del proyecto

NewUX OS es un proyecto experimental construido como ejercicio sobre LFS. **No usar en producción.**
Lo que sí funciona:
- ✅ Boot UEFI con 3 kernels propios
- ✅ XFCE con ricing pulido + instalador funcional sobre disco vacío
- ✅ pnu instalando Flatpaks de Flathub y AppImages
- ✅ Compilación in-tree de paquetes adicionales (CUPS, GnuPG, etc.)
- ⚠️ Calamares 3.4.2 incluido pero con bug del plugin sfdisk-only — usar instalador Python en su lugar
- ⚠️ Wayland: kernel soporta `DRM_VIRTIO_GPU` pero el desktop default es Xorg

## Licencia

Este repositorio y todos los sub-repositorios están bajo **GNU GPL v3**.
Ver [LICENSE](LICENSE) para el texto completo.

## Autor

[tomymartingrinberg-cpu](https://github.com/tomymartingrinberg-cpu) — `tomy.martin.grinberg@gmail.com`
