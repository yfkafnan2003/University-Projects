#include<stdio.h>
#include<stdlib.h>

#define ROWS 20
#define COLS 10

int gameOver;
int x, y, fruitX, fruitY, Your_score;
int tailX[100], tailY[100];
int tailLength;
char direction;

void setupGame() {
    gameOver = 0;
    direction = 'd';
    x = ROWS / 2;
    y = COLS / 2;
    fruitX = rand() % ROWS;
    fruitY = rand() % COLS;
    Your_score = 0;
    tailLength = 0;
}

void drawScreen() {
    system("cls"); 
    printf("Ojogorer Khela\n");
    printf("Score: %d\n",Your_score);
    for (int i=0;i<ROWS+2;i++) printf("#");
    printf("\n");

    for (int i=0;i <COLS; i++) {
        for (int j=0;j< ROWS; j++) {
            if (j == 0) printf("#");

            if (i==y&&j==x)
                printf("O"); 
            else if (i == fruitY && j == fruitX)
                printf("*");
            else {
                int print = 0;
                for (int k=0; k< tailLength;k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        printf("o");
                        print = 1;
                        break;
                    }
                }
                if (!print) printf(" ");
            }

            if (j == ROWS - 1) printf("#");
        }
        printf("\n");
    }

    for (int i=0;i <ROWS + 2;i++) printf("#");
    printf("\n");
}

void inputControl() {
    char c;
    if (scanf(" %c",&c)) {
        if (c == 'w'|| c== 'a' || c =='s'|| c== 'd')
            direction = c;
        else if (c == 'x')
            gameOver = 1;
    }
}

void gameLogic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i=1;i <tailLength; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    if (direction == 'w') y--;
    else if (direction == 's') y++;
    else if (direction == 'a') x--;
    else if (direction == 'd') x++;

    if (x<0 ||x >= ROWS|| y<0 ||y >=COLS)
        gameOver = 1;

    for (int i =0; i< tailLength;i++)
        if (tailX[i]== x&& tailY[i] ==y)
            gameOver = 1;

    if (x == fruitX && y == fruitY) {
        Your_score += 10;
        fruitX = rand() % ROWS;
        fruitY = rand() % COLS;
        tailLength++;
    }
}

void showMenu() {
    int choice;
    while (1) {
        system("cls"); 
        printf("===== Ojogorer Khela =====\n");
        printf("1. Play Game\n");
        printf("2. Quit\n");
        printf("Choose: ");
        scanf("%d", &choice);

        if (choice == 1) {
            setupGame();
            while (!gameOver) {
                drawScreen();
                inputControl();
                gameLogic();
            }
            printf("Game Over! Final Score: %d\n",Your_score);
            printf("Press Enter to go back to menu...");
            getchar();  getchar();
        } else if(choice ==2) {
            exit(0);
        }
    }
}

int main() {
    showMenu();
    return 0;
}
