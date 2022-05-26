**Tópicos**:
* **Limpeza de Dados:**
 - **Por que fazer ?**
 - **Investigação e Definição de Tipos** 
 - **Investigação de inconsistências**
 - **Dados Faltosos:**
     - Origens
     - Tratamentos
 - **Análise de Duplicatas:**
     - Linhas
     - Colunas
 - **Remoção de Colunas Constantes**
 - **Remoção de Colunas por Baixa Variância**
 


```python
import pandas as pd
import numpy as np
```

# Limpeza de Dados

## Por que fazer?

O processo de limpeza de dados engloba diversas etapas que garantem a integridade dos dados. Para que um dado seja utilizado com segurança em qualquer processo, incluindo data science, devemos ter a certeza de que sua origem é confiável. Do contrário, qualquer resultado obtido por meio de um dado não confiável é, via de regra, também não confiável.

Se eu te der um pó branco para fazer um bolo, alegando que é farinha de trigo, você faz? 

**ESPERO QUE NÃO**, pois você:
* Não sabe se é realmente farinha de trigo
* Não sabe qual o prazo de validade
* Não sabe se é com fermento ou sem fermento

Os dados para um cientista de dados, são como os ingredientes de um bolo. E, claro, como qualquer resultado final de uma receita depende de bons produtos e ingredientes, o resultado final de uma análise ou até mesmo um modeo vai depender de dados confiáveis.

## Investigação e Definição de Tipos

A primeira coisa que você deve identificar é o tipo das colunas com as quais você vai lidar. Geralmente lidamos com centenas de colunas e mapear isso é trabalhoso. E esse processo vai depender vai depender do conhecimento de negócio.


```python
nyc_sales = pd.read_csv(r'C:\Users\vitor.silva\Desktop\Estudo Python\Python  - Digital\nyc-rolling-sales_twentieth.csv')
```


```python
nyc_sales.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>EASE-MENT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>...</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>392</td>
      <td>6</td>
      <td></td>
      <td>C2</td>
      <td>153 AVENUE B</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1633</td>
      <td>6440</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>6625000</td>
      <td>2017-07-19 00:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>26</td>
      <td></td>
      <td>C7</td>
      <td>234 EAST 4TH   STREET</td>
      <td>...</td>
      <td>28</td>
      <td>3</td>
      <td>31</td>
      <td>4616</td>
      <td>18690</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>-</td>
      <td>2016-12-14 00:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>39</td>
      <td></td>
      <td>C7</td>
      <td>197 EAST 3RD   STREET</td>
      <td>...</td>
      <td>16</td>
      <td>1</td>
      <td>17</td>
      <td>2212</td>
      <td>7803</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>-</td>
      <td>2016-12-09 00:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2B</td>
      <td>402</td>
      <td>21</td>
      <td></td>
      <td>C4</td>
      <td>154 EAST 7TH STREET</td>
      <td>...</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>2272</td>
      <td>6794</td>
      <td>1913</td>
      <td>2</td>
      <td>C4</td>
      <td>3936272</td>
      <td>2016-09-23 00:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>404</td>
      <td>55</td>
      <td></td>
      <td>C2</td>
      <td>301 EAST 10TH   STREET</td>
      <td>...</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>2369</td>
      <td>4615</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>8000000</td>
      <td>2016-11-17 00:00:00</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



A primeira coisa que observamos é a coluna ```Unnamed: 0```. Devemos removê-la, já que nem nome ela possui. Essa coluna surge geralmente devido a gravação do índice do dataframe como coluna do csv. Se quisermos evitar que um índice de dataframe seja salvo após nossa análise, passamos o parâmetro ```index=False``` para o método ```.to_csv()```.

No Link abaixo podemos ver um pouco da descrição dos dados.<br>
<a href="https://www.kaggle.com/new-york-city/nyc-property-sales"> Descrição dos Dados</a>


```python
nyc_sales.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 16909 entries, 0 to 16908
    Data columns (total 22 columns):
     #   Column                          Non-Null Count  Dtype 
    ---  ------                          --------------  ----- 
     0   Unnamed: 0                      16909 non-null  int64 
     1   BOROUGH                         16909 non-null  int64 
     2   NEIGHBORHOOD                    16909 non-null  object
     3   BUILDING CLASS CATEGORY         16909 non-null  object
     4   TAX CLASS AT PRESENT            16909 non-null  object
     5   BLOCK                           16909 non-null  int64 
     6   LOT                             16909 non-null  int64 
     7   EASE-MENT                       16909 non-null  object
     8   BUILDING CLASS AT PRESENT       16909 non-null  object
     9   ADDRESS                         16909 non-null  object
     10  APARTMENT NUMBER                16909 non-null  object
     11  ZIP CODE                        16909 non-null  int64 
     12  RESIDENTIAL UNITS               16909 non-null  int64 
     13  COMMERCIAL UNITS                16909 non-null  int64 
     14  TOTAL UNITS                     16909 non-null  int64 
     15  LAND SQUARE FEET                16909 non-null  object
     16  GROSS SQUARE FEET               16909 non-null  object
     17  YEAR BUILT                      16909 non-null  int64 
     18  TAX CLASS AT TIME OF SALE       16909 non-null  int64 
     19  BUILDING CLASS AT TIME OF SALE  16909 non-null  object
     20  SALE PRICE                      16909 non-null  object
     21  SALE DATE                       16909 non-null  object
    dtypes: int64(10), object(12)
    memory usage: 2.8+ MB
    

