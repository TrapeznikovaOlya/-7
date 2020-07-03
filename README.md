# -7#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#define N 5
int Me(int A[][N])
    {
        int i,j;
        for (j=0;j<N;j++)
            for (i=0;i<N;i++)
                scanf("%d" , &A[i][j]);
    }
int file(int A[][N])
    {
        int i, j; FILE* f;
        f = fopen("in.txt", "r");
        if (f != NULL)
            for (j = 0; j<N; j++)
                for (i = 0; i<N; i++)
                    fscanf(f, "%d", &A[i][j]);
        else printf("Входной файл отсутствует");
        fclose(f);
    }
int rec(int Y,int X[], int stepi)
    {
        if (stepi< 4)
            if (X[stepi] % 2 != 0)
            {
                Y*= X[stepi] ; rec(Y,X, stepi + 1);
            }
            else return Y;
        else return Y;
    }
int output(int A[][N], int X[])
    {
        FILE* f; int Y =0;
        for (int j = 0; j<N; j++)
            {
                for (int i = 0; i< N; i++)
                printf("%3d", A[i][j]);
                printf("\n");
            }
            for (int i = 0; i< N; i++)
            printf("\nx[%d]=%d", i+1, X[i]);
            printf("\nY = %d", rec(1,X, 0));
            f = fopen("out.txt", "w");
            if (f != NULL)
                {
                    for (int i = 0; i< N; i++)
                        {
                            for (int j = 0; j< N; j++)
                                fprintf(f, "%3d", A[j][i]);
                            fprintf(f, "\n");
                        }
                    for (int i = 0; i < N; i++)
                        fprintf(f, "\nx[%d]=%d", i+1, X[i]);
                    fprintf(f, "\nY = %d", rec(1,X, 0));
                }
            else printf("Ошибка открытия файла");
    }
int *MainCalculation(int A[][5],int (*yka)(int[][N] ),int X[])
    {
        int j,i,t; (*yka)(A);
        for(i=0;i<N;i++)
            {
                for(j=0;j<N-2;j++)
                    if(A[i][j]<A[i][j+1] && A[i][j+1]<A[i][j+2])
                        {
                            X[i]=A[i][j]; break;
                        }
                    else    X[i]=0;
            }
        return X;
    }
int main()
    {
        char* locale = setlocale(LC_ALL,"");
        int A[N][N], X[N], k; int *Sp;
        int (*yka)(int[][N]) = NULL;
        printf("Укажите какой ввод матрицы\nC клавиатуры - введите 1\nC файла - 0\n");    
        printf("Ваш выбор - "); scanf("%d", &k);
        if (k == 1) yka = Me;
