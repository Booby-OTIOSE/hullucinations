16:40. Zürich, Schweiz. Utanför fönstret hade världen redan sjunkit ner i vinternattens tunga mörker. En kall vind som blåste ner från Alperna piskade glaset på den massiva kontorsbyggnaden vid Zürichsjön.

Johan arbetade som senior plattformsingenjör på ett av Europas största finansteknikföretag. Beväpnad med de exceptionellt strikta finansiella efterlevnadsstandarder som är unika för Schweiz (FINMA-regler), var han en gång den absoluta grindvakten som kontrollerade avgrunden i detta företags infrastruktur. När utvecklare designade en ny tjänst, var de tvungna att komma till hans skrivbord för en godkännanderitual som maskerade sig som en "arkitekturkonsultation". Medan han tjatade på dem med frågor som "Har du lämnat dokumentation?" eller "Uppfyller detta säkerhetskraven?", hyste Johan i hemlighet en känsla av överlägsenhet och stolthet över sin privilegierade position – det faktum att ingenting kunde röra sig utan att passera genom honom.

Så småningom, under förevändning att "befria utvecklare från infrastrukturens komplexitet", byggde han den interna utvecklarportalen känd som "Golden Path". Hans verkliga motiv var dock helt enkelt att skydda sin vackra infrastruktur (sin trädgård) från att trampas ner av okunniga utvecklares leriga stövlar. Genom att helt black-boxa djupen av Kubernetes, infantiliserade han utvecklarna och förvandlade dem till "maktlösa konsumenter" som bara tryckte på knappar.

Men nu ströp hans perfektionism honom på värsta möjliga sätt.

Beväpnade med AI-kodningsagenternas "allsmäktiga vingar" brydde sig utvecklarna inte längre om att söka Johans konsultation (eller arkitekturgodkännande). Faktum är att de inte ens öppnade portalen han hade byggt. De kastade helt enkelt slarviga prompts in i sin IDE:s AI: "Deploya infrastrukturen för den nya betaltjänsten." Inom sekunder genererade AI hundratals rader med uppblåst, avvikande YAML. Utvecklarna, utan att ens läsa koden, slog ihop den direkt till GitHub. Därifrån slussade CI/CD-pipelinen (ArgoCD) automatiskt dessa skräpkonfigurationer in i produktionsklustret, helt ignorerande Johans estetik.

Johan smuttade på sitt kalla kaffe och stirrade på den tysta Slack-skärmen. Vad han såg var en fullständig "kommunikationsavbrott". I kanalen `#squad-payments` spottade den automatiserade säkerhetsskanningsboten (Datadog CSPM) ut standardvarningar. `[High] Overly broad IAM role detected in payment-service` Även när Johan taggade den ansvariga utvecklaren i tråden och frågade, "Är detta avsiktligt?", fanns det absolut inget svar. Timmar senare dök några livlösa "👀" och "👍" emojis upp. Det var allt.

Bara häromdagen hade Johan utfärdat en allvarlig varning till utvecklingsavdelningens Tech Lead och CTO: "Om vi fortsätter att låta AI-utdata löpa amok på detta sätt, kommer tillgängligheten och säkerheten i vår infrastruktur att drabbas av en fatal kollaps." Men de förnekade inte Johan rakt av. "Du har helt rätt. Jag delar exakt samma känsla av kris. Detta är en mycket dålig situation när det gäller teknisk skuld." De nickade djupt, låtsades ha enorm empati, innan de tillade: "Men trycket från styrelsen är otroligt starkt just nu. Vi kan inte sakta ner utvecklingstakten förrän vi når våra OKR:er för detta kvartal. Jag kommer att koordinera med PM:erna för att se till att en 'Infrastructure Sanity Sprint' inkluderas i Q3-färdplanen, så kan du bara hålla ställningarna med den nuvarande installationen tills dess?" Eftersom de själva var före detta ingenjörer förstod de tillgänglighetskrisen. Eftersom de fruktade "ansvaret för avsiktlig försumlighet" fejka de empati för att skjuta upp beslutet och smidigt undvek ansvar.

