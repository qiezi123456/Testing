# UML design of Single Word Game

###### Requirement 1. When the application is started, the player may choose to (1) Play a word game, (2) View statistics, or (3) Adjust the game settings.
* Added new class Player.
* Added new class WordGame.
* Added new class Statistics, but it is replaced by GameScoreStatistics and WordStatistics later (Refer to Requirement 5).
* Added new class GameSettings.
* The application is not is the used as a class because the whole design itself is the application.
* How the player chooses from the three options is not represented in the design, as it will be handled within the GUI implementation.


###### Requirement 2. When choosing to adjust the game settings, the player 
###### (1) may choose for the game to end after a certain number of minutes, from 1 to 5, defaulting to 3; (2) may adjust the size of the square board, between 4(x4) and 8(x8), defaulting to 4; (3) may adjust the weights of the letters of the alphabet between 1 and 5, defaulting to 1.
* Added AdjustGameSettings() operation to Player class, change the game settings by call the adjust settings operations in GameSettings class.
* Added NumberOfMinutes, SizeOfSquareBoard, LettersWeights attributes to GameSettings class to record the settings.
* Added DefaultGameSettings() operation to GameSettings class, which will reset the GameSettings attributes to default after exiting the game.
* Added AdjustNumberOfMinutes(), AdjustSizeOfSquareBoard (), and AdjustLettersWeights () operations to GameSettings class to adjust settings.
* Added association Adjust between Player and GameSettings. Since the Player can change the GameSettings many times, it is 1 to n relationship.
* How the player chooses a setting to adjust and the adjusting details are not represented in the design, as it will be handled within the GUI implementation.


###### Requirement 3. When choosing to play a word game, a player will
###### (a) Be shown a ‘board’ of randomly generated letters. (b) Be shown a timer counting down the number of minutes available for the game, as set in the settings. (c) Start with 0 points, which is not required to be displayed during the game.
* Added PlayOrExitWordGamePlayer() operation to Player class, which allows the Player class to start the word game, this is partly within the GUI implementation.
* Added relationship Play between Player and WordGame. Since the Player can play many WordGame, it is 1 to n  relationship.
* Added NumberOfMinutes, SizeOfSquareBoard, LettersWeights attributes to WordGame class to record the GameSettings.
* Added GetGameSettings() operation in WordGame class, which will set the NumberOfMinutes, SizeOfSquareBoard, LettersWeights attributes in WordGame the same as GameSettings class, at the start of a game.
* Added association Get between GameSettings and WordGame. Since one WordGame can only have one specific GameSettings, and but the same GameSettings may be used in may WordGame, it is 1 to n relationship.
* Added SquareBoard, Timer, Score classes.
* Removed Timer and Score classes, created RemainingTime and Score attributes in WordGame.
* How to show the SquareBoard and the RemainingTime will be handled within the GUI implementation.


###### Requirement 3 continued. (d) Until the game timer counts to zero and the game ends: i. Enter a unique word made up of two or more letters on the board. The word must contain only letters from the board that are each adjacent to the next (horizontally, vertically, or diagonally) and a single letter on the board may not be used twice.  The word does not need to be checked against a dictionary (for simplicity, we will assume the player enters only real words that are spelled correctly).  or ii. Choose to re-roll the board at a cost of 5 points.  The board will be re-created in the same way it is generated at the start of each game, replacing each letter.  The timer will not be reset or paused.  The player’s score may go into negative values.  or iii. Choose to exit the game early.
* Added WordsThisBoard attribute to the SquareBoard class, which will record all the words played on a board in a game.
* Added WordsThisGame attribute to the WordGame class, which will record all the words played in a game.
* Added NumberOfBoardRerolls attribute to the WordGame class, which will record how many times the board is rerolled during a game.
* Added EnterWord() operation to the Player class, which will take the word the player entered, and pass it to the current SquareBoard.
* Added RerollSquareBoard() operation to the WordGame class, which will initialize/reroll the SquareBoard on request of player using SizeOfSquareBoard and LettersWeights attributes in WordGame class. 
* Added ExitWordGameTimer() operation in WordGame class, which allow the game to exit automatically when the RemainingTime counts to zero.
* Added UpdateScore() operation to WordGame class, which will calculate the Score at the end of the game using NumberOfBoardRerolls and WordsThisGame right before rerolling board, and pass the value to Score attribute in WordGame.
* Added BoardRerollWordsUpdateToGame() operation to the SquareBoard class, which will return the WordsThisBoard to the WordsThisGame and set WordsThisBoard to empty before next board reroll.
* Added CheckEnteredWordLength() operation to the SquareBoard class, which will check if the entered word is made up of two or more letters.
* Added CheckEnteredWordUsed() operation to the SquareBoard class, which will check if any letter of the entered word has already been used in current game, on current board.
* Added CheckEnteredWordHOrizontalVerticalDiagnal() operation to the SquareBoard class, which will check if each letter of the entered word are adjacent to the next horizontally, vertically or diagonally in current game, on current board.
* Added MardUsedLettersOnSquareBoard() operation to mark the used letters in current game, on current board on a SquareBoard to avoid using it twice (BoardLettersUsed in Requirement 4).
* Added association Reroll between Player and WordGame. Since the Player can reroll the board many times during the WordGame, otherwise it is going to exit, it is 1 to n  relationship.
* Added association Enter between Player and SquareBoard. Since the Player can enter many words based on one SquareBoard during a WordGame, otherwise it is going to exit, it is 1 to n  relationship.
* Added aggregation between WordGame and SquareBoard. Since the WordGame can have many SquareBoards, it is 1 to n relationship.


