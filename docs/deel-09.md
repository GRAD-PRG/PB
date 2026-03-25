# Programmeren Basis - Deel 09
## 1. Vroegtijdig een loop beëindigen
Vaak moeten we een string of array overlopen op zoek naar een bepaald onderdeel, bijvoorbeeld:

1.  nagaan of een string minstens 1 leesteken bevat

2.  checken of een string uit alleen maar hoofdletters bestaat

3.  zoeken op welke positie in een array een bepaalde waarde voorkomt

4.  beslissen of een array negatieve getallen bevat (minstens eentje dus)

Dit wordt telkens opgelost met **een loop die alle elementen één voor één overloopt** en checkt :

1.  is dit karakter een leesteken?

2.  is dit karakter een hoofdletter?

3.  is dit getal de gezocht waarde?

4.  is dit getal negatief?

De *loop body* bevat dus ook **een *if* die de check doet**.

Bij al deze voorbeelden is het echter zo, dat we eigenlijk kunnen **stoppen met zoeken** zodra we een bepaald soort element gevonden hebben :

1.  stoppen zodra we een leesteken tegenkomen

2.  stoppen zodra we een karakter vinden dat geen hoofdletter is

3.  stoppen zodra we het gezochte getal tegenkomen

4.  stoppen zodra we een negatief getal tegenkomen

Het heeft immers geen zin om de overige elementen te checken, want we hebben al gevonden wat we zochten.

Ons programma zal efficiënter omgaan met de *CPU cycles* als we niet onnodig blijven verder zoeken. In korte lijsten van 10'000 elementen valt dit niet op voor de gebruiker, computers zijn rap genoeg. Maar in lange lijsten van miljoenen elementen (of als je meermaals per seconde in een korte lijst moeten zoeken) maakt dit wel een verschil. Bovendien kost het soms ook geld : als je programma op een server '*in the cloud*' draait, dan betaal je voor elke seconde uitvoeringstijd van die virtuele CPU.

Voorbeeld

