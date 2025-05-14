# Supreme-QCNN

### 1. Description:

This is a quantum convolutional neural network with autoencoder and PCA used for dimension reduction.The QCNN_Code folder carries audio and image classification code for QCNN.

### 2. Ansatzes used in Convolutional Layer and Pooling Layer
Ansatzes used in this [papers](https://arxiv.org/abs/2108.00661) are
![image](https://github.com/user-attachments/assets/ddc84a2f-4608-4715-a5f2-45a57529b28e)

### 3. Encoding: 

Here, we have used Angular Hybrid Encoding and Amplitude Hybrid Encoding.

### 4. QCNN Structure:
QCNN Architecture used from this [paper](https://arxiv.org/abs/2108.00661)
![image](https://github.com/user-attachments/assets/0e59a17f-4985-40f4-b394-4d0b0799ba22)

### 5. Cost functions: 

As cost functions, we have used MSE (Mean Squared Error) and cross-entropy.

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

The image classification has an accuracy of around 98%, whereas the audio classification has an accuracy of around 75%. It can be improved by increasing the number of qubits or using Hybrid QCNN.
