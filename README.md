# Função 2: Obtendo os bounding boxes de uma classe (utilizando lambda)

## Contexto

  A visão computacional é um ramo da inteligência artificial que aplica algoritmos de aprendizado de máquina para analisar e compreender imagens e vídeos. Uma das principais tarefas nesse campo é a detecção de objetos, que consiste em localizar e identificar itens relevantes em uma imagem ou vídeo.

Durante a detecção, três informações essenciais são geradas:

  Bounding Boxes (BBoxes): São retângulos que delimitam a posição dos objetos detectados, definidos por quatro coordenadas (x1,y1,x2,y2) que marcam os cantos superior esquerdo e inferior direito. Essas caixas ajudam a mostrar onde os objetos estão.

Scores: Cada bounding box recebe um score, que indica a confiança do modelo na detecção do objeto, variando de 0 a 1. Valores mais altos significam maior certeza, sendo crucial para eliminar detecções incorretas.

Classes: Cada objeto identificado é classificado em uma categoria específica, como "gato" ou "cachorro", dependendo do treinamento do modelo.


## Nome e tipo da função
```haskell
selectBoundingBoxesForClass :: Int -> [(Float, Float, Float, Float)] -> [Int] -> [(Float, Float, Float, Float)]
```
A função recebe um número representando a classe (0 ou 1), a lista de bounding boxes e também a lista de classes.
Ela deve retornar uma lista de bounding boxes de uma determinada classe passada para função.

<br>

## Dados para Teste

```haskell
-- Bounding boxes: (xmin, ymin, xmax, ymax)
boundingBoxes :: [(Float, Float, Float, Float)]
boundingBoxes = [ (34.0, 60.0, 200.0, 320.0),
              	(100.0, 150.0, 250.0, 380.0),
              	(300.0, 220.0, 450.0, 450.0) ]

-- Scores: Confidence scores for each detection
scores :: [Float]
scores = [0.95, 0.80, 0.60]

-- Classes: Class 0 or 1 representing two object types
classes :: [Int]
classes = [0, 1, 0]
```

<br>

## Exemplo de aplicação

ghci> selectBoundingBoxesForClass 1 boundingBoxes classes

saída: [(100.0,150.0,250.0,380.0)]

A classe passada para função é a 1 que está na segunda posição da lista de classes, então retorna as bounding boxes da segunda posição da lista de bounding boxes

<br>

## Função zip

A função zip serve para combinar duas listas em uma lista de tuplas, associando o primeiro elemento da primeira lista com o primeiro elemento da segunda lista e assim por diante

Exemplo:

ghci> x = [1, 2, 3]

ghci> y = ["um", "dois", "tres"]

ghcu> zip x y

[(1,"um"),(2,"dois"),(3,"tres")]

<br>

Com isso, podemos utilizar a função zip para relacionar uma classe da lista de classes com seus respectivos bounding boxes

Aplicando a função:

ghci> zip boundingBoxes classes 

[((34.0,60.0,200.0,320.0),0),((100.0,150.0,250.0,380.0),1),((300.0,220.0,450.0,450.0),0)]

<br>

Agora temos uma lista de tuplas, cada uma contendo uma classe e seus respectivos bounding boxes

<br>

## Passos para implementar a função:

<br>

### Primeiro passo: Filtrar a(s) classe(s) desejada(s)

<br>

Para isso foi pensado em utilizar a função lambda juntamente de um filter para verificar se uma das classes da lista de tuplas é a classe desejada:

```haskell
filter (\(_,classe) -> classe == numClass) (zip boundingBoxes classes)
```
Aplicamos um filtro na lista de tuplas utilizando a função lambda (\\). **(\\(_, classe) -> classe == numClass)** é a função lambda que verifica se  o segundo elemento da tupla (neste caso o número da classe) for igual ao **numClass**, passado como argumento para função **selectBoundingBoxesForClass**. No final será criada uma nova lista com apenas as tuplas da classe desejada.

OBS: o _ em (_,classe) significa que o primeiro elemento da tupla é desprezível dentro desse filter e não é utilizado.

<br>

### Segundo passo: Selecionar os bounding boxes da(s) classe(s)

<br>

Para isso foi pensado em utilizar a função map e fst para selecionar os bounding boxes filtrados:

```haskell
selectBoundingBoxesForClass numClass boundingBoxes classes = map fst (filter (\(_,classe) -> classe == numClass) (zip boundingBoxes classes))
```
Usamos o map para percorrer toda a tupla filtrada e usamos a função **fst** para selecionar apenas o primeiro elemento que nesse caso são os bounding boxes da(s) classe(s) filtrada(s).

OBS: a função fst poderia ser substituida pela função lambda (\(x,_) -> x), que possui a mesma funcionalidade

<br>

## Versão errada: Erro de sintaxe

Durante o desenvolvimento da função, a primeira versão foi feita dessa maneira:

```haskell
selectBoundingBoxesForClass numClass boundingBoxes classes = map fst filter (\(_,classe) -> classe == numClass) zip boundingBoxes classes
```
Essa versão apresenta erros devido a falta dos parenteses para delimitar o início e fim de uma função, permitindo que uma função não saiba exatamente quais são os seus argumentos.


<br>
 
## Conclusão

Para chegar nessa solução, primeiramente foi necessário entender como aplicar a função zip dentro do problema, criando uma lista de tuplas com os bounding boxes e suas classes. Após isso, foi necessário fazer com que apenas as classes desejadas pela função fossem escolhidas, para isso foi pensado na função filter, filtrando as tuplas de acordo com as classes de cada uma, assim sobrando só as classes desejadas. Por fim, era necessário passar por toda lista filtrada, selecionando apenas os bounding boxes. Para isso, foi utilizado a função map para percorrer toda lista juntamente com função a fst para gerar como resultado da função selectBoundingBoxesForClass uma nova lista com apenas os bounding boxes das classes filtradas (que neste caso está na primeira posição da tupla).



## Referências
1. https://folivetti.github.io/courses/Haskell/Listas
2. https://www.facom.ufu.br/~madriana/PF/TP3-Listas.pdf
