# Atividade - Estruturas LIFO e FIFO (Pilhas e Filas)

## Identificação
- Nome: Luan Bispo Silva
- RGM: 32635648
- Turma: Estrutura de Dados II
- Data: 03/10
- Ferramentas utilizadas: Python 3.13 (`collections.deque`) e C (gcc 13.3)

---

## Contexto da atividade

### Disciplina e conteúdo
Aula 04 - Estruturas LIFO e FIFO: Fundamentos e Implementação de Pilhas e Filas (Estrutura de Dados 2, Profa. Kadidja Valéria).

### Objetivo
Analisar o problema, identificar o fluxo de dados exigido, escolher a arquitetura correta (LIFO ou FIFO) e só então implementar.

### Regra de escolha usada nas respostas
| Fluxo de dados | Estrutura | Python | Operações |
|---|---|---|---|
| Reversão / retrocesso (último a entrar, primeiro a sair) | Pilha (LIFO) | `list` nativa | `append()` insere no topo, `pop()` remove do topo |
| Sequencial / ordem de chegada (primeiro a entrar, primeiro a sair) | Fila (FIFO) | `collections.deque` | `append()` insere no final, `popleft()` remove do início |

### Observações sobre as decisões
- Todos os códigos foram executados e as saídas mostradas abaixo são as reais.
- Os programas Python usam funções/classes e um bloco de demonstração (`if __name__ == "__main__":`), sem entrada pelo teclado.
- Na Triagem Hospitalar, entre dois pacientes prioritários atende-se primeiro quem chegou primeiro (ver Desafio Master).

---

## Desafios de Fundamentos

### Exercício 1 - O Simulador "Desfazer"

#### Pergunta 1. Implemente uma Pilha para gerenciar ações em um editor de texto. Armazene cada ação (digitar, apagar, substituir). Ao acionar "desfazer", o programa deve remover e reverter estritamente o último comando inserido.

**R:**

**Estrutura escolhida: Pilha (LIFO).** Desfazer exige reverter a ação mais recente primeiro, exatamente o comportamento do topo da pilha. Cada ação é guardada com a informação necessária para revertê-la:

| Ação | O que é guardado na pilha | Como é revertida |
|---|---|---|
| digitar | texto digitado | remove do final do texto esse trecho |
| apagar | texto removido | devolve o trecho ao final do texto |
| substituir | posição, texto antigo e texto novo | troca o texto novo pelo antigo na mesma posição |

**Implementação (`ex1_desfazer.py`):**

```python
class Editor:
    """Editor de texto com 'desfazer' usando uma pilha (LIFO) de ações."""

    def __init__(self):
        self.texto = ""
        self.historico = []  # pilha: append() = push | pop() = remove do topo

    def digitar(self, trecho):
        if not trecho:
            return False
        self.texto += trecho
        self.historico.append(("digitar", trecho))
        return True

    def apagar(self, qtd):
        qtd = min(qtd, len(self.texto))
        if qtd <= 0:
            return False
        removido = self.texto[-qtd:]
        self.texto = self.texto[:-qtd]
        self.historico.append(("apagar", removido))
        return True

    def substituir(self, antigo, novo):
        pos = self.texto.find(antigo)
        if not antigo or pos == -1:
            return False
        self.texto = self.texto[:pos] + novo + self.texto[pos + len(antigo):]
        self.historico.append(("substituir", pos, antigo, novo))
        return True

    def desfazer(self):
        """Remove e reverte estritamente a última ação registrada."""
        if not self.historico:
            return None
        acao = self.historico.pop()
        tipo = acao[0]
        if tipo == "digitar":
            self.texto = self.texto[:-len(acao[1])]
        elif tipo == "apagar":
            self.texto += acao[1]
        elif tipo == "substituir":
            _, pos, antigo, novo = acao
            self.texto = self.texto[:pos] + antigo + self.texto[pos + len(novo):]
        return acao


def mostrar(rotulo, editor):
    print(f"{rotulo:<32} texto: '{editor.texto}' | ações na pilha: {len(editor.historico)}")


if __name__ == "__main__":
    ed = Editor()
    ed.digitar("Olá")
    mostrar("digitar('Olá')", ed)
    ed.digitar(" mundo")
    mostrar("digitar(' mundo')", ed)
    ed.apagar(2)
    mostrar("apagar(2)", ed)
    ed.substituir("Olá", "Oi")
    mostrar("substituir('Olá', 'Oi')", ed)
    print("--- desfazendo tudo ---")
    for _ in range(5):
        acao = ed.desfazer()
        if acao is None:
            print("desfazer() -> nada para desfazer (pilha vazia)")
        else:
            mostrar(f"desfazer() -> {acao[0]}", ed)
```

