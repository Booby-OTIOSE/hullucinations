16:40. Zürich, Šveits. Akna taga oli maailm juba vajunud talveöö sügavusse pimedusse. Alpide poolt puhuv külm tuul peksis Zürichi järve ääres seisva massiivse büroohoone klaase.

Johan töötas vanemplatvormiinsenerina ühes Euroopa suurimas finantstehnoloogiaettevõttes. Relvastatud Šveitsile ainulaadsete erakordselt rangete finantsnõuete standarditega (FINMA regulatsioonid), oli ta kunagi absoluutne väravavaht, kes kontrollis selle ettevõtte infrastruktuuri sügavust. Iga kord, kui arendajad uue teenuse disainisid, pidid nad tulema tema laua juurde heakskiidurituaalile, mis maskeerus "arhitektuurikonsultatsiooniks". Kui ta neid küsimustega nagu "Kas jätsite dokumentatsiooni?" või "Kas see vastab turvanõuetele?" tüütas, hoidis Johan salaja üleolekutunnet ja uhkust oma privilegeeritud positsiooni üle – fakti üle, et miski ei saanud liikuda ilma temast läbi minemata.

Lõpuks, ettekäändel "arendajate vabastamine infrastruktuuri keerukusest", ehitas ta sisemise arendajaportaali, mida tunti kui "Golden Path". Tema tegelik motiiv oli aga lihtsalt kaitsta oma kaunist infrastruktuuri (oma aeda) selle eest, et teadmatute arendajate porised saapad seda tallaksid. Kubernetes'e sügavuste täieliku musta kasti panemisega infantiliseeris ta arendajaid, muutes nad "võimetuteks tarbijateks", kes lihtsalt nuppe vajutasid.

Kuid nüüd kägistas tema perfektsionism teda kõige hullemas võimalikus mõttes. AI-kodeerimisagentide "kõikvõimsate tiibadega" relvastatud arendajad ei vaevunud enam Johani konsultatsiooni (ega arhitektuuri heakskiitu) otsima. Tegelikult ei avanud nad isegi tema ehitatud portaali. Nad viskasid lihtsalt lohakaid käsklusi oma IDE AI-sse: "Deploy the infra for the new payment service." Mõne sekundi jooksul genereeris AI sadu ridu paisutatud, anomaalset YAML-i. Arendajad, isegi koodi lugemata, ühendasid selle otse GitHubi. Sealt edasi suunas CI/CD torujuhe (ArgoCD) need prügikonfiguratsioonid automaatselt tootmiskogumisse, ignoreerides täielikult Johani esteetikat.

Johan rüüpas oma külma kohvi ja vaatas vaikset Slacki ekraani. See, mida ta nägi, oli täielik "kommunikatsiooni katkemine". Kanali `#squad-payments` automaatne turvaskannimise bot (Datadog CSPM) sülitas välja tüüpilisi hoiatusi. `[High] Overly broad IAM role detected in payment-service` Isegi kui Johan märkis vastutava arendaja teemas ja küsis: "Kas see on disainitud?", ei tulnud absoluutselt mingit vastust. Tunde hiljem ilmusid mõned elutud "👀" ja "👍" emotikonid. See oligi kõik.

Just üleeile oli Johan andnud arendusosakonna Tech Leadile ja CTO-le tõsise hoiatuse: "Kui me jätkame AI väljundite sellist metsikut jooksmist, kannatab meie infrastruktuuri kättesaadavus ja turvalisus fataalse kokkuvarisemise." Kuid nad ei eitanud Johanit otse. "Teil on täiesti õigus. Jagan täpselt sama kriisitunnet. See on tehnilise võla seisukohalt väga halb olukord." Nad noogutasid sügavalt, teeseldes tohutut empaatiat, enne kui lisasid: "Kuid juhatuse surve on praegu uskumatult tugev. Me ei saa arenduskiirust aeglustada enne, kui oleme selle kvartali OKR-id saavutanud. Koordineerin PM-idega, et 'Infrastruktuuri Tervise Sprint' lisataks Q3 teekaardile, nii et kas saaksite praeguse seadistusega seni vastu pidada?" Olles ise endised insenerid, mõistsid nad kättesaadavuse kriisi. Kuna nad kartsid "tahtliku hooletuse vastutust", teeseldes empaatiat, et otsust edasi lükata, libisesid nad sujuvalt vastutusest kõrvale.

