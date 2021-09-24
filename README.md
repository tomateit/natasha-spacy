> #  🎉 Proposal <a href="https://github.com/explosion/spaCy/releases/tag/v3.0.0rc3">got accepted in v3.0.0rc3</a>. See https://nightly.spacy.io/models/ru for official Russian pretrained models.

# natasha-spacy

SpaCy official Russian model proposal. Work is heavily inspired and based on <a href="https://github.com/buriy/spacy-ru/">spacy-ru</a> by <a href="http://github.com/buriy/">@buriy</a>. 

Russian model is trained on two resources, both available under MIT license:
1. <a href="https://github.com/natasha/nerus">Nerus</a> — part of <a href="https://github.com/natasha">Natasha project</a>, large silver standard Russian corpus annotated with morphology tags, syntax trees and PER, LOC, ORG NER-tags.
2. <a href="https://github.com/natasha/navec">Navec</a> — also part of Natasha project, pretrained word embeddings for Russian language.

Code in this repo is also available under MIT license.

Resulting model is relatively small due to embeddings table pruning (138MB), works fast on CPU. Shows near SOTA performance on tasks of morphology tagging and syntax parsing, beating heavy DeepPavlov BERT on news and wiki domains. On NER task model shows quality comparable to other top Russian systems, beating DeepPavlov, PullEnti, Stanza. See Naeval <a href="https://github.com/natasha/naeval#morphology-taggers">morphology</a>, <a href="https://github.com/natasha/naeval#syntax-parser">syntax</a>, and <a href="https://github.com/natasha/naeval#ner">NER</a> sections.

## Download

<table>
<tr>
<th>
Model
</th>
<th>
Size
</th>
<th>
SpaCy version
</th>
</tr>
<tr>
<td>
<a href="https://storage.yandexcloud.net/natasha-spacy/models/ru_core_news_md-2.3.0.tar.gz">ru_core_news_md-2.3.0.tar.gz</a>
</td>
<td>
138MB
</td>
<td>
2.3.* 
</td>
</tr>
<tr>
<td>
<a href="https://storage.yandexcloud.net/natasha-spacy/models/ru_core_news_md-3.0.0.tar.gz">ru_core_news_md-3.0.0.tar.gz</a>
</td>
<td>
135MB
</td>
<td>
3.0.*
</td>
</tr>

</table>


## Usage 

First download and install the model. SpaCy 2.3.* is required, model won't work with SpaCy 2.1, 2.2.
```bash
wget https://storage.yandexcloud.net/natasha-spacy/models/ru_core_news_md-2.3.0.tar.gz
pip install ru_core_news_md-2.3.0.tar.gz
```

Model for SpaCy 3.0 is also available.
```
wget https://storage.yandexcloud.net/natasha-spacy/models/ru_core_news_md-3.0.0.tar.gz
pip install ru_core_news_md-3.0.0.tar.gz
```

