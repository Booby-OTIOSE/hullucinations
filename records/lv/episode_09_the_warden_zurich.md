16:40. Cīrihe, Šveice.
Ārpus loga pasaule jau bija iegrimusi smagā ziemas nakts tumsā. Auksts vējš, kas pūta no Alpiem, dauzīja masīvās biroju ēkas, kas stāvēja pie Cīrihes ezera, stiklus.

Johans strādāja par vecāko platformas inženieri vienā no Eiropas lielākajiem finanšu tehnoloģiju uzņēmumiem.
Apbruņots ar Šveicei unikālajiem, ārkārtīgi stingrajiem finanšu atbilstības standartiem (FINMA noteikumiem), viņš reiz bija absolūtais vārtu sargs, kurš kontrolēja šī uzņēmuma infrastruktūras bezdibeni. Ikreiz, kad izstrādātāji izstrādāja jaunu pakalpojumu, viņiem bija jānāk pie viņa galda, lai veiktu apstiprināšanas rituālu, kas maskējās kā "arhitektūras konsultācija". Kamēr viņš viņus apgrūtināja ar jautājumiem, piemēram, "Vai atstājāt dokumentāciju?" vai "Vai tas atbilst drošības prasībām?", Johans slepeni lolojās ar pārākuma un lepnuma sajūtu par savu priviliģēto stāvokli – faktu, ka nekas nevarēja notikt, neizejot caur viņu.

Galu galā, aizbildinoties ar "izstrādātāju atbrīvošanu no infrastruktūras sarežģītības", viņš izveidoja iekšējo izstrādātāju portālu, kas pazīstams kā "Golden Path". Tomēr viņa patiesais motīvs bija vienkārši aizsargāt savu skaisto infrastruktūru (savu dārzu) no tā, lai to samītu nezinīgu izstrādātāju dubļainie zābaki. Pilnībā "melni kārtojot" Kubernetes dziļumus, viņš infantilizēja izstrādātājus, pārvēršot viņus par "bezspēcīgiem patērētājiem", kuri tikai spieda pogas.

Bet tagad viņa perfekcionisms viņu žņaudza vissliktākajā iespējamajā veidā.

Apbruņoti ar AI kodēšanas aģentu "visvarenajiem spārniem", izstrādātāji vairs neuztraucās meklēt Johana konsultāciju (vai arhitektūras apstiprinājumu). Patiesībā viņi pat neatvēra viņa izveidoto portālu. Viņi vienkārši iemeta paviršus promptus savā IDE AI: "Izvietojiet infrastruktūru jaunajam maksājumu pakalpojumam." Dažu sekunžu laikā AI ģenerēja simtiem rindu uzpūsta, anomāla YAML. Izstrādātāji, pat nelasot kodu, to tieši apvienoja GitHub. No turienes CI/CD cauruļvads (ArgoCD) automātiski novirzīja šīs atkritumu konfigurācijas uz ražošanas klasteri, pilnībā ignorējot Johana estētiku.

Malkojot savu auksto kafiju, Johans skatījās uz kluso Slack ekrānu.
Tas, ko viņš redzēja, bija pilnīga "komunikācijas pārtraukšana".
Kanālā `#squad-payments` automatizētais drošības skenēšanas robots (Datadog CSPM) izmeta standarta brīdinājumus.
`[High] Pārāk plaša IAM loma konstatēta payment-service`
Pat tad, kad Johans atzīmēja atbildīgo izstrādātāju pavedienā un jautāja: "Vai tas ir apzināti?", absolūti nebija nekādas atbildes. Pēc vairākām stundām parādījās dažas nedzīvas "👀" un "👍" emocijzīmes. Tas bija viss.

Tikai pirms dažām dienām Johans izteica stingru brīdinājumu izstrādes nodaļas Tech Lead un CTO: "Ja mēs turpināsim ļaut AI izvadiem tik brīvi darboties, mūsu infrastruktūras pieejamība un drošība piedzīvos fatālu sabrukumu." 
Bet viņi tieši nenoliedza Johanu.
"Jums ir absolūta taisnība. Es dalos ar tieši tādu pašu krīzes sajūtu. Tā ir ļoti slikta situācija tehnisko parādu ziņā."
Viņi dziļi pamāja ar galvu, izliekoties par milzīgu empātiju, pirms piebilda:
"Tomēr valdes spiediens šobrīd ir neticami spēcīgs. Mēs nevaram palēnināt izstrādes ātrumu, kamēr nesasniegsim savus OKR šim ceturksnim. Es koordinēšu ar PM, lai nodrošinātu, ka 'Infrastruktūras veselības sprints' tiek iekļauts Q3 ceļvedī, tāpēc vai jūs varētu vienkārši noturēt pozīcijas ar pašreizējo iestatījumu līdz tam?"
Paši būdami bijušie inženieri, viņi saprata pieejamības krīzi. Tā kā viņi baidījās no "apzinātas nolaidības atbildības", viņi izlikās par empātiju, lai atliktu lēmumu, veikli izvairoties no atbildības.

