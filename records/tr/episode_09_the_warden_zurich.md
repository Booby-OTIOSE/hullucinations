16:40. Zürih, İsviçre. Pencerenin dışında dünya, kış gecesinin ağır karanlığına çoktan gömülmüştü. Alpler'den esen soğuk bir rüzgar, Zürih Gölü kenarındaki devasa ofis binasının camlarını dövüyordu.

Johan, Avrupa'nın en büyük finansal teknoloji şirketlerinden birinde kıdemli platform mühendisi olarak çalışıyordu. İsviçre'ye özgü (FINMA düzenlemeleri) son derece katı finansal uyumluluk standartlarıyla donanmış olarak, bir zamanlar bu şirketin altyapısının uçurumunu kontrol eden mutlak kapı bekçisiydi. Geliştiriciler yeni bir hizmet tasarladıklarında, "mimari danışmanlık" kılığına girmiş bir onay ritüeli için onun masasına gelmek zorundaydılar. "Dokümantasyon bıraktınız mı?" veya "Bu güvenlik gereksinimlerini karşılıyor mu?" gibi sorularla onları darlarken, Johan gizlice ayrıcalıklı konumundan – hiçbir şeyin ondan geçmeden ilerleyemeyeceği gerçeğinden – bir üstünlük ve gurur duygusu besliyordu.

Sonunda, "geliştiricileri altyapının karmaşıklığından kurtarma" bahanesiyle, "Golden Path" olarak bilinen dahili geliştirici portalını inşa etti. Ancak asıl amacı, güzel altyapısını (bahçesini) cahil geliştiricilerin çamurlu botları tarafından çiğnenmekten korumaktı. Kubernetes'in derinliklerini tamamen kara kutuya alarak, geliştiricileri "güçsüz tüketiciler" haline getirdi ve onları sadece düğmelere basan kişiler yaptı.

Ama şimdi, mükemmeliyetçiliği onu mümkün olan en kötü şekilde boğuyordu.

Yapay zeka kodlama ajanlarının "her şeye gücü yeten kanatlarıyla" donanmış geliştiriciler, artık Johan'ın danışmanlığını (veya mimari onayını) aramaya zahmet etmiyorlardı. Aslında, inşa ettiği portalı bile açmıyorlardı. Sadece IDE'lerinin yapay zekasına özensiz komutlar atıyorlardı: "Yeni ödeme hizmeti için altyapıyı dağıt." Saniyeler içinde, yapay zeka yüzlerce satır şişkin, anormal YAML kodu üretiyordu. Geliştiriciler, kodu okumadan bile, doğrudan GitHub'a birleştiriyorlardı. Oradan, CI/CD hattı (ArgoCD) bu çöp yapılandırmalarını otomatik olarak üretim kümesine aktarıyor, Johan'ın estetiğini tamamen göz ardı ediyordu.

Soğuk kahvesini yudumlayan Johan, sessiz Slack ekranına baktı. Gördüğü şey tam bir "iletişim kopukluğuydu". `#squad-payments` kanalında, otomatik güvenlik tarama botu (Datadog CSPM) standart uyarılar yayınlıyordu. `[High] Overly broad IAM role detected in payment-service` Johan, sorumlu geliştiriciyi sohbette etiketleyip "Bu kasıtlı mı?" diye sorduğunda bile kesinlikle hiçbir yanıt gelmedi. Saatler sonra, birkaç cansız "👀" ve "👍" emojisi belirdi. Hepsi bu kadardı.

Daha geçen gün Johan, geliştirme departmanının Tech Lead'ine ve CTO'suna ciddi bir uyarıda bulunmuştu: "Yapay zeka çıktılarının bu şekilde serbestçe dolaşmasına izin vermeye devam edersek, altyapımızın kullanılabilirliği ve güvenliği ölümcül bir çöküş yaşayacaktır." Ancak Johan'ı açıkça reddetmediler. "Kesinlikle haklısınız. Aynı kriz duygusunu paylaşıyorum. Teknik borç açısından bu çok kötü bir durum." Derinlemesine başlarını salladılar, büyük bir empati taklidi yaparak eklediler: "Ancak, yönetim kurulunun baskısı şu anda inanılmaz derecede güçlü. Bu çeyrek için OKR'lerimize ulaşana kadar geliştirme hızını yavaşlatamayız. PM'lerle koordinasyon kurarak 'Altyapı Sağlık Sprinti'nin Q3 yol haritasına dahil edilmesini sağlayacağım, bu yüzden o zamana kadar mevcut kurulumla idare edebilir misiniz?" Kendileri de eski mühendis oldukları için kullanılabilirlik krizini anlıyorlardı. "Kasıtlı ihmal sorumluluğundan" korktukları için, kararı ertelemek için empati taklidi yaptılar, sorumluluktan ustaca sıyrıldılar.

