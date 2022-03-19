---
layout: post
title: GPU Utilization Tips for Pytorch Pipeline
subtitle: 6 tips to make your code utilize GPU, that it never done before.
cover-img: /assets/img/kaggle_banner.png
thumbnail-img: https://developer-blogs.nvidia.com/wp-content/uploads/2018/10/Creative_NVIDIA_PyTorch.png
share-img: https://developer-blogs.nvidia.com/wp-content/uploads/2018/10/Creative_NVIDIA_PyTorch.png
tags: [pytorch, gpu]
---

# GPU Utilization Tips for Pytorch Pipeline
I was having this problem, but at the end I was kinda able to figure out the solution, thats why im sharing this here, as a note to myself.

<p align="center">
<img src="https://i.imgur.com/TznQh59.jpg">
</p>

### Check model and data is in cuda or not:
 Make sure to initialize the model on cuda,
 ```python
 model = CNN(in_channels=cfg["in_channels"], num_classes=cfg["num_classes"]).to(device)
 ```
 
 shift your data on cuda, using
 ```python
data = data.to(device=device)
targets = targets.to(device=device)
 ```
 
 ### Use W&B to monitor GPU utilization:
 Log in to w&b with the below code
 
 ```python
 import wandb

try:
    from kaggle_secrets import UserSecretsClient
    user_secrets = UserSecretsClient()
    api_key = user_secrets.get_secret("WANDB")
    wandb.login(key=api_key)
    anonymous = None
except:
    anonymous = "must"
    print('To use your W&B account,\nGo to Add-ons -> Secrets and provide your W&B access token. Use the Label name as WANDB. \nGet your W&B access token from here: https://wandb.ai/authorize')
 
 wandb.init(project="PogChamp2 Baseline")
 ```
 Use below code to log few metric [epoch and loss],
 
 ```python
# Train Network
for epoch in range(cfg["num_epochs"]):
    running_loss = 0.0
    
    
    for i, (data, targets) in enumerate(tqdm(train_dl)):
        # Get data to cuda if possible
        data = data.to(device=device)
        targets = targets.to(device=device)
        
        # forward
        scores = model(data)
        loss = criterion(scores, targets)  # CrossEntropyLoss accepts prediction in the shape of (64,10) and target is 64 [not sure]

        # backward
        optimizer.zero_grad()
        loss.backward()

        # gradient descent or adam step
        optimizer.step()
        
        running_loss += loss.item()

        if i % 2000 == 1999:    # print every 2000 mini-batches
            print('[%d, %5d] loss: %.3f' % (epoch + 1, i + 1, running_loss / 2000))
            wandb.log({'epoch': epoch+1, 'loss': running_loss/2000})                  # Logging the epoch and loss, it will also help to log system config
            running_loss = 0.0

    print('Finished Training')
    
    save_model(epoch,model,f"cnn_adam_e{epoch}.pth")
 ```
 
### Few sanity checks:
Use the below code to see if the data and targets are in cuda or not,
```python
X_train = X_train.to(device)
X_train.is_cuda
True
```
Change dtype:
```python
dtype = torch.cuda.FloatTensor
data = data.to(device=device).type(dtype)
targets = targets.to(device=device).type(dtype)
```
Explicitely Change tensor data type:
```python
torch.set_default_tensor_type('torch.cuda.FloatTensor')
```

Source: [How To Use GPU with PyTorch](https://wandb.ai/wandb/common-ml-errors/reports/How-To-Use-GPU-with-PyTorch---VmlldzozMzAxMDk)

### Forward pass to check if the model is in GPU:
Use the below code for that, reminder dont forget to put the model on device too [not showen in the pic].
<p align="center">
<img width = "500" src="https://i.imgur.com/j9nNG4m.jpg">
</p>

### Change parameters of dataloader:
Change few parameters in the dataloader,
- batch size [increase to see mure GPU memory and GPU utilization]
- Shuffle = False [sometime making shuffle helps speed up]
- pin_memory = True [helps to increase GPU utilization]
- num_workers = 4 * num of gpu [mention that in the dataloader to increase GPU utilization]

```
it's utilising GPU as some memory of GPU seems to be filled up Try increasing batch size, as much as you can fit in memory
And you may have to scale the learning rate likewise. Scaling lr means, Say you had a lr of x for a batch size of 8, then 
if you increase your batch size to 16, then make the learning rate 2x
```
Source: [7 Tips To Maximize PyTorch Performance](https://towardsdatascience.com/7-tips-for-squeezing-maximum-performance-from-pytorch-ca4a40951259)

### Changes in the dataset class:
- try to make the data loading faster, by that I mean if you need to any preprocessing or reducing image size and stuff like that do that out size dataset `__getitem__`. It will help to loading data from CPU and shifting to GPU much faster. Atleast use albumentation for some kind of postprocessing. or create a seperate preprocessed dataset.
 
 Check this thread [GPU is not getting used, Any suggestion,Please need help](https://www.kaggle.com/c/siim-isic-melanoma-classification/discussion/158304)


### Reference:
https://www.kaggle.com/code/soumya9977/using-prebuild-spectogram-data