Hernemen we eens [de oefening van D07bevatleesteken](../deel-07-oefeningen/deel-07-oefeningen.html#_oefening_d07bevatleesteken), waar de gebruiker een tekst kon invoeren en het programma dan toonde of de tekst wel/niet een leesteken bevat.

De oplossing zag er zo uit :

```csharp
Console.Write("Geef een tekst : ");
string input = Console.ReadLine();

bool leestekenGevonden = false;
foreach (char c in input) {       // (1)
    if (Char.IsPunctuation(c)) {  // (2)
        leestekenGevonden = true; // (3)
    }
}

if (leestekenGevonden) {
    Console.WriteLine("De tekst bevat minstens 1 leesteken.");
} else {
    Console.WriteLine("De tekst bevat geen enkel leesteken.");
}
```

1.  de *foreach loop* die ervoor zorgt dat we elk karakter te pakken krijgen

2.  de *if* die voor elk karakter nagaat of het een leesteken is

3.  een leesteken werd gevonden, we zouden nu eigenlijk al kunnen stoppen met de zoektocht

De loop zal sowieso blijven herhalen totdat alle karakters behandeld zijn, ook al hebben we allang gevonden wat we zochten.

In de bespreking van de oplossing werd ook al verwezen naar het vroegtijdig willen stoppen van de loop.

### 1.1. Stoppen zonder break
Loops die alle elementen overlopen in een string of array, schrijven we doorgaans met een *for loop* of een *foreach loop*.

Als we echter de loop vroegtijdig willen afbreken is dit niet zo simpel bij deze twee soorten loops.

Bij een *for loop* zou je

-   aan de teller kunnen prutsen in de *loop body*, zodat die meteen de grenswaarde bereikt en de loop zal stoppen

-   een ingewikkeldere loop voorwaarde gebruiken, bv. `teller<grens && !gevonden`

    -   zodra we vinden wat we zoeken zetten we `gevonden` op `true`

    -   dit maakt de loop voorwaarde `false` zodat de loop kan eindigen

Geen van beide is echter wenselijk!

> **Opmerking**
>
> Denk eraan dat als het om ***for loops*** gaat, we enkel *for loops* willen met een simpele hoofding en een simpele teller.
>
> Als je wel moet prutsen aan een teller, of een ingewikkelde loop voorwaarde hebt, gebruik dan een ***while loop***.

Bij een *foreach loop* is er zelfs geen mogelijkheid om eerder te stoppen.

Je zou natuurlijk een *while loop* of *do while loop* kunnen gebruiken, bv. met een voorwaarde die `&& !gevonden` bevat. De *loop body* kan dan `gevonden` op `true` zetten om vroegtijdig te stoppen.

Voorbeeld

We kunnen eens [oplossing D07bevatleesteken](../deel-07-oplossingen/deel-07-oplossingen.html#_oplossing_d07bevatleesteken) herschrijven met een while loop en de loop vroegtijdig beëindigen.

We krijgen dan :

```csharp
Console.Write("Geef een tekst : ");
string input = Console.ReadLine();

bool leestekenGevonden = false;
int index = 0;                                       // (1)
while (index < input.Length && !leestekenGevonden) { // (2)
    char c = input[index];
    if (Char.IsPunctuation(c)) {
        leestekenGevonden = true;
    } else {
        index++;                                     // (1)
    }
}

if (leestekenGevonden) {
    Console.WriteLine("De tekst bevat minstens 1 leesteken.");
} else {
    Console.WriteLine("De tekst bevat geen enkel leesteken.");
}
```

1.  loop administratie

2.  een redelijk ingewikkelde loop voorwaarde

Strikt genomen zouden we de `leestekenGevonden` variabele kunnen vermijden door het volgende te doen :

```csharp
Console.Write("Geef een tekst : ");
string input = Console.ReadLine();

int index = 0;
while (index < input.Length) {     // (1)
    char c = input[index];
    if (Char.IsPunctuation(c)) {
        index = input.Length + 1;  // (2)
    } else {
        index++;
    }
}

if (index <= input.Length) {       // (3)
    Console.WriteLine("De tekst bevat geen enkel leesteken.");
} else {
    Console.WriteLine("De tekst bevat minstens 1 leesteken.");
}
```

1.  Hier zit er een aanpassing, `index < input.Length && !leestekenGevonden` werd `index < input.Length`.

2.  Ook hier zit er een aanpassing, `leestekenGevonden = true` werd `index = input.Length + 1`.

3.  En hier zit er een aanpassing, `leestekenGevonden` werd `index <= input.Length`.

De code is op die manier echter veel onduidelijker geworden. Weet jij bv. meteen de antwoorden op deze vragen?

-   Waarom wordt index gelijk aan `input.Length + 1` gezet als we een leesteken vinden?

    -   Waarom niet gelijkzetten aan `input.Length`, daarmee wordt de loop toch ook beëindigd?

-   Waarom duidt `index <= input.Length` aan dat er geen leesteken werd gevonden?

    -   Waarom niet `index < input.Length`?

Vandaar het goeie advies :

> **Belangrijk**
>
> **Variabelen kosten niks, gebruik er liever eentje teveel dan eentje te weinig!**

Voor dit soort zoekwerk in een string of array, is een *for loop* of *foreach loop* echt wel de betere keuze. Een *while* of *do while* loop maakt de code complexer en minder leesbaar.

Gelukkig bestaat er in veel programmeertalen een opdracht die soelaas biedt : `break`.

### 1.2. Stoppen mét break
Om eender welk soort loop vroegtijdig te stoppen, kunnen we de `break` opdracht gebruiken.

**De `break` opdracht beëindigt onmiddellijk de loop** en de uitvoering gaat verder *na* de loop.

Dus als de `break` halverwege de *loop body* uitgevoerd wordt, zal de tweede helft van deze *loop body* overgeslaan worden en eindigt de loop!

Simpel voorbeeld

In het programma hieronder staat een loop die vijf keer herhaald zou moeten worden. Bij de derde iteratie (teller `i==2`) wordt echter een `break` uitgevoerd…​

```csharp
Console.WriteLine("opdracht voor de loop");
for (int i = 0; i < 5; i++) {                  // (1)
    Console.WriteLine($"i={i}, eerste helft");
    if (i == 2) {
        Console.WriteLine($"i={i}, break!");
        break;                                 // (2)
    }
    Console.WriteLine($"i={i}, tweede helft");
}
Console.WriteLine("opdracht na de loop");
```

1.  de hoofding van de for loop voorziet 5 iteraties

2.  bij de derde iteratie (als `i==2`) wordt deze `break` uitgevoerd

De output van dit programma is :

```csharp
opdracht voor de loop
i=0, eerste helft
i=0, tweede helft
i=1, eerste helft
i=1, tweede helft
i=2, eerste helft
i=2, break!          // (1)
opdracht na de loop
```

1.  in de derde iteratie (als `i==2`) wordt de `break` uitgevoerd

De `break` opdracht in de derde iteratie heeft ervoor gezorgd dat :

-   de tweede helft van die derde iteratie (met `i==2`) werd overgeslaan

-   de loop vroegtijdig eindigde, er kwamen geen iteraties meer met `i==3` en `i==4`

Dit simpele voorbeeld was enigszins geforceerd, tijd voor een realistischere toepassing van `break`!

Voorbeeld

We zullen eens [oplossing D07bevatleesteken](../deel-07-oplossingen/deel-07-oplossingen.html#_oplossing_d07bevatleesteken) herschrijven en de *foreach loop* vroegtijdig stoppen met `break`.

We krijgen dan :

```csharp
Console.Write("Geef een tekst : ");
string input = Console.ReadLine();

bool leestekenGevonden = false;
foreach (char c in input) {
    if (Char.IsPunctuation(c)) {
        leestekenGevonden = true;
        break;  // (1)
    }
}

if (leestekenGevonden) {
    Console.WriteLine("De tekst bevat minstens 1 leesteken.");
} else {
    Console.WriteLine("De tekst bevat geen enkel leesteken.");
}
```

1.  de loop wordt hier meteen gestopt door de `break` opdracht.

Merk op dat we nog steeds de boolean variable `leestekenGevonden` nodig hebben, anders zouden we na de loop niet weten hoe we daar geraakt zijn :

-   zijn we op het einde van de string gekomen?

-   zijn we vroegtijdig gestopt omdat we een leesteken vonden?

Let op :

> **Belangrijk**
>
> **Een `break` opdracht stopt enkel de direct omsluitende loop**.

Indien de `break` ergens middenin een aantal geneste loops staat, zullen dus niet alle loops gestopt worden!

Voorbeeld

```csharp
string[] woorden = { "Negentiende-eeuws", "eau-de-cologneflesje", "te", "koop" };

bool leestekenGevonden = false;
foreach (string woord in woorden) {
    Console.WriteLine($"{woord}");
    foreach (char c in woord) {
        if (Char.IsPunctuation(c)) {
            Console.Write("^");
            leestekenGevonden = true;
            break; // (1)
        } else {
            Console.Write(" ");
        }
    }
    Console.WriteLine();
}

if (leestekenGevonden) {
    Console.WriteLine("Minstens 1 woord bevat een leesteken.");
} else {
    Console.WriteLine("Geen enkel woord bevat een leesteken");
}
```

1.  de `break` opdracht stopt enkel de binnenste loop (i.e. de loop die de karakters van een woord overloopt).

De uitvoering van dit programma ziet er zo uit :

```csharp
Negentiende-eeuws
           ^
eau-de-cologneflesje (1)
   ^
te   (2)

koop (2)

Minstens 1 woord bevat een leesteken.
```

1.  In het woord `eau-de-cologneflesje` werd stopt met zoeken na het eerste koppelteken. Verwijder de `break` en je zult zien dat het tweede koppelteken ook gevonden wordt.

2.  De buitenste loop (i.e. de loop die de woorden uit het array overloopt) blijft echter gewoon doorlopen

> **Belangrijk**
>
> Dit is een ideale oefening om nog eens met de debugger door de code te stappen.
>
> Als je een breakpoint plaatst op de hoofding van de binnenste *foreach loop* en dan op ![continue button](images/continue-button.png) klikt om verder te gaan, kun je makkelijk de woorden overslaan waarvoor je de binnenste loop niet karakter-per-karakter wil debuggen.
>
> De ![continue button](images/continue-button.png) knop vind je in de toolbar en dient om de uitvoering verder te zetten totdat er weer een breakpoint bereikt wordt. Dat is makkelijker dan 107 keer op een van de 'Step' knoppen te klikken.

## 2. Arrays
### 2.1. Parallelle arrays
Vaak is het nodig over een groep van elementen (bv. studenten) meerdere soorten informatie bij te houden (bv. naam en leeftijd).

Later zullen we zien dat dit heel elegant met objecten kan, maar voorlopig zullen we het op een primitievere manier moeten doen.

De truc is :

-   stop de twee soorten informatie (namen en leeftijden) elk in een eigen array

    -   dus een array met namen en een array met leeftijden

-   zorg ervoor dat de naam en de leeftijd van een student op exact dezelfde index staan in de respectievelijke arrays

    -   bv. jouw naam staat op positie 7 in de lijst met namen en je leeftijd staat op positie 7 in de lijst met leeftijden

Men noemt dit **parallelle arrays** : waarden die in parallelle arrays op eenzelfde positie staan, horen bij elkaar.

Een voorbeeld zal dit wellicht duidelijker maken.

Veronderstel we hebben de volgende studenten

-   Jan, 19 jaar

-   Miet, 23 jaar

-   Joris, 21 jaar

-   Cornelia, 18 jaar

We definiëren twee arrays, eentje voor de namen en eentje voor de leeftijden :

```csharp
string[] namen = { "Jan", "Miet", "Joris", "Cornelia" };
int[] leeftijden = {19, 23, 21, 18 };
```

Zetten we die twee arrays eens in een tabel naast elkaar, dan krijgen we :

| Positie | Waarde in `namen` op die positie | Waarde in `leeftijden` op die positie |
|---------|----------------------------------|---------------------------------------|
| `0`     | `Jan`                            | `19`                                  |
| `1`     | `Miet`                           | `23`                                  |
| `2`     | `Joris`                          | `21`                                  |
| `3`     | `Cornelia`                       | `18`                                  |

Je ziet dus dat we op elke positie de informatie vinden van één bepaalde student.

De informatie in de arrays is 'virtueel' gekoppeld op basis van de index.

-   helaas bestaat de koppeling enkel in het hoofd van de programmeur

-   dit maakt het foutgevoelig in grote programma’s, ooit wordt er iets over het hoofd gezien

Voorbeeld

We willen een programma dat de naam van een student vraagt en toont hoe oud hij/zij is.

Het programma is hoofdlettergevoelig (Engels : *case sensitive*).

Als de gebruiker `Miet` ingeeft

-   dan zoeken we `Miet` in het `namen` array en vinden die tekst op positie `1`

-   vervolgens kijken we op diezelfde positie `1` in array `leeftijden`

-   we vinden daar de leeftijd `23`

-   dus `Miet` is `23` jaar

Als de gebruiker `Cornelia` ingeeft

-   dan zoeken we in het `namen` array naar `Cornelia` en vinden die naam op positie `3`

-   vervolgens kijken we op diezelfde positie `3` in array `leeftijden`

-   daar vinden we de leeftijd `18`

-   dus `Cornelia` is `18` jaar

Als de gebruiker `Jules` ingeeft

-   dan zoeken we in het `namen` array naar `Jules` maar vinden die naam niet

    -   dan heeft het ook geen zin om te zoeken naar een leeftijd natuurlijk

-   dus de student met naam `Jules` is niet gekend in ons programma.

Het programma zou er zo kunnen uitzien :

```csharp
string[] namen = { "Jan", "Miet", "Joris", "Cornelia" };
int[] leeftijden = { 19, 23, 21, 18 };

Console.Write("Geef een naam : ");
string naam = Console.ReadLine();

int index = Array.IndexOf(namen, naam); // (1)

if (index != -1) {
    int leeftijd = leeftijden[index];
    Console.WriteLine($"{naam} is {leeftijd} jaar oud");
} else {
    Console.WriteLine("Niet gekend");
}
```

1.  we zoeken hoofdlettergevoelig dus we kunnen `Array.IndexOf()` gebruiken want die zoekt naar identieke waarden.

Als het programma hoofdletter**on**gevoelig (Engels : case **in**sensitive) moet zijn, dan zullen we de zoektocht in het `namen` array met een loop implementeren zodat we `.ToLower()` (of `.ToUpper()`) kunnen gebruiken bij het vergelijken van de namen :

```csharp
string[] namen = { "Jan", "Miet", "Joris", "Cornelia" };
int[] leeftijden = {19, 23, 21, 18 };

Console.Write("Geef een naam : ");
string naam = Console.ReadLine();

int index = -1;
for (int i = 0; i < namen.Length; i++) {
    if (naam.ToLower() == namen[i].ToLower()) {   // (1)
        // We hebben de student gevonden!
        index = i;
        break;
    }
}

if (index != -1) {
    int leeftijd = leeftijden[index];
    Console.WriteLine($"{naam} is {leeftijd} jaar oud");
} else {
    Console.WriteLine("Niet gekend");
}
```

1.  we zoeken hoofdletter**on**gevoelig en vergelijken de "kleine letter" versie van de namen.

Voorbeeld

We willen een programma dat de gebruiker om een leeftijd vraagt, en vervolgens de namen toont van alle studenten die even oud of jonger zijn.

Als de gebruiker `20` ingeeft

-   dan zoeken we in `leeftijden` naar getallen die `<= 20` zijn.

-   telkens we er zo eentje vinden op een bepaalde positie `i`,

    -   dan kijken we in het `namen` array op diezelfde positie `i`

    -   we vinden daar de naam van de student met die leeftijd

In ons voorbeeld vinden we op die manier studenten `Jan` en `Cornelia`.

Een programma dat dit doet zou er zo kunnen uitzien :

```csharp
string[] namen = { "Jan", "Miet", "Joris", "Cornelia" };
int[] leeftijden = { 19, 23, 21, 18 };

Console.Write("Geef een maximum leeftijd : ");
int maxLeeftijd = int.Parse(Console.ReadLine());

Console.WriteLine($"De studenten die {maxLeeftijd} of jonger zijn :");
for (int i = 0; i < leeftijden.Length; i++) {
    if (leeftijden[i] <= maxLeeftijd) {
        // iemand gevonden die maxLeeftijd heeft (of jonger) op positie i
        string naam = namen[i];
        Console.WriteLine(naam);
    }
}
```

> **Let op**
>
> In alle voorgaande voorbeelden gebruikten we **twee parallelle arrays**, maar het kunnen er natuurlijk ook **meer dan twee** zijn!
>
> Het hangt er maar van af hoeveel stukjes gerelateerde informatie je wil bijhouden (bv. naam, leeftijd, adres, telefoonnummer, etc.).
>
> We zullen later zien dat we verschillende stukjes informatie ook kunnen bundelen in **"objecten"**, wat doorgaans tot eenvoudigere code leidt dan parallelle arrays.

### 2.2. De doorsnede van twee arrays berekenen
Veronderstel de volgende arrays :

```csharp
string[] gentenaars = { "jan", "piet", "miet", "camille" };
string[] voetballers = { "miet", "jan", "bart" };
```

Hoe zou je de vraag beantwoorden "wie woont in Gent en voetbalt?", m.a.w. wat is de doorsnede van die twee verzamelingen?

Wat je kan doen is voor elke waarde in `gentenaars`, de `voetballers` overlopen en de namen vergelijken :

```csharp
1 : foreach(string gentenaar in gentenaars) {       // (1)
2 :     foreach(string voetballer in voetballers) { // (2)
3 :         if (gentenaar == voetballer) {          // (3)
4 :             Console.WriteLine($"Ok, Gentenaar {gentenaar} is een voetballer!");
5 :         } else {
6 :             Console.WriteLine($"    Gentenaar {gentenaar} is niet voetballer {voetballer}.");
7 :         }
8 :     }
9 : }
```

1.  overloop de `gentenaars` lijst en loop 1x voor elk waarde

2.  voor elke `gentenaar`, overloop de `voetballers` lijst en loop 1x voor elke waarde

3.  vergelijk de waarden van `gentenaar` en `voetballer`

Hoe vaak zou de vergelijking op regel `3` uitgevoerd worden?

-   er zijn 4 Gentenaars

-   elke Gentenaar wordt met 3 voetballers vergeleken

dus in totaal gebeuren er `4 x 3 = 12` vergelijkingen.

De uitvoering van dit programma ziet er zo uit :

```csharp
    Gentenaar jan is niet voetballer miet.
Ok, Gentenaar jan is een voetballer!          (1)
    Gentenaar jan is niet voetballer bart.    (2)
    Gentenaar piet is niet voetballer miet.
    Gentenaar piet is niet voetballer jan.
    Gentenaar piet is niet voetballer bart.
Ok, Gentenaar miet is een voetballer!         (1)
    Gentenaar miet is niet voetballer jan.    (3)
    Gentenaar miet is niet voetballer bart.   (3)
    Gentenaar camille is niet voetballer miet.
    Gentenaar camille is niet voetballer jan.
    Gentenaar camille is niet voetballer bart.
```

1.  De Gentenaars die ook voetballen.

2.  Overbodige vergelijking, we weten immers al dat `jan` een voetballer is

3.  Overbodige vergelijkingen, we weten immers al dat `miet` een voetballer is

Voor `jan` en `miet` blijven we ze vergelijken met de overige voetballers, alhoewel we al vastgesteld hebben dat ze voetballers zijn! Dat is dus een verspilling van *CPU cycles* en kan vermeden worden door de binnenste loop vroegtijdig te stoppen :

```csharp
foreach(string gentenaar in gentenaars) {
    foreach(string voetballer in voetballers) {
        if (gentenaar == voetballer) {
            Console.WriteLine($"Ok, Gentenaar {gentenaar} is een voetballer!");
            break; // (1)
        } else {
            Console.WriteLine($"    Gentenaar {gentenaar} is niet voetballer {voetballer}.");
        }
    }
}
```

1.  We weten nu dat `gentenaar` een voetballer is, dus we hoeven die niet meer te vergelijken met andere voetballers. De binnenste loop mag dus stoppen.

De uitvoering van dit programma ziet er dan zo uit :

```csharp
    Gentenaar jan is niet voetballer miet.
Ok, Gentenaar jan is een voetballer!          (1)
    Gentenaar piet is niet voetballer miet.
    Gentenaar piet is niet voetballer jan.
    Gentenaar piet is niet voetballer bart.
Ok, Gentenaar miet is een voetballer!         (1)
    Gentenaar camille is niet voetballer miet.
    Gentenaar camille is niet voetballer jan.
    Gentenaar camille is niet voetballer bart.
```

1.  De Gentenaars die ook voetballen.

Merk op dat er nu minder vergelijkingen gebeurd zijn (`9` i.p.v. `12`). Eenmaal we vaststelden dat Gentenaars `jan` en `miet` voetballers zijn, werden ze immers niet meer met andere voetballers vergeleken!

## 3. Strings
### 3.1. Een string is geen char array
Arrays en strings hebben beiden een `.Length` mogelijkheid. Ze geven beiden toegang tot hun bestanddelen via `[index]` en men kan hun inhoud overlopen met *foreach loops*.

Je zou je dus kunnen afvragen :

-   Is een `string` dan gewoon hetzelfde als een `char` array?

Het korte antwoord is : "**Neen, een `string` is geen `char` array**". Er is echter een verband en in sommige programmeertalen is de grens nogal vaag.

Een array van `char` waarden definiëren we zo :

```csharp
char[] leestekens = { '.', '!', '?' };
```

maar dit is dus een heel ander soort waarde dan een `string` met diezelfde leestekens in :

```csharp
string leestekens = ".!?";
```

### 3.2. De lege string
Er bestaat een speciale string waarde `""` die een *lege* string voorstelt.

Een raar beestje dus, een tekst zonder tekst? Maar een lege string is wel degelijk een geldige `string` waarde. Beschouw het een beetje als een bankrekening zonder geld, of een leeg blad papier.

Je zult deze lege string waarde meestal op 2 plaatsen tegenkomen :

-   bij het inlezen met Console.ReadLine()

    -   als de gebruiker niks intypt maar gewoon op ENTER drukt, resulteert dit in een lege string.

-   als je gaandeweg een string opbouwt, dan begin je vaak met een lege string

    -   en plakt er dan steeds nieuwe stukken achter met `+=`

Voorbeeld

Lege gebruikersinput detecteren door te vergelijken met `""` :

```csharp
string naamAlsTekst;
do {
    Console.Write("Geef een naam : ");
    naamAlsTekst = Console.ReadLine();
} while(naamAlsTekst.Trim() == "");    // (1)
```

1.  Na de `.Trim()` vergelijken we de overgebleven input met een lege string

Dit fragment zal de "Geef een naam : " vraag blijven herhalen totdat de gebruiker daadwerkelijk iets zinvols intypt.

Voorbeeld

Een tekst beginnen met `""` om er steeds iets bij te plakken :

```csharp
string reeks = "";       // (1)
for (int i = 0; i < 10; i++) {
    reeks += "(" + i + ")";  // (2)
}
Console.WriteLine(reeks);
```

1.  beginnen met een lege string

2.  op het einde steeds iets nieuws erbij plakken

De output van dit fragment is `(0)(1)(2)(3)(4)(5)(6)(7)(8)(9)`.

Er bestaat trouwens ook een expressie `string.Empty` die je kan gebruik i.p.v. `""`, als je dit leesbaarder zou vinden.

De voorwaarde van een if of while loop zoals `(tekst == "")`, kun je dan schrijven als `(tekst == string.Empty)`.

### 3.3. De null waarde voor string
We hebben eerder gezien dat elk datatype een standaardwaarde heeft (Engels : *default value*).

De standaardwaarde voor alle numerieke types is `0` (of `0.0`), voor `bool` is dit `false`.

Voor `string` is de standaardwaarde `null`.

Let op, `null` is niet nul of zero, het betekent eerder iets als 'ledig', 'niets' of 'onbestaand' of 'geen waarde'.

**De waarde `null` is dus een waarde om aan te duiden dat er geen waarde is.**

Op zich lijkt dit wat vreemd. Bedenkt echter dat lang geleden, toen getallen enkel hoeveelheden voorstelden (bv. 6 koeien) men ook niet meteen het nut inzag van het getal `0` ("Hoezo, je hebt 0 koeien? Je hebt geen koeien!). [Interessant boek](https://www.amazon.com/Zero-Biography-Dangerous-Charles-Seife/dp/0140296476/)

Het heeft er natuurlijk ook mee te maken dat er in een geheugenlocatie altijd een waarde moet staan. Op een locatie 'niets' zetten is geen optie, dus hebben we een speciale waarde nodig om te zeggen dat er 'niets' is.

Als we een variabele declareren zonder initiële waarde, krijgt die dus de standaardwaarde :

```csharp
int i;    // krijgt de waarde 0
string s; // krijgt de waarde null
```

Dit zullen we echter niet zo vaak tegenkomen, meestal voorzien we bij de declaratie al meteen een waarde.

Die standaardwaarden duiken vooral op wanneer we een leeg array aanmaken :

```csharp
int[] i = new int[10];       // (1)
string[] s = new string[10]; // (2)
```

1.  elk slot in dit array bevat de `int` waarde `0`

2.  elk slot in dit array bevat de `string` waarde `null`

Helaas krijg je in C# de `null` waarde nooit op de console te zien, er wordt gewoon niks getoond :

```csharp
string s = null;
Console.WriteLine($"abc{s}def"); // (1)
```

1.  output is `abcdef`

In andere programmeertalen verschijnt effectief de tekst `null` op de console. De output wordt dan `abcnulldef`, wat natuurlijk [je zintuigen meteen op scherp zet](https://www.youtube.com/watch?v=MbeXzJDYxS0) (want het duidt doorgaans op een bug).

Er bestaan trouwens nog twee goedbedoelde string mogelijkheden :

| Mogelijkheid                       | Equivalent                              |
|------------------------------------|-----------------------------------------|
| `String.IsNullOrEmpty(tekst)`      | `(tekst == null || tekst == "")`        |
| `String.IsNullOrWhiteSpace(tekst)` | `(tekst == null || tekst.Trim() == "")` |

Maar veel leesbaarder maken ze de code eigenlijk niet.

Tot slot nog een klein voorbeeld, om te zien of je het goed begrijpt :

Voorbeeld

Veronderstel deze declaraties

```csharp
string text1;
string text2 = null;
string text3 = "null";
string text4 = "0";
string text5 = "";
```

Wat denk je dat het resultaat is van deze vergelijkingen (`true` of `false`)?

-   `text1 == text2`

-   `text2 == text3`

-   `text2 == text4`

-   `text2 == text5`

Hieronder kun je je antwoorden controleren :

| Vergelijking     | Resultaat | Opmerking                                                                              |
|------------------|-----------|----------------------------------------------------------------------------------------|
| `text1 == text2` | `true`    | `tekst1` heeft geen initiële waarde en is dus `null` (de standaardwaarde voor strings) |
| `text2 == text3` | `false`   | `"null"` is gewoon een tekst, er had net zo goed `"jantje"` kunnen staan               |
| `text2 == text4` | `false`   | `null` is niet `0` (nul/zero)                                                          |
| `text2 == text5` | `false`   | `null` is niet hetzelfde als een lege string                                           |

### 3.4. Een string opsplitsen in een string array, met .Split()
Stel je krijgt een tekst met woorden die gescheiden zijn door komma’s :

```csharp
string tekst = "appel,peer,tomaat,kiwi,druif";
```

Dit gebeurt vaak wanneer je data krijgt aangeleverd als een tekst file die je programma moet verwerken.

-   bv. een bestand met product informatie waarbij elke regel een product beschrijft

    -   elke regel bevat dan de stukjes informatie voor dat product (prijs, aantal, beschrijving, gewicht, etc.)

    -   op elke regel zijn de stukjes informatie gescheiden door komma’s

Dan kun je met de `.Split()` mogelijkheid, de individuele woorden tussen de komma’s te pakken krijgen.

Het resultaat zal een `string` array zijn dat de tussenliggende woorden bevat.

```csharp
string tekst = "appel,peer,tomaat,kiwi,druif";

string[] woorden = tekst.Split(','); // (1)
```

1.  Splits de string in stukken op basis van de komma’s `,`

Het array `woorden` zal na afloop 5 entries bevatten, namelijk de 5 woorden tussen de komma’s. De komma’s zelf zullen er nooit in voorkomen!

| Positie | Waarde   |
|---------|----------|
| 0       | `appel`  |
| 1       | `peer`   |
| 2       | `tomaat` |
| 3       | `kiwi`   |
| 4       | `druif`  |

Indien je wil splitsen op basis van verschillende scheidingstekens, kun je een `char` array gebruiken :

```csharp
string tekst = "appel:peer,tomaat::kiwi:,druif";

char[] separators = { ',' , ':' };          // (1)

string[] woorden = tekst.Split(separators); // (2)
```

1.  het `char` array met scheidingstekens

2.  `.Split()` gebruiken met een `char` array van scheidingstekens

Het `char` array `separators` bevat meerdere karakters. Telkens `.Split()` zo’n scheidingsteken tegenkomt in de tekst, is een nieuw woord gevonden dat in het `woorden` array terechtkomt.

Let op : als er meerdere scheidingskarakters elkaar opvolgen, komen er lege strings in het array terecht!

Daarom bevat `woorden` hierboven 7 entries : 5 woorden (op posities 0, 1, 2, 4 en 6) en 2 lege strings (op posities 3 en 5).

| Positie | Waarde   | Opmerking                     |
|---------|----------|-------------------------------|
| 0       | `appel`  |                               |
| 1       | `peer`   |                               |
| 2       | `tomaat` |                               |
| 3       |          | lege string tussen `:` en `:` |
| 4       | `kiwi`   |                               |
| 5       |          | lege string tussen `:` en `,` |
| 6       | `druif`  |                               |

Er bestaat een variant van `.Split()` waarmee je eventuele lege strings uit het resultaat kunt weren. Hiervoor moet je de waarde `StringSplitOptions.RemoveEmptyEntries` gebruiken :

```csharp
string tekst = "appel:peer,tomaat::kiwi:,druif";

char[] separators = { ',' , ':' };

string[] woorden = tekst.Split(separators, StringSplitOptions.RemoveEmptyEntries); // (1)
```

1.  let op het gebruik van `StringSplitOptions.RemoveEmptyEntries`

Het array `woorden` zal in dit geval slechts 5 entries hebben, de twee lege strings van hierboven zijn verdwenen :

| Positie | Waarde   |
|---------|----------|
| 0       | `appel`  |
| 1       | `peer`   |
| 2       | `tomaat` |
| 3       | `kiwi`   |
| 4       | `druif`  |

> **Tip**
>
> De `.Split()` mogelijkheid komt o.a. goed van pas als je een ***comma separated values* (CSV)** bestand wil verwerken in je programma. Dit is een simpel formaat om grote hoeveelheden data te exporteren uit een ander programma, bv. Microsoft Excel of een databank.
>
> In het Nederlands [kommagescheiden bestand](https://nl.wikipedia.org/wiki/Kommagescheiden_bestand)

### 3.5. Een string array samenvoegen tot één string, met .Join()
Stel je hebt een array met woorden en je wil deze aaneen plakken tot één enkele string, met een scheidingsteken ertussen. Bijvoorbeeld om de tekst te maken voor zo’n CSV bestand.

In [de oplossing van oefening D08toongetallen](../deel-08-oplossingen/deel-08-oplossingen.html#_oplossing_d08toongetallen) programmeerden we dit nog zelf, maar met `string.Join()` is dit heel wat eenvoudiger.

Je kunt de [online documentatie voor string.Join() hier raadplegen](https://learn.microsoft.com/en-us/dotnet/api/system.string.join).

Deze `string.Join()` opdracht heeft 2 stukjes informatie nodig :

-   een string met het scheidingsteken

-   een `string` array dat de eigenlijke woorden bevat

Bijvoorbeeld :

```csharp
string[] woorden = {"appel", "banaan", "cactus"};
string samen = String.Join("|", woorden); // (1)
```

1.  Plak alle woorden aaneen met telkens een `|` ertussen.

De variabele `samen` bevat dan de waarde `appel|banaan|cactus`.

Het scheidingsteken mag ook meer dan één karakter bevatten :

```csharp
string[] woorden = {"appel", "banaan", "cactus"};
string samen = String.Join(" of ", woorden)
```

De variabele `samen` bevat nu de waarde `appel of banaan of cactus`.

### 3.6. Verder mogelijkheden
Strings bieden ook de mogelijkheid om deelteksten in te lassen, te verwijderen en te vervangen.

In de volgende voorbeelden gebruiken we een variabele `tekst` met deze inhoud :

|          |     |     |       |     |     |     |     |     |       |     |     |     |     |     |     |
|----------|-----|-----|-------|-----|-----|-----|-----|-----|-------|-----|-----|-----|-----|-----|-----|
| karakter | `H` | `e` | `l`   | `p` | ` ` | `d` | `e` | ` ` | `w`   | `e` | `r` | `e` | `l` | `d` | `!` |
| index    | 0   | 1   | **2** | 3   | 4   | 5   | 6   | 7   | **8** | 9   | 10  | 11  | 12  | 13  | 14  |

#### 3.6.1. Inlassen met .Insert()
Inlassen gebeurt met `.Insert()`, de algemene vorm is

```csharp
tekst.Insert( positie, inlasTekst )
```

Je kunt de [online documentatie voor string.Insert() hier raadplegen](https://learn.microsoft.com/en-us/dotnet/api/system.string.insert).

Het resultaat is een *nieuwe* string die gebaseerd is op `tekst` en waarin `inlasTekst` werd tussengevoegd op de gegeven `positie`.

Bijvoorbeeld :

```csharp
string tekst = "Help de wereld!";

string tekstMetInsert = tekst.Insert(8, "gekke "); // (1)

Console.WriteLine(tekstMetInsert);
```

1.  `gekke ` (incl spatie) wordt ingelast op positie `8` van `tekst`, de output is `Help de gekke wereld!`

#### 3.6.2. Verwijderen met .Remove()
Een deeltekst verwijderen kun je doen met `.Remove()`, de algemene vorm is

```csharp
tekst.Remove( beginPositie, lengte );
```

Je kunt de [online documentatie voor string.Remove() hier raadplegen](https://learn.microsoft.com/en-us/dotnet/api/system.string.remove).

Er ontstaat een *nieuwe* string gebaseerd op `tekst` en waarvan er `lengte` aantal karakters ontbreken, beginnend bij positie `beginPositie`.

```csharp
string tekst = "Help de wereld!";

string tekstMetVerwijdering = tekst.Remove(2, 10); // (1)

Console.WriteLine(tekstMetVerwijdering);
```

1.  verwijder `10` karakters uit `tekst` vanaf positie `2`, de output is `Held!`

#### 3.6.3. Vervangen met .Replace()
Met `.Replace()` kunnen we in een tekst, alle voorkomens van een deeltekst **vervangen** door een andere tekst, een soort *search & replace* dus. De algemene vorm is :

```csharp
tekst.Replace( zoekTekst, vervangTekst );
```

Je kunt de [online documentatie voor string.Replace() hier raadplegen](https://learn.microsoft.com/en-us/dotnet/api/system.string.replace).

Er ontstaat een *nieuwe* string gebaseerd op `tekst`, waarin elk voorkomen van `zoekTekst` is vervangen door `vervangtekst`.

```csharp
string tekst = "Help de wereld!";

string tekstMetVerwijdering = tekst.Replace("el", "oooo"); // (1)

Console.WriteLine(tekstMetVerwijdering);
```

1.  vervang in `tekst` elke `el` door `oooo`, de output is `Hoooop de werooood!`
