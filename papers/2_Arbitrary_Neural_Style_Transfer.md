# Exploring the structure of a real-time, arbitrary neural artistic stylization network [[pdf]](https://arxiv.org/pdf/1705.06830.pdf)

*Ghiasi, Golnaz, et al. "Exploring the structure of a real-time, arbitrary neural artistic stylization network." Proceedings of the 28th British Machine Vision Conference (BMVC) (2017)*

Style transfer is the task of taking two images and creating a new image which has the content of the first image but the style of the second.
Early solutions to this problem were based on pure optimization approaches where we start off with a random image and keep changing it to better reflect the content and style of the input images.
This paper proposes a more general solution in which we train a network to apply any style to any image using just one forward pass.

## 1. Introduction

### Earlier work

- Early approaches for style transfer were extremely expensive computationally
- *Style transfer networks* were then proposed, which are networks that learn the transformation for a specific content image and specific style
- This approach is more computationally feasible but much less general since we work with all styles / content independently
- No shared representation is learned this way
- There have been approaches that aim to train one network that is able to handle different styles. But so far, these networks generalize badly to new styles

### This paper

- A single network is trained which is supposed to work for any content and any style
- Trained on 80,000 images and 6,000 styles
- The resulting network generalizes well
- To apply a style, the images only need to be send through the network
- Styles are embedded in a space that can be analyzed

## 2. Methods

### Loss

- We get two input images. The goal is to create a third image which has the content of the first image but the style of the second
- To compare the style and content of the images, an image recognition system is used
- The content is similar if the *high-level* features extracted by the recognition system are similar
- The style is similar if the *low-level* features extracted by the recognition system are similar
- To compare features, the Euclidian distance is used
- The full loss is a weighted average of content loss and style loss
- Lower-level features are detected by lower layers, higher-level features by higher layers
- To compute the content loss, the activations of these layers are directly compared
- For the style loss, Gram matrices of the activations are compared. The Gram matrix of a matrix is computed by multiplying the transpose with the matrix itself. This lets us find correlations in the activations

### Optimization architecture

- By minimizing the loss above, we can generate the image for any two given input images. However, this always requires optimizing from scratch
- It is preferable to train a neural network that can directly compute the output image
- Idea from previous work: Train a network that does this for one given style. Instead of optimizing the output, we now optimize the weights of the network. The loss is then a function of the network output and the two input images
- This allows us to render a new image much faster but a separate network needs to be trained for each style
- This is wasteful because all of these networks share a lot of logic


## 3. Results

## 4. Conclusions
