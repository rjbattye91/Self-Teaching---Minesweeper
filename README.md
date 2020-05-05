# Self-Teaching---Minesweeper
Continuing with my self learning and made a basic game of minesweeper... Click to reveal a number of how many 'mines' are around that box or red for a 'mine'. Right click to flag where you think a mine is and the square turns blue... For some reason the right click can be a bit temperamental and register as a regular click. Can anyone tell me if this is the laptop mouse or a problem with the code? If there is an easy way to change the code to be able to change the size of grid and number of 'mines' as well that would be great to know!
import tkinter
import random
gameOver = False
score = 0
squaresToClear = 0

def play_minesweeper():
    create_bombfield(bombfield)
    window = tkinter.Tk()
    layout_window(window)
    window.mainloop()

bombfield = []
def create_bombfield(bombfield):
    global squaresToClear
    for row in range(0,10):
        rowList = []
        for column in range(0,10):
            if random.randint(1,100) <20:
                rowList.append(1)
            else:
                rowList.append(0)
                squaresToClear = squaresToClear + 1
        bombfield.append(rowList)
    #printfield(bombfield)
def printfield(bombfield):
    for rowList in bombfield:
        print(rowList)

def layout_window(window):
    for rowNumber, rowList in enumerate(bombfield):
        for columnNumber, columnEntry in enumerate(rowList):
            if random.randint(1,100) < 35:
                square = tkinter.Label(window, text = "    ", bg = "darkgreen")
            elif random.randint(1,100) > 65:
                square = tkinter.Label(window, text = "    ", bg = "seagreen")
            else:
                square = tkinter.Label(window, text = "    ", bg = "green")
            square.grid(row = rowNumber, column = columnNumber)
            square.bind("<Button-1>", on_Click)
            square.bind("<Button-3>",on_RightClick)
def on_RightClick(event):
    square = event.widget
    row = int(square.grid_info()["row"])
    column = int(square.grid_info()["column"])
    square.config(bg = "blue")

def on_Click(event):
    global score
    global gameOver
    global squaresToClear
    square = event.widget
    row = int(square.grid_info()["row"])
    column = int(square.grid_info()["column"])
    currentText = square.cget("text")
    if gameOver == False:
        if bombfield[row][column] == 1:
            gameOver = True
            square.config(bg = "red")
            print("Game Over! You hit a bomb.")
            print("Your score was:", score)
        elif currentText == "    ":
            square.config(bg = "brown")
            totalBombs = 0
            if row < 9:
                if bombfield[row+1][column] == 1:
                    totalBombs = totalBombs + 1
            if row > 0:
                if bombfield[row-1][column] == 1:
                    totalBombs = totalBombs + 1
            if column > 0:
                if bombfield[row][column-1] == 1:
                    totalBombs = totalBombs + 1
            if column < 9:
                if bombfield[row][column+1] == 1:
                    totalBombs = totalBombs + 1
            if row > 0 and column > 0:
                if bombfield[row-1][column-1] == 1:
                    totalBombs = totalBombs + 1
            if row < 9 and column > 0:
                if bombfield[row+1][column-1] == 1:
                    totalBombs = totalBombs + 1
            if row > 0 and column < 9:
                if bombfield[row-1][column+1] == 1:
                    totalBombs = totalBombs + 1
            if row < 9 and column < 9:
                if bombfield[row+1][column+1] == 1:
                    totalBombs = totalBombs + 1
            square.config(text = " " + str(totalBombs) + " ")
            squaresToClear = squaresToClear - 1
            score = score + 1
            if squaresToClear == 0:
                gameOver = True
                print("Well Done! You found all the safe squares!")
                print("Your score was:", score)
play_minesweeper()
