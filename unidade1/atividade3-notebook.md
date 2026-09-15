# Soluções: Notebook da Aula 03
## Estruturas Dinâmicas em Python — Sistema de Fila com Prioridade

---

## PARTE 1: CONCEITOS DE REFERÊNCIA E MUTABILIDADE

### Demonstração Executada

```
=== 1. Identidade de Objetos (id()) ===

Valor de a: 5 | id(a): 11755816
Valor de b: 5 | id(b): 11755816
✓ a e b apontam para o mesmo objeto? True
```

**Explicação:**
- Em Python, pequenos inteiros (-5 a 256) são reutilizados pelo interpretador
- Quando fazemos `b = a`, ambos apontam para o **mesmo objeto na memória**
- A função `id()` confirma que os identificadores são idênticos

---

### Mutabilidade vs Imutabilidade

```
=== 2. Mutabilidade vs Imutabilidade ===

A. Com Tipo Imutável (Inteiro):
   x = 10, id(x) = 11755976
   x = x + 1
   Novo id(x) = 11756008
   ✓ ID mudou? True (Objeto foi recriado)
```

**Explicação:**
- Inteiros são **imutáveis**
- Ao modificar `x`, Python cria um **novo objeto 11** na memória
- O antigo objeto 10 é descartado pelo garbage collector
- O ID muda porque é um objeto diferente

```
B. Com Tipo Mutável (Lista):
   lista1 = [1, 2, 3], id(lista1) = 139686863096640
   lista1.append(4)
   Novo id(lista1) = 139686863096640
   ✓ ID mudou? False (Objeto alterado no mesmo local)
```

**Explicação:**
- Listas são **mutáveis**
- Quando fazemos `lista1.append(4)`, o objeto **continua na mesma posição de memória**
- O ID permanece **exatamente igual**
- Apenas o conteúdo interno foi modificado

---

### Referência vs Cópia

```
Antes de modificar:
   Original:   [10, 20, 30] | id: 139686863098432
   Referência: [10, 20, 30] | id: 139686863098432
   Cópia:      [10, 20, 30] | id: 139686861633664

Depois de lista_original.append(99):
   Original:   [10, 20, 30, 99] | id: 139686863098432
   Referência: [10, 20, 30, 99] | id: 139686863098432 ← SOFREU ALTERAÇÃO!
   Cópia:      [10, 20, 30] | id: 139686861633664 ← PERMANECEU ISOLADA
```

**Análise:**

| Operação | Tipo | ID Igual | Sofreu Mudança |
|----------|------|----------|----------------|
| `lista_referencia = lista_original` | Referência | ✓ Sim | ✓ Sim |
| `lista_copia = lista_original[:]` | Cópia | ✗ Não | ✗ Não |

---

## PARTE 2: IMPLEMENTAÇÃO DO SISTEMA

### Classe Paciente

```python
class Paciente:
    """Representa um paciente da clínica."""
    
    def __init__(self, nome: str, idade: int):
        self.nome = nome
        self.idade = idade
        self.eh_prioritario = idade >= 60  # Automático!
    
    def __repr__(self):
        status = "PRIORITÁRIO" if self.eh_prioritario else "NORMAL"
        return f"[{self.nome}, {self.idade} anos - {status}]"
```

**Regra de Prioridade:**
- ✓ Idade ≥ 60 anos → PRIORITÁRIO
- ✗ Idade < 60 anos → NORMAL

---

### Classe Node

```python
class Node:
    """Nó da lista encadeada."""
    
    def __init__(self, paciente: Paciente):
        self.dado = paciente    # Armazena o paciente
        self.proximo = None     # Referência para próximo nó
```

**Estrutura:**
```
┌─────────────────────────┐
│     DADO: Paciente      │
│  "João, 30, NORMAL"     │
├─────────────────────────┤
│   PRÓXIMO: Node (ref)   │ ──→ [próximo nó]
└─────────────────────────┘
```

---

### Classe FilaClinica - Método Adicionar

```python
def adicionar(self, nome: str, idade: int):
    """
    Adiciona um paciente respeitando prioridade.
    
    Regras:
    1. Fila vazia → novo nó é início e fim
    2. Prioritário → entra ANTES dos normais
    3. Normal → vai SEMPRE para o final
    """
    novo_paciente = Paciente(nome, idade)
    novo_no = Node(novo_paciente)
    
    # CASO 1: Fila vazia
    if self.esta_vazia():
        self.inicio = novo_no
        self.fim = novo_no
        self._tamanho += 1
        return
    
    # CASO 2: Prioritário
    if novo_paciente.eh_prioritario:
        if not self.inicio.dado.eh_prioritario:
            # Insere no início
            novo_no.proximo = self.inicio
            self.inicio = novo_no
        else:
            # Procura o último prioritário
            atual = self.inicio
            while atual.proximo and atual.proximo.dado.eh_prioritario:
                atual = atual.proximo
            
            # Insere após último prioritário
            novo_no.proximo = atual.proximo
            atual.proximo = novo_no
            
            if novo_no.proximo is None:
                self.fim = novo_no
    
    # CASO 3: Normal (vai para o final)
    else:
        self.fim.proximo = novo_no
        self.fim = novo_no
    
    self._tamanho += 1
```

**Fluxograma de Inserção:**

```
PACIENTE CHEGA
      │
      ├─ É prioritário?
      │  ├─ SIM
      │  │  ├─ Primeiro é normal? → Entra no início
      │  │  └─ Primeiro é prioritário? → Procura último prioritário, insere após
      │  │
      │  └─ NÃO (Normal)
      │     └─ Sempre vai para o final
```

