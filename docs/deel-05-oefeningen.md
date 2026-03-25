# Programmeren Basis - Deel 05 - Oefeningen
> **Opmerking**
>
> We raden je aan om zoveel mogelijk oefeningen te maken, dat is de beste voorbereiding.
>
> Als je te ver achterop raakt bij het volgen van deze cursus, dan zul je daar wellicht niet de tijd voor vinden want het zijn er behoorlijk veel.

> **Waarschuwing**
>
> Enkele aandachtspunten bij het oplossen van deze oefeningen :

## Gemengde oefeningen op herhalingen
### Oefening D05twintigtotenmettien
Breng aan de hand van een `do while` statement alle even getallen van *20* tot en met *10* op de console.

Met andere woorden…​

```csharp
20
18
16
14
12
10
```

### Oefening D05tafelsvan7
Maak een programma dat het resultaat van de vermenigvuldiging van het getal *7* met de factoren *1* tot en met *10* gaat weergeven.

Doe dit aan de hand van een herhalingsstructuur, werk met een `do while` statement.

Zorg voor volgende uitvoer op de console…​

```csharp
1 x 7 = 7
2 x 7 = 14
3 x 7 = 21
4 x 7 = 28
5 x 7 = 35
6 x 7 = 42
7 x 7 = 49
8 x 7 = 56
9 x 7 = 63
10 x 7 = 70
```

### Oefening D05hiephieperhiepst
Onderstaand programma vraagt de gebruiker hoe oud de jarige wordt, en toont dan het juiste aantal vreugdekreten :

```csharp
Console.Write("Hoe oud wordt de jarige : ");
int leeftijd = int.Parse(Console.ReadLine());

int jaar = 1;
while (jaar <= leeftijd) {
    Console.WriteLine("Hiep hiep hiep, hoera!");
    jaar++;
}
```

Pas dit programma (minimaal) aan zodat de output als volgt wordt :

```csharp
Hoe oud wordt de jarige : 4
Hiep hiep hiep, hoera!
  Nog een keer!
Hiep hiep hiep, hoera!
  Nog een keer!
Hiep hiep hiep, hoera!
  Nog een keer!
Hiep hiep hiep, hoera!
```

### ★ Oefening D05tekstherhalen
Schrijf een programma dat de gebruiker om een tekst vraagt en een aantal, vervolgens daarmee een nieuwe tekst bouwt en deze op de console toont.

Deze nieuwe tekst bestaat uit het gewenste aantal herhalingen van de ingegeven tekst. Bij de laatste herhaling moet de ingegeven tekst in hoofdletters verschijnen (zie voorbeeld hieronder).

Je mag ervan uitgaan dat de gebruiker altijd een aantal ingeeft dat groter is dan nul.

Een voorbeeld uitvoering zou kunnen zijn :

```csharp
Geef een tekst : woef
Geef een aantal : 4
woefwoefwoefWOEF (1)
```

1.  Let erop dat de tekst vier keer herhaald werd en de laatste woef in hoofdletters staat.

### Oefening D05getalradengebruiker
Schrijf een programma waarbij de gebruiker een getal moet raden (van 1 t.e.m. 100) dat de computer willekeurig gekozen heeft.

De gebruiker geeft telkens een getal in en het programma toont of het gezochte getal "Hoger!" dan wel "Lager" is dan de gok. Indien de gebruiker correct gokt, toont het programma "Disco!"

Een voorbeeld uitvoering waarbij het programma willekeurig voor 74 koos en de gebruiker achtereenvolgens 50, 75, 67, 72 en 74 gokt :

```csharp
Wat gok je?:50
Hoger!
Wat gok je?:75
Lager!
Wat gok je?:67
Hoger!
Wat gok je?:72
Hoger!
Wat gok je?:74
Disco!
```

### Oefening D05som
Schrijf een programma dat de gebruiker meermaals om een getal vraagt (totdat de gebruiker een *-1* ingeeft). Voor de eenvoud gaan we er hier van uit dat de gebruiker netjes getallen invoert, je hoeft hier niet op te controleren.

