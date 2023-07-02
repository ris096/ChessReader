# Chess Board Recognition

This repository contains code for identifying the chessboard position of the board present in an image. It generates random chess board positions, captures screenshots of the positions, preprocesses the captured images, and trains a neural network model to recognize the pieces on the board. The trained model can then be used to make predictions on new chess board images.

## Dataset Generation

To train a neural network model which reads the chessboard position of an image, we need a dataset of several images of random boards marked with the position present in these images. The standard notation to describe a chessboard is known as FEN notation. 

The first step to creating a database is making a random chess position generator. The random generator used in this projects returns the FEN position of a random chessboard, along with a simplified notation which is then used as the image label and saved as the image name.

After writing the random board generator, a script is used to automate the image generation. For this purpose, Selenium Chromedriver is used. This tool takes the random chessboard generator and uses the output random FEN to generate a corresponding image on lichess.com and takes its screenshot. The screenshot is then cropped to contain only the chessboard.

Post the screenshot generation, all the images are then sliced into 64 squares - one image per chessboard square - and the squares are saved separately along with their piece marking, which is the target labels for the model.

## Model Building

A neural network is trained on the generated dataset. The model contains the following layers:
* Layer 1: Convolutional Layer with 32 filters and a 3x3 kernel size, ReLU activation
* Layer 2: Max Pooling Layer with a 2x2 pool size
* Layer 3: Densely connected layer with 64 units, ReLU activation
* Layer 4: Densely connected layer with 13 units, softmax activation

After training the model and evaluating it on the validation dataset, it's performance accuracy was close to ~100%
To further check the model for overfitting or other errors, an additional dataset is generated which the model hasn't seen before. On that additional dataset, the model performs with a simmilar accuracy of ~100%

Finally, the code outputs the FEN position for a test image, and provides link to continue playing from the position as given in the test image from the point of view of both black and white sides.
