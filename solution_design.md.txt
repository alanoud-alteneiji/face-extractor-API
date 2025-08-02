Face Extractor API – Architecture Overview

This project is a FastAPI-based web service that takes a scanned ID/passport image, detects the face using OpenCV, crops it, and returns either:
- a base64 string, or
- a direct image (PNG)

The whole thing runs in-memor no disk writes, no image storage, no logging.

Process Flow:

1. User uploads an image to the API
2. The image is converted into an array using OpenCV
3. A face is detected using Haarcascade
4. The face is cropped using bounding box coordinates
5. Output is either:
   - Encoded as base64 and returned
   - Or streamed directly as a PNG image

Tech Used:
- FastAPI for the web service
- OpenCV for face detection
- Pillow for image encoding
- pyngrok to expose the API publicly via Colab

Why this setup?
It’s fast, lightweight, portable, and perfect for testing or integration into onboarding systems. No data is saved or stored, and it works with just a single API call.
