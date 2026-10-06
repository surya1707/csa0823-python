READ N, M
READ grid

K ← N / M
SPLIT grid into K × K sheets

sourceSheet ← sheet containing S
destinationSheet ← sheet containing D

FOR each sheet:
    STORE its distinct rotations: 0°, 90°, 180°, 270°

answer ← INFINITY
board ← empty N × N grid
used ← empty set

FUNCTION shortestDistance(board):
    FIND positions of S and D
    queue ← [(S.row, S.column, 0)]
    visited ← {S}

    WHILE queue is not empty:
        row, column, distance ← REMOVE FRONT of queue

        IF board[row][column] = D:
            RETURN distance

        FOR each of the four adjacent cells:
            IF cell is inside board
               AND cell contains T, S, or D
               AND cell is not visited:
                ADD cell to visited
                ADD (cell.row, cell.column, distance + 1) to queue

    RETURN INFINITY

FUNCTION arrange(position):
    IF position = K × K:
        answer ← MIN(answer, shortestDistance(board))
        RETURN

    IF position = 0:
        candidates ← [sourceSheet]
    ELSE IF position = K × K - 1:
        candidates ← [destinationSheet]
    ELSE:
        candidates ← unused sheets excluding sourceSheet and destinationSheet

    FOR each sheet in candidates:
        IF sheet is used:
            CONTINUE

        ADD sheet to used

        FOR each distinct rotation of sheet:
            PLACE rotated sheet at position in board
            arrange(position + 1)
            REMOVE sheet from position in board

        REMOVE sheet from used

arrange(0)

IF answer = INFINITY:
    PRINT -1
ELSE:
    PRINT answer