Algumas variáveis que são do tipo numérico estão representadas como ```object```:
* ```SALE PRICE```
* ```GROSS SQUARE FEET```
* ```LAND SQUARE FEET```
* ```APARTMENT NUMBER```

Da mesma maneira temos variáveis categóricas que estão como ```int64```:
* ```BOROUGH```
* ```BLOCK```
* ```LOT```
* ```TAX CLASS AT TIME OF SALE```

Inclusive, a própria descrição das variáveis nos revela que podemos utilizar BLOCK da seguinte maneira: Manhattan (1), Bronx (2), Brooklyn (3), Queens (4), and Staten Island (5).

Vamos tentar algumas conversões? Para isso iremos tentar as conversões através do método ```.astype()```


```python
# object to float 
nyc_sales['SALE PRICE'] = nyc_sales['SALE PRICE'].astype('float')
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-7-4564fbe2b2cc> in <module>
          1 # object to float
    ----> 2 nyc_sales['SALE PRICE'] = nyc_sales['SALE PRICE'].astype('float')
    

    ~\anaconda3\lib\site-packages\pandas\core\generic.py in astype(self, dtype, copy, errors)
       5875         else:
       5876             # else, only a single dtype is given
    -> 5877             new_data = self._mgr.astype(dtype=dtype, copy=copy, errors=errors)
       5878             return self._constructor(new_data).__finalize__(self, method="astype")
       5879 
    

    ~\anaconda3\lib\site-packages\pandas\core\internals\managers.py in astype(self, dtype, copy, errors)
        629         self, dtype, copy: bool = False, errors: str = "raise"
        630     ) -> "BlockManager":
    --> 631         return self.apply("astype", dtype=dtype, copy=copy, errors=errors)
        632 
        633     def convert(
    

    ~\anaconda3\lib\site-packages\pandas\core\internals\managers.py in apply(self, f, align_keys, ignore_failures, **kwargs)
        425                     applied = b.apply(f, **kwargs)
        426                 else:
    --> 427                     applied = getattr(b, f)(**kwargs)
        428             except (TypeError, NotImplementedError):
        429                 if not ignore_failures:
    

    ~\anaconda3\lib\site-packages\pandas\core\internals\blocks.py in astype(self, dtype, copy, errors)
        671             vals1d = values.ravel()
        672             try:
    --> 673                 values = astype_nansafe(vals1d, dtype, copy=True)
        674             except (ValueError, TypeError):
        675                 # e.g. astype_nansafe can fail on object-dtype of strings
    

    ~\anaconda3\lib\site-packages\pandas\core\dtypes\cast.py in astype_nansafe(arr, dtype, copy, skipna)
       1095     if copy or is_object_dtype(arr) or is_object_dtype(dtype):
       1096         # Explicit copy, or required since NumPy can't view from / to object.
    -> 1097         return arr.astype(dtype, copy=True)
       1098 
       1099     return arr.view(dtype)
    

    ValueError: could not convert string to float: ' -  '



```python
nyc_sales['SALE PRICE']
```




    0        6625000
    1            -  
    2            -  
    3        3936272
    4        8000000
              ...   
    16904        -  
    16905     712500
    16906     740000
    16907    1800000
    16908        -  
    Name: SALE PRICE, Length: 16909, dtype: object



Não houve conversão, pois existe um ```-``` no meio das colunas. Isso é um indício de que valores faltosos são representados dessa maneira no nosso dataframe.

Vamos usar outro método: ```pd.to_nummeric```.


```python
nyc_sales['SALE PRICE'] = pd.to_numeric(nyc_sales['SALE PRICE'], errors='coerce')
```


```python
nyc_sales['SALE PRICE']
```




    0        6625000.0
    1              NaN
    2              NaN
    3        3936272.0
    4        8000000.0
               ...    
    16904          NaN
    16905     712500.0
    16906     740000.0
    16907    1800000.0
    16908          NaN
    Name: SALE PRICE, Length: 16909, dtype: float64




```python
# convertendo as demais colunas para float
for coluna in ['GROSS SQUARE FEET', 'LAND SQUARE FEET', 'APARTMENT NUMBER']:
    print(coluna)
    nyc_sales[coluna] = pd.to_numeric(nyc_sales[coluna], errors='coerce')