---

### Classe FilaClinica - Método Atender

```python
def atender(self):
    """Remove e retorna o primeiro paciente da fila."""
    if self.esta_vazia():
        print("⚠️ Fila vazia!")
        return None
    
    paciente_atendido = self.inicio.dado
    self.inicio = self.inicio.proximo  # Avança
    self._tamanho -= 1
    
    if self.inicio is None:
        self.fim = None
    
    print(f"✅ Atendendo: {paciente_atendido.nome}")
    return paciente_atendido
```

**Operação:**
```
Antes:  [Vovó] ──→ [João] ──→ [Maria] ──→ None
                    ↑
                 (fim)

Depois: [João] ──→ [Maria] ──→ None
         ↑
      (início e fim)
```

---

## PARTE 3: EXECUÇÃO E TESTES

### Teste 1: Chegada de Pacientes

```
=== 1. CHEGADA DE PACIENTES NORMAIS E PRIORITÁRIOS ===

→ João adicionado como primeiro da fila.
→ Maria (25 anos) adicionado à fila.
→ Vovó Ana (72 anos) adicionado à fila.
→ Pedro (40 anos) adicionado à fila.
→ Vovô Bento (80 anos) adicionado à fila.

--------------------------------------------------
📋 FILA DE ESPERA ATUAL
--------------------------------------------------
1º → [Vovó Ana, 72 anos - PRIORITÁRIO]
2º → [Vovô Bento, 80 anos - PRIORITÁRIO]
3º → [João, 30 anos - NORMAL]
4º → [Maria, 25 anos - NORMAL]
5º → [Pedro, 40 anos - NORMAL]

Total na fila: 5
```

**Análise:**
- João chegou primeiro (NORMAL)
- Maria chegou segundo (NORMAL)
- Vovó Ana chegou terceira (PRIORITÁRIA) → **Vai para frente**
- Pedro chegou quarto (NORMAL)
- Vovô Bento chegou quinto (PRIORITÁRIO) → **Vai após Vovó Ana**

**Ordem Final:** Prioritários primeiro, mantendo ordem de chegada!

---

### Teste 2: Atendimento

```
→ Primeiro atendimento (Vovó Ana - Prioritária):

✅ Atendendo: Vovó Ana

--------------------------------------------------
📋 FILA DE ESPERA ATUAL
--------------------------------------------------
1º → [Vovô Bento, 80 anos - PRIORITÁRIO]
2º → [João, 30 anos - NORMAL]
3º → [Maria, 25 anos - NORMAL]
4º → [Pedro, 40 anos - NORMAL]

Total na fila: 4
--------------------------------------------------

→ Segundo atendimento (Vovô Bento - Prioritário):

✅ Atendendo: Vovô Bento

--------------------------------------------------
📋 FILA DE ESPERA ATUAL
--------------------------------------------------
1º → [João, 30 anos - NORMAL]
2º → [Maria, 25 anos - NORMAL]
3º → [Pedro, 40 anos - NORMAL]

Total na fila: 3
```

**Fluxo:**
```
ANTES:  [Vovó] ──→ [Vovô] ──→ [João] ──→ [Maria] ──→ [Pedro]
         ↓
       Atendido

DEPOIS: [Vovô] ──→ [João] ──→ [Maria] ──→ [Pedro]
         ↓
       Atendido

FINALMENTE: [João] ──→ [Maria] ──→ [Pedro]
```

---

### Teste 3: Fila Vazia e Adição de Novos Pacientes

```
=== 4. TENTANDO ATENDER COM FILA VAZIA ===

⚠️ A fila está vazia! Nenhum paciente para atender.

=== 5. ADICIONANDO NOVOS PACIENTES ===

→ Lucas adicionado como primeiro da fila.
→ Carla (35 anos) adicionado à fila.
→ Rosa (68 anos) adicionado à fila.

--------------------------------------------------
📋 FILA DE ESPERA ATUAL
--------------------------------------------------
1º → [Lucas, 65 anos - PRIORITÁRIO]
2º → [Rosa, 68 anos - PRIORITÁRIO]
3º → [Carla, 35 anos - NORMAL]

Total na fila: 3
```

**Comportamento:**
- Lucas (65) chega primeiro e é prioritário → vai para o início
- Carla (35) chega e é normal → vai para o final
- Rosa (68) chega e é prioritária → vai após Lucas mas antes de Carla

---

## CONCLUSÃO: CONCEITOS-CHAVE

### 1. Referências em Python

```
b = a          # Referência (mesmo objeto, mesmo id)
b = a.copy()   # Cópia (objeto novo, id diferente)
```

### 2. Estrutura Dinâmica

```
Node = Dado + Referência para Próximo

[Paciente] ──→ [Paciente] ──→ [Paciente] ──→ None
  (nó 1)         (nó 2)         (nó 3)
```

### 3. Fila com Prioridade

```
Entrada → [PRIORITÁRIOS] → [NORMAIS] → Saída
                  ↑
            (respeitando ordem de chegada)
```

### 4. Erro Crítico: Laço Infinito

```python
# ❌ ERRADO
while atual is not None:
    print(atual.valor)
    # Falta: atual = atual.proximo

# ✓ CORRETO
while atual is not None:
    print(atual.valor)
    atual = atual.proximo  # ← ESSENCIAL!
```

---
