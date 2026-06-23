Pukul 4:40 sore. Zurich, Swiss.
Di luar jendela, dunia sudah tenggelam dalam kegelapan pekat malam musim dingin. Angin dingin yang bertiup dari Pegunungan Alpen menghantam kaca gedung perkantoran besar yang berdiri di tepi Danau Zurich.

Johan bekerja sebagai insinyur platform senior di salah satu perusahaan teknologi keuangan terbesar di Eropa.
Berbekal standar kepatuhan keuangan yang sangat ketat yang unik di Swiss (peraturan FINMA), ia pernah menjadi penjaga gerbang mutlak yang mengendalikan jurang infrastruktur perusahaan ini. Setiap kali pengembang merancang layanan baru, mereka harus datang ke mejanya untuk ritual persetujuan yang menyamar sebagai "konsultasi arsitektur." Saat ia mengomel dengan pertanyaan seperti "Apakah Anda meninggalkan dokumentasi?" atau "Apakah ini memenuhi persyaratan keamanan?", Johan diam-diam menyimpan rasa superioritas dan kebanggaan atas posisinya yang istimewa—fakta bahwa tidak ada yang bisa bergerak tanpa melalui dirinya.

Akhirnya, dengan dalih "membebaskan pengembang dari kompleksitas infrastruktur," ia membangun portal pengembang internal yang dikenal sebagai "Golden Path." Namun, motif sebenarnya hanyalah untuk melindungi infrastrukturnya yang indah (kebunnya) agar tidak diinjak-injak oleh sepatu bot berlumpur para pengembang yang tidak tahu apa-apa. Dengan sepenuhnya menyembunyikan kedalaman Kubernetes, ia menginfantilkan para pengembang, mengubah mereka menjadi "konsumen tak berdaya" yang hanya menekan tombol.

Tapi sekarang, perfeksionismenya mencekiknya dengan cara terburuk.

Berbekal "sayap mahakuasa" agen pengkodean AI, para pengembang tidak lagi repot-repot mencari konsultasi Johan (atau persetujuan arsitektur). Bahkan, mereka bahkan tidak membuka portal yang telah ia bangun. Mereka hanya melemparkan prompt yang ceroboh ke AI IDE mereka: "Deploy infra untuk layanan pembayaran baru." Dalam hitungan detik, AI akan menghasilkan ratusan baris YAML yang membengkak dan anomali. Para pengembang, tanpa membaca kode tersebut, langsung menggabungkannya ke GitHub. Dari sana, pipeline CI/CD (ArgoCD) secara otomatis menyalurkan konfigurasi sampah ini ke klaster produksi, sepenuhnya mengabaikan estetika Johan.

Menyeruput kopi dinginnya, Johan menatap layar Slack yang sunyi.
Apa yang ia lihat adalah "pemutusan komunikasi" yang lengkap.
Di saluran `#squad-payments`, bot pemindaian keamanan otomatis (Datadog CSPM) memuntahkan peringatan boilerplate.
`[High] Peran IAM yang terlalu luas terdeteksi di payment-service`
Bahkan ketika Johan menandai pengembang yang bertanggung jawab di utas dan bertanya, "Apakah ini disengaja?", sama sekali tidak ada jawaban. Beberapa jam kemudian, beberapa emoji "👀" dan "👍" yang tidak bernyawa muncul. Itu saja.

Baru saja, Johan telah mengeluarkan peringatan keras kepada Tech Lead departemen pengembangan dan CTO: "Jika kita terus membiarkan output AI berjalan liar seperti ini, ketersediaan dan keamanan infrastruktur kita akan mengalami keruntuhan fatal." 
Tetapi mereka tidak secara langsung menyangkal Johan.
"Anda benar sekali. Saya merasakan krisis yang sama persis. Ini adalah situasi yang sangat buruk dalam hal technical debt."
Mereka mengangguk dalam-dalam, berpura-pura sangat berempati, sebelum menambahkan:
"Namun, tekanan dari dewan sangat kuat saat ini. Kita tidak bisa memperlambat kecepatan pengembangan sampai kita mencapai OKR kita untuk kuartal ini. Saya akan berkoordinasi dengan PM untuk memastikan 'Infrastructure Sanity Sprint' dimasukkan dalam roadmap Q3, jadi bisakah Anda bertahan dengan pengaturan saat ini sampai saat itu?"
Sebagai mantan insinyur sendiri, mereka memahami krisis ketersediaan. Karena mereka takut akan "tanggung jawab kelalaian yang disengaja," mereka memalsukan empati untuk menunda keputusan, dengan mulus menghindar dari tanggung jawab.

