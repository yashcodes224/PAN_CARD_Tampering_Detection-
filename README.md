**PAN CARD Tampering Detection Project**

The PAN CARD Tampering Detection Project is an open-source initiative aimed at developing a system to detect tampering or alterations in PAN (Permanent Account Number) cards. PAN cards are crucial identity documents used for financial and tax-related purposes, and ensuring their authenticity is of paramount importance. This project leverages computer vision and machine learning techniques to identify potential tampering, alterations, or counterfeit PAN cards.

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Configuration](#configuration)
6. [Contributing](#contributing)
7. [License](#license)
8. [Contact](#contact)

## Introduction
This project is designed to detect tampering of PAN cards using computer vision techniques. It helps organizations verify the authenticity of PAN cards provided by employees, customers, or other individuals by comparing the structural similarity of the original PAN card with the user-provided PAN card.

## Features
- **Structural Similarity Calculation**: Compares the structural similarity index (SSIM) between original and user-provided PAN cards.
- **Image Processing**: Converts images to grayscale and applies thresholding to highlight differences.
- **Contours Detection**: Identifies and marks the areas of difference between the images.
- **Visual Feedback**: Provides visual representation of the differences through marked contours.

## Installation
To get started with the PAN Card Tampering Detection project, follow these steps:

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/pan-card-tampering-detection.git
   cd pan-card-tampering-detection
   ```

2. **Install Dependencies**
   Ensure you have Python installed. Then, install the necessary packages:
   ```bash
   pip install -r requirements.txt
   ```

## Usage
To use the PAN Card Tampering Detection script, follow these steps:

1. **Create Necessary Directories**
   ```bash
   mkdir -p pan_card_tampering/image
   ```

2. **Run the Script**
   ```python
   python main.py
   ```

### Example Code
```python
# Import necessary packages
from skimage.metrics import structural_similarity
import imutils
import cv2
from PIL import Image
import requests

# Load images
original = Image.open(requests.get('https://www.thestatesman.com/wp-content/uploads/2019/07/pan-card.jpg', stream=True).raw)
tampered = Image.open(requests.get('https://assets1.cleartax-cdn.com/s/img/20170526124335/Pan4.png', stream=True).raw)

# Resize images
original = original.resize((250, 160))
original.save('pan_card_tampering/image/original.png')
tampered = tampered.resize((250,160))
tampered.save('pan_card_tampering/image/tampered.png')

# Load images with OpenCV
original = cv2.imread('pan_card_tampering/image/original.png')
tampered = cv2.imread('pan_card_tampering/image/tampered.png')

# Convert images to grayscale
original_gray = cv2.cvtColor(original, cv2.COLOR_BGR2GRAY)
tampered_gray = cv2.cvtColor(tampered, cv2.COLOR_BGR2GRAY)

# Compute SSIM
(score, diff) = structural_similarity(original_gray, tampered_gray, full=True)
diff = (diff * 255).astype("uint8")
print("SSIM: {}".format(score))

# Threshold and contours
thresh = cv2.threshold(diff, 0, 255, cv2.THRESH_BINARY_INV | cv2.THRESH_OTSU)[1]
cnts = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
cnts = imutils.grab_contours(cnts)

# Draw bounding boxes
for c in cnts:
    (x, y, w, h) = cv2.boundingRect(c)
    cv2.rectangle(original, (x, y), (x + w, y + h), (0, 0, 255), 2)
    cv2.rectangle(tampered, (x, y), (x + w, y + h), (0, 0, 255), 2)

# Display images
Image.fromarray(original).show()
Image.fromarray(tampered).show()
Image.fromarray(diff).show()
Image.fromarray(thresh).show()
```

## Configuration
This project does not require extensive configuration. Ensure you have the necessary dependencies installed and images available for comparison.

## Contributing
We welcome contributions! Please follow these steps to contribute:

1. **Fork the Repository**
2. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature
   ```
3. **Commit Your Changes**
   ```bash
   git commit -m 'Add some feature'
   ```
4. **Push to the Branch**
   ```bash
   git push origin feature/your-feature
   ```
5. **Create a Pull Request**

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact
For any questions or feedback, please contact us at [your-email@example.com](mailto:yashsinghmain2002@gmail.com).

---

Thank you for using our PAN Card Tampering Detection project! We hope it helps you in verifying the authenticity of PAN cards efficiently.
