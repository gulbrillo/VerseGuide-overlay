# VerseGuide.com Star Citizen Game Overlay 

Available as zip archive or Windows installer, see  **[installation instructions](https://github.com/gulbrillo/VerseGuide-overlay/wiki/installation)**.

## Interactive transparent browser windows inside the game window

The VerseGuide Overlay includes multiple tools developed by the VerseGuide team including Citizens Pitapa, justMurphy, xabdiben, Graupunkt, StoicMako and LordSkippy.
It watches Windows processes and tries to inject overlay elements into Star Citizen via DirectX upon launch. It registers hotkeys and simulates keyboard input to execute the `/showlocation` command in Global Chat that allows us to retrieve location information via the Windows clipboard.

## Made by the Community

This is an unofficial Star Citizen project, not affiliated with the Cloud Imperium group of companies. All content within this project not authored by its developer or users are property of their respective owners.

# Build from Source

#### Requirements

- Node 14.X.X
- Electron 11.X.X
- Visual Studio 2019 (C++ desktop workspace, winsdk 10.0.x).
- Python 2 (`add to PATH`)

#### Electron with node native-addons `electron-overlay` and `node-ovhook`

```bash
    cd client
    npm link ../electron-overlay
```
if this fails, try manually deleting `/electron-overlay/node_modules/.bin`
```bash
    npm link ../node-ovhook
    npm i
```
manually copy `/electron-overlay` and `/node-ovhook` to `/client/node_modules`
```bash
    npm run compile:electron
    npm run build
```

NPM commands:
- `npm run dev` (run app in developent mode)
- `npm run make` (build windows zip/exe with electron-forge, output in `client/out/`)
- `npm run publish` (build and upload all make targets to GitHub as draft release)

If iohook throws a `not a valid win32 application` error, the pre-build iohook binaries are missing or broken. Try running `npm i iohook@0.9.0`. iohook@0.9.1 is broken.

`make` requires `client/forge.config.js` not included in the source files:

```javascript
module.exports = {
  packagerConfig: {
    icon: './dist/assets/icon.ico',
    appBundleId: 'com.verseguide.overlay',
    ignore: [
      '^/src',
      '/darwin-x64',
      '/linux-arm64',
      '/linux-x64',
    ],
  },
  makers: [
    {
      name: '@electron-forge/maker-squirrel',
      config: {
        iconUrl: 'https://verseguide.com/favicon.ico',
        setupIcon: './dist/assets/icon.ico',
        loadingGif: './dist/assets/install.gif',
      },
    },
    {
      name: '@electron-forge/maker-zip',
    },
  ],
};
```

#### Recompile game-overlay dll

They are precompiled under `client/dist/overlay` but if you are making changes you might want to compile on your own

```bash
    cd game-overlay

    build.bat
```

copy files [`n_overlay.dll`, `n_overlay.x64.dll`, `n_ovhelper.exe`, `n_ovhelper.x64.exe`] from directory `game-overlay/bin/Release` to directory `client/dist/overlay`
