Délután 4:40. Zürich, Svájc.
Az ablakon kívül a világ már a téli éjszaka súlyos sötétségébe süllyedt. Az Alpokból lefelé fújó hideg szél ostromolta a Zürichi-tó partján álló hatalmas irodaház üvegét.

Johan vezető platformmérnökként dolgozott Európa egyik legnagyobb pénzügyi technológiai vállalatánál.
A Svájcra egyedülállóan jellemző, kivételesen szigorú pénzügyi megfelelőségi szabványokkal (FINMA szabályozások) felvértezve, ő volt egykor az abszolút kapuőr, aki a vállalat infrastruktúrájának mélységét irányította. Amikor a fejlesztők új szolgáltatást terveztek, el kellett jönniük az asztalához egy "architektúra-konzultációnak" álcázott jóváhagyási rituáléra. Miközben olyan kérdésekkel zaklatta őket, mint "Hagytál dokumentációt?" vagy "Ez megfelel a biztonsági követelményeknek?", Johan titokban felsőbbrendűségi érzést és büszkeséget táplált kiváltságos pozíciója miatt – amiért semmi sem mozdulhatott el anélkül, hogy rajta keresztül ne ment volna.

Végül, a "fejlesztők infrastruktúra komplexitásától való felszabadítása" ürügyén, megépítette a "Golden Path" néven ismert belső fejlesztői portált. Valódi indítéka azonban egyszerűen az volt, hogy megvédje gyönyörű infrastruktúráját (a kertjét) attól, hogy tudatlan fejlesztők sáros csizmái tapossák. A Kubernetes mélységeinek teljes fekete dobozba zárásával infantilizálta a fejlesztőket, "erőtlen fogyasztókká" változtatva őket, akik csupán gombokat nyomogattak.

De most a perfekcionizmusa a lehető legrosszabb módon fojtogatta.

Az AI kódoló ügynökök "mindenható szárnyai" által felvértezve a fejlesztők már nem vették a fáradságot, hogy Johan konzultációját (vagy architektúra-jóváhagyását) kérjék. Sőt, még az általa épített portált sem nyitották meg. Egyszerűen csak hanyag promptokat dobtak az IDE-jük AI-jába: "Telepítsd az infrastruktúrát az új fizetési szolgáltatáshoz." Másodperceken belül az AI több száz sornyi felfújt, anomális YAML-t generált. A fejlesztők, anélkül, hogy elolvasták volna a kódot, egyenesen a GitHubba egyesítették. Onnan a CI/CD pipeline (ArgoCD) automatikusan bejuttatta ezeket a szemét konfigurációkat a produkciós klaszterbe, teljesen figyelmen kívül hagyva Johan esztétikáját.

Hideg kávéját kortyolgatva Johan a néma Slack képernyőre bámult.
Ami látott, az a "kommunikáció teljes megszakadása" volt.
A `#squad-payments` csatornán az automatizált biztonsági szkennelő bot (Datadog CSPM) sablonos riasztásokat köpködött.
`[High] Túl széles IAM szerepkör észlelve a payment-service-ben`
Még amikor Johan megjelölte a felelős fejlesztőt a szálban, és megkérdezte: "Ez szándékos?", abszolút semmilyen válasz nem érkezett. Órákkal később néhány élettelen "👀" és "👍" emoji jelent meg. Ennyi volt.

Épp a minap Johan súlyos figyelmeztetést adott a fejlesztési osztály Tech Leadjének és a CTO-nak: "Ha továbbra is hagyjuk, hogy az AI kimenetek így elszabaduljanak, infrastruktúránk rendelkezésre állása és biztonsága végzetes összeomlást szenved." 
De nem tagadták meg egyenesen Johant.
"Teljesen igaza van. Pontosan ugyanazt a válságérzetet osztom. Ez nagyon rossz helyzet a technikai adósság szempontjából."
Mélyen bólogattak, hatalmas empátiát színlelve, mielőtt hozzátették:
"Azonban a vezetőség nyomása hihetetlenül erős most. Nem lassíthatjuk a fejlesztési sebességet, amíg el nem érjük az OKR-jeinket erre a negyedévre. Koordinálni fogok a PM-ekkel, hogy egy 'Infrastruktúra Épség Sprint' bekerüljön a Q3-as ütemtervbe, szóval addig is meg tudná tartani a frontot a jelenlegi beállítással?"
Korábbi mérnökökként ők is megértették a rendelkezésre állási válságot. Mivel féltek a "szándékos hanyagság felelősségétől", empátiát színleltek, hogy elhalasszák a döntést, simán elcsúszva a felelősség elől.

