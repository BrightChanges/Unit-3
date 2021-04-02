### Quiz 27


#### Solution


##### Codes:

```.py


class quiz27():

    def __init__(self, array):
        self.array = array


    def return_sum(self):
        first_total = 0
        not_total = 0
        index_of_6 = 0
        index_of_7 = 0

        if len(self.array) == 0:
            return 0
        else:
            for i in range(len(self.array)):
                first_total += self.array[i]
                if self.array[i]== 6:
                    index_of_6 = i
                elif self.array[i]== 7:
                    index_of_7 = i

            if index_of_6!= 0 and index_of_7!=0:
                for i in range(index_of_6,(index_of_7+1)):
                    not_total += self.array[i]


            final_total = first_total - not_total

            return final_total

test1= quiz27([1,2,2])
print(test1.return_sum())

test2= quiz27([1,2,2,6,99,99,7])
print(test2.return_sum())


test3= quiz27([1,1,6,7,2])
print(test3.return_sum())

test4= quiz27([])
print(test4.return_sum())

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-02%20at%2013.49.59.png)
