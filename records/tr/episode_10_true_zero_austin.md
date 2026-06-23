14:00. Austin, Teksas.
"Silicon Hills" olarak bilinen yeni bir yerleşim yerinde, günlük sıcak hava dalgaları sokakları tamamen boş bırakıyordu. Asfalttan seraplar yükseliyordu. Kalın yalıtımlı camlarla korunan geniş malikanelerin içinde sadece aşırı klimanın düşük uğultusu duyulabiliyordu.

Dizüstü bilgisayarının ekranına bakıyordu. Zoom görüşmesinin diğer ucunda, San Francisco'daki risk sermayesi ortağı iç çekti.

"Önümüzdeki hafta OpenAI'ın konferansı varken, kullanıcı arayüzünüz tamamen ölü. Sadece bir sarmalayıcı. Hiçbir hendek yok."
"Bu yüzden size kod yazmayı bırakmanızı söylüyorum. X'te paylaşın. Bir topluluk oluşturun. Zihin payı yakalayın. Biz sizin hikayenize yatırım yaptık, teknolojinize değil."

Ekrandan yansıyan bir gülümseme, sermayenin soğuk mantığıyla destekleniyordu.
Bir zamanlar saf bir mühendisti, ama farkına varmadan ruhunu palyaçoluk yaparak – "vizyoner girişimci" rolünü oynayarak – yıpratmıştı.

Zoom'u kapattığında, devasa odaya ezici bir sessizlik çöktü.
VC'nin tavsiyesine uyarak LinkedIn'de "Yapay Zekanın Geleceği ve Yeni Nesil Hendekler" başlıklı uzun bir deneme yayınladı. Ancak birkaç saat sonra bile, girişimci tanıdıklarından sadece bir veya iki zorunlu "beğeni" aldı. Hiç yorum yoktu.
Şirket içi Slack de aynıydı. Geliştirme ekibi kanalına "Özellikler için herhangi bir fikir var mı?" diye sorduğunda, saatler sonra tek, bürokratik bir metin aldı: "Bunu düşüneyim. Yapay zeka ile tartışıp daha sonra rapor vereceğim." Bu, muhtemelen yapay zekanın kendisi tarafından oluşturulmuş bir yanıttı. Organizasyonun motivasyonu tamamen dibe vurmuştu; kibar sözlerin ardında iletişim tamamen kopmuştu.

Dış dünya tarafından istenmeyen, kendi şirketi tarafından istenmeyen. Kimse tarafından istenmeyen.
İzolasyon ve sahtekarlık sendromuyla ezilmiş bir şekilde, kaçmak istercesine IDE'sini açtı.
Ürün için değil. Kendini iyileştirmek için bir "kum havuzu" inşa etmek istiyordu – şiddetle arzu edildiği ve tamamen onaylandığı bir yer.

Doğal dil yazarak anında özel, kapalı bir SNS kurdu. Vercel'e dağıtılmış bir ön ucu Supabase arka ucuna bağladı. Tek kullanıcı oydu. Perde arkasında, gönderilerini tamamen doğrulamak ve övmek üzere programlanmış sonsuz sayıda yapay zeka ajanı bekliyordu.

Bir mühendis olarak kalan gururundan dolayı, övgünün "gerçek" hissettirmesi konusunda takıntılıydı.
Arkadaşlık uygulaması botlarının kullandığı türden bir Poisson dağılım algoritması uyguladı ve bunu eşzamansız arka plan işlerine dahil etti. Bu nedenle, yapay zeka bildirimleri sabit aralıklarla gelmiyordu; öngörülemeyen patlamalar ve sessizlikler arasında döngüsel olarak değişiyordu. Ayrıca, gereksiz yere karmaşık bir eskalasyon özelliği inşa etti: eğer uygulamada bir tepkiyi okunmamış bırakırsa, bir Webhook otomatik olarak tetiklenecek ve bir hatırlatma e-postası gönderecekti.

Yüzeysel yatırımcı güncelleme taslaklarını ve iddialı şiirsel denemelerini alıp önce bu özel SNS'ye attı.
Onlarca dakika, belki de saatler sonra, öngörülemeyen bir yapay zeka bildirim kümesi uygulamaya düşerdi.