```

    GROSS SQUARE FEET
    LAND SQUARE FEET
    APARTMENT NUMBER
    


```python
def converter_float(coluna):
    coluna = pd.to_numeric(coluna, errors='coerce')
    return coluna
```


```python
nyc_sales.loc[:, ['GROSS SQUARE FEET', 'LAND SQUARE FEET', 'APARTMENT NUMBER']].apply(converter_float)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>GROSS SQUARE FEET</th>
      <th>LAND SQUARE FEET</th>
      <th>APARTMENT NUMBER</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6440.0</td>
      <td>1633.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18690.0</td>
      <td>4616.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7803.0</td>
      <td>2212.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6794.0</td>
      <td>2272.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4615.0</td>
      <td>2369.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>16904</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16905</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16906</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16907</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16908</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>16909 rows × 3 columns</p>
</div>



Perceba que agora aparecem valores Null no nosso dataframe. 

AS variáveis categóricas que estão como inteiro iremos converter para ```object```.

Para as variável BOROUGH, vamos seguir as recomendações da "documentação"



```python
nyc_sales['BOROUGH'] = nyc_sales['BOROUGH'].map({1:'Manhatan', 2:'Bronx', 3:'Brooklyn', 4:'Queens', 5:'Staten Island'})
```


```python
nyc_sales['BOROUGH']
```




    0        Manhatan
    1        Manhatan
    2        Manhatan
    3        Manhatan
    4        Manhatan
               ...   
    16904    Manhatan
    16905    Manhatan
    16906    Manhatan
    16907    Manhatan
    16908    Manhatan
    Name: BOROUGH, Length: 16909, dtype: object



Por fim, temos a coluna ```SALE DATE``` que é uma data, mas que está como string. Vamos convertê-la para data usando a função ```pd.to_datetime```


```python
nyc_sales['SALE DATE']
```




    0        2017-07-19 00:00:00
    1        2016-12-14 00:00:00
    2        2016-12-09 00:00:00
    3        2016-09-23 00:00:00
    4        2016-11-17 00:00:00
                    ...         
    16904    2017-02-08 00:00:00
    16905    2017-02-13 00:00:00
    16906    2017-02-13 00:00:00
    16907    2017-04-27 00:00:00
    16908    2017-05-04 00:00:00
    Name: SALE DATE, Length: 16909, dtype: object




```python
nyc_sales['SALE DATE'] = pd.to_datetime(nyc_sales['SALE DATE'], format='%Y-%m-%d')
```


```python
nyc_sales['SALE DATE']
```




    0       2017-07-19
    1       2016-12-14
    2       2016-12-09
    3       2016-09-23
    4       2016-11-17
               ...    
    16904   2017-02-08
    16905   2017-02-13
    16906   2017-02-13
    16907   2017-04-27
    16908   2017-05-04
    Name: SALE DATE, Length: 16909, dtype: datetime64[ns]



