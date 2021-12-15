---
title: Лабораторная работа №11
summary: ИС
authors:
    - Волкова Т.В.
    - Владимирова Е.С.
    - Ченгарь О.В.
---
### Название работы
Разработка класса-шаблона для создания объектов.

### Пример реализации

Файл `Box.java`:
```java
/**
 * Класс, обозначающий коробку
 */
public class Box {
    // статическая неизменяемая переменная класса задает формат
    // строки описания объекта (общая для всех объектов класса)
    private final static String BOX_FORMAT_STRING =
            "Box: длина: %.2f, ширина: %.2f, высота: %.2f";

    /**
     * Ширина коробки (поле)
     */
    private double width;

    /**
     * Высота коробки (поле)
     */
    private double height;

    /**
     * Глубина коробки (поле)
     */
    private double depth;


    /**
     * Метод-конструктор, выполняет инициализацию класса
     * @param width Ширина коробки
     * @param height Высота коробки
     * @param depth Глубина коробки
     */
    public Box(double width, double height, double depth) {
        this.depth = depth;
        this.width = width;
        this.height = height;
    }

    /**
     * Копирующий конструктор
     * Обратите внимание, что мы можем использовать
     * наш класс как тип данных
     * @param anotherBox
     */
    public Box(Box anotherBox) {
        // Box это ссылочный тип, проверяем, что нам передали
        if(anotherBox == null) {
            // Выбрасываем ошибку если втирают какую-то дичь
            throw new IllegalArgumentException("anotherBox");
        }

        this.width = anotherBox.getWidth();
        this.height = anotherBox.getHeight();
        this.depth = anotherBox.getDepth();
    }

    /**
     * Метод-геттер для получения
     * значения глубины коробки
     * @return глубина коробки
     */
    public double getDepth() {
        return depth;
    }

    /**
     * Метод-геттер для получения
     * значения ширины коробки
     * @return ширина коробки
     */
    public double getWidth() {
        return width;
    }

    /**
     * Метод-геттер для получения
     * значения высоты коробки
     * @return высота коробки
     */
    public double getHeight() {
        return height;
    }

    /**
     * Метод для вычисления объема коробки
     * @return объем коробки
     */
    public double getVolume() {
        return depth * width * height;
    }

    /**
     * Сеттер для изменения глубины коробки
     * @param depth глубина коробки
     */
    public void setDepth(double depth) {
        if (depth < 0) {
            throw new IllegalArgumentException("depth");
        }

        this.depth = depth;
    }

    /**
     * Сеттер для изменения ширины коробки
     * @param width ширина коробки
     */
    public void setWidth(double width) {
        if (width < 0) {
            throw new IllegalArgumentException("width");
        }

        this.width = width;
    }

    /**
     * Сеттер для изменения высоты коробки
     * @param height высоты коробки
     */
    public void setHeight(double height) {
        if (height < 0) {
            throw new IllegalArgumentException("height");
        }

        this.height = height;
    }

    /**
     * Реализация метода toString
     * @return представление объекта в виде строки
     */
    @Override
    public String toString() {
        return String.format(BOX_FORMAT_STRING, width, height, depth);
    }

    /**
     * Реализация метода equals
     * Он предназначен для проверки двух объектов на равенство
     * Мы не можем использовать == для проверки равенства объектов,
     * так как == сравнивает равенство ссылок
     * Для реализация проверки равенства по значению нужен метод equals
     * Но так как логика сравнения разных объектов зависит от того,
     * Что это за объект и какой класс, в каждом классе нужно реализовать этот метод.
     * Если вы хотите сравнивать объекты, само собой.
     * Если вам это не надо - то и не реализовывайте, будто у вас без этого проблем мало.
     * @param anotherBox другой объект класса Box
     * @return true, если объекты равны, иначе false
     */
    public boolean equals(Box anotherBox) {
        // Если ссылки равны (== проверяет равенство ссылок)
        // То anotherObject это этот же самый объект
        if (this == anotherBox) return true;

        // Если передали null-ссылку, то мы точно ей не равны
        // А если вдруг равны - то вполне вероятно, что у вас проблемы
        if (anotherBox == null) return false;

        // А здесь мы сравниваем примитивные типы, можем юзать ==
        // Коробки равны, если все стороны соответственно равны.
        return this.depth == anotherBox.getDepth()
                && this.height == anotherBox.getHeight()
                && this.width == anotherBox.getWidth();
    }
}
```

Файл `Main.java`:
```java
public class Main {
    public static void main(String[] args) {
        // Создаем объект
        Box firstBox = new Box(3, 1, 5);

        // Создаем вторую ссылку на первую коробку
        Box alsoFirstBox = firstBox;
        // firstBox и alsoFirstBox ссылаются на один объект

        // создание копии объекта
        // согласно заданию на ЛР использовать надо
        // конструктор без параметров
        // Но гораздо лучше использовать копирующий конструктор
        Box secondBox = new Box (alsoFirstBox);

        // cоздание объекта с другими значениями полей
        Box completelyAnotherBox = new Box(4, 5, 7);

        //Создание null-ссылки
        Box nullBox = null;

        //Вывод информации об объектах
        System.out.println("firstBox=" + firstBox);
        System.out.println("alsoFirstBox=" + alsoFirstBox);
        System.out.println("secondBox=" + secondBox);
        System.out.println("completelyAnotherBox=" + completelyAnotherBox);
        System.out.println("nullBox=" + nullBox);

        //Вывод результатов сравнения объектов
        System.out.println("firstBox==alsoFirstBox: " + firstBox.equals(alsoFirstBox));
        System.out.println("firstBox==secondBox: " + firstBox.equals(secondBox));
        System.out.println("firstBox==completelyAnotherBox: " + firstBox.equals(completelyAnotherBox));
        System.out.println("alsoFirstBox==secondBox: " + alsoFirstBox.equals(secondBox));
        System.out.println("alsoFirstBox==completelyAnotherBox: " + alsoFirstBox.equals(completelyAnotherBox));
        System.out.println("secondBox==completelyAnotherBox: " + secondBox.equals(completelyAnotherBox));
        System.out.println("secondBox==nullBox: " + secondBox.equals(nullBox));

    }
}
```





