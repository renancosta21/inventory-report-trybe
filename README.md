## O que deverá ser desenvolvido

No projeto passado você implementou algumas funções que faziam leitura e escrita de arquivos `JSON` e `CSV`, correto? Neste projeto nós vamos fazer algo parecido, mas utilizando a Programação Orientada a Objetos! Você implementará um gerador de relatórios que recebe como entrada arquivos com dados de um estoque e gera, como saída, um relatório acerca destes dados.

Esses dados de estoque poderão ser obtidos de diversas fontes:

- Através da importação de um arquivo `CSV`;

- Através da importação de um arquivo `JSON`;

- Através da importação de um arquivo `XML`;

Além disso, o relatório final deverá poder ser gerado em duas versões: simples e completa.

### Como o projeto deve ser executável

Após implementar o requisito bônus, seu programa deverá ser executável **via linha de comando** com o comando `inventory_report <argumento1> <argumento2>`:

- O **<argumento 1>** deve receber o caminho de um arquivo a ser importado. O arquivo pode ser um `csv`, `json` ou `xml`.

- O **<argumento 2>** pode receber duas strings: `simples` ou `completo`, cada uma gerando o respectivo relatório.

---

Para executar os testes, lembre-se de primeiro **criar e ativar o ambiente virtual**, além de também instalar as dependências do projeto. Isso pode ser feito através dos comandos:

```bash
$ python3 -m venv .venv

$ source .venv/bin/activate

$ python3 -m pip install -r dev-requirements.txt
```

O arquivo `dev-requirements.txt` contém todos as dependências que serão utilizadas no projeto, ele está agindo como se fosse um `package.json` de um projeto `Node.js`. Com as dependências já instaladas, para executar os testes basta usar o comando:

```bash
$ python3 -m pytest
```

Para verificar se você está seguindo o guia de estilo do Python corretamente, você pode executá-lo com o seguinte comando:

```bash
$ python3 -m flake8
```

---

## Requisitos obrigatórios:

#### 1 - Criar um método `generate` numa classe `SimpleReport` do módulo `inventory_report/reports/simple_report.py`. Esse método deverá receber dados numa lista contendo estruturas do tipo `dict` e deverá retornar uma string formatada como um relatório.

- Deve ser possível executar o método `generate` sem instanciar um objeto de `SimpleReport`
- O método deve receber de parâmetro uma lista de dicionários no seguinte formato:

   ```json
   [
     {
       "id": 1,
       "nome_do_produto": "CALENDULA OFFICINALIS FLOWERING TOP, GERANIUM MACULATUM ROOT, SODIUM CHLORIDE, THUJA OCCIDENTALIS LEAFY TWIG, ZINC, and ECHINACEA ANGUSTIFOLIA",
       "nome_da_empresa": "Forces of Nature",
       "data_de_fabricacao": "2020-07-04",
       "data_de_validade": "2023-02-09",
       "numero_de_serie": "FR48 2002 7680 97V4 W6FO LEBT 081",
       "instrucoes_de_armazenamento": "in blandit ultrices enim lorem ipsum dolor sit amet consectetuer adipiscing elit proin interdum mauris non ligula pellentesque ultrices    phasellus"
     }
   ]
   ```

- O método deverá retornar uma saída com o seguinte formato:

   ```bash
   Data de fabricação mais antiga: YYYY-MM-DD
   Data de validade mais próxima: YYYY-MM-DD
   Empresa com maior quantidade de produtos estocados: NOME DA EMPRESA
   ```
- A data de validade mais próxima, somente considera itens que ainda não venceram.