Na afloop toont het programma wat de SOM is van alle getallen (de ingegeven *-1* telt niet mee).

### Oefening D05gemiddelde
Schrijf een programma dat de gebruiker meermaals om een getal vraagt (totdat de gebruiker een *-1* ingeeft). Voor de eenvoud gaan we er hier van uit dat de gebruiker netjes getallen invoert, je hoeft hier niet op te controleren.

Na afloop toont het programma wat het GEMIDDELDE is van alle getallen (de ingegeven *-1* telt niet mee).

### Oefening D05grootstegetal
Schrijf een programma dat de gebruiker meermaals om een getal vraagt (totdat de gebruiker een *-1* ingeeft). Voor de eenvoud gaan we er hier van uit dat de gebruiker netjes getallen invoert, je hoeft hier niet op te controleren.

Na afloop toont het programma wat het GROOTSTE GETAL was van alle getallen (de ingegeven *-1* telt niet mee).

### Oefening D05grootstegetalenaantal
Schrijf een programma dat de gebruiker meermaals om een getal vraagt (totdat de gebruiker een *-1* ingeeft). Voor de eenvoud gaan we er hier van uit dat de gebruiker netjes getallen invoert, je hoeft hier niet op te controleren.

Na afloop toont het programma wat het GROOTSTE GETAL was van alle getallen (de ingegeven *-1* telt niet mee) en HOE VAAK dit grootste getal voorkwam.

### ★ Oefening D05aantal
Schrijf een programma dat de gebruiker meermaals om een getal vraagt, totdat de gebruiker twee keer na elkaar hetzelfde getal ingeeft. Voor de eenvoud gaan we er hier van uit dat de gebruiker netjes getallen invoert, je hoeft hier niet op te controleren.

Na afloop toont het programma het aantal ingegeven getallen (de laatste twee getallen tellen niet mee).

### Oefening D05rechthoek
Soms worden in herhalingen tellers bijgehouden om te registreren hoe vaak de herhaling werd uitgevoerd.

Stel dat we een programma wensen die op basis van een bepaalde ingevoerde breedte, een lijn van die breedte bestaande uit sterren gaat afprinten…​

```csharp
Console.Write("Breedte?: ");
int breedte = int.Parse(Console.ReadLine());

int breedteTeller = 0;
do {
    Console.Write("*");
    breedteTeller = breedteTeller + 1;
} while (breedteTeller < breedte);
Console.WriteLine(); //beëindig de lijn met een newline-karakter
```

Voorbeeld programmaverloop bij invoer van *17*…​

```csharp
Breedte?: 17
*****************
```

Dan komt een *"teller"* als `breedteTeller` goed van pas. Telkens na het drukken van een ster, wordt de teller verhoogd. Zolang de teller kleiner blijft dan de gewenste `breedte`, blijven we een ster afdrukken, en de teller verhogen.

Maak een programma dat op basis van een bepaalde ingevoerde hoogte en breedte, een corresponderende rechthoek, bestaande uit sterren gaat afdrukken.

Voorbeeld programmaverloop bij invoer van *3* en *12*…​

```csharp
Hoogte?: 3
Breedte?: 12
************
************
************
```

```csharp
Console.Write("Hoogte?: ");
int hoogte = int.Parse(Console.ReadLine());

Console.Write("Breedte?: ");
int breedte = int.Parse(Console.ReadLine());

// (1)
```

1.  Hier aanvullen…​

### Oefening D05vierkant
Maak een programma dat op basis van een bepaalde ingevoerde zijde een corresponderende vierkant, bestaande uit sterren gaat afdrukken.

Voorbeeld programmaverloop bij invoer van *4*…​

```csharp
Lengte zijde?: 4
****
****
****
****
```

```csharp
Console.Write("Zijde?: ");
int zijde = int.Parse(Console.ReadLine());

// (1)
```

1.  Hier aanvullen…​

