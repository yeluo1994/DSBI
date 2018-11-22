# README
This repository provides the Double-Sided Braille Image Dataset (DSBI) descrided in 
*[DSBI: Double-Sided Braille Image Dataset and Algorithm Evaluation for Braille Dots Detection](https://github.com/yeluo1994/DSBI)* .
The repository contains :  
1. The 114 color double-sided Braille images from six different Braille books and six ordinary single printed Braille documents and their de-skewed images.
2. Braille recto dots, Braille verso dots  annotation information for each double-sided Braille image. The annotation information contains the skewed angle of the Braille dots, vertical and horizontal lines position and each Braille cell information with location and six dots.  

## Detailed Annotation Information
For each double-sided Braille image, we provide their corresponding Braille recto dots annotaion(with the postfix of "+recto.txt" ) and  Braille verso dots annotaion(with the postfix of "+verso.txt" ) .
For each annotation file, the specific conntent and format are  as follows:
* The first line is the skewed angle corresponding Braille dots. When the angle is positive, it indicates that the Braille dots skew in a clockwise direction. Otherwise,  they skew in an anticlockwise direction.
* The second line is the position of vertical lines. The number of vertical lines must be even according to the characteristic of Braille cell.
* The third line is the position of horizontal lines. The number of horizontal lines must be a multiple of 3 according to the characteristic of Braille cell.
* The following each line is one Braille cell annotation with 8 numbers. The first two numbers are the row and column number of Braille cell ( index starting from 1 not 0). The last six numbers are the Braille dots where 1 means a Braille dot and 0 means the background and their  corresponding relationship is shown below.  
<div align=center><img width="150" height="150" src="https://github.com/yeluo1994/DSBI/blob/master/figures/Braille%20dots%20arrangement%20within%20one%20Braille%20cell.jpg?raw=true"/></div> 

For each double-sided Braille image, we also provide their corresponding Braille de-skewed images for recto dots (with the postfix of "+recto.jpg" ) and  Braille deskewed images verso dots annotaion(with the postfix of "+verso.jpg" ). And you can also get these de-skewed images using the below function implemented in C++ and OpenCV . For the original image(denoed by src ) with the skewed angle (denoed by angle ) , you could get the de-skewed image(denoted by dst) by calling "imageRotate(src,dst,-angle)".
  ```
int imageRotate(InputArray src, OutputArray dst, double angle)
{
	/*
	rotate the src with specific angle. 
	src : source image you want to rotate
	dst : the rotated image. The dst size may be larger than src 
	angle : the angle you want to rotate, and the unit is degree. If angle>0,it will rotate the src in a clockwise direction. 
	*/

	Mat input = src.getMat();
	if (input.empty()) {
		return -1;
	} 
	int width = input.cols;
	int height = input.rows; 
	Point2f center;
	center.x = width / 2.0;
	center.y = height / 2.0;  
	double scale = 1.0;
	Mat trans_mat = getRotationMatrix2D(center, -angle, scale);
	//calculate new image size
	double angle_numeric = angle  * CV_PI / 180.;
	double a = sin(angle_numeric) * scale;
	double b = cos(angle_numeric) * scale;
	double out_width = height * fabs(a) + width * fabs(b);
	double out_height = width * fabs(a) + height * fabs(b);
	//add offset
	trans_mat.at<double>(0, 2) += cvRound((out_width - width) / 2);
	trans_mat.at<double>(1, 2) += cvRound((out_height - height) / 2); 
	warpAffine(input, dst, trans_mat, Size(out_width, out_height), 1, 0, Scalar(255, 255, 255));
	return 0;
}
```


## Others
Some Braille images may contain only recto dots or verso dots.If there are no Braille recto dots on one image, the corresponding recto dots annotation file will be empty, which is the same for verso dots.  

