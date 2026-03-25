# Programmeren Basis - Deel 10 - Verdieping
## 1. Methods
### 1.1. Method overloading
Technisch gezien is het mogelijk in dezelfde klasse (bijvoorbeeld de `class Program`) meerdere methods te definiëren met dezelfde naam. Men noemt dit ook wel *method overloading*.

Zolang er een verschil is in parameters (type, volgorde of aantal) blijft voor de compiler duidelijk welke method wordt aangeroepen.

Voorbeeld van method overloading

``` nowrap
todo iets aanpassen aan start verwoording hier
```

Mogelijks wil je bij het laten *afprinten van de lotto cijfers* geen specifiek `label` voorzien. Volstaat het bijvoorbeeld dat voor de getallen de tekst *"Lotto Cijfers"* wordt weergegeven…​

```csharp
Lotto Cijfers: 32|10|27|21|2|13
Lotto Cijfers: 10|24|34|8|19|25
```

Voor dat doeleinde volstaat een method met slechts één parameter (`int[] lottoCijfers`), die we aanroepen als …​

```csharp
PrintLottoCijfers(trekking1);
PrintLottoCijfers(trekking2);
```

Opnieuw is de naam `PrintLottoCijfers` uitgekozen, de *lotto cijfers* worden ook door deze method immers *afgedrukt*.

Deze method kan eenvoudigweg herbruik maken van onze voorgaande `PrintLottoCijfers`. Opnieuw gaat het ten slotte over het *afdrukken van de lotto cijfers*, deze keer met *"Lotto Cijfers"* als `label`…​

```csharp
static void PrintLottoCijfers(int[] lottoCijfers) {
    PrintLottoCijfers("Lotto Cijfers", lottoCijfers); // (1)
}
```

1.  Let op dit is verwarrend: `PrintLottoCijfers` met één parameter roept `PrintLottoCijfers` met twee parameters aan.

Of alle code samen…​

```csharp
static void Main() {
    int[] trekking3 = { 4, 11, 25, 33, 7, 18 };

    PrintLottoCijfers(trekking3);
    PrintLottoCijfers(trekking3, "Trekking van dit weekend");
}

static void PrintLottoCijfers(int[] lottoCijfers, string label) {
    string cijfersInEénTekst = string.Join("|", lottoCijfers);
    Console.WriteLine($"{label}: {cijfersInEénTekst}");
}

static void PrintLottoCijfers(int[] lottoCijfers) {
    PrintLottoCijfers("Lotto Cijfers", lottoCijfers);
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
Lotto Cijfers: 4|11|25|33|7|18
Trekking van dit weekend: 4|11|25|33|7|18
```

> **Waarschuwing: Vermijd het gebruik van method overloading.**
>
> Je merkt het: *method overloading* kan al snel verwarrend werken.
>
> Het valt dan ook af te raden *method overloading* zomaar in te zetten. Zeker in het geval dat de verschillende methods (met dezelfde naam) beduidend verschillende logica omvatten, is het geen goed idee.
>
> Method overloading ondermijnt de leesbaarheid van de code. De code wordt minder expliciet, verlies met andere woorden aan uitdrukkingkracht.
>
> Bij het nalezen van een method call is de naam alleen onvoldoen om af te leiden welke method nu precies wordt aangeroepen. De lezer van de code moet ook nog eens naar de parameterwaardes gaan kijken. De code is meer *ambigu*, wat nooit een pluspunt kan zijn.

Method overloading wordt in een aantal voorgedefinieerde methods gebruikt. Denk aan de `Console.Write` of `WriteLine` methods die je met parameterwaardes van allerhande datatypes kan aanroepen. Tik je in de code-editor van een geavanceerde ontwikkelomgeving als *Visual Studio* de methodnaam in, dan toont *IntelliSense* ons meteen een lijst van alle mogelijk aan te roepen overladen versies.

![Visual Studio - Method Overloading Tooltip 1](images/Visual%20Studio%20-%20Method%20Overloading%20Tooltip%201.png)

Aan de hand van de pijltjes kan je scrollen tussen de verschillende versies van deze method.

Dat men voor al deze verschillende methods toch steeds kiest voor een naam als `WriteLine` of `Write`, is niet onlogisch. Elk van die overladen methods gaat immers de parameterwaarde in tekstvorm op de console *schrijven*. Het zou niet bepaald praktisch zijn indien men aan elke verschillende versie van de `WriteLine` of `Write` methods een andere naam zou geven. Maar opnieuw, wees er zuinig mee in je eigen code!

Bij het aanroepen van onze `PrintLottoCijfers` kregen we trouwens ook dergelijk tooltip te zien…​

![Visual Studio - Method Overloading Tooltip 2](images/Visual%20Studio%20-%20Method%20Overloading%20Tooltip%202.png)

## 2. Parameters
### 2.1. Optionele parameterwaardes
In sommige gevallen kan je *method overloading*, en dus het uitschrijven van meerdere method definities, vermijden door te werken met *optionele parameters*.

Een optionele parameter krijgt een *default waarde* die wordt ingezet wanneer de aanroepende logica geen ander waarde wenst te voorzien.

