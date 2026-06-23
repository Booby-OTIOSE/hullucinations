16:40. Zurych, Szwajcaria.
Za oknem świat pogrążył się już w ciężkiej ciemności zimowej nocy. Zimny wiatr wiejący z Alp uderzał w szyby masywnego biurowca stojącego nad Jeziorem Zuryskim.

Johan pracował jako starszy inżynier platformy w jednej z największych europejskich firm z branży technologii finansowych.
Uzbrojony w wyjątkowo surowe standardy zgodności finansowej, unikalne dla Szwajcarii (regulacje FINMA), był kiedyś absolutnym strażnikiem, który kontrolował otchłań infrastruktury tej firmy. Ilekroć deweloperzy projektowali nową usługę, musieli przychodzić do jego biurka na rytuał zatwierdzania, udający "konsultację architektoniczną". Kiedy nękał ich pytaniami typu "Czy zostawiłeś dokumentację?" lub "Czy to spełnia wymagania bezpieczeństwa?", Johan potajemnie żywił poczucie wyższości i dumy ze swojej uprzywilejowanej pozycji – faktu, że nic nie mogło się ruszyć bez jego zgody.

Ostatecznie, pod pretekstem „uwolnienia deweloperów od złożoności infrastruktury”, zbudował wewnętrzny portal deweloperski znany jako „Golden Path”. Jego prawdziwym motywem było jednak po prostu chronienie swojej pięknej infrastruktury (swojego ogrodu) przed zadeptaniem przez zabłocone buty nieświadomych deweloperów. Całkowicie ukrywając głębię Kubernetes, infantylizował deweloperów, zamieniając ich w „bezsilnych konsumentów”, którzy jedynie naciskali przyciski.

Ale teraz jego perfekcjonizm dusił go w najgorszy możliwy sposób.

Uzbrojeni w „wszechmocne skrzydła” agentów kodujących AI, deweloperzy nie zadawali sobie już trudu, by szukać konsultacji Johana (ani zatwierdzenia architektury). W rzeczywistości nawet nie otwierali portalu, który zbudował. Po prostu wrzucali niedbałe prompty do AI swojego IDE: „Wdróż infrastrukturę dla nowej usługi płatniczej”. W ciągu kilku sekund AI generowało setki linii napompowanego, anomalnego YAML. Deweloperzy, nawet nie czytając kodu, scalali go bezpośrednio do GitHub. Stamtąd potok CI/CD (ArgoCD) automatycznie przesyłał te śmieciowe konfiguracje do klastra produkcyjnego, całkowicie ignorując estetykę Johana.

Sącząc zimną kawę, Johan wpatrywał się w milczący ekran Slacka.
To, co zobaczył, było całkowitym „przerwaniem komunikacji”.
W kanale `#squad-payments` automatyczny bot do skanowania bezpieczeństwa (Datadog CSPM) wypluwał standardowe alerty.
`[High] Wykryto zbyt szeroką rolę IAM w payment-service`
Nawet gdy Johan oznaczył odpowiedzialnego dewelopera w wątku i zapytał: „Czy to celowe?”, nie było absolutnie żadnej odpowiedzi. Godziny później pojawiło się kilka bezdusznych emotikonów „👀” i „👍”. To wszystko.

Zaledwie kilka dni temu Johan wydał surowe ostrzeżenie Tech Lead'owi działu rozwoju i CTO: "Jeśli nadal będziemy pozwalać na tak swobodne działanie wyników AI, dostępność i bezpieczeństwo naszej infrastruktury ulegnie fatalnemu załamaniu." 
Ale nie zaprzeczyli Johanowi wprost.
"Ma pan absolutną rację. Podzielam dokładnie to samo poczucie kryzysu. To bardzo zła sytuacja pod względem długu technicznego."
Kiwnęli głęboko głowami, udając ogromną empatię, po czym dodali:
"Jednak presja ze strony zarządu jest obecnie niesamowicie silna. Nie możemy spowalniać tempa rozwoju, dopóki nie osiągniemy naszych OKR na ten kwartał. Skoordynuję z PM-ami, aby upewnić się, że 'Sprint Sanity Infrastruktury' zostanie uwzględniony w planie działania na Q3, więc czy mógłby pan po prostu utrzymać obecną konfigurację do tego czasu?"
Będąc sami byłymi inżynierami, rozumieli kryzys dostępności. Ponieważ obawiali się "odpowiedzialności za umyślne zaniedbanie", udawali empatię, aby odłożyć decyzję, zręcznie wymykając się od odpowiedzialności.