### ★ Oefening D05driehoek
Maak een programma dat op basis van een bepaalde ingevoerde lengte van een rechthoekszijde een corresponderende gelijkbenige driehoek, bestaande uit sterren gaat afdrukken.

Voorbeeld programmaverloop bij invoer van *5*…​

```csharp
Rechthoekzijde?: 5
*****
****
***
**
*
```

Vul hiervoor volgende code aan…​

```csharp
Console.Write("Rechthoekzijde?: ");
int zijde = int.Parse(Console.ReadLine());

// (1)
```

1.  Hier aanvullen…​

### Oefening D05double
Maak een programma dat de gebruiker vraagt naar een `double` waarde.

Voorbeeld programmaverloop bij invoer van *hallo*…​

```csharp
Voer een (double) getal in?: hallo
Einde (wegens geen double getal).
```

Indien geen naar `double` omzetbare waarde wordt ingevoerd, eindigt het programma met een gepaste melding (*"Einde (wegens geen double getal)."*).

Zolang de gebruiker echter correcte (naar) `double` (om te zetten) waardes invoert, wordt vriendelijk gevraagd opnieuw een getal in te voeren.

Voorbeeld programmaverloop bij invoer van *4*; *6,2*; *8*; *31,5* en *wereld*…​

```csharp
Voer een (double) getal in?: 4
Dank je voor het (double) getal.
Gelieve nog een (double) getal in te voeren?: 6,2
Dank je voor het (double) getal.
Gelieve nog een (double) getal in te voeren?: 8
Dank je voor het (double) getal.
Gelieve nog een (double) getal in te voeren?: 31,5
Dank je voor het (double) getal.
Gelieve nog een (double) getal in te voeren?: wereld
Einde (wegens geen double getal).
```

### Oefening D05reeks
Maak een programma dat de gebruiker vraagt naar twee getallen.

Druk in het programma vervolgens de reeks begrensd door deze twee getallen af. De reeks bestaat uit oplopende (van klein naar groot) opeenvolgende gehele getallen.

Voorbeeld programmaverloop bij invoer van *10* en *20*…​

```csharp
Getal 1?: 10
Getal 2?: 20
Reeks van klein naar groot: 10 11 12 13 14 15 16 17 18 19 20
```

We weten niet welke van de twee ingevoerde getallen de kleinste of grootste zal zijn.

Voorbeeld programmaverloop bij invoer van *13* en *8*…​

```csharp
Getal 1?: 13
Getal 2?: 8
Reeks van klein naar groot: 8 9 10 11 12 13
```

```csharp
Console.Write("Getal 1?: ");
int getal1;
bool invoerOk;
do {
    string getalAlsTekst = Console.ReadLine();
    invoerOk = int.TryParse(getalAlsTekst, out getal1);
    if (!invoerOk) {
        Console.Write("Gelieve een geheel getal in te voeren, getal 1?: ");
    }
} while (!invoerOk);

Console.Write("Getal 2?: ");
int getal2;
do {
    string getalAlsTekst = Console.ReadLine();
    invoerOk = int.TryParse(getalAlsTekst, out getal2);
    if (!invoerOk) {
        Console.Write("Gelieve een geheel getal in te voeren, getal 2?: ");
    }
} while (!invoerOk);

Console.Write("Reeks van klein naar groot: ");

// (1)
```

1.  Hier aanvullen…​

### ★ Oefening D05playlist
Maak een programma dat berekent in hoeveel verschillende volgordes je een bepaald aantal (verschillende) liedjes in een playlist kan plaatsen.

Elke volgorde noemt men ook wel de "permutatie".

