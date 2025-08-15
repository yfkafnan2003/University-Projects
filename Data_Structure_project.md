    #include<stdio.h>
    #include<stdlib.h>
    
    #define ROWS 20
    #define COLS 10
    
    struct Node {
        int x, y;
        struct Node *next;
    };
    
    int gameOver;
    int x, y, fruitX, fruitY, Your_score;
    char direction;
    struct Node *snake = NULL; 
    
    void addTail(int tx,int ty){
        struct Node *newNode=malloc(sizeof(struct Node));
        newNode->x = tx;
        newNode->y = ty;
        newNode->next = snake;
        snake = newNode;
    }
    
    void removeLast() {
        if (!snake || !snake->next) return;
        struct Node *temp = snake;
        struct Node *prev = NULL;
        while (temp->next) {
            prev = temp;
            temp = temp->next;
        }
        prev->next = NULL;
        free(temp);
    }
    
    int snakeLength() {
        int count = 0;
        struct Node *temp = snake;
        while (temp) {
            count++;
            temp = temp->next;
        }
        return count;
    }
    
    int isSnakeHere(int cx, int cy) {
        struct Node *temp = snake;
        while (temp) {
            if (temp->x == cx && temp->y == cy) return 1;
            temp = temp->next;
        }
        return 0;
    }
    
    void setupGame() {
        gameOver = 0;
        direction = 'd';
        x = ROWS / 2;
        y = COLS / 2;
        fruitX = rand() % ROWS;
        fruitY = rand() % COLS;
        Your_score = 0;
        while (snake) { struct Node*tmp =snake;snake =snake->next;free(tmp);}
        addTail(x, y);
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
                else if (isSnakeHere(j,i))
                    printf("o");
                else
                    printf(" ");
    
                if (j == ROWS - 1) printf("#");
            }
            printf("\n");
        }
    
        for (int i=0;i <ROWS + 2;i++) printf("#");
        printf("\n");
    }
    
    void inputControl() {
        char c;
        if (scanf(" %c",&c)){
            if (c =='w' ||c== 'a' ||c =='s'||c=='d' )
                direction = c;
            else if (c == 'x')
                gameOver = 1;
        }
    }
    
    void gameLogic() {
        int prevX = x, prevY = y;
        if (direction == 'w') y--;
        else if (direction == 's') y++;
        else if (direction == 'a') x--;
        else if (direction == 'd') x++;
    
        if (x<0 ||x >= ROWS|| y<0 ||y >=COLS) gameOver = 1;
        if (isSnakeHere(x, y)) gameOver = 1;
    
        addTail(prevX, prevY);
    
        if (x == fruitX && y == fruitY) {
            Your_score += 10;
            fruitX = rand() % ROWS;
            fruitY = rand() % COLS;
        } else {
            removeLast();
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
