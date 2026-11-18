---
title: "Introductie: wat is geodata?"
teaching: 15 # teaching time in minutes
exercises: 5 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 
- Wat is geodata?
- Welke soorten geodata zijn er?
- Wat is een coördinaatreferentiestelsel (CRS) en waarom is het nodig?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Kunnen uitleggen wat geodata is en hoe het verschilt van andere data.
- Het onderscheid kunnen maken tussen vectordata en rasterdata.
- Kunnen uitleggen waarom een coördinaatreferentiestelsel (CRS) onmisbaar is bij geodata.
::::::::::::::::::::::::::::::::::::::::::::::::

## Leerdoelen

In deze les maak je kennis met geodata. Je leert wat geodata is, welke twee
hoofdsoorten er zijn en waarom het coördinaatreferentiestelsel (CRS) het
fundament onder alle geodata vormt.

## Wat is geodata?

**Geodata** (ook wel *ruimtelijke data* of *geografische data*) is data waarin
naast *wat* er beschreven wordt, ook is vastgelegd *waar* het zich bevindt op
het aardoppervlak. Die locatie is niet als losse tekst vastgelegd ('naast de
kerk', 'in Utrecht'), maar als **coördinaten** in een bekend
referentiestelsel. Daardoor kan een computer de data op de juiste plek op een
kaart tonen en ermee rekenen.

Voorbeelden van geodata in het erfgoeddomein:

- de locaties en contouren van rijksmonumenten;
- de begrenzing van beschermde stads- en dorpsgezichten;
- archeologische vindplaatsen en opgravingsputten;
- historische topografische kaarten;
- een hoogtekaart (zoals het Actueel Hoogtebestand Nederland, AHN) waarop
  grafheuvels of oude dijken zichtbaar worden.

::::::::::::::::::::::::::::::::::::: callout

## Geodata is óók gewoon data

Alles wat je in de eerdere modules hebt geleerd over dataorganisatie en
datakwaliteit geldt ook voor geodata. Een rijksmonument met een locatie heeft
nog steeds een unieke identifier nodig, eenduidige begrippen en goed opgemaakte
datums. Geodata voegt daar één dimensie aan toe: de **locatie**.

::::::::::::::::::::::::::::::::::::::::::::::::

## Twee soorten geodata: vector en raster

Er zijn grofweg twee manieren om de werkelijkheid in geodata vast te leggen.

### Vectordata

Bij **vectordata** worden objecten vastgelegd als geometrieën die zijn
opgebouwd uit coördinaten:

| Geometrietype | Beschrijving | Voorbeeld |
|---|---|---|
| **Punt** | Eén coördinaatpaar (x, y) | De locatie van een rijksmonument of een vondst |
| **Lijn** | Een reeks verbonden punten | Een historische trekvaart, een dijk, een spoorlijn |
| **Vlak** (polygoon) | Een gesloten lijn die een gebied omsluit | De contour van een gebouw of een beschermd stadsgezicht |

Aan elke geometrie hangen **attributen**: gegevens over het object, zoals een
naam, een monumentnummer of een bouwjaar. Vectordata is daarmee eigenlijk een
tabel met een extra kolom voor de geometrie. Dat werken we uit in de les
*Vectordata*.

### Rasterdata

Bij **rasterdata** wordt een gebied opgedeeld in een regelmatig rooster van
cellen (pixels). Elke cel heeft één waarde, bijvoorbeeld een kleur, een hoogte
of een temperatuur. Een gescande historische kaart, een luchtfoto en een
hoogtekaart zijn voorbeelden van rasterdata.

Het verschil met een gewone digitale foto is dat bij een raster als
geodata ook is vastgelegd *waar* het op aarde ligt en hoe groot elke cel in
werkelijkheid is. Daarover meer in de les *Rasterdata*.

::::::::::::::::::::::::::::::::::::: callout

## Vector of raster?

Een vuistregel:

- **Vector** past bij *afgebakende objecten* met duidelijke grenzen en
  eigenschappen: gebouwen, percelen, vindplaatsen, wegen.
- **Raster** past bij *doorlopende verschijnselen* die overal een waarde
  hebben: hoogte, bodemgebruik, en beelden zoals luchtfoto's en gescande kaarten.

::::::::::::::::::::::::::::::::::::::::::::::::

## Het coördinaatreferentiestelsel (CRS)

Een coördinaat zoals `(155000, 463000)` betekent op zichzelf niets. Pas als je
weet in welk **coördinaatreferentiestelsel** (Engels: *Coordinate Reference
System*, CRS) deze getallen staan, weet je welke plek op aarde ermee bedoeld
wordt.

Een CRS legt onder andere vast:

- waar het nulpunt ligt;
- in welke eenheid gemeten wordt (meters of graden);
- hoe het gebogen aardoppervlak op een plat vlak (de kaart) wordt
  weergegeven, de zogenoemde **projectie**.

In Nederland kom je vooral deze twee tegen:

| CRS | EPSG-code | Eenheid | Gebruik |
|---|---|---|---|
| Amersfoort / RD New (Rijksdriehoeksmeting) | `EPSG:28992` | meters | Standaard voor Nederlandse overheidsdata, zoals die van de RCE en PDOK |
| WGS84 | `EPSG:4326` | graden (lengte- en breedtegraad) | Wereldwijd, onder andere gps en veel webkaarten |

De **EPSG-code** is een internationaal nummer waarmee een CRS eenduidig wordt
aangeduid. Zoals je in de module *Datakwaliteitsrichtlijnen RCE* hebt gezien,
hoort het CRS altijd met een EPSG-code bij een dataset te worden vastgelegd.

:::::::::::::::::::::::::::::::::::::::::  challenge

## Oefening: dezelfde plek, andere getallen

De Domtoren in Utrecht ligt ongeveer op:

- `x = 136 750, y = 455 900` in RD New (`EPSG:28992`)
- `lat = 52,0907, lon = 5,1214` in WGS84 (`EPSG:4326`)

1. Waarom zijn deze getallen zo verschillend, terwijl ze dezelfde plek aanduiden?
2. Wat gaat er mis als iemand de RD-coördinaten aanlevert zonder te vermelden
   welk CRS is gebruikt, en de ontvanger ervan uitgaat dat het WGS84 is?

:::: solution

1. Beide stelsels hebben een ander nulpunt en een andere eenheid. WGS84 werkt
   met graden vanaf de evenaar en de nulmeridiaan van Greenwich. RD werkt in
   meters en heeft als middelpunt de Onze-Lieve-Vrouwetoren in Amersfoort. Dat
   punt heeft niet de coördinaten `(0, 0)` gekregen, maar `(155 000, 463 000)`.
   Het nulpunt is dus 155 km naar het westen en 463 km naar het zuiden
   verschoven en ligt in Frankrijk, ten zuidoosten van Parijs. Zo zijn alle
   coördinaten in Nederland positief en is de y-coördinaat altijd groter dan de
   x-coördinaat. Daardoor haal je x en y niet snel door elkaar.
2. Software probeert dan het punt te tekenen op 136 750 graden lengte en
   455 900 graden breedte. Dat bestaat niet: het punt verdwijnt, of komt op een
   volkomen verkeerde plek terecht. Zonder CRS is een coördinaat waardeloos.

::::
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Oefening: wat weet je al?

Bespreek kort met elkaar:

1. Met welke geodata heb je in je werk al te maken (gehad)?
2. Was dat vectordata, rasterdata, of allebei?
3. Welke kaartviewers of GIS-programma's ken je, zoals de Erfgoedatlas,
   PDOK-viewer of QGIS?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Opbouw van deze module

| Les | Onderwerp |
|---|---|
| 2. Rasterdata | Wat rasterdata is, waar het voor gebruikt wordt, en waarom een kaart als geodata meer is dan een plaatje. |
| 3. Vectordata | Vectordata als tabel met een geometriekolom, en ruimtelijke bewerkingen zoals spatial joins en buffers. |

:::::::::::::::::::::::::::::::::::::::: keypoints

- Geodata is data waarin naast *wat* ook *waar* is vastgelegd, met coördinaten in een bekend referentiestelsel.
- Er zijn twee hoofdsoorten geodata: vectordata (punten, lijnen en vlakken met attributen) en rasterdata (een rooster van cellen met waarden).
- Een coördinaat heeft alleen betekenis in combinatie met een coördinaatreferentiestelsel (CRS).
- Leg het CRS altijd vast met een EPSG-code, zoals `EPSG:28992` (RD New) of `EPSG:4326` (WGS84).

::::::::::::::::::::::::::::::::::::::::::::::::::
