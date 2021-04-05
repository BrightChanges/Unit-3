### Quiz 25


#### Solution


##### Codes:

```.py

#TASK: COVID Stats: Present statistics on the local corona situation for a local microbiology laboratory.
#Input CSV:
#Number of samples
#SampleID, PatientID, Analysis, Result

#The statistics should be made according to these rules:
#- The analysis must be SARS.
#- Samples with patient ID CONTROL should not be counted.
#- Samples with result ERR should not be counted.
#- Duplicates of the same Sample ID should not be counted.

#Positive samples have the result POS
#Negative samples have the result NEG
#Positive rate is calculated like Individuals positive / Individuals tested x 100%.



File=[["7"],
      ["67316", "081019889", "SARS", "POS"], #this is the first valid postive tested patient =>counted toward the statistic
      ["67317", "081029889", "SARS", "POS"], #this is the second valid positive tested patient =>counted toward the statistic
      ["27593", "170814119", "SARS", "NEG"], #this is the third valid negative tested patient =>counted toward the statistic
      ["27593", "170814119", "SARS", "POS"], #invalid data record with duplicate SampleID =>counted toward the statistic
      ["222", "CONTROL", "SARS", "POS"], #invalid data record with patientID as CONTROL =>counted toward the statistic
      ["223", "17081411", "CRO", "POS"], #invalid data record with Analysis as not SARS =>counted toward the statistic
      ["224", "17091411", "SARS", "ERR"]] #invalid data record with Result as ERR =>counted toward the statistic


SampleID = []


class quiz25():

    def __init__(self, file):
        self.file = file

    def covid_statistic(self):
        positive_patient_num = 0

        num_sample = self.file[0][0]
        invalid_sample_num = 0

        for i in range(1, len(self.file)):

            #the codes below will be used to check if 1 SampleID in the file is repeated or not:
            SampleID.append(self.file[i][0])

            #the code below counts the number of invalid patient:
            if self.file[i][1] == "CONTROL" or self.file[i][3] == "ERR" or self.file[i][2] != "SARS" or SampleID.count(self.file[i][0]) > 1:
                invalid_sample_num += 1

            else:
                #we only count a patient as positive if his record has a "POS" in his Result and is a valid record:
                if "POS" in self.file[i]:
                    positive_patient_num += 1


        valid_num_sample = int(num_sample) - invalid_sample_num
        positive_rate = positive_patient_num / int(valid_num_sample) * 100

        print("Individuals positive: {}".format(positive_patient_num))
        print("Total positive: {}".format(positive_patient_num))
        print("Total tested: {}".format(valid_num_sample))
        print("Positive rate: {}%".format(positive_rate))


test1=quiz25(File)
test1.covid_statistic()

```

##### Testing:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-04-01%20at%2016.52.53.png)
