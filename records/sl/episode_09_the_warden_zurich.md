16:40. Zürich, Švica. Zunaj se je svet že pogreznil v težko temo zimske noči. Mrzel veter, ki je pihal z Alp, je udarjal ob steklo masivne poslovne stavbe ob Züriškem jezeru.

Johan je delal kot višji inženir platforme v enem največjih evropskih finančnih tehnoloških podjetij. Oborožen z izjemno strogimi standardi finančne skladnosti, edinstvenimi za Švico (predpisi FINMA), je bil nekoč absolutni vratar, ki je nadzoroval brezno infrastrukture tega podjetja. Kadarkoli so razvijalci zasnovali novo storitev, so morali priti k njegovi mizi na ritual odobritve, preoblečen v »arhitekturno posvetovanje«. Medtem ko jih je nadlegoval z vprašanji, kot so »Ste pustili dokumentacijo?« ali »Ali to ustreza varnostnim zahtevam?«, je Johan na skrivaj gojil občutek superiornosti in ponosa na svoj privilegiran položaj – dejstvo, da se nič ni moglo premakniti, ne da bi šlo skozi njega.

Sčasoma je pod pretvezo »osvoboditve razvijalcev od kompleksnosti infrastrukture« zgradil interni razvijalski portal, znan kot »Golden Path«. Njegov pravi motiv pa je bil preprosto zaščititi svojo čudovito infrastrukturo (svoj vrt) pred tem, da bi jo poteptali blatni škornji nevednih razvijalcev. S popolnim zakrivanjem globin Kubernetes je razvijalce infantiliziral in jih spremenil v »nemogoče potrošnike«, ki so le pritiskali gumbe.

Toda zdaj ga je njegov perfekcionizem dušil na najslabši možen način.

Oboroženi z »vsemogočnimi krili« agentov za kodiranje z umetno inteligenco, se razvijalci niso več trudili iskati Johanovega posvetovanja (ali arhitekturne odobritve). Pravzaprav sploh niso odprli portala, ki ga je zgradil. Preprosto so v AI svojega IDE vrgli površne pozive: »Namesti infrastrukturo za novo plačilno storitev.« V nekaj sekundah je AI ustvarila na stotine vrstic napihnjenega, anomalnega YAML-a. Razvijalci, ne da bi sploh prebrali kodo, so jo združili naravnost v GitHub. Od tam je CI/CD cevovod (ArgoCD) samodejno preusmeril te smetne konfiguracije v produkcijski klaster, popolnoma ignorirajoč Johanovo estetiko.

Johan je srkal hladno kavo in strmel v tihi zaslon Slacka. Kar je videl, je bila popolna »prekinitev komunikacije«. V kanalu `#squad-payments` je avtomatizirani bot za varnostno skeniranje (Datadog CSPM) izpljuval standardna opozorila. `[High] Overly broad IAM role detected in payment-service` Tudi ko je Johan v niti označil odgovornega razvijalca in vprašal: »Ali je to namerno?«, ni bilo absolutno nobenega odgovora. Ure kasneje se je pojavilo nekaj brezživih emojijev »👀« in »👍«. To je bilo vse.

Pred nekaj dnevi je Johan izdal ostro opozorilo tehničnemu vodji razvojnega oddelka in CTO-ju: »Če bomo še naprej dopuščali, da se izhodi AI tako nekontrolirano širijo, bosta razpoložljivost in varnost naše infrastrukture doživeli usoden kolaps.« Vendar Johana niso odkrito zanikali. »Imate popolnoma prav. Delim popolnoma enak občutek krize. To je zelo slaba situacija z vidika tehničnega dolga.« Globoko so prikimali, pretvarjajoč se, da imajo izjemno empatijo, preden so dodali: »Vendar je pritisk uprave trenutno neverjetno močan. Ne moremo upočasniti hitrosti razvoja, dokler ne dosežemo naših OKR-jev za ta četrtletje. Koordiniral bom s PM-ji, da se 'Infrastructure Sanity Sprint' vključi v načrt Q3, zato bi lahko do takrat obdržali trenutno postavitev?« Ker so bili sami nekdanji inženirji, so razumeli krizo razpoložljivosti. Ker so se bali »odgovornosti za namerno malomarnost«, so se pretvarjali, da so empatični, da bi odložili odločitev, in se gladko izognili odgovornosti.

