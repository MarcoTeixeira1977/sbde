# 2.1 Sistema de Banco de Dados Espaciais

Um exemplo de Sistema de Banco de Dados Espaciais � o Sistema Gerenciador de Banco de Dados Objeto-Relacional distribu�do [PostgreSQL](http://www.postgresql.org/) e a extens�o espacial [PostGIS](https://postgis.net/).

Assim como o PostGIS, o Oracle Spatial � a extens�o espacial para o Sistema Gerenciador de Banco de Dados Objeto-Relacional Oracle. Mas o que isso significa? O que torna um sistema de banco de dados convencionais em um sistema de banco de dados espaciais?

A resposta curta, �...

Os sistemas de bancos de dados espaciais armazenam e manipulam objetos espaciais como qualquer outro objeto no sistema de banco de dados.

A seguir, abordaremos brevemente a evolu��o dos sistema de dados espaciais e, em seguida, analisaremos tr�s aspectos que associam dados espaciais a um sistema de banco de dados - tipos de dados, �ndices e fun��es.

Os tipos de dados espaciais referem-se aos formatos vetorial e raster. Ponto, Linha e Pol�gono s�o exemplos de dados  espaciais vetoriais. Uma imagem de sat�lite ou um modelo digital de eleva��o s�o exemplos de dados espaciais do tipo raster, chamados tamb�m de dados matriciais.

A indexa��o espacial multidimensional � utilizada para processamento eficiente de opera��es espaciais.

As fun��es espaciais, utilizadas em SQL, s�o para consultas de propriedades geom�tricas e relacionamentos espaciais.

Combinados, tipos de dados espaciais, �ndices espaciais e fun��es espaciais, fornecem uma estrutura flex�vel para desempenho e an�lise espaciais otimizadas, a partir de um grande volume de dados ou por meio de consultas complexas.

Nas implementa��es de Sistemas de Informa��es Geogr�ficas (SIG) de primeira gera��o, todos os dados espaciais s�o armazenados em arquivos planos e � necess�rio um software SIG especial para interpretar e manipular os dados. Esses sistemas de gerenciamento de primeira gera��o s�o projetados para atender �s necessidades dos usu�rios, onde todos os dados necess�rios est�o dentro do dom�nio organizacional. Eles s�o sistemas propriet�rios e aut�nomos constru�dos especificamente para manipula��o de dados espaciais.

Os sistemas espaciais de segunda gera��o armazenam alguns dados em bancos de dados relacionais (geralmente o "atributo" ou partes n�o espaciais), mas ainda n�o possuem a flexibilidade oferecida com a integra��o direta.

Os verdadeiros sistemas de bancos de dados espaciais nasceram quando as pessoas come�aram a tratar os recursos espaciais como objetos de banco de dados de primeira classe.

Os sistemas de banco de dados espaciais integram dados espaciais em um sistema de banco de dados objeto-relacional. O foco das aplica��es muda do SIG para o Sistema de Banco de Dados Espaciais.

![](https://github.com/deamorim2/sbde/blob/master/wiki/02/beginning.png)

Nota:
No Brasil, o termo Sistema de Banco de Dados Geogr�ficos � bem mais difundido que o termo Sistema de Banco de Dados Espaciais. Por�m, os Sistemas de Banco de Dados Espaciais podem ser utilizado em aplica��es al�m do mundo geogr�fico. Os bancos de dados espaciais podem ser usados para gerenciar dados relacionados � anatomia do corpo humano, circuitos integrados de grande escala, estruturas moleculares, campos eletromagn�ticos, entre outros.

## 2.1.1 Tipos de dados espaciais

Um banco de dados convencionais possui dados b�sicos como dos tipos texto, n�mero e data. Um banco de dados espacial adiciona tipos adicionais (espaciais) para representar recursos geogr�ficos. Esses tipos de dados espaciais abstraem e encapsulam estruturas espaciais, como limites e dimens�es. Em muitos aspectos, os tipos de dados espaciais podem ser entendidos simplesmente como formas.

![](https://github.com/deamorim2/sbde/blob/master/wiki/02/hierarchy.png)

Os tipos de dados espaciais s�o organizados em uma hierarquia de tipos. Cada sub-tipo herda a estrutura (atributos) e o comportamento (m�todos ou fun��es) do seu super-tipo.

## 2.1.2 �ndices espaciais e Ret�ngulo Envolvente M�nimo R.E.M (Bounding Boxes)

Um banco de dados comum fornece "m�todos de acesso" (comumente conhecidos como �ndices) para permitir acesso r�pido e aleat�rio a subconjuntos de dados. A indexa��o de tipos padr�o (n�meros, textos, datas) geralmente � feita com �ndices de �rvore bin�ria. Uma �rvore bin�ria divide os dados usando a ordem de classifica��o natural para colocar os dados em uma �rvore hier�rquica.

A ordem de classifica��o natural de n�meros, textos e datas � simples de determinar, onde cada valor � menor do que, maior ou igual a qualquer outro valor. Mas, como os pol�gonos podem se sobrepor, podem estar contidos um em rela��o ao outro e est�o dispostos em um espa�o bidimensional (ou mais), uma �rvore bin�ria n�o pode ser usada para index�-los eficientemente. Os bancos de dados espaciais fornecem um "�ndice espacial" que, em vez disso, responde a seguinte pergunta: "quais objetos est�o dentro deste ret�ngulo envolvente m�nimo em particular?".

Um Ret�ngulo Envolvente M�nimo � o menor ret�ngulo, que esteja paralelo aos eixos de coordenadas, capaz de conter uma determinada caracter�stica.

![](https://github.com/deamorim2/sbde/blob/master/wiki/02/boundingbox.png)

Ret�ngulos Envolventes Minimos s�o usados porque respondem facilmente a pergunta "est� A dentro de B?". Caso essa pergunta seja feita a pol�gonos irregulares com muitos v�rtices, este tipo de processamento computacional pode demorar muito se comparado a pol�gonos regulares, como � o caso dos ret�ngulos envolventes m�nimos. At� mesmo os pol�gonos e as linhas mais complexas podem ser representados por um simples ret�ngulo envolvente m�nimo.

A principal utilidade dos �ndices � sua resposta r�pida. Por isso, ao inv�s de fornecerem resultados exatos, como fazem as �rvores bin�rias, os �ndices espaciais fornecem resultados aproximados. A pergunta "quais linhas est�o dentro desse pol�gono?" deve ser interpretada a partir de �ndice espacial como "quais linhas possuem caixas de delimita��o contidas dentro da caixa delimitadora deste pol�gono?"

Existem v�rios tipos de �ndices espaciais implementados em sistema de bancos de dados espaciais. A implementa��o mais comum � a �rvore-R ([R-Tree](http://en.wikipedia.org/wiki/R-tree)), usado no PostGIS, mas tamb�m h� [Quadtrees](http://en.wikipedia.org/wiki/Quadtree), �rvore-k-d ([k-d-trees](https://en.wikipedia.org/wiki/K-d_tree)), [Grid Files](http://en.wikipedia.org/wiki/Grid_(spatial_index)), entre outros.

## 2.1.3 Fun��es Espaciais

Para manipular dados durante uma consulta, um banco de dados comum fornece fun��es como concatenar texto, fazer c�lculos e extrair informa��es de datas. Um banco de dados espacial fornece um conjunto completo de fun��es para an�lise de componentes geom�tricos, determina��o de rela��es espaciais e manipula��o de geometrias. Essas fun��es espaciais servem de base para qualquer projeto espacial.

A maioria das fun��es espaciais podem ser agrupadas em uma das seguintes categorias:

* Convers�o: fun��es de convers�o entre geometrias e formatos de dados externos

* Gerenciamento: fun��es que gerenciam informa��es sobre tabelas espaciais e administra��o dos dados em PostGIS

* Recupera��o: Fun��es que recuperam propriedades e medidas de uma Geometria

* Compara��o: Fun��es que comparam duas geometrias em rela��o � sua rela��o espacial

* Gera��o: fun��es que geram novas geometrias a partir de outros formatos de dados

A lista de fun��es espaciais � bem ampla. As mais comumente implementadas s�o definida pela Open Geospatial Consortium ([OGC](http://www.opengeospatial.org/)) a partir da especifica��o "OpenGIS Implementation Specification for Geographic information-Simple feature access" ([SFSQL](http://www.opengeospatial.org/standards/sfa)) ou pela ISO a partir da "ISO/IEC 13249-3:2016 Part 3: Spatial" ([SQLMM](https://www.iso.org/standard/60343.html)). Mas nada impede que os softwares de sistema de banco de dados espaciais adotem as suas pr�pras fun��es espaciais. No caso do PostGIS, ele possui v�rias fun��es espaciais implementadas pela OGC/ISO, bem como fun��es espaciais pr�prias.

# 2.2 O que � o PostGIS?

O [PostGIS](https://postgis.net/) transforma o Sistema de Gerenciamento de Banco de Dados [PostgreSQL](http://www.postgresql.org/) em um banco de dados espaciais, adicionando suporte para os tr�s recursos: tipos espaciais, �ndices espaciais e fun��es espaciais. Como ele � criado no PostgreSQL, o PostGIS herda automaticamente recursos importantes de sistema de banco de dados, bem como utiliza padr�es abertos (SQL98, SFSQL/SQLMM) em sua implementa��o.

## 2.2.1 Mas o que � PostgreSQL?

O PostgreSQL � um poderoso sistema de gerenciamento de banco de dados objeto-relacional (ORDBMS). � lan�ado sob uma licen�a de estilo BSD e, portanto, � um software livre e de c�digo aberto. Tal como acontece com muitos outros programas de c�digo aberto, o PostgreSQL n�o � controlado por nenhuma empresa, mas tem uma comunidade global de desenvolvedores e empresas para desenvolv�-lo.

Desde o in�cio, o PostgreSQL foi projetado para trabalhar com extens�o, com capacidade de adicionar novos tipos de dados, fun��es e m�todos de acesso em tempo de execu��o. Por isso, a extens�o PostGIS pode ser desenvolvida por uma equipe de desenvolvimento separada, mas ainda assim continua firmemente integrada ao sistema de banco de dados do PostgreSQL.

### 2.2.1.1 Por que escolher o PostgreSQL?

Uma quest�o comum de pessoas familiarizadas com bancos de dados de c�digo aberto � "Por que o PostGIS n�o foi constru�do no MySQL?".

O PostgreSQL possui:

* Confiabilidade comprovada e integridade transacional por padr�o (ACID)

* Suporte a padr�es SQL (SQL92 completo)

* Tipos de Extens�o e fun��es de extens�o plug�veis

* Modelo de desenvolvimento orientado para a comunidade

* Sem limites nos tamanhos das colunas para suportar grandes objetos geom�tricos

* Estrutura gen�rica do �ndice (GiST) para permitir o �ndice R-Tree

* F�cil implementa��o de fun��es personalizadas

Combinado, o PostgreSQL fornece um caminho de desenvolvimento muito f�cil para adicionar novos tipos espaciais. No mundo propriet�rio, apenas o Illustra (agora o Informix Universal Server) permite uma extens�o t�o f�cil. Isso n�o � coincid�ncia, Illustra � um software propriet�rio que aproveitou a base de c�digo original do PostgreSQL da d�cada de 1980.

Como o desenvolvimento para adicionar tipos ao PostgreSQL era t�o direto, fazia sentido come�ar l�. Quando o MySQL lan�ou tipos espaciais b�sicos na vers�o 4.1, a equipe PostGIS examinou seu c�digo e esse exerc�cio refor�ou a decis�o original de usar o PostgreSQL. Como os objetos espaciais MySQL tiveram que ser constru�dos sobre tipos do tipo texto como um caso especial, o c�digo MySQL foi espalhado por todo o c�digo base. O desenvolvimento do PostGIS 0.1 levou menos de um m�s. Fazer um "MyGIS" 0.1 teria demorado muito e, como tal, talvez nunca tivesse visto a luz do dia.

### 2.2.1.2 Por que n�o usar Shapefiles?

O shapefile (e outros formatos de arquivo) tem sido a maneira padr�o de armazenar e interagir com dados espaciais desde que o primeiro software de SIG foi escrito. No entanto, esses arquivos de sistema apresentam as seguintes desvantagens:

* **Os arquivos requerem software especial para ler e escrever**: O SQL � uma abstra��o para acesso e an�lise de dados aleat�rios. Sem essa abstra��o, voc� precisar� escrever todo o c�digo de acesso e an�lise voc� mesmo.

* **Usu�rios simult�neos podem causar corrup��o**: Embora seja poss�vel escrever c�digo extra para garantir que m�ltiplas escritas no mesmo arquivo n�o corrompam os dados, voc� ainda ter� que resolver o problema de desempenho e ter� que escrever o melhor c�digo de um sistema de banco de dados. Por que n�o usar apenas um banco de dados padr�o?

* **Perguntas complicadas requerem um software complicado para responder**: Perguntas complexas, como associa��es ou agrega��es espaciais, que s�o realizadas ??em uma linha de SQL no sistema de banco de dados espaciais, ter� que conter centenas de linhas de c�digo de programa��o para responder a mesma coisa utilizando arquivos de sistema.

A maioria dos usu�rios do PostGIS configuram sistemas em que v�rios aplicativos acessam os mesmos dados, de modo que ter um m�todo de acesso SQL padr�o simplifica a implanta��o e o desenvolvimento da solu��o. Alguns usu�rios trabalham com grandes conjuntos de dados. Com arquivos de sistema, muitas vezes esses dados tem que ser segmentados em v�rios arquivos, mas em um sistema de banco de dados esses dados podem ser armazenados em uma �nica tabela.

Em resumo, o acesso simult�neo aos dados por v�rios usu�rios, consultas ad hoc complexas e alto desempenho em grandes conjuntos de dados s�o o que separa os sistemas de bancos de dados espaciais dos sistemas baseados em arquivos.

### 2.2.1.3 Por que usar o Geopackage?

GeoPackage (GPKG) � um formato de dados espaciais aberto, n�o propriet�rio, independente de plataforma e � baseado em padr�es para sistema de informa��es geogr�ficas implementado como um cont�iner de banco de dados SQLite. Definido pela Open Geospatial Consortium (OGC) com o apoio dos militares dos EUA e publicado em 2014, o GeoPackage recebeu amplo apoio  de v�rias organiza��es governamentais, comerciais e de c�digo aberto.

Um GeoPackage � constru�do como um arquivo de banco de dados SQLite 3 estendido (*.gpkg) que cont�m tabelas de dados e metadados com defini��es especificadas, restri��es de integridade, limita��es de formato e restri��es de conte�do. O padr�o GeoPackage descreve um conjunto de conven��es (requisitos) para armazenar dados nos formatos vetoriais e matriciais em v�rias escalas, esquemas e metadados. Um GeoPackage pode ser estendido usando as regras de extens�o conforme definido na cl�usula 2.3 do padr�o. O padr�o OGC GeoPackage especifica um conjunto de extens�es aprovadas pelos membros OGC no Anexo F.

O GeoPackage foi projetado para ser o mais leve poss�vel, compartilhado em um �nico arquivo e pronto para ser usado. Isso o torna adequado para aplica��es mobile em modo offline e para compartilhamento r�pido por meio de armazenamento em nuvem, unidades USB e etc. O formato Geopackagepossui �ndices espaciais RTree em SQLite que melhoram a performance em consultas espaciais se comparados aos formatos tradicionais de arquivos geoespaciais.

Se comparado com o shapefile, o geopackage suporta tipos de dados n�o espaciais como inteiro, real, texto, blob, data, valores nulos, bem como n�o possui limita��o no comprimento do nome da coluna das tabelas, que no shapefile possui limita��o de 10 caracteres. Mas, uma das principais diferen�as entre o Shapefile e o Geopackage � que o shapefile possui limite em sua capacidade de armazenamento de 2 GB, enquanto o limite do Geopakcage � bem superior: 140 mil GB.

## 2.2.3 Um breve hist�rico do PostGIS

Em maio de 2001, a [Refractions Research](http://www.refractions.net/) lan�ou a primeira vers�o do PostGIS. O PostGIS 0.1 teve objetos, �ndices e umas poucas fun��es. O resultado foi um banco de dados adequado para armazenamento e para recupera��o de dados, mas n�o adequado para an�lise de dados.

� medida que o n�mero de fun��es aumentou, a necessidade de organiza��o tornou-se clara. A especifica��o "OpenGIS Implementation Specification for Geographic information-Simple feature access" (SFSQL) forneceu a estrutura necess�ria com diretrizes para nomea��o e requisitos de fun��es.

Com o suporte do PostGIS para an�lise simples e jun��es espaciais, o Mapserver tornou-se o primeiro aplicativo externo a fornecer visualiza��o de dados a partir do sistema de banco de dados espaciais.

Nos anos seguintes, o n�mero de fun��es do PostGIS cresceu bastante, mas seu poder de processamento permaneceu limitado. Muitas das fun��es mais interessantes como ST_Intersects (), ST_Buffer () ou ST_Union () foram muito dif�ceis de serem codificar. Partindo-se do zero, escrever esses c�digos levariam anos para ficarem prontos. Felizmente, surgiu um segundo projeto, o �Geometry Engine, Open Source� ou [GEOS](http://trac.osgeo.org/geos). A biblioteca GEOS fornece os algoritmos necess�rios para implementar as especifica��es da SFSQL. Ao associar-se ao GEOS, o PostGIS forneceu suporte completo para o SFSQL a partir da vers�o 0.8.

Surgiu um outro problema com o aumento da capacidade de dados do PostGIS: a representa��o usada para armazenar a geometria se mostrou relativamente ineficiente. Para objetos pequenos, como pontos e pequenas linhas, os metadados na representa��o das fei��es geom�tricas compreendiam at� 300% desses dados. Por raz�es de desempenho, foi necess�rio enxugar a representa��o. Os custos de leitura e processamento desses dados foram bastante reduzidos ao encolher o cabe�alho dos metadados e as dimens�es necess�rias.

No PostGIS 1.0, esta nova representa��o, mais r�pida e leve, tornou-se o padr�o. As atualiza��es mais recentes do PostGIS trabalharam na expans�o da conformidade dos padr�es, adicionando suporte para geometrias baseadas em curva e fun��es especificadas pelo padr�o SQLMM (ISO/IEC 13249-3:2016 Part 3: Spatial).

Com foco cont�nuo no desempenho, o PostGIS 1.4 melhorou significativamente a velocidade de processamneto das fun��es e consultas que utilizam geometrias.

## 2.2.4 Quem usa PostGIS?

Para uma lista completa de estudos de caso, veja a p�gina de [estudos de caso que utilizam PostGIS](http://postgis.net/casestudy).

## 2.2.5 Quais aplicativos oferecem suporte ao PostGIS?

A PostGIS tornou-se um banco de dados espacial amplamente utilizado, e o n�mero de programas de terceiros que oferecem suporte ao armazenamento e recupera��o de dados usando essa extens�o tamb�m aumentou. [programas que oferecem suporte ao PostGIS](http://trac.osgeo.org/postgis/wiki/UsersWikiToolsSupportPostgis) incluem softwares de c�digo aberto e propriet�rio em sistemas servidor e desktop.

### LEITURAS COMPLEMENTARES

Casanova, M., et. al.: Bancos de Dados Geogr�ficos. Cap. 1, 6, 8 e 11. MundoGEO. 2005

Yeung, A., Hall, G.: Spatial Database Systems. GeoJournal Library, vol 87. Partes 1 e 2. Springer, Heidelberd (2007)

