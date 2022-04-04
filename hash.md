## 时间复杂度：O（n），空间O(N)
使用dict
### 56:tow sum
```python
from typing import (
    List,
)

class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def two_sum(self, numbers: List[int], target: int) -> List[int]:
        # write your code here
        record_d = {}
        for idx,num in enumerate(numbers):
            if target - num in record_d.keys():
                return [record_d.get(target - num),idx]
            else:
                record_d[num] = idx
        return [-1,-1]
```
### 594 · strStr II
```python
class Solution:
    """
    @param source: A source string
    @param target: A target string
    @return: An integer as index
    """
    def str_str2(self, source: str, target: str) -> int:
        # write your code here
        if len(target) == 0:
            return 0
        if len(source) == 0:
            return -1

        power = 1
        hash_code = 31
        base = 1000000
        # 31^3:(abcd)-31^3=bcd
        for i in range(len(target)):
            power = power*hash_code % base
        #target hashcode
        target_code = 0
        for i in range(len(target)):
            target_code = (target_code * 31 + ord(target[i]))%base
        print(target_code)
        source_code = 0
        for i in range(len(source)):
            source_code = (source_code * 31+ord(source[i]))%base
            if i < len(target):
                continue
            print(i)
            #judge
            source_code -= power*ord(source[i-len(target)])%base
            
            if source_code < 0:
                source_code += base
            print(source_code)
            if source_code == target_code:
                return i-len(target)+1
        return -1
```
