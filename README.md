# Import the sys module
import sys

# Define infinity as a large enough value. This value will be used for vertices not connected to each other
INF = sys.maxsize

# Solves all pair shortest path via Floyd Warshall Algorithm
def floyd_warshall(graph):
    """
    Finds the shortest paths between all pairs of vertices in a graph using Floyd-Warshall algorithm.
    
    Parameters:
    graph (list): The adjacency matrix representing the graph.
    
    Returns:
    list: The matrix containing the shortest distances between every pair of vertices.
    """
    num_vertices = len(graph)
    
    # Define the recursive function to find the shortest distance between vertices i and j
    def find_shortest_distance(i, j, k):
        # Base case: If k reaches the number of vertices, return the value of graph[i][j]
        if k == num_vertices:
            return graph[i][j]
        
        # Recursive case: Try all possible intermediate vertices and choose the minimum of the two options
        return min(find_shortest_distance(i, j, k + 1),
                   find_shortest_distance(i, k, k + 1) + find_shortest_distance(k, j, k + 1))
    
    # Apply the find_shortest_distance function to all pairs of vertices in the graph
    shortest_paths = []
    
    for i in range(num_vertices):
        # Initialize an empty list to store the shortest distances from vertex i to all other vertices
        distances_from_i = []
        
        # Iterate over each vertex j in the range from 0 to num_vertices - 1
        for j in range(num_vertices):
            # Find the shortest distance from vertex i to vertex j using the find_shortest_distance function
            shortest_distance = find_shortest_distance(i, j, 0)
            
            # Append the shortest distance to the list of distances from vertex i to all other vertices
            distances_from_i.append(shortest_distance)
        
        # Append the list of distances from vertex i to all other vertices to the list of shortest paths
        shortest_paths.append(distances_from_i)
    
    return shortest_paths

# Example usage
if __name__ == "__main__":
    # Test Case 1: Normal scenario
    graph1 = [[0, 5, INF, 10],
             [INF, 0, 3, INF],
             [INF, INF, 0, 1],
             [INF, INF, INF, 0]
             ]
    
    expected_result1 = [[0, 5, 8, 9],
                       [INF, 0, 3, 4],
                       [INF, INF, 0, 1],
                       [INF, INF, INF, 0]
                      ]
                      
    # Apply the Floyd-Warshall algorithm to find the shortest paths between all pairs of vertices
    shortest_paths1 = floyd_warshall(graph1)
    if shortest_paths1 == expected_result1:
        print("Test Case 1 Passed: Shortest paths calculated correctly.")
    else:
        print("Test Case 1 Failed: Shortest paths not calculated correctly.")
        
    # Test Case 2: Graph with negative distances
    graph2 = [[0, 5, INF, 10],
             [INF, 0, 3, INF],
             [INF, INF, 0, 1],
             [-1, INF, INF, 0]  # Adding negative distance
             ]
    expected_result2 = [[0, 5, 8, 9],
                       [INF, 0, 3, 4],
                       [INF, INF, 0, 1],
                       [-1, 4, 1, 0]  # Expected shortest paths with negative distance
                      ]
    shortest_paths2 = floyd_warshall(graph2)
    if shortest_paths2 == expected_result2:
        print("Test Case 2 Passed: Shortest paths with negative distances calculated correctly.")
    else:
        print("Test Case 2 Failed: Shortest paths with negative distances not calculated correctly.")
    
    # Test Case 3: All paths found
    graph3 = [
        [0, 3, INF, 7],
        [8, 0, 2, INF],
        [5, INF, 0, 1],
        [2, INF, INF, 0]
    ]
    expected_result3 = [
        [0, 3, 5, 6],
        [5, 0, 2, 3],
        [3, 6, 0, 1],
        [2, 5, 7, 0]
    ]
    shortest_paths3 = floyd_warshall(graph3)
    if shortest_paths3 == expected_result3:
        print("Test Case 3 Passed: All paths found correctly.")
    else:
        print("Test Case 3 Failed: All paths not found correctly.")
