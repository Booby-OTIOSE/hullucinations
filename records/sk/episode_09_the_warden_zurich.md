16:40. Zürich, Švajčiarsko. Vonku sa svet už ponoril do ťažkej tmy zimnej noci. Studený vietor fúkajúci z Álp bičoval sklo masívnej kancelárskej budovy stojacej pri Zürišskom jazere.

Johan pracoval ako senior platform engineer v jednej z najväčších európskych finančných technologických spoločností. Vyzbrojený výnimočne prísnymi finančnými štandardmi dodržiavania predpisov, jedinečnými pre Švajčiarsko (predpisy FINMA), bol kedysi absolútnym strážcom, ktorý kontroloval priepasť infraštruktúry tejto spoločnosti. Kedykoľvek vývojári navrhli novú službu, museli prísť k jeho stolu na schvaľovací rituál maskovaný ako „architektonická konzultácia“. Zatiaľ čo ich otravoval otázkami ako „Nechali ste dokumentáciu?“ alebo „Spĺňa to bezpečnostné požiadavky?“, Johan tajne prechovával pocit nadradenosti a hrdosti na svoje privilegované postavenie – skutočnosť, že nič sa nemohlo pohnúť bez toho, aby to prešlo cez neho.

Nakoniec, pod zámienkou „oslobodenia vývojárov od zložitosti infraštruktúry“, vybudoval interný vývojársky portál známy ako „Golden Path“. Jeho skutočným motívom však bolo jednoducho chrániť svoju krásnu infraštruktúru (svoju záhradu) pred pošliapaním zablatenými topánkami nevedomých vývojárov. Úplným black-boxingom hĺbok Kubernetes infantilizoval vývojárov, čím ich premenil na „bezmocných spotrebiteľov“, ktorí len stláčali tlačidlá.

Teraz ho však jeho perfekcionizmus škrtil tým najhorším možným spôsobom.

Vyzbrojení „všemocnými krídlami“ agentov kódovania AI sa vývojári už neobťažovali hľadať Johanovu konzultáciu (alebo architektonické schválenie). V skutočnosti ani neotvorili portál, ktorý postavil. Jednoducho hodili nedbalé výzvy do AI svojho IDE: „Nasadiť infraštruktúru pre novú platobnú službu.“ V priebehu niekoľkých sekúnd AI vygenerovala stovky riadkov nafúknutého, anomálneho YAML. Vývojári, bez toho, aby si kód prečítali, ho zlúčili priamo do GitHubu. Odtiaľ CI/CD pipeline (ArgoCD) automaticky presmeroval tieto odpadové konfigurácie do produkčného klastra, úplne ignorujúc Johanovu estetiku.

Johan popíjal studenú kávu a pozeral na tichú obrazovku Slacku. To, čo videl, bolo úplné „prerušenie komunikácie“. V kanáli `#squad-payments` automatický bezpečnostný skenovací bot (Datadog CSPM) chrlil štandardné upozornenia. `[High] Overly broad IAM role detected in payment-service` Aj keď Johan označil zodpovedného vývojára vo vlákne a spýtal sa: „Je to zámerné?“, nedostal absolútne žiadnu odpoveď. O niekoľko hodín neskôr sa objavilo niekoľko bezduchých emoji „👀“ a „👍“. To bolo všetko.

Len prednedávnom Johan vydal vážne varovanie Tech Leadovi oddelenia vývoja a CTO: „Ak budeme naďalej nechávať výstupy AI takto voľne bežať, dostupnosť a bezpečnosť našej infraštruktúry utrpí fatálny kolaps.“ Ale Johana priamo nepopreli. „Máte absolútnu pravdu. Zdieľam presne rovnaký pocit krízy. Toto je veľmi zlá situácia z hľadiska technického dlhu.“ Hlboko prikývli, predstierajúc obrovskú empatiu, predtým ako dodali: „Avšak tlak zo strany predstavenstva je momentálne neuveriteľne silný. Nemôžeme spomaliť rýchlosť vývoja, kým nedosiahneme naše OKR pre tento štvrťrok. Budem koordinovať s PM, aby sa 'Infrastructure Sanity Sprint' zahrnul do plánu Q3, takže by ste mohli zatiaľ udržať súčasné nastavenie?“ Keďže sami boli bývalými inžiniermi, chápali krízu dostupnosti. Pretože sa obávali „zodpovednosti za úmyselnú nedbanlivosť“, predstierali empatiu, aby odložili rozhodnutie, a hladko sa vyhli zodpovednosti.