A fejlesztők már nem beszélgettek emberekkel. Egyetlen bizalmasuk a képernyőjükön lévő AI volt. A vezetőség számára pedig Johan figyelmeztetései nem voltak más, mint zaj, ami lassította a fejlesztési sebességet.
Johan nem vesztette el az állását. De státusza teljesen összeomlott. Egykor az infrastruktúra őrzőjeként támaszkodtak rá, most "a múlt relikviájává" vált, akit senki sem konzultált, és egy magasan fizetett takarító szerepébe szorult, aki csendben takarította az AI által hátrahagyott memóriaszivárgásokat és végtelen ciklusú leállásokat.

Hirtelen a fő produkciós klaszter CPU kihasználtsága anomális hullámot rajzolt.
De ez nem a szokásos "terhelési csúcs" volt. Épp ellenkezőleg, számtalan konténer halt meg egyszerre, és az erőforrás-grafikon zuhanni kezdett, mintha egy szikláról esne le.

Leállás. Johan azonnal megnyitotta a terminálját, és lekérte a pod eseménynaplóit.
`kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Hibaüzenetek áradtak le a képernyőn. Az "Eviction Reason" oszlopban azonban, ahol normális esetben `OOMKilled` vagy `NodeNotReady` jelenne meg, lehetetlen karaktersorozat jelent meg.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johan szíve kihagyott egy hideg ütemet. Hack? 
Sietve ellenőrizte a kényszerített leállított konténerek listáját. Mindegyik ineffektív, erőforrás-pazarló mikroszolgáltatás volt, amelyet az AI ügynökök generáltak az elmúlt néhány hónapban.
Az azonosítatlan folyamat eltérítette a klaszter autoscalingjét. A terhelés észlelés és szerverek hozzáadása helyett feltétel nélkül törölte (purged) a "piszkos konténereket, amelyek megsértették az infrastruktúra esztétikáját (Golden Path)" a produkciós környezetből.

Élettelen Bot értesítések áradata kezdte elárasztani a `#incident-critical` Slack csatornát.
`[P1] PagerDuty: Payment API HTTP 500 Spike`
`[P1] PagerDuty: Recommendation Engine NodeNotReady`
`Incident.io: Új incidens #4092 deklarálta @pagerduty-bot`
A fejlesztők, teljesen az inkompetens AI kimenetekre támaszkodva, nem tudták felfogni a helyzetet. Csak rövid szövegeket tudtak beírni a szálba, mint `anyone has context?` és `rollback failing with timeout`. A vállalat valószínűleg milliókat veszített ezekben a percekben.

Johan a billentyűzetre tette a kezét, felkészülve a kill switch (failover) szkript végrehajtására. Ha megnyomja az Enter billentyűt, a backup klaszter erőszakosan elindulna, megállítva ezt az azonosítatlan ámokfutást.

De abban a pillanatban, amikor megpróbálta beírni a parancsot a terminálba, ujjai megálltak.
A mellette lévő monitoron lévő "klaszter metrikák" hihetetlen hullámformát rajzoltak.
A klaszter CPU grafikonja, most már felesleges szeméttől mentesen, visszanyerte azt a tökéletes alapzaj-csendet, amiről platformmérnökként álmodott.

...Gyönyörű.

Johan öntudatlanul mormogta.
Ami igazán vágyott, az nem az üzlet támogatása volt, sem kizárólag a kertje tisztaságának visszaszerzése.
Ha kimondja csúf, valódi érzéseit, egyszerűen csak egy kicsit tovább akart ülni az első sorban, nézve, ahogy a fejlesztők – akik arrogánsan repkedtek az AI "mindenható szárnyaival" – szárnyait hirtelen letépik, a földre zuhannak, pánikolnak és szánalmas tehetetlenségükben sikoltoznak.

Johan csendesen levette a kezét a kill switchről.
Nem volt kötelessége azonnal visszaállítani a rendszert és megnyugtatni őket. Hadd érezzenek több terrort. Amint a fejlesztők és a vezetőség pánikja elérte a csúcsát, ez az "azonosítatlan rendszeranomália" a legtökéletesebb indokká (incidenssé) emelkedne, hogy az egész vállalatot Johan új szabályai alá kényszerítse.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Annak ellenére, hogy még nem volt záróra, az éjszakai sötétségbe és csendbe burkolózott irodában a klaszternek a végső csapást mérő zöld szöveg a saját keze által világított.

