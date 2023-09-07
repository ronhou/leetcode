# Floyd算法
使用Floyed算法思想，计算连通图内任意两点的最短距离。在此题中，即计算任意两个变量的商值。  
## Floyed
```
k = 1~n             // 中间结点需在最外层循环
    i = 1~n         // i->k
        j = 1~n     // k->j
            dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j])
```
## Divisions
`divs[a][b] = divs[a][k] * divs[k][b] = divs[k][b] /  divs[k][a]`

```C++
std::vector<double> calcEquations(std::vector<std::vector<std::string>> &equations, std::vector<double> &values, std::vector<std::vector<std::string>> &queries)
{
	std::unordered_map<std::string, std::unordered_map<std::string, double>> eqGraph;
	for (int i = 0; i < equations.size(); i++)
	{
		const std::vector<std::string> &equation = equations[i];
		eqGraph[equation[0]][equation[1]] = values[i];
		eqGraph[equation[1]][equation[0]] = 1.0 / values[i];
		eqGraph[equation[0]][equation[0]] = eqGraph[equation[1]][equation[1]] = 1.0;
	}
	for (auto &&eqs : eqGraph)
	{
		const std::string k = eqs.first;
		for (auto &&eq1 : eqs.second)
		{
			if (eq1.first != eqs.first)
			{
				for (auto &&eq2 : eqs.second)
				{
					if (eq2.first != eqs.first && eq2.first != eq1.first)
					{
						eqGraph[eq1.first][eq2.first] = eqGraph[eqs.first][eq2.first] / eqGraph[eqs.first][eq1.first];
					}
				}
			}
		}
	}

	std::vector<double> answers;
	for (auto &&query : queries)
	{
		if (eqGraph.count(query[0]) <= 0 || eqGraph.count(query[1]) <= 0 || eqGraph[query[0]].count(query[1]) <= 0)
			answers.push_back(-1.0);
		else
			answers.push_back(eqGraph[query[0]][query[1]]);
	}
	return answers;
}
```