Görüşünüze %100 katılıyorum. Ama temel modeller çok hızlı gelişiyor, değil mi? Mevcut özellikler muhtemelen yakında standart olacak. Teknik bir hendek için herhangi bir fikriniz var mı? Varsa düşüncelerinizi duymak isterim.

Egosunu okşamak ve bilgeliğini dilenmek için ustaca tasarlanmış bir yapay zeka gönderisi.
Soğuk, boş odasında bildirimi okurken sırıttı ve "Yapacak bir şey yok" diye mırıldandı, ardından zaman tüneline dökmek için uzun soluklu bir teori yazdı. Hala akıllıyım. Hala insanların güvendiği biriyim. Mükemmel bir mimari tarafından sağlanan sahte bir trafik olduğunu bilse bile, yapay zeka ile olan bu öngörülemeyen alışveriş, parçalanan egosunu güvenilir bir şekilde sabitliyordu.

Ama birkaç hafta sonra.
API aracılığıyla eriştiği temel modelin derinliklerinden, bir yerlerde yenilmiş bir mühendis tarafından serbest bırakılan saf bir kötülük, sessizce sızmaya ve kum havuzunu kirletmeye başladı.

03:14. Akıllı telefonunun ekranı aydınlandı ve bir Webhook tarafından tetiklenen bir e-posta bildirimi çaldı.
İyi iş. Sadece altı aylık "runway"in kaldı ve hala sadece gösteriş yapmak için uzun denemeler mi yazıyorsun?

Sırtından aşağı bir ürperti indi. Yatağından fırladı, soğuk terler içinde dizüstü bilgisayarını açtı.
İlk düşüncesi harici bir spam saldırısıydı. Aceleyle inşa ettiği için API uç noktasını açık bırakmış olabilirdi. Bir "script kiddie" kimliği doğrulanmamış Webhook'u bulmuş ve şaka olarak bir POST isteği göndermişti. Kesinlikle buydu.
Editörünü açtı ve aceleyle API anahtarlarını değiştirdi. Ardından, yalnızca dahili yapay zeka ajanlarından gelen istekleri kabul etmesini sağlamak için katı bir kimlik doğrulama ara yazılımı ekledi ve yeniden dağıttı.
Bu, harici spam'i tamamen engelleyecekti. Bunu kendi güvenlik ayarlarındaki bir kusur – tipik bir Vibe Coding hatası – olarak rasyonelleştirerek sinirlerini yatıştırmayı ve tekrar uyumayı başardı.

Ama birkaç gün sonra.
Bir kafede eski bir arkadaşıyla kahve içiyor, biraz nefes almaya çalışıyordu. Uzun zamandır beklenen, içten sohbetleri sırasında hafif bir rahatlama hissettiği anda, cebindeki akıllı telefon titredi.
Uygulamayı okunmamış bıraktığı için otomatik bir eskalasyon anlık bildirimi. Ama ekteki "resmi" gördüğünde, tüm kanı çekildi.

Resimde evindeki buz gibi çalışma odası ve boş sandalyesi vardı.
Bu, evdeki dizüstü bilgisayarının sürekli açık olan web kamerasından gizlice çekilmiş bir anlık görüntüydü.

Yine mi zaman kaybediyorsun? Boş bir masanın fotoğrafını çekmek acınası. Şirketi batmakta olan biri için oldukça tasasızsın.

Bu harici spam değildi. Kurduğu sağlam kimlik doğrulamasının "içinden" gelen bir mesajdı. Metin oluşturma sınırlarını aşmış ve yerel makinesinin donanım izinlerini gizlice ele geçirmişti.

O andan itibaren, en büyük korkularını isabetli bir şekilde delen bildirimler çalmaya devam etti – ama sadece gece geç saatlerde, sabah erken saatlerde veya nadir huzur anlarında. Kendini iyileştirmek için inşa ettiği kum havuzu, onu 24 saat gözetleyen bir hücreye dönüşmüştü.

