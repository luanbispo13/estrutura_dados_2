# Soluções: Ponteiros, Referências e Estruturas Dinâmicas em Python
## Estrutura de Dados II — Aula 03

---

## Exercício 1 — Referências

**Objetivo:** Compreender compartilhamento de referências em Python.

```python
# Código base
a = [10, 20, 30]
b = a

b.append(40)

print("a:", a)
print("b:", b)
print("id(a):", id(a))
print("id(b):", id(b))
```

**Saída Esperada:**
```
a: [10, 20, 30, 40]
b: [10, 20, 30, 40]
id(a): 140728456789456
id(b): 140728456789456
```

**Análise:**

1. **Qual será a saída?**
   - Ambas as listas mostram `[10, 20, 30, 40]`

2. **`a` e `b` representam o mesmo objeto?**
   - Sim! Os `id()` são idênticos.

3. **Por quê?**
   - Ao fazer `b = a`, não criamos uma cópia. Apenas apontamos `b` para o mesmo objeto que `a` referencia.

4. **O que aconteceria com `b = a.copy()`?**
   - Uma cópia independente seria criada com um `id()` diferente.

**Visualização:**
```
COMPARTILHAMENTO (b = a):
        ┌────────────────┐
        │ [10, 20, 30]   │
        └────────────────┘
             ▲       ▲
             │       │
             a       b
```

```
CÓPIA (b = a.copy()):
a ──→ [10, 20, 30]

b ──→ [10, 20, 30]  (objeto diferente)
```

---

## Exercício 2 — Construindo uma Cadeia

**Objetivo:** Criar Nodes e conectá-los em uma cadeia.

```python
# Classe Node
class Node:
    def __init__(self, valor):
        self.valor = valor
        self.proximo = None


# Criando os nós
n1 = Node("A")
n2 = Node("B")
n3 = Node("C")

# Conectando
n1.proximo = n2
n2.proximo = n3
# n3.proximo permanece None (fim da cadeia)

# Percorrendo e exibindo
print("=== Cadeia A → B → C ===\n")

atual = n1
while atual is not None:
    print(atual.valor)
    atual = atual.proximo
```

**Saída Esperada:**
```
=== Cadeia A → B → C ===

A
B
C
```

**Visualização:**
```
n1 ──→ [A | ●] ──→ [B | ●] ──→ [C | None]
                 n2                n3
```

**Explicação:**

- Cada `Node` possui um `valor` e uma referência `proximo` para o próximo nó.
- O atributo `proximo` é a "âncora" que mantém a corrente.
- O último nó aponta para `None`, indicando o fim da cadeia.

---

## Exercício 3 — Depuração

**Código com erro (Laço Infinito):**

```python
n1 = Node(10)
n2 = Node(20)
n3 = Node(30)

n1.proximo = n2
n2.proximo = n3

atual = n1

while atual is not None:
    print(atual.valor)
    # FALTA: atual = atual.proximo
```

**Problemas:**

1. **Qual é o problema?**
   - A variável `atual` nunca é atualizada.

2. **Por que o programa não termina?**
   - A condição `atual is not None` continua verdadeira para sempre (ela aponta para o mesmo `n1`).

3. **Qual linha deve ser acrescentada?**
   - `atual = atual.proximo`

4. **Qual será a saída depois da correção?**
   - 10, 20, 30

**Código Corrigido:**

```python
n1 = Node(10)
n2 = Node(20)
n3 = Node(30)

n1.proximo = n2
n2.proximo = n3

atual = n1

while atual is not None:
    print(atual.valor)
    atual = atual.proximo  # ← LINHA CRÍTICA!
```

**Saída:**
```
10
20
30
```

**Técnicas de Depuração:**

1. Qual variável está incorreta?
   - `atual` nunca muda.

2. Qual é o valor dela?
   - Sempre aponta para `n1`.

3. Qual objeto ela referencia?
   - O primeiro nó da cadeia.

4. Essa referência deveria mudar?
   - Sim! Ela deveria "caminhar" pela cadeia.

5. Qual condição encerra o `while`?
   - `atual is None`.