Use <a href="https://github.com/natasha/ipymarkup">ipymarkup</a> for NER and syntax visualization.
```python
>>> import spacy
>>> from ipymarkup import show_dep_ascii_markup, show_span_ascii_markup

>>> nlp = spacy.load('ru_core_news_md')
>>> text = 'Посол Израиля на Украине Йоэль Лион признался, что пришел в шок, узнав о решении властей Львовской области объявить 2019 год годом лидера запрещенной в России Организации украинских националистов (ОУН) Степана Бандеры. Свое заявление он разместил в Twitter. «Я не могу понять, как прославление тех, кто непосредственно принимал участие в ужасных антисемитских преступлениях, помогает бороться с антисемитизмом и ксенофобией. Украина не должна забывать о преступлениях, совершенных против украинских евреев, и никоим образом не отмечать их через почитание их исполнителей», — написал дипломат. 11 декабря Львовский областной совет принял решение провозгласить 2019 год в регионе годом Степана Бандеры в связи с празднованием 110-летия со дня рождения лидера ОУН (Бандера родился 1 января 1909 года). В июле аналогичное решение принял Житомирский областной совет. В начале месяца с предложением к президенту страны Петру Порошенко вернуть Бандере звание Героя Украины обратились депутаты Верховной Рады. Парламентарии уверены, что признание Бандеры национальным героем поможет в борьбе с подрывной деятельностью против Украины в информационном поле, а также остановит «распространение мифов, созданных российской пропагандой». Степан Бандера (1909-1959) был одним из лидеров Организации украинских националистов, выступающей за создание независимого государства на территориях с украиноязычным населением. В 2010 году в период президентства Виктора Ющенко Бандера был посмертно признан Героем Украины, однако впоследствии это решение было отменено судом. '
>>> doc = nlp(text)


#######
#
#   NER
#
#######


>>> spans = [
...     (_.start_char, _.end_char, _.label_)
...     for _ in doc.ents
... ]
>>> show_span_ascii_markup(doc.text, spans)
Посол Израиля на Украине Йоэль Лион признался, что пришел в шок, узнав
      LOC────    LOC──── PER───────                                   
 о решении властей Львовской области объявить 2019 год годом лидера 
                   LOC──────────────                                
запрещенной в России Организации украинских националистов (ОУН) 
              LOC─── ORG─────────────────────────────────────── 
Степана Бандеры. Свое заявление он разместил в Twitter. «Я не могу 
PER────────────                                ORG────             
понять, как прославление тех, кто непосредственно принимал участие в 
ужасных антисемитских преступлениях, помогает бороться с 
антисемитизмом и ксенофобией. Украина не должна забывать о 
                              LOC────                      
преступлениях, совершенных против украинских евреев, и никоим образом 
не отмечать их через почитание их исполнителей», — написал дипломат. 
11 декабря Львовский областной совет принял решение провозгласить 2019
           ORG──────────────────────                                  
 год в регионе годом Степана Бандеры в связи с празднованием 110-летия
                     PER────────────                                  
 со дня рождения лидера ОУН (Бандера родился 1 января 1909 года). В 
                        ORG  PER────                                
июле аналогичное решение принял Житомирский областной совет. В начале 
                                ORG────────────────────────           
месяца с предложением к президенту страны Петру Порошенко вернуть 
                                          PER────────────         
Бандере звание Героя Украины обратились депутаты Верховной Рады. 
PER────              LOC────                     ORG───────────  
Парламентарии уверены, что признание Бандеры национальным героем 
                                     PER────                     
поможет в борьбе с подрывной деятельностью против Украины в 
                                                  LOC────   
информационном поле, а также остановит «распространение мифов, 
созданных российской пропагандой». Степан Бандера (1909-1959) был 
                                   PER───────────                 
одним из лидеров Организации украинских националистов, выступающей за 
                 ORG─────────────────────────────────                 
создание независимого государства на территориях с украиноязычным 
населением. В 2010 году в период президентства Виктора Ющенко Бандера 
                                               PER─────────── PER──── 
был посмертно признан Героем Украины, однако впоследствии это решение 
                             LOC────                                  
было отменено судом. 


#######
#
#   MORPH
#
#######


>>> sent = next(doc.sents)
>>> for token in sent:
...     print(token.text.ljust(10), token.lemma_.ljust(10), token.tag_)
Посол      посол      NOUN__Animacy=Anim|Case=Nom|Gender=Masc|Number=Sing
Израиля    израиль    PROPN__Animacy=Inan|Case=Gen|Gender=Masc|Number=Sing
на         на         ADP___
Украине    украина    PROPN__Animacy=Inan|Case=Loc|Gender=Fem|Number=Sing
Йоэль      йоэль      PROPN__Animacy=Anim|Case=Nom|Gender=Masc|Number=Sing
Лион       лион       PROPN__Animacy=Anim|Case=Nom|Gender=Masc|Number=Sing
признался  признаться VERB__Aspect=Perf|Gender=Masc|Mood=Ind|Number=Sing|Tense=Past|VerbForm=Fin|Voice=Mid
,          ,          PUNCT___
что        что        SCONJ___
пришел     прийти     VERB__Aspect=Perf|Gender=Masc|Mood=Ind|Number=Sing|Tense=Past|VerbForm=Fin|Voice=Act
в          в          ADP___
шок        шок        NOUN__Animacy=Inan|Case=Acc|Gender=Masc|Number=Sing
,          ,          PUNCT___
узнав      узнать     VERB__Aspect=Perf|Tense=Past|VerbForm=Conv|Voice=Act
о          о          ADP___
решении    решение    NOUN__Animacy=Inan|Case=Loc|Gender=Neut|Number=Sing
властей    власть     NOUN__Animacy=Inan|Case=Gen|Gender=Fem|Number=Plur
Львовской  львовский  ADJ__Case=Gen|Degree=Pos|Gender=Fem|Number=Sing
области    область    NOUN__Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
объявить   объявить   VERB__Aspect=Perf|VerbForm=Inf|Voice=Act
2019       2019       ADJ___
год        год        NOUN__Animacy=Inan|Case=Acc|Gender=Masc|Number=Sing
годом      год        NOUN__Animacy=Inan|Case=Ins|Gender=Masc|Number=Sing
лидера     лидер      NOUN__Animacy=Anim|Case=Gen|Gender=Masc|Number=Sing
запрещенной запретить  VERB__Aspect=Perf|Case=Gen|Gender=Fem|Number=Sing|Tense=Past|VerbForm=Part|Voice=Pass
в          в          ADP___
России     россия     PROPN__Animacy=Inan|Case=Loc|Gender=Fem|Number=Sing
Организации организация PROPN__Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
украинских украинских ADJ__Case=Gen|Degree=Pos|Number=Plur
националистов националист NOUN__Animacy=Anim|Case=Gen|Gender=Masc|Number=Plur
(          (          PUNCT___
ОУН        оун        PROPN__Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
)          )          PUNCT___
Степана    степан     PROPN__Animacy=Anim|Case=Gen|Gender=Masc|Number=Sing
Бандеры    бандеры    PROPN__Animacy=Anim|Case=Gen|Gender=Masc|Number=Sing
.          .          PUNCT___


########
#
#  SYNTAX
#
######


>>> words = [_.text for _ in sent]
>>> deps = [
...     (_.head.i, _.i, _.dep_)
...     for _ in sent
...     if _.i != _.head.i
... ]
>>> show_dep_ascii_markup(words, deps)
    ┌►┌─┌─┌─ Посол         nsubj
    │ │ │ └► Израиля       nmod
    │ │ │ ┌► на            case
    │ │ └►└─ Украине       nmod
    │ └──►┌─ Йоэль         appos
    │     └► Лион          flat:name
┌───└─┌───── признался     
│     │ ┌──► ,             punct
│     │ │ ┌► что           mark
│   ┌─└►└─└─ пришел        ccomp
│   │ │   ┌► в             case
│   │ └──►└─ шок           obl
│   │     ┌► ,             punct
│   └──►┌─└─ узнав         advcl
│       │ ┌► о             case
│   ┌───└►└─ решении       obl
│   │ ┌─└──► властей       nmod
│   │ │   ┌► Львовской     amod
│   │ └──►└─ области       nmod
│   └►┌─┌─── объявить      nmod
│     │ │ ┌► 2019          amod
│     │ └►└─ год           obj
│     └──►┌─ годом         obl
│ ┌─┌─────└► лидера        nmod
│ │ │ ┌►┌─── запрещенной   amod
│ │ │ │ │ ┌► в             case
│ │ │ │ └►└─ России        obl
│ │ └►└─┌─── Организации   nmod
│ │     │ ┌► украинских    amod
│ │   ┌─└►└─ националистов nmod
│ │   │   ┌► (             punct
│ │   └►┌─└─ ОУН           parataxis
│ │     └──► )             punct
│ └──────►┌─ Степана       appos
│         └► Бандеры       flat:name
└──────────► .             punct
```

