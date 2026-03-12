---
layout: post
title: "How Electron apps can handle excel downloads"
date: 2026-03-10 17:30
tags: electron react downloads
navigation: true
class: post-template
current: post
---

I'm getting into Electron for some work projects and have recently been learning more about how it works.

I asked an AI agent, using OpenCode, to break down an existing project's structure, particularly regarding how we can download an excel file from our API. Here's what it gave me. Normally I wouldn't just copy/paste text from an AI, but I actually found the explanation helpful, so I want to keep it here for my future reference. At the end, after the AI content, I will share some of my thoughts on using AI in this way.

## How Excel File Downloads Work (AI agent's take)

## What Is Electron?

Before diving in, here's a quick explanation: Electron is a framework that lets you build desktop apps using web technologies (HTML, CSS, JavaScript/TypeScript). An Electron app has two main parts:

- **Main Process**: The "backend" that runs on your computer, with full access to your file system, dialog boxes, and other system features

- **Renderer Process**: The "frontend" (your React app) that displays the user interface

These two processes can't talk to each other directly—they communicate through a special mechanism called **IPC** (Inter-Process Communication).

---

## Key Electron Concepts Explained

### What Is IPC?

**IPC** stands for **Inter-Process Communication**. It's a way for two separate processes to talk to each other.

In Electron:

- The renderer (your React app) is like a website running in a sandboxed browser

- The main process is like a Node.js server with full system access

- They can't directly access each other's variables or functions

- IPC is the "telephone line" that lets them send messages back and forth

Think of it like this: the renderer is a customer (your React app) and the main process is a bank teller. The customer can't walk into the vault directly—they must ask the teller to do things for them. IPC is the request system that passes messages between them.

### What Are IPC Handlers?

An **IPC handler** is a function in the main process that "handles" requests coming from the renderer. It's like a receptionist who receives requests and routes them to the right person.

In this app, handlers are defined in files like `excelOperations.js`:

```javascript
// This is an IPC handler - it waits for requests named 'saveExcelBlob'

ipcMain.handle("saveExcelBlob", async (event, data, fileName) => {
  // Do something with the file (show dialog, save to disk, etc.)

  return { error: false, message: "File saved successfully" };
});
```

The handler does three things:

1. **Listens** for a specific request name (`'saveExcelBlob'`)

2. **Processes** the request (shows dialog, writes file, etc.)

3. **Responds** back to the renderer with a result

### How Are IPC Handlers Registered?

In this app, IPC handlers are registered in `main.js` when the app starts. Here's how it works:

**Step 1**: Import the handler functions at the top of `main.js`:

```javascript
const {
  registerExcelOperationsHandlers,
} = require("./ipcHandlers/excelOperations");
```

**Step 2**: Call them when creating the main window, passing in the window object:

```javascript

function createMainWindow() {
    const mainWindow = new BrowserWindow({...});

    // Register handlers here

    registerExcelOperationsHandlers(mainWindow);

}

```

**Step 3**: Inside the handler file (`excelOperations.js`), the function registers each handler with `ipcMain.handle()`:

```javascript
const registerExcelOperationsHandlers = (mainWindow) => {
  ipcMain.handle("saveExcel", async (event, tableData, fileTitle) => {
    // handler code
  });

  ipcMain.handle("saveExcelBlob", async (event, data, fileName) => {
    // handler code
  });
};
```

