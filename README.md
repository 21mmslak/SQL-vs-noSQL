# En jämförelse mellan SQL och NoSQL-databaser
De första relationsdatabaserna utvecklades på 1970-talet, och `SQL` blev snabbt standarden för datalagring tack vare sin tydliga struktur och starka dataintegritet. När mängden data ökade dramatiskt på 2000-talet, och webbtjänster började växa, uppstod behovet av mer flexibla och skalbara lösningar. Därför skapades `NoSQL-databaser` – inte för att ersätta `SQL`, utan för att komplettera det i system som kräver snabbare åtkomst och horisontell skalbarhet.

## Bakgrund
### SQL (Structured Query Language)
`SQL` används i relationsdatabaser, där data organiseras i tabeller med rader och kolumner. Varje tabell representerar en typ av information, till exempel användare eller ordrar, och relationer mellan tabeller kan definieras med `primärnycklar` och `främmande nycklar` (foreign keys). Denna struktur gör det enkelt att undvika duplicerad data och hålla information konsekvent. `SQL-databaser` är mycket tillförlitliga och följer den så kallade `ACID-modellen` (Atomicity, Consistency, Isolation, Durability) som säkerställer att data alltid är korrekt, även vid systemfel. De passar särskilt bra i system där dataintegritet och transaktioner är viktiga, till exempel i betalningslösningar och lagerhantering.

### NoSQL (not only SQL)
`NoSQL` är ett samlingsnamn för databaser som inte använder den klassiska tabellstrukturen. I stället lagras data ofta i form av `dokument`, `nyckel-värdepar`, `grafer` eller `kolumner`, beroende på typ av `NoSQL-databas`. Detta gör det möjligt att hantera ostrukturerad eller snabbt växande data, där olika poster inte behöver ha samma fält. `NoSQL-databaser` prioriterar flexibilitet, prestanda och skalbarhet framför strikt datakonsistens. De används ofta i moderna webbtjänster, appar och system som kräver snabb åtkomst till stora datamängder, till exempel realtidsanalyser, logghantering och användarprofiler.

## Datamodellering
Exempel - anta att du vill spara användardata med namn, e-post och beställningar.

### SQL
```
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    product TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```
För att hämta ut användaren Pelle och hans ordrar med `SQL`:
```
SELECT users.name, orders.product
FROM users
JOIN orders ON users.id = orders.user_id
WHERE users.name = "Pelle";
```

### NoSQL
```
{
  "_id": 1,
  "name": "Pelle",
  "email": "pelle@example.com",
  "orders": [
    { "product": "Elsparkcykel" },
    { "product": "Hjälm" }
  ]
}
```
För att hämta ut användaren Pelle och hans ordrar med `NoSQL`:
```
db.users.find(
  { name: "Pelle" },
  { name: 1, orders: 1 }
)
```
Här syns tydligt skillnaden: `SQL` använder en `JOIN` mellan två tabeller, medan `NoSQL` kan hämta hela dokumentet direkt eftersom informationen redan ligger samlad.

## Dataintegritet och konsistens
En viktig skillnad mellan `SQL` och `NoSQL` är hur de hanterar dataintegritet.
`SQL-databaser` följer den så kallade `ACID-modellen` (Atomicity, Consistency, Isolation, Durability), vilket garanterar att data alltid är korrekt, även vid systemfel eller avbrott.

`NoSQL-databaser` prioriterar istället skalbarhet och använder ofta modellen `BASE` (Basically Available, Soft state, Eventual consistency), vilket innebär att data kan vara tillfälligt inkonsistent men blir korrekt över tid.

Det gör `SQL` bättre lämpat för till exempel banktransaktioner, medan `NoSQL` passar bättre i system där snabbhet är viktigare än strikt datakonsistens.

## Jämförelsetabell
|   Aspekt  |  SQL  |  NoSQL  |
| --------- | ----- | ------- |
| Struktur   | Fast schema  | Flexibelt schema |
| Skalbarhet  | Vertikalt (Starkare servrar)  | Horisontell (Fler servrar)|
| Prestanda   | Stabil men lite långsammare vid skalning | Snabb vid läsning/skrivning |
| Frågor | SQL-frågor (JOIN, WHERE) | API-baserade |
| Lämpligt för | Finansiella system, lager | Sociala medier, loggar |

## Fördelar och nackdelar
### SQL
SQL – Fördelar:
```
• Strukturerad datahantering: passar bra när datan har tydliga relationer.
• ACID-egenskaper: hög datasäkerhet och integritet vid transaktioner.
• Standardiserat språk (SQL): lätt att lära sig, mycket dokumentation.
• Moget ekosystem: många verktyg, stöd i molnplattformar och stort community.
```
SQL – Nackdelar:
```
• Begränsad skalbarhet: svårare att distribuera över flera servrar.
• Mind­re flexibel struktur: kräver schemaändringar vid ny data.
• Kan bli komplext: många JOINs kan göra frågor långsamma i stora system.
```
### NoSQL
NoSQL – Fördelar:
```
•	Flexibelt schema: du kan lägga till nya fält utan att ändra hela strukturen.
•	Hög prestanda och skalbarhet: särskilt vid stora mängder ostrukturerad data.
•	Bra för moderna appar: till exempel realtidsdata, loggar, användarprofiler.
•	Horisontell skalning: enkelt att lägga till fler servrar vid behov.
```
NoSQL – Nackdelar:
```
• Saknar standard: varje NoSQL-databas fungerar olika.
• Svagare datakonsistens: vissa databaser offrar precision för hastighet.
```

Sammanfattningsvis erbjuder SQL robust struktur och tillförlitlighet, medan NoSQL ger flexibilitet och prestanda för snabbrörliga applikationer.

## Slutsats
Valet mellan `SQL` och `NoSQL` handlar därför inte om vilket som är bäst, utan vilket som är mest lämpligt för systemets behov, utvecklingstakt och datamängd. I takt med att molnplattformar växer blir det allt vanligare att system använder båda teknikerna parallellt för att uppnå bästa balans mellan prestanda, skalbarhet och dataintegritet.

## Källor
https://www.mongodb.com/nosql-explained

https://dev.mysql.com/doc/

https://learn.microsoft.com/en-us/azure/architecture/data-guide/big-data/non-relational-data

https://aws.amazon.com/nosql/

Av Bobo Backman – November 2025
