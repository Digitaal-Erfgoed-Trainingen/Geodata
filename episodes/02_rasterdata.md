---
title: "Rasterdata"
teaching: 10 # teaching time in minutes
exercises: 10 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 
- Wat is rasterdata?
- Waar wordt rasterdata doorgaans voor gebruikt?
- Wat is het verschil tussen een kaart als geodata en een gewoon plaatje van een kaart?
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives
- Kunnen uitleggen wat rasterdata is en hoe het is opgebouwd.
- Voorbeelden kunnen noemen van rasterdata in het erfgoeddomein.
- Kunnen uitleggen waarom een CRS ervoor zorgt dat verschillende kaartlagen en tijdlagen precies op elkaar passen.
::::::::::::::::::::::::::::::::::::::::::::::::

## Leerdoelen

In deze korte les maak je kennis met rasterdata. In de rest van de module
werken we niet met rasterdata, maar het is belangrijk om te herkennen wat het is
en waar je het tegenkomt. Je ziet bovendien goed aan rasterdata wat geodata
anders maakt dan 'gewone' afbeeldingen.

## Wat is rasterdata?

Rasterdata is een **rooster van cellen** (pixels), net als een digitale foto.
Elke cel heeft precies één waarde. Wat die waarde betekent, hangt af van de
dataset:

- bij een **luchtfoto** of **gescande kaart** is het een kleur;
- bij een **hoogtekaart** is het de hoogte van het landschap in meters;
- bij een **bodem- of landgebruikskaart** is het een code voor een categorie
  (bijvoorbeeld `1 = bos`, `2 = akker`).

Het belangrijkste kenmerk van een raster is de **resolutie**: hoe groot één
cel in werkelijkheid is. Het AHN heeft bijvoorbeeld cellen van 0,5 bij 0,5
meter; een satellietbeeld kan cellen van 10 bij 10 meter hebben. Hoe kleiner de
cel, hoe meer detail, maar ook hoe groter het bestand.

::::::::::::::::::::::::::::::::::::: callout

## Veelgebruikte bestandsformaten

Rasterdata kom je vaak tegen als **GeoTIFF** (`.tif`). Dat is een gewone
TIFF-afbeelding met extra informatie over de ligging op aarde en het CRS. Via
internet worden rasters vaak aangeboden als kaartdienst, bijvoorbeeld als
**WMS** of **WMTS**, zodat je de bestanden niet zelf hoeft te downloaden.

::::::::::::::::::::::::::::::::::::::::::::::::

## Waar wordt rasterdata voor gebruikt?

In het erfgoeddomein wordt rasterdata vooral gebruikt als **achtergrond** en
als **bron voor onderzoek**:

- **Historische kaarten**: gescande topografische kaarten laten
  zien hoe een gebied er vroeger uitzag, bijvoorbeeld waar een verdwenen
  boerderij, molen of gracht lag.
- **Luchtfoto's**: actuele en historische luchtfoto's tonen veranderingen in het
  landschap en kunnen grondsporen van archeologische resten
  zichtbaar maken.
- **Hoogtekaarten (AHN)**: kleine hoogteverschillen verraden grafheuvels,
  terpen, oude dijken of verdedigingswerken.
- **Analyses van het landschap**: bijvoorbeeld zichtlijnen of
  verwachtingskaarten voor archeologie.

## Meer dan een plaatje: de Topotijdreis

Met [Topotijdreis][topotijdreis] kun je door de tijd heen kijken naar
topografische kaarten van Nederland. Je kunt inzoomen op een plek en vervolgens
met een schuifbalk wisselen tussen kaarten uit verschillende jaren.

:::::::::::::::::::::::::::::::::::::::::  challenge

## Oefening: verken de Topotijdreis

1. Open [Topotijdreis][topotijdreis] en zoom in op een plek die je goed
   kent, bijvoorbeeld je woonplaats of een monument waar je aan werkt.
2. Schuif door de tijd. Wat is er in de loop van de jaren veranderd? Wat is
   juist hetzelfde gebleven?
3. Zoom verder in en uit en wissel opnieuw van jaartal. Wat valt je op aan de
   ligging van wegen, waterlopen en gebouwen ten opzichte van elkaar?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  challenge

## Oefening: geodata of gewoon een stapel plaatjes?

De Topotijdreis bestaat uit een reeks gescande kaarten: in feite plaatjes. Toch
noemen we het geodata.

Wat is het verschil tussen de Topotijdreis en een map met gescande kaarten
van verschillende jaren die je naast elkaar op je scherm bekijkt?

:::: solution

Elk raster in de Topotijdreis is **geodata**: voor elke kaart is
vastgelegd in welk **coördinaatreferentiestelsel (CRS)** hij staat, waar hij op
aarde ligt en hoe groot elke pixel in werkelijkheid is. Alle kaarten worden
in hetzelfde CRS getoond.

Daardoor:

- blijft een locatie op **dezelfde plek** als je inzoomt of uitzoomt: de
  software weet bij elke zoomstap welke coördinaten bij welke pixel horen;
- blijft een locatie op **dezelfde plek** als je wisselt tussen kaarten uit
  verschillende jaren: een kerk die in 1900 en in 2000 op dezelfde plek staat,
  ligt op beide kaarten precies over elkaar heen;
- kun je de rasters **combineren met andere geodata**, bijvoorbeeld een
  vectorlaag met rijksmonumenten, en die valt dan op de juiste plek;
- kun je **meten**: afstanden en oppervlaktes op de kaart komen overeen met de
  werkelijkheid.

Losse gescande plaatjes hebben alleen pixels zonder plek op aarde. Bij elk
plaatje is de schaal en uitsnede anders, dus om twee jaartallen te vergelijken
moet je zelf zoeken waar je bent. Je kunt ze niet stapelen, niet over elkaar
leggen met andere data en er niet op meten.

::::
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Rasterdata is een rooster van cellen waarin elke cel één waarde heeft, zoals een kleur of een hoogte.
- De resolutie (de celgrootte in werkelijkheid) bepaalt hoeveel detail een raster bevat.
- In het erfgoeddomein wordt rasterdata vooral gebruikt voor historische kaarten, luchtfoto's en hoogtekaarten.
- Een raster als geodata is meer dan een plaatje: dankzij het CRS blijft een locatie op dezelfde plek bij in- en uitzoomen en bij het wisselen tussen kaarten uit verschillende jaren.

::::::::::::::::::::::::::::::::::::::::::::::::::

[topotijdreis]: https://www.topotijdreis.nl/
