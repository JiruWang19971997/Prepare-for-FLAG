### 时间复杂度：O（n），空间O(N)
使用dict
56:tow sum
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
