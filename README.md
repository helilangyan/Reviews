# CS381 Exam 2
## Run length encoding
- Def: A run of pixel in an image (binary or grey-scale) is a sequence of adjacent pixel with the same grey level.
- A run is represented by 4 numbers
  1. Start Row  
  2. Start Col
  3. Grey-Scale
  4. Running-length
- Encoding methods
  1. Encode only **none-zero** no **wrapped around**
  2. Encode **zero & none-zero** no **wrapped around**
  3. Encode only **none-zero** and **wrapped around**
  4. Encode **zero & none-zero** and **wrapped around**
- Algorithm Steps (Method 1 Encode)
  ``` 
  Step 1: Scan L to R T to B 
          //no skip for method 2
  Step 2: skip zero until p(i,j)>0 
            grey-scale = p(i,j)
            output i, j, grey-scale
            count = 1,
  Step 3: j++
  Step 4: if p(i,j) == grey-scale
            count++;
  Step 5: Repeat 3 - 4 until p(i,j) != grey-scale
          // no j >= numCols for method 3
  Step 6: if p(i,j) != grey-scale || j >= numCols
            output count
          if j >= numCols
            i++,
            j = 0
  Step 7: Repeat 1 - 6 until all pixel processed.
  ```
- Algorithm Steps (Method 2 Decode)
  ```
    Step 1: Init image to 0 with header
    Step 2: read input line by line and 
            store value to row,col,grey-scale,count
    Step 3: p(i,j) = grey-scale
            count--,
            col++,
    Step 4: Repeat 3 until count = 0
    Step 5: Repeat 2 - 4 until end of file
  ```
---
## Chain Code
- Def: The chain code of an object(1's) is represented as follow: StartRow, StartCol, Grey-Scale,Direction...(0,1,2...7)
- Direction

| 3 |2  | 1 |
|:--|:--|:--|
| 4 |  | 0 |
| 5 | 6 | 7 |
- Contour of a single object with holes:
  1. Trace the outer border of the object in counter-clockwise direction
  2. For each hole in the object, trace the contour of the hole in clock-wise direction
- Algorithm Step(Single object without hole)
```
  Step 1: Init Image
          - startRow
          - startCol
          - label
          - currentP
          - nextP
          - lastZero
          - nextZero
  Step 2: Scan L to R, T to B
          if p(i,j) > 0
            - startRow = i
            - startCol = j
            - label = p(i,j)
            - currentP = (i,j)
            - lastZero = 4
            - nextZero = (lastZero + 1) % 8
            output startRow,startCol,label
  Step 3: chain = findNextP(currentP,nextZero,nextP)
  Step 4: currentP = nextP
          lastZero = DirectionTable(chain-1)
  Step 5: Repeat 3 - 4 until nextP = startPoint
```
- Direction Table

| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|:--|:--|:--|:--|:--|:--|:--|:--|
| 6 | 0 | 0 | 2 | 2 | 4 | 4 | 6 |
---
## Edge Detection
- 2 kinds of Edges
  - In grey-scale
    - where grey scale change dramatically
  - In binary
    - where bit change from 0 to 1 or 1 to 0
- why Edge detection in grey scale
  - Object recognition
  - Object boundary detection
- Edge detection operator
  - Robert's operator
  - Sobel's operator
  - Gradient's operator
  - zero-crossing
  - model
  - Facet
  - Canny's
  - other
- 1st and 2nd derivative
  - ...10 10 10 69 69 69 10 10 10...
  - ...0 0 0 -59 0 0 59 0 0 0... 1st
  - ...0 0 59 -59 0 -59 59 0 0... 2nd
- 2 X 2 Edge Operator (Robert's)
  - 4 Templates
  - p'(i,j) = p(i,j)\*1 + p(i,j)\*-1...
  
| +1 | -1 |
|:--|:--|
| +1 | -1 |


| +1 | +1 |
|:--|:--|
| -1 | -1 |


| +1 | -1 |
|:--|:--|
| -1 | +1 |


| -1 | +1 |
|:--|:--|
| +1 | -1 |
- Sobel's Operator 3 X 3 Edge

| +1 | +2 | +1 |
|:--|:--|:--|
| 0 | 0 | 0 |
| -1 | -2 | -1 |


| +1 | 0 | -1 |
|:--|:--|:--|
| +2 | 0 | -2 |
| +1 | 0 | -1 |


| 0 | +1 | +2 |
|:--|:--|:--|
| -1 | 0 | +1 |
| -2 | -1 | 0 |


| +2 | +1 | 0 |
|:--|:--|:--|
| +1 | 0 | -1 |
| 0 | -1 | -2 |

- Gradient Edge Detector
  - Measuring the grey scale distance of you and your neighbor

| x | a |
|:--|:--|
| b | c |

$$Edge(x) = \sqrt{(x-a)^2+(x-b)^2}$$
---
## Line Detection
- Hough Transform
  - does not record location of point, therefore we cannot reconstruct the image from Hough
- Equation
$$d = \sqrt{x^2+y^2} \times cos(t)$$
$$t = a - arctan(\frac{y}{x}) - \pi/2$$
- Algorithm Steps
```
  Step 0: Point list <- given
          Create and init Hough Array to zero
  Step 1: (x,y) <- get from pt list
  Step 2: angle = 0;
  Step 3: d = dist((x,y),angle)
  Step 4: HoughArray[angle][d]++
  Step 5: Angle++
  Step 6: Repeat 3 - 5 until angle >= 180
  Step 7: Repeat 1 - 7 until all pt processed
```
