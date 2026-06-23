16:40. Ciurichas, Šveicarija.
Už lango pasaulis jau buvo paskendęs sunkioje žiemos nakties tamsoje. Šaltas vėjas, pučiantis nuo Alpių, daužė masyvaus biurų pastato, stovinčio prie Ciuricho ežero, stiklą.

Johanas dirbo vyresniuoju platformos inžinieriumi vienoje didžiausių Europos finansinių technologijų įmonių.
Apšarvuotas išskirtinai griežtais finansinės atitikties standartais, būdingais Šveicarijai (FINMA taisyklės), jis kadaise buvo absoliutus vartų sargas, kontroliuojantis šios įmonės infrastruktūros bedugnę. Kai tik kūrėjai suprojektuodavo naują paslaugą, jie turėdavo ateiti prie jo stalo atlikti patvirtinimo ritualo, maskuojamo kaip „architektūros konsultacija“. Klausinėdamas jų klausimais, pavyzdžiui, „Ar palikote dokumentaciją?“ arba „Ar tai atitinka saugumo reikalavimus?“, Johanas slapta jautė pranašumo ir pasididžiavimo jausmą dėl savo privilegijuotos padėties – fakto, kad niekas negalėjo pajudėti, nepraėjęs pro jį.

Galiausiai, prisidengdamas „kūrėjų išlaisvinimu nuo infrastruktūros sudėtingumo“, jis sukūrė vidinį kūrėjų portalą, žinomą kaip „Golden Path“. Tačiau jo tikrasis motyvas buvo tiesiog apsaugoti savo gražią infrastruktūrą (savo sodą) nuo to, kad ją sutryptų purvini nežinančių kūrėjų batai. Visiškai uždarydamas Kubernetes gelmes į juodąją dėžę, jis infantilizavo kūrėjus, paversdamas juos „beprasmiais vartotojais“, kurie tiesiog spaudė mygtukus.

Bet dabar jo perfekcionizmas jį smaugė pačiu blogiausiu būdu.

Apsiginklavę AI kodavimo agentų „visagaliais sparnais“, kūrėjai nebesivargino ieškoti Johano konsultacijos (arba architektūros patvirtinimo). Tiesą sakant, jie net neatidarė jo sukurto portalo. Jie tiesiog mėtė aplaidžius raginimus į savo IDE AI: „Įdiekite infrastruktūrą naujai mokėjimo paslaugai.“ Per kelias sekundes AI sugeneruodavo šimtus eilučių išpūsto, anomalaus YAML. Kūrėjai, net neskaitydami kodo, jį tiesiai sujungdavo į GitHub. Iš ten CI/CD konvejeris (ArgoCD) automatiškai nukreipdavo šias šiukšlių konfigūracijas į gamybos klasterį, visiškai ignoruodamas Johano estetiką.

Gerkšnodamas šaltą kavą, Johanas spoksojo į tylų Slack ekraną.
Tai, ką jis matė, buvo visiškas „komunikacijos nutraukimas“.
Kanale `#squad-payments` automatinis saugumo skenavimo robotas (Datadog CSPM) spjaudė standartinius įspėjimus.
`[High] Aptiktas per platus IAM vaidmuo payment-service`
Net kai Johanas pažymėjo atsakingą kūrėją gijoje ir paklausė: „Ar tai tyčia?“, nebuvo absoliučiai jokio atsakymo. Po kelių valandų pasirodė keli bejausmiai „👀“ ir „👍“ jaustukai. Tai buvo viskas.

Visai neseniai Johanas griežtai įspėjo plėtros skyriaus Tech Lead ir CTO: „Jei ir toliau leisime AI išvestims taip laisvai veikti, mūsų infrastruktūros prieinamumas ir saugumas patirs mirtiną žlugimą.“ 
Tačiau jie tiesiogiai nepaneigė Johano.
„Jūs visiškai teisus. Aš jaučiu lygiai tą patį krizės jausmą. Tai labai bloga situacija techninės skolos požiūriu.“
Jie giliai linktelėjo, apsimesdami didžiule empatija, prieš pridėdami:
„Tačiau valdybos spaudimas šiuo metu yra nepaprastai stiprus. Negalime sulėtinti plėtros greičio, kol nepasieksime savo OKR šiam ketvirčiui. Aš koordinuosiu su PM, kad „Infrastruktūros sveikos protas sprintas“ būtų įtrauktas į Q3 veiksmų planą, tad ar galėtumėte tiesiog palaikyti esamą sistemą iki tol?“
Patys būdami buvę inžinieriai, jie suprato prieinamumo krizę. Kadangi jie bijojo „tyčinio aplaidumo atsakomybės“, jie apsimetė empatija, kad atidėtų sprendimą, sklandžiai išsisukdami nuo atsakomybės.