## Investigação de Inconsistências

* Datas fora do range 
* Valores zerados de feet² ou preço de venda
* Valores negativos de preço ou feet²


```python
# Checando  se há datas maiores que hoje
(nyc_sales['SALE DATE'] > '2020-09-25').any()
(nyc_sales['SALE DATE'] > '2020-09-25').sum()
```




    0



resultado_comparacao = nyc_sales['SALE DATE'] > '2020-09-25'
sum(resultado_comparacao==True)


```python
nyc_sales['SALE DATE']
```




    0       2017-07-19
    1       2016-12-14
    2       2016-12-09
    3       2016-09-23
    4       2016-11-17
               ...    
    16904   2017-02-08
    16905   2017-02-13
    16906   2017-02-13
    16907   2017-04-27
    16908   2017-05-04
    Name: SALE DATE, Length: 16909, dtype: datetime64[ns]




```python
# Outro modo
nyc_sales['SALE DATE'].max()
```




    Timestamp('2017-08-31 00:00:00')




```python
# Checando se há valor de venda menor ou igual a 0
(nyc_sales['SALE PRICE'] <= 0).sum()
```




    0




```python
# Checando se há área menor ou igual a 0
for coluna in ['SALE PRICE', 'LAND SQUARE FEET', 'GROSS SQUARE FEET']:
    if (nyc_sales[coluna] <= 0).sum() > 0:
        print(f'{coluna} tem registros inconsistentes')
    else:
        print(f'{coluna} está OK')
```

    SALE PRICE está OK
    LAND SQUARE FEET está OK
    GROSS SQUARE FEET está OK
    

## Dados Faltosos

### Origem

Os dados faltosos podem ter diversas origens:
* Erros de inserção (erro em banco)
* Erros humanos
* Erros de medição (sensores)
* Falhas em Equipamentos

Além disso, eles podem ser classificados da seguinte forma
* Missing Completely At Random (MCAR): Os dados não tem relação nenhuma com qualquer dado do dataset. Ou seja, a probabilidade do dado faltoso existir não está associada a ausência ou presença de uma outra informação ou do próprio valor da coluna.
* Missing At Random (MAR): A probabilide de um valor faltoso ocorrer está associada a outra informação do dataset.
* Missing Not at Random (MNAR):Os dados faltosos tem a ver com o próprio valor.

### Tratamento

Quando um dado faltoso surge por meio de um join numa tabela, quase sempre conseguimos preencher o dado faltoso com um valor que faz sentido.
Ex.: Saque de conta. Nem todos os clientes fazem saque em um mês. Dessa forma, podemos preencher o valor faltoso na coluna de saque para um cliente que não realizou saque como 0.

Quase nunca a opção de deletar tudo é a melhor, pois podem surgir alguns problemas, como:
* Introdução de Viés
* Perda de informação

Alguns métodos de tratamento de dados faltosos:
* Deleção
* Preenchimento com estatísticas (moda para categóricos, mediana ou média para numéricos)
* Preenchimento por meio de um modelo (KNN, Árvore de Decisão)
* Preenchimento com regra de negócio

Algumas boas práticas:
* Criar Null Categórico para a coluna (funciona para numéricas e categóricas)
* Não fazer dropna de cara.


```python
nyc_sales.shape[0]
```




    16909




```python
# Identificar null
100*(nyc_sales.isnull().sum()/nyc_sales.shape[0])
```




    Unnamed: 0                         0.000000
    BOROUGH                            0.000000
    NEIGHBORHOOD                       0.000000
    BUILDING CLASS CATEGORY            0.000000
    TAX CLASS AT PRESENT               0.000000
    BLOCK                              0.000000
    LOT                                0.000000
    EASE-MENT                          0.000000
    BUILDING CLASS AT PRESENT          0.000000
    ADDRESS                            0.000000
    APARTMENT NUMBER                  93.163404
    ZIP CODE                           0.000000
    RESIDENTIAL UNITS                  0.000000
    COMMERCIAL UNITS                   0.000000
    TOTAL UNITS                        0.000000
    LAND SQUARE FEET                  89.762848
    GROSS SQUARE FEET                 90.413389
    YEAR BUILT                         0.000000
    TAX CLASS AT TIME OF SALE          0.000000
    BUILDING CLASS AT TIME OF SALE     0.000000
    SALE PRICE                        20.781832
    SALE DATE                          0.000000
    dtype: float64



