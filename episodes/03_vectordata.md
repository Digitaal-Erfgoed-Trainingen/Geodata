---
title: "Vectordata"
teaching: 30 # teaching time in minutes
exercises: 20 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 
- Wat is vectordata en hoe is het opgebouwd?
- Hoe verhoudt vectordata zich tot de tabulaire data uit de eerdere modules?
- Welke ruimtelijke bewerkingen kun je met vectordata uitvoeren?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Kunnen uitleggen dat vectordata een tabel is met een extra geometriekolom en metadata over het CRS.
- De drie geometrietypen (punt, lijn, vlak) en veelgebruikte bestandsformaten kunnen herkennen.
- Kunnen uitleggen wat een spatial join en een buffer zijn en weten wanneer je ze inzet.
::::::::::::::::::::::::::::::::::::::::::::::::

## Leerdoelen

In deze les gaan we dieper in op vectordata, de soort geodata waar je in de
praktijk het meest mee zult werken. Je ziet dat vectordata eigenlijk gewoon
tabulaire data is, zoals je die kent uit de eerdere modules, met één extra
kolom. En je leert welke bewerkingen pas mogelijk worden als je data een
locatie heeft.

## Vectordata is een tabel met een geometriekolom

In de module *Dataorganisatie in spreadsheets* heb je geleerd dat elke rij een
waarneming is en elke kolom een eigenschap. Vectordata werkt precies zo. Het
enige verschil is dat er een **extra kolom** is waarin de **geometrie** van elk
object staat.

Een vectorlaag met rijksmonumenten ziet er bijvoorbeeld zo uit:

| monumentnummer | naam | bouwjaar | categorie | geometry |
|---|---|---|---|---|
| 36142 | Domtoren | 1382 | Religieuze gebouwen | `POINT (136862 455854)` |
| 518563 | Molen De Ster | 1739 | Molens | `POINT (134680 455610)` |
| 36257 | Paushuize | 1517 | Woonhuizen | `POLYGON ((136940 455720, 136975 455720, 136975 455690, 136940 455690, 136940 455720))` |


- De kolommen `monumentnummer`, `naam`, `bouwjaar` en `categorie` heten de
  **attributen**. Dit is gewone tabulaire data. Alles uit de eerdere modules
  (unieke identifiers, één gegeven per cel, referentielijsten) blijft gelden.
- De kolom `geometry` bevat de **geometrie**: de vorm en ligging van het
  object, opgebouwd uit coördinaten. Hierboven is die geschreven als
  **Well-Known Text (WKT)**, een leesbare standaardnotatie voor geometrieën.

### Metadata over het CRS

De coördinaten in de geometriekolom hebben, zoals je in de introductie zag, pas
betekenis als je weet in welk CRS ze staan. Daarom hoort bij een vectorlaag
altijd **metadata** waarin het CRS is vastgelegd, bijvoorbeeld `EPSG:28992`.

Het CRS wordt niet per rij opgeslagen, maar **één keer voor de hele laag**.
Alle geometrieën in één laag staan dus in hetzelfde CRS.

Samengevat bestaat vectordata uit:

1. een **tabel met attributen**;
2. een **geometriekolom**;
3. **metadata** met (ten minste) het CRS.

::::::::::::::::::::::::::::::::::::: callout

## Eén kolom voor de geometrie: is dat geen fout?

In de eerdere modules leerde je om niet meer dan één gegeven in een cel te
zetten. Een geometrie bestaat uit veel coördinaten. Is dat dan geen fout?

Nee. De geometriekolom heeft een speciaal **geometrie-datatype** dat
GIS-software begrijpt, net zoals een datumkolom een eigen datatype heeft.
Zet je in een gewone spreadsheet wél `136862, 455854` als tekst in één cel, dan
is dat wel een fout: de spreadsheet ziet dat als tekst en niet als locatie.
Gebruik daar twee aparte kolommen `x` en `y`, en leg het CRS vast in je
documentatie.

::::::::::::::::::::::::::::::::::::::::::::::::

### Geometrietypen

Binnen één laag hebben alle objecten in principe hetzelfde geometrietype:

