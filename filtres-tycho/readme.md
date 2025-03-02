
## Cas pràctic 1. Fitxer enfermentats EEUU Tycho.

En aquesta secció repassarem com podem utilitzar els mètodes de `bash` més comuns per a preprocessar el voluminós [dataset d'exemple del Projecte Tycho](https://www.tycho.pitt.edu/data/)

Aquest dataset conté un historial del recompte de casos de diverses enfermetats que han afectat als EEUU des del 1888 al 2014. 

En alguns anys hi ha molts registres (1900-1950) i d’altres menys, però en total podem tenir més d’10M d’observacions; cadascuna de les quals té 10 columnes d'interès.

Per accedir-hi ens podem registrar (és gratuït) però no ens cal, ja que tenim les dades de fa 2 anys i no ens cal que siguin actualitzades per aquest exemple.

**Hem penjat un subset de 1M de línies en aquest fitxer, massa gran per allotjar-lo a GitHub (120 MB un cop descomprimit):**

[Versió reduïda Tycho](https://drive.google.com/file/d/1VsGgR__ajY_dIBJMYry_BMJlRCRdLFc0/view)

Un cop descarregat, obriu un terminal de Linux i proveu aquestes operacions. 

⚠ No obriu el fitxer dirèctament o us arrisqueu a que la màquina se us pengi, o perdreu temps esperant la càrrega ⚠

Mostra les 10 primeres línies del fitxer (la capçalera i 9 més)

```sh
cat tycho.csv 
```

⚠ Mostra tot el contingut del fitxer pel terminal. Si executeu la comanda per accident i no voleu esperar, premeu `Ctrl+C` per finalitzar el procés. ⚠

```sh
cat tycho.csv 
```

Compta el número de línies en total.

```sh
cat tycho.csv | wc -l 
```

Mostra les 10 primeres línies que continguin el text 1888. En el nostre cas l'any 1888.

```sh
head tycho-mini.csv | grep 1888
```

Fixeu-vos que amb el tros de comanda `| grep 1888` podem filtrar per files, de tot el fitxer només les línies que continguin el número 1888. No és perfecte, però segur que seleccionarà totes les mostres de l'any 1888 (i potser algún altre any amb 1888 casos, però això es pot perfeccinar més endavant amb altres comandes o amb Python)

Com podeu veure, tots els filtres que apliquem es mostren per pantalla, però si volem que es guardin en un fitxer disposem de l'operador de redirecció `>`.

Amb aquesta comanda, guardo la info del fixer de 3 anys en el fitxer anomenat tycho-optim.csv:

```sh
cat tycho-mini.csv | grep -E ‘1910|1911|1912’ > tycho-optim.csv
```

Amb una sola comanda hem aconseguit reduïr el fitxer a les 78000 línies (aproximadament) que ens interessen. 
**Oi que val la pena aquest preprocessament ?**

---

Per fer els següents exercicis, podeu descarregar el Tycho:

```sh
wget https://gitlab.com/xtec/bio/pandas/-/raw/main/data/tycho/tycho78k.csv
```

{% image "linux_wget_tycho.png" %}

Fixa't amb les columnes que disposem originalment dins fitxer CSV de Tycho:

```sh
head -n 2 tycho78k.csv
epi_week,country,state,loc,loc_type,disease,event,number,from_date,to_date,url
188824,US,PA,PHILADELPHIA,CITY,TYPHOID FEVER [ENTERIC FEVER],DEATHS,14,1888-06-10,1888-06-16,http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2084171
```

Llistat camps:

- **epi_week**
Setmana epidemiològica (de l'1 al 52 normalment, algún cop hi ha 53). És una mètrica necessària i molt habitual en la informàtica mèdica.

- **country**
País. En aquest dataset només hi ha mostres dels Estats Units, per tant podrem ometre-la. US

- **state**
Sigles de l'estat dels EEUU.

- **loc**
Nom complet de l'estat dels EEUU. Guardar state i loc (info redundant) només si volem visualitzar mapes. 

- **loc_type**
En el dataset pot ser CITY o STATE. 

- **disease**
Enfermetat. Entre [] ens indica informació addicional, que potser en el nostre estudi és necessària i ometre-la ens pot estalviar memòria del dataFrame.

- **event**
Cada event pot ser de 2 tipus, i és important distingir-los segons el que volguem estudiar: CASES (número de casos), DEATHS(número de morts causats per la enfermetat). 
Si volem treballar bé aquest estudi es pot calcular una ràtio de CASES i DEATHS, que el seu resultat serà entre 0 i 1 (1 si tots els casos han estat mortals)

- **number**
Important! Número de casos (si event='CASES') o número de morts (si event='DEATHS') 

- **from_date,to_date**
Dates d'inici i de fi en què es mesura el número de casos. Són (o haurien de ser) intèrvals d'una setmana.

- **url**
El projecte Tycho ha escanejat i/o digitalitzat documents de paper a PDF (anys 1980 i anteriors) que demostren els registres realitzats i han de ser molt interessants. Desgraciadament l'enllaç proporcionat no funciona.

Finalment, remarcar que podem observar que **el dataset està en format Tidy**, que és el que desitgem per poder realitzar estadítica i gràfics. **Aquest punt és fonamental verificar-lo abans de començar a investigar un dataset.**

- Cada fila és una observació 
- Cada columna és una variable
- Cada valor té una única dada

Si el dataset no fos Tidy, hauriem de preprocessar-lo i arreglar-lo fins que ho sigui.

Ara que les coneixem, podem crear una còpia del fitxer que només contingui les columnes de:
state,disease,from_date,number. Hem de mirar en quin número de columna estan.

```sh
        ,        ,3,        ,       ,6,     ,   ,8      ,9,     ,       , 
epi_week,country,state,loc,loc_type,disease,event,number,from_date,to_date,url
```

La comanda és:

```sh
cat tycho78k.csv | cut -d ',' -f 3,6,8,9 > tycho78k_cropped.csv
```

I el resultat:

{% image "linux_cut_tycho.png" %}

### Exercicis.

**A partir del fitxer tycho78k.csv que has descarregat abans.**

Columnes:
```sh
epi_week,country,state,loc,loc_type,disease,event,number,from_date,to_date,url
```

**Q1-** Compta el número de files que hi ha l'any 1909. 

**Q2-**  Mostra l'epi_week, event i el number del fitxer: l'any 1910, a l'estat de CHICAGO

**Q3-** Mostra tota la informació els casos mortals de TUBERCULOSIS, (columna event='DEATHS') l'any 1910 a l'estat de CHICAGO. 

**Q4-** Guarda en un nou fitxer l'epi_week, state i number del fitxer: de casos de tos ferina (WHOOPING COUGH), (columna event='CASES') l'any 1911.

**Q5-Elimina el valor CITY** de la columna state (per exemple que en comptes de KANSAS CITY només aparegui KANSAS).

**Q6-Crea un nou fitxer en format tsv:** el mateix, que en comptes de ser separat per comes ho sigui per tabuladors.

---

{% sol %}

**Q1. Compta el número de files que hi ha l'any 1909.**

```bash
grep '1909' tycho78k.csv | wc -l
```

Explicació:
- `grep '1909'`: Busca totes les línies que contenen l'any 1909 a qualsevol part.
- `wc -l`: Compta el nombre de línies.

**Q2. Mostra l'epi_week, event i el number del fitxer: l'any 1910, a l'estat de CHICAGO.**

```bash
grep '1910' tycho78k.csv | grep 'CHICAGO' | cut -d ',' -f1,7,8
```

Explicació:
- `grep '1910'`: Filtra les línies que contenen l'any 1910.
- `grep 'CHICAGO'`: Filtra les línies que contenen l'estat de CHICAGO.
- `cut -d ',' -f1,7,8`: Selecciona les columnes `epi_week` (1), `event` (7) i `number` (8).

**Q3. Mostra tota la informació dels casos mortals de TUBERCULOSIS, (columna event='DEATHS') l'any 1910 a l'estat de CHICAGO.**

```bash
grep '1910' tycho78k.csv | grep 'CHICAGO' | grep 'TUBERCULOSIS' | grep 'DEATHS'
```

Explicació:
- `grep '1910'`: Filtra l'any 1910.
- `grep 'CHICAGO'`: Filtra l'estat de CHICAGO.
- `grep 'TUBERCULOSIS'`: Filtra els casos de tuberculosis.
- `grep 'DEATHS'`: Filtra els casos mortals (`event='DEATHS'`).

**Q4. Guarda en un nou fitxer l'epi_week, state i number del fitxer: de casos de tos ferina (WHOOPING COUGH), (columna event='CASES') l'any 1911.**

Filtra els casos de tos ferina (`WHOOPING COUGH`) de l'any 1911, i després guarda les columnes seleccionades:

```bash
grep '1911' tycho78k.csv | grep 'WHOOPING COUGH' | grep 'CASES' | cut -d ',' -f1,3,8 > whooping_cough_1911.csv
```

Explicació:
- `grep '1911'`: Filtra l'any 1911.
- `grep 'WHOOPING COUGH'`: Filtra els casos de tos ferina.
- `grep 'CASES'`: Filtra els casos (`event='CASES'`).
- `cut -d ',' -f1,3,8`: Selecciona les columnes `epi_week` (1), `state` (3), i `number` (8).
- `> whooping_cough_1911.csv`: Guarda el resultat en un nou fitxer.

**Q5. Elimina el valor CITY de la columna state (per exemple que en comptes de KANSAS CITY només aparegui KANSAS) usa el sed.**

Utilitza `sed` per eliminar la paraula `CITY` de la columna `state`:

```bash
sed 's/ CITY//g' tycho78k.csv > tycho78k_no_city.csv
```

Explicació:
- `s/ CITY//g`: Substitueix ` CITY` (amb un espai abans) per res (elimina-ho) en tot el fitxer.
- `> tycho78k_no_city.csv`: Guarda el fitxer modificat.

**Q6. Crea un nou fitxer en format tsv: el mateix, que en comptes de ser separat per comes ho sigui per tabuladors. (usa tr)**

Utilitza `tr` per substituir les comes per tabuladors:

```bash
tr ',' '\t' < tycho78k.csv > tycho78k.tsv
```

Explicació:
- `tr ',' '\t'`: Substitueix totes les comes per tabuladors.
- `< tycho78k.csv`: Indica que `tycho78k.csv` és el fitxer d'entrada.
- `> tycho78k.tsv`: Guarda el resultat en un fitxer `tycho78k.tsv` en format TSV. 

Amb aquests exemples, pots aplicar diferents filtres i transformacions al teu fitxer `tycho78k.csv` utilitzant comandes senzilles de Linux!

{% endsol %}
