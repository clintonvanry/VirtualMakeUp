# VirtualMakeUp Project

This is an article how to apply makeup to an image using OpenCV and Dlib libraries

## We will look at implementing 2 makeup features namely
1. Lipstick
2. Blush

However we need to cover a few base topics first.
1. Detect the face
2. Find landmarks (points) on the face.

The above is accomplished with following steps
1. Initialise the face detector

`
frontal_face_detector faceDetector = get_frontal_face_detector();
`

2. Read landmark model for a face using the shape_predictor_68_face_landmarks.dat file

`
  std::string PREDICTOR_PATH =  "../data/models/shape_predictor_68_face_landmarks.dat";
  dlib::shape_predictor landmarkDetector;
  dlib::deserialize(PREDICTOR_PATH) >> landmarkDetector;
`

3. Read the image using OpenCV
4. Convert the image to format the Dlib understands and pass the image to the face detector from Dlib

`
  dlib::cv_image<dlib::bgr_pixel> dlibIm(img);
  std::vector<dlib::rectangle> faceRects = faceDetector(img);
`

5. Retrieve the landmarks from the detected face

`
dlib::full_object_detection landmarks = landmarkDetector(dlibIm, scaledRect);
`

68 landmark points on a face image
---
![](https://github.com/clintonvanry/VirtualMakeUp/blob/main/dlib68points.png)

landmark points we are interested in to apply lipstick and blush
---
![](https://github.com/clintonvanry/VirtualMakeUp/blob/main/poi.jpg)


 ## apply lipstick
 1. Create a mask for the upper lip
 - select the points from the 68 points. e.g. array [48,49,50,51,52,53,54,61,62,63]
 - using convexHull method on the points found. This is important! more [information](https://learnopencv.com/convex-hull-using-opencv-in-python-and-c/)
 - Create image of the same size as the source image but all black
 - use the fillConvexPoly method using points from convexHull and the black image created in the previous step
 3. Create a mask for the lower lip
 - same steps as above
 4. Combine the mask together
 - to combine the the two masks created use the addWeighted function with the weights of 1 for alpha and beta, more [information](https://docs.opencv.org/3.4/d5/dc4/tutorial_adding_images.html) 


 


