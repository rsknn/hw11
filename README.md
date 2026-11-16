```c
#define _CRT_SECURE_NO_DEPRECATE
#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <windows.h>

typedef struct {
    int number;
    char start_stop[40];
    char end_stop[40];
    double length;
    int stops;
} BusRoute;

BusRoute input_route(void);
void print_route(BusRoute r);
int check(BusRoute r, double limit);
int writefile(char* fname, BusRoute r1, BusRoute r2, BusRoute r3);

int main() {
    setlocale(LC_CTYPE, "RUS");
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);
    BusRoute r1, r2, r3;
    double limit;
    int found = 0;

    printf("Ввод данных о трёх автобусных маршрутах\n");
    printf("Маршрут 1\n");
    r1 = input_route();
    printf("\nМаршрут 2\n");
    r2 = input_route();
    printf("\nМаршрут 3\n");
    r3 = input_route();

    double avg1 = r1.length / (r1.stops - 1);
    double avg2 = r2.length / (r2.stops - 1);
    double avg3 = r3.length / (r3.stops - 1);

    printf("\nВведённые маршруты и вычисленное среднее расстояние\n");
    print_route(r1);
    printf("Среднее расстояние между остановками: %.2lf км\n", avg1);
    print_route(r2);
    printf("Среднее расстояние между остановками: %.2lf км\n", avg2);
    print_route(r3);
    printf("Среднее расстояние между остановками: %.2lf км\n", avg3);
    printf("\nВведите заданное значение расстояния для отбора (км): ");
    scanf("%lf", &limit);
    printf("\nМаршруты со средним расстоянием меньше %.2lf км\n", limit);
    if (check(r1, limit) == 1) {
        print_route(r1);
        found++;
    }
    if (check(r2, limit) == 1) {
        print_route(r2);
        found++;
    }
    if (check(r3, limit) == 1) {
        print_route(r3);
        found++;
    }
    if (found == 0) {
        printf("Подходящих маршрутов не найдено.\n");
    }
    if (writefile("routes.txt", r1, r2, r3) == 1) {
        printf("\nЗапись в файл routes.txt выполнена успешно.\n");
    }
    return 0;
}

BusRoute input_route() {
    BusRoute r;
    printf("Номер маршрута: ");
    scanf("%d", &r.number);
    printf("Начальная остановка: ");
    scanf("%39s", r.start_stop);
    printf("Конечная остановка: ");
    scanf("%39s", r.end_stop);
    printf("Длина маршрута (км): ");
    scanf("%lf", &r.length);
    printf("Количество остановок (не менее 2): ");
    scanf("%d", &r.stops);
    return r;
}

void print_route(BusRoute r) {
    printf("Маршрут №%d | %s -> %s | Длина: %.2lf км | Остановок: %d\n", r.number, r.start_stop, r.end_stop, r.length, r.stops);
}

int check(BusRoute r, double limit) {
    double avg = r.length / (r.stops - 1);
    if (avg < limit) {
        return 1;
    }
    else {
        return 0;
    }
}

int writefile(char* fname, BusRoute r1, BusRoute r2, BusRoute r3) {
    FILE* out;
    if ((out = fopen(fname, "wt")) == NULL) {
        printf("Ошибка открытия файла для записи\n");
        return 0;
    }
    fprintf(out, "Маршрут:%d Начало:%s Конец:%s Длина:%.2lf Остановок:%d Среднее:%.2lf\n", r1.number, r1.start_stop, r1.end_stop, r1.length, r1.stops, r1.length / (r1.stops - 1));
    fprintf(out, "Маршрут:%d Начало:%s Конец:%s Длина:%.2lf Остановок:%d Среднее:%.2lf\n", r2.number, r2.start_stop, r2.end_stop, r2.length, r2.stops, r2.length / (r2.stops - 1));
    fprintf(out, "Маршрут:%d Начало:%s Конец:%s Длина:%.2lf Остановок:%d Среднее:%.2lf\n", r3.number, r3.start_stop, r3.end_stop, r3.length, r3.stops, r3.length / (r3.stops - 1));
    fclose(out);
    return 1;
}
```