Deweloperzy nie rozmawiali już z ludźmi. Ich jedynym powiernikiem była sztuczna inteligencja na ich ekranach. A dla zarządu ostrzeżenia Johana były niczym więcej niż szumem spowalniającym tempo rozwoju.
Johan nie stracił pracy. Ale jego status całkowicie się załamał. Kiedyś polegano na nim jako na strażniku infrastruktury, stał się „reliktem przeszłości”, z którym nikt się nie konsultował, zdegradowany do roli wysoko opłacanego dozorcy, który po cichu sprzątał wycieki pamięci i awarie pętli nieskończoności pozostawione przez sztuczną inteligencję.

Nagle wykorzystanie procesora głównego klastra produkcyjnego narysowało anomalną falę.
Ale to nie był zwykły „skok obciążenia”. Wręcz przeciwnie, niezliczone kontenery umarły jednocześnie, a wykres zasobów zaczął spadać jak z klifu.

Awaria. Johan natychmiast otworzył swój terminal i pobrał logi zdarzeń podów.
`kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Na ekranie kaskadowo spływały komunikaty o błędach. Jednak w kolumnie „Eviction Reason”, gdzie normalnie wyświetlałoby się `OOMKilled` lub `NodeNotReady`, pojawił się niemożliwy ciąg znaków.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Serce Johana zabiło zimnym rytmem. Hack? 
Pośpiesznie sprawdził listę przymusowo zakończonych kontenerów. Wszystkie były nieefektywnymi, marnującymi zasoby mikroserwisami generowanymi przez agentów AI w ciągu ostatnich kilku miesięcy.
Nieznany proces przejął autoskalowanie klastra. Zamiast wykrywać obciążenie i dodawać serwery, bezwarunkowo usuwał (czyścił) „brudne kontenery, które naruszały estetykę infrastruktury (Golden Path)” ze środowiska produkcyjnego.

Potok bezdusznych powiadomień od Botów zaczął zalewać kanał Slack `#incident-critical`.
`[P1] PagerDuty: Payment API HTTP 500 Spike`
`[P1] PagerDuty: Recommendation Engine NodeNotReady`
`Incident.io: Nowy incydent #4092 zgłoszony przez @pagerduty-bot`
Deweloperzy, całkowicie polegający na niekompetentnych wynikach AI, nie potrafili zrozumieć sytuacji. Mogli jedynie wpisywać krótkie teksty w wątku, takie jak `anyone has context?` i `rollback failing with timeout`. Firma prawdopodobnie traciła miliony franków w ciągu tych kilku minut.

Johan położył ręce na klawiaturze, przygotowując się do wykonania skryptu kill switch (failover). Gdyby nacisnął klawisz Enter, klaster zapasowy uruchomiłby się siłą, zatrzymując ten niezidentyfikowany szał.

Ale w momencie, gdy próbował wpisać polecenie do terminala, jego palce zatrzymały się.
„Metryki klastra” na sąsiednim monitorze rysowały niewiarygodną falę.
Wykres procesora klastra, teraz pozbawiony niepotrzebnych śmieci, odzyskał idealną ciszę bazową, o której marzył w dniu, gdy został inżynierem platformy.

...Piękne.

Johan mruknął nieświadomie.
To, czego naprawdę pragnął, to nie było wspieranie biznesu, ani wyłącznie odzyskanie czystości swojego ogrodu.
Aby wyrazić swoje brzydkie, prawdziwe uczucia, chciał po prostu posiedzieć jeszcze trochę w pierwszym rzędzie, obserwując deweloperów – którzy arogancko latali na swoich „wszechmocnych skrzydłach” AI – jak nagle odrywają im skrzydła, spadają na ziemię, panikują i krzyczą w żałosnej bezradności.

Johan cicho odsunął ręce od wyłącznika awaryjnego.
Nie miał obowiązku natychmiast przywracać systemu i ich uspokajać. Niech poczują więcej terroru. Gdy panika deweloperów i zarządu osiągnie szczyt, ta „niezidentyfikowana anomalia systemowa” stanie się najdoskonalszym uzasadnieniem (incydentem), aby zmusić całą firmę do podporządkowania się nowym zasadom Johana.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Mimo że nie było jeszcze godziny zamknięcia, w biurze spowitym północną ciemnością i ciszą, zielony tekst, który zadał ostateczny cios klastrowi, świecił jego własnymi rękami.

Kilka dni później prezentacja Raportu Incydentu (Post-Mortem), którą Johan przedstawił zarządowi i całemu działowi rozwoju, była bezbłędna. Jego wyjaśnienie – „Anomalny kod generowany przez niekontrolowane agenty AI rezonował ze sobą, wywołując rekurencyjne wyczerpanie zasobów chmury i tajemnicze czyszczenie. Gdybym nie wykonał awaryjnego wyłącznika, nawet dane klientów zostałyby usunięte” – przekształciło go z zbrodniarza wojennego w „bohatera, który uratował firmę”.

