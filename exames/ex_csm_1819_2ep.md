#1. 

```Considere a mensagem [ 3 1 2 4 1 7 9 1 1 ].```

* ```a) (1.5 val) Descodifique-a, assumindo que foi usado o codificador LZW, com o dicionário inicial [1-”A”; 2-”I”; 3-”L”].```

```python
# Algoritmo de decompressão LZW
c = read code
write simbol (c)
w = simbol (c)
while ( not end )
    c = read code
    s = simbol (c) 
    if s=[]
        s = w + w[0]
    write s
    add string “w s[0]” to dictionary
    w = s
```

|   input|string s|symbol w|w + s[0]|    code|
|:------:|:------:|:------:|:------:|:------:|
|        |        |        |       A|       1|
|        |        |        |       I|       2|
|        |        |        |       L|       3|
|       3|        |       L|        |        |
|       1|       A|       A|      LA|       4|
|       2|       I|       I|      AI|       5|
|       4|      LA|      LA|      IL|       6|
|       1|       A|       A|     LAA|       7|
|       7|     LAA|     LAA|      AL|       8|
|       9|        |        |        |        |

* ```b) (1.5 val) Admitindo o menor número possível de bits para cada codificação, calcule a taxa de compressão.```

# 2. 

```Considere as seguintes afirmações e justifique se são verdadeiros ou falsos.```
* ```a) (1.5 val) Para a mesma mensagem usando os codificadores LZW e Huffman obtêm-se valores de entropia diferentes.```

Falso (?), não é mandatório de acontecer.

* ```b) (1.5 val) Para uma fonte de símbolos com H=2.4, um codificador obteve L = 2.0.```

Verdadeiro, mas significa que a fonte foi codificada com perdas, porque o número médio de bits por símbolo é inferior à entropia da fonte.

* ```c) (1.5 val) Para uma determinada mensagem, uma implementação do codificador obteve um valor de L = 0.79.```

Verdadeiro, _L_ é o número médio de bits por símbolo, contudo para o valor de _L_ = 0.79, tem de existir símbolos diferentes com uma codificação idêntica.

# 3.

```Considere a matriz apresentada ao lado```

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  0|  0|  0|  0|  0|  0|  0|  0|
|100|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
<br>


* ```a) (1.5 val) Admitindo que a matriz representa a DCT do primeiro bloco de luminância de uma imagem, represente o código correspondente segundo a norma JPEG.```

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  8|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
<br>

![diagrama](./imagens/zigzag.png)

DC = [ 0 ] -> (00, 0)

AC = [ (0,1), (4,8), (0,0) ]

**mensagem:** 00 DC 00 1111111110011011 AC 1010 EOB 

* ```b) (1.5 val) Diga justificando qual das opções em baixo é o resultado da IDCT (2D)```

![diagrama](./imagens/cosine-funcs.png)

A reposta é a matriz b1). Como o resultado da DCT apenas teve valores na casa y = (0,1), é possível deduzir que a imagem apresentava um degrade de valores positivo em cima para valores negativos em baixo.

* ```c) (1.5 val) No contexto da norma JPEG diga o que significa o resultado da alínea anterior.```

huh? Que as transições de alt frequência acontecem de Norte para Sul da imagem?

* ```d) (1.5 val) Compare os modos progressivo e sequencial da norma JPEG.```

[progressive-vs-baseline](https://medium.com/hd-pro/jpeg-formats-progressive-vs-baseline-73b3938c2339)

The baseline JPEG algorithm renders an image line by line as it processes the data when downloading from the network. The data is processed in streams, as the data arrives in the computer’s buffer coming from the network. It then renders the image from top to bottom, and left to right.

The progressive JPEG algorithm gives the appearance of a blurry image at first. Then little by little, the image begins to render until it displays the fully rendered image. The browser is actually interpreting the image line by line, but gives a blurry preview of the full image in the placeholder which is where the image is inserted in a web page.

* ```e) (1.5 val) Diga justificando o que realiza o código seguinte e o que representa o seu resultado.```

```python
import matplotlib.pyplot as plt
import cv2
import numpy as np
x = cv2.imread("Lena.tif",cv2.IMREAD_GRAYSCALE)
v, bins, patches = plt.hist(x.ravel(),256,[0,256])
I= v!=0
pi= v/sum(v)
H=-np.sum( pi[I]*np.log2(pi[I]) )
```

O código presente serve para calcular a entropia da fonte.

# 3.

```Admita que se pretende desenhar um sistema de vídeo onde a transmissão tem definição UHD 4K (3840x2160) a 30Hz usando chroma-subsampling 4:2:2 com 8 bit por amostra. A qualidade necessária é atingida com os factores de compressão indicados na tabela. Para garantir o acesso aleatório ao vídeo é necessário garantir que:```
- ```exista pelo menos uma I-frame em cada 300 ms;```
- ```entre cada duas I-frames deve haver uma P-frame;```
- ```não pode haver mais do que 3 B-frames consecutivas.```

* ```a) (1.5 val) Apresente uma caracterização temporal da estrutura de codificação das I, P, B-frames que deve ser adotada.```
* ```b) (2.0 val) Determine o débito binário médio associado à estrutura encontrada na alínea anterior.```
* ```c) (1.5 val) Com o método de compensação de movimento “3-passos”, obtém-se no descodificador:```
* ```c1) um SNR melhor que a compensação de movimento com o método Full-search;```
* ```c2) um SNR pior que a compensação de movimento com o método Full-search;```
* ```c3) um SNR equivalente à compensação de movimento com o método Full-search;```