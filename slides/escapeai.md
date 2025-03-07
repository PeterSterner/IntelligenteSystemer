# Introduktion til brugen af AI og maskinelæring i escape rooms vha. Unity

## Af Henrik Sterner


## Målet med dette oplæg
- Introducere ganske kort forskellige former for AI i spil
- Give en introduktion til konkret brug af AI og maskinelæring i escape rooms vha. Unity
- 

## Forskellige former for AI
Vi kan i grove træk dele AI i spil op i følgende kategorier:
- **Reaktive** - AI der reagerer på spillerens handlinger
- **Planlæggende** - AI der planlægger sine handlinger
- **Lærende** - AI der lærer af sine handlinger
- **Adaptiv** - AI der tilpasser sig spillerens handlinger
- **Kreativ** - AI der skaber nye ting

Disse kategorier er ikke gensidigt udelukkende, og en AI kan sagtens være en blanding af flere kategorier. 

## Særligt for AI i spil og escape rooms
Vi kan kombinere klassisk maskinelæring med mere simple former for AI, for at skabe mere komplekse AI'er.

AI i spil og escape rooms er ofte mere simple end AI i andre sammenhænge, da de ikke behøver at være så realistiske. Det er vigtigere at de er sjove og underholdende, men ikke altid at de er realistiske og korrekte.


## Hvad er reaktive AI
Reaktive AI er AI der reagerer på spillerens handlinger. Det kan være alt fra at en fjende skyder på spilleren, til at en NPC siger noget til spilleren.

Eksempler på en reaktiv AI i et escape room kan være:
- En NPC der siger noget til spilleren
- En NPC der giver spilleren et item
- En NPC der åbner en dør for spilleren
- En NPC der giver spilleren et hint
- En NPC der giver spilleren en opgave
- En NPC der giver spilleren en gåde

## Hvordan kan reaktiv AI implementeres
Reaktiv AI kan implementeres på mange forskellige måder. Typisk bruges der en form for state machine (eller tilstandsmaskine), der kan skifte mellem forskellige tilstande.

En state machine er en maskine der kan være i forskellige tilstande. Den kan skifte tilstand, og den kan have forskellige handlinger i de forskellige tilstande.

## Statemachines i Unity
Lad os se på et eksempel på en state machine i Unity. Vi har en NPC der skal sige noget til spilleren, og derefter give spilleren et item:
    
```csharp
    public class NPC : MonoBehaviour
    {
        public string text;
        public Item item;

        private bool hasTalked = false;

        void Update()
        {
            if (Input.GetKeyDown(KeyCode.Space))
            {
                if (!hasTalked)
                {
                    Debug.Log(text);
                    hasTalked = true;
                }
                else
                {
                    Inventory.instance.AddItem(item);
                }
            }
        }
    }
```

## Mere generelt om konstruktion af tilstandsmaskiner
Start med at:
- Definer tilstandene i din tilstandsmaskine. Dette kunne være ting som spillerens nuværende placering, de genstande, de har samlet, eller de handlinger, de har udført.
- Definer overgangene mellem tilstandene. Dette er, hvordan spilleren vil bevæge sig fra én tilstand til en anden. For eksempel kan spilleren overgå fra den "idle"-tilstand til den "walking"-tilstand, når de trykker på "W"-tasten.
- Implementer tilstandsmaskinen i din kode. Du kan gøre dette ved at oprette en klasse, der repræsenterer din tilstandsmaskine. Klassen skal have metoder til at få og ændre den aktuelle tilstand, samt metoder til at håndtere overgange mellem tilstande.

## Objekt-orienteret eksempel
Vi kan oprette en klasse der repræsenterer vores state machine. Denne klasse skal have metoder til at få og ændre den aktuelle tilstand, samt metoder til at håndtere overgange mellem tilstande. Her en tilstandsmaskine med tilstandene idle, haskey og solved:

```csharp
public class EscapeRoomStateMachine
{
    private string _currentState = "idle";

    public string GetCurrentState()
    {
        return _currentState;
    }

    public void TransitionToState(string newState)
    {
        _currentState = newState;
    }

    public void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.tag == "Key")
        {
            TransitionToState("hasKey");
        }
    }

    public void OnButtonPressed()
    {
        TransitionToState("solved");
    }
}

```

## Statemachines i Unity ved brug af ML-Agents
Vi kan også bruge ML-Agents til at lave en state machine. 
ML-agent implementerer en state machine, der kan bruges til at styre en agent. Denne state machine har en række foruddefinerede tilstande, som agenten kan være i:
- **Idle** - Agenten står stille
- **Heal** - Agenten heler sig selv
- **Eat** - Agenten spiser
- **Drink** - Agenten drikker
- **Attack** - Agenten angriber

Herunder kode til at skifte tilstanden til "Attack":

```csharp
public class Attack : Agent
{
    public override void AgentAction(float[] vectorAction, string textAction)
    {
        if (textAction == "Attack")
        {
            SetReward(1.0f);
            Done();
        }
    }
}
```

## Hvad er planlæggende AI
Planlæggende AI er AI der planlægger sine handlinger. Det kan være alt fra at en fjende skyder på spilleren, til at en NPC siger noget til spilleren.

Hvordan adskiller planlæggende AI sig fra reaktiv AI? Planlæggende AI er mere kompleks, og kan tage flere forskellige beslutninger. Reaktiv AI er mere simpel, og kan kun reagere på spillerens handlinger.

## Eksempler på brugen af planlæggende AI i escape rooms og spil
Det kunne være: 
- En NPC der bevæger sig rundt i rummet
- En NPC der bevæger sig rundt i rummet, og skyder på spilleren
- Et objekt der bevæger sig rundt i rummet uden at ramme spilleren
- En vagt der bevæger sig rundt i rummet, og skyder på spilleren hvis de kommer for tæt på
- En flok fugle der flyver rundt i rummet som en sværm
- En flok zombier der jager en
- En NPC der følger efter spilleren

