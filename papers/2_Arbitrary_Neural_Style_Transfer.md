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

## 3. Results

## 4. Conclusions
