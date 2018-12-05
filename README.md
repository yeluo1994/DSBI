# README
This repository provides the Double-Sided Braille Image Dataset (DSBI) descrided in 
*[DSBI: Double-Sided Braille Image Dataset and Algorithm Evaluation for Braille Dots Detection](https://arxiv.org/abs/1811.10893)* .
The repository contains :  
1. The 114 color double-sided Braille images from six different Braille books and six ordinary single printed Braille documents and their de-skewed images.
2. Braille recto dots, Braille verso dots  annotation information for each double-sided Braille image. The annotation information contains the skewed angle of the Braille dots, vertical and horizontal lines position and each Braille cell information with location and six dots.  

This project is supported and maintained by Institute of Computing Technology, Chinese Academy of Sciences.  Please feel free to email us via lirenqiang@ict.ac.cn or hliu@ict.ac.cn for more details.

## Braille Image Acquisition Way

We select flatbed scanner to obtain the double-sided Braille images, which is convenient and can provide good quality of Braille images. The scanner used in this paper is HP LaserJet Pro MFP M226dn. To reduce storing memory and remaining enough clarity, we use 200dpi resolution to capture color Braille images and store them in JPEG format.
##  Details of dataset DSBI
There are total 114 color double-sided Braille images from six different Braille books and six ordinary printed Braille documents. These Braille books include reference books, such as Massage, professional textbooks, such as middle school Chinese textbook, and novels, such as Shaver Yang Fengting.The detailed description of our constructed Braille image dataset DSBI is show below.

| Book name |  Total pages | Verso dots pages |Image quality|
| :------| :------: | :------: | :------: |
| 1.Massage | 20 |  1 | Bad |
| 2.Fundamentals of Massage | 20 | 1 | Normal|
| 3.The Second Volume of Ninth Grade Chinese Book 1 | 20 |  1 | Normal|
| 4.The Second Volume of Ninth Grade Chinese Book 2 | 10 | 1 | Normal|
| 5.Math | 32 | 0 | Normal|
| 6.Shaver Yang Fengting | 6 |  0 | Normal|
| 7.Ordinary printed document | 6 | 0 | Good |
| Total | 114 | 4 | N/A|

## Details of Annotation Information
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
##  Training Set and Test Set 
We divide our DSBI dataset into training set and test set. The total Braille images number of DSBI is 114, we select about 1/4 amount, 26 images, as training set and 3/4 amount, 88 images, as test set. Training images are from selected professional tool books,textbooks, novels and ordinary printed documents. Test images are from each captured Braille book. The specific statistical information of training set and test set is shown below.

| Book name |  Training set pages | Test set pages |
| :------| :------: | :------: |
| 1.Massage | 10 |  10 |
| 2.Fundamentals of Massage | 0 | 20 |
| 3.The Second Volume of Ninth Grade Chinese Book 1 | 0 |  20 |
| 4.The Second Volume of Ninth Grade Chinese Book 2 | 0 | 20 |
| 5.Math | 10 | 22 |
| 6.Shaver Yang Fengting | 3 |  3 |
| 7.Ordinary printed document | 3 | 3 |
| Total | 26 | 88 |

We provide the relative pathes of specific Training set files in  'train.txt ', and the Test set files in 'test.txt'.

## Citation
If you use DSBI in your work, please cite:

Renqiang Li, Hong Liu, Xiangdong Wan, Yueliang Qiang 2018 arXiv:1811.10893 [cs.CV]


## Others
Some Braille images may contain only recto dots or verso dots.If there are no Braille recto dots on one image, the corresponding recto dots annotation file will be empty, which is the same for verso dots.  