**Saída:**

```text
digitar('Olá')                   texto: 'Olá' | ações na pilha: 1
digitar(' mundo')                texto: 'Olá mundo' | ações na pilha: 2
apagar(2)                        texto: 'Olá mun' | ações na pilha: 3
substituir('Olá', 'Oi')          texto: 'Oi mun' | ações na pilha: 4
--- desfazendo tudo ---
desfazer() -> substituir         texto: 'Olá mun' | ações na pilha: 3
desfazer() -> apagar             texto: 'Olá mundo' | ações na pilha: 2
desfazer() -> digitar            texto: 'Olá' | ações na pilha: 1
desfazer() -> digitar            texto: '' | ações na pilha: 0
desfazer() -> nada para desfazer (pilha vazia)
```

As ações foram revertidas na ordem inversa da inserção (substituir, apagar, digitar, digitar), e desfazer com a pilha vazia é tratado sem erro.

---

### Exercício 2 - O Sistema de Impressão

#### Pergunta 1. Implemente uma Fila para gerenciar um spooler de impressora. Cada novo documento é enfileirado. O sistema deve garantir que o documento impresso seja sempre o mais antigo aguardando na fila.

**R:**

**Estrutura escolhida: Fila (FIFO).** O documento que chegou primeiro deve ser impresso primeiro. Usei `collections.deque`: `append()` enfileira no final e `popleft()` remove do início, ambos em tempo constante. Uma lista comum com `pop(0)` seria ineficiente, pois desloca todos os outros elementos na memória.

**Implementação (`ex2_impressao.py`):**

```python
from collections import deque


def enfileirar(fila, doc):
    """Cada novo documento entra no final da fila."""
    fila.append(doc)
    print(f"[+] '{doc}' enfileirado. Fila: {list(fila)}")


def imprimir_proximo(fila):
    """Imprime sempre o documento mais antigo (início da fila)."""
    if not fila:
        print("[!] Nenhum documento aguardando.")
        return None
    doc = fila.popleft()
    print(f"[#] Imprimindo '{doc}'. Fila: {list(fila)}")
    return doc


if __name__ == "__main__":
    spooler = deque()
    enfileirar(spooler, "Doc_1")
    enfileirar(spooler, "Doc_2")
    enfileirar(spooler, "Doc_3")
    imprimir_proximo(spooler)
    enfileirar(spooler, "Doc_4")
    while spooler:
        imprimir_proximo(spooler)
    imprimir_proximo(spooler)
```

**Saída:**

```text
[+] 'Doc_1' enfileirado. Fila: ['Doc_1']
[+] 'Doc_2' enfileirado. Fila: ['Doc_1', 'Doc_2']
[+] 'Doc_3' enfileirado. Fila: ['Doc_1', 'Doc_2', 'Doc_3']
[#] Imprimindo 'Doc_1'. Fila: ['Doc_2', 'Doc_3']
[+] 'Doc_4' enfileirado. Fila: ['Doc_2', 'Doc_3', 'Doc_4']
[#] Imprimindo 'Doc_2'. Fila: ['Doc_3', 'Doc_4']
[#] Imprimindo 'Doc_3'. Fila: ['Doc_4']
[#] Imprimindo 'Doc_4'. Fila: []
[!] Nenhum documento aguardando.
```

A impressão seguiu exatamente a ordem de chegada (Doc_1, Doc_2, Doc_3, Doc_4), inclusive para o documento enfileirado depois da primeira impressão.

---

## Desafio Master - Triagem Hospitalar

#### Pergunta 1. Criar um programa de gerenciamento para uma fila de atendimento médico. Pacientes normais entram no final da fila. Pacientes prioritários (idosos, emergências) devem furar a fila e entrar diretamente no início do atendimento.

**R:**

**Estrutura escolhida: Fila de duas pontas (`collections.deque`).** O fluxo padrão é FIFO (normais entram no final e saem pelo início), e o fluxo de exceção insere perto do início, sem reescrever a fila inteira.

**Decisão de regra:** quando há mais de um prioritário, atende-se primeiro o que chegou primeiro. Para isso a classe guarda quantos prioritários estão no início da fila (`qtd_prioritarios`) e insere o novo prioritário logo depois deles, com `deque.insert()`. Assim os prioritários ficam à frente de todos os normais, mantendo a ordem de chegada entre si. (Usar apenas `appendleft()`, como na dica do slide, faria o último prioritário passar à frente dos prioritários anteriores.) O custo do `insert` é proporcional à quantidade de prioritários na frente da fila, não ao tamanho total dela.

