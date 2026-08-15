# odin-template

Starter repository for [Odin](https://odin-lang.org) with [OLS](https://github.com/DanielGavin/ols) and [odinfmt](https://github.com/DanielGavin/ols#odinfmt-configurations) already configured.

Toolchain is managed with [mise](https://mise.jdx.dev).

## Layout

```
mise.toml         # Odin + OLS/odinfmt via mise
ols.json          # OLS language server (checker profile, format, hover, etc.)
odinfmt.json      # odinfmt style
src/main.odin     # hello-world package
.vscode/          # VS Code: OLS extension + format on save
.github/          # GitHub Actions: check, build, format
```

OLS discovers Odin's `core` and `vendor` collections from your `odin` install. Add extra collections in `ols.json` only when you have project-local shared packages.

## Requirements

- [mise](https://mise.jdx.dev)
- In VS Code: the [OLS extension](https://marketplace.visualstudio.com/items?itemName=DanielGavin.ols) (`DanielGavin.ols`)

Install the compiler, language server, and formatter:

```bash
mise trust
mise install
```

That provides `odin`, `ols`, and `odinfmt`. GitHub OLS releases ship platform-suffixed binaries (`ols-arm64-darwin`, and so on); `mise.toml` renames them to `ols` and `odinfmt`.

If OLS is already installed globally with mise, add the same `rename_exe` mapping there so `ols` is on your PATH:
```toml
[tools]
odin = "latest"
"github:DanielGavin/ols" = { version = "latest", rename_exe = { "ols-*" = "ols", "odinfmt-*" = "odinfmt" } }
```

Then `mise install --force github:DanielGavin/ols`.

## Run

```bash
odin run src
```

Build a binary into `bin/`:

```bash
mkdir -p bin
odin build src -out:bin/odin-template
```

Check the package the same way OLS does:

```bash
odin check src
```

Format from the CLI (OLS also formats on save in the editor):

```bash
odinfmt src -w
```

## CI

Pushes and pull requests run `.github/workflows/ci.yml`: it installs the mise toolchain, then `odin check`, `odin build`, and an `odinfmt` drift check. Do not pass `-vet-tabs` or `-strict-style` unless you switch this template back to tabs.
