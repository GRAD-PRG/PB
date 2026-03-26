# Collecties in C#

## Waarom collecties?

Je kent ondertussen arrays. Je weet hoe je er waardes in stopt, hoe je er doorheen itereert, hoe je elementen verwijdert door alles op te schuiven, en hoe je een array moet resizen als hij vol zit. Dat werkt, maar het is *veel handwerk*. Elke keer dat je een element wilt toevoegen, moet je zelf checken of er nog plaats is, eventueel een nieuwe grotere array aanmaken, alles kopiëren, en dan pas je element toevoegen.

Collecties lossen dat op. Het zijn kant-en-klare datastructuren die intern het geheugenbeheer voor je regelen. Je hoeft niet meer zelf te schuiven, resizen, of bijhouden hoeveel elementen er effectief in zitten.

C# biedt een hele reeks collectietypes aan in de namespace `System.Collections.Generic`. Elk type heeft zijn eigen sterktes — en het is belangrijk dat je het juiste type kiest voor de juiste situatie.

---

## `List<T>` — de flexibele array

### Wat is het?

Een `List<T>` is in essentie een array die zichzelf beheert. Intern gebruikt een `List<T>` gewoon een array, maar hij regelt het resizen, opschuiven en bijhouden van het aantal elementen automatisch.

### Basisgebruik

```csharp
List<string> namen = new List<string>();

// Toevoegen
namen.Add("Amir");
namen.Add("Fatima");
namen.Add("Jan");

// Aantal opvragen
Console.WriteLine(namen.Count); // 3

// Element opvragen via index (net als bij een array)
Console.WriteLine(namen[0]); // Amir

// Element verwijderen
namen.Remove("Fatima");

// Checken of een element aanwezig is
bool aanwezig = namen.Contains("Jan"); // true
```

### Vergelijking met een array

| Bewerking | Array | `List<T>` |
|---|---|---|
| Element toevoegen | Zelf resizen + kopiëren | `Add()` |
| Element verwijderen | Zelf opschuiven + bijhouden | `Remove()` of `RemoveAt()` |
| Aantal elementen | Zelf bijhouden met een teller | `.Count` |
| Element opvragen via index | `array[i]` | `list[i]` |

### Wanneer gebruik je een `List<T>`?

Gebruik een `List<T>` als je **standaardvervanging voor een array** wanneer:

- Je niet op voorhand weet hoeveel elementen er zullen zijn.
- Je regelmatig elementen wilt toevoegen of verwijderen.
- De volgorde van elementen belangrijk is.
- Je elementen via een index wilt opvragen.

### Nuttige methodes

```csharp
List<int> getallen = new List<int> { 5, 3, 8, 1, 9 };

getallen.Sort();              // Sorteert: 1, 3, 5, 8, 9
getallen.Reverse();           // Keert om: 9, 8, 5, 3, 1
getallen.Insert(2, 42);       // Voegt 42 in op positie 2
getallen.RemoveAt(0);         // Verwijdert het eerste element
int index = getallen.IndexOf(8); // Geeft de index van 8
getallen.Clear();             // Maakt de lijst leeg
```

### Wat gebeurt er intern?

Een `List<T>` begint met een interne array van een bepaalde grootte (standaard capaciteit 0, bij de eerste `Add` springt die naar 4). Zodra de interne array vol zit en je voegt nog een element toe, maakt de lijst intern een nieuwe array aan die **dubbel zo groot** is, kopieert alles, en gooit de oude weg. Je merkt hier zelf niets van, maar het verklaart waarom een `List<T>` een eigenschap `Capacity` heeft (de grootte van de interne array) naast `Count` (het aantal elementen dat er effectief in zit).

```csharp
List<int> getallen = new List<int>();
Console.WriteLine(getallen.Capacity); // 0
getallen.Add(1);
Console.WriteLine(getallen.Capacity); // 4 (eerste Add reserveert meteen ruimte)
getallen.Add(2);
getallen.Add(3);
getallen.Add(4);
getallen.Add(5); // 5e element: capaciteit verdubbelt
Console.WriteLine(getallen.Capacity); // 8
```

---

## `HashSet<T>` — een verzameling zonder duplicaten

### Wat is het?

Een `HashSet<T>` is een collectie die **enkel unieke waarden** bijhoudt. Als je probeert een waarde toe te voegen die er al in zit, gebeurt er simpelweg niets. Een `HashSet<T>` heeft **geen volgorde** en **geen index** — je kunt niet opvragen "geef mij element 3".

