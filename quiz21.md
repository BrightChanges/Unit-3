### Quiz 21


#### Solution


##### Codes:

```.py

#TASK: Create a function that outputs a bracket emoji with both arms. The input 
# given its face and its left arm. The right arm is a mirror image of the left arm


#exception for unicode pattern (+2):
# / : 47
# \ : 92
# (: 40
# ): 41

# <...\ : left_input
# [?????] : middle_input
# /...> : right_input (opposite of input 1)

#ord convert letter to Unicode's number
#chr convert Unicode's number to letter

class quiz21():

    def __init__(self, left_input, middle_input):
        self.left_input = left_input
        self.middle_input = middle_input

    def emoji(self):
        right_input = ""
        for i in range(len(self.left_input)):

            left_symbol_of_right_input = self.left_input[-1]
            right_symbol_of_right_input = self.left_input[0]

            if left_symbol_of_right_input not in "/ \\ ! - |. abcdefghijklmnopqrstuvwxyz":
                left_symbol_of_right_input = chr(ord(left_symbol_of_right_input)+2)
            if right_symbol_of_right_input not in "/ \\ ! - |. abcdefghijklmnopqrstuvwxyz":
                right_symbol_of_right_input = chr(ord(right_symbol_of_right_input)+2)

#exception symbol for left symbol:
            if left_symbol_of_right_input == "/":
                left_symbol_of_right_input = "\\"

            if left_symbol_of_right_input == "\\":
                left_symbol_of_right_input = "/"

            if left_symbol_of_right_input == "(":
                left_symbol_of_right_input = ")"

            if left_symbol_of_right_input == ")":
                left_symbol_of_right_input = "("

# exception symbol for right symbol:
            if right_symbol_of_right_input == "/":
                right_symbol_of_right_input = "\\"

            if right_symbol_of_right_input == "\\":
                right_symbol_of_right_input = "/"

            if right_symbol_of_right_input == "(":
                right_symbol_of_right_input = ")"

            if right_symbol_of_right_input == ")":
                    right_symbol_of_right_input = "("


            right_input = left_symbol_of_right_input + self.left_input[1:-1] + right_symbol_of_right_input
            # print(right_input)

        return self.left_input + self.middle_input + right_input


test1 = quiz21("<...\\", "[?????]")
print(test1.emoji())

test2 = quiz21("o-","('|-|')")
print(test2.emoji())

test3 = quiz21("|-","(~n~)")
print(test3.emoji())

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-05%20at%205.01.20%20PM.png)
