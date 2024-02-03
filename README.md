import random

def initialize_board():
    # Initialize a 4x4 board with two random tiles
    board = [[0] * 4 for _ in range(4)]
    add_new_tile(board)
    add_new_tile(board)
    return board

def add_new_tile(board):
    # Add a new tile (2 or 4) at a random empty position on the board
    empty_positions = [(i, j) for i in range(4) for j in range(4) if board[i][j] == 0]
    if empty_positions:
        i, j = random.choice(empty_positions)
        board[i][j] = random.choice([2, 4])

def print_board(board):
    # Print the current state of the board
    for row in board:
        print(' '.join(map(str, row)))

def move_left(board):
    # Move tiles to the left and merge if possible
    for row in board:
        row = [tile for tile in row if tile != 0]
        row += [0] * (4 - len(row))
        for i in range(3):
            if row[i] == row[i + 1]:
                row[i] *= 2
                row[i + 1] = 0
        row = [tile for tile in row if tile != 0]
        row += [0] * (4 - len(row))
    return board

def transpose(board):
    # Transpose the board (swap rows and columns)
    return [[board[j][i] for j in range(4)] for i in range(4)]

def move_right(board):
    # Move tiles to the right and merge if possible
    board = [row[::-1] for row in board]
    board = move_left(board)
    board = [row[::-1] for row in board]
    return board

def move_up(board):
    # Move tiles upwards and merge if possible
    board = transpose(board)
    board = move_left(board)
    board = transpose(board)
    return board

def move_down(board):
    # Move tiles downwards and merge if possible
    board = transpose(board)
    board = move_right(board)
    board = transpose(board)
    return board

def is_game_over(board):
    # Check if the game is over (no possible moves)
    for i in range(4):
        for j in range(4):
            if board[i][j] == 0:
                return False
            if j < 3 and board[i][j] == board[i][j + 1]:
                return False
            if i < 3 and board[i][j] == board[i + 1][j]:
                return False
    return True

# Main game loop
board = initialize_board()
while not is_game_over(board):
    print_board(board)
    move = input("Enter move (left, right, up, down): ").lower()

    if move == 'left':
        board = move_left(board)
    elif move == 'right':
        board = move_right(board)
    elif move == 'up':
        board = move_up(board)
    elif move == 'down':
        board = move_down(board)
    else:
        print("Invalid move. Please enter left, right, up, or down.")

    add_new_tile(board)

print_board(board)
print("Game over!")