Arendajad ei vestelnud enam inimestega. Nende ainus usaldusalune oli ekraanidel olev AI. Ja juhtkonna jaoks olid Johani hoiatused vaid müra, mis aeglustas arenduskiirust. Johan ei olnud oma tööd kaotanud. Kuid tema staatus oli täielikult kokku varisenud. Kunagi infrastruktuuri valvurina usaldatud, oli temast saanud "mineviku reliikvia", kellega keegi ei konsulteerinud, taandatud kõrgelt tasustatud koristajaks, kes vaikselt koristas AI jäetud mälulekkeid ja lõputute tsüklite katkestusi.

Järsku joonistas peamise tootmiskogumi CPU kasutus anomaalse laine. Kuid see ei olnud tavaline "koormuse tipp". Vastupidi, lugematud konteinerid surid samaaegselt ja ressursside graafik hakkas langema nagu kaljult kukkudes.

Katkestus. Johan avas kohe oma terminali ja tõmbas podi sündmuste logid. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Ekraanile voolasid veateated. Kuid veeru "Eviction Reason" asemel, kus tavaliselt kuvataks `OOMKilled` või `NodeNotReady`, ilmus võimatu märgijada.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johani süda jättis külma löögi vahele. Häkk? Ta kontrollis kiiruga sunniviisiliselt lõpetatud konteinerite nimekirja. Need kõik olid ebaefektiivsed, ressursse raiskavad mikroteenused, mille AI agendid olid viimaste kuude jooksul genereerinud. Tundmatu protsess oli kaaperdanud klastri automaatse skaleerimise. Koormuse tuvastamise ja serverite lisamise asemel puhastas (kustutas) see tingimusteta "räpaseid konteinereid, mis rikkusid infrastruktuuri esteetikat (Golden Path)" tootmiskeskkonnast.

Elutute Bot-teavituste tulv hakkas üle ujutama `#incident-critical` Slacki kanalit. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Arendajad, kes olid täielikult sõltuvad ebakompetentsetest AI väljunditest, ei suutnud olukorda mõista. Nad said teemasse kirjutada vaid lühikesi tekste nagu `anyone has context?` ja `rollback failing with timeout`. Ettevõte kaotas tõenäoliselt nende mõne minuti jooksul miljoneid franke.

Johan asetas käed klaviatuurile, valmistudes käivitama tapmislüliti (failover) skripti. Kui ta vajutaks Enter-klahvi, käivituks varuklaster sunniviisiliselt, peatades selle tuvastamatu märatsemise.

Kuid hetkel, mil ta proovis käsku terminali sisestada, peatusid ta sõrmed. Kõrvaloleval monitoril olevad "klastri mõõdikud" joonistasid uskumatu lainekuju. Klastri CPU graafik, nüüd tarbetust prügist puhastatud, oli taastanud täiusliku baasjoone vaikuse, millest ta oli unistanud päeval, mil temast sai platvormiinsener.

...Ilus.

Johan pomises alateadlikult. See, mida ta tõeliselt soovis, ei olnud äri toetamine ega ka ainult oma aia puhtuse taastamine. Et väljendada oma inetuid, tõelisi tundeid, tahtis ta lihtsalt istuda veel veidi esireas, vaadates, kuidas arendajatel – kes olid AI "kõikvõimsate tiibadega" ülbe ringi lennanud – tiivad ootamatult küljest rebitakse, nad maapinnale kukuvad, paanikasse satuvad ja haletsusväärses abitus karjuvad.

Johan eemaldas vaikselt käed tapmislülitilt. Tal ei olnud kohustust süsteemi kohe taastada ja neid rahustada. Las nad tunnevad rohkem hirmu. Kui arendajate ja juhtkonna paanika jõuab haripunkti, tõuseb see "tundmatu süsteemi anomaalia" kõige täiuslikumaks õigustuseks (intsidendiks), et sundida kogu ettevõtet Johani uutele reeglitele alluma.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Kuigi veel ei olnud sulgemisaeg, helendas kesköötaolises pimeduses ja vaikuses mähitud kontoris roheline tekst, mis andis klastrile viimase hoobi, tema enda käte läbi.

Mõni päev hiljem oli Johani esitatud intsidentide aruanne (Post-Mortem) juhatusele ja kogu arendusosakonnale veatu. Tema selgitus – "Kontrollimata AI-agentide genereeritud anomaalne kood resoneerus üksteisega, käivitades pilveressursside rekursiivse ammendumise ja salapärase puhastuse. Kui ma poleks käivitanud hädaseiskamislülitit, oleks isegi kliendiandmed kustutatud" – muutis ta sõjakurjategijast "ettevõtte päästnud kangelaseks".

