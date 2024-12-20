# AD-in-GameDev-laba3


## Отчет по лабораторной работе №3

Выполнила:
- Прокошева Мария Евгеньевна
- РИ-230941

### Отметка о выполнении заданий:

| Задание   | Выполнение | Баллы |
|-----------|------------|-------|
| Задание 1 | ✅          | 60    |
| Задание 2 | ✅          | 20    |
| Задание 3 | ✅          | 20    |

### Работу проверили:
- к.т.н., доцент Иванов И.И.
- к.э.н., доцент Петров П.П.
- ст. преп., Сидоров С.С.

## Цель работы
Изучить структуры игры Save RTF, ознакомиться с прототипом игры на движке Unity, проанализировать доступное в игре оружие и предложить новые варианты вооружения.

## Задание 1
### Расширение арсенала оружия в игре Save RTF
Создание таблицы с характеристиками оружия, таких как урон и вероятность попадания. Для этого использовалась Google таблица: Google Sheets (https://docs.google.com/spreadsheets/d/12V0fRsdPyaTffW7ctOaCRHxkH1Bxn4dfSq2ouFk-yuQ/edit?gid=0#gid=0).

На основе тестирования оружия в игре, где здоровье первого босса было 1000 единиц, определены средний урон и количество ударов. Применяемые формулы:

1. Урон: Урон = ХП босса / Количество ударов или патронов
2. Скорострельность: Скорострельность = Количество патронов / Время расхода обоймы
3. Скорость ударов: Скорость ударов = Количество ударов / Время убийства босса

![image](https://github.com/user-attachments/assets/e1d021d1-fe1d-4e4f-a83c-71bf0537830a)


Примерный урон каждого оружия:

![image](https://github.com/user-attachments/assets/fbfe9a4b-3fd4-4d24-ae74-6a1cb6e7ae44)


weapons = ['Пистолет', 'Винтовка', 'Тесак', 'Бензопила']
damage = [50, 70, 100, 200]
![image](https://github.com/user-attachments/assets/6d7e60f4-0109-4e21-9375-8c162ab3316e)


plt.figure(figsize=(10, 6))
plt.bar(weapons, damage, color='skyblue')
plt.title('Средний урон различных видов оружия')
plt.xlabel('Тип оружия')
plt.ylabel('Средний урон')
plt.show()

![image](https://github.com/user-attachments/assets/1d3c0c28-95ef-4f14-aca0-be3ced492231)

Эффективность различных видов оружия теперь зависит от условий дальности стрельбы. Лидируют бензопила, АКМ и дробовик, но есть важные нюансы. Дробовик наиболее эффективен на коротких дистанциях, однако его стоимость и цена патронов выше, чем у других типов вооружения. АКМ является самым дорогостоящим как при покупке, так и в уходе, поэтому игроки не всегда могут его себе позволить. Бензопила остается дорогим оружием для ближнего боя, но с высоким уроном, что оправдывает риск. Метательные ножи, несмотря на низкую эффективность, покупаются как расходные, так как стоят дешевле некоторых патронов. У тесака и бензопилы вероятность попадания на близкой дистанции составляет 100%, так как удары этими орудиями не могут быть случайными.

## Задание 2
### Анализ вариативности времени поведения игрока
Программа для моделирования СКО арбалета и ее рузультаты:

import numpy as np
import matplotlib.pyplot as plt
np.random.seed(42) 

Фиксируем рандом для воспроизводимости

shots = np.random.normal(loc=0, scale=5, size=(2, 2))  # Отклонения (x, y) из арбалета произведено 2 выстрела
Вычисляем отклонения (расстояния от центра цели)
distances = np.linalg.norm(shots, axis=1)
Среднеквадратическое отклонение
std_dev = np.std(distances)
print(f"СКО отклонений: {std_dev:.2f} пикселей")
Визуализация попаданий
plt.figure(figsize=(6, 6))
plt.scatter(shots[:, 0], shots[:, 1], color='red', label='Попадания')
plt.scatter(0, 0, color='blue', label='Цель', s=100)
plt.xlim(-20, 20)
plt.ylim(-20, 20)
plt.axhline(0, color='black', lw=0.5)
plt.axvline(0, color='black', lw=0.5)
plt.legend()
plt.title("Модель разброса выстрелов")
plt.gca().set_aspect('equal', adjustable='box')
plt.show()

СКО: 2,85 пикселей

![image](https://github.com/user-attachments/assets/3e054146-10bf-4299-9cd9-155b2739dd93)

Программа для моделирования разброса урона арбалета и ее рузультаты:

import numpy as np
import matplotlib.pyplot as plt

Генерация случайных значений урона с нормальным распределением

np.random.seed(123)
damage = np.random.normal(loc=25, scale=3, size=100)  
Визуализация гистограммы урона
plt.figure(figsize=(8, 6))
plt.hist(damage, bins=10, color='purple', alpha=0.7, edgecolor='black')
plt.axvline(damage.mean(), color='red', linestyle='dashed', linewidth=2, label=f'Среднее: {damage.mean():.2f}')
plt.axvline(damage.mean() + np.std(damage), color='green', linestyle='dotted', label=f'СКО: {np.std(damage):.2f}')
plt.axvline(damage.mean() - np.std(damage), color='green', linestyle='dotted')
plt.legend()
plt.title('Распределение урона оружия')
plt.xlabel('Урон')
plt.ylabel('Частота')
plt.show()
![image](https://github.com/user-attachments/assets/696cfa85-b5a8-4d3e-934c-ef24b95559b3)

Программа вариативности времени отклика игрока и ее рузультаты:

import numpy as np
import matplotlib.pyplot as plt

reaction_times = np.random.normal(loc=500, scale=40, size=20)  # Среднее время = 500 мс

plt.figure(figsize=(8, 6))
plt.plot(reaction_times, 'o-', color='teal')
plt.axhline(reaction_times.mean(), color='red', linestyle='dashed', label=f'Среднее: {reaction_times.mean():.2f} мс')
plt.fill_between(range(20),
                 reaction_times.mean() - np.std(reaction_times),
                 reaction_times.mean() + np.std(reaction_times),
                 color='lightgreen', alpha=0.3, label=f'СКО: ±{np.std(reaction_times):.2f} мс')
plt.legend()
plt.title('Время реакции игрока')
plt.xlabel('Событие')
plt.ylabel('Время (мс)')
plt.show()

![image](https://github.com/user-attachments/assets/03f10d2c-bc95-44b1-b591-9d618d5dd47f)


## Задание 3
### Предварительный просмотр данных из Google и интеграция с Unity
Данные из таблиц визуализированы с помощью Python и переданы в Unity.

import pandas as pd

url = 'https://docs.google.com/spreadsheets/d/12V0fRsdPyaTffW7ctOaCRHxkH1Bxn4dfSq2ouFk-yuQ/export?format=csv'
data = pd.read_csv(url)

plt.figure(figsize=(10, 6))
plt.bar(data['Weapon'], data['Damage'], color='coral')
plt.title('Урон различных видов оружия')
plt.xlabel('Оружие')
plt.ylabel('Урон')
plt.show()


## Выводы
В результате работы были проанализированы характеристики оружия в игре Save RTF и предложены новые варианты вооружения. Модели сбалансированного оружия, созданные с помощью Python, могут быть интегрированы в проект Unity, созданного более разнообразный игровой процесс.
