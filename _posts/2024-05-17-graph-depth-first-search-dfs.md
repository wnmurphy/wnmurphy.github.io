---
title: Graph Depth-First Search (DFS)
date: 2024-05-17T13:40:22-08:00
layout: single
permalink: /graph-depth-first-search-dfs/
category: 'programming'
tags: ['technical interviews', 'algorithms', 'data structures']
---

A *graph* is a data structure that represents a network, consisting of nodes called *vertices* (an intersection of connections), and connections between nodes called *edges*.

Each vertex in a graph tracks its own edges by maintaining an *adjacency list* of other connected vertices.

## DFS vs. BFS

One method of traversing a graph is called Depth-First Search (DFS). In DFS, you search for a target vertex by following each possible unique path through the graph until the end of that path.

This is different from Breadth-First Search (BFS), where you consider each of the immediately-adjacent vertices in turn first before choosing to traverse to any one of them.

BFS looks at all possible immediately-adjacent vertices before choosing one (imagine fungus spreading in a petri dish), and DFS goes as far as it can for a given option before backtracking and considering another (imagine a bolt of lightning).

DFS is ideal when you need to determine whether a complete path to a target vertex exists, because you fully explore each path to see if it qualifies before moving to the next possible path. One example would be to determine if a maze contains a viable path from the entrance to the exit.

BFS is ideal when you're looking for a target vertex which is likely to be nearby the starting node. An example might be traversing a social network to find someone who knows your friend.

## Tracking progress with a set

For DFS, we'll use an auxiliary data structure to track which nodes we've already visited. We need to know if we've already seen a vertex because a graph can be cyclical. A cycle in the graph is a path through that forms a loop and returns you to a node you've already seen.

A set is ideal for this, because we can check in constant time whether a reference to the current vertex exists in the set. If so, then we've already been here before and can skip it.

## Pseudocode

1. Initialize a set `already_visited` to track vertices we've already visited.
2. For a given starting vertex `V1` and a given target vertex `Vt`,
3. Check if `V1` is `Vt`. If so, return True because we've found the target.
4. Otherwise, check whether we've already visited `V1` by checking whether it's in the `already_visited` set. If so, it's not the target, so return False.
5. Otherwise, iterate over the vertices in `V1.adjacency_list` and recursively call this function on each in turn, returning the result.

This guarantees that we either return True if the target node `Vt` exists in the graph, or False if it does not.

For an iterative solution, you can use a stack as another auxiliary data structure to simulate the call stack from a recursive approach.

## Implementation

```python
class Vertex():

	def __init__(self, id, adjacency_list=None):
		if adjacency_list is None:
			adjacency_list = []
		self.id = id
		self.adjacency_list = adjacency_list

	def add_edge_to(self, vertex):
		self.adjacency_list.append(vertex)

class Graph():

	def __init__(self):
		self.vertices = {}
		self.last_vertex_id = 0

	def add_vertex(self, adjacency_list=None):
		new_vertex_id = self.last_vertex_id + 1
		self.last_vertex_id = new_vertex_id
		new_vertex = Vertex(new_vertex_id, adjacency_list)
		self.vertices[new_vertex_id] = new_vertex
		return new_vertex

	def remove_vertex_by_id(self, vertex_id):
		if vertex_id not in self.vertices.keys():
			return False
		self.vertices.pop(vertex_id)
		return True

	def dfs(self, current_vertex, target_vertex_id, already_visited=None):
		if already_visited is None:
			already_visited = set()
		# Are we already looking at the vertex we want?
		if current_vertex.id == target_vertex_id:
			return True
		# Have we been to this vertex already?
		if current_vertex.id in already_visited:
			return False
		# Mark that we've been here,
		already_visited.add(current_vertex.id)
		# Keep looking for the target ID until we reach the end of a recursive branch.
		for vertex in current_vertex.adjacency_list:
			if self.dfs(vertex, target_vertex_id, already_visited):
				return True
		return False
```
