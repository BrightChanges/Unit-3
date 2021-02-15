### Quiz 16

#### My solutions:

##### 1. Non-Forloops:
-Pseudocodes: 

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4765.jpg)

-Codes:
```.py

def make_bricks(small:int,big:int,goal:int):

    if (small >= goal and small%goal==0) or (big*5 >= goal and big*5%goal==0) or \
            ((small+big*5) >= goal and (small+big*5)%goal ==0):
        return True
    return False


print(make_bricks(3,1,8))

print(make_bricks(3,4,9))

print(make_bricks(3,2,10))

```
-Testing:
![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-15%20at%204.47.29%20PM.png)



##### 2. Forloops:
-Pseudocodes: 

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4766.jpg)

-Codes:
```.py

def make_bricks(small:int,big:int,goal:int):

    total_small = 0
    total_big = 0

    for i in range(0,small):
        total_small+=i
    for i in range(0, big):
        total_big += 5

    # print(total_small)
    print(total_big)
    total_all = total_small + total_big

    if (total_all >=goal and total_all%goal==0) or (total_small >= goal and total_small%goal==0) or (total_big>=goal and total_big%goal==0):
        return True
    return False



print(make_bricks(3,1,8))

print(make_bricks(3,4,9))

print(make_bricks(3,2,10))


```
-Testing:
![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-15%20at%204.47.37%20PM.png)
