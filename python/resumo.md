# Python

## ambiente virtual

- venv
  - craição do ambiente `python3 -m venv <nome>=.venv`
  - ativação do ambiente `source .venv/bin/activate`
  - validação da ativação `which python3`
  - desativação `deactivate`
- pip
  - `pip install ...`
  - arquivo requirements.txt `pip freeze > requirements.txt`
- env
  - para escrita usar arquivo .env normal 
  - para leitura importar módulo `os`
    - disponíveis em `os.environ`
  - OU instalar o pacote `python-decouple`
```python
from decouple import config

API_USER = config("USER")
DEBUG_MODE = config("DEBUG_MODE", default=False, cast=bool)
```
## Leitura e escrita de arquivos

- Json
  -  Seus principais métodos para manipulação são: load, loads, dump, dumps
    - O loads carrega o JSON a partir de um texto e o load carrega o JSON a partir de um arquivo
    - O dumps escreve o python em formato json e salva em uma variavel e o dump escreve o python diretamente no arquivo, fazendo a conversão internamente
```python
import json

# leitura do arquivo e posterior conversão (loads)
with open("foo.json") as file:
    content = file.read()  # leitura do arquivo
    bar = json.loads(content) # estrutura python
    
# conversão do arquivo para codigo diretamente (load)
with open("foo.json") as file:
    bar = json.load(file) # estrutura python
    
# escrita em json fazendo a conversão para texto (dumps)
with open("foo.json", "w") as file:
    json_to_write = json.dumps(bar)  # conversão de Python para o formato json (str)
    file.write(json_to_write)
    
# escrita em json com conversão interna (dump)
with open("foo.json", "w") as file:
    json.dump(bar, file) # escreve no arquivo já transformando em formato json a estrutura
```
- CSV
  - O módulo csv, contém duas principais funções, um leitor (reader) que faz a leitura do conteúdo, já fazendo as transformações dos valores para Python e um escritor (writer) que faz a escrita nesse formato 
  - o método writer tem o método writeRow
```python
import csv

# leitura do arquivo (reader)
with open("foo.csv", encoding = "utf-8") as file:
    bar_reader = csv.reader(file, delimiter=",", quotechar='"')
    header, *data = bar_reader
    
# escrita do arquivo (writer)
with open("foo.csv", "w", encoding = "utf-8") as file:
    writer = csv.writer(file)

    headers = ["bar", "zee"]
    writer.writerow(headers)

    for pee, qee in mee.items():
        row = [pee, qee]
        writer.writerow(row)
```

