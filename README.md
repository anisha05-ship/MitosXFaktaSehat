<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Cek Mitos atau Fakta</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f1f5f9;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .container {
      background: #fff;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.1);
      width: 90%;
      max-width: 500px;
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
      color: #0f172a;
    }

    input[type="text"] {
      width: 100%;
      padding: 12px;
      margin-bottom: 15px;
      border-radius: 10px;
      border: 1px solid #ccc;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 12px;
      background-color: #3b82f6;
      border: none;
      border-radius: 10px;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    button:hover {
      background-color: #2563eb;
    }

    .result {
      margin-top: 20px;
      padding: 15px;
      border-radius: 10px;
      display: none;
    }

    .mitos {
      background-color: #fee2e2;
      color: #991b1b;
    }

    .fakta {
      background-color: #d1fae5;
      color: #065f46;
    }

    .tidak-ditemukan {
      background-color: #fef3c7;
      color: #78350f;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Cek Mitos atau Fakta</h2>
    <input type="text" id="inputMitos" placeholder="Contoh: Makan malam bikin gemuk...">
    <button onclick="cekMitos()">Cek Sekarang</button>
    <div class="result" id="hasilAnalisis"></div>
  </div>

  <script>
    const dataMitos = [
      {
        keyword: "mandi malam",
        hasil: "Fakta: Mandi malam tidak menyebabkan rematik, kecuali jika Anda sudah memiliki kondisi rematik sebelumnya.",
        tipe: "fakta"
      },
      {
        keyword: "makan malam bikin gemuk",
        hasil: "Mitos: Yang bikin gemuk adalah total kalori harian, bukan kapan waktunya kamu makan.",
        tipe: "mitos"
      },
      {
        keyword: "menelan permen karet",
        hasil: "Mitos: Permen karet tidak akan tinggal di perut selama 7 tahun. Tubuh akan mengeluarkannya lewat feses.",
        tipe: "mitos"
      },
      {
        keyword: "baca sambil tiduran merusak mata",
        hasil: "Mitos: Tidak merusak mata permanen, tapi bisa bikin mata cepat lelah.",
        tipe: "mitos"
      },
      {
        keyword: "es bikin batuk",
        hasil: "Mitos: Batuk disebabkan oleh infeksi, bukan karena minum es. Tapi es bisa memperparah gejala jika tenggorokan sedang sensitif.",
        tipe: "mitos"
      },
      {
        keyword: "telur bikin bisul",
        hasil: "Mitos: Bisul disebabkan oleh infeksi bakteri, bukan karena makan telur.",
        tipe: "mitos"
      },
      {
        keyword: "vaksin menyebabkan autisme",
        hasil: "Mitos: Tidak ada bukti ilmiah bahwa vaksin menyebabkan autisme. Itu sudah dibantah banyak penelitian.",
        tipe: "mitos"
      },
      {
        keyword: "mata minus bisa sembuh tanpa kacamata",
        hasil: "Mitos: Mata minus tidak bisa sembuh sendiri. Penggunaan kacamata tidak memperparah atau memperbaiki minus, hanya membantu penglihatan.",
        tipe: "mitos"
      },
      {
        keyword: "makan wortel bikin mata sehat",
        hasil: "Fakta: Wortel mengandung vitamin A yang baik untuk kesehatan mata, tapi tidak bisa menyembuhkan gangguan mata tertentu.",
        tipe: "fakta"
      },
      {
        keyword: "makan wortel bikin mata sehat",
        hasil: "Fakta: Wortel mengandung vitamin A yang baik untuk kesehatan mata, tapi tidak bisa menyembuhkan gangguan mata tertentu.",
        tipe: "fakta"
      },
      {
        keyword: "kerokan menyembuhkan masuk angin",
        hasil: "Fakta sebagian: Bisa membantu melancarkan peredaran darah dan meredakan gejala.",
        tipe: "fakta"
      },
      {
        keyword: "tidur siang bikin gemuk",
        hasil: "Mitos: Tidur siang tidak membuat gemuk jika pola makan tetap seimbang.",
        tipe: "mitos"
      },
      {
        keyword: "minum air es bikin gemuk",
        hasil: "Mitos: Air es tidak mengandung kalori, tidak bikin gemuk.",
        tipe: "mitos"
      },
      {
        keyword: "mata minus bisa sembuh tanpa kacamata",
        hasil: "Mitos: Mata minus tidak bisa sembuh sendiri. Penggunaan kacamata tidak memperparah atau memperbaiki minus, hanya membantu penglihatan.",
        tipe: "mitos"
      },
      {
        keyword: "makan wortel bikin mata sehat",
        hasil: "Fakta: Wortel mengandung vitamin A yang baik untuk kesehatan mata, tapi tidak bisa menyembuhkan gangguan mata tertentu.",
        tipe: "fakta"
      },
      {
        keyword: "minum es saat haid menyebabkan kista",
        hasil: "Mitos: Tidak ada bukti ilmiah bahwa minum es saat haid menyebabkan kista.",
        tipe: "mitos"
      },
      {
        keyword: "tidur dengan kipas angin bisa meninggal",
        hasil: "Mitos: Tidak ada bukti bahwa tidur dengan kipas angin bisa menyebabkan kematian.",
        tipe: "mitos"
      },
      {
        keyword: "memakai bra saat tidur sebabkan kanker payudara",
        hasil: "Mitos: Tidak ada kaitan antara tidur memakai bra dan risiko kanker.",
        tipe: "mitos"
      },
      {
        keyword: "memakai deodoran sebabkan kanker",
        hasil: "Mitos: Belum ada bukti ilmiah kuat yang menghubungkan deodoran dengan kanker.",
        tipe: "mitos"
      },
      {
        keyword: "menahan bersin bisa bikin pembuluh darah pecah",
        hasil: "Mitos: Sangat jarang terjadi, meski menahan bersin bisa terasa tidak nyaman.",
        tipe: "mitos"
      },
      {
        keyword: "patah hati bisa bikin sakit jantung",
        hasil: "Fakta: Kondisi 'broken heart syndrome' secara medis bisa meniru gejala serangan jantung.",
        tipe: "fakta"
      },
      {
        keyword: "jangan mandi saat demam",
        hasil: "Mitos: Mandi air hangat justru bisa membantu menurunkan suhu tubuh.",
        tipe: "mitos"
      },
      {
      keyword: "memakai handphone di pom bensin bisa meledak",
      hasil: "Mitos: Tidak ada bukti nyata insiden ledakan akibat handphone di SPBU.",
      tipe: "mitos"
      },
      {
        keyword: "kalau digigit ular berbisa harus disedot racunnya",
        hasil: "Mitos: Menyedot racun tidak efektif dan bisa memperparah luka.",
        tipe: "mitos"
      },
      {
        keyword: "mematahkan jari menyebabkan radang sendi",
        hasil: "Mitos: Belum terbukti memicu arthritis.",
        tipe: "mitos"
      },
      {
        keyword: "sering begadang bisa bikin gemuk",
        hasil: "Fakta: Kurang tidur bisa ganggu hormon lapar dan meningkatkan risiko kenaikan berat badan.",
        tipe: "fakta"
      },
      {
        keyword: "vitamin C bisa mencegah flu",
        hasil: "Fakta sebagian: Bisa memperkuat imun, tapi tidak menjamin mencegah flu sepenuhnya.",
        tipe: "fakta"
      },
      {
        keyword: "pakai celana ketat bikin mandul",
        hasil: "Mitos: Belum ada bukti kuat, walau bisa memengaruhi kualitas sperma jika terlalu panas.",
        tipe: "mitos"
      },
      {
        keyword: "mengunyah permen karet bikin usus lengket",
        hasil: "Mitos: Tidak ada hubungannya dengan usus lengket.",
        tipe: "mitos"
      },
      {
        keyword: "makan cokelat bikin jerawatan",
        hasil: "Mitos: Tidak ada bukti langsung, jerawat lebih dipengaruhi oleh hormon dan kebersihan kulit.",
        tipe: "mitos"
      },
      {
        keyword: "tidur kepala basah bikin sakit kepala",
        hasil: "Mitos: Tidak ada bukti kuat, meski bisa membuat tubuh merasa tidak nyaman.",
        tipe: "mitos"
      },
      {
        keyword: "pingsan harus disiram air",
        hasil: "Mitos: Yang terbaik adalah baringkan orangnya dan periksa pernapasan.",
        tipe: "mitos"
      },
      {
        keyword: "kalau menstruasi tidak boleh keramas",
        hasil: "Mitos: Keramas saat haid tidak berbahaya, bahkan bisa membantu merasa lebih segar.",
        tipe: "mitos"
      },
      {
        keyword: "baca di tempat gelap merusak mata",
        hasil: "Mitos: Tidak merusak secara permanen, hanya membuat mata cepat lelah.",
        tipe: "mitos"
      },
      {
        keyword: "jangan makan udang dan vitamin C bersama",
        hasil: "Mitos: Tidak ada bukti kuat bahwa kombinasi ini beracun.",
        tipe: "mitos"
      },
      {
        keyword: "daun jambu bisa sembuhkan diare",
        hasil: "Fakta: Beberapa studi menunjukkan efek antibakteri daun jambu untuk diare ringan.",
        tipe: "fakta"
      },
      {
        keyword: "menangis bikin mata minus",
        hasil: "Mitos: Tidak ada kaitan antara menangis dan mata minus.",
        tipe: "mitos"
      },
      {
        keyword: "kalau luka kena darah menstruasi bisa infeksi",
        hasil: "Mitos: Darah haid tidak lebih berbahaya dari darah biasa.",
        tipe: "mitos"
      },
      {
        keyword: "berdiri dekat microwave bisa bahaya",
        hasil: "Mitos: Microwave modern memiliki pelindung yang aman untuk digunakan.",
        tipe: "mitos"
      },
      {
        keyword: "keramas malam bikin masuk angin",
        hasil: "Mitos: Masuk angin bukan istilah medis. Keramas malam tidak masalah jika dikeringkan dengan benar.",
        tipe: "mitos"
      },
      {
        keyword: "minum air dingin bikin lemak menumpuk",
        hasil: "Mitos: Air dingin tidak menyebabkan penumpukan lemak.",
        tipe: "mitos"
      },
      {
        keyword: "sakit kepala karena terlalu banyak mikir",
        hasil: "Fakta sebagian: Stres mental bisa memicu sakit kepala tegang.",
        tipe: "fakta"
      },
      {
        keyword: "makan kulit ayam bikin kolesterol tinggi",
        hasil: "Fakta: Kulit ayam mengandung lemak jenuh yang dapat menaikkan kolesterol jika dikonsumsi berlebihan.",
        tipe: "fakta"
      },
      {
        keyword: "gigi berlubang bisa sembuh sendiri",
        hasil: "Mitos: Gigi berlubang perlu ditangani oleh dokter gigi.",
        tipe: "mitos"
      },
      {
        keyword: "buah semangka bisa sebabkan pilek",
        hasil: "Mitos: Pilek disebabkan oleh virus, bukan buah.",
        tipe: "mitos"
      },
      {
        keyword: "berkeringat artinya sudah sembuh dari demam",
        hasil: "Fakta sebagian: Berkeringat bisa menurunkan suhu tubuh, tapi belum tentu sembuh total.",
        tipe: "fakta"
      },
      {
        keyword: "tidak sarapan bikin maag",
        hasil: "Fakta: Lambung kosong terlalu lama bisa meningkatkan risiko iritasi lambung.",
        tipe: "fakta"
      },
      {
        keyword: "keringat di kepala bayi tanda kepintaran",
        hasil: "Mitos: Itu hanya respons suhu tubuh, bukan tanda kecerdasan.",
        tipe: "mitos"
      },
      {
        keyword: "makan durian dengan soda bisa mematikan",
        hasil: "Mitos: Tidak ada bukti ilmiah bahwa kombinasi ini berbahaya.",
        tipe: "mitos"
      },
      {
        keyword: "kalau cegukan harus kagetin",
        hasil: "Mitos: Kaget belum tentu efektif mengatasi cegukan.",
        tipe: "mitos"
      },
      {
        keyword: "duduk di lantai bikin wasir",
        hasil: "Mitos: Wasir lebih disebabkan oleh konstipasi dan tekanan berlebih saat BAB.",
        tipe: "mitos"
      },
      {
        keyword: "cepat tumbuh uban kalau sering stres",
        hasil: "Fakta: Stres berat bisa mempercepat munculnya uban pada beberapa orang.",
        tipe: "fakta"
      },
      {
        keyword: "air putih hangat lebih sehat dari air dingin",
        hasil: "Fakta sebagian: Air hangat bisa membantu pencernaan, tapi tidak lebih 'sehat' secara mutlak.",
        tipe: "fakta"
      },
      {
        keyword: "tidur pagi bikin tubuh lemas",
        hasil: "Fakta sebagian: Tidur pagi bisa ganggu ritme sirkadian dan kualitas tidur malam.",
        tipe: "fakta"
      },
      {
        keyword: "menutup kepala bayi biar nggak masuk angin",
        hasil: "Fakta sebagian: Bayi memang sensitif terhadap suhu, jadi menutup kepala bisa membantu kehangatan.",
        tipe: "fakta"
      },
      {
        keyword: "jangan makan nasi dan mie bersamaan",
        hasil: "Fakta sebagian: Kombinasi ini tinggi karbohidrat, tapi bukan larangan mutlak.",
        tipe: "fakta"
      },
      {
        keyword: "daging kambing bikin darah tinggi",
        hasil: "Fakta sebagian: Daging kambing bisa meningkatkan tekanan darah jika dikonsumsi berlebihan.",
        tipe: "fakta"
      },
      {
        keyword: "minum es bikin batuk",
        hasil: "Mitos: Batuk disebabkan oleh infeksi virus atau bakteri, bukan karena suhu minuman.",
        tipe: "mitos"
      },
      {
        keyword: "berkeringat banyak berarti membakar lemak",
        hasil: "Mitos: Keringat adalah respon tubuh terhadap panas, bukan indikator pembakaran lemak.",
        tipe: "mitos"
      },
      {
        keyword: "tidur tanpa bantal lebih sehat",
        hasil: "Fakta sebagian: Tidur tanpa bantal bisa baik untuk punggung, tapi tidak cocok untuk semua posisi tidur.",
        tipe: "fakta"
      },
      {
        keyword: "kulit mengelupas tandanya kulit bersih",
        hasil: "Mitos: Kulit mengelupas bisa jadi tanda iritasi atau kerusakan, bukan kebersihan.",
        tipe: "mitos"
      },
      {
        keyword: "tekanan darah rendah selalu berbahaya",
        hasil: "Mitos: Tekanan darah rendah belum tentu berbahaya jika tidak menimbulkan gejala.",
        tipe: "mitos"
      },
      {
        keyword: "makan sayur bayam setiap hari bikin keracunan zat besi",
        hasil: "Mitos: Bayam aman dikonsumsi setiap hari dalam jumlah wajar.",
        tipe: "mitos"
      },
      {
        keyword: "minum vitamin C bisa mencegah flu",
        hasil: "Fakta sebagian: Vitamin C dapat memperkuat sistem imun, tapi tidak menjamin flu tidak terjadi.",
        tipe: "fakta"
      },
      {
        keyword: "menahan bersin bisa berbahaya",
        hasil: "Fakta: Menahan bersin bisa berisiko cedera pembuluh darah atau sinus.",
        tipe: "fakta"
      },
      {
        keyword: "makan pisang bikin ngantuk",
        hasil: "Fakta sebagian: Pisang mengandung magnesium dan triptofan yang bisa menenangkan tubuh.",
        tipe: "fakta"
      },
      {
        keyword: "memotong rambut sering bikin cepat panjang",
        hasil: "Mitos: Pertumbuhan rambut berasal dari akar, bukan ujungnya.",
        tipe: "mitos"
      },
      {
        keyword: "mengunyah permen karet bisa bantu konsentrasi",
        hasil: "Fakta: Beberapa studi menunjukkan permen karet bisa membantu fokus sementara.",
        tipe: "fakta"
      },
      {
        keyword: "anak yang aktif pasti sehat",
        hasil: "Fakta sebagian: Anak aktif bisa jadi sehat, tapi tetap perlu pemantauan gizi dan tumbuh kembang.",
        tipe: "fakta"
      },
      {
        keyword: "kerokan bisa sembuhkan masuk angin",
        hasil: "Mitos: Kerokan hanya memberi sensasi hangat, tidak menyembuhkan penyakit tertentu.",
        tipe: "mitos"
      },
      {
        keyword: "berjemur jam 12 siang bagus untuk vitamin D",
        hasil: "Mitos: Paparan sinar matahari terbaik umumnya antara pukul 9–10 pagi.",
        tipe: "mitos"
      },
      {
        keyword: "air dingin memperlambat pencernaan",
        hasil: "Mitos: Tidak ada bukti ilmiah kuat bahwa air dingin memperlambat pencernaan.",
        tipe: "mitos"
      },
      {
        keyword: "jangan mandi saat demam",
        hasil: "Mitos: Mandi air hangat justru bisa membantu menurunkan suhu tubuh saat demam.",
        tipe: "mitos"
      },
      {
        keyword: "madu aman untuk bayi",
        hasil: "Mitos: Madu tidak disarankan untuk bayi di bawah 1 tahun karena risiko botulisme.",
        tipe: "mitos"
      },
      {
        keyword: "makan cokelat bikin jerawatan",
        hasil: "Mitos: Jerawat lebih dipengaruhi hormon dan kebersihan kulit, bukan cokelat.",
        tipe: "mitos"
      },
      {
        keyword: "daun jambu biji bisa obati diare",
        hasil: "Fakta: Beberapa studi mendukung manfaat daun jambu biji untuk meredakan diare.",
        tipe: "fakta"
      },
      {
        keyword: "sering keramas bikin rambut rontok",
        hasil: "Mitos: Rontok biasanya karena faktor hormonal atau akar rambut yang lemah.",
        tipe: "mitos"
      },
      {
        keyword: "olahraga saat haid tidak boleh",
        hasil: "Mitos: Olahraga ringan justru bisa membantu meredakan nyeri haid.",
        tipe: "mitos"
      },
      {
        keyword: "makanan pedas bisa sebabkan maag",
        hasil: "Mitos: Maag disebabkan oleh infeksi H. pylori atau pola makan buruk, bukan hanya pedas.",
        tipe: "mitos"
      },
      {
        keyword: "tidur sehabis makan bikin gemuk",
        hasil: "Fakta sebagian: Tidur segera setelah makan bisa mengganggu pencernaan dan berkontribusi ke penambahan berat badan.",
        tipe: "fakta"
      },
      {
        keyword: "minum susu bikin dahak tambah banyak",
        hasil: "Mitos: Tidak ada bukti bahwa susu memperbanyak dahak, hanya memberi sensasi kental di mulut.",
        tipe: "mitos"
      },
      {
        keyword: "mengupil bisa menyebabkan infeksi",
        hasil: "Fakta: Mengupil terlalu dalam bisa melukai hidung dan menimbulkan infeksi.",
        tipe: "fakta"
      },
      {
        keyword: "pakai earphone terlalu lama bisa bikin tuli",
        hasil: "Fakta: Paparan suara keras dalam waktu lama bisa merusak pendengaran.",
        tipe: "fakta"
      },
      {
        keyword: "sarapan bisa bantu turunkan berat badan",
        hasil: "Fakta: Sarapan bergizi bisa membantu mengatur nafsu makan sepanjang hari.",
        tipe: "fakta"
      },
      {
        keyword: "tidak BAB setiap hari artinya sembelit",
        hasil: "Mitos: Frekuensi BAB tiap orang berbeda, sembelit jika sulit atau jarang lebih dari 3 kali seminggu.",
        tipe: "mitos"
      },
      {
        keyword: "kulit gelap lebih kuat terhadap sinar matahari",
        hasil: "Fakta sebagian: Kulit gelap punya lebih banyak melanin, tapi tetap perlu perlindungan dari UV.",
        tipe: "fakta"
      },
      {
        keyword: "kalau lapar berarti tubuh kekurangan energi",
        hasil: "Fakta: Rasa lapar adalah sinyal bahwa tubuh butuh asupan energi.",
        tipe: "fakta"
      },
      {
        keyword: "makan malam setelah jam 7 bikin gemuk",
        hasil: "Mitos: Kegemukan ditentukan oleh total kalori harian, bukan jam makan.",
        tipe: "mitos"
      },
      {
        keyword: "makan wortel bikin mata sehat",
        hasil: "Fakta: Wortel mengandung vitamin A yang penting untuk kesehatan mata.",
        tipe: "fakta"
      },
      {
        keyword: "kentang bikin gemuk",
        hasil: "Mitos: Kentang rebus sehat dan kaya nutrisi, tergantung cara pengolahan dan porsinya.",
        tipe: "mitos"
      },
      {
        keyword: "tidur dengan kipas angin bisa mati mendadak",
        hasil: "Mitos: Tidak ada bukti ilmiah yang mendukung klaim ini.",
        tipe: "mitos"
      },
      {
        keyword: "senyum bisa memperbaiki suasana hati",
        hasil: "Fakta: Tersenyum dapat memicu otak melepaskan hormon bahagia.",
        tipe: "fakta"
      },
      {
        keyword: "terlalu sering minum air bisa ganggu ginjal",
        hasil: "Mitos: Minum air cukup justru baik untuk fungsi ginjal.",
        tipe: "mitos"
      },
      {
        keyword: "minum lemon pagi hari bantu detoks tubuh",
        hasil: "Fakta sebagian: Lemon bantu hidrasi dan pencernaan, tapi tubuh punya sistem detoks alami sendiri.",
        tipe: "fakta"
      },
      {
        keyword: "makanan cepat saji bikin ketagihan",
        hasil: "Fakta: Kandungan lemak, gula, dan garam tinggi bisa merangsang otak untuk terus menginginkannya.",
        tipe: "fakta"
      },
      {
        keyword: "minum air kelapa setelah olahraga bikin tubuh segar",
        hasil: "Fakta: Air kelapa membantu rehidrasi karena mengandung elektrolit alami.",
        tipe: "fakta"
      },
      {
        keyword: "tidur siang bikin malas",
        hasil: "Mitos: Tidur siang singkat bisa meningkatkan produktivitas.",
        tipe: "mitos"
      },
      {
        keyword: "mengupil bisa menyebarkan virus",
        hasil: "Fakta: Tangan kotor saat mengupil bisa memasukkan kuman ke tubuh.",
        tipe: "fakta"
      },
      {
        keyword: "menghentikan antibiotik sebelum waktunya tidak apa-apa",
        hasil: "Mitos: Menghentikan antibiotik terlalu cepat bisa menyebabkan resistensi bakteri.",
        tipe: "mitos"
      },
      {
        keyword: "duduk terlalu lama buruk untuk kesehatan",
        hasil: "Fakta: Duduk lama dapat meningkatkan risiko penyakit jantung dan diabetes.",
        tipe: "fakta"
      },
      {
        keyword: "berjemur terlalu lama bisa sebabkan kanker kulit",
        hasil: "Fakta: Paparan sinar UV berlebihan dapat merusak kulit dan meningkatkan risiko kanker.",
        tipe: "fakta"
      },
      {
        keyword: "air putih bisa mengatasi sembelit",
        hasil: "Fakta: Air putih membantu melancarkan pencernaan dan mencegah sembelit.",
        tipe: "fakta"
      },
      {
        keyword: "tidur larut malam bikin wajah kusam",
        hasil: "Fakta: Kurang tidur bisa mengganggu regenerasi kulit dan menyebabkan wajah terlihat lelah.",
        tipe: "fakta"
      },
      {
        keyword: "kurma baik untuk penderita anemia",
        hasil: "Fakta: Kurma mengandung zat besi yang bisa membantu atasi anemia.",
        tipe: "fakta"
      },
      {
        keyword: "berdiri setelah makan langsung bisa sebabkan maag",
        hasil: "Mitos: Berdiri setelah makan tidak menyebabkan maag, justru bisa bantu pencernaan.",
        tipe: "mitos"
      },
      {
        keyword: "minum air hangat lebih sehat dari air dingin",
        hasil: "Fakta sebagian: Air hangat bisa membantu pencernaan, tapi secara hidrasi, keduanya sama efektif.",
        tipe: "fakta"
      },
      {
        keyword: "berjemur bisa membunuh virus di tubuh",
        hasil: "Mitos: Berjemur membantu produksi vitamin D, tapi tidak membunuh virus dalam tubuh.",
        tipe: "mitos"
      },
      {
        keyword: "semua orang butuh 8 gelas air per hari",
        hasil: "Mitos: Kebutuhan air tiap orang berbeda tergantung usia, aktivitas, dan kondisi tubuh.",
        tipe: "mitos"
      },
      {
        keyword: "vitamin C bisa mencegah flu",
        hasil: "Fakta sebagian: Vitamin C bisa memperkuat sistem imun, tapi tidak sepenuhnya mencegah flu.",
        tipe: "fakta"
      },
      {
        keyword: "makan tengah malam bikin gemuk",
        hasil: "Mitos: Yang penting adalah jumlah kalori harian, bukan jam makannya.",
        tipe: "mitos"
      },
      {
        keyword: "makan cokelat bikin jerawatan",
        hasil: "Mitos: Jerawat lebih dipengaruhi hormon dan kebersihan kulit, bukan cokelat.",
        tipe: "mitos"
      },
      {
        keyword: "patah tulang bisa sembuh lebih cepat dengan minum susu",
        hasil: "Fakta sebagian: Susu mengandung kalsium yang bantu pembentukan tulang, tapi tidak mempercepat penyembuhan secara langsung.",
        tipe: "fakta"
      },
      {
        keyword: "semua lemak itu buruk",
        hasil: "Mitos: Lemak sehat seperti omega-3 sangat dibutuhkan tubuh.",
        tipe: "mitos"
      },
      {
        keyword: "banyak tidur bikin gemuk",
        hasil: "Mitos: Kualitas tidur yang buruk justru lebih berkontribusi terhadap kenaikan berat badan.",
        tipe: "mitos"
      },
      {
        keyword: "detoks pakai jus bisa membersihkan racun",
        hasil: "Mitos: Tubuh sudah punya organ detoks alami seperti hati dan ginjal.",
        tipe: "mitos"
      },
      {
        keyword: "diet tanpa karbohidrat paling sehat",
        hasil: "Mitos: Karbohidrat kompleks penting untuk energi dan fungsi otak.",
        tipe: "mitos"
      },
      {
        keyword: "pakai deodoran bisa sebabkan kanker payudara",
        hasil: "Mitos: Tidak ada bukti ilmiah yang mendukung klaim ini.",
        tipe: "mitos"
      },
      {
        keyword: "makan banyak telur menyebabkan kolesterol tinggi",
        hasil: "Fakta sebagian: Telur memang mengandung kolesterol, tapi tidak selalu berdampak langsung pada kadar kolesterol darah.",
        tipe: "fakta"
      },
      {
        keyword: "minum susu bikin tubuh tinggi",
        hasil: "Fakta sebagian: Susu mendukung pertumbuhan, tapi tinggi badan juga dipengaruhi genetik dan faktor lain.",
        tipe: "fakta"
      },
      {
        keyword: "makan sebelum tidur bikin mimpi buruk",
        hasil: "Mitos: Tidak ada bukti ilmiah yang mendukung klaim ini.",
        tipe: "mitos"
      },
      {
        keyword: "mengangkat beban bikin perempuan jadi kekar",
        hasil: "Mitos: Hormon perempuan berbeda, sulit membentuk otot besar seperti laki-laki tanpa latihan ekstrem.",
        tipe: "mitos"
      },
      {
        keyword: "kuku putih tandanya tubuh sehat",
        hasil: "Mitos: Warna kuku bisa bervariasi karena banyak faktor, tidak selalu jadi indikator utama kesehatan.",
        tipe: "mitos"
      },
      {
        keyword: "keringat tanda lemak terbakar",
        hasil: "Mitos: Berkeringat menunjukkan tubuh mengatur suhu, bukan berarti lemak terbakar langsung.",
        tipe: "mitos"
      },
      {
        keyword: "minyak zaitun lebih sehat dari minyak biasa",
        hasil: "Fakta: Minyak zaitun mengandung lemak tak jenuh tunggal yang baik untuk jantung.",
        tipe: "fakta"
      },
      {
        keyword: "semua makanan sehat itu mahal",
        hasil: "Mitos: Banyak makanan sehat seperti sayur dan buah lokal yang terjangkau.",
        tipe: "mitos"
      },
      {
        keyword: "minum kopi bikin ketergantungan",
        hasil: "Fakta sebagian: Konsumsi berlebihan bisa menyebabkan ketergantungan ringan karena kafein.",
        tipe: "fakta"
      },
      {
        keyword: "gula alami seperti madu lebih sehat dari gula putih",
        hasil: "Fakta sebagian: Madu mengandung sedikit nutrisi tambahan, tapi tetap tinggi kalori.",
        tipe: "fakta"
      },
      {
        keyword: "cuka apel bisa sembuhkan semua penyakit",
        hasil: "Mitos: Cuka apel punya manfaat, tapi bukan obat segala penyakit.",
        tipe: "mitos"
      },
      {
        keyword: "semakin banyak olahraga semakin sehat",
        hasil: "Mitos: Olahraga berlebihan bisa menimbulkan cedera atau kelelahan.",
        tipe: "mitos"
      },
      {
        keyword: "makanan organik selalu lebih bergizi",
        hasil: "Mitos: Tidak selalu lebih bergizi, meskipun lebih sedikit pestisida.",
        tipe: "mitos"
      },
      {
        keyword: "tidak sarapan bikin gemuk",
        hasil: "Fakta sebagian: Tidak sarapan bisa menyebabkan makan berlebih di siang hari, tapi efeknya tergantung pola makan harian.",
        tipe: "fakta"
      },
      {
        keyword: "olahraga pagi lebih baik daripada sore",
        hasil: "Mitos: Waktu olahraga terbaik adalah saat tubuh paling nyaman.",
        tipe: "mitos"
      },
      {
        keyword: "kurangi makan garam bisa cegah hipertensi",
        hasil: "Fakta: Garam berlebih meningkatkan tekanan darah.",
        tipe: "fakta"
      },
      {
        keyword: "makanan beku tidak sehat",
        hasil: "Mitos: Banyak makanan beku tetap bergizi jika tidak ditambah pengawet berbahaya.",
        tipe: "mitos"
      },
      {
        keyword: "jika tidak lapar, tidak perlu makan",
        hasil: "Fakta sebagian: Kadang tubuh menunda rasa lapar saat stres, tapi asupan tetap penting untuk energi.",
        tipe: "fakta"
      },
      {
        keyword: "semua vitamin bisa dikonsumsi berlebihan tanpa efek samping",
        hasil: "Mitos: Beberapa vitamin seperti A dan D bisa toksik jika berlebihan.",
        tipe: "mitos"
      },
      {
        keyword: "berdiri lama bisa sebabkan varises",
        hasil: "Fakta: Berdiri lama bisa meningkatkan tekanan di pembuluh darah dan memicu varises.",
        tipe: "fakta"
      },
      {
        keyword: "obat herbal selalu aman karena alami",
        hasil: "Mitos: Obat herbal tetap bisa punya efek samping atau interaksi obat.",
        tipe: "mitos"
      },
      {
        keyword: "kalori dari buah tidak bikin gemuk",
        hasil: "Mitos: Kalori tetaplah kalori, konsumsi berlebih bisa menambah berat badan.",
        tipe: "mitos"
      },
      {
        keyword: "jika merasa sehat, tidak perlu cek kesehatan",
        hasil: "Mitos: Deteksi dini penting bahkan saat tidak ada gejala.",
        tipe: "mitos"
      },
      {
        keyword: "menahan buang air kecil bisa sebabkan infeksi",
        hasil: "Fakta: Menahan urin terlalu lama bisa meningkatkan risiko infeksi saluran kemih.",
        tipe: "fakta"
      },
      {
        keyword: "buah lebih baik dari jus buah",
        hasil: "Fakta: Buah utuh mengandung serat yang hilang saat dibuat jus.",
        tipe: "fakta"
      },
      {
        keyword: "sakit kepala berarti harus minum obat",
        hasil: "Mitos: Tidak semua sakit kepala butuh obat, bisa diredakan dengan istirahat atau hidrasi.",
        tipe: "mitos"
      },
      {
        keyword: "pusing berarti tekanan darah rendah",
        hasil: "Mitos: Pusing bisa disebabkan banyak hal, tidak selalu karena tekanan darah rendah.",
        tipe: "mitos"
      },
      {
        keyword: "semakin banyak keringat, semakin sehat",
        hasil: "Mitos: Keringat tidak selalu menandakan kebugaran atau pembakaran kalori.",
        tipe: "mitos"
      },
      {
        keyword: "makanan pedas bisa menyembuhkan flu",
        hasil: "Mitos: Pedas bisa melonggarkan lendir, tapi tidak menyembuhkan infeksi.",
        tipe: "mitos"
      },
      {
        keyword: "berat badan ideal berarti pasti sehat",
        hasil: "Mitos: Kesehatan tidak hanya dilihat dari berat badan, tapi juga kebugaran dan gaya hidup.",
        tipe: "mitos"
      },
      {
        keyword: "merokok setelah makan lebih nikmat",
        hasil: "Mitos: Merokok tetap berbahaya kapan pun dilakukan.",
        tipe: "mitos"
      },
      {
        keyword: "tekanan darah tinggi selalu ada gejala",
        hasil: "Mitos: Hipertensi sering tanpa gejala dan disebut 'silent killer'.",
        tipe: "mitos"
      },
      {
        keyword: "berdengkur tanda tidur nyenyak",
        hasil: "Mitos: Berdengkur bisa jadi tanda sleep apnea yang berbahaya.",
        tipe: "mitos"
      },
      {
        keyword: "terapi bekam bisa menyembuhkan semua penyakit",
        hasil: "Mitos: Belum ada bukti ilmiah kuat bahwa bekam menyembuhkan semua penyakit.",
        tipe: "mitos"
      },
    ];
    
    function cekMitos() {
      const input = document.getElementById('inputMitos').value.toLowerCase();
      const hasil = document.getElementById('hasilAnalisis');

      const ditemukan = dataMitos.find(item => input.includes(item.keyword));

      if (ditemukan) {
        hasil.innerHTML = ditemukan.hasil;
        hasil.className = 'result ' + ditemukan.tipe;
      } else {
        hasil.innerHTML = "Maaf, kami belum punya informasi tentang itu.";
        hasil.className = 'result tidak-ditemukan';
      }

      hasil.style.display = "block";
    }
  </script>
</body>
</html>
