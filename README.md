# PixelCNN

PixelCNN is an autoregressive image generation model published in 2016 by Aäron van den Oord et al.,  
from the DeepMind team at Google. It belongs to the family of autoregressive models, where each element of a sequence  
is generated based on the previous elements.

Images are generated pixel by pixel following a defined order (typically from left to right and top to bottom).  
Each pixel is conditioned only on the pixels that have already been generated, which imposes a directional dependency constraint.  
To ensure this constraint, PixelCNN uses **masked convolution layers**, which prevent access to "future" pixels  
when making predictions.

Two types of masked convolutions are used:  
- **Mask type A**: Prevents access to the current pixel being generated, useful in the first layer to ensure  
  that each pixel is generated independently of the current values.  
- **Mask type B**: Allows access to the current pixel being generated, used in the following layers to allow  
  better information flow while respecting the generation order.

After this convolution, the model produces a **discrete distribution** over the possible pixel values,  
and its value is then sampled according to this distribution.

To improve the model's ability to capture long-range relationships in the image, **residual blocks**  
have been introduced, adding direct connections between deep and shallow layers.

During training, PixelCNN minimizes the **cross-entropy** between the predicted pixels and the actual pixels.

During inference, the image is generated sequentially:  
1. A blank or noisy image is initialized.  
2. For each pixel, the model predicts a probability distribution over its possible values.  
3. A sample is drawn from this distribution, and the pixel is set.  
4. This process is repeated until the entire image is generated.