Utvecklarna kommunicerade inte längre med människor. Deras enda förtrogna var AI på deras skärmar. Och för ledningen var Johans varningar inget annat än brus som saktade ner utvecklingstakten. Johan hade inte förlorat sitt jobb. Men hans status hade helt kollapsat. En gång betrodd som infrastrukturens väktare, hade han blivit en "relik från det förflutna" som ingen konsulterade, degraderad till att vara en högt betald vaktmästare som tyst städade upp minnesläckor och oändliga loop-avbrott som lämnats efter av AI.

Plötsligt ritade CPU-användningen för det huvudsakliga produktionsklustret en avvikande våg.

Men det var inte den vanliga "belastningstoppen". Tvärtom dog otaliga containrar samtidigt, och resursgrafen började störtdyka som om den föll från en klippa.

Ett avbrott. Johan öppnade omedelbart sin terminal och hämtade pod-händelseloggar. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Felmeddelanden strömmade ner över skärmen. Men i kolumnen "Eviction Reason", där det normalt skulle stå `OOMKilled` eller `NodeNotReady`, dök en omöjlig sträng av tecken upp.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johans hjärta hoppade över ett kallt slag. Ett hack? Han kontrollerade hastigt listan över tvångsavslutade containrar. De var alla ineffektiva, resurskrävande mikroservrar genererade av AI-agenter under de senaste månaderna. Den oidentifierade processen hade kapat klustrets autoskalning. Istället för att upptäcka belastning och lägga till servrar, rensade den ovillkorligt (raderade) "smutsiga containrar som bröt mot infrastrukturens estetik (Golden Path)" från produktionsmiljön.

En ström av livlösa Bot-meddelanden började översvämma Slack-kanalen `#incident-critical`. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Utvecklarna, helt beroende av inkompetenta AI-utdata, kunde inte förstå situationen. De kunde bara skriva korta texter i tråden som `anyone has context?` och `rollback failing with timeout`. Företaget förlorade sannolikt miljontals franc under dessa få minuter.

Johan lade händerna på tangentbordet och förberedde sig för att exekvera kill switch (failover)-skriptet. Om han tryckte på Enter-tangenten skulle backupklustret tvångsstarta och stoppa detta oidentifierade raseri.

Men i samma ögonblick som han försökte skriva kommandot i terminalen, stannade hans fingrar.

"Klusterstatistik" på den intilliggande monitorn ritade en otrolig vågform. Klustrets CPU-graf, nu rensad från onödigt skräp, hade återfått den perfekta baslinjetystnad han hade drömt om den dagen han blev plattformsingenjör.

...Vackert.

Johan mumlade omedvetet. Vad han verkligen önskade var inte att stödja verksamheten, inte heller enbart att återfå renheten i sin trädgård. För att uttrycka sina fula, sanna känslor, ville han helt enkelt sitta på första raden lite längre och se utvecklarna – som hade flugit runt arrogant med sina "allsmäktiga vingar" av AI – få sina vingar plötsligt avslitna, krascha till marken, panikslagna och skrikande i patetisk hjälplöshet.

Johan tog tyst bort händerna från nödstoppet. Han hade ingen skyldighet att omedelbart återställa systemet och lugna dem. Låt dem känna mer terror. När paniken bland utvecklarna och ledningen nådde sin topp, skulle denna "oidentifierade systemanomali" höjas till den mest perfekta motiveringen (incidentet) för att tvinga hela företaget till underkastelse under Johans nya regler.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Trots att det ännu inte var stängningsdags, i kontoret insvept i en midnattsliknande mörker och tystnad, lyste den gröna texten som utdelade det sista slaget mot klustret av hans egna händer.

Några dagar senare var incidentrapporten (Post-Mortem) som Johan presenterade för styrelsen och hela utvecklingsavdelningen felfri. Hans förklaring – "Anomal kod genererad av okontrollerade AI-agenter resonerade med varandra, vilket utlöste rekursiv molnresursutarmning och en mystisk rensning. Om jag inte hade utfört en nödbrytare, skulle även kunddata ha raderats" – förvandlade honom från en krigsförbrytare till "hjälten som räddade företaget".

