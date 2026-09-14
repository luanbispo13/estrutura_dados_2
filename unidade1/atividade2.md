# Soluções: Arrays, Matrizes e Structs
## Estrutura de Dados II — Aula 02

---

## Exercício 1 — Soma de um Vetor

**Objetivo:** Ler 10 números inteiros, apresentar todos, a soma e a média.

```cpp
#include <iostream>
using namespace std;

int main() {
    int numeros[10];
    int soma = 0;
    
    // Leitura dos dados
    cout << "Digite 10 numeros inteiros:\n";
    for (int i = 0; i < 10; i++) {
        cout << "Numero " << (i + 1) << ": ";
        cin >> numeros[i];
        soma += numeros[i];
    }
    
    // Exibição dos números
    cout << "\nNumeros lidos:\n";
    for (int i = 0; i < 10; i++) {
        cout << numeros[i] << " ";
    }
    
    // Cálculos
    float media = soma / 10.0;
    
    cout << "\n\nSoma: " << soma;
    cout << "\nMedia: " << media << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
Digite 10 numeros inteiros:
Numero 1: 5
Numero 2: 8
...
Numeros lidos:
5 8 3 9 7 4 6 2 10 1

Soma: 55
Media: 5.50
```

---

## Exercício 2 — Maior e Menor

**Objetivo:** Ler 10 números, determinar maior, menor e suas posições.

```cpp
#include <iostream>
using namespace std;

int main() {
    int numeros[10];
    
    // Leitura dos dados
    cout << "Digite 10 numeros inteiros:\n";
    for (int i = 0; i < 10; i++) {
        cout << "Numero " << (i + 1) << ": ";
        cin >> numeros[i];
    }
    
    // Inicializar maior e menor com o primeiro elemento
    int maior = numeros[0];
    int menor = numeros[0];
    int posicao_maior = 0;
    int posicao_menor = 0;
    
    // Encontrar maior e menor
    for (int i = 1; i < 10; i++) {
        if (numeros[i] > maior) {
            maior = numeros[i];
            posicao_maior = i;
        }
        if (numeros[i] < menor) {
            menor = numeros[i];
            posicao_menor = i;
        }
    }
    
    // Exibir resultados
    cout << "\nMaior valor: " << maior << " (posicao " << posicao_maior << ")\n";
    cout << "Menor valor: " << menor << " (posicao " << posicao_menor << ")" << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
Digite 10 numeros inteiros:
Numero 1: 15
Numero 2: 3
...

Maior valor: 50 (posicao 7)
Menor valor: 2 (posicao 3)
```

---

## Exercício 3 — Números Pares

**Objetivo:** Ler 20 números, mostrar pares, contar e somar.

```cpp
#include <iostream>
using namespace std;

int main() {
    int numeros[20];
    int soma_pares = 0;
    int quantidade_pares = 0;
    
    // Leitura dos dados
    cout << "Digite 20 numeros inteiros:\n";
    for (int i = 0; i < 20; i++) {
        cout << "Numero " << (i + 1) << ": ";
        cin >> numeros[i];
    }
    
    // Processar pares
    cout << "\nNumeros pares: ";
    for (int i = 0; i < 20; i++) {
        if (numeros[i] % 2 == 0) {
            cout << numeros[i] << " ";
            soma_pares += numeros[i];
            quantidade_pares++;
        }
    }
    
    // Exibir resultados
    cout << "\n\nQuantidade de pares: " << quantidade_pares;
    cout << "\nSoma dos pares: " << soma_pares << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
Digite 20 numeros inteiros:
Numero 1: 2
Numero 2: 5
...

Numeros pares: 2 4 8 12 16 20 ...

Quantidade de pares: 10
Soma dos pares: 60
```

---

## Exercício 4 — Inversão de Vetor (Desafio)

**Objetivo:** Ler 10 números e inverter a ordem SEM usar outro array.

```cpp
#include <iostream>
using namespace std;

int main() {
    int numeros[10];
    
    // Leitura dos dados
    cout << "Digite 10 numeros inteiros:\n";
    for (int i = 0; i < 10; i++) {
        cout << "Numero " << (i + 1) << ": ";
        cin >> numeros[i];
    }
    
    // Exibir vetor original
    cout << "\nVetor original:\n";
    for (int i = 0; i < 10; i++) {
        cout << numeros[i] << " ";
    }
    
    // Inverter usando dois ponteiros (sem array auxiliar)
    int inicio = 0;
    int fim = 9;
    while (inicio < fim) {
        // Trocar elementos
        int temp = numeros[inicio];
        numeros[inicio] = numeros[fim];
        numeros[fim] = temp;
        
        inicio++;
        fim--;
    }
    
    // Exibir vetor invertido
    cout << "\n\nVetor invertido:\n";
    for (int i = 0; i < 10; i++) {
        cout << numeros[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
Digite 10 numeros inteiros:
Numero 1: 1
Numero 2: 2
...

Vetor original:
1 2 3 4 5 6 7 8 9 10

Vetor invertido:
10 9 8 7 6 5 4 3 2 1
```

