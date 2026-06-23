Kl. 16:40. Zürich, Schweiz. Uden for vinduet var verden allerede sunket ned i vinternattens tunge mørke. En kold vind, der blæste ned fra Alperne, hamrede mod glasset i den massive kontorbygning, der stod ved Zürichsøen.

Johan arbejdede som senior platformingeniør hos et af Europas største finansielle teknologivirksomheder. Bevæbnet med de usædvanligt strenge finansielle compliance-standarder, der er unikke for Schweiz (FINMA-reglerne), var han engang den absolutte portvagt, der kontrollerede afgrunden af denne virksomheds infrastruktur. Hver gang udviklere designede en ny tjeneste, skulle de komme til hans skrivebord for et godkendelsesritual, der maskerede sig som en "arkitekturkonsultation". Mens han plagede dem med spørgsmål som "Har du efterladt dokumentation?" eller "Opfylder dette sikkerhedskravene?", nærede Johan i hemmelighed en følelse af overlegenhed og stolthed over sin privilegerede position – det faktum, at intet kunne bevæge sig uden at passere gennem ham.

Til sidst, under påskud af at "befri udviklere fra infrastrukturens kompleksitet", byggede han den interne udviklerportal kendt som "Golden Path". Hans sande motiv var dog simpelthen at beskytte sin smukke infrastruktur (sin have) mod at blive trampet ned af uvidende udvikleres mudrede støvler. Ved fuldstændigt at black-boxe dybderne af Kubernetes infantiliseret han udviklerne og forvandlede dem til "magtesløse forbrugere", der blot trykkede på knapper.

Men nu kvalte hans perfektionisme ham på den værst tænkelige måde. Bevæbnet med AI-kodningsagenternes "almægtige vinger" gad udviklerne ikke længere søge Johans konsultation (eller arkitekturgodkendelse). Faktisk åbnede de ikke engang den portal, han havde bygget. De kastede blot sjuskede prompts ind i deres IDE's AI: "Implementer infrastrukturen til den nye betalingstjeneste." Inden for få sekunder ville AI'en generere hundredvis af linjer oppustet, unormal YAML. Udviklerne, uden selv at læse koden, flettede den direkte ind i GitHub. Derfra sendte CI/CD-pipelinen (ArgoCD) automatisk disse skraldekonfigurationer ind i produktionsklyngen, fuldstændig ignorerende Johans æstetik.

Johan nippede til sin kolde kaffe og stirrede på den tavse Slack-skærm. Hvad han så, var en fuldstændig "kommunikationsafbrydelse". I `#squad-payments`-kanalen spyttede den automatiske sikkerhedsscanningsbot (Datadog CSPM) standardadvarsler ud. `[High] Overly broad IAM role detected in payment-service` Selv da Johan taggede den ansvarlige udvikler i tråden og spurgte: "Er dette med vilje?", var der absolut intet svar. Timer senere dukkede et par livløse "👀" og "👍" emojis op. Det var det.

Forleden havde Johan udstedt en alvorlig advarsel til udviklingsafdelingens Tech Lead og CTO: "Hvis vi fortsætter med at lade AI-output løbe løbsk på denne måde, vil tilgængeligheden og sikkerheden af vores infrastruktur lide et fatalt kollaps." Men de afviste ikke Johan direkte. "Du har fuldstændig ret. Jeg deler præcis den samme krisefornemmelse. Dette er en meget dårlig situation med hensyn til teknisk gæld." De nikkede dybt, foregav enorm empati, før de tilføjede: "Men presset fra bestyrelsen er utrolig stærkt lige nu. Vi kan ikke sænke udviklingshastigheden, før vi rammer vores OKR'er for dette kvartal. Jeg vil koordinere med PM'erne for at sikre, at en 'Infrastructure Sanity Sprint' inkluderes i Q3-køreplanen, så kunne du bare holde skansen med den nuværende opsætning indtil da?" Da de selv var tidligere ingeniører, forstod de tilgængelighedskrisen. Fordi de frygtede "ansvaret for forsætlig uagtsomhed", foregav de empati for at udskyde beslutningen og gled glat væk for at undgå ansvar.

