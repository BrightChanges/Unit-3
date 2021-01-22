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

### 5. Advanced codes:
(letting the user adding their own items into the system to check when their items are not already in the system)
```.py

food = ["bread","pizza", "pho"]
liquer = ["bear", "juice", "Cola"]
electronics = ["ipad", "iphone", "macbook"]

def total(items,prices):
    tax = 0
    print("Your input is:{},{}".format(items, prices))
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
        else:
            user_input = input("Which category is {}? Please choose by typing in 1 of the"
                               "following 3 categories: food, liquer, electronics:".format(items[i]))

            if user_input != "Food" and user_input != "food" and user_input !="Liquer" and user_input != "liquer" and user_input != "Electronics" and user_input != "electronics":
                print("Please restart the program. You had entered an invalid category" "for {}. The tax shown below IS NOT ACCURATE. PLEASE RESTART".format(items[i]))
                break


            elif user_input == "Food" or user_input == "food":
                food.append(items[i])
                if items[i] in food:
                    price = prices[i] * 110 / 100
                    tax += price
                elif items[i] in electronics:
                    price = prices[i] * 115 / 100
                    tax += price
                elif items[i] in liquer:
                    price = prices[i] * 120 / 100
                    tax += price

            elif user_input == "Liquer" or "liquer":
                liquer.append(items[i])
                if items[i] in food:
                    price = prices[i] * 110 / 100
                    tax += price
                elif items[i] in electronics:
                    price = prices[i] * 115 / 100
                    tax += price
                elif items[i] in liquer:
                    price = prices[i] * 120 / 100
                    tax += price

            elif user_input == "Electronics" or "electronics":
                electronics.append(items[i])
                if items[i] in food:
                    price = prices[i] * 110 / 100
                    tax += price
                elif items[i] in electronics:
                    price = prices[i] * 115 / 100
                    tax += price
                elif items[i] in liquer:
                    price = prices[i] * 120 / 100
                    tax += price


    return tax

test1= total(["bread","bear","ipad", "apple"],[300,800,30000,5000])
print(test1)

```
