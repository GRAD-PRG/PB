<!--
Vereiste markdown_extensions in mkdocs.yml (Material for MkDocs):
  - admonition
  - pymdownx.highlight
  - pymdownx.superfences
  - pymdownx.tabbed: { alternate_style: true }
  - pymdownx.details
  - tables
  - def_list
  - attr_list
-->

# C# ontwikkelen met Visual Studio Code


In dit opleidingsonderdeel bouwen we in hoofdzaak .NET Console App's.  Je kan dat met een ontwikkelomgeving als **Visual Studio** (*Community*/*Professional*/*Enterprise*), maar je zou ook aan de slag kunnen gaan met **Visual Studio Code** (kortweg ook wel VS Code).   VS Code draait op Windows, macOS en Linux, dus iedereen kan ermee aan de slag, ongeacht het besturingssysteem.

Belangrijk om te begrijpen: Visual Studio en VS Code zijn dus **twee verschillende programma's**.

- **Visual Studio** (*Community*/*Professional*/*Enterprise*) is een volledige IDE (*Integrated Development Environment*): alles voor C# zit er standaard in.
- **VS Code** is een lichte code-editor die je uitbreidt met *extensies*. Voor C# installeer je de extensie **C# Dev Kit**. Die voegt o.a. een Solution Explorer, IntelliSense, testondersteuning en een debugger toe.

In beide gevallen gebeurt het eigenlijke compileren en uitvoeren door de **.NET SDK** (.NET Software Development Kit). Een project dat je in Visual Studio maakt, kan je dus zonder aanpassingen openen in VS Code en omgekeerd.

> Deze stukje is geschreven in september 2026. VS Code en aansluitende tools als Copilot krijgen elke maand updates: menu's en knoppen kunnen er bijgevolg dus iets anders uitzien wanneer jij ze te zien krijgt. De basisprincipes blijven wel dezelfde.

## 1. Installatie

Installeer de onderdelen en bij voorkeur **in deze volgorde**:

1. de .NET SDK
2. Visual Studio Code
3. de extensie C# Dev Kit

### 1.1 De .NET SDK installeren

> Installeer de **nieuwste stabiele versie** van de .NET SDK. Op dit moment is dat **.NET 10 (LTS)** (Long Term Support).
>
> Installeer **geen** versie met *Preview* of *RC* (Release Candidate) in de naam. **.NET 11** bijvoorbeeld is op van schrijven een release candidate en verschijnt normaal in november 2026.

### "Windows"

Heb je ooit reeds een ontwikkelomgeving geïnstalleerd om .NET applicaties te bouwen, dan staat de .NET SDK vermoedelijk al op je toestel. Controleer dat eerst:

1. Open **Terminal** (of *PowerShell*) via het startmenu.
2. Typ:
    ```
    dotnet --list-sdks
    ```
3. Zie je een regel die begint met `10.0.`? Dan kan je deze stap overslaan.

Anders:

1. Surf naar [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download).
2. Kies **.NET 10.0** en download onder **SDK** de **Windows Installer**:
    - **x64** voor de meeste laptops (Intel of AMD);
    - **Arm64** enkel voor laptops met een ARM-processor (bv. Snapdragon).
        Twijfel je? Kijk bij *Instellingen → Systeem → Info → Systeemtype*.
3. Voer het installatiebestand uit en volg de stappen.
4. Sluit alle geopende terminalvensters. Open daarna een nieuwe terminal en controleer met `dotnet --list-sdks`.

=== "macOS"

    1. Ga na welke processor je Mac heeft: klik op het **Apple-menu (appel linksboven) → Over deze Mac**.
        - Staat er **Chip: Apple M1/M2/M3/M4/…**? Dan heb je **Arm64** nodig.
        - Staat er **Processor: Intel**? Dan heb je **x64** nodig.
    2. Surf naar [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download).
    3. Kies **.NET 10.0** en download onder **SDK** de **macOS Installer** (Arm64 of x64).
    4. Open het `.pkg`-bestand en volg de stappen. Je Mac-wachtwoord wordt gevraagd.
    5. Open **Terminal** (via Spotlight: `⌘ Spatie` en typ *Terminal*) en controleer:
       ```
       dotnet --list-sdks
       ```
       Je zou een regel moeten zien die begint met `10.0.`.

