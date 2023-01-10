# HomographyImageProjection

## Description
This is a python project using OpenCV methods to project an image into a specific frame of another image, through perspective transformation using the homography matrix. More specifically, we have two images from which the one appears into the other in a different perspecrive. The goal of this project is to detect the common points of the two images and calculate the corners in which the first image appears into the second. As far as this information is under our possession, we will be able to project a new image, with the same dimensions as the previous one, into the second image. The steps of this process are listed below:

1. Read the 2 images and store them in a list named 'imgs'. 'imgs[1]' contains 'imgs[0]'.
2. Use FAST detector to detect corners in the two images and SIFT detector to detect spots.
3. Use the SIFT object to access 'detectAndCompute()' function that will return the descriptors of the two images.
4. Use Brute-Force Matcher or Flann Matcher to match the two descriptors and calculate matches between the two images.
5. Find the homography matrix that transforms 'imgs[0]' so that it becomes exactly as it appears in 'imgs[1]', using 'cv2.fingHomography()' function.
6. Pass this matrix as a parameter in 'cv2.perspectiveTransform()' function that will return us the corners in which 'imgs[0]' is projected inside 'imgs[1]'.
7. Load the new image to be projected into 'imgs[1]', in the place where 'ímgs[0]' appears. This new image is 'proj_img'.
8. Define the corners of 'proj_img' and their correspondence inside 'imgs[1]'. This is basically the information we obtained in step number 6.
9. Pass these corners as parameters into function 'cv2.getPerspectiveTransform()' that returns a matrix that transforms 'proj_img' into the wanted one.
10. Pass this matrix as a parameter into 'cv2.warpPerspective()' function that will transform the image.
11. Create a black frame in 'imgs[1]', where 'proj_img' needs to be projected. This is accomplished by the following code:
'#transform proj_img
proj_img = cv2.warpPerspective(proj_img,m,(cols,rows)) 

black = np.zeros([1490, 1070, 3], np.uint8) #create a black image with the size of imgs[0]
black.fill(255) #make the image white

#transform the white image in the same way imgs[0] was transformed (using the same matrix m)
white_proj = cv2.warpPerspective(black,m,(cols,rows)) 

#invert the colors (make white black and opposite)
mask = cv2.bitwise_not(white_proj) 

#replace the white area with imgs[1]
masked_image = cv2.bitwise_and(imgs[1], mask) '
'masked_image' now holds an image just like 'imgs[1]' but with a black frame in the place where 'proj_img' needs to be projected.
12. Add images 'proj_img' and 'masked_image'
