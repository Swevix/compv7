# Лабораторная работа 7  
**Тема:** Преобразование и анализ кода с использованием Clang и LLVM.  
- Цель: Познакомиться с инструментами Clang и LLVM, научиться собирать AST и IR-промежуточное представление кода на C/C++, а также извлекать базовую информацию о программе.
---


Тема: Преобразование и анализ кода с использованием Clang и LLVM

Цель: Познакомиться с инструментами Clang и LLVM, научиться собирать AST и IR-промежуточное представление кода на C/C++, а также извлекать базовую информацию о программе.

Содержание
Установка и подготовка среды

Исходный код

Получение AST

Генерация LLVM IR

Оптимизация IR

5.1. Без оптимизаций (-O0)

5.2. С оптимизациями (-O2)

Граф потока управления (CFG)

Выводы

Контрольные вопросы

1. Установка и подготовка среды
Работа выполнялась в Ubuntu 22.04.
Установите Clang, LLVM и Graphviz:

bash
Копировать
Редактировать
sudo apt update
sudo apt install -y clang llvm graphviz
2 Исходный код
Файл main.c:

c
Копировать
Редактировать
#include <stdio.h>

int square(int x) {
    return x * x;
}

int main() {
    int a = 5;
    int b = square(a);
    printf("%d\n", b);
    return 0;
}
3 Получение AST
Чтобы увидеть дерево разбора (AST), используйте:

bash
Копировать
Редактировать
clang -Xclang -ast-dump -fsyntax-only main.c
Вывод покажет структуру функции square и main в виде AST.

4 Генерация LLVM IR
Сгенерировать IR без оптимизаций:

bash
Копировать
Редактировать
clang -S -emit-llvm main.c -o main.ll
5 Оптимизация IR
5.1 Без оптимизаций (-O0)
bash
Копировать
Редактировать
clang -O0 -S -emit-llvm main.c -o main_O0.ll
Все локальные переменные хранятся через alloca.

Множество операций load/store.

Функция square остаётся отдельной.

5.2 С оптимизациями (-O2)
bash
Копировать
Редактировать
clang -O2 -S -emit-llvm main.c -o main_O2.ll
Основные пассы, применённые в -O2:

mem2reg – перевод переменных из памяти в регистры (SSA)

inline – встраивание небольших функций (square)

constprop – подстановка констант (square(5) → 25)

instcombine, simplifycfg, reassociate, gvn, sroa, dce и др.

Результат:

square инлайнится и исчезает.

Убираются все alloca, store, load.

В main_O2.ll остаётся только один вызов printf(25).

bash
Копировать
Редактировать
diff main_O0.ll main_O2.ll
6 Граф потока управления (CFG)
Сгенерировать IR:

bash
Копировать
Редактировать
clang -O2 -S -emit-llvm main.c -o main.ll
Построить CFG:

bash
Копировать
Редактировать
opt -dot-cfg -disable-output main.ll
Получатся файлы cfg_main.dot, (cfg_square.dot).

Конвертировать в PNG:

bash
Копировать
Редактировать
dot -Tpng cfg_main.dot  -o cfg_main.png
dot -Tpng cfg_square.dot -o cfg_square.png
Открыть:

bash
Копировать
Редактировать
xdg-open cfg_main.png
xdg-open cfg_square.png
7 Выводы
Clang + LLVM дают полный контроль над AST, IR и CFG.

IR-пасс — мощный инструмент для оптимизаций и анализа.

Ваши собственные трансформации проще реализовать на LLVM-IR.
