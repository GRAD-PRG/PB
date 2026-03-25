# Programmeren Basis - Deel 08
## 1. Inleiding tot arrays
Zo goed als elke applicatie werkt met *verzamelingen van gegevens*. Dit kan gaan over verzamelingen van werknemers, artikels of recent geopende documenten, enzovoort …​ .

Enerzijds moeten we de verzameling van waardes -in het geheugen- kunnen bijhouden, anderzijds moeten we op, of met die verzameling allerhande operaties uitvoeren.

Typisch gaan veel programma’s…​

-   Waardes **toevoegen** aan, of **verwijderen** uit een verzameling. *Nieuwe werknemers worden bijvoorbeeld aanvaard of ontslaan.*

-   Waardes **wijzigen** binnen een verzameling. *Het adres van werknemer moet bijvoorbeeld worden aangepast indien zij/hij verhuist.*

-   Waardes in een verzameling **opzoeken**. *Werknemers moeten bijvoorbeeld op basis van hun naam worden opgezocht, om daarna bv. hun informatie te wijzigen.*

-   De volgorde van de waardes in de verzameling aanpassen (**ordenen**). *Bij het tonen van overzichtelijke lijsten helpt het bijvoorbeeld om onze werknemers op naam, adres, …​ te kunnen sorteren.*

Zowel voor het bijhouden van de verzameling waardes, als het voorzien in handelingen als toevoegen, verwijderen, wijzigen, opzoeken, ordenen, …​, kan je aan de slag met ***arrays***.

Arrays beschikken typisch over **meerdere elementen**, soms ook *slots* genoemd. Elk element/slot in deze array kan een waarde uit je verzameling bevatten.

Voorbeeld van een eerste array

Een array met zes `int` elementen, kan zes `int` waardes bijhouden. Bijvoorbeeld de *temperaturen* die op verschillende tijdstippen zijn opgemeten…​

![Temperaturen die op verschillende tijdstippen zijn opgemeten.](images/Array%20-%20Temperaturen.png)

Elke waarde neemt een bepaalde positie aan binnen deze verzameling. Waarde *8* is terug te vinden in het *eerste* element van de array, *10* zit in het tweede element, …​ .

Array variabelen (zoals `temperaturen`) bevatten verwijzingen naar de plaats in het geheugen waarop deze array terug te vinden is. Om die reden werken we vaak in visualisaties als bovenstaande met een *pijltje*.

> **Opmerking**
>
> Een belangrijke technische beperking is dat binnen één array **alle elementen van hetzelfde datatype** zijn.
>
> Een array zou bijvoorbeeld kunnen bestaan uit allemaal `int` elementen, of allemaal `double` elementen, maar geen combinatie van beide.

Ten opzicht van het werken met aparte variabelen, heeft het **werken met een array verschillende voordelen**. Zo kan je tijdens uitvoer beslissen…​

-   hoeveel waardes je wenst bij het houden.

-   welke waarde uit de verzameling je wenst aan te spreken.

Beide voordelen zijn cruciaal. Zonder deze mogelijkheden kan je nooit programma’s bouwen die met een dynamisch aantal (een op voorhand niet gekend aantal) waardes overweg kunnen. Aan de hand van enkele voorbeelden worden deze voordelen verderop in detail geïllustreerd.

In dit voorbeeld zal de array `dagen` ruimte voorzien voor de zeven dagen van de week. Alvast een viertal elementen van deze array worden opgevuld.

```csharp
string[] dagen;                                     // (1)
dagen = new string[7];                              // (2)

dagen[0] = "maandag";                               // (3)
dagen[1] = "dinsdag";
//...
dagen[5] = "zaterdag";
dagen[6] = "zondag";

Console.WriteLine("De 6de dag is: " + dagen[5]);    // (4)
Console.WriteLine("Aantal dagen: " + dagen.Length); // (5)
```

1.  Declaratie van de array-variabele.

2.  Creatie van de array, en initialisatie van de array-variabele.

3.  Toekennen van een waarde aan het eerste slot.

4.  Uitlezen van het 6de element.

