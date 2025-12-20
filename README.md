# Gradio App Playground (Tests 1–7)

A collection of small **Gradio** examples (“tests”) showcasing different UI patterns and model integrations:

* Basic `gr.Interface` with text + slider
* `Textbox` configuration (multi-line + placeholder)
* Multiple inputs/outputs (checkbox + slider + numeric output)
* Image processing (sepia filter with NumPy)
* Event-driven UI with `gr.Blocks` and button click handlers
* Hugging Face `transformers` pipeline integration (Italian → English translation)
* A simple rule-based chatbot using `gr.Chatbot`

These snippets are designed for quick experimentation in **Google Colab** or a local Python environment. 

---

## Contents

* [Project Structure](#project-structure)
* [Requirements](#requirements)
* [Setup](#setup)
* [How to Run](#how-to-run)
* [Tests Overview](#tests-overview)
* [Using the `/predict` API Endpoint](#using-the-predict-api-endpoint)
* [Notes and Known Issues](#notes-and-known-issues)
* [License](#license)

---

## Project Structure

This repository contains a single script or notebook-style file with multiple independent demos (“Test 1” to “Test 7”). Each test defines its own `demo = gr.Interface(...)` or `with gr.Blocks() as demo:` and launches it.

> Tip: Run **one test at a time** (comment out the others) to avoid port conflicts and multiple servers launching. 

---

## Requirements

* Python 3.9+ (Colab is fine)
* `gradio`
* `numpy` (for the sepia image filter)
* `transformers` (for translation demo)
* Optional: `sacremoses` (recommended by `transformers` for Marian tokenization)

---

## Setup

### Option A — Local (recommended for development)

```bash
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
# .venv\Scripts\activate    # Windows

pip install --upgrade pip
pip install gradio numpy transformers sacremoses
```

### Option B — Google Colab

In Colab, install dependencies in a cell:

```bash
!pip install gradio numpy transformers sacremoses
```

---

## How to Run

1. Open the file and choose the test you want to run.
2. Ensure only that section is active (comment out other tests).
3. Launch the Gradio app:

* Many tests call `demo.launch(share=True)` which generates a temporary public URL in notebook environments. 
* Locally, you can typically use `demo.launch()` without `share=True`.

Example (local):

```bash
python app.py
```

Then open the printed local URL (commonly `http://127.0.0.1:7860`).

---

## Tests Overview

### Test 1 — Greeting UI (Text + Slider)

Creates a greeting function with a “repetition/intensity” slider:

* Input: `name` (text), `intensity` (slider)
* Output: text
* Exposes endpoint via `api_name="predict"` 

### Test 2 — Textbox with Placeholder

Uses `gr.Textbox(lines=2, placeholder="Name here")` as input, returning a simple greeting. 

### Test 3 — Multi-Input, Multi-Output + Temperature Conversion

* Inputs: name (text), morning? (checkbox), temperature (slider)
* Outputs: a greeting string + temperature converted to Celsius (number) 

### Test 4 — Sepia Image Filter (NumPy)

Applies a sepia filter matrix to an input image using NumPy, returning the filtered image. 

### Test 5 — `gr.Blocks` + Button Click

A basic `Blocks` layout:

* Textbox for name
* Button triggers `greet`
* Output textbox displays result 

### Test 6 — Italian → English Translation (Transformers Pipeline)

Uses Hugging Face `transformers`:

* Pipeline task: `translation`
* Model: `Helsinki-NLP/opus-mt-it-en`
* UI with two columns (Italian input → English output)
* Includes `gr.Examples` for quick testing 

### Test 7 — Simple Rule-Based Chatbot

A lightweight chatbot that responds based on message prefixes:

* `"how many"` → random 1–10
* `"how"` → random sentiment word
* `"where"` → random location word
* otherwise → `"I do not know"` 

---

## Using the `/predict` API Endpoint

Some demos set `api_name="predict"`, which exposes an HTTP endpoint.

If `share=True` is used in Colab, Gradio prints a public URL like:

* `https://<random>.gradio.live`

The API endpoint becomes:

* `https://<random>.gradio.live/predict` 

You can call it programmatically (example using `requests`):

```python
import requests

url = "https://<your-subdomain>.gradio.live/predict"
payload = {"data": ["Alice", 3]}  # example: name, intensity

r = requests.post(url, json=payload, timeout=30)
print(r.json())
```
