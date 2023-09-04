# 数组+哈希表
```C++
Node *copyRandomList(Node *head)
{
	std::vector<Node *> copies;
	std::unordered_map<Node *, int> nodeIndices;
	Node *pointer = head;
	int index = 0;
	while (pointer != NULL)
	{
		Node *duplicateNode = new Node(pointer->val);
		if (!copies.empty())
			copies.back()->next = duplicateNode;
		copies.push_back(duplicateNode);
		nodeIndices[pointer] = index++;
		pointer = pointer->next;
	}

	pointer = head;
	index = 0;
	while (pointer != NULL)
	{
		if (pointer->random != NULL)
			copies[index]->random = copies[nodeIndices[pointer->random]];
		pointer = pointer->next;
		index++;
	}
	return !copies.empty() ? copies[0] : NULL;
}
```