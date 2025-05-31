# Passo a Passo para a execução do projeto

Para executar o projeto, siga estes passos simples:

1 - Baixe o arquivo `arvore_rubro_negra_parcialmente_persistente.ipynb`;

2 - Acesse sua conta no <a href="https://colab.google/">Google Colab</a> e faça o upload do notebook.

3 - Antes de executar qualquer célula, crie e faça o upload de um arquivo `input.txt` contendo os comandos para a árvore rubro-negra.

4 - No notebook, execute as duas células principais (os dois “blocos de código”):
  - Bloco 1 — Definições iniciais, Classe Node, Controle de versões, Funções Auxiliares e Funções principais
  - Bloco 2 — Entrada/Saída

5 - Ao rodar o Bloco 2, será gerado automaticamente um arquivo `output.txt`. Faça o download desse arquivo para conferir os resultados.

A seguir, você encontra vários exemplos de `input.txt` e a saída esperada em `output.txt`. Sinta-se à vontade para criar seus próprios casos de teste, seguindo o mesmo formato.


### Exemplo 1 - Inserções sequenciais, consulta de sucessor e impressão antes/depois da remoção
Entrada (coloque estas linhas em `input.txt`):
```
INC 13
INC 42
INC 50
INC 52
INC 65
SUC 50 3
IMP 3
REM 42
IMP 6
```
Este conjunto de comandos faz, passo a passo:

- INC 13 → insere o valor 13 na versão 1.
- INC 42 → insere o valor 42 na versão 2.
- INC 50 → insere o valor 50 na versão 3.
- INC 52 → insere o valor 52 na versão 4.
- INC 65 → insere o valor 65 na versão 5.
- SUC 50 3 → pede o sucessor de 50 na versão 3 (que contém apenas {13, 42, 50}).
- IMP 3 → imprime a árvore da versão 3 em ordem crescente, mostrando (valor, profundidade, cor).
- REM 42 → remove o elemento 42 da versão 5, gerando a versão 6.
- IMP 6 → imprime a árvore da versão 6 (já sem o 42), em ordem, com profundidades e cores.

Saída esperada (`output.txt`):

```
SUC 50 3
infinito
IMP 3
13,1,R 42,0,N 50,1,R
IMP 6
13,1,N 50,0,N 52,1,N 65,2,
```

- SUC 50 3 → infinito: na versão 3 a árvore tem apenas {13, 42, 50}; não existe valor estritamente maior que 50, portanto o sucessor é “infinito”.
- IMP 3 → 13,1,R 42,0,N 50,1,R: na versão 3, o nó 42 é raiz (profundidade 0, cor N), com filhos 13 (profundidade 1, cor R) e 50 (profundidade 1, cor R).
- IMP 6 → 13,1,N 50,0,N 52,1,N 65,2,R: após remover o 42 (versão 6), a raiz passa a ser 50 (profundidade 0, cor N). À esquerda, 13 (profundidade 1, cor N). À direita, 52 (profundidade 1, cor N) e, abaixo de 52, 65 (profundidade 2, cor R).

### Exemplo 2 - Remoção de elemento inexistente
Entrada (coloque estas linhas em `input.txt`):
```
INC 10
INC 20
INC 30
REM 25
IMP 3
SUC 15 10
```
A sequência faz:

- INC 10 → versão 1 passa a conter {10}.
- INC 20 → versão 2 passa a conter {10, 20}.
- INC 30 → versão 3 passa a conter {10, 20, 30}.
- REM 25 → como 25 não está presente, a versão 4 é idêntica à 3 ({10, 20, 30}).
- IMP 3 → imprime a árvore na versão 3.
- SUC 15 10 → pede o sucessor de 15 na versão especificada (“10”), mas a versão 10 não existe; o código faz fallback para a versão mais recente (no caso, 3) e retorna o menor valor > 15.

Saída esperada (`output.txt`):
```
IMP 3
10,1,R 20,0,N 30,1,R
SUC 15 10
20 
```
- IMP 3 → 10,1,R 20,0,N 30,1,R: na versão 3, 20 é raiz (profundidade 0, cor N), com filhos 10 (profundidade 1, cor R) e 30 (profundidade 1, cor R).
- SUC 15 10 → 20: a versão 10 não existe; portanto, usa‐se a versão mais recente (3). O menor valor maior que 15 em {10, 20, 30} é 20.

### Exemplo 3 - Impressão de versão vazia e inserções múltiplas
Entrada (coloque estas linhas em `input.txt`):
```
IMP 0
INC 5
INC 3
INC 7
INC 1
INC 4
IMP 4
SUC 6 4
```
Neste caso:
- IMP 0 → tenta imprimir a versão 0, que é a árvore vazia.
- INC 5 → versão 1: {5}.
- INC 3 → versão 2: {5, 3}.
- INC 7 → versão 3: {5, 3, 7}.
- INC 1 → versão 4: {5, 3, 7, 1}.
- INC 4 → versão 5: {5, 3, 7, 1, 4}.
- IMP 4 → imprime a árvore da versão 4 em ordem.
- SUC 6 4 → pede o sucessor de 6 na versão 4 ({5, 3, 7, 1}); o menor valor > 6 é 7.

