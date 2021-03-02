### Quiz 19


#### Solution

##### Codes:

```.py

#CONTEXT: Reverse Mode: Given the input/outputs shown, create the program that produces the output. Binary to decimal

#the decode_list belows contain all the possible different 3 values
#that is created from 0 and 1. If we add 0 to the beginning of
#values in the list, we get binary number from 0~7.
#Interestingly, the index of each value is equal to its value.

decode_list = ["000","001","010","011","100","101","110","111"]
output = []
final_output = []
str_output = ""


class ReverseMode():
    #Initialize the code
    def __init__(self, input):
        self.input = input

    #Creating the algorithms that will run the program:
    def do_function(self):

        #looping through certain ranges of the values of the inputs
        #and compare them with a ranges of values in
        #the preset decode_list:
        for i in range(len(decode_list)):
            if self.input[0:3] == decode_list[i]:
                output.append(i)

       
        for i in range(len(decode_list)):
            if self.input[4:7] == decode_list[i]:
                output.append(i)

        for i in range(len(decode_list)):
            if self.input[8:11] == decode_list[i]:
                output.append(i)

        return output

#testing:
test1 = ReverseMode("100!000!111").do_function()
for i in range(len(test1)):
    str_output += str(test1[i])


print(str_output)


```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-01%20at%204.02.09%20PM.png)