### Basisgebruik

```csharp
HashSet<string> kleuren = new HashSet<string>();

kleuren.Add("rood");
kleuren.Add("blauw");
kleuren.Add("rood"); // Wordt genegeerd, "rood" zit er al in

Console.WriteLine(kleuren.Count); // 2

bool zitErin = kleuren.Contains("blauw"); // true

kleuren.Remove("rood");
```

### Wanneer gebruik je een `HashSet<T>`?

- Je wilt **duplicaten vermijden** zonder zelf te moeten checken.
- Je moet vaak controleren of **een element aanwezig is** (dit gaat snel bij een `HashSet<T>`).
- De **volgorde doet er niet toe**.
- Je wilt **verzamelingsbewerkingen** doen (unie, doorsnede, verschil).

### Verzamelingsbewerkingen

Dit is waar een `HashSet<T>` goed in is: bewerkingen die je kent uit de wiskundige verzamelingenleer:

```csharp
string[] groepA = [ "Amir", "Fatima", "Jan" ];
string[] groepB = [ "Fatima", "Sara", "Jan" ];

// Doorsnede: wie zit in BEIDE groepen?
HashSet<string> doorsnede = new HashSet<string>(groepA);
doorsnede.IntersectWith(groepB);
// Resultaat: { "Fatima", "Jan" }
Console.WriteLine(string.Join(", ", doorsnede));

// Unie: wie zit in minstens één groep?
HashSet<string> unie = new HashSet<string>(groepA);
unie.UnionWith(groepB);
// Resultaat: { "Amir", "Fatima", "Jan", "Sara" }
Console.WriteLine(string.Join(", ", unie));

// Verschil: wie zit in groepA maar NIET in groepB?
HashSet<string> verschil = new HashSet<string>(groepA);
verschil.ExceptWith(groepB);
// Resultaat: { "Amir" }
Console.WriteLine(string.Join(", ", verschil));
```

### Snelheid: `Contains()` bij `List<T>` vs `HashSet<T>`

Bij een `List<T>` moet `Contains()` elk element één voor één aflopen tot het een match vindt. Bij 10.000 elementen kan dat dus tot 10.000 vergelijkingen kosten. Dat noemen we **O(n)** — de tijd groeit lineair met het aantal elementen.

Bij een `HashSet<T>` wordt via een wiskundige berekening (een *hashfunctie*) direct berekend *waar* het element zou moeten zitten. Dat kost (gemiddeld) altijd even lang, ongeacht of er 10 of 10 miljoen elementen in zitten. Dat noemen we **O(1)** — constante tijd.

> **Vuistregel:** als je vaak `Contains()` nodig hebt op een grote collectie, kies dan een `HashSet<T>` (of een `Dictionary`).

> **Korte uitweiding: Big-O notatie**
>
> Je ziet in deze cursus soms `O(n)` en `O(1)` staan. Dat is de zogenaamde *Big-O notatie* — een manier om uit te drukken **hoe de snelheid van een bewerking schaalt naarmate de hoeveelheid data groeit**.
>
> Je hoeft er geen wiskundige analyse van te kunnen maken. Het volstaat om deze paar patronen te herkennen:
>
> | Notatie | Betekenis | Voorbeeld |
> |---|---|---|
> | `O(1)` | **Constante tijd** — altijd even snel, ongeacht de grootte. | Opzoeken via hashcode in een `HashSet<T>` |
> | `O(n)` | **Lineaire tijd** — duurt proportioneel langer naarmate er meer elementen (`n`) zijn. | `Contains()` op een `List<T>`: in het slechtste geval moet je alles aflopen. |
> | `O(n²)` | **Kwadratische tijd** — bij verdubbeling van `n` wordt het vier keer trager. | Een geneste loop: voor elk element in lijst A, elk element in lijst B doorlopen. |
> | `O(log n)` | **Logaritmische tijd** — bij verdubbeling van `n` groeit de tijd slechts met één stap. | Binaire zoekopdracht in een gesorteerde lijst. |
>
> De `n` staat voor het aantal elementen in je collectie. De notatie zegt niets over de exacte tijd in milliseconden — het zegt iets over **hoe snel het erger wordt** als je collectie groeit.


## Hoe werkt hashing? — Buckets uitgelegd