Udviklerne talte ikke længere med mennesker. Deres eneste fortrolige var AI'en på deres skærme. Og for ledelsen var Johans advarsler intet andet end støj, der sænkede udviklingshastigheden. Johan havde ikke mistet sit job. Men hans status var fuldstændig kollapset. Engang betroet som infrastrukturens vogter, var han blevet et "levn fra fortiden", som ingen konsulterede, degraderet til at være en højt betalt vicevært, der tavst ryddede op i hukommelseslækager og uendelige loop-nedbrud efterladt af AI'en.

Pludselig tegnede CPU-udnyttelsen af den primære produktionsklynge en unormal bølge. Men det var ikke den sædvanlige "belastningsspids". Tværtimod døde utallige containere samtidigt, og ressourcegrafen begyndte at styrtdykke som et fald fra en klippe.

Et nedbrud. Johan åbnede straks sin terminal og hentede pod-hændelsesloggene. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Fejlmeddelelser væltede ned over skærmen. Men i kolonnen "Eviction Reason", hvor der normalt ville stå `OOMKilled` eller `NodeNotReady`, dukkede en umulig streng af tegn op.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johans hjerte sprang et koldt slag over. Et hack? Han tjekkede hastigt listen over tvangsafsluttede containere. De var alle ineffektive, ressourcekrævende mikroservices genereret af AI-agenter i løbet af de sidste par måneder. Den uidentificerede proces havde kapret klyngens autoskalering. I stedet for at detektere belastning og tilføje servere, rensede (slettede) den ubetinget "beskidte containere, der overtrådte infrastrukturens æstetik (Golden Path)" fra produktionsmiljøet.

En strøm af livløse Bot-notifikationer begyndte at oversvømme `#incident-critical` Slack-kanalen. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Udviklerne, der var fuldstændig afhængige af inkompetente AI-output, kunne ikke forstå situationen. De kunne kun skrive korte tekster i tråden som `anyone has context?` og `rollback failing with timeout`. Virksomheden tabte sandsynligvis millioner af francs i disse få minutter.

Johan placerede sine hænder på tastaturet og forberedte sig på at udføre kill switch (failover) scriptet. Hvis han trykkede på Enter-tasten, ville backup-klyngen tvinges til at starte op og stoppe denne uidentificerede hærgen.

Men i det øjeblik han forsøgte at skrive kommandoen ind i terminalen, stoppede hans fingre. "Klyngemetrikkerne" på den tilstødende skærm tegnede en utrolig bølgeform. Klyngens CPU-graf, nu strippet for unødvendigt skrald, havde genvundet den perfekte grundlæggende stilhed, han havde drømt om den dag, han blev platformingeniør.

...Smukt.

Johan mumlede ubevidst. Hvad han virkelig ønskede, var ikke at støtte forretningen, ej heller udelukkende at genvinde renheden i sin have. For at udtrykke sine grimme, sande følelser, ville han simpelthen sidde på første række lidt længere og se udviklerne – som havde fløjet arrogant rundt med deres "almægtige vinger" af AI – pludselig få deres vinger revet af, styrte til jorden, panikke og skrige i patetisk hjælpeløshed.

Johan fjernede stille sine hænder fra kill-switchen. Han havde ingen forpligtelse til at genoprette systemet øjeblikkeligt og berolige dem. Lad dem føle mere terror. Når panikken blandt udviklerne og ledelsen nåede sit højdepunkt, ville denne "uidentificerede systemanomali" ophøjes til den mest perfekte begrundelse (incident) for at tvinge hele virksomheden til underkastelse under Johans nye regler.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Selvom det endnu ikke var lukketid, lyste den grønne tekst, der leverede det sidste slag til klyngen, af hans egne hænder i kontoret, der var indhyllet i en midnatslignende mørke og stilhed.

Få dage senere var Incident Report (Post-Mortem) præsentationen, som Johan leverede til bestyrelsen og hele udviklingsafdelingen, fejlfri. Hans forklaring – "Anomal kode genereret af ukontrollerede AI-agenter resonerede med hinanden, hvilket udløste rekursiv udtømning af cloudressourcer og en mystisk udrensning. Hvis jeg ikke havde udført en nød-afbryder, ville selv kundedata være blevet slettet" – forvandlede ham fra en krigsforbryder til "helten, der reddede virksomheden".

