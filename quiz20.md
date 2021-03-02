### Quiz 20


#### Solution

##### Flow Diagram:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4830.JPG)

##### Codes:

```.py

#CONTEXT: For each pair of characters of an input String, swap the two characters.


class quiz20():
    #Initialize the class:
    def __init__(self, S):
        self.S = S

    #Creating the algorithm to run the program:
    def function(self):
        
        #if the string don't contain at least 2 values,
        #S[i+1] cannot exist=>causing error for the
        #program
        if len(self.S)<2:
            output = self.S
        else:
            output = ""
            #the for loop below loops through
            #each 2 values, adding them
            #in opposite order into the
            #blank output variable:
            for i in range(0,len(self.S),2):
                output += self.S[i+1] + self.S[i]
        print(output)


#testing:
test1=quiz20("Kien").function()


```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-02%20at%2010.57.10%20AM.png)
