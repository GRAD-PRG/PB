# Programmeren Basis - Deel 01
> **Opmerking**
>
> In dit cursusmateriaal wordt verondersteld dat Visual Studio op je toestel is geïnstalleerd.
>
> In dit cursusmateriaal wordt gewerkt met de *Enterprise* editie, geïnstalleerd op een Windows 10. We raden aan de studenten van onze opleiding dezelfde configuratie aan . De Enterprise versie heeft de meest volledige set aan mogelijkheden.
>
> In het verdiepingsmateriaal vind je meer informatie over de verschillende versies van Visual Studio, en over het downloaden en installeren van Visual Studio.

## 1. Hello World!
### 1.1. Projecten en solutions
Om een *programma*, *applicatie*, *app*, …​, en dit zijn allemaal synoniemen, op te bouwen, creëren we in Visual Studio een *project*. Dit project hoort op zijn beurt tot een *solution*. Een project is een technologische eenheid die deel uitmaakt van het te ontwikkelen systeem. Het resultaat is een applicatie of een applicatie-component.

We kunnen verschillende soorten van projecten creëren. Visual Studio helpt ons daarmee door verschillende templates aan te bieden. Enkele voorbeelden van templates zijn: *console applicaties*, *Windows Forms applicaties*, *WPF applicaties* en *class libraries*.

Meerdere van dergelijke applicatie-componenten kunnen gecombineerd worden om tot een gehele *oplossing* te komen. In dat geval spreken we over een *solution*. Een solution is dus een verzameling en configuratie van alle componenten (als projecten) die deel uit maken van het te ontwikkelen systeem.

De meeste voorbeelden en oefeningen die je in dit cursusmateriaal vindt, zijn opgesteld aan de hand van console applicaties. Dit soort van applicaties heeft beperkte mogelijkheden voor invoer en uitvoer, en zijn bedoeld voor het creëren van opdrachtregel programma’s (*command line applications*). Console applicaties worden in deze cursus ook ingezet vanwege hun eenvoud. De aangebrachte stof kan je echter later ook in andere soorten applicaties (bijvoorbeeld desktop, web of mobile applicaties) gaan inzetten.

### 1.2. Starten vanuit de voorbeeldsolution
Voor de eenvoud vertrekken we in het begin van een reeds opgezet project en solution.

Download het [solution1.zip](attachments/solution1.zip) archief en pak het uit.

> **Opmerking: Wat is een archief (zip) bestand?**
>
> In een archief bestand, bijvoorbeeld met de extensie *zip*, zitten meerdere bestanden en mappen samen verpakt.

> **Tip: Hoe pak ik in Windows een archief uit?**
>
> In *Windows* kan je deze *uitpakken* (*decomprimeren*) door in de *Verkenner* te rechterklikken op de bestandsnaam, en te kiezen voor een optie als *Alles uitpakken…​*…​
>
> Kies in welke map je dit archief wil decomprimeren, klik op *Uitpakken* en open deze doelmap…​

Open het bestand met de *.sln* extensie (bijvoorbeeld door er op te dubbelklikken)…​

![Voorbeeldsolution openen.png](images/Voorbeeldsolution%20openen.png)

Visual Studio opent de solution en het bijhorende project…​

![Voorbeeldsolution geopend in Visual Studio](images/Voorbeeldsolution%20geopend%20in%20Visual%20Studio.png)

Het is in dit bestand (*Program.cs*) dat we onze eigen code gaan verwerken.

> **Tip: Wat doe ik als ik geen broncode krijg te zien?**
>
> Indien je geen broncode te zien krijgt, vouw je in het *Solution Explorer* toolvenster de *Project1* node open (klik op het pijltje naast *Project1*), en dubbelklik je op de naam van ons broncodebestand *Program.cs*.

> **Tip: Waar vind ik de Solution Explorer?**
>
> Vind je de *Solution Explorer* niet terug kies dan in de *View* menu van Visual Studio voor *Solution Explorer*. Doorgaans verschijnt dit toolvenster vervolgens aan de rechterkant van onze ontwikkelomgeving.

> **Opmerking: Waarvoor dient deze Solution Explorer?**
>
> In de *Solution Explorer* krijgen we te zien hoe het broncodebestand *Program.cs* deel uit maakt van het project *Project1*, dat op zijn beurt deel uitmaakt van de solution *Solution1*. Naast het eenvoudigweg openen van broncodebestanden, kan je dit toolvenster gebruiken om de configuratie van je oplossing te *verkennen* of aan te passen.
>
> Je zou er bijvoorbeeld broncodebestanden of projecten kunnen toevoegen, hernoemen, verwijderen, …​ .
>
> Momenteel is dat voor ons nog niet noodzakelijk. Wanneer we later complexere projecten en solutions gaan opzetten, komen we hierop terug.

### 1.3. Een app(licatie) uitvoeren
Om het programma te starten klik je in de werkbalk op de *play knop* (een groene pijl) waarop de projectnaam staat vermeld…​

![Programma uit de voorbeeldsolution starten.](images/Visual%20Studio%20-%20Programma%20starten.png)

> **Tip**
>
> Er bestaan verschillende manieren om programma’s op te starten. Naast het klikken op de play knop kan je in de *Debug* menu van Visual Studio bijvoorbeeld ook kiezen voor *Start Debugging*.

Het programma wordt uitgevoerd en de tekst *Hello World!* wordt op de console gedrukt…​

![Uitvoer van het Hello World voorbeeld](images/Voorbeeldsolution%20-%20Hello%20World%20-%20Uitvoer.png)

> **Opmerking**
>
> We krijgen van de console mee dat het programma succesvol beëindigd werd (*exited with code 0*).

Druk als gebruiker eender welke toets om de console af te sluiten, of gebruik de systeemknop voor het sluiten (het kruisje bovenaan rechts in het venster). Sinds Visual Studio 2019 zal bij het uitvoeren van een console applicatie by default de console niet meer sluiten eens het programma is beëindigd. Vroeger was dit wel het geval, en was het moeilijk op deze wijze de uitvoer af te lezen. Het console scherm sloot immers onmiddelijk na het afdrukken van de tekst *Hello World!*.

> **Opmerking**
>
> Wat hier de *console* wordt genoemd, is het venster -of anders gezegd de uitvoeringsomgeving- in dewelke het programma wordt uitgevoerd.
>
> In het verdiepingsmateriaal vind je meer informatie over deze tekstuele *uitvoeromgeving*.

### 1.4. Wat is een Hello World app?
Programmeercursussen starten typisch met een *Hello World* voorbeeld. Dergelijk voorbeeld illustreert een basishandeling van een programma, namelijk het brengen van uitvoer. In dit geval het brengen van tekst op de *console*.

Vaak brengt men in deze voorbeelden de tekst *Hello World!* naar voor, maar om te illustreren hoe je die uitvoer brengt maakt het niet uit met welke tekst je werkt.

Als je naar de code kijkt begrijpt je allicht hoe ons algoritme het programma de opdracht geeft een *regel te schrijven* (`WriteLine`) die bestaat uit de tekst `"Hello World!"`. Hiervoor zorgt dus de instructie `Console.WriteLine("Hello World!")`.

Het goede bron voor *Hello World* broncode -in zowat elke mogelijke taal- is de [The Hello World Collection (http://helloworldcollection.de)](http://helloworldcollection.de/)

### 1.5. Verdere werkwijze in deze cursus
Voorlopig komen alle instructies van de voorbeelden en oefeningen in dit cursusmateriaal in de `Main` *method* terecht. Meer specifiek tussen diens accolades, of met ander woorden tussen de `{` en `}` symbolen…​

Program.cs

```csharp
using System;

namespace Project1
{
    class Program
    {
        static void Main()
        {
            ... (1)
        }
    }
}
```

1.  hier komt onze eigen code te staan

We kunnen momenteel steeds dezelfde Solution1, Project1 en Program.cs voor het uittesten van de voorbeelden, en het maken van de oefeningen gebruiken.

Indien je een stuk programmacode wil bewaren, kan je bijvoorbeeld de inhoud van het broncode document (of van de `Main` method) kopiëren, en in een ander document opslaan. Daarna kan je eventueel de inhoud van de `Main` method vervangen om met een nieuw voorbeeld of oefening aan de slag te gaan.

Taalelementen als *namespaces* (`namespace` sleutelwoord) en *klassen* (`class` sleutelwoord) mag je momenteel gewoon negeren. We komen hier later uitvoering op terug.

> **Opmerking**
>
> Kan je je toch niet bedwingen, dan vind je in bij de het verdiepingsmateriaal meer informatie terug over:
>
> -   Dot notaties, bijvoorbeeld in `Console.WriteLine` of `Console` *dot* `WriteLine`
>
> -   Using directives (`using` sleutelwoord)
>
> -   Namespaces (`namespace` sleutelwoord)
>
> -   Sleutelwoorden

> **Opmerking**
>
> Ook over het hoe en waarom code te structuren over verschillende methods, klassen, namespace of broncodedocumenten, kan je bij de verdieping nalezen:
>
> -   Code onderdelen of klassen
>
> -   Hoofdmethod Main
>
> -   Meerdere methods in een klasse
>
> -   Een method publiek of privaat definiëren

> **Opmerking**
>
> Ook al bij de verdieping kan je terugvinden hoe we in Visual Studio een nieuwe project creëren.

> **Opmerking: Program.cs**
>
> Technisch gezien kan je sinds *C# 9.0* ook aan de slag met *top-level statements*. Deze komen rechtstreeks (eventueel in combinatie met using directives) in een broncode bestand worden opgenomen.
>
> De omsluitende `Main(string[] args)`, klasse en namespace container wordt er dan op de achtergrond voor ons rond gezet.
>
> Je kan met andere woorden hetzelfde resultaat bereiken met…​
>
> ```csharp
> using System;
>
> ... (1)
> ```
>
> 1.  hier komt onze eigen code te staan
>
> Bijvoorbeeld…​
>
> Program.cs
>
> ```csharp
> using System;
>
> Console.WriteLine("Hello World!");
> ```
>
> Toch zal je al snel gebruik willen maken van extra methods, klassen of namespace. Om die reden is het geen slecht idee, deze voor de volledigheid meteen ook op te nemen.

## 2. Compileren en de IDE
### 2.1. Compiler
Visual Studio maakt gebruik van de C# compiler om onze C# broncode, bij het creëren van een Console Application, om te zetten naar een uitvoerbaar bestand. Onze C# broncode zit doorgaans vervat in een bestand met de .cs extensie. *cs* als logische afkorting voor *See Sharp*, de eerder fonetische schrijfwijze van *C#*. Het uitvoerbaar bestand, ook wel de *executable* of simpelweg de *applicatie* genoemd, zit typisch vervat in een bestand met de .exe extensie.

De compiler is de tool waar wij als ontwikkelaar mee communiceren. We vertellen deze compiler, aan de hand van een *algoritme* (een verzameling van instructies), noem het gerust een *programma*, hoe onze applicatie er uit ziet, hoe het programmaverloop in elkaar zit.

Als de compiler ons, of dus de broncode, verstaat, kan het een applicatie, of een onderdeel voor een applicatie *bouwen*. Men spreekt hier ook wel over het *build* proces. Visual Studio zal na het *compileren*, en dus het bouwen van de Console Application executable, deze executable gaan opstarten in de opdrachtregel-omgeving. Dit op het moment dat we op de *play* knop klikken op de *Standard* werkbalk, of kiezen voor *Start Debugging* in de *Debug* menu-optie.

> **Opmerking**
>
> Je kan het openen van de opdrachtregel-omgeving, en het opstarten van de applicatie, ook manueel verrichten. Visual Studio maakt dit proces voor ons echter vele malen eenvoudiger.

#### 2.1.1. Compilefouten
Als de compiler ons niet verstaat, zal het aan ons vertellen *wat* het niet verstaat, en soms zelfs suggesties maken *hoe* we deze fout kunnen corrigeren. We spreken hier over een *compile-* of *buildfout*. Stel dat we in voorgaand programma, in plaats van de voor gedefinieerde functionaliteit met de naam `WriteLine`, opeens `WwriteLine` proberen aan te roepen. We tikken bijvoorbeeld per ongeluk een *w* te veel. Dan krijgen we in Visual Studio te zien dat de compiler ons niet meer begrijpt.

![Visual Studio - Compilefout - Error List](images/Visual%20Studio%20-%20Compilefout%20-%20Error%20List.png)

##### 2.1.1.1. Error List toolvenster
Het *Error List* toolvenster toon een overzicht van alle compilefouten. Per fout krijgen we een omschrijving, eventuele verbeter suggesties en de locatie (bestand en regel) te zien.

> **Tip**
>
> Dubbelklik je in dit toolvenster op dergelijke foutregel dan verspringt de focus naar de code editor en staat de cursus meteen op de positie waar iets fout is. Dit is uitermate handig in situaties waar we met veel broncode en eventueel veel compilefouten zitten.

> **Tip: Waar kan ik het Error List toolvenster vinden?**
>
> Vind je het Error List toolvenster niet terug, kan kies je in de menu van Visual Studio onder *View* voor *Error List*.
>
> ![Visual Studio - View - Error List](images/Visual%20Studio%20-%20View%20toolvensters.png)

##### 2.1.1.2. Error squiggles
Ook de rode kartellijn (*error squiggle*) onder *wWriteLine* maakt meteen in de code editor duidelijk dat zich daar een fout bevindt.

![Visual Studio - Compilefout - Kartellijn](images/Visual%20Studio%20-%20Compilefout%20-%20Kartellijn.png)

> **Tip**
>
> Hover je met je muisindicator boven deze kartellijn, dan krijg je dezelfde omschrijving van deze fout te zien, als in het *Error List* toolvenster wordt getoond.

##### 2.1.1.3. Quick Action
Merk op dat in geval van compilefouten de code editor soms ook *quick action* lichtbol icoontjes vertoont. Deze bieden ons *vlugge* toegang tot *handelingen* die hopelijk leiden tot verbetering.

![Visual Studio - Compilefout - Quick action](images/Visual%20Studio%20-%20Compilefout%20-%20Quick%20action.png)

Kies je hier effectief voor *Change wWriteLine to WriteLine.* dan zal onze code worden aangepast. Het preview venster visualiseerde dit alvast.

> **Opmerking**
>
> Bij elke nieuwe versie van een taal als C#, zeg dus maar gerust bij elke nieuwe versie van een compiler, verstaat deze andere, nieuwe, eenvoudigere instructies/algoritmes en wordt deze beter in het duidelijk maken wat deze niet verstaat.

### 2.2. Integrated Development Environment
Doorgaans verschijnt het *Error List* toolvenster onderaan links in de Visual Studio IDE. Deze plaatsing valt echter makkelijk aan te passen, je kan bijvoorbeeld het toolvenster aan eender welke kant van het Visual Studio hoofdvenster, of eender welke kant van een ander toolvenster vasthaken (*docking*), of zelf laten drijven (*floating*) boven het hoofdvenster.

Versleep het toolvenster (klik op de titel en beweeg de muisindicator) om deze ergens anders te plaatsen. Als je met de muisindicator sleept boven één van de voorgedefinieerde layout-posities, in het kruis midden op het scherm, dan geeft Visual Studio een preview, in de vorm van een blauw vak, waar dit toolvenster zal terechtkomen.

![Visual Studio - Toolvensters verplaatsen](images/Visual%20Studio%20-%20Toolvensters%20verplaatsen.png)

Of rechterklik op de titel van het toolvenster om de mogelijkheden te verkennen.

> **Opmerking**
>
> Men spreekt wel eens over een *IDE*, of voluit *Integrated Development Environment*. Logisch, want we merken al snel dat deze Visual Studio ontwikkelomgeving een samenhangen is van verschillende tools als: de code editor, de compiler, het *Error List* of *Solution Explorer* toolvenster, …​ .

## 3. Enkele taalelementen
### 3.1. Statements en terminators
*Instructies* worden ook wel eens *statements* genoemd.

```csharp
Console.WriteLine("Hello World!");
```

Bemerk dat instructies worden afgesloten met een `;`. Deze moet duidelijk maken dat dat het einde is van de opdracht die we geven (*statement terminator*).

[Microsoft Docs: Statements (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/statements)

### 3.2. Literals
```csharp
Console.WriteLine("Hello World!");
```

De bedoeling van deze instructie is om de tekst *Hello World!* op de console te drukken, hoe vaak je het programma ook uitvoert.

Omdat deze tekst een onveranderlijk -lees *letterlijk*- stuk tekst is, noemt men dit ook wel een *string literal*. Een *string* is een *aaneenschakeling* van meerdere karakters.

Een string literal plaats men steeds tussen aanhalingstekens (`"…​"`).

Probeer het zelf eens zelf, maak hiervan…​

```csharp
Console.WriteLine("Console.WriteLine();");
```

Voer het programma uit, en merk op dat we een geheel andere uitvoer verkrijgen. Deze keer komt de tekst *Console.WriteLine();* op de console.

> **Tip**
>
> Gelukkig toont Visual Studio tekstuele data in het rood (bij default kleuren-instellingen), dus kan je makkelijk zien of je goed bezig bent qua aanhalingstekens. Als plots de helft van je programma in het rood staat ben je wellicht een afsluitend aanhalingsteken vergeten.
>
> Je kan Visual Studio ook zo configureren dat andere kleuren in de code editor worden gebruikt.

[Microsoft Docs: string (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types#the-string-type)

### 3.3. Commentaar
Wens je je programma’s te verduidelijken dan kan dit aan de hand van *commentaar*.

Volgend voorbeeld zal, net als voorgaande, *Hello World!* afdrukken. Tekst die worden voorafgegaan door `//` wordt geïnterpreteerd als *commentaar* en wordt bijgevolg door de compiler genegeerd.

```csharp
//Volgende instructie
//zorgt voor
//uitvoer...
Console.WriteLine("Hello World!");
```

Indien we het voorbeeld uitvoeren dan krijgen we opnieuw net dezelfde uitvoer…​

```csharp
Hello World!
```

Let echter op, het feit dat commentaar eender wat kan inhouden (gezien het niet wordt gecompileerd, of dus ook niet wordt gecontroleerd), is zowel het sterkste als het zwakste punt van commentaar. Maar al te vaak wordt nagelaten bij aanpassingen van de code, ook de commentaar aan te passen.

Commentaar dat niet up-to-date is, is zinloos en contraproductief.

> **Tip**
>
> Commentaar wordt vaak gebruikt tijdens het aanpassen van code. Soms helpt het bepaalde stukken code tijdelijk uit te schakelen.

> **Opmerking**
>
> Bemerk dat ook lege regels, of overige *whitespace* karakters zoals tabs, genegeerd worden door de compiler, en dus geen enkele invloed hebben op het programmaverloop.

Commentaar die meerdere regels beslaat, kan je ook éénmalig starten met `/*` en afsluiten met `*/`.

```csharp
/* Volgende instructie
   zorgt voor
   uitvoer... */
Console.WriteLine("Hello World!");
```

### 3.4. Variabelen
Om tijdens uitvoer van een programma informatie bij te houden, kan je gebruik maken van variabelen. Bij het introduceren van een variabele wordt geheugen gereserveerd. Dat geheugen wordt gebruikt om de informatie op te slaan.

> **Opmerking**
>
> De meeste algoritmes gaan typisch *informatie* aanpassen en doorsluizen doorheen het programma.

Om welke informatie of waardes gaat het dan?

-   letterlijke waarden die we in ons algoritme nodig hebben, dit noemen we *literal values*

-   resultaten van berekeningen

-   input van de gebruiker

-   waardes ingelezen uit een bestand

-   informatie die binnenkomt over een netwerkverbinding

-   …​

De informatie die een method als `Console.WriteLine` moet afdrukken, kunnen we bijvoorbeeld aan de hand een *variabele* opgeven…​

```csharp
string s = "Hallo";     (1)
Console.WriteLine(s);   (2)

s = "Salut";            (3)
Console.WriteLine(s);   (4)
```

1.  we introduceren een variabele met naam *s* en kennen er de waarde *Hallo* aan toe

2.  we lezen de inhoud van de variabele uit door van zijn naam *s* gebruik te maken

3.  we kennen een nieuwe waarde *Salut* toe aan onze *s* variabele

4.  we kunnen opnieuw onze variabele *s* uitgelezen

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
Hallo
Salut
```

Bij het uitlezen van een variabele verwijs je eenvoudigweg naar zijn naam.

> **Let op**
>
> Laat opvallen dat er bijvoorbeeld geen omsluitende karakters worden ingezet.
>
> Kun je de output van deze code voorspellen en verklaren?
>
> ```csharp
> string s = "Hello World";
> Console.WriteLine(s);
> Console.WriteLine("s");
> ```

#### 3.4.1. Declaratie
Nog voor je er een waarde aan kan toekennen, moet deze *gedeclareerd* worden.

Bij dergelijke *declaratie* vertel je wat…​

-   de naam is van deze variabele

-   het soort van informatie is die je wenst bij te houden

Het soort van informatie, of de vorm die deze kan aannemen, noemen we ook wel het *datatype*.

In dit geval werken we met tekst, bijgevolg kiezen we voor het `string` datatype.

```csharp
string s;
s = "Hallo";
```

Op de declaratieregel kan je ook meteen een waarde aan deze variabele toekennen.

```csharp
string s = "Hallo";
```

Zo meteen bekijken we ook enkele andere datatypes.

#### 3.4.2. Analogie, geheugen en datatype
Een variabele kan je zien als een doosje met een naam op, waarin alleen een bepaald soort van data past. `string s = "Hallo";` levert een doosje op -voor tekstuele informatie- met naam *s* waarin de tekst *Hallo* zit.

De waarde die een variabele aanneemt kan tijdens de uitvoer van het programma *variëren*. In ons voorbeeld eerst de tekst *Hallo*, daarna de tekst *Salut*. Als je er een waarde instopt, blijft die er altijd inzitten, totdat je ze overschrijft met een andere waarde. Bij het toekennen van deze laatste waarde, gaat de eerst uiteraard verloren.

Pas op dat je de analogie tussen variabelen en 'een doosje' niet te ver doortrekt. Als je de inhoud van een echt doosje gebruikt, blijft er een leeg doosje achter. Voor een variabele geldt dit helemaal niet, deze behouden hun waarde! Een echt doosje kan om het even wat bevatten maar een variabele kan enkel data bevatten van een specifieke soort (van een specifiek *datatype*).

#### 3.4.3. Meerdere variabelen en hun naam
Een programma kan gerust meerdere variabelen introduceren en gebruiken.

Kies goeie namen voor je variabelen. *i* en *d* en *s* zijn goed voor kleine prutsprogramma’s, een beter keuze is bijvoorbeeld *leeftijd*, *voornaam* of *gemiddelde*.

> **Opmerking: Naming guideline**
>
> We spreken af de naam van een variabele te starten met een kleine letter (*camelCasing*).
>
> Bestaat de naam uit meerdere woorden, dan start men elk nieuw woord in de samenstelling met een hoofdletter. Bijvoorbeeld *gemiddeldeLeeftijd* of *maxLengte*.

Merk op dat een naam geen spaties mag bevatten.

#### 3.4.4. Gebruiken van de variabele
Je kunt de naam van een variabele op twee manieren gebruiken:

1.  om een andere waarde in de variabele te stoppen, bv. `s = "Salut";`, dit stopt de waarde *Salut* in de variabele `s`

2.  om huidige waarde van de variabele te gebruiken, bv. `Console.WriteLine(s);`, dit lees de waarde van de variabele `s` uit en geeft ze mee met de `WriteLine` method

### 3.5. String, int en double waardes
Vaak gebruikte datatypes zijn:

-   `string`: gebruikt voor tekst

-   `int`: gebruikt voor gehele getallen

-   `double`: gebruikt voor getallen met cijfers na de *komma*

Het datatype geeft aan wat …​

-   wat voor informatie deze variabele kan bevatten

-   welke acties we met deze, of op deze variabele kunnen toepassen

Twee numerieke variabelen bijvoorbeeld, kunnen getallen bevatten die men kan optellen, delen, vermenigvuldigen, enzovoort.

```csharp
int bedrag = 125;
Console.WriteLine(bedrag * 2);            // 250 (verdubbelde bedrag)

double kilometer = 12.3;
Console.WriteLine(kilometer * 0.621371);  // 7,6428633 (mijl)
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
250
7,6428633
```

We kunnen twee waardes van numerieke datatypes combineren met de `*` operator. Het product (resultaat van de vermenigvuldiging) wordt opgeleverd. Straks bespreken we andere *rekenkundige operatoren*.

> **Opmerking: int en double literals**
>
> Bemerk dat *int literals*, vast-opgegeven `int` waardes, in tegenstelling tot *string literals*, niet worden omsloten door aanhalingstekens, maar gewoon bestaan uit een geheel getal. Bijvoorbeeld `125`.
>
> *Double literals*, vast-opgegeven `double` waardes, bevatten een punt als *scheidingssymbool*. Dit symbool scheidt de eenheden van de fracties. Bijvoorbeeld `12.3`.

Bij het afdrukken van de `double` waarde *7,6428633* zie je waarschijnlijk op de console een komma als decimal. Op de console wordt steeds met tekst gewerkt. Geef je bij het aanroepen van een method als `Console.WriteLine` een getal mee, dan wordt deze omgezet in tekst. Deze tekst wordt vervolgens op de console weergegeven. Bij die omzetting wordt bekeken hoe het huidige systeem, waarop de toepassing wordt uitgevoerd, is geconfigureerd. Er wordt opgezocht welk symbool wordt gebruikt voor het scheiden van de eenheden en fracties is ingesteld. Omdat wij (allicht) werken met een komma als scheidingssymbool, wordt het getal ook zo weergegeven. Voer je dit voorbeeld uit onder andere *regional settings* dan krijg je misschien wel een punt te zien.

### 3.6. Static type checking
De compiler helpt ons fouten te vermijden. Bij een poging handelingen uit te voeren die niet ondersteunt zijn, krijgen we *compilefouten*.

Wat als operatie (handeling) al dan is toegelaten, is afhankelijk van het datatype van deze informatie.

Proberen we `string` waardes te combineren met de `*` operator, dan krijgen we een compilefout.

```csharp
string groet = "Hallo";
Console.Write(groet * "wereld");  // compilefout: Operator '*' cannot be applied
                                  //              to operands of type 'string' and 'string'
```

Er is geen ondersteuning voor het combineren van `string` waardes met deze operator. Geen ondersteuning betekent dat nergens is gedefinieerd wat het zou betekenen om `string` waardes hiermee te gaan combineren.

Op `string` waardes kan je dan weer andere operaties uitvoeren, die niet mogelijk zijn met waardes van types als `int` of `double`.

Zo kan je bijvoorbeeld van teksten een hoofdlettervariant opvragen via de `ToUpper` method.

```csharp
string groet = "Hallo";
Console.WriteLine(groet.ToUpper());
Console.WriteLine("wereld".ToUpper());
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
HALLO
WERELD
```

Zoiets is dan weer onmogelijk met int’s of double’s…​

```csharp
int bedrag = 125;
Console.Write(bedrag.ToUpper());      // compilefout: 'int' does not contain
                                      //               a definition for 'ToUpper' ...
Console.Write(125.ToUpper());         // compilefout

double kilometer = 12.3;
Console.Write(kilometer.ToUpper());   // compilefout
Console.Write(12.3.ToUpper());        // compilefout
```

Gelukkig krijgen we meteen bij het compileren foutmelding over onzinnige operaties als `groet * "wereld"` of `kilometer.ToUpper()`. Het zou vervelend zijn als deze code zou compileren, en we pas tijdens uitvoering opmerken dat voor deze operaties geen ondersteuning bestaat. We zouden enkel tijd verliezen, wat de productiviteit niet ten goede zou komen.

#### 3.6.1. Expressies en hun datatype
Voor de compiler is elke waarde die we in onze code uitdrukken van een welbepaald datatype. Dat stukje code dat de waarde uitdrukt wordt ook wel de *expressie* genoemd. Het datatype van die expressie gebruikt de compiler om uit te vissen wat mogelijk is met deze expressie. Om op te zoeken hoe die waarde kan worden ingezet, en bijgevolg ook uit te zoeken of er een foutmelding wordt opgeleverd.

Tot nu toe hebben we gezien hoe we grammaticaal op verschillende plaatsen expressie kunnen gebruiken.

-   Rechts van de toekenningsoperator `=` staat het stukje code die aangeeft *welke waarde* aan de variabele wordt toegekend. Bv. `x = <expressie>`

-   Tussen haakjes bij `WriteLine` staat het stukje code die aangeeft *welke waarde* op de console wordt gedrukt. Bv. `Console.WriteLine(<expressie>)`

-   Links en rechts van de vermenigvuldigingsoperator `*` staat het stukje code die aangeeft *welke waardes* worden vermenigvuldigd. Bv. `x = <expressie1> * <expressie2>`

Tijdens runtime zal steeds worden bekeken naar welke waarde deze expressie evalueert, welke waarde hiermee met andere woorden wordt voorgesteld.

Een expressie kan voorkomen…​

-   in literal vorm, bijvoorbeeld `"Hallo"`

-   in de vorm van het uitlezen van een variabele, bijvoorbeeld `s`

-   in nog complexere vorm als `bedrag * 2` waarbij *subexpressies* gecombineerd worden met operatoren

Zo meteen kijken we hoe we verschillende waardes aan de hand van dergelijke *operatoren* (bijvoorbeeld `*`) kunnen combineren.

> **Opmerking: Datatype van een literal**
>
> Aan de literal vorm kan de compiler herkennen wat het type is van deze waarde…​
>
> -   staat de literal tussen aanhalingstekens dan is dit voor de compiler een `string`
>
> -   bestaat de literal uit een geheel getal dan wordt dit beschouwd als een `int`
>
> -   heeft het getal een punt als separator dan is dit voor de compiler een `double` waarde
>
> -   …​

> **Opmerking: Datatype van een variabele**
>
> Bij het uitlezen van een variabele kan de compiler op basis van de declaratie opzoeken wat het type is van deze waarde. Daarvoor dient ook deze declaratie, we vertellen aan de compiler met welke soort waardes we werken. En vragen hiermee bijgevolg ook ons toe te laten, of te verhinderen, bepaalde operaties uit te voeren.

### 3.7. Berekeningen maken
Naast de `*` operator voor het berekenen van het product, beschikken we ook nog over:

-   de `+` voor het berekenen van de som

-   de `-` voor het berekenen van het verschil

-   de `/` voor het berekenen van het quotiënt

-   de `%` voor het berekenen van de rest na deling

```csharp
int a = 6;
int b = 5;

int som = a + b;
int verschil = a - b;
int product = a * b;
int quotient = b / a;
int restNaDeling = a % b;

Console.WriteLine(som);
Console.WriteLine(verschil);
Console.WriteLine(product);
Console.WriteLine(quotient);
Console.WriteLine(restNaDeling);
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
11
1
30
0
1
```

> **Tip: Hoe bekom ik cijfers na de komma bij een deling?**
>
> Vervang in deze demo eens alle *ints* door *doubles* en probeer de demo opnieuw uit. Bemerk dat voor de `/` operator, afhankelijk van het datatype van de waardes die we combineren, een ander resultaat wordt opgeleverd.
>
> Indien beide *operanden* (waardes die we combineren) uitgedrukt zijn in `int` vorm bekomen we het resultaat van een *gehele deling*. Je kan één gehele keer *2* uit *3* halen…​
>
> ```csharp
> Console.WriteLine(3 / 2);  // levert 1 op
>
> int x = 3;
> Console.WriteLine(x / 2);  // levert 1 op
> ```
>
> Vanaf minstens één operand geformuleerd werd aan de hand van een `double` expressie, bekomen we het resultaat van een *gewone deling*. Waarmee we bedoelen dat er in het resultaat ook cijfers na de *komma* kunnen staan…​
>
> ```csharp
> Console.WriteLine(3.0 / 2);  // levert 1,5 op
>
> double x = 3.0;
> Console.WriteLine(x / 2);    // levert 1,5 op
> ```

> **Opmerking: Datatype van een gecombineerde expressie**
>
> In een gecombineerde expressie worden aan de hand van *operatoren* meerdere waardes met elkaar gecombineerd, bijvoorbeeld `x / 2`.
>
> Het datatype van zo’n gecombineerde expressie, is afhankelijk van hoe deze operator is gedefinieerd. Bij de definitie van operatoren kon men vrij bepalen wat de vorm is in dewelke het resultaat wordt opgeleverd.

#### 3.7.1. Rest na deling
De modulo operator `%` kan handig zijn om na te gaan of een getal even of oneven is…​

``` nowrap
7 % 2 = 1
5 % 2 = 1
8 % 2 = 0
3 % 2 = 1
1 % 2 = 1
```

Bijvoorbeeld om wisselgeld te berekenen kan de operator zijn nut bewijzen…​

```csharp
int bedrag = 19;

int biljettenVan5 = bedrag / 5;
int bedragInBiljettenVan5 = biljettenVan5 * 5;
int muntstukkenVan2 = (bedrag - bedragInBiljettenVan5) / 2;  (1)
int muntstukkenVan1 = (bedrag - bedragInBiljettenVan5) % 2 ; (1)

Console.WriteLine(biljettenVan5);
Console.WriteLine(muntstukkenVan2);
Console.WriteLine(muntstukkenVan1);
```

1.  bemerk het gebruik van meerdere operatoren en de ronde haakjes

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
3
2
0
```

#### 3.7.2. Operator precedence
Merk op hoe we verschillende operatoren kunnen combineren. In één bewerking gebruik maken zowel van de `-` als `/` is bijvoorbeeld geen enkel probleem.

> **Tip**
>
> Test eens uit wat de uitvoer is van voorgaand voorbeeld indien je de haakjes zou verwijderen.

Net zoals in de gewone wiskunde hebben bepaalde operatoren voorrang op andere operatoren. Zo hebben de `*`, `/` en `%` operatoren voorrang op de `+` en `-` operatoren.

In een notatie als `100 x 1,21 + 200 x 1,21` verwachten de meesten onder jullie allicht ook dat hiermee de som wordt bedoeld van *121* en *242* (of `121 + 242`).

Net hetzelfde mag je verwachten van deze code…​

```csharp
Console.WriteLine(100 * 1.21 + 200 * 1.21);  // 363
```

De som van *121* en *242*, of dus *363* wordt opgeleverd.

##### 3.7.2.1. Haakjes
Indien je de leesbaarheid wil bevorderen, of indien je de volgorde wil veranderen, gebruik je ronde haakjes om prioriteiten aan te duiden.

Strikt gezien zijn de haakjes hier overbodig, toch bevorderen ze de leesbaarheid van onze code…​

```csharp
Console.WriteLine((100 * 1.21) + (200 * 1.21));  // 363
```

Wat zou volgend voorbeeld opleveren?

```csharp
Console.WriteLine(100 * (1.21 + 200) * 1.21);
```