## Training

### v2

Both <a href="https://github.com/natasha/nerus">Nerus</a> and <a href="https://github.com/natasha/navec">Navec</a> are adapted to fit SpaCy utilities. Training procedure uses only standart `spacy convert`, `spacy init-model`, `spacy train`.

Version 2 uses `data` directory for downloaded data

Initialize the environment. We use SpaCy 2.3 for training, Russian language in SpaCy requires PyMorphy for morphology.
```bash
pip install spacy==2.3.5 pymorphy2==0.8
mkdir -p data train/data train/base train/model
```

Download 650MB embeddings table. Navec is precomputed on fiction texts, has 500 000 words in vocabulary.
```bash
wget https://storage.yandexcloud.net/natasha-spacy/data/navec.12B.300d.txt.gz -P data
```

Download 1.5GB training data. We use 10% slice of original Nerus, it contains 100 000 documents, 1 000 000 sentences.
```bash
wget https://storage.yandexcloud.net/natasha-spacy/data/nerus-dev.conllu.gz -P data
wget https://storage.yandexcloud.net/natasha-spacy/data/nerus-train.conllu.gz -P data
gunzip data/nerus-*.conllu.gz
```


WARNING! Conversion requires 32GB of RAM, resulting in JSON that is 4.5GB in size. 
```bash
# --n-sents explanation: on average there are 10 sentences per document in Nerus
spacy convert --n-sents 10 --morphology data/nerus-train.conllu train/data
spacy convert --n-sents 10 --morphology data/nerus-dev.conllu train/data
```

