1. Descrição do Problema e Objetivos
A Sabor Express enfrenta um desafio operacional crítico: o gerenciamento de entregas em horários de pico. O processo atual é manual, baseado na experiência dos entregadores, resultando em rotas ineficientes. Isso causa atrasos, aumento no custo de combustível e, o mais importante, insatisfação dos clientes.

Objetivo Principal: Desenvolver uma solução de software, batizada de "Rota Inteligente", que utilize algoritmos de Inteligência Artificial para automatizar e otimizar o planejamento de rotas de entrega.

Metas Específicas:

Agrupar pedidos geograficamente próximos para otimizar a alocação de entregadores.

Calcular a rota mais eficiente (menor tempo/distância) para cada entregador realizar múltiplas entregas.

Reduzir o tempo médio de entrega e o custo operacional (combustível).

Aumentar a capacidade de entregas e a satisfação do cliente.

2. Abordagem Adotada (A Solução em Duas Fases)
A solução atua em duas fases computacionais principais:

Fase 1: Agrupamento de Pedidos (Clustering)

Utilizamos o algoritmo K-Means para agrupar todos os pedidos pendentes em k clusters, onde k é o número de entregadores disponíveis.

Isso divide a cidade em "zonas de entrega" dinâmicas, garantindo que cada entregador seja responsável por um conjunto de pedidos geograficamente próximos.

Fase 2: Otimização de Rota (Pathfinding)

Após a definição dos clusters, precisamos encontrar a ordem ótima para visitar os pontos de cada cluster (uma variação do Problema do Caixeiro Viajante - TSP).

Modelamos a cidade como um grafo ponderado.

Usamos o algoritmo *A (A-Star)** para encontrar o caminho mais curto real entre dois pontos quaisquer no grafo.

Com as distâncias reais, aplicamos uma heurística (Vizinho Mais Próximo) para definir a sequência de entregas, começando pelo restaurante.

3. Algoritmos Utilizados
🤖 K-Means (Clustering)
Por quê? É um algoritmo eficiente e popular para particionar dados em grupos.

Como? Recebe as coordenadas (latitude, longitude) dos pedidos e o número k de entregadores, e retorna k grupos de pedidos.

🗺️ Representação em Grafo
A cidade é modelada com a biblioteca NetworkX:

Nós (Vértices): Representam interseções ou locais (com atributos lat, lon).

Arestas (Edges): Representam as ruas, com um peso (ex: distancia).

📍 A* (A-Star) (Busca de Caminho)
Por quê? É um dos algoritmos de busca de caminho mais eficientes para grafos ponderados, pois usa uma heurística (distância em linha reta) para se guiar.

Uso: É usado pelo NetworkX para calcular a distância real (astar_path_length) entre dois nós no grafo.

🚚 Heurística TSP (Vizinho Mais Próximo - Nearest Neighbour)
Por quê? Encontrar a rota perfeita (TSP) é computacionalmente muito caro. Uma heurística nos dá uma solução "boa o suficiente" de forma muito rápida.

Como? Começa no restaurante, usa o A* para encontrar o ponto ainda não visitado mais próximo, move-se para ele e repete até visitar todos os pontos.
