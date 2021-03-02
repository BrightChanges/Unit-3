### Quiz 18


#### Solution

##### Codes:

```.py

class StringN():

    def __init__(self,N):
        self.N = N

    def do_function(self):

        result = 1
        step = 1
        new_string = ""

        for i in range(len(self.N)):
            result *= int(self.N[i])
            new_string = str(result)
       

    #convert new_string to a list, splitting each integer into 1 value:
        list_new_string = list(new_string)


        while result > 9:
            for i in range(len(list_new_string)):
                step +=1
                result = 1
                result *= int(list_new_string[i])

        print(step)

test1=StringN("39").do_function()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-01%20at%204.01.59%20PM.png)
