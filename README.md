# Generative Adversarial Network for MNIST using Keras

Generative adversarial networks are a class of artificial intelligence algorithms used in unsupervised machine learning, implemented by a system of two neural networks contesting with each other.

## How Generative Adversarial Networks work

One neural network, called the generator, generates new data instances, while the other, the discriminator, evaluates them for authenticity; i.e. the discriminator decides whether each instance of data it reviews belongs to the actual training dataset or not.

We’re going to generate hand-written numerals like those found in the MNIST dataset, which is taken from the real world. The goal of the discriminator, when shown an instance from the true MNIST dataset, is to recognize them as authentic.

Meanwhile, the generator is creating new images that it passes to the discriminator. It does so in the hopes that they, too, will be deemed authentic, even though they are fake. The goal of the generator is to generate passable hand-written digits, to lie without being caught. The goal of the discriminator is to identify images coming from the generator as fake.

### A Generative Adversarial Network takes the next steps:

1. The generator takes in random numbers and returns an image.

2. This generated image is fed into the discriminator alongside a stream of images taken from the actual dataset.

3. The discriminator takes in both real and fake images and returns probabilities, a number between 0 and 1, with 1 representing a prediction of authenticity and 0 representing fake.

So we have a double feedback loop.

### Double feedback loop:

1. The discriminator is in a feedback loop with the ground truth of the images, which we know.

2. The generator is in a feedback loop with the discriminator.

The discriminator network is a standard convolutional network that can categorize the images fed to it, a binomial classifier labeling images as real or fake. The generator is an inverse convolutional network, in a sense: while a standard convolutional classifier takes an image and downsamples it to produce a probability, the generator takes a vector of random noise and upsamples it to an image. The first throws away data through downsampling techniques like maxpooling, and the second generates new data.

Both networks are trying to optimize a different and opposing objective function, or loss function, in a zero-sum game. As the discriminator changes its behavior, so does the generator, and vice versa. Their losses push against each other.

![img](https://skymind.ai/images/wiki/GANs.png)

## Tips in training a Generative Adversarial Network

1. When you train the discriminator, hold the generator values constant; and when you train the generator, hold the discriminator constant. Each should train against a static adversary. 

2. Pretraining the discriminator against MNIST before you start training the generator will establish a clearer gradient.

3. Each side of the GAN can overpower the other. If the discriminator is too good, it will return values so close to 0 or 1 that the generator will struggle to read the gradient. If the generator is too good, it will persistently exploit weaknesses in the discriminator that lead to false negatives. This may be mitigated by the networks’ respective learning rates.

## Possible problems

1. **Problem:** Generated images look like noise. **Solution:** Use dropout on both Discriminator and Generator. Low dropout values (0.3 to 0.6) generate more realistic images.

2. **Problem:** Discriminator loss converges rapidly to zero thus preventing the Generator from learning. **Solution**: Do not pre-train the Discriminator. Instead make its learning rate bigger than the Adversarial model learning rate. Use a different training noise sample for the Generator.

3. **Problem:** Generator images still look like noise. **Solution:** Check if the activation, batch normalization and dropout are applied in the correct sequence.

4. **Problem:** Figuring out the correct training/model parameters. **Solution:** Start with some known working values from published papers and codes and adjust one parameter at a time. Before training for 2000 or more steps, observe the effect of parameter value adjustment at about 500 or 1000 steps.
---

Sources: [skymind.ai](https://skymind.ai/wiki/generative-adversarial-network-gan), [towardsdatascience.com](https://towardsdatascience.com/gan-by-example-using-keras-on-tensorflow-backend-1a6d515a60d0?gi=9c5db36e5ddb) 