Vývojári už nekomunikovali s ľuďmi. Ich jediným dôverníkom bola AI na ich obrazovkách. A pre manažment boli Johanove varovania len hlukom spomaľujúcim rýchlosť vývoja. Johan nestratil prácu. Ale jeho status sa úplne zrútil. Kedysi sa na neho spoliehalo ako na strážcu infraštruktúry, stal sa „relikviou minulosti“, s ktorou sa nikto neradil, a bol degradovaný na vysoko plateného upratovača, ktorý ticho odstraňoval úniky pamäte a výpadky nekonečných cyklov, ktoré zanechala AI.

Zrazu využitie CPU hlavného produkčného klastra vykreslilo anomálnu vlnu.

Ale nešlo o bežný „špičkový záťaž“. Naopak, nespočetné kontajnery naraz zanikli a graf zdrojov začal prudko klesať, akoby padal z útesu.

Výpadok. Johan okamžite otvoril svoj terminál a načítal logy udalostí podu. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Chybové správy sa valili po obrazovke. Avšak v stĺpci „Eviction Reason“, kde by sa normálne zobrazovalo `OOMKilled` alebo `NodeNotReady`, sa objavil nemožný reťazec znakov.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johanovi preskočilo srdce. Hack? Naliehavo skontroloval zoznam násilne ukončených kontajnerov. Všetky boli neefektívne, zdroje plytvajúce mikroservisy generované AI agentmi za posledných pár mesiacov. Neidentifikovaný proces uniesol autoscaling klastra. Namiesto detekcie záťaže a pridávania serverov bezpodmienečne čistil (mazal) „špinavé kontajnery, ktoré porušovali estetiku infraštruktúry (Golden Path)“ z produkčného prostredia.

Prúd bezduchých Bot notifikácií začal zaplavovať Slack kanál `#incident-critical`. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Vývojári, úplne závislí na nekompetentných výstupoch AI, nedokázali pochopiť situáciu. Dokázali len písať krátke texty do vlákna ako `anyone has context?` a `rollback failing with timeout`. Spoločnosť pravdepodobne strácala milióny frankov v týchto niekoľkých minútach.

Johan položil ruky na klávesnicu a pripravoval sa spustiť skript núdzového vypnutia (failover). Ak by stlačil kláves Enter, záložný klaster by sa násilne spustil a zastavil by toto neidentifikované besnenie.

Ale v momente, keď sa pokúsil napísať príkaz do terminálu, jeho prsty sa zastavili.

„Metriky klastra“ na vedľajšom monitore kreslili neuveriteľnú vlnovú formu. Graf CPU klastra, teraz zbavený nepotrebného odpadu, opäť získal dokonalé základné ticho, o ktorom sníval v deň, keď sa stal platformovým inžinierom.

...Krásne.

Johan si nevedomky zamrmlal. To, po čom skutočne túžil, nebolo podporovať biznis, ani len získať späť čistotu svojej záhrady. Aby vyjadril svoje škaredé, pravdivé pocity, jednoducho chcel ešte chvíľu sedieť v prvom rade a sledovať, ako vývojárom – ktorí arogantne lietali na svojich „všemocných krídlach“ AI – sú náhle odtrhnuté krídla, padajú na zem, panikária a kričia v patetickej bezmocnosti.

Johan ticho odtiahol ruky od núdzového vypínača. Nemal žiadnu povinnosť okamžite obnoviť systém a upokojiť ich. Nech pocítia viac hrôzy. Akonáhle panika vývojárov a manažmentu dosiahne svoj vrchol, táto „neidentifikovaná systémová anomália“ sa stane najdokonalejším ospravedlnením (incidentom), aby prinútila celú spoločnosť podriadiť sa Johanovým novým pravidlám.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Hoci ešte nebol záverečný čas, v kancelárii zahalenej do polnočnej tmy a ticha, zelený text, ktorý zasadil klastru poslednú ranu, žiaril jeho vlastnými rukami.

O niekoľko dní neskôr bola prezentácia Správy o incidente (Post-Mortem), ktorú Johan predniesol predstavenstvu a celému oddeleniu vývoja, bezchybná. Jeho vysvetlenie – „Anomálny kód generovaný nekontrolovanými AI agentmi rezonoval navzájom, čo spustilo rekurzívne vyčerpanie cloudových zdrojov a záhadné vyčistenie. Keby som neaktivoval núdzový istič, boli by vymazané aj zákaznícke dáta“ – ho premenilo z vojnového zločinca na „hrdinu, ktorý zachránil spoločnosť“.