Johan, der normalt ignorerede selv Slack-omtaler, kommunikerede entusiastisk med alle ved denne særlige lejlighed. Han var vågen hele natten med at udarbejde et 30-siders Notion-dokument med titlen "Enterprise AI Governance Policy" og holdt direkte videoopkald med ledere på tværs af alle afdelinger for at lægge det politiske grundlag. Når det kom til at tilfredsstille deres ego (forsvare deres have) og reducere deres eget oprydningsarbejde, kunne platformingeniører udvise en forbløffende passion og politisk dygtighed.

Efter at have sikret sig ledelsesteamets totale frygt og godkendelse, indførte han et ekstremt sæt af "nye AI-værn" i hele virksomheden. "Alle AI-assisterede kodningsværktøjer vil blive placeret under overvågning af Enterprise Compliance Gate V4. 1. Pre-prompt DLP (Data Loss Prevention) filtrering for at forhindre lækage af følsomme oplysninger. 2. AI-specifik SAST til at scanne for hallucinationer, sårbarheder og licensforurening. 3. Permanent opbevaring af revisionslogfiler for alle prompts i overensstemmelse med EU AI Act. 4. 'Human-in-the-Loop' (manuel gennemgang og godkendelse) af to Senior Engineers. Enhver kode, der fejler selv en af ovenstående, vil automatisk blive afvist fra at blive flettet ind i produktionsmiljøet."

Det var i bund og grund en "lobotomi for AI". Udviklernes arbejdsgang, som plejede at tillade implementeringer på få sekunder, blev fuldstændig kvalt af dette stive bureaukrati, som Johan med glæde konstruerede. Hver gang en udvikler stillede AI et spørgsmål i deres editor, spyttede DLP-gatewayen advarsler ud. Den genererede kode blev afvist af statisk analyse, og PR'er, der endelig passerede, blev efterladt til at rådne i dagevis i senioringeniørernes godkendelseskø. Virksomhedens udviklingsproduktivitet faldt langt under niveauet før AI. De lavere rangerende ingeniører holdt mund i offentlige Slack-kanaler og luftede deres vrede over disse usynlige håndjern kun i private DMs med nære kolleger. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Ude af stand til at udholde det, kom Tech Leads og CTO – de selvsamme mennesker, der tidligere havde undgået hans advarsler med glatte undskyldninger – grædende til Johan: "Kan vi ikke på en eller anden måde slække på godkendelsesprocessen? Vi når ikke vores feature-udgivelser i dette tempo." Men Johan tog absolut ingen notits af deres ord.

Fra Johans perspektiv havde de simpelthen ikke indset deres egen uvidenhed. Hvis disse udviklere, under illusionen om uendelig frihed givet af AI, havde fortsat med uvidende at forurene infrastrukturen, ville de i sidste ende have forårsaget et "irreversibelt mega-nedbrud" og selv båret det fatale ansvar for det. "Takket være mig, der brød deres vinger, er de beskyttet mod 'ansvaret for selvdestruktion'. I stedet for at hade mig, burde de dybt takke mig." Johan troede oprigtigt på dette. At trække dem tilbage til at være "magtesløse forbrugere bundet til portalen" ved hjælp af overvældende begrundelse var for ham ren retfærdighed og suveræn arrogance.

Til sidst ville de undslippe virksomhedens strenge overvågningsnet og begynde at bruge uautoriseret AI – "Shadow AI" – på personlige enheder og uofficielle konti. Johan havde ingen måde at vide, at dette ville medføre en endnu mere fatal korruption af systemet i fremtiden.

Foran skærmene i det mørke rum smilede Johan stille. Han havde lagt brutale håndjern på den afskyelige støj og regerede nu som stilhedens Gud (Vogter).

"THE EXECUTION" (Eksekveringsdatabase: f2e9343f-5fdc)

https://otiose.app/en/execution