!!! tip "Alternatief: installatie via de C# Dev Kit"
    Na de installatie van C# Dev Kit (stap 1.3) opent een *walkthrough*. Via **Set up your environment → Install .NET SDK** kan je de SDK ook van daaruit installeren. De manuele installatie hierboven is wel voorspelbaarder: je weet precies welke versie je krijgt.

### 1.2 Visual Studio Code installeren

=== "Windows"

    1. Surf naar [code.visualstudio.com](https://code.visualstudio.com) en klik op **Download for Windows**. Je krijgt dan de *User Installer*. Die heeft geen administratorrechten nodig.
    2. Voer het installatiebestand uit.
    3. Laat bij **Select Additional Tasks** deze opties aangevinkt of vink ze aan:
        - **Add "Open with Code" action to Windows Explorer directory context menu**: hiermee kan je een map openen met rechtsklik → *Open with Code*;
        - **Add to PATH**: hiermee kan je `code .` typen in een terminal.
    4. Rond de installatie af en start VS Code.

=== "macOS"

    1. Surf naar [code.visualstudio.com](https://code.visualstudio.com) en klik op **Download for Mac**. Je krijgt een `.zip`-bestand (*Universal*: werkt op Apple Silicon en Intel).
    2. Pak het bestand uit (dubbelklik in *Downloads*).
    3. **Sleep `Visual Studio Code.app` naar de map *Programma's*** (*Applications*).
       Laat het programma niet in *Downloads* staan. Anders werken automatische updates niet goed.
    4. Start VS Code vanuit *Programma's* of via Spotlight. Bevestig de beveiligingsmelding met **Open**.
    5. Open het **Command Palette** met `⇧⌘P`, typ `shell command` en kies **Shell Command: Install 'code' command in PATH**. Nu kan je in Terminal `code .` typen om een map te openen.

!!! note "Taal van de interface"
    Laat VS Code in het **Engels** staan. Documentatie, foutmeldingen, tutorials en deze handleiding gebruiken de Engelse namen van menu's en commando's.

### 1.3 De extensie C# Dev Kit installeren

1. Open in VS Code de weergave **Extensions**: klik op het blokjes-icoon in de *Activity Bar* links, of gebruik `Ctrl+Shift+X` (Windows) of `⇧⌘X` (macOS).
2. Zoek naar **C# Dev Kit**.
3. Controleer dat de uitgever **Microsoft** is (met blauw vinkje). Installeer geen namaakextensies met een gelijkaardige naam.
4. Klik op **Install**. De extensies **C#** en **.NET Install Tool** worden automatisch mee geïnstalleerd.
5. Er opent een *walkthrough* (**Get Started with C# Dev Kit**). Heb je de SDK al geïnstalleerd, dan mag je die sluiten.

!!! info "Licentie en aanmelden"
    C# Dev Kit is **gratis voor persoonlijk, academisch en open-source gebruik**. Het valt onder dezelfde licentievoorwaarden als Visual Studio Community.

    De extensie kan vragen om je aan te melden met een Microsoft-account. Aanmelden wordt technisch niet afgedwongen, maar hoort wel bij de licentievoorwaarden. Meld je daarom aan met je **schoolaccount** (werk- of schoolaccount) via het **Accounts**-icoon linksonder.

### 1.4 Aanbevolen instellingen

Open de instellingen als JSON:

1. Open het Command Palette: `Ctrl+Shift+P` / `⇧⌘P`.
2. Typ `user settings json` en kies **Preferences: Open User Settings (JSON)**.
3. Voeg deze regels toe tussen de accolades `{ }`. Staan er al instellingen? Zet dan een komma na de laatste bestaande regel.

```json
{
    "files.autoSave": "afterDelay",
    "csharp.debug.console": "integratedTerminal"
}
```

Waarom?

| Instelling | Effect |
|---|---|
| `files.autoSave` | Bestanden worden automatisch bewaard. Zonder deze instelling voert `dotnet run` in de terminal de **laatst bewaarde** versie uit, niet wat je op het scherm ziet. |
| `csharp.debug.console` | Je programma draait in het **Terminal**-paneel in plaats van in de *Debug Console*. `Console.ReadLine()` werkt in beide, maar `Console.ReadKey()`, `Console.Clear()` en het verplaatsen van de cursor werken enkel correct in een echte terminal. |

---

## 2. Een console-app aanmaken

Bij .NET heb je twee niveaus:

- Een **project** (`.csproj`) is één programma: je code, instellingen en verwijzingen.
- Een **solution** (`.slnx`, bij oudere projecten `.sln`) groepeert één of meer projecten, bv. alle oefeningen van een hoofdstuk.

Dat is identiek aan Visual Studio.

!!! warning "Open altijd een map, geen los bestand"
    VS Code werkt met **mappen**. Open altijd de map waarin je `.slnx`- of `.csproj`-bestand staat (**File → Open Folder…**). Open je een los `.cs`-bestand, of een map die veel te hoog ligt (bv. je hele *Documenten*-map), dan werkt C# Dev Kit niet of maar half.

### 2.1 Via C# Dev Kit (grafisch)

1. Open het Command Palette: `Ctrl+Shift+P` / `⇧⌘P`.
2. Typ `new project` en kies **.NET: New Project…**.
3. Kies de template **Console App**.
4. Kies de **map** waarin het project moet komen, bv. `Documenten/PB`.
5. Geef het project een **naam**, bv. `HalloWereld`, en druk op `Enter`.
6. Kies **Create Project**. Wil je eerst extra opties zien? Kies dan **Show all template options**.
7. VS Code opent de nieuwe map. Krijg je de vraag **Do you trust the authors of the files in this folder?** Kies dan **Yes, I trust the authors**. Anders werkt de C#-ondersteuning niet.

Links in de **Explorer** (`Ctrl+Shift+E` / `⇧⌘E`) zie je nu twee secties:

- de gewone **bestandsweergave** met alle mappen en bestanden;
- **Solution Explorer** met de solution, projecten en C#-bestanden, zoals in Visual Studio.

### 2.2 Via de terminal (dotnet CLI)

Soms gaat het sneller via de terminal. Die opdrachten werken op Windows én macOS. Je hebt ook meer controle, bv. om een klassieke `Main`-methode te krijgen in plaats van *top-level statements*.

Open een terminal in VS Code met **Terminal → New Terminal** (`` Ctrl+` `` / `` ⌃` ``) en typ:

```
mkdir Hoofdstuk1
cd Hoofdstuk1
dotnet new sln -n Hoofdstuk1
dotnet new console -n Oefening1 --use-program-main
dotnet sln add Oefening1/Oefening1.csproj
code .
```

| Opdracht | Betekenis |
|---|---|
| `dotnet new sln -n Hoofdstuk1` | maakt de solution `Hoofdstuk1.slnx` |
| `dotnet new console -n Oefening1 --use-program-main` | maakt een console-project in de map `Oefening1`, met een klasse `Program` en een `static void Main(...)` |
| `dotnet sln add …` | voegt het project toe aan de solution |
| `code .` | opent de huidige map in VS Code |

Laat je `--use-program-main` weg, dan krijg je een `Program.cs` met *top-level statements*: code zonder zichtbare klasse of `Main`.

### 2.3 Wat zit er in de map?

```
Hoofdstuk1/
├── Hoofdstuk1.slnx          ← solution
└── Oefening1/
    ├── Oefening1.csproj     ← projectbestand (instellingen, target framework)
    ├── Program.cs           ← je code
    ├── bin/                 ← gecompileerde uitvoer (niet in Git!)
    └── obj/                 ← tussenbestanden (niet in Git!)
```

!!! tip "Werk je met Git?"
    Voer in de map van de solution eenmalig `dotnet new gitignore` uit. Dan komen `bin/` en `obj/` niet in je repository terecht.

---

## 3. Een programma uitvoeren

Er zijn verschillende manieren om een programma te starten. Ze doen allemaal hetzelfde: het project bouwen en daarna uitvoeren.

| Manier | Windows | macOS |
|---|---|---|
| Uitvoeren **zonder** debugger | `Ctrl+F5` | `⌃F5` |
| Uitvoeren **met** debugger | `F5` | `F5` (op een MacBook eventueel `fn+F5`) |
| Menu | **Run → Run Without Debugging** / **Run → Start Debugging** | idem |
| Knop | ▷-icoon rechtsboven in de editor, terwijl een `.cs`-bestand open staat | idem |
| Solution Explorer | rechtsklik op het project → **Debug** → **Start New Instance** (met debugger) of **Start without Debugging** | idem |
| Terminal | `dotnet run` in de projectmap, of `dotnet run --project Oefening1` vanuit de solutionmap | idem |

De eerste keer kan VS Code vragen welke debugger je wil gebruiken. Kies **C#**. Bevat de solution meerdere projecten, dan vraagt VS Code ook welk project je wil starten.

De uitvoer en invoer van je programma verschijnen in het paneel **Terminal** onderaan, als je de instelling uit 1.4 hebt toegevoegd.

!!! note "Mac-toetsenbord"
    Op een MacBook zijn `F5`, `F10` enz. standaard mediatoetsen. Houd `fn` ingedrukt, of zet in *Systeeminstellingen → Toetsenbord* de optie aan om F1, F2 … als standaard functietoetsen te gebruiken.

---

## 4. Meerdere bestanden en opstartobjecten

### 4.1 Een klasse toevoegen aan een project

1. Rechtsklik in **Solution Explorer** op het project, bv. `Oefening1`, en kies **Add New File**.
2. Kies de template **Class** (of **Interface**, **Enum**, …).
3. Typ de naam, bv. `Persoon`, en druk op `Enter`.

Er verschijnt een bestand `Persoon.cs` met de juiste `namespace` en een lege klasse. Alle `.cs`-bestanden in de projectmap (en submappen) horen automatisch bij het project. Je hoeft ze dus nergens te registreren.

!!! tip "Handig: klasse naar eigen bestand verplaatsen"
    Heb je een klasse geschreven in `Program.cs`? Zet je cursor op de klassenaam en druk op `Ctrl+.` / `⌘.` (*Quick Fix*). Kies dan **Move type to Persoon.cs**.

### 4.2 Meerdere projecten in één solution

Vaak zet je meerdere oefeningen als aparte projecten in één solution.

**Project toevoegen**

- **Grafisch:** rechtsklik in Solution Explorer op de solution → **Add New Project** → **Console App** → naam.
- **Terminal** (in de solutionmap):
  ```
  dotnet new console -n Oefening2 --use-program-main
  dotnet sln add Oefening2/Oefening2.csproj
  ```

**Kiezen welk project start**

Visual Studio heeft een *startup project*. In VS Code kies je bij het starten welk project je uitvoert:

- Met `F5` of `Ctrl+F5` vraagt VS Code **welk project** je wil starten.
- In Solution Explorer: rechtsklik op het gewenste project → **Debug → Start New Instance**. Dat is meestal het snelst.
- In de terminal: `dotnet run --project Oefening2`.

Wil je niet telkens kiezen? Maak dan een vaste startconfiguratie. Maak in de solutionmap een map `.vscode` met daarin het bestand `launch.json`:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Oefening 1",
            "type": "dotnet",
            "request": "launch",
            "projectPath": "${workspaceFolder}/Oefening1/Oefening1.csproj"
        },
        {
            "name": "Oefening 2",
            "type": "dotnet",
            "request": "launch",
            "projectPath": "${workspaceFolder}/Oefening2/Oefening2.csproj"
        }
    ]
}
```

In de weergave **Run and Debug** (`Ctrl+Shift+D` / `⇧⌘D`) kies je nu bovenaan in de keuzelijst *Oefening 1* of *Oefening 2*. `F5` start daarna telkens de gekozen configuratie.

### 4.3 Meerdere klassen met een `Main`-methode in één project (opstartobject)

Soms wil je meerdere kleine programma's in **één** project, elk met een eigen `Main`:

```csharp title="Oefening1.cs"
namespace Hoofdstuk3;

