//# Matrix-

#include <stdio.h>
#include <math.h>
#include <stdlib.h>
#define SIZE 100
//in the name of god 
double determin(double matrix[SIZE][SIZE], int n) {
    double det = 0;
    if (n == 1) {
        return matrix[0][0];
    }
    double temp[SIZE][SIZE];
    int sign = 1;
    for (int f = 0; f < n; f++) {
        int subi = 0;
        for (int i = 1; i < n; i++) {
            int subj = 0;
            for (int j = 0; j < n; j++) {
                if (j == f)
                    continue;
                temp[subi][subj++] = matrix[i][j];
            }
            subi++;
        }
        det += sign * matrix[0][f] * determin(temp, n - 1);
        sign = -sign;
    }
    return det;
}

void adjoint(double matrix[SIZE][SIZE], double adj[SIZE][SIZE], int n) {
    if (n == 1) {
        adj[0][0] = 1;
        return;
    }
    double temp[SIZE][SIZE];
    int sign = 1;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int subi = 0;
            for (int row = 0; row < n; row++) {
                if (row == i)
                    continue;
                int subj = 0;
                for (int col = 0; col < n; col++) {
                    if (col == j)
                        continue;
                    temp[subi][subj++] = matrix[row][col];
                }
                subi++;
            }
            adj[j][i] = (sign * determin(temp, n - 1));
            sign = -sign;
        }
    }
}

int inverse(double matrix[SIZE][SIZE], double inverse[SIZE][SIZE], int n) {
    double det = determin(matrix, n);
    if (det == 0) {
        printf("Error: Determinant is zero , Matrix is not invertible.\n");
        return 0;
    }
    double adj[SIZE][SIZE];
    adjoint(matrix, adj, n);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            inverse[i][j] = adj[i][j] / det;
        }
    }
    return 1;
}

int main() {
    double matrix[SIZE][SIZE], inv[SIZE][SIZE];
    int n;

    printf("Please enter the size of the matrix: ");
    scanf("%d", &n);

    printf("\nPlease enter the elements of the matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("Element [%d][%d]: ", i, j);
            scanf("%lf", &matrix[i][j]);
        }
    }

    if (inverse(matrix, inv, n)) {
        printf("The inverse of the matrix is:\n");
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                printf("%.2lf ", inv[i][j]);
            }
            printf("\n");
        }
    }

    return 0;
}
