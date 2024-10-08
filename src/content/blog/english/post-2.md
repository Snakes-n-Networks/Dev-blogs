---
title: "Building an Image-to-Text Converter with Handwritten Text Recognition"
meta_title: ""
description: "Image to text"
date: 2024-09-04T05:00:00Z
image: "https://www.pdfgear.com/pdf-converter/img/how-to-convert-handwriting-to-text-1.png"
categories: ["Project","python", "Image text converter"]
author: "Krishna Chaitanya Ethamukkala"
tags: ["Python", "image-to-text"]
draft: false
---
## Introduction

In the era of digitization, converting images to text has become a vital technology for various applications, including digitizing handwritten notes, automating form processing, and enabling accessibility features. While optical character recognition (OCR) for printed text is common, recognizing handwritten text presents unique challenges due to the diversity in handwriting styles. In this blog post, I'll walk you through my journey of building an image-to-text converter that can recognize handwritten text, leveraging the power of PyTorch for custom model training.

## Project Overview

This project is a Flask-based web application that allows users to upload images containing text, which is then converted into digital text. The real challenge lies in enabling the app to accurately recognize handwritten text. To achieve this, I trained a custom model using PyTorch, focusing on a Convolutional Recurrent Neural Network (CRNN) architecture to handle the complexity of handwritten characters.

## Key Features
- **Custom Handwritten Text Recognition**: Trained a PyTorch model on a dataset of handwritten text images.
- **lask Web Application**: A simple and intuitive interface where users can upload images and receive the corresponding text output.
- **Deployment**: The application is deployed on Heroku and integrated with my GitHub Pages site for easy access.

## Step-by-Step Development
1. **Dataset Collection and Preparation**
The first step in any machine learning project is gathering and preparing the dataset. For handwritten text recognition, I needed a diverse set of handwritten samples. I considered using existing datasets like the IAM Handwriting Database but also gathered additional samples to train the model on various handwriting styles.

Data Preprocessing:
Each image was resized and normalized to a consistent size. I also applied data augmentation techniques like rotation, scaling, and adding noise to improve the model's robustness.

2. **Building the PyTorch Model**
For handwritten text recognition, I opted for a Convolutional Recurrent Neural Network (CRNN) architecture. This approach combines the feature extraction capabilities of Convolutional Neural Networks (CNNs) with the sequential processing power of Recurrent Neural Networks (RNNs).

CNN: Extracts features from the input image, such as edges and textures.
RNN (LSTM/GRU): Processes these features over time, allowing the model to understand the sequence of characters in the text.

Here's a simplified version of the model:

```python
import torch.nn as nn

class CRNN(nn.Module):
    def __init__(self, imgH, nc, nclass, nh):
        super(CRNN, self).__init__()
        self.cnn = nn.Sequential(
            nn.Conv2d(nc, 64, 3, 1, 1),
            nn.ReLU(True),
            nn.MaxPool2d(2, 2),
            nn.Conv2d(64, 128, 3, 1, 1),
            nn.ReLU(True),
            nn.MaxPool2d(2, 2),
            nn.Conv2d(128, 256, 3, 1, 1),
            nn.ReLU(True),
            nn.MaxPool2d(2, 2)
        )
        self.rnn = nn.Sequential(
            nn.LSTM(256, nh, bidirectional=True),
            nn.Linear(nh * 2, nclass)
        )

    def forward(self, x):
        x = self.cnn(x)
        x = x.squeeze(2)
        x = x.permute(2, 0, 1)
        x, _ = self.rnn(x)
        return x
```

3. **Training the Model**
Training the model was a critical step. I used the Connectionist Temporal Classification (CTC) loss, which is well-suited for sequence-based tasks like text recognition. The model was trained over several epochs, fine-tuning hyperparameters like learning rate and batch size to improve accuracy.

4. **Integrating with Flask**
Once the model was trained, I integrated it into a Flask web application. The app allows users to upload an image, which is processed by the model to output the recognized text. Flask makes it easy to handle user inputs, process images, and display results.

```python
@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        file = request.files['file']
        if file:
            image = Image.open(file.stream).convert('L')
            text = pytesseract.image_to_string(image)
            return render_template('index.html', extracted_text=text)
    return render_template('index.html')
```

5. **Deploying the Application**
For deployment, I chose Heroku, a cloud platform that supports easy deployment of Flask applications. The deployment process involved creating a Procfile, specifying dependencies in requirements.txt, and setting up automatic deployment through GitHub Actions.

```bash
web: gunicorn main:app
```

This setup ensures that the application is accessible online, and updates are automatically deployed whenever I push changes to the GitHub repository.

6. **Linking to GitHub Pages**
To make the app easily accessible, I linked it from my GitHub Pages site. This allows users to visit my website and access the image-to-text converter directly.

## Challenges and Learnings

One of the biggest challenges was handling the variability in handwriting styles. No two individuals write the same way, and this diversity can make recognition difficult. By applying data augmentation and experimenting with different model architectures, I was able to improve the model's accuracy significantly.

Another challenge was optimizing the model for deployment. Since the model needs to be lightweight and fast, I had to balance complexity with performance, ensuring that the app runs smoothly on Heroku.

## Conclusion

Building an image-to-text converter with handwritten text recognition was a rewarding experience. It combined my interests in machine learning, web development, and cloud deployment into a single project. This project not only helped me deepen my understanding of PyTorch and Flask but also gave me a glimpse into the challenges of real-world OCR tasks.

Feel free to try out the Image-to-Text Converter and explore the code on my GitHub repository https://github.com/ekrishnachaitanya2004/Image-to-text-Converter.git.