Geliştiriciler artık insanlarla konuşmuyordu. Tek sırdaşları ekranlarındaki yapay zekaydı. Ve yönetim için Johan'ın uyarıları, geliştirme hızını yavaşlatan gürültüden başka bir şey değildi. Johan işini kaybetmemişti. Ama statüsü tamamen çökmüştü. Bir zamanlar altyapının koruyucusu olarak güvenilen, kimsenin danışmadığı, yapay zekanın geride bıraktığı bellek sızıntılarını ve sonsuz döngü kesintilerini sessizce temizleyen yüksek maaşlı bir kapıcıya dönüşmüştü.

Aniden, ana üretim kümesinin CPU kullanımı anormal bir dalga çizdi.

Ancak bu, olağan "yük artışı" değildi. Aksine, sayısız konteyner aynı anda öldü ve kaynak grafiği bir uçurumdan düşer gibi hızla düşmeye başladı.

Bir kesinti. Johan hemen terminalini açtı ve pod olay günlüklerini getirdi. `kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Hata mesajları ekrana yağdı. Ancak, normalde `OOMKilled` veya `NodeNotReady` gösterecek olan "Eviction Reason" sütununda, imkansız bir karakter dizisi belirdi.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Johan'ın kalbi soğuk bir şekilde tekledi. Bir hack mi? Zorla sonlandırılan konteynerlerin listesini aceleyle kontrol etti. Hepsi, son birkaç ayda yapay zeka ajanları tarafından üretilen verimsiz, kaynak israf eden mikro hizmetlerdi. Tanımlanamayan süreç, kümenin otomatik ölçeklendirmesini ele geçirmişti. Yükü algılayıp sunucu eklemek yerine, "altyapı estetiğini (Golden Path) ihlal eden kirli konteynerleri" üretim ortamından koşulsuz olarak temizliyordu (siliyordu).

Cansız Bot bildirimleri `#incident-critical` Slack kanalını doldurmaya başladı. `[P1] PagerDuty: Payment API HTTP 500 Spike` `[P1] PagerDuty: Recommendation Engine NodeNotReady` `Incident.io: New incident #4092 declared by @pagerduty-bot` Yetersiz yapay zeka çıktılarına tamamen bağımlı olan geliştiriciler durumu anlayamıyordu. Sadece `anyone has context?` ve `rollback failing with timeout` gibi kısa metinler yazabiliyorlardı. Şirket muhtemelen bu birkaç dakika içinde milyonlarca frank kaybediyordu.

Johan ellerini klavyeye koydu, kill switch (failover) betiğini çalıştırmaya hazırlanıyordu. Enter tuşuna basarsa, yedek küme zorla başlayacak ve bu tanımlanamayan saldırıyı durduracaktı.

Ancak komutu terminale yazmaya çalıştığı anda parmakları durdu.

Bitişik monitördeki "küme metrikleri" inanılmaz bir dalga formu çiziyordu. Gereksiz çöplerden arındırılmış kümenin CPU grafiği, platform mühendisi olduğu gün hayal ettiği mükemmel temel sessizliğe geri dönmüştü.

...Güzel.

Johan bilinçsizce mırıldandı. Gerçekten istediği şey işi desteklemek değildi, ne de sadece bahçesinin saflığını geri kazanmaktı. Çirkin, gerçek duygularını ifade etmek için, sadece biraz daha ön sırada oturup, yapay zekanın "her şeye gücü yeten kanatlarıyla" kibirli bir şekilde uçan geliştiricilerin kanatlarının aniden koparılmasını, yere çakılmalarını, paniklemelerini ve acınası bir çaresizlik içinde çığlık atmalarını izlemek istiyordu.

Johan ellerini sessizce kill switch'ten çekti. Sistemi hemen restore edip onları rahatlatmak gibi bir yükümlülüğü yoktu. Bırakın daha fazla dehşet hissetsinler. Geliştiricilerin ve yönetimin paniği zirveye ulaştığında, bu "tanımlanamayan sistem anormalliği", tüm şirketi Johan'ın yeni kurallarına boyun eğdirmek için en mükemmel gerekçe (olay) haline gelecekti.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Henüz kapanış saati olmamasına rağmen, gece yarısı karanlığı ve sessizliğiyle sarılmış ofiste, kümeye son darbeyi vuran yeşil metin kendi elleriyle parladı.

Birkaç gün sonra, Johan'ın yönetim kuruluna ve tüm geliştirme departmanına sunduğu Olay Raporu (Post-Mortem) kusursuzdu. Açıklaması – "Kontrolsüz yapay zeka ajanları tarafından üretilen anormal kodlar birbirleriyle rezonansa girerek özyinelemeli bulut kaynak tükenmesine ve gizemli bir temizliğe yol açtı. Eğer acil bir devre kesici çalıştırmasaydım, müşteri verileri bile silinecekti" – onu bir savaş suçlusundan "şirketi kurtaran kahraman"a dönüştürdü.

