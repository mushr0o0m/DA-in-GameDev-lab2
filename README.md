# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Филоник Кирилл Русланович
- РИ210931

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Написать программы Hello World на Python и Unity. 

#### Python
- Скриншот с демонстрацией сохранения документа google.colab

![Скриншот с демонстрацией сохранения документа google.colab](https://github.com/mushr0o0m/DA-in-GameDev-lab1-description-and-attachments/blob/main/img/Task-1/1.2-py.png)

- Скриншот с запуском программы

![Скриншот с запуском программы](https://github.com/mushr0o0m/DA-in-GameDev-lab1-description-and-attachments/blob/main/img/Task-1/1.1-py.png)

#### Unity
- Скриншот с запуском программы

![Скриншот с запуском программы](https://github.com/mushr0o0m/DA-in-GameDev-lab1-description-and-attachments/blob/main/img/Task-1/1.1-unity.png)

## Задание 2
### В разделе «ход работы» пошагово выполнить каждый пункт с описанием и примером реализации задачи по теме лабораторной работы.

1.Произвести подготовку данных для работы с алгоритмом линейной регрессии. 10 видов данных были установлены случайным образом, и данные находились в линейной зависимости. Данные преобразуются в формат массива, чтобы их можно было вычислить напрямую при использовании умножения и сложения.

```py

In [ ]:
#Import the required modules, numpy for calculation, and Matplotlib for drawing
import numpy as np
import matplotlib.pyplot as plt
#This code is for jupyter Notebook only
%matplotlib inline

# define data, and change list to array
x = [3,21,22,34,54,34,55,67,89,99]
x = np.array(x)
y = [2,22,24,65,79,82,55,130,150,199]
y = np.array(y)

#Show the effect of a scatter plot
plt.scatter(x,y)

```

2.Определите связанные функции. Функция модели: определяет модель линейной регрессии wx+b. Функция потерь: функция потерь среднеквадратичной ошибки. Функция оптимизации: метод градиентного спуска для нахождения частных производных w и b.

```py

In [ ]:
#The basic linear regression model is wx+ b, and since this is a two-dimensional space, the model is ax+ b

def model(a, b, x):
    return a*x + b

#Tahe most commonly used loss function of linear regression model is the loss function of mean variance difference
def loss_function(a, b, x, y):
    num = len(x)
    prediction=model(a,b,x)
    return (0.5/num) * (np.square(prediction-y)).sum()

#The optimization function mainly USES partial derivatives to update two parameters a and b
def optimize(a,b,x,y):
    num = len(x)
    prediction = model(a,b,x)
    #Update the values of A and B by finding the partial derivatives of the loss function on a and b
    da = (1.0/num) * ((prediction -y)*x).sum()
    db = (1.0/num) * ((prediction -y).sum())
    a = a - Lr*da
    b = b - Lr*db
    return a, b

#iterated function, return a and b
def iterate(a,b,x,y,times):
    for i in range(times):
        a,b = optimize(a,b,x,y)
    return a,b
    
```

3.Начать итерацию
Шаг 1 Инициализация и модель итеративной оптимизации
```py

In [ ]:
#Initialize parameters and display
a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001

#For the first iteration, the parameter values, losses, and visualization after the iteration are displayed
a,b = iterate(a,b,x,y,1)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)

```



Шаг 2 На второй итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации

```py

In [ ]:
a,b = iterate(a,b,x,y,2)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)

```

Шаг 3 Третья итерация показывает значения параметров, значения потерь и визуализацию после итерации

```py

In [ ]:
a,b = iterate(a,b,x,y,3)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)

```

Шаг 4 На четвертой итерации отображаются значения параметров, значения потерь и эффекты визуализации

```py

In [ ]:
a,b = iterate(a,b,x,y,4)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)

```

Шаг 5 Пятая итерация показывает значение параметра, значение потерь и эффект визуализации после итерации

```py

In [ ]:
a,b = iterate(a,b,x,y,5)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)

```

Шаг 6 10000-я итерация, показывающая значения параметров, потери и визуализацию после итерации

```py

In [ ]:
a,b = iterate(a,b,x,y,10000)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)

```

## Задание 3
### Изучить код на Python и ответить на вопросы:
- Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.
Да, так как с каждой следующе итерацией величина loss уменьшается, что видно на следующем скриншате:

![](https://github.com/mushr0o0m/DA-in-GameDev-lab1-description-and-attachments/blob/main/img/Task-3/3.1.png)

- Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра.
Lr отвечает за точность модели, так как при изменении этого параметра меняется значение функции потери, что представляют собой меру ошибок, которая делает модель 
на наборе данных

Значения выводимые при Lr * 10^2:

![](https://github.com/mushr0o0m/DA-in-GameDev-lab1-description-and-attachments/blob/main/img/Task-3/3.3-x10%5E2.png)

Значения выводимые при Lr * 10^-2:

![](https://github.com/mushr0o0m/DA-in-GameDev-lab1-description-and-attachments/blob/main/img/Task-3/3.3-x10%5E-2.png)

## Выводы

В первую очередь, я получил опыт работы с github, также попрактиковался в чтении и изучении кода на python, на платформе google.colab. При ответе на второй вопрос задания 3 я более подробно изучил линейную регрессию и функцию потерь методом наименьших квадратов.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
