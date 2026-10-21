# Anatomy of a Training Loop

A training loop teaches a model by showing it examples, measuring its mistakes, and adjusting its parameters to improve its predictions. In this project, the CNN learns to classify CIFAR-10 images into ten categories.

## 1. Forward pass

The input images go through the model with `logits = model(images)`. The convolution layers learn visual patterns, and the last layer returns ten scores (logits) for each image. The highest score is the predicted class. The model has made predictions at this point, but its weights have not been updated yet.

## 2. Loss

The loss function compares the logits with the correct labels. I use `nn.CrossEntropyLoss()` because this is a multi-class classification task. A lower loss generally means the model is assigning more confidence to the correct classes. The loss gives the training process a value to improve.

## 3. Backward pass

Before calculating gradients for a new batch, `optimizer.zero_grad()` clears the gradients left from the previous batch. Then `loss.backward()` calculates how each trainable parameter contributed to the loss using backpropagation and the chain rule. These gradients indicate how the parameters could be changed to reduce the loss.

## 4. Optimizer step

`optimizer.step()` updates the model parameters using the gradients and the optimizer's learning rate. The model then processes the next batch with updated weights. Repeating these steps across batches and epochs helps it learn useful image features.

## Why normalization helps

After `ToTensor()`, pixel values are scaled to 0–1. Normalization subtracts a mean and divides by a standard deviation for each colour channel. This puts the input channels on a more consistent scale, which can make optimization more stable and help the model train effectively. I apply the same normalization values to training, validation, and test images so they are processed consistently.

## In short

The forward pass makes predictions, the loss measures the error, the backward pass calculates gradients, and the optimizer step updates the weights. Normalization helps keep the inputs on a useful scale. Together, these stages form the core loop used to train the CIFAR-10 CNN.
