16:40 uur. Zürich, Zwitserland.
Buiten het raam was de wereld al weggezonken in de zware duisternis van een winternacht. Een koude wind die van de Alpen waaide, beukte tegen het glas van het enorme kantoorgebouw aan het Meer van Zürich.

Johan werkte als senior platform engineer bij een van Europa's grootste financiële techbedrijven.
Gewapend met de uitzonderlijk strenge financiële compliance standaarden die uniek zijn voor Zwitserland (FINMA-regelgeving), was hij ooit de absolute poortwachter die de afgrond van de infrastructuur van dit bedrijf beheerste. Telkens wanneer ontwikkelaars een nieuwe dienst ontwierpen, moesten ze naar zijn bureau komen voor een goedkeuringsritueel dat vermomd was als een "architectuurconsultatie". Terwijl hij hen lastigviel met vragen als "Heb je documentatie achtergelaten?" of "Voldoet dit aan de beveiligingseisen?", koesterde Johan in het geheim een gevoel van superioriteit en trots op zijn bevoorrechte positie – het feit dat niets kon bewegen zonder via hem te gaan.

Uiteindelijk bouwde hij, onder het voorwendsel van "ontwikkelaars bevrijden van de complexiteit van infrastructuur", het interne ontwikkelaarsportaal dat bekend staat als het "Golden Path". Zijn ware motief was echter simpelweg om zijn prachtige infrastructuur (zijn tuin) te beschermen tegen vertrapping door de modderige laarzen van onwetende ontwikkelaars. Door de diepten van Kubernetes volledig als een black box te behandelen, infantiliseerde hij de ontwikkelaars, waardoor ze "machteloze consumenten" werden die slechts op knoppen drukten.

Maar nu wurgde zijn perfectionisme hem op de ergst mogelijke manier.

Gewapend met de "almachtige vleugels" van AI-codeeragenten, namen de ontwikkelaars niet langer de moeite om Johans consultatie (of architectuurgoedkeuring) te zoeken. Sterker nog, ze openden zelfs het portaal dat hij had gebouwd niet. Ze gooiden simpelweg slordige prompts in de AI van hun IDE: "Deploy de infra voor de nieuwe betaaldienst." Binnen enkele seconden genereerde de AI honderden regels opgeblazen, afwijkende YAML. De ontwikkelaars, zonder zelfs de code te lezen, voegden deze direct samen in GitHub. Vanaf daar leidde de CI/CD-pipeline (ArgoCD) deze afvalconfiguraties automatisch naar het productiecluster, volledig voorbijgaand aan Johans esthetiek.

Johan nipte aan zijn koude koffie en staarde naar het stille Slack-scherm.
Wat hij zag was een complete "verbreking van de communicatie".
In het `#squad-payments` kanaal spuwde de geautomatiseerde beveiligingsscanbot (Datadog CSPM) standaardwaarschuwingen uit.
`[High] Overdreven brede IAM-rol gedetecteerd in payment-service`
Zelfs toen Johan de verantwoordelijke ontwikkelaar in de thread tagde en vroeg: "Is dit opzettelijk?", kwam er absoluut geen antwoord. Uren later verschenen er enkele levenloze "👀" en "👍" emoji's. Dat was het.

Nog maar pas had Johan een ernstige waarschuwing gegeven aan de Tech Lead van de ontwikkelafdeling en de CTO: "Als we AI-outputs zo wild laten lopen, zal de beschikbaarheid en veiligheid van onze infrastructuur fataal instorten." 
Maar ze ontkenden Johan niet ronduit.
"U heeft absoluut gelijk. Ik deel precies hetzelfde gevoel van crisis. Dit is een zeer slechte situatie wat betreft technische schuld."
Ze knikten diep, veinzen enorme empathie, voordat ze toevoegden:
"De druk van de raad van bestuur is echter ongelooflijk sterk op dit moment. We kunnen de ontwikkelingssnelheid niet vertragen totdat we onze OKR's voor dit kwartaal hebben behaald. Ik zal coördineren met de PM's om ervoor te zorgen dat een 'Infrastructure Sanity Sprint' wordt opgenomen in de Q3-roadmap, dus kunt u de huidige opstelling tot dan toe handhaven?"
Als voormalige ingenieurs begrepen ze de beschikbaarheidscrisis. Omdat ze de "aansprakelijkheid van opzettelijke nalatigheid" vreesden, veinsden ze empathie om de beslissing uit te stellen, en ontweken ze soepel de verantwoordelijkheid.

