# 广度优先搜索

```C++
int minMutations(std::string startGene, std::string endGene, std::vector<std::string> &bank)
{
	if (startGene == endGene)
		return 0;

	const std::vector<char> geneFragments = {'A', 'C', 'G', 'T'};

	// 存放变化到目标基因所需的次数
	std::unordered_map<std::string, int> geneMutationMap;
	for (auto &&gene : bank)
		geneMutationMap[gene] = -1;
	geneMutationMap[startGene] = 0;

	std::queue<std::string> q;
	q.push(startGene);
	while (!q.empty())
	{
		std::string gene = q.front();
		q.pop();
		int step = geneMutationMap[gene];
		for (int i = 0; i < gene.size(); i++)
		{
			const char geneFragment = gene[i];
			for (auto &&fragment : geneFragments)
			{
				if (fragment != geneFragment)
				{
					gene[i] = fragment;
					if (geneMutationMap.count(gene) > 0 && geneMutationMap[gene] == -1)
					{
						if (gene == endGene)
							return step + 1;
						geneMutationMap[gene] = step + 1;
						q.push(gene);
					}
				}
			}
			gene[i] = geneFragment;
		}
	}
	return -1;
}
```