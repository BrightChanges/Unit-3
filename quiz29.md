### Quiz 45


#### Solution


##### Codes:

```.py
#TASK: generate 10 lists with 10 random elements
#find the list with the highest sum

from random import randrange

def ten_list():

    obj = {}
    max_sum = 0
    max_list = 0
    for i in range(1,11):
        sum = 0

        obj["list"+str(i)] = []

        for a in range(10):
            x = randrange(100)
            obj["list" + str(i)].append(x)

        for b in range(len(obj["list" + str(i)])):
            sum += obj["list" + str(i)][b]
            if sum > max_sum:
                max_sum = sum
                max_list = i

        print("List {}: {}, sum: {}".format(i,obj["list"+str(i)],sum))

    print("List with biggest sum is list number {}, with total of {}".format(max_list,max_sum))


ten_list()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-4/blob/main/Screen%20Shot%200003-09-06%20at%209.06.05.png)
