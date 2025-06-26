# Transparent Twitch Chat Overlay Electron
Twitch chat on top of windowed games for single monitor streamers 

Inspired by
[Transparent Twitch Chat Overlay](https://github.com/baffler/Transparent-Twitch-Chat-Overlay/)

This simple Electron script launches [jChat](https://www.giambaj.it/twitch/jchat/) in a frameless, transparent window with user-defined customization like opacity, font, scale, and CSS-based styling (background color, shadows, etc).

It simply show the public jChat web interface — no code modifications, just styled and displayed.
![Preview](https://raw.githubusercontent.com/Waffelson/Transparent-Twitch-Chat-Overlay-Electron/refs/heads/main/20250626_09h52m39s_grim.png)

# Requirements 
Electron
# Instalation and Usage 
Create file_name.js and paste this code

```
const { app, BrowserWindow, globalShortcut } = require('electron');

app.whenReady().then(() => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    frame: false,          
    transparent: true,     
    alwaysOnTop: true,     
    hasShadow: false,
    resizable: true,
    webPreferences: {
      contextIsolation: true,
      webSecurity: true
    }
  });

  
  win.loadURL('https://www.giambaj.it/twitch/jchat/v2/?channel=streamer_name&size=3&font=1&animate=true&bots=true&hide_badges=true&shadow=3');

  
  win.webContents.on('did-finish-load', () => {
    win.setBackgroundColor('#00000000');

    // Change font and opacity 
    win.webContents.insertCSS(`
      * {
        font-family: 'Ubuntu', monospace !important;
      }

      html {
        background-color: rgba(0, 0, 0, 0.2) !important;
      }
    `);
  });

  // Changing scale with shortcuts 
  let zoom = 1.0;

  globalShortcut.register('CommandOrControl+-', () => {
    zoom = Math.max(0.25, zoom - 0.1);
    win.webContents.setZoomFactor(zoom);
  });

  globalShortcut.register('CommandOrControl++', () => {
    zoom += 0.1;
    win.webContents.setZoomFactor(zoom);
  });
});
```

# Usage

Edit script and write your twitch name after "channel=" with no gaps                                                                              
Example:    
 ``` win.loadURL('https://www.giambaj.it/twitch/jchat/v2/?channel=kaicenat&size=3&font=1&animate=true&bots=true&hide_badges=true&shadow=3');```


 launching on wayland

```electron --ozone-platform=wayland file_name.js```

 launching everywhere except wayland

```electron file_name.js```


In KDE Plasma, you can create a rule to simulate window overlay so that it stays on top of a fullscreen window (a popup window opens when you press ALT+F3).
https://github.com/user-attachments/assets/4c03ccdf-1636-4349-b80d-e08d443a164a

