## ToolUsage 

### 论文泛看
#### Ture Free
- **CoT[91]** 通过给予prompt的样例，让模型有一个思维链，从而通过few-shot让模型可以给出task-planing **Taks Planing, Tool Selection**。
- **ReACT[90]** 通过设定temperature=0，并给予样例的prompt，大概六条，让模型具备思维链，每条示例展现“Thought → Action → Observation”交替序列，提升明显 **Taks Planing, Tool Selection**。
- **ART[51]** 这个是与toolusage有关系的第一个模型，文章采用若干的任务示例来给模型作few-shot，如下
    ```
    Input: <原始问题>
    Q1: [<工具名>] <子任务说明>
    #1: <子任务输出>
    …  
    Qn: [EOQ]  
    Ans: <最终答案>
    ```
    []中为阶段，包含工具的使用。任务库可更改 **Taks Planing, Tool Selection**。
- **RestGPT** 带有任务分解工作，拥有planner、API Selector、Executor。planner通过用户的指令和现在已经做了的事情生成现在这一步需要处理的小任务。API Selector用的是已有的api库，即为事先收集好的网络接口，也是规范化输出，类似*GET /search/movie?query=Inception*，之后则是Executor，利用python程序去获得信息。 **Taks Planing, Tool Selection**
- **HuggingGPT** 这个和RestGPT类似，但区别在于他的API Selector变成了Model Selection，去hugging face上寻找对应的模型去做任务，想法很fancy，但感觉实现上会有困难。 **Taks Planing, Tool Selection**
- **ToolChain\*** 加入了A星算法，把所有任务可能的调用作为叶节点， 找到最优路径从而解决这个问题
#### Fine Ture
- **ToolFormer** 这个就参杂了训练的内容，比较值得关注的点在于数据集生成的方式。ToolFormer的方法是用大模型特定的prompt来清洗数据集，举例：
	
	```
  Prompts:
        Your task is to add calls to a
    Calculator API to a piece of text.
    The calls should help you get
    information required to complete the
    text. You can call the API by writing
    "[Calculator(expression)]" where
    "expression" is the expression to be
    computed. Here are some examples of API
    calls:
    Input: The number in the next term is 18
    + 12 x 3 = 54.
    Output: The number in the next term is
    18 + 12 x 3 = [Calculator(18 + 12 * 3)]
	  54.
	
  example:
        Input: The population is 658,893 people.
    This is 11.4% of the national average of
    5,763,868 people.
    Output: The population is 658,893 people.
    This is 11.4% of the national average of
    [Calculator(658,893 / 11.4%)] 5,763,868
	  people.
	```
	之后利用如上清理过的数据集对模型进行微调，数据增强的标准是添加了[]之后后续模型预测的结果的loss降低了，即表明这个数据增强是有效的。模型用的是6.7 B（67 亿）参数的 GPT‑J 
- 
