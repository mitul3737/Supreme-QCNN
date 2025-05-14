# Supreme-QCNN

### 1. Description:

This is a quantum convolutional neural network with autoencoder and PCA used for dimension reduction.

### 2. Ansatzes used in Convolutional Layer and Pooling Layer
Ansatzes used in this [paper](https://arxiv.org/abs/2108.00661)
![image](https://github.com/user-attachments/assets/ddc84a2f-4608-4715-a5f2-45a57529b28e)

The accuracy of a QCNN also depends on ansatzes. So, checking its [expressivity](https://github.com/mitul3737/Supreme-QCNN/tree/main/Expressivity) and [entangling capability](https://github.com/mitul3737/Supreme-QCNN/tree/main/Entangling_Capability) might help choose the perfect ansatz.


### 3. Encoding: 


Encoding refers to the process of mapping classical data (numbers, vectors, images, etc.) into quantum states that can be processed by a quantum computer. Since quantum algorithms operate on qubits, classical information must first be converted into a quantum-readable format. 
This step is crucial for quantum machine learning (QML), optimization, and simulation tasks. There are encoding techniques lie Basis Encoding, Amplitude Encoding, Angle Encoding, Qubit Encoding, Hamiltonian Encoding etc.

Here, we have used Angular Hybrid Encoding and Amplitude Hybrid Encoding.

### 4. QCNN Structure:
QCNN Architecture used from this [paper](https://arxiv.org/abs/2108.00661)
![image](https://github.com/user-attachments/assets/0e59a17f-4985-40f4-b394-4d0b0799ba22)


Here, I have used 

```

Conv_Pooling_Layer_Ansatzs = [['U_SU4',"custom_Pooling1",'U_SU4','custom_Pooling4','U_SU4','Pooling_ansatz1']]

Conv_Pooling_Layer_Params = [[15,10,15,14,15,2]]

Encodings = ['resize256', 'pca8', 'autoencoder8'] # dimensionality reduction techniques

dataset = 'mnist'

classes = [0, 1]

binary = False

cost_fn = 'cross_entropy'

```

Meaning, I have used these ansatzes in my QCNN architecture.

![image](https://github.com/user-attachments/assets/b7f6c42b-cbb8-401c-b4df-2c5ea7ba19dc)

Here, U_SU4 has 15 parameters, custom_Pooling1 has 10 parameters, custom_Pooling4 has 14 parameters, and Pooling_ansatz1 has 2 parameters.

You may change the anstazes and then change the parameters (Conv_Pooling_Layer_Params) accordingly. Within the encoding array, we have dimensionality reduction techniques used in each iteration. 

Moreover, the dataset carries the dataset type (for image classification, we used 'mnist' or 'fashion mnist') or path (for audio classification, we referred to the Dataset/Mel_Spectrograms folder). Binary remains "False" for the "cross-entropy" cost function, whereas the binary value is "True" when using "MSE."
### 5. Cost functions: 

As cost functions, we have used MSE (Mean Squared Error) and cross-entropy. Mean Squared Error (MSE) and Cross-Entropy are two commonly used cost functions in machine learning and deep learning for training models. 
They measure the difference between predicted outputs and true labels, but they are suited for different types of problems.

### 6. Training and Test Dataset:

For the audio classification, the Capuchinbird's audio Mel spectrogram is used. 

Here the training dataset path is ```Dataset/Mel_Spectrograams/Train/Parsed_Capuchinbird_Clips and Dataset/Mel_Spectrograams/Train/Parsed_Not_Capuchinbird_Clips```

Also, the test dataset path is ```Dataset/Mel_Spectrograams/Test/Parsed_Capuchinbird_Clips and Dataset/Mel_Spectrograams/Test/Parsed_Not_Capuchinbird_Clips```

In the code, it's written as

```

capuchinbird_dir = os.path.join(dataset, 'Train', 'Parsed_Capuchinbird_Clips')

not_capuchinbird_dir = os.path.join(dataset, 'Train', 'Parsed_Not_Capuchinbird_Clips')

capuchinbird_test_dir = os.path.join(dataset, 'Test', 'Parsed_Capuchinbird_Clips')

not_capuchinbird_test_dir = os.path.join(dataset, 'Test', 'Parsed_Not_Capuchinbird_Clips')

dataset = 'Dataset\Mel_Spectrograms'

```

So, modify these paths for your own dataset.

But for the image classification, the Keras dataset has been used.

```

if dataset == 'fashion_mnist':

        (x_train, y_train), (x_test, y_test) = tf.keras.datasets.fashion_mnist.load_data()

elif dataset == 'mnist':

        (x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data()

```

### 7. Conclusion: 
Image classification has a higher accuracy, which is around 98%, and audio classification seems to have an accuracy near 75%. The audio accuracy can be increased using Hybrid QCNN or by using different data pre-processing techniques.