The `mainWindow` parameter is passed because some dialogs need to be associated with a window (they're "modal" to that window).

### What Is the Context Bridge?

The **context bridge** is a safety feature that controls what the renderer can access. Without it, the renderer would have **no way** to communicate with the main process (completely isolated for security).

In `preload.js`, the context bridge is set up like this:

```javascript
const { contextBridge, ipcRenderer } = require("electron");

contextBridge.exposeInMainWorld("electron", {
  saveExcelBlob: (buffer, fileName) =>
    ipcRenderer.invoke("saveExcelBlob", buffer, fileName),
});
```

Here's what each part does:

- **`contextBridge`**: Electron's API for safely exposing things to the renderer

- **`exposeInMainWorld('electron', {...})`**: Makes this object available as `window.electron` in your React code

- **`ipcRenderer.invoke()`**: Sends a message to the main process and waits for a response

The key security benefit: **you choose exactly what to expose**. In this app, the renderer can only call the specific functions listed in the preload script. It can't:

- Access the file system directly

- Open dialogs directly

- Run any other Node.js code

This is called **context isolation**—the renderer's JavaScript environment is isolated from Node.js, and it can only communicate through the bridge you explicitly create.

### Summary of the Relationship

```
main.js

   │
   ├── imports handler functions from ipcHandlers/*.js
   │

   ▼

registerExcelOperationsHandlers(mainWindow)

   │
   ├── registers ipcMain.handle() for each operation
   │

   ▼

excelOperations.js defines handlers like:

   │
   ├── 'saveExcel'    → writes table data to Excel
   ├── 'saveExcel2'   → writes JSON to Excel
   └── 'saveExcelBlob' → saves base64 data to file

       │

       ◄─────────────── receives requests via IPC

preload.js

   │
   ├── creates window.electron.saveExcelBlob()
   │

   ▼

React code (useDownloadJobsExcel.ts)

   │
   └── calls window.electron.saveExcelBlob()

```

---

## The Complete Download Flow

Here's what happens when you click a button to download an Excel file of jobs:

### Step 1: User Initiates Download (React Hook)

The process starts in `useDownloadJobsExcel.ts`, which is a React hook that manages the download functionality:

```typescript
// The hook provides a function that gets called when user clicks the download button

const downloadjobsExcel = async (clientID: string, clientName: string) => {
  // ... validation and loading state handling

  // This calls the API to get the Excel data
  const blobResult = await exportjobsByClientID(clientID);

  // Convert the blob to base64 (more on this later)
  const base64 = arrayBufferToBase64(arrayBuffer);

  // Call the Electron "saveExcelBlob" function to show a save dialog
  const result = await window.electron.saveExcelBlob(base64, fileName);
};
```

**Key point**: The React code runs in the "renderer" (frontend), but it can't directly access your file system or show save dialogs. That's why it needs Electron's help.

---

### Step 2: Fetching Data from the API

The `exportjobsByClientID` function in `jobAPI.ts` makes an HTTP request to the backend API:

```typescript
export const exportjobsByClientID = async (
  clientID: string,
): Promise<Blob | Error> => {
  // The API returns raw binary data (a "Blob"), not JSON

  const response = await fetch(
    `${baseUrl}/api/jobs/client/${clientID}/export`,
    {
      method: "GET",

      headers: {
        authorization: `Bearer ${token}`,

        sessionid: `${sessionid}`,
      },
    },
  );

  // Convert the response to a Blob (binary data)

  const blob = await response.blob();

  return blob;
};
```

**Note**: This function uses `fetch` directly instead of the usual `fetchData` helper because it needs raw binary data (the Excel file), not JSON.

---

### Step 3: Converting Data for Electron

Back in the React hook, the binary blob is converted to a **base64 string**:

```typescript
// Why convert to base64?
// Electron's IPC (communication between processes) has trouble sending raw binary data.
// Base64 is a way to represent binary data as text characters.
const base64 = arrayBufferToBase64(arrayBuffer);
```

---

### Step 4: Calling Electron via the Preload Bridge

The React code calls `window.electron.saveExcelBlob(base64, fileName)`. But wait—what is `window.electron`?

This is defined in `preload.js`, which acts as a bridge between the renderer and main processes:

```javascript
// In preload.js

contextBridge.exposeInMainWorld("electron", {
  // This exposes the saveExcelBlob function to the renderer

  saveExcelBlob: (buffer, fileName) =>
    ipcRenderer.invoke("saveExcelBlob", buffer, fileName),
});
```

**What does this do?**

- `contextBridge.exposeInMainWorld` makes functions available in your React app's `window` object

- `ipcRenderer.invoke` sends a message to the main process, asking it to run the `'saveExcelBlob'` handler

---

### Step 5: Main Process Handles the Request

The main process (in `main.js`) has already registered a handler for `'saveExcelBlob'`. This is set up in `excelOperations.js`:

```javascript
// In excelOperations.js (registered in main.js)

ipcMain.handle("saveExcelBlob", async (event, data, fileName) => {
  // 1. Show a "Save As" dialog so user can choose where to save

  const result = await dialog.showSaveDialog({
    defaultPath: fileName,

    filters: [{ name: "Excel Files", extensions: ["xlsx"] }],
  });

  // 2. If user didn't cancel, write the file to the chosen location

  if (!result.canceled && result.filePath) {
    // Convert base64 back to binary buffer
    const buffer = Buffer.from(data, "base64");

    // Write to the file system
    await fsPromises.writeFile(result.filePath, buffer);

    return { error: false, message: "File saved successfully" };
  }
});
```

**What happens here:**

1. `dialog.showSaveDialog()` opens a native OS dialog where you choose where to save the file

2. The base64 data is converted back to a binary Buffer

3. `fsPromises.writeFile()` saves the file to the chosen path

---

## Summary Diagram

```
┌─────────────────────────────────────────────────────────────────┐

│                      RENDERER PROCESS (React)                  │

│                                                                 │

│  1. userDownloadjobsExcel()                               │

│         │                                                       │

│         ▼                                                       │

│  2. exportjobsByClientID() ──────► API Server             │

│         │                        (gets Excel as Blob)           │

│         ▼                                                       │

│  3. Convert Blob → ArrayBuffer → Base64                        │

│         │                                                       │

│         ▼                                                       │

│  4. window.electron.saveExcelBlob(base64, filename)           │

│         │                                                       │

│         └────────────── ipcRenderer.invoke() ─────────────┐   │

└─────────────────────────────────────────────────────────────┘   │

                                                              │

                              ┌────────────────────────────────┘

                              │

                              ▼

┌─────────────────────────────────────────────────────────────────┐

│                       MAIN PROCESS (Node.js)                    │

│                                                                 │

│  5. ipcMain.handle('saveExcelBlob', ...)                      │

│         │                                                       │

│         ▼                                                       │

│  6. dialog.showSaveDialog() ──► User sees "Save As" dialog     │

│         │                                                       │

│         ▼                                                       │

│  7. Buffer.from(base64, 'base64') ──► Convert back to binary   │

│         │                                                       │

│         ▼                                                       │

│  8. fsPromises.writeFile() ──► File saved to disk!            │

└─────────────────────────────────────────────────────────────────┘

```

---

## Why Is This Architecture Necessary?

You might wonder why there are so many steps. Here's why:

1. **Security**: Electron apps run web code, which could be dangerous if it had direct access to your file system. By default, the renderer process can't access files.

2. **Context Isolation**: The preload script carefully exposes only specific functions to the renderer, preventing malicious code from causing harm.

3. **IPC Serialization**: When sending data between processes, it must be serializable. Raw binary data doesn't serialize well, so we convert to base64.

---

## Key Files Involved

| File | Purpose |

|------|---------|

| `useDownloadJobsExcel.ts` | React hook that orchestrates the download |

| `jobAPI.ts` | Makes HTTP request to get Excel data from API |

| `preload.js` | Bridge that exposes Electron functions to React |

| `excelOperations.js` | Main process handlers for file operations |

| `main.js` | Electron app entry point that registers handlers |

## End of AI generated content - my take / reflection on using AI agents to analyze and learn about a codebase

This is a bit tangential to the original topic of Electron.

I was surprised at how well the agent analyzed the codebase given my original prompt, which probably could have been written better. I don't remember exactly what I asked, but it was probably something along the lines of "given these files a,b, and c, explain to me how file downloads are done in an electron app - assume I don't know very much about electron".

The output it gave me, with the explanations of IPC, renderer, and main processes, was quite helpful for my learning, especially seeing the real files involved.

Sometimes I'm concerned about whether coding with AI will take the fun out of it - after all, I got into this mostly because I loved tinkering around making front-ends. That was fully hands-on.

I couldn't help but feel, however, that having tools like this that could help explain a codebase to me would have been a complete <strong>gamechanger</strong> when I was starting out as a junior, feeling lost in legacy codebases without much documentation. If I could make my own documentation with these tools, I would have been up to speed a lot faster.

I think a lot of these CEOs who are saying that junior/entry level roles are not going to be needed any more are not considering this - sure, you could delegate junior level work to an AI. But, AI can assist a junior to be more productive (as long as they are still given the space to learn hands-on) than in previous generations. And then there's the (hopefully obvious) point - we need juniors to get our next mid and senior level developers.

Ok, getting off my soapbox now. It's been interesting trying out these new tools. I'll probably end up making more posts about using AI tools while building projects.
