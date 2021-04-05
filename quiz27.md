### Quiz 27


#### Solution

##### Flow Diagram:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_0295.jpg)

##### Codes:

```.py
#TASK: An abundant number is a number that is smaller than the sum of its proper divisors
#(1 and itself excluded)
#Calculate the number of abundant numbers between an integer a and an integer b (a and b included)

class quiz27():

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def abundant(self):

        #we created a list called abundants so we could then add all the valid abundants
        #and take their the list's length as the output
        abundants = []

        #we loop through every i number between a and b:
        for i in range(self.a,self.b+1):

            sum = 0

            #we loop from all integer from 1 to every number i
            #we loop as below to get all the divisors of each number i
            for x in range(1,i):

                if i%x == 0:
                    sum+=x

            #we check if each i is lower than the sum of its divisors.
            #if yes, it's an abundant number:
            if sum > i:
                abundants.append(i)

        return len(abundants)


test1=quiz27(1,40)

print(test1.abundant())

test2=quiz27(10,86)

print(test2.abundant())

test3=quiz27(13,78)

print(test3.abundant())


```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-05%20at%2014.57.38.png)