**Dica**: O módulo [datetime](https://docs.python.org/3/library/datetime.html) vai te ajudar.

##### As seguintes verificações serão feitas:

- 1.1 - Será validado que é possível que o método `generate` da classe `SimpleReport` retorne a data de fabricação mais antiga

- 1.2 - Será validado que é possível que o método `generate` da classe `SimpleReport` retorne a validade mais próxima

- 1.3 - Será validado que é possível que o método `generate` da classe `SimpleReport` retorne a empresa com maior estoque

- 1.4 - Será validado que é possível que o método `generate` da classe `SimpleReport` retorne o relatório no formato correto

#### 2 - Criar um método `generate` numa classe `CompleteReport` do módulo `inventory_report/reports/complete_report.py`. Esse método deverá receber dados numa lista contendo estruturas do tipo `dict` e deverá retornar uma string formatada como um relatório.

- A classe `CompleteReport` deve herdar o método (`generate`) da classe `SimpleReport`, de modo a especializar seu comportamento.

- O método deve receber de parâmetro uma lista de dicionários no seguinte formato:

   ```json
   [
     {
       "id": 1,
       "nome_do_produto": "CALENDULA OFFICINALIS FLOWERING TOP, GERANIUM MACULATUM ROOT, SODIUM CHLORIDE, THUJA OCCIDENTALIS LEAFY TWIG, ZINC, and ECHINACEA ANGUSTIFOLIA",
       "nome_da_empresa": "Forces of Nature",
       "data_de_fabricacao": "2020-07-04",
       "data_de_validade": "2023-02-09",
       "numero_de_serie": "FR48 2002 7680 97V4 W6FO LEBT 081",
       "instrucoes_de_armazenamento": "in blandit ultrices enim lorem ipsum dolor sit amet consectetuer adipiscing elit proin interdum mauris non ligula pellentesque ultrices    phasellus"
     }
   ]
   ```

- O método deverá retornar uma saída com o seguinte formato:

   ```bash
   Data de fabricação mais antiga: YYYY-MM-DD
   Data de validade mais próxima: YYYY-MM-DD
   Empresa com maior quantidade de produtos estocados: NOME DA EMPRESA

   Produtos estocados por empresa:
   - Physicians Total Care, Inc.: QUANTIDADE
   - Newton Laboratories, Inc.: QUANTIDADE
   - Forces of Nature: QUANTIDADE
   ```

##### As seguintes verificações serão feitas:

- 2.1 - Será validado que é possível que o método `generate` da classe `CompleteReport` retorne a data de fabricação mais antiga

- 2.2 - Será validado que é possível que o método `generate` da classe `CompleteReport` retorne a validade de fabricação mais próxima

- 2.3 - Será validado que é possível que o método `generate` da classe `CompleteReport` retorne a empresa com maior estoque

- 2.4 - Será validado que é possível que o método `generate` da classe `CompleteReport` retorne a quantidade de produtos por empresa

- 2.5 - Será validado que é possível que o método `generate` da classe `CompleteReport` retorne o relatório no formato correto

#### 3 - Criar um método `import_data` dentro de uma classe `Inventory` do módulo `inventory_report/inventory/inventory.py`, capaz de ler um arquivo CSV o qual o caminho é passado como parâmetro.

- O método, receberá como parâmetro o caminho para o arquivo CSV e o tipo de relatório a ser gerado (`"simples"`, `"completo"`). De acordo com os parâmetros recebidos, deve recuperar os dados do arquivo e chamar o método de gerar relatório correspondente à entrada passada. Ou seja, o método da classe `Inventory` deve chamar o método `generate` da classe que vai gerar o relatório (`SimpleReport`, `CompleteReport`).

##### As seguintes verificações serão feitas:

- 3.1 - Será validado que ao importar um arquivo csv simples será retornado com sucesso

- 3.2 - Será validado que ao importar um arquivo csv completo será retornado com sucesso

#### 4 - Criar um método `import_data` dentro de uma classe `Inventory` do módulo `inventory_report/inventory/inventory.py`, capaz de ler um arquivo JSON o qual o caminho é passado como parâmetro.

- O método, receberá como parâmetro o caminho para o arquivo JSON e o tipo de relatório a ser gerado (`"simples"`, `"completo"`). De acordo com os parâmetros recebidos, deve recuperar os dados do arquivo e chamar o método de gerar relatório correspondente à entrada passada. Ou seja, o método da classe `Inventory` deve chamar o método `generate` da classe que vai gerar o relatório (`SimpleReport`, `CompleteReport`).

📌 Atente que estamos utilizando o mesmo método do requisito anterior.

##### As seguintes verificações serão feitas:

- 4.1 - Será validado que ao importar um arquivo json simples será retornado com sucesso

- 4.2 - Será validado que ao importar um arquivo json completo será retornado com sucesso

#### 5 - Criar um método `import_data` dentro de uma classe `Inventory` do módulo `inventory_report/inventory/inventory.py`, capaz de ler um arquivo XML o qual o caminho é passado como parâmetro.

- O método, receberá como parâmetro o caminho para o arquivo XML e o tipo de relatório a ser gerado (`"simples"`, `"completo"`). De acordo com os parâmetros recebidos, deve recuperar os dados do arquivo e chamar o método de gerar relatório correspondente à entrada passada. Ou seja, o método da classe `Inventory` deve chamar o método `generate` da classe que vai gerar o relatório (`SimpleReport`, `CompleteReport`).

📌 Atente que estamos utilizando o mesmo método do requisito anterior.

##### As seguintes verificações serão feitas:

- 5.1 - Será validado que ao importar um arquivo xml simples será retornado com sucesso

- 5.2 - Será validado que ao importar um arquivo xml completo será retornado com sucesso

#### 6 - Criar uma classe abstrata `Importer` no módulo `inventory_report/importer/importer.py`, que terá três classes herdeiras: `CsvImporter`, `JsonImporter` e `XmlImporter`, cada uma definida em seu respectivo módulo.

- A classe abstrata deve definir a assinatura do método `import_data` a ser implementado por cada classe herdeira. Ela deve receber como parâmetro o nome do arquivo a ser importado.

- O método `import_data` definido por cada classe herdeira deve lançar uma exceção caso a extensão do arquivo passado por parâmetro seja inválida. Por exemplo, quando se passa um  caminho de um arquivo extensão CSV para o `JsonImporter`.

- O método deverá ler os dados do arquivo passado e retorná-los estruturados em uma lista de dicionários conforme exemplo abaixo:

   ```json
   [
     {
       "id": 1,
       "nome_do_produto": "CALENDULA OFFICINALIS FLOWERING TOP, GERANIUM MACULATUM ROOT, SODIUM CHLORIDE, THUJA OCCIDENTALIS LEAFY TWIG, ZINC, and ECHINACEA ANGUSTIFOLIA",
       "nome_da_empresa": "Forces of Nature",
       "data_de_fabricacao": "2020-07-04",
       "data_de_validade": "2023-02-09",
       "numero_de_serie": "FR48 2002 7680 97V4 W6FO LEBT 081",
       "instrucoes_de_armazenamento": "in blandit ultrices enim lorem ipsum dolor sit amet consectetuer adipiscing elit proin interdum mauris non ligula pellentesque ultrices    phasellus"
     }
   ]
   ```

##### As seguintes verificações serão feitas:

- 6.1 - Será validado que a casse CsvImporter está herdando a classe Importer

- 6.2 - Será validado que a casse JsonImporter está herdando a classe Importer

- 6.3 - Será validado que a casse XmlImporter está herdando a classe Importer

- 6.4 - Será validado que a classe CsvImporter esta importando os dados para uma lista

- 6.5 - Será validado que a classe JsonImporter esta importando os dados para uma lista

- 6.6 - Será validado que a classe XmlImporter esta importando os dados para uma lista

- 6.7 - Será validado que ao enviar um arquivo com extensão incorreta para o CsvImporter irá gerar um erro

- 6.8 - Será validado que ao enviar um arquivo com extensão incorreta para o JsonImporter irá gerar um erro

- 6.9 - Será validado que ao enviar um arquivo com extensão incorreta para o XmlImporter irá gerar um erro

👀 Estamos separando a lógica em várias classes (estratégias), preparando para aplicarmos o padrão de projeto **Strategy**. É uma solução para o caso em que uma classe possui muitas responsabilidades (propósitos).
