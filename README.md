# Snatchy Extension

Take text from anywhere on the web, even where it is not selectable.

**Snatchy** is a Chrome extension that allows users to extract and copy text from images, scanned documents, and other non-selectable content using OCR technology.

[**Backend Repository**](https://github.com/parthtyagi9/Snatchy-Backend) (Private) · [**Issues**](https://github.com/parthtyagi9/Snatchy-Extension/issues)

---

## Overview

Many websites contain text inside images, screenshots, or rendered canvases where normal copy and paste does not work. Snatchy solves this by detecting text inside images and overlaying selectable regions directly on the page.

The extension is designed with a **local-first approach** for performance and privacy, with an optional advanced mode for improved accuracy on difficult extractions.

---

## Features

* **Hover-based interaction** - A small button appears when you hover over an image
* **Selectable text overlay** - Text is rendered as selectable regions on top of images
* **Standard copy behavior** - Use normal keyboard shortcuts to copy extracted text
* **Local OCR (Free)** - Runs entirely in the browser using Tesseract.js, no data sent externally
* **Advanced Mode (Optional)** - Connect to a backend for higher accuracy on complex images
* **Privacy-first** - Local mode keeps all data on your device

---

## Modes

### Local Mode (Default, Always Free)

* Uses in-browser OCR (Tesseract.js)
* Fast and instananeous
* Works well for clear images and screenshots
* No backend connection required
* **Your data never leaves your device**

### Advanced Mode (Optional, Requires Backend)

* Connects to a private backend server for enhanced OCR
* Higher accuracy for:
  * Blurry or low-resolution images
  * Complex layouts and stylized fonts
  * Scanned documents and handwriting
* Requires self-hosted or private backend server
* Requires authentication token

---

## Installation & Usage

### For Users

1. Install from Chrome Web Store (coming soon)
2. Click the extension icon to open settings
3. Hover over any image on a webpage
4. Click "Extract Text" button
5. Select and copy the text

### For Developers

1. Clone the repository:
   ```bash
   git clone https://github.com/parthtyagi9/Snatchy-Extension.git
   cd Snatchy-Extension
   ```

2. Open Chrome and navigate to `chrome://extensions/`

3. Enable **Developer Mode** (top right)

4. Click **Load unpacked** and select this directory

---

## Development

### Tech Stack
* **Language:** TypeScript / JavaScript
* **API:** Chrome Extensions Manifest V3
* **OCR:** Tesseract.js (WebAssembly)
* **Storage:** Chrome Storage API (for local backups)

### Architecture

```
┌─────────────────────────────┐
│  Content Scripts (DOM)       │  Extract images, render overlays
├─────────────────────────────┤
│  Service Worker             │  Handle background logic
├─────────────────────────────┤
│  Tesseract.js (Local OCR)   │  Process images in-browser
├─────────────────────────────┤
│  Optional Backend API       │  Advanced OCR (if configured)
└─────────────────────────────┘
```

### Backend Configuration

To enable Advanced Mode, configure the backend API endpoint in `config.js`:

```javascript
// config.js
const API_CONFIG = {
  backendUrl: ‘https://your-backend-domain.com’,
  apiKey: ‘your-api-key’, // or use Chrome authentication flow
  enableAdvancedMode: true,
};
```

The extension will send image data to your backend only when users explicitly request Advanced Mode.

---

## API Integration (Optional)

If you’re running the [Snatchy Backend](https://github.com/parthtyagi9/Snatchy-Backend), the extension will communicate via these endpoints:

* `POST /api/auth/login` - Authenticate
* `POST /api/ocr/advanced` - Extract text using advanced OCR
* `POST /api/texts` - Save extraction history (optional)
* `GET /api/texts` - Retrieve extraction history (optional)

See the [Backend Repository](https://github.com/parthtyagi9/Snatchy-Backend) for full API documentation.

---

## How It Works

1. User hovers over an image
2. Extension overlay detects the image
3. "Extract Text" button appears on hover
4. User clicks the button
5. **Local Mode:** Tesseract.js processes immediately
   - OR
   **Advanced Mode:** Image sent to backend for processing
6. Text results rendered as selectable overlays
7. User selects and copies text normally

---

## Limitations

* Handwriting recognition is limited in local mode
* Very low-resolution images may reduce accuracy
* Complex layouts work better in advanced mode
* Some cross-origin images may not be accessible due to browser security
* CORS restrictions may apply when connecting to backend

---

## Roadmap

* Region-based selection (drag to scan specific areas)
* Translation of extracted text
* Summarization of content
* Batch OCR support
* Local history of extractions
* Settings panel for advanced configuration

---

## Contributing

Contributions are welcome! Please open an issue before making major changes to discuss the approach.

### Development Guidelines
* Fork the repository
* Create a feature branch
* Test thoroughly in Chrome Developer Mode
* Submit a pull request

---

## License

MIT License - See LICENSE file for details

---

## Related Projects

* **[Snatchy Backend](https://github.com/parthtyagi9/Snatchy-Backend)** (Private) - Optional Java/Spring Boot backend for advanced OCR, user accounts, and history tracking
