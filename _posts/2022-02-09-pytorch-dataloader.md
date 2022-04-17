---
layout: post
title: DataLoder in pytorch Three ways that I know
subtitle: After digging a little bit more I got to know that,there are three ways of loading data in a pytorch model...
cover-img: /assets/img/kaggle_banner.png
thumbnail-img: https://pytorch.org/tutorials/_static/img/thumbnails/cropped/profiler.png
share-img: https://pytorch.org/tutorials/_static/img/thumbnails/cropped/profiler.png
tags: [Deep Learning, pytorch, dataset]
---




When I started learning PyTorch, it was very difficult for me to understand how to load data as batches in a model as different tutorials were using different techniques to load data. After digging a little bit more I got to know that, there are three ways of loading data in a PyTorch model, `datasets.ImageFolder`,`creating a custom class for loading data`, and `downloading directly from torchvision datasets and using DataLoader`. And this is because file structure and the arrangement of data are different in different casses. By that I mean, say you have the cat vs
dog classification dataset, in most of the cases after downloading the data you will see that images of dogs and cats are seperated into two folders. Now if you have this kind of data arrangement then you will be using `datasets.ImageFolder`. If you download the dataset from `torchvision datasets` then you can directly use `DataLoader` and if the arrengement of data is different than others(mentioned previously) then you use a `custom class` to load the data. I will be explaning these three parts down below,


# Downloading directly from pytorch datasets and using DataLoader:

