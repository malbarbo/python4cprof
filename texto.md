---
# vim: set spell spelllang=pt_br:
title: Uma visão geral do Python para professores que usam C
author:
    - Marco A L Barbosa
    - "[malbarbo.pro.br](https://malbarbo.pro.br)"
date: \today
papersize: a4
geometry:
- margin=2cm
- nohead
fontsize: 12
colorlinks: true
lang: pt-BR
---

# Introdução

Neste texto apresentamos uma visão geral da linguagem Python para professores que vão ministrar algoritmos ou estrutura de dados em Python e que têm experiência com o ensino usando C. O objetivo é esclarecer alguns pontos e mostrar como as construções algorítmicas de C podem ser expressas em Python. Note que o objetivo não é apresentar todas as construções do Python e nem descrever uma metodologia de ensino de algoritmos e estruturas de dados.

<!-- TODO: A escolha da linguagem depende dos objetivos, se o obj for projetar programas, então Python é uma melhor opção. -->

Começamos com algumas diferenças entre as duas linguagens, depois discutimos as principais construções do Python, em seguida mostramos a implementação de alguns algoritmos e estruturas de dados e por fim apresentamos algumas convenções de código para Python e indicamos algumas referências.


# Diferenças entre Python e C

As linguagens Python e C têm muitos aspectos distintos, pois são comumente usadas em situações distintas. Nessa seção discutimos algumas dessas diferenças. Usamos os seguintes exemplos para ilustrar algumas dessas diferenças.

Código em C

```c
includefile src/conta_no_intervalo.c
```

Código em Python

```python
includefile src/conta_no_intervalo.py
```


## Forma geral da sintaxe

A sintaxe do C é baseada em blocos delimitados por chaves (`{}`{.c}), a indentação do código não influencia a semântica. As chaves são opcionais nas instruções de controle quando apenas uma sentença é especificada. Na definição de estruturas e funções as chaves são sempre necessárias. As expressões/sentenças das instruções de controle devem ser especificadas entre parênteses. Os operadores lógicos são especificados com símbolos (`!`{.c}, `||`{.c}, `&&`{.c}).

Os blocos em Python inciam com ":" e são delimitados pela indentação. As expressões/sentenças das instruções de controle não precisam ser especificadas entre parênteses. Os operados lógicos são especificados com as palavras chaves `not`{.python}, `or`{.python} e `and`{.python}.

Em Python os operadores relacionais pode ser encadeados, dessa forma é possível escrever `a < b < c`{.python} ao invés de `a < b and b < c`{.python}.

Em C os tipos das variáveis são especificadas antes do nome da variável. Já em Python, os tipos são especificados após o nome da variável, pela sintaxe `: tipo`{.python}. As especificação do tipo em C é obrigatória, em Python é opcional. Aspectos da semântica de tipos são discutidas na seção \ref{sistema-tipos}.

Python utiliza a palavra chave `def`{.python} para definições de funções e o tipo de retorna da função é especificado com `-> tipo`{.python}

Em Python não existe operadores de pós e pré incremento/decremento, mas existem os operadores de atribuição com incremento/decremento (entre outros).

Strings em Python podem ser especificadas com aspas (`"`{.python}) ou apóstrofo (`'`{.python}). Strings com múltiplas linhas são especificadas com três aspas ou apóstrofos.

Em C existem dois tipos de comentário, o de linha (`//`{.c}) e o de bloco (`/* */`{.c}). Python também tem dois tipos de comentário, o de linha (`#`{.python}) e o de documentação (`''' '''`{.python}), que deve aparecer apenas como primeiro item de uma definição.


## Forma de execução

C é usado comumente como um linguagem compilada. Isso implica que para testar qualquer trecho de código em C é necessário escrever um programa completo (com função `main`), compilar e depois executar o programa. Além disso, para exibir o valor de uma variável é necessário chamar uma função como `printf` explicitamente (também é possível consultar os valores das variáveis usando um depurador).

Já o Python é usado comumente como uma linguagem interpretada de duas maneira, a primeira é especificando o nome de um arquivo `.py`, nesse caso o código do arquivo é executado diretamente (compilado para código de uma máquina virtual e depois executado).

A segunda maneira é sem especificar um arquivo, nesse caso é iniciado um modo interativo para avaliação de trechos de código (REPL - _Read, Eval, Print and Loop_). O _prompt_ padrão é identificado com `>>>`. O usuário digita um trecho de código, que é executado e o resultado é exibido em seguida. Por exemplo

