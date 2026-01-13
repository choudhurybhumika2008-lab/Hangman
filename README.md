#include <iostream>
#include <string>
#include <conio.h>
#include <windows.h>
#include <ctime>
#include <cctype>

using namespace std;

HANDLE hConsole = GetStdHandle(STD_OUTPUT_HANDLE);

// ---------------- HANGMAN DRAWING ----------------
void drawHangman(int stage) {
    SetConsoleTextAttribute(hConsole, 14);
    cout << "\n\n\n";
    cout << "          ___\n";
    cout << "          |       |\n";
    cout << "          |       " << (stage >= 1 ? 'O' : ' ') << "\n";
    cout << "          |      "
         << (stage >= 3 ? '/' : ' ')
         << (stage >= 2 ? '|' : ' ')
         << (stage >= 4 ? '\\' : ' ') << "\n";
    cout << "          |       " << (stage >= 2 ? '|' : ' ') << "\n";
    cout << "          |      "
         << (stage >= 5 ? '/' : ' ')
         << " "
         << (stage >= 6 ? '\\' : ' ') << "\n";
    cout << "          |\n";
    cout << "        -----\n";
}

// ---------------- MAIN ----------------
int main() {
    srand(time(0));

    string words[] = {
        "COMPUTER",
        "HANGMAN",
        "PROGRAM",
        "LANGUAGE",
        "KEYBOARD"
    };

    int wordCount = sizeof(words) / sizeof(words[0]);
    string word = words[rand() % wordCount];
    string guess(word.length(), '_');
    string usedLetters = "";

    int wrong = 0;
    char ch;
    bool found;

    while (wrong < 6 && guess != word) {
        system("cls");
        drawHangman(wrong);

        SetConsoleTextAttribute(hConsole, 11);
        cout << "\nWord: ";
        for (char c : guess)
            cout << c << " ";

        SetConsoleTextAttribute(hConsole, 10);
        cout << "\n\nWrong attempts left: " << 6 - wrong << endl;

        SetConsoleTextAttribute(hConsole, 15);
        cout << "\nUsed letters: " << usedLetters << endl;
        cout << "Enter a letter: ";
        cin >> ch;
        ch = toupper(ch);

        // Validate alphabet
        if (!isalpha(ch)) {
            cout << "\nEnter alphabet only!";
            getch();
            continue;
        }

        // Check repeated letter
        if (usedLetters.find(ch) != string::npos) {
            cout << "\nYou already tried this letter!";
            getch();
            continue;
        }

        usedLetters += ch;
        found = false;

        for (size_t i = 0; i < word.length(); i++) {
            if (word[i] == ch) {
                guess[i] = ch;
                found = true;
            }
        }

        if (!found) {
            wrong++;
        }
    }

    system("cls");
    drawHangman(wrong);

    if (guess == word) {
        SetConsoleTextAttribute(hConsole, 10);
        cout << "\n🎉 Congratulations! You guessed the word: " << word << endl;
    } else {
        SetConsoleTextAttribute(hConsole, 12);
        cout << "\n☠️ Game Over! The word was: " << word << endl;
    }

    getch();
    return 0;
}
