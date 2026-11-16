In practice, the number of weights in a NN grows rapidly, making training difficult. Many architectures use explicit **inductive biases**, which are assumptions a learning algorithm relies on to generalize on unseen examples. It constrains or shapes the range of possible predictions to help the model select among multiple plausible solutions that may have a similar level of fit to the data. Without such bias, a model cannot reliably generalize beyond its training examples.

For example, logistic regression exhibits strong inductive bias because it assumes that the input-output relationship can be modelled by a linear function, whereas high-degree polynomials impose a weaker assumption. **Convolutional Neural Networks (CNNs)** are a class of NNs characterized by strong inductive bias. They are the backbone of modern vision systems, including autonomous vehicles. Consider the following example:
- An RGB image with dimensions 224 x 224, relatively small
- This means 224 x 224 x 3 = 150,528 input features
- A NN with a single hidden dense layer would require over 22 billion parameters.
In a fully connected feedforward NN with $k$ layers, if each layer contains $n$ units (1 per input feature), the number of parameters scales as $O(kn^2)$.

To understand CNNs, we first start with classical 2D image processing, then look at 1D neural networks to understand the mechanics, then finally 2D neural networks.
### Classical Image Processing
![[Pasted image 20261116000148.png]]
A **kernel** (filter) is a small matrix, usually 3 x 3 or 5 x 5, that slides over the input data (an image) to perform cross-correlation. An image is a 2D array of numbers. In this image, a black pixel is represented by 0, and a white pixel by 255. Together, the pixels form the letter O. 

Here, the kernel is a 2 x 2 matrix, with each entry corresponding to a value. Starting from the upper left corner, we slide over the image one column at a time. At the end of the row, we start the process again one row down. With a 4 x 4 image and a 2 x 2 kernel, there are 3 x 3 = 9 valid placements. Only positions where the kernels overlap over the input are used. Boundaries are handled later.

![[Pasted image 20261116000521.png]]
At each kernel placement, multiply each pixel by the kernel entry directly above it. Then, add up the products to give one output number in the response map (or **feature map**). In this example, the kernel is an edge detector. The right edge of the O shows a transition from dark to bright and gives positive responses. The left edge shows a transition from bright to dark, leading to negative responses. The central column shows no edge change and gives zero. The sign therefore records direction of the change, not just the presence of an edge.

This operation uses the kernel as written, which is called **cross-correlation**. **Convolution** in a mathematical sense flips the kernel along both axes first. ML libraries usually compute cross-correlation but refer to it as convolution by convention. Since this kernel is asymmetric, flipping it would reverse the sign of its response.

In classical image processing, the output is called a **feature map**. In a CNN, it is called a **feature map**.
#### Edge Detection
The Sobel $x$ kernel ($K_x$) estimates how fast image intensity changes across columns. A strong left-to-right change indicates a vertical edge. The sign of the response indicates whether it is a dark-to-light boundary or light-to-dark.

Transposing the Sobel $x$ kernel gives the Sobel $y$ kernel. It does the same, but for horizontal edges row-by-row.
### Central Computational Idea