## Hvordan kan planlæggende AI implementeres
Planlæggende AI kan implementeres på mange forskellige måder. Typisk bruges der en form for state machine (eller tilstandsmaskine), der kan skifte mellem forskellige tilstande.

En state machine er en maskine der kan være i forskellige tilstande. Den kan skifte tilstand, og den kan have forskellige handlinger i de forskellige tilstande.

## Sværme eller flokke af objekter (boids)
Vi kan implementere sværm-intelligens ved at bruge boids. Boids er en form for AI, der kan bruges til at simulere sværme eller flokke af objekter. Boids er en forkortelse for "bird-oid object", og er en reference til fugle, der flyver i flok.

## Regler for boids:
Vi opnår rigtig fin effekt ved at få vores boids til at følge nogle simple regler:
- Følg: Boids følger hinandens baner.
- Undgå: Boids undgår at kollidere med hinanden eller med forhindringer.
- Separations: Boids holder en vis afstand mellem hinanden.
- Vandretning: Boids orienterer sig mod flokkens centrering.

## Implementering af boids i Unity
Vi kan relativt let implementere boids i både 2d og 3d ved objekt-orienteret programmering. Se evt. mine slides.

Vi kan implementere boids i Unity ved at bruge ML-Agents. ML-Agents har en indbygget boid-agent, der kan bruges til at simulere sværme eller flokke af objekter:
[https://zombie-einstein.github.io/2020/09/26/rl_flock.html](https://zombie-einstein.github.io/2020/09/26/rl_flock.html)

Se mere i de næste slides.

## Hvad er ML-Agents?
ML-Agents er en open-source Unity-plugin, der kan bruges til at lave AI i Unity. ML-Agents er udviklet af Unity Technologies, og er en del af Unitys Machine Learning-platform.

ML-Agents er en forkortelse for "Machine Learning Agents". ML-Agents kan bruges til at lave AI-agenter, der kan lære at udføre forskellige opgaver.

## Hvordan kommer jeg i gang med ML-Agents?
Kort fortalt:
- Start et nyt Unity-projekt
- Importer ML-Agents-pakken ved at gå til Window -> Package Manager -> ML-Agents -> Install
- Importer ML-Agents eksemplerne ved at gå til Window -> ML-Agents -> Import All Samples
- Åben et af eksemplerne ved at gå til Window -> ML-Agents -> RollerBall
- Kør eksemplet ved at trykke på play-knappen

## Hvad er lærende AI
Lærende AI er AI der lærer af sine handlinger. Det kan være alt fra at en fjende skyder på spilleren, til at en NPC siger noget til spilleren.

Hvordan adskiller lærende AI sig fra planlæggende AI? Lærende AI er mere kompleks, og kan tage flere forskellige beslutninger. Planlæggende AI er mere simpel, og kan kun reagere på spillerens handlinger.

## Hvordan kunne lærende AI bruges i escape rooms og spil
Det kunne være:
- En NPC der lærer at bevæge sig rundt i rummet
- En NPC der bliver bedre til at svarer på spillerens spørgsmål
- En mus som blir bedre til at løse en labyrint
- Genkende mønstre i spillerens adfærd og reagere på dem
- Genkende billeder

## Hvordan kan lærende AI implementeres
Lærende AI kan implementeres ved brug af klassisk maskinelæring, eller ved brug af deep learning.

## Hvad er klassisk maskinelæring
Klassisk maskinelæring er en form for maskinelæring, der bruger en række foruddefinerede algoritmer til at løse et problem. Klassisk maskinelæring bruger ikke deep learning. Eksempler på algoritmer:

- **K-Nearest Neighbors** - En algoritme der bruges til at klassificere data
- **K-Means** - En algoritme der bruges til at gruppere data
- **Decision Trees** - En algoritme der bruges til at klassificere data
- **Random Forest** - En algoritme der bruges til at klassificere data
- **Naive Bayes** - En algoritme der bruges til at klassificere data

Algoritmerne kan også kombineres for at skabe mere komplekse AI'er.
Se [https://henriksterner.github.io/IntelligenteSystemer/](https://henriksterner.github.io/IntelligenteSystemer/) for MEGET mere info om de konkrete algoritmer.

## Inspiration: Anvendelse af K-Nearest Neighbors i escape rooms

K-Nearest Neighbors er en algoritme der bruges til at klassificere data. Den kan bruges til at genkende mønstre i spillerens adfærd, og reagere på dem:

- Samle data om spillerens adfærd og klassificere spillere som "nybegynder", "middel" eller "ekspert"
- Reagere på spillerens adfærd baseret på deres klassifikation
- Reagere på spillerens adfærd baseret på deres klassifikation, og give dem et hint hvis de har brug for det
- Klassificere NPC'er som "ven" eller "fjende" baseret på spillerens adfærd
  
## Inspiration: Anvendelse af K-Means i escape rooms

K-Means er en algoritme der bruges til at gruppere data. Den kan bruges til at genkende mønstre i spillerens adfærd, og reagere på dem:

- Samle data om spillerens adfærd og gruppere spillere i grupper baseret på deres adfærd
- Klassificere spillere efter Bartle's spiller typer
- Reagere på spillerens adfærd baseret på deres gruppe
- Få en NPC til at reagere på spillerens adfærd baseret på deres gruppe
- Segmentere et billede af noget i rummet, og reagere på det

## Hvad er deep learning
Deep learning er en form for maskinelæring, der bruger neurale netværk til at løse et problem. Deep learning bruger ikke klassisk maskinelæring. Eksempler på neurale netværk:








