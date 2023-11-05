# SPARSE-MATRIX-ADDITION
/* Abna Basheer
S3 CSE A
Roll no :11*/
#include <stdio.h>

// Structure to represent a sparse matrix element
struct SparseMatrixElement {
    int row;
    int col;
    int value;
};

// Function to add two sparse matrices
void addSparseMatrices(
    struct SparseMatrixElement matrix1[],
    int numElements1,
    struct SparseMatrixElement matrix2[],
    int numElements2
) {
    // Assuming the matrices have the same dimensions
    int numRows, numCols;
    
    if (numElements1 > 0) {
        numRows = matrix1[0].row + 1;
        numCols = matrix1[0].col + 1;
    } else if (numElements2 > 0) {
        numRows = matrix2[0].row + 1;
        numCols = matrix2[0].col + 1;
    } else {
        printf("Both matrices are empty!\n");
        return;
    }
    
    struct SparseMatrixElement result[numElements1 + numElements2];
    int resultIndex = 0;
    
    int index1 = 1;
    int index2 = 1;

    while (index1 <= numElements1 && index2 <= numElements2) {
        if (matrix1[index1].row < matrix2[index2].row) {
            result[resultIndex] = matrix1[index1];
            index1++;
        } else if (matrix1[index1].row > matrix2[index2].row) {
            result[resultIndex] = matrix2[index2];
            index2++;
        } else {
            if (matrix1[index1].col < matrix2[index2].col) {
                result[resultIndex] = matrix1[index1];
                index1++;
            } else if (matrix1[index1].col > matrix2[index2].col) {
                result[resultIndex] = matrix2[index2];
                index2++;
            } else {
                // Rows and columns match, add the values
                result[resultIndex] = matrix1[index1];
                result[resultIndex].value += matrix2[index2].value;
                index1++;
                index2++;
            }
        }
        resultIndex++;
    }

    // Copy any remaining elements from the first matrix
    while (index1 <= numElements1) {
        result[resultIndex] = matrix1[index1];
        index1++;
        resultIndex++;
    }

    // Copy any remaining elements from the second matrix
    while (index2 <= numElements2) {
        result[resultIndex] = matrix2[index2];
        index2++;
        resultIndex++;
    }

    // Display the result matrix
    printf("Result Matrix:\n");
    printf("Row\tColumn\tValue\n");
    printf("----\t------\t-----\n");
    for (int i = 0; i < resultIndex; i++) {
        printf("%d\t%d\t%d\n", result[i].row, result[i].col, result[i].value);
    }
}

int main() {
    struct SparseMatrixElement matrix1[] = {
        {3, 4, 6},
        {4, 2, 8}
    };
    int numElements1 = sizeof(matrix1) / sizeof(matrix1[0]);

    struct SparseMatrixElement matrix2[] = {
        {3, 4, 5},
        {4, 2, 2},
        {1, 1, 3}
    };
    int numElements2 = sizeof(matrix2) / sizeof(matrix2[0]);

    addSparseMatrices(matrix1, numElements1, matrix2, numElements2);

    return 0;
}