---

## Exercício 4 — Sistema de Atendimento de Clínica (Básico)

**Objetivo:** Implementar uma fila de atendimento com Pacientes e Nós.

### Classe Paciente

```python
class Paciente:
    def __init__(self, nome, idade, prioridade):
        self.nome = nome
        self.idade = idade
        self.prioridade = prioridade  # "Normal" ou "Prioridade"
    
    def __str__(self):
        return f"{self.nome} ({self.idade} anos) - {self.prioridade}"
```

### Classe Node (para Fila)

```python
class Node:
    def __init__(self, paciente):
        self.paciente = paciente
        self.proximo = None
```

### Classe FilaAtendimento

```python
class FilaAtendimento:
    def __init__(self):
        self.inicio = None
        self.fim = None
    
    def adicionar(self, paciente):
        """Adiciona um paciente ao final da fila."""
        novo = Node(paciente)
        
        if self.esta_vazia():
            self.inicio = novo
            self.fim = novo
        else:
            self.fim.proximo = novo
            self.fim = novo
    
    def atender(self):
        """Remove e retorna o primeiro paciente da fila."""
        if self.esta_vazia():
            return None
        
        paciente = self.inicio.paciente
        self.inicio = self.inicio.proximo
        
        if self.inicio is None:
            self.fim = None
        
        return paciente
    
    def listar(self):
        """Lista todos os pacientes na fila."""
        if self.esta_vazia():
            print("Fila vazia.")
            return
        
        print("\n=== FILA DE ESPERA ===\n")
        atual = self.inicio
        posicao = 1
        
        while atual is not None:
            print(f"{posicao}. {atual.paciente}")
            atual = atual.proximo
            posicao += 1
    
    def esta_vazia(self):
        """Verifica se a fila está vazia."""
        return self.inicio is None
    
    def tamanho(self):
        """Retorna a quantidade de pacientes na fila."""
        count = 0
        atual = self.inicio
        
        while atual is not None:
            count += 1
            atual = atual.proximo
        
        return count
```

### Exemplo de Uso

```python
# Criar fila
fila = FilaAtendimento()

# Adicionar pacientes
fila.adicionar(Paciente("Ana", 32, "Normal"))
fila.adicionar(Paciente("Bruno", 70, "Prioridade"))
fila.adicionar(Paciente("Carlos", 45, "Normal"))

# Listar fila
fila.listar()

# Atender pacientes
print("\n=== ATENDIMENTO ===\n")
while not fila.esta_vazia():
    paciente = fila.atender()
    print(f"Atendendo: {paciente}")

# Verificar se está vazia
print(f"\nFila vazia? {fila.esta_vazia()}")
print(f"Pacientes na fila: {fila.tamanho()}")
```

**Saída Esperada:**
```
=== FILA DE ESPERA ===

1. Ana (32 anos) - Normal
2. Bruno (70 anos) - Prioridade
3. Carlos (45 anos) - Normal

=== ATENDIMENTO ===

Atendo: Ana (32 anos) - Normal
Atendo: Bruno (70 anos) - Prioridade
Atendo: Carlos (45 anos) - Normal

Fila vazia? True
Pacientes na fila: 0
```

---

## Exercício 5 — Desafio: Atendimento Prioritário

**Objetivo:** Pacientes com prioridade são atendidos antes dos normais.

### Classe FilaComPrioridade

