# Roteiro da apresentação (5 minutos)

**Estudante:** Luan Bispo Silva · **RGM:** 32635648  
**Disciplina:** Estrutura de Dados II · **Docente:** Profa. Kadidja Valéria

Antes de começar: deixar o notebook aberto no Colab já executado e o `grafo_cenarios.png` à mão.

**0:00 – 0:30 · Abertura**
Eu sou o Luan, RGM 32635648. Minha atividade usa a pane de GPS do Aeroporto de Brasília só como motivação. A rede de cidades e os bloqueios são inventados, então o resultado não serve para voo de verdade. A pergunta que eu quis responder foi: quantas conexões preciso bloquear para Brasília não chegar mais em Curitiba?

**0:30 – 1:15 · O grafo**
Mostrar o `grafo_diagrama.png`. Montei 10 cidades e 14 conexões, sem direção e sem pesos. Coloquei São Paulo e Goiânia como centros, algumas rotas alternativas e uma ponta no sul (Florianópolis e Porto Alegre). Usei lista de adjacência feita a partir da lista de arestas, para cada conexão aparecer dos dois lados.

**1:15 – 2:15 · BFS e DFS**
O BFS usa fila e olha as cidades por níveis, então acha o caminho com menos arestas. O DFS usa pilha e vai fundo em um ramo antes de voltar, então o caminho depende da ordem dos vizinhos. Os dois usam o conjunto de visitados para não ficar rodando em ciclo. O bloqueio é feito numa cópia, então o grafo original nunca muda.

**2:15 – 3:30 · Demonstração**
Rodar o Experimento A (BFS com 3 arestas e DFS com 5) e o B (bloqueio Goiânia–São Paulo, os dois com 3 arestas). Mostrar a validação: digitar `Curitba` e ver a sugestão "Você quis dizer: Curitiba". Se der tempo, ligar `EXECUTAR_INTERATIVO` e digitar origem, destino e um bloqueio.

**3:30 – 4:15 · Resultados**
Para separar Brasília de Curitiba são necessários 3 bloqueios, porque existem 3 caminhos sem aresta em comum e cada bloqueio quebra só um. No Experimento D, só de inverter a lista de vizinhos de Brasília o DFS foi de 5 para 7 arestas, com o mesmo grafo. Em todos os 90 pares de cidades, o BFS teve média de 2,16 arestas e o DFS de 4,31.

**4:15 – 5:00 · Testes e limitações**
Fiz 11 testes com `unittest` e comparei com o `networkx`. Todos passaram. As limitações são: a rede é inventada, não tem pesos (tempo, custo), não tem regra de aviação, e o menor corte foi feito na força bruta, que só serve para grafo pequeno. Fechar lembrando que isso é um exercício e não orienta voo real.

## Perguntas que a professora pode fazer

- **Por que o BFS acha o menor caminho?** Porque ele visita todos os vértices a *k* arestas da origem antes dos que estão a *k + 1*. Vale quando não tem pesos.
- **Por que o DFS deu 5 e o BFS 3?** O DFS foi sempre pelo primeiro vizinho (Goiânia, São Paulo, Belo Horizonte, Rio) antes de tentar Curitiba direto.
- **E sem o conjunto de visitados?** Em grafo com ciclo a busca volta para cidades já vistas, e se não existir caminho ela nunca termina.
- **Qual a complexidade?** Nos dois a busca é O(V + E). Na minha versão ainda copio o caminho a cada inserção, o que custa um pouco mais.
- **Como sei que o bloqueio não mudou a rede original?** Tem um teste que compara a rede antes e depois e confere que a conexão sai dos dois lados só na cópia.
- **Dá para usar em voo real?** Não: faltam pesos, horários, capacidade, clima e regras operacionais.
