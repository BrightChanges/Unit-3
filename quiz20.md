### Quiz 20


#### Solution

##### Flow Diagram:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4830.JPG)

##### Codes:

```.py

class quiz20():

    def __init__(self, S):
        self.S = S

    def function(self):
        if len(self.S)<2:
            output = self.S
        else:
            output = ""
            for i in range(0,len(self.S),2):
                output += self.S[i+1] + self.S[i]
        print(output)



test1=quiz20("Kien").function()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-03-02%20at%2010.57.10%20AM.png)
