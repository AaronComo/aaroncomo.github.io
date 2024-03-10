---
author: Weifeng Liu
math: true
---


项目地址: [github.com/AaronComo/chomsky_hierarchy_classifier
](https://github.com/AaronComo/chomsky_hierarchy_classifier)

## 1	实验介绍

### 1.1	实验内容

构建 Chomsky 法分类器



### 1.2	实验环境

- 硬件: Macbook Air
- 系统架构: arm64
- 编程环境: VS Code
- Python 环境: python 3.9.2



### 1.3	原理分析

- 3 型文法: 左侧只有一个非终结符, 右侧为左线性 / 右线性
- 2 型文法: 左侧至少一个非终结符
- 1 型文法: 左侧多个非终结符, 左侧 < 右侧
- 0 型文法: 非 1、2、3 型



## 2	实验流程

### 2.1 主要函数

- `judge` 一次将文法规则送入 3~1 型文法判断函数, 由高到低判断类型, 使用 `flag` 变量记录文法种类

    ~~~python
    def judge(rules, flag=0):
        if is_chomsky3(rules): flag = 3
        elif is_chomsky2(rules): flag = 2
        elif is_chomsky1(rules): flag = 1
        return flag
    ~~~

- `is_chomsky3` 按合法规则判断是否为三型文法

    ~~~python
    def is_chomsky3(rules) -> bool:
        for l, r in rules:  # l: left part   r: right parts.
            if len(l) > 1 or len(r) == 1 and l.islower(): return False
            for i in r:
                if len(i) == 1 and not i.isupper(): continue
                elif len(i) == 2 and not i[0].isupper() and i[1].isupper(): continue  
                elif len(i) == 2 and i[0].isupper() and not i[1].isupper(): continue  
                else: return False
        return True
    ~~~

    - $A::=a$
    - $A::=aB$
    - $A::=Ab$

- `is_chomsky2` 判断左侧是否只有非终结符

    ~~~python
    def is_chomsky2(rules) -> bool:
        for l, r in rules:
            if not l.isupper(): return False  # A->anything
        return True
    ~~~

- `is_chomsky1` 判断左侧是否含有非终结符, 且左 < 右

    ~~~python
    def is_chomsky1(rules) -> bool:
        for l, r in rules:
            if l.islower(): return False  # at least one V_N
            for i in r:
                if len(l) <= len(i): continue  # length of left <= right
                else: return False
        return True
    ~~~



### 2.2	测试

- 1 型文法

    ![image-20230404235614007](/assets/img/2023-04-05-chomsky-hierarchy-classifier.assets/image-20230404235614007.png)

- 2 型文法

    ![image-20230404235633257](/assets/img/2023-04-05-chomsky-hierarchy-classifier.assets/image-20230404235633257.png)

- 3 型文法

    ![image-20230405000137601](/assets/img/2023-04-05-chomsky-hierarchy-classifier.assets/image-20230405000137601.png)

- 0 型文法

    ![image-20230404235957368](/assets/img/2023-04-05-chomsky-hierarchy-classifier.assets/image-20230404235957368.png)
