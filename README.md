# JioAug：基于 JioNLP 的文本数据增强

## Installation

```shell
git clone https://github.com/zejunwang1/JioAug
cd ./JioAug
pip install .
```

## Usage

***homophone_substitution***

对于句子中的中文字词，以一定的概率进行同音词替换，从而实现数据增强。

```python
def homophone_substitution(
    text: str,
    augmentation_num: int = 3,
    homo_ratio: float = 0.1,
    min_count: int = 1000,
    pos: bool = False,
    allow_mispronounce: bool = True,
    seed: int = 37
)
```

- `text`: 原始句子

- `augmentation_num`: 数据增强得到的句子数，默认为 3

- `homo_ratio`: 对每一个中文词汇的同音词替换概率，默认为 0.1

- `min_count`: 拼音对应的所有词汇的最小总频次，默认为 1000

- `pos`: 是否进行词性标注以防止替换人名词汇，默认为 False

- `allow_mispronounce`: 是否允许方言读音误读，如 zh 与 z 卷舌不分，默认为 True，允许词汇错音

- `seed`: 随机种子

```python
from jionlp import homophone_substitution
# 模型和词典加载初始化
warmup = homophone_substitution("模型和词典加载初始化", augmentation_num=1, homo_ratio=0.1, pos=False)
text = "人工智能与机器学习是当下非常热门的一个学科。"
sentences = homophone_substitution(text, augmentation_num=3, homo_ratio=0.1, pos=False)
for sentence in sentences:
    print(sentence)
```

```context
人工智能于机器学习是当下非常热门的一个学科。
人工智能与机器学习使当下非常热门的一个学科。
人工智能与机器学习四诞下非常热门的一个学科。
```

***homophone_substitution_window***

设定窗口大小 `w`，在每 `w` 个词语中随机选择一个中文字词进行同音词替换，从而实现数据增强。

```python
def homophone_substitution_window(
    text,
    augmentation_num: int = 3,
    window: int = 10,
    min_count: int = 1000,
    pos: bool = False,
    allow_mispronounce: bool = True,
    seed: int = 37
)
```

- `window`: 滑动窗口大小，默认为 10，即从每 10 个词语中随机选择一个中文字词进行替换。其余参数与 `homophone_substitution` 中的参数含义相同

```python
from jionlp import homophone_substitution_window
# 模型和词典加载初始化
warmup = homophone_substitution_window("模型和词典加载初始化", augmentation_num=3, window=10, pos=False)
text = "人工智能与机器学习是当下非常热门的一个学科。"
sentences = homophone_substitution_window(text, augmentation_num=3, window=10, pos=False)
for sentence in sentences:
    print(sentence)
```

```context
人工智能与机器学习是当下非常热门得一个学科。
人工智能与机器学习十当下非常热门的一个学科。
人工智能与机器学习时当下非常热门的一个学科。
```