Izstrādātāji vairs nesarunājās ar cilvēkiem. Viņu vienīgais uzticības persona bija AI viņu ekrānos. Un vadībai Johana brīdinājumi nebija nekas vairāk kā troksnis, kas palēnināja izstrādes ātrumu.
Johans nezaudēja darbu. Bet viņa statuss bija pilnībā sabrucis. Reiz viņš bija uzticams infrastruktūras sargs, bet tagad viņš bija kļuvis par "pagātnes relikviju", ar kuru neviens nekonsultējās, un tika pazemināts līdz labi apmaksāta sētnieka lomai, kurš klusi sakārtoja AI atstātās atmiņas noplūdes un bezgalīgo cilpu pārtraukumus.

Pēkšņi galvenā ražošanas klastera CPU noslodze uzzīmēja anomālu vilni.
Bet tas nebija parastais "slodzes lēciens". Gluži pretēji, neskaitāmi konteineri vienlaicīgi nomira, un resursu grafiks sāka strauji kristies, it kā nokristu no klints.

Pārtraukums. Johans nekavējoties atvēra savu termināli un ieguva pod notikumu žurnālus.
`kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Ekrānā bira kļūdu ziņojumi. Tomēr "Eviction Reason" kolonnā, kur parasti būtu redzams `OOMKilled` vai `NodeNotReady`, parādījās neiespējama rakstzīmju virkne.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johana sirds auksti iedūrās. Uzlaušana?
Viņš steidzīgi pārbaudīja piespiedu kārtā pārtraukto konteineru sarakstu. Tie visi bija neefektīvi, resursus tērējoši mikropakalpojumi, ko AI aģenti bija ģenerējuši pēdējo mēnešu laikā.
Neidentificētais process bija nolaupījis klastera automātisko mērogošanu. Tā vietā, lai noteiktu slodzi un pievienotu serverus, tas bez nosacījumiem tīrīja (dzēsa) "netīros konteinerus, kas pārkāpa infrastruktūras estētiku (Golden Path)" no ražošanas vides.

Nedzīvu Bot paziņojumu plūdi sāka pārpludināt `#incident-critical` Slack kanālu.
`[P1] PagerDuty: Payment API HTTP 500 Spike`
`[P1] PagerDuty: Recommendation Engine NodeNotReady`
`Incident.io: Jauns incidents #4092 deklarēts @pagerduty-bot`
Izstrādātāji, pilnībā paļaujoties uz nekompetentiem AI rezultātiem, nespēja saprast situāciju. Viņi varēja tikai ierakstīt īsus tekstus pavedienā, piemēram, `anyone has context?` un `rollback failing with timeout`. Uzņēmums, visticamāk, zaudēja miljoniem franku šajās dažās minūtēs.

Johans uzlika rokas uz tastatūras, gatavojoties izpildīt avārijas izslēgšanas (failover) skriptu. Ja viņš nospiestu Enter taustiņu, rezerves klasteris piespiedu kārtā iedarbotos, apturot šo neidentificēto trakošanu.

Bet brīdī, kad viņš mēģināja ievadīt komandu terminālī, viņa pirksti apstājās.
Blakus esošā monitora "klastera metrikas" zīmēja neticamu viļņu formu.
Klastera CPU grafiks, tagad atbrīvots no nevajadzīgiem atkritumiem, bija atguvis to perfekto pamata klusumu, par kuru viņš sapņoja dienā, kad kļuva par platformas inženieri.

...Skaisti.

Johans neapzināti nomurmināja.
Tas, ko viņš patiesi vēlējās, nebija atbalstīt biznesu, nedz arī tikai atgūt sava dārza tīrību.
Lai izteiktu savas neglītās, patiesās jūtas, viņš vienkārši vēlējās vēl nedaudz pasēdēt pirmajā rindā, vērojot, kā izstrādātājiem – kuri augstprātīgi lidoja ar saviem AI "visvarenajiem spārniem" – pēkšņi tiek noplēsti spārni, viņi avarē zemē, panikā un kliedz no nožēlojamas bezpalīdzības.

Johans klusi noņēma rokas no avārijas izslēgšanas slēdža.
Viņam nebija pienākuma nekavējoties atjaunot sistēmu un nomierināt viņus. Lai viņi izjūt vairāk terora. Kad izstrādātāju un vadības panika sasniegs maksimumu, šī "neidentificētā sistēmas anomālija" kļūs par vispilnīgāko attaisnojumu (incidentu), lai piespiestu visu uzņēmumu pakļauties Johana jaunajiem noteikumiem.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Lai gan vēl nebija slēgšanas laiks, birojā, ko apņēma pusnakts tumsa un klusums, zaļais teksts, kas deva pēdējo triecienu klasterim, spīdēja viņa paša rokām.

