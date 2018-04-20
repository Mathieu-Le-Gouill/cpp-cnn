I'm at work and there's is nothing to do. So I'm going to write a CNN.. in C++ .. from scratch..

.. becuase that's what people do when they're bored out of their minds

So, my initial thoughts are:

1. I need some linear algebra library to make the vector stuff fast. I've worked with Eigen in the past, and mlpack uses Armadillo so I'm familiar with that too. I don't want to go through the hassle of installing Eigen (which may not be too much of a hassle, but still..) so I'm going to use Armadillo.

2. I'll need a class for the Convolution Layer, a class for the Pooling layer and a class for the dense layer at the end

3. Like all good boys who write CNNs, I think it would be best to test this on the MNIST dataset -- so we'll need code to parse that too. Since the data is stored in binary form, this might be unpleasant.. Oh well, I'll handle it when I get to it.

Let's begin right in the middle of everything - with the Convolution Layer.

So we've hit the ground running with the ConvolutionalLayer constructor -- It initializes stuff like dimensions of input volume, number of filters, stride and filter dimensions. It also initializes the filter weights. I've decided to use a truncated normal initialization (Ie. random values sampled from a Gaussian distribution having mean 0 and variance 1. Values more than two standard deviations away from the mean are rejected).

Cool cool .. the weight initializations look correct. Time for a git commit and then we move on to the forward pass through the conv layer.

I'm going to go for the simplest kind of convolution implementation there is -- no padding, no FFT.

I've realized that filters (in general) have a depth dimension as well, and I've defined filters as 2D matrices.. Need to fix that.

Done fixing the filter dimensions. Commit and move on to the forward pass.

Update: I went for a snack break. My boss told me to do a few things -- which I mostly ignored, because he's an idiot. And I just finished the forward pass implementation. It feels weird working on this in the office, so I'm going to head home and continue from there. I plan to test the forward pass implementation first and then try and figure out how the backward pass is going to go.

A few more notes on the rest of the implementation:
1. The optimizer -- for now, I'll just use vanilla mini-batch SGD  to train. Maybe later I'll switch it up to Adam or RMSprop.

2. I realize that I'll need to add layers for ReLU activation in the hidden units, and a softmax layer at the very end of the network.

3. I'll probably add a class like "class LeNet" that contains the entire CNN architecture. I dont really plan on reusing any of the layers, so its fine if they're a bit dirty.

3 minutes till my cab arrives .. better head down.

Okay, I didn't really do much at home yesterday, and today I was a bit busy doing pointless things at work. I found some time now to work on this, and have completed the backward pass. Actually, I'd been thinking about the math of the backward pass through the conv layer today.. and I was quite surprised at how easily it worked out to nice expressions. Hopefully I'll get some time to write a blog post about it -- I think I actually found a nice method to it as well. Anyway, I'd scribbled down
most of it in my little notebook at work and the implementation wasn't too hard.

I've still got the forward pass testing in the backlog. Now, I can add the backward pass testing as well. I've added a function stub for gradient check as well in the CovolutionLayer class. I think I'll make 100% sure that theres nothing wring with my Conv layer before proceeding with the other components (dense layer, max pooling, relu and sigmoid). Hopefully tomorrow I'll get time for testing and then finish the CNN over the weekend.

Oh yeah, a minor note -- I'd not differentiated between the strides in the vertical and horizontal directions. Updated this.

I've been giving a bit of thought to writing proper tests -- test driven development and all that. I've decided to go with the boost.Test framework (which also happens to be used by mlpack).