Para pengembang tidak lagi berbicara dengan manusia. Satu-satunya orang kepercayaan mereka adalah AI di layar mereka. Dan bagi manajemen, peringatan Johan tidak lebih dari kebisingan yang memperlambat kecepatan pengembangan.
Johan tidak kehilangan pekerjaannya. Tetapi statusnya telah runtuh sepenuhnya. Pernah diandalkan sebagai penjaga infrastruktur, ia telah menjadi "relik masa lalu" yang tidak ada yang berkonsultasi, terdegradasi menjadi petugas kebersihan bergaji tinggi yang diam-diam membersihkan kebocoran memori dan pemadaman loop tak terbatas yang ditinggalkan oleh AI.

Tiba-tiba, pemanfaatan CPU klaster produksi utama menarik gelombang anomali.
Tapi itu bukan "lonjakan beban" yang biasa. Sebaliknya, banyak kontainer mati secara bersamaan, dan grafik sumber daya mulai anjlok seperti jatuh dari tebing.

Pemadaman. Johan segera membuka terminalnya dan mengambil log peristiwa pod.
`kubectl get events -n production --sort-by='.metadata.creationTimestamp'`

Pesan kesalahan berjatuhan di layar. Namun, di kolom "Eviction Reason", di mana biasanya akan menampilkan `OOMKilled` atau `NodeNotReady`, muncul string karakter yang mustahil.

text
Reason: Evicted_By_Unknown_Admission_Webhook
Message: panic: runtime error: invalid memory address or nil pointer dereference... [REDACTED_ANOMALY]


Jantung Johan berdetak dingin. Sebuah peretasan?
Ia buru-buru memeriksa daftar kontainer yang dihentikan secara paksa. Semuanya adalah microservice yang tidak efisien, membuang-buang sumber daya yang dihasilkan oleh agen AI selama beberapa bulan terakhir.
Proses yang tidak teridentifikasi telah membajak autoscaling klaster. Alih-alih mendeteksi beban dan menambahkan server, ia secara tanpa syarat membersihkan (menghapus) "kontainer kotor yang melanggar estetika infrastruktur (Golden Path)" dari lingkungan produksi.

Banjir notifikasi Bot yang tidak bernyawa mulai membanjiri saluran Slack `#incident-critical`.
`[P1] PagerDuty: Payment API HTTP 500 Spike`
`[P1] PagerDuty: Recommendation Engine NodeNotReady`
`Incident.io: Insiden baru #4092 dideklarasikan oleh @pagerduty-bot`
Para pengembang, sepenuhnya bergantung pada output AI yang tidak kompeten, tidak dapat memahami situasinya. Mereka hanya bisa mengetik teks pendek ke utas seperti `anyone has context?` dan `rollback failing with timeout`. Perusahaan kemungkinan kehilangan jutaan franc dalam beberapa menit ini.

Johan meletakkan tangannya di keyboard, bersiap untuk menjalankan skrip kill switch (failover). Jika ia menekan tombol Enter, klaster cadangan akan secara paksa boot up, menghentikan amukan yang tidak teridentifikasi ini.

Tetapi saat ia mencoba mengetik perintah ke terminal, jari-jarinya berhenti.
"Metrik klaster" di monitor yang berdekatan menggambar bentuk gelombang yang luar biasa.
Grafik CPU klaster, yang sekarang terbebas dari sampah yang tidak perlu, telah mendapatkan kembali keheningan dasar yang sempurna yang ia impikan pada hari ia menjadi insinyur platform.

...Indah.

Johan bergumam tanpa sadar.
Apa yang benar-benar ia inginkan bukanlah untuk mendukung bisnis, juga bukan semata-mata untuk merebut kembali kemurnian kebunnya.
Untuk mengungkapkan perasaannya yang jelek dan sebenarnya, ia hanya ingin duduk di kursi barisan depan sedikit lebih lama, menyaksikan para pengembang—yang telah terbang dengan angkuh dengan "sayap mahakuasa" AI mereka—sayap mereka tiba-tiba dicabut, jatuh ke tanah, panik, dan berteriak dalam ketidakberdayaan yang menyedihkan.

Johan diam-diam melepaskan tangannya dari kill switch.
Ia tidak memiliki kewajiban untuk segera memulihkan sistem dan menenangkan mereka. Biarkan mereka merasakan lebih banyak teror. Setelah kepanikan para pengembang dan manajemen mencapai puncaknya, "anomali sistem yang tidak teridentifikasi" ini akan meningkat menjadi pembenaran (insiden) yang paling sempurna untuk memaksa seluruh perusahaan tunduk di bawah aturan baru Johan.

`kubectl scale deploy -n payments,recommendations,legacy --replicas=0`

Meskipun belum waktu tutup, di kantor yang diselimuti kegelapan dan keheningan seperti tengah malam, teks hijau yang memberikan pukulan terakhir pada klaster bersinar oleh tangannya sendiri.

Beberapa hari kemudian, Presentasi Laporan Insiden (Post-Mortem) yang disampaikan Johan kepada dewan direksi dan seluruh departemen pengembangan tidak bercela. Penjelasannya—"Kode anomali yang dihasilkan oleh agen AI yang tidak terkontrol beresonansi satu sama lain, memicu kelelahan sumber daya cloud rekursif dan pembersihan misterius. Jika saya tidak menjalankan pemutus sirkuit darurat, bahkan data pelanggan akan terhapus"—mengubahnya dari penjahat perang menjadi "pahlawan yang menyelamatkan perusahaan."

Johan, yang biasanya mengabaikan bahkan sebutan Slack, berkomunikasi dengan antusias dengan semua orang pada kesempatan khusus ini.
Ia begadang semalaman menyusun dokumen Notion 30 halaman berjudul "Enterprise AI Governance Policy" dan mengadakan panggilan video langsung dengan manajer di semua departemen untuk meletakkan dasar politik. Ketika datang untuk memuaskan ego mereka (mempertahankan kebun mereka) dan mengurangi pekerjaan pembersihan mereka sendiri, insinyur platform dapat menunjukkan gairah dan kecakapan politik yang menakjubkan.

Setelah mengamankan ketakutan dan persetujuan total dari tim manajemen, ia memberlakukan serangkaian "penjaga AI baru" yang ekstrem di seluruh perusahaan.
"Semua alat pengkodean yang dibantu AI akan ditempatkan di bawah pengawasan Enterprise Compliance Gate V4.
1. Pemfilteran DLP (Data Loss Prevention) pra-prompt untuk mencegah kebocoran informasi sensitif.
2. SAST khusus AI untuk memindai halusinasi, kerentanan, dan kontaminasi lisensi.
3. Retensi permanen log audit untuk semua prompt sesuai dengan EU AI Act.
4. 'Human-in-the-Loop' (peninjauan dan persetujuan manual) oleh dua Senior Engineer.
Setiap kode yang gagal bahkan salah satu dari di atas akan secara otomatis ditolak untuk digabungkan ke lingkungan produksi."

Itu pada dasarnya adalah "lobotomi untuk AI."
Alur kerja pengembang, yang dulunya memungkinkan deployment dalam hitungan detik, benar-benar tercekik oleh birokrasi kaku yang dengan gembira dibangun Johan ini. Setiap kali seorang pengembang mengajukan pertanyaan kepada AI di editor mereka, gateway DLP memuntahkan peringatan. Kode yang dihasilkan ditolak oleh analisis statis, dan PR yang akhirnya lolos dibiarkan membusuk selama berhari-hari dalam antrean persetujuan insinyur senior. Produktivitas pengembangan perusahaan anjlok jauh di bawah tingkat pra-AI.
Para insinyur tingkat bawah bungkam di saluran Slack publik, melampiaskan kebencian mereka atas borgol tak terlihat ini hanya di DM pribadi dengan rekan-rekan dekat.
![image](https://pub-9d8e491188f440cba805364773eee7c7.r2.dev/newsletter/img_4138d945-d2c8-4769-8c9f-0b672711e55e.jpg)
Tidak dapat menahannya, para Tech Lead dan CTO—orang-orang yang sama yang sebelumnya menghindari peringatannya dengan alasan licin—datang menangis kepada Johan: "Tidak bisakah kita melonggarkan proses persetujuan? Kita tidak akan mencapai rilis fitur kita dengan kecepatan ini."
Tetapi Johan sama sekali tidak memperhatikan kata-kata mereka.

Dari sudut pandang Johan, mereka hanya belum menyadari ketidaktahuan mereka sendiri. Jika para pengembang ini, di bawah ilusi kebebasan tak terbatas yang diberikan oleh AI, terus tanpa sadar mencemari infrastruktur, mereka pada akhirnya akan menyebabkan "mega-pemadaman yang tidak dapat diubah" dan menanggung tanggung jawab fatal untuk itu sendiri.
"Berkat saya mematahkan sayap mereka, mereka terlindungi dari 'tanggung jawab penghancuran diri.' Daripada membenci saya, mereka seharusnya sangat berterima kasih kepada saya."
Johan benar-benar percaya ini. Menarik mereka kembali menjadi "konsumen tak berdaya yang terikat pada portal" menggunakan pembenaran yang luar biasa adalah, baginya, keadilan murni dan arogansi tertinggi.

Akhirnya, mereka akan melarikan diri dari jaring pengawasan ketat perusahaan dan mulai menggunakan AI yang tidak sah—"Shadow AI"—pada perangkat pribadi dan akun tidak resmi. Johan tidak tahu bahwa ini akan membawa korupsi yang lebih fatal ke sistem di masa depan.

Di depan monitor di ruangan gelap, Johan tersenyum pelan.
Ia telah memasang borgol brutal pada kebisingan yang menjijikkan, sekarang berkuasa sebagai Dewa Keheningan (Warden).

"THE EXECUTION" (Basis data eksekusi: f2e9343f-5fdc)

https://otiose.app/en/execution
