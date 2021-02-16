### Quiz 17


#### Solution:


##### 1.OOP:

-Pseudocodes:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4767.jpg)

-Codes:
```.py

class evenlySpaced():

    def __init__(self, small, medium, large):
        self.small:int = small
        self.medium:int = medium
        self.large:int = large

    def find_answers(self):
        if (((self.large-self.medium)%(self.medium-self.small))==0):

            return True
        return False



test1=evenlySpaced(2,4,6)
test2=evenlySpaced(4,6,2)
test3=evenlySpaced(4,6,3)

print(test1.find_answers())
print(test2.find_answers())
print(test3.find_answers())

```

-Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-16%20at%2012.15.05%20PM.png)

##### 2. Normal functions:
-Pseudocodes:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4768.jpg)

-Codes:
```.py

def evenlySpaced(small:int,medium:int,large:int):

    if (((large - medium) % (medium - small)) == 0):

        return True
    return False


test1=evenlySpaced(2,4,6)
test2=evenlySpaced(4,6,2)
test3=evenlySpaced(4,6,3)

print(test1)
print(test2)
print(test3)

```

-Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-16%20at%2012.15.09%20PM.png)