Om te begrijpen waarom een `HashSet<T>` en een `Dictionary<TKey, TValue>` zo snel zijn, moet je het concept van **hashing** snappen.

### Het idee

Stel: je hebt een rij van 8 "emmers" (buckets), genummerd 0 tot 7. Wanneer je een element wilt opslaan, bereken je een getal op basis van dat element (de **hashcode**), en je gebruikt dat getal om te bepalen in welke emmer het terechtkomt.

Concreet: `bucketIndex = hashcode % aantalBuckets`

Als de hashcode van `"Amir"` bijvoorbeeld `574829` is, dan wordt de bucket: `574829 % 8 = 5`. Het element komt in bucket 5.

Wil je later opzoeken of `"Amir"` in de collectie zit? Dezelfde berekening: hashcode berekenen, modulo het aantal buckets, en je weet direct in welke bucket je moet kijken. Geen duizenden elementen aflopen.

### Wat als twee elementen in dezelfde bucket terechtkomen?

Dat heet een **collision**. Het gebeurt onvermijdelijk — je hebt immers meer mogelijke elementen dan buckets. In dat geval worden de elementen in diezelfde bucket in een lijstje (of een andere structuur) opgeslagen. Bij het opzoeken ga je naar de juiste bucket en loop je enkel dat korte lijstje af.

Zolang de elementen redelijk gelijk verspreid zijn over de buckets, blijft elk lijstje kort en blijft het opzoeken snel. Als de collectie te vol raakt (te veel collisions), wordt het aantal buckets automatisch verhoogd en worden alle elementen herverdeeld — vergelijkbaar met hoe een `List<T>` zijn interne array verdubbelt.

### Samengevat

| Stap | Wat gebeurt er? |
|---|---|
| Element toevoegen | Hashcode berekenen → bucket bepalen → element in die bucket plaatsen |
| Element opzoeken | Hashcode berekenen → bucket bepalen → enkel die bucket doorzoeken |
| Te veel collisions | Aantal buckets vergroten → alles opnieuw verdelen (rehashing) |

---

## `Dictionary<TKey, TValue>` — opzoeken via een sleutel

### Wat is het?

Een `Dictionary<TKey, TValue>` slaat **sleutel-waarde-paren** op. Elke sleutel is uniek en verwijst naar precies één waarde. Je kunt razendsnel een waarde opvragen als je de sleutel kent — intern werkt een `Dictionary` met dezelfde hashing-techniek als een `HashSet`.

### Basisgebruik

```csharp
Dictionary<string, int> leeftijden = new Dictionary<string, int>();

// Toevoegen
leeftijden.Add("Amir", 22);
leeftijden.Add("Fatima", 19);
leeftijden["Jan"] = 21; // Alternatieve manier om toe te voegen

// Opvragen
Console.WriteLine(leeftijden["Amir"]); // 22

// Waarde wijzigen
leeftijden["Amir"] = 23;

// Checken of een sleutel bestaat
if (leeftijden.ContainsKey("Sara"))
{
    Console.WriteLine(leeftijden["Sara"]);
}
else
{
    Console.WriteLine("Sara niet gevonden");
}
```

### Veilig opvragen met `TryGetValue`

Als je een sleutel opvraagt die niet bestaat via `dictionary[sleutel]`, krijg je een `KeyNotFoundException`. Met `TryGetValue` kun je dat vermijden:

```csharp
if (leeftijden.TryGetValue("Sara", out int leeftijd))
{
    Console.WriteLine($"Sara is {leeftijd} jaar.");
}
else
{
    Console.WriteLine("Sara niet gevonden.");
}
```

### Itereren over een `Dictionary`

```csharp
foreach (KeyValuePair<string, int> paar in leeftijden)
{
    Console.WriteLine($"{paar.Key} is {paar.Value} jaar.");
}
```

### Wanneer gebruik je een `Dictionary<TKey, TValue>`?

- Je wilt data **opzoeken via een sleutel** in plaats van via een index.
- Je hebt een **logisch verband** tussen twee stukken data (naam → leeftijd, studentnummer → student, productcode → prijs).
- Je wilt snel (`O(1)`) kunnen opvragen of een sleutel bestaat en wat de bijhorende waarde is.

### Veelgemaakte fouten

```csharp
// FOUT: twee keer dezelfde sleutel toevoegen met Add() → exception
leeftijden.Add("Amir", 22);
leeftijden.Add("Amir", 25); // ArgumentException!

// GOED: gebruik de indexer om te overschrijven
leeftijden["Amir"] = 25; // Overschrijft de bestaande waarde
```