**Implementação (`ex3_triagem.py`):**

```python
from collections import deque


class Triagem:
    """Fila de atendimento: normais entram no final; prioritários entram
    logo após os prioritários que já esperam (ordem de chegada entre eles)."""

    def __init__(self):
        self.fila = deque()        # itens: (nome, prioritario)
        self.qtd_prioritarios = 0  # prioritários que estão no início da fila

    def chegar(self, nome, prioritario=False):
        if prioritario:
            self.fila.insert(self.qtd_prioritarios, (nome, True))
            self.qtd_prioritarios += 1
        else:
            self.fila.append((nome, False))

    def atender(self):
        if not self.fila:
            return None
        nome, prioritario = self.fila.popleft()
        if prioritario:
            self.qtd_prioritarios -= 1
        return nome

    def estado(self):
        return [f"{nome}*" if prio else nome for nome, prio in self.fila]


if __name__ == "__main__":
    t = Triagem()
    for nome in ["Ana", "Bruno", "Carla"]:
        t.chegar(nome)
    print("Chegaram 3 normais:       ", t.estado())
    t.chegar("Rita", prioritario=True)
    print("Chegou Rita (idosa):      ", t.estado())
    t.chegar("João", prioritario=True)
    print("Chegou João (emergência): ", t.estado())
    t.chegar("Diego")
    print("Chegou Diego (normal):    ", t.estado())
    print("(* = prioritário)")
    print("--- atendimento ---")
    while t.fila:
        nome = t.atender()
        print(f"Atendido: {nome:<6} | Restam: {t.estado()}")
    print("Atender com fila vazia ->", t.atender())
```

**Saída:**

```text
Chegaram 3 normais:        ['Ana', 'Bruno', 'Carla']
Chegou Rita (idosa):       ['Rita*', 'Ana', 'Bruno', 'Carla']
Chegou João (emergência):  ['Rita*', 'João*', 'Ana', 'Bruno', 'Carla']
Chegou Diego (normal):     ['Rita*', 'João*', 'Ana', 'Bruno', 'Carla', 'Diego']
(* = prioritário)
--- atendimento ---
Atendido: Rita   | Restam: ['João*', 'Ana', 'Bruno', 'Carla', 'Diego']
Atendido: João   | Restam: ['Ana', 'Bruno', 'Carla', 'Diego']
Atendido: Ana    | Restam: ['Bruno', 'Carla', 'Diego']
Atendido: Bruno  | Restam: ['Carla', 'Diego']
Atendido: Carla  | Restam: ['Diego']
Atendido: Diego  | Restam: []
Atender com fila vazia -> None
```

Os prioritários (Rita e João) furaram a fila e foram atendidos na ordem em que chegaram; os normais seguiram em FIFO.

---

## Exercícios de Fixação - Trilha Python

### 01 - A Pilha Matemática

#### Pergunta 1. Crie uma pilha (LIFO) que armazene números inteiros. Escreva um algoritmo que esvazie a pilha iterativamente, calculando e retornando a soma total dos elementos removidos.

**R:**

**Estrutura escolhida: Pilha (LIFO)**, com `list` nativa (`append()` empilha e `pop()` desempilha do topo). O algoritmo repete `pop()` enquanto a pilha não estiver vazia, acumulando a soma.

**Implementação (`ex4_pilha_matematica.py`):**

```python
def somar_e_esvaziar(pilha, verbose=False):
    """Esvazia a pilha iterativamente e retorna a soma dos elementos removidos."""
    soma = 0
    while pilha:
        elemento = pilha.pop()  # remove do topo (LIFO)
        soma += elemento
        if verbose:
            print(f"  pop() -> {elemento:>2} | soma parcial = {soma}")
    return soma


if __name__ == "__main__":
    pilha = []
    for n in [4, 8, 15, 16, 23, 42]:
        pilha.append(n)
    print("Pilha (base -> topo):", pilha)
    total = somar_e_esvaziar(pilha, verbose=True)
    print("Soma total:", total)
    print("Pilha após esvaziar:", pilha)
    print("Pilha vazia desde o início:", somar_e_esvaziar([]))
```

**Saída:**