Original Navec embeddings have 500 000 words in vocabulary. Pruning to 125 000 words we lose just 0.5 percentage points in accuracy.
```bash
spacy init-model ru train/base --vectors-loc data/navec.12B.300d.txt.gz --prune-vectors 125000
```

Training takes ~2 hours per epoch on CPU (~5 times faster on GPU).
```bash
spacy train --base-model train/base --n-iter 10 ru train/model train/data/nerus-train.json train/data/nerus-dev.json
```
```
Itn  Tag Loss    Tag %    Dep Loss    UAS     LAS    NER Loss   NER P   NER R   NER F   Token %  CPU WPS
---  ---------  --------  ---------  ------  ------  ---------  ------  ------  ------  -------  -------
  1  1612372.812    96.587  3097137.617  96.141  94.451  293158.500  93.775  94.075  93.925  100.000     4881
  2  1141301.281    96.991  2201299.666  96.538  95.057  194409.327  94.119  94.816  94.466  100.000     4743
  3  1036780.770    97.188  2005859.872  96.802  95.374  169705.725  94.431  95.051  94.740  100.000     4662
  4  979072.228    97.313  1896801.782  96.872  95.548  157141.959  94.716  95.337  95.026  100.000     4622
  5  940867.435    97.354  1826734.471  97.006  95.722  147808.848  94.628  95.472  95.048  100.000     4794
  6  913604.756    97.414  1772406.880  97.058  95.793  142175.940  94.733  95.371  95.051  100.000     4710
  7  891458.779    97.474  1734561.844  97.150  95.882  137502.791  94.628  95.472  95.048  100.000     4813
  8  873880.652    97.516  1704919.560  97.171  95.932  134417.454  95.035  95.691  95.362  100.000     4869
  9  860200.442    97.532  1681551.213  97.214  95.989  131294.160  95.188  95.556  95.372  100.000     4932
 10  848243.402    97.581  1656692.182  97.264  96.035  127838.633  94.981  95.556  95.268  100.000     4395
```

### v3

We use <a href="https://nightly.spacy.io/usage/projects">SpaCy projects</a>, training procedure is described in <a href="https://github.com/natasha/natasha-spacy/blob/master/project/project.yml">project/project.yml</a>.
Version 3 uses `assets` for downloaded data


Download, uncompress embeddings table and training data.
```bash
spacy project assets
spacy project run extract
```

Convert training data for SpaCy binary format. WARNING! 32 GB of RAM is required.
```bash
spacy project run corpus
```

Convert and prune embeddings table (for navec vectors).
```bash
spacy project run vectors
```

