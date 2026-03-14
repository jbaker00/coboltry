# Copilot instructions for this repository

Purpose
- Minimal GnuCOBOL example and VS Code integration for compiling, running, and basic debugging of COBOL programs on macOS.

## Build, test, and lint commands
- Build a single COBOL file (compile & link into an executable):
  - cobc -x -free -o <output> <source.cob>
  - Example: cobc -x -free -o hello hello.cob
- From VS Code (preconfigured task):
  - Run Task → "cobol: build current file" (uses: cobc -x -free -o ${fileDirname}/${fileBasenameNoExtension} ${file})
  - Run Task → "cobol: run last build" (executes ${fileDirname}/${fileBasenameNoExtension})
- Tests: No test framework or test files are present. To exercise a single program, compile and run the single-file command above.
- Linting: No linter is configured in this repo. The installed extensions (see below) provide language server / syntax checks; add a dedicated linter or pre-commit hook if desired.

## High-level architecture
- Single-file sample COBOL program(s) live at the repository root (currently `hello.cob`).
- Development workflow: edit .cob source → compile with GnuCOBOL (`cobc`) → run native executable produced beside the source file.
- VS Code integration lives in `.vscode/` and includes:
  - `tasks.json` — per-file build and run tasks
  - `launch.json` — two debug configurations (one using the C/C++ "cppdbg" adapter with LLDB, and one for the CodeLLDB adapter). Both rebuild the current file before launching.

## Key conventions and repository-specific patterns
- Free-format sources: tasks and examples compile using `-free`. Keep the source in free-format or ensure appropriate compile flags when using fixed format.
- File naming: source file extension `.cob` used here; compiled binary name equals `${fileBasenameNoExtension}` and is placed next to the source by the existing tasks.
- Per-file workflow: current VS Code tasks are file-scoped (they rely on `${file}`, `${fileDirname}`, and `${fileBasenameNoExtension}`). For multi-file projects, provide a build script or Makefile and update tasks/launch to use the project build output.
- Debugging: two launch configs are provided:
  - "Debug COBOL (C/C++ extension - cppdbg)" — uses cpptools/cppdbg with `MIMode: lldb` and `/usr/bin/lldb` on macOS.
  - "Debug COBOL (CodeLLDB extension)" — uses the CodeLLDB adapter. Both expect the compiled executable at `${fileDirname}/${fileBasenameNoExtension}` and run the preLaunchTask `cobol: build current file`.

## VS Code extensions (recommended / installed in this environment)
- bitlang.cobol — COBOL language support (syntax, snippets, navigation)
- ms-vscode.cpptools — C/C++ tools (used here for the cppdbg debug config)
- vadimcn.vscode-lldb — LLDB support (optional)
- broadcomMFD.cobol-language-support — alternative COBOL language support LSP (optional)
- RocketSoftware.rocket-cobol — vendor IDE support (optional)
- OCamlPro.SuperBOL — SuperBOL/GnuCOBOL assistance (optional)

Note: these extensions improve editor features and debugging; they are not required to compile with `cobc`.

## Where Copilot should look / quick pointers
- Primary sources: repository root (`*.cob`) and `.vscode/` for tasks/launch behavior.
- Build commands are executed with `cobc` — keep any generated/machine artifacts next to the source or add an explicit `build/` folder and update tasks.

---

Created: .github/copilot-instructions.md — summarizes build/run/debug commands, the VS Code integration, and repository conventions. If you'd like, I can:
- Add example Makefile or scripts for multi-file builds
- Extend the instructions with recommended linter and test scaffolding

Would you like any adjustments or additions to this file?