###### Requirement 4. Whenever the board is generated, or re-generated it will meet the following criteria: 
###### (a) The board will consist of a square full of letters. The square should have the number of letters, both horizontally and vertically, indicated by the size of the square board from the game settings (4x4, 5x5, 6x6, 7x7, or 8x8).  
* Added 2d char array attribute BoardLetters to SquareBoard class, the size will be SizeOfSquareBoard x SizeOfSquareBoard. The index of Letters will represent the positions on the SquareBoard, the values at each position will represent the letter at this position on the SquareBoard.
* Added 2d Boolean array attribute BoardLettersUsed to SquareBoard class, the size will be SizeOfSquareBoard x SizeOfSquareBoard. The index of Letters will represent the positions on the SquareBoard, the values at each position will represent whether the letter at this position on the SquareBoard has already been used by previous word entered by player in current game, on current board.
* Edited RerollSquareBoard () operation in WordGame class will update NumberOfBoardRerolls attribute in WordGame, and reset BoardLetters and BoardLettersUsed attribute in SquareBoard.

###### (b) ⅕ (rounded up) of the letters will be vowels (a,e,i,o,u). ⅘ will be consonants. 
* Edit RerollSquareBoard() operation in WordGame to achieve this, this is not shown in detail in the design.
###### (c) The letter Q will be displayed as ‘Qu’ (so that Q never appears alone).  
* This is not represented in my design, as it will be handled entirely within the GUI implementation.
###### (d) The location and particular letters should be randomly selected from a distribution of letters reflecting the weights of letters from the settings. A letter with a weight of 5 should be 5 times as likely to be chosen as a letter with a weight of 1 (assuming both are consonants or both are vowels).  In this way, more common letters can be set to appear more often.
* Edit RerollSquareBoard() operation in WordGame to achieve this, this is  not shown in detail in the design.
###### (e) A letter may appear on the board more than once.
* No need to add constrains to the RerollSquareBoard() operation in WordGame about how many times a letter occurs on the SquareBoard.


###### Requirement 5. When choosing to view statistics, the player may view (1) game score statistics, or (2) word statistics.
* Deleted Statistics class.
* Added new classes GameScoreStatistics and WordStatistics, which will save the relative statistics for the player to view.
* Added ViewGameScoreStatistics() and ViewWordStatistics() operations to Player class.
* How to choose one of the two statistics to view is not represented in my design, as it will be handled within the GUI implementation.
* Added association View between Player and GameScoreStatistics. Since the Player can view GameScoreStatistics many times, it is 1 to n relationship.
* Added association View between Player and WordStatistics. Since the Player can view WordStatistics many times, it is 1 to n relationship.


###### Requirement 6. For game score statistics, the player will view the list of scores, in descending order by final game score, displaying: 
###### (a) The final game score; (b) The number of times the board was reset; (c) The number of words entered in the game.
* Added derive attribute NumberOfWords to WordGame, which will be calculated using WordsThisGame before the game ends to count the number of words entered in this game.
* Added attributes NumberOfBoardRerolls, Score and NumberOfWords to GameScoreStatistics to record the related properties of this game.
* Added operation UpdateGameScoreStatistics() in WordGame, which will pass the NumberOfBoardRerolls, Score and NumberOfWords of a game to GameScoreStatistics to be recorded before game ends.
* Added association Update between WordGame and GameScoreStatistics. Since the GameScoreStatistics contains information of many WordGame, it is 1 to n  relationship.


###### Requirement 6 continued. The player may select any of the game scores from this list to view the settings for that game’s board size, number of minutes, and the highest scoring word played in the game (if multiple words score an equal number of points, the first played will be displayed).
* Added derive attribute HighestScoringWord to WordGame, which will be calculated using WordsThisGame before the game ends, which finds the word found in this game that got the highest score.
* Added SizeOfSquareBoard, NumberOfMinutes and HighestScoringWord to GameScoreStatistics.
* Edit operation UpdateGameScoreStatistics() in WordGame, which will pass the SizeOfSquareBoard, NumberOfMinutes and HighestScoringWord of a game to GameScoreStatistics before game ends.
* The GameScoreStatistics shows NumberOfBoardRerolls, Score and NumberOfWords of a game by descending order of Score. Player can select any Score to view SizeOfSquareBoard, NumberOfMinutes and HighestScoringWord of the game. This is not represented in my design, as it will be handled entirely within the GUI implementation.


###### Requirement 7. For the word statistics, the player will view the list of words used, starting from the most frequently played, displaying: i. The word. ii. The number of times the word has been played, across all games
* Added Word and NumberOfTimesWordPlayed attributes to WordStatistics to record the word statistics across all games. 
* Added UpdateWordStatistics() operation to WordGame, which will update WordStatistics using attribute WordsThisGame in WordGame before the game ends.
* Added association Update between WordGame and WordStatistics. Since the WordStatistics contains information of many WordGame, it is 1 to n  relationship.


###### Requirement 8. The user interface must be intuitive and responsive. 
* This is not represented in my design, as it will be handled entirely within the GUI implementation.


###### Requirement 9. The performance of the game should be such that students do not experience any considerable lag between their actions and the response of the application.
* This is not represented in my design, as it will be handled entirely within the GUI implementation.


###### Requirement 10. For simplicity, you may assume there is a single system running the application.
* The system and application are not shown in the design because there is a single system running a single application.
