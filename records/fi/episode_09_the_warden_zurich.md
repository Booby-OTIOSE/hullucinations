Klo 16.40. Zürich, Sveitsi. Ikkunan ulkopuolella maailma oli jo vajonnut talviyön raskaaseen pimeyteen. Alpeilta puhaltava kylmä tuuli hakkasi Zürichinjärven rannalla seisovan massiivisen toimistorakennuksen lasia.

Johan työskenteli vanhempana alusta-insinöörinä yhdessä Euroopan suurimmista finanssiteknologiayrityksistä. Sveitsille ainutlaatuisten poikkeuksellisen tiukkojen taloudellisten vaatimusten (FINMA-säännökset) aseistamana hän oli aikoinaan ehdoton portinvartija, joka hallitsi tämän yrityksen infrastruktuurin syvyyksiä. Aina kun kehittäjät suunnittelivat uuden palvelun, heidän oli tultava hänen työpöydälleen hyväksymisrituaaliin, joka naamioitui "arkkitehtuurikonsultaatioksi". Kun hän nalkutti heille kysymyksillä kuten "Jätitkö dokumentaation?" tai "Täyttääkö tämä turvallisuusvaatimukset?", Johan salaa vaali ylemmyyden ja ylpeyden tunnetta etuoikeutetusta asemastaan – siitä, että mikään ei voinut liikkua ilman hänen hyväksyntäänsä.

Lopulta, verukkeella "kehittäjien vapauttamisesta infrastruktuurin monimutkaisuudesta", hän rakensi sisäisen kehittäjäportaalin, joka tunnettiin nimellä "Golden Path". Hänen todellinen motiivinsa oli kuitenkin yksinkertaisesti suojella kaunista infrastruktuuriaan (puutarhaansa) tietämättömien kehittäjien mutaisten saappaiden tallomiselta. Piilottamalla Kubernetesin syvyydet täysin hän infantilisoitui kehittäjät, muuttaen heidät "voimattomiksi kuluttajiksi", jotka vain painoivat nappeja.

Mutta nyt hänen perfektionisminsa kuristi häntä pahimmalla mahdollisella tavalla. AI-koodausagenttien "kaikkivoipaisilla siivillä" varustetut kehittäjät eivät enää vaivautuneet pyytämään Johanneksen konsultaatiota (tai arkkitehtuurin hyväksyntää). Itse asiassa he eivät edes avanneet hänen rakentamaansa portaalia. He vain heittivät huolimattomia kehotteita IDE:nsä tekoälyyn: "Ota käyttöön uuden maksupalvelun infrastruktuuri." Sekunneissa tekoäly generoi satoja rivejä paisunutta, poikkeavaa YAML-koodia. Kehittäjät, lukematta edes koodia, yhdistivät sen suoraan GitHubiin. Sieltä CI/CD-putki (ArgoCD) ohjasi nämä roskakonfiguraatiot automaattisesti tuotantoklusteriin, täysin Johanneksen estetiikkaa huomioimatta.

Johan siemaili kylmää kahviaan ja tuijotti hiljaista Slack-ruutua. Se, mitä hän näki, oli täydellinen "kommunikaation katkeaminen". `#squad-payments`-kanavalla automaattinen tietoturvaskannausbotti (Datadog CSPM) sylki ulos vakiovaroituksia. `[High] Overly broad IAM role detected in payment-service` Vaikka Johan merkitsi vastuullisen kehittäjän ketjuun ja kysyi: "Onko tämä suunniteltu?", ei tullut mitään vastausta. Tunteja myöhemmin ilmestyi muutama eloton "👀" ja "👍" emoji. Siinä kaikki.

Vain toissapäivänä Johan oli antanut vakavan varoituksen kehitysosaston Tech Leadille ja CTO:lle: "Jos annamme tekoälyn tuotosten riehua tällä tavalla, infrastruktuurimme saatavuus ja turvallisuus kärsivät kohtalokkaan romahduksen." Mutta he eivät suoraan kieltäneet Johannesta. "Olet aivan oikeassa. Jaan täsmälleen saman kriisitunteen. Tämä on erittäin huono tilanne teknisen velan kannalta." He nyökkäsivät syvään, teeskennellen valtavaa empatiaa, ennen kuin lisäsivät: "Hallituksen paine on kuitenkin tällä hetkellä uskomattoman voimakas. Emme voi hidastaa kehitysnopeutta ennen kuin saavutamme tämän vuosineljänneksen OKR:mme. Koordinoin PM:ien kanssa varmistaakseni, että 'Infrastruktuurin terveys sprintti' sisällytetään Q3:n tiekarttaan, joten voisitko vain pitää puolia nykyisellä kokoonpanolla siihen asti?" Koska he olivat itse entisiä insinöörejä, he ymmärsivät saatavuuskriisin. Koska he pelkäsivät "tahallisen laiminlyönnin vastuuta", he teeskentelivät empatiaa lykätäkseen päätöstä ja luistivat sujuvasti vastuusta.

