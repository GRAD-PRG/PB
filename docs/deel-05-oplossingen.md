# Programmeren Basis - Deel 05 - Oplossingen
## Gemengde oefeningen op herhalingen
### Oplossing D05twintigtotenmettien
```csharp
int getal = 20;
do {
    Console.WriteLine(getal);
    getal = getal - 2;
} while (getal >= 10);
```

### Oplossing D05tafelsvan7
```csharp
int basisGetal = 7;
int factor = 1;

do {
    int product = basisGetal * factor;
    Console.WriteLine($"{factor} x {basisGetal} = {product}");
    factor = factor + 1;
} while (factor <= 10);
```

### Oplossing D05hiephieperhiepst
De extra `Nog een keer!` output mag in de laatste herhaling niet meer verschijnen, we zullen deze dus in een *if statement* plaatsen zodat we ze soms kunnen overslaan. De voorwaarde van die *if* moet altijd `true` zijn, maar in de laatste herhaling moet ze `false` zijn. Dit geeft :

```csharp
Console.Write("Hoe oud wordt de jarige : ");
int leeftijd = int.Parse(Console.ReadLine());

int jaar = 1;
while (jaar <= leeftijd) {
    Console.WriteLine("Hiep hiep hiep, hoera!");

    if (jaar!=leeftijd) { // (1)
        Console.WriteLine("  Nog een keer!");
    }

    jaar++;
}
```

1.  Let op de voorwaarde, die enkel in de laatste herhaling `false` zal zijn.

Mocht je de extra output opdracht NA het verhogen van de teller gezet hebben, zal de voorwaarde er iets anders uitzien (de `jaar` teller is dan immers al verhoogd!) :

```csharp
Console.Write("Hoe oud wordt de jarige : ");
int leeftijd = int.Parse(Console.ReadLine());

int jaar = 1;
while (jaar <= leeftijd) {
    Console.WriteLine("Hiep hiep hiep, hoera!");

    jaar++;

    if (jaar != leeftijd+1) { // (1)
        Console.WriteLine("  Nog een keer!");
    }
}
```

1.  de voorwaarde kreeg een `+1` omdat we net ervoor al `jaar` verhoogd hebben

Je ziet dat de `if`-voorwaarde nu moeilijker te begrijpen is (*"Hoezo +1 ?!?"*), zeker als je de `while`-voorwaarde bekijkt.

Daarom valt het aan te raden om de teller pas op het einde van de *loop body* te verhogen, dat is minder verwarrend.

### Oplossing D05tekstherhalen ☒
Voor deze oefening is er geen voorbeeld oplossing beschikbaar.

### Oplossing D05getalradengebruiker
```csharp
Random r = new Random();
int getal = r.Next(1, 101);
int gok = 0;
Console.WriteLine("De computer denkt aan een getal van 1 tem 100, kun je raden welk?");

do
{
    Console.Write("Wat gok je?: ");
    string input = Console.ReadLine();
    gok = int.Parse(input);
    if (gok < getal)
    {
        Console.WriteLine("Hoger!");
    }
    else if (gok > getal)
    {
        Console.WriteLine("Lager!");
    }
    else
    {
        Console.WriteLine("Disco!");
    }
} while (gok != getal);
```

### Oplossing D05som
```csharp
int som = 0;
int getal = 0;
do {
    Console.Write("Geef een getal (-1 om te stoppen) : ");
    string getalAlsTekst = Console.ReadLine();
    getal = int.Parse(getalAlsTekst);
    som += getal;
} while (getal != -1);

// compenseer voor de afsluitende -1 die werd meegeteld
som -= getal;

Console.WriteLine($"De som is {som}");
```

### Oplossing D05gemiddelde
```csharp
int som = 0;
int aantal = 0;
int getal = 0;
do {
    Console.Write("Geef een getal (-1 om te stoppen) : ");
    string getalAlsTekst = Console.ReadLine();
    getal = int.Parse(getalAlsTekst);
    som += getal;
    aantal++;
} while (getal != -1);

// compenseer voor de afsluitende -1 die werd meegeteld
som -= getal;
aantal--;

if (aantal > 0) {
    double gemiddelde = Convert.ToDouble(som) / aantal;
    Console.WriteLine($"Het gemiddelde is {gemiddelde}");
} else {
    Console.WriteLine("Er werden geen bruikbare getallen ingegeven");
}
```

