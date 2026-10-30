# Grafos, BFS e DFS

**Disciplina:** Estrutura de Dados II

**Docente:** Profa. Kadidja Valéria

**Estudante:** Luan Bispo Silva

**RGM:** 32635648

---

Atividade em que montei uma rede fictícia de 10 cidades e usei BFS e DFS para achar caminhos de Brasília até Curitiba, com e sem conexões bloqueadas. A pane de GPS no Aeroporto de Brasília (G1, 22/09/2026) é só a motivação: a rede e os bloqueios são inventados e não servem para rota de voo real.

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/luanbispo13/estrutura_dados_2/blob/main/grafos_bfs_dfs/solucao_grafos.ipynb)

## Arquivos

| Arquivo | O que é |
|---|---|
| [`atividade-grafos-bfs-dfs.md`](atividade-grafos-bfs-dfs.md) | Documento da atividade, com perguntas (P:), dados (D:), respostas (R:), código em Python e saídas |
| [`solucao_grafos.ipynb`](solucao_grafos.ipynb) | Notebook com o mesmo código, pronto para rodar no Colab |
| [`grafo_diagrama.png`](grafo_diagrama.png) | Desenho da rede (10 cidades e 14 conexões) |
| [`grafo_cenarios.png`](grafo_cenarios.png) | BFS e DFS no cenário normal e com o bloqueio Goiânia–São Paulo |
| [`Material_Estudante_Grafos_BFS_DFS.md`](Material_Estudante_Grafos_BFS_DFS.md) · [`.pdf`](Material_Estudante_Grafos_BFS_DFS.pdf) | Enunciado da atividade |

## Como rodar

- **Colab:** clicar no botão acima e usar *Ambiente de execução → Executar tudo*.
- **No computador:** `pip install networkx matplotlib jupyter` e depois `jupyter notebook solucao_grafos.ipynb`.
- Para digitar origem, destino e bloqueios, trocar `EXECUTAR_INTERATIVO = False` por `True` na célula da seção 5.

## Resultado em resumo

- Para Brasília não chegar mais em Curitiba é preciso bloquear **3 conexões** (existem 3 caminhos sem nenhuma aresta em comum).
- Na rede completa, o BFS achou um caminho com 3 arestas e o DFS com 5. Invertendo a lista de vizinhos de Brasília, o DFS foi para 7.
- Testes: 11 testes com `unittest`, todos passaram.