class Oefening1
{
    static void Main()
    {
        Console.WriteLine("Dit is oefening 1");
    }
}
```

```csharp title="Oefening2.cs"
namespace Hoofdstuk3;

class Oefening2
{
    static void Main()
    {
        Console.WriteLine("Dit is oefening 2");
    }
}
```

Bouw je dit project, dan krijg je in het paneel **Problems** (`Ctrl+Shift+M` / `⇧⌘M`) deze fout:

> **CS0017**: Program has more than one entry point defined. Compile with /main to specify the type that contains the entry point.

De compiler weet niet welke `Main` hij moet gebruiken. In Visual Studio los je dat op via *Project Properties → Application → Startup object*. VS Code heeft geen eigenschappenvenster, maar Visual Studio schrijft die instelling gewoon in het `.csproj`-bestand. Dat kan je zelf ook:

1. Open `Hoofdstuk3.csproj` via de gewone bestandsweergave in de Explorer.
2. Voeg binnen `<PropertyGroup>` het element `<StartupObject>` toe. Gebruik de **volledige naam** van de klasse: namespace + klassenaam.

    ```xml title="Hoofdstuk3.csproj" hl_lines="8"
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>net10.0</TargetFramework>
        <ImplicitUsings>enable</ImplicitUsings>
        <Nullable>enable</Nullable>
        <StartupObject>Hoofdstuk3.Oefening2</StartupObject>
      </PropertyGroup>

    </Project>
    ```

3. Bewaar het bestand en start opnieuw (`Ctrl+F5` / `⌃F5`). Nu wordt `Oefening2.Main` uitgevoerd.

Wil je een andere oefening starten? Pas de waarde van `<StartupObject>` aan.

!!! warning "Niet combineren met top-level statements"
    Een opstartobject kiezen werkt **niet** als een bestand in het project *top-level statements* bevat, zoals de standaard `Program.cs` zonder `--use-program-main`. Gebruik in zo'n project overal een klassieke `static void Main()`. Of verwijder de `Program.cs` met top-level statements.

!!! tip "Alternatief: programma's van één bestand"
    Sinds .NET 10 kan je een los `.cs`-bestand uitvoeren zonder projectbestand. Maak een aparte map **zonder** `.csproj`, zet daar bv. `oefening5.cs` in met top-level statements, en voer uit in de terminal:
    ```
    dotnet run oefening5.cs
    ```
    Dat is handig voor heel kleine experimenten. Voor echte oefeningen en opdrachten werken we met projecten.

---

## 5. Debuggen

We gebruiken dit voorbeeld. Zet de code in `Program.cs` van een console-project:

```csharp title="Program.cs"
namespace DebugDemo;

