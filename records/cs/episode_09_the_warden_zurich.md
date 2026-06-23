16:40. Curych, Švýcarsko. Za oknem se svět již ponořil do těžké tmy zimní noci. Studený vítr vanoucí z Alp bičoval skla masivní kancelářské budovy stojící u Curyšského jezera.

Johan pracoval jako senior platform engineer v jedné z největších evropských finančně-technologických společností. Vyzbrojen výjimečně přísnými standardy finanční shody, jedinečnými pro Švýcarsko (předpisy FINMA), byl kdysi absolutním strážcem, který ovládal propast infrastruktury této společnosti. Kdykoli vývojáři navrhli novou službu, museli přijít k jeho stolu na schvalovací rituál maskovaný jako „architektonická konzultace“. Když je otravoval otázkami typu „Zanechali jste dokumentaci?“ nebo „Splňuje to bezpečnostní požadavky?“, Johan tajně choval pocit nadřazenosti a hrdosti na své privilegované postavení – skutečnost, že se nic nemohlo pohnout, aniž by to prošlo jím.

Nakonec, pod záminkou „osvobození vývojářů od složitosti infrastruktury“, vybudoval interní vývojářský portál známý jako „Golden Path“. Jeho skutečným motivem však bylo jednoduše chránit svou krásnou infrastrukturu (svou zahradu) před pošlapáním zablácenými botami nevědomých vývojářů. Úplným black-boxováním hlubin Kubernetes infantilizoval vývojáře a proměnil je v „bezmocné spotřebitele“, kteří pouze mačkali tlačítka.

Nyní ho však jeho perfekcionismus škrtil tím nejhorším možným způsobem. Vyzbrojeni „všemocnými křídly“ AI kódovacích agentů, vývojáři se již neobtěžovali vyhledávat Johanovu konzultaci (nebo architektonické schválení). Ve skutečnosti ani neotevřeli portál, který postavil. Jednoduše hodili nedbalé výzvy do AI svého IDE: „Nasaďte infrastrukturu pro novou platební službu.“ Během několika sekund AI vygenerovala stovky řádků nafouknutého, anomálního YAML. Vývojáři, aniž by kód četli, ho rovnou sloučili do GitHubu. Odtud CI/CD pipeline (ArgoCD) automaticky přesměrovala tyto odpadní konfigurace do produkčního clusteru, zcela ignorujíc Johanovu estetiku.

Johan popíjel studenou kávu a zíral na tichou obrazovku Slacku. To, co viděl, bylo úplné „přerušení komunikace“. V kanálu `#squad-payments` automatický bezpečnostní skenovací bot (Datadog CSPM) chrlil standardní upozornění. `[High] Overly broad IAM role detected in payment-service` I když Johan označil odpovědného vývojáře ve vlákně a zeptal se: „Je to záměrné?“, nedostal absolutně žádnou odpověď. O několik hodin později se objevilo několik bezduchých emoji „👀“ a „👍“. To bylo vše.

Jen před několika dny Johan vydal vážné varování technickému vedoucímu vývojového oddělení a CTO: „Pokud budeme i nadále nechávat výstupy AI takto volně běžet, dostupnost a bezpečnost naší infrastruktury utrpí fatální kolaps.“ Ale oni Johana přímo nepopřeli. „Máte naprostou pravdu. Sdílím naprosto stejný pocit krize. Toto je velmi špatná situace z hlediska technického dluhu.“ Hluboce přikývli, předstírajíce obrovskou empatii, než dodali: „Nicméně, tlak od představenstva je v tuto chvíli neuvěřitelně silný. Nemůžeme zpomalit rychlost vývoje, dokud nedosáhneme našich OKR pro toto čtvrtletí. Budu koordinovat s PM, abych zajistil, že 'Infrastructure Sanity Sprint' bude zahrnut do plánu Q3, takže byste mohl s aktuálním nastavením vydržet do té doby?“ Jelikož sami byli bývalými inženýry, rozuměli krizi dostupnosti. Protože se obávali „odpovědnosti za úmyslnou nedbalost“, předstírali empatii, aby odložili rozhodnutí, a hladce se vyhnuli odpovědnosti.