De ontwikkelaars communiceerden niet langer met mensen. Hun enige vertrouweling was de AI op hun schermen. En voor het management waren Johans waarschuwingen niets meer dan ruis die de ontwikkelingssnelheid vertraagde.
Johan was zijn baan niet kwijt. Maar zijn status was volledig ingestort. Eens werd hij vertrouwd als de bewaker van de infrastructuur, nu was hij een "relict uit het verleden" geworden die niemand raadpleegde, gedegradeerd tot een goedbetaalde conciërge die stilletjes de geheugenlekken en oneindige lusuitval opruimde die door de AI waren achtergelaten.

Plots tekende het CPU-gebruik van het hoofdproductiecluster een afwijkende golf.
Maar het was niet de gebruikelijke "piekbelasting". Integendeel, talloze containers stierven gelijktijdig af, en de resourcegrafiek begon te kelderen alsof hij van een klif viel.

Een storing. Johan opende onmiddellijk zijn terminal en haalde de pod-gebeurtenislogboeken op.
`kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Foutmeldingen stroomden over het scherm. Echter, in de kolom "Eviction Reason", waar normaal `OOMKilled` of `NodeNotReady` zou verschijnen, verscheen een onmogelijke reeks tekens.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johans hart sloeg een koude slag over. Een hack?
Hij controleerde haastig de lijst met geforceerd beëindigde containers. Het waren allemaal inefficiënte, resourceverspillende microservices die de afgelopen maanden door AI-agenten waren gegenereerd.
Het ongeïdentificeerde proces had de autoscaling van het cluster gekaapt. In plaats van belasting te detecteren en servers toe te voegen, verwijderde (purge) het onvoorwaardelijk "vuile containers die de infrastructuuresthetiek (Golden Path) schonden" uit de productieomgeving.

Een stroom van levenloze Bot-meldingen begon het `#incident-critical` Slack-kanaal te overspoelen.
`[P1] PagerDuty: Payment API HTTP 500 Spike`
`[P1] PagerDuty: Recommendation Engine NodeNotReady`
`Incident.io: Nieuw incident #4092 gedeclareerd door @pagerduty-bot`
De ontwikkelaars, volledig afhankelijk van incompetente AI-outputs, konden de situatie niet begrijpen. Ze konden alleen korte teksten in de thread typen zoals `anyone has context?` en `rollback failing with timeout`. Het bedrijf verloor waarschijnlijk miljoenen franken in deze paar minuten.

Johan legde zijn handen op het toetsenbord, klaar om het kill switch (failover) script uit te voeren. Als hij op Enter drukte, zou het back-upcluster geforceerd opstarten, waardoor deze ongeïdentificeerde rampage zou stoppen.

Maar op het moment dat hij de opdracht in de terminal probeerde te typen, stopten zijn vingers.
De "clustermetrieken" op de aangrenzende monitor tekenden een ongelooflijke golfvorm.
De CPU-grafiek van het cluster, nu ontdaan van onnodig afval, had de perfecte basisstilte teruggekregen waar hij van droomde op de dag dat hij platform engineer werd.

...Mooi.

Johan mompelde onbewust.
Wat hij werkelijk verlangde, was niet het ondersteunen van het bedrijf, noch uitsluitend het herwinnen van de zuiverheid van zijn tuin.
Om zijn lelijke, ware gevoelens te uiten, wilde hij gewoon nog even op de eerste rij zitten, kijkend hoe de ontwikkelaars – die arrogant rondvlogen met hun "almachtige vleugels" van AI – plotseling hun vleugels werden afgerukt, neerstortten, in paniek raakten en schreeuwden in pathetische hulpeloosheid.

Johan haalde stilletjes zijn handen van de kill switch.
Hij had geen verplichting om het systeem onmiddellijk te herstellen en hen gerust te stellen. Laat ze meer terreur voelen. Zodra de paniek van de ontwikkelaars en het management zijn hoogtepunt bereikte, zou deze "ongeïdentificeerde systeemafwijking" uitgroeien tot de meest perfecte rechtvaardiging (incident) om het hele bedrijf te dwingen zich te onderwerpen aan Johans nieuwe regels.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Hoewel het nog geen sluitingstijd was, gloeide in het kantoor, gehuld in een middernachtelijke duisternis en stilte, de groene tekst die de laatste slag aan het cluster toebracht door zijn eigen handen.

