implementation of a max Heap:
```
class Heap:
    def __init__(self):
        self.heap = []
    def left_child_index(self, i):
        return 2*i+1
    def right_child_index(self, i):
        return 2*i+2
    def pop(self):
        if not self.heap:
            raise IndexError("heap is empty")
        value = self.heap[0]
        self.heap[0] = self.heap[-1]
        self.heap.pop()
        self.heapify_down()
        return value
    def push(self, value):
        self.heap.append(value)
        self.heapify_up()
    def heapify_up(self):
        index = len(self.heap)-1
        parent_index = (index -1)//2
        while parent_index >= 0 and self.heap[parent_index] < self.heap[index]:
            self.heap[parent_index], self.heap[index] = self.heap[index], self.heap[parent_index]
            index = parent_index
            parent_index = (index-1) //2
    def heapify_down(self):
        index = 0
        left_child_index = self.left_child_index(index)
        while left_child_index<len(self.heap):
            larger_child_index = left_child_index
            right_child_index = self.right_child_index(index)
            if right_child_index<len(self.heap) and self.heap[right_child_index] > self.heap[left_child_index]:
                larger_child_index = right_child_index
            if self.heap[index]>=self.heap[larger_child_index]:
                break
            self.heap[index], self.heap[larger_child_index] = self.heap[larger_child_index], self.heap[index]
            index = larger_child_index
            left_child_index = self.left_child_index(index)
    
```