Razvijalci se niso več pogovarjali z ljudmi. Njihov edini zaupnik je bila AI na njihovih zaslonih. In za vodstvo so bila Johanova opozorila le hrup, ki je upočasnjeval hitrost razvoja. Johan ni izgubil službe. Toda njegov status se je popolnoma sesul. Nekoč so se nanj zanašali kot na varuha infrastrukture, postal je »relikvija preteklosti«, s katero se nihče ni posvetoval, in je bil degradiran v visoko plačanega hišnika, ki je tiho čistil uhajanje pomnilnika in izpade neskončnih zank, ki jih je pustila AI.

Nenadoma je izkoriščenost CPU glavnega produkcijskega klastra narisala anomalen val.

Vendar to ni bil običajen »vrh obremenitve«. Nasprotno, nešteto kontejnerjev je hkrati umrlo, in graf virov je začel strmoglavljati kot s pečine.

Izpad. Johan je takoj odprl svoj terminal in pridobil dnevnike dogodkov podov. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Sporočila o napakah so se kaskadno spuščala po zaslonu. Vendar se je v stolpcu »Eviction Reason«, kjer bi se običajno prikazalo `OOMKilled` ali `NodeNotReady`, pojavil nemogoč niz znakov.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johanu je srce hladno poskočilo. Vlom? Naglo je preveril seznam prisilno ustavljenih kontejnerjev. Vsi so bili neučinkoviti, viri zapravljivi mikroservisi, ki so jih v zadnjih mesecih ustvarili agenti AI. Neznani proces je ugrabil avtomatsko skaliranje klastra. Namesto da bi zaznal obremenitev in dodal strežnike, je brezpogojno čistil (brisal) »umazane kontejnerje, ki so kršili estetsko infrastrukturo (Golden Path)« iz produkcijskega okolja.

Tok brezživih obvestil Bot je začel preplavljati Slack kanal `#incident-critical`. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Razvijalci, popolnoma odvisni od nekompetentnih izhodov AI, niso mogli razumeti situacije. V nit so lahko vtipkali le kratka besedila, kot sta `anyone has context?` in `rollback failing with timeout`. Podjetje je v teh nekaj minutah verjetno izgubljalo milijone frankov.

Johan je položil roke na tipkovnico in se pripravljal na izvedbo skripta za izklop v sili (failover). Če bi pritisnil tipko Enter, bi se rezervni klaster prisilno zagnal in ustavil to neznano divjanje.

Toda v trenutku, ko je poskušal vtipkati ukaz v terminal, so se mu prsti ustavili.

»Metrike klastra« na sosednjem monitorju so risale neverjetno valovno obliko. Graf CPU klastra, zdaj očiščen nepotrebnih smeti, je ponovno pridobil popolno osnovno tišino, o kateri je sanjal tistega dne, ko je postal inženir platforme.

...Lepo.

Johan je nezavedno zamrmral. Kar si je resnično želel, ni bilo podpiranje posla, niti zgolj povrnitev čistosti svojega vrta. Da bi izrazil svoje grde, resnične občutke, je preprosto želel še malo sedeti v prvi vrsti in opazovati razvijalce – ki so arogantno leteli s svojimi »vsemogočnimi krili« umetne inteligence – kako so jim nenadoma odtrgali krila, strmoglavili na tla, paničarili in kričali v patetični nemočnosti.

Johan je tiho umaknil roke z izklopnega stikala. Ni imel obveznosti, da bi takoj obnovil sistem in jih pomiril. Naj občutijo več groze. Ko bo panika razvijalcev in vodstva dosegla vrhunec, se bo ta »neznana sistemska anomalija« povzdignila v najpopolnejšo utemeljitev (incident), da se celotno podjetje prisili v podreditev Johanovim novim pravilom.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Čeprav še ni bil čas za zapiranje, je v pisarni, oviti v polnočno temo in tišino, zeleno besedilo, ki je zadalo končni udarec klastru, zasvetilo po njegovih lastnih rokah.

Nekaj dni kasneje je bila predstavitev Poročila o incidentu (Post-Mortem), ki jo je Johan predstavil upravnemu odboru in celotnemu razvojnemu oddelku, brezhibna. Njegova razlaga – »Anomalna koda, ki so jo ustvarili nenadzorovani agenti AI, je medsebojno resonirala, kar je sprožilo rekurzivno izčrpavanje virov v oblaku in skrivnostno čiščenje. Če ne bi izvedel zasilnega izklopa, bi bili izbrisani celo podatki strank« – ga je spremenila iz vojnega zločinca v »junaka, ki je rešil podjetje«.