```python
>>> 'Resposta ' + str(2 + 10 * 4)
'Resposta 42'
```

O modo interativo é uma ferramente muito valioso, pois permite a rápida exploração do funcionamento de trechos de código, mas é importante instruir o aluno a usar esse modo apenas para exploração e não desenvolvimento do código final (a escrita do programa deve ser feita seguindo o processo de projeto de programas).

Não é necessário uma função `main` em Python, mas é uma boa prática criar uma e chamá-la explicitamente (como no exemplo do início dessa seção -- [documentação](https://docs.python.org/3/library/__main__.html)).

O Python tem alguns mecanismos para escrita e execução de testes, o mais simples é o `doctest`. Os exemplos escritos na documentação podem ser executados e verificados de forma automática. Por exemplo, se a função `conta_no_intervalo` está em um arquivo chamado `x.py`, os exemplos podem ser verificados com o comando `python -m doctest -v x.py`.


## Sistema de tipos {#sistema-tipos}

C é comumente classificada como estaticamente tipada, isto é, os tipos são vinculados as variáveis em tempo de compilação (note que `void*`{.c} é um escape para essa regra que é amplamente utilizado pelas funções da biblioteca padrão, entre elas `malloc/free`{.c}). Os compiladores de C detectam muitos erros de tipo durante a compilação, no entanto, C permite a conversão implícita entre muitos tipos de dados, o que diminui a confiabilidade e pode ser confuso para o aluno iniciante (por exemplo, `double`{.c} e `float`{.c} para `int`{.c}, arranjos para ponteiros, etc).

Historicamente Python tem sido classificada como dinamicamente tipada, isto é, os tipos são vinculados em tempo de execução aos valores e não as variáveis. Isso implica, por exemplo, que uma mesma variável pode em momentos distintos armazenar valores de tipos distintos. Python tem algumas conversões implícitas de valores (como `int`{.python} para `float`{.python}), mas menos do que C, por esse e outro motivos, o Python é considerado mais fortemente tipada do que C.

Atualmente o Python suporta a especificação de tipos tanto de variáveis como funções. Essa característica foi adicionada inicialmente no Python 3.5 (lançado em 2015) e têm sido aprimorado a cada versão (o Python 2 não suporta especificação de tipos).

O interpretador padrão do [Python](https://python.org) ignora todas as anotações de tipos, mas existem ferramentes que podem utilizar essas anotações para fazer verificações estáticas no código. Uma dessas ferramentes é o [mypy](https://mypy-lang.org/). O `mypy` faz uma verificação estática dos tipos e indica os erros se forem encontrados. Por exemplo, no código a seguir, declaramos `n` como inteiro mas estamos atribuindo um ponto flutuante, o quê o `mypy` identifica como erro.

```python
n: int = 10.2
```

Dessa forma, usando o `mypy`, podemos desenvolver os programas em Python como se eles fossem estaticamente tipados, mesmo que de fato as anotações de tipo não alterem a forma como o programa é executado. O sistema de tipos do `mypy` é muito poderoso e permite expressar muitas restrições estaticamente. A seção \ref{exemplos} apresenta mais informações sobres essas questões.

Outro aspecto diferente entre as duas linguagem é que em C todos os tipos são "tipos valores", enquanto que em Python os tipos são ["tipos referências"](https://en.wikipedia.org/wiki/Value_type_and_reference_type).

O principal efeito dessa diferença é visto na atribuição e na passagem de parâmetros. Discutimos essa questão na próxima seção.


## Atribuição e passagem de parâmetros {#atribuicao}

Como em C "tudo é um valor", todas as atribuições e passagem de parâmetros são feitas por cópia (não tem referência!). No entanto, o uso de ponteiros permite contornar essa limitação, pois a cópia de um ponteiro em uma atribuição ou passagem de parâmetro acaba tendo o mesmo efeito de referências múltiplas para o mesmo valor.

Já em Python como ["tudo é uma referência"](https://docs.python.org/3/tutorial/classes.html#a-word-about-names-and-objects), todas as atribuições e passagem de parâmetros são feitas por referência. Os operadores `is`{.python} e `is not`{.python} são usados para verificar se duas referências são iguais (referenciam o mesmo objeto). O exemplo a seguir mostra como as atribuições de referências funcionam e a diferença entre igualdade de referências e igualdade de valores.

```python
>>> x = [1, 4]
>>> # y passa a referenciar o mesmo valor referenciado por x
>>> y = x
>>> # is produz True se as referências referenciam o mesmo valor
>>> x is y
True
>>> # == produz True se os dois valores referenciados são iguais
>>> x == y
True

>>> # Alteração da lista referenciada por x, que também é referenciada por y
>>> x.append(6)
>>> x
[1, 4, 6]
>>> y
[1, 4, 6]
>>> x is y
True
>>> x == y
True

>>> # x passa a referenciar uma nova lista, y não é alterado
>>> x = [1, 4, 6]
>>> y
[1, 4, 6]
>>> # Os valores referenciados por x e y são iguais
>>> x == y
True
>>> # As referências não são iguais
>>> x is y
False
>>> x is not y
True
```

Os tipos em Python podem ser mutáveis ou imutáveis. Os tipos numéricos, strings, tuplas, entre outros, são imutáveis, ou seja, dado uma referência para um número, não é possível alterar esse número. Já as listas, conjuntos, dicionários e outros tipos são mutáveis. Uma consequência disso é que se um valor de tipo imutável é passado como parâmetro, não é possível devolver um resultado alterando esse valor (pois o tipo é imutável). Outra consequência é que para passar parâmetros de tipos mutáveis por cópia, é preciso fazer a cópia explicitamente.

Considere por exemplo a implementação de uma função de busca em um TAD de dicionário (string para inteiro). Os argumentos de entrada seriam o dicionário e a chave e a saída uma indicação se existe um elemento associado com a chave e qual é o elemento. Em C poderíamos definir uma função que devolve `bool`{.c} indicando o resultado da busca e utilizar um argumento do tipo ponteiro para inteiro para indicar o elemento encontrado. A assinatura da função seria algo como

```c
/* Devolve true se chave está presente em dic, false caso contrário.
 * Se a chave estiver presente, o valor associado com a chave é
 * armazenado em valor.
 */
bool busca_chave(Dicionario *dic, char* chave, int* valor);
```

Em Python não é possível fazer dessa forma. Considere uma função com a assinatura

```python
def busca_chave(dic: Dicionario, chave: str, valor: int) -> bool
```

Se um inteiro é atribuído para `valor` no corpo de `busca_chave`, `valor` passa a referenciar um novo valor, o valor referenciado anteriormente permanece inalterado (números são imutáveis). Discutimos uma forma de resolver essa situação na seção \ref{uniao}.


## Gerência de memória

Os objetos em C podem ser alocados na pilha ou no heap. A alocação/desalocação dos objetos na pilha é feita automaticamente pelo ambiente de execução. Já a alocação/desalocação de objetos no heap deve ser feita de forma explícita pelo programador.

Em Python todos os objetos são alocados implicitamente no heap. A desalocação é feita de forma automática usando uma combinação de contagem de referências e coleta de lixo. Dessa forma, dois dos erros mais comuns em C, a tentativa de desalocação de memória já desalocada e a tentativa de uso de memória já desalocada, não podem acontecer em Python. (Um outro erro comum em C, acesso de arranjo fora do intervalo válido, é detectado e reportado com uma exceção em Python, conforme discutido na seção \ref{lista}).


## Biblioteca padrão

A biblioteca padrão de C é pequena quando comparada com outras linguagens. Ela inclui diversas funções para comunicação com o sistema operacional, alguns funções para strings e números e poucas funções algorítmicas e de estruturas de dados.

Por outro lado, o Python tem uma [biblioteca](https://docs.python.org/3/library/index.html) padrão bastante extensa, que é um dos motivos pelos quais o Python é bastante utilizado. No entanto, também pode ser motivo de preocupação para alguns professores, pois existe a possibilidade dos alunos usarem as funcionalidade prontas e deixarem de implementar coisas que são importante para o aprendizado. Mas isso não precisa ser dessa forma, o professor pode deixar claro o que pode ou não ser usado e o que é importante implementar.

Nas próximas duas seção mostramos um subconjunto do Python e como esse subconjunto pode ser usado na implementação de alguns algoritmos e estruturas de dados.


# O básico de Python

O Python é uma linguagem extensa e tem uma biblioteca padrão extensa. O sistema de tipos do `mypy` é bastante poderoso e também extenso. Dessa forma, para evitar que os alunos se percam na linguagem e deixem de focar nos fundamentos, é importante delimitar um subconjunto das construções da linguagem para ser utilizado pelos alunos.

Apresentamos a seguir um subconjunto suficiente para escrever qualquer algoritmo e estruturas de dados. Note que o código escrito nesse subconjunto pode ficar um pouco mais extenso e não ser considerado idiomático (pythônico) pela comunidade.


## Tipos e operações pré-definidas

Nesta seção apresentamos alguns dos principais tipos pré-definidos em Python e algumas operações sobre esses tipos.


### Números

Os principais [tipos numéricos](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex) em Python são `int`{.python} e `float`{.python}. O tipo `int`{.python} representa inteiros de tamanho arbitrário enquanto `float`{.python} números de pontos flutuantes no padrão IEEE 754 binary64 (mesmo que o `double`{.c} em C).

Além das quatro operações básicas, outras operações comuns em Python com números são a de módulo (`%`{.python}), exponenciação (`**`{.python}) e piso da divisão (`//`{.python}). O operador `/`{.python} sempre gera um `float`{.python} como resposta, mesmo que os operandos sejam inteiros. Outras operações matemáticas estão disponíveis no módulo [`math`](https://docs.python.org/3/library/math.html).

Em operações aritméticas que envolvem inteiros e floats, os inteiros são convertidos para floats antes da execução das operações.

A seguir mostramos alguns exemplos com valores numéricos.

```python
>>> # Soma
>>> 4 + 2
6
>>> 4 + 2.0
6.0

>>> # Divisão
>>> 7 / 2
3.5

>>> # Piso da divisão
>>> 14 // 3
4
>>> 5 // 1.3
3.0

>>> # Módulo
>>> 14 % 3
2
>>> 5 % 1.3
1.0999999999999999
>>> -7 % 5
3

>>> # Exponenciação
>>> 2 ** 80
1208925819614629174706176

>>> # Arredondamento, piso e teto
>>> round(3.4)
3
>>> round(3.5)
4
>>> round(3.5134, 2)
3.51
>>> import math
>>> math.floor(4.2)
4
>>> math.ceil(4.2)
5

>>> # Conversão entre int e float
>>> int(7.6)
7
>>> float(4)
4.0

>>> # Conversão entre números e str
>>> str(10)
'10'
>>> int('123')
123
>>> str(3.2)
'3.2'
>>> float('5.6')
5.6
```


### Booleano

Os [booleanos](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex) são representados pelo tipo `bool`{.python}, e podem assumir os valores `True`{.python} ou `False`{.python}. As operações com booleanos são `not`{.python}, `and`{.python} e `or`{.python}.

```python
>>> not 3 > 5
True
>>> 3 > 5 or 3 < 5
True
>>> # and tem prioridade sobre or
>>> True or False and False
True
```


### String

As [strings](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) são representadas pelo tipo `str`{.python} (armazenadas em utf-8). As strings são imutáveis em Python. Algumas operações comuns com strings são mostradas a seguir.

```python
>>> # Concatenação
>>> 'Idade: ' + str(32)
'Idade 32'

>>> # Repetição
>>> 'abc' * 3
'abcabcabc'
>>> 'nada' * -1
''

>>> # Remoção de espaços da direita e esquerda
>>> ' alguns espaços  '.strip()
'alguns espaços'
>>> # Versão com chamada na forma de função ao invés de método
>>> str.strip(' alguns espaços  ')
'alguns espaços'

>>> # Substring
>>> din = 'departamento de informatica'
>>> din[13:15] # inicio:fim, inicio está incluido e fim não está
'de'

>>> # Quantidade de caracteres (code points)
>>> len('teste')
5

>>> # Verificação se uma string é substring de outra
>>> 'este' in 'um teste simples'
True
```

O Python não tem um tipo específico para representar um caractere, strings com um _code point_ são usadas com esse propósito.

### Lista {#lista}

O tipo mais comum para [sequência](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range) de valores é o tipo `list`{.python}. O tipo `list`{.python} representa arranjos dinâmicos (os elementos são contíguos). Python não tem um tipo para arranjos de tamanho fixo. Os elementos de `list`{.python} são indexados a partir de `0`{.python} e o acesso é checado, se o índice estiver fora da faixa, uma exceção é lançada. Algumas operações com listas são mostradas a seguir.

```python
>>> # Inicialização com alguns elementos
>>> lst = [3, 2, 4]

>>> # Acesso pelo índice
>>> lst[0]
3
>>> lst[2]
4
>>> lst[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range

>>> # Quantidade de elementos - tempo O(1)
>>> len(lst)
3

>>> # Verificação de pertinência
>>> 2 in lst
True
>>> 7 in lst
False
>>> 7 not in lst
True

>>> # Inicialização sem nenhum elemento
>>> lst = [] # ou list()

>>> # Adição de elemento no final - tempo amortizado de O(1)
>>> lst.append(3)
>>> lst.append(1)
>>> lst
[3, 1]

>>> # Concatenação gerando uma nova lista
>>> lst + [4, 0, 1]
[3, 1, 4, 0, 1]

>>> # Concatenação alterando a lista
>>> lst.extend([4, 0, 1])
>>> lst
[3, 1, 4, 0, 1]

>>> # Remoção do final - tempo O(1)
>>> lst.pop()
1
>>> lst
[3, 1, 4, 0]

>>> # Criação de cópia - tempo O(n)
>>> lst.copy()
[3, 1, 4, 0]

>>> # Sublista
>>> lst[1:3]
[1, 4]

>>> # Remoção de todos os elementos - tempo O(n)
>>> lst.clear()
>>> lst
[]
```

&nbsp;

**Repetição de elementos**

Assim como o tipo `str`{.python}, o tipo `list`{.python} também tem operação de repetição de elementos. A operação de repetição é geralmente utilizada para inicializar uma lista com uma quantidade pré-definida de elementos, conforme os exemplos abaixo

```python
>>> # Inicialização com 5 zeros
>>> lst = [0] * 5
>>> lst
[0, 0, 0, 0, 0]

>>> # Modificação da lista
>>> lst[0] = 10
>>> lst[4] = 2
>>> lst
[10, 0, 0, 0, 2]
```

Apesar de útil, é preciso cuidado para usar essa operação. A questão é que, conforme discutido nas seções \ref{sistema-tipos} e \ref{atribuicao}, todos os tipos em Python são tipos referências, dessa forma, quando os elementos de uma lista são repetidos, o que de fato está sendo repetido (copiado) são as referências para os elementos. Para tipos imutáveis, como no exemplo anterior, isso não é um problema, mas para tipos mutáveis, isso pode gerar resultados inesperados.

A seguir apresentamos uma tentativa de inicializar uma lista de listas (arranjo bidimensional 3 x 3). Usamos a variável `linha` para ficar mais claro o ponto que vamos destacar, mas não é necessário, poderíamos escrever `m = [[0, 0, 0]] * 3`{.python} e a questão seria a mesmo.

```python
>>> linha = [0, 0, 0]
>>> m = [linha] * 3
>>> m
[[0, 0, 0], [0, 0, 0], [0, 0,0]]
```

Por enquanto, parece que está tudo certo. No entanto, se notarmos que `linha`, `m[0]`, `m[1]` e `m[2]` referenciam a mesma lista (objeto), então sabemos que uma mudança feita nessa lista por qualquer dessas variáveis, será refletida em todas as outras!

```python
>>> m[0][0] = 3
>>> m
[[3, 0, 0], [3, 0, 0], [3, 0, 0]]
```

Como proceder nesse caso? Existem formas compactas para fazer isso em Python, mas não são discutidas nesse texto. Usando apenas as construções mais básicas podemos fazer

```python
m = []
for i in range(3):
    linha = []
    for j in range(3):
        linha.append(0)
    m.append(linha)
```

&nbsp;

**Mistura de tipos**

Nós vimos na seção \ref{sistema-tipos} que Python é geralmente utilizada como uma linguagem com vinculação dinâmica de tipo, isto é, os tipos são associados aos valores, não as variáveis. Uma consequência desse fato é que os valores em uma lista podem ter tipos distintos, ou mesmo mudarem de tipo, como no exemplo a seguir:

```python
>>> # Mistura de valores com tipos diferentes
>>> lst = [10, 'casa', False]

>>> # Alteração do tipo do valor armazenado em 0
>>> lst[0] = [1, 2]
>>> lst
[[1, 2], 'casa', False]
```

Embora este tipo de construção seja permitida, ela pode ser confusa se usada de forma arbitrária, por isso é importante estabelecer um fundamento sobre a utilidade desse tipo de construção. Discutimos essa questão mais a fundo na seção \ref{uniao}, por hora, podemos usar anotações de tipos e o programa `mypy` para identificar como erro esse tipo de construção. Por exemplo, se quiséssemos restringir os tipos de `lst` para inteiros, escreveríamos:

```python
lst: list[int] = [10, 'casa', False]
```

Nesse caso, o seguinte erro seria gerado pelo `mypy`:

```txt
x.py:1: error: List item 1 has incompatible type "str"; expected "int"  [list-item]
Found 1 error in 1 file (checked 1 source file)
```

Note que só é possível usar o `mypy` para processar arquivos `.py`, não é possível utilizar o `mypy` no modo interativo. É importante lembrar também que o interpretador padrão do Python ignora as anotações de tipo, então, mesmo que o `mypy` indique erro, o código é executado pelo interpretador (gerando erro apenas quando uma operação é invocada com tipos inválidos).


## Estruturas de controle

Nessa seção apresentamos as principais estruturas de controle do Python.


### Funções

A funções em Python são definidas com a palavra chave `def`{.python}. A palavra chave `return`{.python} é usada para indicar o valor produzido pela função e pode ser usada mais que uma vez no corpo da função. Funções que não produzem valores explicitamente (não usam `return`{.python}) produzem como saída o valor `None`{.python}. O exemplo a seguir mostra a definição de uma função:

```python
includefile src/par.py
```


### Seleção

A principal estrutura de seleção do Python é o `if`{.python}, (o Python 3.10 introduziu a construção `match/case`{.python}, que é de certa forma similar ao `switch/case`{.c} do C, mas mais geral).

Assim como no `if`{.c} do C, a cláusula `else`{.python} do `if`{.python} do Python é opcional. Para evitar o aumento de níveis na indentação de `if`{.python}s aninhamos, o Python permite a união de um `else`{.python} seguido de `if`{.python} com a palavra chave `elif`{.python}. O exemplo a seguir mostra o uso do `if`{.python}.

```python
includefile src/sinal.py
```


### Repetição

O `for`{.c} clássico do C não existe em Python. Em Python o `for`{.python} é usado para fazer iteração pelos elementos de uma estrutura de dados. A outra forma de repetição do Python é o `while`{.python}. O exemplo a seguir mostra o uso do `for`{.python} e do `while`{.python}.

```python
includefile src/ordena.py
```

Quando for necessário o índice dos elementos em uma iteração, podemos usar o `while`{.python} ou o `for`{.python} combinado com a função [`range`](https://docs.python.org/3/library/stdtypes.html#typesseq-range).

```python
includefile src/indice_maximo.py
```


### Asserção

<!-- TODO: destacar o propósito -->

No exemplo anterior usamos uma outra estrutura de controle, o `assert`{.python}. O `assert`{.python} pode ser usado com uma ou duas expressões. O Python avalia a primeira expressão, se o resultador for `True`{.python}, a execução continua para a próxima linha, caso contrário, uma exceção é gerada com uma mensagem padrão ou com o resultado da segunda expressão do `assert`{.python} (se existir).


## Definição de tipos de dados

Python é uma linguagem multiparadigma, mas a forma de criação de tipos é através de classes. De fato, quase tudo em Python é um objeto, com atributos e métodos. Por exemplo, valores inteiros são objetos e podemos invocar métodos com eles:

```python
>>> # Invoca explicitamente o método __add__ (operação +)
>>> x = 1
>>> x.__add__(2) # ou (1).__add__(2)
3
```

Antes de vermos a forma geral de definição de classes, vamos ver formas simplificadas que são similares a definição de estruturas e enumerações em C.


### Estruturas

Uma forma simplificada de declarar uma classe é usando o decorador [`@dataclass`](https://docs.python.org/3/library/dataclasses.html). Um decorador é um mecanismo de meta-programação utilizado para modificar classes, métodos e atributos. Embora o funcionamento dos decoradores possa ser bastante elaborado, o seu uso em geral é direto e simples.

Utilizamos o decorador `@dataclass`{.python} quando queremos criar uma classe que se comporte de forma semelhante a uma `struct`{.c} em C. No exemplo a seguir criamos uma classe ponto e mostramos algumas operações:

```python
from dataclasses import dataclass

@dataclass
class Ponto:
    x: int
    y: int

>>> # Construtor
>>> p = Ponto(10, 20)
>>> p.x
10
>>> p.y
20
>>> p.x = p.x + 1
>>> p
Ponto(x=11, y=20)
>>> # Comparação
>>> p == Ponto(11, 20)
True
```

Observe que uma classe criada com `@dataclass`{.python} tem construtor (que recebe todos os campos como parâmetro na ordem que eles foram definidos), operador de igualdade (que compara os valores de cada campo), entre outras operações.

O exemplo a seguir mostra a definição de uma classe e o seu uso em uma função (note a forma dos exemplos):

```python
includefile src/futebol.py
```


### Enumerações

Outra forma simplificada para criar classe é usando o [Enum](https://docs.python.org/3/library/enum.html). Usamos essa forma quando queremos criar um tipo enumerado, isto é, quando os valores permitidos para o tipo podem ser enumerados explicitamente. Classes criadas com esse mecanismos são semelhantes a tipos criados com `enum`{.c} em C. No exemplo a seguir, mostramos a declaração e uso de uma classe para um tipo enumerado (observe que, por convenção, escrevemos os valores enumerados em maiúsculas).

```python
includefile src/cor.py
```

Além do campo `name`{.python}, cada valor da enumeração também tem um campo `value`{.python}, que é um valor inteiro gerado automaticamente (podemos indicar os valores de cada item da enumeração diretamente ao invés de usar o `auto`{.python} na declaração).

O `mypy` é bastante robusto na verificação do uso de enumerações, especialmente se todos os valores estão sendo considerados. Use o verificador [online](https://mypy-play.net) do `mypy` e teste o código anterior removendo algum dos casos e observe o erro gerado.


### Uniões {#uniao}

As [uniões](https://en.wikipedia.org/wiki/Tagged_union) em Python são discriminadas (contém um rótulo que diz o tipo armazenado) e são usadas em diversas situações. O operador `|` (a partir do Python 3.10) é utilizado para especificar a união de tipos e a função `isinstance`{.python} é utilizada para verificar o tipo (rótulo) de um valor. O exemplo a seguir mostra o uso de uniões (` \ `{.python} é usado para permitir que a expressão continue na próxima linha).

```python
includefile src/embalagem.py
```

Um uso bastante comum de uniões é na representação de ausência de valores. Por exemplo, um nó em uma lista pode ou não ter um próximo elemento, um nó em uma árvore binária pode ou não ter filhos a direita e a esquerda, uma busca em um dicionário pode ou não encontrar um valor associado com uma chave (veja a seção \ref{atribuicao}), etc. Para ilustrar de forma breve esse uso, mostramos a seguir a assinatura de uma função que busca o valor associado com uma chave em um dicionário, exemplos completos são apresentados na seção \ref{exemplos}.

```python
def busca_chave(dic: Dicionario, chave: str) -> None | int:
    '''Devolve o valor associado com chave em dic, ou None se chave não estiver em dic.'''
```

Assim como para enumerações, o `mypy` também é bastante robusto no tratamento de uniões devido a análise de [refinamento de tipos](https://mypy.readthedocs.io/en/stable/type_narrowing.html). No exemplo da embalagem, o `mypy` só permite que o campo `diametro`{.python} seja acessado através da variável `emb`{.python} se ele puder provar que `emb`{.python} é uma instância de `Rolo`{.python}, que é o caso no exemplo devido ao `if`{.python}. Use o verificador [online](https://mypy-play.net) do `mypy` e tente construções inválidas que só seriam identificas pelo Python em tempo de execução e veja que o `mypy` identifica essas construções de forma estática.

O refinamento de tipos é bastante útil, e nos permite, por exemplo, garantir que não haverá acesso a referências nulas em tempo de execução (como mostrado na seção \ref{exemplos})


### Classes {#classes}

A forma geral para definição de tipos em Python é a construção `class`{.python}. A seguir mostramos a declaração de uma classe e um método e alguns exemplos de uso:

```python
class Ponto:
    x: int
    y: int

    # Construtores que recebem parâmetros precisam ser criados explicitamente
    # O parâmetro self é a instância do ponto que está sendo inicializada
    def __init__(self, x: int, y: int):
        self.x = x
        self.y = y

    def distancia_origem(self) -> float: # self é uma referência para uma instância
        '''
        Calcula a distância do ponto a origem.
        Exemplos
        >>> p = Ponto(3, 4)
        >>> p.distancia_origem()
        5.0
        '''
        return (x ** 2 + y ** 2) ** 0.5

>>> p = Ponto(10, 20)
>>> p.x
10
>>> p.y
20
>>> p.x = p.x + 1
>>> # Por padrão, a representação do objeto é opaca
>>> p
<__main__.Ponto object at 0x7fad1fac8530>
>>> # Por padrão, a comparação é por referência e não pelo conteúdo
>>> p == Ponto(11, 20)
False
>>> p == p
True
```

Quando devemos usar `@dataclass`{.python} e classes "normais"? Usamos `@dataclass`{.python} quando queremos apenas agrupar dados e o encapsulamento não é importante. Classes "normais" devem ser usadas nas demais situações, como por exemplo, para criar tipos abstratos de dados (veja a seção \ref{tad}).


## Entrada e saída

A principal função de entrada em Python é a função `input`{.python}, que recebe um argumento (mensagem a ser exibida) e retorna uma linha lida (string) da entrada padrão. O Python não tem funções para fazer entrada de dados formatada.

O Python tem diversas funções de saída (incluindo saída formatada), a mais comum é a função `print`{.python}. A função `print`{.python} recebe um número variado de argumentos (de qualquer tipo), converte cada argumento para uma string com a função `str`{.python} e exibe os valores separando-os por espaço e adicionando final de linha (esse comportamento pode ser alterado).

O exemplo a seguir mostra como ler um nome e a data de nascimento de uma pessoa:

```python
nome = input('Qual é o seu nome?')
data = input('Qual é a sua data de nascimento?')
# Fazemos um processamento para obter o ano.
# Omitimos o tratamento de erro.
ano = int(data.split('/')[2])
print(nome + ',', 'em 2050 você terá', 2050 - ano, 'anos.')
```

Execução

```
Qual é o seu nome? João
Qual é a sua data de nascimento? 01/02/2003
João, em 2050 você terá 47 anos.
```


# Exemplos {#exemplos}

Nesta seção mostramos alguns exemplos de algoritmos e estruturas de dados implementados em Python.


## Ordenação por intercalação

O exemplo a seguir mostra a implementação do algoritmo de ordenação por intercalação (merge sort). Note que o código poderia ser simplificado utilizando sublistas (_slices_), mas a intenção do exemplo é mostrar a implementação usando apenas as construções que são comuns na maioria das linguagens.

```python
includefile src/ordena_intercalacao.py
```


## TAD dicionário implementado com funções

O exemplo a seguir mostra a implementação de um TAD dicionário usando busca linear e funções. Apenas as operações de inserção e busca são mostradas. Note que usamos `@dataclass`{.python} para a implementação ficar mais semelhante a que seria feita em C. Na próxima seção mostramos um exemplo que utiliza classe "normal" para enfatizar o encapsulamento.

```python
includefile src/dicionario.py
```


## TAD lista ordenada implementado com classes {#tad}

Nos exemplos a seguir mostramos a implementação de um TAD de lista ordenada usando encadeamento. No primeiro exemplo os nós são alocados dinamicamente. No segundo exemplo os nós são alocados quando a lista é criada (simulando alocação estática) e o encadeamento é feitos com índices (cursores). Note que são duas implementações diferentes para o mesmo TAD. Apenas a função de inserção é mostrada.

**Implementação com encadeamento ilimitado**

```python
includefile src/lista.py
```

**Implementação com encadeamento de tamanho máximo fixo**

```python
includefile src/lista_tamanho_fixo.py
```


# Convenções de código

Essas são algumas [convenções](https://peps.python.org/pep-0008/) oficiais para escrita de código em Python.

- Indente o código usando 4 espaços, não utilize tabulações;

- Nomeie tipos da forma `CapitalizedWords`;

- Nomeie funções e variáveis da forma `lower_case_with_underscores`;

- Nomeie constantes da forma `UPPER_CASE_WITH_UNDERSCORES`;

- Use um `import`{.python} por linha;

- Evite espaçamentos extras (`spam(ham[1], {eggs: 2})`{.python} ao invés de `spam( ham[ 1 ], { eggs: 2 } )`{.python});

- Use espaços entre operadores (`c += (a + b) * (a - b)`{.python} ao invés de `c+=(a+b)*(a-b)`{.python})


# Bibliografia

A seguir estão algumas referências bibliográficas que podem ser úteis tanto para os professores quanto para os alunos.

**Introdução a programação**

- [A Data-Centric Introduction to Computing](https://dcic-world.org/)

- [How to Design Programs](https://htdp.org/) ([vídeos](https://www.youtube.com/@systematicprogramdesign7962/playlists))

- [SICP in Python](https://wizardforcel.gitbooks.io/sicp-in-python/content/) ([vídeos](https://inst.eecs.berkeley.edu/~cs61a/fa22/))

- [Think Python 2e](https://greenteapress.com/wp/think-python-2e/) ([tradução](https://penseallen.github.io/PensePython2e/) em português)

- Notas de aula de [introdução a programação](https://malbarbo.pro.br/ensino/2023/6879/) e de [estruturas de dados](https://malbarbo.pro.br/ensino/2023/6884/) em Python (do mesmo autor desse texto)

**Estrutura de dados**

- [Open data structures](https://opendatastructures.org/ods-python.pdf) (em pseudo código que foi gerado automaticamente a partir do código Python)

**Linguagem Python**

- [Dive into Python 3](https://diveintopython3.net/)