Saída esperada (`output.txt`):
```
IMP 0

IMP 4
1,2,R 3,1,R 4,2,B 5,0,N 7,1,R
SUC 6 4
7
```
- IMP 0 → (linha em branco): a versão 0 é vazia, então não há nós para imprimir (apenas a tag “IMP 0”).
- IMP 4 → 1,2,R 3,1,R 4,2,B 5,0,N 7,1,R: na versão 4 a árvore é reequilibrada como Rubro-Negra: 5 é raiz (profundidade 0, cor N); à esquerda, 3 (profundidade 1, cor R), cujos filhos são 1 (profundidade 2, cor R) e 4 (profundidade 2, cor B); à direita, 7 (profundidade 1, cor R).
- SUC 6 4 → 7: o sucessor de 6 na versão 4 é 7, pois é o menor valor maior que 6.

### Exemplo 4 - Remover raiz e verificar rebalanceamento
Entrada (coloque estas linhas em `input.txt`):
```
INC 8
INC 4
INC 12
INC 2
INC 6
INC 10
INC 14
IMP 7
REM 8
IMP 8
```
A sequência faz:
- INC 8 → versão 1: {8}.
- INC 4 → versão 2: {8, 4}.
- INC 12 → versão 3: {8, 4, 12}.
- INC 2 → versão 4: {8, 4, 12, 2}.
- INC 6 → versão 5: {8, 4, 12, 2, 6}.
- INC 10 → versão 6: {8, 4, 12, 2, 6, 10}.
- INC 14 → versão 7: {8, 4, 12, 2, 6, 10, 14}.
- IMP 7 → imprime a árvore da versão 7, em que 8 é raiz.
- REM 8 → remove o nó raiz (8) na versão 7, produzindo a versão 8 e provocando rebalanceamento para que o novo raiz seja correto (normalmente 6 ou 10, conforme as regras Rubro-Negras).
- IMP 8 → imprime a estrutura resultante da versão 8.

Saída esperada (`output.txt`):
```
IMP 7
2,2,R 4,1,R 6,0,N 8,1,R 10,1,R 12,2,B 14,3,R
IMP 8
2,1,N 4,0,B 6,1,N 10,1,N 12,0,B 14,1,N
```
- IMP 7 → 2,2,R 4,1,R 6,0,N 8,1,R 10,1,R 12,2,B 14,3,R: na versão 7, a inserção em ordem crescente forma uma árvore com raiz 6 (profundidade 0, cor N), mas após rotações internas, 6 se torna raiz e 8 passa a ser filho direito de 6, etc. A profundidade e cores apresentadas refletem o rebalanceamento padrão.
- IMP 8 → 2,1,N 4,0,B 6,1,N 10,1,N 12,0,B 14,1,N: ao remover o 8, o novo arranjo faz 4 se tornar raiz (profundidade 0, cor B) e 6 se reorganizar como filho direito (profundidade 1, cor N), mantendo as propriedades Rubro-Negras válidas.

> Observação: devido às várias opções de sequência de rotações e recolorações, as cores podem variar ligeiramente, mas a ordem em‐itinerante e a profundidade de cada nó devem obedecer às regras da árvore Rubro-Negra.

### Exemplo 5 - Uso de versão inexistente em IMP e SUC
Entrada (coloque estas linhas em `input.txt`):
```
INC 100
INC 200
IMP 5
SUC 50 0
SUC 150 1
REM 100
SUC 50 10
```
Neste conjunto:
- INC 100 → versão 1: {100}.
- INC 200 → versão 2: {100, 200}.
- IMP 5 → tenta imprimir a versão 5. Como ela não existe, faz fallback para a versão mais recente (2) e imprime {100, 200}.
- SUC 50 0 → pede o sucessor de 50 na versão 0 (vazia). A versão 0 não tem nós, então faz fallback para a versão mais recente válida (2). O menor valor > 50 em {100, 200} é 100.
- SUC 150 1 → pede o sucessor de 150 na versão 1 ({100}). Como não existe valor maior que 150 em {100}, retorna “infinito”.
- REM 100 → versão 3: remove 100, sobrando apenas {200}.
- SUC 50 10 → tenta a versão 10 (inexistente). Faz fallback para a versão mais recente (3), em que só existe {200}. Logo, o sucessor de 50 é 200.

Saída esperada (`output.txt`):
```
IMP 5
100,0,N 200,1,R
SUC 50 0
100
SUC 150 1
200
SUC 50 10
200
```
- IMP 5 → 100,0,N 200,1,R: a versão 5 não existe; usa‐se a versão 2. 100 é raiz (profundidade 0, cor N) e 200 é filho direito (profundidade 1, cor R).
- SUC 50 0 → 100: a versão 0 é vazia; fallback leva à versão 2. O menor valor > 50 é 100.
- SUC 150 1 → infinito: na versão 1 a única chave é 100; não há valor > 150, portanto “infinito”.
- SUC 50 10 → 200: a versão 10 não existe; fallback leva à versão 3 ({200}); o único valor > 50 é 200.

## 👨‍💻 Contribuidores

🚀 Equipe de desenvolvimento

<table>
  <tr align="center">
    <td>
      <a href="https://github.com/daviteixeira-dev">
        <img src="https://avatars.githubusercontent.com/daviteixeira-dev" width=100 />
        <p>Davi <br/>Teixeira</p>
      </a>
    </td>
    <td>
      <a href="https://github.com/LeonardoQuezado">
        <img src="https://avatars.githubusercontent.com/LeonardoQuezado" width=100 />
        <p>Leonardo <br/>Quezado</p>
      </a>
    </td>
  </tr>
</table>
