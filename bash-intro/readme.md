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
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sed "s/\"//g"
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
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort
```

El comandament `sort` també pot fer ordenacions numèriques i pot ordenar per qualsevol dels camps presents en el fitxer (consulta el manual per a més informació).

### Comandament `uniq`

En l'exemple anterior, al ordenar amb `sort`, hem vist que en la llista obtinguda hi ha gens repetits. Amb el comandament `uniq` podem eliminar les línies duplicades consecutives. 

Per a una eliminació completa, cal recordar ordenar amb `sort` abans d'utilitzar `uniq`.

**Visualitza el nom i la descripció del gen relacionat amb la leucèmia, però sense que apareguin repeticions de gens:**

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | uniq
```

**Comprovació del nombre de línies abans i després d'eliminar les repeticions:**

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | wc -l
89
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | uniq | wc -l
55
```

**Ordena alfabèticament els gens relacionats amb la leucèmia:**

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort
```

El comandament `sort` també pot fer ordenacions numèriques i pot ordenar per qualsevol dels camps presents en el fitxer (consulta el manual per a més informació).

### Comandament `uniq`

En l'exemple anterior, al ordenar amb `sort`, hem vist que en la llista obtinguda hi ha gens repetits. Amb el comandament `uniq` podem eliminar les línies duplicades consecutives. 

Per a una eliminació completa, cal recordar ordenar amb `sort` abans d'utilitzar `uniq`.

**Visualitza el nom i la descripció del gen relacionat amb la leucèmia, però sense que apareguin repeticions de gens:**

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | uniq
```

**Comprovació del nombre de línies abans i després d'eliminar les repeticions:**

```bash
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | wc -l
89
~$ grep leukemia microarray_adenoma_hk69.csv | cut -f 3,4 | sort | uniq | wc -l
55
```

---

# Tutorial: Comandaments `paste` i `join`

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

## TODO: 

https://docs.google.com/document/d/1MFcShrG3W-5Uset3eIthnzIMG19GeeKOXj_NKKTCYjo/edit?tab=t.0

https://www.marcusrb.com/unix/05-procesamiento-texto/

