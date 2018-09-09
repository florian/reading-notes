# Exploring the structure of a real-time, arbitrary neural artistic stylization network [[pdf]](https://arxiv.org/pdf/1705.06830.pdf)

*Ghiasi, Golnaz, et al. "Exploring the structure of a real-time, arbitrary neural artistic stylization network." Proceedings of the 28th British Machine Vision Conference (BMVC) (2017)*

Style transfer is the task of taking two images and creating a new image which has the content of the first image but the style of the second.
Early solutions to this problem were based on pure optimization approaches where we start off with a random image and keep changing it to better reflect the content and style of the input images.
This paper proposes a more general solution in which we train a network to apply any style to any image using just one forward pass.

## 1. Introduction

### Earlier work

- Early approaches for style transfer based on conventional computer vision methods were extremely expensive computationally
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

### Optimization process for each style and content image

- We get two input images, a style and a content image. The goal is to create a third image which has the content of the first image but the style of the second
- To compare the style and content of the images, an image recognition system is used
- The content is similar if the *high-level* features extracted by the recognition system are similar
- The style is similar if the *low-level* features extracted by the recognition system are similar
- To compare features, the Euclidian distance is used
- The full loss is a weighted average of content loss and style loss
- Lower-level features are detected by lower layers, higher-level features by higher layers
- To compute the content loss, the activations of these layers are directly compared
- For the style loss, Gram matrices of the activations are compared. The Gram matrix of a matrix is computed by multiplying the transpose with the matrix itself. This lets us find correlations in the activations

### One neural network for each style

- By minimizing the loss above, we can generate the image for any two given input images. However, this always requires optimizing from scratch
- It is preferable to train a neural network that can directly compute the output image
- Idea from previous work: Train a network, called the *style transfer network*, which does this for one given style. Instead of optimizing the output, we now optimize the weights of the network. The loss is then a function of the network output and the two input images
- This allows us to render a new image much faster but a separate network needs to be trained for each style
- This is wasteful because all of these networks share a lot of logic

### A single neural network for all known styles

- Styles are embedded in a lower-dimensional space using a standard autoencoder architecture
- Most parameters are the same for all inputs (the styles). Only the normalization parameters for *conditional [instance normalization](https://arxiv.org/abs/1607.08022)* are optimized for each style individually. These are only 0.2% of all the parameters (2758 weights)
- The set of all these normalization parameters is the embedded representation we use for a style
- The style transfer network now takes as input the content image and the embedding of the style
- This approach is nicer because we reuse most of the weights directly related to the style
- The style representation space is useful: For example, we could mix two styles by averaging their embeddings
- However, we still need to learn new weights when a completely new style is supposed to be used

### A single neural network that generalizes to new styles

- To fix this, the *style prediction network* is introduced
- It takes a style as input and predicts its embedding
- The embedding is then passed to the style transfer network
- The style prediction network is initialized using a pretrained Inception-v3 architecture
- Both networks are then trained end-to-end
- After the training, it is possible to use any content and style images as input
- There has been [parallel work](https://arxiv.org/abs/1703.06868) for producing an embedding without retraining. However, that paper is just based on heuristics, while the approach above directly learns a representation

## 3. Results

- Dataset:
  - Content images from ImageNet
  - Styles from Kaggle's [Paint By Numbers](https://www.kaggle.com/c/painter-by-numbers) and [Describable Textures Dataset](https://www.robots.ox.ac.uk/~vgg/data/dtd/)
  - Augmented by applying image transformations
- Unlike in previous work, the same content and image loss weighting can be used for all styles
- Training on a lot of styles was crucial for generalization
- Similarly styled images are close to each other in the latent space. Proximity in this space also reflects semantic similarity

## 4. Conclusions

- This approach to style transfer generalizes well to new styles. It works especially well if the new style is close to the styles that were trained on
- The low-dimensional embedding captures properties of the paintings
- Future work:
  - Enforce consistency between frames in a video
  - Learn an embedding based on semantic information about the style
- More generally, style transfer could be useful in situations where data is limited and previous knowledge should be transferred