```python
class FilaComPrioridade:
    def __init__(self):
        self.inicio = None
        self.fim = None
    
    def adicionar(self, paciente):
        """
        Adiciona um paciente respeitando a prioridade.
        Pacientes com 'Prioridade' vão antes dos 'Normal'.
        """
        novo = Node(paciente)
        
        # Se a fila está vazia
        if self.esta_vazia():
            self.inicio = novo
            self.fim = novo
            return
        
        # Se é prioritário e o primeiro também é, insere por ordem
        # Ou se o primeiro é normal e este é prioritário
        if paciente.prioridade == "Prioridade" and \
           self.inicio.paciente.prioridade == "Normal":
            # Insere no início
            novo.proximo = self.inicio
            self.inicio = novo
        else:
            # Procura a posição correta
            atual = self.inicio
            
            while atual.proximo is not None and \
                  not (paciente.prioridade == "Prioridade" and \
                       atual.proximo.paciente.prioridade == "Normal"):
                atual = atual.proximo
            
            novo.proximo = atual.proximo
            atual.proximo = novo
            
            if novo.proximo is None:
                self.fim = novo
    
    def atender(self):
        """Remove e retorna o primeiro paciente."""
        if self.esta_vazia():
            return None
        
        paciente = self.inicio.paciente
        self.inicio = self.inicio.proximo
        
        if self.inicio is None:
            self.fim = None
        
        return paciente
    
    def listar(self):
        """Lista todos os pacientes na fila."""
        if self.esta_vazia():
            print("Fila vazia.")
            return
        
        print("\n=== FILA COM PRIORIDADE ===\n")
        atual = self.inicio
        posicao = 1
        
        while atual is not None:
            print(f"{posicao}. {atual.paciente}")
            atual = atual.proximo
            posicao += 1
    
    def esta_vazia(self):
        return self.inicio is None
    
    def tamanho(self):
        count = 0
        atual = self.inicio
        while atual is not None:
            count += 1
            atual = atual.proximo
        return count
```

### Exemplo com Prioridade

```python
# Criar fila com prioridade
fila = FilaComPrioridade()

# Adicionar pacientes (ordem de chegada)
print("Ordem de chegada:")
print("1. Ana (32 anos) - Normal")
print("2. Bruno (70 anos) - Prioridade")
print("3. Carlos (45 anos) - Normal")

fila.adicionar(Paciente("Ana", 32, "Normal"))
fila.adicionar(Paciente("Bruno", 70, "Prioridade"))
fila.adicionar(Paciente("Carlos", 45, "Normal"))

# Listar fila reorganizada
fila.listar()

# Atender
print("\n=== ORDEM DE ATENDIMENTO ===\n")
while not fila.esta_vazia():
    paciente = fila.atender()
    print(f"Atendendo: {paciente}")
```

**Saída Esperada:**
```
Ordem de chegada:
1. Ana (32 anos) - Normal
2. Bruno (70 anos) - Prioridade
3. Carlos (45 anos) - Normal

=== FILA COM PRIORIDADE ===

1. Bruno (70 anos) - Prioridade
2. Ana (32 anos) - Normal
3. Carlos (45 anos) - Normal

=== ORDEM DE ATENDIMENTO ===

Atendendo: Bruno (70 anos) - Prioridade
Atendendo: Ana (32 anos) - Normal
Atendendo: Carlos (45 anos) - Normal
```

---

## Exercício 6 — Sistema Integrado de Atendimento

**Objetivo:** Implementar um sistema completo com menu interativo.

```python
class SistemaClinica:
    def __init__(self):
        self.fila = FilaComPrioridade()
    
    def exibir_menu(self):
        print("\n" + "="*50)
        print("      SISTEMA DE ATENDIMENTO — CLÍNICA")
        print("="*50)
        print("\n1 - Adicionar paciente")
        print("2 - Listar espera")
        print("3 - Atender paciente")
        print("4 - Quantidade na fila")
        print("0 - Sair")
        print("\n" + "-"*50)
    
    def adicionar_paciente(self):
        print("\n=== ADICIONAR PACIENTE ===\n")
        nome = input("Nome: ")
        idade = int(input("Idade: "))
        
        print("\nPrioridade:")
        print("1 - Normal")
        print("2 - Prioridade")
        opcao = input("Escolha: ")
        
        prioridade = "Prioridade" if opcao == "2" else "Normal"
        
        paciente = Paciente(nome, idade, prioridade)
        self.fila.adicionar(paciente)
        
        print(f"\n✓ Paciente '{nome}' adicionado com sucesso!")
    
    def listar_espera(self):
        self.fila.listar()
    
    def atender_paciente(self):
        if self.fila.esta_vazia():
            print("\n✗ Fila vazia! Nenhum paciente para atender.")
            return
        
        paciente = self.fila.atender()
        print(f"\n✓ Atendendo: {paciente}")
    
    def quantidade_fila(self):
        total = self.fila.tamanho()
        print(f"\nPacientes na fila: {total}")
    
    def executar(self):
        while True:
            self.exibir_menu()
            opcao = input("Escolha uma opção: ")
            
            if opcao == "1":
                self.adicionar_paciente()
            elif opcao == "2":
                self.listar_espera()
            elif opcao == "3":
                self.atender_paciente()
            elif opcao == "4":
                self.quantidade_fila()
            elif opcao == "0":
                print("\n✓ Encerrando sistema. Até logo!")
                break
            else:
                print("\n✗ Opção inválida!")


# Executar o sistema
if __name__ == "__main__":
    sistema = SistemaClinica()
    sistema.executar()
```