5.  Opvragen van het aantal elementen.

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
De 6de dag is: zaterdag
Aantal dagen: 7
```

De lengte, of het aantal elementen van een array, kan je makkelijk bevragen aan de hand van de **Length property**.

Alle overige aspecten uit voorgaand voorbeeld, het werken met array-variabelen, het aanmaken van de array zelf, het benaderen van elementen, …​, worden hierna in detail besproken.

> **Opmerking**
>
> We spreken in onze uitleg zowel over ***de*** *array* als ***het*** *array*. Beide mogelijkheden worden in taalreferenties vermeld, en we slaagden er zelf ook niet in een keuze te maken 😉.

## 2. Array variabelen
Om in de code gebruik te kunnen maken van een array moeten we naar deze array kunnen wijzen. We gebruiken hiervoor de *array-variabele*.

Net als overige variabelen moet je deze declareren voor gebruik.

Het datatype van de elementen van deze array wordt in de declaratie gevolgd door vierkante haken.

Voorbeelden van declaraties van array-variabelen

-   `int[] lottoGetallen` om te kunnen verwijzen naar een array van `int` elementen

-   `string[] dagen` in het geval van een verzameling van `string` waardes

Let goed op de vierkante haakje. Het gaat hier niet om de mogelijkheid één `int` of `string` te bewaren, maar een verzameling van verschillende `int` of `string` waardes.

> **Belangrijk**
>
> Let op, het is niet omdat er een *array-variabele* bestaat, dat er ook sprake is van een array. De declaratie zorgt voor de mogelijkheid te wijzen naar een array, maar het aanmaken van de array is een aparte stap.

Voorbeeld

Plaats je ons voorgaand voorbeeld een breakpoint op de tweede regel, en voer de code tot daar uit…​

![Er is nog geen array.](images/Array%20-%20null.png)

De array-variabele staat op `null`, wat aangeeft dat er nog geen sprake is van een array. De variabele is er wel, maar wijst nog niet naar een array.

Zet je een breakpoint een regel verder, en voer je uit tot daar…​

![Er is wel een array.](images/Array%20-%20Niet%20null.png)

Dan zie je hoe er wel sprake zal zijn van dergelijke array. In dit geval een array met zeven elementen van type `string`.

### Samen declareren en initialiseren
Je kan uiteraard code als…​

```csharp
string[] dagen;
dagen = new string[7];
```

Ook op één regel uitschrijven…​

```csharp
string[] dagen = new string[7];
```

Declareren en initialiseren van de array-variabele mag met andere woorden op één regel gebeuren.

### Namen van onze array-variabelen.
Doorgaans wordt met de naam van een array-variabele in **meervoudsvorm** verwezen naar de inhoud van deze array.

Zo spreken we over ***dagen*** omdat er meerdere dagen in de array worden bijgehouden. Of ***lottoGetallen*** omdat het over meerdere getallen gaat.

## 3. Datatypes en hun defaultwaardes
De defaultwaardes van de datatypes die we tot dus ver hebben gebruikt zijn:

-   **`0`** voor `int`, `double` en alle overige numerieke datatypes

-   **`false`** voor `bool`

-   **`null`** voor `string`

-   **`'\0'`** (*een speciaal null symbool*) voor `char`

-   **`null`** voor *array datatypes*

Een variabele van voorvermeld datatype zal tijdens uitvoer, nog voor je er een waarde aan zou toekennen, deze defaultwaarde bevatten.

Defaultwaarde van een double

Neem je in volgend voorbeeld een breakpoint op om de code te onderbreken nog vóór een waarde aan `getal` wordt toegekend…​

```csharp
double getal;

getal = 12.3;  // (1)
```

1.  Neem hier een breakpoint op.

Dan krijg je in *debugmodus* te zien hoe deze variabele op dat moment tijdens uitvoer op *0* komt te staan…​

![Defaultwaarde van een double.](images/Double%20-%20Defaultwaarde.png)

### 3.1. Null en reference types
Net als array-datatypes is het `string` datatype een zogenaamde *reference type*.

Dit maakt dat een variabele van dergelijke *reference type* ofwel niets (`null`) bevat, ofwel een verwijzing (*referentie*) naar de plaats in het geheugen waar deze informatie terug te vinden is.

Om die reden visualiseren we hier vaak de inhoud van dergelijke variabele met een pijltje.

Stel dat we over volgende code zouden beschikken…​

```csharp
string a = "b";
int c = 123;
```

Dan zouden we dat zo kunnen visualiseren…​

![Reference types vs value types.](images/Reference%20types%20vs%20Value%20types.png)

De `c` variabele is van het type `int` wat een zogenaamd *value type* is. Daar worden variabelen rechtstreeks aan hun waarde gekoppeld. In onze visualisaties daarvan gebruiken we dan ook geen pijltjes.

Het werken met *reference types* kan bepaalde voordelen opleveren. Later komen we uitvoerig terug op deze *reference types*, en hun voordelen.

## 4. Array initializers
We geven in onze code aan over *hoeveel elementen* een array moet beschikken. Tijdens uitvoer wordt geheugenruimte voor deze elementen voorzien.

Zo werd in ons voorgaand voorbeeld de mogelijkheid voorzien zeven namen van dagen op te nemen.

Het opgeven van het aantal elementen kan op twee manieren:

-   Je kan bij het aanmaken van een **nieuwe lege array** *expliciet opgegeven over hoeveel slots de array moet beschikken*. Bijvoorbeeld `string[] dagen = `**`new string[7]`**.

-   Je kan, door op te lijsten over welke waardes een **nieuwe meteen opgevulde array** moet beschikken, *impliciet duidelijk maken hoeveel elementen aanwezig zijn*. Bijvoorbeeld `string[] namen = `**`{ "Jan", "Piet", "Rita" }`**. Door *drie waardes* op te lijsten maak je duidelijk dat *drie slots* vereist zijn.

Bij het creëren van een *nieuwe array* moet je dus een onderscheid maken tussen twee situaties:

-   We kennen (op de plaats in onze code waar we de array-variabele willen introduceren) de waardes voor onze array nog niet. In dat geval wensen we een *nieuwe lege array*.

-   We weten op voorhand reeds over welke waardes onze array moet beschikken. Waarbij we vertrekken van een *(nieuwe) opgevulde array*.

### 4.1. Nieuwe lege array
Om een nieuwe array te creëren maken we typisch gebruik van het `new` sleutelwoord. Na `new` vermeld je het datatype van de elementen. Op dit datatype volgt, tussen vierkante haken, het aantal elementen van deze array.

Voorbeelden van array initializers voor lege arrays

-   `int[] lottoGetallen = `**`new int[6]`** voor een array met *6* `int` elementen

![Array - Defaultwaardes van een int array.](images/Array%20-%20Defaultwaardes%20van%20een%20int%20array.png)

-   `bool[] voorwaardes = `**`new bool[3]`** voor een array met *3* `bool` elementen

![Array - Defaultwaardes van een bool array.](images/Array%20-%20Defaultwaardes%20van%20een%20bool%20array.png)

-   `int x = 4; string[] namen = `**`new string[x]`** voor een array met *x aantal* (of dus *4*) `string` elementen

![Array - Defaultwaardes van een string array.](images/Array%20-%20Defaultwaardes%20van%20een%20string%20array.png)

> **Opmerking**
>
> Elk element van een nog niet opgevulde array is op de defaultwaarde ingesteld van het elementtype van deze array.
>
> Alle zes elementen van de `new int[6]` array bijvoorbeeld zijn initieel met waarde *0* opgevuld.

### 4.2. Meteen opgevulde array
Je kan meteen tijdens creatie van een nieuwe array-instantie opgeven welke waardes aan de verschillende elementen worden toegekend.

Je gebruikt hiervoor accolades. Tussen accolades vermeld je voor elke slot de initiële waarde.

Voorbeeld van een array initializer voor een meteen opgevulde arrays

We kennen reeds alle namen van de verschillende *maanden* in een jaar.

```csharp
string[] maanden = new string[12] { "jan", "feb", "mrt", "apr",
                                    "mei", "jun", "jul", "aug",
                                    "sep", "okt", "nov", "dec" };
```

Het geeft hier bijvoorbeeld geen nut voor elke maandnaam een aparte toekenning in te zetten…​

```csharp
string[] maanden = new string[12];
maanden[0] = "jan";
maanden[1] = "feb";
//...
maanden[11] = "dec";
```

Deze code is meer omslachtig om op te stellen, én is minder leesbaar.

Je kan tussen accolades ook van variabele expressies gebruik maken…​

Nog een voorbeeld van een array initializer voor een meteen opgevulde arrays

Het maximum aantal *dagen* is voor elke maand op voorhand geweten.

Voor *februari* echter moeten we opletten, daar zijn we afhankelijk van het `jaar`…​

```csharp
int jaar = 2020;

int dagenFeb = 28;
if (jaar % 400 == 0 || jaar % 4 == 0 && jaar % 100 != 0) {
    dagenFeb = 29;
}

int[] dagen = new int[12]{ 31, dagenFeb, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
```

Omdat we op voorhand niet weten of het om *28* of *29* dagen gaat, verwijzen we eenvoudigweg naar onze variabele `dagenFeb`.

#### Verkorte notatie en type inference
Een stukje code als…​

`string[] namen = new string[`**`3`**`]{ "Jan", "Piet", "Rita" };`

…​kan ook als…​

`string[] namen = `**`new string[]`**`{ "Jan", "Piet", "Rita" };`

Het aantal elementen (de *3*) is alvast weggelaten. Op basis van het aantal waardes, opgelijst tussen accolades, is immers duidelijk hoeveel slots worden voorzien.

Het vermelden van deze *3* heeft weinig nut, of het zou zijn om expliciet te benadrukken dat het om een array met *3* elementen zal gaan.

Op een declaratieregel kan het nog korter, we kunnen ook het `new string[]` gedeelte weglaten…​

`string[] namen = { "Jan", "Piet", "Rita" };`

Ook het elementtype voor deze array kan worden afgeleid (*type inference*). Deze keer op basis van het datatype van onze array-variabele, en het datatype van de tussen accolades vermelde waardes.

Indien de toekenning, aan de array-variabele, niet op de declaratieregel gebeurt, moet je op zijn minst aangeven wat het type van de creëren array is…​

```csharp
string[] namen;                                 // (1)
...
//namen = { "Jan", "Piet", "Rita" };            // (2)
namen = new string[] { "Jan", "Piet", "Rita" }; // (3)
```

1.  Op deze regel wordt de array-variabele enkel gedeclareerd.

2.  Dit kan niet, de compiler geeft te weinig informatie om te begrijpen welk type array je wenst te creëren.

3.  Vermeld op zijn minst het `new string[]` gedeelte.

## 5. Aanspreken van array-elementen
Om in de code een array-element aan te spreken kan je na de naam van de array-variabele tussen vierkante haakjes de *index* plaatsen van het desbetreffende element.

De *index* is een rangnummer dat aangeeft wat de positie is van dat element binnen de tabel.

Voorbeeld van het benaderen van array-elementen

In volgend voorbeeld lezen we het *eerste*, *derde* en *laatste* element uit van de array `werkdagen`…​

```csharp
string[] werkdagen = { "maandag", "dinsdag", "woensdag", "donderdag", "vrijdag" };

int index;

index = 0;
Console.WriteLine(werkdagen[index]);

index = 2;
Console.WriteLine(werkdagen[index]);

index = werkdagen.Length - 1;
Console.WriteLine(werkdagen[index]);
```

Geeft onze volgende uitvoer…​

```csharp
maandag
woensdag
vrijdag
```

> **Opmerking: Werk steeds met de Length property.**
>
> In plaats van de expressie `werkdagen.Length - 1` hadden we hier ook gebruik kunnen maken van de literal `4`. De uitvoer had niet anders geweest.
>
> Toch is dat niet aan te raden. Je zou die regel code immers zo afhankelijk maken van het gegeven dat er zich 5 elementen in de array bevinden. Wordt onze *werkweek* aangepast naar *maandag tot en met donderdag*, dan zou je van die `4` een `3` moeten maken.
>
> Had je echter gewerkt met `werkdagen.Length - 1`, dan hoefde er niets te veranderen.

Om het eerste element te benaderen maak je gebruik van de laagste index (ook wel ***lowerbound*** genoemd). Deze is steeds `0`. *Resistance is futile!* 😉

De hoogste index (ook wel ***upperbound*** genoemd) is één minder dan het aantal elementen.

> **Opmerking: Uitlezen van slots kan quasi ogenblikkelijk.**
>
> De waardes in een array staan in het geheugen allemaal naast elkaar.
>
> Alle slots hebben overigens dezelfde omvang (evenveel bits) omdat elk slot voorzien is voor een waarde van hetzelfde datatype. In een `ìnt[]` (lees *int array*) bijvoorbeeld zijn alle elementen van type `int` (op niveau van .NET ook wel `Int32` genoemd). Wat maakt dat elke element 32 bits volume (in het geheugen steeds volgend op het voorgaand element) in beslag neemt.
>
> Een array-variabele bevat een *verwijzing* (ook wel *referentie* genoemd) naar het beginadres van het totale blok geheugen (waarin alle bitsgewijze representaties voor de opeenvolgende elementen ook opeenvolgend in dat geheugenblok zijn opgenomen).
>
> Vraag je naar het eerste element van een array, met iets als `eenIntArray[0]` bijvoorbeeld, dan zal op *0 offset* van het beginadres gekeken worden naar de eerstvolgende 32 bits. `eenIntArray[3]` zal dan bijvoorbeeld op *3 keer 32bits offset ten opzicht van het beginadres* kijken naar de daar gevonden 32 bits.
>
> Dit soort van benaderingstechniek maakt het *adresseren* (*aanspreken van*) array-elementen natuurlijk razendsnel. Quasi ogenblikkelijk. Het vereist natuurlijk wel dat alle elementen van hetzelfde datatype zijn. Of met andere woorden evenveel bits beslaan.

### 5.1. Opvullen van een array-element
Naast het uitlezen van array-elementen, kan je aan de hand van een index uiteraard ook opgeven aan welke element je een waarde wenst toe te kennen.

Voorbeeld van het benaderen van array-elementen

Om de inhoud van elementen van de `labels` array aan te passen, kennen we aan deze elementen (bijvoorbeeld op indices *0* en *1*) een nieuwe waarde toe.

```csharp
string[] labels = { "Jan", "Piet" };

labels[0] = "Pol";
labels[1] = "Rita";

Console.WriteLine(labels[0]);  // (1)
Console.WriteLine(labels[1]);  // (2)
```

1.  Pol

2.  Rita

### 5.2. Gelijkenis met het opvragen van karakters uit een 'string'
Het werken met *indices* bij arrays doet je allicht denken aan het werken met een *index* bij het benaderen van een karakter (`char`) van een `string`.

Ook daar was het zo dat het eerste element, het eerste karakter in dat geval, te bereiken via *index 0*.

> **Waarschuwing**
>
> Een string lijkt in sommige opzichten op een `char[]` maar ze zijn niet hetzelfde want er zijn ook belangrijke verschillen!

Voorbeeld van arrays vs strings

Net zoals bij een array, kan je bij een `string` het aantal elementen, het aantal karakters in dat geval, met de `Length` property bevragen…​

```csharp
int[] getallen = { 1, 2, 3 };

Console.WriteLine($"Eerste waarde uit de array: {getallen[0]}");
Console.WriteLine($"Laatste waarde uit de array: {getallen[getallen.Length - 1]}");

Console.WriteLine("Aanpassen van de tweede waarde van de array...")
getallen[1] = 20;
Console.WriteLine($"De tweede waarde uit de array is nu: {getallen[1]}");
```

Geeft…​

```csharp
Eerste waarde uit de array: 1
Laatste waarde uit de array: 3
Aanpassen van de tweede waarde van de array...
De tweede waarde uit de array is nu: 20
```

Het aanpassen van één karakter van een `string` is niet mogelijk.

```csharp
string tekst = "abc";

Console.WriteLine($"Eerste karakter uit de string: {tekst[0]}");
Console.WriteLine($"Laatste karakter uit de string: {tekst[tekst.Length - 1]}");

Console.WriteLine("Aanpassen van het tweede karakter van de string kan niet!")
//tekst[1] = 'd';  // (1)
```

1.  Zou een compilefout opleveren.

Geeft…​

```csharp
Eerste karakter uit de string: a
Laatste karakter uit de string: c
Aanpassen van het tweede karakter van de string kan niet!"
```

Zoals reeds eerder aangehaald is het `string` datatype *immutable*.

### 5.3. IndexOutOfRange exceptions
Let ook bij het aanspreken van array-elementen op voor een *off by one* fout…​

Voorbeeld van een IndexOutOfRange exception

Spreek je (per ongeluk) een element aan op een index kleiner dan *0* (lager dan de *lowerbound*), of hoger dan de *lengte + 1* (hoger dan de *upperbound*) dan treedt een `IndexOutOfRange` exception op…​

```csharp
int[] getallen = { 1, 2, 3 };

Console.WriteLine(getallen[-1]); // (1)
Console.WriteLine(getallen[3]);  // (2)
```

1.  De gebruikte index is lager dan de lowerbound ⇒ IndexOutOfRangeException

2.  De gebruikte index is hoger dan de upperbound ⇒ IndexOutOfRangeException

![Array - IndexOutOfRange exception.](images/Array%20-%20IndexOutOfRangeException.png)

Merk ook de *exception details* op. Ook daar wordt aangegeven dat de index *buiten de grenzen* (*"outside the bounds"*) valt.

## 6. Itereren met for en foreach
### 6.1. Dynamisch elementen benaderen
Op basis van een *index* bepaal je als programmeur welk slot van de array je wenst te benaderen.

Vaak gebeurt dit aan de hand van een `int` literal, bijvoorbeeld `werkdagen[2]`. De `int` literal `2` legt vast dat je het *derde element* wenst aan te spreken.

Het gebruik van een `int` literal -tussen de vierkante haken- is niet de enigste mogelijkheid. Men kan net zo goed aan de hand van een variabele `int` expressie bepalen wil slot van de array je wenst te benaderen.

Voorbeeld van het dynamisch benaderen van array-elementen

In ons vorig voorbeeld is je misschien opgevallen hoe we het *eerste*, *derde* en *laatste* element op exact dezelfde wijze benaderen…​

```csharp
string[] werkdagen = { "maandag", "dinsdag", "woensdag", "donderdag", "vrijdag" };

int index;

index = 0;
Console.WriteLine(werkdagen[index]); // (1)

index = 2;
Console.WriteLine(werkdagen[index]); // (1)

index = werkdagen.Length - 1;
Console.WriteLine(werkdagen[index]); // (1)
```

1.  Telkens wordt op exact dezelfde wijze een bepaald element, op een bepaalde index aangesproken.

Toch krijgen we telkens een andere waarde te zien. Het voorbeeld geeft onze volgende uitvoer…​

```csharp
maandag
woensdag
vrijdag
```

Het is de waarde van onze `index` variabele die bepaald welk slot van de array wordt aangesproken.

Dit levert ons een *dynamische wijze* op om elementen te benaderen. Tijdens uitvoer van de code wordt, op basis van de variabele expressie, bepaald welk slot wordt uitgelezen.

> **Opmerking: Array vs aparte variabelen**
>
> Merk op dat dit aan de hand van aparte variabelen onmogelijk is.
>
> ```csharp
> string werkdag1 = "maandag";
> string werkdag2 = "dinsdag";
> string werkdag3 = "woensdag";
> string werkdag4 = "donderdag";
> string werkdag5 = "vrijdag";
>
> Console.WriteLine(werkdag1);
> Console.WriteLine(werkdag3);
> Console.WriteLine(werkdag5);
> ```
>
> Indien je met aparte variabelen werkt, die elke een unieke naam (moeten) hebben, heb je geen andere mogelijkheid dan het specifiek aanspreken van die ene of die andere variabele. Dit op basis van hun eigen (unieke) naam.

Voorbeeld van het dynamisch benaderen op basis van invoer

Uiteraard kan je ook basis van een ingevoerde positie (*positie*) beslissen welk slot in de array aan te spreken.

```csharp
string[] werkdagen = { "maandag", "dinsdag", "woensdag", "donderdag", "vrijdag" };

Console.Write("Positie van de werkdag?: ");
int positie = int.Parse(Console.ReadLine());

if (positie >= 1 && positie <= werkdagen.Length)
{
    int index = positie - 1;
    string werkdag = werkdagen[index];
    Console.WriteLine($"Werkdag {positie} is {werkdag}.");
}
```

Indien de gebruiker *3* invoert, wordt hiervan `index` *2* gemaakt, en krijgen we *woensdag* als werkdag…​

```csharp
Positie van de werkdag?: 3
Werkdag 3 is woensdag.
```

Bij een `positie` kleiner dan *1*, of groter of gelijk aan het aantal elementen, vermijden we een `IndexOutOfRange` exception.

### 6.2. Itereren met for
Een belangrijk voordeel van het werken met een array -in vergelijking met losse variabelen- is de mogelijkheid dezelfde operatie uit te kunnen voeren op alle, of een aantal, van deze array-elementen.

Voorbeeld van iteratieve benadering met for

Als we in onderstaand voorbeeld de elementen van de array `zenders` willen uitlezen, kunnen we herhaaldelijk een element op een bepaalde variabele `index` in de array aanspreken.

Doen we dit voor elke waarde die `index` aanneemt in het *index-bereik* van deze array (van lowerbound *0* tot upperbound *3*) dan benaderen we zo elke array-element.

```csharp
string[] zenders = new string[4];
zenders[0] = "mozaïek";
zenders[1] = "Eén";
zenders[2] = "Canvas/Ketnet";
zenders[3] = "VTM";

for (int index = 0; index < zenders.Length; index++)
{
    Console.WriteLine(zenders[index]);
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
mozaïek
Eén
Canvas/Ketnet
VTM
```

> **Opmerking**
>
> Het kan ook met een `do while` of `while`…​
>
> ```csharp
> int index = 0;
> while (index < zenders.Length)
> {
>     Console.WriteLine(zenders[index]);
>     index++;
> }
> ```
>
> Maar omdat je hier weet hoeveel iteraties er zijn (evenveel als het aantal elementen) is een `for` logischer.
>
> Er is hier sprake van een soort van *tellervariabele* (onze `index`). Je weet perfect waar deze *teller* moet starten (bij de *lowerbound*), welke stap hij onderneemt (hier `+ 1`), en wat de waarde voor deze *teller* tijdens de laatste iteratie moet zijn (*upperbound*).
>
> Geen enkel ervaren programmeur zou hier een voor `while` (of `do while`) kiezen. In onze `for` is elk deelaspect van onze herhaling (*van waar*, *tot waar* en *met welke stap*) immers meer geconcentreerd (samen in de *hoofding* terug te vinden) en zo beter leesbaar.

Het iteratief benaderen van de elementen van een array kan je ook gebruiken om de array op te vullen. Of met andere woorden aan de array-elementen een waarde toe te kennen.

Voorbeeld van iteratieve benadering met for om de array op te vullen

In volgende code wordt een array `veelvouden` opgevuld met tien veelvouden van *5*.

Voor elk volgend element in de array, vanaf index *0* tot *9*, wordt…​

-   aan dat element een bepaald `veelvoud` toegekend

-   het `veelvoud` alvast verhoogd voor de volgende iteratie (voor het volgend array-element)

```csharp
int[] veelvouden = new int[10];
int veelvoud = 5;

// opvullen:
for (int index = 0; index < veelvouden.Length; index++)
{
    veelvouden[index] = veelvoud;
    veelvoud += 5;
}

// afdrukken:
for (int index = 0; index < veelvouden.Length; index++)
{
    Console.Write(veelvouden[index] + " ");
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
5 10 15 20 25 30 35 40 45 50
```

Hetzelfde resultaat zou je kunnen bereiken zonder een variabele als `veelvoud` steeds met *5* te moeten verhogen.

De waarde voor onze array-elementen kan hier immers worden gebaseerd op de positie (`index`). Het vierde element (op index *3*) bijvoorbeeld moet worden opgevuld met *4* (`index + 1`) keer *5*.

```csharp
int[] veelvouden = new int[10];

// opvullen:
for (int index = 0; index < veelvouden.Length; index++)
{
    veelvouden[index] = 5 * (index + 1);
}

// afdrukken:
for (int index = 0; index < veelvouden.Length; index++)
{
    Console.Write(veelvouden[index] + " ");
}
```

Indien we het voorbeeld uitvoeren bekomen we dezelfde output.

### 6.3. Itereren met foreach
Om op eenvoudige wijze, van voor naar achter, alle elementen van een array uit te lezen kunnen we ook gebruik maken van een `foreach` statement.

Aan de *elementvariabele* wordt steeds een kopie van het volgende array-element toegekend. Je code geeft zo, door de eenvoud, beter aan *wat het met elke waarde doet*, in tegenstelling tot code die naast dit *wat* ook nog doorweven is met code die aangeeft *hoe we elke waarde bekomen*.

Voorbeeld van iteratieve benadering met foreach

In dit voorbeeld gaan we alle elementen van de array `woorden` benaderen, en elke waarde afdrukken.

```csharp
string[] woorden = { "dit", "is", "een", "test" };

foreach (string woord in woorden)
{
    Console.Write(woord + " ");
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
dit is een test
```

Deze `foreach` is een verkorte notatie voor…​

```csharp
for (int index = 0; index < woorden.Length; index++)
{
    string woord = woorden[index];
    Console.Write(woord + " ");
}
```

In de `in` clausule geef je aan over welke array je wenst te itereren.

> **Opmerking**
>
> De elementvariabele (`woord` in dit voorbeeld) moet gedeclareerd worden in de hoofding van `foreach`, en moet van hetzelfde datatype zijn als de elementen van de array.
>
> Bij een `string[]` (*string array*) als in dit voorbeeld kiezen we dus voor onze elementvariabele `woord` type `string`.

Je hoeft geen gebruik meer te maken van een *index-variabele* die je over het *index-bereik* van de array laat lopen. Je code wordt niet meer vervuild met technische details als *lower*- en *upperbounds* van de array.

Op die manier is een `foreach`, in vergelijking met een gewone `for`, beter leesbaar.

#### Enkel van voor naar achter uitlezen van alle elementen
Een `for` kan vaak door een beter leesbare `foreach` vervangen worden, maar er zijn een aantal beperkingen:

-   je benadert er altijd **alle elementen** mee

-   je benadert ze altijd van **voor naar achter**

-   je kan de **elementen enkel uitlezen**

Wens je de waardes in de array te veranderen, wil je ze niet allemaal aanspreken, of is het noodzakelijk dat dit in een andere volgorde gebeurt dan van *voor naar achter*, dan is een `foreach` bijgevolg niet bruikbaar.

Voorbeeld waar we niet met een foreach kunnen werken

Stel dat we de waardes in de `getallen` array wensen te verdubbelen.

Indien je een poging onderneemt om aan de *elementvariabele* een waarde toe te kennen, levert de compiler ons een fout op.

```csharp
int[] getallen = { 1, 2, 3, 4, 5 };

// aanpassen
foreach (int getal in getallen)
{
    getal = getal * 2;  // (1)
}
```

1.  Compilefout: Cannot assign to 'getal' because it is a 'foreach iteration variable'.

Omdat de elementvariabele telkens met een kopie werkt van een volgend array-element, zou je ten hoogste deze kopie kunnen veranderen. Omdat de code dan echter zou suggereren dat ook het array-element zelf wordt aangepast, gaat de compiler ons dit verhinderen.

Uiteraard zou je met een gewone `for` wel kunnen…​

```csharp
int[] getallen = { 1, 2, 3, 4, 5 };

// aanpassen
for (int index = 0; index < getallen.Length; index++)
{
    getallen[index] = getallen[index] * 2;
}

// afdrukken
foreach (int getal in getallen)
{
    Console.Write(getal + " ");
}
```

Deze keer zijn de waardes effectief verdubbeld.

## 7. Elementwaardes samenstellen
Bij het opvullen van een element van een array kan je de nieuwe waarde voor dit element baseren op een andere element van de array.

Voorbeeld van opvullen elementen op basis van andere elementen

In dit voorbeeld willen we array `machten` opvullen met de eerste tien machten van *2*, van *2* tot de *1e*, tot en met *2* tot de *10de*.

Het eerste element wordt statisch ingesteld op *2*. Alle hieropvolgende elementen, van index *1* tot en met *9*, worden ingesteld op de waarde van het voorgaande element vermenigvuldigd met *2*. Het voorgaande element is het element op een index die *1* kleiner is dan de *doelIndex*.

```csharp
int[] machten = new int[10];

machten[0] = 2;
for (int doelIndex = 1; doelIndex < machten.Length; doelIndex++)
{
    machten[doelIndex] = machten[doelIndex - 1] * 2;
}

foreach (int macht in machten)
{
    Console.Write(macht + " ");
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
2 4 8 16 32 64 128 256 512 1024
```

De meeste programmeurs zouden `doelIndex` allicht gewoon `index` noemen. Hier werd voor de duidelijkheid toch maar even de naam *doel-index* gebruikt, om verwarring met de *bron-index* (hier `doelIndex - 1`) te vermijden.

## 8. Dynamische grootte van een array
In alle voorbeelden tot dusver, werd in de array-initializer aan de hand van een `int` literal bepaald hoeveel slots in de array werden voorzien. Bijvoorbeeld: `new string[`**`6`**`]` zal een array met ruimte voor *6* elementen aanmaken.

Je kan echter ook een variabele `int` expressie inzetten om dat aantal te bepalen!

Bijvoorbeeld: `new int[`**`x`**`]`, waarbij `x` een `int` variabele is. De actuele waarde van variabele `x` tijdens de uitvoering zal bepalen hoeveel slots worden voorzien.

Op die manier wordt dus *pas tijdens de uitvoering* bepaalt hoe groot de array moet zijn. Vandaar dat we het een *dynamische* grootte noemen.

Voorbeeld van een array met een dynamische grootte

Zo zal in volgend programma -tijdens uitvoering- de inhoud van de `aantal` variabele gebruikt worden om te bepalen over hoeveel elementen de array `namen` moet beschikken.

We zullen het array opvullen met namen die de gebruiker invoert…​

```csharp
// Vraag de gebruiker om het aantal gasten
Console.Write("Hoeveel gasten zijn er : ");
int aantal = int.Parse(Console.ReadLine());

string[] namen = new string[aantal]; // (1)

// Vul de array op met de namen van de gasten die de gebruiker invoert
for (int idx = 0; idx < namen.Length; idx++)
{
    Console.Write($"Geef de naam van gast #{idx+1} : ");
    string naam = Console.ReadLine();
    namen[idx] = naam; // (2)
}

// Toon nog eens alle namen
Console.WriteLine("De gasten die we verwachten zijn :");
foreach (string naam in namen)
{
    Console.WriteLine(naam);
}
```

1.  Er werd geen literal maar een variabele gebruikt om het aantal elementen te bepalen.

2.  Hier wordt de naam in de array gestopt.

Zoals je ziet staat er nergens in het programma neergeschreven hoe groot de array zal zijn (noch bij het aanmaken, noch in de loop voorwaarde).

Indien we het voorbeeld uitproberen en de gebruiker *3* namen wil ingeven krijgen we de volgende uitvoering…​

```csharp
Hoeveel gasten zijn er : 3
Geef naam #1 : Jan
Geef naam #2 : Miet
Geef naam #3 : Joris
De gasten die we verwachten zijn :
Jan
Miet
Joris
```

De array zal beschikken over *3* slots, en elk element werd netjes opgevuld met een `string`.

## 9. Zoeken in arrays
Regelmatig valt het voor dat je wil nagaan of een bepaalde waarde zich al dan niet in een bepaalde array bevindt.

Indien de array ongesorteerd is, heb je niet veel ander opties dan element voor element vergelijken met je *zoekwaarde*.

Voorbeeld van een lineaire zoekmethode

Stel dat je in een verzameling `steden` wil nagaan of een bepaalde `opTeZoekenStad` aanwezig is…​

```csharp
string[] steden = { "Brussel", "Antwerpen", "Hasselt",
                    "Brugge", "Kortrijk", "Gent" };

do
{
    Console.Write("Stad?: ");
    string opTeZoekenStad = Console.ReadLine();

    int index = 0;                               // (2)
    bool gevonden = false;
    while (!gevonden && index < steden.Length)
    {
        if (steden[index].ToLower() == opTeZoekenStad.ToLower())
        {
            gevonden = true;
        }
        else
        {
            index++;                             // (1)
        }
    }

    if (gevonden)
    {
        Console.WriteLine("De stad werd teruggevonden.");
    }
    else
    {
        Console.WriteLine("De stad werd niet gevonden.");
    }
    Console.WriteLine();
} while (true);
```

1.  Telkens bekijken we het volgende element (op een `index` die *1* hoger is).

2.  Vertrekken doen we bij het eerste element (op `index` *0*).

Bij invoer van de waardes *Gent*, *Hasselt* en *Damme* bekomen we bijvoorbeeld…​

```csharp
Stad?: Gent
De stad werd teruggevonden.

Stad?: Hasselt
De stad werd teruggevonden.

Stad?: Damme
De stad werd niet gevonden.

Stad?:
```

Merk op dat we niet alleen stoppen met zoeken op het moment dat onze `index` te groot wordt (aan de hand van de voorwaarde `index < steden.Length`), maar ook indien de waarde reeds werd `gevonden`.

Door een `bool` variabele als `gevonden` op `true` in te stellen, op het moment dat op de `index` positie onze *zoekwaarde werd gevonden*, kunnen we niet alleen de herhaling afbreken, maar ook achteraf dit resultaat opnieuw uitlezen. Onze `if` maakt immers opnieuw gebruik van deze `gevonden` variabele.

Deze zoekmethod wordt ook wel het ***lineair zoeken*** genoemd. Deze aanpak is niet erg goed schaalbaar.

> **Opmerking**
>
> Het lineair zoeken is niet bijster krachtig, niet goed schaalbaar.
>
> Bij een miljoen elementen, ga je in het slechtste geval een miljoen keer de waarde van dat element moeten vergelijken met je *zoekwaarde*. Indien het aantal elementen verdubbeld, ga je in het slechtste geval ook dubbel zoveel waardes moeten vergelijken.
>
> Wens je bijgevolg regelmatig te zoeken in een ongesorteerde verzameling, dan valt het te overwegen of het niet beter is eerst de array te gaan sorteren. In geordende verzamelingen kan je immers vlugger waardes terugvinden.
>
> Uiteraard wordt aan dat ordenen ook veel tijd verloren. Maar soms, afhankelijk van het aantal keer dat je wil zoeken, kan dit toch opleveren.
>
> Later hebben we het over het sorteren van verzamelingen.

> **Opmerking: Oneindige herhaling**
>
> Een *oneindige loop* werd gecreëerd door in de `while` clausule van het `do while` statement te werken met `true` als voorwaarde.
>
> Deze `true` zorgt ervoor dat altijd voldaan zal zijn aan de voorwaarde die bepaald nogmaals de body van deze herhaling uit te voeren.
>
> In realiteit worden *oneindige herhalingen* zelden gebruikt, maar voor onze voorbeelden en oefeningen komt het wel eens van pas.

### 9.1. Zoeken naar een positie
Soms ben je ook geïnteresseerd in de positie (*index*) waarop een bepaalde *zoekwaarde* wordt teruggevonden.

Bijvoorbeeld omdat je die waarde wil aanpassen of verwijderen.

Voorbeeld van het zoeken naar een positie

Ons voorgaand voorbeeld was daar eigenlijk reeds op voorzien. Eens de *zoekwaarde* werd teruggevonden werd `index` niet meer verhoogd.

`index` kan bijgevolg eenvoudigweg worden uitgelezen om de positie te bevragen…​

```csharp
int index = 0;
bool gevonden = false;
while (!gevonden && index < steden.Length)
{
    if (steden[index].ToLower() == opTeZoekenStad.ToLower())
    {
        gevonden = true;
    }
    else
    {
        index++;
    }
}

if (gevonden)
{
    Console.WriteLine($"De stad werd teruggevonden op index {index}."); // (1)
}
...
```

1.  `index` levert ons de positie op

#### Array.IndexOf
Aan de hand van een voorgedefinieerde `Array.IndexOf()` kan je ook erg makkelijk de positie van een bepaalde waarde in een array terugvinden.

Bij het aanroepen van deze functionaliteit geef je aan in welke array wordt gezocht, en wat de zoekwaarde is…​

Voorbeeld van het zoeken naar een positie met Array.IndexOf

```csharp
int index = Array.IndexOf(steden, opTeZoekenStad);
bool gevonden = (index >= 0);

if (gevonden)
{
    Console.WriteLine($"De stad werd teruggevonden op index {index}.");
}
...
```

`Array.IndexOf()` levert ofwel…​

-   de *index* op van het element waarop de zoekwaarde werd gevonden

Ofwel…​

-   *-1* indien de zoekwaarde niet werd gevonden

Indien de `index` bijgevolg *groter is of gelijk aan 0* is onze zoekwaarde `gevonden`.

#### Array.LastIndexOf
`Array.IndexOf` zoekt naar het eerste voorkomen van de zoekwaarde. `Array.LastIndexOf` zoekt naar het laatste voorkomen.

Voorbeeld van het zoeken met Array.IndexOf en LastIndexOf

Indien waardes verschillende keren voorkomen in een array ,leveren `Array.IndexOf` en `Array.LastIndexOf` een andere waarde op…​

```csharp
string[] steden = { "Brussel", "Gent", "Antwerpen", "Gent" };

Console.WriteLine(Array.IndexOf(steden, "Gent"));
Console.WriteLine(Array.LastIndexOf(steden, "Gent"));
```

Geeft als uitvoer…​

```csharp
1
3
```
