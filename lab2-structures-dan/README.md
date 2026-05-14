# Лабораторная работа №2

## Тема
Перемножение квадратных матриц размера 2048x2048 с элементами типа `double`.

---

## Цель работы
Изучение производительности различных алгоритмов умножения матриц и сравнение их эффективности.

---

## Задание

1. Перемножить две квадратные матрицы размера `2048x2048`.
2. Исходные матрицы генерируются случайным образом.
3. Оценить сложность алгоритма:

\[
C = 2n^3
\]

где:
- `n` — размерность матрицы.

4. Оценить производительность в MFLOPS:

\[
P = \frac{C}{t \cdot 10^6}
\]

где:
- `t` — время выполнения алгоритма.

5. Реализовать 3 варианта перемножения:
- классический алгоритм;
- BLAS (`cblas_dgemm`);
- оптимизированный алгоритм.

---

# Используемые технологии

- C++
- OpenBLAS / Intel MKL
- g++

---

# Код программы

## main.cpp

```cpp
#include <iostream>

// 3. Оптимизированное умножение
void optimizedMultiply(const vector<double>& A,
                       const vector<double>& B,
                       vector<double>& C)
{
    for (int i = 0; i < N; i++)
    {
        for (int k = 0; k < N; k++)
        {
            double temp = A[i * N + k];

            for (int j = 0; j < N; j++)
            {
                C[i * N + j] += temp * B[k * N + j];
            }
        }
    }
}

void benchmark(void (*func)(const vector<double>&,
                            const vector<double>&,
                            vector<double>&),
               const vector<double>& A,
               const vector<double>& B,
               vector<double>& C,
               const string& name)
{
    auto start = high_resolution_clock::now();

    func(A, B, C);

    auto end = high_resolution_clock::now();

    double time = duration<double>(end - start).count();

    double complexity = 2.0 * N * N * N;
    double mflops = complexity / (time * 1e6);

    cout << fixed << setprecision(3);
    cout << "\n" << name << endl;
    cout << "Time: " << time << " sec" << endl;
    cout << "MFLOPS: " << mflops << endl;
}

int main()
{
    vector<double> A(N * N);
    vector<double> B(N * N);
    vector<double> C(N * N, 0.0);

    generateMatrix(A);
    generateMatrix(B);

    benchmark(classicMultiply, A, B, C, "Classic Multiply");

    fill(C.begin(), C.end(), 0.0);
    benchmark(blasMultiply, A, B, C, "BLAS Multiply");

    fill(C.begin(), C.end(), 0.0);
    benchmark(optimizedMultiply, A, B, C, "Optimized Multiply");

    return 0;
}

---

```cpp
return 0;
}
```

## Результат выполнения программы

<img width="887" height="441" alt="image" src="https://github.com/user-attachments/assets/29fb06df-76ee-4c1d-8147-c1063ed852e6" />