### Training model with static vectors
~3 hours per epoch on CPU, requires ~24 GB of RAM.
```bash
spacy project run train
```
```
E    #       LOSS TOK2VEC  LOSS TAGGER  LOSS PARSER  LOSS NER  TAG_ACC  DEP_UAS  DEP_LAS  SENTS_F  ENTS_F  ENTS_P  ENTS_R  SCORE
---  ------  ------------  -----------  -----------  --------  -------  -------  -------  -------  ------  ------  ------  ------
  0       0          0.00       198.77       325.44     95.21    14.05    20.94     6.86     0.00    0.54    4.42    0.29    0.10
  0   10000    1176499.70    694829.12   1066434.84  124868.43    94.87    94.41    92.26    99.62   90.03   89.68   90.39    0.93
  0   20000    2768135.49    710927.47   1157628.86  133416.70    95.90    95.36    93.50    99.71   92.12   91.54   92.71    0.94
  1   30000    4134293.51    609565.16   1005064.56  116021.79    96.34    95.80    94.18    99.71   92.54   91.18   93.94    0.95
  1   40000    5612606.23    570474.00    943323.94  108390.26    96.55    95.79    94.19    99.77   92.98   92.75   93.22    0.95
  2   50000    7069821.13    536273.18    895354.35  102616.23    96.68    95.94    94.42    99.71   93.06   92.50   93.62    0.95
  2   60000    8420589.99    521484.19    868375.86  100185.40    96.75    96.08    94.56    99.78   93.31   92.91   93.72    0.95
  3   70000    9584882.66    503431.96    841835.52  97766.84    96.82    96.14    94.70    99.81   93.81   93.25   94.38    0.95
  3   80000   10693665.22    497292.09    831536.94  96698.17    96.85    96.22    94.81    99.83   93.56   93.55   93.57    0.95
  4   90000   11533142.84    484925.86    808917.28  93526.94    96.95    96.32    94.87    99.75   93.58   93.76   93.40    0.95
  4  100000   12442474.92    482277.28    804398.14  93961.81    96.93    96.31    94.87    99.78   93.53   93.37   93.69    0.95
  5  110000   13037916.06    471829.23    788692.77  91600.88    96.96    96.30    94.87    99.78   93.67   93.32   94.02    0.95
  5  120000   13616621.93    470462.12    785364.54  91812.84    97.02    96.28    94.93    99.72   93.43   92.67   94.21    0.95
  6  130000   14109942.25    464641.05    776902.13  90060.62    96.97    96.27    94.91    99.56   93.99   93.30   94.70    0.96
  6  140000   14587665.69    465145.74    777173.79  90970.47    97.09    96.52    95.15    99.74   93.81   93.18   94.45    0.96
```

### Training model with transformer
~12 hours per epoch on CPU, requires ~ 18 GB of RAM.
```bash
spacy project run train_trf
```
```
E    #       LOSS TRANS...  LOSS TAGGER  LOSS PARSER  LOSS NER  TAG_ACC  DEP_UAS  DEP_LAS  SENTS_F  ENTS_F  ENTS_P  ENTS_R  SCORE 
---  ------  -------------  -----------  -----------  --------  -------  -------  -------  -------  ------  ------  ------  ------
  0       0           0.00       198.77       367.62     92.71    37.00    20.40     5.96     0.42    0.00    0.00    0.00    0.17
  0    2000      237298.63    112123.93    166346.63  22479.29    85.57    86.37    81.42    97.63   68.68   69.19   68.19    0.79
  0    4000      193068.16     75679.09    118518.96  14318.26    88.45    88.95    84.70    98.01   75.30   75.61   74.99    0.84
  0    6000      647685.62    177915.53    306052.26  33282.77    93.36    93.17    90.44    99.32   86.06   86.29   85.83    0.90
  0    8000      940762.17    191434.10    364609.77  35919.41    94.57    94.13    91.82    99.16   88.62   89.11   88.13    0.92
  0   10000     1072413.91    165304.60    333995.27  31946.06    95.10    94.47    92.33    99.41   89.29   89.40   89.18    0.93
  0   12000     1234972.01    152512.71    319579.24  29568.94    95.45    94.95    92.98    99.51   89.94   88.78   91.13    0.93
  0   14000     1405387.85    142933.20    307429.13  27691.42    95.77    95.10    93.22    99.40   90.14   89.30   90.99    0.93
  0   16000     1580512.04    136893.26    298776.20  26318.25    95.88    94.82    93.03    99.62   90.97   89.59   92.39    0.94
  0   18000     1771592.29    132091.50    290075.84  25421.61    96.09    95.27    93.46    99.55   90.50   91.57   89.45    0.94
  0   20000     1961068.18    128026.00    283425.95  24633.05    96.24    95.41    93.71    99.48   91.52   92.03   91.01    0.94
  0   22000     2162061.53    124545.96    278546.28  24351.04    96.35    95.49    93.77    99.66   91.75   92.01   91.50    0.94
  1   24000     2367755.80    121997.59    275034.40  23751.87    96.44    95.62    93.88    99.49   91.57   90.55   92.61    0.94
  1   26000     2521884.90    115304.51    270248.87  22164.53    96.32    95.70    94.10    99.56   91.76   90.84   92.69    0.94
  1   28000     2768747.92    113792.86    267512.70  22401.25    96.52    95.71    94.09    99.61   91.18   90.00   92.39    0.94
  1   30000     2996752.16    113350.12    265288.29  22703.73    96.54    95.76    94.14    99.63   91.25   90.27   92.26    0.94
  1   32000     3196385.79    112571.76    262780.27  22514.28    96.57    95.93    94.34    99.69   92.18   91.20   93.18    0.95
  1   34000     3347405.30    111535.32    260068.26  21424.42    96.64    95.90    94.31    99.69   91.88   90.42   93.38    0.95
  1   36000     3525068.79    109889.49    258312.47  21504.05    96.76    96.04    94.55    99.77   92.27   91.58   92.96    0.95
  1   38000     3765247.17    109774.41    260154.60  21237.02    96.77    96.08    94.56    99.63   92.20   92.61   91.79    0.95
  1   40000     3915477.45    107658.28    256863.55  21175.83    96.72    96.05    94.48    99.74   92.33   92.85   91.82    0.95
  1   42000     4092806.17    107243.59    256006.52  21246.33    96.98    96.10    94.64    99.66   92.89   93.23   92.54    0.95
```

