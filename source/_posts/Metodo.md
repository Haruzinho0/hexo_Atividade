---
title: Método
date: 2024-06-22 22:18:22
tags:
---
### Métodos de Interpolação

#### Método de Lagrange

O Método de Lagrange é um dos métodos de interpolação mais antigos e bem conhecidos. Ele permite a construção de um polinômio de interpolação que passa exatamente pelos pontos dados. A fórmula de Lagrange é simples e direta, tornando-se uma ferramenta poderosa para a interpolação de dados.
Formulas para realizar os métodos:
![](/images/Formula_lagrange.png)
![](/images/Formula_lagrange2.png)

<!-- more -->

Código em JS do método de Lagrange:
``` bash
$ function lagrangeInterpolation(x, y, value) {
    let n = x.length;
    let steps = document.getElementById('steps');
    steps.innerHTML = '';

    steps.innerHTML += `<p>Valores dos pontos:</p>`;
    for (let i = 0; i < n; i++) {
        steps.innerHTML += `<p>x${i} = ${x[i]}, y${i} = ${y[i]}</p>`;
    }

    let interpolation = 0;
    let polynomialTerms = `P(${value}) = `;

    for (let i = 0; i < n; i++) {
        let term = y[i];
        let termDetails = `${y[i]}`;
        
        for (let j = 0; j < n; j++) {
            if (j !== i) {
                term *= (value - x[j]) / (x[i] - x[j]);
                let intermediateTerm = (value - x[j]) / (x[i] - x[j]);
                if (intermediateTerm < 0) {
                    termDetails += ` * (${intermediateTerm})`;
                } else {
                    termDetails += ` * ${intermediateTerm}`;
                }
            }
        }

        polynomialTerms += ` + ${termDetails}`;
        interpolation += term;
    }

    steps.innerHTML += `<p>${polynomialTerms}</p>`;
    return interpolation;
}

```

#### Método de Newton

O Método de Newton, por outro lado, oferece uma abordagem alternativa para a interpolação, utilizando diferenças divididas. Este método é particularmente útil quando se deseja adicionar novos pontos ao conjunto de dados existente, pois o polinômio de Newton pode ser facilmente ajustado sem a necessidade de recalcular todo o polinômio desde o início.
Formulas para realizar os métodos:
![](/images/Formula_newton.png)
![](/images/Formula_newton2.png)


Código em JS do método de Newton:
``` bash
$ function newtonInterpolation(x, y, value) {
    let n = x.length;
    let dividedDifferences = [];
    let steps = document.getElementById('steps');
    steps.innerHTML = '';

    steps.innerHTML += `<p>Valores dos f[x,0]:</p>`;
    for (let i = 0; i < n; i++) {
        dividedDifferences.push([y[i]]);
        steps.innerHTML += `<p>f[${i},0] = ${y[i]}</p>`;
    }

    steps.innerHTML += `<p>Valores dos pontos:</p>`;
    for (let i = 0; i < n; i++) {
        steps.innerHTML += `<p>x${i} = ${x[i]}, y${i} = ${y[i]}</p>`;
    }

    for (let j = 1; j < n; j++) {
        for (let i = 0; i < n - j; i++) {
            let value = (dividedDifferences[i + 1][j - 1] - dividedDifferences[i][j - 1]) / (x[i + j] - x[i]);
            if (!dividedDifferences[i][j]) {
                dividedDifferences[i].push(value);
            } else {
                dividedDifferences[i][j] = value;
            }
            steps.innerHTML += `<p>f[${i},${j}] = (f[${i + 1},${j - 1}] - f[${i},${j - 1}]) / (${x[i + j]} - ${x[i]}) = ${value}</p>`;
        }
    }

    let interpolation = dividedDifferences[0][0];
    let polynomialTerms = `P(${value}) = ${interpolation}`;
    
    for (let i = 1; i < n; i++) {
        let term = dividedDifferences[0][i];
        let termDetails = `${term}`;

        if (term < 0) {
            termDetails = `(${term})`;
        }
        
        for (let j = 0; j < i; j++) {
            term *= (value - x[j]);
            let intermediateTerm = (value - x[j]);
            if (intermediateTerm < 0) {
                termDetails += ` * (${intermediateTerm})`;
            } else {
                termDetails += ` * ${intermediateTerm}`;
            }
        }
        
        polynomialTerms += ` + ${termDetails}`;
        interpolation += term;
    }

    steps.innerHTML += `<p>${polynomialTerms}</p>`;
    return interpolation;
}
```