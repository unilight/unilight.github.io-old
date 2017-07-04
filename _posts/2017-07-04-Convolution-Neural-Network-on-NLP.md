---
layout: "post"
title: "[Deep Learning] Convolution Neural Network on NLP"
date: "2017-07-04 10:16"
tag: [deeplearning]
---

> 之前上李宏毅老師的Machine Learning時，他只有提到2D的CNN。但在HW5時有看到網路上有把1D CNN做在NLP task上，拿來做在那次的作業上發現成效很好。當時便很好奇到底1D CNN的原理是什麼，於是這次就來好好介紹一番。以下的來源基本上都是以下文章的翻譯(笑)：[Understanding Convolutional Neural Networks for NLP](http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/)
<!-- more -->

## CNN的兩大特色
以下就2D CNN來解釋這兩大特色的intuition。

##### Location Invariance
假設我們想要做image classification(或object detection)，想要知道圖片裡面有沒有一隻大象。在做convolution時我們會把filter掃過整張圖，因此我們不在乎到底大象出現在圖片的哪個地方，只要圖片裡有大象就好。

##### Compositionality
每個filter都是機器理解input(圖片)的小小元件，讓機器可以從low level的特徵(feature)去組織high level的概念。以圖像而言，機器會從像素組成邊邊角角，從邊邊角角組成基本幾何形狀，再從幾何形狀組成一個個的object。這也是為什麼CNN在電腦視覺上這麼強大。

## 如何將CNN應用於NLP？

在NLP中，input data通常是句子或者文章，用matrix表示。每一列會代表一個*token*，我將它理解成最小文字單位，這是可以自己決定的，通常是單字，但也可以是字母。通常每一列會是一個vector，代表該token的*word embedding*。因此假設我們有一個10個字的句子，每個字用一個100維的vector表示，則我們會得到一個10x100的「圖片」。
在影像上做convolution時，我們通常會掃完一列之後就換行掃下一列。但在NLP上，我們通常只會掃一個方向，也就是說以上述的10x100「圖片」而言，我們會設filter的形狀為$$(region\;size)\times 100$$，然後由上掃到下。這大概是為什麼這種CNN被稱為1D CNN。這裡的region size也是可以自己調的，但通常是2-5。
看到這裡不禁想問，那前面提到的兩大CNN特色呢？在這裡有得到實現嗎？看起來似乎不然。
 - **Location Invariance:** 我們的確會很在乎單字出現在哪，這會大大影響詞性和意思。
 - **Compositionality:** 的確，某些字的組合會代表某些意義，例如形容詞修飾名詞，組合出一個帶有特色的名詞。但這種文字上的小元件到底能否像圖片一樣，從像素組成high level的概念，似乎有點說不太通。
這樣看來，比起CNN，較類似於人腦處理語言的RNN，似乎更適合做在NLP上？但出乎意料的，CNN在NLP一直都取得了不錯的成效。就像Bag-of-word模型，雖然有一些錯誤的假設，但已經被應用行之有年且都有很棒的結果。
CNN的一大優點是計算速度相當快，歸功於GPU的發展。和RNN, n-gram相比，CNN不僅速度快，某種程度上也能夠讓filter學到不錯的representation。直覺上，CNN的第一層layer學到的東西與n-gram相當類似。

## CNN Hyperparemeters

##### Narrow vs. Wide convolution

當遇到邊緣時，narrow convolution會避開，wide convolution則是做padding，也就是將input以外沒有值的地方以0計算。以下是一個1D的例子

![img](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/11/Screen-Shot-2015-11-05-at-9.47.41-AM.png)
*Source: A Convolutional Neural Network for Modelling Sentences (2014)*

可以發現當filter size很大時，若做narrow convolution，會得到很少的值，以上圖來說，便只得到$$(7-5)+1=3$$個output。但如果做wide convolution，則會得到$$(7+2*4-5)+1=11$$個output。透過這個方法，通常可以得到跟input size相同，甚至是更多的output。

##### Stride Size

這決定了每次掃filter時要移動多少，stride size越大，output size越小。一般而言，標準的stride size都是1。
![img](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/11/Screen-Shot-2015-11-05-at-10.18.08-AM.png)
*Source: http://cs231n.github.io/convolutional-networks/*

##### Pooling Layers

Pooling常常跟在convolution後，會將input做subsampling，通常最常見的是max pooling。Pooling的window也是可以自己調的，但在NLP中通常會對整個input做pooling，得到一個scalar作為output。
Pooling有以下好處：
 - **保持output size固定:** 舉例來說，如果今天有1000個filter，然後對每個filter做max pooling，則會得到一個1000維的output。這麼一來，output size和filter、input的大小便無關，便可以很有彈性的吃不同size的input。這對classification是很重要的，因為classification常常會需要處理input size不一的資料。
 - **降低維度，保留資訊:** 可以將每個filter想成在偵測某種特徵，例如包含某個片語。如果該片語出現在句子中某處，則將該filter和該處做convolution後應該會得到一個很大的值，反之則會得到很小的值。做max pooling後，「該特徵是否出現在句子中的某處」的資訊會留下；但相反的，「該特徵出現在句子中的哪裡」的資訊則會被丟掉。我個人認為這有好有壞。

##### Channels

Channel就很像資料的不同「view」。以影像為例，RGB便是三個不同的channel，我們可以對不同的channel做不同的convolution，給予不同的權重。在NLP上，可以用不同的word embedding、語言、理解方式作為不同的channel。

## Conclusion

我個人的感想是，CNN除了計算速度相對快，特色是能夠從資料抽出某些特徵，但會流失那些特徵的局部性，也就是不知道那些特徵出現在哪裡。這對NLP的某些task當然是相當有利的，但不代表可以做在所有的task上。還是得要先評估看看該task有什麼特性，再決定要不要使用。
