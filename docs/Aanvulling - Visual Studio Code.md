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

# Visual Studio Code in plaats van Visual Studio (Community/Professional/Enterprise)

In dit opleidingsonderdeel bouwen we in hoofdzaak .NET Console App's.  Je kan dat met een ontwikkelomgeving als Visual Studio (*Community*/*Professional*/*Enterprise*), maar je zou ook aan de slag kunnen gaan met **Visual Studio Code** (kortweg ook wel VS Code).   VS Code draait op **Windows, macOS en Linux**, dus iedereen kan ermee aan de slag, ongeacht het besturingssysteem.

Belangrijk om te begrijpen: **Visual Studio en VS Code zijn dus twee verschillende ontwikkelomgevingen**.

- Visual Studio (*Community*/*Professional*/*Enterprise*) is een **volledige IDE** (*Integrated Development Environment*): alles voor C# zit er standaard in.
- VS Code is een **lichte code-editor** die je **uitbreidt met *extensies***. Voor C# bijvoorbeeld installeer je de extensie *C# Dev Kit*. Die voegt o.a. een *Solution Explorer* (*C# Project Details*) toolvenster toe, *IntelliSense* code-aanvulassistentie, testondersteuning en een debugger toe.

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
> Installeer geen versie met *Preview* of *RC* (Release Candidate) in de naam. *.NET 11* bijvoorbeeld is op van schrijven een release candidate en verschijnt normaal in november 2026.

#### Op Windows

Heb je ooit reeds een ontwikkelomgeving geïnstalleerd om .NET applicaties te bouwen, dan staat de .NET SDK vermoedelijk al op je toestel. Controleer dat eerst:

1. Open de opdrachtprompt of de Terminal via het startmenu: Zoek in je programma's naar "opdrachtprompt" of "terminal" en start deze op, of druk op `Win+R`, typ `cmd` of `powershell` en druk op `Enter`.
2. Neem volgend commando over (hiermee laten we de geïnstalleerde .NET SDK's oplijsten):
    ```
    dotnet --list-sdks
    ```
3. Zie je een regel die begint met `10.0.`?  Dan kan je het installeren van deze .NET SDK overslaan.  Heb je nog een oudere versie, dan kan je die gerust laten staan: meerdere versies kunnen naast elkaar bestaan.  Ga in dat geval wel verder met het installeer van de nieuwste versie.

Downloaden en installeren:

1. Surf naar [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download).
2. Kies **.NET 10.0** en download onder **SDK** de **Windows Installer**: (Twijfel je over je processor? Kijk bij *Instellingen → Systeem → Info → Systeemtype*.)
    - **x64** voor de meeste laptops (Intel of AMD);
    - **Arm64** enkel voor laptops met een ARM-processor (bv. Snapdragon).
3. Voer het installatiebestand uit en volg de stappen.
4. Sluit alle geopende terminalvensters. 
5. Controleer opnieuw met `dotnet --list-sdks`.

#### Op macOS

1. Open **Terminal** (via Spotlight: `⌘ Spatie` en typ *Terminal*) en controleer of welk .NET SDK reeds geïnstalleerd zijn:
    ```
    dotnet --list-sdks
    ```
    Indien er geen regel is die begint met `10.0.` ga je verder met de volgende stappen (installatie).
2. Ga na welke processor je Mac heeft: klik op het **Apple-menu (appel linksboven) → Over deze Mac**.
    - Staat er **Chip: Apple M1/M2/M3/M4/…**? Dan heb je **Arm64** nodig.
    - Staat er **Processor: Intel**? Dan heb je **x64** nodig.
3. Surf naar [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download).
4. Kies **.NET 10.0** en download onder **SDK** de **macOS Installer** (Arm64 of x64).
5. Open het `.pkg`-bestand en volg de stappen. (Je zal allicht je Mac-wachtwoord nodig hebben.)
6. Controleer opnieuw met `dotnet --list-sdks`.  Je zou nu wel een regel moeten zien die begint met `10.0.`.

#### Alternatief: installatie van de SDK via de C# Dev Kit

Na de installatie van VS Code en de extensie C# Dev Kit (stap 1.3) opent een *walkthrough*. Via **Set up your environment → Install .NET SDK** kan je de SDK ook van daaruit installeren. 
De manuele installatie hierboven is voorspelbaarder, je kiest zelf welke versie je krijgt.

### 1.2 Visual Studio Code installeren

#### Op Windows

1. Surf naar [code.visualstudio.com](https://code.visualstudio.com) en klik op **Download for Windows**. Je krijgt dan de *User Installer*. (Die heeft geen administratorrechten nodig.)
2. Voer het installatiebestand uit.
3. Laat bij **Select Additional Tasks** deze opties aangevinkt of vink ze aan:
    - **Add "Open with Code" action to Windows Explorer directory context menu**: hiermee kan je een map openen met rechtsklik → *Open with Code*;
    - **Add to PATH**: hiermee kan je `code .` typen in een terminal (zie verderop).
4. Rond de installatie af en start VS Code.

#### Op macOS

1. Surf naar [code.visualstudio.com](https://code.visualstudio.com) en klik op **Download for Mac**. Je krijgt een `.zip`-bestand (*Universal*: werkt op Apple Silicon en Intel).
2. Pak het bestand uit (dubbelklik in *Downloads*).
3. **Sleep `Visual Studio Code.app` naar de map *Programma's*** (*Applications*).
    Laat het programma niet in *Downloads* staan. Anders werken automatische updates niet goed.
4. Start VS Code vanuit *Programma's* of via Spotlight. Bevestig de beveiligingsmelding met **Open**.
5. Open het **Command Palette** met `⇧⌘P`, typ `shell command` en kies **Shell Command: Install 'code' command in PATH**. Nu kan je in Terminal `code .` typen om een map te openen (zie verderop).

### 1.3 De extensie C# Dev Kit installeren

1. Open in VS Code de weergave **Extensions**: klik op het blokjes-icoon in de *Activity Bar* links, of gebruik `Ctrl+Shift+X` (Windows) of `⇧⌘X` (macOS).
2. Zoek naar **C# Dev Kit**.
3. Controleer dat de uitgever **Microsoft** is (met blauw vinkje). 
4. Klik op **Install**. De extensies **C#** en **.NET Install Tool** worden automatisch mee geïnstalleerd.
5. Er opent een *walkthrough* (**Get Started with C# Dev Kit**). Heb je de SDK al geïnstalleerd, dan mag je die sluiten.

> "Licentie en aanmelden"
    C# Dev Kit is **gratis voor persoonlijk, academisch en open-source gebruik**. Het valt onder dezelfde licentievoorwaarden als Visual Studio Community.
>
> De extensie kan vragen om je aan te melden met een Microsoft-account. Aanmelden wordt technisch niet afgedwongen, maar hoort wel bij de licentievoorwaarden. Je kan je aanmelden met je Microsoft **schoolaccount** (werk- of schoolaccount) via het **Accounts**-icoon linksonder.

### 1.4 Aanbevolen instellingen

Open de instellingen als JSON:

1. Open het Command Palette: `Ctrl+Shift+P` / `⇧⌘P`.
2. Typ `user settings json` en kies **Preferences: Open User Settings (JSON)**.
3. Voeg deze regels toe tussen de accolades `{ }`. Staan er al instellingen? Zet dan een komma na de laatste bestaande regel.

```json
{
    "files.autoSave": "onFocusChange",
    "csharp.debug.console": "externalTerminal"
}
```

| Instelling | Effect |
|---|---|
| `"files.autoSave": "onFocusChange"` | Slaat bestanden automatisch op zodra de actieve teksteditor de muis- of toetsenbordfocus verliest.  Doe je dat niet, dan zal bijvoorbeeld bij het uitvoeren van code mogelijks de **laatst expliciet bewaarde** versie worden uitgevoerd, soms is dat niet wat je op het scherm ziet en dat kan uiteraard leiden tot verwarring. |
| `"csharp.debug.console": "externalTerminal"` | Deze instelling zorgt ervoor dat je C#-consoleapplicatie tijdens het debuggen opstart in een los, extern terminalvenster van je besturingssysteem, in plaats van binnen de interface van Visual Studio Code. Het gebruik van `"externalTerminal"` biedt ook de hoogst mogelijke compatibiliteit voor low-level console API's.`Console.ReadLine()` werkt in beide (*internal* en *external* terminal), maar instructies als `Console.ReadKey()`, `Console.Clear()` of deze voor het verplaatsen van de cursor werken enkel correct in een echte (*externe*) terminal. |



## 2. Een console-app aanmaken

Bij .NET heb je twee niveaus:

- Een **project** (`.csproj`) is één programma: je code, instellingen en verwijzingen.
- Een **solution** (`.slnx`, bij oudere projecten `.sln`) groepeert één of meer projecten, eigenlijk ook pas nodig als je meerdere projecten hebt en die van elkaar wil laten gebruik maken

Dat is dus identiek aan Visual Studio.

Open altijd een map, geen los bestand. VS Code werkt met **mappen**. Open altijd de map waarin je `.slnx`- of `.csproj`-bestand staat (**File → Open Folder…**). Open je een los `.cs`-bestand, of een map die veel te hoog ligt (bv. je hele *Documenten*-map), dan werkt C# Dev Kit niet of maar half, en ga je het project (of dus je programma) niet kunnen uitvoeren.

### 2.1 Via C# Dev Kit (grafisch)

1. Open het Command Palette: `Ctrl+Shift+P` / `⇧⌘P`.
2. Typ `new project` en kies **.NET: New Project…**.
3. Kies de template **Console App**.
4. Kies de **map** waarin het project moet komen, bv. `Documenten/PB`.
5. Geef het project een **naam**, bv. `HalloWereld`, en druk op `Enter`.
6. Kies **Create Project**. Wil je eerst extra opties zien? Kies dan **Show all template options**.
7. VS Code opent de nieuwe map. Krijg je de vraag **Do you trust the authors of the files in this folder?** Kies dan **Yes, I trust the authors**. Anders werkt de C#-ondersteuning niet.

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
    ├── bin/                 ← gecompileerde uitvoer 
    └── obj/                 ← tijdelijke tussenbestanden
```

> Werk je met Git?
>
> Voer in de map van de solution eenmalig `dotnet new gitignore` uit. Dan komen `bin/` en `obj/` niet in je repository terecht.

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