```text
Pilha (base -> topo): [4, 8, 15, 16, 23, 42]
  pop() -> 42 | soma parcial = 42
  pop() -> 23 | soma parcial = 65
  pop() -> 16 | soma parcial = 81
  pop() -> 15 | soma parcial = 96
  pop() ->  8 | soma parcial = 104
  pop() ->  4 | soma parcial = 108
Soma total: 108
Pilha após esvaziar: []
Pilha vazia desde o início: 0
```

Conferência: 4+8+15+16+23+42 = 108. Os elementos saíram do último para o primeiro, e a pilha vazia retorna 0.

---

### 02 - O Call Center

#### Pergunta 1. Crie uma fila (FIFO) que armazene nomes de clientes. Simule um ciclo contínuo de atendimento: adicionar novos clientes à espera e "chamar" o próximo cliente da fila para atendimento.

**R:**

**Estrutura escolhida: Fila (FIFO)** com `collections.deque`: o cliente que chegou primeiro é chamado primeiro. A simulação percorre uma sequência de eventos, em que cada um é a chegada de um cliente ou a chamada do próximo.

**Implementação (`ex5_callcenter.py`):**

```python
from collections import deque


def adicionar_cliente(fila, nome):
    fila.append(nome)  # entra no final da fila de espera
    print(f"[+] {nome} entrou na espera. Fila: {list(fila)}")


def chamar_proximo(fila):
    if not fila:
        print("[!] Ninguém aguardando atendimento.")
        return None
    nome = fila.popleft()  # sai quem chegou primeiro
    print(f"[>] Chamando {nome} para atendimento. Fila: {list(fila)}")
    return nome


if __name__ == "__main__":
    fila = deque()
    # Simulação do ciclo de atendimento: ("chega", nome) ou ("chama", None)
    eventos = [
        ("chega", "Ana"), ("chega", "Bruno"), ("chama", None),
        ("chega", "Carla"), ("chama", None), ("chega", "Diego"),
        ("chama", None), ("chama", None), ("chama", None),
    ]
    for tipo, nome in eventos:
        if tipo == "chega":
            adicionar_cliente(fila, nome)
        else:
            chamar_proximo(fila)
```

**Saída:**

```text
[+] Ana entrou na espera. Fila: ['Ana']
[+] Bruno entrou na espera. Fila: ['Ana', 'Bruno']
[>] Chamando Ana para atendimento. Fila: ['Bruno']
[+] Carla entrou na espera. Fila: ['Bruno', 'Carla']
[>] Chamando Bruno para atendimento. Fila: ['Carla']
[+] Diego entrou na espera. Fila: ['Carla', 'Diego']
[>] Chamando Carla para atendimento. Fila: ['Diego']
[>] Chamando Diego para atendimento. Fila: []
[!] Ninguém aguardando atendimento.
```

Os clientes foram chamados na ordem de chegada (Ana, Bruno, Carla, Diego), e a chamada com a fila vazia é tratada sem erro.

---

## Exercícios de Fixação - Trilha C (Baixo Nível)

### 01 - Pilha via Array Estático

#### Pergunta 1. Implemente uma pilha em C alocando um array de tamanho fixo. Desenvolva as funções push() e pop(), criando uma variável de controle manual para rastrear o índice exato do topo (top).

**R:**

**Estrutura escolhida: Pilha (LIFO) sobre array estático.** O array `pilha[MAX]` guarda os dados e a variável `top` guarda o índice do topo, começando em `-1` (pilha vazia). `push()` incrementa `top` e grava; `pop()` lê em `top` e decrementa. As duas funções retornam `1` em sucesso e `0` nos casos de erro: pilha cheia (`top == MAX - 1`, overflow) e pilha vazia (`top == -1`, underflow).

**Implementação (`ex6_pilha.c`):**