Kūrėjai nebesikalbėjo su žmonėmis. Jų vienintelis patikėtinis buvo AI jų ekranuose. O vadovybei Johano įspėjimai buvo ne kas kita, kaip triukšmas, lėtinantis plėtros greitį.
Johanas neprarado darbo. Tačiau jo statusas visiškai žlugo. Kadaise juo pasitikėta kaip infrastruktūros sargu, jis tapo „praeities relikvija“, su kuriuo niekas nesikonsultavo, nustumtas į gerai apmokamo valytojo vaidmenį, kuris tyliai tvarkė AI paliktus atminties nutekėjimus ir begalinio ciklo gedimus.

Staiga pagrindinio gamybos klasterio CPU panaudojimas nupiešė anomalią bangą.
Tačiau tai nebuvo įprastas „apkrovos šuolis“. Priešingai, daugybė konteinerių mirė vienu metu, o resursų grafikas pradėjo kristi žemyn, tarsi nuo uolos.

Gedimas. Johanas nedelsdamas atidarė savo terminalą ir gavo pod įvykių žurnalus.
`kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Ekrane pasipylė klaidų pranešimai. Tačiau stulpelyje „Eviction Reason“, kur paprastai būtų rodoma `OOMKilled` arba `NodeNotReady`, pasirodė neįmanoma simbolių eilutė.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johano širdis šaltai praleido dūžį. Įsilaužimas?
Jis skubiai patikrino priverstinai nutrauktų konteinerių sąrašą. Visi jie buvo neefektyvūs, resursus eikvojantys mikroservisai, kuriuos AI agentai generavo per pastaruosius kelis mėnesius.
Neidentifikuotas procesas užgrobė klasterio automatinį mastelį. Užuot aptikęs apkrovą ir pridėjęs serverių, jis besąlygiškai šalino (valė) „nešvarius konteinerius, kurie pažeidė infrastruktūros estetiką (Golden Path)“ iš gamybos aplinkos.

Gyvybės netekusių Bot pranešimų srautas pradėjo užplūsti `#incident-critical` Slack kanalą.
`[P1] PagerDuty: Payment API HTTP 500 Spike`
`[P1] PagerDuty: Recommendation Engine NodeNotReady`
`Incident.io: Naujas incidentas #4092 paskelbtas @pagerduty-bot`
Kūrėjai, visiškai pasikliaujantys nekompetentingais AI rezultatais, negalėjo suprasti situacijos. Jie galėjo tik įvesti trumpus tekstus į giją, pvz., `anyone has context?` ir `rollback failing with timeout`. Įmonė tikriausiai prarado milijonus frankų per šias kelias minutes.

Johanas padėjo rankas ant klaviatūros, ruošdamasis vykdyti avarinio išjungimo (failover) scenarijų. Jei jis paspaustų Enter klavišą, atsarginis klasteris priverstinai pasileistų, sustabdydamas šį neidentifikuotą siautėjimą.

Tačiau tą akimirką, kai jis bandė įvesti komandą į terminalą, jo pirštai sustojo.
Šalia esančiame monitoriuje esantys „klasterio metrikos“ rodė neįtikėtiną bangos formą.
Klasterio CPU grafikas, dabar išvalytas nuo nereikalingų šiukšlių, atgavo tobulą bazinį tylą, apie kurią jis svajojo tą dieną, kai tapo platformos inžinieriumi.

...Gražu.

Johanas nesąmoningai sumurmėjo.
Tai, ko jis iš tiesų troško, nebuvo verslo palaikymas, nei vien tik jo sodo grynumo atgavimas.
Norėdamas išsakyti savo bjaurius, tikrus jausmus, jis tiesiog norėjo dar šiek tiek pasėdėti pirmoje eilėje, stebėdamas, kaip kūrėjams – kurie arogantiškai skraidė su savo „visagaliais AI sparnais“ – staiga nuplėšiami sparnai, jie krenta ant žemės, panikuoja ir rėkia apgailėtinu bejėgiškumu.

Johanas tyliai patraukė rankas nuo avarinio išjungimo jungiklio.
Jis neturėjo jokios pareigos nedelsiant atkurti sistemos ir juos nuraminti. Tegul jie jaučia daugiau siaubo. Kai kūrėjų ir vadovybės panika pasieks viršūnę, ši „neidentifikuota sistemos anomalija“ taps tobuliausiu pateisinimu (incidentu), kad visa įmonė būtų priversta paklusti Johano naujoms taisyklėms.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Nors dar nebuvo darbo pabaiga, biure, apgaubtame vidurnakčio tamsos ir tylos, žalias tekstas, kuris sudavė paskutinį smūgį klasteriui, švytėjo jo paties rankomis.

