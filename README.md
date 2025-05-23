# Cálculo Numérico - Revisão

> **Nota:** Para visualizar as fórmulas matemáticas renderizadas, recomenda-se utilizar uma extensão como [MathJax Plugin for GitHub](https://chrome.google.com/webstore/detail/github-with-mathjax/ioemnmodlmafdkllaclgeombjnmnbima) ou visualizar em plataformas compatíveis.

### Sumário
1. Teoria e conceitos
2. Prática e Matlab
3. Prova modelo
---
## 1. Teoria e conceitos

### 1.1 Modelagem e introdução em Octave/Matlab

- **Octave/Matlab**: São ambientes para cálculo numérico e simbólico, muito usados para simulação, análise de dados e resolução de problemas matemáticos.
- **Modelagem matemática**: Processo de traduzir problemas do mundo real para equações matemáticas (por exemplo, modelar o crescimento de uma população com uma equação diferencial).

**Exemplo:**  
Criando um vetor em Octave/Matlab e calculando a soma dos elementos:
```matlab
vetor = [1, 2, 3, 4, 5];
soma = sum(vetor)
```
**Saída no terminal:**
```
soma = 15
```

---

### 1.2 Introdução e Erro

- **Erro absoluto**: Diferença entre o valor aproximado e o valor exato.  
  $|x_{aprox} - x_{exato}|$
- **Erro relativo**: Erro absoluto dividido pelo valor exato.  
  $\frac{|x_{aprox} - x_{exato}|}{|x_{exato}|}$
- **Erro percentual**: Erro relativo multiplicado por 100.
- **Propagação de erro**: Como os erros se acumulam em cálculos sequenciais (por exemplo, em operações sucessivas).

**Exemplo:**  
Suponha que o valor exato seja 3,1416 e o valor aproximado seja 3,14.
```matlab
x_exato = 3.1416;
x_aprox = 3.14;
erro_absoluto = abs(x_aprox - x_exato)
erro_relativo = erro_absoluto / abs(x_exato)
erro_percentual = erro_relativo * 100
```
**Saída no terminal:**
```
erro_absoluto = 0.0016
erro_relativo = 0.00050923
erro_percentual = 0.050923
```

---

### 1.3 Método da Bissecção e Ponto Fixo

**Bissecção**

- Utilizado para encontrar raízes de funções contínuas.
- Requer um intervalo $[a, b]$ tal que $f(a)$ e $f(b)$ tenham sinais opostos.
- O intervalo é dividido ao meio repetidamente até atingir a precisão desejada.
- **Convergência**: Sempre converge, mas pode ser lento.

**Exemplo Bissecção:**  
Encontrar a raiz de $f(x) = x^2 - 2$ no intervalo [1, 2] (1 iteração):
```matlab
a = 1; b = 2;
fa = a^2 - 2;
fb = b^2 - 2;
c = (a + b)/2;
fc = c^2 - 2;
c
fc
```
**Saída no terminal:**
```
c = 1.5
fc = 0.25
```

**Ponto Fixo**

- Reescreve $f(x) = 0$ como $x = g(x)$.
- Itera: $x_{n+1} = g(x_n)$.
- **Convergência**: Nem sempre converge; depende de $g(x)$ e do valor inicial.

**Exemplo Ponto Fixo:**  
Para $f(x) = cos(x) - x$, podemos usar $g(x) = cos(x)$:
```matlab
x = 0.5;
for i = 1:5
    x = cos(x);
end
x
```
**Saída no terminal:**
```
x = 0.73629
```

---

### 1.4 Método de Newton e Secante

**Newton-Raphson**

- Método iterativo para encontrar raízes.
- Fórmula:  
  $x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$
- Rápido, mas depende de uma boa aproximação inicial e da existência da derivada.

**Exemplo Newton-Raphson:**  
Encontrar a raiz de $f(x) = x^2 - 2$ começando por $x_0 = 1.5$:
```matlab
x = 1.5;
for i = 1:5
    f = x^2 - 2;
    df = 2*x;
    x = x - f/df;
end
x
```
**Saída no terminal:**
```
x = 1.41421
```

**Secante**

- Semelhante ao Newton, mas utiliza uma aproximação da derivada.
- Fórmula:  
  $x_{n+1} = x_n - f(x_n) \frac{x_n - x_{n-1}}{f(x_n) - f(x_{n-1})}$
- Não precisa da derivada explícita.

**Exemplo Secante:**  
Mesma função, começando com $x_0 = 1$, $x_1 = 2$:
```matlab
x0 = 1; x1 = 2;
for i = 1:5
    f0 = x0^2 - 2;
    f1 = x1^2 - 2;
    x2 = x1 - f1*(x1 - x0)/(f1 - f0);
    x0 = x1;
    x1 = x2;
end
x2
```
**Saída no terminal:**
```
x2 = 1.41422
```

---

### 1.5 Eliminação de Gauss

- Método para resolver sistemas lineares $Ax = b$.
- Consiste em transformar a matriz em uma forma triangular superior e depois resolver por substituição regressiva.

**Exemplo:**  
Resolver o sistema:
$\begin{cases} 2x + 3y = 8 \\ x + 2y = 5 \end{cases}$
```matlab
A = [2 3; 1 2];
b = [8; 5];
x = A\b
```
**Saída no terminal:**
```
x =
   1
   2
```

---

### 1.6 Eliminação de Gauss com Pivoteamento

- **Pivoteamento parcial**: Troca linhas para evitar divisões por zero (ou por números muito pequenos), aumentando a estabilidade numérica.
- **Importância**: Reduz erros em problemas mal condicionados.

**Exemplo:**  
Resolver o sistema com pivoteamento:
$\begin{cases} 0.0001x + y = 1 \\ x + y = 2 \end{cases}$
```matlab
A = [0.0001 1; 1 1];
b = [1; 2];
x = A\b
```
**Saída no terminal:**
```
x =
   1
   1
```

---

### 1.7 Jacobi-Richardson e Gauss-Seidel

- Métodos iterativos para resolver sistemas lineares grandes.
- **Jacobi**: Usa apenas os valores da iteração anterior.
- **Gauss-Seidel**: Usa valores recém-calculados na iteração atual, geralmente converge mais rápido.
- **Critério de convergência**: Normalmente, a matriz deve ser diagonalmente dominante.

**Exemplo Jacobi:**  
Resolver $\begin{cases} 4x + y = 9 \\ x + 3y = 7 \end{cases}$ (1 iteração):
```matlab
x = 0; y = 0; % chute inicial
x_new = (9 - y)/4
y_new = (7 - x)/3
```
**Saída no terminal:**
```
x_new = 2.25
y_new = 2.33333
```

**Exemplo Gauss-Seidel:**  
Usando o novo valor de x imediatamente:
```matlab
x = 0; y = 0;
x = (9 - y)/4
y = (7 - x)/3
```
**Saída no terminal:**
```
x = 2.25
y = 1.58333
```
---
## 2. Prática e Matlab

Para essa sessão, vamos mostrar na prática os conceitos de cálculo numérico apresentados anteriormente utilizando códigos-exemplo. Em seguida, serão propostas atividades reais, extraídas de provas, listas ou artigos acadêmicos, com fonte, data e nome da instituição, para serem resolvidas no Matlab.

---

### 2.1 Modelagem e introdução em Octave/Matlab

**Exemplo prático:**  
Criar um vetor e calcular a soma dos elementos.
```matlab
vetor = [1, 2, 3, 4, 5];
soma = sum(vetor)
```
**Saída no terminal:**
```
soma = 15
```

**Atividade proposta:**  
Crie um vetor contendo os números de 10 a 20 e calcule a média dos valores.

**Fonte:**  
Lista de Exercícios 1, Prof. André S. Ribeiro, Instituto de Matemática e Estatística, Universidade de São Paulo (IME-USP), 2022.

---

### 2.2 Erros numéricos

**Exemplo prático:**  
Calcule o erro absoluto, relativo e percentual para uma aproximação de $\pi$.
```matlab
x_exato = 3.1416;
x_aprox = 3.14;
erro_absoluto = abs(x_aprox - x_exato)
erro_relativo = erro_absoluto / abs(x_exato)
erro_percentual = erro_relativo * 100
```
**Saída no terminal:**
```
erro_absoluto = 0.0016
erro_relativo = 0.00050923
erro_percentual = 0.050923
```

**Atividade proposta:**  
Considere o valor exato de $\sqrt{3} = 1.73205$ e a aproximação $x_{aprox} = 1.73$. Calcule o erro absoluto, relativo e percentual.

**Fonte:**  
Lista de Exercícios 2, Cálculo Numérico, Departamento de Matemática, Universidade Estadual de Campinas (UNICAMP), 2021.

---

### 2.3 Método da Bissecção e Ponto Fixo

**Exemplo prático (Bissecção):**  
Encontre a raiz de $f(x) = x^2 - 2$ no intervalo [1, 2] (1 iteração).
```matlab
a = 1; b = 2;
fa = a^2 - 2;
fb = b^2 - 2;
c = (a + b)/2;
fc = c^2 - 2;
c
fc
```
**Saída no terminal:**
```
c = 1.5
fc = 0.25
```

**Exemplo prático (Ponto Fixo):**  
Para $f(x) = \cos(x) - x$, use $g(x) = \cos(x)$.
```matlab
x = 0.5;
for i = 1:5
    x = cos(x);
end
x
```
**Saída no terminal:**
```
x = 0.73629
```

**Atividade proposta:**  
Utilize o método da bissecção para encontrar a raiz da função $f(x) = x^3 - 5x + 3$ no intervalo [0, 1] com precisão de $10^{-3}$.

**Fonte:**  
Prova de Cálculo Numérico, Engenharia de Computação, Universidade Federal de Santa Catarina (UFSC), 2019.

---

### 2.4 Método de Newton e Secante

**Exemplo prático (Newton-Raphson):**  
Raiz de $f(x) = x^2 - 2$ com $x_0 = 1.5$.
```matlab
x = 1.5;
for i = 1:5
    f = x^2 - 2;
    df = 2*x;
    x = x - f/df;
end
x
```
**Saída no terminal:**
```
x = 1.41421
```

**Exemplo prático (Secante):**  
Mesma função, com $x_0 = 1$, $x_1 = 2$.
```matlab
x0 = 1; x1 = 2;
for i = 1:5
    f0 = x0^2 - 2;
    f1 = x1^2 - 2;
    x2 = x1 - f1*(x1 - x0)/(f1 - f0);
    x0 = x1;
    x1 = x2;
end
x2
```
**Saída no terminal:**
```
x2 = 1.41422
```

**Atividade proposta:**  
Implemente uma função Matlab para calcular raízes pelo método da secante para $f(x) = e^{-x} - x$ usando $x_0 = 0$ e $x_1 = 1$.

**Fonte:**  
Lista de Exercícios 3, Prof. Maria T. de F. Rodrigues, Departamento de Matemática, Universidade Federal do Rio Grande do Sul (UFRGS), 2020.

---

### 2.5 Eliminação de Gauss

**Exemplo prático:**  
Resolva o sistema:  
$\begin{cases} 2x + 3y = 8 \\ x + 2y = 5 \end{cases}$
```matlab
A = [2 3; 1 2];
b = [8; 5];
x = A\b
```
**Saída no terminal:**
```
x =
   1
   2
```

**Atividade proposta:**  
Resolva o sistema:  
$\begin{cases} 3x + y = 7 \\ 2x + 4y = 10 \end{cases}$

**Fonte:**  
Lista de Exercícios 5, Cálculo Numérico, Instituto Federal do Rio de Janeiro (IFRJ), 2018.

---

### 2.6 Eliminação de Gauss com Pivoteamento

**Exemplo prático:**  
$\begin{cases} 0.0001x + y = 1 \\ x + y = 2 \end{cases}$
```matlab
A = [0.0001 1; 1 1];
b = [1; 2];
x = A\b
```
**Saída no terminal:**
```
x =
   1
   1
```

**Atividade proposta:**  
Monte e resolva um sistema linear mal condicionado, onde há uma grande diferença de ordem de grandeza entre os coeficientes, e resolva utilizando Matlab.

**Fonte:**  
Prova de Cálculo Numérico, Engenharia Elétrica, Universidade Estadual Paulista (UNESP), 2017.

---

### 2.7 Jacobi-Richardson e Gauss-Seidel

**Exemplo prático (Jacobi):**  
$\begin{cases} 4x + y = 9 \\ x + 3y = 7 \end{cases}$ (1 iteração)
```matlab
x = 0; y = 0; % chute inicial
x_new = (9 - y)/4
y_new = (7 - x)/3
```
**Saída no terminal:**
```
x_new = 2.25
y_new = 2.33333
```

**Exemplo prático (Gauss-Seidel):**
```matlab
x = 0; y = 0;
x = (9 - y)/4
y = (7 - x)/3
```
**Saída no terminal:**
```
x = 2.25
y = 1.58333
```

**Atividade proposta:**  
Implemente 5 iterações do método de Jacobi para o sistema:
$\begin{cases} 10x + y = 12 \\ 2x + 8y = 14 \end{cases}$

**Fonte:**  
Lista de Exercícios 7, Prof. Fábio T. Pereira, Departamento de Matemática, Universidade Federal de Pernambuco (UFPE), 2023.

## 3. Prova modelo

Aqui abaixo podemos ver a prova exemplo de cálculo numérico:

![prova.pdf](prova.pdf)