---

## Andere nuttige collecties

### `Queue<T>` — wachtrij (FIFO)

Een `Queue<T>` werkt als een wachtrij in het dagelijkse leven: wie eerst aansluit, wordt eerst bediend (**First In, First Out**).

```csharp
Queue<string> wachtrij = new Queue<string>();

wachtrij.Enqueue("Klant 1"); // Achteraan aansluiten
wachtrij.Enqueue("Klant 2");
wachtrij.Enqueue("Klant 3");

string volgende = wachtrij.Dequeue(); // "Klant 1" (vooraan weghalen)
string wieBenBenieuwd = wachtrij.Peek(); // "Klant 2" (kijken zonder te verwijderen)
```

**Wanneer?** Als de volgorde van verwerking ertoe doet en je elementen in dezelfde volgorde wilt afhandelen als ze binnenkomen.

#### Reëel voorbeeld: een printwachtrij

Een klassiek voorbeeld van een `Queue<T>` is een printwachtrij. Documenten worden aangeboden in een bepaalde volgorde, en de printer verwerkt ze in diezelfde volgorde — het eerste document dat in de wachtrij komt, wordt als eerste afgedrukt.

```csharp
class Document
{
    public string Titel { get; set; }

    public void Print()
    {
        Console.WriteLine($"Printing {Titel}...");
    }
}

class Program
{
    static Queue<Document> printQueue = new Queue<Document>();

    static void Main(string[] args)
    {
        Document d1 = new Document() { Titel = "document 1" };
        Document d2 = new Document() { Titel = "document 2" };
        Document d3 = new Document() { Titel = "document 3" };

        printQueue.Enqueue(d1);
        printQueue.Enqueue(d2);
        PrintNextFromQueue(); // Printing document 1...

        printQueue.Enqueue(d3);
        PrintNextFromQueue(); // Printing document 2...
        PrintNextFromQueue(); // Printing document 3...
    }

    static void PrintNextFromQueue()
    {
        Document tePrintenDocument = printQueue.Dequeue();
        tePrintenDocument.Print();
    }
}
```

Merk op dat `d1` eerst wordt afgedrukt, ook al zit `d2` er al in op het moment dat `PrintNextFromQueue()` wordt aangeroepen. De queue garandeert de volgorde. En wanneer `d3` later wordt toegevoegd, schuift die gewoon achteraan aan — de al wachtende `d2` wordt niet overgeslagen.

Dit patroon zie je overal terug: berichten in een chatapplicatie die in volgorde moeten worden verwerkt, taken die een server één voor één moet afhandelen, bestellingen in een webshop die in volgorde moeten worden klaargemaakt.

### `Stack<T>` — stapel (LIFO)

Een `Stack<T>` werkt als een stapel borden: je legt er bovenop en neemt er bovenaf weg (**Last In, First Out**).

```csharp
Stack<string> stapel = new Stack<string>();

stapel.Push("Actie 1"); // Bovenop leggen
stapel.Push("Actie 2");
stapel.Push("Actie 3");

string laatst = stapel.Pop(); // "Actie 3" (bovenste eraf halen)
string bovenste = stapel.Peek(); // "Actie 2" (kijken zonder te verwijderen)
```

**Wanneer?** Als het *laatst toegevoegde* element het *eerst verwerkt* moet worden: undo-functionaliteit, navigatiegeschiedenis, expressies evalueren.

#### Reëel voorbeeld: haakjes valideren

Een minder voor de hand liggend maar krachtig voorbeeld: controleren of haakjes in een tekst correct genest zijn. Denk aan expressies als `"abc(def[ghi])jkl"` — elke openingshaak moet afgesloten worden door het juiste type sluithaak, en in de juiste volgorde.

Waarom past een `Stack<T>` hier perfect? Omdat het **laatst geopende haakje** het **eerst gesloten** moet worden — dat is precies LIFO-gedrag.

Het algoritme werkt als volgt: loop door elk karakter in de tekst. Als je een openingshaakje tegenkomt, `Push` het op de stack. Als je een sluithaakje tegenkomt, `Pop` het bovenste element van de stack en controleer of het matcht. Als de stack leeg is terwijl je een sluithaakje tegenkomt, of als er na afloop nog openingshaakjes op de stack liggen, is de tekst ongeldig.