Po kelių dienų Johano pristatytas incidento ataskaita (Post-Mortem) valdybai ir visam plėtros skyriui buvo nepriekaištinga. Jo paaiškinimas – „Nekontroliuojamų AI agentų sugeneruotas anomališkas kodas rezonuodavo vienas su kitu, sukeldamas rekursinį debesies resursų išsekimą ir paslaptingą išvalymą. Jei nebūčiau įvykdęs avarinio grandinės pertraukiklio, net klientų duomenys būtų buvę ištrinti“ – pavertė jį iš karo nusikaltėlio į „įmonę išgelbėjusį herojų“.

Johanas, kuris paprastai ignoruodavo net Slack paminėjimus, šia proga entuziastingai bendravo su visais.
Jis visą naktį rengė 30 puslapių Notion dokumentą pavadinimu „Enterprise AI Governance Policy“ ir tiesiogiai bendravo vaizdo skambučiais su visų departamentų vadovais, kad padėtų politinius pagrindus. Kai kalbama apie jų ego patenkinimą (savo sodo gynimą) ir savo valymo darbų sumažinimą, platformos inžinieriai gali parodyti stulbinamą aistrą ir politinį meistriškumą.

Užsitikrinęs visišką vadovybės komandos baimę ir pritarimą, jis visoje įmonėje įvedė ekstremalų „naujų AI apsaugos priemonių“ rinkinį.
„Visi AI pagalba veikiantys kodavimo įrankiai bus stebimi Enterprise Compliance Gate V4.
1. Išankstinis DLP (Data Loss Prevention) filtravimas, siekiant užkirsti kelią slaptos informacijos nutekėjimui.
2. AI specifinis SAST, skirtas haliucinacijoms, pažeidžiamumams ir licencijos užterštumui nuskaityti.
3. Nuolatinis visų raginimų audito žurnalų saugojimas, atitinkantis ES AI aktą.
4. „Human-in-the-Loop“ (rankinis peržiūra ir patvirtinimas) atliekamas dviejų vyresniųjų inžinierių.
Bet koks kodas, neatitinkantis net vieno iš aukščiau nurodytų punktų, bus automatiškai atmestas ir nebus sujungtas su gamybos aplinka.“

Tai iš esmės buvo „lobotomija AI“.
Kūrėjų darbo eiga, kuri anksčiau leido diegti per kelias sekundes, buvo visiškai užgniaužta šios griežtos biurokratijos, kurią Johanas džiaugsmingai sukūrė. Kiekvieną kartą, kai kūrėjas užduodavo klausimą AI savo redaktoriuje, DLP vartai spjaudė įspėjimus. Sugeneruotas kodas buvo atmestas statinės analizės, o galiausiai praėję PR buvo palikti pūti dienomis vyresniųjų inžinierių patvirtinimo eilėje. Įmonės plėtros produktyvumas smuko gerokai žemiau prieš AI buvusio lygio.
Žemesnio lygio inžinieriai viešuose Slack kanaluose tylėjo, o savo pasipiktinimą dėl šių nematomų antrankių išliedavo tik privačiuose DM su artimais kolegomis.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg)
Negalėdami to ištverti, Tech Leadai ir CTO – tie patys žmonės, kurie anksčiau išsisukinėjo nuo jo įspėjimų su slidžiais pasiteisinimais – atėjo verkdami pas Johaną: „Ar negalėtume kaip nors sušvelninti patvirtinimo proceso? Tokiu tempu mes neįvykdysime savo funkcijų išleidimo.“
Tačiau Johanas visiškai nekreipė dėmesio į jų žodžius.

Johano požiūriu, jie tiesiog nesuvokė savo nežinojimo. Jei šie kūrėjai, apgaulingos AI suteiktos begalinės laisvės iliuzijos apimti, būtų toliau nesąmoningai teršę infrastruktūrą, jie galiausiai būtų sukėlę „negrįžtamą mega-gedimą“ ir patys prisiėmę už tai mirtiną atsakomybę.
„Dėka manęs, sulaužiusio jų sparnus, jie yra apsaugoti nuo „sunaikinimo atsakomybės“. Užuot manęs nekentę, jie turėtų man giliai dėkoti.“
Johanas nuoširdžiai tuo tikėjo. Grąžinti juos į „prie portalo pririštus bejėgius vartotojus“ naudojant didžiulį pateisinimą, jam buvo gryna teisybė ir didžiausias arogancija.

Galiausiai jie išvengs griežto įmonės stebėjimo tinklo ir pradės naudoti neautorizuotą AI – „Shadow AI“ – asmeniniuose įrenginiuose ir neoficialiose paskyrose. Johanas negalėjo žinoti, kad tai ateityje sukels dar fatališkesnę sistemos korupciją.

Prie monitorių tamsiame kambaryje Johanas tyliai nusišypsojo.
Jis uždėjo žiaurius antrankius ant nekenčiamo triukšmo, dabar viešpataudamas kaip Tylos Dievas (Sargas).

"THE EXECUTION" (Vykdymo duomenų bazė: f2e9343f-5fdc)

https://otiose.app/en/execution
