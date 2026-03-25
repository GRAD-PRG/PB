# Programmeren Basis - Deel 13 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Tekstbestanden en exceptions
### Oefening D13srt
Bij het afspelen van video of audio, bijvoorbeeld lokaal of op streaming platformen, kunnen de video- of audiobestand gecombineerd worden met een *.srt* bestand die *ondertiteling* (*subtitles*) bevatten.

Een *.srt* bestand is een eenvoudig tekstbestand die er bijvoorbeeld als volgt uitziet…​

test.srt

```csharp
1
00:02:16,012 --> 00:02:19,376
Senator, we're making
our final approach into Coruscant.

2
00:02:19,482 --> 00:02:21,609
Very good, Lieutenant.

3
00:03:13,336 --> 00:03:15,167
We made it.

4
00:03:18,608 --> 00:03:20,371
I guess I was wrong.

5
00:03:20,476 --> 00:03:22,671
There was no danger at all.
```

De opbouw per *ondertiteling* is als volgt ([WikipediA - SubRip file format](https://en.wikipedia.org/wiki/SubRip#SubRip_file_format)):

1.  Een ***teller*** die het tekstfragment identificeert. De teller start uiteraard op *1*. Elke volgend tekstfragment zal met het volgende gehele getal genummerd worden.

2.  De ***tijdspanne*** waarin het tekstfragment moet worden getoond. Twee *timecodes* worden gescheiden door de tekst *" --&gt; "* (spatie, twee koppeltekens, groter dan teken en spatie). Een *timecode* heeft volgende opbouw: `hours:minutes:seconds,milliseconds`. `hours`, `minutes` en `seconds` steeds in twee cijfers, `milliseconds` aan de hand van drie cijfers.

3.  Het ***tekstfragment*** in één of meerdere lijnen.

4.  Een ***lege regel*** om het einde van het voorafgaande tekstfragment te markeren.

Omdat in video- of audiomateriaal intro’s vaak anders versneden worden (vroeger of later worden ingezet) is de synchronisatie met de ondertiteling vaak zoek. Om hieraan te verhelpen schrijven we een programma dat de *timecodes* in een *.srt bestand* kan aanpassen.

Het programma vraagt de gebruiker welk bestand moet worden aangepast, en leest een (positieve of negatieve) *milliseconden offset* in. De aanpassing wordt doorgevoerd binnen hetzelfde bestand, een backup van de orginele *.srt* is echter weggeschreven (in dezelfde folder als het orginele bestand) naar een bestand met een gelijkaardige benaming met uitbreiding van *".backup"* (bijvoorbeeld *test.srt.backup*).

Bij invoer van het bestandspad *test.srt*, en offset *-1100* bekomen we volgend programmaverloop…​

```csharp
Welke .srt bestand wil u aanpassen?: test.srt
Milliseconden (positief of negatief) offset?: -1100

1
Senator, we're making
our final approach into Coruscant.
Start timecode 00:02:16,012 aangepast in 00:02:14,912.
Einde timecode 00:02:19,376 aangepast in 00:02:18,276.

2
Very good, Lieutenant.
Start timecode 00:02:19,482 aangepast in 00:02:18,382.
Einde timecode 00:02:21,609 aangepast in 00:02:20,509.

3
We made it.
Start timecode 00:03:13,336 aangepast in 00:03:12,236.
Einde timecode 00:03:15,167 aangepast in  00:03:14,067.

4
I guess I was wrong.
Start timecode 00:03:18,608 aangepast in 00:03:17,508.
Einde timecode 00:03:20,371 aangepast in 00:03:19,271.

5
There was no danger at all.
Start timecode 00:03:20,476 aangepast in 00:03:19,376.
Einde timecode 00:03:22,671 aangepast in 00:03:21,571.

De aanpassingen zijn doorgevoerd in: test.srt
De orginele .srt is nog steeds terug te vinden in: test.srt.backup
```

De aanpassingen zien er effectief zo(als het programma meldt) uit…​

test.srt

```csharp
1
00:02:14,912 --> 00:02:18,276
Senator, we're making
our final approach into Coruscant.

2
00:02:18,382 --> 00:02:20,509
Very good, Lieutenant.

3
00:03:12,236 --> 00:03:14,067
We made it.

4
00:03:17,508 --> 00:03:19,271
I guess I was wrong.

5
00:03:19,376 --> 00:03:21,571
There was no danger at all.
```

Let op de *eind-timecode* voor *tekstfragment 3*. Laat daar opvallen dat het om `067` *milliseconden* gaat, en bijvoorbeeld niet `67`. Zorg dat dit bij jouw ook het geval is.

Controleer natuurlijk ook of de orginele informatie nog steeds in het *backup bestand* terug te vinden is.

Breng enkele uitzonderlijke omstandigheden in rekening:

-   Het aan te passen bestand is om één of ander reden niet benaderbaar, het bestand wordt bijvoorbeeld niet teruggevonden.

-   Iets loopt fout bij het maken van het *backup bestand*, het bestaat bijvoorbeeld al.

-   Het aanpassen van het *.srt bestand* lukt niet, het bestand is bijvoorbeeld *read-only*.

> **Tip**
>
> Rechterklik in de *Windows Verkenner* op een bestand en kies in de context-menu voor iets als *Eigenschappen* (of *Properties*). Vink daar het *attribuut 'Read-only'* aan, om dergelijke omstandigheid uit te testen.

In elk geval brengt het programma op zijn minst (vanaf het kan) een foutmelding *"Er treedt een probleem op "* (aangevuld met de *opgevangen exception* `Message`), en begint het programma overnieuw (vragen naar een *.srt pad* en *offset*).

Enkele voorbeelden…​

```csharp
Er treedt een probleem op (interne fout: "Could not find file '...\bestaat-niet.srt'."),
probeer het opnieuw...
```

```csharp
Er treedt een probleem op (interne fout: "The file '...\test.srt.backup' already exists."),
probeer het opnieuw...
```

```csharp
Er treedt een probleem op (interne fout: "Access to the path '...\test.srt' is denied."),
probeer het opnieuw...
```

Je mag uiteraard (indien het eenvoudig in je code te verweven is) ook van meer precieze foutmeldingen gebruik maken. *"Het bestand werd niet gevonden"*, *"de vermelde folder bestaat niet"*, *"het backup bestand bestaat reeds"*, …​ . Zonder dat dan bijvoorbeeld op een Engelstalige *exception* `Message` moet worden teruggevallen.

Zorg dat je programma foutieve timecodes (in een niet erkend formaat) rapporteert, maar verder gaat met het verwerken van timecodes die daar op volgen…​

```csharp
Welke .srt bestand wil u aanpassen?: foutief.srt
Milliseconden (positief of negatief) offset?: -1100

1
Senator, we're making
our final approach into Coruscant.
Start timecode "00:0214,912" wordt niet herkend, en wordt bijgevolg niet aangepast.
Einde timecode 00:02:19,376 aangepast in 00:02:18,276.

2
Very good, Lieutenant.
Start timecode 00:02:19,482 aangepast in 00:02:18,382.
Einde timecode "00:02:2a,509" wordt niet herkend, en wordt bijgevolg niet aangepast.

3
We made it.
Start timecode 00:03:13,336 aangepast in 00:03:12,236.
Einde timecode "00:03:14,67" wordt niet herkend, en wordt bijgevolg niet aangepast.

4
I guess I was wrong.
Start timecode 00:03:18,608 aangepast in 00:03:17,508.
Einde timecode 00:03:20,371 aangepast in 00:03:19,271.

5
There was no danger at all.
Start timecode 00:03:20,476 aangepast in 00:03:19,376.
Einde timecode 00:03:22,671 aangepast in 00:03:21,571.

De aanpassingen zijn doorgevoerd in: test.srt
De orginele .srt is nog steeds terug te vinden in: test.srt.backup
```

Bijvoorbeeld bij…​

foutief.srt

```csharp
1
00:0214,912 --> 00:02:18,276
Senator, we're making
our final approach into Coruscant.

2
00:02:18,382 --> 00:02:2a,509
Very good, Lieutenant.

3
00:03:12,236 --> 00:03:14,67
We made it.

4
00:03:17,508 --> 00:03:19,271
I guess I was wrong.

5
00:03:19,376 --> 00:03:21,571
There was no danger at all.
```
