## README
This repository provides the Double-Sided Braille Image Dataset (DSBI) descrided in 
*[DSBI: Double-Sided Braille Image Dataset and Algorithm Evaluation for Braille Dots Detection](https://github.com/yeluo1994/DSBI)* .
The repository contains :  
1. The 114 color double-sided Braille images from six different Braille books and six ordinary single printed Braille documents.
2. Braille recto dots, Braille verso dots  annotation information for each double-sided Braille image. The annotation information contains the skewed angle of the Braille dots, vertical and horizontal lines position and each Braille cell information with location and six dots.  

## Detailed Annotation Information
For each double-sided Braille image, we provide corresponding Braille recto dots annotaion(with the postfix of "+recto.txt" ) and  Braille verso dots annotaion(with the postfix of "+verso.txt" ) .
For each annotation file, the specific conntent and format are  as follows:
* The first line is the skewed angle corresponding Braille dots. When the angle is positive, it indicates that the Braille dots skew in a clockwise direction. Otherwise,  they skew in an anticlockwise direction.
* The second line is the position of vertical lines. The number of vertical lines must be even according to the characteristic of Braille cell.
* The third line is the position of horizontal lines. The number of horizontal lines must be a multiple of 3 according to the characteristic of Braille cell.
* The following each line is one Braille cell annotation with 8 numbers. The first two numbers are the row and column number of Braille cell. The last six numbers are the Braille dots where 1 means a Braille dot and 0 means the background.   
