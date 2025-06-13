//TIC-TAC-TOE-
#include <stdio.h>

char board[3][3];
char currentMarker;
int currentPlayer;

void resetBoard() {
    for (int i = 0, count = '1'; i < 3; i++) {
        for (int j = 0; j < 3; j++, count++) {
            board[i][j] = count;
        }
    }
}

void drawBoard() {
    printf("\n");
    for (int i = 0; i < 3; i++) {
        printf(" %c | %c | %c ", board[i][0], board[i][1], board[i][2]);
        if (i != 2) printf("\n---|---|---\n");
    }
    printf("\n\n");
}

int placeMarker(int slot) {
    int row = (slot - 1) / 3;
    int col = (slot - 1) % 3;

    if (board[row][col] != 'X' && board[row][col] != 'O') {
        board[row][col] = currentMarker;
        return 1;
    }
    return 0;
}

int checkWinner() {
    for (int i = 0; i < 3; i++) {
        if ((board[i][0] == board[i][1] && board[i][1] == board[i][2]) ||
            (board[0][i] == board[1][i] && board[1][i] == board[2][i]))
            return 1;
    }
    if ((board[0][0] == board[1][1] && board[1][1] == board[2][2]) ||
        (board[0][2] == board[1][1] && board[1][1] == board[2][0]))
        return 1;

    return 0;
}

void swapPlayerAndMarker() {
    if (currentMarker == 'X') {
        currentMarker = 'O';
        currentPlayer = 2;
    } else {
        currentMarker = 'X';
        currentPlayer = 1;
    }
}

int main() {
    int slot;
    int moves = 0;
    resetBoard();
    currentMarker = 'X';
    currentPlayer = 1;

    drawBoard();

    while (1) {
        printf("Player %d (%c), choose your slot (1-9): ", currentPlayer, currentMarker);
        scanf("%d", &slot);

        if (slot < 1 || slot > 9) {
            printf("Invalid slot! Try again.\n");
            continue;
        }

        if (!placeMarker(slot)) {
            printf("Slot already taken! Try another.\n");
            continue;
        }

        moves++;
        drawBoard();

        if (checkWinner()) {
            printf("Player %d wins! ðŸŽ‰\n", currentPlayer);
            break;
        } else if (moves == 9) {
            printf("It's a draw!\n");
            break;
        }

        swapPlayerAndMarker();
    }

    return 0;
}
