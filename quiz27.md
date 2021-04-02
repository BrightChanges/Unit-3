### Quiz 27


#### Solution


##### Codes:

```.py



class quiz27():

    def __init__(self, array):
        self.array = array

    def return_sum(self):

        flag = False
        sum = 0

        for i in range(len(self.array)):

            if flag == False:
                if self.array[i] == 6:
                    flag = True
                else:
                    sum += self.array[i]

            else:
                if self.array[i] == 7:
                    flag = False

        return sum

test1= quiz27([1,2,2])
print(test1.return_sum())

test2= quiz27([1,2,2,6,99,99,7])
print(test2.return_sum())


test3= quiz27([1,1,6,7,2])
print(test3.return_sum())

test4= quiz27([])
print(test4.return_sum())

test5= quiz27([1,2,2,6,99,99,7,6,7,5])
print(test5.return_sum())

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-02%20at%2014.40.35.png)