```csharp
class HaakjesParser
{
    private Dictionary<string, string> haakjesCombos = new Dictionary<string, string>();

    public void AddHaakjesCombo(string openingsHaakje, string sluitHaakje)
    {
        haakjesCombos[openingsHaakje] = sluitHaakje;
    }

    public bool IsGeldig(string tekst)
    {
        Stack<string> stack = new Stack<string>();

        foreach (char c in tekst)
        {
            string karakter = c.ToString();

            if (haakjesCombos.ContainsKey(karakter))
            {
                // Het is een openingshaakje → op de stack duwen
                stack.Push(karakter);
            }
            else if (haakjesCombos.ContainsValue(karakter))
            {
                // Het is een sluithaakje
                if (stack.Count == 0)
                {
                    // Er is geen openingshaakje dat hierop wacht
                    return false;
                }

                string laatsteOpening = stack.Pop();

                if (haakjesCombos[laatsteOpening] != karakter)
                {
                    // Het sluithaakje matcht niet met het laatst geopende
                    return false;
                }
            }
            // Andere karakters negeren we gewoon
        }

        // Als de stack leeg is, zijn alle haakjes correct afgesloten
        return stack.Count == 0;
    }
}
```

```csharp
HaakjesParser parser = new HaakjesParser();
parser.AddHaakjesCombo(openingsHaakje: "(", sluitHaakje: ")");
parser.AddHaakjesCombo(openingsHaakje: "[", sluitHaakje: "]");
parser.AddHaakjesCombo(openingsHaakje: "{", sluitHaakje: "}");

Console.WriteLine(parser.IsGeldig("abc(def[ghi])jkl")); // True
Console.WriteLine(parser.IsGeldig("abc(def"));            // False — ( nooit gesloten
Console.WriteLine(parser.IsGeldig("abc(de][fgh)i"));      // False — ] matcht niet met (
Console.WriteLine(parser.IsGeldig("abc{def})ghi"));       // False — ) matcht niet met {
Console.WriteLine(parser.IsGeldig("[abc](def)[ghi)"));    // False — ) matcht niet met [
```

Merk op dat dit voorbeeld tegelijk een `Stack<T>` *en* een `Dictionary<TKey, TValue>` gebruikt — de dictionary legt het verband tussen openings- en sluithaakjes, de stack houdt de nesting bij. Dat is een mooi voorbeeld van hoe collectietypes elkaar aanvullen.

### `LinkedList<T>` — de gelinkte lijst

Een `LinkedList<T>` slaat elementen op als een ketting van *nodes* (knooppunten). Elk knooppunt bevat de waarde zelf, plus een verwijzing naar het vorige en het volgende knooppunt. Er is geen interne array — de elementen zitten verspreid in het geheugen en zijn enkel via die verwijzingen aan elkaar gelinkt.

```csharp
LinkedList<string> ketting = new LinkedList<string>();

ketting.AddLast("Amir");
ketting.AddLast("Fatima");
ketting.AddFirst("Jan"); // Vooraan toevoegen

// Ketting is nu: Jan ↔ Amir ↔ Fatima

// Ergens in het midden invoegen
LinkedListNode<string> fatima = ketting.Find("Fatima");
ketting.AddBefore(fatima, "Sara");
// Ketting is nu: Jan ↔ Amir ↔ Sara ↔ Fatima

// Verwijderen
ketting.Remove("Amir");
// Ketting is nu: Jan ↔ Sara ↔ Fatima
```

#### Hoe verschilt dit van een `List<T>`?

Het cruciale verschil zit in hoe invoegen en verwijderen werken. Bij een `List<T>` (die intern een array gebruikt) moet bij het invoegen of verwijderen in het midden alles erna opgeschoven worden — precies het werk dat je kent van arrays. Bij een `LinkedList<T>` worden enkel de verwijzingen van de buurknooppunten aangepast. Dat is een operatie die altijd even snel is, ongeacht de grootte van de lijst.

Maar daar staat een groot nadeel tegenover: je kunt **niet via een index** een element opvragen. Er is geen `ketting[3]`. Om bij het vierde element te komen, moet je bij het eerste beginnen en drie keer "volgende" volgen. Dat maakt een `LinkedList<T>` traag voor willekeurige toegang.

