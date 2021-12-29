﻿Домашнее задание для Netology.ru для курса Java Developer   

Описание и инструкция к выполнению [здесь](https://github.com/netology-code/jd-homeworks/tree/master/jvm/README.md)

# Задача "Понимание JVM"

## Пояснение кода
```java

public class JvmComprehension {             // JVM отправляет подсистеме загрузчиков классов команду загрузить класс
                                            // JvmComprehension. Application ClassLoader делигирует загрузку Platform ClassLoader,
                                            // а он уже делигирует Bootstrap ClassLoader, если он не нашел, то ищет Platform ClassLoader,
                                            // Если и он не находит, то ищет Application ClassLoader
                                            // Далее класс (его код, заголовок и проее) грузится в метаспейс,
                                            // после чего начинается выполнение кода

    public static void main(String[] args) {// Создается новый фрейм в стеке для ф-ции main, в который сохраняются
                                            // ссылки на аргументы метода
        int i = 1;                      // 1. В стеке создается объект - число 1
        Object o = new Object();        // 2. В стеке создается переменная-ссылка на объект (о),
                                        // а сам новый созданный объект (new Object()) помещается в кучу (heap)
        Integer ii = 2;                 // 3. То же самое, т.к. Integer - ссылочный тип, то объект-ссылка создается
                                        // в стеке, а в куче создается объект-обертка числа 2
        printAll(o, i, ii);             // 4. В стеке создается новый фрейм для ф-ции printAll, в который помещаются
                                        // ссылки на переданные ей параметры, а также ссылки на переменные,
                                        // создаваемые в самом методе
                                        // После выполнения этой ф-ции сборщик мусора удаляет объекты, на которые мы ссылались
                                        // через ссылки o, i и ii. И стирается фрейм, связанный с этой ф-цией
        System.out.println("finished"); // 7. В стеке создается новый фрейм для ф-ции println, в который помещается ссылка
                                        // на объект-строку "finished", который был размещен в куче. После отработки метода
                                        // стирается фрейм, связанный с этой ф-цией
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5. Ссылка на переменную помещается во фрейм вызванной ф-ции printAll
                                                    // в стеке. Сам объект-обертка числа 700 помещается в куче

        System.out.println(o.toString() + i + ii);  // 6. В стеке создается новый фрейм для ф-ции println. Далее вычисляется
                                                    // значение параметра, значение которого будет передано методу. Для
                                                    // этого вызывается метод toString() у объекта по ссылке o, что приводит
                                                    // к тому, что в стеке создается новый фрейм для этой ф-ции. После вычисления
                                                    // значения ф-ции toString возвращаемое значение конкатенируется со
                                                    // значениями объектов из кучи, на которые указывают ссылки i и ii
                                                    // По окончании работы метода сборщик мусора удаляет объект - строковое
                                                    // представление объекта o, т.к. этот объект-строка больше не используется.
                                                    // То же самое происходит и объектом-оберткой числа 700. После
                                                    // отработки метода println стирается фрейм, связанный с этой ф-цией
    }
}

```