There are planty of dummy datasets available in `torchvision` datasets and this is the [link](https://pytorch.org/vision/stable/datasets.html) you can download then as shown below 
by providing the name of the dataset. And then use `DataLoader` to load the data in batches.
```python
train_dataset = datasets.MNIST(root="dataset/", train=True, transform=transforms.ToTensor(), download=True)
test_dataset = datasets.MNIST(root="dataset/", train=False, transform=transforms.ToTensor(), download=True)
train_loader = DataLoader(dataset=train_dataset, batch_size=batch_size, shuffle=True)
test_loader = DataLoader(dataset=test_dataset, batch_size=batch_size, shuffle=True)
```

# datasets.ImageFolder:
The easiest way to load image data is with datasets.ImageFolder from torchvision ([documentation](http://pytorch.org/docs/master/torchvision/datasets.html#imagefolder)). 
In general you'll use ImageFolder like so:

`python
dataset = datasets.ImageFolder('path/to/data', transform=transform)
`
where `'path/to/data'` is the file path to the data directory and `transform` is a list of processing steps built with the `transforms` module from `torchvision`. 
ImageFolder expects the files and directories to be constructed like so:

```
root/dog/xxx.png
root/dog/xxy.png
root/dog/xxz.png

root/cat/123.png
root/cat/nsdf3.png
root/cat/asd932_.png
```

where each class has it's own directory (cat and dog) for the images. The images are then labeled with the class taken from the directory name. So here, the image 123.png would be loaded with the class label cat. You can download the dataset already structured like this from [here](https://s3.amazonaws.com/content.udacity-data.com/nd089/Cat_Dog_data.zip).
I've also split it into a training set and test set.

# Transforms:
When you load in the data with `ImageFolder`, you'll need to define some transforms. For example, the images are different sizes but we'll need them to all be the same size for training. You can either resize them with `transforms.Resize()` or crop with `transforms.CenterCrop()`, `transforms.RandomResizedCrop()`, etc. We'll also need to convert the images to PyTorch tensors with `transforms.ToTensor()`. Typically you'll combine these transforms into a pipeline with `transforms.Compose()`, which accepts a list of transforms and runs them in sequence. It looks something like this to scale, then crop, then convert to a tensor:

```python
transform = transforms.Compose([transforms.Resize(255),
                                 transforms.CenterCrop(224),
                                 transforms.ToTensor()])

```

There are plenty of transforms available, I'll cover more in a bit and you can read through the [documentation](http://pytorch.org/docs/master/torchvision/transforms.html). 

# Data Loaders

With the `ImageFolder` loaded, you have to pass it to a [`DataLoader`](http://pytorch.org/docs/master/data.html#torch.utils.data.DataLoader). The `DataLoader` takes a dataset (such as you would get from `ImageFolder`) and returns batches of images and the corresponding labels. You can set various parameters like the batch size and if the data is shuffled after each epoch.

```python
dataloader = torch.utils.data.DataLoader(dataset, batch_size=32, shuffle=True)
```

Here `dataloader` is a [generator](https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/). To get data out of it, you need to loop through it or convert it to an iterator and call `next()`.

```python
# Looping through it, get a batch on each loop 
for images, labels in dataloader:
    pass

# Get one batch
images, labels = next(iter(dataloader))
```

Now if I write them all together, then it would look like this,
```python
# Define default PATH
PATH = '../input/dogs-vs-cats-for-pytorch/cat_dog_data/Cat_Dog_data'

# data_dir = 'Cat_Dog_data/train'
data_dir = PATH + '/train' # load from Kaggle

transform = transforms.Compose([transforms.Resize(255),
                                transforms.CenterCrop(224),
                                transforms.ToTensor()
                               ])# TODO: compose transforms here
dataset = datasets.ImageFolder(data_dir, transform=transform) # TODO: create the ImageFolder
dataloader = torch.utils.data.DataLoader(dataset, batch_size=32, shuffle=True) # TODO: use the ImageFolder dataset to create the DataLoader

```

# custom class and DataLoader:
Say you are trying to load [`melanoma-classification`](https://www.kaggle.com/c/siim-isic-melanoma-classification) dataset. If you look at the data, you will find that in the `jpg` folder there is 
`train` folder which consists of all the training images and each image has a unique id, but where is the labels? So labels are provided in a csv file called `train.csv`. Now neither the `datasets.ImageFolder` nor the `torchvision datasets`
will work. For this you need to load them using a custom class. 

```python
class SkinCancerDataset(Dataset):
    def __init__(self, csv_file, root_dir, transform=None):
        self.annotations = pd.read_csv(csv_file).sample(5000) # We will just use random 1000 images from the training images
        self.root_dir = root_dir
        self.transform = transform
    def __len__(self):
        return len(self.annotations)
    def __getitem__(self, index):
        img_path = os.path.join(self.root_dir, self.annotations.iloc[index, 0]+".jpg")
        
        image = Image.open(img_path)
        y_label = torch.tensor(int(self.annotations.iloc[index, 7]))

        if self.transform:
            image = self.transform(image)
        return image, y_label
        
```
If youobserve carefully, you will notice that there are three dunder menthods, __ init __(),__ len __() and __ getitem __(). In the init we instanciate the required variables, here we 
have instanciated the dataframe by reading from csv and the root directory and a boolean variable called `transform` which denotes that we want to apply transforms in the data while loading or not.
Inside __ len __ we return length of the datadrame, and inside __ getitem __ we read the image and define the y labels and apply transforms if specified and return the image and the corresponding y label.

After that we specify the transforms that we wanna perform,
```python
# Data augmentation and normalization for training
# Just normalization for validation
data_transforms = {
    'train': transforms.Compose([
        transforms.Grayscale(num_output_channels=3),
        transforms.RandomResizedCrop(input_size),    # sometimes we need to provide input_size like this, (input_size,input_size)
        transforms.RandomRotation(degrees=25),
        transforms.ColorJitter(),
        transforms.RandomHorizontalFlip(),
        transforms.ToTensor()
    ]),
    'val': transforms.Compose([
        transforms.Grayscale(num_output_channels=3),
        transforms.Resize(input_size),
        transforms.CenterCrop(input_size),
        transforms.ToTensor()
    ]),
}
```

After specifying the `transforms` we instanciate the class split the data randomly and load the data in the `DataLoader`,

```python
print("Initializing Datasets and Dataloaders...")

# Creating our dataset
train_dataset = SkinCancerDataset(csv_file=data_csv_file, root_dir=data_dir, transform=data_transforms['train'])
print(len(train_dataset))

train_dataset, val_dataset = torch.utils.data.random_split(train_dataset, [4000, 1000])

# Dataloader iterators, make sure to shuffle
train_dataloader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True)
val_dataloader = DataLoader(val_dataset, batch_size=batch_size, shuffle=True)

# Create training and validation dataloaders
dataloaders_dict = {'train': train_dataloader, 'val': val_dataloader}
```

As we have discussed about all the data loading techniques, now lets talk about how we perform daat augmentation in pytorch.

# Data Augmentation

A common strategy for training neural networks is to introduce randomness in the input data itself. For example, you can randomly rotate, mirror, scale, and/or crop your images during training. This will help your network generalize as it's seeing the same images but in different locations, with different sizes, in different orientations, etc.

To randomly rotate, scale and crop, then flip your images you would define your transforms like this:

```python
train_transforms = transforms.Compose([transforms.RandomRotation(30),
                                       transforms.RandomResizedCrop(224),
                                       transforms.RandomHorizontalFlip(),
                                       transforms.ToTensor(),
                                       transforms.Normalize([0.5, 0.5, 0.5], 
                                                            [0.5, 0.5, 0.5])])
```

You'll also typically want to normalize images with `transforms.Normalize`. You pass in a list of means and list of standard deviations, then the color channels are normalized like so

```input[channel] = (input[channel] - mean[channel]) / std[channel]```

Subtracting `mean` centers the data around zero and dividing by `std` squishes the values to be between -1 and 1. Normalizing helps keep the network work weights near zero which in turn makes backpropagation more stable. Without normalization, networks will tend to fail to learn.

You can find a list of all [the available transforms here](http://pytorch.org/docs/0.3.0/torchvision/transforms.html). When you're testing however, you'll want to use images that aren't altered (except you'll need to normalize the same way). So, for validation/test images, you'll typically just resize and crop.

```python
# Define default PATH
PATH = '../input/dogs-vs-cats-for-pytorch/cat_dog_data/Cat_Dog_data'

# data_dir = 'Cat_Dog_data'
data_dir = PATH

# TODO: Define transforms for the training data and testing data
train_transforms = transforms.Compose([transforms.RandomRotation(30),
                                      transforms.RandomResizedCrop(224),
                                      transforms.RandomHorizontalFlip(),
                                      transforms.ToTensor()])

test_transforms = transforms.Compose([transforms.RandomRotation(30),
                                     transforms.RandomResizedCrop(224),
                                     transforms.ToTensor()])


# Pass transforms in here, then run the next cell to see how the transforms look
train_data = datasets.ImageFolder(data_dir + '/train', transform=train_transforms)
test_data = datasets.ImageFolder(data_dir + '/test', transform=test_transforms)

trainloader = torch.utils.data.DataLoader(train_data, batch_size=32)
testloader = torch.utils.data.DataLoader(test_data, batch_size=32)

```



# Reference:
1. https://www.kaggle.com/houssemayed/cnn-architectures-for-skin-cancer-classification#Prepare-dataloaders-(Train,-Validation)
2. https://www.kaggle.com/soumya9977/intro-to-pytorch-loading-image-data/edit
3. https://github.com/aladdinpersson/Machine-Learning-Collection/blob/master/ML/Pytorch/Basics/pytorch_simple_fullynet.py

Thank you for reading.
















