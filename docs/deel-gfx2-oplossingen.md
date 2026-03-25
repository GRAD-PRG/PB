# Programmeren Basis - Deel GFX2 - Oplossingen
## Oplossingen
### Oplossing DGFX2.01
De oplossing vind je in [deel-gfx2-oplossing-01.zip](attachments/deel-gfx2-oplossing-01.zip)

### Oplossing DGFX2.02
Er is een potentieel conflict tussen de namen van de klassen

-   domain.Rectangle

-   System.Drawing.Rectangle

Ze hebben namelijk beiden de korte naam 'Rectangle'!

Normaliter is dit geen probleem : bij de volledige naam is er immers geen conflict, de ene begint met namespace `domain` en de andere met namespace `System.Drawing`.

Zodra we echter `using System.Drawing;` bovenaan onze code zetten, ontstaat er een conflict op plaatsen elders in de code waar het type `Rectangle` staat.

De compiler ziet nu 2 mogelijke klassen die bij de naam `Rectangle` zouden kunnen passen, vandaar het conflict.

### Oplossing DGFX2.03
Een objectdiagramn dat de situatie in het geheugen toont, op het moment dat method `DiagramControl.OnPaint()` wordt uitgevoerd.

![de oplossing](images/deel-gfx2-oplossing-03.png)

### Oplossing DGFX2.04
De oplossing vind je in [deel-gfx2-oplossing-04.zip](attachments/deel-gfx2-oplossing-04.zip)