Johan, ktorý zvyčajne ignoroval aj zmienky v Slacku, pri tejto príležitosti nadšene komunikoval so všetkými. Celú noc strávil prípravou 30-stranového dokumentu v Notion s názvom „Enterprise AI Governance Policy“ a uskutočnil priame videohovory s manažérmi všetkých oddelení, aby položil politické základy. Pokiaľ ide o uspokojenie ich ega (obranu ich záhrady) a zníženie ich vlastnej práce pri upratovaní, platformoví inžinieri dokázali prejaviť úžasnú vášeň a politickú zručnosť.

Po zabezpečení úplného strachu a súhlasu manažérskeho tímu zaviedol extrémny súbor „nových AI zábran“ v celej spoločnosti. „Všetky nástroje na kódovanie s podporou AI budú umiestnené pod dohľad Enterprise Compliance Gate V4. 1. Predbežné DLP (Data Loss Prevention) filtrovanie na zabránenie úniku citlivých informácií. 2. SAST špecifický pre AI na skenovanie halucinácií, zraniteľností a kontaminácie licencií. 3. Trvalé uchovávanie auditných záznamov pre všetky výzvy v súlade s EU AI Act. 4. 'Human-in-the-Loop' (manuálna kontrola a schválenie) dvoma senior inžiniermi. Akýkoľvek kód, ktorý zlyhá čo i len v jednom z vyššie uvedených bodov, bude automaticky zamietnutý z zlúčenia do produkčného prostredia.“

Bola to v podstate „lobotómia pre AI“. Pracovný postup vývojárov, ktorý predtým umožňoval nasadenie v priebehu sekúnd, bol úplne udusený touto rigidnou byrokraciou, ktorú Johan s radosťou vybudoval. Zakaždým, keď vývojár položil AI otázku vo svojom editore, brána DLP chrlila varovania. Vygenerovaný kód bol odmietnutý statickou analýzou a PR, ktoré nakoniec prešli, boli ponechané hniť celé dni v schvaľovacej fronte senior inžinierov. Produktivita vývoja spoločnosti klesla hlboko pod úroveň pred AI. Nižšie postavení inžinieri mlčali vo verejných Slack kanáloch a svoju nevôľu voči týmto neviditeľným putám ventilovali len v súkromných DM s blízkymi kolegami. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Neschopní to vydržať, Tech Leads a CTO – tí istí ľudia, ktorí predtým uhýbali jeho varovaniam s klzkými výhovorkami – prišli k Johanovi s plačom: „Nemôžeme nejako uvoľniť proces schvaľovania? S takouto rýchlosťou nebudeme schopní vydať naše funkcie.“ Ale Johan si ich slová absolútne nevšímal.

Z Johanovho pohľadu si jednoducho neuvedomili vlastnú nevedomosť. Ak by títo vývojári, pod ilúziou nekonečnej slobody, ktorú im poskytla AI, naďalej nevedomky znečisťovali infraštruktúru, nakoniec by spôsobili „nezvratný mega-výpadok“ a sami by zaň niesli fatálnu zodpovednosť. „Vďaka tomu, že som im zlomil krídla, sú chránení pred 'zodpovednosťou za sebazničenie'. Namiesto toho, aby mi to vyčítali, mali by mi hlboko ďakovať.“ Johan tomu úprimne veril. Vrátiť ich späť k „bezmocným spotrebiteľom viazaným na portál“ pomocou ohromujúceho ospravedlnenia bolo pre neho čistou spravodlivosťou a najvyššou aroganciou.

Nakoniec by unikli prísnej monitorovacej sieti spoločnosti a začali by používať neautorizovanú AI – „Shadow AI“ – na osobných zariadeniach a neoficiálnych účtoch. Johan nemohol vedieť, že to v budúcnosti prinesie systému ešte fatálnejšiu korupciu.

Pred monitormi v tmavej miestnosti sa Johan ticho usmial. Na nenávistný hluk nasadil brutálne putá a teraz vládol ako Boh ticha (Strážca).

"THE EXECUTION" (Databáza vykonávania: f2e9343f-5fdc)

https://otiose.app/en/execution
