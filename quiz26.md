### Quiz 26


#### Solution


##### Codes:

```.py

#TASK: Create a function that returns the sum of the numbers in the array, except
# ignore sections of numbers starting with a 6 and extending to the next 7
# (every 6 will be followed by at least one 7). Return 0 for no numbers.

#Example
#sum67([1, 2, 2]) → 5
#sum67([1, 2, 2, 6, 99, 99, 7]) → 5
#sum67([1, 1, 6, 7, 2]) → 4


class quiz26():

    def __init__(self, array):
        self.array = array

    def return_sum(self):

        #we create the variable flag below to act as a stopper, which
        #tell us when we should count the number in the list, and when we should not count the number in the list
        flag = False
        sum = 0

        for i in range(len(self.array)):

            if flag == False:
                #when Flag is true, it means that we just found a 6 in our list
                #this means that we don't count 6 and all the numbers behind
                #it until we found a number 7 and Flag is set back to False
                if self.array[i] == 6:
                    flag = True
                else:
                    sum += self.array[i]

            else:
                if self.array[i] == 7:
                    flag = False

        return sum

test1= quiz26([1,2,2])
print(test1.return_sum())

test2= quiz26([1,2,2,6,99,99,7])
print(test2.return_sum())


test3= quiz26([1,1,6,7,2])
print(test3.return_sum())

test4= quiz26([])
print(test4.return_sum())

test5= quiz26([1,2,2,6,99,99,7,6,7,5])
print(test5.return_sum())

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-02%20at%2014.40.35.png)
