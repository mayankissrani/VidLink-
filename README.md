[https://github.com/mayankissrani/VidLink-/blob/main/assests/webrtc-flow.png](Flow)



# 🔗 VidLink

> **Real-time peer-to-peer video & data communication built with WebRTC, React, TypeScript, and WebSockets.**

VidLink is a WebRTC-based real-time communication project that demonstrates how modern browsers can establish direct peer-to-peer connections for exchanging media and data.

The application uses a **WebSocket signaling server** to coordinate the WebRTC connection while **STUN/TURN** mechanisms assist with connectivity across different network configurations.

---

## ✨ Features

- 🎥 **Real-time video communication**
- 🔊 **Audio communication through WebRTC media streams**
- 🔗 **Peer-to-peer browser communication**
- 📡 **WebSocket-based signaling**
- 🌐 **ICE candidate exchange for connection establishment**
- 🧭 **STUN support for NAT traversal**
- 🔄 **TURN-compatible architecture for relay connections**
- ⚛️ **React + TypeScript frontend**
- 🟢 **Node.js + WebSocket backend**
- ⚡ **Low-latency media communication when a direct P2P path is available**

---

## 🏗️ Architecture

VidLink separates **signaling** from **media communication**.

The signaling server is responsible for helping peers discover and negotiate a connection. Once WebRTC negotiation is completed, media can travel directly between the browsers when network conditions allow it.

### WebRTC Connection Flow

![VidLink WebRTC Flow](./assets/webrtc-flow.png)

### High-level flow

```text
┌──────────────┐                         ┌──────────────┐
│   Browser 1  │                         │   Browser 2  │
│    Peer A    │                         │    Peer B    │
└──────┬───────┘                         └──────┬───────┘
       │                                        │
       │          SDP Offer / Answer             │
       │◄──────── WebSocket Signaling ─────────►│
       │                                        │
       │         ICE Candidate Exchange         │
       │                                        │
       └────────────── WebRTC ──────────────────┘
                       │
                 P2P Media/Data
```

---

## 🧠 How WebRTC Negotiation Works

### 1. Create a peer connection

Each browser creates an `RTCPeerConnection` instance.

```javascript
const pc = new RTCPeerConnection();
```

### 2. Peer A creates an offer

The initiating browser creates an SDP offer and sets it as its local description.

```javascript
const offer = await pc.createOffer();
await pc.setLocalDescription(offer);
```

The offer is sent to the other peer through the signaling server.

### 3. Peer B creates an answer

The receiving browser sets the offer as its remote description, creates an answer, and sends it back.

```javascript
await pc2.setRemoteDescription(offer);

const answer = await pc2.createAnswer();
await pc2.setLocalDescription(answer);
```

### 4. Exchange ICE candidates

WebRTC gathers possible network paths using ICE. Candidates are exchanged through the signaling server so the peers can determine how they can communicate.

### 5. Establish the connection

After successful negotiation, the browsers establish the best available connection path.

When a direct path is possible:

```text
Browser 1 ─────────────── Browser 2
              P2P
```

If a direct connection cannot be established, a TURN relay can be used when configured.

---

## 🌐 STUN, TURN & NAT

### STUN

**STUN — Session Traversal Utilities for NAT**

A STUN server helps a browser discover its public-facing network address and assists WebRTC in finding a viable connection path through NAT.

### TURN

**TURN — Traversal Using Relays around NAT**

TURN acts as a relay when the peers cannot establish a suitable direct connection.

```text
Direct connection:
Browser 1 ───────────── Browser 2

Relay connection:
Browser 1 ───► TURN Server ───► Browser 2
```

### NAT

**NAT — Network Address Translation**

NAT allows devices on private networks to communicate with external networks while using private/local IP addresses internally.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React |
| Language | TypeScript |
| Build Tool | Vite |
| Backend | Node.js |
| Signaling | WebSocket |
| Real-time Communication | WebRTC |
| Media API | `getUserMedia()` |
| Peer Connection | `RTCPeerConnection` |
| NAT Traversal | STUN / TURN |
| Package Manager | npm |

---

## 📁 Project Structure

```text
VidLink/
│
├── Frontend/
│   ├── public/
│   ├── src/
│   ├── package.json
│   ├── index.html
│   └── ...
│
├── Backend/
│   ├── src/
│   ├── package.json
│   └── ...
│
├── assets/
│   └── webrtc-flow.png
│
└── README.md
```

> The exact contents of `src/` may change as the project evolves.

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/)
- npm
- A modern browser with WebRTC support