Johan, ki je običajno ignoriral celo omembe v Slacku, je ob tej priložnosti navdušeno komuniciral z vsemi. Celotno noč je preživel pri pripravi 30-stranskega dokumenta v Notionu z naslovom »Enterprise AI Governance Policy« in opravil neposredne videoklice z vodji vseh oddelkov, da bi postavil politične temelje. Ko gre za zadovoljevanje njihovega ega (obrambo njihovega vrta) in zmanjšanje lastnega dela čiščenja, lahko inženirji platforme pokažejo osupljivo strast in politično spretnost.

Ko si je zagotovil popoln strah in odobravanje vodstvene ekipe, je po celotnem podjetju uvedel skrajni nabor »novih AI varoval«. »Vsa orodja za kodiranje, podprta z AI, bodo pod nadzorom Enterprise Compliance Gate V4. 1. Predhodno filtriranje DLP (Data Loss Prevention) za preprečevanje uhajanja občutljivih informacij. 2. SAST, specifičen za AI, za skeniranje halucinacij, ranljivosti in kontaminacije licenc. 3. Trajno shranjevanje revizijskih dnevnikov za vse pozive v skladu z EU AI Act. 4. 'Human-in-the-Loop' (ročni pregled in odobritev) s strani dveh višjih inženirjev. Vsaka koda, ki ne uspe izpolniti niti enega od zgoraj navedenih pogojev, bo samodejno zavrnjena za združitev v produkcijsko okolje.«

To je bila v bistvu »lobotomija za AI«. Delovni proces razvijalcev, ki je prej omogočal implementacije v nekaj sekundah, je bil popolnoma zadušen s to togo birokracijo, ki jo je Johan z veseljem zgradil. Vsakič, ko je razvijalec v svojem urejevalniku postavil vprašanje AI, je prehod DLP izpljunil opozorila. Ustvarjena koda je bila zavrnjena s statično analizo, PR-ji, ki so končno prešli, pa so dneve gnili v čakalni vrsti za odobritev višjih inženirjev. Produktivnost razvoja podjetja je padla daleč pod raven pred AI. Nižji inženirji so molčali v javnih Slack kanalih, svoje nezadovoljstvo nad temi nevidnimi lisicami pa so izražali le v zasebnih DM-jih z bližnjimi sodelavci. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Ker tega niso mogli prenesti, so Tech Leads in CTO – isti ljudje, ki so se prej izogibali njegovim opozorilom z izmikajočimi se izgovori – prišli k Johanu jokajoč: »Ali ne moremo nekako sprostiti postopka odobritve? S to hitrostjo ne bomo uspeli izdati naših funkcij.« Toda Johan ni posvečal nobene pozornosti njihovim besedam.

Z Johanovega vidika preprosto niso spoznali lastne nevednosti. Če bi ti razvijalci, pod iluzijo neskončne svobode, ki jo je podelila AI, še naprej nevede onesnaževali infrastrukturo, bi sčasoma povzročili »nepovratno mega-izpad« in sami nosili usodno odgovornost za to. »Zahvaljujoč meni, ki sem jim zlomil krila, so zaščiteni pred 'odgovornostjo samouničenja'. Namesto da bi mi zamerili, bi se mi morali globoko zahvaliti.« Johan je v to iskreno verjel. Vlečenje nazaj v »nemogoče potrošnike, vezane na portal« z uporabo premočne utemeljitve je bilo zanj čista pravičnost in vrhunska aroganca.

Sčasoma bi pobegnili iz strogega nadzornega omrežja podjetja in začeli uporabljati nepooblaščeno AI – »Shadow AI« – na osebnih napravah in neuradnih računih. Johan ni mogel vedeti, da bo to v prihodnosti prineslo še bolj usodno korupcijo v sistem.

Pred monitorji v temni sobi se je Johan tiho nasmehnil. Na osovraženi hrup je namestil brutalne lisice, zdaj pa je vladal kot Bog tišine (Varuh).

"THE EXECUTION" (Baza podatkov izvedbe: f2e9343f-5fdc)

https://otiose.app/en/execution