class Program
{
    static void Main()
    {
        int som = 0;
        for (int i = 1; i <= 5; i++)
        {
            som += i;
        }
        Console.WriteLine($"Som: {som}");
        Console.WriteLine($"5! = {Faculteit(5)}");
    }

    static int Faculteit(int n)
    {
        if (n <= 1)
        {
            return 1;
        }
        return n * Faculteit(n - 1);
    }
}
```

### 5.1 Breakpoints plaatsen

- Klik in de **marge links van het regelnummer**. Er verschijnt een rood bolletje.
- Of zet de cursor op een regel en druk op `F9`.

Zet een breakpoint op de regel `som += i;` en op de regel `return 1;`.

### 5.2 De debugger starten en stappen

Druk op `F5`, of klik rechtsboven op het ▷-icoon met het kevertje. Het programma stopt op het eerste breakpoint. De huidige regel is geel gemarkeerd.

Bovenaan verschijnt de **debug-werkbalk**:

| Knop | Actie | Windows | macOS |
|---|---|---|---|
| ▷ Continue | verder tot het volgende breakpoint | `F5` | `F5` |
| ↷ Step Over | volgende regel; een methode-aanroep wordt in één stap uitgevoerd | `F10` | `F10` |
| ↓ Step Into | volgende regel; **ga binnen** in de aangeroepen methode | `F11` | `F11` |
| ↑ Step Out | voer de huidige methode af en keer terug naar de aanroeper | `Shift+F11` | `⇧F11` |
| ⟲ Restart | opnieuw starten | `Ctrl+Shift+F5` | `⇧⌘F5` |
| □ Stop | debuggen stoppen | `Shift+F5` | `⇧F5` |

### 5.3 Variabelen, Watch en Call Stack

Tijdens het debuggen opent links automatisch de weergave **Run and Debug** (`Ctrl+Shift+D` / `⇧⌘D`). Die bevat deze secties:

**VARIABLES**
: Klap **Locals** open. Daar zie je alle lokale variabelen van de huidige methode met hun huidige waarde, bv. `som` en `i`. Waarden die net veranderd zijn, worden gemarkeerd. Objecten en collecties klap je open met het pijltje. Via rechtsklik → **Set Value** kan je een waarde tijdens het debuggen aanpassen.

**WATCH**
: Klik op **+** en typ een expressie, bv. `som * 2` of `i > 3`. De waarde wordt na elke stap opnieuw berekend.

**CALL STACK**
: Toont de keten van methode-aanroepen die tot de huidige regel geleid heeft. De bovenste regel is de methode waarin je nu staat. Klik op een lagere regel om naar die aanroep te springen. **VARIABLES** toont dan de lokale variabelen van díe aanroep.

**BREAKPOINTS**
: Een overzicht van alle breakpoints. Hier kan je ze tijdelijk uitvinken. Je vindt hier ook **All Exceptions** en **User-Unhandled Exceptions**: vink die aan om de debugger te laten stoppen op het moment dat een exception optreedt.

!!! example "Probeer het"
    Druk op `Continue` tot je op `return 1;` stopt. In **CALL STACK** zie je nu vijf keer `Faculteit` boven `Main`. Klik ze één voor één aan en bekijk in **Locals** de waarde van `n` in elke aanroep.

Nog twee handige hulpmiddelen:

- **Hover:** houd je muis boven een variabele in de editor om de waarde te zien.
- **Debug Console** (`Ctrl+Shift+Y` / `⇧⌘Y`): typ een expressie, bv. `som + 100`, en druk op `Enter`. De debugger berekent die meteen, zoals het *Immediate Window* in Visual Studio.

### 5.4 Voorwaardelijke breakpoints en logpoints

Rechtsklik in de marge naast een regel:

- **Add Conditional Breakpoint…**: kies **Expression** en typ bv. `i == 4`. De debugger stopt enkel als de voorwaarde waar is. Met **Hit Count** stop je pas na een aantal keren.
- **Add Logpoint…**: de debugger stopt niet, maar schrijft een bericht naar de Debug Console, bv. `i = {i}, som = {som}`.

!!! note "macOS: wachtwoord bij de eerste debugsessie"
    Op macOS staat *Developer Mode* standaard uit. De eerste keer dat je debugt, vraagt macOS je wachtwoord om de debugger toegang te geven tot je programma. Dat is normaal.

---

## 6. GitHub Copilot

GitHub Copilot is ingebouwd in VS Code. Je hebt enkel een **GitHub-account** nodig met toegang tot een Copilot-abonnement.

!!! warning "Afspraken rond AI"
    Volg altijd de afspraken van het opleidingsonderdeel over het gebruik van AI. Die kunnen per opdracht of evaluatie verschillen. In 6.3 lees je hoe je inline suggesties uitschakelt.

### 6.1 Toegang krijgen

Er zijn twee gratis mogelijkheden:

**Copilot Free**
: Iedereen met een GitHub-account kan dit gebruiken, zonder verificatie. Je krijgt per maand een beperkt aantal codesuggesties en een beperkt chatgebruik.

**Copilot Student** (via GitHub Education)
: Gratis voor **geverifieerde studenten**. Je krijgt onbeperkt codesuggesties en een maandelijks budget voor chat.

    1. Meld je aan op [github.com](https://github.com). Voeg bij voorkeur je **schoolmailadres** toe aan je account (*Settings → Emails*).
    2. Ga naar [github.com/settings/education/benefits](https://github.com/settings/education/benefits) en kies **Start an application**.
    3. Volg de stappen. Je school wordt herkend via je schoolmailadres, en soms moet je een bewijs van inschrijving opladen (bv. een foto van je studentenkaart).
    4. **Goedkeuring en activering van Copilot zijn twee aparte stappen.** Na je goedkeuring kan het enkele dagen duren voor Copilot Student actief is. Je kan dat controleren via je profielfoto → **Copilot settings**.

    GitHub controleert elke maand opnieuw of je nog in aanmerking komt.

Tot je aanvraag goedgekeurd is, kan je gewoon met Copilot Free werken. In beide gratis plannen kiest Copilot automatisch het taalmodel: je kan dat niet zelf kiezen.

!!! info "Limieten veranderen"
    De exacte limieten van de gratis plannen past GitHub geregeld aan. Bekijk de actuele cijfers op [docs.github.com – Copilot plans](https://docs.github.com/en/copilot/get-started/plans).

### 6.2 Aanmelden in VS Code

1. Klik rechtsonder in de **Status Bar** op het **Copilot-icoon**.
2. Kies **Use AI Features** of **Sign in to use Copilot**.
3. Kies **Continue with GitHub**. Je browser opent: meld je aan en geef VS Code toestemming.
4. Terug in VS Code is Copilot actief. Het Copilot-icoon toont je plan en je verbruik.

Alternatief: via het **Accounts**-icoon linksonder, of via het Command Palette met **GitHub Copilot: Sign in**.

### 6.3 Inline suggesties (auto-aanvulling)

Terwijl je typt, stelt Copilot code voor in **grijze tekst** (*ghost text*).

| Actie | Windows | macOS |
|---|---|---|
| Suggestie aanvaarden | `Tab` | `Tab` |
| Enkel het volgende woord aanvaarden | `Ctrl+→` | `⌘→` |
| Suggestie negeren | `Esc` | `Esc` |
| Andere suggesties bekijken | beweeg de muis over de suggestie en gebruik de pijltjes in de werkbalk | idem |

- Je kan Copilot sturen met een duidelijke **methodenaam** of een **commentaarregel**, bv. `// geeft true terug als het jaar een schrikkeljaar is`.
- **Next Edit Suggestions:** na een wijziging stelt Copilot soms een aanpassing *elders* in het bestand voor, aangeduid met een pijltje in de marge. Met `Tab` spring je ernaartoe en met nog eens `Tab` aanvaard je de wijziging.

