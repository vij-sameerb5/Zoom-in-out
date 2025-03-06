
# Zoom-in-out

**Description:** This project demonstrates a Python application that enables you to zoom in and out of any image using hand gestures. By leveraging the OpenCV library and the cvzone module, it detects and tracks hand gestures, allowing you to easily control the zoom level of the image.

**Technologies Used:**
- **OpenCV**: For computer vision tasks.
- **cvzone**: Module for easy handling of OpenCV operations.

**Features:**
- Detects and tracks hand gestures.
- Allows easy control of the zoom level of the image.

**Steps to Build the Project:**

**Installation:**
You need to install the required packages using pip:
```
pip install cvzone==1.4.1
pip install mediapipe==0.8.3.1
```

**Deployment:**
To deploy the project, run the following code:
```python
import cv2
from cvzone.HandTrackingModule import HandDetector

cap = cv2.VideoCapture(0)
cap.set(3, 1280)
cap.set(4, 720)

detector = HandDetector(detectionCon=0.7)

startDis = None
scale = 0
cx, cy = 200, 200

while True:
    success, img = cap.read()
    hands, img = detector.findHands(img)
    
    img1 = cv2.imread("one.jpg")  # Specify the image file you want to use

    if len(hands) == 2:
        hand1 = hands[0]
        hand2 = hands[1]

        hand1_fingers = detector.fingersUp(hands[0])
        hand2_fingers = detector.fingersUp(hands[1])

        if hand1_fingers == [1, 1, 0, 0, 0] and hand2_fingers == [1, 1, 0, 0, 0]:
            lmList1 = hand1["lmList"]
            lmList2 = hand2["lmList"]

            if startDis is None:
                length, info, img = detector.findDistance(hand1["center"], hand2["center"], img)
                startDis = length

            length, info, img = detector.findDistance(hand1["center"], hand2["center"], img)
            scale = int((length - startDis)//2)
            cx, cy = info[4:]

    else:
        startDis = None
    
    try:
        h1, w1, _ = img1.shape
        newH, newW = ((h1 + scale)//2)*2 , ((w1 + scale)//2)*2
        img1 = cv2.resize(img1, (newH, newW))
        img[cy - newH//2 : cy + newH//2, cx - newW//2 : cx + newW//2] = img1
    
    except:
        pass
    
    cv2.imshow("Problem Solve with Sameer Shaik", img)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
```

**Get in Touch:**
- Website: [www.sameer-shaik.com](http://www.sameer-shaik.com)
- Gmail: sameer.shaik@gwu.edu

If you have any questions or need clarification, feel free to reach out. Thank you!

**License:**
This script is released under the MIT License. You are free to use, modify, and distribute it as you wish. If you encounter any bugs or have suggestions for improvement, please submit an issue or pull request on this repository.
