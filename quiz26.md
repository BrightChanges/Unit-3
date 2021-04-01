### Quiz 26


#### Solution


##### Codes:

```.py

File=[["7"],
      ["67316", "081019889", "SARS", "POS"], #this is the first valid postive tested patient =>counted toward the statistic
      ["67317", "081029889", "SARS", "POS"], #this is the second valid positive tested patient =>counted toward the statistic
      ["27593", "170814119", "SARS", "NEG"], #this is the third valid negative tested patient =>counted toward the statistic
      ["27593", "170814119", "SARS", "POS"], #invalid data record with duplicate SampleID =>counted toward the statistic
      ["222", "CONTROL", "SARS", "POS"], #invalid data record with patientID as CONTROL =>counted toward the statistic
      ["223", "17081411", "CRO", "POS"], #invalid data record with Analysis as not SARS =>counted toward the statistic
      ["224", "17091411", "SARS", "ERR"]] #invalid data record with Result as ERR =>counted toward the statistic


SampleID = []


class quiz26():

    def __init__(self, file):
        self.file = file

    def covid_statistic(self):
        positive_patient_num = 0

        num_sample = self.file[0][0]
        invalid_sample_num = 0

        for i in range(1, len(self.file)):

            SampleID.append(self.file[i][0])

            if self.file[i][1] == "CONTROL" or self.file[i][3] == "ERR" or self.file[i][2] != "SARS" or SampleID.count(self.file[i][0]) > 1:
                invalid_sample_num += 1

            else:
                if "POS" in self.file[i]:
                    positive_patient_num += 1

        #print(invalid_sample_num)
        valid_num_sample = int(num_sample) - invalid_sample_num
        positive_rate = positive_patient_num / int(valid_num_sample) * 100

        print("Individuals positive: {}".format(positive_patient_num))
        print("Total positive: {}".format(positive_patient_num))
        print("Total tested: {}".format(valid_num_sample))
        print("Positive rate: {}%".format(positive_rate))


test1=quiz26(File)
test1.covid_statistic()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-01%20at%2016.52.53.png)
