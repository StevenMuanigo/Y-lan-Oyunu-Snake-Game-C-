Yılan oyunu, klasik bir konsol tabanlı yılan oyununun C++ ile yapılmış sürümüdür Kullanıcı yılanı yönlendirerek meyve toplar ve her meyve yediğinde yılanın boyu uzar Amaç yılanın kendisine çarpmadan olabildiğince uzun süre hayatta kalmaktır

Özellikler

Yılanın başı "O", kuyruğu ise "o" ile temsil edilir

Ekranın etrafında duvarlar bulunur

Yılan, yediği meyveyle büyür

Konsolda dinamik olarak güncellenen oyun ekranı

Oyunu bitirmek için "x" tuşuna basılabilir

Oyun boyunca kullanıcı skoru takip edebilir

Kurulum ve Çalıştırma

Windows Kullanıcıları:

Visual Studio veya başka bir C++ derleyicisiyle projeyi açın.

Derleyin ve çalıştırın.

Linux/macOS Kullanıcıları:

g++ -o snake main.cpp komutuyla projeyi derleyin.

./snake komutuyla oyunu başlatın.

cpp
#include <iostream>
#include <conio.h> // For Windows, use <ncurses.h> for Linux/macOS
#include <windows.h> // For Sleep function

#define WIDTH 20
#define HEIGHT 20

using namespace std;

int x, y, fruitX, fruitY, score;
int tailX[100], tailY[100];
int nTail;
enum eDirection { STOP = 0, LEFT, RIGHT, UP, DOWN };
eDirection dir;

bool gameOver;

void Setup() {
    gameOver = false;
    dir = STOP;
    x = WIDTH / 2;
    y = HEIGHT / 2;
    fruitX = rand() % WIDTH;
    fruitY = rand() % HEIGHT;
    score = 0;
}

void Draw() {
    system("cls"); // system("clear"); for Linux/macOS
    for (int i = 0; i < WIDTH + 2; i++)
        cout << "#";
    cout << endl;

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            if (j == 0)
                cout << "#"; // Wall
            
            if (i == y && j == x)
                cout << "O"; // Snake head
            else if (i == fruitY && j == fruitX)
                cout << "F"; // Fruit
            else {
                bool print = false;
                for (int k = 0; k < nTail; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        cout << "o"; // Snake tail
                        print = true;
                    }
                }
                if (!print)
                    cout << " ";
            }

            if (j == WIDTH - 1)
                cout << "#"; // Wall
        }
        cout << endl;
    }

    for (int i = 0; i < WIDTH + 2; i++)
        cout << "#";
    cout << endl;

    cout << "Score:" << score << endl;
}

void Input() {
    if (_kbhit()) {
        switch (_getch()) {
        case 'a':
            dir = LEFT;
            break;
        case 'd':
            dir = RIGHT;
            break;
        case 'w':
            dir = UP;
            break;
        case 's':
            dir = DOWN;
            break;
        case 'x':
            gameOver = true;
            break;
        }
    }
}

void Logic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }
    switch (dir) {
    case LEFT:
        x--;
        break;
    case RIGHT:
        x++;
        break;
    case UP:
        y--;
        break;
    case DOWN:
        y++;
        break;
    default:
        break;
    }

    if (x >= WIDTH) x = 0; else if (x < 0) x = WIDTH - 1;
    if (y >= HEIGHT) y = 0; else if (y < 0) y = HEIGHT - 1;

    for (int i = 0; i < nTail; i++)
        if (tailX[i] == x && tailY[i] == y)
            gameOver = true;

    if (x == fruitX && y == fruitY) {
        score += 10;
        fruitX = rand() % WIDTH;
        fruitY = rand() % HEIGHT;
        nTail++;
    }
}

int main() {
    Setup();
    while (!gameOver) {
        Draw();
        Input();
        Logic();
        Sleep(100); // sleep(100ms) for Linux/macOS using <unistd.h>
    }
    return 0;
}
