### Test correction of quiz 11 of this test:
https://docs.google.com/document/d/1lmmFEQ1UN7NRnGZGhXE6tGXgBouqHg7d2GlR8s32UVM/edit#heading=h.b7nr46pbljrr

#### My redo:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4807.JPG)

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4808.JPG)


#### My codes so far:

1st version (looks more like the codes provided in the test, but could not be run):

```.py

NEURON = [1,2,3,4]
CUR_OUTPUT = [0.5,0.2,0.2, -0.1]
NUM_INPUTS = [1,2,2,1]
SOURCE = [4,1,3,1,2,1]
STRENGTH = [-0.1, 0.5, -0.4, 0.3, -0.4, 0.2]

def simulate(NEURON, CUR_OUTPUT, NUM_INPUTS, STRENGTH, SOURCE):

    def check_input():
        start_idx = 0
        for n in range(len(NEURON)):
            end_idx = start_idx + NUM_INPUTS[n]
            out = 0.0
            for k in range(start_idx,(end_idx-1)):
                tempt = get_index(SOURCE[k]) #get_index is a function that I will need to create
                out += STRENGTH[tempt] * CUR_OUTPUT[tempt]
            CUR_OUTPUT[n] = out
            start_idx = end_idx
            print("Output of neuron id", NEURON[n], "is", out)

        return CUR_OUTPUT
    return "Error in the initialization"


def get_index(id):

    for i in range(len(NEURON)):
        if NEURON[i] == id:
            return id


object = simulate(NEURON,CUR_OUTPUT,NUM_INPUTS,SOURCE,STRENGTH)
object.check_input()


```

2st version (could run, but provided error because the tempt variable become "None-type" sometimes during the program):

```.py
NEURON = [1,2,3,4]
CUR_OUTPUT = [0.5,0.2,0.2, -0.1]
NUM_INPUTS = [1,2,2,1]
SOURCE = [4,1,3,1,2,1]
STRENGTH = [-0.1, 0.5, -0.4, 0.3, -0.4, 0.2]


def simulate(NEURON, CUR_OUTPUT, NUM_INPUTS, STRENGTH, SOURCE):

    def get_index(id):

        for i in range(len(NEURON)):
            if NEURON[i] == id:
                return id

    start_idx = 0
    for n in range(len(NEURON)):
        end_idx = start_idx + NUM_INPUTS[n]
        out = 0.0
        for k in range(start_idx, (end_idx - 1)):
            tempt = get_index(SOURCE[k])# get_index is a function that I will need to create
            out += STRENGTH[tempt] * CUR_OUTPUT[tempt]
        CUR_OUTPUT[n] = out
        start_idx = end_idx
        print("Output of neuron id", NEURON[n], "is", out)

    return CUR_OUTPUT


simulate(NEURON,CUR_OUTPUT,NUM_INPUTS,SOURCE,STRENGTH)


```
