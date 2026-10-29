#  Bio-RAG: Lab Protocol & SOP Information Extractor

I got tired of manually copying messy protocols into Benchling, so I built a local-first RAG tool that turns PDFs into structured JSON/tables. I built an entirely client-side web app called Bio-RAG. You drop in a PDF of a research paper, and it spits out a cleanly formatted table of reagents, volumes, and chronological steps. It runs in the browser, so your proprietary IP/ research is never exposed a random server.

demo here: https://pvalissery.github.io/Bio-RAG/

Transferring legacy SOPs or scraping methods sections from scanned PDFs into your ELN or LIMS is a massive time sink. Whether you’re at a CRO, a biopharma CMC team, or just an academic lab, unstructured data is the bane of reproducible science. I was looking for a way to automate this without having to spin up a heavy backend or risk uploading sensitive protocols to some random third-party server. So, I built a serverless frontend tool that handles it locally.

## How it actually works
It’s not just a wrapper for a basic prompt. Getting an LLM to reliably extract just the reagents and chronologically order the steps without hallucinating extra details requires rigid structure. It uses Mozilla’s pdf.js to rip the text directly in your browser. No files are ever uploaded anywhere. It uses the Google Gemini API (gemini-3-flash-preview), but it forces a strictly defined JSON schema. This stops the model from rambling and guarantees you get a programmatically readable output (materials + volumes mapped correctly, steps sequenced). Copy-Paste Ready. The output renders into clean HTML tables you can instantly dump into Benchling, Labguru, or whatever ELN you're using

Since there's no backend, you don't even need to npm install anything. You can just clone the repo and double-click index.html.

One catch: You have to supply your own Gemini API key (you can grab a free one from Google AI Studio). You drop it into the app's settings UI, and it saves locally to your browser storage. If anyone is working in translational medicine or biomanufacturing and deals with standardizing mammalian cell culture or protein purification workflows, give it a spin. Would love to hear if it actually handles the specific brand of formatting chaos your lab produces. Repo is up on GitHub. Let me know if you run into bugs or if there's a local model (like Llama-3) integration you'd want to see next.

## Usage / Getting Started

Since this is a fully self-contained, serverless application, setup is instant:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/bio-rag-extractor.git
   ```
2. **Open the App:**
   Simply open `index.html` in any modern web browser. No `npm install` or backend server required.
3. **Configure API:**
   * Click the "API Settings" button in the top right.
   * Enter your [Google Gemini API Key](https://aistudio.google.com/app/apikey). *(Note: The key is stored safely in your browser's local storage and is only sent directly to Google's API).*
4. **Extract:** Drag and drop a protocol PDF or paste unstructured text to generate structured data.

##  Architecture
* **Frontend:** Vanilla JavaScript + HTML5.
* **Styling:** Tailwind CSS (via CDN).
* **PDF Processing:** Mozilla's `pdf.js` 
* **AI Engine:** Google Gemini (`gemini-3-flash-preview` via REST API).

## Use Cases
* **Translational Medicine:** Rapidly converting legacy clinical trial SOPs into digital, machine-readable formats.
* **Biomanufacturing (CMC):** Standardizing mammalian cell culture and protein purification workflows.
* **Bioinformatics:** Scraping methodology sections from hundreds of journal articles for meta-analyses.