**Uitschakelen:** klik op het Copilot-icoon in de Status Bar. Daar kan je inline suggesties uitschakelen voor alle bestanden of enkel voor C#, of ze tijdelijk pauzeren (*snooze*).

!!! tip "Lees wat je aanvaardt"
    Een suggestie is een **voorstel**, geen garantie. Aanvaard nooit code die je niet kan uitleggen: bij een evaluatie of mondelinge toelichting moet jij het kunnen verklaren.

### 6.4 Het chatpaneel

Open de **Chat view** via het chaticoon bovenaan het venster, of met `Ctrl+Alt+I` / `⌃⌘I`.

- **Agent kiezen:** onderaan in het invoerveld kies je hoe Copilot werkt, bijvoorbeeld:
    - **Ask**: stelt vragen en geeft uitleg, maar past geen bestanden aan;
    - **Plan**: werkt eerst een stappenplan uit dat je kan nakijken;
    - **Agent**: past zelf bestanden aan en kan terminalopdrachten uitvoeren, telkens na jouw bevestiging.

    Het aanbod en de namen veranderen geregeld.
- **Context toevoegen:** typ `#` om naar een bestand (`#file`) of je hele codebase (`#codebase`) te verwijzen. Je kan ook bestanden naar het chatvenster slepen, of op **Add Context** klikken. Geselecteerde code in de editor wordt automatisch meegestuurd.
- **Slash-commando's:** bijvoorbeeld `/explain` (leg de geselecteerde code uit), `/fix` (stel een oplossing voor) en `/tests` (genereer unit tests).
- **Wijzigingen nakijken:** past Copilot bestanden aan, dan zie je de wijzigingen als *diff*. Je kan ze per bestand behouden of ongedaan maken.
- **Nieuwe chat:** begin een nieuw gesprek voor een nieuwe vraag, via **+** bovenaan of `Ctrl+N` / `⌘N` in de Chat view. Zo neemt Copilot geen irrelevante context mee uit een vorig gesprek.