---

## Exercício 5 — Matriz 3×3

**Objetivo:** Ler matriz 3×3, exibir, calcular soma e maior valor.

```cpp
#include <iostream>
using namespace std;

int main() {
    int matriz[3][3];
    int soma = 0;
    int maior = INT_MIN;
    
    // Leitura dos dados
    cout << "Digite os valores da matriz 3x3:\n";
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << "Elemento [" << i << "][" << j << "]: ";
            cin >> matriz[i][j];
        }
    }
    
    // Exibir matriz
    cout << "\nMatriz:\n";
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << matriz[i][j] << " ";
        }
        cout << "\n";
    }
    
    // Calcular soma e maior
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            soma += matriz[i][j];
            if (matriz[i][j] > maior) {
                maior = matriz[i][j];
            }
        }
    }
    
    // Exibir resultados
    cout << "\nSoma de todos os elementos: " << soma;
    cout << "\nMaior valor: " << maior << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
Digite os valores da matriz 3x3:
Elemento [0][0]: 1
Elemento [0][1]: 2
...

Matriz:
1 2 3
4 5 6
7 8 9

Soma de todos os elementos: 45
Maior valor: 9
```

---

## Exercício 6 — Diagonal Principal

**Objetivo:** Ler matriz 4×4, exibir diagonal principal e sua soma.

```cpp
#include <iostream>
using namespace std;

int main() {
    int matriz[4][4];
    int soma_diagonal = 0;
    
    // Leitura dos dados
    cout << "Digite os valores da matriz 4x4:\n";
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            cout << "Elemento [" << i << "][" << j << "]: ";
            cin >> matriz[i][j];
        }
    }
    
    // Exibir matriz
    cout << "\nMatriz:\n";
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            cout << matriz[i][j] << " ";
        }
        cout << "\n";
    }
    
    // Extrair diagonal principal
    cout << "\nDiagonal principal: ";
    for (int i = 0; i < 4; i++) {
        cout << matriz[i][i] << " ";
        soma_diagonal += matriz[i][i];
    }
    
    // Exibir soma
    cout << "\n\nSoma da diagonal: " << soma_diagonal << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
Digite os valores da matriz 4x4:
Elemento [0][0]: 1
Elemento [0][1]: 2
...

Matriz:
1 2 3 4
5 6 7 8
9 10 11 12
13 14 15 16

Diagonal principal: 1 6 11 16

Soma da diagonal: 34
```

---

## Exercício 7 — Matriz de Notas

**Objetivo:** Matriz 4×3 (alunos × avaliações), calcular média de cada aluno.

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

int main() {
    float notas[4][3];
    
    // Leitura das notas
    cout << "Digite as notas de 4 alunos em 3 avaliacoes:\n\n";
    for (int i = 0; i < 4; i++) {
        cout << "Aluno " << (i + 1) << ":\n";
        for (int j = 0; j < 3; j++) {
            cout << "  P" << (j + 1) << ": ";
            cin >> notas[i][j];
        }
    }
    
    // Exibir matriz e calcular médias
    cout << "\n========== RELATORIO DE NOTAS ==========\n\n";
    cout << "          P1       P2       P3      MEDIA\n";
    cout << "----------------------------------------\n";
    
    for (int i = 0; i < 4; i++) {
        cout << "Aluno " << (i + 1) << ": ";
        
        float soma = 0;
        for (int j = 0; j < 3; j++) {
            cout << fixed << setprecision(2) << notas[i][j] << "     ";
            soma += notas[i][j];
        }
        
        float media = soma / 3.0;
        cout << media << "\n";
    }
    
    cout << "----------------------------------------\n";
    
    return 0;
}
```

**Saída Esperada:**
```
Digite as notas de 4 alunos em 3 avaliacoes:

Aluno 1:
  P1: 7.5
  P2: 8.0
  P3: 7.8
Aluno 2:
...

========== RELATORIO DE NOTAS ==========

          P1       P2       P3      MEDIA