```c
#include <stdio.h>

#define MAX 5

int pilha[MAX];
int top = -1; /* índice do topo; -1 indica pilha vazia */

/* Retorna 1 se inseriu, 0 se a pilha está cheia (overflow). */
int push(int valor) {
    if (top == MAX - 1) {
        return 0;
    }
    top++;
    pilha[top] = valor;
    return 1;
}

/* Retorna 1 se removeu (valor em *saida), 0 se a pilha está vazia (underflow). */
int pop(int *saida) {
    if (top == -1) {
        return 0;
    }
    *saida = pilha[top];
    top--;
    return 1;
}

void mostrar(void) {
    printf("  pilha (base->topo): [");
    for (int i = 0; i <= top; i++) {
        printf("%s%d", i ? " " : "", pilha[i]);
    }
    printf("]  top = %d\n", top);
}

int main(void) {
    int v;

    printf("Empilhando 10, 20, 30:\n");
    push(10);
    push(20);
    push(30);
    mostrar();

    if (pop(&v)) {
        printf("pop() -> %d\n", v);
    }
    mostrar();

    printf("Empilhando 40, 50, 60, 70 (capacidade = %d):\n", MAX);
    int valores[] = {40, 50, 60, 70};
    for (int i = 0; i < 4; i++) {
        if (push(valores[i])) {
            printf("  push(%d) ok\n", valores[i]);
        } else {
            printf("  push(%d) falhou: pilha cheia\n", valores[i]);
        }
    }
    mostrar();

    printf("Esvaziando a pilha:\n");
    while (pop(&v)) {
        printf("  pop() -> %d\n", v);
    }
    if (!pop(&v)) {
        printf("  pop() falhou: pilha vazia\n");
    }
    mostrar();
    return 0;
}
```

Compilação: `gcc -Wall -Wextra -std=c99 -o ex6_pilha ex6_pilha.c && ./ex6_pilha`

**Saída:**

```text
Empilhando 10, 20, 30:
  pilha (base->topo): [10 20 30]  top = 2
pop() -> 30
  pilha (base->topo): [10 20]  top = 1
Empilhando 40, 50, 60, 70 (capacidade = 5):
  push(40) ok
  push(50) ok
  push(60) ok
  push(70) falhou: pilha cheia
  pilha (base->topo): [10 20 40 50 60]  top = 4
Esvaziando a pilha:
  pop() -> 60
  pop() -> 50
  pop() -> 40
  pop() -> 20
  pop() -> 10
  pop() falhou: pilha vazia
  pilha (base->topo): []  top = -1
```

O `top` acompanhou o índice exato do topo (de `-1` a `4`), o `push(70)` foi recusado com a pilha cheia e o `pop()` final foi recusado com a pilha vazia.

---

### 02 - Fila Circular

#### Pergunta 1. Otimize o uso de memória implementando uma fila sobre um array circular. Desenvolva enqueue() e dequeue(), controlando independentemente os ponteiros de início (front) e fim (rear).

**R:**

**Estrutura escolhida: Fila (FIFO) sobre array circular.** Em uma fila linear em array, as posições liberadas no início nunca seriam reaproveitadas. Na fila circular, `front` (próximo a sair) e `rear` (próxima posição livre) avançam com `(índice + 1) % MAX`, voltando ao início do array ao chegar no fim. Como `front == rear` ocorre tanto com a fila vazia quanto cheia, uso a variável `count` para distinguir os dois casos.

**Implementação (`ex7_fila_circular.c`):**

```c
#include <stdio.h>

#define MAX 5

int fila[MAX];
int front = 0; /* índice do próximo elemento a sair */
int rear = 0;  /* índice da próxima posição livre para entrar */
int count = 0; /* quantidade de elementos (distingue fila cheia de vazia) */

/* Retorna 1 se enfileirou, 0 se a fila está cheia. */
int enqueue(int valor) {
    if (count == MAX) {
        return 0;
    }
    fila[rear] = valor;
    rear = (rear + 1) % MAX; /* volta ao início do array ao chegar no fim */
    count++;
    return 1;
}

/* Retorna 1 se removeu (valor em *saida), 0 se a fila está vazia. */
int dequeue(int *saida) {
    if (count == 0) {
        return 0;
    }
    *saida = fila[front];
    front = (front + 1) % MAX;
    count--;
    return 1;
}

void mostrar(void) {
    printf("  fila (front->rear): [");
    for (int i = 0; i < count; i++) {
        printf("%s%d", i ? " " : "", fila[(front + i) % MAX]);
    }
    printf("]  front = %d, rear = %d, count = %d\n", front, rear, count);
}

int main(void) {
    int v;

    printf("Enfileirando 1 a 5 (capacidade = %d):\n", MAX);
    for (int i = 1; i <= 5; i++) {
        enqueue(i);
    }
    mostrar();
    if (!enqueue(6)) {
        printf("  enqueue(6) falhou: fila cheia\n");
    }

    printf("Removendo dois elementos:\n");
    for (int i = 0; i < 2; i++) {
        if (dequeue(&v)) {
            printf("  dequeue() -> %d\n", v);
        }
    }
    mostrar();

    printf("Enfileirando 6 e 7 (reaproveita as posicoes liberadas):\n");
    enqueue(6);
    enqueue(7);
    mostrar();

    printf("Esvaziando a fila:\n");
    while (dequeue(&v)) {
        printf("  dequeue() -> %d\n", v);
    }
    if (!dequeue(&v)) {
        printf("  dequeue() falhou: fila vazia\n");
    }
    mostrar();
    return 0;
}
```