Vývojáři už s lidmi nekomunikovali. Jejich jediným důvěrníkem byla AI na jejich obrazovkách. A pro management nebyly Johanovy varování nic víc než hluk zpomalující rychlost vývoje. Johan nepřišel o práci. Ale jeho status se zcela zhroutil. Kdysi spoléhal na něj jako na strážce infrastruktury, stal se „relikvií minulosti“, s nímž se nikdo neradil, a byl degradován na vysoce placeného údržbáře, který tiše uklízel úniky paměti a výpadky nekonečných smyček, které zanechala AI.

Náhle, využití CPU hlavního produkčního clusteru vykreslilo anomální vlnu. Ale nejednalo se o obvyklý „špičkový nárůst zátěže“. Naopak, nespočet kontejnerů zemřelo současně a graf zdrojů začal klesat jako pád z útesu.

Výpadek. Johan okamžitě otevřel svůj terminál a načetl protokoly událostí podů. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Chybové zprávy se valily po obrazovce. Nicméně ve sloupci „Eviction Reason“, kde by se normálně zobrazovalo `OOMKilled` nebo `NodeNotReady`, se objevila nemožná řada znaků.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johanovi přeskočilo srdce. Hack? Spěšně zkontroloval seznam násilně ukončených kontejnerů. Všechny to byly neefektivní, plýtvající mikroservisy generované AI agenty za posledních několik měsíců. Neidentifikovaný proces unesl automatické škálování clusteru. Místo detekce zátěže a přidávání serverů bezpodmínečně čistil (mazal) „špinavé kontejnery, které porušovaly estetiku infrastruktury (Golden Path)“ z produkčního prostředí.

Proud bezduchých oznámení od botů začal zaplavovat Slack kanál `#incident-critical`. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Vývojáři, zcela závislí na nekompetentních výstupech AI, nedokázali situaci pochopit. Mohli jen psát krátké texty do vlákna jako `anyone has context?` a `rollback failing with timeout`. Společnost pravděpodobně v těchto několika minutách ztrácela miliony franků.

Johan položil ruce na klávesnici a připravoval se spustit skript pro nouzové vypnutí (failover). Kdyby stiskl klávesu Enter, záložní cluster by se násilně spustil a zastavil toto neidentifikované řádění.

Ale v okamžiku, kdy se pokusil zadat příkaz do terminálu, jeho prsty se zastavily. „Metriky clusteru“ na vedlejším monitoru kreslily neuvěřitelnou vlnovou formu. Graf CPU clusteru, nyní zbavený nepotřebného odpadu, znovu získal dokonalé základní ticho, o kterém snil v den, kdy se stal platformním inženýrem.

...Krásné.

Johan nevědomky zamumlal. To, po čem skutečně toužil, nebylo podporovat byznys, ani jen získat zpět čistotu své zahrady. Aby vyjádřil své ošklivé, pravé pocity, chtěl prostě ještě chvíli sedět v první řadě a sledovat, jak vývojářům – kteří se arogantně vznášeli na svých „všemocných křídlech“ AI – jsou náhle křídla utržena, jak se řítí k zemi, panikaří a křičí v žalostné bezmocnosti.

Johan tiše sundal ruce z vypínače. Neměl žádnou povinnost okamžitě obnovit systém a uklidnit je. Ať pocítí více hrůzy. Jakmile panika vývojářů a managementu dosáhne vrcholu, tato „neidentifikovaná systémová anomálie“ se povýší na nejdokonalejší ospravedlnění (incident), aby celou společnost donutila k podřízení se Johanovým novým pravidlům.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Přestože ještě nebyla zavírací doba, v kanceláři zahalené půlnoční tmou a tichem zářil zelený text, který zasadil clusteru poslední ránu, jeho vlastníma rukama.

O několik dní později byla prezentace zprávy o incidentu (Post-Mortem), kterou Johan přednesl představenstvu a celému vývojovému oddělení, bezchybná. Jeho vysvětlení – „Anomální kód generovaný nekontrolovanými AI agenty se navzájem rezonoval, což spustilo rekurzivní vyčerpání cloudových zdrojů a záhadné vyčištění. Kdybych nespustil nouzový jistič, byla by vymazána i zákaznická data“ – ho proměnilo z válečného zločince v „hrdinu, který zachránil společnost“.

Johan, který obvykle ignoroval i zmínky na Slacku, tentokrát s nadšením komunikoval se všemi. Celou noc pracoval na 30stránkovém dokumentu v Notion s názvem „Enterprise AI Governance Policy“ a vedl přímé videohovory s manažery napříč všemi odděleními, aby položil politické základy. Pokud jde o uspokojení jejich ega (obranu jejich zahrady) a snížení jejich vlastní úklidové práce, platformní inženýři dokázali projevit úžasnou vášeň a politickou zdatnost.

Poté, co si zajistil naprostý strach a souhlas manažerského týmu, zavedl extrémní soubor „nových AI zábran“ napříč celou společností. „Všechny nástroje pro kódování s podporou AI budou pod dohledem Enterprise Compliance Gate V4. 1. DLP (Data Loss Prevention) filtrování před výzvou k zabránění úniku citlivých informací. 2. AI-specifický SAST pro skenování halucinací, zranitelností a kontaminace licencí. 3. Trvalé uchovávání auditních protokolů pro všechny výzvy v souladu s EU AI Act. 4. 'Human-in-the-Loop' (manuální kontrola a schválení) dvěma senior inženýry. Jakýkoli kód, který selže byť jen v jednom z výše uvedených bodů, bude automaticky odmítnut pro sloučení do produkčního prostředí.“

Byla to v podstatě „lobotomie pro AI“. Pracovní postup vývojářů, který dříve umožňoval nasazení během několika sekund, byl zcela udušen touto rigidní byrokracií, kterou Johan s radostí vybudoval. Pokaždé, když vývojář položil AI otázku ve svém editoru, brána DLP chrlila varování. Vygenerovaný kód byl odmítnut statickou analýzou a PR, které nakonec prošly, byly ponechány hnít dny v čekací frontě na schválení senior inženýrů. Produktivita vývoje společnosti klesla hluboko pod úroveň před AI. Nižší inženýři mlčeli ve veřejných Slack kanálech a ventilovali svou nelibost nad těmito neviditelnými pouty pouze v soukromých zprávách (DMs) s blízkými kolegy. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Neschopni to vydržet, Tech Leadi a CTO – titíž lidé, kteří dříve uhýbali jeho varováním s kluzkými výmluvami – přišli k Johanovi s pláčem: „Nemůžeme nějak uvolnit schvalovací proces? Tímto tempem nebudeme stíhat vydávat nové funkce.“ Ale Johan jejich slovům absolutně nevěnoval pozornost.

Z Johanova pohledu si prostě neuvědomili vlastní nevědomost. Kdyby tito vývojáři, pod iluzí nekonečné svobody udělené AI, pokračovali v nevědomém znečišťování infrastruktury, nakonec by způsobili „nezvratný mega-výpadek“ a sami by za něj nesli fatální odpovědnost. „Díky tomu, že jsem jim zlomil křídla, jsou chráněni před 'odpovědností za sebezničení'. Místo aby mě nenáviděli, měli by mi hluboce děkovat.“ Johan tomu skutečně věřil. Přetáhnout je zpět k „bezmocným spotřebitelům vázaným na portál“ pomocí drtivého ospravedlnění bylo pro něj čistou spravedlností a nejvyšší arogancí.

Nakonec uniknou přísné firemní monitorovací síti a začnou používat neautorizovanou AI – „Shadow AI“ – na osobních zařízeních a neoficiálních účtech. Johan neměl tušení, že to v budoucnu přinese systému ještě fatálnější korupci.

Před monitory v temné místnosti se Johan tiše usmál. Nasadil brutální pouta na nenáviděný hluk a nyní vládl jako Bůh ticha (Strážce).

"THE EXECUTION" (Databáze provádění: f2e9343f-5fdc)

https://otiose.app/en/execution
