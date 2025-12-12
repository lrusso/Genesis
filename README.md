# Genesis

A Sega Genesis emulator designed for running in vanilla JavaScript pre-ECMAScript 2015 (no WebAssembly). Simply open the link below, click the red icon, and select a ROM file in `BIN` or `MD` format from your computer; it will be loaded and booted automatically.

## Links:

- [Genesis emulator](https://lrusso.github.io/Genesis/Genesis.htm)
- [Demo booting a sample game](https://lrusso.github.io/Genesis/Genesis.htm?demo)

## Screenshots:

![alt screenshot1](https://lrusso.github.io/Genesis/SCREENSHOT1.jpg)

![alt screenshot2](https://lrusso.github.io/Genesis/SCREENSHOT2.jpg)

![alt screenshot3](https://lrusso.github.io/Genesis/SCREENSHOT3.jpg)

![alt screenshot4](https://lrusso.github.io/Genesis/SCREENSHOT4.jpg)

## How to use it:

Examples of loading local and online files can be found [here](https://github.com/lrusso/Genesis/blob/main/Genesis.htm#L138-L186) and [here](https://github.com/lrusso/Genesis/blob/main/Genesis.htm#L214-L256).

```js
embedGenesis({
  container: "game",
  name: "Sonic The Hedgehog",
  rom: romFile,
  showMobileControls: true,
  saveButtonText: "SAVE",
  loadButtonText: "LOAD",
  soundButtonText: "SOUND",
  player1: {
    up: "ArrowUp",
    down: "ArrowDown",
    left: "ArrowLeft",
    right: "ArrowRight",
    start: "Enter",
    a: "KeyA",
    b: "KeyS",
    c: "KeyD",
    x: "KeyQ",
    y: "KeyW",
    z: "KeyE",
  },
  player2: {
    up: "KeyI",
    down: "KeyK",
    left: "KeyJ",
    right: "KeyL",
    start: "KeyH",
    a: "KeyB",
    b: "KeyN",
    c: "KeyM",
    x: "KeyU",
    y: "KeyO",
    z: "KeyP",
  },
  cbStarted: function cbStarted() {
    console.log("Emulator started.")
  },
})
```

| Parameter          |    Type     | Required | Default value | Description               |
| :----------------- | :---------: | :------: | :-----------: | :------------------------ |
| container          |   string    |   yes    |       –       | Target element ID.        |
| name               |   string    |   yes    |       –       | Game name.                |
| rom                | ArrayBuffer |   yes    |       –       | ROM file.                 |
| showMobileControls |   boolean   |    no    |     false     | Shows mobile controls.    |
| saveButtonText     |   string    |    no    |     SAVE      | Save Button Text.         |
| loadButtonText     |   string    |    no    |     LOAD      | Load Button Text.         |
| soundButtonText    |   string    |    no    |     SOUND     | Sound Button Text.        |
| player1            |   object    |    no    |       –       | Player 1 keys.            |
| player2            |   object    |    no    |       –       | Player 2 keys.            |
| cbStarted          |  function   |    no    |       -       | Called on emulator start. |

## Special keys:

| Action          | macOS Shortcut | Windows Shortcut | Safari Shortcut |
| :-------------- | :------------: | :--------------: | :-------------: |
| Save state      |  Command + 1   |     Ctrl + 1     |    Ctrl + 1     |
| Load state      |  Command + 2   |     Ctrl + 2     |    Ctrl + 2     |
| Toggle sound    |  Command + 3   |     Ctrl + 3     |    Ctrl + 3     |
| Fullscreen mode |  Command + F   |     Ctrl + F     |    Ctrl + F     |

## Main differences with the original project:

- Added logic to load states.
- Added logic to save states.
- Added logic to toggle sound.
- Added logic for setting keys for player 1.
- Added logic for setting keys for player 2.
- Fixed rendering issues when using tiles.
- Fixed issue where PAL games were not loaded.
- Fixed issue where PAL games were playing the sound too fast.
- Fixed issue where PAL games were not filling the screen width.
- Fixed issue where the player 2 keys was not yet implemented in wasm.
- Fixed issue where the sound event was called from the JavaScript side.
- Pulled the latest changes from the original Genesis Plus GX repository.
- Implemented a diferrent setting for porting the emulator file to a asm.js file.
- Implemented logic for pausing the emulator on blur and resuming on focus.
- Implemented a process that converts the JavaScript build to pre-ECMAScript 2015.

## Build the emulator

- Install Node.js: https://nodejs.org/en/download
- Install Emscripten: https://emscripten.org/docs/getting_started/downloads.html
- Run the following commands:

```
mkdir build && cd build
emcmake cmake ../src
emmake make
```

## Based on the work of:

https://github.com/h1romas4/wasm-genplus

https://github.com/ekeeke/Genesis-Plus-GX
