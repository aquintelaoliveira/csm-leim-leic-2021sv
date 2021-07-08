# 1. 

```Considere as normas de compressão de vídeo.```
* ```a) 1.5 val) Admita que pretende transmitir um vídeo num canal de com 21Mbit/s. Considere que o fator de compressão é de 50 e 20 para a luminância e crominância respetivamente, que se usa um subsampling de cor 4:2:2, 8 bits por amostra, e que o formato do vídeo é 16:9 (largura/altura=16/9). Calcule a resolução máxima de uma trama em termos do número de linhas e colunas para se obter uma taxa de aproximadamente 15 tramas por segundo (frame rate≈15).```

* ```b) (1,5 val) Quais os objetivos principais da norma MPEG2.```

![diagrama](./imagens/mpeg2.png)


* ```c) (1,5 val) Qual o mecanismo usado nos codificadores de vídeo.```

Codificação em blocos (?)

# 2.

```Considere mensagem com 4 símbolos (A; B; L; O): “BOLABOLAAABOLAAA”```

* ```a) Pretende-se codificar esta mensagem usando um código de Huffman.```

* ```a-i. (2,0 val) Estime as probabilidades dos símbolos baseado no número de ocorrências na mensagem, e crie o código de Huffman para estes símbolos. Codifique a mensagem.```

|             |             |             |
|:------------|:------------|:------------|
| A(7)   **0**| A(7)   **0**| A(7)   **0**|
| B(3)  **10**| LO(6) **11**| LOB(9) **1**|
| L(3) **110**| B(3)  **10**|             |
| O(3) **111**|             |             |

| Símb. | Prob. | Código |
|:-----:|:-----:|:------:|
|      A|   7/16|       0|
|      B|   3/16|      10|
|      L|   3/16|     110|
|      O|   3/16|     111|

**Mensagem codificada:** 10 111 110 0 10 111 110 0 0 0 111 110 0 0 0

* ```a-ii. (1,5 val) Calcule a eficiência do código e a taxa de compressão. Explicite todos os pressupostos assumidos.```

```python
# total number of symbols
t = np.sum(occurrences)
t = 16
# probability of each symbol
p = [occ / t for occ in occurrences]
p = [7/16, 3/16, 3/16, 3/16]

H(S) = -np.sum([p * np.log2(p)])
H(S) = ...
```

É possível codificar a fonte sem perdas, porque o número médio de bits por símbolo é maior ou igual à entropia da fonte.

```python
E = H(S) / L
L = 1 + 2 + 3 + 3 = 9
```

TC = 128 bits / 29 bits

* ```b) (2,5 val) Codifique esta mensagem usando o código LZW. Assuma o dicionário inicial [1-”A”; 2-”B”; 3-”L”; 4-”O”].```

|string w|symbol s|  output|  string|    code|
|:------:|:------:|:------:|:------:|:------:|
|        |        |        |       A|       1|
|        |        |        |       B|       2|
|        |        |        |       L|       3|
|        |        |        |       O|       4|
|       B|       O|       2|      BO|       5|
|       O|       L|       4|      OL|       6|
|       L|       A|       3|      LA|       7|
|       A|       B|       1|      AB|       8|
|       B|       O|        |        |        |
|      BO|       L|       5|     BOL|       9|
|       L|       A|        |        |        |
|      LA|       A|       7|     LAA|      10|
|       A|       A|       1|      AA|      11|
|       A|       B|        |        |        |
|      AB|       O|       8|     ABO|      12|
|       O|       L|        |        |        |
|      OL|       A|       6|     OLA|      13|
|       A|       A|        |        |        |
|      AA|       A|      11|        |        |
|       A|        |       1|        |        |

**Mensagem codificada:** 2 4 3 1 5 7 1 8 6 11 1

* ```c) (1,0 val) Calcule a eficiência do código LZW e a taxa de compressão.```

```python
Tc = Do / Dc

Do = 16 simbolos *  8 = 128 bits
Dc = 11 códigos  * 12 = 122 bits
Tc = 128 / 122
```

* ```d) (1,0 val) Calcule o número médio de bits por símbolo para os dois códigos (Huffman e LZW), compare e comente os resultados. Proponha um método de codificação alternativo.```

```python
Huffman:
LZW:
```

```
Algorítmo Shannon-Fano

1 – ordenar os símbolos por ordem decrescente de probabilidade (contabilizando o número de ocorrências de cada símbolo na mensagem)
2 – Separar em dois grupos os símbolos com aproximadamente o mesmo número de occorrências
3 – Atribuir a cada grupo o bit 0 e 1
4 – voltar ao ponto dois até que cada grupo tenha apenas um símbolo
```

# 3. 

```Considere a norma de compressão JPEG de uma imagem a cores com resolução de 1280x720 na luminância, subsampling de cor, 4:2:0 e 8 bits por amostra.```

* ```a) (1,0 val) Qual o tamanho em bits da imagem, sabendo que o factor de compressão para a luminância é de 30 e da crominância de 40.```

![diagrama](./imagens/chroma-subsample.jpg)

lum = 1280 x 720 x 8 / 30 = 245.760 bits

crom = 1280 x 720 x 2 x 8 / 4 / 40 = 92.160 bits

* ```b) (3,0 val) Admita que a figura representa a DCT do primeiro bloco de luminância. Codifique este bloco. (Use para o efeito a as matrizes e tabelas da norma)```

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  0|  4|  3| -1|  2|  5| -4|  5|
|  1|  2| -1|  2| -2|  3| -1|  2|
|  3|  4| -3|  5|  6|  3|  2|  2|
| -1| -1|  2|-58|  4|  5|  4|  2|
|  1|-25|  3|  4| -1|  2|  2|  1|
|  3|  2| -5|  4| -3|  2|  1|  1|
|  2| -1|  3|  2|  2| -1|  2|  1|
|  1| -1|  1|  2| -3|  4|  1| -1|
<br>

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0| -2|  0|  0|  0|  0|
|  0| -1|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
<br>

![diagrama](./imagens/zigzag.png)

DC = [ 0 ]

AC = [ (15,0), (5,0), (1,-1), (4,0), (1,-2), (0,0)]

# 4. 

```Considere a transformada DCT 2D na codificação de imagens.```

* ```a) (1.0 val) Considere que todos os pixeis do primeiro bloco de 8x8 de luminância de uma imagem têm um valor constante igual a 15 (quinze). Codifique este bloco usando o modo sequencial da norma JPEG, com um fator de qualidade de 25% (matriz de quantificação é duas vezes K1).```

![diagrama](./imagens/jpeg-diagram.png)

```python
block = np.ones((8,8), dtype='int16')*15
```

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|
| 15| 15| 15| 15| 15| 15| 15| 15|

```python
block_dct = dct(block)
```

```
y(0,0) = 2/8 * sqrt(2)/2 * sqrt(2)/2 * sum sum cos(0) * cos(0) * (15*64)
y(0,0) = 960 / 8 = 120
```

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|120|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|

|   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  4|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|
|  0|  0|  0|  0|  0|  0|  0|  0|

![diagrama](./imagens/zigzag.png)

DC = [ 4 ] -> (100,100)

AC = [ (0,0) ]

**mensagem:** 100 100 DC 1010 EOB 


* ```b) (2,5 val) Calcule a DCT inversa do seguinte bloco de 2x2:```

|   |   |
|:-:|:-:|
|  2| -2|
|  3| -1|

