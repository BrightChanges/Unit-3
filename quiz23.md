### Quiz 23


#### Solution


##### Codes:

```.py

#TASK: Write a function that checks if a triangle can be built using three integers a, b, c, which are the sides of the triangles


class quiz23():

    def __init__(self, A,B,C):
        self.A = A
        self.B = B
        self.C = C

    def function(self):
        #in Math, every side of a triangle needs to be smaller than
        #the sum of the other two sides.
        #the if statement below is constructed based on that rule:
        if (self.A+self.B)>self.C and (self.B+self.C)>self.A and (self.A+self.C)>self.B:
            return True

        return False


test3 = quiz23(1,1,2)
print(test3.function())


test2 = quiz23(5,6,3)
print(test2.function())

test2 = quiz23(1,2,9)
print(test2.function())

test1 = quiz23(3,9,2)
print(test1.function())







```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-09%20at%208.14.45%20AM.png)
