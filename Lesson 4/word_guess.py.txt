"""word_guess.py - guess a word by selecting letters

More details

Julien Abshere
Lab #4

Problem analysis:

Program specifications:

Design (pseudo code):


Play Loop:

	Ask Player #1 for a word; Assumption: player 2 is unaware of the word
	Ask Player #1 for a number of allowed guesses or the word 'Default'; if Default, the guess count = 13 + 2*(word length)
	Store secret word as an array of chars
	Create a 'discovery' array for tracking letters that have been guessed

	Guess Loop:
		Prompt player #2 to choose a single alpha letter
		validate the input; pay attention to only the first char
			if not an alpha, warn the user and Continue
			if an alpha already guessed, decrement the remaining guess count. warn user, and Continue
		Mark the discovery array with matches; decrement the remaining guess count if no letters are matched
		Show the result as letters (where matched) or a star (where not matched). Show the remaining guess count
		If all letters have been discovered, indicate success, and Break
		If the guess count = 0 indicate failure, and Break

	Ask if the the players want to play again. If yes, then Continue, otherwise Break


Testing:
"""
import math
import re
import os

#				Declarations 				------------------------------------------------------------------

InPlay = True

#				Functions 				------------------------------------------------------------------

def GenerateResultWord(GuessLetter: str, WordLetters: list, Matches: list):
	ResultWord = ''
	#	Mark the discovery array with matches; decrement the remaining guess count if no letters are matched
	#	if an alpha already guessed, decrement the remaining guess count. warn user, and Continue
	for i in range(len(WordLetters)):
		#	Show the result as letters (where matched) or a star (where not matched). Show the remaining guess count
		if GuessLetter == Letters[i]:
			Matches[i] = 1
			ResultWord += Letters[i]
		else:
			if Matches[i]: ResultWord += Letters[i]
			else: ResultWord += '*'
	return ResultWord

#				Main 				------------------------------------------------------------------

os.system('cls')

#Play Loop:
while (InPlay):

	TheWord = ''
	WordLength = 0
	IsGuessCountValid = False
	GuessCount = 0
	LastGuessSum = 0

	#Ask Player #1 for a word
	while (len(TheWord) < 3):
		TheWord = input("Player 1 - Select a secret word (min 3 letters): ")
		if TheWord.lower() in ('end', 'done', 'exit'):
			print('Thanks for playing!'); exit()

	WordLength = len(TheWord)
	#Ask Player #1 for a number of allowed guesses or null; if null, the guess count = 13 + 2*(word length)
	while (not IsGuessCountValid):
		GuessCount = input("Player 1 - # of allowed guesses (minimum of 5, Enter for default): ")
		if (GuessCount == ''): GuessCount = 13 + math.floor(2*WordLength); IsGuessCountValid = True; break
		if GuessCount.isnumeric():
			GuessCount = math.floor(abs(int(GuessCount)))
			if GuessCount > 4: IsGuessCountValid = True

	os.system('cls')

	#Store secret word as an array of chars; Create a 'discovery' array for tracking letters that have been guessed
	Letters = list(TheWord)
	Matches = [0]*WordLength

	#Guess Loop:

	while (GuessCount > 0):
		#	Prompt player #2 to choose a single alpha letter
		GuessLetter = input("Player 2 - guess a letter: ")
		if (GuessLetter == ''): print ('Invalid input; try again'); continue
		if (GuessLetter.lower() in ('end', 'done', 'exit')): InPlay = False; break
		#	validate the input; pay attention to only the first char
		GuessLetter = GuessLetter[0]
		m = re.search('[a-zA-Z]', GuessLetter)
		if m is None: print ('Invalid input; try again'); continue

		ResultWord = GenerateResultWord(GuessLetter, Letters, Matches)
		NewGuessSum = sum(Matches)
		if NewGuessSum == LastGuessSum: GuessCount -= 1
		else: LastGuessSum = NewGuessSum

		print('Guesses Remaining: %s      The word: %s' % (GuessCount, ResultWord))

		if NewGuessSum == WordLength:
			#	If all letters have been discovered, indicate success
			print('Congradurations ! You done it!'); break
		elif GuessCount == 0:
			#	If the guess count = 0 indicate failure, and Break
			print('Out of guesses. The word was: %s' % TheWord)

	# end Guess Loop

	# Ask if the the players want to play again. If yes, then Continue, otherwise Break
	IsAPlaya = input('Would you like to Play again (Y or N)? ')
	InPlay = True if IsAPlaya.upper() == 'Y' else False

#end play loop