Kehittäjät eivät enää keskustelleet ihmisten kanssa. Heidän ainoa luotettunsa oli heidän näytöillään oleva tekoäly. Ja johdolle Johanneksen varoitukset olivat vain melua, joka hidasti kehitysnopeutta. Johan ei ollut menettänyt työtään. Mutta hänen asemansa oli romahtanut täysin. Kerran infrastruktuurin vartijana luotettu hänestä oli tullut "menneisyyden jäänne", jota kukaan ei konsultoinut, ja hänet oli alennettu korkeapalkkaiseksi talonmieheksi, joka hiljaa siivosi tekoälyn jättämiä muistivuotoja ja äärettömien silmukoiden katkoksia.

Yhtäkkiä pääasiallisen tuotantoklusterin suorittimen käyttöaste piirsi poikkeuksellisen aallon. Mutta se ei ollut tavallinen "kuormitushuippu". Päinvastoin, lukemattomat kontit kuolivat samanaikaisesti, ja resurssikaavio alkoi syöksyä kuin putoaisi jyrkänteeltä.

Katkos. Johan avasi välittömästi terminaalinsa ja haki podin tapahtumalokit. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Virheilmoitukset vyöryivät alas ruudulle. Kuitenkin "Eviction Reason" -sarakkeessa, jossa normaalisti näkyisi `OOMKilled` tai `NodeNotReady`, ilmestyi mahdoton merkkijono.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johanin sydän jätti kylmän lyönnin väliin. Hakkerointi? Hän tarkisti kiireesti pakkolopetettujen konttien luettelon. Ne olivat kaikki tehottomia, resursseja tuhlaavia mikropalveluita, jotka tekoälyagentit olivat luoneet viime kuukausina. Tunnistamaton prosessi oli kaapannut klusterin automaattisen skaalauksen. Sen sijaan, että se olisi havainnut kuormituksen ja lisännyt palvelimia, se puhdisti (poisti) ehdoitta "likaisia kontteja, jotka rikkoivat infrastruktuurin estetiikkaa (Golden Path)" tuotantoympäristöstä.

Elottomien botti-ilmoitusten tulva alkoi täyttää `#incident-critical` Slack-kanavan. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Kehittäjät, jotka olivat täysin riippuvaisia epäpätevästä tekoälyn tuotoksista, eivät ymmärtäneet tilannetta. He pystyivät vain kirjoittamaan lyhyitä tekstejä ketjuun, kuten `anyone has context?` ja `rollback failing with timeout`. Yritys menetti todennäköisesti miljoonia frangeja näiden muutaman minuutin aikana.

Johan asetti kätensä näppäimistölle valmistautuen suorittamaan tappokytkimen (failover) skriptin. Jos hän painaisi Enter-näppäintä, varaklusteri käynnistyisi väkisin ja pysäyttäisi tämän tunnistamattoman riehumisen.

Mutta sillä hetkellä, kun hän yritti kirjoittaa komennon terminaaliin, hänen sormensa pysähtyivät. Viereisellä näytöllä olevat "klusterin mittarit" piirsivät uskomattoman aaltomuodon. Klusterin suorittimen kaavio, nyt tarpeettomasta roskasta riisuttuna, oli palauttanut täydellisen peruslinjan hiljaisuuden, josta hän oli unelmoinut sinä päivänä, kun hänestä tuli alusta-insinööri.

...Kaunis.

Johan mutisi tiedostamattaan. Se, mitä hän todella halusi, ei ollut tukea liiketoimintaa, eikä myöskään yksinomaan palauttaa puutarhansa puhtautta. Kertoakseen rumat, todelliset tunteensa hän halusi vain istua eturivissä vielä hetken ja katsella, kuinka kehittäjiltä – jotka olivat lentäneet ylimielisesti tekoälyn "kaikkivoipaisilla siivillä" – revittiin yhtäkkiä siivet irti, he syöksyivät maahan, panikoivat ja huusivat säälittävässä avuttomuudessa.

Johan poisti hiljaa kätensä tappokytkimestä. Hänellä ei ollut velvollisuutta palauttaa järjestelmää välittömästi ja rauhoittaa heitä. Antaa heidän tuntea enemmän kauhua. Kun kehittäjien ja johdon paniikki saavuttaisi huippunsa, tämä "tunnistamaton järjestelmäanomalia" nousisi täydellisimmäksi perusteluksi (tapaukseksi) pakottaa koko yritys alistumaan Johanneksen uusiin sääntöihin.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Vaikka ei ollut vielä sulkemisaika, keskiyön kaltaiseen pimeyteen ja hiljaisuuteen verhotussa toimistossa, vihreä teksti, joka antoi klusterille viimeisen iskun, hehkui hänen omista käsistään.

Muutamaa päivää myöhemmin Johanneksen hallitukselle ja koko kehitysosastolle esittämä Incident Report (Post-Mortem) -esitys oli virheetön. Hänen selityksensä – "Valvomattomien tekoälyagenttien luoma poikkeava koodi resonoi keskenään, laukaisten rekursiivisen pilviresurssien ehtymisen ja salaperäisen puhdistuksen. Jos en olisi suorittanut hätäkatkaisijaa, jopa asiakastiedot olisi pyyhitty pois" – muutti hänet sotarikollisesta "yrityksen pelastaneeksi sankariksi".

