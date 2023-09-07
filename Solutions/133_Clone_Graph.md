# DFS/BFS+哈希表
1. DFS/BFS遍历结点，拷贝该结点，并建立原Node到拷贝Node的映射
2. DFS/BFS深拷贝相邻结点，若相邻结点已在哈希表中，则表示该结点已经完成拷贝，直接取其深拷贝结点即可。

```C++
Node* deepCopyGraph(Node* node, std::unordered_map<Node*, Node*>& cloneNodesMap)
{
	if (cloneNodesMap.count(node) > 0)
		return cloneNodesMap[node];

	Node* cloneNode = new Node(node->val);
	cloneNodesMap[node] = cloneNode;
	for (Node* neighbor : node->neighbors)
	{
		Node* cloneNeighbor = deepCopyGraph(neighbor, cloneNodesMap);
		cloneNode->neighbors.push_back(cloneNeighbor);
	}
	return cloneNode;
}

class Solution
{
public:
	Node *cloneGraph(Node *node)
	{
		if (node == NULL)
			return NULL;
		std::unordered_map<Node*, Node*> nodeMap;
		return deepCopyGraph(node, nodeMap);
	}
};
```