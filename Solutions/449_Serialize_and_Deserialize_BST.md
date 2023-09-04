# 递归
### 序列化
+ 递归序列化TreeNode
+ 使用`val(left->val,right->val)`格式对TreeNode进行递归序列化
### 反序列化
+ 递归进行反序列化生成TreeNode
+ index记录当前反序列化进度和起始索引位置
+ 如果数字后跟着左括号，则需要递归序列化左右孩子结点，否则没有孩子结点，直接返回

```C++
class Codec
{
public:
	// Encodes a tree to a single string.
	std::string serialize(TreeNode *root)
	{
		if (root == NULL)
			return "";
		std::string left = serialize(root->left);
		std::string right = serialize(root->right);
		return std::to_string(root->val) + "(" + left + "," + right + ")";
	}

	// Decodes your encoded data to tree.
	TreeNode *deserialize(std::string data)
	{
		int index = 0;
		return deserialize(data, index);
	}

private:
	TreeNode *deserialize(const std::string &data, int &index)
	{
		int beginIndex = index;
		while (index < data.size() && '0' <= data[index] && data[index] <= '9')
			++index;
		if (index == beginIndex)
			return NULL;

		int val = std::stoi(data.substr(beginIndex, index - beginIndex));
		TreeNode *node = new TreeNode(val);
		if (index >= data.size() || data[index] == ',' || data[index] == ')')
			return node;

		if (data[index] == '(')
		{
			node->left = deserialize(data, ++index);
			if (data[index] == ',')
			{
				node->right = deserialize(data, ++index);
				if (data[index] == ')')
					++index;
			}
		}
		return node;
	}
};
```