Opções de Tratamento:
* Deletar as colunas Land Square Feet, Gross Square Feet e Apartment Number e acrescentar uma coluna Null Categórica.
* Manter a coluna e categorizar (faixas de valores), acrescentando o Null Categórico 


```python
nyc_sales['IS_NULL_APARTMENT_NUMBER'] = False
```


```python
nyc_sales.loc[nyc_sales['APARTMENT NUMBER'].isnull(), 'IS_NULL_APARTMENT_NUMBER'] = True
```




    0        True
    1        True
    2        True
    3        True
    4        True
             ... 
    16904    True
    16905    True
    16906    True
    16907    True
    16908    True
    Name: IS_NULL_APARTMENT_NUMBER, Length: 15753, dtype: bool




```python
nyc_sales['IS_NULL_APARTMENT_NUMBER'].sum()/nyc_sales.shape[0]
```




    0.9316340410432314



Vamos criar uma coluna com faixa de valores e o null categórico, usando a função ```pd.cut```


```python
# Cut
nyc_sales['LAND SQUARE FEET FAIXA'] = pd.cut(nyc_sales['LAND SQUARE FEET'], bins=5)
nyc_sales['LAND SQUARE FEET FAIXA'] = nyc_sales['LAND SQUARE FEET FAIXA'].astype('object')
```


```python
%%time
nyc_sales['LAND SQUARE FEET FAIXA'].fillna('FAIXA DESCONHECIDA')
```

    CPU times: user 1.07 ms, sys: 31 µs, total: 1.1 ms
    Wall time: 1.09 ms
    




    0        (-275.912, 72870.4]
    1        (-275.912, 72870.4]
    2        (-275.912, 72870.4]
    3        (-275.912, 72870.4]
    4        (-275.912, 72870.4]
                    ...         
    16904     FAIXA DESCONHECIDA
    16905     FAIXA DESCONHECIDA
    16906     FAIXA DESCONHECIDA
    16907     FAIXA DESCONHECIDA
    16908     FAIXA DESCONHECIDA
    Name: LAND SQUARE FEET FAIXA, Length: 16909, dtype: object




```python
nyc_sales['GROSS SQUARE FEET FAIXA'] = pd.cut(nyc_sales['GROSS SQUARE FEET'], bins=[0, 100000, 500000, 7500000, np.inf])
```


```python
nyc_sales['GROSS SQUARE FEET FAIXA'] = nyc_sales['GROSS SQUARE FEET FAIXA'].astype('object')
nyc_sales['GROSS SQUARE FEET FAIXA'] = nyc_sales['GROSS SQUARE FEET FAIXA'].fillna('FAIXA DESCONHECIDA')
```


```python
nyc_sales['GROSS SQUARE FEET FAIXA']
```




    0           (0.0, 100000.0]
    1           (0.0, 100000.0]
    2           (0.0, 100000.0]
    3           (0.0, 100000.0]
    4           (0.0, 100000.0]
                    ...        
    16904    FAIXA DESCONHECIDA
    16905    FAIXA DESCONHECIDA
    16906    FAIXA DESCONHECIDA
    16907    FAIXA DESCONHECIDA
    16908    FAIXA DESCONHECIDA
    Name: GROSS SQUARE FEET FAIXA, Length: 16909, dtype: object




```python
df_random = pd.DataFrame(np.random.randint(0,100,size=(5,4)), columns=['A', 'B', 'C', 'D'])
```


```python
df_random
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>71</td>
      <td>94</td>
      <td>76</td>
    </tr>
    <tr>
      <th>1</th>
      <td>45</td>
      <td>93</td>
      <td>1</td>
      <td>42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>19</td>
      <td>80</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25</td>
      <td>67</td>
      <td>70</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50</td>
      <td>14</td>
      <td>51</td>
      <td>98</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_random.iloc[0:2, 2] = np.nan
```


```python
df_random.iloc[[1,2], 0] = np.nan
df_random.iloc[[2, 3], 3] = np.nan
```


