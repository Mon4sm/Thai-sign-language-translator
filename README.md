# Thai sign language translator (ThSLT)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](#license)

## Description 
This is a Thai sign language library for python used for translating Thai sign language into plain Thai characters. This library is used with [Mediapipe](https://developers.google.com/mediapipe) and [OpenCV](https://opencv.org/), so you should install those libraries first to make ThSLT function properly. Although the code works about 90% of the time, there are a lot of factors that you need to consider such as background , lighting , camera quality , hardware qualities. To increase the accuracy, you can adjust the <span style="background-color: gray; padding: 2px 4px; border-radius: 4px; color:black;">min_tracking_confidence</span> and <span style="background-color: gray; padding: 2px 4px; border-radius: 4px; color:black;">min_detection_confidence</span> from the default values.


## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Features](#features)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Installation
### Use the following methods to install nessecary libraries
 - To install OpenCV, use the following command
```
pip install opencv-python
```
 - To install Mediapipe, use the following command
```
pip install mediapipe
```
### Use the following method to install ThSLT
 - To install ThSLT, use the following command
```
pip install thslt
```
## Usage
### Here's an example setup for ThSLT:
```
import thslt , cv2 
import mediapipe as mp

mpdraw = mp.solutions.drawing_utils
mphands = mp.solutions.hands
mpdrawing_styles = mp.solutions.drawing_styles
hands = mphands.Hands(static_image_mode=False,
                      model_complexity=1,
                      min_detection_confidence=0.8,
                      min_tracking_confidence=0.8,
                      max_num_hands=2)

capture = cv2.VideoCapture(0)
text_encode = [] 

while True:
    ret, frame = capture.read()
    bgrtorgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    results = hands.process(bgrtorgb)
    thslt.translate(frame,results,text_encode)
    if results.multi_hand_landmarks:
        for hand_landmarks in results.multi_hand_landmarks:
            for id, lm in enumerate(hand_landmarks.landmark):
                h,w,c = frame.shape
                Cx,Cy = int(lm.x*w), int(lm.y*h)
                mpdraw.draw_landmarks(frame,
                                    hand_landmarks,
                                    mphands.HAND_CONNECTIONS,
                                    mpdrawing_styles.get_default_hand_landmarks_style(),
                                    mpdrawing_styles.get_default_hand_connections_style())

    cv2.imshow("Hand Tracking",frame)
    key = cv2.waitKey(1)
    if key == 27: ### Esc key
        break
cv2.destroyAllWindows()

translation = thslt.decode(text_encode)
print(translation)
```
 ThSLT has a built in Mediapipe hand mapping. So mapping the hand landmarks is not necessary. It only helps you notice that the camera is actually mapping your hand.

## Configuration
### Text_encode
 
- declare text_encode as a blank array before start capturing the video
```
text_encode = [] 
```

## Features
### Translate
  - use this function after you create a loop for OpenCV
```
translate(frame,results,text_encode)
```
#### Parameters
- frame = capture.read()
- results = Hands.process(bgrtorgb)
- text_encode = [ ]
### Decode
 - use this function after you exit the loop for OpenCV to encode <span style="background-color: gray; padding: 2px 4px; border-radius: 4px; color:black;">text_encode</span>
```
translation = encode(text_encode)
```
## License

Copyright 2023 Papangkon Ninarundech

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Acknowledgments

Thank you to the individuals who assisted in creating this project.
 - Mr.Jiraroj Wiruchpongsanon


