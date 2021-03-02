### Quiz 19


#### Solution

##### Codes:

```.py

decode_list = ["000","001","010","011","100","101","110","111"]
output = []
final_output = []
str_output = ""


class ReverseMode():

    def __init__(self, input):
        self.input = input

    def do_function(self):

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


test1 = ReverseMode("100!000!111").do_function()
for i in range(len(test1)):
    str_output += str(test1[i])
    
print(str_output)

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-01%20at%204.02.09%20PM.png)
