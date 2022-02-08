---
layout: post
title: Use kaggle Dataset api
subtitle: This guide is specific to Kaggle NB. The main steps are same, you might need to ...
cover-img: /assets/img/landing_python-2x.png
thumbnail-img: /assets/img/api_logo.png
share-img: /assets/img/api_logo.png
tags: [kaggle]
---


## [Mini tutorial] Create Kaggle Dataset w/ kaggle API >>
This guide is specific to Kaggle NB. The main steps are same, you might need to change some code thats it. 


- copy your kaggle api token to the root directory:
```python
!cp -v /kaggle/input/random-private-files/kaggle.json /root/.kaggle
```
- check out your `kaggle.json` file [optional]:
```python
!cat /kaggle/input/random-private-files/kaggle.json
```
```
>>> {"username":"soumya9977","key":"xxx99898y9uhoxxxxausdui"}
```
- initialize a dataset:
+ Create a folder containing the files you want to upload,
+ then run the below command on your dataset path, that will create a metadata json file,
```python
!kaggle datasets init -p /path/to/dataset
```
```
>>> Warning: Your Kaggle API key is readable by other users on this system! To fix this, you can run 'chmod 600 /root/.kaggle/kaggle.json'
Data package template written to: ./runs/evolve/exp/dataset-metadata.json
```
- Check out the created metadata `.json` file [optional]:
```python
!cat ./path/to/dataset/dataset-metadata.json
```
```
>>>
{
  "title": "INSERT_TITLE_HERE",
  "id": "soumya9977/INSERT_SLUG_HERE",
  "licenses": [
    {
      "name": "CC0-1.0"
    }
  ]
}
```

- Rewrite the `.json` file with your dataset title and id[URL id]:

```python
%%writefile ./path/to/dataset/dataset-metadata.json

{
  "title": "yolov5_evolve_e5_files",
  "id": "soumya9977/yolov5-evolve-e5-files",
  "licenses": [
    {
      "name": "CC0-1.0"
    }
  ]
}
```


- Push your files to that initialized dataset:

```python
!kaggle datasets create -p ./runs/evolve/exp/
```

```
>>> 
Some progress bar associated with your file names
Your private Dataset is being created. Please check progress at https://www.kaggle.com/soumya9977/yolov5-evolve-e5-files
```

- Your dataset will be private by default. You can also add a -u flag to make it public when you create it, or navigate to "Settings" > "Sharing" from your dataset's page to make it public or share with collaborators.


## Credit:
- [https://www.kaggle.com/product-feedback/52640](https://www.kaggle.com/product-feedback/52640)
- [https://www.kaggle.com/faisalalsrheed/how-to-kaggle-on-colab-comprehensive-guide](https://www.kaggle.com/product-feedback/52640)



