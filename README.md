# Определение AI-сгенерированных эссе

## Описание проекта
Цель данного проекта — разработать модель, которая сможет определять, является ли эссе написанным учеником (человеком) или сгенерированным крупной языковой моделью (LLM).  
С ростом использования генеративного ИИ в образовательной среде становится важно предотвращать нарушения академической честности, такие как плагиат, и сохранять возможности для развития письменных навыков учащихся.

```bash
.
├── Dockerfile
├── LICENSE
├── README.md
├── data
│   ├── test_essays.csv
│   └── train_essays.csv
├── model
│   ├── __init__.py
│   ├── dataset.py
│   ├── infer.py
│   └── train.py
├── poetry.lock
└── pyproject.toml



```
---

## Данные

### Источники данных
Данные предоставлены в рамках соревнования на Kaggle:  
[LLM Detect - AI Generated Text](https://www.kaggle.com/competitions/llm-detect-ai-generated-text/overview).

### Состав данных
1. **Обучающая выборка**:  
   - Эссе, написанные учениками по двум заданиям (промтам).  
   - Несколько примеров сгенерированных LLM текстов.  
2. **Тестовая выборка**:  
   - Эссе по пяти другим заданиям, включающие как ученические, так и AI-сгенерированные тексты.  
   - Целевые метки недоступны.  

### Файлы и поля
- `{test|train}_essays.csv`: 
  - `id` — уникальный идентификатор эссе.  
  - `prompt_id` — идентификатор задания.  
  - `text` — текст эссе.  
  - `generated` — метка цели: `0` (ученик), `1` (LLM).  
- `train_prompts.csv`: 
  - `prompt_id` — идентификатор задания.  
  - `prompt_name` — название задания.  
  - `instructions` — инструкция для написания эссе.  
  - `source_text` — контекстное содержание для задания.  

### Возможные особенности данных
- Тематика эссе варьируется, что позволяет моделям работать с текстами разных жанров.  
- Дисбабаланс данных: большая часть обучающей выборки состоит из ученических текстов.  
- Генерализация: тестовые данные содержат задания, не встречавшиеся в обучении.  

---

## Подход к решению

### Модели
1. **Базовые методы**:
   - Логистическая регрессия и Random Forest с использованием признаков (длина предложений, частота слов, лексическое разнообразие).  
2. **Нейросетевые методы**:
   - Fine-tuning языковых моделей, таких как BERT, RoBERTa и DeBERTa (Hugging Face).  
3. **Генерация дополнительных данных**:
   - Создание синтетических эссе с использованием GPT-4 или BLOOM для обучения.  

### Предобработка
- Удаление специальных символов и нормализация текста.  
- Токенизация текста и учет структуры данных.  

### Архитектура
[Сырые данные] -> [Предобработка] -> [Обучение модели] -> [Оценка] -> [Выбор лучшей модели]

---

## Развертывание модели

### Финальный пайплайн
1. **Ввод текста**: загрузка эссе через веб-интерфейс или API.  
2. **Обработка данных**: предобработка текста и токенизация.  
3. **Предсказание**: классификация эссе как `ученическое` или `AI-сгенерированное`.  
4. **Логирование и мониторинг**: сохранение результатов и настройка модели по мере необходимости.  

### Применение
- Проверка академических текстов на плагиат в образовательных учреждениях.  
- Интеграция с платформами для анализа текстов и генерации отчетов.  

---

## Возможные для использования библиотеки и инструменты
- **Python 3.9+**
- **Hugging Face Transformers**
- **Pytorch**
- **scikit-learn**
- **Pandas**
- **NumPy**
- **Flask/FastAPI** для развертывания API
- **etc**

---
