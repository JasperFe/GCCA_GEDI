# GCCA-GEDI
 Introductie tot NASA's Global Ecosystems Dynamics Investigation (GEDI).

## Installatie
Voor de verkenning van de GEDI-shots wordt Anaconda gebruikt.

## 1. Installeer Anaconda

We maken gebruik van Anaconda als distributeur van python (versie 3.X) en jupyter lab. Dit is een open-source sofware, waardoor de installatie volledig gratis is. Ga naar website van Anaconda: [https://www.anaconda.com/download/](https://www.anaconda.com/download/).

[Meer informatie? Lees hier over de installatie van Anaconda op windows](https://docs.anaconda.com/free/anaconda/install/windows/).


## 2. Maak een nieuw **conda environment** aan
Anaconda werkt met **conda environments**, wat python-omgeving is met alle noodzakelijke pakketten voor een bepaalde toepassing (op die manier kun je meerdere environments hebben, voor verschillende applicaties). We maken een nieuwe **conda environment** aan, die de noodzakelijke paketten bevat voor deze cursus. 


De [environment.yml]() file is nodig voor deze installatie en bevat de informatie omtrent;
  * de naam van de omgeving
  * de channels waaruit python-pakketten gedownload worden
  * een lijst met alle te installeren paketten.

Ga naar de environment.yml file en download deze naar een folder (bijvoorbeeld C:/Users/UserName/Documents/GCCA_GEDI).


Open de anaconda prompt. 
Typ hierbij volgende code in (lijn per lijn):

```shell
conda config --add channels conda-forge
conda config --set channel_priority strict
cd C:/Users/UserName/Documents/GCCA_GEDI
conda env create -f environment.yml
```  
Vervang C:/Users/UserName/Documents/GCCA_GEDI door het pad naar de map waarin het environment.yml bestand zich bevindt.

De environment en pakketten zullen vervolgens worden geïnstalleerd, dit kan enkele minuten duren. Antwoord met '''y''' indien dit gevraagd wordt.

### 3. Starten van de Jupyter lab

Om jupyter lab te openen en de cursus-*notebooks* te kunnen gebruiken, open de Anaconda prompt opnieuw:

1. Navigeer naar de folder waar je notebooks hebt gedownload:

```shell
cd FOLDER_PAD_NAAR_CURSUSMATERIAAL
```   

Vervang hierbij de FOLDER_PAD_NAAR_CURSUSMATERIAAL door het correct pad.
(bijvoorbeeld: C:/Users/yourusername/Documents/GCCA_GEDI)

2. Activeer de conda environment:

```shell
conda activate GCCA-GEDI
``` 

3. Start Jupyter Notebook
Jupyter notebook is een interactieve omgeving om code te schrijven en te laten lopen. Een voordeel is dat de code kan worden afgewisseld met extra verduidelijkende tekst.

Om jupyter notebook te starten, typ binnen het geactiveerde environment:

```shell
jupyter lab
```  

Een chrome-venster gaat open, waarin de jupyter lab tevoorschijn komt.
