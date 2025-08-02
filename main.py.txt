from fastapi import FastAPI, UploadFile, File, HTTPException
from fastapi.responses import JSONResponse, StreamingResponse
import cv2
import numpy as np
import base64
from io import BytesIO
from PIL import Image

app = FastAPI()

# Load the OpenCV face detector
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

@app.get("/")
def root():
    return {"message": "Face Extractor API is running."}

@app.post("/extract/base64")
async def extract_face_base64(file: UploadFile = File(...)):
    try:
        contents = await file.read()
        image_np = np.frombuffer(contents, np.uint8)
        img = cv2.imdecode(image_np, cv2.IMREAD_COLOR)

        if img is None:
            raise HTTPException(status_code=400, detail="Invalid image format.")

        gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5)

        if len(faces) == 0:
            raise HTTPException(status_code=422, detail="No face detected.")

        x, y, w, h = sorted(faces, key=lambda x: x[2]*x[3], reverse=True)[0]
        face = img[y:y+h, x:x+w]

        _, buffer = cv2.imencode('.jpg', face)
        base64_face = base64.b64encode(buffer).decode('utf-8')

        return {"base64_image": base64_face}

    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))


@app.post("/extract/image")
async def extract_face_image(file: UploadFile = File(...)):
    try:
        contents = await file.read()
        image_np = np.frombuffer(contents, np.uint8)
        img = cv2.imdecode(image_np, cv2.IMREAD_COLOR)

        if img is None:
            raise HTTPException(status_code=400, detail="Invalid image format.")

        gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5)

        if len(faces) == 0:
            raise HTTPException(status_code=422, detail="No face detected.")

        x, y, w, h = sorted(faces, key=lambda x: x[2]*x[3], reverse=True)[0]
        face = img[y:y+h, x:x+w]

        face_rgb = cv2.cvtColor(face, cv2.COLOR_BGR2RGB)
        pil_img = Image.fromarray(face_rgb)
        buffer = BytesIO()
        pil_img.save(buffer, format="PNG")
        buffer.seek(0)

        return StreamingResponse(buffer, media_type="image/png")

    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