De aanroepende code heeft zo meer flexibiliteit, het kan ervoor kiezen al dan niet een waarde voor deze optionele parameter te voorzien

Voorbeeld van optionele parameter

In dit voorbeeld gebruiken we voor de tweede parameter `label` de default waarde *"Lotto Cijfers"*.

Wordt toch een tweede parameterwaarde doorgegeven, dan wordt deze default waarde met de doorgegeven waarde overschreven.

```csharp
static void Main() {
    int[] trekking3 = { 4, 11, 25, 33, 7, 18 };

    PrintLottoCijfers(trekking3);                                // (3)
    PrintLottoCijfers("Trekking van dit weekend", trekking3);    // (4)
}

static void PrintLottoCijfers(int[] lottoCijfers,
                              string label = "Lotto Cijfers") {  // (1)
    string cijfersInEénTekst = string.Join("|", lottoCijfers);
    Console.WriteLine($"{label}: {cijfersInEénTekst}");
}

//static void PrintLottoCijfers(int[] lottoCijfers) {            // (2)
//    PrintLottoCijfers("Lotto Cijfers", lottoCijfers);
//}
```

1.  Merk op dat aan de parameter `label` alvast een *default waarde* wordt toegekend.

2.  Deze versie van `PrintLottoCijfers` is overbodig geworden.

3.  Hier wordt de defaultwaarde gebruikt.

4.  Hier wordt de defaultwaarde overschreven met de doorgegeven waarde.

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
Lotto Cijfers: 4|11|25|33|7|18
Trekking van dit weekend: 4|11|25|33|7|18
```

> **Opmerking: Optionele parameters achteraan de parameterlijst.**
>
> Je mag met meerdere optionele parameters werken, maar deze moeten telkens achteraan de parameterlijst staan.

### 2.2. Parameterwaardes doorgeven met positie of naam
Tot nu toe was het steeds de positie van de parameterwaarde die bepaalde voor welke parameter deze waarde werd gebruikt.

In volgend voorbeeld zal de eerste parameterwaarde `afstandInCm` (voor de komma) toegekend worden aan de parameter `deeltal`. De tweede parameterwaarde *100* (na de komma) wordt gebruikt voor parameter `deler`.

```csharp
static void Main()
{
    double afstandInCm = 183;

    PrintQuotient(afstandInCm, 100);
}