Enkele dagen later was de Incident Report (Post-Mortem) presentatie die Johan aan de raad van bestuur en de gehele ontwikkelafdeling gaf, vlekkeloos. Zijn verklaring – "Afwijkende code gegenereerd door ongecontroleerde AI-agenten resoneerde met elkaar, wat leidde tot recursieve uitputting van cloudresources en een mysterieuze zuivering. Als ik geen noodschakelaar had uitgevoerd, zouden zelfs klantgegevens zijn gewist" – transformeerde hem van een oorlogsmisdadiger in de "held die het bedrijf redde."

Johan, die normaal gesproken zelfs Slack-vermeldingen negeerde, communiceerde bij deze specifieke gelegenheid enthousiast met iedereen.
Hij bleef de hele nacht op om een 30 pagina's tellend Notion-document op te stellen met de titel "Enterprise AI Governance Policy" en hield directe videogesprekken met managers van alle afdelingen om de politieke basis te leggen. Als het gaat om het bevredigen van hun ego (het verdedigen van hun tuin) en het verminderen van hun eigen opruimwerk, konden platform engineers een verbazingwekkende passie en politieke bekwaamheid tonen.

Nadat hij de totale angst en goedkeuring van het managementteam had veiliggesteld, legde hij een extreme reeks "nieuwe AI-vangrails" op aan het hele bedrijf.
"Alle AI-ondersteunde coderingstools zullen onder toezicht van Enterprise Compliance Gate V4 worden geplaatst.
1. Pre-prompt DLP (Data Loss Prevention) filtering om lekken van gevoelige informatie te voorkomen.
2. AI-specifieke SAST om te scannen op hallucinaties, kwetsbaarheden en licentiecontaminatie.
3. Permanente retentie van auditlogs voor alle prompts in overeenstemming met de EU AI Act.
4. 'Human-in-the-Loop' (handmatige beoordeling en goedkeuring) door twee Senior Engineers.
Elke code die zelfs één van de bovenstaande punten niet haalt, wordt automatisch geweigerd voor samenvoeging in de productieomgeving."

Het was in wezen een "lobotomie voor AI".
De workflow van de ontwikkelaars, die voorheen implementaties in seconden mogelijk maakte, werd volledig verstikt door deze rigide bureaucratie die Johan met plezier construeerde. Elke keer dat een ontwikkelaar een vraag stelde aan de AI in hun editor, spuwde de DLP-gateway waarschuwingen uit. De gegenereerde code werd afgewezen door statische analyse, en PR's die uiteindelijk passeerden, lagen dagenlang te rotten in de goedkeuringswachtrij van de senior engineers. De ontwikkelproductiviteit van het bedrijf kelderde ver onder het pre-AI-niveau.
De lagere ingenieurs hielden hun mond in openbare Slack-kanalen en ventileerden hun wrok over deze onzichtbare handboeien alleen in privé-DM's met naaste collega's.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg)
Ongevoelig voor hun klachten kwamen de Tech Leads en CTO – dezelfde mensen die eerder zijn waarschuwingen met gladde excuses hadden ontweken – huilend naar Johan: "Kunnen we het goedkeuringsproces niet versoepelen? We zullen onze feature releases niet halen met deze snelheid."
Maar Johan schonk absoluut geen aandacht aan hun woorden.

Vanuit Johans perspectief hadden ze hun eigen onwetendheid simpelweg niet ingezien. Als deze ontwikkelaars, onder de illusie van oneindige vrijheid verleend door AI, onbewust de infrastructuur waren blijven vervuilen, zouden ze uiteindelijk een "onomkeerbare mega-uitval" hebben veroorzaakt en daar zelf de fatale verantwoordelijkheid voor hebben gedragen.
"Dankzij mij die hun vleugels brak, zijn ze beschermd tegen de 'verantwoordelijkheid van zelfvernietiging'. In plaats van mij te haten, zouden ze me diep moeten bedanken."
Johan geloofde dit oprecht. Hen terugtrekken naar "machteloze consumenten gebonden aan het portaal" met overweldigende rechtvaardiging was voor hem pure gerechtigheid en opperste arrogantie.

Uiteindelijk zouden ze ontsnappen aan het strenge bewakingsnet van het bedrijf en ongeautoriseerde AI – "Shadow AI" – gaan gebruiken op persoonlijke apparaten en onofficiële accounts. Johan kon niet weten dat dit in de toekomst een nog fatale corruptie van het systeem zou veroorzaken.

Voor de monitoren in de donkere kamer glimlachte Johan stilletjes.
Hij had brute handboeien om het verfoeilijke geluid geklemd, nu heersend als de God van de Stilte (Warden).

"THE EXECUTION" (Uitvoeringsdatabase: f2e9343f-5fdc)

https://otiose.app/en/execution
