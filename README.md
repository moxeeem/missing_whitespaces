# Missing Whitespaces

Модель восстанавливает пропущенные пробелы в строках. Решение оформлено в Jupyter‑ноутбуке.

## Подход

Базируется на статье **[Fast Whitespace Correction with Encoder‑Only Transformers](https://aclanthology.org/2023.acl-demo.37/)** (ACL 2023). Это быстрый и компактный SOTA‑подход: задача формулируется как **позиционная классификация**, где для каждого символа параллельно решается одно из действий - *оставить / удалить / вставить пробел перед*. Такой дизайн даёт высокую скорость и малый размер модели.

## Данные

Мы объединяем два источника (для покрытия и коротких, и более длинных пользовательских текстов):

1. Фрагменты датасета соревнования **[Avito ML Cup 2025 — Поиск дублей](https://ods.ai/competitions/avitotechmlchallenge2025_2)** - поля `base_title`, `cand_title`, `base_description`, `cand_description`.
2. Новости **[WMT News Crawl](https://data.statmt.org/news-crawl/ru/)** на русском - из них извлекаются короткие предложения, чтобы модель адекватно работала на небольших строках.


## Содержимое репозитория

```
.
├── README.md
├── requirements.txt
├── missing_whitespaces.ipynb      # основной ноутбук (train + inference)
├── dataset_1937770_3.txt          # тестовый датасет для сабмита (id,text_no_spaces)
├── submission.csv                  # результат инференса
└── restored_texts.txt                   # пример восстановленных строк (id,текст_с_пробелами)
```

## Как запустить

### 1) Окружение (Python 3.10+)
```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/Mac:
. .venv/bin/activate
pip install -r requirements.txt
```

### 2) Инференс через ноутбук
1. Откройте `missing_whitespaces.ipynb` и выполните **Run All**.
2. Ноутбук:
   - прочитает `dataset_1937770_3.txt` (формат: `id,text_no_spaces`);
   - загрузит лучший чекпоинт (если присутствует) или обучит модель;
   - сохранит результаты:
     - `sub_infer.csv` — таблица для сабмита;
     - `restored.txt` — пары `id,восстановленный_текст`.

#### Как не обучать заново, а сразу использовать модель

Если вы не хотите обучать модель, а сразу использовать уже обученную, скачайте [архив с чекпоинтом](https://drive.google.com/drive/folders/1HrPQOcORhsCyqjTVmX6IY-46V89-KqpC?usp=sharing) и распакуйте его в папку `experiments/eo_byte_finetune`, чтобы получился файл `experiments/eo_byte_finetune/checkpoint_best.pt`.

Потом запустите раздел инференса в ноутбуке (последний раздел), он самодостаточный и не зависит от предыдущих ячеек.

## Экспериментальная среда

GPU: NVIDIA RTX A4000, 16 ГБ VRAM

RAM: 46.97 ГБ
