# LSB Steganography
What is LSB?<br>
Least Significant bit also called RightMost Bit is a lowest bit of a binary number. For example in binary number 10010010, “0”is the least significant bit.
<br><br>
What is LSB-Steganography?<br>
LSB-Steganography is a steganography technique in which we hide messages inside an image by replacing Least significant bit of image with the bits of message to be hidden.
By modifying only the first most right bit of an image we can insert our secret message and it also make the picture unnoticeable, but if our message is too large it will start modifying the second right most bit and so on and an attacker can notice the changes in picture.
<br>(source : https://www.cybrary.it/0p3n/hide-secret-message-inside-image-using-lsb-steganography/ )
<br>So the main idea is changing just the last bit wont show a significant change in the rgb values, therefore the image will look similar, though it now contains the data.
![alt text](https://img.wonderhowto.com/img/02/61/63645877844452/0/steganography-hide-secret-data-inside-image-audio-file-seconds.w1456.jpg)
<br> <br>
Example:
https://medium.com/@98kartik.sharma/steganography-challenge-ez-331-points-e38d41315f64