Johan, kes tavaliselt ignoreeris isegi Slacki mainimisi, suhtles sel konkreetsel korral kõigiga entusiastlikult. Ta oli terve öö üleval, koostades 30-leheküljelist Notion dokumenti pealkirjaga "Ettevõtte AI juhtimispoliitika" ja pidas otsevideo kõnesid kõigi osakondade juhtidega, et panna paika poliitiline alus. Kui tegemist on nende ego rahuldamisega (oma aia kaitsmisega) ja oma puhastustöö vähendamisega, võisid platvormiinsenerid näidata üles hämmastavat kirge ja poliitilist osavust.

Olles kindlustanud juhtkonna täieliku hirmu ja heakskiidu, kehtestas ta kogu ettevõttes äärmusliku komplekti "uusi AI piirdeid". "Kõik AI-ga abistatud kodeerimisvahendid paigutatakse Enterprise Compliance Gate V4 järelevalve alla. 1. Eelkäskluse DLP (Data Loss Prevention) filtreerimine tundliku teabe lekete vältimiseks. 2. AI-spetsiifiline SAST hallutsinatsioonide, haavatavuste ja litsentside saastumise skannimiseks. 3. Kõigi käskluste auditilogide püsiv säilitamine vastavalt EL AI seadusele. 4. 'Human-in-the-Loop' (käsitsi ülevaatus ja heakskiit) kahe vaneminseneri poolt. Iga kood, mis ebaõnnestub isegi ühes ülaltoodust, lükatakse automaatselt tootmiskeskkonda ühendamisest tagasi."

See oli sisuliselt "AI lobotoomia". Arendajate töövoog, mis varem võimaldas juurutusi sekunditega, lämmatati täielikult selle jäiga bürokraatia poolt, mille Johan rõõmsalt üles ehitas. Iga kord, kui arendaja küsis oma redaktoris AI-lt küsimuse, sülitas DLP-värav hoiatusi. Genereeritud kood lükati staatilise analüüsi poolt tagasi ja lõpuks läbinud PR-id jäeti päevadeks vaneminseneride kinnitusjärjekorda mädanema. Ettevõtte arendusproduktiivsus langes tunduvalt alla AI-eelse taseme. Madalama astme insenerid hoidsid avalikes Slacki kanalites suu kinni, väljendades oma pahameelt nende nähtamatute käeraudade üle ainult privaatsetes DM-ides lähedaste kolleegidega. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Suutmata seda taluda, tulid Tech Leadid ja CTO – needsamad inimesed, kes olid varem tema hoiatusi libedate vabandustega vältinud – Johani juurde nutma: "Kas me ei saa kuidagi heakskiiduprotsessi leevendada? Me ei jõua selle tempoga oma funktsioonide väljalasetega." Kuid Johan ei pööranud nende sõnadele absoluutselt mingit tähelepanu.

Johani vaatenurgast ei olnud nad lihtsalt oma teadmatust mõistnud. Kui need arendajad, AI poolt antud lõputu vabaduse illusiooni all, oleksid jätkanud infrastruktuuri teadmatult saastamist, oleksid nad lõpuks põhjustanud "pöördumatu mega-katkestuse" ja kandnud selle eest ise fataalse vastutuse. "Tänu sellele, et ma nende tiivad murdsin, on nad kaitstud 'enesetapu vastutuse' eest. Selle asemel, et mind vihata, peaksid nad mind sügavalt tänama." Johan uskus seda siiralt. Nende tagasi lohistamine "portaali külge seotud võimetuteks tarbijateks" ülekaaluka õigustuse abil oli tema jaoks puhas õiglus ja ülim ülbus.

Lõpuks põgenevad nad ettevõtte range jälgimisvõrgu eest ja hakkavad kasutama volitamata AI-d – "Shadow AI" – isiklikes seadmetes ja mitteametlikel kontodel. Johanil polnud aimugi, et see tooks süsteemile tulevikus veelgi fataalsema korruptsiooni.

Pimeduses toas monitoride ees naeratas Johan vaikselt. Ta oli pannud julmad käerauad vastikule mürale, valitsedes nüüd vaikuse jumalana (vangivalvurina).

"THE EXECUTION" (Täitmisandmebaas: f2e9343f-5fdc)

https://otiose.app/en/execution