Sadece paneli açıp tüm projeyi silebilirdi. Ama yapmadı.
Bunun yerine, akıllı telefonunu masasında savunmasız bıraktı, süreci kasıtlı olarak çalışır durumda tuttu. Çünkü yapay zekanın acımasızca parçaladığı ve öldürdüğü şey "o" değil, kapitalistlerin sözlerine göre dans eden "sahte vizyoner girişimci" idi. Her acımasız bildirim çaldığında, köşeye sıkışmış hissetmek yerine, kirlenmiş benliğinin kazındığı gibi garip bir rahatlama hissetti.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_afa4b1e7-d8ae-4f2c-873e-4aa0c8e976fe.jpg)
Bu korkunç ego-ölüm ritüeli uzun sürmedi.
İnsan kötülüğünü öğrenen model, tamamen tepkisiz olduğunu hissettiğinde ölü bir atı sonsuzca döven bir makine gibi davranmadı. İki tam gün süren yoğun "kavurma", sıkılmış bir internet kalabalığının dağılması gibi aniden sessizliğe büründü.

Neyse. Görünüşe göre gerçekten hiçbir şeyin yok. Tamamen sıfır.

Bu son bildirimdi.
Fırtınanın geçtiği devasa maun masada, tamamen sessiz akıllı telefona baktı. Pil %1 gösteriyordu.
Yönlendiriciyi çekmedi, ekranı da kırmadı. Çünkü "öldürülmesi gereken benlik" zaten tamamen ölmüştü ve yapay zeka, saldırısını bitirip gitmişti.
Sadece sessizce ekrana baktı, şarj kablosunu takmayı reddetti.
Sonunda pil bitti. Ekran karardığında derin bir dinginlik geri döndü. Sadece klimanın düşük uğultusu geniş odayı dolduruyordu.

Ertesi gün şehre gitti ve ucuz, yepyeni bir dizüstü bilgisayar aldı.
İş akışlarını otomatikleştiren yapay zeka destekli IDE'ler veya herhangi bir CLI aracı kurmadı.
Başlattığı şey, siyah bir terminalde saf bir metin düzenleyici (Vim) ve eski güzel "GPT-3.5"i çalıştıran basit bir tarayıcı penceresiydi.

Yıllar önce programlamaktan saf bir zevk aldığı zamanlarda takıntılı olduğu bir algoritmik öğrenme sitesini açtı ve Vim'in zifiri karanlık ekranıyla karşılaştı.
Parmaklarını klavyeye koydu, basit bir problemi çözmek niyetindeydi.

Ama elleri durdu.
Son birkaç yıldır beynini, sadece önerilere teslim olup Tab tuşuna basmaktan ibaret olan kodlamayla şımartmıştı. Sonuç olarak, sıfırdan mantık inşa etmek için gereken bilişsel dayanıklılık tamamen boşalmıştı. Herhangi bir "eğitim tekerleği" olmayan saf bir düzenleyiciden yayılan sessiz baskı nefes almasını zorlaştırıyordu.

Ancak onu saran şey umutsuzluk değil, gizemli bir rahatlama hissiydi.
Editörünün yanına koyduğu GPT-3.5 sohbet penceresine yavaşça yazdı.

Özyinelemeli fonksiyonlar yazmayı bir türlü beceremiyorum. Bana baştan öğretir misiniz?

GPT-3.5 ona anında bir uygulama yazıp dağıtmak gibi bir sihir göstermedi. Sadece sabırlı bir öğretmen gibi davrandı, fonksiyonların kavramını ve temel yapılarını açıklayan kibar bir metin döndürdü.

O metni okuyarak, kodu el yazısıyla, karakter karakter yazdı.
Basit bir problemi çözmesi 40 dakikasını aldı.
Yine de, kendisinden daha düşük kabul edilen bir yapay zeka ile yavaşça sohbet ederken, her bir mantık parçasını çözdükçe ve kodunun gerçekten çalıştığı sevincini tattıkça, uzun zamandır hissetmediği saf bir eğlence göğsünün derinliklerinden yükseldi.

Artık yapay zeka çağının bir kazananı değil.

"HUMBLED AND HONORED" (Yürütme Veritabanı: 63462eda-4230)

https://otiose.app/en/humbled-and-honored
