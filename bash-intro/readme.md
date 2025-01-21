# Activitat: Processament de fitxers de text genòmics amb `bash`.

Activitat per realitzar tractament de fitxers genòmics amb `bash`, el terminal de comandes i scripts de `GNU/Linux`.

- [Índex Activitats](../README.md)

## Primers passos.

El primer que farem és baixar i descomprimir uns fitxers que ens serviran per treballar els diferents exemples:

```sh
curl -L https://raw.githubusercontent.com/miquelamorosaldev/bio-bash/main/bash-intro/biobash1.tar.gz | tar -xz
```

Si no et funciona la comanda, el pots baixar i descomprimir manualment des de la URL:

[URL Fitxer biobash1.tar.gz](https://raw.githubusercontent.com/miquelamorosaldev/bio-bash/main/bash-intro/biobash1.tar.gz)

Aquesta fitxer comprimit està acompanyat de **9 fitxers de text**:  

- `microarray_adenoma_hk69.csv`  
- `seqs_1.fasta`, `seqs_2.fasta` i `seqs_3.fasta`  
- `cancer_ciego.txt`, `cancer_progresion.txt`  
- `tomate_caracterizacion.txt`, `tomate_pasaporte.txt`  
- `tomate.sam`  

## Referències

Alguns dels fitxers s’han extret de:  
[https://www.ncbi.nlm.nih.gov/](https://www.ncbi.nlm.nih.gov/)  

Alguns dels exercicis i explicacions s'han extret de:  
[https://www.marcusrb.com/unix/05-procesamiento-texto/](https://www.marcusrb.com/unix/05-procesamiento-texto/)  

---

## Què és el processament de fitxers de text

Un dels punts forts dels sistemes **Unix** i per tant, de qualsevol distribució de **GNU/Linux** és la facilitat que ofereix la línia de comandes per analitzar els fitxers de text. 
Aquests sistemes inclouen una sèrie d’eines que permeten realitzar moltes manipulacions en aquests fitxers sense necessitat d’instal·lar eines especialitzades.  

Una de les funcions habituals d’un bioinformàtic és la de `parsejar` fitxers. Parsejar un fitxer significa analitzar sintàcticament el seu contingut.  

### Fitxers de text versus fitxers binaris

Abans de “parsejar” un fitxer, hem de saber què és i què no és un fitxer de text.  

Un fitxer de text és un fitxer dividit en línies i el contingut del qual és text. Malgrat el que pugui semblar a priori, un document de **Microsoft Office** o de **LibreOffice** **no** és un fitxer de text.  

Un dels punts fonamentals de la filosofia Unix, és la utilització de fitxers de text. 

Mentre altres sistemes operatius afavoreixen la utilització de fitxers binaris, que han de ser acompanyats d'eines especials per poder manipular-los, a Unix es va optar per crear un conjunt d'eines per a manipulació de fitxers de text (`vi`, `nano`, `gedit`) i per utilitzar per als fitxers del sistema només fitxers de text sempre que això fos possible.

Una altra punt d'aquesta filosfia Unix ("fes una cosa i fes-la bé") s'aplica amb la preferència de comprimir fitxers amb el format `.tar.gz` en comptes del `zip`. 
`tar` és una eina nativa als sistemes Unix des dels anys 70, pensada per empaquetar múltiples fitxers en un sol arxiu. Això el feia molt útil per transferir dades o fer còpies de seguretat. Posteriorment s'afegeix `gzip` per a la compressió. El resultat és el format combinat .tar.gz.

Els nous editors de text i de codi com `notepad++` o `VSCode` també segueixen aquesta filosofia i ens serviran.

## Crea directori per tractar experiment microarray_adenoma_hk69.csv 

Crea, dins del teu directori personal, el directori **microarray**; mou-hi el fitxer `microarray_adenoma_hk69.csv`.  

```bash
mkdir microarray
mv microarray_adenoma_hk69.csv microarray
```

Entra dins del nou directori i comprova `quant ocupa el fitxer`.

```bash
cd microarray
ls -lh
```

En el fitxer `microarray_adenoma_hk69.csv` estan emmagatzemats els resultats d’un experiment d’expressió diferencial en què s’han analitzat diferents adenomes.  
Aquest és un fitxer tabular on la informació es representa dividint els camps mitjançant comes o tabuladors.  

### Què és l’adenoma i per què ens interessa aquest microarray?  

- **Adenoma:**  
  Tumor que no és cancerós. Comença en cèl·lules que semblen glàndules dins del teixit epitelial (capa fina de teixit que cobreix òrgans, glàndules i altres estructures del cos).  
  Tot i que són benignes, poden derivar en càncer si no són tractats.  
- **Microarray:**  
  Tecnologia basada en la hibridació del DNA que ens permet analitzar un `genoma` sencer en un sol experiment.  

| **Metodologia tradicional (genètica)** | **Nova tecnologia (genòmica)** |  
|---------------------------------------|--------------------------------|  
| Un gen = Un experiment                | Un genoma = Un experiment     |

---

## Mostrant contingut dels fitxers: `wc`, `cat`, `head` i `tail`

Aquest fitxer conté els resultats d’un experiment d’expressió diferencial en què s’han analitzat diversos adenomes.  
És un fitxer tabular en què la informació es representa separant els camps amb **comes** o **tabuladors**.  

- Els fitxers que representen taules en format text s’anomenen **csv** (Comma Separated Values).  
- **Significat del contingut:**  
  - Cada fila del fitxer correspon a una **sonda del microarray**.  
  - Cada columna indica una **propietat sobre la sonda** o un **resultat de la hibridació**.  

El primer que podem fer amb un fitxer de text és obrir-lo per veure'n els continguts. 

Hi ha editors de text que funcionen en finestres, com el gedit, i editors que funcionen a la terminal, com el nano. 

Però de vegades els fitxers amb què volem treballar són tan grans que fins i tot els bons editors de text poden tenir problemes de rendiment per obrir-los.

És per això que usarem comandes per visualitzar el seu contingut.

### Comptar linies del fitxer: `wc` (word count)

Per saber el nombre de línies, paraules i caràcters abans d'obrir el fitxer usarem la comanda `wc`:  

```bash
wc microarray_adenoma_hk69.csv
```

### Accés al contingut del fitxer: `cat`

Podem utilitzar el següent comandament per accedir al contingut complet del fitxer:  
```bash
~$ cat microarray_adenoma_hk69.csv
```

⚠️ Si el fitxer és molt gran, la terminal podria **quedar bloquejada** durant un temps depenent de la memòria disponible al sistema. ⚠️

- Per **finalitzar o "matar"** el programa que està executant-se (en aquest cas, el `cat`), podem utilitzar la següent combinació de tecles:  
```text
Control + C
```

---

### Visualitzar només les primeres `head` i últimes línies `tail`.

Per obtenir una idea del contingut d’un fitxer molt gran sense bloquejar la terminal, podem imprimir només les primeres o darreres línies.

Per imprimir les primeres línies del fitxer usem `head`:  

- Les primeres **10 línies**:  
  ```bash
  ~$ head microarray_adenoma_hk69.csv
  ```
- Les primeres **20 línies**:  
  ```bash
  ~$ head -n 20 microarray_adenoma_hk69.csv
  ```

En canvi, usem el comandament `tail` per veure les darreres línies

```bash
~$ tail microarray_adenoma_hk69.csv
```

--- 

## grep: Buscar patrons en fitxers de text

La comanda **grep** (*Generalized Regular Expression Parser*) pren un fitxer d'entrada (o l'entrada estàndard) i filtra les línies que contenen el patró de cerca que li hem indicat. 

És útil per filtrar el contingut d'un fitxer segons un terme o patró específic.

---

### Exemple d'ús bàsic del grep

**Quina és l'expressió dels gens relacionats amb la <a hfref="https://medlineplus.gov/spanish/ency/article/001299.htm">leucèmia</a> (leukemia en anglès) en el fitxer del microarray?**  
Això, que en altres sistemes operatius podria ser complex, a Unix és senzill:

```bash
grep leukemia microarray_adenoma_hk69.csv
```

Per defecte, **grep** retorna les línies que contenen el patró especificat. Si volem obtenir les línies que **no contenen** el patró, podem utilitzar l'opció `-v` (**inVert**).  
La comanda **grep** distingeix entre majúscules i minúscules, però podem desactivar aquest comportament amb l'opció `-i` (**ignore case**):

```bash
grep -i leukemia microarray_adenoma_hk69.csv
```

---

### Trobar la línia i comptar coincidències

**En quines posicions del fitxer es troben les línies que contenen la paraula *leukemia*?**  
**Quantes línies hi ha amb aquest patró?**

Llistar les línies amb el número de línia:
```bash
grep -n leukemia microarray_adenoma_hk69.csv
```
Comptar el nombre total de línies:

```bash
grep -n leukemia microarray_adenoma_hk69.csv | wc -l
```

**Sortida:**  
```
89
```

### Altres usos avançats del grep

**Manual i opcions avançades**
**grep** suporta expressions regulars, fet que el fa extremadament potent. Pots consultar totes les opcions disponibles al manual:  
```bash
man grep
```

---

### Combinació amb pipes (`|`)

Una de les grans avantatges de **grep** és la seva capacitat per combinar-se amb altres programes mitjançant pipes (`|`).

**Busca el terme *leukemia* només en les primeres 100 línies del fitxer:**
```bash
head -n 100 microarray_adenoma_hk69.csv | grep leukemia
```

**Quants gens relacionats con la leucèmia hi ha al fitxer?**
```bash
~$ grep leukemia microarray_adenoma_hk69.csv | wc
     89    5256   31600
```

### Redirecció del resultat amb (`>`)

**Redirigeix el resultat de la cerca a un fitxer anomenat *busqueda_leukemia_100.txt* per guardar-lo:**

```bash
head -n 100 microarray_adenoma_hk69.csv | grep leukemia > busqueda_leukemia_100.txt
```

Amb **grep**, pots gestionar grans volums de dades de manera eficient i personalitzar les cerques segons les teves necessitats! 🚀

---

### Exemple de `cut`:

Quan el fitxer està dividit en camps, com en el cas de la taula que estem utilitzant, podem seleccionar algun d'aquests camps utilitzant la comanda `cut`. A continuació, veurem com podem millorar la cerca seleccionant només el nom i la descripció del gen.

**Visualitza el nom i la descripció del gen relacionat amb la leucèmia:**

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | wc -l
89
```

Amb el paràmetre `-f` indiquem la llista de camps (fields) que volem seleccionar. `cut` assumeix que els camps en el fitxer estan separats per tabuladors. Però també podem especificar que els camps estan separats per un altre caràcter, com per exemple per comes:

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 -d ','
```

Un altre exemple seria:

```bash
cat /etc/passwd | cut -d ':' -f 1,5
```

---


### Comanda `sed` (stream editor)

La llista de gens obtinguda en el pas anterior pot ser útil per a molts propòsits, però encara no està neta del tot. 

Per exemple, seria `millor eliminar les cometes que envolten els camps`. 

Aquesta tasca la podem realitzar amb la comanda `sed` (stream editor).

Però abans veiem un exemple més senzill.

**Exemple previ:**

Crea un fitxer:

```bash
echo "Hola Joan! Estàs bé Joan?" > prova.txt
```

Aplica la transformació a un altre fitxer:

```bash
sed 's/Joan/Miquel/g' prova.txt > prova2.txt
```

Comprovació:

```bash
cat prova2.txt
```

Aplica la transformació al mateix fitxer:

```bash
sed -i 's/Joan/Miquel/g' prova.txt
```

Comprovació:

```bash
cat prova.txt
```

El comandament `sed` processa les línies una a una, aplica la transformació que li indiquem i retorna les línies modificades.

### Eliminar les cometes dels camps obtinguts:

Per eliminar les cometes dels camps del fitxer obtingut en la pregunta anterior, utilitzem:

```bash
grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sed "s/\"//g"
```

Resultat:
```
BAALC   Brain and acute leukemia, cytoplasmic
DEK     DEK oncogene (DNA binding)
```

La sintaxi utilitzada per `sed` pot resultar una mica complicada al principi, però un coneixement bàsic del comandament ens permetrà fer modificacions en el text que d'altra manera serien molt complexes. En aquest cas, el comandament `sed` que hem utilitzat és `s/\"//g`. 

Primer, hem indicat a `sed` què volíem fer. 

Podem, com en aquest cas, substituir un patró per un altre (comandament `s`, substituir), però també podríem indicar-li que elimini línies (comandament `d`, delete). 

Per substituir cal indicar el patró que volem substituir i el patró pel qual volem substituir-lo:

```bash
s/patró_a_substituir/nou_patró/
```

En aquest exemple, hem substituït les cometes per res (una cadena buida). 

En principi hauríem d'haver escrit `s/\"//`, però les cometes tenen un significat especial en aquest context, per la qual cosa hem hagut d'escapar-les, així que escrivim `s/\"//`. 
A més, hem afegit la lletra `g`, que indica a `sed` que substitueixi totes les aparicions del patró a cada línia, no només la primera.

`sed` accepta expressions regulars com a patrons i amb elles podem realitzar pràcticament qualsevol substitució que puguem imaginar.

---

### Ordenació amb `sort`:

Si desitgem ordenar alfabèticament un fitxer de text, només hem d'utilitzar el comandament `sort`.

**Ordena alfabèticament els gens relacionats amb la leucèmia:**

```bash
grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort
```

El comandament `sort` també pot fer ordenacions numèriques i pot ordenar per qualsevol dels camps presents en el fitxer (consulta el manual per a més informació).

### Eliminar duplicats amb `uniq`

En l'exemple anterior, al ordenar amb `sort`, hem vist que en la llista obtinguda hi ha gens repetits. Amb el comandament `uniq` podem eliminar les línies duplicades consecutives. 

Per a una eliminació completa, cal recordar ordenar amb `sort` abans d'utilitzar `uniq`.

**Visualitza el nom i la descripció del gen relacionat amb la leucèmia, però sense que apareguin repeticions de gens:**

```bash
grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | uniq
```

**Comprovació del nombre de línies abans i després d'eliminar les repeticions:**

```bash
grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | wc -l
```
89

```bash
grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | uniq | wc -l
```
55

**Ordena alfabèticament els gens relacionats amb la leucèmia:**

```bash
grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort
```

El comandament `sort` també pot fer ordenacions numèriques i pot ordenar per qualsevol dels camps presents en el fitxer (consulta el manual per a més informació).

--- 

## Tractament fitxers `.fasta`

Probablement és el format de fitxer més utilitzat per a seqüències i un dels tipus de formats de fitxer més comuns en bioinformàtica. 

El format de fitxer `FASTA` té els seus orígens en el programa FAST, utilitzat per a l'alineació de seqüències. 

El format de fitxer es defineix simplement com un fitxer de text pla amb una o més entrades que consisteixen en una línia amb un símbol `>` seguit d'una línia de definició identificativa única, o `defline`, i una o més `línies de dades de seqüència`. 

**Crear un fitxer de text fasta** és molt fàcil, tant en un editor de text pla com `notepad` o `VSCode`.

Podem crear un **fitxer unifasta (una sola seqüencia)** que es digui `uniseq.fasta` amb l'editor VSCode, simplement copia el text i guarda'l.

```txt
>Seqüència aminoàcids de prova
MTHCP*MTI*
```

O bé crear un **fitxer multifasta** (més d'una seqüència) anomenat `sequences.fa` des del terminal Linux:

```sh
echo ">a
ACGCGTACGTGACGACGATCG
>b
ATTTCGCGACTCTGCCTACGCTAC
>c
GGGAAACCTTTTTTT" > sequences.fa
```

Bingo! Ja tenim un fitxer multifasta :) amb les seqüències a, b i c.

El requisit fonamental és que el fitxer sigui `plain text` per tal que es pugui tractar amb qualsevol aplicació de processament de textos o llenguatge de programació.

Per tant, aquests fitxers es tracten millor en editors de text com `nano`, `sublime` o `VSCode`. 

Per veure un fitxer `FASTA` des de la línia d'ordres sense editar-lo, pots fer servir la aplicació `cat`.

```sh
cat uniseq.fasta
>Seqüència aminoàcids de prova
MTHCP*MTI*
```

També pots llegir-les linia a linia amb `more` i `less`; que això és ideal per a fitxers grans:

```py
less sequences.fa
```

Per sortir de less només has d’apretar la tecla `q` (“quit”).

També pots filtrar les primeres i últimes files amb les comandes: `head` i `tail`. 

---

### Característiques. 

Realment no hi ha cap restricció sobre la seqüència de la definició en el format estàndard, excepte que la definició ha de seguir immediatament el `>` sense cap espai intermedi. 

Per tant, dins de d’seqüència, hi ha dos coses:

* Un `comentari/descripció` que comença per `>` i es `una sola linea`
* La `seqüència de bases` dividides per línies de ...[70] caràcters, pot variar.

Si es un fitxer MULTI-FASTA (més d'una línia), és separen les seqüències amb una linea de capçalera ( `>` ).

Qualsevol tipus de seqüència o conjunt de seqüències es pot posar en un fitxer FASTA: els 3.000 milions de parells de bases del genoma humà complet, la seqüència d'ARN completa d'un virus o la seqüència d'ADN del promotor del gen del teu ratolí favorit són exemples vàlids.

### On trobar fitxers FASTA.

Ja sabeu que un dels millors llocs on aconseguir fitxers `.fasta` d’organismes reals de forma legal és el repositori públic de l’`NCBI`, que ens facilita un cercador molt complet. 

Exposem algusns enllaços directes com a exemples.

Gènoma complet del SarsCov2:
- <https://www.ncbi.nlm.nih.gov/nuccore/NC_045512.2?report=fasta>

Gen de la orquídea:
- <https://github.com/biopython/biopython/blob/master/Doc/examples/ls_orchid.fasta>

Gen insulina a l’ésser humà.
- <https://www.ncbi.nlm.nih.gov/nuccore/L15440.1?report=fasta>

Gen de l'insulina al gos:
- <https://www.ncbi.nlm.nih.gov/nuccore/AF013216.1?report=fasta>

### Com descarregar-los ? 

Per automatitzar el nostre programari, pots descarregar-te fitxers fasta com qualsevol fitxer.

Des del terminal (`bash`):

```sh
wget -O ls_orchid.fasta https://raw.githubusercontent.com/biopython/biopython/refs/heads/master/Doc/examples/ls_orchid.fasta
```

També pots veure com fer-ho amb `Python` i la llibreria `urllib3`; per tal de descarregar-lo únicament si no hi és.

<https://xtec.dev/python/http/#cache>

⚠️ Però compte! Hi ha restriccions si els volem baixar d'NCBI. ⚠️

NCBI té obert un repositori `FTP` amb fitxers de les seves bases de dades (com els articles PMC o PUBMED) que es troba a:
- <https://ftp.ncbi.nlm.nih.gov/>

Però ni des d'aquest repositori ni des de programari fet amb Python o bash se'ns permet baixar directament qualsevol fitxer fasta.

Més endavant veurem com fer-ho amb un programari específic que distribueix NCBI/Entrez:
- https://xtec.dev/bio/entrez/>

---

### Lectura i tractament de fitxers fasta amb GNU/Linux (Bash).

#### Mostrar contingut fitxers

Obre un terminal en Linux (la icona és una finestra negra) o bé prem la drecera Ctrl + Alt + T, que funciona en distribucions basades en Debian (Ubuntu, Mint, PopOS …)

Introdueix aquesta comanda, que ens permet veure la capçalera i el contingut de les seqüències del fitxer sequences.fa (que hem creat abans):

```sh
grep -E '^\S+$' sequences.fa | awk '{printf "%s ", $0; getline; print $0}'
```

Resultat:
```sh
>a ACGCGTACGTGACGACGATCG
>b ATTTCGCGACTCTGCCTACGCTAC
>c GGGAAACCTTTTTTT
```

Gràcies al llenguatge del shell `awk` podem aconseguir-ho.

Una altra manera és usar un script de `bash`; que anomenarem `readfasta.sh`:

```sh
#!/bin/bash
echo 'Llegim tots els fitxers fasta de la carpeta actual.'
for i in *.fasta; do
    echo "Núm Seqüències"
    grep -c "^>" $i
    echo "Info Seqüències"
    # Capçalera seqüència.
    grep "^>" $i
    # Contingut seqüència:
    grep -v "^>" $i
done
```

Recorda com executar-lo:

```sh
chmod u+x readfasta.sh
./readfasta.sh 
```

#### Recompte dades de seqüències

El símbol pipa `|` (pipe) envia la sortida d'un programa (`grep '>' sequences.fasta`) a l'entrada d'un altre programa (`wc`), ja que amb aquest exemple estem enviant la sortida de grep al `wc` per comptar. 

**Important!**  Aneu amb compte per assegurar-vos que el símbol `>` estigui inclòs entre cometes en aquesta ordre. Si no s'inclouen les cometes, es sobreescriurà el fitxer! 

```sh
 $ grep '>' sequences.fasta | wc -l
  3
```

#### Guardar resultats en nous fitxers

A més a més; un símbol `>` a la línia d'ordres redirigeix ​​la sortida d'un programa a un fitxer i sobreescriu el fitxer. 

Per exemple, podem redirigir la sortida de l'exemple anterior així:

```sh
 $ grep ">" sequences.fasta -l | wc > numSequences.txt
```

D'aquesta manera no imprimeix res a la pantalla, sinó emmagatzema la informació en un fitxer. 

#### Transcripció i edició de seqüències

També podem realitzar algunes operacions, com la transcripció; amb la comanda `sed` (stream editor).

```sh
 cat seq_3.fasta | sed ‘s/T/U/g’
```

---

## Concatenació de files de fitxers: `paste` i `join`

### Comandament `paste`

El comandament `paste` permet unir fitxers tabulars línia per línia. 

Suposem que tenim dos fitxers, un amb dades sobre la progressió de la malaltia d'una sèrie de pacients i un altre amb el genotipat dels mateixos:

**Crea el fitxer pacients.txt**

```txt
id_paciente,nivel_colesterol
1,190
2,250
3,220
4,260
5,160
```

**Crea el fitxer genotipat.txt**

```txt
id_paciente,SNP_a,SNP_b
1,AA,CC
2,AC,GG
3,AA,CG
4,AT,GG
5,AA,CC
```

Per fusionar aquests dos fitxers per columnes i per files, utilitzem el comandament `paste`:

```bash
~$ paste -d',' pacientes.txt genotipado.txt
id_paciente,nivel_colesterol,id_paciente,SNP_a,SNP_b
1,190,1,AA,CC
2,250,2,AC,GG
3,220,3,AA,CG
4,260,4,AT,GG
5,160,5,AA,CC
```

Per visualitzar el contingut dels fitxers, podem utilitzar `cat`:

```bash
~$ cat pacientes.txt genotipado.txt
```

---

### Comandament `join`

El comandament `join` permet unir dos fitxers de text tabulars en un de sol utilitzant una columna com a clau comuna. A diferència de `paste`, les files no han de ser ordenades i la columna comuna no es duplica.

Suposem que tenim dos fitxers, un amb dades de diferents varietats de tomàquet i un altre amb la seva caracterització:

**Crea el fitxer pasaporte.txt**

```txt
nombre        origen
valenciano    Valencia
penjar        Barcelona
amarillo      Roma
invernadero   Murcia
```

**Crea el fitxer caracterizacion.txt**

```txt
nombre      color    forma            duración
valenciano  rojo     apuntado         corta
penjar      rojo     ovalado          larga
amarillo    amarillo ovalado          grande normal
invernadero rojo     ovalado          normal
```

El comandament `join` ens permet unir aquestes dues taules en una sola utilitzant el nom de la varietat de tomàquet com a clau comuna:

```bash
$ join pasaporte.txt caracterizacion.txt
nombre origen color forma duración
valenciano Valencia rojo apuntado corta
penjar Barcelona rojo ovalado larga
amarillo Roma amarillo ovalado grande normal
invernadero Murcia rojo ovalado normal
```

Si utilitzem `paste`, les taules es fusionaran sense tenir en compte la clau comuna, i es duplicaran les columnes amb el mateix nom:

```bash
$ paste pasaporte.txt caracterizacion.txt
nombre        origen  nombre        color    forma            duración
valenciano    Valencia    valenciano  rojo     apuntado         corta
penjar        Barcelona    penjar      rojo     ovalado          larga
amarillo      Roma        amarillo    amarillo ovalado      grande   normal
invernadero   Murcia      invernadero   rojo     ovalado          normal
```

---

### Reptes codificació fitxers.

Els ordinadors codifiquen els caràcters del llenguatge natural mitjançant **codi binari**, que es transforma en caràcters utilitzant una **taula de codificació**. La taula **ASCII**, una de les més populars, inclou només caràcters de la llengua anglesa i no permet accents ni caràcters d'altres alfabets.

Per solucionar aquesta limitació, es van crear variants de l'ASCII per a cada llengua. Això, però, va generar el problema de la **incompatibilitat entre taules**, ja que usar una taula incorrecta produïa caràcters estranys. La norma **Unicode** va ser desenvolupada com una solució global, oferint més de 100.000 símbols que inclouen gairebé tots els alfabets humans. 

A la pràctica, per obrir un fitxer correctament, cal saber en quina taula de caràcters ha estat codificat. A **Linux**, s'usa el format **UTF-8**, un estàndard Unicode, mentre que **Windows** utilitza sovint **Windows-1252** (Latin1). Els editors de text permeten seleccionar la codificació correcta per evitar errors de visualització.

### Ús del Comandament `file`

El comandament `file` permet identificar la codificació dels fitxers. Exemple:

```bash
$ file *
caracterizacion.txt:         ASCII text
genotipado:                  ASCII text
microarray_adenoma_hk69.csv: ASCII text, with very long lines
pacientes:                   ASCII text
pasaporte.txt:               ASCII text
sagan.utf-8_mod.txt:         ASCII text
```

### Conversió de Codificacions amb `iconv`

Amb el programa **iconv**, podem convertir fitxers d'una codificació a una altra. Exemple:

```bash
# De UTF-8 a ISO-8859-1
$ iconv -t ISO-8859-1 -f UTF-8 caracterizacion.txt > caracterizacion.txt.utf-8_mod.txt

# De ISO-8859-1 a UTF-8
$ iconv -f ISO-8859-1 -t UTF-8 caracterizacion.txt > caracterizacion.txt.iso-8859-1_mod.txt
```

Després de la conversió, podem verificar la codificació amb `file`:

```bash
$ file caracterizacion.txt.iso-8859-1_mod.txt 
caracterizacion.txt.iso-8859-1_mod.txt: UTF-8 Unicode text
```

### Final de Línia

Els fitxers de text utilitzen diferents caràcters per marcar el final de línia segons el sistema operatiu:
- **Unix/Linux**: `\n`
- **Windows**: `\r\n`
- **MacOS clàssic**: `\r`

Editors moderns reconeixen aquests formats i mostren el text correctament, però eines com el **Notepad** de Windows poden tenir problemes amb fitxers creats en Linux, mostrant tot el text en una sola línia amb símbols estranys.

---

## Exercicis.

```markdown
### Anàlisi i Gestió de Fitxers de Dades Biomèdiques

S'ha realitzat un estudi d'un nou tractament per a un **limfoma**, i s'han proporcionat dos fitxers:
- **“cancer_progresion.txt”**: conté dades dels pacients i els resultats del tractament.
- **“cancer_ciego.txt”**: inclou la informació per desxifrar l'assaig a doble cec, amb identificadors dels pacients i la dosi administrada.

#### 1. Quants pacients hi havia a l'estudi?

```bash
$ grep -v nombre cancer_progresion.txt | wc -l
```

```bash
$ grep -v id cancer_ciego.txt | wc -l
```


#### 2. De quants pacients no tenim dades de progressió?

```bash
$ grep desconocido cancer_progresion.txt | wc -l
```


#### 3. Convertir la separació per comes a tabuladors en “cancer_ciego.txt” i redirigir la sortida a “cancer_ciego_tab.txt”

```bash
$ sed -e 's/,/\t/' cancer_ciego.txt > cancer_ciego_tab.txt
```

#### 4. Unir “cancer_progresion.txt” i “cancer_ciego_tab.txt” en el fitxer “cancer.txt”

```bash
$ join cancer_progresion.txt cancer_ciego_tab.txt > cancer.txt
```

```bash
$ join -t $'\t' cancer_progresion.txt cancer_ciego_tab.txt > cancer.txt
```


#### 5. Transformar els espais del fitxer “cancer.txt” per tabuladors

```bash
$ sed -e 's/ /\t/g' cancer.txt
```


#### 6. Com ha anat el tractament segons la dosi?

```bash
$ grep -i placebo cancer.txt
```

```bash
$ grep -i 1mg cancer.txt
```

```bash
$ grep -i 2mg cancer.txt
```

---

#### 7. Crear un fitxer amb els primers 100 resultats del microarray i només 10 columnes (“micro.txt”)

```bash
$ grep -v '^"' microarray_adenoma_hk69.csv | head -n 100 | cut -f 1-10 > micro.txt
```

#### 8. Ordenar el fitxer “micro.txt” pel nom del gen (camp 3) i per l’ID en ordre numèric invers

```bash
$ sort -k 3 micro.txt
```

```bash
$ sort -nr micro.txt
```

---


```markdown
### 9. Disposem de dos fitxers amb seqüències d’ADN (**seqs_1.fasta** i **seqs_2.fasta**).  

#### a. Quantes seqüències hi ha en cada fitxer?  

```bash
$ cat seqs_1.fasta | grep '>' | wc -l
11
```

```bash
$ cat seqs_2.fasta | grep '>' | wc -l
11
```

#### b. Hi ha alguna seqüència present en tots dos fitxers?  
Comprovem primer el total de seqüències no repetides sumant les dues llistes.  

```bash
$ cat seqs_1.fasta seqs_2.fasta | grep '>' | sort | uniq | wc -l
20
```

**Resultat:** Hi ha seqüències repetides perquè el nombre total és inferior a la suma de seqüències úniques.  

#### c. Identifiquem quines seqüències es repeteixen:  

```bash
$ cat seqs_1.fasta seqs_2.fasta | grep '>' | sort | uniq -d
>gi|311207420|gb|GT728904.1|GT728904
>gi|311210057|gb|GT715712.1|GT715712
```

**Confirmació:** Verifiquem que aquestes seqüències apareixen en ambdós fitxers:  

```bash
$ grep '>gi|311207420|gb|GT728904.1|GT728904' seqs_1.fasta
>gi|311207420|gb|GT728904.1|GT728904
```

```bash
$ grep '>gi|311207420|gb|GT728904.1|GT728904' seqs_2.fasta
>gi|311207420|gb|GT728904.1|GT728904
```

---

### 10. Disposem d’un fitxer amb seqüències d’ADN (**seqs_3.fasta**).  
#### a. Extraiem els noms de les seqüències:  

Primer, inspeccionem el fitxer:  

```bash
$ head seqs_3.fasta
```

Després, extreiem els noms de les seqüències:  

```bash
$ grep ">" seqs_3.fasta | cut -c 2- | cut -f 1 -d " "
```

---

#### 11. Transcrivim l’ADN a ARN substituint totes les **T** per **U**:  


```bash
$ cat seqs_3.fasta | sed 's/T/U/g'
```

---

### 12. Reemplaçar **ATG** per **GAT** en el fitxer **seqs_3.fasta**  

```bash
$ cat seqs_3.fasta | sed 's/ATG/GAT/g'
```

---

### 13. Disposem d’un fitxer de mapeig en format SAM (**tomate.sam**)  
#### a. Quantes seqüències s’han mapejat?  

```bash
$ grep -v "^@" tomate.sam | wc -l
```

#### b. Quantes s’han mapejat en direcció reversa?  
(Camp 2 amb valor 16):  

```bash
$ grep -v "^@" tomate.sam | cut -f 2 | grep 16 | wc -l
```

#### c. Quins són els **unigenes** als quals s’ha pogut mapar alguna seqüència?  

```bash
$ grep -v "^@" tomate.sam | cut -f 3 | sort -u
```

#### d. Ordenar les seqüències mapejades segons el nom del **unigene** i la posició:  

```bash
$ grep -v "^@" tomate.sam | sort -k 3,4 | cut -f 1
```
