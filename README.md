<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name:MADHANRAJ P     </h3>
<h3>Register Number: 212223220052          </h3>
<h3>Date: 26-10-2026

<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>



1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
   ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
successor.f = successor.g + successor.h
          
iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
            end (for loop)
  
e) push q on the closed list
end (while loop)



<H3>Program:</H3>

```


from collections import defaultdict
import networkx as nx
import matplotlib.pyplot as plt

def a_star(graph, heuristic, start, goal):
    open_set = {start}
    closed_set = set()
    g = {start: 0}
    parent = {start: None}

    while open_set:
        current = min(open_set, key=lambda node: g[node] + heuristic[node])

        if current == goal:
            path = []
            while current is not None:
                path.append(current)
                current = parent[current]

            path.reverse()
            print("\nShortest Path :", " -> ".join(path))
            print("Total Cost :", g[goal])
            return path

        open_set.remove(current)
        closed_set.add(current)

        for neighbour, cost in graph[current]:
            if neighbour in closed_set:
                continue

            new_cost = g[current] + cost

            if neighbour not in open_set:
                open_set.add(neighbour)
            elif new_cost >= g.get(neighbour, float('inf')):
                continue

            g[neighbour] = new_cost
            parent[neighbour] = current

    print("Path does not exist!")
    return None


graph = defaultdict(list)
G = nx.Graph()

n, e = map(int, input("Enter number of nodes and edges: ").split())

print("\nEnter the edges (u v cost):")
for i in range(e):
    u, v, cost = input(f"Edge {i+1}: ").split()
    cost = int(cost)

    graph[u].append((v, cost))
    graph[v].append((u, cost))
    G.add_edge(u, v, weight=cost)

print("\nAdjacency List")
for node in graph:
    print(node, "->", graph[node])

heuristic = {}

print("\nEnter Heuristic Values")
for i in range(n):
    node, h = input(f"{i+1}. Node Heuristic: ").split()
    heuristic[node] = int(h)

print("\nHeuristic Values")
print(heuristic)

# Draw Original Graph
plt.figure(figsize=(8, 6))
pos = nx.spring_layout(G, seed=20)

nx.draw_networkx_nodes(G, pos, node_color="skyblue", node_size=1800)
nx.draw_networkx_labels(G, pos, font_size=12, font_weight="bold")
nx.draw_networkx_edges(G, pos, width=2)

edge_labels = nx.get_edge_attributes(G, 'weight')
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)

plt.title("Original Graph")
plt.axis("off")
plt.show()

start = input("\nEnter Start Node: ")
goal = input("Enter Goal Node: ")

if start not in graph:
    print("Invalid Start Node!")
elif goal not in graph:
    print("Invalid Goal Node!")
else:
    shortest_path = a_star(graph, heuristic, start, goal)

    if shortest_path:
        # Highlight Shortest Path
        path_edges = []
        for i in range(len(shortest_path) - 1):
            path_edges.append((shortest_path[i], shortest_path[i+1]))

        plt.figure(figsize=(8, 6))

        nx.draw_networkx_nodes(G, pos, node_color="skyblue", node_size=1800)
        nx.draw_networkx_labels(G, pos, font_size=12, font_weight="bold")

        # Draw all edges
        nx.draw_networkx_edges(G, pos, edge_color="gray", width=2)

        # Highlight Shortest Path
        nx.draw_networkx_edges(
            G, pos, edgelist=path_edges,
            edge_color="red", width=4
        )

        nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)

        plt.title("Shortest Path Highlighted (Red)")
        plt.axis("off")
        plt.show()

```
<H3>output:</H3>

<img width="835" height="637" alt="image" src="https://github.com/user-attachments/assets/0eb00610-90f1-4c00-a21d-a423cb165734" />
<img width="776" height="625" alt="image" src="https://github.com/user-attachments/assets/fc01d033-5150-4998-ac74-efb0e02cd710" />
<img width="852" height="721" alt="image" src="https://github.com/user-attachments/assets/8ba05e32-af5a-4b9d-b208-09ce95ca8edc" />



<hr>
<h3>Result:</h3>
<hr>
<p>Thus,The experiment was done successfully.</p>