## Package

Update `model-best/meta.json` with description, authors, sources. On model name `core_news_md`:
- `core` — provides all three components: tagger, parser and ner;
- `news` — trained on Nerus that is large automatically annotated news corpus;
- `md` — in SpaCy small models are 10-50MB in size, `md` - 50-200MB, `lg` - 200-600MB, out model is ~140MB.

### v2
Unlike v.3, v.2 uses `train` folder for model output instead of `training`.

```javascript
{
  ...
  "name": "core_news_md",
  "lang": "ru",
  "version": "2.3.0",
  "spacy_version": ">=2.3.0,<2.4.0",
  "description": "Russian multitask CNN initialized with Navec embeddings trained on Nerus dataset. Assigns context-specific token vectors, POS tags, dependency parse and named entities.",
  "author": "Yuri Baburov, Alexander Kukushkin",
  "email": "burchik@gmail.com, alex@alexkuk.ru",
  "url": "https://github.com/natasha/natasha-spacy",
  "license": "MIT",
  "sources": [
    {
      "name": "Nerus",
      "url": "https://github.com/natasha/nerus"
    },
    {
      "name": "Navec",
      "url": "https://github.com/natasha/navec"
    }
  ]
}
```

Use `spacy package` and `python sdist` to produce tar.gz archive.
```bash
mkdir package
spacy package train/model package
(cd package/* && python setup.py sdist)
mv package/*/dist/*.tar.gz .
rm -r package
```

### v3

Change versions, rest is the same as in v2.
```javascript
{
  ...
  "version": "3.0.0",
  "spacy_version":">=3.0.0rc2,<3.1.0",
  ...
}
```

Use SpaCy projects to build package, config is in <a href="https://github.com/natasha/natasha-spacy/blob/master/project/project.yml">project/project.yml</a>.

```bash
spacy project run package
```

### v3 transformer
```bash
spacy project run package_trf
```

## History

- 2020-12-24 <a href="https://github.com/explosion/spaCy/discussions/6628">SpaCy discussion #6628 "Russian model proposal"</a>
- 2021-01-08 <a href="https://github.com/explosion/spaCy/discussions/6628#discussioncomment-268842">Support SpaCy v3</a>
- 2021-01-19 <a href="https://github.com/explosion/spaCy/releases/tag/v3.0.0rc3">Proposal accepted in v3.0.0rc3</a>
