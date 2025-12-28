# Scan Surprise
## I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?

## Solution 
- ZBar does NOT convert a normal image into readable text like OCR.
- It only extracts encoded data from barcodes & QR codes present in an image.
-
## Main ZBar Tools
### 1️⃣ zbarimg (MOST USED)
- Reads barcodes/QR codes from image files.
- Example:
```
zbarimg image.png
```
  - Output example:
     QR-Code:https://example.com
- ✔ Converts QR → text
- ❌ Cannot read plain text from images
### 2️⃣ zbarcam
- Scans barcodes using a camera (webcam).
```
zbarcam
```
- Used for:
  - Live QR scanning
  - Real-time barcode reading
### 3️⃣ libzbar
- Programming library for developers.
- Used with:
  - Python
  - C/C++
  - Java bindings
- Python example:
```
from pyzbar.pyzbar import decode
from PIL import Image

data = decode(Image.open("qr.png"))
print(data[0].data.decode())
```