Let erop dat we geen gemiddelde kunnen berekenen voor 0 getallen (deling door 0!)

### Oplossing D05grootstegetal
```csharp
int grootste = 0;
int teller = 0;
int getal = 0;
do {
    Console.Write("Geef een getal (-1 om te stoppen) : ");
    string getalAlsTekst = Console.ReadLine();
    getal = int.Parse(getalAlsTekst);

    if (getal != -1) {
        teller++;
        if (teller == 1 || getal > grootste) {
            // nieuw maximum gevonden
            grootste = getal;
        }
    }
} while (getal != -1);

if (teller != 0) {
    Console.WriteLine($"Het grootste getal was {grootste}");
} else {
    Console.WriteLine("Er werden geen bruikbare getallen ingegeven");
}
```

*-1* zal nooit een geldige `grootste` getal zijn. Als we `grootste` initieel instellen op *-1* kunnen we de eerste iteratie detecteren, en zo kunnen we de variabele `teller` vermijden…​

```csharp
int grootste = -1;
int getal = 0;
do {
    Console.Write("Geef een getal (-1 om te stoppen) : ");
    string getalAlsTekst = Console.ReadLine();
    getal = int.Parse(getalAlsTekst);

    if (getal != -1) {
        // enkel in de eerste herhaling is grootste == -1
        if (grootste == -1 || getal > grootste) {
            // nieuw maximum gevonden
            grootste = getal;
        }
    }
} while (getal != -1);

if (grootste != -1) {
    Console.WriteLine($"Het grootste getal was {grootste}");
} else {
    Console.WriteLine("Er werden geen bruikbare getallen ingegeven");
}
```

> **Waarschuwing**
>
> Deze oplossing is misschien wel TE clever, we besparen een variabele maar maken de oplossing minder duidelijk.
>
> Don’t be too clever ;)

### Oplossing D05grootstegetalenaantal
```csharp
int grootste = 0;
int aantalKeerGrootste = 0;
int teller = 0;
int getal = 0;
do {
    Console.Write("Geef een getal (-1 om te stoppen) : ");
    string getalAlsTekst = Console.ReadLine();
    getal = int.Parse(getalAlsTekst);

    if (getal != -1) {
        teller++;
        if (teller == 1 || getal > grootste) {
            // nieuw maximum gevonden
            grootste = getal;
            aantalKeerGrootste = 1;
        } else if (getal == grootste) {
            aantalKeerGrootste++;
        }
    }
} while (getal != -1);

if (teller != 0) {
    Console.WriteLine($"Het grootste getal was {grootste} en kwam {aantalKeerGrootste} keer voor");
} else {
    Console.WriteLine("Er werden geen bruikbare getallen ingegeven");
}
```

### Oplossing D05aantal
We moeten steeds het ingelezen getal bijhouden UIT DE VORIGE HERHALING, in variabele `vorigGetal`, en vergelijken met het nieuw ingelezen getal.

De variabele `vorigGetal` krijgt een initiele waarde, *0* in deze oplossing.

Indien de gebruiker 0 als eerste getal intypt mogen we niet stoppen! Het is belangrijk dat we altijd minstens 2 getallen vragen.

```csharp
int teller = 0;
int vorigGetal = 0;
bool verderDoen = true;
do {
    Console.Write("Geef een getal : ");
    string getalAlsTekst = Console.ReadLine();
    int getal = int.Parse(getalAlsTekst);
    teller++;
    if (teller >= 2 && getal == vorigGetal) {
        verderDoen = false;
    } else {
        vorigGetal = getal;
    }
} while (verderDoen);

teller -= 2;
Console.WriteLine($"Aantal getallen ingevoerd: {teller}");
```

Of het kan ook zo…​

```csharp
int teller = 0;
int vorigGetal = 0;
int getal = 0;
do {
    vorigGetal = getal; // ESSENTIEEL DAT DIT HIER STAAT
    Console.Write("Geef een getal : ");
    string getalAlsTekst = Console.ReadLine();
    getal = int.Parse(getalAlsTekst);
    teller++;
} while (teller < 2 || getal != vorigGetal);

teller -= 2;
Console.WriteLine($"Aantal getallen ingevoerd: {teller}");
```

