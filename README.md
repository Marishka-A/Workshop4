# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev] perceptron

Отчет по лабораторной работе #4 выполнил(а):
- Абросимова Марина Михайловна
- РИ-220931
  
Отметка о выполнении заданий:

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Цель работы
- Создайте пустой проект на Unity. 
- Добавьте пустой объект Empty Game Object с именем Perceptron. 
- Подключите Perceptron.cs к пустому Game Object с именем Perceptron.
- Научить Персептрон производить логические вычесления.
## Задание 1:

Ход работы:

В новом пустом проекте Unity реализовать персептрон, умеющий производить вычисления.

(таблица истинности)

- OR   | 0+0=0   | 0+1=1   | 1+0=1   | 1+1=1   | 
- AND  | 0+0=0   | 0+1=0   | 1+0=0   | 1+1=1   |
- NAND | 0+0=1   | 0+1=1   | 1+0=1   | 1+1=0   |
- XOR  | 0+0=0   | 0+1=1   | 1+0=1   | 1+1=0   |

Скрипт для перцептрона из предоставленого репозитория:
```C#

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}

```

1) Реализация логической операции OR для модели перцептрона

![2023-12-11_00-41-14](https://github.com/Marishka-A/Workshop4/assets/126682278/e80ae6ad-a9c0-42e7-9cf7-fe601d7542fc)

2) Реализация логической операции AND для модели перцептрона

![2023-12-11_00-40-18](https://github.com/Marishka-A/Workshop4/assets/126682278/a1ac5b5f-9539-45c0-9fb3-e412f1ac65ea)

3) Реализация логической операции NAND для модели перцептрона

![2023-12-11_00-40-52](https://github.com/Marishka-A/Workshop4/assets/126682278/a9734947-a5a8-4ddb-9b31-ea9c0c8d5882)

4) Реализация логической операции XOR для модели перцептрона

![2023-12-11_00-41-54](https://github.com/Marishka-A/Workshop4/assets/126682278/9d78a25c-30a1-4a5e-886e-b7b03ea3e31f)

Синим выделено поле, указывающее на обучение. При TOTAL ERROR != 0 обучение не проходит.


## Задание 2:

Ход работы:

Построить графики зависимости количества эпох от ошибки обучения. Указать, от чего зависит необходимое количество эпох обучения.

Метод, с помощью которого меняем эпохи:
![2023-12-11_01-28-22](https://github.com/Marishka-A/Workshop4/assets/126682278/74225a05-c6c6-451c-bec4-e37add86f503)

При запуске персептрона для всех логических операций было выявленно:

1) Для операций OR и AND персептрон к 6 эпохе перестает делать ошибки

2) Для операции NAND ошибок нет к 7 эпохе.

3) Для операции XOR модель персептрона не являеться рабочей. Она в силах решать только линейные задачи, когда же операция XOR ей не является. Следовательно ни к какой эпохе модель не обучится.


Так как выяснилось, что для всех линейных операций персептрон обучается к 7 эпохе, построила таблицу (Построение вела за счет среднего показателя TOTAL ERROR за каждую эпоху, так как эта величина указывает на результат обучения):

![2023-12-11_01-42-13](https://github.com/Marishka-A/Workshop4/assets/126682278/31c7fa6c-4226-40bd-88ea-4a518427cdb7)
![Зависимось](https://github.com/Marishka-A/Workshop4/assets/126682278/76571a82-4a7c-4782-ba74-887133f7e942)

Ссылка на таблицу (https://docs.google.com/spreadsheets/d/1-jckvv8SPSbgoYJc_WY-63xmfX1p_PlmNQbqshxScWU/edit#gid=0)

## Задание 3:

Ход работы:

Визуализировать работу персептрона на сцене Unity

Для этого создала проект в Unity и создала 4 сцены (OR, AND, NAND, XOR).
Ознакомиться с проектом можно выше в этом же репозитории.

Примеры из моего проекта:

Для AND:
![giphy](https://github.com/Marishka-A/Workshop4/assets/126682278/6a9cd82d-0dca-4105-8686-cbf1a9c11920)

Для OR:
![giphy (2)](https://github.com/Marishka-A/Workshop4/assets/126682278/6b8602a3-3bed-4343-a017-12b2a1bb38e4)

Для NAND:
![giphy (1)](https://github.com/Marishka-A/Workshop4/assets/126682278/632c14b1-6b1c-4cfb-ada2-4cfadaac0a33)

Для XOR:
![giphy (3)](https://github.com/Marishka-A/Workshop4/assets/126682278/e4f5d05b-353f-43f3-b5ef-1de1e00c98d7)

## Выводы

В ходе проведенной лабороторной работы были подробно изучены такие логические операции как OR(Логическое сложение), AND(Логическое умножение), NAND(Логическое умножение с отрицанием) и XOR(Исключающая логическая сумма). На основе этих операций был реализован Персептрон и в ходе его обущения были выявлены эпохи. От эпох были выявлены зависимость их колличества для каждой из логих, и на этой основе построены графики. А также вся работа была визуализированна на сцене Unity (проект приложен).


## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
