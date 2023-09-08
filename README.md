using System;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

class ArrayGenerator
{
    static void Main()
    {
        Console.WriteLine("Генератор, сортировщик и сохранитель массивов");
        Console.WriteLine("1. Сгенерировать новый массив");
        Console.WriteLine("2. Загрузить массив из файла");
        int choice = GetMenuChoice(1, 2);

        int[] array;

        if (choice == 1)
        {
            Console.WriteLine("Введите размер массива:");
            int size = GetPositiveInt();

            Console.WriteLine("Введите минимальное значение:");
            int minValue = GetInt();

            Console.WriteLine("Введите максимальное значение:");
            int maxValue = GetInt();

            array = GenerateIntArray(size, minValue, maxValue);

            Console.WriteLine("Сгенерированный массив перед сортировкой:");
            PrintArray(array);
        }
        else
        {
            array = LoadArrayFromFile("array.dat");
            Console.WriteLine("Загруженный массив из файла:");
            PrintArray(array);
        }

        Console.WriteLine("1. Сортировать массив");
        Console.WriteLine("2. Сохранить массив в файл");
        int actionChoice = GetMenuChoice(1, 2);

        if (actionChoice == 1)
        {
            BubbleSort(array);
            Console.WriteLine("Массив после сортировки:");
            PrintArray(array);
        }
        else
        {
            SaveArrayToFile(array, "array.dat");
            Console.WriteLine("Массив сохранен в файл 'array.dat'.");
        }
    }

    static int[] GenerateIntArray(int size, int minValue, int maxValue)
    {
        int[] array = new int[size];
        Random random = new Random();

        for (int i = 0; i < size; i++)
        {
            array[i] = random.Next(minValue, maxValue + 1);
        }

        return array;
    }

    static void BubbleSort(int[] array)
    {
        int n = array.Length;
        bool swapped;
        do
        {
            swapped = false;
            for (int i = 1; i < n; i++)
            {
                if (array[i - 1] > array[i])
                {
                    int temp = array[i - 1];
                    array[i - 1] = array[i];
                    array[i] = temp;
                    swapped = true;
                }
            }
        } while (swapped);
    }

    static void PrintArray(int[] array)
    {
        foreach (int item in array)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }

    static int GetPositiveInt()
    {
        int number;
        while (true)
        {
            if (int.TryParse(Console.ReadLine(), out number) && number > 0)
                return number;
            Console.WriteLine("Пожалуйста, введите положительное целое число.");
        }
    }

    static int GetInt()
    {
        int number;
        while (true)
        {
            if (int.TryParse(Console.ReadLine(), out number))
                return number;
            Console.WriteLine("Пожалуйста, введите целое число.");
        }
    }

    static int GetMenuChoice(int minValue, int maxValue)
    {
        int choice;
        while (true)
        {
            if (int.TryParse(Console.ReadLine(), out choice) && choice >= minValue && choice <= maxValue)
                return choice;
            Console.WriteLine("Пожалуйста, введите правильный выбор.");
        }
    }

    static void SaveArrayToFile(int[] array, string fileName)
    {
        using (FileStream fs = new FileStream(fileName, FileMode.Create))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            formatter.Serialize(fs, array);
        }
    }

    static int[] LoadArrayFromFile(string fileName)
    {
        if (File.Exists(fileName))
        {
            using (FileStream fs = new FileStream(fileName, FileMode.Open))
            {
                BinaryFormatter formatter = new BinaryFormatter();
                return (int[])formatter.Deserialize(fs);
            }
        }
        else
        {
            Console.WriteLine($"Файл '{fileName}' не найден. Возвращен массив из нулей.");
            return new int[0];
        }
    }
}
