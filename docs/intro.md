---
sidebar_position: 1
title: OpenCV Topics
---

# OpenCV Topics

1. **SIFT** (Scale-Invariant Feature Transform)
    
    - Finds unique keypoints in an image that stay recognizable even if the image is scaled or rotated.
    - ```python
        sift = cv2.SIFT_create()
        keypoints, descriptors = sift.detectAndCompute(img, None)
        ```
        
2. **HOG** (Histogram of Oriented Gradients)
    
    - Describes the shape of objects by counting which direction edges point in small patches.
    - ```python
        hog = cv2.HOGDescriptor()
        features = hog.compute(img)
        ```
        
3. **MorphologyEx**
    
    - Cleans up binary images — removes noise, fills holes, or separates touching blobs using shape-based operations.
    - ```python
        kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
        result = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
        ```
        
4. **Blur**
    
    - Smooths an image to reduce noise or detail.
    - ```python
        blurred = cv2.GaussianBlur(img, (5, 5), 0)
        ```
        
5. **Threshold**
    
    - Converts a grayscale image to black & white by turning pixels above a value white and the rest black.
    - ```python
        _, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)
        ```
        
6. **Color Gradients**
    
    - Measures how fast pixel colors change — highlights edges and transitions.
    - ```python
        grad_x = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=3)
        grad_y = cv2.Sobel(img, cv2.CV_64F, 0, 1, ksize=3)
        magnitude = cv2.magnitude(grad_x, grad_y)
        ```
        
7. **Sobel**
    
    - Detects edges by computing the gradient of pixel intensity in X and/or Y direction.
    - ```python
        sobel_x = cv2.Sobel(gray, cv2.CV_64F, 1, 0, ksize=3)
        sobel_y = cv2.Sobel(gray, cv2.CV_64F, 0, 1, ksize=3)
        ```
        
8. **Hough**
    
    - Detects geometric shapes (lines, rectangles, circles) even if they're partially broken or noisy.
    - **Lines**
        
        ```python
        lines = cv2.HoughLinesP(edges, 1, np.pi/180, 100, minLineLength=50, maxLineGap=10)
        ```
        
    - **Circles**
        
        ```python
        circles = cv2.HoughCircles(gray, cv2.HOUGH_GRADIENT, 1.2, 50)
        ```
        
    - **Rectangle** — detected via contours after Hough line grouping (no direct single call).
9. **Canny**
    
    - A smart edge detector that finds clean, thin edges using two thresholds.
    - ```python
        edges = cv2.Canny(img, 100, 200)
        ```
        
10. **Blob**
    
    - Finds and measures circular-ish regions (blobs) in an image based on color, size, or shape.
    - ```python
        detector = cv2.SimpleBlobDetector_create()
        keypoints = detector.detect(img)
        ```
        
11. **Connected Components with Stats**
    
    - Labels separate white regions in a binary image and gives stats like area and bounding box for each.
    - ```python
        num_labels, labels, stats, centroids = cv2.connectedComponentsWithStats(binary)
        ```
        
12. **CLAHE** (Contrast Limited Adaptive Histogram Equalization)
    
    - Boosts contrast in dark or washed-out images without over-brightening.
    - ```python
        clahe = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8, 8))
        result = clahe.apply(gray)
        ```
        
13. **ORB** (Oriented FAST and Rotated BRIEF)
    
    - A fast, free alternative to SIFT — finds and describes keypoints for matching images in real time.
    - ```python
        orb = cv2.ORB_create()
        keypoints, descriptors = orb.detectAndCompute(img, None)
        ```
        
14. **DNN** (Deep Neural Networks)
    
    - Runs pre-trained deep learning models (e.g. YOLO, MobileNet) directly inside OpenCV.
    - **readNet**
        
        ```python
        net = cv2.dnn.readNet("model.weights", "model.cfg")
        ```
        
    - **blobFromImage**
        
        ```python
        blob = cv2.dnn.blobFromImage(img, 1/255, (416, 416), swapRB=True)net.setInput(blob)outputs = net.forward()
        ```
        
    - **NMSBoxes**
        
        ```python
        indices = cv2.dnn.NMSBoxes(boxes, scores, score_threshold=0.5, nms_threshold=0.4)
        ```
        
15. **Contours**
    
    - Finds and analyzes the outlines of shapes in a binary image.
    - **findContours**
        
        ```python
        contours, _ = cv2.findContours(binary, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
        ```
        
    - **approxPolyDP**
        
        ```python
        approx = cv2.approxPolyDP(contour, 0.02 * cv2.arcLength(contour, True), True)
        ```
        
    - **boundingRect**
        
        ```python
        x, y, w, h = cv2.boundingRect(contour)
        ```
        
    - **convexHull**
        
        ```python
        hull = cv2.convexHull(contour)
        ```
        