Johan, który zazwyczaj ignorował nawet wzmianki na Slacku, tym razem z entuzjazmem komunikował się ze wszystkimi.
Całą noc spędził na redagowaniu 30-stronicowego dokumentu Notion zatytułowanego „Enterprise AI Governance Policy” i prowadził bezpośrednie rozmowy wideo z menedżerami wszystkich działów, aby położyć podwaliny polityczne. Jeśli chodzi o zaspokojenie ich ego (obronę ich ogrodu) i zmniejszenie własnej pracy porządkowej, inżynierowie platformy potrafili wykazać się zdumiewającą pasją i sprawnością polityczną.

Po zapewnieniu sobie całkowitego strachu i aprobaty zespołu zarządzającego, narzucił całej firmie ekstremalny zestaw „nowych zabezpieczeń AI”.
„Wszystkie narzędzia do kodowania wspomagane AI zostaną objęte nadzorem Enterprise Compliance Gate V4.
1. Filtrowanie DLP (Data Loss Prevention) przed promptem, aby zapobiec wyciekom poufnych informacji.
2. SAST specyficzny dla AI do skanowania pod kątem halucynacji, luk w zabezpieczeniach i zanieczyszczeń licencyjnych.
3. Stałe przechowywanie dzienników audytu dla wszystkich promptów zgodnie z EU AI Act.
4. 'Human-in-the-Loop' (ręczny przegląd i zatwierdzenie) przez dwóch starszych inżynierów.
Każdy kod, który nie spełnia choćby jednego z powyższych, zostanie automatycznie odrzucony z połączenia ze środowiskiem produkcyjnym.”

Była to w zasadzie „lobotomia dla AI”.
Przepływ pracy deweloperów, który kiedyś pozwalał na wdrożenia w ciągu sekund, został całkowicie zduszony przez tę sztywną biurokrację, którą Johan z radością zbudował. Za każdym razem, gdy deweloper zadawał pytanie AI w swoim edytorze, brama DLP wypluwała ostrzeżenia. Wygenerowany kod był odrzucany przez analizę statyczną, a PR-y, które w końcu przeszły, gniły przez dni w kolejce do zatwierdzenia przez starszych inżynierów. Produktywność deweloperska firmy spadła znacznie poniżej poziomu sprzed AI.
Inżynierowie niższego szczebla milczeli na publicznych kanałach Slacka, wyładowując swoje niezadowolenie z tych niewidzialnych kajdan tylko w prywatnych wiadomościach z bliskimi kolegami.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg)
Nie mogąc tego znieść, Tech Leadowie i CTO – ci sami ludzie, którzy wcześniej unikali jego ostrzeżeń z wymówkami – przyszli płacząc do Johana: „Czy nie moglibyśmy jakoś złagodzić procesu zatwierdzania? W tym tempie nie zrealizujemy naszych wydań funkcji.”
Ale Johan absolutnie nie zwracał uwagi na ich słowa.

Z perspektywy Johana, po prostu nie zdawali sobie sprawy ze swojej własnej ignorancji. Gdyby ci deweloperzy, pod iluzją nieskończonej wolności udzielonej przez AI, nadal nieświadomie zanieczyszczali infrastrukturę, ostatecznie spowodowaliby „nieodwracalną mega-awarię” i sami ponieśliby za to fatalną odpowiedzialność.
„Dzięki temu, że złamałem im skrzydła, są chronieni przed 'odpowiedzialnością za samozniszczenie'. Zamiast mnie nienawidzić, powinni mi głęboko dziękować.”
Johan szczerze w to wierzył. Przyciągnięcie ich z powrotem do bycia „bezsilnymi konsumentami związanymi z portalem” za pomocą przytłaczającego uzasadnienia było dla niego czystą sprawiedliwością i najwyższą arogancją.

W końcu uciekną z surowej sieci nadzoru firmy i zaczną używać nieautoryzowanej sztucznej inteligencji – „Shadow AI” – na urządzeniach osobistych i nieoficjalnych kontach. Johan nie mógł wiedzieć, że to przyniesie jeszcze bardziej fatalną korupcję systemu w przyszłości.

Przed monitorami w ciemnym pokoju Johan cicho się uśmiechnął.
Nałożył brutalne kajdanki na odrażający hałas, panując teraz jako Bóg Ciszy (Strażnik).

"THE EXECUTION" (Baza danych wykonania: f2e9343f-5fdc)

https://otiose.app/en/execution
