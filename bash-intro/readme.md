# Activitat: Processament de fitxers de text genòmics amb `bash`.

Activitat per realitzar tractament de fitxers genòmics amb `bash`, el terminal de comandes i scripts de `GNU/Linux`.

- [Índex Activitats](../README.md)

## Primers passos.

El primer que farem és baixar i descomprimir uns fitxers que ens serviran per treballar els diferents exemples:

```sh
curl -L -o bio-bash-1.zip https://github.com/miquelamorosaldev/bio-bash/raw/e49cbd84e6656deb43978f94c645289dd97874d5/bash-intro/biobash1.tar.gz | tar -xz
```

Si no et funciona la comanda, el pots baixar i descomprimir manualment des de la URL:

<[https://github.com/miquelamorosaldev/bio-bash/blob/main/bio-bash-1.zip](https://github.com/miquelamorosaldev/bio-bash/blob/main/bash-intro/bio-bash-1.zip)>

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

## TODO: 

https://docs.google.com/document/d/1MFcShrG3W-5Uset3eIthnzIMG19GeeKOXj_NKKTCYjo/edit?tab=t.0

https://www.marcusrb.com/unix/05-procesamiento-texto/

