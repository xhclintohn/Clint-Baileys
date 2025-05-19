# 🚀 Baileys WhatsApp API - By kagenouReal (Modified by 𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧)

![GitHub stars](https://img.shields.io/github/stars/kagenouReal/Baileys?style=social)
![GitHub license](https://img.shields.io/github/license/xhclintohn/Clint-Baileys)
![Node.js](https://img.shields.io/badge/node-%3E%3D14.0-green)
![Contributors](https://img.shields.io/github/contributors/xhclintohn/Clint-Baileys)

Baileys WhatsApp API is a Node.js based library for communicating with WhatsApp Web without the need for additional WebSocket. This is a modified version of Whiskey Baileys to make it more stable and support more message types.

Developed with high performance for bot needs, message automation, and integration with other WhatsApp applications.

## 📌 About This Project
This repository is developed and maintained by **kagenou** along with other open-source contributors.  
Support and contributions from the community are greatly appreciated! 💖  

---

## ✨ Key Features

✅ **QR-less Authentication** using session authentication/Pair code
✅ **Latest Multi-Device (MD)** support from WhatsApp  
✅ **Send & receive messages** in various formats  
✅ **Manage groups** (create groups, add/kick members, set descriptions, etc.)  
✅ **Event integration** such as group join/leave, message received, message read  
✅ **TypeScript support** for safer development  

---

## 📦 Installation

Make sure **Node.js ≥ 14.0** is installed,  
Then run the following command in the terminal:

```sh
npm install @xhclintohn/Clint-Baileys
```

Or with **Yarn**:

```sh
yarn add @xhclintohn/Clint-Baileys
```

---

## 🚀 Basic Usage

```javascript
import makeWASocket, { DisconnectReason } from '@kagenouReal/baileys'
import { Boom } from '@hapi/boom'

async function connectToWhatsApp () {
    const sock = makeWASocket({
        // can provide additional config here
        printQRInTerminal: true
    })
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect.error as Boom)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('connection closed due to ', lastDisconnect.error, ', reconnecting ', shouldReconnect)
            // reconnect if not logged out
            if(shouldReconnect) {
                connectToWhatsApp()
            }
        } else if(connection === 'open') {
            console.log('opened connection')
        }
    })
    sock.ev.on('messages.upsert', m => {
        console.log(JSON.stringify(m, undefined, 2))

        console.log('replying to', m.messages[0].key.remoteJid)
        await sock.sendMessage(m.messages[0].key.remoteJid!, { text: 'Hello there!' })
    })
}
// run in main file
connectToWhatsApp()
```

---

## 📜 API Documentation