Compilação: `gcc -Wall -Wextra -std=c99 -o ex7_fila_circular ex7_fila_circular.c && ./ex7_fila_circular`

**Saída:**

```text
Enfileirando 1 a 5 (capacidade = 5):
  fila (front->rear): [1 2 3 4 5]  front = 0, rear = 0, count = 5
  enqueue(6) falhou: fila cheia
Removendo dois elementos:
  dequeue() -> 1
  dequeue() -> 2
  fila (front->rear): [3 4 5]  front = 2, rear = 0, count = 3
Enfileirando 6 e 7 (reaproveita as posicoes liberadas):
  fila (front->rear): [3 4 5 6 7]  front = 2, rear = 2, count = 5
Esvaziando a fila:
  dequeue() -> 3
  dequeue() -> 4
  dequeue() -> 5
  dequeue() -> 6
  dequeue() -> 7
  dequeue() falhou: fila vazia
  fila (front->rear): []  front = 2, rear = 2, count = 0
```

O `rear` voltou a `0` ao passar da última posição e os elementos 6 e 7 ocuparam as posições 0 e 1, liberadas pelos `dequeue()` anteriores. Isso mostra o reaproveitamento de memória. Com `front == rear` e `count = 5` a fila está cheia; com `count = 0`, vazia.

---

## Validação

Antes de entregar, conferi:
- **Execução:** os 5 programas Python rodaram em Python 3.13 e os 2 programas em C compilaram com `gcc 13.3 -Wall -Wextra -std=c99` sem avisos. Todas as saídas deste documento foram copiadas da execução real.
- **Casos de borda testados:** desfazer com a pilha vazia (Ex. 1), imprimir com a fila vazia (Ex. 2), atender com a fila vazia (Master), soma de pilha vazia = 0 (Python 01), `push` com pilha cheia e `pop` com pilha vazia (C 01), `enqueue` com fila cheia, `dequeue` com fila vazia e volta do `rear` ao início do array (C 02).
- **Ordem de saída:** pilhas retiraram do último para o primeiro (LIFO); filas atenderam na ordem de chegada (FIFO); na triagem, os prioritários ficaram à frente dos normais, mantendo a ordem de chegada entre si.
- **Soma:** 4+8+15+16+23+42 = 108, igual ao resultado do programa.

---

## Take Away

### Pergunta 1. Qual estrutura foi escolhida em cada exercício e por quê?

**R:**

| Exercício | Estrutura | Por que |
|---|---|---|
| Ex. 1 - Desfazer | Pilha (LIFO) | A ação mais recente deve ser revertida primeiro |
| Ex. 2 - Impressão | Fila (FIFO) | O documento mais antigo deve ser impresso primeiro |
| Master - Triagem | Fila de duas pontas (`deque`) | FIFO para os normais; prioritários entram no início sem reescrever a fila |
| Python 01 - Pilha Matemática | Pilha (LIFO) | Esvaziar pela extremidade única (topo) |
| Python 02 - Call Center | Fila (FIFO) | Atendimento por ordem de chegada |
| C 01 - Pilha em array | Pilha (LIFO) | Índice `top` controla a única extremidade |
| C 02 - Fila circular | Fila (FIFO) | `front` e `rear` controlam as duas extremidades; o módulo reaproveita o array |

### Pergunta 2. Por que usar `collections.deque` para filas em vez de lista comum?

**R:** Em uma lista, remover do início com `pop(0)` obriga o deslocamento de todos os outros elementos na memória. No `deque`, `append()` e `popleft()` operam nas pontas e são eficientes.

### Pergunta 3. O que a escolha entre LIFO e FIFO determina em um sistema?

**R:** Determina quem espera mais, quem é revertido e quem é atendido primeiro. Por isso a estrutura deve ser escolhida a partir do fluxo de dados do problema, antes de escrever o código.

---

## Referência

Material de apoio: slides "Estruturas LIFO e FIFO: Fundamentos e Implementação de Pilhas e Filas" (Aula 04), Estrutura de Dados 2, Profa. Kadidja Valéria, Cruzeiro do Sul Educacional.

### Inteligência artificial utilizada
- Ferramenta: Claude (Anthropic), modelo Claude Sonnet 5.5
- Data de uso: 03/10/2026