Let erop dat `vorigGetal = getal` nu aan het begin van de herhaling staat! Dit is essentieel, anders kunnen we immers geen `getal != vorigGetal` in de herhalingsvoorwaarde vermelden.

### Oplossing D05rechthoek
```csharp
Console.Write("Hoogte?: ");
int hoogte = int.Parse(Console.ReadLine());

Console.Write("Breedte?: ");
int breedte = int.Parse(Console.ReadLine());

int hoogteTeller = 0;
do {
    int breedteTeller = 0;
    do {
        Console.Write("*");
        breedteTeller = breedteTeller + 1;
    } while (breedteTeller < breedte);
    Console.WriteLine();
    hoogteTeller = hoogteTeller + 1;
} while (hoogteTeller < hoogte);
```

### Oplossing D05vierkant
```csharp
Console.Write("Zijde?: ");
int zijde = int.Parse(Console.ReadLine());

int hoogteTeller = 0;
do {
    int breedteTeller = 0;
    do {
        Console.Write("*");
        breedteTeller = breedteTeller + 1;
    } while (breedteTeller < zijde);
    Console.WriteLine();

    hoogteTeller = hoogteTeller + 1;
} while (hoogteTeller < zijde);
```

### Oplossing D05driehoek
```csharp
Console.Write("Rechthoekzijde?: ");
int zijde = int.Parse(Console.ReadLine());
int breedteZijde = zijde;

int hoogteTeller = 0;
do {
    int breedteTeller = 0;
    do {
        Console.Write("*");
        breedteTeller = breedteTeller + 1;
    } while (breedteTeller < breedteZijde);
    Console.WriteLine();

    hoogteTeller = hoogteTeller + 1;
    breedteZijde = breedteZijde - 1;
} while (hoogteTeller < zijde);
```

### Oplossing D05double
```csharp
Console.Write("Voer een (double) getal in?: ");
double getal;

bool getalIngevoerd = double.TryParse(Console.ReadLine(), out getal);
while (getalIngevoerd) {
    Console.WriteLine("Dank je voor het (double) getal.");
    Console.Write("Gelieve nog een (double) getal in te voeren?: ");
    getalIngevoerd = double.TryParse(Console.ReadLine(), out getal);
}

//Zonder bool variabele kan het natuurlijk ook:
//while (double.TryParse(Console.ReadLine(), out getal)) {
//    Console.WriteLine("Dank je voor het (double) getal.");
//    Console.Write("Gelieve nog een (double) getal in te voeren?: ");
//}

Console.WriteLine("Einde (wegens geen double getal).");
```

### Oplossing D05reeks
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

int kleinste;
int grootste;
if (getal1 <= getal2) {
    kleinste = getal1;
    grootste = getal2;
} else {
    kleinste = getal2;
    grootste = getal1;
}

int getalInReeks = kleinste;
while (getalInReeks <= grootste) {
    Console.Write($"{getalInReeks} ");
    getalInReeks = getalInReeks + 1;
}
```

### Oplossing D05playlist
```csharp
Console.Write("Aantal liedjes in de playlist?: ");
string aantalLiedjesAlsTekst = Console.ReadLine();

int aantalLiedjes;
bool invoerOk = int.TryParse(aantalLiedjesAlsTekst, out aantalLiedjes);

if (invoerOk && aantalLiedjes >= 1) {
    int faculteit;

    faculteit = 1;
    int factor = 2;
    while (factor <= aantalLiedjes) {
        faculteit = faculteit * factor;
        factor = factor + 1;
    }

    string meervoud = "";
    if (faculteit > 1) {
        meervoud = "s";
    }
    Console.Write($"{aantalLiedjes} liedje{meervoud} kan je in {faculteit} verschillende volgorde{meervoud} in een playlist plaatsen.");
}
```

### Oplossing D05wildebertram
```csharp
int maanden;
Console.Write("Aantal maanden groei?: ");
bool invoerOk = int.TryParse(Console.ReadLine(), out maanden);