----------------------------------------
Aluno 1: 7.50     8.00     7.80     7.77
Aluno 2: 6.50     7.00     8.50     7.33
Aluno 3: 9.00     8.50     9.50     9.00
Aluno 4: 5.50     6.00     6.50     6.00
----------------------------------------
```

---

## Exercício 8 — Struct Produto

**Objetivo:** Struct com nome, código, preço, quantidade. Cadastrar 5 produtos, calcular valor em estoque.

```cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

struct Produto {
    int codigo;
    string nome;
    float preco;
    int quantidade;
};

int main() {
    Produto produtos[5];
    int maior_indice = 0;
    float maior_valor = 0;
    
    // Cadastro dos produtos
    cout << "=== CADASTRO DE PRODUTOS ===\n\n";
    for (int i = 0; i < 5; i++) {
        cout << "Produto " << (i + 1) << ":\n";
        cout << "Codigo: ";
        cin >> produtos[i].codigo;
        cin.ignore(); // Limpar buffer
        cout << "Nome: ";
        getline(cin, produtos[i].nome);
        cout << "Preco: ";
        cin >> produtos[i].preco;
        cout << "Quantidade: ";
        cin >> produtos[i].quantidade;
        cout << "\n";
    }
    
    // Exibir produtos e calcular valor em estoque
    cout << "\n===== RELATORIO DE PRODUTOS =====\n\n";
    cout << "COD  NOME              PRECO    QTD   VALOR TOTAL\n";
    cout << "---------------------------------------------------\n";
    
    for (int i = 0; i < 5; i++) {
        float valor_estoque = produtos[i].preco * produtos[i].quantidade;
        
        cout << produtos[i].codigo << "    "
             << left << setw(16) << produtos[i].nome << " "
             << fixed << setprecision(2) << produtos[i].preco << "   "
             << produtos[i].quantidade << "     "
             << valor_estoque << "\n";
        
        if (valor_estoque > maior_valor) {
            maior_valor = valor_estoque;
            maior_indice = i;
        }
    }
    
    cout << "---------------------------------------------------\n";
    cout << "\nProduto com maior valor em estoque:\n";
    cout << "Nome: " << produtos[maior_indice].nome << "\n";
    cout << "Valor total: R$ " << fixed << setprecision(2) << maior_valor << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
=== CADASTRO DE PRODUTOS ===

Produto 1:
Codigo: 101
Nome: Notebook
Preco: 3500.00
Quantidade: 2

...

===== RELATORIO DE PRODUTOS =====

COD  NOME              PRECO    QTD   VALOR TOTAL
---------------------------------------------------
101  Notebook         3500.00   2     7000.00
102  Mouse            50.00    10      500.00
103  Teclado          150.00    5      750.00
104  Monitor          800.00    3     2400.00
105  Webcam           200.00    8     1600.00
---------------------------------------------------

Produto com maior valor em estoque:
Nome: Notebook
Valor total: R$ 7000.00
```

---

## Exercício 9 — Struct Aluno

**Objetivo:** Struct com nome, idade, 3 notas. Calcular média, classificar, contar aprovados/reprovados.

```cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

struct Aluno {
    string nome;
    int idade;
    float notas[3];
};

int main() {
    Aluno alunos[5];
    int aprovados = 0, reprovados = 0;
    int aluno_maior_media = 0;
    float maior_media = 0;
    
    // Cadastro dos alunos
    cout << "=== CADASTRO DE ALUNOS ===\n\n";
    for (int i = 0; i < 5; i++) {
        cout << "Aluno " << (i + 1) << ":\n";
        cout << "Nome: ";
        cin.ignore();
        getline(cin, alunos[i].nome);
        cout << "Idade: ";
        cin >> alunos[i].idade;
        cout << "Nota 1: ";
        cin >> alunos[i].notas[0];
        cout << "Nota 2: ";
        cin >> alunos[i].notas[1];
        cout << "Nota 3: ";
        cin >> alunos[i].notas[2];
        cout << "\n";
    }
    
    // Exibir relatório
    cout << "\n===== RELATORIO DE ALUNOS =====\n\n";
    cout << "NOME              N1      N2      N3     MEDIA   STATUS\n";
    cout << "-----------------------------------------------------------\n";
    
    for (int i = 0; i < 5; i++) {
        float media = (alunos[i].notas[0] + alunos[i].notas[1] + alunos[i].notas[2]) / 3.0;
        string status = (media >= 7.0) ? "APROVADO" : "REPROVADO";
        
        cout << left << setw(16) << alunos[i].nome << " "
             << fixed << setprecision(2)
             << alunos[i].notas[0] << "    "
             << alunos[i].notas[1] << "    "
             << alunos[i].notas[2] << "    "
             << media << "   "
             << status << "\n";
        
        if (media >= 7.0) {
            aprovados++;
        } else {
            reprovados++;
        }
        
        if (media > maior_media) {
            maior_media = media;
            aluno_maior_media = i;
        }
    }
    
    cout << "-----------------------------------------------------------\n";
    cout << "\nRESUMO:\n";
    cout << "Aprovados: " << aprovados << "\n";
    cout << "Reprovados: " << reprovados << "\n";
    cout << "\nAluno com maior media:\n";
    cout << "Nome: " << alunos[aluno_maior_media].nome << "\n";
    cout << "Media: " << fixed << setprecision(2) << maior_media << endl;
    
    return 0;
}
```

**Saída Esperada:**
```
=== CADASTRO DE ALUNOS ===

Aluno 1:
Nome: Carlos
Idade: 20
Nota 1: 8.5
Nota 2: 7.5
Nota 3: 9.0

...

===== RELATORIO DE ALUNOS =====

NOME              N1      N2      N3     MEDIA   STATUS
-----------------------------------------------------------
Carlos           8.50    7.50    9.00    8.33   APROVADO
Ana              6.00    5.50    6.50    6.00   REPROVADO
Pedro            9.00    8.50    9.50    9.00   APROVADO
Maria            7.50    8.00    7.80    7.77   APROVADO
Lucas            5.50    6.00    6.50    6.00   REPROVADO
-----------------------------------------------------------

RESUMO:
Aprovados: 3
Reprovados: 2

Aluno com maior media:
Nome: Pedro
Media: 9.00
```

---

## Exercício 10 — Sistema Integrado de RH

**Objetivo:** Sistema com menu para cadastro, listagem e consultas de 10 funcionários.

```cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

struct Funcionario {
    int id;
    string nome;
    int idade;
    string cargo;
    float salario;
};

int main() {
    Funcionario funcionarios[10];
    int total_funcionarios = 0;
    int opcao;
    
    do {
        // Exibir menu
        cout << "\n=================================\n";
        cout << "    SISTEMA DE FUNCIONARIOS\n";
        cout << "=================================\n\n";
        cout << "1 - Cadastrar funcionarios\n";
        cout << "2 - Listar funcionarios\n";
        cout << "3 - Maior salario\n";
        cout << "4 - Media salarial\n";
        cout << "5 - Salarios acima da media\n";
        cout << "0 - Sair\n\n";
        cout << "Escolha uma opcao: ";
        cin >> opcao;
        cin.ignore();
        
        switch (opcao) {
            case 1: {
                // Cadastrar
                if (total_funcionarios < 10) {
                    cout << "\n=== CADASTRO DE FUNCIONARIO ===\n\n";
                    cout << "ID: ";
                    cin >> funcionarios[total_funcionarios].id;
                    cin.ignore();
                    cout << "Nome: ";
                    getline(cin, funcionarios[total_funcionarios].nome);
                    cout << "Idade: ";
                    cin >> funcionarios[total_funcionarios].idade;
                    cin.ignore();
                    cout << "Cargo: ";
                    getline(cin, funcionarios[total_funcionarios].cargo);
                    cout << "Salario: ";
                    cin >> funcionarios[total_funcionarios].salario;
                    
                    total_funcionarios++;
                    cout << "\nFuncionario cadastrado com sucesso!\n";
                } else {
                    cout << "\nErro: Numero maximo de funcionarios atingido!\n";
                }
                break;
            }
            
            case 2: {
                // Listar
                if (total_funcionarios == 0) {
                    cout << "\nNenhum funcionario cadastrado!\n";
                } else {
                    cout << "\n===== LISTAGEM DE FUNCIONARIOS =====\n\n";
                    cout << "ID   NOME              IDADE  CARGO          SALARIO\n";
                    cout << "------------------------------------------------------\n";
                    
                    for (int i = 0; i < total_funcionarios; i++) {
                        cout << funcionarios[i].id << "    "
                             << left << setw(16) << funcionarios[i].nome << " "
                             << funcionarios[i].idade << "    "
                             << left << setw(14) << funcionarios[i].cargo << " "
                             << "R$ " << fixed << setprecision(2) << funcionarios[i].salario << "\n";
                    }
                    cout << "------------------------------------------------------\n";
                }
                break;
            }
            
            case 3: {
                // Maior salário
                if (total_funcionarios == 0) {
                    cout << "\nNenhum funcionario cadastrado!\n";
                } else {
                    int indice = 0;
                    float maior = funcionarios[0].salario;
                    
                    for (int i = 1; i < total_funcionarios; i++) {
                        if (funcionarios[i].salario > maior) {
                            maior = funcionarios[i].salario;
                            indice = i;
                        }
                    }
                    
                    cout << "\n===== FUNCIONARIO COM MAIOR SALARIO =====\n\n";
                    cout << "Nome: " << funcionarios[indice].nome << "\n";
                    cout << "Cargo: " << funcionarios[indice].cargo << "\n";
                    cout << "Salario: R$ " << fixed << setprecision(2) << funcionarios[indice].salario << "\n";
                }
                break;
            }
            
            case 4: {
                // Média salarial
                if (total_funcionarios == 0) {
                    cout << "\nNenhum funcionario cadastrado!\n";
                } else {
                    float soma = 0;
                    for (int i = 0; i < total_funcionarios; i++) {
                        soma += funcionarios[i].salario;
                    }
                    float media = soma / total_funcionarios;
                    
                    cout << "\n===== MEDIA SALARIAL =====\n\n";
                    cout << "Media: R$ " << fixed << setprecision(2) << media << "\n";
                }
                break;
            }
            
            case 5: {
                // Salários acima da média
                if (total_funcionarios == 0) {
                    cout << "\nNenhum funcionario cadastrado!\n";
                } else {
                    float soma = 0;
                    for (int i = 0; i < total_funcionarios; i++) {
                        soma += funcionarios[i].salario;
                    }
                    float media = soma / total_funcionarios;
                    
                    cout << "\n===== FUNCIONARIOS COM SALARIO ACIMA DA MEDIA =====\n\n";
                    cout << "NOME              CARGO          SALARIO\n";
                    cout << "---------------------------------------------------\n";
                    
                    int encontrados = 0;
                    for (int i = 0; i < total_funcionarios; i++) {
                        if (funcionarios[i].salario > media) {
                            cout << left << setw(16) << funcionarios[i].nome << " "
                                 << left << setw(14) << funcionarios[i].cargo << " "
                                 << "R$ " << fixed << setprecision(2) << funcionarios[i].salario << "\n";
                            encontrados++;
                        }
                    }
                    
                    if (encontrados == 0) {
                        cout << "Nenhum funcionario com salario acima da media.\n";
                    }
                    cout << "---------------------------------------------------\n";
                }
                break;
            }
            
            case 0:
                cout << "\nEncerrando sistema. Ate logo!\n";
                break;
                
            default:
                cout << "\nOpcao invalida!\n";
        }
        
    } while (opcao != 0);
    
    return 0;
}
```

**Saída Esperada:**
```
=================================
    SISTEMA DE FUNCIONARIOS
=================================

1 - Cadastrar funcionarios
2 - Listar funcionarios
3 - Maior salario
4 - Media salarial
5 - Salarios acima da media
0 - Sair

Escolha uma opcao: 1

=== CADASTRO DE FUNCIONARIO ===

ID: 1
Nome: Carlos Silva
Idade: 35
Cargo: Gerente
Salario: 5000.00

Funcionario cadastrado com sucesso!

...

Escolha uma opcao: 2

===== LISTAGEM DE FUNCIONARIOS =====

ID   NOME              IDADE  CARGO          SALARIO
------------------------------------------------------
1    Carlos Silva      35    Gerente        R$ 5000.00
2    Ana Costa         28    Analista       R$ 3500.00
...
------------------------------------------------------
```

---

## Resumo das Estruturas Utilizadas

| Exercício | Estrutura | Operações Principais |
|-----------|-----------|----------------------|
| 1 | Array 1D | Leitura, soma, média |
| 2 | Array 1D | Comparações, índices |
| 3 | Array 1D | Filtragem, contagem |
| 4 | Array 1D | Inversão, swaps |
| 5 | Matriz 2D | Soma total, máximo |
| 6 | Matriz 2D | Diagonal principal |
| 7 | Matriz 2D | Cálculo de médias por linha |
| 8 | Struct | Arrays de structs, operações |
| 9 | Struct | Classificação, comparações |
| 10 | Struct + Menu | Sistema integrado, múltiplas operações |

---

## Dicas de Implementação

### 1. **Memória**
- Arrays são contiguos: `int arr[10]` ocupa 10 * 4 = 40 bytes
- Structs combinam múltiplos tipos na memória

### 2. **Indexação**
- Sempre comece do índice 0
- Use variáveis `i` para linhas, `j` para colunas em matrizes

### 3. **Validação**
- Verifique limites de arrays para evitar overflow
- Valide entrada do usuário quando possível

### 4. **Modularização**
- Separe leitura, processamento e exibição em partes lógicas
- Use loops aninhados para matrizes e arrays de structs
