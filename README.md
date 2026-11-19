# alienware-boost
Control Alienware Performance Boost from an external keyboard or desktop app.
Independent application tested on an Alienware Aurora 16 with a Alienware Command Center v6.14.105.0 installation. Compatibility is checked against the tested DLL version. Other models or Command Center updates may require changes. Not affiliated with Dell or Alienware. Vibecoded by ChatGPT.

<img width="1277" height="749" alt="image" src="https://github.com/user-attachments/assets/8f5ce4ef-2498-47a3-8f5c-42d8cf70738e" />

<img width="1262" height="740" alt="image" src="https://github.com/user-attachments/assets/db95c1a1-a77d-4128-bd1e-1ce769b67a3d" />


# Alienware Boost 1.4 — source code / código-fonte

## Build on Windows / Compilar no Windows

Run `Build.cmd` (English) or `Compilar.cmd` (Português) from this directory.
They invoke the x64 .NET Framework C# compiler installed with Windows:
`%SystemRoot%\Microsoft.NET\Framework64\v4.0.30319\csc.exe`.

The project targets Windows x64 with .NET Framework 4.8. It uses C# 5 syntax
and framework libraries only; no NuGet packages are needed on the user's PC.
The scripts produce `AlienwareBoost.exe` and `BoostHook.dll` in this directory.
Keep both outputs together. The PNG and ICO resources are embedded in the EXE.

Execute `Build.cmd` ou `Compilar.cmd` nesta pasta. Os arquivos compilados
serão criados aqui. Mantenha o executável e a DLL juntos.

## Files / Arquivos

| File | Purpose |
| --- | --- |
| `BoostApp.cs` | WinForms UI, language selection, shortcut, lifecycle, IPC client |
| `UiText.cs` | Portuguese/English strings and display translation |
| `BoostHook.cs` | Temporary startup hook, local IPC server, Boost handler call |
| `NativeLocator.cs` | Version-guarded lookup for the installed keyboard receiver |
| `AssemblyInfo.cs` | Application name and version |
| `AlienwareBoost.exe.manifest` | Windows compatibility, DPI awareness, normal user launch |
| `AlienwareBoost.ico` | Executable and tray speedometer icon |
| `AlienwareMark.png` | Alien mark embedded in the UI |
| `Build.cmd` / `Compilar.cmd` | English / Portuguese build commands |

## Implementation notes

The display language is independent of the helper's diagnostic wire format.
Changing language must not change IPC commands, protocol prefixes, status
comparisons, method names, field names, native method tokens, or the DLL hash.
Only the display text is translated. Old in-memory log lines retain their
original language; external exception messages can follow the OS language.

`NativeLocator.cs` deliberately requires the exact tested DLL hash. Do not
remove that guard or reuse native offsets against an unverified DLL version.
Command Center updates can require a new compatibility analysis.

The temporary helper is scoped to AWCC.UCSubAgent in the user's session.
Closing/disconnecting attempts to restart the uninstrumented agent. A Windows
restart removes the temporary session if the application is force-closed.

The included Alienware mark was extracted from the user's provided AWCC.exe.
Brand names and marks belong to their respective owners. This application is
independent of Dell; no third-party Command Center binaries are redistributed.

## Validation

The release is compiled for Windows x64. Translation mappings are tested for
both directions, known status responses, error display, and unchanged wire
messages. The new language selector still requires a UI check on Windows.

Run `RunTests.cmd` on Windows to run the included localization checks (363 assertions).
