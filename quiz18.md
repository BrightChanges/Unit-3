### Quiz 18


#### Solution

##### Codes:

```.py

class StringN():

    def __init__(self,N):
        self.N = N

    def do_function(self):
        #because I will use result in multiplication, i cannot set it equal to 0 because
        #that will turn all the result of the multiplication to 0.
        result = 1

        #initialize other necessary variables that I could changes using for loops later on:
        step = 1
        new_string = ""

        for i in range(len(self.N)):
            result *= int(self.N[i])
            new_string = str(result)


        #when result>9, the string N or new_string(updated value of string N after each multiplication)
        #contains 2 digits, which could be used for multiplication.
        # a string with 1 digit cannot be used for multiplication.
        while result > 9:
            for i in range(len(new_string)):
                step +=1
                result = 1
                result *= int(new_string[i])

        print(step)

#testing:
test1=StringN("39").do_function()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-01%20at%204.01.59%20PM.png)
