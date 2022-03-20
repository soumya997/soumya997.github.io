---
layout: post
title: What are optim.step(), loss.backward() and optim.zero_grad() [mini tutorial]
subtitle: I always had doubts regarding these pieces of code, but a simple search made it more clear  
cover-img: /assets/img/kaggle_banner.png
thumbnail-img: https://i.imgur.com/cEWVlgv.png
share-img: https://i.imgur.com/cEWVlgv.png
tags: [pytorch, note, mini, deep learning]
---

## What are optim.step(), loss.backward() and optim.zero_grad() [mini tutorial]

<p align="center">
<img src="https://i.imgur.com/cEWVlgv.png">
</p>

This there things are part of backpropagation, after doing forward pass by doing `model(x_input)` we need to calculate the loss for each back and update the parameters based on the derivatives. Doing `loss.backward()` helps to calculate the derivatives/gradients and `optim.step()` goes backward and update all the parameters. And we mainly use `optim.zero_grad()` to get rid of gradient accumulation problem, we prefer to claculate the gradients of each bach seperately. looking into the code might help. General Deep Learning pipeline looks like this,

```python
# Import library
import torch

# Define parameters
batch, dim_in, dim_h, dim_out = 128, 2000, 200, 20 

# Create Random tensors
input_X = torch.randn(batch, dim_in)
output_Y = torch.randn(batch, dim_out)

# Define model and loss function
Adam_model = torch.nn.Sequential(
    torch.nn.Linear(dim_in, dim_h),
    torch.nn.ReLU(),
    torch.nn.Linear(dim_h, dim_out),
)

loss_fn = torch.nn.MSELoss(reduction='sum')

# Define learning rate
rate_learning = 1e-4

# Initialize optimizer
optim = torch.optim.Adam(SGD_model.parameters(), lr=rate_learning)

# Forward pass
for values in range(500):

     pred_y = Adam_model(input_X)

   loss = loss_fn(pred_y, output_Y)

   if values % 100 == 99:

      print(values, loss.item())

# Zero all gradients
optim.zero_grad()

# Backward pass
loss.backward()

# Call step function
step = optim.step()
```

If you see the last three lines of the pipeline, it first performs `optim.zero_grad()` then `loss.backward()` and `optim.step()`. So, why is that?

- But first talk about `loss.backward()` and `optim.step()`:
 > `loss.backward()` sets the grad attribute of all tensors with `requires_grad=True` in the computational graph of which loss is the leaf (only x in this case).
 
 > Optimizer just iterates through the list of parameters (tensors) it received on initialization and everywhere where a tensor has `requires_grad=True`, it subtracts the
 value of its gradient stored in its `.grad` property (simply multiplied by the learning rate in case of SGD). It doesn't need to know with respect to what loss the 
 gradients were computed it just wants to access that `.grad` property so it can do `x = x - lr * x.grad`.
 
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
 
- What is `optim.zero_grad()`:
  > Note that if we were doing this in a train loop we would call `optim.zero_grad()` because in each train step we want to compute new gradients - we don't care 
  > about gradients from the previous batch. Not zeroing grads would lead to gradient accumulation across batches.


### References:
- [https://stackoverflow.com/a/63651323/12568833](https://stackoverflow.com/a/63651323/12568833)
- [What does optimizer step do in pytorch](https://www.projectpro.io/recipes/what-does-optimizer-step-do)