| Bewerking | `List<T>` | `LinkedList<T>` |
|---|---|---|
| Element opvragen via index | Snel (`O(1)`) | Niet mogelijk |
| Invoegen/verwijderen aan begin of einde | Begin: traag (`O(n)`), einde: snel | Snel (`O(1)`) |
| Invoegen/verwijderen in het midden | Traag (`O(n)`, opschuiven) | Snel (`O(1)`)* |
| Geheugenverbruik per element | Laag (enkel de waarde) | Hoger (waarde + 2 verwijzingen) |
| `Contains()` | Traag (`O(n)`) | Traag (`O(n)`) |

\* Mits je al een referentie naar het knooppunt hebt (via `Find()` of door het bij te houden). Het zoeken naar dat knooppunt kost zelf wel `O(n)`.

#### Wanneer gebruik je een `LinkedList<T>`?

Eerlijk: in de praktijk is een `LinkedList<T>` in C# zelden de beste keuze. Een `List<T>` is voor de meeste toepassingen sneller dankzij betere *cache-locality* — de elementen zitten achter elkaar in het geheugen, wat de processor veel efficiënter kan verwerken dan verspreide knooppunten.

> **Korte uitweiding: cache-locality**
>
> Je processor haalt data niet één waarde tegelijk op uit het werkgeheugen (RAM). Dat zou veel te traag zijn — RAM is, vanuit het perspectief van een processor, *ongelooflijk* traag. In plaats daarvan haalt de processor telkens een heel blok geheugen op en bewaart dat in een kleine, razendsnelle tussenopslag: de **cache**.
>
> Bij een `List<T>` (of een array) staan alle elementen netjes achter elkaar in het geheugen. Als de processor element 0 ophaalt, zitten elementen 1, 2, 3, … waarschijnlijk al mee in datzelfde blok. De volgende iteraties van je `foreach`-loop zijn dan praktisch gratis — de data zit al in de cache.
>
> Bij een `LinkedList<T>` is elk knooppunt een apart object dat *ergens* in het geheugen staat. Knooppunt 0 kan op geheugenadres 1000 zitten, knooppunt 1 op adres 50000, knooppunt 2 op adres 8200. Elke stap in je loop vereist potentieel een nieuwe, trage ophaling uit RAM, omdat het volgende knooppunt nergens in de buurt van het vorige hoeft te staan. Dat heet een **cache miss**, en die zijn duur.
>
> Het verschil is in theorie "slechts" een constante factor — beide zijn `O(n)` om alles te doorlopen — maar in de praktijk kan dat verschil makkelijk een factor 10 tot 100 zijn bij grote collecties. Daarom wint een `List<T>` het bijna altijd, zelfs voor bewerkingen waar een `LinkedList<T>` op papier beter scoort.

Een `LinkedList<T>` is pas zinvol als je **heel vaak invoegt of verwijdert aan het begin of in het midden** van een grote collectie, en je de knooppuntreferenties bijhoudt zodat je niet telkens opnieuw hoeft te zoeken. Denk aan scenario's als een playlist-editor of een tekst-buffer waar je voortdurend tussenvoegt.

> **In de praktijk:** als je twijfelt tussen `List<T>` en `LinkedList<T>`, kies dan `List<T>`. Het is bijna altijd de betere keuze in C#.

---

## Wanneer gebruik je welk collectietype?

| Situatie | Collectietype |
|---|---|
| Vaste grootte, maximale performance, geordende lijst, index nodig, duplicaten toegestaan | `Array (`T[]`)` |
| Geordende lijst, index nodig, duplicaten toegestaan, voorgedefinieerde functionaliteiten voor toevoegen, invoegen, verwijderen, ... | `List<T>` |
| Geen duplicaten, snelle `Contains()` nodig, geen aparte elementen aan te spreken, maken van doorsnede, unie of verschil | `HashSet<T>` |
| Opzoeken via sleutel, snelle `Contains()` nodig | `Dictionary<TKey, TValue>` |
| First In, First Out verwerking | `Queue<T>` |
| Last In, First Out verwerking | `Stack<T>` |
| Veel invoegen/verwijderen aan begin of midden | `LinkedList<T>` |
| Vaste grootte, maximale performantie | Array (`T[]`) |

> **De standaardkeuze:** als je twijfelt, gebruik een `Array<T>` bij vaste grootte, of een `List<T>` indien er tijdens uitvoer elementen aan worden toegevoegd of verwijderd.  Pas als je merkt dat je specifiek unieke waarden nodig hebt of data via een sleutel wilt opzoeken, schakel je over naar een `HashSet<T>` of `Dictionary<TKey, TValue>`.

