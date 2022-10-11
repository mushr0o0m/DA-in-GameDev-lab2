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
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity. 

- Скриншоты подключения API для работы с google sheets и google drive в console cloud google

![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-1-1.png)
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-1-2.png)

- Скриншоты реализации и запись данных из скрипта на python в google-таблицу

![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-2-1.png)
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-2-2.png)

```py

import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatasciense-365213-b41b7798cdd7.json')
sh = gc.open("UnitySheet")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((price[i-1]-price[i-2])/price[i-2])*100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)

```

- Скриншоты проекта на Unity, который получают данные из google-таблицы, в которую были записаны данные в предыдущем пункте

![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-3-1.png)

- Скриншоты функционала на Unity, в котором будет воспризводиться аудио-файл в зависимости от значения данных из таблицы

![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-4-1.png)
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-4-2.png)
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/1-4-3.png)


```cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class Data : MonoBehaviour
{
    [SerializeField] private AudioClip goodSpeak;
    [SerializeField] private AudioClip normalSpeak;
    [SerializeField] private AudioClip badSpeak;

    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int counter = 1;

    void Start()
    {
        StartCoroutine(GoogleSheet());
    }

    void Update()
    {
        if(dataSet["Mon_" + counter.ToString()] <= 10 && !statusStart && counter != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + counter.ToString()]);
        }

        if (dataSet["Mon_" + counter.ToString()] > 10 && dataSet["Mon_" + counter.ToString()] < 100
            && !statusStart && counter != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + counter.ToString()]);
        }

        if(dataSet["Mon_" + counter.ToString()] >= 100 && !statusStart && counter != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + counter.ToString()]);
        }
    }

    IEnumerator GoogleSheet()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1BunomDF4_9-10cpm3dKWcXucEfIr3qEa6jKYAPDI1qU/values/Лист1?key=AIzaSyAW_ZU0srE6ifkI2uJcReML4CQiDesSfEA");
        yield return curentResp.SendWebRequest();
        var rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach(var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        counter++;
    }

    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        counter++;
    }

    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        counter++;
    }

}


```

## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1.

- Скриншоты результаты работы модели линейной регрессии и отображения их в Google-таблице

![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/2-1-1.png)
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/2-1-2.png)

```py

import numpy as np
import gspread
import matplotlib.pyplot as plt

gc = gspread.service_account(filename='unitydatasciense-365213-b41b7798cdd7.json')
sh = gc.open("UnitySheet")

x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]
x = np.array(x)
y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y = np.array(y)

plt.scatter(x, y)


def model(a, b, x):
    return a * x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5 / num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0 / num) * ((prediction - y) * x).sum()
    db = (1.0 / num) * ((prediction - y).sum())
    a = a - Lr * da
    b = b - Lr * db
    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b


a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
sh.sheet1.update('A1', str(1))
sh.sheet1.update('B1', str(a)[1:-1])
sh.sheet1.update('C1', str(b)[1:-1])
Lr = 0.000001

for i in range(2, 11):
    a, b = iterate(a, b, x, y, i)
    prediction = model(a, b, x)
    loss = loss_function(a, b, x, y)
    print(a, b, loss)
    sh.sheet1.update(('A' + str(i)), str(i))
    sh.sheet1.update(('B' + str(i)), str(a)[1:-1])
    sh.sheet1.update(('C' + str(i)), str(b)[1:-1])
    sh.sheet1.update(('D' + str(i)), str(loss))
    plt.scatter(x, y)
    plt.plot(x, prediction)

a, b = iterate(a, b, x, y, 10000)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
sh.sheet1.update('A11', str(10000))
sh.sheet1.update('B11', str(a)[1:-1])
sh.sheet1.update('C11', str(b)[1:-1])
sh.sheet1.update('D11', str(loss))
plt.scatter(x, y)
plt.plot(x, prediction)

plt.show()


```


## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.

- Скриншоты вывода знчаений через Debug.Log() в Unity значений Loss из Google-таблицы и скрипт с помощью которого это происходит

![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/3-1-1.png)
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/3-1-2.png)

- Этот участок кода отвечает за изменение голосовых команд, теперь взаимосвязь команд и значений следующая: 
  Хорошо -  n <= 1000; 
  Нормально - n > 1000 and n < 1500;
  Плохо - n >= 1500.
  
![](https://github.com/mushr0o0m/DA-in-GameDev-lab2/blob/main/img/3-1-3.png)

- Весь измененный скрипт

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;


public class DataLinearRegression : MonoBehaviour
{
    [SerializeField] private AudioClip goodSpeak;
    [SerializeField] private AudioClip normalSpeak;
    [SerializeField] private AudioClip badSpeak;

    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int counter = 1;

    private void Start()
    {
        StartCoroutine(GoogleSheet());
    }

    private void Update()
    {
        if (!statusStart && counter != dataSet.Count && dataSet["Mon_" + counter.ToString()] <= 1000)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + counter.ToString()]);
        }

        if (!statusStart && counter != dataSet.Count 
            && dataSet["Mon_" + counter.ToString()] > 1000 && dataSet["Mon_" + counter.ToString()] < 1500)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + counter.ToString()]);
        }

        if (!statusStart && counter != dataSet.Count && dataSet["Mon_" + counter.ToString()] >= 1500)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + counter.ToString()]);
        }
    }

    private IEnumerator GoogleSheet()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1BunomDF4_9-10cpm3dKWcXucEfIr3qEa6jKYAPDI1qU/values/Лист1?key=AIzaSyAW_ZU0srE6ifkI2uJcReML4CQiDesSfEA");
        yield return curentResp.SendWebRequest();
        var rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[3]));
        }
    }

    private IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        counter++;
    }

    private IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        counter++;
    }

    private IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        counter++;
    }

}

```

## Выводы

В этой лабораторной работе я узнал как автоматизировать выгрузку данных из python в Google-таблицы с помощью console cloud google, также увидел как изпользовать API на практике и узнал как экспортировать данные из Google-таблиц в Unity и взаимодействовать с ними с помощью словоря. 

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
