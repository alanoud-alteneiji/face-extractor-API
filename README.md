# Face Extractor API

This project lets you upload a scanned ID or passport and returns the portrait face — either as a cropped image or base64 string. It’s built using FastAPI and OpenCV, and everything runs in-memory (nothing gets saved).

---

##  Live Demo

Here’s the public API (running on ngrok):  
**https://7c48a516a154.ngrok-free.app/docs**

You can try both endpoints:
- `/extract/base64` → returns the face as base64
- `/extract/image` → returns the cropped image

---

## What It Does

- Detects the largest face in an image
- Crops it and returns the result
- Works best on scanned IDs, passports, or profile-like images
- Fast, lightweight, and doesn’t store anything

---

## Project Structure

face-extractor-api/
├── app/
│ └── main.py
├── requirements.txt
├── face_extractor_colab.ipynb
├── sample/
│ └── test_id.jpg

---

## Example Response

{
  "base64_image": "iVBORw0KGgoAAAANSUhEUgAA..."
}

##  Tech Stack

- FastAPI
- OpenCV
- Pillow
- NumPy
- pyngrok

## Why I Built It

Cropping faces manually is annoying and repetitive. I wanted something simple that could do it in one API call — especially for scanned documents like IDs or passports, and for things like onboarding workflows.


---
## Run Locally (Optional)

If you want to run it on your laptop:

```bash
git clone https://github.com/yourusername/face-extractor-api.git
cd face-extractor-api
pip install -r requirements.txt
uvicorn app.main:app --reload

or you can open this link : https://7c48a516a154.ngrok-free.app/docs

---


