<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [

  { "en": "Definisi telekomunikasi?", "id": "Komunikasi jarak jauh melalui media." },
  { "en": "Komponen dasar sistem telekomunikasi?", "id": "Transmitter, channel, dan receiver." },
  { "en": "Fungsi transmitter (pengirim)?", "id": "Mengubah informasi menjadi sinyal." },
  { "en": "Fungsi channel (saluran)?", "id": "Media perambatan sinyal." },
  { "en": "Fungsi receiver (penerima)?", "id": "Mengubah sinyal kembali menjadi informasi." },
  { "en": "Apa itu sinyal?", "id": "Besaran fisik pembawa informasi." },
  { "en": "Apa itu noise?", "id": "Gangguan yang merusak sinyal." },
  { "en": "Apa itu modulasi?", "id": "Proses menumpangkan sinyal informasi." },
  { "en": "Sinyal yang ditumpangi?", "id": "Sinyal pembawa (carrier wave)." },
  { "en": "Tiga jenis modulasi analog?", "id": "AM, FM, dan PM." },
  { "en": "Kepanjangan AM (Amplitude Modulation)?", "id": "Amplitude Modulation." },
  { "en": "Prinsip kerja AM (Amplitude Modulation)?", "id": "Amplitudo carrier diubah sesuai informasi." },
  { "en": "Kepanjangan FM (Frequency Modulation)?", "id": "Frequency Modulation." },
  { "en": "Prinsip kerja FM (Frequency Modulation)?", "id": "Frekuensi carrier diubah sesuai informasi." },
  { "en": "Kepanjangan PM (Phase Modulation)?", "id": "Phase Modulation." },
  { "en": "Apa itu spektrum elektromagnetik?", "id": "Rentang semua jenis radiasi elektromagnetik." },
  { "en": "Apa itu frekuensi radio (RF)?", "id": "Frekuensi untuk komunikasi nirkabel." },
  { "en": "Kepanjangan VHF (Very High Frequency)?", "id": "Very High Frequency." },
  { "en": "Rentang frekuensi VHF (Very High Frequency)?", "id": "30 MHz hingga 300 MHz." },
  { "en": "Kepanjangan UHF (Ultra High Frequency)?", "id": "Ultra High Frequency." },
  { "en": "Rentang frekuensi UHF (Ultra High Frequency)?", "id": "300 MHz hingga 3 GHz." },
  { "en": "Fungsi utama antena?", "id": "Mengubah sinyal listrik ke gelombang." },
  { "en": "Tipe antena dasar?", "id": "Monopole, dipole." },
  { "en": "Antena omnidirectional memancar ke?", "id": "Segala arah secara merata." },
  { "en": "Antena directional memancar ke?", "id": "Satu arah tertentu." },
  { "en": "Apa itu propagasi gelombang?", "id": "Cara gelombang merambat di ruang." },
  { "en": "Propagasi 'Line-of-Sight' (LOS)?", "id": "Gelombang lurus tanpa halangan." },
  { "en": "Apa itu refleksi (reflection)?", "id": "Gelombang memantul dari permukaan." },
  { "en": "Apa itu difraksi (diffraction)?", "id": "Gelombang membelok di sekitar halangan." },
  { "en": "Apa itu atenuasi (attenuation)?", "id": "Pelemahan sinyal seiring jarak." },
  { "en": "Kelebihan radio FM dibanding AM?", "id": "Kualitas suara lebih baik, tahan noise." },
  { "en": "Kelemahan radio AM?", "id": "Rentan terhadap gangguan atau noise." },
  { "en": "Beda komunikasi digital & analog?", "id": "Digital pakai bit, analog kontinu." },
  { "en": "Proses ubah sinyal analog-digital?", "id": "Menggunakan ADC (Analog-to-Digital Converter)." },
  { "en": "Proses ubah sinyal digital-analog?", "id": "Menggunakan DAC (Digital-to-Analog Converter)." },
  { "en": "Kepanjangan ASK (Amplitude-Shift Keying)?", "id": "Amplitude-Shift Keying." },
  { "en": "Kepanjangan FSK (Frequency-Shift Keying)?", "id": "Frequency-Shift Keying." },
  { "en": "Kepanjangan PSK (Phase-Shift Keying)?", "id": "Phase-Shift Keying." },
  { "en": "Kepanjangan HT (Handy Talkie)?", "id": "Handy Talkie." },
  { "en": "Fungsi utama HT (Handy Talkie) Polri?", "id": "Alat komunikasi portabel di lapangan." },
  { "en": "Fungsi utama repeater?", "id": "Memperluas jangkauan sinyal radio." },
  { "en": "Apa itu mode simplex?", "id": "Komunikasi satu arah secara bergantian." },
  { "en": "Apa itu mode duplex?", "id": "Komunikasi dua arah." },
  { "en": "Beda half-duplex & full-duplex?", "id": "Half bergantian, full bersamaan." },
  { "en": "Mode komunikasi pada HT (Handy Talkie)?", "id": "Umumnya mode simplex atau half-duplex." },
  { "en": "Apa itu 'bandwidth'?", "id": "Lebar pita frekuensi yang digunakan." },
  { "en": "Satuan 'bandwidth'?", "id": "Hertz (Hz)." },
  { "en": "Apa itu 'channel spacing'?", "id": "Jarak antar frekuensi kanal." },
  { "en": "Fungsi 'squelch' pada radio?", "id": "Meredam noise saat tidak ada sinyal." },
  { "en": "Apa itu 'gain' antena?", "id": "Kemampuan antena memfokuskan sinyal." },
  { "en": "Satuan 'gain' antena?", "id": "dBi atau dBd." },
  { "en": "Apa itu polarisasi gelombang?", "id": "Orientasi getaran medan listrik." },
  { "en": "Contoh polarisasi antena?", "id": "Vertikal dan Horisontal." },
  { "en": "Apa itu 'callsign'?", "id": "Tanda panggil unik stasiun radio." },
  { "en": "Jaringan telepon seluler disebut?", "id": "Jaringan seluler (cellular network)." },
  { "en": "Area cakupan satu menara seluler?", "id": "Sel (cell)." },
  { "en": "Peralihan antar sel disebut?", "id": "Handover atau handoff." },
  { "en": "Kepanjangan BTS (Base Transceiver Station)?", "id": "Base Transceiver Station." },
  { "en": "Fungsi BTS (Base Transceiver Station)?", "id": "Menghubungkan perangkat mobile ke jaringan." },
  { "en": "Generasi pertama jaringan seluler?", "id": "1G (menggunakan sinyal analog)." },
  { "en": "Kepanjangan GSM (Global System for Mobile Communications)?", "id": "Global System for Mobile Communications." },
  { "en": "Kepanjangan CDMA (Code Division Multiple Access)?", "id": "Code Division Multiple Access." },
  { "en": "Teknologi pada jaringan 2G?", "id": "GSM dan CDMA." },
  { "en": "Fitur utama jaringan 3G?", "id": "Layanan data internet mobile." },
  { "en": "Fitur utama jaringan 4G?", "id": "Kecepatan data tinggi (broadband)." },
  { "en": "Kepanjangan LTE (Long-Term Evolution)?", "id": "Long-Term Evolution." },
  { "en": "LTE (Long-Term Evolution) adalah teknologi generasi?", "id": "Generasi keempat (4G)." },
  { "en": "Fitur utama jaringan 5G?", "id": "Kecepatan sangat tinggi, latensi rendah." },
  { "en": "Apa itu 'multiplexing'?", "id": "Menggabungkan banyak sinyal jadi satu." },
  { "en": "Tipe-tipe 'multiplexing'?", "id": "FDM, TDM, CDM." },
  { "en": "Kepanjangan FDM (Frequency Division Multiplexing)?", "id": "Frequency Division Multiplexing." },
  { "en": "Prinsip FDM (Frequency Division Multiplexing)?", "id": "Bagi-bagi frekuensi." },
  { "en": "Kepanjangan TDM (Time Division Multiplexing)?", "id": "Time Division Multiplexing." },
  { "en": "Prinsip TDM (Time Division Multiplexing)?", "id": "Bagi-bagi slot waktu." },
  { "en": "Kepanjangan CDM (Code Division Multiplexing)?", "id": "Code Division Multiplexing." },
  { "en": "Prinsip CDM (Code Division Multiplexing)?", "id": "Setiap sinyal punya kode unik." },
  { "en": "Apa itu 'sampling rate'?", "id": "Jumlah sampel per detik." },
  { "en": "Teorema sampling dasar?", "id": "Teorema Nyquist-Shannon." },
  { "en": "Bunyi teorema Nyquist?", "id": "Sampling rate > 2x frekuensi maks." },
  { "en": "Efek jika sampling rate kurang?", "id": "Aliasing." },
  { "en": "Apa itu 'aliasing'?", "id": "Sinyal frekuensi tinggi tampak rendah." },
  { "en": "Media transmisi terpandu?", "id": "Kabel tembaga, fiber optik." },
  { "en": "Media transmisi tidak terpandu?", "id": "Udara (gelombang radio)." },
  { "en": "Kelebihan fiber optik?", "id": "Kapasitas besar, tahan interferensi." },
  { "en": "Apa itu interferensi elektromagnetik (EMI)?", "id": "Gangguan dari sinyal elektromagnetik lain." },
  { "en": "Kepanjangan EMI (Electromagnetic Interference)?", "id": "Electromagnetic Interference." },
  { "en": "Apa itu 'crosstalk'?", "id": "Interferensi antar kabel yang berdekatan." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah perubahan simbol per detik." },
  { "en": "Beda 'bit rate' & 'baud rate'?", "id": "Bit rate bisa lebih tinggi." },
  { "en": "Apa itu 'demodulation'?", "id": "Proses memisahkan sinyal informasi." },
  { "en": "Perangkat untuk modulasi-demodulasi?", "id": "Modem." },
  { "en": "Sistem komunikasi satelit?", "id": "Menggunakan satelit sebagai repeater." },
  { "en": "Orbit satelit geostasioner (GEO)?", "id": "Orbit tetap di atas khatulistiwa." },
  { "en": "Kelebihan satelit GEO?", "id": "Cakupan luas, antena tidak perlu gerak." },
  { "en": "Kelemahan satelit GEO?", "id": "Latensi tinggi (delay)." },
  { "en": "Orbit satelit LEO (Low Earth Orbit)?", "id": "Orbit rendah, bergerak cepat." },
  { "en": "Kelebihan satelit LEO?", "id": "Latensi sangat rendah." },
  { "en": "Kelemahan satelit LEO?", "id": "Butuh banyak satelit untuk cakupan." },
  { "en": "Kepanjangan MSC (Mobile Switching Center)?", "id": "Mobile Switching Center." },
  { "en": "Fungsi MSC (Mobile Switching Center)?", "id": "Pusat penyambungan panggilan seluler." },
  { "en": "Kepanjangan HLR (Home Location Register)?", "id": "Home Location Register." },
  { "en": "Fungsi HLR (Home Location Register)?", "id": "Database utama informasi pelanggan." },
  { "en": "Kepanjangan VLR (Visitor Location Register)?", "id": "Visitor Location Register." },
  { "en": "Fungsi VLR (Visitor Location Register)?", "id": "Database sementara pelanggan roaming." },
  { "en": "Kepanjangan VoLTE (Voice over LTE)?", "id": "Voice over Long-Term Evolution." },
  { "en": "Keuntungan VoLTE (Voice over LTE)?", "id": "Panggilan suara jernih via jaringan 4G." },
  { "en": "Kepanjangan MIMO (Multiple-Input Multiple-Output)?", "id": "Multiple-Input Multiple-Output." },
  { "en": "Fungsi MIMO (Multiple-Input Multiple-Output)?", "id": "Gunakan banyak antena, tingkatkan kecepatan." },
  { "en": "Apa itu 'small cells'?", "id": "Base station kecil berdaya rendah." },
  { "en": "Tujuan 'small cells'?", "id": "Meningkatkan kapasitas jaringan di area padat." },
  { "en": "Standar teknis untuk Wi-Fi?", "id": "IEEE 802.11." },
  { "en": "Frekuensi Wi-Fi 802.11b/g/n?", "id": "2.4 GHz." },
  { "en": "Frekuensi Wi-Fi 802.11a/ac/ax?", "id": "5 GHz (dan 2.4 GHz)." },
  { "en": "Nama lain Wi-Fi 6?", "id": "802.11ax." },
  { "en": "Apa itu 'channel bonding'?", "id": "Menggabungkan kanal untuk bandwidth lebih besar." },
  { "en": "Fungsi enkripsi pada Wi-Fi?", "id": "Mengamankan jaringan dari akses ilegal." },
  { "en": "Standar enkripsi Wi-Fi kuat?", "id": "WPA2 atau WPA3." },
  { "en": "Fungsi Bluetooth?", "id": "Komunikasi nirkabel jarak sangat pendek." },
  { "en": "Kepanjangan NFC (Near Field Communication)?", "id": "Near Field Communication." },
  { "en": "Jarak maksimal NFC (Near Field Communication)?", "id": "Beberapa sentimeter saja." },
  { "en": "Penggunaan umum NFC (Near Field Communication)?", "id": "Pembayaran nirkontak, pairing perangkat." },
  { "en": "Teknologi nirkabel untuk IoT (Internet of Things)?", "id": "Zigbee dan Z-Wave." },
  { "en": "Kepanjangan DMR (Digital Mobile Radio)?", "id": "Digital Mobile Radio." },
  { "en": "Keunggulan radio DMR (Digital Mobile Radio)?", "id": "Suara jernih, efisiensi frekuensi." },
  { "en": "Teknologi akses DMR (Digital Mobile Radio)?", "id": "TDMA (Time Division Multiple Access)." },
  { "en": "Kepanjangan TDMA (Time Division Multiple Access)?", "id": "Time Division Multiple Access." },
  { "en": "Standar radio digital Polri lain?", "id": "TETRA atau P25." },
  { "en": "Kepanjangan AES (Advanced Encryption Standard)?", "id": "Advanced Encryption Standard." },
  { "en": "Fungsi AES (Advanced Encryption Standard) di radio?", "id": "Mengamankan percakapan dari penyadapan." },
  { "en": "Kepanjangan RoIP (Radio over IP)?", "id": "Radio over Internet Protocol." },
  { "en": "Fungsi RoIP (Radio over IP)?", "id": "Menghubungkan sistem radio via internet." },
  { "en": "Perangkat penghubung RoIP (Radio over IP)?", "id": "RoIP Gateway." },
  { "en": "Antena Yagi termasuk tipe?", "id": "Antena directional (pengarah)." },
  { "en": "Antena parabola digunakan untuk?", "id": "Komunikasi satelit, microwave link." },
  { "en": "Apa itu 'beamforming'?", "id": "Memfokuskan sinyal nirkabel ke arah target." },
  { "en": "Apa itu 'fading'?", "id": "Fluktuasi kekuatan sinyal diterima." },
  { "en": "Apa itu 'multipath'?", "id": "Sinyal diterima dari banyak jalur." },
  { "en": "Efek 'multipath'?", "id": "Interferensi, sinyal jadi lemah/kuat." },
  { "en": "Apa itu 'diversity'?", "id": "Teknik mengatasi fading dan multipath." },
  { "en": "Contoh 'diversity'?", "id": "Space, frequency, dan time diversity." },
  { "en": "Apa itu 'error detection code'?", "id": "Kode untuk mendeteksi kesalahan transmisi." },
  { "en": "Contoh 'error detection code'?", "id": "CRC (Cyclic Redundancy Check)." },
  { "en": "Apa itu 'error correction code'?", "id": "Kode untuk memperbaiki kesalahan transmisi." },
  { "en": "Contoh 'error correction code'?", "id": "FEC (Forward Error Correction)." },
  { "en": "Kepanjangan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Orthogonal Frequency-Division Multiplexing." },
  { "en": "OFDM (Orthogonal Frequency-Division Multiplexing) digunakan di teknologi?", "id": "4G LTE, Wi-Fi, siaran DVB-T2." },
  { "en": "Keuntungan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Tahan terhadap interferensi multipath." },
  { "en": "Apa itu 'SIM card'?", "id": "Modul identitas pelanggan." },
  { "en": "Kepanjangan SIM (Subscriber Identity Module)?", "id": "Subscriber Identity Module." },
  { "en": "Identitas unik perangkat mobile?", "id": "IMEI (International Mobile Equipment Identity)." },
  { "en": "Kepanjangan IMEI?", "id": "International Mobile Equipment Identity." },
  { "en": "Apa itu 'spectrum analyzer'?", "id": "Alat ukur sinyal di domain frekuensi." },
  { "en": "Fungsi 'spectrum analyzer'?", "id": "Mendeteksi sumber interferensi." },
  { "en": "Apa itu 'signal-to-noise ratio' (SNR)?", "id": "Rasio kekuatan sinyal terhadap noise." },
  { "en": "Kepanjangan SNR?", "id": "Signal-to-Noise Ratio." },
  { "en": "SNR (Signal-to-Noise Ratio) yang baik?", "id": "Nilai yang tinggi." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Tingkat kesalahan dalam transmisi data." },
  { "en": "Kepanjangan BER?", "id": "Bit Error Rate." },
  { "en": "BER (Bit Error Rate) yang baik?", "id": "Nilai yang sangat rendah." },
  { "en": "Apa itu 'trunking' radio?", "id": "Berbagi beberapa kanal secara otomatis." },
  { "en": "Tujuan sistem 'trunking'?", "id": "Efisiensi penggunaan spektrum frekuensi." },
  { "en": "Sistem komunikasi 'push-to-talk'?", "id": "Tekan untuk bicara, lepas untuk dengar." },
  { "en": "Apa itu 'channel bandwidth'?", "id": "Lebar pita frekuensi satu kanal." },
  { "en": "Apa itu 'guard band'?", "id": "Pita frekuensi kosong antar kanal." },
  { "en": "Tujuan 'guard band'?", "id": "Mencegah interferensi antar kanal." },
  { "en": "Apa itu 'duplexer'?", "id": "Perangkat untuk transmisi-penerimaan bersamaan." },
  { "en": "Fungsi 'duplexer' di repeater?", "id": "Memisahkan sinyal pancar dan terima." },
  { "en": "Komunikasi data nirkabel?", "id": "Mobile data, Wi-Fi, satelit." },
  { "en": "Apa itu 'throughput'?", "id": "Kecepatan transfer data efektif." },
  { "en": "Beda 'bandwidth' dan 'throughput'?", "id": "Bandwidth teori, throughput aktual." },
  { "en": "Apa itu 'access method'?", "id": "Metode perangkat mengakses media transmisi." },
  { "en": "Contoh 'access method'?", "id": "CSMA/CA pada Wi-Fi." },
  { "en": "Kepanjangan CSMA/CA?", "id": "Carrier Sense Multiple Access/Collision Avoidance." },
  { "en": "Apa itu 'microwave link'?", "id": "Komunikasi titik-ke-titik via gelombang mikro." },
  { "en": "Syarat 'microwave link'?", "id": "Harus 'Line-of-Sight' (LOS)." },
  { "en": "Apa itu 'cognitive radio'?", "id": "Radio pintar yang bisa deteksi spektrum." },
  { "en": "Tujuan 'cognitive radio'?", "id": "Penggunaan spektrum frekuensi lebih dinamis." },
  { "en": "Apa itu 'software-defined radio' (SDR)?", "id": "Radio yang komponennya diimplementasi software." },
  { "en": "Kepanjangan SDR?", "id": "Software-Defined Radio." },
  { "en": "Kelebihan SDR (Software-Defined Radio)?", "id": "Sangat fleksibel dan dapat diprogram." },
  { "en": "Apa itu 'orthogonal' dalam sinyal?", "id": "Sinyal tidak saling mengganggu." },
  { "en": "Apa itu 'equalizer'?", "id": "Rangkaian untuk kompensasi distorsi sinyal." },
  { "en": "Apa itu 'QAM (Quadrature Amplitude Modulation)'?", "id": "Modulasi digital gabungan ASK dan PSK." },
  { "en": "Kepanjangan QAM?", "id": "Quadrature Amplitude Modulation." },
  { "en": "Apa itu 'constellation diagram'?", "id": "Visualisasi sinyal modulasi digital." },
  { "en": "Sistem penentuan posisi global?", "id": "GPS (Global Positioning System)." },
  { "en": "Kepanjangan GPS?", "id": "Global Positioning System." },
  { "en": "Prinsip kerja GPS (Global Positioning System)?", "id": "Trilaterasi dari sinyal satelit." },
  { "en": "Alternatif dari GPS?", "id": "GLONASS, Galileo, BeiDou." },
  { "en": "Apa itu 'jammer'?", "id": "Perangkat pengacak sinyal." },
  { "en": "Cara kerja 'jammer'?", "id": "Memancarkan noise kuat pada frekuensi target." },
  { "en": "Apa itu 'amplifier'?", "id": "Rangkaian penguat sinyal." },
  { "en": "Apa itu 'filter'?", "id": "Rangkaian untuk meloloskan frekuensi tertentu." },
  { "en": "Sistem penyiaran TV digital?", "id": "DVB-T2 di Indonesia." },
  { "en": "Komponen aktif utama satelit?", "id": "Transponder." },
  { "en": "Fungsi utama transponder?", "id": "Menerima, menguatkan, dan memancarkan sinyal." },
  { "en": "Sinyal dari bumi ke satelit?", "id": "Uplink." },
  { "en": "Sinyal dari satelit ke bumi?", "id": "Downlink." },
  { "en": "Area cakupan sinyal satelit?", "id": "Footprint." },
  { "en": "Kepanjangan MEO (Medium Earth Orbit)?", "id": "Medium Earth Orbit." },
  { "en": "Kepanjangan VSAT (Very Small Aperture Terminal)?", "id": "Very Small Aperture Terminal." },
  { "en": "Fungsi VSAT (Very Small Aperture Terminal)?", "id": "Terminal bumi kecil untuk komunikasi satelit." },
  { "en": "Prinsip kerja fiber optik?", "id": "Pembiasan cahaya total internal." },
  { "en": "Komponen utama sistem fiber optik?", "id": "Sumber cahaya, serat, dan detektor optik." },
  { "en": "Sumber cahaya untuk fiber optik?", "id": "LED atau dioda laser." },
  { "en": "Beda serat 'single-mode' & 'multi-mode'?", "id": "Single-mode inti lebih kecil, jarak jauh." },
  { "en": "Kepanjangan WDM (Wavelength-Division Multiplexing)?", "id": "Wavelength-Division Multiplexing." },
  { "en": "Fungsi WDM (Wavelength-Division Multiplexing)?", "id": "Mengirim banyak sinyal di satu serat." },
  { "en": "Apa itu 'Mobile IP (Internet Protocol)'?", "id": "Protokol untuk menjaga koneksi saat berpindah." },
  { "en": "Kepanjangan WAP (Wireless Application Protocol)?", "id": "Wireless Application Protocol." },
  { "en": "Apa itu 'mesh network'?", "id": "Jaringan dimana semua node saling terhubung." },
  { "en": "Keuntungan 'mesh network'?", "id": "Andal, rute bisa diperbaiki sendiri (self-healing)." },
  { "en": "Apa itu 'ad-hoc network'?", "id": "Jaringan nirkabel sementara tanpa infrastruktur." },
  { "en": "Apa itu 'convolution' sinyal?", "id": "Operasi matematis penggabungan dua sinyal." },
  { "en": "Apa itu 'correlation' sinyal?", "id": "Mengukur tingkat kemiripan dua sinyal." },
  { "en": "Apa itu 'quantization error'?", "id": "Kesalahan akibat pembulatan nilai kuantisasi." },
  { "en": "Apa itu 'companding'?", "id": "Kompresi-ekspansi sinyal perbaiki SNR." },
  { "en": "Keamanan sinyal disebut?", "id": "SIGSEC (Signal Security)." },
  { "en": "Apa itu 'spoofing' GPS (Global Positioning System)?", "id": "Memalsukan sinyal GPS untuk menipu penerima." },
  { "en": "Kepanjangan ITU (International Telecommunication Union)?", "id": "International Telecommunication Union." },
  { "en": "Peran ITU (International Telecommunication Union)?", "id": "Mengatur telekomunikasi dan spektrum global." },
  { "en": "Kepanjangan IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Institute of Electrical and Electronics Engineers." },
  { "en": "Peran IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Mengembangkan standar teknis (misal: 802.11)." },
  { "en": "Kepanjangan 3GPP (3rd Generation Partnership Project)?", "id": "3rd Generation Partnership Project." },
  { "en": "Peran 3GPP (3rd Generation Partnership Project)?", "id": "Mengembangkan standar jaringan seluler." },
  { "en": "Teorema kapasitas kanal maksimum?", "id": "Teorema Shannon-Hartley." },
  { "en": "Kapasitas kanal dipengaruhi oleh?", "id": "Bandwidth dan Signal-to-Noise Ratio (SNR)." },
  { "en": "Apa itu 'coding gain'?", "id": "Peningkatan performa dari 'error correction coding'." },
  { "en": "Apa itu 'inter-symbol interference' (ISI)?", "id": "Interferensi antar simbol data berurutan." },
  { "en": "Kepanjangan ISI (Inter-Symbol Interference)?", "id": "Inter-Symbol Interference." },
  { "en": "Penyebab ISI (Inter-Symbol Interference)?", "id": "Efek multipath pada kanal." },
  { "en": "Apa itu 'Nyquist rate'?", "id": "Tingkat sampling minimum (2x bandwidth)." },
  { "en": "Apa itu 'oversampling'?", "id": "Sampling jauh di atas Nyquist rate." },
  { "en": "Keuntungan 'oversampling'?", "id": "Meningkatkan resolusi, mengurangi aliasing." },
  { "en": "Kepanjangan PCM (Pulse-Code Modulation)?", "id": "Pulse-Code Modulation." },
  { "en": "Apa itu PCM (Pulse-Code Modulation)?", "id": "Metode standar ubah sinyal analog-digital." },
  { "en": "Apa itu 'differential PCM' (DPCM)?", "id": "Hanya mengkodekan perbedaan antar sampel." },
  { "en": "Apa itu 'codec'?", "id": "Perangkat/program untuk Coder-Decoder." },
  { "en": "Apa itu 'vocoder'?", "id": "Codec khusus untuk suara manusia." },
  { "en": "Apa itu 'equalization'?", "id": "Proses kompensasi distorsi pada kanal." },
  { "en": "Apa itu 'adaptive equalizer'?", "id": "Equalizer yang bisa beradaptasi." },
  { "en": "Apa itu 'Doppler effect'?", "id": "Perubahan frekuensi akibat gerakan." },
  { "en": "Pengaruh 'Doppler effect'?", "id": "Penting dalam komunikasi mobile." },
  { "en": "Apa itu 'carrier frequency offset'?", "id": "Perbedaan frekuensi carrier Tx dan Rx." },
  { "en": "Apa itu 'phase noise'?", "id": "Fluktuasi fasa acak pada sinyal." },
  { "en": "Sumber utama 'phase noise'?", "id": "Osilator pada pemancar atau penerima." },
  { "en": "Apa itu 'orthogonal codes'?", "id": "Kode yang tidak saling berinterferensi." },
  { "en": "Contoh 'orthogonal codes'?", "id": "Kode Walsh-Hadamard." },
  { "en": "Penggunaan 'orthogonal codes'?", "id": "Pada sistem seluler CDMA." },
  { "en": "Apa itu 'spreading factor'?", "id": "Faktor penyebaran sinyal di CDMA." },
  { "en": "Apa itu 'rake receiver'?", "id": "Penerima untuk mengatasi efek multipath." },
  { "en": "Apa itu 'link budget'?", "id": "Perhitungan semua gain/loss sinyal." },
  { "en": "Tujuan 'link budget'?", "id": "Memastikan sinyal diterima dengan baik." },
  { "en": "Apa itu 'fresnel zone'?", "id": "Area elips di sekitar jalur LOS." },
  { "en": "Pentingnya 'fresnel zone'?", "id": "Harus bebas dari halangan." },
  { "en": "Apa itu 'feedhorn'?", "id": "Komponen antena parabola di titik fokus." },
  { "en": "Apa itu 'waveguide'?", "id": "Pemandu gelombang untuk frekuensi tinggi." },
  { "en": "Material untuk 'waveguide'?", "id": "Logam konduktif." },
  { "en": "Apa itu 'scattering'?", "id": "Hamburan gelombang oleh objek kecil." },
  { "en": "Contoh 'scattering'?", "id": "Hamburan oleh tetesan hujan." },
  { "en": "Apa itu 'free-space path loss'?", "id": "Pelemahan sinyal di ruang bebas." },
  { "en": "Apa itu 'received signal strength indicator' (RSSI)?", "id": "Indikator kekuatan sinyal yang diterima." },
  { "en": "Kepanjangan RSSI?", "id": "Received Signal Strength Indicator." },
  { "en": "Apa itu 'effective isotropic radiated power' (EIRP)?", "id": "Daya total yang dipancarkan antena." },
  { "en": "Kepanjangan EIRP?", "id": "Effective Isotropic Radiated Power." },
  { "en": "Apa itu 'protocol stack'?", "id": "Tumpukan lapisan protokol (misal: OSI)." },
  { "en": "Apa itu 'handshaking'?", "id": "Proses negosiasi parameter komunikasi." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda total pengiriman data." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi waktu tunda (latency)." },
  { "en": "Apa itu 'quality of service' (QoS)?", "id": "Manajemen kualitas layanan jaringan." },
  { "en": "Parameter QoS (Quality of Service)?", "id": "Bandwidth, latency, jitter, packet loss." },
  { "en": "Apa itu 'IP telephony'?", "id": "Telepon menggunakan jaringan internet." },
  { "en": "Sinonim 'IP telephony'?", "id": "Voice over IP (VoIP)." },
  { "en": "Protokol untuk VoIP (Voice over IP)?", "id": "SIP, H.323, RTP." },
  { "en": "Kepanjangan SIP (Session Initiation Protocol)?", "id": "Session Initiation Protocol." },
  { "en": "Fungsi SIP (Session Initiation Protocol)?", "id": "Membangun, memodifikasi, mengakhiri sesi." },
  { "en": "Kepanjangan RTP (Real-time Transport Protocol)?", "id": "Real-time Transport Protocol." },
  { "en": "Fungsi RTP (Real-time Transport Protocol)?", "id": "Mengirimkan data media (suara/video)." },
  { "en": "Jaringan nirkabel personal?", "id": "WPAN (Wireless Personal Area Network)." },
  { "en": "Contoh WPAN (Wireless Personal Area Network)?", "id": "Bluetooth, Zigbee." },
  { "en": "Jaringan nirkabel perkotaan?", "id": "WMAN (Wireless Metropolitan Area Network)." },
  { "en": "Contoh WMAN (Wireless Metropolitan Area Network)?", "id": "WiMAX." },
  { "en": "Apa itu 'spread spectrum'?", "id": "Teknik menyebar sinyal di pita lebar." },
  { "en": "Tujuan 'spread spectrum'?", "id": "Tahan interferensi, lebih aman." },
  { "en": "Tipe 'spread spectrum'?", "id": "DSSS dan FHSS." },
  { "en": "Kepanjangan DSSS (Direct-Sequence Spread Spectrum)?", "id": "Direct-Sequence Spread Spectrum." },
  { "en": "Kepanjangan FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Frequency-Hopping Spread Spectrum." },
  { "en": "Prinsip FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Frekuensi berubah-ubah secara acak." },
  { "en": "Apa itu 'chipping code'?", "id": "Kode yang digunakan dalam DSSS." },
  { "en": "Apa itu 'dwell time'?", "id": "Waktu tinggal di satu frekuensi FHSS." },
  { "en": "Apa itu 'TEMPEST'?", "id": "Standar keamanan dari emisi sinyal." },
  { "en": "Apa itu 'information entropy'?", "id": "Ukuran ketidakpastian suatu informasi." },
  { "en": "Satuan 'information entropy'?", "id": "Bits." },
  { "en": "Siapa pencetus teori informasi?", "id": "Claude Shannon." },
  { "en": "Apa itu 'source coding'?", "id": "Proses kompresi data dari sumber." },
  { "en": "Contoh 'source coding'?", "id": "Huffman coding, Lempel-Ziv." },
  { "en": "Apa itu 'channel coding'?", "id": "Penambahan redundansi untuk deteksi/koreksi eror." },
  { "en": "Kepanjangan SONET (Synchronous Optical Networking)?", "id": "Synchronous Optical Networking." },
  { "en": "Padanan SONET (Synchronous Optical Networking) di Eropa?", "id": "SDH (Synchronous Digital Hierarchy)." },
  { "en": "Kepanjangan FTTH (Fiber to the Home)?", "id": "Fiber to the Home." },
  { "en": "Arsitektur jaringan FTTH (Fiber to the Home)?", "id": "Jaringan akses fiber optik ke rumah." },
  { "en": "Komponen pasif di jaringan optik?", "id": "Splitter, connector, attenuator." },
  { "en": "Fungsi 'optical splitter'?", "id": "Membagi satu sinyal optik ke banyak." },
  { "en": "Kepanjangan OTDR (Optical Time-Domain Reflectometer)?", "id": "Optical Time-Domain Reflectometer." },
  { "en": "Fungsi OTDR (Optical Time-Domain Reflectometer)?", "id": "Mengukur jarak, loss, dan fault serat." },
  { "en": "Apa itu 'fusion splicing'?", "id": "Metode penyambungan serat optik permanen." },
  { "en": "Beda QPSK (Quadrature Phase-Shift Keying) dan BPSK (Binary Phase-Shift Keying)?", "id": "QPSK punya empat fasa, BPSK dua." },
  { "en": "Kepanjangan QPSK (Quadrature Phase-Shift Keying)?", "id": "Quadrature Phase-Shift Keying." },
  { "en": "Keuntungan 16-QAM (Quadrature Amplitude Modulation) dari QPSK?", "id": "Efisiensi spektral lebih tinggi." },
  { "en": "Apa itu 'trellis coded modulation' (TCM)?", "id": "Menggabungkan coding dan modulasi." },
  { "en": "Apa itu 'eye diagram'?", "id": "Visualisasi untuk analisa kualitas sinyal." },
  { "en": "'Eye opening' yang lebar artinya?", "id": "Kualitas sinyal yang baik." },
  { "en": "Apa itu 'smart antenna'?", "id": "Antena dengan pemrosesan sinyal cerdas." },
  { "en": "Tipe 'smart antenna'?", "id": "Switched beam dan adaptive array." },
  { "en": "Apa itu 'beam steering'?", "id": "Kemampuan mengarahkan pancaran antena." },
  { "en": "Apa itu komunikasi akustik bawah air?", "id": "Komunikasi menggunakan gelombang suara di air." },
  { "en": "Tantangan komunikasi bawah air?", "id": "Atenuasi tinggi, multipath, kecepatan rendah." },
  { "en": "Kepanjangan Li-Fi (Light Fidelity)?", "id": "Light Fidelity." },
  { "en": "Prinsip kerja Li-Fi (Light Fidelity)?", "id": "Transmisi data menggunakan cahaya (LED)." },
  { "en": "Apa itu 'troposcatter'?", "id": "Komunikasi dengan hamburan sinyal troposfer." },
  { "en": "Fungsi 'mixer' pada radio?", "id": "Mengubah frekuensi sinyal." },
  { "en": "Kepanjangan VCO (Voltage-Controlled Oscillator)?", "id": "Voltage-Controlled Oscillator." },
  { "en": "Fungsi VCO (Voltage-Controlled Oscillator)?", "id": "Osilator yang frekuensinya diatur tegangan." },
  { "en": "Kepanjangan PLL (Phase-Locked Loop)?", "id": "Phase-Locked Loop." },
  { "en": "Fungsi PLL (Phase-Locked Loop)?", "id": "Menstabilkan dan mensintesis frekuensi." },
  { "en": "Kepanjangan SAW (Surface Acoustic Wave) filter?", "id": "Surface Acoustic Wave filter." },
  { "en": "Kepanjangan LNA (Low-Noise Amplifier)?", "id": "Low-Noise Amplifier." },
  { "en": "Fungsi LNA (Low-Noise Amplifier)?", "id": "Menguatkan sinyal lemah tanpa tambah noise." },
  { "en": "Dimana LNA (Low-Noise Amplifier) diletakkan?", "id": "Sedekat mungkin dengan antena." },
  { "en": "Apa itu 'noise figure'?", "id": "Ukuran penambahan noise oleh perangkat." },
  { "en": "Nilai 'noise figure' yang baik?", "id": "Nilai yang serendah mungkin." },
  { "en": "Apa itu 'impedance matching'?", "id": "Menyesuaikan impedansi untuk transfer daya." },
  { "en": "Tujuan 'impedance matching'?", "id": "Transfer daya maksimal, kurangi pantulan." },
  { "en": "Standar impedansi sistem RF (Radio Frequency)?", "id": "50 Ohm atau 75 Ohm." },
  { "en": "Apa itu 'standing wave ratio' (SWR)?", "id": "Rasio gelombang pantul dan maju." },
  { "en": "Kepanjangan SWR (Standing Wave Ratio)?", "id": "Standing Wave Ratio." },
  { "en": "Nilai SWR (Standing Wave Ratio) yang ideal?", "id": "1:1 (satu banding satu)." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran daya sinyal yang dipantulkan." },
  { "en": "Apa itu 'dispersion' pada fiber optik?", "id": "Pelebaran pulsa sinyal optik." },
  { "en": "Jenis-jenis 'dispersion'?", "id": "Chromatic, modal, polarization mode." },
  { "en": "Apa itu 'optical amplifier'?", "id": "Penguat sinyal optik tanpa konversi." },
  { "en": "Contoh 'optical amplifier'?", "id": "EDFA (Erbium-Doped Fiber Amplifier)." },
  { "en": "Apa itu 'attenuator' optik?", "id": "Perangkat untuk melemahkan sinyal optik." },
  { "en": "Apa itu 'circulator'?", "id": "Perangkat tiga port untuk mengarahkan sinyal." },
  { "en": "Apa itu 'isolator'?", "id": "Perangkat yang hanya izinkan sinyal satu arah." },
  { "en": "Apa itu 'Shannon limit'?", "id": "Batas atas teoritis kecepatan data." },
  { "en": "Apa itu 'line coding'?", "id": "Metode representasi data digital." },
  { "en": "Contoh 'line coding'?", "id": "NRZ, RZ, Manchester." },
  { "en": "Apa itu 'scrambler'?", "id": "Mengacak data untuk hindari urutan panjang." },
  { "en": "Apa itu 'carrier sense'?", "id": "Mendeteksi apakah kanal sedang digunakan." },
  { "en": "Apa itu 'collision detection'?", "id": "Mendeteksi tabrakan data di media." },
  { "en": "Metode akses CSMA/CD?", "id": "Listen, send, dan deteksi tabrakan." },
  { "en": "Kepanjangan CSMA/CD?", "id": "Carrier Sense Multiple Access/Collision Detection." },
  { "en": "CSMA/CD (Carrier Sense Multiple Access/Collision Detection) digunakan di?", "id": "Jaringan Ethernet lama." },
  { "en": "Apa itu 'orthogonal subcarriers'?", "id": "Sub-carrier yang tidak saling mengganggu." },
  { "en": "Konsep dasar OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Membagi data ke banyak sub-carrier." },
  { "en": "Apa itu 'cyclic prefix'?", "id": "Salinan akhir simbol di awal." },
  { "en": "Tujuan 'cyclic prefix' di OFDM?", "id": "Mengatasi inter-symbol interference (ISI)." },
  { "en": "Apa itu 'channel state information' (CSI)?", "id": "Informasi karakteristik kanal komunikasi." },
  { "en": "Kepanjangan CSI?", "id": "Channel State Information." },
  { "en": "Apa itu 'automatic repeat request' (ARQ)?", "id": "Protokol untuk transmisi ulang data." },
  { "en": "Kepanjangan ARQ?", "id": "Automatic Repeat Request." },
  { "en": "Tipe ARQ (Automatic Repeat Request)?", "id": "Stop-and-wait, Go-Back-N, Selective Repeat." },
  { "en": "Apa itu 'error burst'?", "id": "Kesalahan data yang terjadi berurutan." },
  { "en": "Apa itu 'interleaving'?", "id": "Teknik menyebar data untuk atasi error burst." },
  { "en": "Apa itu 'forward link'?", "id": "Link dari base station ke pengguna." },
  { "en": "Apa itu 'reverse link'?", "id": "Link dari pengguna ke base station." },
  { "en": "Apa itu 'frequency reuse'?", "id": "Penggunaan frekuensi sama di sel berbeda." },
  { "en": "Tujuan 'frequency reuse'?", "id": "Meningkatkan kapasitas sistem seluler." },
  { "en": "Apa itu 'cell sectoring'?", "id": "Membagi sel menjadi beberapa sektor." },
  { "en": "Tujuan 'cell sectoring'?", "id": "Mengurangi interferensi, tingkatkan kapasitas." },
  { "en": "Apa itu 'soft handover'?", "id": "Koneksi ke BTS baru sebelum putus." },
  { "en": "Teknologi yang gunakan 'soft handover'?", "id": "CDMA." },
  { "en": "Apa itu 'hard handover'?", "id": "Putus koneksi lama, baru sambung." },
  { "en": "Teknologi yang gunakan 'hard handover'?", "id": "GSM." },
  { "en": "Apa itu 'pilot signal'?", "id": "Sinyal referensi dari base station." },
  { "en": "Fungsi 'pilot signal'?", "id": "Sinkronisasi, estimasi kanal, pengukuran." },
  { "en": "Apa itu 'fading margin'?", "id": "Daya ekstra untuk kompensasi fading." },
  { "en": "Apa itu 'polarization diversity'?", "id": "Gunakan antena dengan polarisasi berbeda." },
  { "en": "Apa itu 'radio resource management' (RRM)?", "id": "Manajemen sumber daya di jaringan nirkabel." },
  { "en": "Contoh RRM (Radio Resource Management)?", "id": "Manajemen daya, penjadwalan, handover." },
  { "en": "Apa itu 'link adaptation'?", "id": "Penyesuaian modulasi/coding sesuai kondisi." },
  { "en": "Apa itu 'channel sounding'?", "id": "Proses mengukur karakteristik kanal." },
  { "en": "Apa itu 'noise floor'?", "id": "Tingkat kebisingan dasar suatu sistem." },
  { "en": "Apa itu 'dynamic range' penerima?", "id": "Rentang sinyal yang bisa diolah." },
  { "en": "Apa itu 'intermodulation distortion'?", "id": "Distorsi akibat sinyal non-linear." },
  { "en": "Apa itu 'eavesdropping'?", "id": "Mendengarkan komunikasi secara diam-diam." },
  { "en": "Serangan 'Man-in-the-Middle' (MitM) nirkabel?", "id": "Penyadapan di antara dua pihak." },
  { "en": "Kepanjangan SIGINT (Signal Intelligence)?", "id": "Signal Intelligence." },
  { "en": "Apa itu 'lawful interception'?", "id": "Penyadapan yang sah secara hukum." },
  { "en": "Konsep keamanan utama 5G?", "id": "Network slicing, security by design." },
  { "en": "Apa itu 'network slicing'?", "id": "Membuat jaringan virtual di atas fisik." },
  { "en": "Kepanjangan WSN (Wireless Sensor Network)?", "id": "Wireless Sensor Network." },
  { "en": "Fungsi WSN (Wireless Sensor Network)?", "id": "Mengumpulkan data dari banyak sensor." },
  { "en": "Protokol untuk WSN (Wireless Sensor Network)?", "id": "Zigbee, LoRaWAN, Bluetooth LE." },
  { "en": "Kepanjangan LoRaWAN?", "id": "Long Range Wide Area Network." },
  { "en": "Tantangan utama WSN (Wireless Sensor Network)?", "id": "Konsumsi daya baterai yang rendah." },
  { "en": "Beda TV analog & digital?", "id": "Sinyal, kualitas gambar, efisiensi." },
  { "en": "Standar TV digital di Indonesia?", "id": "DVB-T2." },
  { "en": "Kepanjangan DAB (Digital Audio Broadcasting)?", "id": "Digital Audio Broadcasting." },
  { "en": "Apa itu 'datacasting'?", "id": "Mengirim data melalui sinyal siaran." },
  { "en": "Apa itu 'spectrum sharing'?", "id": "Berbagi spektrum frekuensi antar pengguna." },
  { "en": "Apa itu 'dynamic spectrum access' (DSA)?", "id": "Akses spektrum secara dinamis." },
  { "en": "Apa itu 'spectrum auction'?", "id": "Lelang hak penggunaan spektrum frekuensi." },
  { "en": "Apa itu 'unlicensed spectrum'?", "id": "Spektrum bebas digunakan (misal: Wi-Fi)." },
  { "en": "Contoh 'unlicensed spectrum'?", "id": "Pita frekuensi 2.4 GHz dan 5 GHz." },
  { "en": "Tipe 'fading' Rayleigh?", "id": "Fading tanpa komponen 'Line-of-Sight' (LOS)." },
  { "en": "Tipe 'fading' Rician?", "id": "Fading dengan komponen 'Line-of-Sight' (LOS)." },
  { "en": "Apa itu 'path loss model'?", "id": "Model matematis untuk pelemahan sinyal." },
  { "en": "Model Okumura-Hata untuk?", "id": "Prediksi path loss di area urban." },
  { "en": "Apa itu 'antenna pattern'?", "id": "Pola radiasi 3D dari sebuah antena." },
  { "en": "Apa itu 'beamwidth'?", "id": "Lebar sudut pancaran utama antena." },
  { "en": "Apa itu 'sidelobes'?", "id": "Pancaran minor yang tidak diinginkan." },
  { "en": "Apa itu 'nulls'?", "id": "Area tanpa pancaran sinyal." },
  { "en": "Apa itu 'front-to-back ratio'?", "id": "Rasio pancaran depan dan belakang." },
  { "en": "Konsep dasar teori antrian?", "id": "Studi matematis tentang antrian." },
  { "en": "Model antrian M/M/1?", "id": "Model antrian paling dasar." },
  { "en": "Apa itu 'blocking probability'?", "id": "Probabilitas panggilan ditolak karena sibuk." },
  { "en": "Apa itu 'call admission control' (CAC)?", "id": "Mekanisme untuk membatasi panggilan masuk." },
  { "en": "Apa itu 'grade of service' (GoS)?", "id": "Ukuran performa sistem telekomunikasi." },
  { "en": "Apa itu 'erlang'?", "id": "Satuan beban lalu lintas telekomunikasi." },
  { "en": "Apa itu 'optical wireless communication' (OWC)?", "id": "Komunikasi nirkabel menggunakan cahaya." },
  { "en": "Contoh OWC (Optical Wireless Communication)?", "id": "Li-Fi, IrDA." },
  { "en": "Apa itu 'full-duplex radio'?", "id": "Radio yang bisa pancar-terima bersamaan." },
  { "en": "Tantangan 'full-duplex radio'?", "id": "Mengatasi interferensi diri (self-interference)." },
  { "en": "Apa itu 'channel reciprocity'?", "id": "Karakteristik kanal uplink dan downlink sama." },
  { "en": "Apa itu 'channel state information' (CSI)?", "id": "Informasi properti fisik kanal." },
  { "en": "Fungsi CSI (Channel State Information) di MIMO?", "id": "Untuk beamforming dan precoding." },
  { "en": "Apa itu 'precoding'?", "id": "Teknik pemrosesan sinyal di pemancar." },
  { "en": "Apa itu 'massive MIMO'?", "id": "Sistem MIMO dengan antena sangat banyak." },
  { "en": "Penggunaan 'massive MIMO'?", "id": "Teknologi kunci untuk jaringan 5G." },
  { "en": "Apa itu 'cell-free massive MIMO'?", "id": "Banyak access point melayani pengguna." },
  { "en": "Apa itu 'backhaul'?", "id": "Koneksi dari jaringan akses ke inti." },
  { "en": "Contoh teknologi 'backhaul'?", "id": "Fiber optik, microwave link." },
  { "en": "Apa itu 'fronthaul'?", "id": "Koneksi antara baseband unit dan radio." },
  { "en": "Apa itu 'C-RAN (Cloud Radio Access Network)'?", "id": "Arsitektur RAN berbasis cloud." },
  { "en": "Keuntungan C-RAN (Cloud Radio Access Network)?", "id": "Efisiensi, skalabilitas, manajemen terpusat." },
  { "en": "Apa itu 'power amplifier' (PA)?", "id": "Penguat daya sinyal sebelum dipancarkan." },
  { "en": "Efisiensi 'power amplifier' (PA)?", "id": "Ukuran seberapa efisien mengubah daya." },
  { "en": "Apa itu 'linearity' PA (Power Amplifier)?", "id": "Kemampuan menguatkan sinyal tanpa distorsi." },
  { "en": "Apa itu 'digital pre-distortion' (DPD)?", "id": "Teknik untuk perbaiki linearitas PA." },
  { "en": "Apa itu 'gallium nitride' (GaN)?", "id": "Bahan semikonduktor untuk RF power." },
  { "en": "Keunggulan GaN (Gallium Nitride)?", "id": "Efisien di frekuensi dan daya tinggi." },
  { "en": "Apa itu 'millimeter wave' (mmWave)?", "id": "Frekuensi radio dari 30 hingga 300 GHz." },
  { "en": "Karakteristik 'mmWave'?", "id": "Bandwidth sangat lebar, jangkauan pendek." },
  { "en": "Penggunaan 'mmWave'?", "id": "Jaringan 5G, radar mobil." },
  { "en": "Apa itu 'beam management'?", "id": "Proses mengelola beam di sistem mmWave." },
  { "en": "Apa itu 'initial access'?", "id": "Proses perangkat pertama kali terhubung." },
  { "en": "Apa itu 'random access channel' (RACH)?", "id": "Kanal untuk permintaan akses awal." },
  { "en": "Apa itu 'synchronization signal block' (SSB)?", "id": "Sinyal untuk sinkronisasi di 5G." },
  { "en": "Apa itu 'symbol' dalam telekomunikasi?", "id": "Satu unit sinyal merepresentasikan data." },
  { "en": "Apa itu 'chip' dalam CDMA?", "id": "Unit terkecil dari spreading code." },
  { "en": "Apa itu 'near-far problem'?", "id": "Masalah di CDMA, sinyal dekat menutupi." },
  { "en": "Solusi 'near-far problem'?", "id": "Power control yang ketat." },
  { "en": "Apa itu 'adjacent channel interference' (ACI)?", "id": "Interferensi dari kanal di sebelahnya." },
  { "en": "Apa itu 'co-channel interference' (CCI)?", "id": "Interferensi dari kanal frekuensi sama." },
  { "en": "Apa itu 'pilot pollution'?", "id": "Interferensi dari banyak pilot signal." },
  { "en": "Apa itu 'network planning'?", "id": "Proses merancang penempatan base station." },
  { "en": "Tujuan 'network planning'?", "id": "Memaksimalkan cakupan dan kapasitas." },
  { "en": "Tools untuk 'network planning'?", "id": "Software simulasi propagasi radio." },
  { "en": "Apa itu 'drive test'?", "id": "Pengukuran performa jaringan di lapangan." },
  { "en": "Apa itu 'channel impulse response' (CIR)?", "id": "Respon kanal terhadap sinyal impuls." },
  { "en": "Apa itu 'link adaptation'?", "id": "Penyesuaian transmisi sesuai kondisi kanal." },
  { "en": "Contoh 'link adaptation'?", "id": "Ubah modulasi, coding rate." },
  { "en": "Apa itu 'HARQ (Hybrid Automatic Repeat Request)'?", "id": "Gabungan FEC dan ARQ." },
  { "en": "Keuntungan HARQ (Hybrid Automatic Repeat Request)?", "id": "Lebih efisien dari ARQ biasa." },
  { "en": "Apa itu 'multiple access'?", "id": "Teknik berbagi sumber daya komunikasi." },
  { "en": "Contoh skema 'multiple access'?", "id": "FDMA, TDMA, CDMA, OFDMA." },
  { "en": "Kepanjangan OFDMA?", "id": "Orthogonal Frequency-Division Multiple Access." },
  { "en": "OFDMA (Orthogonal Frequency-Division Multiple Access) digunakan di?", "id": "Wi-Fi 6 dan 5G." },
  { "en": "Apa itu 'resource block'?", "id": "Unit sumber daya terkecil di LTE/5G." },
  { "en": "Apa itu 'scheduler'?", "id": "Algoritma yang alokasikan resource block." },
  { "en": "Apa itu 'spatial multiplexing'?", "id": "Mengirim stream data berbeda via MIMO." },
  { "en": "Apa itu 'transmit diversity'?", "id": "Mengirim sinyal sama via banyak antena." },
  { "en": "Apa itu 'Public Switched Telephone Network' (PSTN)?", "id": "Jaringan telepon kabel tradisional." },
  { "en": "Karakteristik PSTN (Public Switched Telephone Network)?", "id": "Circuit-switched." },
  { "en": "Beda 'circuit-switched' & 'packet-switched'?", "id": "Circuit dedicated, packet berbagi." },
  { "en": "Apa itu 'Internet of Bodies' (IoB)?", "id": "Jaringan perangkat terhubung ke tubuh." },
  { "en": "Apa itu 'visible light communication' (VLC)?", "id": "Komunikasi menggunakan cahaya tampak." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
