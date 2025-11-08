Rota Inteligente: Otimização de Entregas para Sabor Express

1. Descrição do Problema e Objetivos
A Sabor Express enfrenta um desafio operacional crítico: o gerenciamento de entregas em horários de pico. O processo atual é manual, baseado na experiência dos entregadores, resultando em rotas ineficientes. Isso causa atrasos, aumento no custo de combustível e, o mais importante, insatisfação dos clientes.

Objetivo Principal: Desenvolver uma solução de software, batizada de "Rota Inteligente", que utilize algoritmos de Inteligência Artificial para automatizar e otimizar o planejamento de rotas de entrega.

Metas Específicas:

Agrupar pedidos geograficamente próximos para otimizar a alocação de entregadores.

Calcular a rota mais eficiente (menor tempo/distância) para cada entregador realizar múltiplas entregas.

Reduzir o tempo médio de entrega e o custo operacional (combustível).

Aumentar a capacidade de entregas e a satisfação do cliente.

2. Abordagem Adotada (A Solução em Duas Fases)
Para resolver o desafio, a solução "Rota Inteligente" atua em duas fases computacionais principais, executadas assim que um lote de pedidos está pronto para sair:

Fase 1: Agrupamento de Pedidos (Clustering)

Durante picos de demanda, é ineficiente tratar cada pedido individualmente.

Utilizamos um algoritmo de aprendizado não supervisionado (K-Means) para agrupar todos os pedidos pendentes em k clusters, onde k é o número de entregadores disponíveis no momento.

Isso divide a cidade em "zonas de entrega" dinâmicas, garantindo que cada entregador seja responsável por um conjunto de pedidos geograficamente próximos.

Fase 2: Otimização de Rota (Pathfinding)

Após a definição dos clusters, cada entregador tem uma lista de entregas (ex: 5 pedidos).

O problema agora é encontrar a ordem ótima para visitar esses 5 pontos e retornar à base (uma variação do Problema do Caixeiro Viajante - TSP).

Para isso, modelamos a cidade como um grafo ponderado.

A distância/tempo real entre dois pontos quaisquer (ex: Pedido A -> Pedido B) não é uma linha reta. Usamos o algoritmo *A (A-Star)** para encontrar o caminho mais curto real entre eles, navegando pelas arestas (ruas) do grafo.

Com as distâncias reais entre todos os pontos do cluster, aplicamos uma heurística (como o "Vizinho Mais Próximo") para definir a sequência de entregas, começando pelo restaurante.

3. Algoritmos Utilizados
🤖 K-Means (Clustering)
Por quê? É um algoritmo eficiente e popular para particionar dados em grupos (clusters).

Como?

Recebe as coordenadas (latitude, longitude) de todos os N pedidos pendentes.

Recebe o número k de entregadores disponíveis.

Ele agrupa os N pedidos nos k clusters mais coesos e geograficamente densos.

Resultado: k listas de pedidos, uma para cada entregador.

🗺️ Representação em Grafo
A cidade é modelada como um grafo onde:

Nós (Vértices): Representam interseções de ruas ou os próprios locais de entrega/restaurante.

Arestas (Edges): Representam os segmentos de rua que conectam os nós.

Pesos: Cada aresta tem um "custo" (peso), que pode ser a distância (em metros) ou o tempo estimado (distância / velocidade média da via).

📍 A* (A-Star) (Busca de Caminho)
Por quê? É um dos algoritmos de busca de caminho mais eficientes para grafos ponderados. É superior ao BFS (Busca em Largura), que só encontra o caminho mais curto em grafos não ponderados (ignora o custo), e ao DFS (Busca em Profundidade), que não garante o caminho mais curto.

Como? O A* encontra o caminho de menor custo de um ponto A para um ponto B. Ele equilibra o custo já percorrido (distância real) com uma heurística (uma estimativa do custo restante, como a distância em linha reta até o destino).

Uso: É a nossa "régua" para medir a distância real entre dois pontos de entrega no grafo.

🚚 Heurística TSP (Vizinho Mais Próximo - Nearest Neighbour)
Por quê? Encontrar a rota perfeita para visitar múltiplos pontos (TSP) é computacionalmente muito caro (NP-difícil). Uma heurística nos dá uma solução "boa o suficiente" de forma muito rápida.

Como?

Começa no restaurante (ponto inicial).

Usa o A* para calcular a distância do ponto atual para todos os pontos ainda não visitados no cluster.

Move-se para o ponto mais próximo (menor custo A*).

Repete o processo até que todos os pontos do cluster sejam visitados.

Por fim, calcula a rota A* do último ponto de volta ao restaurante.

4. Diagrama do Grafo/Modelo
(Nesta seção, seria incluído um diagrama visual. O diagrama representaria um grafo simples:)

[Diagrama do Modelo de Grafo]

Nós: R (Restaurante), A, B, C, D, E (Interseções)

Pontos de Entrega: P1, P2

Arestas: Linhas conectando os nós (ex: R-A, A-B, A-E, B-C, B-P1, E-D, D-P2, C-P2)

Pesos: Números em cada aresta (ex: R-A = 5 min, A-E = 3 min, E-D = 4 min, D-P2 = 2 min)

*Exemplo de Rota A (R -> P2):** A rota mais curta não seria R-A-B-C-P2 (custo alto), mas sim R -> A -> E -> D -> P2 (custo total = 5 + 3 + 4 + 2 = 14 min).

5. Análise e Próximos Passos
Eficiência e Resultados
A implementação combinada de K-Means e A* (com heurística TSP) transforma o processo de dispatch.

K-Means garante que os entregadores não cruzem a cidade desnecessariamente, focando em zonas densas.

A* garante que a rota entre os pontos de entrega seja a mais rápida possível, considerando a estrutura real das ruas.

Impacto Esperado: Redução de até 30% no tempo total de entrega e economia de 15-25% em combustível (similar a benchmarks de mercado, como o da UPS ORION).

Limitações Encontradas
Grafos Estáticos: A solução atual usa pesos fixos (distância ou tempo médio). Ela não considera o tráfego em tempo real, que pode alterar drasticamente o "custo" de uma rota.

K-Means Simples: O K-Means exige que o número de clusters (k) seja definido manualmente (número de entregadores). Além disso, ele cria clusters de formato esférico, o que pode não ser ideal para a geografia das ruas (rios, viadutos).

Heurística Subótima: O "Vizinho Mais Próximo" é rápido, mas pode levar a rotas que não são as ideais, especialmente em clusters complexos.

Sugestões de Melhoria (Próximos Passos)
Integração com APIs (Tráfego Real): Substituir os pesos estáticos por dados dinâmicos de APIs (como Google Maps ou Waze) para que o A* calcule rotas baseadas nas condições atuais do trânsito.

Clustering Avançado (DBSCAN): Explorar o DBSCAN, que pode encontrar clusters de formas arbitrárias (seguindo o desenho das ruas) e identificar "ruído" (pedidos muito isolados que exigem tratamento especial).

Otimização de TSP (Algoritmos Genéticos): Para clusters maiores, substituir a heurística simples por um algoritmo mais robusto, como Algoritmos Genéticos ou Simulated Annealing, para encontrar rotas mais próximas da perfeição.

Aprendizado por Reforço (RL): A longo prazo (como visto no estudo do ResearchGate), um agente de RL poderia aprender as melhores políticas de roteamento e dispatch dinâmico, considerando padrões de tráfego, horários de pico e até mesmo a preparação de pedidos na cozinha.