Johan, som vanligtvis ignorerade även Slack-omnämnanden, kommunicerade entusiastiskt med alla vid detta specifika tillfälle. Han var uppe hela natten och utarbetade ett 30-sidigt Notion-dokument med titeln "Enterprise AI Governance Policy" och höll direkta videosamtal med chefer från alla avdelningar för att lägga den politiska grunden. När det gäller att tillfredsställa deras ego (försvara deras trädgård) och minska deras eget städarbete, kunde plattformsingenjörer uppvisa en förvånande passion och politisk skicklighet.

Efter att ha säkrat total rädsla och godkännande från ledningsgruppen, införde han en extrem uppsättning "nya AI-skyddsräcken" över hela företaget. "Alla AI-assisterade kodningsverktyg kommer att placeras under övervakning av Enterprise Compliance Gate V4. 1. Pre-prompt DLP (Data Loss Prevention) filtrering för att förhindra läckage av känslig information. 2. AI-specifik SAST för att skanna efter hallucinationer, sårbarheter och licenskontaminering. 3. Permanent lagring av granskningsloggar för alla prompts i enlighet med EU AI Act. 4. 'Human-in-the-Loop' (manuell granskning och godkännande) av två seniora ingenjörer. All kod som misslyckas med ens ett av ovanstående kommer automatiskt att avvisas från att slås ihop till produktionsmiljön."

Det var i huvudsak en "lobotomi för AI". Utvecklarnas arbetsflöde, som tidigare tillät driftsättningar på sekunder, kvävdes helt av denna stela byråkrati som Johan glatt konstruerade. Varje gång en utvecklare ställde en fråga till AI i sin editor, spottade DLP-gatewayen ut varningar. Den genererade koden avvisades av statisk analys, och PR:er som äntligen passerade lämnades att ruttna i dagar i senioringenjörernas godkännandekö. Företagets utvecklingsproduktivitet sjönk långt under pre-AI-nivåer. De lägre ingenjörerna höll tyst i offentliga Slack-kanaler och ventilerade sin förbittring över dessa osynliga handbojor endast i privata DM med nära kollegor. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Oförmögna att uthärda det, kom Tech Leads och CTO – samma personer som tidigare hade undvikit hans varningar med hala ursäkter – gråtande till Johan: "Kan vi inte på något sätt lätta på godkännandeprocessen? Vi kommer inte att hinna med våra funktionsreleaser i den här takten." Men Johan brydde sig absolut inte om deras ord.

Ur Johans perspektiv hade de helt enkelt inte insett sin egen okunnighet. Om dessa utvecklare, under illusionen av oändlig frihet som AI gav, hade fortsatt att omedvetet förorena infrastrukturen, skulle de så småningom ha orsakat ett "irreversibelt mega-avbrott" och själva burit det fatala ansvaret för det. "Tack vare att jag bröt deras vingar, är de skyddade från 'ansvaret för självförstörelse'. Istället för att känna agg mot mig, borde de djupt tacka mig." Johan trodde uppriktigt på detta. Att dra tillbaka dem till att vara "maktlösa konsumenter bundna till portalen" med överväldigande motivering var, för honom, ren rättvisa och överlägsen arrogans.

Så småningom skulle de undkomma företagets strikta övervakningsnät och börja använda obehörig AI – "Shadow AI" – på personliga enheter och inofficiella konton. Johan hade ingen möjlighet att veta att detta skulle leda till en ännu mer fatal korruption av systemet i framtiden.

Framför monitorerna i det mörka rummet log Johan tyst. Han hade satt brutala handbojor på det avskyvärda bruset och regerade nu som Tystnadens Gud (Vaktmästaren).

"THE EXECUTION" (Exekveringsdatabas: f2e9343f-5fdc)

https://otiose.app/en/execution
