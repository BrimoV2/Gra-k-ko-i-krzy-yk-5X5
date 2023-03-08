# Gra kolko i krzyzyk 5X5 w jezyku C

#include <stdio.h>
#include <stdlib.h>

#define FILENAME "gra.txt"

const int points[6] = {0, 0, 1, 3, 7, 15};
const int directions[4][2] = {{0, 1}, {1, 0}, {1, 1}, {1, -1}};

int check_row(char t[5][5], int i, int j, const int dir[2], char player) {

    int total_points = 0;
    int in_row = 0;

    while (i >= 0 && j >= 0 && i < 5 && j < 5) {

        if (t[i][j] == player) {
            in_row++;
        } else {
            total_points += points[in_row];
            in_row = 0;
        }

        i += dir[0];
        j += dir[1];
    }

    return total_points + points[in_row];
}

int count_player(char t[5][5], char player) {

    int total_points = 0;

    for (int j = 0; j < 5; j++) {
        total_points += check_row(t, 0, j, directions[1], player);
        total_points += check_row(t, 0, j, directions[2], player);
        total_points += check_row(t, 0, j, directions[3], player);
    }

    for (int i = 0; i < 5; i++) {
        total_points += check_row(t, i, 0, directions[0], player);

        if (i != 0) {
            total_points += check_row(t, i, 0, directions[2], player);
            total_points += check_row(t, i, 4, directions[3], player);
        }
    }

    return total_points;
}

int main() {

    FILE *file = fopen(FILENAME, "r");

    if (file == NULL) {
        printf("Nie mozna otworzyc");
        return 1;
    }

    char t[5][5];

    char c;
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            c = 0;
            while (c != 'o' && c != 'x')
                fscanf(file, "%c", &c);
            t[i][j] = c;
        }
    }

    fclose(file);

    int kacper = count_player(t, 'x');
    int olek = count_player(t, 'o');

    printf("Punkty Kacpra: %d\n", kacper);
    printf("Punkty Olka:   %d\n", olek);

    if (kacper == olek)
        printf("remis\n");
    else if (kacper > olek)
        printf("wygral Kacper\n");
    else
        printf("wygral Olek\n");

    return 0;
}