---

## Valkuil: verwijderen tijdens het itereren

Een van de lastigste bugs die je kunt tegenkomen: elementen verwijderen uit een collectie terwijl je er met een `foreach` doorheen loopt.

### Het probleem

```csharp
List<int> getallen = new List<int> { 1, 2, 3, 4, 5 };

// DIT CRASHT: InvalidOperationException
foreach (int getal in getallen)
{
    if (getal % 2 == 0)
    {
        getallen.Remove(getal); // Collection was modified!
    }
}
```

Een `foreach` gebruikt intern een *enumerator* die bijhoudt waar hij in de collectie zit. Als je de collectie wijzigt terwijl die enumerator actief is, gooit C# een `InvalidOperationException`.

Maar *waarom* eigenlijk? Wat gaat er precies mis?

#### Wat doet een enumerator?

Wanneer je `foreach` schrijft, vertaalt de compiler dat naar iets dat conceptueel hierop neerkomt:

```csharp
// Dit:
foreach (int getal in getallen)
{
    Console.WriteLine(getal);
}

// Wordt intern ruwweg:
IEnumerator<int> enumerator = getallen.GetEnumerator();
while (enumerator.MoveNext())
{
    int getal = enumerator.Current;
    Console.WriteLine(getal);
}
```

De enumerator is een object dat een **interne positie** bijhoudt in de collectie — "ik ben nu bij element 2, het volgende is element 3". Dat doet hij typisch via een index of pointer die bij elke `MoveNext()` eentje opschuift.

#### Waarom is wijzigen dan een probleem?

Stel dat je een `List<int>` hebt met `{ 1, 2, 3, 4, 5 }` en de enumerator staat bij index 1 (waarde `2`). Je verwijdert die `2`. Wat gebeurt er intern?

- Alle elementen na index 1 schuiven een positie op: de lijst wordt `{ 1, 3, 4, 5 }`.
- De enumerator weet daar niets van en gaat gewoon door naar index 2.
- Maar index 2 is nu `4` in plaats van `3` — je hebt `3` overgeslagen!

Ook bij andere collectietypes als `HashSet<T>` of `Dictionary<TKey, TValue>` kom je bij het verwijderen van elementen tijdens het enumereren in de problemen. 

Daarom kiest C# ervoor om het gewoon te verbieden: elke collectie houdt intern een **versienummer** bij. Bij elke wijziging (toevoegen, verwijderen) wordt dat versienummer verhoogd. De enumerator onthoudt het versienummer van het moment dat hij werd aangemaakt, en bij elke `MoveNext()` controleert hij of het nog klopt. Zo niet → `InvalidOperationException`.

### Oplossingen

**Optie 1: achterwaartse `for`-loop (bij `List<T>`)**

```csharp
List<int> getallen = new List<int> { 1, 2, 3, 4, 5 };

for (int i = getallen.Count - 1; i >= 0; i--)
{
    if (getallen[i] % 2 == 0)
    {
        getallen.RemoveAt(i);
    }
}
// Resultaat: { 1, 3, 5 }
```

Waarom achterwaarts? Als je voorwaarts loopt en element op index 2 verwijdert, schuiven alle elementen erna een positie op. Element dat op index 3 stond, staat nu op index 2 — maar jouw teller gaat door naar index 3, waardoor je een element overslaat. Achterwaarts lopen vermijdt dit probleem, want het opschuiven gebeurt enkel na de huidige positie.

**Optie 2: `RemoveAll()` met een conditie (bij `List<T>`)**

```csharp
List<int> getallen = new List<int> { 1, 2, 3, 4, 5 };

getallen.RemoveAll(getal => getal % 2 == 0);
// Resultaat: { 1, 3, 5 }
```

Dit is de kortste en meest leesbare oplossing als je met een `List<T>` werkt. De lambda-expressie `getal => getal % 2 == 0` beschrijft welke elementen verwijderd moeten worden.

> **Opmerking:** lambda-expressies zijn een onderwerp dat jullie later nog apart zullen zien. Voorlopig kun je `getal => getal % 2 == 0` lezen als "een functie die voor een gegeven getal `true` teruggeeft als het even is". 

**Optie 3: een kopie maken om doorheen te itereren**