### 6.5 Inline chat en slimme acties

- **Inline chat** (`Ctrl+I` / `⌘I`): stel een vraag of geef een opdracht **rechtstreeks in de editor**, bv. selecteer een methode en typ *"voeg invoercontrole toe"*. Je ziet het voorstel meteen in de code en kiest om het te aanvaarden of te verwerpen. Dat werkt ook in de terminal, bv. *"hoe voer ik project Oefening2 uit?"*.
- **Fouten:** klik bij een rood onderlijnde fout op het **lampje** (of `Ctrl+.` / `⌘.`). Naast de gewone *Quick Fixes* van C# vind je daar ook een optie om de fout met Copilot op te lossen.
- **Contextmenu:** rechtsklik in de editor → **Copilot** voor acties zoals uitleg, fix, review of tests genereren. De exacte namen kunnen per versie verschillen.
- **Commitberichten:** in de weergave **Source Control** genereert het ✨-icoon in het berichtveld een commitbericht op basis van je wijzigingen.

---

## 7. Sneltoetsen

| Actie | Windows | macOS |
|---|---|---|
| Command Palette (alle commando's) | `Ctrl+Shift+P` | `⇧⌘P` |
| Bestand snel openen | `Ctrl+P` | `⌘P` |
| Explorer / Solution Explorer | `Ctrl+Shift+E` | `⇧⌘E` |
| Extensions | `Ctrl+Shift+X` | `⇧⌘X` |
| Run and Debug | `Ctrl+Shift+D` | `⇧⌘D` |
| Terminal tonen/verbergen | `` Ctrl+` `` | `` ⌃` `` |
| Problems-paneel | `Ctrl+Shift+M` | `⇧⌘M` |
| Instellingen | `Ctrl+,` | `⌘,` |
| Uitvoeren zonder debugger | `Ctrl+F5` | `⌃F5` |
| Debuggen starten / verder | `F5` | `F5` |
| Breakpoint aan/uit | `F9` | `F9` |
| Quick Fix (lampje) | `Ctrl+.` | `⌘.` |
| Document formatteren | `Shift+Alt+F` | `⇧⌥F` |
| Hernoemen (overal) | `F2` | `F2` |
| Ga naar definitie | `F12` | `F12` |
| Copilot Chat view | `Ctrl+Alt+I` | `⌃⌘I` |
| Copilot inline chat | `Ctrl+I` | `⌘I` |

!!! note "AZERTY-toetsenbord"
    Sommige sneltoetsen met leestekens werken anders of niet op een Belgisch AZERTY-toetsenbord, bv. de terminal tonen met `` Ctrl+` ``. Gebruik dan het menu (**View → Terminal**), of zoek de juiste toetscombinatie op via **File → Preferences → Keyboard Shortcuts** (macOS: **Code → Settings → Keyboard Shortcuts**).

---

## 8. Van Visual Studio naar VS Code

| Visual Studio 2026 | VS Code + C# Dev Kit |
|---|---|
| *File → New → Project* | Command Palette → **.NET: New Project…** of `dotnet new console` |
| Solution Explorer | **Solution Explorer** (in de Explorer-weergave) |
| *Set as Startup Project* | project kiezen bij `F5`, rechtsklik → **Debug** → **Start New Instance**, of `launch.json` |
| *Project Properties → Startup object* | `<StartupObject>` in het `.csproj`-bestand |
| *Add → Class…* | rechtsklik op project → **Add New File** → **Class** |
| *Error List* | **Problems**-paneel |
| *Locals* / *Watch* / *Call Stack* | **VARIABLES → Locals** / **WATCH** / **CALL STACK** |
| *Immediate Window* | **Debug Console** |
| *Exception Settings* | **BREAKPOINTS** → *All Exceptions* / *User-Unhandled Exceptions* |
| *NuGet Package Manager* | `dotnet add package <naam>` in de terminal |

---

## 9. Problemen oplossen

??? question "`dotnet` wordt niet herkend als opdracht"
    Sluit **alle** terminals en VS Code volledig af en open ze opnieuw. Werkt het nog niet? Meld je op Windows af en opnieuw aan. Controleer daarna of de SDK echt geïnstalleerd is (stap 1.1).

??? question "Geen IntelliSense, alles rood onderlijnd of een lege Solution Explorer"
    - Heb je de **map** met het `.slnx`- of `.csproj`-bestand geopend (**File → Open Folder…**)? Een los bestand of een te hoge map werkt niet.
    - Heb je de map als **trusted** aangeduid?
    - Kijk in het paneel **Output** (**View → Output**) en kies rechts in de keuzelijst **C# Dev Kit** of **C#** voor foutmeldingen.
    - Probeer Command Palette → **Developer: Reload Window**.

??? question "Fout: *You must install or update .NET to run this application*"
    Het project gebruikt een andere .NET-versie dan de versie die op je computer staat. Kijk in het `.csproj`-bestand naar `<TargetFramework>`, bv. `net10.0`, en installeer die versie van de .NET SDK. Controleer met `dotnet --list-sdks`.

??? question "Mijn programma wacht op invoer, maar ik kan niets typen"
    Het programma draait waarschijnlijk in de *Debug Console*. Voeg de instelling `"csharp.debug.console": "integratedTerminal"` toe (stap 1.4) en start opnieuw. Typ je invoer daarna in het paneel **Terminal**.

??? question "VS Code vraagt telkens welke solution geopend moet worden"
    Er staan meerdere `.sln`- of `.slnx`-bestanden in de geopende map. Open een map die dieper ligt, of kies één solution. C# Dev Kit onthoudt die keuze.

??? question "macOS: VS Code update niet of gedraagt zich vreemd"
    Controleer of `Visual Studio Code.app` in de map *Programma's* staat en niet in *Downloads* (stap 1.2).
