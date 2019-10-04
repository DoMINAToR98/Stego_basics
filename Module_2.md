# Types of Steganography
1.Textual<br>
2.Image<br>
3.Audio<br>
<h2>Textual Steganography</h2>
It hides the text behind some other files, changing the format of an existing text within a file, to change the words within the text or to generate random character sequences.
Basically here we use a text file as a cover media to embed the secret information. <br>It is more vulnerable to attack as it can be easy for an attacker to detect the pattern, text steganography its self is has this following three categories
such as:<br>
a. <b>Format Based Methods</b>, in this method text data is embedded in the carrier text by changing the format of the cover text itself. <br>
b. <b>Linguistic Methods</b>, in this method just doing analysis the linguistic. <br>
c. <b>Random and Statistical generation methods</b>, generating its carrier text according to the statistical and embedding the information in random sequence
of characters. <br>
Example:
<p align="center">
  <img src="https://i.imgur.com/5SoFF4i.png">
</p>
This appears as simple text but actually it is an example of <b>NULL CIPHER</b>.
<p align="center">
  <img src="https://i.imgur.com/hFdeBPy.png">
</p>
The original message can be seen now.
<br>
<h2>Image Steganography </h2>
As the name suggests, Image Steganography refers to the process of hiding data within an image file. 
The image selected for this purpose is called the cover-image and the image obtained after steganography is called the stego-image.
<br>Images are used as a message carriers. They are the most popular cover objects used for steganography.<br>
<br>Common approaches include:<br>
1.Least Significant Bit Insertion<br>
2.Masking and Filtering<br>
3.Redundant Pattern Encoding<br>
4.Encrypt and Scatter<br>
5.Coding and Cosine Transformation
<br><br>
Example:
<p align="center">
  <img src="https://i.imgur.com/rVuvziE.png">
</p>
The image above looks like random boring image but instead it contains a hidden message. There are only 2 pixel values used and which hints of binary using a python script gives us the image below.
<p align="center">
  <img src="https://i.imgur.com/2wcUx5Y.jpg">
</p>
Script Used: https://gist.github.com/DoMINAToR98/59c3cc9b80e1c733aaf253d94196db76
<br>
<h2>Audio Steganography</h2>
In audio steganography, the secret message is embedded into an audio signal which alters the binary sequence of the 
corresponding audio file. Hiding secret messages in digital sound is a much more difficult process when compared to
others, such as Image Steganography.
<br>
Example:<br>
Audio can be downloaded from https://mega.nz/#!mTBC1KjC!mke4qAYmMu0HDbAkQqGTUmmJk0kO2TjWPkGBZ7PgZK4 <br>
Listening to the audio, it looks like a simple song but it contains a message in it. <br>
Viewing the audio in Sonic Visualizer, we get the hidden message.
<p align="center">
  <img src="https://i.imgur.com/rHOhknF.png">
</p>