```csharp
HashSet<string> namen = new HashSet<string> { "Amir", "Fatima", "Jan", "Sara" };

foreach (string naam in namen.ToList()) // .ToList() maakt een kopie
{
    if (naam.StartsWith("A"))
    {
        namen.Remove(naam); // Veilig: we itereren over de kopie
    }
}
```

Dit werkt voor *elk* collectietype. Je itereert over een kopie (via `.ToList()`), dus de originele collectie mag gewoon gewijzigd worden.

**Optie 4: een aparte lijst van te verwijderen elementen bijhouden**

```csharp
Dictionary<string, int> scores = new Dictionary<string, int>
{
    { "Amir", 45 }, { "Fatima", 80 }, { "Jan", 30 }, { "Sara", 92 }
};

List<string> teVerwijderen = new List<string>();

foreach (KeyValuePair<string, int> paar in scores)
{
    if (paar.Value < 50)
    {
        teVerwijderen.Add(paar.Key);
    }
}

foreach (string sleutel in teVerwijderen)
{
    scores.Remove(sleutel);
}
// scores bevat nu: { "Fatima": 80, "Sara": 92 }
```

---

## Overschakelen tussen collectietypes

Je zit niet vast aan één collectietype. C# maakt het eenvoudig om van het ene type naar het andere over te schakelen.

### Van array naar collectie

```csharp
string[] namenArray = { "Amir", "Fatima", "Jan" };

// Array → List
List<string> namenList = new List<string>(namenArray);
// of: List<string> namenList = namenArray.ToList();

// Array → HashSet
HashSet<string> namenSet = new HashSet<string>(namenArray);
```

### Van collectie naar array

```csharp
List<string> namen = new List<string> { "Amir", "Fatima", "Jan" };

string[] namenArray = namen.ToArray();
```

### Tussen collectietypes onderling

```csharp
List<string> lijst = new List<string> { "Amir", "Fatima", "Amir", "Jan" };

// List → HashSet (verwijdert automatisch duplicaten!)
HashSet<string> set = new HashSet<string>(lijst);
// set bevat: { "Amir", "Fatima", "Jan" }

// HashSet → List
List<string> terugNaarLijst = new List<string>(set);
// of: List<string> terugNaarLijst = set.ToList();
```

### Praktisch nut: duplicaten verwijderen

Een handige truc: als je duplicaten uit een `List<T>` wilt verwijderen, converteer je tijdelijk naar een `HashSet<T>` en terug:

```csharp
List<string> metDuplicaten = new List<string> { "rood", "blauw", "rood", "groen", "blauw" };

List<string> zonderDuplicaten = new HashSet<string>(metDuplicaten).ToList();
// Resultaat: { "rood", "blauw", "groen" }
```

> **Let op:** bij de conversie naar een `HashSet<T>` verlies je de gegarandeerde volgorde. In de praktijk blijft de volgorde vaak behouden, maar je mag er niet op rekenen.

### Een `Dictionary` opbouwen vanuit een lijst

```csharp
List<string> studenten = new List<string> { "Amir", "Fatima", "Jan" };

Dictionary<string, int> scores = new Dictionary<string, int>();
foreach (string student in studenten)
{
    scores[student] = 0; // Iedereen begint op 0
}
```

---

## Samenvatting

- **`List<T>`** is je standaard vervanging voor een array: flexibel, geordend, index-gebaseerd.
- **`HashSet<T>`** garandeert unieke elementen en biedt razendsnelle opzoekingen via hashing.
- **`Dictionary<TKey, TValue>`** laat je data opzoeken via een sleutel, ook op basis van hashing.
- **`Queue<T>`** en **`Stack<T>`** zijn gespecialiseerde collecties voor respectievelijk FIFO- en LIFO-verwerking.
- **`LinkedList<T>`** blinkt uit in invoegen/verwijderen zonder opschuiven, maar mist index-gebaseerde toegang en is in C# zelden de beste keuze.
- **Verwijderen tijdens iteratie** mag niet via `foreach`. Gebruik een achterwaartse `for`-loop, `RemoveAll()`, een kopie, of een aparte lijst.
- Je kunt eenvoudig **schakelen tussen collectietypes** via constructors of `.ToList()` / `.ToArray()`.
- Hashing-gebaseerde collecties (zowel `HashSet<T>` als `Dictionary<TKey, TValue>`) gebruiken een interne bucketstructuur die opzoekingen in constante tijd (`O(1)`) mogelijk maakt.