static void PrintQuotient(double deeltal, double deler)
{
    Console.WriteLine(deeltal / deler);
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
1,83
```

Toch kan je naast deze *parameter passing by position* ook werken met ***parameter passing by name***.

Vermeld hiervoor bij het aanroepen van de method tussen haakjes de naam van de parametervariabele. Na de identifier zet je een `:` gevolgd door de parameterwaarde…​

```csharp
static void Main()
{
    double afstandInCm = 183d;

    PrintQuotient(deler: 100, deeltal: afstandInCm);
    PrintQuotient(afstandInCm, deler: 100);
}

static void PrintQuotient(double deeltal, double deler)
{
    Console.WriteLine(deeltal / deler);
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
1,83
1,83
```

De volgorde van de parameterwaardes is niet meer van belang. Deze aanpak kan de leesbaarheid ten goede komen. Je hoeft niet meer te kijken naar de method definitie om een beeld te hebben van wat de rol is van deze parameterwaardes.

Je hoeft niet alle parameterwaardes te benoemen, maar benoemde moeten steeds achteraan de parameterlijst staan.

## 3. Methods genereren in Visual Studio
Bij het opstellen van nieuwe methods kan een geavanceerde ontwikkelomgeving als *Visual Studio* ons ondersteunen. Sterker nog het is in staat, op basis van bestaande code, zelf methods te genereren.

Als *productivity feature* kan dat wel tellen. We kunnen hiermee wel wat tijdswinst boeken.

Voorbeeld van het genereren van methods met Visual Studio

Stel dat je graag een method zou hebben om een bepaald `aantal` elementen van `string` array als `weekdagen` af te printen.

Je neemt misschien alvast een method call als `Print(weekdagen, aantal)` in je code op. Uiteraard krijg je een compilefout omdat deze method nog niet bestaat…​

![Visual Studio - Quick Actions 1](images/Visual%20Studio%20-%20Quick%20Actions%201.png)

Zou je hier het *Quick Actions* lichtbol icoontje aanklikken dan wordt ons voorgesteld deze method automatisch te laten genereren.

Er wordt een voorbeeld getoond van hoe onze method er kan gaan uitzien.

![Visual Studio - Quick Actions 2](images/Visual%20Studio%20-%20Quick%20Actions%202.png)

Kies je hier effectief voor *Generate method …​* dan wordt onze code aangevuld met een definitie van deze method.

```csharp
static void Main()
{
    string[] weekdagen = { "ma", "di", "wo", "do", "vr", "za", "zo" };
    int aantal = 5;

    Print(weekdagen, aantal);
}
private static void Print(string[] weekdagen, int aantal)
{
    throw new NotImplementedException();
}
```

Je hoeft geen tijd meer te verliezen aan het zelf uitschrijven van de method hoofding.

Het `private` keyword in de methodsignatuur was niet noodzakelijk en mag je verwijderen. Al kan het geen kwaad dit sleutelwoord te behouden. Later bespreken we wel eens de betekenis van `private`.

*Visual Studio* kon natuurlijk niet weten wat we graag zouden verwezelijken in de implementatie en gaat eenvoudigweg markeren dat er nog geen implemantie is voorzien (*NotImplemented*).

We kunnen uiteraard zelf een zinvol gedrag voor deze `Print` method uitschrijven…​

```csharp
static void Print(string[] weekdagen, int aantal)
{
    for (int index = 0; index < aantal; index++) {
        Console.Write(weekdagen[index] + " ");
    }
    Console.WriteLine();
}
```

Ook de parameternaam gaan we aanpassen. De method kan eender welke `string[]` (*string array*) afdrukken, dit hoeven niet persé *weekdagen* te zijn. Straks willen we misschien aan deze method *familienamen* of *stadsnamen* doorgeven.

Een vagere benaming als *reeks* lijkt meer aangewezen.

```csharp
static void Print(string[] reeks, int aantal)
{
    for (int index = 0; index < aantal; index++) {
        Console.Write(reeks[index] + " ");
    }
    Console.WriteLine();
}
```

Nog een voorbeeld van het genereren van methods

In onderstaande code merk je misschien op hoe bepaalde logica meermaals is uitgeschreven…​

We gaan twee keer de elementen van een `int[]` (*int array*) benaderen om deze op de console te drukken. Voor de waardes van de array, wordt het *aantal element* afgedrukt.

```csharp
int[] dagen = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
int[] temperaturen = { 10, 21, 13, 17 };

Console.Write($"({dagen.Length} elementen) ");
foreach (int dag in dagen) {
    Console.Write(dag + " ");
}
Console.WriteLine();

Console.Write($"({temperaturen.Length} elementen) ");
foreach (int temperatuur in temperaturen) {
    Console.Write(temperatuur + " ");
}
```

Indien we het voorbeeld uitvoeren dan krijgen we volgende output…​

```csharp
(12 elementen) 31 28 31 30 31 30 31 31 30 31 30 31
(4 elementen) 10 21 13 17
```

Je kan hier beslissen een extra method toe te voegen voor het printen van dergelijke array. Deze method kan je echter ook laten generen door *Visual Studio*.

Selecteer in de code-editor de instructieregels die je in een nieuwe method wil onderbrengen. Rechterklik op deze selectie en kies voor *Quick Actions…​*.

![Visual Studio - Quick Actions 3](images/Visual%20Studio%20-%20Quick%20Actions%203.png)

Er wordt voorgesteld een extra method `NewMethod` toe te voegen.

We zien aan de voorgestelde definitie dat deze method een `int[]` parameterwaarde zou verwachten. De geselecteerde code zou worden vervangen door een call naar deze method.

![Visual Studio - Quick Actions 4](images/Visual%20Studio%20-%20Quick%20Actions%204.png)

Als het voorgestelde je bevalt, kies je voor *Extract Method* onder het lichtbol icoontje.

De broncode in de code-editor wordt aangepast.

*Visual Studio* heeft ook wel door dat `NewMethod` geen goede naam is voor deze extra method. Het vraagt ons meteen (bij wijze van de *Rename* feature) een nieuwe, hopelijk betere, naam in te stellen.

![Visual Studio - Quick Actions 5](images/Visual%20Studio%20-%20Quick%20Actions%205.png)

`Print` lijkt een aangewezen naam.

Ook voor het afprinten van onze `temperaturen` `int[]` kunnen we nu de `Print` method inzetten…​

```csharp
static void Main()
{
    int[] dagen = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
    int[] temperaturen = { 10, 21, 13, 17 };

    Print(dagen);
    Print(temperaturen);
}

private static void Print(int[] dagen)
{
    Console.Write($"({dagen.Length} elementen) ");
    foreach (int dag in dagen) {
        Console.Write(dag + " ");
    }
    Console.WriteLine();
}
```

De naam van de parameter en de naam van de elementvariabele van de `foreach` is eigenlijk niet goed uitgekozen.

We gebruiken deze `Print` method zowel om *dagen* als om *temperaturen* aan door te geven. Over het algemeen kan je stellen dat het een *getallenreeks* is die wordt doorgegeven.

Misschien zijn *getallen* en *getal* beter benamingen…​

```csharp
private static void Print(int[] getallen)
{
    Console.Write($"({getallen.Length} elementen) ");
    foreach (int getal in getallen)
        Console.Write(getal + " ");
    Console.WriteLine();
}
```

De *Quick Actions* in *Visual Studio* bieden ons uitstekende ondersteuning voor het toevoegen van extra methods.

> **Waarschuwing**
>
> Let wel op: bekijk goed of wat gegenereerd wordt ook effectief voldoet aan de verwachtingen. Meestal moet toch manueel één en ander worden aangepast.
>
> Zeker de benamingen van lokale variabelen en parameters moet je vaak nog zelf corrigeren.