if (invoerOk && maanden >= 1) {
    int fibo1 = 0;
    int fibo2 = 1;
    int fibo3;

    int maandTeller = 1;
    do {
        fibo3 = fibo1 + fibo2;

        fibo1 = fibo2;
        fibo2 = fibo3;

        maandTeller = maandTeller + 1;
    } while (maandTeller < maanden);

    Console.Write($"Aantal knooppunten: {fibo3}");
}
```

### Oplossing D05somvanafstop
```csharp
string getalOfStop;
bool invoerOk;
int som = 0;
do {
    getalOfStop = Console.ReadLine();
    invoerOk = int.TryParse(getalOfStop, out int getal);
    if (invoerOk) {
        som += getal;
        Console.WriteLine("+");
    } else if (getalOfStop.ToUpper().Trim() != "STOP") {
        Console.WriteLine("Gelieve een geheel getal in te voeren (of STOP om te stoppen).");
    }
} while (getalOfStop.ToUpper().Trim() != "STOP");

Console.Write($"=\n{som}");
```

Je ziet dat de declaratie van de variabele `getal` niet echt opvalt en een beetje verstopt zit op het einde van de `TryParse()` opdracht. Dat is het nadeel van de declaratie daar te zetten ipv op een aparte regel.

De oplossing bevat 2x de vergelijking `getalOfStop.ToUpper().Trim() != "STOP"`, dit kan vermeden worden door een bijkomende `bool` variabele zoals in onderstaande alternatieve oplossing :

```csharp
int getal;
int som = 0;
string getalAlsTekst;
bool geenStopIngetypt; // (1)
do
{
    getalAlsTekst = Console.ReadLine();

    geenStopIngetypt = (getalAlsTekst.Trim().ToUpper() != "STOP"); // (1)

    bool invoerOk = int.TryParse(getalAlsTekst, out getal);
    if (invoerOk)
    {
        // gebruiker een getal heeft ingetypt
        som += getal;
        Console.WriteLine("+");
    }
    else if (geenStopIngetypt) // (1)
    {
        // andere tekst ingetypt
        Console.WriteLine("Gelieve een geheel getal in te voeren (of STOP om te stoppen).");

    }
} while (geenStopIngetypt); // (1)

Console.WriteLine("=");
Console.WriteLine(som);
```

1.  De nieuwe variabele `geenStopIngetypt`

### Oplossing D05somofverschil
```csharp
int resultaat = int.Parse(Console.ReadLine());
string symbool = Console.ReadLine();

while (symbool != "=") {
    int getal = int.Parse(Console.ReadLine());

    if (symbool == "+") {
        resultaat = resultaat + getal;
    } else if (symbool == "-") {
        resultaat = resultaat - getal;
    }

    symbool = Console.ReadLine();
}
Console.Write(resultaat);
```

Een (ietwat) andere oplossing :

```csharp
int resultaat = 0;
string symbool = "+"; // (1)

do {
    int getal = int.Parse(Console.ReadLine());

    if (symbool == "+") {
        resultaat += getal;
    } else if (symbool == "-")  {
        resultaat -= getal;
    }

    symbool = Console.ReadLine();
} while (symbool != "=");

Console.WriteLine(resultaat);
```

1.  Deze startwaarde `"+"` moet er staan zodat `resultaat` de waarde krijgt van het eerste ingevoerde getal in de eerste herhaling van de do..while loop.

Het verschil tussen deze twee oplossingen is hoe het eerste getal & symbool worden afgehandeld (**voor** vs. **in** de loop). Er werd ook overgegaan van een while naar een do..while loop.

Een derde mogelijke oplossing :

```csharp
int resultaat = int.Parse(Console.ReadLine());
string symbool;

do  {
    symbool = Console.ReadLine();
    if (symbool != "=") { // (1)
        int getal = int.Parse(Console.ReadLine());
        if (symbool == "+") {
            resultaat += getal;
        } else if (symbool == "-") {
            resultaat -= getal;
        }
    }
} while (symbool != "=");

Console.WriteLine(resultaat);
```

1.  Deze `if` is erbij gekomen om alle bewerkingen over te slaan indien de gebruiker een `=` symbool ingaf.

Het verschil tussen deze derde oplossing en de twee voorgaande, is dat er in de loop eerst het symbool wordt opgevraagd en dan het getal. In de andere twee oplossingen werd eerste het getal gevraagd en dan pas het symbool.

### Oplossing D05getalradencomputer ☒
Voor deze oefening is er geen voorbeeld oplossing beschikbaar.
