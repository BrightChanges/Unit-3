### Quiz 24


#### Solution


##### Codes:

```.py




#TASK:FOIL is the standard method for multiplying two binomials together.
# The word FOIL is an acronym for the four terms of the product:
#In general: (ax + b)(cx + d) = acx² + adx + bcx + bd = acx² + (ad+bc)x + bd
#Input: list with the coefficients a, b, c, d
#Output: expanded binomial



class quiz24():

    def __init__(self, a, b, c, d):
        self.a = a
        self.b = b
        self.c = c
        self.d = d

    def activate_function(self):
        #the code below calculate the binomial following math rules
        first_output = self.a * self.c
        middle_output = (self.a*self.d)+(self.b*self.c)
        last_output = self.b * self.d
        
        #the code below is just to make sure the format
        #of the output will follow math convention:
        if first_output == 1:
            first_output = ""


        if middle_output > 0:
            middle_output = f"+ {middle_output}"
        elif middle_output == 0:
            middle_output = ""


        if last_output > 0:
            last_output = f"+ {last_output}"
        elif last_output == 0:
            last_output = ""

        return f"{first_output}x^2 {middle_output}x {last_output} "


test1 = quiz24(1,3,1,2)
test2 = quiz24(1, 13, 1, -27)

print(test1.activate_function())
print(test2.activate_function())

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-12%20at%2011.45.52%20AM.png)
