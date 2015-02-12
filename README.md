# patch-based-digital-signature
Here is a brief description and a partial source code of my project, regarding digital signatures. 

-	Outline

The proposed algorithm called PBIS (patch-based image signature), included coding and decoding stages. First, the coding algorithm decomposed the secret message to its characters while the digital image was randomly initialized. Then for each character in the message, the corresponding patch in the codex (i.e. a set of randomly generated primary patches) was copied to a random position in the image. After encryption of some essential coding parameters, the whole image was distorted by additive noise. Finally, the digital image was reshaped to hide its original size from the user. In the suggested decoding algorithm, after decrypting the coding parameters, for each patch in the image, the corresponding character was specified using a fitness function. 


-	Detailed description 

We are assuming the message is a string of numbers. x and y represent the x -axis coordinate (horizontal axis) and y -axis coordinate (vertical axis) respectively. 

To hide the message from users, we use steganography methods, meaning we hide the message in an image. The image did not need to be meaningful so we created a noisy RGB image. 

Consider Hash tables in the sense that we represent a meaningful data by something random and meaningless. We aimed for something similar. So we created random n*n*3 matrixes to represent digits 1-9. Here we allocated 25 different matrixes for each digit, and each matrix was called a “patch” and a set of them was called “codex/codecs”. So each time we randomly chose a patch among 9*25 of them. These patches were shared between the sender and the receiver.  

We also defined 2 set of numbers (arrays), one for delta x and one for delta y. There were 5 members in each array. It was also essential for these arrays to be shared along with the codex. 

For coding the message into the image, we started by turning the image into an m*32*3 rectangle. The image then consisted of (m/32) 32*32 smaller rectangles. Each digit would be hidden in a small rectangle. For each rectangle, we also considered an origin ( (0, 0) point ) which was the bottom leftmost corner of it. The required coordinates for placing the codex were chosen according to these origins. For each digit, we needed randomly chosen x and y plus a member of each array. The combination of the indices and the digit gave us a specific patch. 

 While the first rectangle was saved for coding the information needed for decoding the image, with randomly chosen x and y for the second rectangle, we began replacing the image pixels with the chosen patch. 
 
Before choosing the next patch for subsequent digit, the random members of the arrays were added to the ulterior x and y. These new x and y were used for the starting point in the next small rectangle. This process continued till all digits were coded and hidden in the image.

Other than the to-be-transferred message, we hid other numbers such as the image size, the coordinates of the first starting point, replacement of the starting points in x and y axes according to their previous starting points, and the message length. We needed these numbers for the process of decoding and all of them need to be coded in the first 32*32*3 window. 
At the end, we reshaped the image into a square-shaped image, and sent it to the receiver side. 


-	Source code

This function gets the message and the image and it has two outputs: i) an image which hides the message in itself, ii) parameters required for decoding the image. 
