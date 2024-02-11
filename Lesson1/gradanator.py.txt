"""gradanator.py - grades based on inputs

More details

Julien Abshere
Lab #1

Problem analysis: grade papers

Program specifications: do  a good job

Design (pseudo code):

Testing:
"""
import math

#	------------		Run Factors 		------------------------------------------------------

UseTestVals = True
TestVals = [10,78,10,84,30,100,50,3,14,15,17,20,19,25,10]

ExamCount = 2
SectionMaxPoints = 34
SectionValue = 3

#	------------		Declarations 		------------------------------------------------------

TestIndex = 0
QryWeight = "Weight (0-100)? "
QryScore = "Score Earned? "
QryNumAssign = "Number of Assignments? "
OutTotal = "Total point = %s / 100"
OutWeightedScore = "Weighted score = %s"
QryHWScore = "Assignment %s score? "
QryHWMax = "Assignment %s max? "
QrySectionsAttended = "How many sections did you attend? "
LetterGrades = {90:['A', 'Excellent!'], 80:['B', 'Nice Job!'],70:['C', 'Could do better'],60:['D', 'Needs Improvement'],0:['F', 'Try again next time'],}
HomeworkCount = 0
HomeworkMaxPoints = 0
HomeworkTotalPoints = 0
SectionCount = 0
TotalWeightedScore = 0.0

#	------------		Functions 		------------------------------------------------------

def FetchExam(ExamName):

	print('\nExam %s:' % ExamName)

	if(UseTestVals):
		global TestIndex
		Weight = TestVals[TestIndex]; TestIndex+=1
		print(QryWeight + str(Weight))
		Score = TestVals[TestIndex]; TestIndex+=1
		print(QryScore + str(Score))

	else:
		Weight = int(input(QryWeight))
		Score = int(input(QryScore))

	print(OutTotal % Score)
	WeightedScore = Score*(Weight/100)
	print(OutWeightedScore % str(round(WeightedScore,1)))
	global TotalWeightedScore; TotalWeightedScore += WeightedScore
	return ''
# end FetchExam

def FetchHomework(HomeworkName):

	if(UseTestVals):
		global TestIndex
		Score = TestVals[TestIndex]; TestIndex+=1
		print(QryHWScore % HomeworkName + str(Score))
		Max = TestVals[TestIndex]; TestIndex+=1
		print(QryHWMax % HomeworkName + str(Max))

	else:
		Score = int(input(QryHWScore % HomeworkName))
		Max = int(input(QryHWMax % HomeworkName))

	global HomeworkMaxPoints; HomeworkMaxPoints += Max
	global HomeworkTotalPoints; HomeworkTotalPoints += Score
	return
	# end for
# end FetchHomework

#	------------		Main 		------------------------------------------------------

print('This program reads exam/homework scores '
'and reports your overall course grade.')

for i in range(ExamCount):
	FetchExam('Exam ' + str(i+1))

FetchExam('Final Exam')
print('\nHomework:')

if(UseTestVals):
	Weight = TestVals[TestIndex]; TestIndex+=1
	print(QryWeight + str(Weight))
	HomeworkCount = TestVals[TestIndex]; TestIndex+=1
	print(QryNumAssign + str(HomeworkCount))

else:
	Weight = int(input(QryWeight))
	HomeworkCount = int(input(QryNumAssign))

for i in range(HomeworkCount):
	FetchHomework(str(i+1))

HomeworkTotalPoints = min(HomeworkTotalPoints, HomeworkMaxPoints)

if(UseTestVals):
	SectionCount = TestVals[TestIndex]; TestIndex+=1
	print(QrySectionsAttended + str(SectionCount))

else:
	SectionCount = int(input(QrySectionsAttended))

HomeworkMaxPoints += SectionMaxPoints
HomeworkTotalPoints += min(SectionCount * SectionValue, SectionMaxPoints)

print('Section Points = %s / %s' % (SectionCount*SectionValue, SectionMaxPoints) )
print('Total Points = %s / %s' % (HomeworkTotalPoints, HomeworkMaxPoints) )
HomeworkWeightedScore = (HomeworkTotalPoints*Weight)/ HomeworkMaxPoints
print(OutWeightedScore % str(round(HomeworkWeightedScore, 1)))
TotalWeightedScore+=HomeworkWeightedScore
print('\nOverall percentage = %s' % str(round(TotalWeightedScore,1)))
GradeKey = math.floor(TotalWeightedScore/10)*10
print('Your grade will be at least: %s' % LetterGrades[GradeKey][0])
print(LetterGrades[GradeKey][1])






