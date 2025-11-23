# Database

Her finder du alt data om GeoFA database schemaet. Informationern er delt op i to:
- Schemaer, som indeholder alle table-schemaer i JSON dokumenter.
- Meta, som er JSON dokumenter med meta-data for de enkelte udgivne temaer. Temaer er teknisk set database views, som er baseret på tabellerne nævnt ovenfor.
  
Det er først og fremmest meta-dokumenterne, som er interessante for dataanalytikere, udviklingere, GIS mv. Disse afspejler temaerne som de fremstår i de forskellige services, herunder
WFS og SQL. De indeholder feltstruktur, udfaldsrum for felter, titler, beskrivelser mv. Hvis man er interesseret i constraints og indekser på en enkelte felter,
fremgår de kun af schemaerne.
Ved ændringer af schemaer og meta-data bliver dette repository opdateret. Commit message vil indeholde oplysninger om typen af opdateringen, fx om det er en egentlig schema-ændring
i databasen eller om det blot er fx en titel, som er blevet ændret.
Commits til dette repository bliver automatisk skabt af GeoFA, så der er ingen mulighed for at lave pull requests.
