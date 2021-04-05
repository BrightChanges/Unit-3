### Quiz 28


#### Solution


##### Codes:

```.py
class quiz28():

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def abundant(self):

        abundants = []

        for i in range(self.a,self.b+1):

            sum = 0

            for x in range(1,i):

                if i%x == 0:
                    sum+=x

            if sum > i:
                abundants.append(i)

        return len(abundants)


test1=quiz28(1,40)

print(test1.abundant())

test2=quiz28(10,86)

print(test2.abundant())

test3=quiz28(13,78)

print(test3.abundant())

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-05%20at%2014.57.38.png)