| Type | WKT-voorbeeld | Erfgoedvoorbeeld |
|---|---|---|
| Punt | `POINT (x y)` | Vondst, monumentlocatie, boorpunt |
| Lijn | `LINESTRING (x1 y1, x2 y2, ...)` | Trekvaart, dijk, landweer |
| Vlak | `POLYGON ((x1 y1, x2 y2, ..., x1 y1))` | Gebouwcontour, archeologisch monument, beschermd gezicht |

Daarnaast bestaan er *multi*-varianten (`MULTIPOINT`, `MULTILINESTRING`,
`MULTIPOLYGON`) voor objecten die uit meerdere losse delen bestaan,
bijvoorbeeld een buitenplaats met meerdere percelen.

Merk op dat het eerste en laatste punt van een polygoon gelijk zijn: het vlak
moet **gesloten** zijn. Dat is een van de eisen uit de dimensie *ruimtelijke
geometrische correctheid* van de RCE-richtlijnen.

### Bestandsformaten

| Formaat | Extensie | Kenmerken |
|---|---|---|
| **GeoPackage** | `.gpkg` | Open standaard, één bestand, kan meerdere lagen bevatten. Aanbevolen. |
| **Shapefile** | `.shp` (+ `.shx`, `.dbf`, `.prj`, ...) | Formaat van Esri (begin jaren '90). De specificatie is openbaar, waardoor bijna alle GIS-software het ondersteunt. Oud maar veelgebruikt. Bestaat uit meerdere bestanden die bij elkaar horen; het CRS staat in het `.prj`-bestand. Kolomnamen maximaal 10 tekens. |
| **GeoJSON** | `.geojson` | Tekstformaat, populair op het web. Volgens de standaard altijd in WGS84. |
| **CSV met coördinaten** | `.csv` | Gewone tabel met `x`/`y`- of WKT-kolom. Het CRS moet je apart vastleggen. |

::::::::::::::::::::::::::::::::::::: callout

## Het `.prj`-bestand

Krijg je een shapefile zonder `.prj`-bestand, dan weet de software niet in
welk CRS de data staat. Vraag dan altijd bij de aanleverancier na welk CRS is
gebruikt, in plaats van te gokken.

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  challenge

## Oefening: herken de onderdelen

Stel, je ontvangt een bestand `vondsten.csv` met de volgende inhoud:

| vondstnummer | materiaal | x | y |
|---|---|---|---|
| V001 | aardewerk | 136810 | 455870 |
| V002 | glas | 136825 | 455861 |
| V003 | metaal | 136790 | 455902 |

1. Welke kolommen zijn attributen en welke kolommen bevatten de geometrie?
2. Welk geometrietype hebben deze objecten?
3. Welk onderdeel van vectordata ontbreekt nog, en waarom is dat een probleem?

:::: solution

1. `vondstnummer` en `materiaal` zijn attributen. `x` en `y` vormen samen de
   geometrie.
2. Punten: elke vondst heeft één coördinaatpaar.
3. Het **CRS** ontbreekt. Aan de getallen te zien is het waarschijnlijk RD New
   (`EPSG:28992`), maar dat is een aanname. Zonder vastgelegd CRS kan
   software de punten niet betrouwbaar op de kaart zetten en kun je ze niet
   veilig combineren met andere data.

::::
::::::::::::::::::::::::::::::::::::::::::::::::::

## Ruimtelijke bewerkingen

Omdat vectordata een tabel is, kun je alle bewerkingen doen die je ook op
tabulaire data doet: filteren, sorteren, groeperen en koppelen op basis van
een gemeenschappelijke sleutel. Maar omdat elke rij ook een geometrie heeft,
komt daar een hele reeks **ruimtelijke bewerkingen** bij. Juist die maken
geodata zo krachtig.

::::::::::::::::::::::::::::::::::::: callout

## Ruimtelijke relaties

Ruimtelijke bewerkingen zijn gebaseerd op vragen over hoe geometrieën ten
opzichte van elkaar liggen. De belangrijkste zijn:

- **intersects**: raken of overlappen de geometrieën elkaar?
- **within / contains**: ligt de ene geometrie helemaal binnen de andere?
- **touches**: raken ze elkaar alleen aan de rand?
- **distance**: hoe ver liggen ze uit elkaar?

::::::::::::::::::::::::::::::::::::::::::::::::

### Spatial join (ruimtelijke koppeling)

In de module over relationele databases heb je tabellen gekoppeld op basis van
een gemeenschappelijke sleutel, zoals een monumentnummer. Een **spatial join**
koppelt tabellen niet op een sleutel, maar op **ligging**.

*Voorbeeld:* je hebt een laag met rijksmonumenten (punten) en een laag met
gemeentegrenzen (vlakken). Met een spatial join op *within* krijgt elk monument
de attributen van de gemeente waarin het ligt, zoals de gemeentenaam, ook al
staat de gemeentenaam nergens in de monumentendata.

| monumentnummer | naam | geometry | → | gemeentenaam |
|---|---|---|---|---|
| 36142 | Domtoren | `POINT (...)` | → | Utrecht |

Je kunt daarna weer gewone tabulaire bewerkingen doen, zoals tellen hoeveel
monumenten er per gemeente zijn.

### Buffer

Een **buffer** is een zone op een vaste afstand rond een geometrie. Een buffer
rond een punt is een cirkel; een buffer rond een lijn is een 'worst'; een
buffer rond een vlak is een vergroot vlak.

*Voorbeelden:*

- Welke archeologische vindplaatsen liggen binnen 100 meter van een geplande
  nieuwe weg? Maak een buffer van 100 meter rond de weg en kijk welke
  vindplaatsen erin vallen.
- Een beschermingszone van 50 meter rond een monument.

::::::::::::::::::::::::::::::::::::: callout

## Buffers en het CRS

Een buffer van "100" betekent 100 *eenheden van het CRS*. In RD New zijn dat
meters, maar in WGS84 zijn dat **graden**: een buffer van 100 graden beslaat
een groot deel van de aardbol! Zorg er daarom voor dat je data in een CRS
in meters staat, zoals RD New, voordat je met afstanden en oppervlaktes gaat
rekenen.

::::::::::::::::::::::::::::::::::::::::::::::::

### Andere ruimtelijke bewerkingen

Naast de spatial join en de buffer zijn er nog veel meer ruimtelijke
bewerkingen. Een paar die je vaak tegenkomt:

- **Clip**: een laag afknippen op de grens van een andere laag.
- **Intersectie**: het overlappende deel van twee lagen bepalen, inclusief de
  attributen van beide lagen.
- **Dissolve**: geometrieën met dezelfde attribuutwaarde samenvoegen tot één
  geometrie (het ruimtelijke equivalent van *groeperen*).
- **Afstand en nabijheid**: de afstand tussen objecten berekenen of het
  dichtstbijzijnde object vinden (*nearest neighbour*).
- **Geometrische eigenschappen**: de oppervlakte, lengte of het zwaartepunt
  (*centroid*) van een geometrie berekenen.

:::::::::::::::::::::::::::::::::::::::::  challenge

## Oefening: welke bewerking past?

Welke ruimtelijke bewerking(en) zou je gebruiken om de volgende vragen te
beantwoorden?

1. In welke provincie ligt elk rijksmonument?
2. Welke monumenten liggen binnen 250 meter van een windturbine?
3. Hoeveel vondsten zijn er per opgravingsput gedaan?

:::: solution

1. Een **spatial join** van de monumenten (punten) met de provinciegrenzen
   (vlakken), op basis van *within*.
2. Een **buffer** van 250 meter rond de windturbines, gevolgd door een
   selectie of spatial join op *intersects* met de monumenten. Zorg dat beide
   lagen in een CRS in meters staan.
3. Een **spatial join** van de vondsten (punten) met de opgravingsputten
   (vlakken), op basis van *within*. Daarna **groeperen** per put en het
   aantal vondsten tellen, net als bij gewone tabulaire data.

::::
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  challenge

## Opdracht

<!-- FIXME: opdracht nog uitwerken -->

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Vectordata is tabulaire data met een extra geometriekolom en metadata over het CRS.
- Geometrieën zijn punten, lijnen of vlakken; vlakken moeten gesloten zijn.
- Gebruik bij voorkeur GeoPackage; bij een shapefile staat het CRS in het `.prj`-bestand.
- Ruimtelijke bewerkingen koppelen en bewerken data op basis van ligging. Een spatial join koppelt lagen op ligging in plaats van op een sleutel; een buffer maakt een zone op een vaste afstand rond een object.
- Reken met afstanden en oppervlaktes altijd in een CRS met meters, zoals RD New.

::::::::::::::::::::::::::::::::::::::::::::::::::
