# Passo a Passo para a execução do projeto

Para executar o projeto, siga estes passos simples:

1 - Baixe o arquivo `estrutura_van_Emde_Boas_(vEB).ipynb`;

2 - Acesse sua conta no <a href="https://colab.google/">Google Colab</a> e faça o upload do notebook.

3 - Antes de executar qualquer célula, crie e faça o upload de um arquivo `entrada.txt` contendo os comandos para a árvore van Emde Boas.

4 - No notebook, execute a célula principal (“bloco de código”):

5 - Ao rodar o Bloco, será gerado automaticamente um arquivo `saida.txt`. Faça o download desse arquivo para conferir os resultados.

A seguir, você encontra vários exemplos de `entrada.txt` e a saída esperada em `output.txt`. Sinta-se à vontade para criar seus próprios casos de teste, seguindo o mesmo formato.


### Exemplo
Entrada (coloque estas linhas em `entrada.txt`):
```
INC 20
INC 30
INC 40
INC 50
INC 70
INC 140
INC 150
IMP
SUC 50
PRE 50
SUC 140
PRE 20
REM 70
IMP
SUC 50
PRE 30
```
Este conjunto de comandos faz, passo a passo:

- INC 20 → insere o valor 20.
- INC 30 → insere o valor 30.
- INC 40 → insere o valor 40.
- INC 50 → insere o valor 50.
- INC 70 → insere o valor 70.
- INC 140 → insere o valor 140.
- INC 150 → insere o valor 150.
- IMP → Imprime os valores em sequência.
- SUC 50 → pede o sucessor de 50.
- PRE 50 → pede o antessesor de 50.
- SUC 140 → pede o sucessor de 140.
- PRE 20 → pede o antessesor de 20.
- REM 70 → remove o valor 70 da árvore.
- IMP → Imprime os valores em sequência.
- SUC 50 → pede o sucessor de 50.
- PRE 30 → pede o antessesor de 30.

Saída esperada (`saida.txt`):

```
SUC 50 3
infinito
IMP 3
13,1,R 42,0,N 50,1,R
IMP 6
13,1,N 50,0,N 52,1,N 65,2,

INC 20
INC 30
INC 40
INC 50
INC 70
INC 140
INC 150
IMP
Min: 20, C[0]: 30, 40, 50, 70, 140, 150
SUC 50
70
PRE 50
40
SUC 140
150
PRE 20
None
REM 70
IMP
Min: 20, C[0]: 30, 40, 50, 140, 150
SUC 50
140
PRE 30
20
```
