The data files are as follows: 

For Age: 
The projection vectors: 
AgeVectors.txt      	2989X21 MATRIX each column is a projection that is applied to a 
			cropped (61x49) version of a face, reshaped by stacking columns from the left to right.
			output for a single face is a 1x21 projected face. 

AgeSamples.txt		A training set of 4550 projected faces.  (4550x21)
AgeTruth.txt		the "true" estimated ages of each face. 4550x1 There are 7 age classes: 
			1-->	0-2
			5-->	3-7
			10-->	8-12
			16-->	13-19
			28-->	20-36
			51-->	37-65
			75-->	66+

GenderVectors.txt	2989x8 projection for gender

GenderSamples.txt	Training Set of 4550 projected faces (4550x8)

GenderTruth		The True Genders. 4550x1
			1--> Female
			2--> Male

Note that the Gender and Age samples (4550) are actually in the same order. 



Algorithm: 
1. Get a face. Resize to 61x49. Turn into a single feature vector (1x2989)
2. Project onto the AgeVectors and onto the GenderVectors. 
3. Measure Euclidean distance to each AgeSample and Each Gender Sample. 
4. Find the nearest N (e.g. N=25) samples in each space. 
5. Find the associated Age and Gender Classes for the nearest neighbor. 
6. Use these class assignments to estimate P(age | appearance) and P(gender|appearance). 

			






% MATLAB COMMANDS TO CREATE THE ASCII DATA FILES FOR AGE AND GENDER CLASSIFICATION: 

load eventrain
load eventest
load c:\research\GroupShotAnalysis\AgeGenderTraining\genderFisher8 %loads finalFisherGender
load c:\research\GroupShotAnalysis\AgeGenderTraining\ageFisher21 %has finalFisher in it
genProject = double(trcoll.faceimg)*finalFisherGender;
ageProject = double(trcoll.faceimg)*finalFisher;
genProject2 = double(tecoll.faceimg)*finalFisherGender;
ageProject2 = double(tecoll.faceimg)*finalFisher;
allAges = [trcoll.ageClass;tecoll.ageClass];
allGenders = [trcoll.genClass;tecoll.genClass];
genSamples = [genProject;genProject2];
ageSamples = [ageProject;ageProject2];

%SAVE THE DATA IN ASCII FORMAT
save GenderSamples.txt genSamples -Ascii
ageSamples = double(ageSamples);
save AgeSamples.txt ageSamples -Ascii

finalFisher = double(finalFisher);
save AgeVectors.txt finalFisher -Ascii
save GenderVectors.txt finalFisherGender -Ascii

allAges = double(allAges);
save AgeTruth.txt allAges -Ascii
allGenders = double(allGenders);
save GenderTruth.txt allGenders -Ascii