```python
df_random
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15.0</td>
      <td>71</td>
      <td>NaN</td>
      <td>76.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>93</td>
      <td>NaN</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>19</td>
      <td>80.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25.0</td>
      <td>67</td>
      <td>70.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50.0</td>
      <td>14</td>
      <td>51.0</td>
      <td>98.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_random['A'].mean()
```




    30.0




```python
# Usar a média 
df_random['A'] = df_random['A'].fillna(df_random['A'].mean())
```


```python
df_random
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15.0</td>
      <td>71</td>
      <td>NaN</td>
      <td>76.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30.0</td>
      <td>93</td>
      <td>NaN</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30.0</td>
      <td>19</td>
      <td>80.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25.0</td>
      <td>67</td>
      <td>70.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50.0</td>
      <td>14</td>
      <td>51.0</td>
      <td>98.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Backward Fill
df_random['C'] = df_random['C'].fillna(method='bfill')
```


```python
df_random
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15.0</td>
      <td>71</td>
      <td>80.0</td>
      <td>76.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30.0</td>
      <td>93</td>
      <td>80.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30.0</td>
      <td>19</td>
      <td>80.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25.0</td>
      <td>67</td>
      <td>70.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50.0</td>
      <td>14</td>
      <td>51.0</td>
      <td>98.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Forward fill
df_random['D'] = df_random['D'].fillna(method='ffill')
```


```python
df_random
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15.0</td>
      <td>71</td>
      <td>80.0</td>
      <td>76.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30.0</td>
      <td>93</td>
      <td>80.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30.0</td>
      <td>19</td>
      <td>80.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>25.0</td>
      <td>67</td>
      <td>70.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50.0</td>
      <td>14</td>
      <td>51.0</td>
      <td>98.0</td>
    </tr>
  </tbody>
</table>
</div>



* Exercício: Faça uma função ou código para eliminar linhas do dataset que tenham um % de valores faltosos acima de um valor estipulado.

# Análise de Duplicatas

Duplicatas são registros iguais em sua totalidade ou em um conjunto específico de colunas. Além disso, também podemos ter colunas equivalentes e duplicadas em nosso dataset que não queremos manter, pois gera redundância. E num eventual processo de treinamento de modelo de machine learning isso pode levar ao overfitting ou a superestimação da  performance (no caso de linhas duplicadas)

Para remover duplicatas utilizamos a função ```drop_duplicates()```


```python

```


```python

```


```python

```

* Exercício: Como vocês fariam para achar duplicatas colunas iguais?

## Remoção de Colunas Constantes

Uma coluna constante não agrega em informação para nenhuma análise. Geralmente excluímos.


```python

```


```python

```

##  Remoção Por baixa variância



Variância é uma medida do quão espalhados estão os seus dados ao redor da média.


<img src="https://www.portalaction.com.br/sites/default/files/resize/EstatisticaBasica/figuras/ebe2.2-650x132.png" height=350 widht=150>


Os dados do conjunto 2 estão mais dispersos e, dessa maneira, dizemos que este possui maior variância em relação ao primeiro conjunto.<br>

Quando coletamos dados para utilizar em um modelo de machine learning, é interessante que eles possuam um nível mínimo de variância. Dessa forma, iremos filtrar colunas cuja variância seja muito próxima de zero (lembre-se de que já eliminamos aquelas que possuíam todos os elementos iguais).



O conjunto 1 abaixo possui um único valor diferente (6) que varia 1 unidade em relação aos demais. O conjunto 2, por sua vez, também possui apenas um valor diferente dos demais (600) e 100 unidades a mais que os demais. <br><br>Proporcionalmente, os dois conjuntos variam na mesma escala. Mas pelo cálculo da variância, elas são diferentes.
Vamos demonstrar como remover colunas (variáveis com baixa variância) do dataset.
Antes de tudo nós precisamos escalar nossos dados antes de calcular a variância.

<br>Vamos fazer isso utilizando o MinMaxScaler do Scikit-Learn.



<img src="https://media.geeksforgeeks.org/wp-content/uploads/min-max-normalisation.jpg" heigh=350 width=350>


```python

```


```python

```


```python

```


```python

```

**PROVOCAÇÃO**: Tentem construir funções para realizar cada uma das etapas pela quais passamos no processo de limpeza!