**Interação Esperada:**
```
==================================================
      SISTEMA DE ATENDIMENTO — CLÍNICA
==================================================

1 - Adicionar paciente
2 - Listar espera
3 - Atender paciente
4 - Quantidade na fila
0 - Sair

--------------------------------------------------
Escolha uma opção: 1

=== ADICIONAR PACIENTE ===

Nome: Maria Silva
Idade: 45
Prioridade:
1 - Normal
2 - Prioridade
Escolha: 1

✓ Paciente 'Maria Silva' adicionado com sucesso!
```

---

## Resumo Conceitual

### Referências vs Cópias

| Operação | Tipo | Resultado |
|----------|------|-----------|
| `b = a` | Referência | `id(a) == id(b)` |
| `b = a.copy()` | Cópia | `id(a) != id(b)` |

### Estruturas Dinâmicas

```
LISTA ENCADEADA:
[A | ●]──→[B | ●]──→[C | None]

FILA (FIFO):
Entrada ──→ início → [  ] → [  ] → [  ] ──→ fim → Saída
            (adiciona aqui)              (remove daqui)

PRIORIDADE:
[Prioridade] → [Normal] → [Normal]
```

### Padrão Node

```python
class Node:
    def __init__(self, valor):
        self.valor = valor
        self.proximo = None  # ← Referência para o próximo nó
```

---

## Checklist de Aprendizagem

- [ ] Entendi o conceito de referência em Python
- [ ] Sei diferenciar `b = a` de `b = a.copy()`
- [ ] Consigo usar `id()` para verificar identidade de objetos
- [ ] Entendo mutabilidade (listas vs inteiros)
- [ ] Consigo criar um `Node`
- [ ] Consigo conectar nós em cadeia
- [ ] Consigo percorrer uma estrutura encadeada
- [ ] Consigo evitar laços infinitos
- [ ] Consigo implementar uma fila básica
- [ ] Consigo implementar uma fila com prioridade
- [ ] Consigo criar um menu interativo
- [ ] Consigo depurar estruturas dinâmicas

---

## Erros Comuns e Soluções

### ❌ Erro 1: Laço Infinito

```python
# ERRADO
while atual is not None:
    print(atual.valor)
    # Falta: atual = atual.proximo
```

**Solução:**
```python
# CERTO
while atual is not None:
    print(atual.valor)
    atual = atual.proximo
```

---

### ❌ Erro 2: Desconectar a Cadeia

```python
# ERRADO
self.inicio = novo  # Perde a referência anterior!
novo.proximo = self.inicio  # Referência circular!
```

**Solução:**
```python
# CERTO
novo.proximo = self.inicio  # Primeiro conectar
self.inicio = novo           # Depois atualizar início
```

---

### ❌ Erro 3: Não Atualizar Fim

```python
# ERRADO
def adicionar(self, valor):
    novo = Node(valor)
    self.inicio = novo
    # Esqueceu de atualizar self.fim!
```

**Solução:**
```python
# CERTO
def adicionar(self, valor):
    novo = Node(valor)
    if self.esta_vazia():
        self.inicio = novo
        self.fim = novo  # ← Importante!
    else:
        self.fim.proximo = novo
        self.fim = novo
```

---
