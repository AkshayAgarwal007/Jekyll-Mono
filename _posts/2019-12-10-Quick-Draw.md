---
layout: post
title: Re-creating the Quick Draw Neural Net using convolutional neural network 
author: Tino Margely
---
When starting with machine learning, you usually try to find some cool and easy projects to test tour newly acquired skills. "Titanic, ML from disaster" is one if not the most famous beginner ML problem that you can find on the internet.  It was launched on the Kaggle website a few years ago and more than 10,000 persons/teams entered the challenge.

## Re-creating the "Draw, Quick" Neural Net 
-----

## Import libraries
```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import tensorflow as tf
import sklearn
```

## Import data




```python
X = np.load('full_numpy_bitmap_pizza.npy')
X = np.reshape(X , (X.shape[0],28,28))
```


```python
plt.imshow(X[0], cmap='Greys')
plt.title('Pizza')
```




    Text(0.5, 1.0, 'Pizza')




![png](/images/output_7_1.png)



```python
names= ['tennis racquet', 'pizza', 'ladder', 'banana', 'book', 'airplane', 'The Eiffel Tower']
X = np.zeros((1,28,28))
y = np.array(0)
i=0

for name in names :
    fileName = 'full_numpy_bitmap_' + name + '.npy'
    tempX = np.load(fileName)
    tempX = np.reshape(tempX , (tempX.shape[0],28,28))
    tempX = tempX[0:10000,:,:]
    print(name, ' :' , tempX.shape[0])
    X = np.vstack((X, tempX))
    y = np.hstack((y,np.repeat(i, 10000)))
    i+=1

print(X.shape)
X = X[1:(X.shape[0]),:,:]
print(X.shape)

print('X : ')
print(X)

print(y.shape)
y = y[1:(y.shape[0])]
print(y.shape)
y = y.T
print(y.shape)
```

    tennis racquet  : 10000
    pizza  : 10000
    ladder  : 10000
    banana  : 10000
    book  : 10000
    airplane  : 10000
    The Eiffel Tower  : 10000
    (70001, 28, 28)
    (70000, 28, 28)






```python
from sklearn.utils import shuffle
X , y = shuffle(X,y)
plt.figure(figsize=(12,12))

for i in range(16):
    plt.subplot(4,4,i+1)
    plt.imshow(X[i], cmap='Greys')
    plt.title(names[y[i]])
```


![png](/images/output_9_0.png)



```python
from sklearn import preprocessing
```


```python
scaler  = preprocessing.StandardScaler()
X = X.reshape(-1, 784)

print("Shape :" ,X.shape)
print("mean : " ,X.mean())
print("std" , X.std())

X = scaler.fit_transform(X)

print("/***********************/")

print("Shape :" ,X.shape)
print("mean : " ,X.mean())
print("std" , X.std())

```

    Shape : (70000, 784)
    mean :  3.910056599846032e-19
    std 0.9285714285714055
    /***********************/
    Shape : (70000, 784)
    mean :  1.5558917937638988e-17
    std 0.9285714285714156
    


```python
from sklearn.model_selection import train_test_split

X_train, X_test , y_train , y_test = train_test_split(X,y, train_size = 0.8)
```



## Define the model

> Note à moi même : mettre une image de VGG16


```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D
from tensorflow.keras.layers import Flatten, Dense


my_VGG16 = Sequential()  # Création d'un réseau de neurones vide 

# Ajout de la première couche de convolution, suivie d'une couche ReLU
my_VGG16.add(Conv2D(64, (3, 3), input_shape=(28, 28, 1), padding='same', activation='relu'))

# Ajout de la deuxième couche de convolution, suivie  d'une couche ReLU
#my_VGG16.add(Conv2D(64, (3, 3), padding='same', activation='relu'))

# Ajout de la première couche de pooling
my_VGG16.add(MaxPooling2D(pool_size=(2,2), strides=(2,2)))


# Ajout de la première couche de convolution, suivie d'une couche ReLU
my_VGG16.add(Conv2D(64, (3, 3), input_shape=(28, 28, 1), padding='same', activation='relu'))

# Ajout de la deuxième couche de convolution, suivie  d'une couche ReLU
#my_VGG16.add(Conv2D(64, (3, 3), padding='same', activation='relu'))

# Ajout de la première couche de pooling
my_VGG16.add(MaxPooling2D(pool_size=(2,2), strides=(2,2)))


my_VGG16.add(Flatten())  # Conversion des matrices 3D en vecteur 1D

# Ajout de la première couche fully-connected, suivie d'une couche ReLU
my_VGG16.add(Dense(64, activation='relu'))

# Ajout de la deuxième couche fully-connected, suivie d'une couche ReLU
my_VGG16.add(Dense(32, activation='relu'))

# Ajout de la dernière couche fully-connected qui permet de classifier
my_VGG16.add(Dense(7, activation='softmax'))
```


```python
my_VGG16.compile(optimizer='Adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])
```


```python
my_VGG16.fit(X_train.reshape(-1,28,28,1),y_train , epochs=3 , validation_split=0.2)
```

    Train on 44800 samples, validate on 11200 samples
    Epoch 1/3
    44800/44800 [==============================] - 7s 148us/sample - loss: 0.2780 - accuracy: 0.9182 - val_loss: 0.1972 - val_accuracy: 0.9440
    Epoch 2/3
    44800/44800 [==============================] - 4s 88us/sample - loss: 0.1521 - accuracy: 0.9546 - val_loss: 0.1733 - val_accuracy: 0.9487
    Epoch 3/3
    44800/44800 [==============================] - 4s 88us/sample - loss: 0.1186 - accuracy: 0.9632 - val_loss: 0.1615 - val_accuracy: 0.9522
    



    <tensorflow.python.keras.callbacks.History at 0x27a22c574e0>




```python
plt.figure(figsize=(15,15))
for i in range(16):
    plt.subplot(4,4,i+1)
    plt.imshow(X_test[i].reshape(28,28), cmap='Greys')
    a  = my_VGG16.predict(X_test[i].reshape(1,28,28,1))[0]
    index = np.argmax(a)
    plt.title(names[y_test[i]] + '| Prediction :' + names[index])

```


![png](/images/output_20_0.png)



```python
val_loss, val_acc = my_VGG16.evaluate(X_test.reshape(-1,28,28,1), y_test)
print(val_loss, val_acc)
```

    0.15574146871481623 0.9545714
    


```python
my_VGG16.save('firstmodel.h5')
```


```python
from tensorflow.keras.models import load_model
my_model = load_model('firstmodel.h5')
```


```python
a = my_model.predict(X_test[1].reshape(1,28,28,1))
plt.imshow(X_test[1].reshape(28,28), cmap='Greys')
plt.title('|' + names[y_test[1]] + '| Prediction : ' + names[np.argmax(a)] + '|')
```




    Text(0.5, 1.0, '|tennis racquet| Prediction : tennis racquet|')




![png](/images/output_24_1.png)



```python
import cv2
PATH = 'C:/Users/Tino/A_DATA_SCI/Quick, Draw !/my_drawings/'
for i in range(1):
    image_name = PATH +  str(i+1) + '.png'
    img = cv2.imread(image_name,0)
    img = cv2.resize(img, (28,28))
    a = np.asarray(img)
    a = 255 -a
    a = a.reshape(1, 784)
    a = (a -a.mean())/(a.std())
    pred = my_VGG16.predict(a.reshape(1,28,28,1))
    plt.title(names[np.argmax(pred)])
    plt.imshow(a.reshape(28,28), cmap = 'Greys')
    

```


![png](/images/output_25_0.png)



