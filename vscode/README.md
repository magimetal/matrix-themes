# Matrix Green Theme

Matrix Green color theme for VS Code.

## Build `.vsix`

From repo root:

```bash
pushd vscode
npm run package
popd
```

This creates `vscode/matrix-green-theme-0.1.0.vsix`.

## Install `.vsix`

From repo root:

```bash
code --install-extension vscode/matrix-green-theme-0.1.0.vsix
```

Then run `Preferences: Color Theme` and select `Matrix Green`.
