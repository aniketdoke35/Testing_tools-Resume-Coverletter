# 📄 Cover Letter Template System

**Version 1.0**

Welcome to the **Cover Letter Template System**. This project is designed to allow frontend developers to create modular, high-quality, and printable cover letter templates that are dynamically populated with user data. 

The system separates **Data** (JSON) from **Presentation** (HTML/CSS), enabling users to switch designs instantly without re-typing their content.

---

## 🏗 System Architecture

The project consists of three main components:

1.  **The Viewer (`viewer.html`)**: A central dashboard that acts as the "Parent" window. It loads the user data (`index.json`) and provides a UI to switch between templates.
2.  **The Templates (`coverletter/*.html`)**: Standalone HTML files that act as "Child" pages. They are loaded inside an `<iframe>` within the Viewer.
3.  **The Bridge (`postMessage`)**: The Viewer communicates with the Template using the browser's `window.postMessage` API. This allows us to inject data securely without requiring a backend or complex build steps.

**Data Flow:**
`index.json` -> `viewer.html` -> [User Selects Template] -> `iframe` loads Template -> `postMessage({type: 'RENDER_DATA', payload: ...})` -> Template updates DOM.

---

## 📁 Project Structure

```text
.
├── index.json                # User data source (Edit this to change content)
├── viewer.html               # The testing dashboard / GUI
├── README.md                 # Documentation
└── coverletter/              # Directory for all templates
    ├── template1.html        # Standard Template
    ├── template2.html        # Creative Template
    └── assets/               # (Optional) Shared images/fonts
```

---

## 🚀 Developer Guide: Creating a New Template

Follow this strict guide to ensure your template renders correctly in the viewer and prints perfectly to PDF.

### 1. File Setup
Create a new file in `coverletter/` (e.g., `modern-blue.html`). 
*   **Do not** depend on external CSS files unless they are CDNs (Google Fonts, FontAwesome). Keep styles internal (`<style>`) to ensure the template is self-contained and portable.

### 2. The HTML Structure
Use semantic HTML5. Assign unique `id` attributes to every element that needs dynamic data.

**Standard ID Reference:**
*   **Sender**: `sender-name`, `sender-title`, `sender-phone`, `sender-email`, `sender-address`, `sender-website`
*   **Recipient**: `recipient-name`, `recipient-position`, `recipient-company`, `recipient-address`
*   **Letter**: `letter-date`, `letter-subject`, `letter-body` (Container)
*   **Sign-off**: `closing-text`, `signature-text`, `signature-img`

### 3. CSS & Print Optimization
This is the most critical part. Your CSS must handle both screen viewing and printing.

**Critical Requirements:**
*   **Dimensions**: Always use `210mm` x `297mm` (A4).
*   **Backgrounds**: If you use colored backgrounds, enable `print-color-adjust`.
*   **Margins**: The `@page` rule must zero out browser defaults.

```css
/* BASE STYLES */
body {
    margin: 0;
    padding: 0;
    /* Hide scrollbars in the viewer specifically */
    overflow: hidden; 
    scrollbar-width: none; 
}

/* THE PAGE CONTAINER */
.page {
    width: 210mm;
    min-height: 297mm;
    margin: 0 auto;
    background: white;
    padding: 40px;
    box-sizing: border-box;
    position: relative;
}

/* PRINT MEDIA QUERY (Saving as PDF) */
@media print {
    @page {
        margin: 0; /* Removing browser header/footers */
        size: A4 portrait;
    }
    html, body {
        height: 100%;
        margin: 0 !important;
        padding: 0 !important;
    }
    .page {
        width: 100%;
        height: 100%;
        box-shadow: none; /* Remove drop shadows for print */
        margin: 0;
    }
    /* Force background colors to print */
    * {
        -webkit-print-color-adjust: exact !important;
        print-color-adjust: exact !important;
    }
}
```

### 4. JavaScript Integration
Copy and paste this logic script at the bottom of your template body. This handles the data injection.

```html
<script>
    // 1. LISTEN: Wait for the Viewer to send data
    window.addEventListener("message", function(event) {
        if (event.data && event.data.type === "RENDER_DATA") {
            renderRawData(event.data.payload);
        }
    });

    // 2. RENDER: Map JSON fields to IDs
    function renderRawData(data) {
        if (!data) return;

        function text(id, val) {
            const el = document.getElementById(id);
            if (el) {
                el.innerText = val || '';
                // Hide element if value is empty so no gaps appear
                el.style.display = val ? '' : 'none'; 
            }
        }

        // Sender
        text('sender-name', data.nameAndContact?.fullName);
        text('sender-title', data.nameAndContact?.title); // Optional
        text('sender-email', data.nameAndContact?.email);
        
        // Recipient
        if (data.recipient) {
            text('recipient-name', data.recipient.hiringManagerName);
            text('recipient-company', data.recipient.companyName);
        }

        // Body (Handling Arrays)
        const bodyEl = document.getElementById('letter-body');
        if (bodyEl && data.letterBody?.paragraphs) {
            bodyEl.innerHTML = ''; // Clear strict HTML
            data.letterBody.paragraphs.forEach(para => {
                const p = document.createElement('p');
                p.textContent = para;
                bodyEl.appendChild(p);
            });
        }
        
        // ... Add other fields as needed
    }
</script>
```

---

## 🛠 Testing & Usage

Since this project fetches local JSON files and uses iframes, browsers will block it if you simply double-click `viewer.html` (due to **CORS policy**). You **must** run a local web server.

### Option A: VS Code Live Server (Fastest)
1.  Open this folder in VS Code.
2.  Install the **"Live Server"** extension (by Ritwick Dey).
3.  Right-click `viewer.html` in the file explorer sidebar.
4.  Select **"Open with Live Server"**.

### Option B: Node.js (Standard)
If you have Node installed, use the generic http-server:
```bash
npx http-server .
```
Then open `http://localhost:8080/viewer.html` in your browser.

---

## ❓ Troubleshooting

**Q: The data isn't loading / "Network Error".**
A: Check the URL bar. If it starts with `file://`, it won't work. It must allow `http://localhost...`. See the "Testing & Usage" section above.

**Q: The background colors aren't printing.**
A: Ensure your CSS includes `-webkit-print-color-adjust: exact;` inside the `@media print {}` block.

**Q: The PDF has an extra blank page.**
A: This usually happens if an element has `height: 100vh` or extra margin. Ensure `.page` has `min-height: 297mm` but not a fixed height that forces overflow.

---

## 🤝 Contribution Guidelines

*   **Naming**: Name templates `templateX.html` or descriptive names like `modern-minimal.html`.
*   **Images**: Do not rely on local image paths (like `../img/logo.png`) unless you bundle them. Prefer Base64 or external URLs for demo avatars.
*   **Clean Code**: Remove all `console.log` statements before committing.


