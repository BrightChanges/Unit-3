### Quiz 28


#### Solution


##### Codes:

```.py

#TASK: Mirrored time number is a number of two digits x and y which
#if we write like xy:xy or xy:yx we get a valid time in hh:mm

#Your task is to print the valid combination separated
#by spaces. If no such combinations exists print None

class quiz28():

    def __init__(self,d):
        self.d = d

    def mirroredTime(self):

        #as an integer cannot be iterable, we first
        #convert it into a string, then a list:
        d1=str(self.d)
        d=list(d1)

        x = d[0]
        y = d[1]

        xy= str(x)+str(y)
        xy_xy= xy +":"+xy

        yx= str(y) + str(x)
        xy_yx = xy+ ":" +yx

        #the code below test if xy or yx would be valid in thr form
        #of hh:mm :
        if int(xy_xy[0:2])>24 or int(xy_xy[3:5])>60 or int(xy_yx[3:5])>60:
            return None
        else:
            #the codes below print the output following the proper method
            #the question asks
            print(xy_xy, ",",xy_yx)



test1 = quiz28(11)
test1.mirroredTime()

test2 = quiz28(12)
test2.mirroredTime()

test3 = quiz28(96)
test3.mirroredTime()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-06%20at%209.19.02.png)