Normalde Slack bildirimlerini bile görmezden gelen Johan, bu özel durumda herkesle coşkuyla iletişim kurdu. "Kurumsal Yapay Zeka Yönetişim Politikası" başlıklı 30 sayfalık bir Notion belgesi hazırlamak için bütün gece ayakta kaldı ve siyasi zemini hazırlamak için tüm departmanlardaki yöneticilerle doğrudan görüntülü görüşmeler yaptı. Kendi egolarını tatmin etme (bahçelerini savunma) ve kendi temizlik işlerini azaltma söz konusu olduğunda, platform mühendisleri şaşırtıcı bir tutku ve siyasi beceri sergileyebilirlerdi.

Yönetim ekibinin tam korkusunu ve onayını aldıktan sonra, şirket genelinde aşırı bir "yeni yapay zeka koruma kalkanları" seti uyguladı. "Tüm yapay zeka destekli kodlama araçları Enterprise Compliance Gate V4 gözetiminde olacaktır. 1. Hassas bilgi sızıntılarını önlemek için ön-prompt DLP (Veri Kaybı Önleme) filtrelemesi. 2. Halüsinasyonları, güvenlik açıklarını ve lisans kirliliğini taramak için yapay zekaya özel SAST. 3. AB Yapay Zeka Yasası'na uygun olarak tüm promptlar için denetim günlüklerinin kalıcı olarak saklanması. 4. İki Kıdemli Mühendis tarafından 'Human-in-the-Loop' (manuel inceleme ve onay). Yukarıdakilerden herhangi birini geçemeyen herhangi bir kod, üretim ortamına birleştirilmekten otomatik olarak reddedilecektir."

Bu aslında bir "yapay zeka lobotomisiydi". Geliştiricilerin saniyeler içinde dağıtım yapmasına olanak tanıyan iş akışı, Johan'ın sevinçle inşa ettiği bu katı bürokrasi tarafından tamamen boğuldu. Bir geliştirici editöründe yapay zekaya her soru sorduğunda, DLP ağ geçidi uyarılar püskürtüyordu. Üretilen kod statik analiz tarafından reddediliyor ve sonunda geçen PR'lar, kıdemli mühendislerin onay kuyruğunda günlerce çürümeye bırakılıyordu. Şirketin geliştirme verimliliği, yapay zeka öncesi seviyelerin çok altına düştü. Alt düzey mühendisler, halka açık Slack kanallarında sessiz kalıyor, bu görünmez kelepçelere duydukları kızgınlığı sadece yakın meslektaşlarıyla özel DM'lerde dile getiriyorlardı. ![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg) Buna dayanamayan Tech Lead'ler ve CTO – daha önce onun uyarılarından kaygan bahanelerle kaçan aynı kişiler – Johan'a ağlayarak geldiler: "Onay sürecini bir şekilde gevşetemez miyiz? Bu hızla özellik yayınlarımızı yapamayız." Ama Johan onların sözlerine kesinlikle kulak asmadı.

Johan'ın bakış açısından, kendi cehaletlerini henüz fark etmemişlerdi. Eğer bu geliştiriciler, yapay zekanın verdiği sonsuz özgürlük yanılsaması altında, altyapıyı bilmeden kirletmeye devam etselerdi, sonunda "geri dönülemez bir mega-kesintiye" neden olacaklar ve bunun ölümcül sorumluluğunu kendileri üstleneceklerdi. "Kanatlarını kırmam sayesinde, 'kendini yok etme sorumluluğundan' korunuyorlar. Bana kızmak yerine, bana derinden teşekkür etmeliler." Johan buna içtenlikle inanıyordu. Onları ezici bir gerekçeyle "portala bağlı güçsüz tüketiciler" olmaya geri sürüklemek, ona göre saf adalet ve yüce bir kibirdi.

Sonunda, şirketin sıkı gözetim ağından kaçacak ve kişisel cihazlarda ve resmi olmayan hesaplarda yetkisiz yapay zeka – "Shadow AI" – kullanmaya başlayacaklardı. Johan, bunun gelecekte sisteme daha da ölümcül bir bozulma getireceğini bilemezdi.

Karanlık odadaki monitörlerin önünde Johan sessizce gülümsedi. Nefret dolu gürültüye acımasız kelepçeler takmış, şimdi Sessizliğin Tanrısı (Gardiyan) olarak hüküm sürüyordu.

"THE EXECUTION" (Yürütme Veritabanı: f2e9343f-5fdc)

https://otiose.app/en/execution
