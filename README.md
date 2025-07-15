# 🌀 Speech Emotion Recognition

Классификация эмоций по коротким аудиофрагментам речи с помощью сверточной нейронной сети (2D CNN) на TensorFlow/Keras.

---

## 📋 Описание проекта

Автоматическое распознавание эмоций (happy, angry, calm, sad и др.) по аудиозаписям человеческой речи.

* **Датасет:** [RAVDESS Emotional Speech Audio](https://www.kaggle.com/datasets/uwrfkaggler/ravdess-emotional-speech-audio) (`files/paths_labels_csv/ravdess_paths_and_labels.csv`)
* **Модель:** 2D Convolutional Neural Network (Conv2D на MFCC)
* **Метрики:** Accuracy, Confusion Matrix, F1-score

---

## 📂 Структура проекта

```
speech-emotion-recognition/
├── files/
│   └── paths_labels_csv/
│       └── ravdess_paths_and_labels.csv   # Пути к аудиофайлам и метки эмоций
├── notebooks/
│   ├── Mel_spectrogramm.ipynb             # Основной ноутбук с обучением и анализом
│   └── collecting_dataset.ipynb           # Сбор и препроцессинг датасета
├── requirements.txt                       # Необходимые библиотеки
└── .gitignore
```

---

## ⚙️ Требования и совместимость

* Python 3.9+
* TensorFlow: 2.10.1
* CUDA Toolkit: 11.8 <sub>(*только если требуется ускорение на GPU, для CPU не нужен*)</sub>
* cuDNN: 8.9.4 (для CUDA 11.x) <sub>(*только если требуется ускорение на GPU, для CPU не нужен*)</sub>

> Для корректной работы на GPU убедитесь, что у вас установлены совместимые версии CUDA и cuDNN для TensorFlow 2.10.x.
>
> ⚠️ См. официальную таблицу совместимости: [TensorFlow GPU Support](https://www.tensorflow.org/install/source#gpu)
>
> **Примечание:**
> Если у вас нет дискретной видеокарты NVIDIA или вы не хотите использовать GPU — CUDA Toolkit и cuDNN устанавливать НЕ обязательно, всё будет работать на CPU "из коробки".

---

## Инструкция по установке

<details>
<summary><b>Как установить CUDA Toolkit и cuDNN для работы на GPU</b></summary>

1. Скачайте и установите **CUDA Toolkit 11.8**:

   * [CUDA Toolkit 11.8 Download](https://developer.nvidia.com/cuda-11-8-0-download-archive)
   * Выберите вашу ОС и скачайте инсталлятор (Windows: local `.exe`, Linux: `.run`).
   * Установите в папку, например: `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8`

2. Скачайте **cuDNN 8.9.4 для CUDA 11.x**:

   * [cuDNN 8.9.4 Download](https://developer.nvidia.com/rdp/cudnn-archive)
   * Выберите версию под вашу ОС (Windows или Linux)
   * Распакуйте архив (например, `cudnn-windows-x86_64-8.9.4.25_cuda11-archive.zip`)

3. Скопируйте содержимое cuDNN в CUDA Toolkit:

   * содержимое `bin` → в `CUDA\v11.8\bin\`
   * содержимое `include` → в `CUDA\v11.8\include\`
   * содержимое `lib` → в `CUDA\v11.8\lib\x64\` (Windows) или `lib64` (Linux)

4. Проверьте переменные среды (Windows):

   * Добавьте в PATH:

     * `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\bin`
     * `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\libnvvp`
   * `CUDA_PATH` или `CUDA_HOME` укажите на `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8`

5. Проверьте установку:

   * В терминале: `nvcc --version`
   * В Python:

     ```python
     import tensorflow as tf
     print(tf.config.list_physical_devices('GPU'))
     ```

</details>

---

## 🚀 Быстрый старт

1. **Склонируйте репозиторий и установите зависимости:**

```
git clone https://github.com/German229/speech-emotion-recognition.git
```

```
cd speech-emotion-recognition
```

```
pip install -r requirements.txt
```

2. **Запустите ноутбук:**

* Перейдите в папку `notebooks/`
* Откройте `Mel_spectrogramm.ipynb` в Jupyter Notebook, JupyterLab или VSCode

---

## 🛠 Используемые библиотеки

* Python 3.9+
* pandas, numpy, scikit-learn
* tensorflow (>=2.10, рекомендуется >=2.13, поддержка CUDA 11.8)
* keras
* librosa, audiomentations
* matplotlib, tqdm
  Полный список — в `requirements.txt`.

---

## 🔗 Pipeline ноутбука

1. Импорт библиотек
2. Фиксация random seed для воспроизводимости
3. Проверка наличия GPU
4. Загрузка и анализ датасета
5. Аугментация аудио для train
6. Извлечение MFCC (или мел-спектрограмм)
7. Деление на train/test
8. One-hot encoding меток
9. Балансировка классов (class\_weight)
10. Подбор гиперпараметров (Grid Search)
11. Финальное обучение модели
12. Оценка: accuracy, confusion matrix, F1-score

---

## ⚡️ Автоматический выбор устройства

```
has_gpu = len(tf.config.list_physical_devices('GPU')) > 0
device_name = '/GPU:0' if has_gpu else '/CPU:0'
print(f"Используется устройство: {device_name}")
```

---

## 📊 Основные результаты

**Точность на тестовой выборке:**

* Accuracy: 0.69
* Macro F1-score: 0.67

<details>
<summary>Показать confusion matrix</summary>

```
Confusion Matrix:
[[31  0  3  1  2  0  1  0]
 [ 0 33  0  0  0  5  0  0]
 [ 5  2 22  2  1  0  3  3]
 [ 3  2  1 25  3  0  2  4]
 [ 1  0  0  6 26  2  0  4]
 [ 0  2  2  0  1 16  0  0]
 [ 3  6  2  2  1  6 17  1]
 [ 1  0  3  1  3  0  0 31]]
```

</details>

---

## 💡 Возможные улучшения

* K-fold cross-validation для более объективной оценки
* Аугментация данных (ещё больше аудиопримеров)
* Pre-trained аудиоэмбеддинги (VGGish, YAMNet)
* Ensemble из нескольких моделей

---

## 📬 Контакты

Вопросы по коду или установке — [German229](https://github.com/German229)

---

**P.S.**
Если проект был полезен — поставьте ⭐️ или сделайте fork!