16. **Geometric Transforms**
    
    - Warps or repositions an image using a transformation matrix — useful for rotation, skew correction, and perspective fixes.
    - **warpAffine** (rotation, translation)
        
        ```python
        M = cv2.getRotationMatrix2D(center, angle=45, scale=1)rotated = cv2.warpAffine(img, M, (w, h))
        ```
        
    - **warpPerspective** (deskewing)
        
        ```python
        M = cv2.getPerspectiveTransform(src_pts, dst_pts)warped = cv2.warpPerspective(img, M, (w, h))
        ```
        
17. **InRange** (Color Masking / HSV Segmentation)
    
    - Isolates pixels within a specific color range — great for tracking a colored object.
    - ```python
        hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
        mask = cv2.inRange(hsv, lower_bound, upper_bound)
        result = cv2.bitwise_and(img, img, mask=mask)
        ```
        
18. **Watershed & Distance Transform** (Separating touching objects)
    
    - Splits objects that are stuck together by treating pixel intensity like a topographic map.
    - ```python
        dist = cv2.distanceTransform(binary, cv2.DIST_L2, 5)
        _, sure_fg = cv2.threshold(dist, 0.7 * dist.max(), 255, 0)
        markers = cv2.watershed(img, markers)
        ```
        
19. **Optical Flow**
    
    - Tracks how pixels move between frames in a video.
    - **PyrLK** (sparse — tracks specific points)
        
        ```python
        next_pts, status, _ = cv2.calcOpticalFlowPyrLK(prev_gray, curr_gray, prev_pts, None)
        ```
        
    - **Farneback** (dense — tracks every pixel)
        
        ```python
        flow = cv2.calcOpticalFlowFarneback(prev_gray, curr_gray, None, 0.5, 3, 15, 3, 5, 1.2, 0)
        ```
        
20. **Background Subtraction**
    
    - Separates moving foreground objects from a static background in video.
    - **MOG2**
        
        ```python
        subtractor = cv2.createBackgroundSubtractorMOG2()fg_mask = subtractor.apply(frame)
        ```
        
    - **KNN**
        
        ```python
        subtractor = cv2.createBackgroundSubtractorKNN()fg_mask = subtractor.apply(frame)
        ```
        
21. **Data Augmentation**
    
    - Artificially creates variations of images to help train ML models.
    - ```python
        resized   = cv2.resize(img, (224, 224))
        flipped   = cv2.flip(img, 1)
        M         = cv2.getRotationMatrix2D(center, 30, 1)
        rotated   = cv2.warpAffine(img, M, (w, h))
        transposed = cv2.transpose(img)
        ```
        
22. **Pyramids**
    
    - Creates smaller or larger versions of an image for multi-scale processing.
    - ```python
        smaller = cv2.pyrDown(img)   # half size
        larger  = cv2.pyrUp(img)     # double size
        ```
        
23. **GrabCut** (Interactive Foreground Extraction)
    
    - Cuts out a foreground object from its background using a rough rectangle as a hint.
    - ```python
        mask = np.zeros(img.shape[:2], np.uint8)
        bgd_model = np.zeros((1, 65), np.float64)
        fgd_model = np.zeros((1, 65), np.float64)
        cv2.grabCut(img, mask, rect, bgd_model, fgd_model, 5, cv2.GC_INIT_WITH_RECT)
        result_mask = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
        ```
        
24. **FLANN** (Fast Library for Approximate Nearest Neighbors)
    
    - Quickly matches descriptors between two images — faster than brute force for large sets.
    - ```python
        index_params = dict(algorithm=1, trees=5)
        search_params = dict(checks=50)
        flann = cv2.FlannBasedMatcher(index_params, search_params)
        matches = flann.knnMatch(desc1, desc2, k=2)
        ```
        
25. **SSIM / BRISQUE** (Image Quality Metrics)
    
    - Measures how similar two images are (SSIM) or how good a single image looks without a reference (BRISQUE).
    - ```python
        # SSIM (requires skimage)
        from skimage.metrics import structural_similarity as ssim
        score, _ = ssim(img1, img2, full=True)
        
        # BRISQUE (OpenCV contrib)
        brisque = cv2.quality.QualityBRISQUE_create("model.yml", "range.yml")
        score = brisque.compute(img)
        ```
        
26. **KalmanFilter**
    
    - Predicts and smooths the position of a moving object over time, even when measurements are noisy.
    - ```python
        kf = cv2.KalmanFilter(4, 2)   # 4 state vars (x,y,dx,dy), 2 measurements (x,y)
        kf.measurementMatrix = np.array([[1,0,0,0],[0,1,0,0]], np.float32)
        kf.transitionMatrix  = np.array([[1,0,1,0],[0,1,0,1],[0,0,1,0],[0,0,0,1]], np.float32)
        predicted = kf.predict()
        corrected = kf.correct(measurement)
        ```
        
27. **Label Normalization**
    
    - În practică, dacă nu știi dinainte x și y (min și max din date), le calculezi din train:
    - ```python
        min_label = df_train['angle'].min()
        max_label = df_train['angle'].max()

        # In dataset:
        label_norm = (label - min_label) / (max_label - min_label)

        # La predictie:
        pred = pred_norm * (max_label - min_label) + min_label
        ```
    
