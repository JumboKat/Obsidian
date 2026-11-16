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
### Computational Basis
In image processing, the kernel values are manually specified. CNNs learn their kernels and biases automatically during training.
#### 1D Data
One-dimensional data are organized along one ordered axis. This axis can be time, position in a text, or distance along a route. Neighbouring points are usually related, so ordering matters.

When dealing with 1D data, the inductive bias (assumptions) include:
- Nearby measurements form meaningful local patterns.
- A useful pattern may occur at different positions.
- The same detector should be reused everywhere.

Where a dense layer lets every output depend on every input, a convolutional layer says only local neighbourhoods matter, which creates **sparse interactions** (one output activation depends on a short input window rather than the complete sequence). It also makes use of **parameter sharing**, where the same kernel weights are reused at every window position. These two restraints reduce the total number of parameters, and ensures that the feature map responds consistently when the same local patterns appear in different places.
#### Parameter Reduction
![[Pasted image 20261116121609.png]]
Moving the window across every valid position fills the output vector (feature map). Each position records how strongly the learned local pattern is there. Each output value uses only five neighbouring input values (local connectivity), reusing the same five weights for all 124 valid positions (parameter sharing). For comparison, a dense mapping from 128 input values to 124 output values would require 128 x 124 = 15,872 distinct weights. This convolution uses only 5 weights.

This parameter reduction does not come from discarding input positions, but by applying the same local rule everywhere. 
#### Translation Equivariance
![[Pasted image 20261116122016.png]]**Translation invariance** means that the output does not change when the input shifts. A classifier still assigns "cat present" to all three images above. By contrast, a convolutional feature map shifts with the input. This is represented by the following equation:
$$F(T_{\Delta}X)=T_{\Delta}F(X)$$
$T_{\Delta}$ is the translation operator, shifting every position by $\Delta$. For every translation in the input, the feature map is identically translated, which is known as **translation equivariance**. Padding and boundaries can break exact equivariance near edges. A stride-S convolution is only equivariant to a shifts in multiples of S. 
#### Padding
Every layer of cross-correlation shrinks the output from the input: $L_{out}=L-K+1$ (K is the kernel length). Every layer stacked upon it shrinks it further. This also means the edge positions of the input are used by fewer windows than the middle ones.

Padding fixes both issues by adding `P_left` values to the start and `P_right` to the end of the input before running cross-correlation. With padding values of 2 on each side, the output and input match in length, meaning that every score in the output aligns with every original input position. This is called **same padding**. In general, for an odd kernel with stride 1:
$$P_{left} = P_{right} = \frac{K-1}{2}$$
This allows the centre of the kernel to align with every original input position. Padding does not necessarily have to be balanced. It also creates new artificial values, which alters the output.
- **Zero padding** makes all padded values equal to zero.
- **Reflection padding** mirrors values near the boundary.
- **Replication padding** repeats the edge value.
- **Circular padding**connects the end of the sequence to the start.
While padding only occurs at the boundary in one layer, it can propagate in a deep network. 
#### Stride
**Stride** is the distance between kernel placements. With a stride S = 2, the number of outputs is halved; it does not half the values themselves. This is downsampling; saves on computation, but reduces resolution.
#### Multiple Kernels
We can model multiple local patterns with multiple kernels. Take two kernels as an example:
$$w^{(0)}=[-1, -1, 0, 1, 1] \\
w^{(1)}=[1, 1, 0,-1,-1]$$
The first kernel gives a positive score if the last pair exceeds the first pair. The second kernel is its negation. Each kernel produces its out feature map/output channel. Stacked as columns, they form a weight matrix W ∈ R^(K × C_out), which is of the shape 5 x 2 (5 kernel values x 2 feature maps).

Too few kernels is a representational bottleneck. Too many wastes resources and can cause overfitting.
#### Multiple Channels
![[Pasted image 20261116125329.png]]
A kernel slides along the sequence dimension but spans all channels simultaneously. If there are three channels (e.g. x, y, z) and five kernel values, one output uses a 5x3 patch. This means 15 products produces one scalar in the feature map. 
#### Stacking Layers
![[Pasted image 20261116125744.png]]
One layer's output is the next layer's input. This is **hierarchical composition**: one layer describes local contrasts, the next describes a pattern made from those contrasts; deeper layers learn "patterns of patterns." 
#### Bias
There's one bias per output channel, shared across all positions. it shifts the whole feature map and, with ReLU, sets the threshold for activation. Equivariance is preserved since bias can't encode where a pattern occurs.
#### Receptive Field
![[Pasted image 20261116130306.png]]
A **patch** or **window** is the slice of a layer's current input used at one kernel placement. A unit's **receptive field** is the set of original-input positions that can influence it, however many layers deep. In layer 1, they are the same thing, since layer 1's input is simply the original input. The receptive field increases the deeper the layer.

Take the example above, with two layers each with kernel size K = 5 and stride 1. In layer 1, both the patch size and receptive field = 5. In layer 2, patch size is still 5, but each input is already summarizing 5 original samples, though they overlap heavily. Starting from the first window in layer 1, each consecutive window just adds one new sample at the right edge. The formulas for receptive field size are:$$R_1=K_1 \\ R_2=R_1+(K_2-1)$$
Each additional layer adds K-1 to the receptive field; meaning receptive field grows linearly with depth. In this example, $R_2=5+5-1=9$.
#### Pooling
**Pooling** summarizes nearby activations without learning another set of weights. It applies a fixed rule (max or mean) to each small window of a feature map. It is essentially convolution, except replace the computation with simply taking the max (**max pooling**) or mean (**mean pooling**). 

![[Pasted image 20261116131326.png]]
Example: With pool size 2, stride 2, every adjacent pair is replaced by its max, so 12 activations become 6. 

Pooling is used to:
- Reduce sequence length to reduce storage and computation in deeper layers.
- Give later layers a larger receptive field.
- Give tolerance to small translations (robustness to small changes).

It is still limited in its function, however. It does not make the network translation invariant (only slightly), and it does not prevent overfitting.