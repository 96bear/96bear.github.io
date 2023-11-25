---
title: "python小工具示例1《日程管理小工具》"
date: "2021-07-13"
categories: 
  - "python"
  - "编程"
---

```python
# -*- coding = utf-8 -*-
# @Author : 96bearli
# @python : 3.8
"""
<日程管理小工具>
新增待办；
删除已有的待办；
标记一个待办为已完成；
查看所有待完成和已完成的待办，并且能够区分展示。
"""
from IPython.core.display import clear_output

class Task:
    """
    属性：文本内容、状态(初始值是未完成)
    方法：标记已完成。
    """

    def __init__(self, text):
        self.text = text
        self.status = "未完成"

    def mark_finished(self):
        self.status = "已完成"

class TaskList:
    """
    属性：一个列表变量
    方法：增加待办，删除待办，获得已完成的待办列表，获得未完成的待办列表
    """

    # 初始化函数，因为我们的待办都是一个个添加的，所以我们的列表变量初始化为空列表
    def __init__(self):
        self.tasks = []

    # 添加待办，接收一个参数：待办对象，然后调用列表的 append 函数将其添加到列表属性中
    def add_task(self, task):
        self.tasks.append(task)

    # 删除待办，接收一个参数：要删除待办的序号，然后通过 del 语句进行删除
    def remove_task(self, idx):
        del self.tasks[idx]

    # 获取所有已经完成的待办，没有参数。通过循环列表属性，如果状态是已完成，则添加到 result 列表中。最后返回 result 列表
    def get_finished_tasks(self):
        result = []
        for i in self.tasks:
            if i.status == "已完成":
                result.append(i)
        # 标准答案是以下循环
        # for i in range(0,len(self.tasks)):
        #     if self.tasks[i].status == "已完成"
        #         result.append(self.tasks[i])
        return result

    # 获取所有未完成的待办，逻辑类似，只是判断条件是“状态是否等于未完成”。
    def get_unfinished_tasks(self):
        result = []
        for i in range(0, len(self.tasks)):
            if self.tasks[i].status == "未完成":
                result.append(self.tasks[i])
        return result

def print_menu():
    print("Hello~\n")
    print("请问你希望做什么呢？")
    print("1. 添加待办")
    print("2. 删除待办")
    print("3. 标记待办已完成")
    print("4. 退出")

def print_tasks(finished, unfinished):
    print("未完成的待办：")
    for i in range(0, len(unfinished)):
        print(i, unfinished[i].text)
    print("")
    print("已完成的待办：")
    for i in range(0, len(finished)):
        print(i, finished[i].text)
    print("")

def print_all_tasks(all_tasks):
    for i in range(0, len(all_tasks)):
        print(i, all_tasks[i].text)

if __name__ == '__main__':
    my_task_list = TaskList()
    while True:
        clear_output()

        # 获取已完成和未完成的待办，存储在下面两个列表变量中
        finished_tasks = my_task_list.get_finished_tasks()
        unfinished_tasks = my_task_list.get_unfinished_tasks()
        # 打印待办，直接调用 print_tasks 函数，并传入上面的两个变量即可
        print_tasks(finished_tasks, unfinished_tasks)
        # 打印菜单，获取选项
        print_menu()
        choice = input("请输入要操作的序号并回车:")

        if choice == "1":
            # 通过 input 让用户输入待办内容，存储在text中
            text = input("请输入希望添加的待办内容")
            # 通过 text 构造一个 Task 对象，并添加到 my_task_list 中
            task = Task(text)
            my_task_list.add_task(task=task)
        elif choice == "2":
            clear_output()
            # 打印出当前的所有 task，并让用户选择要删的序号
            print_all_tasks(my_task_list.tasks)
            # 因为用户输入的内容是字符串的类型，所以需要转换为整型
            idx = int(input("请输入想删除的待办序号:"))
            # 调用 remove_task 方法删除对应的 task
            my_task_list.remove_task(idx)
        elif choice == "3":
            # 等于 3 时为标记待办完成，处理流程类似删除待办
            # 只是最后一步不一样
            clear_output()
            print_all_tasks(my_task_list.tasks)
            idx = input("请输入希望标记完成的待办序号")
            idx = int(idx)
            # 调用具体的 task 对象的 mark_finished 方法
            my_task_list.tasks[idx].mark_finished()
        elif choice == "4":
            # 等于 4 时代表退出，直接跳出循环即可
            break
```
