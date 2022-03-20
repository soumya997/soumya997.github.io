---
layout: post
title: Pytorch Training Loop Explained [mini tutorial]
subtitle: I always had doubts regarding few pieces of code used in the training loop, but it actually make more sence when you think of forward and backward pass.  
cover-img: /assets/img/kaggle-header@2x.png
thumbnail-img: https://i.imgur.com/cEWVlgv.png
share-img: https://i.imgur.com/cEWVlgv.png
tags: [pytorch, note, mini, deep learning]
---

## Pytorch Training Loop Explained 

<p align="center">
<img src="https://i.imgur.com/cEWVlgv.png">
</p>

This there things are part of backpropagation, after doing forward pass by doing `model(x_input)` we need to calculate the loss for each back and update the parameters based on the derivatives. Doing `loss.backward()` helps to calculate the derivatives/gradients and `optim.step()` goes backward and update all the parameters. And we mainly use `optim.zero_grad()` to get rid of gradient accumulation problem, we prefer to claculate the gradients of each bach seperately. looking into the code might help. General Deep Learning pipeline looks like this,

```python
epochs = 5

for e in range(epochs):
    train_loss = 0.0
    for data, labels in tqdm(trainloader):
        # Transfer Data to GPU if available
        if torch.cuda.is_available():
            data, labels = data.cuda(), labels.cuda()
        
        # Clear the gradients
        optimizer.zero_grad()
        # Forward Pass
        target = model(data)
        # Find the Loss
        loss = criterion(target,labels)
        # Calculate gradients 
        loss.backward()
        # Update Weights
        optimizer.step()
        # Calculate Loss
        train_loss += loss.item()
    
    print(f'Epoch {e+1} \t\t Training Loss: {train_loss / len(trainloader)}')
```

If you see the last three lines of the pipeline, it first performs `optim.zero_grad()` then `loss.backward()` and `optim.step()`. So, why is that?

- **But first talk about `loss.backward()` and `optim.step()`:**


 > `loss.backward()` sets the grad attribute of all tensors with `requires_grad=True` in the computational graph of which loss is the leaf (only x in this case). 
 
 > Optimizer just iterates through the list of parameters (tensors) it received on initialization and everywhere where a tensor has `requires_grad=True`, it subtracts the value of its gradient stored in its `.grad` property (simply multiplied by the learning rate in case of SGD). It doesn't need to know with respect to what loss the gradients were computed it just wants to access that `.grad` property so it can do `x = x - lr * x.grad`. 
 
 > When you call `loss.backward()`, all it does is compute gradient of loss w.r.t all the parameters in loss that have `requires_grad = True` and store them in `parameter.grad` 
 
 > attribute for every parameter.  `optimizer.step()` updates all the parameters based on `parameter.grad`.


 
 ```python
  # Our "model"
  x = torch.tensor([1., 2.], requires_grad=True)
  y = 100*x

  # Compute loss
  loss = y.sum()

  # Compute gradients of the parameters w.r.t. the loss
  print(x.grad)     # None
  loss.backward()      
  print(x.grad)     # tensor([100., 100.])

  # MOdify the parameters by subtracting the gradient
  optim = torch.optim.SGD([x], lr=0.001)
  print(x)        # tensor([1., 2.], requires_grad=True)
  optim.step()
  print(x)        # tensor([0.9000, 1.9000], requires_grad=True)
 ```
 
- **What is `optim.zero_grad()`:**
  > Note that if we were doing this in a train loop we would call `optim.zero_grad()` because in each train step we want to compute new gradients - we don't care 
  > about gradients from the previous batch. Not zeroing grads would lead to gradient accumulation across batches. Apparently we dont have to put `optim.zero_grad()` at the first all the time. As you have already understood its used to clear the gradients for each batch. we we can also use this after calculating the gradients.


- **Use of `loss.item()`:**
  > `.item()` moves the data to CPU. It converts the value into a plain python number. And plain python number can only live on the CPU. But you can also try using loss directly. but this is a better option.
  ```python
  x = torch.tensor([1.0])
  x.item()
  1.0
  ```
  
- **Why we are doing `train_loss / len(trainloader)`:**
  > The average of the batch losses will give you an estimate of the “epoch loss” during training.
  > Since you are calculating the loss anyway, you could just sum it [`train_loss += loss.item()`] and calculate the mean [`train_loss / len(trainloader)` => `sum of loss/no of loss`] after the epoch finishes.
  > This training loss is used to see, how well your model performs on the training dataset.
  > Alternatively you could also plot the batch loss values, but this is usually not necessary and will give you a lot of outputs.


### References:
- [https://stackoverflow.com/a/63651323/12568833](https://stackoverflow.com/a/63651323/12568833)
- [What does optimizer step do in pytorch](https://www.projectpro.io/recipes/what-does-optimizer-step-do)
- [Training Neural Networks with Validation using PyTorch](https://www.geeksforgeeks.org/training-neural-networks-with-validation-using-pytorch/)