Johan, joka yleensä jätti huomiotta jopa Slack-maininnat, kommunikoi tällä kertaa innokkaasti kaikkien kanssa. Hän valvoi koko yön laatien 30-sivuisen Notion-dokumentin nimeltä "Enterprise AI Governance Policy" ja piti suoria videopuheluita kaikkien osastojen johtajien kanssa luodakseen poliittisen pohjan. Kun kyse oli heidän egonsa tyydyttämisestä (puutarhansa puolustamisesta) ja oman siivoustyönsä vähentämisestä, alusta-insinöörit saattoivat osoittaa hämmästyttävää intohimoa ja poliittista taitoa.

Varmistettuaan johtoryhmän täydellisen pelon ja hyväksynnän hän asetti äärimmäisen joukon "uusia tekoälyn suojakaiteita" koko yritykseen. "Kaikki tekoälyavusteiset koodaustyökalut asetetaan Enterprise Compliance Gate V4:n valvontaan. 1. Ennakkokehotteen DLP (Data Loss Prevention) -suodatus arkaluonteisten tietojen vuotojen estämiseksi. 2. Tekoälykohtainen SAST hallusinaatioiden, haavoittuvuuksien ja lisenssien saastumisen skannaamiseksi. 3. Kaikkien kehotteiden auditilokien pysyvä säilytys EU:n tekoälylain mukaisesti. 4. 'Human-in-the-Loop' (manuaalinen tarkistus ja hyväksyntä) kahden vanhemman insinöörin toimesta. Kaikki koodi, joka epäonnistuu edes yhdessä yllä olevista, hylätään automaattisesti yhdistämisestä tuotantoympäristöön."

Se oli pohjimmiltaan "tekoälyn lobotomia". Kehittäjien työnkulku, joka aiemmin mahdollisti käyttöönotot sekunneissa, tukahdutettiin täysin tällä jäykällä byrokratialla, jonka Johan iloisesti rakensi. Joka kerta kun kehittäjä kysyi tekoälyltä kysymyksen editorissaan, DLP-yhdyskäytävä sylki varoituksia. Generoitu koodi hylättiin staattisen analyysin toimesta, ja lopulta läpäisseet PR:t jätettiin mätänemään päiviksi vanhempien insinöörien hyväksyntäjonoon. Yrityksen kehitystuottavuus romahti paljon tekoälyä edeltävän tason alapuolelle. Alemman tason insinöörit pitivät suunsa kiinni julkisilla Slack-kanavilla ja purkivat kaunaansa näistä näkymättömistä käsiraudoista vain yksityisissä DM-viesteissä läheisten kollegoiden kanssa. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Kykenemättä kestämään sitä, Tech Leadit ja CTO – samat ihmiset, jotka olivat aiemmin väistäneet hänen varoituksiaan liukkailla tekosyillä – tulivat itkien Johanneksen luo: "Emmekö voisi jotenkin löysätä hyväksyntäprosessia? Emme saa ominaisuusjulkaisujamme valmiiksi tällä vauhdilla." Mutta Johan ei kiinnittänyt heidän sanoihinsa mitään huomiota.

Johanin näkökulmasta he eivät yksinkertaisesti olleet ymmärtäneet omaa tietämättömyyttään. Jos nämä kehittäjät, tekoälyn antaman äärettömän vapauden illuusion alla, olisivat jatkaneet infrastruktuurin tiedostamatonta saastuttamista, he olisivat lopulta aiheuttaneet "peruuttamattoman megakatkoksen" ja kantaneet siitä kohtalokkaan vastuun itse. "Kiitos siitä, että mursin heidän siipensä, he ovat suojassa 'itsetuhon vastuulta'. Sen sijaan, että he kaunaavat minua, heidän pitäisi kiittää minua syvästi." Johan uskoi tähän vilpittömästi. Heidän vetäminen takaisin "portaaliin sidotuiksi voimattomiksi kuluttajiksi" ylivoimaisella perustelulla oli hänelle puhdasta oikeutta ja ylintä ylimielisyyttä.

Lopulta he pakenisivat yrityksen tiukasta valvontaverkosta ja alkaisivat käyttää luvatonta tekoälyä – "Shadow AI" – henkilökohtaisilla laitteilla ja epävirallisilla tileillä. Johanilla ei ollut mitään keinoa tietää, että tämä toisi järjestelmään tulevaisuudessa vielä kohtalokkaamman korruption.

Pimeässä huoneessa näyttöjen edessä Johan hymyili hiljaa. Hän oli pannut julmat käsiraudat vastenmieliseen meluun, halliten nyt hiljaisuuden jumalana (vartijana).

"THE EXECUTION" (Suoritustietokanta: f2e9343f-5fdc)

https://otiose.app/en/execution