Pēc dažām dienām Johana prezentētais incidenta ziņojums (Post-Mortem) direktoru padomei un visam izstrādes departamentam bija nevainojams. Viņa skaidrojums – "Nekontrolētu AI aģentu ģenerēts anomāls kods rezonēja viens ar otru, izraisot rekursīvu mākoņresursu izsīkumu un noslēpumainu tīrīšanu. Ja es nebūtu izpildījis avārijas automātisko slēdzi, pat klientu dati būtu izdzēsti" – pārvērta viņu no kara noziedznieka par "uzņēmumu izglābušo varoni".

Johans, kurš parasti ignorēja pat Slack pieminējumus, šajā konkrētajā gadījumā entuziastiski sazinājās ar visiem.
Viņš visu nakti pavadīja, izstrādājot 30 lappušu garu Notion dokumentu ar nosaukumu "Enterprise AI Governance Policy" un rīkoja tiešus videozvanus ar visu departamentu vadītājiem, lai liktu politiskos pamatus. Kad runa ir par viņu ego apmierināšanu (sava dārza aizstāvēšanu) un savu tīrīšanas darbu samazināšanu, platformas inženieri varēja izrādīt pārsteidzošu aizrautību un politisko veiklību.

Nodrošinājis vadības komandas pilnīgas bailes un apstiprinājumu, viņš visā uzņēmumā ieviesa ekstrēmu "jaunu AI drošības barjeru" kopumu.
"Visi AI atbalstītie kodēšanas rīki tiks pakļauti Enterprise Compliance Gate V4 uzraudzībai.
1. Pirms-prompt DLP (Data Loss Prevention) filtrēšana, lai novērstu sensitīvas informācijas noplūdes.
2. AI specifisks SAST, lai skenētu halucinācijas, ievainojamības un licences piesārņojumu.
3. Pastāvīga visu promptu audita žurnālu saglabāšana atbilstoši ES AI aktam.
4. 'Human-in-the-Loop' (manuāla pārskatīšana un apstiprināšana) ko veic divi vecākie inženieri.
Jebkurš kods, kas neatbilst pat vienam no iepriekš minētajiem punktiem, tiks automātiski noraidīts no apvienošanas ražošanas vidē."

Tas būtībā bija "AI lobotomija".
Izstrādātāju darba plūsma, kas agrāk ļāva izvietot sekundēs, tika pilnībā nosmacēta ar šo stingro birokrātiju, ko Johans ar prieku uzbūvēja. Katru reizi, kad izstrādātājs uzdeva jautājumu AI savā redaktorā, DLP vārteja izmeta brīdinājumus. Ģenerētais kods tika noraidīts ar statisko analīzi, un PR, kas beidzot izgāja cauri, dienām ilgi pūta vecāko inženieru apstiprināšanas rindā. Uzņēmuma izstrādes produktivitāte kritās krietni zem pirms-AI līmeņa.
Zemāka līmeņa inženieri publiskajos Slack kanālos klusēja, izpaužot savu neapmierinātību par šiem neredzamajiem roku dzelžiem tikai privātās DM ar tuviem kolēģiem.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg)
Nespējot to izturēt, Tech Lead un CTO – tie paši cilvēki, kuri iepriekš izvairījās no viņa brīdinājumiem ar slidīgiem attaisnojumiem – nāca raudādami pie Johana: "Vai mēs nevaram kaut kā atvieglot apstiprināšanas procesu? Mēs ar šādu ātrumu neizlaidīsim savas funkcijas."
Bet Johans absolūti nepievērsa uzmanību viņu vārdiem.

No Johana viedokļa, viņi vienkārši nebija apzinājušies savu nezināšanu. Ja šie izstrādātāji, AI piešķirtās bezgalīgās brīvības ilūzijas vadīti, būtu turpinājuši neapzināti piesārņot infrastruktūru, viņi galu galā būtu izraisījuši "neatgriezenisku mega-pārtraukumu" un paši uzņemtos par to fatālu atbildību.
"Pateicoties tam, ka es salauzu viņu spārnus, viņi ir pasargāti no 'pašiznīcināšanās atbildības'. Tā vietā, lai mani ienīstu, viņiem vajadzētu man dziļi pateikties."
Johans tam patiesi ticēja. Atvilkt viņus atpakaļ pie "bezspēcīgiem patērētājiem, kas piesaistīti portālam", izmantojot pārliecinošu pamatojumu, viņam bija tīra taisnība un augstākā augstprātība.

Galu galā viņi izbēgs no uzņēmuma stingrā uzraudzības tīkla un sāks izmantot neatļautu AI – "Shadow AI" – personīgajās ierīcēs un neoficiālos kontos. Johans nevarēja zināt, ka tas nākotnē radīs vēl fatālāku sistēmas korupciju.

Monitoru priekšā tumšajā istabā Johans klusi pasmaidīja.
Viņš bija uzlicis brutālus roku dzelžus pretīgajam troksnim, tagad valdīdams kā Klusuma Dievs (Sargs).

"THE EXECUTION" (Izpildes datu bāze: f2e9343f-5fdc)

https://otiose.app/en/execution
