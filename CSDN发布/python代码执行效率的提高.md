## 1.列表用堆替代



## 2.用replace代替re.sub

下面是清除表情文字的一个测试例子。

```
#coding:utf-8
import re
import unittest
chat = "dafafafa[蜡烛]dafaf[发火]a"
def funcSub(chat):
    for i in range(1000000):
        chat = re.sub("da","",chat)
    print(chat)


def funcRaplce(chat):
    for i in range(1000000):
        chat = chat.replace("da","")
    print(chat)


class UserCase(unittest.TestCase):
    def testTime(self):
        #funcSub(chat)
        #funcRaplce(chat)

if __name__ == '__main__':
    unittest.main()
```

测试funcSub(chat)用时：0.808s

----------------------------------------------------------------------
Ran 1 test in 0.808s

测试funcRaplce(chat)用时：0.195s

----------------------------------------------------------------------
Ran 1 test in 0.195s

**用raplace的时间明显比re.sub缩短了很多.**

