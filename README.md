# Theme Toolkit

Theme Toolkit v1.3.2 is a Windows 11 desktop-theme management application. The WPF app targets `.NET 8.0-windows` and manages wallpapers, cursors, sounds, icons, native Windows theme installation, and optional Cairo Shell themes.

![Preview](https://gitlab.com/theme-toolkit/theme-toolkit/-/raw/main/docs/assets/preview.png)

## Install or Run

Build the application or installer from the release tag. Binary installer archives are not published with releases.

- **Portable app:** Publish `ThemeToolkit.exe` as a self-contained `.NET 8.0-windows` executable and run it from any writable location. On first start it creates its normal user-data folders under `%LOCALAPPDATA%\Theme Toolkit`.
- **Installer:** Publish `ThemeToolkitInstaller.exe` for the target architecture. It requires the .NET 10 Desktop Runtime and installs without elevation to `%LOCALAPPDATA%\Programs\Theme Toolkit`.

The installer:

- Copies `ThemeToolkit.exe` and its bundled `Tools` directory to `%LOCALAPPDATA%\Programs\Theme Toolkit`.
- Creates `%LOCALAPPDATA%\Theme Toolkit` with configuration, cache, log, output, repository, and resource folders.
- Creates the per-user resource root at `%LOCALAPPDATA%\Theme Toolkit\Resources`, including `Cursors`, `Sounds`, `Wallpapers`, `Icons`, their `Imported` download folders, and `_hacks` for user-reviewed optional registry files.
- Generates missing templates in `%LOCALAPPDATA%\Theme Toolkit\Resources\_templates` without overwriting existing files.
- Creates a Start Menu shortcut and can optionally create a Desktop shortcut.

Git and GitLab CLI (`glab`) are optional integrations for GitLab workflows. rclone is not required. When building the installer from a clone, install Git LFS and fetch the repository's LFS-managed registry archive first:

```powershell
git clone https://gitlab.com/theme-toolkit/theme-toolkit.git
cd theme-toolkit
git lfs install
git lfs pull
dotnet publish app\ThemeToolkit\ThemeToolkit.csproj -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true -o _build\v1.3.2\app\win-x64
dotnet publish installer\ThemeToolkit_Installer\ThemeToolkitInstaller.csproj -c Release -r win-x64 --self-contained false -p:PublishSingleFile=true -o _build\v1.3.2\installer\win-x64
```

Use `win-arm64` or `win-x86` in place of `win-x64` when building for those architectures. Publish the portable app first: the installer embeds the matching portable app payload and the LFS-managed `Shutdown-Logoff-Logon-Sound-Hacks.zip` archive.

## Quick Start

1. Start Theme Toolkit.
2. In **Theme Manager**, select local resources and customize the Windows appearance options.
3. Use the resource tabs to manage wallpapers, cursors, sounds, and icons.
4. Load a saved configuration to generate and apply its Windows theme, or select **Install Theme** to generate the theme without applying it.
5. Use **Theme Gallery** to inspect installed Windows themes, their mapped resources, and their metadata; apply, export, or open the selected theme.
6. Use **Theme Store** to browse trusted source catalogs and import a direct HTTPS wallpaper, cursor, WAV sound, icon, or desktop theme pack. Each download is validated and has a source record stored beside it. The **Theme Gallery** tab browses the `windows-themes` GitLab collections and downloads their `.deskthemepack` files through the same validated pipeline.

Use **Export Theme** in Theme Manager or Theme Gallery to create a validated `.deskthemepack`.

## Optional Sound-Event Files

The installer provisions the supplied optional sound-event `.reg` files under `%LOCALAPPDATA%\Theme Toolkit\Resources\_hacks\`. They are never imported or executed by Theme Toolkit. Review them and use the Windows manual import flow only if you choose to expose the hidden Windows Logon, Windows Logoff, and System Exit labels in Control Panel.

## Documentation

| Topic | Guides |
| :--- | :--- |
| **Themes and resources** | [Native Theme Format](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/theme/Configuration.md), [CLI Reference](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/theme/Scripts.md) |
| **Sounds** | [Sound Guide](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/sounds/Sound-Guide.md), [Sound Management](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/sounds/Sound-Management.md) |
| **Assets** | [Verification](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/assets/Asset-Verification.md), [Theme Catalog](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/assets/All-Themes.md), [Repository Structure](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/assets/Repository-Structure.md) |
| **Contributing** | [Contributing Guide](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/contributing/Contributing.md), [FAQ](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/contributing/FAQ.md) |
| **Project history** | [Changelog](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/CHANGELOG.md), [Roadmap](https://gitlab.com/theme-toolkit/theme-toolkit/-/blob/main/docs/ROADMAP.md) |

## Build

```powershell
dotnet build app\ThemeToolkit\ThemeToolkit.csproj -c Release
```

The app targets .NET 8. The separate installer project targets .NET 10.

## License

BSD 3-Clause — see [LICENSE.md](LICENSE.md).
