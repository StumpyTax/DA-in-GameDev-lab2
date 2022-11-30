# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #5 выполнил(а):
- Демидов Никита Александрович
- РИ-210913
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
Интеграция экономической системы в проект Unity и обучение ML-Agent.

## Задание 1
### 1. Измените параметры файла. yaml-агента и определить какие параметры и как влияют на обучение модели.

```yaml
#Указывает какое поведение отслеживается
behaviors:
  #Имя модели поведения
  Economic:
    #Тип обучения, по умолчанию ppo
    trainer_type: ppo
    #Параметры содержащие данные которые управляют обучением
    hyperparameters:
      #Количество опытов в каждой итерации градиентного спуска.
      batch_size: 1024
      #При trainer_type:
      # PPO: количество опытов, которые необходимо собрать перед обновлением модели поведения. Соответствует тому, сколько опыта должно быть собрано, прежде чем мы будем изучать или обновлять модель. Это значение должно быть в несколько раз больше, чем batch_size. Обычно больший размер буфера соответствует более стабильным обновлениям обучения.
      # SAC: максимальный размер буфера опыта — примерно в тысячи раз больше, чем ваши эпизоды, чтобы SAC мог учиться как на старом, так и на новом опыте.
      buffer_size: 10240
      # Начальная скорость обучения для градиентного спуска
      learning_rate: 3.0e-4
      #Определяет, как скорость обучения изменяется с течением времени.
      learning_rate_schedule: linear
      #Сила энтропийной регуляризации, которая делает поведение «более случайным».
      beta: 1.0e-5
      #Влияет на то, насколько быстро поведение может развиваться во время обучения
      epsilon: 0.6
      # Параметр регуляризации (лямбда), используемый при расчете обобщенной оценки преимущества. Его можно рассматривать как то, насколько агент полагается на свою текущую оценку стоимости при расчете обновленной оценки стоимости.
      lambd: 0.95
      # Количество проходов через буфер опыта при выполнении оптимизации градиентного спуска.
      num_epoch: 8     
    #Конфигурация для нейронной сети
    network_settings:
      # Применяется ли нормализация к входным данным векторного наблюдения.
      normalize: false
      #Количество элементов в скрытых слоях нейронной сети
      hidden_units: 128
      #Количество скрытых слоев в нейронной сети
      num_layers: 2
    #Позволяет задавать настройки как для внешних (т. е. основанных на среде), так и для внутренних сигналов вознаграждения (например, любопытство и GAIL).
    reward_signals:
      #Настройки для сигналов основанных на среде
      extrinsic:
        #Коэффициент дисконтирования для будущих вознаграждений, поступающих из окружающей среды.
        gamma: 0.99
        #Коэффициент, на который умножается вознаграждение, данное средой. Типичные диапазоны будут варьироваться в зависимости от сигнала вознаграждения.
        strength: 0.9
    #Количество опытов, собранных тренером между каждой контрольной точкой
    checkpoint_interval: 500000
    #Максимальное кол-во шагов
    max_steps: 750000
    #Сколько шагов опыта нужно собрать для каждого агента, прежде чем добавить его в буфер опыта.
    time_horizon: 64
    #Количество опытов, которое необходимо собрать перед созданием и отображением статистики обучения.
    summary_freq: 5000
    self_play:
      #Количество шагов тренера между сохранениями политики.
      save_steps: 20000
      #Количество train_steps между переключением обучающей команды
      team_change: 100000
      #Кол-во шагов которые делает агент с заданой политикой, не обучаясь.
      swap_steps: 10000
      #Вероятность того, что агент будет играть против последней политики оппонента.
      play_against_latest_model_ratio: 0.1
      #Размер скользящего окна прошлых снимков, из которых выбираются противники агента.
      window: 10
```

## Задание 2
### Опишите результаты, выведенные в TensorBoard. 

С изначальным yaml:
![1](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/1.png)
![2](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/2.png)
![3](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/3.png)
![4](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/4.png)


C измененным yaml(beta:1.0e-3=>1.0e-5,
                  epsilon: 0.2=>0.6,
                  num_epoch:3=>8,     
                  strength: 1.0=>0.9):

![2.1](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/2.1.png)
![2.2](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/2.2.png)
![2.3](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/2.3.png)
![2.4](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/2.4.png)
![2.5](https://raw.githubusercontent.com/StumpyTax/DA-in-GameDev-lab2/lab5/images/2.5.png)

Cumulative Rewards - Накопленное вознаграждение, должно постоянно увеличиваться пока не дойдет до стабильного значения.

Episode Length - Длительность эпизода, со временема должен уменьшаться, т.к при правильном обучении агент должен решать задачу с каждой итерацией быстрее.

Policy Loss — это потеря предсказания наилучшего хода. Агент пытается предсказать, каким будет наилучшее действие в текущей ситуации. Это политика. Потери политики измеряются тем, насколько это было неправильно.

Value loss- коэффициент, который также должен уменьшаться со временем, поскольку модель становится более точной с каждым новым эпизодом.

После изменения настроек обучения можно заметить, что график Cumulative Reward, Value Loss, Beta.
Cumulative Reward растет на протяжении всего времени обучения.
Policy Loss примерно равно нулю на протяжении всего обучения.
Value Loss стремится к нулю, что может быть показателем успешного обучения.


## Выводы
Интегрировал экономическую систему в проект Unity и обучил ML-Agent.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
