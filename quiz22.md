
### Quiz 22


#### Solution


##### Codes:

```.py

#TASK: https://docs.google.com/presentation/d/1UM2YhbMQ3aSwWPUbhLs0PvUREkstN_mZLQgBblLl6WA/edit#slide=id.p

class quiz22():

    def __init__(self, S):
        self.S = S

    def function(self):
        
        #the reason why we need to split the string and then add
        #them in the end to the output is because we 
        #don't want to output them in a group of separate strings
        output=[]
        S_list = self.S.split()

        for i in range(len(S_list)):

            #the code below counts the length of all the character
            #between the first and the last character of the input string
            middle_length = len(S_list[i][1:-1])

            if middle_length == 0:
                middle_length = ""

            if len(S_list[i]) > 1:
                output.append(str(S_list[i][0]) + str(middle_length) + str(S_list[i][-1]))
            else:
                output.append(str(S_list[i]))

        output = " ".join(output)

        return output


test1= quiz22("internationalization")
print(test1.function())

test2= quiz22("localization")
print(test2.function())

test3= quiz22("Hello world !")
print(test3.function())

test4= quiz22("(codin) + (game) = (codingame)")
print(test4.function())
```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-08%20at%202.11.15%20PM.png)
