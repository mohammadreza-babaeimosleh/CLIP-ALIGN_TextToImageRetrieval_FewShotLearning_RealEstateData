# CLIP-ALIGN_TextToImageRetrieval_FewShotLearning_RealEstateData
This repository implements few-shot learning using CLIP and ALIGN models on a custom (could be replaced with any data). This repository contains a .ipynb (instead of.py) to help the process be more descriptive, but you can use it in .py format too (if you only want to see the final result)

##### Note: the data used is private and cannot be shared but there is no limit to it

# How to use
if you want to use a .py file you should install the libraries installed at the first cell. Also, edit the path to your target images and labels and the path for the trained output model. you should prepare a training and testing set for the model. The train and test sets must contain a JSON file for labels (a sample JSON file is provided). In this file, keys are images' names, and values are a list of labels in a comma-separated format. there are different cells for different training models (in the training code), each with a slight difference to the others. you can choose between these, considering your entry set of images. In the next step, you should use confidence-tuning code to find the best dynamic and fixed threshold for your application. at the final use the test code to evaluate your models

##### Note: if you want to use CPU instead of GPU you should install a GPU version of the 'faiss' library (use the command below)
```bash
pip install faiss-cpu 
```

# Code description
Both ClIP and Align models are powerful and general multimodal models. These models have proven to have a superior performance on benchmarks. but in your special case, they might not act with your image retrieval tasks, the goal is to retrieve a limited but variable number of images based on the provided input text. The reason for This problem is the fact that we do not know the exact number of images that the system should retrieve. On the other hand, when you use these general models to detect objects within the same context, they fail to provide a differentiating score for each image and scores are too close together. This problem causes you to be unable to set a fixed threshold for the score of images that are relevant or irrelevant. This repository uses few-shot learning to optimize the model for this specific goal.

The training code for each of the CLIP and ALIGN models downloads the model and does the finetunning on it using your properly formatted data. the scores for images are based on the cosine similarity scores between the embedded vectors of your input text and images.

As mentioned, you need a threshold to differentiate between the relevant and irrelevant. You can use confidence-tuning code to find the best confidence threshold for your target. the confidence-tuning code uses a fixed and a dynamic threshold. a fixed threshold is the threshold for the output score that an image with a score less than that is considered to be 100% irrelevant. the dynamic threshold on the other hand is the part that helps you to have a dynamic number of outputs. this threshold dynamically traces the scores in images (sorted based on scores) and whenever it sees a significant drop (5% in our case) in the score, it will cut the output.

Finally, the test code is the code that you can validate the model. in our case we had a small number of images in our real-world test scenario. So we split images of the building into different folders (each folder for images of one building). you can put them as a whole if it works for you
