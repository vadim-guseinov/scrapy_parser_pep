<div align="center">

# 🕷 PEP Parser (Scrapy)

**Асинхронный парсер всех PEP с peps.python.org: полный реестр и сводная статистика по статусам в CSV**

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![Scrapy](https://img.shields.io/badge/Scrapy-2.9-60A839?logo=scrapy&logoColor=white)
![Twisted](https://img.shields.io/badge/Twisted-async-6C4C9E)
![Tests](https://img.shields.io/badge/pytest-14_passed-2ea44f?logo=pytest&logoColor=white)

</div>

## О проекте

Паук `pep` обходит таблицу всех Python Enhancement Proposals, переходит на страницу каждого документа и собирает номер, название и статус.
На выходе два CSV-файла:

| Файл | Содержимое | Как формируется |
|---|---|---|
| `results/pep_<дата>.csv` | номер, название, статус каждого PEP | Scrapy Feed Exports |
| `results/status_summary_<дата>.csv` | количество PEP в каждом статусе и строка `Total` | собственный `PepParsePipeline` |

Пример последнего запуска — **727 PEP**: Final 370, Rejected 130, Withdrawn 70, Draft 43, Active 38, Deferred 36, Superseded 25, Accepted 14 ([`results/`](results)).

## Что сделано

- Паук на CSS- и XPath-селекторах: `parse()` собирает ссылки из общей таблицы, `parse_pep()` разбирает карточку документа.
- `PepParsePipeline` считает статусы на лету и в `close_spider` записывает сводку с итоговой строкой.
- Настроены Feed Exports, кодировка UTF-8 и «вежливый» обход: `ROBOTSTXT_OBEY`, `DOWNLOAD_DELAY = 1`, один запрос к домену одновременно.
- **14 автотестов pytest** проходят.

## Стек

Python 3.12 · Scrapy 2.9 · Twisted · parsel (CSS/XPath) · itemadapter · csv · pytest · flake8

## Запуск

**Требования:** Python 3.10+ (проверено на 3.12).

```bash
git clone https://github.com/JuliaDJ1/scrapy_parser_pep.git
cd scrapy_parser_pep

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

scrapy crawl pep
```

Полный обход занимает около 15 минут из-за задержки между запросами.

### Тесты

```bash
pytest
```

## Структура

```
pep_parse/
├── spiders/pep.py   # паук: parse() и parse_pep()
├── items.py         # PepParseItem: number, name, status
├── pipelines.py     # PepParsePipeline: сводка по статусам
└── settings.py      # Feed Exports, задержки, пайплайны
results/             # результаты парсинга
tests/
```

## См. также

[bs4_parser_pep](https://github.com/JuliaDJ1/bs4_parser_pep) — синхронная версия на requests + BeautifulSoup с дополнительными режимами.

## Автор

**Вадим Гусейнов** — Python-разработчик (Backend) · [Telegram](https://t.me/eviliing)
