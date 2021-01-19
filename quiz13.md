# Quiz 13

### 1.Sudo codes:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_9237.JPG)

### 2.Flow Diagram:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_3477.JPG)

### 3.Codes in Python:

```.py

food = ["bread","pizza", "pho"]
liquer = ["bear", "juice", "Cola"]
electronics = ["ipad", "iphone", "macbook"]

def total(items,prices):
    tax = 0
    print(items, prices)
    for i in range(len(items)):
        if items[i] in food:
            price = prices[i] * 110/100
            tax += price
        elif items[i] in electronics:
            price = prices[i] * 115/100
            tax += price
        elif items[i] in liquer:
            price = prices[i] * 120/100
            tax += price

    return tax

test1= total(["bread","bear","ipad"],[300,800,30000])
print(test1)

test2= total(["bread"],[300])
print(test2)

test3= total(["bear"],[800])
print(test3)

test4 = total(["ipad"],[30000])
print(test4)

print("test2+test3+test4={}".format(test2+test3+test4))

```

### 4.Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-01-19%20at%2010.24.20%20AM.png)