---

### 1. Clone the repository

```bash
git clone https://github.com/mayankissrani/VidLink.git
cd VidLink
```

---

### 2. Start the frontend

```bash
cd Frontend
npm install
npm run dev
```

The frontend will normally be available at:

```text
http://localhost:5173/
```

---

### 3. Start the backend

Open another terminal:

```bash
cd VidLink/Backend
```

Install dependencies if required:

```bash
npm install
```

Compile the TypeScript backend:

```bash
tsc -b
```

Start the signaling server:

```bash
node dist/index.js
```

---

## 🔌 Running the Application

Once both the frontend and backend are running:

1. Open the application in your browser.
2. Allow camera and microphone permissions.
3. Establish a connection with another browser/client.
4. The signaling server handles the WebRTC negotiation.
5. WebRTC establishes the media/data connection between the peers.

> For testing across different devices or networks, make sure the signaling server is reachable by both peers and that your WebRTC ICE/STUN/TURN configuration is appropriate for the environment.

---

## 🔐 WebRTC & Privacy

VidLink is designed around WebRTC's peer-to-peer communication model.

The signaling server is used for **connection negotiation**, including information such as SDP offers/answers and ICE candidates. It does not inherently mean that the signaling server carries the actual audio/video media.

The actual media path depends on the ICE connection selected by WebRTC. A direct peer-to-peer path may be used when possible, while a configured TURN server can relay traffic when necessary.

---

## 📚 Core WebRTC Concepts

| Concept | Purpose |
|---|---|
| `RTCPeerConnection` | Manages the WebRTC peer connection |
| SDP Offer | Describes the initiating peer's capabilities |
| SDP Answer | Response describing the receiving peer's capabilities |
| ICE | Finds a viable network path between peers |
| ICE Candidate | Represents a possible connection endpoint |
| STUN | Helps discover public network information |
| TURN | Relays traffic when direct connectivity fails |
| WebSocket | Provides the signaling channel |

---

## 🧪 Development

To make changes to the frontend:

```bash
cd Frontend
npm install
npm run dev
```

For backend development:

```bash
cd Backend
npm install
tsc -b
node dist/index.js
```

---

## 🔮 Future Improvements

Potential extensions for VidLink include:

- [ ] Multi-user video rooms
- [ ] Screen sharing
- [ ] Text chat using WebRTC DataChannels
- [ ] File transfer using DataChannels
- [ ] Better connection-status indicators
- [ ] Network quality monitoring
- [ ] Configurable STUN/TURN servers
- [ ] Authentication and user accounts
- [ ] Production deployment
- [ ] Mobile-responsive calling interface

---

## 🎯 Learning Objectives

This project is useful for understanding:

- WebRTC peer-to-peer architecture
- SDP offer/answer negotiation
- ICE candidate discovery and exchange
- NAT traversal
- STUN and TURN
- WebSocket signaling
- Real-time browser communication
- React + TypeScript application architecture
- Client/server coordination in real-time applications

---

## 👨‍💻 Author

**Mayank Issrani**

GitHub: [@mayankissrani](https://github.com/mayankissrani)

---

## ⭐ Project

If you find VidLink useful for learning about WebRTC and real-time communication, consider giving the repository a ⭐.

**Built with React, TypeScript, Node.js, WebSocket, and WebRTC.**
