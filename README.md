# Лабораторная работа 7  
**Тема:** Преобразование и анализ кода с использованием Clang и LLVM.  
- Цель: Познакомиться с инструментами Clang и LLVM, научиться собирать AST и IR-промежуточное представление кода на C/C++, а также извлекать базовую информацию о программе.
---



1. Работа выполнялась в среде Ubuntu 22.04. Установлены следующие 
инструменты: 
● clang — компилятор языка C/C++; 
● llvm — инструменты анализа и оптимизации кода; 
● opt — инструмент для работы с LLVM IR и применения 
оптимизаций; 
● Graphviz — инструмент для визуализации кода. 
Команда установки: sudo apt install clang llvm

![image](https://github.com/user-attachments/assets/d59abf23-1eb9-4d65-b9fa-64e8474e5aad)

---
2. Исходный код 
Программа на языке C: 
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
Сохранена в файл main.c.
![image](https://github.com/user-attachments/assets/95ce485b-7981-40e7-ab27-1532defdfffc)
---
3. Получение AST 
Команда: clang -Xclang -ast-dump -fsyntax-only main.c
![image](https://github.com/user-attachments/assets/97f2cb7b-9656-4dc0-996e-6c3dbae10e08)
Функция square принята, содержит параметр x и возвращает x * x.
---
4. Генерация LLVM IR 
Команда: clang -S -emit-llvm main.c -o main.ll