[WIKIPEDIA: Permutaties](https://nl.wikipedia.org/wiki/Permutatie)

Het aantal permutaties kan je berekenen aan de hand van een "faculteit".

Bij een faculteitsberekening wordt elk geheel getal, startende bij 1, vermenigvuldigt met het volgend geheel getal, en dat tot aan het getal waarvan de faculteit wordt bepaald.

Zo is de faculteit van *5* gelijk aan *1 x 2 x 3 x 4 x 5* of dus *120*.

Voorbeeld programmaverloop bij invoer van *5*…​

```csharp
Aantal liedjes in de playlist?: 5
5 liedjes kan je in 120 verschillende volgordes in een playlist plaatsen.
```

Voorbeeld programmaverloop bij invoer van *1*…​

```csharp
Aantal liedjes in de playlist?: 1
1 liedje kan je in 1 verschillende volgorde in een playlist plaatsen.
```

Vul hiervoor volgende code aan…​

```csharp
Console.Write("Aantal liedjes in de playlist?: ");
string aantalLiedjesAlsTekst = Console.ReadLine();

int aantalLiedjes;
bool invoerOk = int.TryParse(aantalLiedjesAlsTekst, out aantalLiedjes);

if (invoerOk && aantalLiedjes >= 1) {
    int faculteit;

    // (1)

    string meervoud = "";
    if (faculteit > 1) {
        meervoud = "s";
    }
    Console.Write($"{aantalLiedjes} liedje{meervoud} kan je in {faculteit} verschillende volgorde{meervoud} in een playlist plaatsen.");
}
```

1.  Hier aanvullen…​

### ★ Oefening D05wildebertram
Op onze boerderij kweken we *wilde bertram* (*achillea ptarmica*) om niespoeder uit te produceren.

[WIKIPEDIA: Wilde Bertram](https://nl.wikipedia.org/wiki/Wilde_bertram)

Bij deze bloem zal een nieuwe (bron)aftakking twee maanden moeten groeien voor het sterk genoeg is zelf aftakkingen te creëren. Daarna zal deze (bron)aftakking elke maand verder aftakken.

[Vertakkingen aan de hand van de Fibonacci Reeks](https://www.geestkunde.net/images/scientias2.jpg)

Het aantal knooppunten kan je bijgevolg op een wiskundige rij plaatsen als:

``` nowrap
0  1  1  2  3  5  8  13  21  34  55  89  ...
```

Deze reeks getallen wordt ook wel de *"fibonacci rij"* genoemd, vernoemd naar de bijnaam van de wiskundige die de reeks beschreef. In de fibonacci reeks is het eerste getal *0*, het tweede getal *1* en elke volgend getal de *som* van de voorgaande twee.

[WIKIPEDIA: Rij van Fibonacci](https://nl.wikipedia.org/wiki/Rij_van_Fibonacci)

Maak een programma dat vraagt naar een aantal maanden.

Bereken in het programma hoeveel knooppunten onze bertram plantjes zullen vertonen na dat aantal maanden groei.

-   Na 1 maand wensen we als output 1.

-   Na 2 maand wensen we als output 1.

-   Na 3 maand wensen we als output 2.

-   Na 4 maand wensen we als output 3.

-   Na 5 maand wensen we als output 5.

-   Na 6 maand wensen we als output 8.

-   Na 7 maand wensen we als output 13.

-   Na 8 maand wensen we als output 21.

-   Na 9 maand wensen we als output 34.

-   …​

Voorbeeld programmaverloop bij invoer van *8*…​

```csharp
Aantal maanden groei?: 8
Aantal knooppunten: 21
```

Vul hiervoor volgende code aan…​

```csharp
int maanden;
Console.Write("Aantal maanden groei?: ");
bool invoerOk = int.TryParse(Console.ReadLine(), out maanden);

if (invoerOk && maanden >= 1) {
    int fibo1 = 0;
    int fibo2 = 1;
    int fibo3 = fibo1 + fibo2;

    // (1)

    Console.Write($"Aantal knooppunten: {fibo3}");
}
```

1.  Hier aanvullen

### ★ Oefening D05somvanafstop
Maak een programma dat de gebruiker toelaat meerdere gehele getallen in te voeren, en dit tot de gebruiker *STOP* invoert. Je weet met andere woorden niet hoeveel getallen de gebruiker zal invoeren.

Na de invoer van "STOP" zal het programma de som van de ingevoerde gehele getallen afdrukken.

Voorbeeld programmaverloop bij invoer van *9*, *8*, *7* en *STOP*…​

```csharp
9
+
8
+
7
+
STOP
=
24
```

Na elke invoer drukt de gebruiker op de Enter toets. Bij een correcte invoer (van een getal) drukt het programma hierna op de volgende regel een *"+"* teken af. Bij de invoer van *STOP* volgt er een *"="* symbool en de som.

Bij een niet naar `int` om te zetten ingevoerde tekst zal het programma een foutmelding opleveren. Waarna de gebruiker opnieuw de mogelijkheid krijgt een waarde in te voeren.

Voorbeeld programmaverloop bij invoer van *9*, *hallo*, *wereld*, *8* en *stoP*…​

```csharp
9
+
hallo
Gelieve een geheel getal in te voeren (of STOP om te stoppen).
wereld
Gelieve een geheel getal in te voeren (of STOP om te stoppen).
8
+
  stoP
=
17
```

Merk op dat ook bij invoer van een tekst als *" stoP"* het programma kan beëindigd worden. Negereer met andere woorden hoofdlettergebruik, of eventuele spaties voorafgaande aan of volgende op dit "stop" woord.

Ook indien meteen "STOP" wordt ingevoerd moet het programma een acceptabel resultaat opleveren.

Voorbeeld programmaverloop bij invoer van *STOP*…​

```csharp
STOP
=
0
```

### Oefening D05somofverschil
Maak een programma om een reeks van gehele getallen op te tellen of af te trekken. Het aantal getallen in de berekening is niet vastgelegd.

We gaan er voor de eenvoud vanuit dat steeds netjes getallen en correct operatoren (*"+"*, *"-"* of *"="*) worden ingevoerd. Je hoeft hierop dus geen controle toe te passen.

Zorg ervoor dat je oplossing exact verloopt zoals in volgende programmaverlopen wordt gedemonstreerd. Na elk getal en elke symbool die door de gebruiker wordt ingevoerd, zal de gebruiker ook op de Enter toets drukken.

Voorbeeld programmaverloop bij invoer van *1* en *=*…​

```csharp
1
=
1
```

Voorbeeld programmaverloop bij invoer van *1*, *+*, *2* en *=*…​

```csharp
1
+
2
=
3
```

Voorbeeld programmaverloop bij invoer van *1*, *-*, *-5* en *=*…​

```csharp
1
-
-5
=
6
```

Voorbeeld programmaverloop bij invoer van *1*, *+*, *2*, *-*, *3*, *-*, *4*, *+*, *5* en *=*…​

```csharp
1
+
2
-
3
-
4
+
5
=
1
```

### ★ Oefening D05getalradencomputer
Schrijf een programma dat het getal (van 1 t.e.m. 100) probeert te raden waar de gebruiker aan denkt.

Telkens het programma een gok doet, moet de gebruiker aangeven of het gezochte getal HOGER dan wel LAGER is dan de gok.

De gebruiker kan "hoger", "lager" of "juist" invoeren (hoofdletterongevoelig). Indien de gebruiker iets anders ingeeft wordt de vraag herhaald. Na afloop toont het programma hoeveel gokken er nodig waren om het getal te raden.

Een voorbeeld uitvoering waarbij de gebruiker aan het getal 74 denkt :

```csharp
De computer gokt 50, is jouw getal hoger/lager of was het juist?
Hoger
De computer gokt 75, is jouw getal hoger/lager of was het juist?
LAGer
De computer gokt 67, is jouw getal hoger/lager of was het juist?
hoooooooooger
De computer gokt 67, is jouw getal hoger/lager of was het juist? (1)
hogER
De computer gokt 72, is jouw getal hoger/lager of was het juist?
hoger
De computer gokt 74, is jouw getal hoger/lager of was het juist?
juist
Het programma raadde je getal in 5 gokken
```

1.  de vraag werd herhaald omdat de gebruiker geen geschikt antwoord ingaf.