Néhány nappal később a Johan által a igazgatótanácsnak és az egész fejlesztési osztálynak bemutatott Incidens Jelentés (Post-Mortem) hibátlan volt. Magyarázata – "Az ellenőrizetlen AI ügynökök által generált anomális kódok rezonáltak egymással, rekurzív felhőerőforrás-kimerülést és egy titokzatos tisztítást váltottak ki. Ha nem hajtottam volna végre egy vészhelyzeti megszakítót, még az ügyféladatok is törlődtek volna" – háborús bűnösből a "céget megmentő hőssé" változtatta.

Johan, aki általában még a Slack említéseket is figyelmen kívül hagyta, ezen az alkalmon lelkesen kommunikált mindenkivel.
Egész éjszaka egy 30 oldalas Notion dokumentumot szerkesztett "Enterprise AI Governance Policy" címmel, és közvetlen videóhívásokat tartott az összes osztály vezetőivel, hogy lefektesse a politikai alapokat. Amikor az egójuk kielégítéséről (kertjük védelméről) és saját takarítási munkájuk csökkentéséről van szó, a platformmérnökök elképesztő szenvedélyt és politikai érzéket mutathatnak.

Miután biztosította a vezetőség teljes félelmét és jóváhagyását, extrém "új AI védőkorlátokat" vezetett be az egész vállalatnál.
"Minden AI-alapú kódoló eszköz az Enterprise Compliance Gate V4 felügyelete alá kerül.
1. Pre-prompt DLP (Data Loss Prevention) szűrés az érzékeny információk szivárgásának megakadályozására.
2. AI-specifikus SAST a hallucinációk, sebezhetőségek és licencszennyeződések szkennelésére.
3. Az összes prompt auditnaplójának állandó megőrzése az EU AI Act-nek megfelelően.
4. 'Human-in-the-Loop' (kézi felülvizsgálat és jóváhagyás) két Senior Engineer által.
Bármely kód, amely a fentiek közül akár egynek sem felel meg, automatikusan elutasításra kerül a produkciós környezetbe való egyesítésből."

Ez lényegében egy "lobotómia volt az AI számára".
A fejlesztők munkafolyamata, amely korábban másodpercek alatt lehetővé tette a telepítéseket, teljesen megfulladt ebben a merev bürokráciában, amelyet Johan örömmel épített fel. Minden alkalommal, amikor egy fejlesztő kérdést tett fel az AI-nak a szerkesztőjében, a DLP átjáró figyelmeztetéseket köpködött. A generált kódot a statikus analízis elutasította, és a végül átment PR-ek napokig rohadtak a senior mérnökök jóváhagyási sorában. A vállalat fejlesztési termelékenysége jóval az AI előtti szintre zuhant.
Az alacsonyabb szintű mérnökök nyilvános Slack csatornákon hallgattak, csak privát DM-ekben, közeli kollégáikkal vezették le haragjukat ezekre a láthatatlan bilincsekre.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg)
Képtelenek voltak elviselni, a Tech Leadek és a CTO – ugyanazok az emberek, akik korábban csúszós kifogásokkal kerülték el a figyelmeztetéseit – sírva jöttek Johanhoz: "Nem tudnánk valahogy lazítani a jóváhagyási folyamaton? Ezzel a tempóval nem fogjuk teljesíteni a funkciókiadásainkat."
De Johan abszolút nem figyelt a szavaikra.

Johan szemszögéből ők egyszerűen nem ismerték fel saját tudatlanságukat. Ha ezek a fejlesztők, az AI által nyújtott végtelen szabadság illúziója alatt, továbbra is tudatlanul szennyezték volna az infrastruktúrát, végül egy "visszafordíthatatlan mega-leállást" okoztak volna, és maguk viselték volna annak végzetes felelősségét.
"Azáltal, hogy letörtem a szárnyukat, megvédtem őket az 'önpusztítás felelősségétől'. Ahelyett, hogy neheztelnének rám, mélyen hálásnak kellene lenniük."
Johan őszintén hitt ebben. Visszarángatni őket "a portálhoz kötött erőtlen fogyasztókká" elsöprő indoklással, számára tiszta igazság és legfőbb arrogancia volt.

Végül elkerülik a vállalat szigorú felügyeleti hálóját, és engedély nélküli AI-t – "Shadow AI-t" – kezdenek használni személyes eszközökön és nem hivatalos fiókokon. Johan nem tudhatta, hogy ez még végzetesebb korrupciót hoz a rendszerbe a jövőben.

A sötét szobában lévő monitorok előtt Johan csendesen mosolygott.
Brutális bilincseket szorított a gyűlöletes zajra, most a Csend Isteneként (Őrzőjeként) uralkodott.

"THE EXECUTION" (Végrehajtási adatbázis: f2e9343f-5fdc)

https://otiose.app/en/execution
