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


  { "en": "Apa Itu Resistansi Efektif (Effective Resistance) AC?", "id": "Total Hambatan Termasuk Efek Kulit." },
  { "en": "Apa Itu Kapasitansi Parasitik (Parasitic Capacitance)?", "id": "Kapasitansi Tak Diinginkan Antar Komponen." },
  { "en": "Apa Itu Induktansi Parasitik (Parasitic Inductance)?", "id": "Induktansi Tak Diinginkan Jalur PCB." },
  { "en": "Apa Itu Ground Bounce (Pantulan Ground)?", "id": "Fluktuasi Tegangan Referensi Ground." },
  { "en": "Penyebab Ground Bounce (Pantulan Ground)?", "id": "Switching Arus Cepat Melalui Induktansi Ground." },
  { "en": "Bagaimana Mengurangi Ground Bounce (Pantulan Ground)?", "id": "Gunakan Ground Plane Impedansi Rendah." },
  { "en": "Apa Itu Crosstalk (Crosstalk) Sinyal?", "id": "Induksi Sinyal Antar Jalur Berdekatan." },
  { "en": "Bagaimana Mengurangi Crosstalk (Crosstalk)?", "id": "Jaga Jarak Jalur, Gunakan Ground Plane." },
  { "en": "Apa Itu Integritas Sinyal (Signal Integrity)?", "id": "Kualitas Sinyal Listrik (Bentuk, Waktu)." },
  { "en": "Masalah Apa Terkait Integritas Sinyal (Signal Integrity)?", "id": "Refleksi, Ringing, Crosstalk, Ground Bounce." },
  { "en": "Apa Itu Refleksi (Reflection) Sinyal?", "id": "Pantulan Sinyal Akibat Mismatch Impedansi." },
  { "en": "Apa Itu Ringing (Deringan) Sinyal?", "id": "Osilasi Sinyal Setelah Transisi Cepat." },
  { "en": "Apa Itu Integritas Daya (Power Integrity)?", "id": "Kualitas Distribusi Daya Rangkaian." },
  { "en": "Mengapa Integritas Daya (Power Integrity) Penting?", "id": "Menjamin Operasi Stabil Komponen Digital." },
  { "en": "Apa Fungsi Kapasitor Decoupling (Decoupling Capacitor)?", "id": "Menyediakan Energi Lokal Cepat IC." },
  { "en": "Dimana Kapasitor Decoupling (Decoupling Capacitor) Ditempatkan?", "id": "Sangat Dekat Pin Daya IC." },
  { "en": "Nilai Kapasitor Decoupling (Decoupling Capacitor) Tipikal?", "id": "Nol Koma Satu Mikrofarad (0.1 µF)." },
  { "en": "Apa Itu Impedansi Terkontrol (Controlled Impedance) PCB?", "id": "Desain Jalur Impedansi Spesifik." },
  { "en": "Mengapa Perlu Impedansi Terkontrol (Controlled Impedance)?", "id": "Untuk Sinyal Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Differential Pair (Pasangan Diferensial) Routing?", "id": "Merutekan Dua Sinyal Berdekatan Seimbang." },
  { "en": "Manfaat Differential Pair (Pasangan Diferensial) Routing?", "id": "Penolakan Noise Baik, Kurangi EMI." },
  { "en": "Apa Itu Panjang Elektrik (Electrical Length)?", "id": "Panjang Fisik Relatif Kecepatan Sinyal." },
  { "en": "Kapan Panjang Elektrik (Electrical Length) Penting?", "id": "Saat Panjang Jalur Sebanding Lambda." },
  { "en": "Apa Itu Skew (Skew) Sinyal Diferensial?", "id": "Perbedaan Waktu Tiba Dua Sinyal." },
  { "en": "Apa Akibat Skew (Skew) Sinyal?", "id": "Konversi Mode Diferensial Ke Bersama." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Visualisasi Kualitas Sinyal Digital Serial." },
  { "en": "Apa Yang Bisa Dilihat Diagram Mata?", "id": "Jitter, Noise Margin, Inter-Symbol Interference." },
  { "en": "Apa Itu Jitter (Jitter) Sinyal?", "id": "Variasi Waktu Tepi Sinyal Digital." },
  { "en": "Apa Itu Phase Noise (Derau Fasa)?", "id": "Fluktuasi Fasa Acak Osilator." },
  { "en": "Apa Pengaruh Phase Noise (Derau Fasa)?", "id": "Membatasi Kinerja Komunikasi RF." },
  { "en": "Apa Itu Resonator (Resonator)?", "id": "Komponen Penyimpan Energi Frekuensi Tertentu." },
  { "en": "Contoh Resonator (Resonator) Elektronik?", "id": "Rangkaian LC, Kristal Kuarsa, Resonator Keramik." },
  { "en": "Apa Itu Kristal Kuarsa (Quartz Crystal)?", "id": "Bahan Piezoelektrik Resonansi Mekanis Stabil." },
  { "en": "Apa Faktor Kualitas (Q Factor) Kristal?", "id": "Sangat Tinggi (Resonansi Sangat Tajam)." },
  { "en": "Apa Itu Osilator Terkendali Tegangan (VCO)?", "id": "Frekuensi Osilasi Diatur Tegangan Kontrol." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Umpan Balik Kunci Fasa Osilator." },
  { "en": "Apa Aplikasi Utama PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi Sinyal." },
  { "en": "Apa Itu Sintesiser Frekuensi (Frequency Synthesizer)?", "id": "Penghasil Frekuensi Variabel Tapi Stabil." },
  { "en": "Apa Itu Direct Digital Synthesis (DDS)?", "id": "Sintesis Frekuensi Secara Digital Langsung." },
  { "en": "Apa Keunggulan DDS (Direct Digital Synthesis)?", "id": "Resolusi Frekuensi Halus, Switching Cepat." },
  { "en": "Apa Itu Mixer (Mixer) Frekuensi?", "id": "Mengalikan Dua Sinyal Hasilkan Frekuensi Baru." },
  { "en": "Apa Frekuensi Output Utama Mixer?", "id": "Frekuensi Jumlah Dan Selisih Input." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator) (LO)?", "id": "Sinyal Referensi Frekuensi Untuk Mixer." },
  { "en": "Apa Itu Frekuensi Menengah (Intermediate Frequency) (IF)?", "id": "Frekuensi Hasil Konversi Turun Penerima." },
  { "en": "Apa Itu Arsitektur Penerima Superheterodyne?", "id": "Konversi RF Ke IF Tetap Penguatan Selektif." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi Tak Diinginkan Masuk Lewat Mixer." },
  { "en": "Bagaimana Menekan Frekuensi Gambar (Image Frequency)?", "id": "Gunakan Filter Image Reject Sebelum Mixer." },
  { "en": "Apa Itu Penguat Derau Rendah (LNA)?", "id": "Penguat Awal Penerima Noise Sangat Rendah." },
  { "en": "Mengapa LNA (Low Noise Amplifier) Penting?", "id": "Meningkatkan Sensitivitas Penerima." },
  { "en": "Apa Itu Angka Derau (Noise Figure) (NF)?", "id": "Ukuran Tambahan Noise Oleh Penguat." },
  { "en": "NF (Noise Figure) Rendah Berarti Penguat Bagaimana?", "id": "Penguat Kualitasnya Sangat Baik." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier) (PA)?", "id": "Penguat Akhir Pemancar Daya Tinggi." },
  { "en": "Apa Ukuran Linearitas (Linearity) PA?", "id": "P1dB (Titik Kompresi), IP3 (Intersep Orde-3)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Penguat Daya?", "id": "Rasio Daya Output RF, Input DC." },
  { "en": "Apa Itu Power Added Efficiency (PAE)?", "id": "Efisiensi Perhitungkan Daya Input RF." },
  { "en": "Kelas Penguat RF (Frekuensi Radio) Umum?", "id": "Kelas A, AB, B, C, E, F." },
  { "en": "Kelas Apa Paling Linear Tapi Boros?", "id": "Kelas A." },
  { "en": "Kelas Apa Paling Efisien Tapi Non-Linear?", "id": "Kelas C (Untuk Modulasi Konstan)." },
  { "en": "Kelas Apa Efisiensi Tinggi Switching?", "id": "Kelas D, E, F." },
  { "en": "Apa Itu Duplexer (Duplexer) RF?", "id": "Memungkinkan Antena Sama Transmit, Receive." },
  { "en": "Bagaimana Duplexer (Duplexer) Bekerja?", "id": "Gunakan Filter Pisahkan Frekuensi TX, RX." },
  { "en": "Apa Itu Sirkulator (Circulator) RF?", "id": "Perangkat Tiga Port Arah Sirkulasi." },
  { "en": "Apa Itu Isolator (Isolator) RF?", "id": "Sirkulator (Circulator) Port Ketiga Diberi Beban." },
  { "en": "Fungsi Isolator (Isolator) RF?", "id": "Lindungi Sumber Dari Refleksi Beban." },
  { "en": "Apa Itu Kopler Arah (Directional Coupler)?", "id": "Mencuplik Sebagian Daya Arah Tertentu." },
  { "en": "Parameter Kopler Arah (Directional Coupler)?", "id": "Coupling Factor, Directivity, Isolation." },
  { "en": "Apa Itu Filter RF (Frekuensi Radio)?", "id": "Memilih Frekuensi Tertentu Sinyal RF." },
  { "en": "Jenis Filter RF (Frekuensi Radio) Umum?", "id": "LC, SAW, BAW, Keramik, Rongga." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter Gelombang Akustik Permukaan." },
  { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter Gelombang Akustik Bulk (Lebih HF)." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Transduser Gelombang EM, Listrik." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Energi Antena." },
  { "en": "Apa Satuan Penguatan (Gain) Antena?", "id": "Desibel Isotropik (dBi)." },
  { "en": "Apa Itu Antena Isotropik (Isotropic Antenna)?", "id": "Antena Ideal Memancar Sama Semua Arah." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Grafik Distribusi Pancaran Antena." },
  { "en": "Apa Itu Lobe Utama (Main Lobe)?", "id": "Arah Pancaran Energi Terkuat Antena." },
  { "en": "Apa Itu Lobe Samping (Side Lobe)?", "id": "Pancaran Energi Arah Tak Diinginkan." },
  { "en": "Apa Itu Lebar Berkas (Beamwidth) Antena?", "id": "Sudut Antara Titik Setengah Daya Lobe Utama." },
  { "en": "Apa Itu Rasio Depan-Belakang (Front-to-Back Ratio)?", "id": "Rasio Penguatan Arah Depan, Belakang." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Dipancarkan." },
  { "en": "Jenis Polarisasi (Polarization) Antena?", "id": "Linear (Vertikal/Horizontal), Sirkular (RHCP/LHCP)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Antena?", "id": "Rasio Daya Radiasi, Daya Input Total." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Antena?", "id": "Impedansi Terminal Input Antena." },
  { "en": "Mengapa Impedansi Antena Penting?", "id": "Untuk Pencocokan Impedansi Saluran Transmisi." },
  { "en": "Impedansi Input (Input Impedance) Dipol Setengah Gelombang?", "id": "Sekitar Tujuh Puluh Tiga Ohm (73 Ω)." },
  { "en": "Apa Itu Antena Dipol Terlipat (Folded Dipole)?", "id": "Impedansi Input Lebih Tinggi (Sekitar 300 Ω)." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda)?", "id": "Antena Direktif (Driven, Reflector, Director)." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Umpan (Feed) Antena Parabola?", "id": "Antena Kecil Di Titik Fokus Parabola." },
  { "en": "Apa Itu Antena Mikrostrip (Microstrip Antenna) / Patch?", "id": "Antena Dicetak Di Atas Substrat PCB." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Kumpulan Elemen Antena Terkoordinasi." },
  { "en": "Apa Itu Beamforming (Beamforming)?", "id": "Mengatur Fasa Array Arahkan Berkas." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Sistem Komunikasi Multi Antena Tx, Rx." },
  { "en": "Apa Itu Keanekaragaman (Diversity) Antena?", "id": "Menggunakan Multi Antena Atasi Fading." },
  { "en": "Apa Itu Radar (Radar)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Prinsip Dasar Radar (Radar)?", "id": "Pancarkan Sinyal, Deteksi Pantulan (Echo)." },
  { "en": "Apa Itu Radar Doppler (Doppler Radar)?", "id": "Mengukur Kecepatan Objek (Pergeseran Frekuensi)." },
  { "en": "Apa Itu Kapasitor Keramik (Ceramic Capacitor)?", "id": "Kapasitor Dielektrik Bahan Keramik." },
  { "en": "Apa Keunggulan Kapasitor Keramik (Ceramic Capacitor)?", "id": "Ukuran Kecil, Murah, Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor Elektrolit (Electrolytic Capacitor)?", "id": "Kapasitor Polar Kapasitansi Tinggi." },
  { "en": "Apa Ciri Kapasitor Elektrolit (Electrolytic Capacitor)?", "id": "Memiliki Polaritas Positif, Negatif." },
  { "en": "Apa Dielektrik Kapasitor Elektrolit (Electrolytic Capacitor)?", "id": "Lapisan Oksida Tipis (Aluminium/Tantalum)." },
  { "en": "Apa Itu Kapasitor Film (Film Capacitor)?", "id": "Kapasitor Dielektrik Lapisan Plastik Tipis." },
  { "en": "Jenis Kapasitor Film (Film Capacitor)?", "id": "Polyester (Mylar), Polypropylene, Polystyrene." },
  { "en": "Keunggulan Kapasitor Film (Film Capacitor)?", "id": "Stabil, Toleransi Ketat, Non-Polar." },
  { "en": "Apa Itu Kapasitor Mika (Mica Capacitor)?", "id": "Kapasitor Dielektrik Mika Alami." },
  { "en": "Keunggulan Kapasitor Mika (Mica Capacitor)?", "id": "Sangat Stabil, Presisi Tinggi (RF)." },
  { "en": "Apa Itu Kapasitor Tantalum (Tantalum Capacitor)?", "id": "Jenis Kapasitor Elektrolit Padat." },
  { "en": "Keunggulan Kapasitor Tantalum (Tantalum Capacitor)?", "id": "Kepadatan Tinggi, ESR Rendah." },
  { "en": "Apa Itu Kapasitor Variabel (Variable Capacitor)?", "id": "Kapasitansi Dapat Diatur Secara Mekanis." },
  { "en": "Dimana Kapasitor Variabel (Variable Capacitor) Digunakan?", "id": "Rangkaian Penala (Tuning) Radio." },
  { "en": "Apa Itu Kapasitor Trimmer (Trimmer Capacitor)?", "id": "Kapasitor Variabel Kecil Penyetelan Presisi." },
  { "en": "Apa Itu Toleransi (Tolerance) Kapasitor?", "id": "Penyimpangan Nilai Kapasitansi Dari Nominal." },
  { "en": "Apa Itu Tegangan Kerja (Working Voltage) Kapasitor?", "id": "Tegangan Maksimum Aman Diterapkan." },
  { "en": "Apa Itu Koefisien Suhu (Temperature Coefficient) Kapasitor?", "id": "Perubahan Kapasitansi Terhadap Suhu." },
  { "en": "Apa Itu ESR (Equivalent Series Resistance)?", "id": "Resistansi Seri Internal Kapasitor Ideal." },
  { "en": "ESR (Equivalent Series Resistance) Rendah Diinginkan?", "id": "Ya, Mengurangi Rugi Daya Internal." },
  { "en": "Apa Itu ESL (Equivalent Series Inductance)?", "id": "Induktansi Seri Internal Kapasitor Ideal." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Kapasitor?", "id": "Arus Kecil Lewat Dielektrik Kapasitor." },
  { "en": "Apa Itu Penandaan Kode (Code Marking) Kapasitor?", "id": "Kode Angka Menunjukkan Nilai Kapasitansi." },
  { "en": "Arti Kode 104 Pada Kapasitor Keramik?", "id": "Seratus N F (100 NF) Atau 0.1 Μf." },
  { "en": "Arti Kode 222 Pada Kapasitor Keramik?", "id": "Dua Koma Dua N F (2.2 NF)." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Komponen Pasif Menyimpan Energi Magnetik." },
  { "en": "Apa Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Apa Itu Inti (Core) Induktor?", "id": "Material Pusat Lilitan Kawat." },
  { "en": "Jenis Inti (Core) Induktor Umum?", "id": "Udara, Besi, Ferit." },
  { "en": "Apa Keuntungan Inti (Core) Ferit?", "id": "Rugi Rendah Pada Frekuensi Tinggi." },
  { "en": "Apa Keuntungan Inti (Core) Besi?", "id": "Induktansi Tinggi Frekuensi Rendah." },
  { "en": "Apa Itu Induktor Inti Udara (Air Core)?", "id": "Induktor Tanpa Material Inti Magnetik." },
  { "en": "Dimana Induktor Inti Udara (Air Core) Digunakan?", "id": "Aplikasi Frekuensi Radio (RF) Tinggi." },
  { "en": "Apa Itu Induktor Toroidal (Toroidal Inductor)?", "id": "Lilitan Kawat Pada Inti Donat." },
  { "en": "Keuntungan Induktor Toroidal (Toroidal Inductor)?", "id": "Medan Magnet Terbatas, Efisiensi Tinggi." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor) Induktor?", "id": "Rasio Reaktansi Terhadap Resistansi Seri." },
  { "en": "Q (Faktor Kualitas) Tinggi Induktor Menandakan Apa?", "id": "Induktor Lebih Ideal (Rugi Rendah)." },
  { "en": "Apa Itu Self-Resonant Frequency (SRF) Induktor?", "id": "Frekuensi Induktor Beresonansi Kapasitansi Parasitik." },
  { "en": "Apa Itu Arus Saturasi (Saturation Current) Induktor?", "id": "Arus Menyebabkan Induktansi Turun Signifikan." },
  { "en": "Apa Itu Resistansi DC (DC Resistance) (DCR) Induktor?", "id": "Resistansi Kawat Lilitan Induktor." },
  { "en": "DCR (DC Resistance) Rendah Diinginkan?", "id": "Ya, Mengurangi Rugi Daya Konduksi." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Komponen Pasif Mengubah Tegangan AC." },
  { "en": "Apa Prinsip Kerja Transformator (Transformer)?", "id": "Induksi Elektromagnetik Mutual." },
  { "en": "Apa Itu Belitan Primer (Primary Winding)?", "id": "Sisi Input Transformator." },
  { "en": "Apa Itu Belitan Sekunder (Secondary Winding)?", "id": "Sisi Output Transformator." },
  { "en": "Apa Itu Rasio Belitan (Turns Ratio)?", "id": "Perbandingan Jumlah Lilitan Primer, Sekunder." },
  { "en": "Apa Itu Trafo Step-Up (Penaik)?", "id": "Menaikkan Tegangan Sekunder (Ns > Np)." },
  { "en": "Apa Itu Trafo Step-Down (Penurun)?", "id": "Menurunkan Tegangan Sekunder (Ns < Np)." },
  { "en": "Apa Itu Trafo Isolasi (Isolation Transformer)?", "id": "Rasio 1:1 Memberi Isolasi Galvanik." },
  { "en": "Apa Itu Autotransformator (Autotransformer)?", "id": "Transformator Hanya Satu Belitan." },
  { "en": "Apa Itu Efisiensi (Efficiency) Transformator?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Rugi Utama (Main Loss) Transformator?", "id": "Rugi Tembaga Dan Rugi Inti." },
  { "en": "Apa Itu Rugi Tembaga (Copper Loss)?", "id": "Rugi Akibat Resistansi Belitan (I²R)." },
  { "en": "Apa Itu Rugi Inti (Core Loss)?", "id": "Rugi Histeresis Dan Arus Eddy." },
  { "en": "Apa Itu Arus Magnetisasi (Magnetizing Current)?", "id": "Arus Membangun Fluks Magnetik Inti." },
  { "en": "Apa Itu Arus Inrush (Inrush Current)?", "id": "Arus Tinggi Sesaat Trafo Dihidupkan." },
  { "en": "Apa Itu Impedansi (Impedance) Trafo?", "id": "Impedansi Internal Dilihat Dari Terminal." },
  { "en": "Apa Itu Regulasi Tegangan (Voltage Regulation)?", "id": "Perubahan Tegangan Output Saat Berbeban." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis." },
  { "en": "Bagian Utama Relay (Relay) Elektromekanis?", "id": "Koil Elektromagnet Dan Kontak Saklar." },
  { "en": "Apa Fungsi Koil (Coil) Relay?", "id": "Membangkitkan Medan Magnet Saat Dialiri Arus." },
  { "en": "Apa Fungsi Kontak (Contact) Relay?", "id": "Menyambung Atau Memutus Sirkuit Beban." },
  { "en": "Apa Itu Kontak Normally Open (NO)?", "id": "Terbuka Saat Relay Tidak Aktif." },
  { "en": "Apa Itu Kontak Normally Closed (NC)?", "id": "Tertutup Saat Relay Tidak Aktif." },
  { "en": "Apa Itu Kontak Common (COM)?", "id": "Terminal Umum Kontak NO/NC." },
  { "en": "Apa Itu Relay (Relay) SPST?", "id": "Single Pole Single Throw (Satu NO/NC)." },
  { "en": "Apa Itu Relay (Relay) SPDT?", "id": "Single Pole Double Throw (NO, NC, COM)." },
  { "en": "Apa Itu Relay (Relay) DPDT?", "id": "Double Pole Double Throw (Dua Set SPDT)." },
  { "en": "Apa Itu Tegangan Koil (Coil Voltage)?", "id": "Tegangan Operasi Untuk Mengaktifkan Koil." },
  { "en": "Apa Itu Arus Kontak (Contact Current Rating)?", "id": "Arus Maksimum Dapat Dilewatkan Kontak." },
  { "en": "Apa Itu Dioda Flyback (Flyback Diode)?", "id": "Dioda Paralel Koil Lindungi Driver." },
  { "en": "Mengapa Perlu Dioda Flyback (Flyback Diode)?", "id": "Meredam Tegangan Induksi Balik Koil." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Tanpa Bagian Bergerak (Semikonduktor)." },
  { "en": "Apa Keuntungan SSR (Solid State Relay)?", "id": "Lebih Cepat, Awet, Tidak Berisik." },
  { "en": "Apa Kerugian SSR (Solid State Relay)?", "id": "Menghasilkan Panas, Jatuh Tegangan Lebih Besar." },
  { "en": "Apa Komponen Input SSR (Solid State Relay)?", "id": "LED (Light Emitting Diode) (Optocoupler)." },
  { "en": "Apa Komponen Output SSR (Solid State Relay) AC?", "id": "TRIAC (Triode for Alternating Current) Atau SCR (Silicon Controlled Rectifier) Anti-Paralel." },
  { "en": "Apa Komponen Output SSR (Solid State Relay) DC?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) Atau BJT (Bipolar Junction Transistor)." },
  { "en": "Apa Itu Zero Crossing Detection (Deteksi Persilangan Nol)?", "id": "Fitur SSR (Solid State Relay) Hidupkan Dekat Tegangan Nol." },
  { "en": "Manfaat Zero Crossing Detection (Deteksi Persilangan Nol)?", "id": "Mengurangi EMI (Electromagnetic Interference) Saat Switching." },
  { "en": "Apa Itu Random Turn-On (Hidup Acak) SSR?", "id": "SSR (Solid State Relay) Hidup Langsung Saat Input Aktif." },
  { "en": "Apa Itu Kontaktor (Contactor)?", "id": "Relay (Relay) Daya Besar Kendali Motor." },
  { "en": "Apa Perbedaan Utama Relay, Kontaktor?", "id": "Kontaktor (Kontaktor) Untuk Arus Jauh Lebih Besar." },
  { "en": "Apa Itu Kontak Bantu (Auxiliary Contact)?", "id": "Kontak Tambahan Kontaktor/Relay (Sinyal Logika)." },
  { "en": "Apa Itu Thermal Overload Relay (TOR)?", "id": "Pelindung Motor Arus Berlebih Termal." },
  { "en": "Bagaimana TOR (Thermal Overload Relay) Bekerja?", "id": "Elemen Bimetal Memuai Putus Kontak." },
  { "en": "Apa Itu Saklar (Switch)?", "id": "Komponen Mekanis Buka/Tutup Sirkuit." },
  { "en": "Apa Itu Saklar Toggle (Toggle Switch)?", "id": "Saklar Tuas On/Off." },
  { "en": "Apa Itu Saklar Rocker (Rocker Switch)?", "id": "Saklar Jungkat-Jungkit On/Off." },
  { "en": "Apa Itu Saklar Slide (Slide Switch)?", "id": "Saklar Geser Pilih Posisi." },
  { "en": "Apa Itu Saklar Putar (Rotary Switch)?", "id": "Saklar Pilih Posisi Dengan Memutar." },
  { "en": "Apa Itu Tombol Tekan (Push Button Switch)?", "id": "Saklar Aktif Saat Ditekan." },
  { "en": "Apa Itu Saklar Momentary (Momentary Switch)?", "id": "Aktif Hanya Selama Ditekan/Digerakkan." },
  { "en": "Apa Itu Saklar Latching (Latching Switch)?", "id": "Tetap Posisi Terakhir Setelah Digerakkan." },
  { "en": "Apa Itu Saklar Batas (Limit Switch)?", "id": "Saklar Aktif Sentuhan Objek Mekanis." },
  { "en": "Apa Itu Saklar Reed (Reed Switch)?", "id": "Saklar Kontak Dalam Tabung Kaca (Magnet)." },
  { "en": "Bagaimana Saklar Reed (Reed Switch) Diaktifkan?", "id": "Oleh Medan Magnet Eksternal." },
  { "en": "Apa Itu Saklar Merkuri (Mercury Switch)?", "id": "Saklar Gunakan Merkuri Cair Konduktor." },
  { "en": "Apa Itu Saklar DIP (Dual In-line Package)?", "id": "Kelompok Saklar Kecil Paket IC." },
  { "en": "Apa Itu Keypad (Keypad)?", "id": "Susunan Tombol (Biasanya Matriks)." },
  { "en": "Apa Itu Bounce (Pantulan) Kontak Saklar?", "id": "Getaran Kontak Saat Buka/Tutup." },
  { "en": "Apa Akibat Bounce (Pantulan) Kontak?", "id": "Pembacaan Ganda Sinyal Digital." },
  { "en": "Bagaimana Mengatasi Bounce (Pantulan) Kontak?", "id": "Perangkat Keras (RC) Atau Lunak (Debouncing)." },
  { "en": "Apa Itu Rangkaian Debouncing (Debouncing Circuit)?", "id": "Filter Menghilangkan Efek Bounce." },
  { "en": "Apa Itu Konektor (Connector)?", "id": "Perangkat Penghubung Antar Kabel/Peralatan." },
  { "en": "Apa Itu Konektor Header (Header Connector)?", "id": "Konektor Pin Tegak Lurus PCB." },
  { "en": "Apa Itu Konektor Soket (Socket Connector)?", "id": "Konektor Wadah Menerima Pin Header." },
  { "en": "Apa Itu Konektor USB (Universal Serial Bus)?", "id": "Konektor Standar Komunikasi Serial." },
  { "en": "Apa Itu Konektor Audio Jack (Jack Audio)?", "id": "Konektor Umum Sinyal Audio (TRS/TRRS)." },
  { "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?", "id": "Konektor Koaksial RF Tipe Bayonet." },
  { "en": "Apa Itu Konektor SMA (SubMiniature version A)?", "id": "Konektor Koaksial RF Ulir Kecil." },
  { "en": "Apa Itu Konektor DB9/DB25 (DB9/DB25 Connector)?", "id": "Konektor Standar Serial RS-232." },
  { "en": "Apa Itu Terminal Block (Blok Terminal)?", "id": "Konektor Sekrup Sambungan Kawat." },
  { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Kabel Datar Banyak Konduktor Paralel." },
  { "en": "Apa Itu Konektor IDC (Insulation Displacement Connector)?", "id": "Konektor Kabel Pita Tanpa Kupas." },
  { "en": "Apa Itu Kabel Koaksial (Coaxial Cable)?", "id": "Kabel Konduktor Pusat Pelindung Melingkar." },
  { "en": "Apa Fungsi Pelindung (Shield) Kabel Koaksial?", "id": "Melindungi Sinyal Dari Interferensi." },
  { "en": "Impedansi (Impedance) Umum Kabel Koaksial?", "id": "Lima Puluh Ohm Atau Tujuh Puluh Lima Ohm." },
  { "en": "Apa Itu Kabel Twisted Pair (Pasangan Terpilin)?", "id": "Dua Kawat Konduktor Dipilin Bersama." },
  { "en": "Mengapa Kawat Dipilin (Twisted)?", "id": "Mengurangi Interferensi Elektromagnetik (EMI)." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Twisted Pair Tanpa Pelindung." },
  { "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?", "id": "Kabel Twisted Pair Dengan Pelindung." },
  { "en": "Kategori Kabel UTP (Unshielded Twisted Pair) Umum Jaringan?", "id": "Cat5e, Cat6, Cat6a." },
  { "en": "Apa Itu Konektor RJ45 (Registered Jack 45)?", "id": "Konektor Standar Kabel Ethernet UTP." },
  { "en": "Apa Itu Kabel Serat Optik (Fiber Optic)?", "id": "Kabel Transmisi Data Cahaya." },
  { "en": "Apa Keunggulan Kabel Serat Optik?", "id": "Bandwidth Tinggi, Kebal Interferensi." },
  { "en": "Apa Inti (Core) Serat Optik?", "id": "Bagian Tengah Serat Tempat Cahaya." },
  { "en": "Apa Selubung (Cladding) Serat Optik?", "id": "Lapisan Sekitar Inti Pantulkan Cahaya." },
  { "en": "Apa Itu Single Mode Fiber (SMF)?", "id": "Serat Optik Inti Sangat Kecil." },
  { "en": "Apa Keunggulan SMF (Single Mode Fiber)?", "id": "Jarak Sangat Jauh, Bandwidth Tinggi." },
  { "en": "Apa Itu Multi Mode Fiber (MMF)?", "id": "Serat Optik Inti Lebih Besar." },
  { "en": "Apa Keunggulan MMF (Multi Mode Fiber)?", "id": "Lebih Murah, Konektor Lebih Mudah." },
  { "en": "Apa Sumber Cahaya (Light Source) SMF?", "id": "Laser Diode (LD)." },
  { "en": "Apa Sumber Cahaya (Light Source) MMF?", "id": "LED (Light Emitting Diode) Atau VCSEL." },
  { "en": "Apa Kepanjangan VCSEL (Vertical Cavity Surface Emitting Laser)?", "id": "Laser Pemancar Permukaan Rongga Vertikal." },
  { "en": "Apa Itu Atenuasi (Attenuation) Serat Optik?", "id": "Pelemahan Intensitas Cahaya Sepanjang Serat." },
  { "en": "Satuan Atenuasi (Attenuation) Serat Optik?", "id": "Desibel Per Kilometer (dB/km)." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Pelebaran Pulsa Cahaya Sepanjang Serat." },
  { "en": "Akibat Dispersi (Dispersion) Serat Optik?", "id": "Membatasi Laju Data Maksimal." },
  { "en": "Apa Jenis Dispersi (Dispersion) Utama?", "id": "Kromatik Dan Mode Polarisasi (PMD)." },
  { "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?", "id": "Perbedaan Kecepatan Rambat Panjang Gelombang." },
  { "en": "Apa Itu Dispersi Mode Polarisasi (PMD)?", "id": "Perbedaan Kecepatan Rambat Dua Polarisasi." },
  { "en": "Apa Itu Penyambungan (Splicing) Serat Optik?", "id": "Menyambung Dua Ujung Serat Permanen." },
  { "en": "Metode Splicing (Penyambungan) Umum?", "id": "Splicing Fusi (Fusion Splicing)." },
  { "en": "Apa Itu Konektor (Connector) Serat Optik?", "id": "Alat Sambungan Serat Optik Non-Permanen." },
  { "en": "Contoh Tipe Konektor (Connector) Optik?", "id": "SC, LC, ST, FC." },
  { "en": "Apa Itu Rugi Penyisipan (Insertion Loss)?", "id": "Kehilangan Daya Akibat Konektor/Splicing." },
  { "en": "Apa Itu Rugi Kembali (Return Loss)?", "id": "Ukuran Cahaya Dipantulkan Kembali Sambungan." },
  { "en": "Apa Itu Pengukur Daya Optik (OPM)?", "id": "Optical Power Meter (OPM)." },
  { "en": "Apa Kepanjangan OPM (Optical Power Meter)?", "id": "Pengukur Daya Optik." },
  { "en": "Apa Fungsi OPM (Optical Power Meter)?", "id": "Mengukur Kekuatan Sinyal Cahaya (dBm)." },
  { "en": "Apa Itu Sumber Cahaya Optik (OLS)?", "id": "Optical Light Source (OLS)." },
  { "en": "Apa Kepanjangan OLS (Optical Light Source)?", "id": "Sumber Cahaya Optik." },
  { "en": "Apa Fungsi OLS (Optical Light Source) & OPM?", "id": "Mengukur Total Redaman Jalur Optik." },
  { "en": "Apa Itu OTDR (Optical Time Domain Reflectometer)?", "id": "Alat Karakterisasi Serat Optik." },
  { "en": "Apa Fungsi OTDR (Optical Time Domain Reflectometer)?", "id": "Ukur Panjang, Redaman, Lokasi Gangguan." },
  { "en": "Apa Itu Visual Fault Locator (VFL)?", "id": "Sumber Laser Merah Cari Tekukan/Putus." },
  { "en": "Apa Kepanjangan VFL (Visual Fault Locator)?", "id": "Lokator Kesalahan Visual." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Berdasarkan Panjang Gelombang." },
  { "en": "Apa Fungsi WDM (Wavelength Division Multiplexing)?", "id": "Tingkatkan Kapasitas Transmisi Satu Serat." },
  { "en": "Apa Itu DWDM (Dense Wavelength Division Multiplexing)?", "id": "WDM (Wavelength Division Multiplexing) Kanal Sangat Rapat." },
  { "en": "Apa Itu CWDM (Coarse Wavelength Division Multiplexing)?", "id": "WDM (Wavelength Division Multiplexing) Kanal Lebih Lebar." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Tanpa Konversi." },
  { "en": "Contoh Penguat Optik (Optical Amplifier)?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Transceiver (Transceiver) Optik?", "id": "Modul Gabungan Pemancar, Penerima Optik." },
  { "en": "Contoh Form Factor Transceiver (Transceiver)?", "id": "SFP, SFP+, QSFP." },
  { "en": "Apa Kepanjangan SFP (Small Form-factor Pluggable)?", "id": "Form Faktor Kecil Dapat Dicolok." },
  { "en": "Apa Itu Jaringan Optik Pasif (PON)?", "id": "Arsitektur Jaringan Serat Ke Rumah." },
  { "en": "Komponen Pasif Utama PON (Passive Optical Network)?", "id": "Splitter Optik (Optical Splitter)." },
  { "en": "Apa Itu Splitter (Splitter) Optik?", "id": "Membagi Sinyal Cahaya Ke Banyak Arah." },
  { "en": "Apa Itu OLT (Optical Line Terminal)?", "id": "Perangkat Pusat Jaringan PON." },
  { "en": "Apa Itu ONT/ONU (Optical Network Terminal/Unit)?", "id": "Perangkat Di Sisi Pelanggan PON." },
  { "en": "Standar PON (Passive Optical Network) Umum?", "id": "GPON (Gigabit PON), EPON (Ethernet PON)." },
  { "en": "Apa Itu Fotolistrik (Photoelectricity)?", "id": "Interaksi Cahaya Dengan Materi Listrik." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Emisi Elektron Logam Disinari Cahaya." },
  { "en": "Apa Itu Efek Fotokonduktif (Photoconductive Effect)?", "id": "Perubahan Konduktivitas Bahan Disinari Cahaya." },
  { "en": "Apa Itu Efek Fotovoltaik (Photovoltaic Effect)?", "id": "Pembangkitan Tegangan Sambungan Disinari Cahaya." },
  { "en": "Komponen Apa Berbasis Efek Fotovoltaik?", "id": "Sel Surya (Solar Cell)." },
  { "en": "Apa Itu Elektroluminesensi (Electroluminescence)?", "id": "Pancaran Cahaya Bahan Dialiri Listrik." },
  { "en": "Komponen Apa Berbasis Elektroluminesensi?", "id": "LED (Light Emitting Diode), OLED (Organic LED)." },
  { "en": "Apa Itu Katodoluminesensi (Cathodoluminescence)?", "id": "Pancaran Cahaya Bahan Ditembak Elektron." },
  { "en": "Dimana Katodoluminesensi (Cathodoluminescence) Digunakan?", "id": "Layar Tabung CRT (Cathode Ray Tube) Lama." },
  { "en": "Apa Itu Termoluminesensi (Thermoluminescence)?", "id": "Pancaran Cahaya Bahan Dipanaskan." },
  { "en": "Apa Itu Pendarfosfor (Phosphorescence)?", "id": "Pancaran Cahaya Tertunda Setelah Eksitasi." },
  { "en": "Apa Itu Pendarfluor (Fluorescence)?", "id": "Pancaran Cahaya Langsung Saat Eksitasi." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber Cahaya Koheren, Monokromatik." },
  { "en": "Prinsip Dasar Laser (Laser)?", "id": "Emisi Terstimulasi (Stimulated Emission)." },
  { "en": "Komponen Utama Laser (Laser)?", "id": "Medium Aktif, Sumber Pompa, Resonator Optik." },
  { "en": "Apa Itu Medium Aktif (Active Medium) Laser?", "id": "Bahan Tempat Terjadi Inversi Populasi." },
  { "en": "Apa Itu Sumber Pompa (Pump Source) Laser?", "id": "Sumber Energi Eksitasi Medium Aktif." },
  { "en": "Apa Itu Resonator Optik (Optical Resonator) Laser?", "id": "Dua Cermin Pantulkan Cahaya Penguatan." },
  { "en": "Apa Itu Cahaya Koheren (Coherent Light)?", "id": "Gelombang Cahaya Fasa Sinkron." },
  { "en": "Apa Itu Cahaya Monokromatik (Monochromatic Light)?", "id": "Cahaya Satu Panjang Gelombang." },
  { "en": "Apa Itu Kolimasi (Collimation) Berkas Laser?", "id": "Berkas Cahaya Paralel (Tidak Menyebar)." },
  { "en": "Jenis Laser (Laser) Utama?", "id": "Laser Gas, Padat, Semikonduktor, Pewarna." },
  { "en": "Contoh Laser Gas (Gas Laser)?", "id": "Laser Helium-Neon (HeNe), CO₂." },
  { "en": "Contoh Laser Padat (Solid-State Laser)?", "id": "Laser Nd:YAG, Ruby." },
  { "en": "Contoh Laser Semikonduktor (Semiconductor Laser)?", "id": "Dioda Laser (Laser Diode)." },
  { "en": "Apa Aplikasi Laser (Laser)?", "id": "Komunikasi Optik, Pembaca CD/DVD, Industri, Medis." },
  { "en": "Apa Itu Holografi (Holography)?", "id": "Teknik Rekaman Gambar Tiga Dimensi Laser." },
  { "en": "Apa Itu Interferometri (Interferometry)?", "id": "Teknik Pengukuran Presisi Interferensi Cahaya." },
  { "en": "Apa Itu Spektroskopi (Spectroscopy)?", "id": "Analisis Interaksi Cahaya, Materi." },
  { "en": "Apa Itu Meter Daya Optik (Optical Power Meter)?", "id": "Mengukur Kekuatan Sinyal Cahaya." },
  { "en": "Satuan Daya Optik (Optical Power) Umum?", "id": "Desibel Miliwatt (dBm)." },
  { "en": "Apa Hubungan dBm (Desibel Miliwatt) Ke Milliwatt?", "id": "dBm = 10 * log10(P_mW)." },
  { "en": "Apa Itu Redaman (Attenuation)?", "id": "Pelemahan Sinyal." },
  { "en": "Apa Satuan Redaman (Attenuation)?", "id": "Desibel (dB)." },
  { "en": "Redaman (Attenuation) 3 dB Berarti Apa?", "id": "Daya Berkurang Setengah." },
  { "en": "Redaman (Attenuation) 10 dB Berarti Apa?", "id": "Daya Berkurang Sepuluh Kali." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Peningkatan Kekuatan Sinyal." },
  { "en": "Apa Satuan Penguatan (Gain)?", "id": "Desibel (dB)." },
  { "en": "Penguatan (Gain) 3 dB Berarti Apa?", "id": "Daya Menjadi Dua Kali Lipat." },
  { "en": "Apa Itu Pemrosesan Sinyal Analog (Analog Signal Processing)?", "id": "Manipulasi Sinyal Bentuk Kontinu." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital (Digital Signal Processing)?", "id": "Manipulasi Sinyal Bentuk Diskrit." },
  { "en": "Langkah Dasar DSP (Digital Signal Processing)?", "id": "Sampling, Kuantisasi, Pemrosesan, Rekonstruksi." },
  { "en": "Apa Itu Aliasing (Aliasing) DSP?", "id": "Frekuensi Tinggi Muncul Frekuensi Rendah." },
  { "en": "Apa Itu Filter Anti-Aliasing (Anti-Aliasing Filter)?", "id": "Filter Lolos Rendah Sebelum Sampling." },
  { "en": "Apa Itu Filter Rekonstruksi (Reconstruction Filter)?", "id": "Filter Lolos Rendah Setelah DAC." },
  { "en": "Apa Itu Transformasi Fourier Diskrit (DFT)?", "id": "Analisis Frekuensi Sinyal Waktu Diskrit." },
  { "en": "Apa Kepanjangan DFT (Discrete Fourier Transform)?", "id": "Transformasi Fourier Diskrit." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Menghitung DFT." },
  { "en": "Apa Kepanjangan FFT (Fast Fourier Transform)?", "id": "Transformasi Fourier Cepat." },
  { "en": "Apa Itu Windowing (Windowing) DSP?", "id": "Mengalikan Sinyal Dengan Fungsi Window." },
  { "en": "Tujuan Windowing (Windowing) DSP?", "id": "Mengurangi Kebocoran Spektral (Spectral Leakage)." },
  { "en": "Contoh Fungsi Window (Window Function)?", "id": "Rectangular, Hamming, Hanning, Blackman." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma Modifikasi Spektrum Sinyal Digital." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Tanpa Umpan Balik (Selalu Stabil)." },
  { "en": "Keuntungan Filter FIR (Finite Impulse Response)?", "id": "Fasa Linear Dapat Dirancang." },
  { "en": "Kerugian Filter FIR (Finite Impulse Response)?", "id": "Membutuhkan Orde Lebih Tinggi Dari IIR." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Dengan Umpan Balik (Bisa Tidak Stabil)." },
  { "en": "Keuntungan Filter IIR (Infinite Impulse Response)?", "id": "Lebih Efisien Komputasi Dari FIR." },
  { "en": "Kerugian Filter IIR (Infinite Impulse Response)?", "id": "Fasa Non-Linear." },
  { "en": "Metode Desain Filter FIR (Finite Impulse Response)?", "id": "Window Method, Frequency Sampling." },
  { "en": "Metode Desain Filter IIR (Infinite Impulse Response)?", "id": "Bilinear Transform, Impulse Invariance." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit Domain-Z." },
  { "en": "Apa Itu Pole (Kutub) Dan Zero (Nol) Domain-Z?", "id": "Menentukan Kestabilan, Respon Frekuensi Filter." },
  { "en": "Kondisi Kestabilan (Stability) Filter Digital Domain-Z?", "id": "Semua Pole Berada Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Prosesor Sinyal Digital (DSP Processor)?", "id": "Mikroprosesor Dioptimalkan Operasi DSP." },
  { "en": "Arsitektur Apa Umum Digunakan DSP Processor?", "id": "Arsitektur Harvard Modifikasi." },
  { "en": "Apa Instruksi Khusus DSP Processor?", "id": "Instruksi MAC (Multiply-Accumulate)." },
  { "en": "Apa Itu Fixed-Point (Titik Tetap) DSP?", "id": "Representasi Bilangan Dengan Posisi Koma Tetap." },
  { "en": "Apa Itu Floating-Point (Titik Mengambang) DSP?", "id": "Representasi Bilangan Dengan Eksponen, Mantissa." },
  { "en": "Mana Lebih Cepat, Fixed Atau Floating Point?", "id": "Fixed-Point Umumnya Lebih Cepat, Hemat Daya." },
  { "en": "Mana Rentang Dinamis Lebih Besar?", "id": "Floating-Point." },
  { "en": "Apa Itu Upsampling (Upsampling)?", "id": "Meningkatkan Laju Sampel Sinyal Digital." },
  { "en": "Apa Itu Downsampling (Downsampling)?", "id": "Mengurangi Laju Sampel Sinyal Digital." },
  { "en": "Apa Itu Interpolasi (Interpolation) DSP?", "id": "Menambah Sampel Di Antara Sampel Asli." },
  { "en": "Apa Itu Desimasi (Decimation) DSP?", "id": "Membuang Sampel Sinyal Digital." },
  { "en": "Apa Itu Filter Multirate (Multirate Filter)?", "id": "Filter Operasi Laju Sampel Berbeda." },
  { "en": "Apa Itu Filter CIC (Cascaded Integrator-Comb)?", "id": "Filter Efisien Interpolasi/Desimasi Besar." },
  { "en": "Apa Itu Adaptive Filter (Filter Adaptif)?", "id": "Filter Koefisien Otomatis Menyesuaikan Diri." },
  { "en": "Aplikasi Adaptive Filter (Filter Adaptif)?", "id": "Pembatalan Derau, Ekualisasi Kanal." },
  { "en": "Algoritma Adaptif (Adaptive Algorithm) Umum?", "id": "LMS (Least Mean Squares), RLS (Recursive Least Squares)." },
  { "en": "Apa Itu Spektral Analisis (Spectral Analysis)?", "id": "Estimasi Spektrum Daya Sinyal." },
  { "en": "Metode Spektral Analisis (Spectral Analysis)?", "id": "Periodogram, Metode Welch." },
  { "en": "Apa Itu Periodogram (Periodogram)?", "id": "Estimasi Spektrum Berbasis DFT Kuadrat." },
  { "en": "Apa Itu Metode Welch (Welch's Method)?", "id": "Rata-Rata Periodogram Segmen Bertumpuk." },
  { "en": "Apa Itu Transformasi Wavelet (Wavelet Transform)?", "id": "Analisis Sinyal Domain Waktu-Frekuensi." },
  { "en": "Apa Keunggulan Transformasi Wavelet?", "id": "Resolusi Waktu Baik Frekuensi Tinggi, Sebaliknya." },
  { "en": "Apa Itu Kompresi Data (Data Compression)?", "id": "Mengurangi Jumlah Bit Representasi Data." },
  { "en": "Apa Itu Kompresi Lossless (Tanpa Rugi)?", "id": "Data Asli Dapat Dipulihkan Sempurna." },
  { "en": "Apa Itu Kompresi Lossy (Berugi)?", "id": "Sebagian Informasi Dihilangkan Kompresi." },
  { "en": "Contoh Algoritma Kompresi Lossless?", "id": "Huffman Coding, Lempel-Ziv (LZ)." },
  { "en": "Contoh Format Kompresi Lossy?", "id": "JPEG (Gambar), MP3 (Audio), MPEG (Video)." },
  { "en": "Apa Itu Kode Koreksi Kesalahan (Error Correction Code)?", "id": "Menambah Redundansi Deteksi/Koreksi Error." },
  { "en": "Contoh Kode Koreksi Kesalahan (Error Correction Code)?", "id": "Hamming Code, Reed-Solomon Code." },
  { "en": "Apa Itu Spread Spectrum (Spektrum Tersebar)?", "id": "Teknik Komunikasi Sebar Sinyal Spektrum Luas." },
  { "en": "Keuntungan Spread Spectrum (Spektrum Tersebar)?", "id": "Tahan Interferensi, Keamanan." },
  { "en": "Jenis Spread Spectrum (Spektrum Tersebar) Utama?", "id": "Direct Sequence (DSSS), Frequency Hopping (FHSS)." },
  { "en": "Apa Itu Direct Sequence Spread Spectrum (DSSS)?", "id": "Mengalikan Sinyal Dengan Kode Penyebar Cepat." },
  { "en": "Apa Itu Frequency Hopping Spread Spectrum (FHSS)?", "id": "Mengubah Frekuensi Pembawa Secara Acak Cepat." },
  { "en": "Apa Itu Orthogonal Frequency Division Multiplexing (OFDM)?", "id": "Teknik Modulasi Multi-Carrier Orthogonal." },
  { "en": "Dimana OFDM (Orthogonal Frequency Division Multiplexing) Digunakan?", "id": "Wi-Fi, LTE (Long Term Evolution), DVB (Digital Video Broadcasting)." },
  { "en": "Keuntungan OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Tahan Terhadap Multipath Fading." },
  { "en": "Apa Itu Cyclic Prefix (Awalan Siklik) OFDM?", "id": "Salinan Akhir Simbol Ditambah Ke Awal." },
  { "en": "Fungsi Cyclic Prefix (Awalan Siklik)?", "id": "Mengatasi Inter-Symbol Interference (ISI)." },
  { "en": "Apa Itu Inter-Symbol Interference (ISI)?", "id": "Gangguan Antar Simbol Berdekatan Kanal." },
  { "en": "Apa Itu Channel Equalization (Ekualisasi Kanal)?", "id": "Mengkompensasi Distorsi Akibat Kanal Transmisi." },
  { "en": "Apa Itu Software Defined Radio (SDR)?", "id": "Radio Komponen RF Diimplementasi Software." },
  { "en": "Keuntungan SDR (Software Defined Radio)?", "id": "Fleksibilitas, Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Itu Cognitive Radio (Radio Kognitif)?", "id": "Radio Cerdas Deteksi, Adaptasi Lingkungan RF." },
  { "en": "Apa Itu Analisis Sinyal (Signal Analysis)?", "id": "Ekstraksi Informasi Dari Sinyal." },
  { "en": "Apa Itu Sintesis Sinyal (Signal Synthesis)?", "id": "Pembangkitan Sinyal Dengan Karakteristik Tertentu." },
  { "en": "Apa Itu Pemodelan Sistem (System Modeling)?", "id": "Membuat Representasi Matematis Sistem." },
  { "en": "Apa Itu Estimasi Parameter (Parameter Estimation)?", "id": "Menentukan Nilai Parameter Model." },
  { "en": "Apa Itu Filter Wiener (Wiener Filter)?", "id": "Filter Optimal Estimasi Sinyal (MSE Minimum)." },
  { "en": "Apa Itu Filter Kalman (Kalman Filter)?", "id": "Filter Rekursif Optimal Estimasi Keadaan." },
  { "en": "Apa Itu Konverter Laju Sampel (Sample Rate Converter)?", "id": "Mengubah Laju Sampel Sinyal Digital." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect)?", "id": "Perubahan Frekuensi Akibat Gerak Relatif." },
  { "en": "Bagaimana Koreksi Efek Doppler (Doppler Effect)?", "id": "Mengestimasi, Mengkompensasi Pergeseran Frekuensi." },
  { "en": "Apa Itu Sinkronisasi Waktu (Time Synchronization)?", "id": "Menyelaraskan Clock Penerima, Pemancar." },
  { "en": "Apa Itu Sinkronisasi Frekuensi (Frequency Synchronization)?", "id": "Menyelaraskan Frekuensi Osilator Lokal." },
  { "en": "Apa Itu Sinkronisasi Fasa (Phase Synchronization)?", "id": "Menyelaraskan Fasa Sinyal Pembawa." },
  { "en": "Apa Itu Carrier Recovery (Pemulihan Pembawa)?", "id": "Proses Estimasi Frekuensi, Fasa Pembawa." },
  { "en": "Apa Itu Timing Recovery (Pemulihan Waktu)?", "id": "Proses Estimasi Waktu Sampling Optimal." },
  { "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?", "id": "Plot Titik Simbol Modulasi Digital." },
  { "en": "Apa Itu Error Vector Magnitude (EVM)?", "id": "Ukuran Kualitas Sinyal Modulasi Digital." },
  { "en": "Apa Kepanjangan EVM (Error Vector Magnitude)?", "id": "Magnitudo Vektor Kesalahan." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Rasio Jumlah Bit Error Terhadap Total Bit." },
  { "en": "Apa Kepanjangan BER (Bit Error Rate)?", "id": "Laju Kesalahan Bit." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Kekuatan Sinyal Terhadap Derau." },
  { "en": "Apa Kepanjangan SNR (Signal-to-Noise Ratio)?", "id": "Rasio Sinyal Terhadap Derau." },
  { "en": "Apa Itu Kapasitas Kanal (Channel Capacity)?", "id": "Laju Data Maksimum Teoretis Kanal." },
  { "en": "Apa Itu Teorema Shannon-Hartley (Shannon-Hartley Theorem)?", "id": "Rumus Menghitung Kapasitas Kanal." },
  { "en": "Apa Itu Teori Informasi (Information Theory)?", "id": "Studi Kuantifikasi, Kompresi, Komunikasi Informasi." },
  { "en": "Siapa Bapak Teori Informasi (Father of Information Theory)?", "id": "Claude Shannon." },
  { "en": "Apa Itu Entropi (Entropy) Informasi?", "id": "Ukuran Ketidakpastian Rata-Rata Sumber Informasi." },
  { "en": "Apa Satuan Entropi (Entropy) Informasi?", "id": "Bit (Bit)." },
  { "en": "Apa Itu Pemrosesan Audio Digital (Digital Audio Processing)?", "id": "DSP (Digital Signal Processing) Untuk Sinyal Suara." },
  { "en": "Format Audio Digital Umum?", "id": "WAV (Waveform Audio File Format), MP3 (MPEG-1 Audio Layer III), AAC (Advanced Audio Coding)." },
  { "en": "Apa Itu Kompresi Audio Lossy (Berugi)?", "id": "Menghilangkan Komponen Suara Kurang Terdengar." },
  { "en": "Apa Itu Efek Psikoakustik (Psychoacoustics)?", "id": "Studi Persepsi Suara Manusia." },
  { "en": "Apa Itu Pemrosesan Citra Digital (Digital Image Processing)?", "id": "DSP (Digital Signal Processing) Untuk Gambar Digital." },
  { "en": "Operasi Dasar Pemrosesan Citra?", "id": "Filter Spasial, Transformasi Fourier, Segmentasi." },
  { "en": "Apa Itu Filter Spasial (Spatial Filtering)?", "id": "Operasi Berbasis Lingkungan Piksel." },
  { "en": "Contoh Filter Spasial (Spatial Filtering)?", "id": "Smoothing (Blur), Sharpening (Penajaman)." },
  { "en": "Apa Itu Histogram (Histogram) Citra?", "id": "Distribusi Tingkat Keabuan Piksel." },
  { "en": "Apa Itu Ekualisasi Histogram (Histogram Equalization)?", "id": "Meningkatkan Kontras Citra." },
  { "en": "Apa Itu Segmentasi Citra (Image Segmentation)?", "id": "Mempartisi Citra Menjadi Objek Bermakna." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Menemukan Batas Objek Dalam Citra." },
  { "en": "Operator Deteksi Tepi (Edge Detection) Contoh?", "id": "Sobel, Canny, Prewitt." },
  { "en": "Apa Itu Pengenalan Pola (Pattern Recognition)?", "id": "Mengidentifikasi Pola Dalam Data." },
  { "en": "Apa Itu Klasifikasi (Classification) Pola?", "id": "Menetapkan Objek Ke Kelas Tertentu." },
  { "en": "Apa Itu Clustering (Pengelompokan) Pola?", "id": "Mengelompokkan Data Mirip Bersama." },
  { "en": "Apa Itu Ekstraksi Fitur (Feature Extraction)?", "id": "Memilih Karakteristik Pembeda Data." },
  { "en": "Apa Itu Sistem Kendali (Control System)?", "id": "Mengelola Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Menggunakan Output Mengatur Input." },
  { "en": "Apa Itu Sistem Loop Terbuka (Open Loop)?", "id": "Kontrol Tanpa Umpan Balik Output." },
  { "en": "Apa Itu Sistem Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik Output." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function)?", "id": "Representasi Matematis Hubungan Input-Output." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Respon Waktu (Time Response)?", "id": "Perilaku Output Sistem Terhadap Waktu." },
  { "en": "Apa Itu Respon Transien (Transient Response)?", "id": "Bagian Awal Respon Sebelum Stabil." },
  { "en": "Apa Itu Respon Keadaan Tunak (Steady State)?", "id": "Perilaku Sistem Setelah Waktu Lama." },
  { "en": "Apa Itu Kesalahan Keadaan Tunak (Steady State Error)?", "id": "Selisih Antara Target, Output Tunak." },
  { "en": "Apa Itu Kontroler PID (Proportional Integral Derivative)?", "id": "Kontroler Umpan Balik Umum Industri." },
  { "en": "Apa Itu Root Locus (Tempat Kedudukan Akar)?", "id": "Metode Analisis Kestabilan Grafis." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Magnitudo, Fasa Respon Frekuensi." },
  { "en": "Apa Itu Gain Margin (GM) (Batas Penguatan)?", "id": "Ukuran Kestabilan Relatif Terhadap Gain." },
  { "en": "Apa Itu Phase Margin (PM) (Batas Fasa)?", "id": "Ukuran Kestabilan Relatif Terhadap Fasa." },
  { "en": "Apa Itu Kontrol Digital (Digital Control)?", "id": "Implementasi Kontroler Menggunakan Komputer." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Tentang Robot." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Gerakan Robot." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Memperhitungkan Gaya." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Independen." },
  { "en": "Apa Itu Sensor (Sensor) Robot?", "id": "Memberi Informasi Tentang Lingkungan/Internal." },
  { "en": "Apa Itu Aktuator (Actuator) Robot?", "id": "Penggerak Sendi Atau Bagian Robot." },
  { "en": "Apa Itu Perencanaan Gerak (Motion Planning)?", "id": "Menentukan Jalur Gerak Robot." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Menginterpretasi Gambar." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis Dan Modifikasi Sinyal." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Dengan Nilai Kontinu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Dengan Nilai Diskrit." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengubah Sinyal Kontinu Jadi Diskrit Waktu." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Mengubah Nilai Sampel Jadi Diskrit Amplitudo." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Memproses Sinyal Digital Untuk Modifikasi Spektrum." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pengiriman Informasi." },
  { "en": "Apa Komponen Dasar Sistem Komunikasi?", "id": "Pemancar, Kanal, Penerima." },
  { "en": "Apa Itu Kanal (Channel) Komunikasi?", "id": "Medium Perambatan Sinyal." },
  { "en": "Apa Itu Noise (Derau) Komunikasi?", "id": "Gangguan Acak Pada Sinyal." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Sinyal Informasi Ke Pembawa." },
  { "en": "Apa Jenis Modulasi (Modulation) Analog Dasar?", "id": "AM (Amplitude Modulation), FM (Frequency Modulation), PM (Phase Modulation)." },
  { "en": "Apa Jenis Modulasi (Modulation) Digital Dasar?", "id": "ASK (Amplitude Shift Keying), FSK (Frequency Shift Keying), PSK (Phase Shift Keying)." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Digunakan Sinyal/Kanal." },
  { "en": "Apa Itu Multiplexing (Multipleksasi)?", "id": "Mengirim Banyak Sinyal Satu Kanal." },
  { "en": "Jenis Multiplexing (Multipleksasi) Umum?", "id": "FDM (Frequency Division Multiplexing), TDM (Time Division Multiplexing), WDM (Wavelength Division Multiplexing)." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Komunikasi Antar Perangkat Jaringan." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Lapisan Jaringan." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Arsitektur Jaringan Internet." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Antarmuka Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Perangkat Penghubung Perangkat Satu Jaringan." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Teknologi Jaringan Lokal Berkabel." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan Lokal Nirkabel." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Global." },
  { "en": "Apa Itu Bahan Konduktor (Conductor Material)?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Apa Itu Bahan Isolator (Insulator Material)?", "id": "Bahan Sulit Alirkan Listrik." },
  { "en": "Apa Itu Bahan Semikonduktor (Semiconductor Material)?", "id": "Bahan Sifat Antara Konduktor, Isolator." },
  { "en": "Apa Itu Celah Energi (Energy Gap)?", "id": "Energi Minimum Eksitasi Elektron Valensi." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Kontrol Konduktivitas." },
  { "en": "Apa Itu Bahan Tipe-N (N-Type Material)?", "id": "Semikonduktor Kelebihan Elektron." },
  { "en": "Apa Itu Bahan Tipe-P (P-Type Material)?", "id": "Semikonduktor Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Dasar Dioda Dan Transistor." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Penguat/Saklar Arus Terkontrol Arus." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Penguat/Saklar Arus Terkontrol Tegangan." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Kombinasi Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Pemancar/Penerima Gelombang Elektromagnetik." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Pemandu Gelombang Elektromagnetik." },
  { "en": "Apa Itu Optika Serat (Fiber Optics)?", "id": "Transmisi Cahaya Melalui Serat Kaca." },
  { "en": "Apa Prinsip Dasar Optika Serat?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Penggunaan Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Dari Gangguan." },
  { "en": "Apa Itu Rele (Relay) Proteksi?", "id": "Sensor Kondisi Abnormal Sistem." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Pemutus Arus Gangguan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengukur Output Untuk Mengatur Input." },
  { "en": "Apa Itu Kontrol PID (PID Control)?", "id": "Metode Kontrol Umpan Balik Populer." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Penentuan Nilai Besaran Fisik." },
  { "en": "Apa Instrumen (Instrument) Pengukuran Listrik?", "id": "Voltmeter, Ammeter, Ohmmeter, Dll." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Membandingkan Alat Ukur Dengan Standar." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty)?", "id": "Estimasi Rentang Kesalahan Pengukuran." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Elemen Pendeteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Elemen Penggerak Hasil Kontrol." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Manipulasi Sinyal Untuk Ekstraksi Informasi." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal Sebagai Fungsi Waktu." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal Sebagai Fungsi Frekuensi." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Konversi Antara Domain Waktu, Frekuensi." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Memodifikasi Komponen Frekuensi Sinyal." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pengiriman, Penerimaan Informasi." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Pemisahan Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Fungsi Kapasitor Timing (Timing Capacitor)?", "id": "Menentukan Waktu Osilator Atau Timer." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant) Relatif (εr)?", "id": "Rasio Permitivitas Bahan Terhadap Vakum." },
  { "en": "Nilai εr (Konstanta Dielektrik Relatif) Udara Kira-Kira Berapa?", "id": "Kira-Kira Satu (≈1)." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Sebelum Tembus." },
  { "en": "Apa Satuan Kekuatan Dielektrik (Dielectric Strength)?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Itu Rugi Dielektrik (Dielectric Loss)?", "id": "Energi Hilang Jadi Panas Dielektrik AC." },
  { "en": "Apa Itu Faktor Disipasi (Dissipation Factor) (DF)?", "id": "Ukuran Rugi Dielektrik Kapasitor." },
  { "en": "Nama Lain Faktor Disipasi (Dissipation Factor)?", "id": "Tangen Rugi (Loss Tangent - Tan δ)." },
  { "en": "Apakah ESR (Equivalent Series Resistance) Kapasitor Konstan?", "id": "Tidak, Berubah Terhadap Frekuensi Suhu." },
  { "en": "Apa Efek ESL (Equivalent Series Inductance) Kapasitor?", "id": "Membatasi Kinerja Kapasitor Frekuensi Tinggi." },
  { "en": "Bagaimana Arus Bocor (Leakage Current) Kapasitor Ideal?", "id": "Nol Ampere." },
  { "en": "Apa Itu Self-Healing (Self-Healing) Kapasitor Film?", "id": "Kemampuan Perbaiki Diri Setelah Breakdown Kecil." },
  { "en": "Apa Itu Resistansi Isolasi (Insulation Resistance) Kapasitor?", "id": "Resistansi Sangat Tinggi Dielektrik DC." },
  { "en": "Apa Itu Induktor Mode Bersama (Common Mode Choke)?", "id": "Menekan Noise Mode Bersama Jalur Daya." },
  { "en": "Bagaimana Lilitan Common Mode Choke (CMC)?", "id": "Dua Lilitan Arah Berlawanan Inti Sama." },
  { "en": "Apa Itu Induktor Mode Diferensial (Differential Mode Inductor)?", "id": "Menekan Noise Mode Diferensial." },
  { "en": "Bagaimana Induktor Mode Diferensial (Differential Mode Inductor) Dipasang?", "id": "Seri Dengan Satu Saluran Daya." },
  { "en": "Apa Itu Inti Udara (Air Core) Induktor?", "id": "Induktor Tanpa Inti Bahan Magnetik." },
  { "en": "Apa Keuntungan Induktor Inti Udara (Air Core)?", "id": "Linearitas Baik, Tidak Ada Saturasi." },
  { "en": "Apa Kerugian Induktor Inti Udara (Air Core)?", "id": "Induktansi Relatif Rendah Ukuran Sama." },
  { "en": "Apa Itu Resistansi AC (AC Resistance) Induktor?", "id": "Resistansi Termasuk Efek Kulit, Kedekatan." },
  { "en": "Apa Itu Kapasitansi Terdistribusi (Distributed Capacitance) Induktor?", "id": "Kapasitansi Parasitik Antar Lilitan." },
  { "en": "Apa Penyebab Utama Rugi Inti (Core Loss)?", "id": "Histeresis Dan Arus Eddy." },
  { "en": "Bagaimana Rugi Histeresis (Hysteresis Loss) Terjadi?", "id": "Energi Ubah Orientasi Domain Magnetik." },
  { "en": "Bagaimana Rugi Arus Eddy (Eddy Current Loss) Terjadi?", "id": "Arus Induksi Berputar Dalam Inti Konduktif." },
  { "en": "Bagaimana Mengurangi Rugi Arus Eddy (Eddy Current Loss)?", "id": "Gunakan Inti Laminasi Atau Ferit." },
  { "en": "Apa Itu Bahan Ferit (Ferrite Material)?", "id": "Keramik Magnetik Resistivitas Tinggi." },
  { "en": "Mengapa Ferit (Ferrite) Baik Frekuensi Tinggi?", "id": "Resistivitas Tinggi Kurangi Arus Eddy." },
  { "en": "Apa Itu Trafo Frekuensi Tinggi (High Frequency Transformer)?", "id": "Trafo Operasi Frekuensi Switching (SMPS)." },
  { "en": "Bahan Inti Apa Untuk Trafo HF?", "id": "Umumnya Bahan Ferit (Ferrite)." },
  { "en": "Apa Itu Trafo Pulsa (Pulse Transformer)?", "id": "Trafo Transmisi Pulsa Listrik." },
  { "en": "Aplikasi Trafo Pulsa (Pulse Transformer)?", "id": "Pemicu Thyristor, Isolasi Sinyal Digital." },
  { "en": "Apa Itu Induktansi Bocor (Leakage Inductance)?", "id": "Induktansi Akibat Fluks Tak Terkopel Penuh." },
  { "en": "Apa Efek Induktansi Bocor (Leakage Inductance)?", "id": "Membatasi Transfer Energi, Sebabkan Spike." },
  { "en": "Apa Itu Kapasitansi Antar Belitan (Interwinding Capacitance)?", "id": "Kapasitansi Parasitik Primer, Sekunder Trafo." },
  { "en": "Apa Efek Kapasitansi Antar Belitan (Interwinding Capacitance)?", "id": "Melewatkan Noise Mode Bersama." },
  { "en": "Apa Itu Perisai Faraday (Faraday Shield) Trafo?", "id": "Lapisan Konduktif Antar Belitan Kurangi Kapasitansi." },
  { "en": "Apa Itu Rasio Penolakan Mode Bersama (CMRR)?", "id": "Common Mode Rejection Ratio (CMRR)." },
  { "en": "Apa Fungsi Perisai Faraday (Faraday Shield)?", "id": "Meningkatkan CMRR (Common Mode Rejection Ratio) Trafo Isolasi." },
  { "en": "Apa Itu Relay (Relay) Buluh (Reed Relay)?", "id": "Relay Kontak Buluh Diaktifkan Koil." },
  { "en": "Apa Keuntungan Relay (Relay) Buluh (Reed Relay)?", "id": "Ukuran Kecil, Switching Cepat (Relatif)." },
  { "en": "Apa Itu Busur Api (Arcing) Kontak Relay?", "id": "Percikan Api Saat Kontak Buka/Tutup." },
  { "en": "Apa Akibat Busur Api (Arcing) Kontak?", "id": "Memperpendek Umur Kontak Relay." },
  { "en": "Bagaimana Mengurangi Busur Api (Arcing) Kontak?", "id": "Gunakan Rangkaian Snubber (RC, Dioda)." },
  { "en": "Apa Itu Rangkaian Snubber (Snubber Circuit)?", "id": "Rangkaian Meredam Lonjakan Tegangan/Arus." },
  { "en": "Apa Itu Snubber (Snubber) RC?", "id": "Resistor Seri Kapasitor Paralel Kontak." },
  { "en": "Apa Itu Bounce (Pantulan) Kontak Mekanis?", "id": "Getaran Singkat Kontak Saat Menutup." },
  { "en": "Apa Itu Waktu Operasi (Operate Time) Relay?", "id": "Waktu Dari Koil Aktif Hingga Kontak Menutup." },
  { "en": "Apa Itu Waktu Lepas (Release Time) Relay?", "id": "Waktu Dari Koil Mati Hingga Kontak Membuka." },
  { "en": "Apa Itu Tegangan Pickup (Pickup Voltage) Relay?", "id": "Tegangan Minimum Koil Aktifkan Relay." },
  { "en": "Apa Itu Tegangan Dropout (Dropout Voltage) Relay?", "id": "Tegangan Maksimum Koil Lepaskan Relay." },
  { "en": "Apa Itu Sekring (Fuse) Termal?", "id": "Sekring Putus Berdasarkan Suhu Berlebih." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker) Termal?", "id": "Trip Akibat Pemanasan Elemen Bimetal." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker) Magnetik?", "id": "Trip Akibat Medan Magnet Arus Tinggi." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker) Termal-Magnetik?", "id": "Kombinasi Proteksi Termal, Magnetik." },
  { "en": "Apa Itu Kapasitas Pemutusan (Breaking Capacity) Pemutus?", "id": "Arus Hubung Singkat Maksimum Dapat Diputus." },
  { "en": "Apa Itu Kurva Trip (Trip Curve) Pemutus?", "id": "Grafik Waktu Trip Vs Arus." },
  { "en": "Apa Itu Diskriminasi (Discrimination) / Selektivitas Proteksi?", "id": "Pemutus Dekat Gangguan Trip Duluan." },
  { "en": "Apa Itu Varistor Oksida Logam (MOV)?", "id": "Metal Oxide Varistor (MOV)." },
  { "en": "Apa Kepanjangan MOV (Metal Oxide Varistor)?", "id": "Varistor Oksida Logam." },
  { "en": "Apa Fungsi MOV (Metal Oxide Varistor)?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Bagaimana MOV (Metal Oxide Varistor) Bekerja?", "id": "Resistansi Tinggi Normal, Rendah Saat Surja." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppressor)?", "id": "Dioda Pelindung Lonjakan Tegangan Cepat." },
  { "en": "Apa Keunggulan Dioda TVS (Transient Voltage Suppressor)?", "id": "Waktu Respon Sangat Cepat." },
  { "en": "Apa Itu Tabung Pelepasan Gas (GDT)?", "id": "Gas Discharge Tube (GDT)." },
  { "en": "Apa Kepanjangan GDT (Gas Discharge Tube)?", "id": "Tabung Pelepasan Gas." },
  { "en": "Apa Fungsi GDT (Gas Discharge Tube)?", "id": "Proteksi Lonjakan Tegangan Energi Tinggi." },
  { "en": "Apa Itu Polyswitch (Polymeric PTC)?", "id": "Sekring Dapat Direset Sendiri (PPTC)." },
  { "en": "Apa Kepanjangan PPTC (Polymeric Positive Temperature Coefficient)?", "id": "Koefisien Suhu Positif Polimerik." },
  { "en": "Bagaimana Polyswitch (Polymeric PTC) Bekerja?", "id": "Resistansi Naik Drastis Saat Arus Lebih." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor)?", "id": "Mengubah Suhu Jadi Sinyal Listrik." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Dua Logam Berbeda Hasilkan Tegangan Suhu." },
  { "en": "Apa Itu Kompensasi Sambungan Dingin (CJC)?", "id": "Koreksi Pengukuran Termokopel Suhu Referensi." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Resistansi Logam (Platina) Berubah Suhu." },
  { "en": "Apa Keunggulan RTD (Resistance Temperature Detector)?", "id": "Akurat, Stabil, Linearitas Baik." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Semikonduktor Sangat Sensitif Suhu." },
  { "en": "Apa Keunggulan Termistor (Thermistor)?", "id": "Sensitivitas Tinggi, Respons Cepat." },
  { "en": "Apa Kerugian Termistor (Thermistor)?", "id": "Non-Linearitas Tinggi." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor) IC?", "id": "Sirkuit Terpadu Output Proporsional Suhu." },
  { "en": "Contoh Sensor Suhu (Temperature Sensor) IC?", "id": "LM35 (Output Tegangan), DS18B20 (Digital)." },
  { "en": "Apa Itu Sensor Inframerah (Infrared) Termometer?", "id": "Mengukur Suhu Radiasi Inframerah Objek." },
  { "en": "Apa Keuntungan Termometer Inframerah (Infrared)?", "id": "Pengukuran Non-Kontak." },
  { "en": "Apa Itu Sensor Kelembaban (Humidity Sensor)?", "id": "Mengukur Kadar Uap Air Udara." },
  { "en": "Jenis Sensor Kelembaban (Humidity Sensor) Umum?", "id": "Kapasitif Dan Resistif." },
  { "en": "Apa Itu Kelembaban Relatif (Relative Humidity) (RH)?", "id": "Persentase Kejenuhan Uap Air Udara." },
  { "en": "Apa Itu Titik Embun (Dew Point)?", "id": "Suhu Udara Mulai Mengembun." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Sensor)?", "id": "Mengukur Tekanan Fluida (Gas/Cair)." },
  { "en": "Jenis Sensor Tekanan (Pressure Sensor) Umum?", "id": "Piezoresistif, Kapasitif, Piezoelektrik." },
  { "en": "Apa Itu Sensor Tekanan Absolut (Absolute Pressure)?", "id": "Mengukur Tekanan Relatif Vakum Sempurna." },
  { "en": "Apa Itu Sensor Tekanan Gauge (Gauge Pressure)?", "id": "Mengukur Tekanan Relatif Tekanan Atmosfer." },
  { "en": "Apa Itu Sensor Tekanan Diferensial (Differential Pressure)?", "id": "Mengukur Perbedaan Tekanan Dua Titik." },
  { "en": "Apa Itu Strain Gauge (Pengukur Regangan)?", "id": "Sensor Resistansi Berubah Akibat Regangan." },
  { "en": "Apa Itu Faktor Gauge (Gauge Factor) Strain Gauge?", "id": "Ukuran Sensitivitas Perubahan Resistansi." },
  { "en": "Apa Itu Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Rangkaian Ukur Perubahan Resistansi Kecil." },
  { "en": "Mengapa Strain Gauge (Pengukur Regangan) Pakai Jembatan?", "id": "Sensitivitas Tinggi Deteksi Perubahan Kecil." },
  { "en": "Apa Itu Load Cell (Sel Beban)?", "id": "Sensor Mengukur Gaya Atau Berat." },
  { "en": "Apa Komponen Sensor Utama Load Cell?", "id": "Strain Gauge (Pengukur Regangan)." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Mengukur Percepatan (Getaran, Kemiringan)." },
  { "en": "Jenis Akselerometer (Accelerometer) Umum?", "id": "Kapasitif MEMS, Piezoelektrik." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Sensor Mengukur Kecepatan Sudut (Rotasi)." },
  { "en": "Jenis Giroskop (Gyroscope) Umum?", "id": "MEMS (Micro-Electro-Mechanical System), Serat Optik (FOG)." },
  { "en": "Apa Itu Unit Pengukuran Inersia (IMU)?", "id": "Gabungan Akselerometer, Giroskop (Kadang Magnetometer)." },
  { "en": "Apa Itu Magnetometer (Magnetometer)?", "id": "Sensor Mengukur Medan Magnet Bumi (Kompas)." },
  { "en": "Apa Itu Sensor Jarak Ultrasonik (Ultrasonic)?", "id": "Mengukur Jarak Pantulan Gelombang Suara." },
  { "en": "Apa Itu Sensor Jarak Inframerah (Infrared)?", "id": "Mengukur Jarak Pantulan Cahaya Inframerah." },
  { "en": "Apa Itu Sensor Jarak Laser (Laser Distance)?", "id": "Mengukur Jarak Waktu Tempuh Cahaya Laser." },
  { "en": "Apa Itu Sensor Proximity Induktif (Inductive)?", "id": "Mendeteksi Keberadaan Objek Logam Dekat." },
  { "en": "Apa Itu Sensor Proximity Kapasitif (Capacitive)?", "id": "Mendeteksi Keberadaan Objek Logam/Non-Logam Dekat." },
  { "en": "Apa Itu Sensor Fotoelektrik (Photoelectric)?", "id": "Mendeteksi Objek Pemutusan/Pantulan Cahaya." },
  { "en": "Jenis Sensor Fotoelektrik (Photoelectric)?", "id": "Through-Beam, Retro-Reflective, Diffuse." },
  { "en": "Apa Itu Encoder (Encoder) Putar?", "id": "Sensor Mengukur Posisi Sudut Putaran." },
  { "en": "Apa Itu Encoder (Encoder) Inkremental?", "id": "Menghasilkan Pulsa Jumlah Proporsional Putaran." },
  { "en": "Apa Itu Encoder (Encoder) Absolut?", "id": "Memberikan Kode Posisi Unik Setiap Sudut." },
  { "en": "Apa Itu Kode Gray (Gray Code) Encoder?", "id": "Encoder Output Kode Gray." },
  { "en": "Keuntungan Kode Gray (Gray Code) Encoder?", "id": "Mengurangi Error Transisi Posisi." },
  { "en": "Apa Itu Sensor Linear Variable Differential Transformer?", "id": "LVDT (Sensor Posisi Linear Presisi)." },
  { "en": "Apa Kepanjangan LVDT (Linear Variable Differential Transformer)?", "id": "Transformator Diferensial Variabel Linear." },
  { "en": "Prinsip Kerja LVDT (Linear Variable Differential Transformer)?", "id": "Perubahan Kopling Induktif Akibat Posisi Inti." },
  { "en": "Apa Keunggulan LVDT (Linear Variable Differential Transformer)?", "id": "Non-Kontak, Akurat, Tahan Lama." },
  { "en": "Apa Itu Potensiometer (Potentiometer) Linear?", "id": "Sensor Posisi Linear Berbasis Resistansi Variabel." },
  { "en": "Apa Itu Sensor Kapasitif (Capacitive Sensor) Jarak?", "id": "Mengukur Jarak Perubahan Kapasitansi." },
  { "en": "Apa Itu Sensor Induktif (Inductive Sensor) Jarak?", "id": "Mengukur Jarak Perubahan Induktansi (Logam)." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect) Arus?", "id": "Mengukur Arus Medan Magnet Dihasilkan." },
  { "en": "Keuntungan Sensor Arus (Current Sensor) Efek Hall?", "id": "Isolasi Galvanik, Ukur AC/DC." },
  { "en": "Apa Itu Resistor Shunt (Shunt Resistor) Arus?", "id": "Resistor Presisi Rendah Ukur Jatuh Tegangan." },
  { "en": "Apa Itu Transformator Arus (Current Transformer) (CT)?", "id": "Mengukur Arus AC Tinggi Secara Induktif." },
  { "en": "Apa Itu Sensor Rogowski Coil (Rogowski Coil)?", "id": "Sensor Arus AC Fleksibel Non-Intrusif." },
  { "en": "Apa Prinsip Kerja Rogowski Coil (Rogowski Coil)?", "id": "Integrasi Tegangan Induksi Koil Udara." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor) Termokopel?", "id": "Tegangan Proporsional Perbedaan Suhu." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor) RTD?", "id": "Resistansi Logam Berubah Linear Suhu." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor) Termistor?", "id": "Resistansi Semikonduktor Berubah Eksponensial Suhu." },
  { "en": "Apa Itu Pirometer (Pyrometer) Optik?", "id": "Mengukur Suhu Tinggi Radiasi Cahaya Tampak." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor) Massa Termal?", "id": "Mengukur Aliran Massa Efek Pendinginan." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor) Turbin?", "id": "Mengukur Aliran Kecepatan Putaran Turbin." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor) Elektromagnetik?", "id": "Mengukur Aliran Konduktif Efek Faraday." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor) Ultrasonik?", "id": "Mengukur Aliran Waktu Tempuh Gelombang Suara." },
  { "en": "Apa Itu Sensor Level (Level Sensor) Konduktif?", "id": "Mendeteksi Level Cairan Konduktif Kontak Elektroda." },
  { "en": "Apa Itu Sensor Level (Level Sensor) Kapasitif?", "id": "Mendeteksi Level Perubahan Kapasitansi." },
  { "en": "Apa Itu Sensor Level (Level Sensor) Ultrasonik?", "id": "Mengukur Level Pantulan Gelombang Suara." },
  { "en": "Apa Itu Sensor Level (Level Sensor) Radar?", "id": "Mengukur Level Pantulan Gelombang Mikro." },
  { "en": "Apa Itu Sensor Level (Level Sensor) Pelampung?", "id": "Mendeteksi Level Posisi Pelampung Mekanis." },
  { "en": "Apa Itu Sensor Getaran (Vibration Sensor) Piezoelektrik?", "id": "Akselerometer Hasilkan Muatan Proporsional Getaran." },
  { "en": "Apa Itu Sensor Kecepatan (Velocity Sensor) Getaran?", "id": "Mengukur Kecepatan Getaran (Integrasi Akselerasi)." },
  { "en": "Apa Itu Sensor Perpindahan (Displacement Sensor) Getaran?", "id": "Mengukur Simpangan Getaran (Probe Eddy Current)." },
  { "en": "Apa Itu Sensor pH (pH Sensor)?", "id": "Mengukur Keasaman/Kebasaan Larutan (Potensial Elektroda)." },
  { "en": "Apa Itu Elektroda Referensi (Reference Electrode)?", "id": "Elektroda Potensial Stabil Pengukuran Elektrokimia." },
  { "en": "Apa Itu Elektroda Kaca (Glass Electrode) pH?", "id": "Elektroda Sensitif Terhadap Konsentrasi Ion H+." },
  { "en": "Apa Itu Sensor Konduktivitas (Conductivity Sensor)?", "id": "Mengukur Kemampuan Larutan Hantarkan Listrik." },
  { "en": "Apa Satuan Konduktivitas (Conductivity) Larutan?", "id": "Siemens Per Meter (S/m)." },
  { "en": "Apa Itu Sensor Oksigen Terlarut (Dissolved Oxygen)?", "id": "Mengukur Kadar Oksigen Terlarut Air." },
  { "en": "Apa Itu Sensor ORP (Oxidation Reduction Potential)?", "id": "Mengukur Potensi Oksidasi-Reduksi Larutan." },
  { "en": "Apa Itu Sensor Turbiditas (Turbidity Sensor)?", "id": "Mengukur Kekeruhan Cairan Hamburan Cahaya." },
  { "en": "Apa Itu Aktuator (Actuator) Elektrik?", "id": "Penggerak Menggunakan Energi Listrik." },
  { "en": "Apa Itu Solenoida (Solenoid) Linear?", "id": "Aktuator Elektromagnetik Gerak Maju Mundur." },
  { "en": "Apa Itu Solenoida (Solenoid) Rotary?", "id": "Aktuator Elektromagnetik Gerak Putar Terbatas." },
  { "en": "Apa Itu Voice Coil Motor (VCM)?", "id": "Motor Linear Presisi Tinggi Prinsip Lorentz." },
  { "en": "Apa Itu Motor DC (Direct Current) Ber-sikat?", "id": "Motor DC (Direct Current) Komutasi Mekanis Sikat." },
  { "en": "Apa Itu Motor DC (Direct Current) Tanpa Sikat (BLDC)?", "id": "Motor DC (Direct Current) Komutasi Elektronik." },
  { "en": "Apa Keuntungan Motor BLDC (Brushless DC Motor)?", "id": "Efisien Tinggi, Awet, Tanpa Percikan." },
  { "en": "Apa Itu Komutasi (Commutation) Elektronik?", "id": "Switching Arus Fasa Berbasis Posisi Rotor." },
  { "en": "Sensor Apa Digunakan Komutasi BLDC?", "id": "Sensor Efek Hall (Hall Effect)." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Sudut Diskrit." },
  { "en": "Apa Itu Kontrol Loop Terbuka (Open Loop) Stepper?", "id": "Kontrol Tanpa Umpan Balik Posisi Rotor." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Loop Tertutup Posisi/Kecepatan." },
  { "en": "Komponen Umpan Balik (Feedback) Servo?", "id": "Encoder Atau Potensiometer." },
  { "en": "Apa Itu Aktuator Piezoelektrik (Piezo Actuator)?", "id": "Penggerak Presisi Berbasis Efek Piezo Terbalik." },
  { "en": "Apa Skala Gerakan Aktuator Piezo?", "id": "Sangat Kecil (Mikrometer Hingga Nanometer)." },
  { "en": "Apa Itu Relay (Relay) Elektromekanis?", "id": "Saklar Dikontrol Elektromagnet." },
  { "en": "Apa Itu Kontaktor (Contactor)?", "id": "Relay (Relay) Daya Tinggi Arus Besar." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Saklar Elektronik Tanpa Kontak Mekanis." },
  { "en": "Apa Itu Katup Solenoida (Solenoid Valve)?", "id": "Katup Elektromekanis Kontrol Aliran Fluida." },
  { "en": "Apa Itu Katup Proporsional (Proportional Valve)?", "id": "Katup Aliran Dapat Diatur Kontinu." },
  { "en": "Apa Itu Pemanas Resistif (Resistive Heater)?", "id": "Elemen Hasilkan Panas Arus Listrik (Joule)." },
  { "en": "Apa Itu Pendingin Termoelektrik (TEC)?", "id": "Modul Peltier Memindahkan Panas Listrik." },
  { "en": "Apa Itu Sistem Pengereman (Braking) Motor?", "id": "Metode Menghentikan Putaran Motor." },
  { "en": "Apa Itu Pengereman Regeneratif (Regenerative Braking)?", "id": "Motor Jadi Generator Kembalikan Energi." },
  { "en": "Apa Itu Pengereman Dinamis (Dynamic Braking)?", "id": "Energi Kinetik Dibuang Jadi Panas Resistor." },
  { "en": "Apa Itu Pengereman Plugging (Plugging Braking)?", "id": "Membalik Polaritas/Fasa Hasilkan Torsi Balik." },
  { "en": "Apa Itu Pengereman Injeksi DC (DC Injection)?", "id": "Memberi Arus DC Ke Stator Motor AC." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Pengontrol Kecepatan Motor Induksi AC." },
  { "en": "Bagaimana VFD (Variable Frequency Drive) Mengontrol Kecepatan?", "id": "Mengubah Frekuensi Tegangan Output." },
  { "en": "Apa Itu Kontrol Skalar (Scalar Control) VFD?", "id": "Menjaga Rasio Tegangan/Frekuensi (V/f) Konstan." },
  { "en": "Apa Itu Kontrol Vektor (Vector Control) VFD?", "id": "Kontrol Kinerja Tinggi (Orientasi Medan)." },
  { "en": "Apa Itu Soft Starter (Soft Starter)?", "id": "Mengurangi Arus Lonjakan Start Motor AC." },
  { "en": "Bagaimana Soft Starter (Soft Starter) Bekerja?", "id": "Menaikkan Tegangan Motor Secara Bertahap." },
  { "en": "Apa Beda VFD (Variable Frequency Drive) Dan Soft Starter?", "id": "VFD (Kontrol Kecepatan), Soft Starter (Start Saja)." },
  { "en": "Apa Itu Kontrol Numerik Komputer (CNC)?", "id": "Computer Numerical Control (CNC)." },
  { "en": "Apa Kepanjangan CNC (Computer Numerical Control)?", "id": "Kontrol Numerik Komputer." },
  { "en": "Apa Fungsi Mesin CNC (Computer Numerical Control)?", "id": "Otomatisasi Gerakan Presisi Mesin Perkakas." },
  { "en": "Apa Itu Kode-G (G-Code)?", "id": "Bahasa Pemrograman Standar Mesin CNC." },
  { "en": "Apa Itu Robot (Robot) Industri?", "id": "Manipulator Otomatis Dapat Diprogram Ulang." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Sumbu Gerak Independen." },
  { "en": "Apa Itu End Effector (End Effector) Robot?", "id": "Alat Kerja Ujung Lengan Robot." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Geometri Gerakan Robot." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Memperhitungkan Gaya." },
  { "en": "Apa Itu Lintasan (Trajectory) Robot?", "id": "Jalur Gerak Yang Direncanakan Robot." },
  { "en": "Apa Itu Pengajaran (Teaching) Robot?", "id": "Memprogram Gerakan Robot Secara Manual." },
  { "en": "Apa Itu Pemrograman Offline (Offline Programming) Robot?", "id": "Memprogram Robot Menggunakan Simulasi Komputer." },
  { "en": "Apa Itu Robot Kolaboratif (Cobot)?", "id": "Robot Aman Bekerja Dekat Manusia." },
  { "en": "Apa Itu Sistem Visi (Vision System) Robot?", "id": "Kamera Dan Pemrosesan Citra Panduan Robot." },
  { "en": "Apa Itu Sensor Gaya/Torsi (Force/Torque Sensor) Robot?", "id": "Mengukur Interaksi Gaya Robot Lingkungan." },
  { "en": "Apa Itu Robot Bergerak (Mobile Robot)?", "id": "Robot Dapat Berpindah Tempat." },
  { "en": "Apa Itu Kendaraan Pemandu Otomatis (AGV)?", "id": "Automated Guided Vehicle (AGV)." },
  { "en": "Apa Kepanjangan AGV (Automated Guided Vehicle)?", "id": "Kendaraan Pemandu Otomatis." },
  { "en": "Bagaimana Navigasi AGV (Automated Guided Vehicle)?", "id": "Garis Magnetik, Laser, Visi." },
  { "en": "Apa Itu Robot Bergerak Otonom (AMR)?", "id": "Autonomous Mobile Robot (AMR)." },
  { "en": "Apa Kepanjangan AMR (Autonomous Mobile Robot)?", "id": "Robot Bergerak Otonom." },
  { "en": "Apa Beda AGV (Automated Guided Vehicle) Dan AMR (Autonomous Mobile Robot)?", "id": "AMR (Autonomous Mobile Robot) Lebih Fleksibel Navigasinya." },
  { "en": "Apa Itu SLAM (Simultaneous Localization And Mapping)?", "id": "Robot Membangun Peta Sambil Melokalisasi Diri." },
  { "en": "Apa Itu Drone (Drone) / UAV?", "id": "Unmanned Aerial Vehicle (UAV)." },
  { "en": "Apa Kepanjangan UAV (Unmanned Aerial Vehicle)?", "id": "Wahana Udara Tanpa Awak." },
  { "en": "Apa Itu Quadcopter (Quadcopter)?", "id": "Drone (Drone) Dengan Empat Rotor." },
  { "en": "Apa Itu Sistem Kontrol Penerbangan (Flight Controller)?", "id": "Otak Elektronik Pengendali Drone." },
  { "en": "Sensor Apa Digunakan Kontrol Drone?", "id": "IMU (Inertial Measurement Unit), GPS (Global Positioning System), Barometer." },
  { "en": "Apa Itu Gimbal (Gimbal) Kamera Drone?", "id": "Mekanisme Stabilisasi Kamera Drone." },
  { "en": "Apa Itu Return-to-Home (RTH) Drone?", "id": "Fitur Kembali Otomatis Titik Lepas Landas." },
  { "en": "Apa Itu Geofencing (Geofencing) Drone?", "id": "Pembatasan Area Terbang Virtual Drone." },
  { "en": "Apa Itu Medan Dekat (Near Field) Antena?", "id": "Daerah Reaktif Dekat Antena." },
  { "en": "Apa Itu Medan Jauh (Far Field) Antena?", "id": "Daerah Radiasi Jauh Dari Antena." },
  { "en": "Batas Medan Dekat Dan Jauh Disebut Apa?", "id": "Batas Fraunhofer." },
  { "en": "Apa Itu EIRP (Effective Isotropic Radiated Power)?", "id": "Daya Pancar Efektif Relatif Isotropik." },
  { "en": "Apa Itu ERP (Effective Radiated Power)?", "id": "Daya Pancar Efektif Relatif Dipol." },
  { "en": "Mana Lebih Besar, EIRP Atau ERP?", "id": "EIRP = ERP + 2.15 DB." },
  { "en": "Apa Itu Redaman Ruang Bebas (Free Space Loss)?", "id": "Pelemahan Sinyal Akibat Jarak Propagasi." },
  { "en": "Bagaimana Redaman Ruang Bebas Berubah Jarak?", "id": "Proporsional Kuadrat Jarak." },
  { "en": "Apa Itu Fading (Redaman) Multipath?", "id": "Fluktuasi Sinyal Akibat Pantulan Ganda." },
  { "en": "Apa Itu Koherensi Bandwidth (Coherence Bandwidth)?", "id": "Rentang Frekuensi Mengalami Fading Mirip." },
  { "en": "Apa Itu Waktu Koherensi (Coherence Time)?", "id": "Durasi Kanal Dianggap Konstan." },
  { "en": "Apa Itu Fading Selektif Frekuensi (Frequency Selective)?", "id": "Fading Berbeda Pada Frekuensi Berbeda." },
  { "en": "Kapan Fading Selektif Frekuensi Terjadi?", "id": "Bandwidth Sinyal Lebih Besar Koherensi." },
  { "en": "Apa Itu Fading Datar (Flat Fading)?", "id": "Fading Sama Semua Frekuensi Sinyal." },
  { "en": "Kapan Fading Datar (Flat Fading) Terjadi?", "id": "Bandwidth Sinyal Lebih Kecil Koherensi." },
  { "en": "Apa Itu Keanekaragaman (Diversity) Penerimaan?", "id": "Teknik Mengatasi Fading Penerima." },
  { "en": "Apa Itu Keanekaragaman Spasial (Spatial Diversity)?", "id": "Menggunakan Beberapa Antena Terpisah." },
  { "en": "Apa Itu Keanekaragaman Frekuensi (Frequency Diversity)?", "id": "Mengirim Sinyal Frekuensi Berbeda." },
  { "en": "Apa Itu Keanekaragaman Waktu (Time Diversity)?", "id": "Mengirim Ulang Sinyal Waktu Berbeda." },
  { "en": "Apa Itu Keanekaragaman Polarisasi (Polarization Diversity)?", "id": "Menggunakan Antena Polarisasi Ortogonal." },
  { "en": "Apa Itu Combining (Penggabungan) Keanekaragaman?", "id": "Menggabungkan Sinyal Dari Cabang Diversity." },
  { "en": "Jenis Combining (Penggabungan) Keanekaragaman?", "id": "Selection, Equal Gain, Maximal Ratio." },
  { "en": "Apa Itu Selection Combining (SC)?", "id": "Memilih Cabang Dengan Sinyal Terkuat." },
  { "en": "Apa Itu Equal Gain Combining (EGC)?", "id": "Menjumlahkan Sinyal Semua Cabang (Fasa Sama)." },
  { "en": "Apa Itu Maximal Ratio Combining (MRC)?", "id": "Menjumlahkan Sinyal Bobot Proporsional SNR." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Multi Antena Pemancar Dan Penerima." },
  { "en": "Manfaat MIMO (Multiple Input Multiple Output)?", "id": "Keanekaragaman, Multiplexing Spasial, Beamforming." },
  { "en": "Apa Itu Multiplexing Spasial (Spatial Multiplexing)?", "id": "Mengirim Stream Data Berbeda Bersamaan." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengarahkan Energi Pancaran Antena Array." },
  { "en": "Apa Itu Estimasi Kanal (Channel Estimation)?", "id": "Memperkirakan Karakteristik Kanal Transmisi." },
  { "en": "Mengapa Perlu Estimasi Kanal (Channel Estimation)?", "id": "Untuk Ekualisasi Dan Deteksi Koheren." },
  { "en": "Apa Itu Sinyal Pilot (Pilot Signal)?", "id": "Sinyal Dikenal Untuk Estimasi Kanal." },
  { "en": "Apa Itu Ekualiser (Equalizer) Kanal?", "id": "Filter Mengkompensasi Distorsi Kanal." },
  { "en": "Apa Itu Zero Forcing (ZF) Equalizer?", "id": "Equalizer Meminimalkan ISI Secara Langsung." },
  { "en": "Apa Itu Minimum Mean Square Error (MMSE) Equalizer?", "id": "Equalizer Minimalkan Error Kuadrat Rata-Rata." },
  { "en": "Apa Kepanjangan MMSE (Minimum Mean Square Error)?", "id": "Error Kuadrat Rata-Rata Minimum." },
  { "en": "Apa Itu Decision Feedback Equalizer (DFE)?", "id": "Equalizer Gunakan Keputusan Sebelumnya Kurangi ISI." },
  { "en": "Apa Kepanjangan DFE (Decision Feedback Equalizer)?", "id": "Equalizer Umpan Balik Keputusan." },
  { "en": "Apa Itu Orthogonal Frequency Division Multiplexing (OFDM)?", "id": "Teknik Modulasi Multi Pembawa Orthogonal." },
  { "en": "Mengapa Subcarrier (Subpembawa) OFDM Orthogonal?", "id": "Tidak Saling Mengganggu Satu Sama Lain." },
  { "en": "Apa Keuntungan OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Tahan Terhadap Fading Selektif Frekuensi." },
  { "en": "Apa Itu Cyclic Prefix (CP) OFDM?", "id": "Salinan Akhir Simbol Ditambah Ke Awal." },
  { "en": "Apa Fungsi Cyclic Prefix (CP)?", "id": "Menghilangkan ISI Dan ICI." },
  { "en": "Apa Itu Inter-Carrier Interference (ICI)?", "id": "Gangguan Antar Subpembawa Akibat Non-Ortogonalitas." },
  { "en": "Apa Kekurangan OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Rentan Terhadap Pergeseran Frekuensi, PAPR Tinggi." },
  { "en": "Apa Itu Peak-to-Average Power Ratio (PAPR)?", "id": "Rasio Daya Puncak Terhadap Rata-Rata." },
  { "en": "Mengapa PAPR (Peak-to-Average Power Ratio) Tinggi Masalah?", "id": "Membutuhkan Penguat Daya Linear Rentang Lebar." },
  { "en": "Teknik Reduksi PAPR (Peak-to-Average Power Ratio)?", "id": "Clipping, Coding, Tone Reservation." },
  { "en": "Apa Itu Single Carrier FDMA (SC-FDMA)?", "id": "Teknik Mirip OFDM (Orthogonal Frequency Division Multiplexing) PAPR Lebih Rendah." },
  { "en": "Dimana SC-FDMA (Single Carrier FDMA) Digunakan?", "id": "Uplink Pada Jaringan LTE." },
  { "en": "Apa Itu Multiple Access (Akses Ganda)?", "id": "Teknik Berbagi Kanal Komunikasi Bersama." },
  { "en": "Apa Itu FDMA (Frequency Division Multiple Access)?", "id": "Berbagi Berdasarkan Alokasi Frekuensi." },
  { "en": "Apa Itu TDMA (Time Division Multiple Access)?", "id": "Berbagi Berdasarkan Alokasi Slot Waktu." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Berbagi Berdasarkan Kode Unik Pengguna." },
  { "en": "Apa Itu Kode Penyebar (Spreading Code) CDMA?", "id": "Kode Pseudo-Acak Membedakan Pengguna." },
  { "en": "Apa Itu Orthogonalitas (Orthogonality) Kode CDMA?", "id": "Kode Tidak Saling Mengganggu Idealnya." },
  { "en": "Apa Itu Near-Far Problem (Masalah Dekat-Jauh) CDMA?", "id": "Sinyal Dekat Kuat Mengganggu Jauh Lemah." },
  { "en": "Bagaimana Mengatasi Near-Far Problem (Masalah Dekat-Jauh)?", "id": "Kontrol Daya (Power Control) Ketat." },
  { "en": "Apa Itu SDMA (Space Division Multiple Access)?", "id": "Berbagi Berdasarkan Pemisahan Spasial (Antena)." },
  { "en": "Apa Itu OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Akses Ganda Berbasis Subcarrier OFDM." },
  { "en": "Apa Itu Random Access (Akses Acak) Kanal?", "id": "Metode Akses Kanal Tanpa Jadwal Tetap." },
  { "en": "Contoh Protokol Random Access (Akses Acak)?", "id": "ALOHA, Slotted ALOHA, CSMA." },
  { "en": "Apa Itu ALOHA (ALOHA Protocol)?", "id": "Pengguna Kirim Kapan Saja (Bisa Tabrakan)." },
  { "en": "Apa Itu Slotted ALOHA (Slotted ALOHA)?", "id": "Kirim Hanya Awal Slot Waktu (Kurangi Tabrakan)." },
  { "en": "Apa Itu CSMA (Carrier Sense Multiple Access)?", "id": "Dengarkan Kanal Sebelum Mengirim." },
  { "en": "Apa Kepanjangan CSMA (Carrier Sense Multiple Access)?", "id": "Akses Ganda Deteksi Pembawa." },
  { "en": "Apa Itu CSMA/CD (Collision Detection)?", "id": "Deteksi Tabrakan Saat Mengirim (Ethernet)." },
  { "en": "Apa Itu CSMA/CA (Collision Avoidance)?", "id": "Penghindaran Tabrakan Sebelum Mengirim (Wi-Fi)." },
  { "en": "Apa Itu Hidden Node Problem (Masalah Node Tersembunyi)?", "id": "Dua Node Tak Dengar Satu Sama Lain." },
  { "en": "Apa Itu Exposed Node Problem (Masalah Node Terpapar)?", "id": "Node Menahan Diri Padahal Bisa Kirim." },
  { "en": "Solusi Hidden/Exposed Node (RTS/CTS)?", "id": "Request To Send / Clear To Send." },
  { "en": "Apa Kepanjangan RTS (Request To Send)?", "id": "Permintaan Untuk Mengirim." },
  { "en": "Apa Kepanjangan CTS (Clear To Send)?", "id": "Bersih Untuk Mengirim." },
  { "en": "Apa Itu Teori Antrian (Queueing Theory)?", "id": "Studi Matematis Sistem Antrian." },
  { "en": "Parameter Utama Teori Antrian (Queueing Theory)?", "id": "Laju Kedatangan, Laju Layanan, Jumlah Server." },
  { "en": "Apa Itu Model Antrian M/M/1?", "id": "Kedatangan Poisson, Layanan Eksponensial, Satu Server." },
  { "en": "Apa Itu Hukum Little (Little's Law)?", "id": "Hubungan Rata-Rata Jumlah Pelanggan, Waktu Tunggu." },
  { "en": "Apa Itu Kriptografi (Cryptography)?", "id": "Ilmu Teknik Pengamanan Informasi." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Mengubah Plaintext Menjadi Ciphertext." },
  { "en": "Apa Itu Dekripsi (Decryption)?", "id": "Mengubah Ciphertext Kembali Plaintext." },
  { "en": "Apa Itu Algoritma Kunci Simetris (Symmetric Key)?", "id": "Kunci Sama Enkripsi, Dekripsi." },
  { "en": "Contoh Algoritma Kunci Simetris?", "id": "DES (Data Encryption Standard), AES (Advanced Encryption Standard)." },
  { "en": "Apa Itu Algoritma Kunci Asimetris (Asymmetric Key)?", "id": "Kunci Berbeda Enkripsi (Publik), Dekripsi (Privat)." },
  { "en": "Contoh Algoritma Kunci Asimetris?", "id": "RSA (Rivest–Shamir–Adleman), ECC (Elliptic Curve Cryptography)." },
  { "en": "Apa Itu Fungsi Hash (Hash Function) Kriptografis?", "id": "Fungsi Satu Arah Hasilkan Hash Unik Tetap." },
  { "en": "Contoh Fungsi Hash (Hash Function) Kriptografis?", "id": "MD5 (Message Digest 5), SHA-256 (Secure Hash Algorithm 256)." },
  { "en": "Apakah MD5 (Message Digest 5) Masih Aman?", "id": "Tidak, Rentan Terhadap Kolisi (Collision)." },
  { "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?", "id": "Otentikasi, Integritas Pesan Kunci Asimetris." },
  { "en": "Apa Itu Sertifikat Kunci Publik (Public Key Certificate)?", "id": "Mengikat Kunci Publik Ke Identitas (X.509)." },
  { "en": "Apa Itu Infrastruktur Kunci Publik (PKI)?", "id": "Public Key Infrastructure (PKI)." },
  { "en": "Apa Kepanjangan PKI (Public Key Infrastructure)?", "id": "Infrastruktur Kunci Publik." },
  { "en": "Apa Fungsi PKI (Public Key Infrastructure)?", "id": "Manajemen Sertifikat Digital, Kepercayaan." },
  { "en": "Apa Itu Otoritas Sertifikat (CA)?", "id": "Certificate Authority (CA) (Penerbit Sertifikat)." },
  { "en": "Apa Itu SSL/TLS (Secure Sockets Layer/Transport Layer Security)?", "id": "Protokol Keamanan Komunikasi Internet." },
  { "en": "Apa Fungsi SSL/TLS (Secure Sockets Layer/Transport Layer Security)?", "id": "Enkripsi, Otentikasi Koneksi Web (HTTPS)." },
  { "en": "Apa Itu Serangan Kriptanalisis (Cryptanalysis)?", "id": "Upaya Memecahkan Sandi Tanpa Kunci." },
  { "en": "Apa Itu Serangan Brute Force (Brute Force Attack)?", "id": "Mencoba Semua Kemungkinan Kunci." },
  { "en": "Apa Itu Serangan Kamus (Dictionary Attack)?", "id": "Mencoba Kata Sandi Umum Dari Kamus." },
  { "en": "Apa Itu Serangan Rainbow Table (Rainbow Table Attack)?", "id": "Menggunakan Tabel Hash Pra-Hitung." },
  { "en": "Apa Itu Kerberos (Kerberos)?", "id": "Protokol Otentikasi Jaringan Terpercaya." },
  { "en": "Apa Itu Single Sign-On (SSO)?", "id": "Login Sekali Akses Banyak Aplikasi." },
  { "en": "Apa Kepanjangan SSO (Single Sign-On)?", "id": "Masuk Tunggal." },
  { "en": "Apa Itu OAuth (Open Authorization)?", "id": "Standar Otorisasi Terdelegasi." },
  { "en": "Apa Itu OpenID Connect (OpenID Connect)?", "id": "Lapisan Identitas Di Atas OAuth 2.0." },
  { "en": "Apa Itu SAML (Security Assertion Markup Language)?", "id": "Standar Pertukaran Data Otentikasi Otorisasi." },
  { "en": "Apa Kepanjangan SAML (Security Assertion Markup Language)?", "id": "Bahasa Markup Assertion Keamanan." },
  { "en": "Apa Itu Keamanan Jaringan (Network Security)?", "id": "Perlindungan Infrastruktur Jaringan Komputer." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Filter Lalu Lintas Jaringan Masuk Keluar." },
  { "en": "Jenis Firewall (Firewall)?", "id": "Packet Filtering, Stateful, Proxy, Next-Gen." },
  { "en": "Apa Itu Packet Filtering Firewall (Firewall Packet Filtering)?", "id": "Filter Berdasarkan Header Paket IP/Port." },
  { "en": "Apa Itu Stateful Firewall (Firewall Stateful)?", "id": "Melacak Status Koneksi Jaringan." },
  { "en": "Apa Itu Proxy Firewall (Firewall Proxy)?", "id": "Bertindak Perantara Antara Klien, Server." },
  { "en": "Apa Itu Next-Generation Firewall (NGFW)?", "id": "Firewall (Firewall) Fitur Lebih Canggih (Aplikasi)." },
  { "en": "Apa Kepanjangan NGFW (Next-Generation Firewall)?", "id": "Firewall (Firewall) Generasi Berikutnya." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Mendeteksi Aktivitas Mencurigakan Jaringan/Host." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Mendeteksi Dan Memblokir Serangan Otomatis." },
  { "en": "Apa Beda IDS (Intrusion Detection System), IPS (Intrusion Prevention System)?", "id": "IPS (Intrusion Prevention System) Dapat Memblokir Langsung." },
  { "en": "Apa Itu Virtual Private Network (VPN)?", "id": "Membuat Terowongan Aman Jaringan Publik." },
  { "en": "Protokol VPN (Virtual Private Network) Umum?", "id": "IPSec (Internet Protocol Security), SSL/TLS (OpenVPN)." },
  { "en": "Apa Itu Network Address Translation (NAT)?", "id": "Mengubah Alamat IP Privat Ke Publik." },
  { "en": "Apa Fungsi Keamanan NAT (Network Address Translation)?", "id": "Menyembunyikan Struktur Jaringan Internal." },
  { "en": "Apa Itu Demilitarized Zone (DMZ)?", "id": "Segmen Jaringan Perantara Server Publik." },
  { "en": "Apa Tujuan DMZ (Demilitarized Zone)?", "id": "Isolasi Server Publik Dari Jaringan Internal." },
  { "en": "Apa Itu Network Segmentation (Segmentasi Jaringan)?", "id": "Membagi Jaringan Jadi Sub-Jaringan Terisolasi." },
  { "en": "Manfaat Network Segmentation (Segmentasi Jaringan)?", "id": "Meningkatkan Keamanan, Kinerja Jaringan." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Aturan Izinkan/Tolak Lalu Lintas." },
  { "en": "Apa Kepanjangan ACL (Access Control List)?", "id": "Daftar Kontrol Akses." },
  { "en": "Dimana ACL (Access Control List) Diterapkan?", "id": "Router Dan Firewall." },
  { "en": "Apa Itu Network Access Control (NAC)?", "id": "Kontrol Akses Jaringan Berbasis Kebijakan." },
  { "en": "Apa Kepanjangan NAC (Network Access Control)?", "id": "Kontrol Akses Jaringan." },
  { "en": "Apa Itu Honeypot (Honeypot)?", "id": "Sistem Jebakan Deteksi Serangan." },
  { "en": "Apa Itu Wireless Security (Keamanan Nirkabel)?", "id": "Pengamanan Jaringan Wi-Fi." },
  { "en": "Protokol Keamanan Wi-Fi (Wi-Fi Security) Terkuat?", "id": "WPA3 (Wi-Fi Protected Access 3)." },
  { "en": "Apa Itu MAC Address Filtering (Penyaringan MAC)?", "id": "Membatasi Akses Berdasarkan Alamat MAC." },
  { "en": "Apakah MAC Filtering (Penyaringan MAC) Sangat Aman?", "id": "Tidak, Alamat MAC Bisa Dipalsukan." },
  { "en": "Apa Itu SSID Hiding (Menyembunyikan SSID)?", "id": "Menyembunyikan Nama Jaringan Wi-Fi." },
  { "en": "Apakah SSID Hiding (Menyembunyikan SSID) Efektif?", "id": "Keamanan Minimal (Bisa Dideteksi)." },
  { "en": "Apa Itu Serangan Evil Twin (Kembar Jahat)?", "id": "Membuat Access Point Palsu Nama Sama." },
  { "en": "Apa Itu Serangan Deauthentication (Deauthentication Attack)?", "id": "Memutuskan Paksa Klien Dari Jaringan Wi-Fi." },
  { "en": "Apa Itu Endpoint Security (Keamanan Titik Akhir)?", "id": "Pengamanan Perangkat Pengguna Akhir (PC, Laptop)." },
  { "en": "Komponen Endpoint Security (Keamanan Titik Akhir)?", "id": "Antivirus, Firewall Personal, HIPS." },
  { "en": "Apa Kepanjangan HIPS (Host-based Intrusion Prevention System)?", "id": "Sistem Pencegahan Intrusi Berbasis Host." },
  { "en": "Apa Itu Data Loss Prevention (DLP)?", "id": "Pencegahan Kebocoran Data Sensitif." },
  { "en": "Apa Kepanjangan DLP (Data Loss Prevention)?", "id": "Pencegahan Kehilangan Data." },
  { "en": "Apa Itu Mobile Device Management (MDM)?", "id": "Pengelolaan Perangkat Seluler Organisasi." },
  { "en": "Apa Kepanjangan MDM (Mobile Device Management)?", "id": "Manajemen Perangkat Seluler." },
  { "en": "Apa Itu Keamanan Cloud (Cloud Security)?", "id": "Pengamanan Data, Aplikasi Infrastruktur Awan." },
  { "en": "Apa Model Tanggung Jawab Bersama (Shared Responsibility)?", "id": "Pembagian Tanggung Jawab Keamanan (Awan)." },
  { "en": "Apa Itu Cloud Access Security Broker (CASB)?", "id": "Titik Kontrol Keamanan Antara Pengguna, Cloud." },
  { "en": "Apa Kepanjangan CASB (Cloud Access Security Broker)?", "id": "Broker Keamanan Akses Awan." },
  { "en": "Apa Itu Keamanan Aplikasi (Application Security)?", "id": "Pengamanan Perangkat Lunak Dari Kerentanan." },
  { "en": "Apa Itu Secure SDLC (Secure Software Development Lifecycle)?", "id": "Mengintegrasikan Keamanan Siklus Hidup Software." },
  { "en": "Apa Itu Static Application Security Testing (SAST)?", "id": "Analisis Kode Sumber Cari Kerentanan." },
  { "en": "Apa Kepanjangan SAST (Static Application Security Testing)?", "id": "Pengujian Keamanan Aplikasi Statis." },
  { "en": "Apa Itu Dynamic Application Security Testing (DAST)?", "id": "Pengujian Aplikasi Saat Berjalan." },
  { "en": "Apa Kepanjangan DAST (Dynamic Application Security Testing)?", "id": "Pengujian Keamanan Aplikasi Dinamis." },
  { "en": "Apa Itu Interactive Application Security Testing (IAST)?", "id": "Kombinasi SAST (Static Application Security Testing), DAST (Dynamic Application Security Testing)." },
  { "en": "Apa Kepanjangan IAST (Interactive Application Security Testing)?", "id": "Pengujian Keamanan Aplikasi Interaktif." },
  { "en": "Apa Itu Software Composition Analysis (SCA)?", "id": "Analisis Komponen Open Source Aplikasi." },
  { "en": "Apa Kepanjangan SCA (Software Composition Analysis)?", "id": "Analisis Komposisi Perangkat Lunak." },
  { "en": "Apa Itu OWASP (Open Web Application Security Project)?", "id": "Organisasi Nirlaba Keamanan Aplikasi Web." },
  { "en": "Apa Itu OWASP Top 10 (OWASP Top 10)?", "id": "Daftar Risiko Keamanan Aplikasi Web Teratas." },
  { "en": "Apa Itu Injeksi (Injection) (OWASP)?", "id": "Kerentanan Input Tidak Divalidasi." },
  { "en": "Apa Itu Broken Authentication (Otentikasi Rusak)?", "id": "Kerentanan Proses Login, Manajemen Sesi." },
  { "en": "Apa Itu Sensitive Data Exposure (Paparan Data Sensitif)?", "id": "Data Sensitif Tidak Terlindungi Baik." },
  { "en": "Apa Itu XML External Entities (XXE)?", "id": "Kerentanan Pemrosesan Input XML." },
  { "en": "Apa Kepanjangan XXE (XML External Entities)?", "id": "Entitas Eksternal XML." },
  { "en": "Apa Itu Broken Access Control (Kontrol Akses Rusak)?", "id": "Pengguna Bisa Akses Diluar Hak." },
  { "en": "Apa Itu Security Misconfiguration (Kesalahan Konfigurasi Keamanan)?", "id": "Konfigurasi Sistem Tidak Aman." },
  { "en": "Apa Itu Cross-Site Scripting (XSS)?", "id": "Injeksi Skrip Berbahaya Ke Situs Web." },
  { "en": "Apa Itu Insecure Deserialization (Deserialisasi Tidak Aman)?", "id": "Kerentanan Proses Deserialisasi Objek." },
  { "en": "Apa Itu Using Components with Known Vulnerabilities?", "id": "Menggunakan Komponen Berkerentanan Diketahui." },
  { "en": "Apa Itu Insufficient Logging & Monitoring?", "id": "Pencatatan, Pemantauan Keamanan Kurang." },
  { "en": "Apa Itu Threat Modeling (Pemodelan Ancaman)?", "id": "Identifikasi Potensi Ancaman, Kerentanan Aplikasi." },
  { "en": "Apa Itu Penetration Testing (Pengujian Penetrasi)?", "id": "Simulasi Serangan Temukan Kerentanan." },
  { "en": "Apa Itu Bug Bounty (Hadiah Bug)?", "id": "Program Hadiah Penemuan Kerentanan." },
  { "en": "Apa Itu Zero Trust Architecture (Arsitektur Zero Trust)?", "id": "Model Keamanan (Jangan Percaya, Verifikasi)." },
  { "en": "Apa Prinsip Dasar Zero Trust?", "id": "Least Privilege, Micro-segmentation, Verifikasi Ketat." },
  { "en": "Apa Itu Least Privilege (Hak Istimewa Minimum)?", "id": "Berikan Hak Akses Minimal Diperlukan." },
  { "en": "Apa Itu Micro-segmentation (Mikro-Segmentasi)?", "id": "Membagi Jaringan Segmen Sangat Kecil." },
  { "en": "Apa Itu Identity and Access Management (IAM)?", "id": "Manajemen Identitas Dan Akses Pengguna." },
  { "en": "Apa Kepanjangan IAM (Identity and Access Management)?", "id": "Manajemen Identitas Dan Akses." },
  { "en": "Apa Itu Multi-Factor Authentication (MFA)?", "id": "Otentikasi Menggunakan Lebih Satu Faktor." },
  { "en": "Apa Kepanjangan MFA (Multi-Factor Authentication)?", "id": "Otentikasi Multi-Faktor." },
  { "en": "Apa Itu Security Information and Event Management (SIEM)?", "id": "Pengumpulan, Analisis Log Keamanan Real-Time." },
  { "en": "Apa Itu Security Orchestration Automation Response (SOAR)?", "id": "Otomatisasi Respon Insiden Keamanan." },
  { "en": "Apa Kepanjangan SOAR (Security Orchestration Automation Response)?", "id": "Orkestrasi, Otomatisasi, Respon Keamanan." },
  { "en": "Apa Itu Forensik Digital (Digital Forensics)?", "id": "Investigasi Bukti Digital Kejahatan Siber." },
  { "en": "Apa Itu Rantai Bukti (Chain of Custody)?", "id": "Dokumentasi Penanganan Bukti Digital." },
  { "en": "Apa Itu Analisis Malware (Malware Analysis)?", "id": "Mempelajari Perilaku, Fungsi Malware." },
  { "en": "Apa Itu Analisis Statis (Static Analysis) Malware?", "id": "Analisis Kode Malware Tanpa Eksekusi." },
  { "en": "Apa Itu Analisis Dinamis (Dynamic Analysis) Malware?", "id": "Eksekusi Malware Lingkungan Terkontrol (Sandbox)." },
  { "en": "Apa Itu Sandbox (Sandbox)?", "id": "Lingkungan Terisolasi Aman Eksekusi Kode." },
  { "en": "Apa Itu Reverse Engineering (Rekayasa Balik)?", "id": "Menganalisis Produk Pahami Cara Kerja." },
  { "en": "Apa Itu Intelijen Ancaman (Threat Intelligence)?", "id": "Informasi Tentang Ancaman Siber Terkini." },
  { "en": "Apa Itu Indikator Kompromi (IoC)?", "id": "Indicator of Compromise (IoC)." },
  { "en": "Apa Kepanjangan IoC (Indicator of Compromise)?", "id": "Indikator Kompromi." },
  { "en": "Apa Fungsi IoC (Indicator of Compromise)?", "id": "Bukti Forensik Serangan Siber (IP, Hash)." },
  { "en": "Apa Itu Kriptografi (Cryptography)?", "id": "Ilmu Pengamanan Komunikasi Informasi." },
  { "en": "Apa Itu Steganografi (Steganography)?", "id": "Menyembunyikan Pesan Dalam Media Lain." },
  { "en": "Apa Beda Kriptografi, Steganografi?", "id": "Kripto (Sembunyikan Isi), Stegano (Sembunyikan Keberadaan)." },
  { "en": "Apa Itu Kriptografi Kurva Eliptik (ECC)?", "id": "Elliptic Curve Cryptography (ECC)." },
  { "en": "Apa Kepanjangan ECC (Elliptic Curve Cryptography)?", "id": "Kriptografi Kurva Eliptik." },
  { "en": "Apa Keunggulan ECC (Elliptic Curve Cryptography)?", "id": "Ukuran Kunci Lebih Kecil Keamanan Sama." },
  { "en": "Apa Itu Kriptografi Kuantum-Resisten (Post-Quantum)?", "id": "Algoritma Kripto Tahan Serangan Kuantum." },
  { "en": "Apa Itu Homomorphic Encryption (Enkripsi Homomorfik)?", "id": "Enkripsi Izinkan Komputasi Data Terenkripsi." },
  { "en": "Apa Itu Zero-Knowledge Proof (Bukti Nol Pengetahuan)?", "id": "Buktikan Pernyataan Benar Tanpa Ungkap Info." },
  { "en": "Apa Itu Differential Non-Linearity (DNL)?", "id": "Penyimpangan Lebar Kode ADC Ideal." },
  { "en": "Nilai DNL (Differential Non-Linearity) Ideal ADC?", "id": "Nol LSB (Least Significant Bit)." },
  { "en": "DNL (Differential Non-Linearity) Negatif Besar Sebabkan Apa?", "id": "Kode Hilang (Missing Codes)." },
  { "en": "Apa Itu Integral Non-Linearity (INL)?", "id": "Penyimpangan Fungsi Transfer ADC Garis Lurus." },
  { "en": "Nilai INL (Integral Non-Linearity) Ideal ADC?", "id": "Nol LSB (Least Significant Bit)." },
  { "en": "Apa Itu Offset Error (Kesalahan Offset) ADC/DAC?", "id": "Pergeseran Fungsi Transfer Dari Titik Nol." },
  { "en": "Apa Itu Gain Error (Kesalahan Penguatan) ADC/DAC?", "id": "Penyimpangan Kemiringan Fungsi Transfer Ideal." },
  { "en": "Apa Itu Settling Time (Waktu Turun) ADC?", "id": "Waktu ADC Siap Konversi Berikutnya." },
  { "en": "Apa Itu Latency (Latensi) ADC?", "id": "Waktu Tunda Awal Input Hingga Output Valid." },
  { "en": "Apa Itu Throughput (Throughput) ADC?", "id": "Laju Konversi Maksimum ADC (Sampel/Detik)." },
  { "en": "Apa Itu Aperture Delay (Tunda Apertur) S&H?", "id": "Waktu Tunda Antara Sinyal Hold, Sampling." },
  { "en": "Apa Itu Aperture Uncertainty (Ketidakpastian Apertur)?", "id": "Variasi Aperture Delay (Jitter)." },
  { "en": "Apa Efek Aperture Uncertainty (Ketidakpastian Apertur)?", "id": "Membatasi SNR (Signal-to-Noise Ratio) Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Penguat Sampel Tahan (SHA)?", "id": "Sample and Hold Amplifier (SHA)." },
  { "en": "Apa Kepanjangan SHA (Sample and Hold Amplifier)?", "id": "Penguat Sampel Dan Tahan." },
  { "en": "Apa Fungsi SHA (Sample and Hold Amplifier) Depan ADC?", "id": "Menjaga Input Konstan Selama Konversi." },
  { "en": "Apa Itu Tegangan Referensi (Vref) ADC/DAC?", "id": "Tegangan Stabil Rujukan Konversi." },
  { "en": "Mengapa Vref (Tegangan Referensi) Stabil Penting?", "id": "Mempengaruhi Akurasi Konversi Langsung." },
  { "en": "Apa Itu Sumber Tegangan Referensi (Voltage Reference)?", "id": "IC Penghasil Tegangan Referensi Presisi." },
  { "en": "Jenis Sumber Tegangan Referensi (Voltage Reference)?", "id": "Bandgap Reference, Zener Reference." },
  { "en": "Apa Itu Bandgap Reference (Referensi Celah Pita)?", "id": "Referensi Tegangan Stabil Terhadap Suhu." },
  { "en": "Apa Itu Modulasi Delta (Delta Modulation) (DM)?", "id": "Teknik ADC Sederhana (Komparator, Integrator)." },
  { "en": "Apa Itu Slope Overload (Beban Lereng) DM?", "id": "Sinyal Input Berubah Terlalu Cepat." },
  { "en": "Apa Itu Granular Noise (Derau Granular) DM?", "id": "Noise Saat Sinyal Input Lambat Berubah." },
  { "en": "Apa Itu Modulasi Sigma-Delta (Sigma-Delta Modulation)?", "id": "Teknik Oversampling, Noise Shaping ADC/DAC." },
  { "en": "Apa Keunggulan Modulator Sigma-Delta (Sigma-Delta Modulator)?", "id": "Resolusi Sangat Tinggi, Linearitas Baik." },
  { "en": "Apa Itu Oversampling (Oversampling)?", "id": "Sampling Jauh Di Atas Laju Nyquist." },
  { "en": "Apa Itu Noise Shaping (Pembentukan Derau)?", "id": "Memindahkan Noise Kuantisasi Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Desimasi (Decimation Filter)?", "id": "Filter Low-Pass Kurangi Laju Data Sigma-Delta." },
  { "en": "Apa Itu Interpolasi (Interpolation) Filter DAC?", "id": "Menambah Laju Data Sebelum Modulasi Sigma-Delta." },
  { "en": "Apa Itu Interleaving (Interleaving) ADC?", "id": "Menggunakan Beberapa ADC Paralel Tingkatkan Sample Rate." },
  { "en": "Apa Itu Pipelined ADC (ADC Pipelined)?", "id": "ADC Bertahap (Stage) Kecepatan Tinggi." },
  { "en": "Apa Itu Jitter (Jitter) Clock ADC?", "id": "Variasi Waktu Tepi Sinyal Clock." },
  { "en": "Apa Efek Jitter (Jitter) Clock ADC?", "id": "Membatasi SNR (Signal-to-Noise Ratio) Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Dua Logam Berbeda." },
  { "en": "Apa Prinsip Kerja Termokopel?", "id": "Efek Seebeck (Tegangan Akibat Beda Suhu)." },
  { "en": "Apa Itu Sambungan Panas (Hot Junction) Termokopel?", "id": "Titik Pengukuran Suhu." },
  { "en": "Apa Itu Sambungan Dingin (Cold Junction) Termokopel?", "id": "Titik Referensi (Perlu Kompensasi)." },
  { "en": "Apa Itu Kompensasi Sambungan Dingin (CJC)?", "id": "Mengoreksi Pengaruh Suhu Sambungan Dingin." },
  { "en": "Tipe Termokopel (Thermocouple) Umum?", "id": "Tipe K, J, T, E." },
  { "en": "Material Apa Termokopel (Thermocouple) Tipe K?", "id": "Chromel Dan Alumel." },
  { "en": "Apa Keunggulan Termokopel (Thermocouple)?", "id": "Rentang Suhu Luas, Murah, Kuat." },
  { "en": "Apa Kerugian Termokopel (Thermocouple)?", "id": "Output Tegangan Kecil, Non-Linear, Perlu CJC." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Resistansi Logam." },
  { "en": "Material Apa Umum Dipakai RTD?", "id": "Platina (Platinum) (Pt100, Pt1000)." },
  { "en": "Apa Arti Pt100 (Pt100)?", "id": "RTD (Resistance Temperature Detector) Platina 100 Ohm Di 0°C." },
  { "en": "Apa Keunggulan RTD (Resistance Temperature Detector)?", "id": "Akurat, Stabil, Cukup Linear." },
  { "en": "Apa Kerugian RTD (Resistance Temperature Detector)?", "id": "Lebih Mahal, Respon Lambat, Self-Heating." },
  { "en": "Apa Itu Self-Heating (Pemanasan Diri) RTD?", "id": "Panas Dihasilkan Arus Pengukuran." },
  { "en": "Konfigurasi Kabel RTD (Resistance Temperature Detector) Umum?", "id": "2-Kawat, 3-Kawat, 4-Kawat." },
  { "en": "Mengapa Perlu Konfigurasi 3/4-Kawat RTD?", "id": "Kompensasi Resistansi Kabel Penghubung." },
  { "en": "Rangkaian Apa Digunakan Baca RTD?", "id": "Jembatan Wheatstone Atau Sumber Arus." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Semikonduktor Sangat Peka Suhu." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient) Thermistor?", "id": "Resistansi Turun Jika Suhu Naik." },
  { "en": "Apa Itu PTC (Positive Temperature Coefficient) Thermistor?", "id": "Resistansi Naik Jika Suhu Naik." },
  { "en": "Apa Keunggulan Termistor (Thermistor)?", "id": "Sensitivitas Sangat Tinggi, Respons Cepat." },
  { "en": "Apa Kerugian Termistor (Thermistor)?", "id": "Sangat Non-Linear, Rentang Suhu Terbatas." },
  { "en": "Apa Itu Persamaan Steinhart-Hart?", "id": "Persamaan Aproksimasi Hubungan R-T Termistor." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor) IC?", "id": "Sirkuit Terpadu Output Listrik Proporsional Suhu." },
  { "en": "Jenis Output Sensor Suhu IC?", "id": "Tegangan Analog (LM35), Arus, Digital (I2C/SPI)." },
  { "en": "Keunggulan Sensor Suhu (Temperature Sensor) IC?", "id": "Linear, Mudah Digunakan, Terkalibrasi." },
  { "en": "Apa Itu Pirometer Inframerah (Infrared Pyrometer)?", "id": "Mengukur Suhu Non-Kontak Radiasi Termal." },
  { "en": "Apa Itu Emisivitas (Emissivity)?", "id": "Parameter Penting Pengukuran Pirometer." },
  { "en": "Apa Itu Strain Gauge (Pengukur Regangan)?", "id": "Sensor Resistansi Berubah Akibat Regangan." },
  { "en": "Apa Prinsip Kerja Strain Gauge?", "id": "Perubahan Dimensi Ubah Resistansi Konduktor." },
  { "en": "Apa Itu Faktor Gauge (Gauge Factor)?", "id": "Ukuran Sensitivitas Strain Gauge." },
  { "en": "Rangkaian Apa Untuk Ukur Strain Gauge?", "id": "Jembatan Wheatstone (Mengukur Perubahan Kecil)." },
  { "en": "Apa Itu Konfigurasi Jembatan Seperempat (Quarter Bridge)?", "id": "Satu Strain Gauge Aktif." },
  { "en": "Apa Itu Konfigurasi Jembatan Setengah (Half Bridge)?", "id": "Dua Strain Gauge Aktif." },
  { "en": "Apa Itu Konfigurasi Jembatan Penuh (Full Bridge)?", "id": "Empat Strain Gauge Aktif." },
  { "en": "Konfigurasi Mana Paling Sensitif?", "id": "Konfigurasi Jembatan Penuh (Full Bridge)." },
  { "en": "Apa Itu Kompensasi Suhu (Temperature Compensation) Strain Gauge?", "id": "Menggunakan Gauge Dummy Hilangkan Efek Suhu." },
  { "en": "Apa Itu Load Cell (Sel Beban)?", "id": "Sensor Mengukur Gaya Atau Beban." },
  { "en": "Apa Komponen Sensor Utama Load Cell?", "id": "Strain Gauge Dipasang Struktur Mekanis." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Sensor)?", "id": "Mengukur Tekanan Fluida." },
  { "en": "Jenis Sensor Tekanan (Pressure Sensor) Umum?", "id": "Piezoresistif, Kapasitif, Strain Gauge." },
  { "en": "Apa Itu Sensor Tekanan Piezoresistif?", "id": "Resistansi Berubah Akibat Tekanan Diafragma." },
  { "en": "Apa Itu Sensor Tekanan Kapasitif?", "id": "Kapasitansi Berubah Akibat Defleksi Diafragma." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor)?", "id": "Mengukur Laju Aliran Fluida." },
  { "en": "Apa Itu Sensor Aliran Massa Termal?", "id": "Mengukur Aliran Efek Pendinginan Elemen Panas." },
  { "en": "Apa Itu Sensor Aliran Turbin?", "id": "Kecepatan Putaran Turbin Proporsional Aliran." },
  { "en": "Apa Itu Sensor Aliran Vortex (Vortex Flow Sensor)?", "id": "Frekuensi Vortex Proporsional Laju Aliran." },
  { "en": "Apa Itu Sensor Aliran Elektromagnetik?", "id": "Tegangan Induksi Proporsional Aliran Konduktif." },
  { "en": "Apa Itu Sensor Level (Level Sensor)?", "id": "Mendeteksi Atau Mengukur Ketinggian Cairan/Padatan." },
  { "en": "Jenis Sensor Level (Level Sensor) Titik?", "id": "Pelampung, Konduktif, Kapasitif (Deteksi Batas)." },
  { "en": "Jenis Sensor Level (Level Sensor) Kontinu?", "id": "Ultrasonik, Radar, Tekanan Diferensial." },
  { "en": "Apa Itu Sensor Posisi (Position Sensor)?", "id": "Mengukur Posisi Linear Atau Sudut." },
  { "en": "Apa Itu Potensiometer (Potentiometer) Sensor Posisi?", "id": "Resistor Variabel Output Tegangan Proporsional." },
  { "en": "Apa Itu Encoder (Encoder) Putar?", "id": "Sensor Posisi Sudut (Inkremental/Absolut)." },
  { "en": "Apa Itu LVDT (Linear Variable Differential Transformer)?", "id": "Sensor Posisi Linear Non-Kontak." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Mengukur Percepatan Linear." },
  { "en": "Prinsip Kerja Akselerometer MEMS?", "id": "Perubahan Kapasitansi Akibat Gerakan Massa Bukti." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Sensor Mengukur Kecepatan Sudut (Rotasi)." },
  { "en": "Prinsip Kerja Giroskop MEMS?", "id": "Efek Coriolis Pada Massa Bergetar." },
  { "en": "Apa Itu IMU (Inertial Measurement Unit)?", "id": "Sensor Gabungan Akselerometer, Giroskop." },
  { "en": "Apa Itu Magnetometer (Magnetometer)?", "id": "Sensor Mengukur Medan Magnet." },
  { "en": "Apa Aplikasi Magnetometer (Magnetometer)?", "id": "Kompas Digital, Deteksi Logam." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect)?", "id": "Mendeteksi Medan Magnet Hasilkan Tegangan." },
  { "en": "Apa Itu Sensor Optik (Optical Sensor)?", "id": "Sensor Menggunakan Cahaya Deteksi/Ukur." },
  { "en": "Contoh Sensor Optik (Optical Sensor)?", "id": "Fotodioda, Fototransistor, Sensor Fotoelektrik." },
  { "en": "Apa Itu Sensor Warna (Color Sensor)?", "id": "Mendeteksi Warna Cahaya." },
  { "en": "Apa Itu Sensor Gambar (Image Sensor)?", "id": "Mengubah Citra Optik Ke Sinyal Elektronik." },
  { "en": "Apa Itu Sensor Kimia (Chemical Sensor)?", "id": "Mendeteksi Keberadaan/Konsentrasi Zat Kimia." },
  { "en": "Apa Itu Sensor Gas (Gas Sensor)?", "id": "Mendeteksi Jenis Atau Konsentrasi Gas." },
  { "en": "Apa Itu Biosensor (Biosensor)?", "id": "Sensor Menggunakan Komponen Biologis." },
  { "en": "Contoh Biosensor (Biosensor)?", "id": "Sensor Glukosa Darah." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Penggerak Mengubah Energi Ke Gerakan." },
  { "en": "Apa Itu Aktuator Elektrik (Electric Actuator)?", "id": "Penggerak Menggunakan Energi Listrik." },
  { "en": "Contoh Aktuator Elektrik (Electric Actuator)?", "id": "Motor, Solenoida, Relay." },
  { "en": "Apa Itu Aktuator Pneumatik (Pneumatic Actuator)?", "id": "Penggerak Menggunakan Udara Bertekanan." },
  { "en": "Apa Itu Aktuator Hidrolik (Hydraulic Actuator)?", "id": "Penggerak Menggunakan Cairan Bertekanan." },
  { "en": "Apa Satuan Dasar Fluks Listrik (Electric Flux)?", "id": "Volt Meter (V·m)." },
  { "en": "Apa Satuan Dasar Kuat Medan Listrik (E)?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Satuan Dasar Potensial Listrik (Voltage)?", "id": "Volt (V)." },
  { "en": "Apa Satuan Dasar Energi Potensial Listrik?", "id": "Joule (J)." },
  { "en": "Apa Itu Permitivitas (Permittivity) Hampa (ε₀)?", "id": "Konstanta Listrik Ruang Hampa." },
  { "en": "Apa Itu Permeabilitas (Permeability) Hampa (μ₀)?", "id": "Konstanta Magnetik Ruang Hampa." },
  { "en": "Apa Hubungan Kecepatan Cahaya (c), ε₀, μ₀?", "id": "C = 1 / √(ε₀μ₀)." },
  { "en": "Apa Itu Kapasitansi (Capacitance)?", "id": "Kemampuan Simpan Muatan Listrik." },
  { "en": "Apa Itu Induktansi (Inductance)?", "id": "Kemampuan Simpan Energi Magnetik." },
  { "en": "Apa Rumus Energi Tersimpan Kapasitor?", "id": "U = ½ * C * V²." },
  { "en": "Apa Rumus Energi Tersimpan Induktor?", "id": "U = ½ * L * I²." },
  { "en": "Apa Itu Arus Konduksi (Conduction Current)?", "id": "Aliran Muatan Bebas Konduktor." },
  { "en": "Apa Itu Arus Perpindahan (Displacement Current)?", "id": "Arus Akibat Perubahan Medan Listrik." },
  { "en": "Siapa Mengusulkan Konsep Arus Perpindahan?", "id": "James Clerk Maxwell." },
  { "en": "Apa Itu Hukum Ampere-Maxwell (Ampere-Maxwell Law)?", "id": "Hukum Ampere Termasuk Arus Perpindahan." },
  { "en": "Apa Sumber Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Muatan Listrik Yang Dipercepat." },
  { "en": "Apakah Gelombang Elektromagnetik (Electromagnetic Wave) Butuh Medium?", "id": "Tidak, Dapat Merambat Vakum." },
  { "en": "Apa Arah Medan E, B, Propagasi?", "id": "Saling Tegak Lurus Satu Sama Lain." },
  { "en": "Apa Itu Polarisasi (Polarization) Linear?", "id": "Medan Listrik Bergetar Satu Garis." },
  { "en": "Apa Itu Polarisasi (Polarization) Sirkular?", "id": "Medan Listrik Berputar Membentuk Lingkaran." },
  { "en": "Apa Itu Vektor Poynting (Poynting Vector)?", "id": "Arah Dan Laju Aliran Energi EM." },
  { "en": "Apa Satuan Vektor Poynting (Poynting Vector)?", "id": "Watt Per Meter Persegi (W/m²)." },
  { "en": "Apa Itu Impedansi Intrinsik (Intrinsic Impedance) Medium?", "id": "Rasio Amplitudo Medan E Terhadap H." },
  { "en": "Nilai Impedansi Intrinsik (Intrinsic Impedance) Ruang Hampa?", "id": "Sekitar Tiga Ratus Tujuh Puluh Tujuh Ohm (≈377 Ω)." },
  { "en": "Apa Itu Atenuasi (Attenuation) Gelombang?", "id": "Pelemahan Amplitudo Gelombang Saat Merambat." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth)?", "id": "Ukuran Penetrasi Gelombang EM Konduktor." },
  { "en": "Bagaimana Kedalaman Kulit (Skin Depth) Berubah Frekuensi?", "id": "Berkurang Jika Frekuensi Meningkat." },
  { "en": "Apa Itu Refleksi (Reflection) Gelombang?", "id": "Pemantulan Gelombang Batas Dua Medium." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient)?", "id": "Rasio Amplitudo Gelombang Pantul, Datang." },
  { "en": "Apa Itu Refraksi (Refraction) Gelombang?", "id": "Pembelokan Gelombang Antar Dua Medium." },
  { "en": "Apa Hukum Snellius (Snell's Law)?", "id": "Hubungan Sudut Refraksi Indeks Bias." },
  { "en": "Apa Itu Indeks Bias (Refractive Index)?", "id": "Rasio Kecepatan Cahaya Vakum, Medium." },
  { "en": "Apa Itu Pemantulan Internal Total (Total Internal Reflection)?", "id": "Pemantulan Sempurna Dalam Medium Rapat." },
  { "en": "Apa Itu Difraksi (Diffraction)?", "id": "Pelenturan Gelombang Sekitar Hambatan." },
  { "en": "Apa Itu Interferensi (Interference)?", "id": "Superposisi Gelombang Hasilkan Pola." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Struktur Pemandu Perambatan Gelombang." },
  { "en": "Parameter Utama Saluran Transmisi (Transmission Line)?", "id": "R, L, G, C Per Satuan Panjang." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance) (Z₀)?", "id": "Impedansi Khas Saluran Transmisi Ideal." },
  { "en": "Apa Itu Konstanta Propagasi (Propagation Constant) (γ)?", "id": "Menentukan Atenuasi, Pergeseran Fasa." },
  { "en": "Apa Rumus Konstanta Propagasi (Propagation Constant) (γ)?", "id": "Gamma Sama Dengan Alfa Plus J Beta." },
  { "en": "Apa Itu Konstanta Atenuasi (Attenuation Constant) (α)?", "id": "Bagian Riil Gamma (γ)." },
  { "en": "Apa Itu Konstanta Fasa (Phase Constant) (β)?", "id": "Bagian Imajiner Gamma (γ)." },
  { "en": "Apa Itu Kecepatan Fasa (Phase Velocity) (vp)?", "id": "Vp Sama Dengan Omega Dibagi Beta." },
  { "en": "Apa Itu Saluran Tanpa Rugi (Lossless Line)?", "id": "R = 0 Dan G = 0." },
  { "en": "Apa Impedansi Karakteristik (Z₀) Saluran Tanpa Rugi?", "id": "Z₀ Sama Dengan Akar (L/C)." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient) Beban (Γ_L)?", "id": "Ukuran Pantulan Di Ujung Beban." },
  { "en": "Rumus Koefisien Refleksi (Reflection Coefficient) Beban (Γ_L)?", "id": "(Zl - Z₀) / (Zl + Z₀)." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Tegangan Maksimum, Minimum Gelombang Berdiri." },
  { "en": "Rumus VSWR (Voltage Standing Wave Ratio) Dari Refleksi?", "id": "VSWR = (1 + |Γ|) / (1 - |Γ|)." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Saluran?", "id": "Impedansi Dilihat Sumber Di Awal Saluran." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Membuat Impedansi Beban Sama Sumber/Saluran." },
  { "en": "Mengapa Perlu Pencocokan Impedansi (Impedance Matching)?", "id": "Transfer Daya Maksimal, Tanpa Pantulan." },
  { "en": "Apa Itu Transformator Lambda Perempat (Quarter-Wave Transformer)?", "id": "Saluran λ/4 Untuk Matching Impedansi Riil." },
  { "en": "Apa Itu Stub (Stub) Tunggal?", "id": "Saluran Pendek Paralel/Seri Matching Impedansi Kompleks." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Karakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu S11 (S11)?", "id": "Koefisien Refleksi Input." },
  { "en": "Apa Itu S21 (S21)?", "id": "Penguatan (Gain) Transmisi Maju." },
  { "en": "Apa Itu S12 (S12)?", "id": "Penguatan (Gain) Transmisi Mundur (Isolasi)." },
  { "en": "Apa Itu S22 (S22)?", "id": "Koefisien Refleksi Output." },
  { "en": "Apa Itu Jaringan Resiprokal (Reciprocal Network)?", "id": "S12 Sama Dengan S21." },
  { "en": "Apa Itu Jaringan Tanpa Rugi (Lossless Network)?", "id": "Total Daya Masuk Sama Dengan Keluar." },
  { "en": "Apa Itu Antena Dipol (Dipole Antenna)?", "id": "Antena Dasar Dua Konduktor Lurus." },
  { "en": "Panjang Resonansi (Resonance Length) Dipol Setengah Gelombang?", "id": "Kira-Kira Setengah Panjang Gelombang (λ/2)." },
  { "en": "Impedansi Input (Input Impedance) Dipol Setengah Gelombang?", "id": "Sekitar Tujuh Puluh Ohm (≈70 Ω)." },
  { "en": "Apa Pola Radiasi (Radiation Pattern) Dipol Setengah Gelombang?", "id": "Berbentuk Donat (Maksimum Tegak Lurus)." },
  { "en": "Apa Itu Antena Monopol (Monopole Antenna)?", "id": "Setengah Dipol Di Atas Bidang Tanah." },
  { "en": "Panjang Resonansi (Resonance Length) Monopol Seperempat Gelombang?", "id": "Kira-Kira Seperempat Panjang Gelombang (λ/4)." },
  { "en": "Impedansi Input (Input Impedance) Monopol Seperempat Gelombang?", "id": "Setengah Impedansi Dipol (≈36 Ω)." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Dibanding Isotropik." },
  { "en": "Apa Satuan Penguatan (Gain) Antena?", "id": "Desibel Isotropik (dBi)." },
  { "en": "Apa Itu Antena Isotropik (Isotropic Antenna)?", "id": "Antena Hipotetis Memancar Sama Rata." },
  { "en": "Apa Itu Lebar Berkas (Beamwidth) Antena?", "id": "Sudut Pancaran Utama Antena." },
  { "en": "Apa Itu Rasio Depan Belakang (Front-to-Back Ratio)?", "id": "Perbandingan Pancaran Arah Depan, Belakang." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Dipancarkan." },
  { "en": "Jenis Polarisasi (Polarization) Linear?", "id": "Vertikal Dan Horizontal." },
  { "en": "Jenis Polarisasi (Polarization) Sirkular?", "id": "Putar Kanan (RHCP), Putar Kiri (LHCP)." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Kombinasi Beberapa Elemen Antena." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda Antenna)?", "id": "Array Direktif (Driven, Reflektor, Direktor)." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Antena Mikrostrip (Microstrip Antenna)?", "id": "Antena Dicetak Di PCB (Patch)." },
  { "en": "Apa Itu Radar (Radar)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Prinsip Dasar Radar (Radar)?", "id": "Memancarkan Sinyal, Menerima Pantulan." },
  { "en": "Apa Itu Radar Pulsa (Pulse Radar)?", "id": "Mengukur Jarak Waktu Tempuh Pulsa." },
  { "en": "Apa Itu Radar Doppler (Doppler Radar)?", "id": "Mengukur Kecepatan Pergeseran Frekuensi Doppler." },
  { "en": "Apa Itu Radar FMCW (Frequency Modulated Continuous Wave)?", "id": "Mengukur Jarak Perbedaan Frekuensi TX-RX." },
  { "en": "Apa Itu Penampang Lintang Radar (RCS)?", "id": "Ukuran Kemampuan Objek Pantulkan Radar." },
  { "en": "Apa Itu Komunikasi Optik (Optical Communication)?", "id": "Komunikasi Menggunakan Cahaya." },
  { "en": "Apa Media Utama Komunikasi Optik?", "id": "Serat Optik (Optical Fiber)." },
  { "en": "Apa Keuntungan Serat Optik (Fiber Optic)?", "id": "Bandwidth Besar, Redaman Rendah, Kebal EMI." },
  { "en": "Apa Prinsip Propagasi Cahaya Serat?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Serat Mode Tunggal (Single Mode)?", "id": "Inti Kecil, Jarak Jauh." },
  { "en": "Apa Itu Serat Mode Jamak (Multi Mode)?", "id": "Inti Besar, Jarak Pendek." },
  { "en": "Apa Sumber Cahaya (Light Source) Komunikasi Optik?", "id": "LED (Light Emitting Diode) Atau Laser Diode." },
  { "en": "Apa Detektor (Detector) Komunikasi Optik?", "id": "Fotodioda (PIN Atau APD)." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Berdasarkan Panjang Gelombang." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung." },
  { "en": "Contoh Penguat Optik (Optical Amplifier)?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Sistem Komunikasi (Communication System)?", "id": "Transfer Informasi Sumber Ke Tujuan." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Ekstraksi Informasi Dari Sinyal Termodulasi." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Amplitudo Pembawa Diubah Sesuai Informasi." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Frekuensi Pembawa Diubah Sesuai Informasi." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Fasa Pembawa Diubah Sesuai Informasi." },
  { "en": "Apa Keuntungan FM Dibanding AM?", "id": "Kualitas Lebih Baik, Tahan Noise." },
  { "en": "Apa Kerugian FM Dibanding AM?", "id": "Membutuhkan Bandwidth Lebih Lebar." },
  { "en": "Apa Itu Modulasi Digital (Digital Modulation)?", "id": "Merepresentasikan Data Digital Sinyal Analog." },
  { "en": "Apa Itu Amplitude Shift Keying (ASK)?", "id": "Amplitudo Pembawa Bervariasi (On/Off)." },
  { "en": "Apa Itu Frequency Shift Keying (FSK)?", "id": "Frekuensi Pembawa Bervariasi (Dua Frekuensi)." },
  { "en": "Apa Itu Phase Shift Keying (PSK)?", "id": "Fasa Pembawa Bervariasi (Sudut Fasa)." },
  { "en": "Apa Itu Binary Phase Shift Keying (BPSK)?", "id": "PSK (Phase Shift Keying) Dengan Dua Fasa (0°, 180°)." },
  { "en": "Apa Itu Quadrature Phase Shift Keying (QPSK)?", "id": "PSK (Phase Shift Keying) Dengan Empat Fasa (4 Titik)." },
  { "en": "Berapa Bit Per Simbol Untuk QPSK?", "id": "Dua Bit Per Simbol." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Kombinasi Modulasi Amplitudo Dan Fasa." },
  { "en": "Apa Contoh QAM (Quadrature Amplitude Modulation)?", "id": "16-QAM, 64-QAM, 256-QAM." },
  { "en": "Semakin Tinggi Orde QAM, Bagaimana Laju Data?", "id": "Laju Data Lebih Tinggi (Bit/Simbol Banyak)." },
  { "en": "Semakin Tinggi Orde QAM, Bagaimana Kerentanan Noise?", "id": "Lebih Rentan Terhadap Noise." },
  { "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?", "id": "Plot Titik Simbol Modulasi Bidang Kompleks." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Kanal?", "id": "Rentang Frekuensi Dapat Dilewatkan Kanal." },
  { "en": "Apa Itu Kapasitas Kanal (Channel Capacity)?", "id": "Laju Data Maksimum Teoretis Kanal." },
  { "en": "Apa Itu Teorema Shannon-Hartley (Shannon-Hartley Theorem)?", "id": "Menghitung Kapasitas Kanal Dengan Noise." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Daya Sinyal Terhadap Daya Noise." },
  { "en": "SNR (Signal-to-Noise Ratio) Tinggi Berarti Kualitas Bagaimana?", "id": "Kualitas Sinyal Lebih Baik." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Rasio Jumlah Bit Salah Terhadap Total Bit." },
  { "en": "BER (Bit Error Rate) Rendah Berarti Kinerja Bagaimana?", "id": "Kinerja Komunikasi Lebih Baik." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi Deteksi/Koreksi Error." },
  { "en": "Apa Itu Kode Blok (Block Code)?", "id": "Kode Kanal Operasi Blok Bit Tetap." },
  { "en": "Contoh Kode Blok (Block Code)?", "id": "Kode Hamming, Kode BCH, Kode Reed-Solomon." },
  { "en": "Apa Itu Kode Konvolusi (Convolutional Code)?", "id": "Kode Kanal Operasi Aliran Bit (State)." },
  { "en": "Apa Itu Algoritma Viterbi (Viterbi Algorithm)?", "id": "Algoritma Dekode Kode Konvolusi Optimal." },
  { "en": "Apa Itu Kode Turbo (Turbo Code)?", "id": "Kode Kanal Kinerja Sangat Baik (Dekat Shannon)." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Code?", "id": "Kode Kanal Kinerja Baik Lainnya." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengubah Urutan Bit Atasi Burst Error." },
  { "en": "Apa Itu Burst Error (Kesalahan Beruntun)?", "id": "Error Terjadi Berkelompok Berdekatan." },
  { "en": "Apa Itu Ekualisasi (Equalization)?", "id": "Mengkompensasi Distorsi Kanal." },
  { "en": "Apa Itu Multipath Fading (Redaman Multipath)?", "id": "Sinyal Sampai Lewat Banyak Jalur Berbeda." },
  { "en": "Apa Akibat Multipath Fading (Redaman Multipath)?", "id": "Interferensi Konstruktif/Destruktif (Fluktuasi)." },
  { "en": "Apa Itu Keanekaragaman (Diversity)?", "id": "Teknik Mengirim/Menerima Sinyal Atasi Fading." },
  { "en": "Jenis Keanekaragaman (Diversity)?", "id": "Waktu, Frekuensi, Spasial (Antena)." },
  { "en": "Apa Itu Spread Spectrum (Spektrum Tersebar)?", "id": "Menyebarkan Sinyal Bandwidth Sangat Lebar." },
  { "en": "Apa Itu Direct Sequence Spread Spectrum (DSSS)?", "id": "Mengalikan Data Dengan Kode Penyebar Cepat." },
  { "en": "Apa Itu Frequency Hopping Spread Spectrum (FHSS)?", "id": "Mengubah Frekuensi Pembawa Cepat Acak." },
  { "en": "Apa Itu Multiple Access (Akses Ganda)?", "id": "Berbagi Sumber Daya Kanal Banyak Pengguna." },
  { "en": "Apa Itu FDMA (Frequency Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Frekuensi." },
  { "en": "Apa Itu TDMA (Time Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Slot Waktu." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Kode Unik." },
  { "en": "Apa Itu OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Akses Ganda Berbasis Subcarrier OFDM." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network)?", "id": "Jaringan Komunikasi Nirkabel Area Sel." },
  { "en": "Apa Itu Sel (Cell) Jaringan Seluler?", "id": "Area Geografis Dilayani Satu Base Station." },
  { "en": "Apa Itu Base Station (Stasiun Pangkalan) (BS)?", "id": "Pemancar/Penerima Tetap Dalam Sel." },
  { "en": "Apa Itu Mobile Station (Stasiun Bergerak) (MS)?", "id": "Perangkat Pengguna (Ponsel)." },
  { "en": "Apa Itu Handoff (Handoff) / Handover?", "id": "Perpindahan Koneksi MS Antar Sel." },
  { "en": "Apa Itu Penggunaan Kembali Frekuensi (Frequency Reuse)?", "id": "Menggunakan Frekuensi Sama Sel Jarak Jauh." },
  { "en": "Apa Itu Interferensi Co-Channel (Interferensi Kanal Sama)?", "id": "Gangguan Dari Sel Lain Frekuensi Sama." },
  { "en": "Apa Itu Interferensi Adjacent Channel (Kanal Berdekatan)?", "id": "Gangguan Dari Kanal Frekuensi Berdekatan." },
  { "en": "Generasi Jaringan Seluler (Mobile Network Generation)?", "id": "1G (Analog), 2G (Digital), 3G, 4G (LTE), 5G." },
  { "en": "Teknologi Utama 2G (Second Generation)?", "id": "GSM (Global System for Mobile Communications), CDMA (Code Division Multiple Access)." },
  { "en": "Teknologi Utama 3G (Third Generation)?", "id": "UMTS (Universal Mobile Telecommunications System) (WCDMA)." },
  { "en": "Teknologi Utama 4G (Fourth Generation)?", "id": "LTE (Long Term Evolution)." },
  { "en": "Teknologi Utama 5G (Fifth Generation)?", "id": "NR (New Radio), Massive MIMO, mmWave." },
  { "en": "Apa Keunggulan 5G (Fifth Generation)?", "id": "Kecepatan Sangat Tinggi, Latensi Rendah, Konektivitas Masif." },
  { "en": "Apa Itu Milimeter Wave (mmWave)?", "id": "Frekuensi Sangat Tinggi (30-300 GHz)." },
  { "en": "Tantangan Penggunaan mmWave (Milimeter Wave)?", "id": "Jangkauan Pendek, Mudah Terhalang." },
  { "en": "Apa Itu Massive MIMO (Massive MIMO)?", "id": "Penggunaan Antena Sangat Banyak Base Station." },
  { "en": "Apa Itu Network Slicing (Pemotongan Jaringan) 5G?", "id": "Membuat Jaringan Virtual Khusus Aplikasi." },
  { "en": "Apa Itu Komunikasi Satelit (Satellite Communication)?", "id": "Komunikasi Menggunakan Satelit Orbit." },
  { "en": "Apa Itu Orbit Geostasioner (GEO)?", "id": "Orbit Satelit Tampak Diam Dari Bumi." },
  { "en": "Apa Itu Orbit Bumi Rendah (LEO)?", "id": "Orbit Dekat Permukaan Bumi." },
  { "en": "Apa Itu Orbit Bumi Menengah (MEO)?", "id": "Orbit Antara LEO Dan GEO." },
  { "en": "Keuntungan Satelit LEO (Low Earth Orbit)?", "id": "Latensi Lebih Rendah Dari GEO." },
  { "en": "Kerugian Satelit LEO (Low Earth Orbit)?", "id": "Membutuhkan Konstelasi Satelit Banyak." },
  { "en": "Apa Itu Konstelasi Satelit (Satellite Constellation)?", "id": "Sekelompok Satelit Bekerja Bersama." },
  { "en": "Contoh Konstelasi LEO (Low Earth Orbit)?", "id": "Starlink, OneWeb." },
  { "en": "Apa Itu Uplink (Uplink) Satelit?", "id": "Transmisi Sinyal Dari Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink (Downlink) Satelit?", "id": "Transmisi Sinyal Dari Satelit Ke Bumi." },
  { "en": "Apa Itu Transponder (Transponder) Satelit?", "id": "Penerima, Penguat, Pemancar Frekuensi Satelit." },
  { "en": "Apa Itu Tunda Propagasi (Propagation Delay) Satelit?", "id": "Waktu Sinyal Tempuh Jarak Bumi-Satelit." },
  { "en": "Apa Itu Jejak Kaki (Footprint) Satelit?", "id": "Area Cakupan Sinyal Satelit Bumi." },
  { "en": "Apa Itu Global Positioning System (GPS)?", "id": "Sistem Navigasi Satelit Global." },
  { "en": "Bagaimana GPS (Global Positioning System) Menentukan Lokasi?", "id": "Trilaterasi Sinyal Dari Beberapa Satelit." },
  { "en": "Sistem Navigasi Satelit Lainnya?", "id": "GLONASS (Rusia), Galileo (Eropa), BeiDou (Cina)." },
  { "en": "Apa Itu Komunikasi Nirkabel Jarak Pendek?", "id": "Wi-Fi, Bluetooth, NFC, Zigbee." },
  { "en": "Apa Itu Bluetooth (Bluetooth)?", "id": "Standar Komunikasi Nirkabel Daya Rendah." },
  { "en": "Apa Itu Near Field Communication (NFC)?", "id": "Komunikasi Nirkabel Jarak Sangat Pendek." },
  { "en": "Apa Aplikasi NFC (Near Field Communication)?", "id": "Pembayaran Nirkontak, Pemasangan Cepat." },
  { "en": "Apa Itu Zigbee (Zigbee)?", "id": "Standar Jaringan Nirkabel Mesh Daya Rendah." },
  { "en": "Dimana Zigbee (Zigbee) Digunakan?", "id": "Otomatisasi Rumah, Jaringan Sensor." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Wireless Sensor Network (WSN)." },
  { "en": "Apa Kepanjangan WSN (Wireless Sensor Network)?", "id": "Jaringan Sensor Nirkabel." },
  { "en": "Apa Fungsi WSN (Wireless Sensor Network)?", "id": "Kumpulan Sensor Terdistribusi Pantau Lingkungan." },
  { "en": "Apa Itu LoRa (Long Range)?", "id": "Teknologi Komunikasi Nirkabel Jarak Jauh Daya Rendah." },
  { "en": "Apa Itu LoRaWAN (Long Range Wide Area Network)?", "id": "Protokol Jaringan Luas Berbasis LoRa." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Protokol Komunikasi IoT (Internet of Things) Umum?", "id": "MQTT, CoAP, HTTP." },
  { "en": "Apa Itu MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan Ringan (Pub/Sub)." },
  { "en": "Apa Itu Teori Informasi (Information Theory)?", "id": "Studi Kuantifikasi, Komunikasi Informasi." },
  { "en": "Apa Itu Entropi (Entropy) Informasi?", "id": "Ukuran Ketidakpastian Sumber Informasi." },
  { "en": "Apa Itu Informasi Mutual (Mutual Information)?", "id": "Ukuran Ketergantungan Dua Variabel Acak." },
  { "en": "Apa Itu Pengkodean Sumber (Source Coding)?", "id": "Kompresi Data (Hapus Redundansi)." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi Koreksi Error." },
  { "en": "Apa Batas Shannon (Shannon Limit)?", "id": "Batas Kapasitas Kanal Teoretis." },
  { "en": "Apa Itu Spektrum Frekuensi Radio (Radio Spectrum)?", "id": "Sumber Daya Terbatas Frekuensi Radio." },
  { "en": "Siapa Mengatur Spektrum Frekuensi Radio?", "id": "Badan Regulasi Telekomunikasi (ITU, Pemerintah)." },
  { "en": "Apa Itu Lisensi Spektrum (Spectrum Licensing)?", "id": "Pemberian Hak Penggunaan Frekuensi Tertentu." },
  { "en": "Apa Itu Spektrum Tak Berlisensi (Unlicensed Spectrum)?", "id": "Frekuensi Bebas Digunakan (Wi-Fi, Bluetooth)." },
  { "en": "Apa Itu Alokasi Spektrum (Spectrum Allocation)?", "id": "Pembagian Spektrum Untuk Layanan Berbeda." },
  { "en": "Apa Itu Interferensi (Interference)?", "id": "Superposisi Gelombang Hasilkan Pola Baru." },
  { "en": "Apa Itu Interferensi Konstruktif (Constructive Interference)?", "id": "Amplitudo Bertambah (Fasa Sama)." },
  { "en": "Apa Itu Interferensi Destruktif (Destructive Interference)?", "id": "Amplitudo Berkurang/Nol (Fasa Berlawanan)." },
  { "en": "Apa Itu Koherensi (Coherence)?", "id": "Hubungan Fasa Konstan Antar Gelombang." },
  { "en": "Syarat Terjadi Interferensi (Interference) Stabil?", "id": "Sumber Harus Koheren." },
  { "en": "Apa Itu Percobaan Celah Ganda Young (Young's Double Slit)?", "id": "Demonstrasi Interferensi Cahaya." },
  { "en": "Apa Pola Terbentuk Percobaan Young?", "id": "Pola Terang Gelap Bergantian (Frinji)." },
  { "en": "Apa Itu Difraksi (Diffraction)?", "id": "Pelenturan Gelombang Sekitar Hambatan." },
  { "en": "Apa Prinsip Huygens (Huygens' Principle)?", "id": "Setiap Titik Muka Gelombang Sumber Baru." },
  { "en": "Apa Itu Difraksi Celah Tunggal (Single Slit Diffraction)?", "id": "Pola Difraksi Cahaya Lewat Celah Sempit." },
  { "en": "Apa Pola Terbentuk Difraksi Celah Tunggal?", "id": "Pola Terang Pusat Lebar, Samping Lebih Kecil." },
  { "en": "Apa Itu Kisi Difraksi (Diffraction Grating)?", "id": "Banyak Celah Sempit Paralel." },
  { "en": "Apa Fungsi Kisi Difraksi (Diffraction Grating)?", "id": "Memisahkan Cahaya Menjadi Spektrum Warna." },
  { "en": "Apa Itu Spektrometer (Spectrometer)?", "id": "Alat Ukur Spektrum Cahaya (Gunakan Kisi)." },
  { "en": "Apa Itu Resolusi (Resolution) Optik?", "id": "Kemampuan Bedakan Dua Objek Berdekatan." },
  { "en": "Apa Itu Kriteria Rayleigh (Rayleigh Criterion)?", "id": "Batas Resolusi Dua Sumber Cahaya." },
  { "en": "Apa Itu Polarisasi (Polarization) Cahaya?", "id": "Pembatasan Arah Getar Medan Listrik." },
  { "en": "Mengapa Cahaya Bisa Dipolarisasi?", "id": "Karena Cahaya Gelombang Transversal." },
  { "en": "Apa Itu Cahaya Tak Terpolarisasi (Unpolarized Light)?", "id": "Arah Getar Acak Tegak Lurus Arah Rambat." },
  { "en": "Apa Itu Polarisator (Polarizer)?", "id": "Filter Melewatkan Cahaya Polarisasi Tertentu." },
  { "en": "Apa Itu Hukum Malus (Malus's Law)?", "id": "Intensitas Cahaya Lewat Polarisator Kedua." },
  { "en": "Apa Itu Polarisasi Pemantulan (Polarization by Reflection)?", "id": "Cahaya Pantul Terpolarisasi Sudut Brewster." },
  { "en": "Apa Itu Sudut Brewster (Brewster's Angle)?", "id": "Sudut Pantul Hasilkan Polarisasi Sempurna." },
  { "en": "Apa Itu Polarisasi Hamburan (Polarization by Scattering)?", "id": "Cahaya Hambur Terpolarisasi Sebagian." },
  { "en": "Contoh Polarisasi Hamburan (Polarization by Scattering)?", "id": "Cahaya Biru Langit." },
  { "en": "Apa Itu Bahan Birefringent (Dwi Bias)?", "id": "Bahan Indeks Bias Berbeda Polarisasi." },
  { "en": "Contoh Bahan Birefringent (Dwi Bias)?", "id": "Kalsit (Calcite), Kuarsa (Quartz)." },
  { "en": "Apa Itu Aktivitas Optik (Optical Activity)?", "id": "Kemampuan Bahan Putar Bidang Polarisasi." },
  { "en": "Apa Itu Efek Faraday (Faraday Effect)?", "id": "Pemutaran Polarisasi Medan Magnet." },
  { "en": "Apa Itu Efek Kerr (Kerr Effect)?", "id": "Birefringence Diinduksi Medan Listrik Eksternal." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber Cahaya Koheren, Monokromatik, Intens." },
  { "en": "Apa Kepanjangan Laser (Laser)?", "id": "Light Amplification Stimulated Emission Radiation." },
  { "en": "Prinsip Dasar Laser (Laser)?", "id": "Emisi Terstimulasi (Stimulated Emission)." },
  { "en": "Apa Itu Emisi Spontan (Spontaneous Emission)?", "id": "Atom Pancarkan Foton Acak." },
  { "en": "Apa Itu Emisi Terstimulasi (Stimulated Emission)?", "id": "Foton Datang Picu Emisi Foton Identik." },
  { "en": "Apa Itu Inversi Populasi (Population Inversion)?", "id": "Lebih Banyak Atom Tingkat Energi Tinggi." },
  { "en": "Mengapa Perlu Inversi Populasi (Population Inversion)?", "id": "Agar Emisi Terstimulasi Dominan." },
  { "en": "Apa Itu Pompa (Pumping) Laser?", "id": "Proses Menciptakan Inversi Populasi." },
  { "en": "Metode Pompa (Pumping) Laser?", "id": "Optik, Listrik, Kimia." },
  { "en": "Apa Itu Resonator Optik (Optical Resonator)?", "id": "Dua Cermin Pantulkan Cahaya Penguatan." },
  { "en": "Apa Sifat Cahaya Laser (Laser Light)?", "id": "Koheren, Monokromatik, Terarah, Intensitas Tinggi." },
  { "en": "Apa Itu Koherensi (Coherence) Spasial?", "id": "Korelasi Fasa Antar Titik Berbeda Berkas." },
  { "en": "Apa Itu Koherensi (Coherence) Temporal?", "id": "Korelasi Fasa Gelombang Waktu Berbeda." },
  { "en": "Apa Itu Mode Longitudinal (Longitudinal Mode) Laser?", "id": "Frekuensi Resonansi Diperbolehkan Resonator." },
  { "en": "Apa Itu Mode Transversal (Transverse Mode) Laser?", "id": "Pola Intensitas Penampang Berkas Laser." },
  { "en": "Mode Transversal (Transverse Mode) Dasar Laser?", "id": "TEM₀₀ (Gaussian Beam)." },
  { "en": "Jenis Laser (Laser) Berdasarkan Medium Aktif?", "id": "Gas, Padat, Semikonduktor, Cair (Pewarna)." },
  { "en": "Contoh Laser Gas (Gas Laser)?", "id": "HeNe (Helium-Neon), CO₂, Argon Ion." },
  { "en": "Contoh Laser Padat (Solid-State Laser)?", "id": "Nd:YAG (Neodymium-doped Yttrium Aluminum Garnet), Ruby, Ti:Sapphire." },
  { "en": "Contoh Laser Semikonduktor (Semiconductor Laser)?", "id": "Dioda Laser (Laser Diode)." },
  { "en": "Contoh Laser Cair (Liquid Laser)?", "id": "Laser Pewarna (Dye Laser) (Panjang Gelombang Bisa Diatur)." },
  { "en": "Apa Itu Laser Excimer (Excimer Laser)?", "id": "Laser Gas Hasilkan Cahaya UV." },
  { "en": "Apa Itu Laser Serat (Fiber Laser)?", "id": "Laser Medium Aktif Serat Optik Didoping." },
  { "en": "Aplikasi Laser (Laser)?", "id": "Komunikasi, Industri, Medis, Riset, Hiburan." },
  { "en": "Apa Itu Holografi (Holography)?", "id": "Teknik Rekam, Rekonstruksi Gambar 3D Laser." },
  { "en": "Apa Beda Hologram, Fotografi Biasa?", "id": "Hologram Rekam Amplitudo Dan Fasa Cahaya." },
  { "en": "Apa Itu Interferometer (Interferometer)?", "id": "Alat Ukur Presisi Interferensi Gelombang." },
  { "en": "Contoh Interferometer (Interferometer)?", "id": "Michelson, Fabry-Perot." },
  { "en": "Aplikasi Interferometri (Interferometry)?", "id": "Ukur Panjang, Indeks Bias, Deteksi Gravitasi." },
  { "en": "Apa Itu LIGO (Laser Interferometer Gravitational-Wave Observatory)?", "id": "Observatorium Deteksi Gelombang Gravitasi." },
  { "en": "Apa Itu Optika Non-Linear (Non-Linear Optics)?", "id": "Studi Interaksi Cahaya Intensitas Tinggi Materi." },
  { "en": "Apa Itu Pembangkitan Harmonik Kedua (SHG)?", "id": "Second Harmonic Generation (SHG)." },
  { "en": "Apa Kepanjangan SHG (Second Harmonic Generation)?", "id": "Pembangkitan Harmonik Kedua." },
  { "en": "Apa Proses SHG (Second Harmonic Generation)?", "id": "Mengubah Cahaya Frekuensi f Jadi 2f." },
  { "en": "Apa Itu Pencampuran Frekuensi (Frequency Mixing)?", "id": "Menghasilkan Frekuensi Jumlah/Selisih." },
  { "en": "Apa Itu Efek Kerr (Kerr Effect) Optik?", "id": "Perubahan Indeks Bias Proporsional Intensitas." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Ilmu Teknologi Hasilkan, Kontrol Foton." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Perangkat Konversi Cahaya Ke Listrik, Sebaliknya." },
  { "en": "Contoh Perangkat Optoelektronik?", "id": "LED, Fotodioda, Sel Surya, Laser Dioda." },
  { "en": "Apa Itu Kristal Fotonik (Photonic Crystal)?", "id": "Struktur Periodik Pengaruhi Propagasi Cahaya." },
  { "en": "Apa Itu Celah Pita Fotonik (Photonic Band Gap)?", "id": "Rentang Frekuensi Cahaya Dilarang Merambat." },
  { "en": "Apa Itu Metamaterial (Metamaterial) Optik?", "id": "Material Buatan Sifat Optik Tak Biasa." },
  { "en": "Apa Itu Indeks Bias Negatif (Negative Refractive Index)?", "id": "Sifat Metamaterial Bisa Bengkokkan Cahaya Aneh." },
  { "en": "Apa Itu Jubah Tembus Pandang (Invisibility Cloak)?", "id": "Konsep Gunakan Metamaterial Bengkokkan Cahaya." },
  { "en": "Apa Itu Optika Kuantum (Quantum Optics)?", "id": "Studi Interaksi Cahaya, Materi Level Kuantum." },
  { "en": "Apa Itu Keadaan Terperas (Squeezed State) Cahaya?", "id": "Cahaya Ketidakpastian Kuantum Dikurangi Satu Arah." },
  { "en": "Apa Itu Komputasi Kuantum Optik (Optical Quantum Computing)?", "id": "Komputasi Kuantum Gunakan Foton Sebagai Qubit." },
  { "en": "Apa Itu Efek Casimir (Casimir Effect)?", "id": "Gaya Tarik Antara Pelat Konduktor Dekat Vakum." },
  { "en": "Penyebab Efek Casimir (Casimir Effect)?", "id": "Fluktuasi Vakum Medan Elektromagnetik Kuantum." },
  { "en": "Apa Itu Radiasi Hawking (Hawking Radiation)?", "id": "Radiasi Termal Diprediksi Dipancarkan Lubang Hitam." },
  { "en": "Apa Itu Fisika Statistik (Statistical Physics)?", "id": "Gunakan Probabilitas Deskripsi Sistem Banyak Partikel." },
  { "en": "Apa Itu Ensemble (Ensemble) Statistik?", "id": "Kumpulan Sistem Makroskopik Identik." },
  { "en": "Apa Itu Ensemble Mikrokanonik (Microcanonical Ensemble)?", "id": "Energi, Volume, Jumlah Partikel Tetap." },
  { "en": "Apa Itu Ensemble Kanonik (Canonical Ensemble)?", "id": "Suhu, Volume, Jumlah Partikel Tetap." },
  { "en": "Apa Itu Ensemble Kanonik Besar (Grand Canonical Ensemble)?", "id": "Suhu, Volume, Potensial Kimia Tetap." },
  { "en": "Apa Itu Fungsi Partisi (Partition Function)?", "id": "Besaran Pusat Mekanika Statistik Kanonik." },
  { "en": "Apa Itu Distribusi Maxwell-Boltzmann (Maxwell-Boltzmann Distribution)?", "id": "Distribusi Kecepatan Partikel Gas Ideal Klasik." },
  { "en": "Apa Itu Distribusi Bose-Einstein (Bose-Einstein Distribution)?", "id": "Distribusi Energi Partikel Boson." },
  { "en": "Apa Itu Distribusi Fermi-Dirac (Fermi-Dirac Distribution)?", "id": "Distribusi Energi Partikel Fermion." },
  { "en": "Apa Itu Transisi Fasa (Phase Transition)?", "id": "Perubahan Keadaan Makroskopik Materi." },
  { "en": "Contoh Transisi Fasa (Phase Transition)?", "id": "Mencair, Membeku, Menguap, Mengembun." },
  { "en": "Apa Itu Titik Kritis (Critical Point)?", "id": "Titik Akhir Kurva Koeksistensi Fasa." },
  { "en": "Apa Itu Fenomena Kritis (Critical Phenomena)?", "id": "Perilaku Universal Sistem Dekat Titik Kritis." },
  { "en": "Apa Itu Grup Renormalisasi (Renormalization Group)?", "id": "Teknik Matematis Analisis Fenomena Kritis." },
  { "en": "Apa Itu Model Ising (Ising Model)?", "id": "Model Sederhana Feromagnetisme Transisi Fasa." },
  { "en": "Apa Itu Sistem Kompleks (Complex System)?", "id": "Sistem Banyak Komponen Interaksi Non-Linear." },
  { "en": "Apa Itu Teori Chaos (Chaos Theory)?", "id": "Studi Sistem Dinamis Sangat Sensitif Kondisi Awal." },
  { "en": "Apa Itu Efek Kupu-Kupu (Butterfly Effect)?", "id": "Ilustrasi Sensitivitas Kondisi Awal Chaos." },
  { "en": "Apa Itu Fraktal (Fractal)?", "id": "Bentuk Geometri Kompleks Self-Similar." },
  { "en": "Contoh Fraktal (Fractal)?", "id": "Himpunan Mandelbrot, Segitiga Sierpinski." },
  { "en": "Apa Itu Self-Similarity (Kemiripan Diri)?", "id": "Bagian Objek Mirip Keseluruhan Skala Berbeda." },
  { "en": "Apa Itu Teori Jaringan (Network Theory)?", "id": "Studi Graf Representasi Relasi Entitas." },
  { "en": "Apa Itu Jaringan Skala Bebas (Scale-Free Network)?", "id": "Distribusi Derajat Ikuti Hukum Pangkat." },
  { "en": "Apa Itu Jaringan Dunia Kecil (Small-World Network)?", "id": "Jarak Antar Node Rata-Rata Pendek." },
  { "en": "Apa Itu Teorema Millman (Millman's Theorem)?", "id": "Menyederhanakan Sumber Tegangan Paralel." },
  { "en": "Apa Syarat Berlaku Teorema Millman?", "id": "Sumber Tegangan Seri Resistor Paralel." },
  { "en": "Apa Itu Teorema Resiprositas (Reciprocity Theorem)?", "id": "Pertukaran Sumber Respon Rangkaian Linear." },
  { "en": "Apa Itu Jaringan Dua Port (Two-Port Network)?", "id": "Rangkaian Listrik Dua Pasang Terminal." },
  { "en": "Parameter Apa Mengkarakterisasi Jaringan Dua Port?", "id": "Parameter Z, Y, H, ABCD, S." },
  { "en": "Apa Itu Parameter Z (Impedansi)?", "id": "Menggambarkan Tegangan Sebagai Fungsi Arus." },
  { "en": "Apa Itu Parameter Y (Admitansi)?", "id": "Menggambarkan Arus Sebagai Fungsi Tegangan." },
  { "en": "Apa Itu Parameter H (Hibrid)?", "id": "Campuran (Berguna Analisis Transistor)." },
  { "en": "Apa Itu Parameter ABCD (Transmisi)?", "id": "Menghubungkan Input Ke Output (Kaskade)." },
  { "en": "Apa Itu Parameter S (Hamburan)?", "id": "Parameter Jaringan Frekuensi Tinggi (RF)." },
  { "en": "Apa Itu Kondisi Batas (Boundary Condition) EM?", "id": "Perilaku Medan Antarmuka Dua Medium." },
  { "en": "Bagaimana Medan Listrik Tangensial Di Konduktor?", "id": "Nol Di Permukaan Konduktor Ideal." },
  { "en": "Bagaimana Medan Magnet Normal Di Konduktor?", "id": "Nol Di Permukaan Konduktor Ideal." },
  { "en": "Apa Itu Persamaan Gelombang (Wave Equation)?", "id": "Persamaan Diferensial Deskripsi Perambatan Gelombang." },
  { "en": "Apa Solusi Umum Persamaan Gelombang Satu Dimensi?", "id": "Gelombang Berjalan Maju Dan Mundur." },
  { "en": "Apa Itu Kecepatan Fasa (Phase Velocity)?", "id": "Kecepatan Perambatan Titik Fasa Konstan." },
  { "en": "Apa Itu Kecepatan Grup (Group Velocity)?", "id": "Kecepatan Perambatan Amplop Gelombang (Energi)." },
  { "en": "Apa Itu Dispersi (Dispersion)?", "id": "Kecepatan Gelombang Tergantung Frekuensi." },
  { "en": "Apa Itu Medium Non-Dispersif (Non-Dispersive)?", "id": "Kecepatan Gelombang Tidak Tergantung Frekuensi." },
  { "en": "Apa Itu Impedansi Gelombang (Wave Impedance)?", "id": "Rasio Medan Listrik Terhadap Magnet Gelombang." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang Linear?", "id": "Medan Listrik Bergetar Sepanjang Garis." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang Sirkular?", "id": "Medan Listrik Berputar Membentuk Lingkaran." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang Eliptik?", "id": "Medan Listrik Berputar Membentuk Elips." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Terkonsentrasi Dekat Permukaan Konduktor." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth) (Delta)?", "id": "Ukuran Penetrasi Arus AC Konduktor." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Struktur Pemandu Perambatan Gelombang EM." },
  { "en": "Model Parameter Terdistribusi Saluran?", "id": "Model Rangkaian R, L, G, C Kontinu." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance) (Z₀)?", "id": "Impedansi Saluran Tak Terhingga." },
  { "en": "Apa Itu Konstanta Propagasi (Propagation Constant) (Gamma)?", "id": "Menentukan Atenuasi Dan Fasa Gelombang." },
  { "en": "Apa Itu Konstanta Atenuasi (Attenuation Constant) (Alpha)?", "id": "Ukuran Pelemahan Gelombang Per Jarak." },
  { "en": "Apa Itu Konstanta Fasa (Phase Constant) (Beta)?", "id": "Ukuran Perubahan Fasa Per Jarak." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient) (Gamma)?", "id": "Rasio Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Penyebab Refleksi (Reflection) Sinyal?", "id": "Ketidakcocokan Impedansi Beban Saluran." },
  { "en": "Apa Itu Gelombang Berdiri (Standing Wave)?", "id": "Hasil Interferensi Gelombang Datang Pantul." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Tegangan Maksimum Minimum Gelombang Berdiri." },
  { "en": "Nilai VSWR (Voltage Standing Wave Ratio) Ideal?", "id": "Satu (Tidak Ada Pantulan)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Membuat Impedansi Beban Sama Saluran." },
  { "en": "Mengapa Pencocokan Impedansi (Impedance Matching) Penting?", "id": "Transfer Daya Maksimal, Minimalkan Pantulan." },
  { "en": "Apa Itu Transformator Lambda Perempat (λ/4)?", "id": "Saluran λ/4 Untuk Mencocokkan Impedansi." },
  { "en": "Apa Itu Stub (Stub) Pencocokan?", "id": "Saluran Transmisi Pendek Tambahan Matching." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Diagram Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Karakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Transduser Konversi Sinyal Listrik Gelombang EM." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Distribusi Spasial Daya Dipancarkan Antena." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Dibanding Sumber Isotropik." },
  { "en": "Apa Itu Direktifitas (Directivity) Antena?", "id": "Rasio Intensitas Maksimum Terhadap Rata-Rata." },
  { "en": "Apa Itu Efisiensi (Efficiency) Antena?", "id": "Rasio Daya Radiasi Terhadap Input." },
  { "en": "Apa Itu Lebar Berkas (Beamwidth) Antena?", "id": "Sudut Antara Titik Setengah Daya Utama." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Dipancarkan." },
  { "en": "Apa Itu Antena Dipol (Dipole Antenna)?", "id": "Antena Dasar Dua Elemen Lurus." },
  { "en": "Apa Itu Antena Monopol (Monopole Antenna)?", "id": "Setengah Dipol Di Atas Bidang Tanah." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda)?", "id": "Antena Direktif Dengan Elemen Parasitik." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Susunan Beberapa Elemen Antena." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengatur Fasa Array Arahkan Berkas." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Struktur Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Mode Dominan (Dominant Mode)?", "id": "Mode Propagasi Frekuensi Cutoff Terendah." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Menyimpan Energi Elektromagnetik." },
  { "en": "Aplikasi Resonator Rongga (Cavity Resonator)?", "id": "Filter Frekuensi Tinggi, Osilator." },
  { "en": "Apa Itu Komunikasi Optik (Optical Communication)?", "id": "Transmisi Informasi Menggunakan Cahaya." },
  { "en": "Apa Media Utama Komunikasi Optik?", "id": "Serat Optik (Optical Fiber)." },
  { "en": "Prinsip Propagasi Cahaya Serat Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Serat Mode Tunggal (Single Mode)?", "id": "Serat Inti Kecil Jarak Jauh." },
  { "en": "Apa Itu Serat Mode Jamak (Multi Mode)?", "id": "Serat Inti Besar Jarak Pendek." },
  { "en": "Apa Sumber Cahaya (Light Source) Optik?", "id": "LED Atau Laser Diode." },
  { "en": "Apa Detektor (Detector) Optik?", "id": "Fotodioda (PIN Atau APD)." },
  { "en": "Apa Itu Redaman (Attenuation) Serat Optik?", "id": "Pelemahan Intensitas Cahaya Serat." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Pelebaran Pulsa Cahaya Serat." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Berdasarkan Panjang Gelombang." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung." },
  { "en": "Contoh Penguat Optik (Optical Amplifier)?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Menumpangkan Informasi Ke Pembawa." },
  { "en": "Apa Itu Modulasi Analog (Analog Modulation)?", "id": "AM (Amplitude Modulation), FM (Frequency Modulation), PM (Phase Modulation)." },
  { "en": "Apa Itu Modulasi Digital (Digital Modulation)?", "id": "ASK (Amplitude Shift Keying), FSK (Frequency Shift Keying), PSK (Phase Shift Keying), QAM (Quadrature Amplitude Modulation)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Sinyal?", "id": "Rentang Frekuensi Ditempati Sinyal." },
  { "en": "Apa Itu Teorema Shannon (Shannon Theorem) Kapasitas?", "id": "Batas Laju Data Maksimum Kanal." },
  { "en": "Apa Itu Rasio Sinyal-Derau (SNR)?", "id": "Perbandingan Daya Sinyal Terhadap Derau." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi Deteksi/Koreksi Error." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengubah Urutan Bit Atasi Burst Error." },
  { "en": "Apa Itu Multipath Fading (Redaman Multipath)?", "id": "Fluktuasi Sinyal Akibat Lintasan Ganda." },
  { "en": "Apa Itu Keanekaragaman (Diversity)?", "id": "Teknik Mengurangi Efek Fading." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Penggunaan Multi Antena Transmit Receive." },
  { "en": "Apa Itu OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Modulasi Multi Pembawa Orthogonal." },
  { "en": "Apa Itu Akses Ganda (Multiple Access)?", "id": "Berbagi Kanal Komunikasi Banyak Pengguna." },
  { "en": "Apa Itu FDMA (Frequency Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Frekuensi." },
  { "en": "Apa Itu TDMA (Time Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Waktu." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Berbagi Kanal Berdasarkan Kode Unik." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network)?", "id": "Jaringan Komunikasi Nirkabel Area Sel." },
  { "en": "Apa Itu Handoff (Handoff) / Handover?", "id": "Perpindahan Koneksi Pengguna Antar Sel." },
  { "en": "Apa Itu Komunikasi Satelit (Satellite Communication)?", "id": "Komunikasi Menggunakan Satelit Orbit." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Penentuan Posisi Global Satelit." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Pertukaran Data." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Tujuh Lapisan Jaringan." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Arsitektur Jaringan Internet." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Unik Di Jaringan." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Unik Perangkat Keras." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Teknologi Jaringan LAN Kabel." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan LAN Nirkabel." },
  { "en": "Apa Itu Router (Router)?", "id": "Menghubungkan Jaringan, Meneruskan Paket." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Menghubungkan Perangkat Dalam Satu LAN." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Penyaring Lalu Lintas." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Pembangkitan Hingga Penggunaan Listrik." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Konversi, Kontrol Daya Listrik." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Pengukuran Listrik (Electrical Measurement)?", "id": "Proses Kuantifikasi Besaran Listrik." },
  { "en": "Apa Instrumen Pengukuran (Measuring Instrument) Dasar?", "id": "Voltmeter, Ammeter, Ohmmeter." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Pengukuran Yang Sama." },
  { "en": "Apa Itu Resolusi (Resolution)?", "id": "Perubahan Terkecil Dapat Diukur." },
  { "en": "Apa Itu Sensitivitas (Sensitivity)?", "id": "Rasio Perubahan Output Terhadap Input." },
  { "en": "Apa Itu Kesalahan (Error) Pengukuran?", "id": "Selisih Antara Nilai Ukur, Nilai Benar." },
  { "en": "Jenis Kesalahan (Error) Pengukuran Utama?", "id": "Sistematik, Acak, Dan Kotor." },
  { "en": "Apa Itu Kesalahan Sistematik (Systematic Error)?", "id": "Kesalahan Konsisten (Kalibrasi)." },
  { "en": "Apa Itu Kesalahan Acak (Random Error)?", "id": "Kesalahan Fluktuatif Tak Terduga (Noise)." },
  { "en": "Apa Itu Kesalahan Kotor (Gross Error)?", "id": "Kesalahan Akibat Kecerobohan Pengamat." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Membandingkan Alat Ukur Dengan Standar." },
  { "en": "Apa Itu Standar Pengukuran (Measurement Standard)?", "id": "Referensi Akurat Besaran Ukur." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty) Pengukuran?", "id": "Rentang Keraguan Nilai Ukur." },
  { "en": "Apa Itu Multimeter Analog (Analog Multimeter)?", "id": "Pengukur Jarum Skala Kontinu." },
  { "en": "Apa Itu Gerakan D'Arsonval (PMMC)?", "id": "Prinsip Dasar Meter Analog Kumparan Putar." },
  { "en": "Apa Itu Multimeter Digital (Digital Multimeter) (DMM)?", "id": "Pengukur Tampilan Angka Digital." },
  { "en": "Keuntungan DMM (Digital Multimeter) Dibanding Analog?", "id": "Akurasi Lebih Tinggi, Mudah Dibaca." },
  { "en": "Apa Itu True RMS (True RMS) Multimeter?", "id": "Mengukur Nilai RMS Akurat Gelombang Non-Sinus." },
  { "en": "Apa Itu Efek Pemanasan (Loading Effect) Voltmeter?", "id": "Voltmeter Mengubah Tegangan Yang Diukur." },
  { "en": "Bagaimana Resistansi Internal Voltmeter Ideal?", "id": "Tak Terhingga Ohm." },
  { "en": "Bagaimana Resistansi Internal Amperemeter Ideal?", "id": "Nol Ohm." },
  { "en": "Apa Itu Resistor Shunt (Shunt Resistor)?", "id": "Resistor Paralel Perluas Jangkauan Amperemeter." },
  { "en": "Apa Itu Resistor Pengganda (Multiplier Resistor)?", "id": "Resistor Seri Perluas Jangkauan Voltmeter." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Bentuk Gelombang Tegangan Waktu." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Osiloskop?", "id": "Frekuensi Maksimum Dapat Ditampilkan Akurat." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) Osiloskop Digital?", "id": "Kecepatan ADC (Analog-to-Digital Converter) Mengambil Sampel." },
  { "en": "Apa Itu Pemicuan (Triggering) Osiloskop?", "id": "Menstabilkan Tampilan Gelombang Berulang." },
  { "en": "Apa Itu Probe (Probe) Osiloskop?", "id": "Kabel Input Osiloskop Dengan Atenuasi." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Penghasil Sinyal Uji Berbagai Bentuk." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Menampilkan Komponen Frekuensi Sinyal." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Menganalisis Sinyal Digital Multi Kanal." },
  { "en": "Apa Itu Pengukur LCR (LCR Meter)?", "id": "Mengukur Resistansi, Kapasitansi, Induktansi." },
  { "en": "Apa Itu Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Rangkaian Presisi Pengukuran Resistansi." },
  { "en": "Apa Itu Jembatan Kelvin (Kelvin Bridge)?", "id": "Pengukuran Presisi Resistansi Sangat Rendah." },
  { "en": "Apa Itu Jembatan Maxwell (Maxwell Bridge)?", "id": "Pengukuran Induktansi Tidak Diketahui." },
  { "en": "Apa Itu Jembatan Hay (Hay Bridge)?", "id": "Pengukuran Induktansi Faktor Q Tinggi." },
  { "en": "Apa Itu Jembatan Schering (Schering Bridge)?", "id": "Pengukuran Kapasitansi, Faktor Disipasi." },
  { "en": "Apa Itu Wattmeter (Wattmeter)?", "id": "Mengukur Daya Listrik Nyata (Real Power)." },
  { "en": "Prinsip Kerja Wattmeter (Wattmeter) Elektrodinamometer?", "id": "Interaksi Medan Magnet Koil Arus, Tegangan." },
  { "en": "Apa Itu Pengukur Energi (Energy Meter)?", "id": "Mengukur Konsumsi Energi Listrik (kWh)." },
  { "en": "Apa Nama Lain Pengukur Energi (Energy Meter)?", "id": "kWh Meter." },
  { "en": "Apa Itu Pengukur Faktor Daya (Power Factor Meter)?", "id": "Mengukur Faktor Daya Rangkaian AC." },
  { "en": "Apa Itu Megohmmeter (Insulation Tester)?", "id": "Mengukur Resistansi Isolasi Sangat Tinggi." },
  { "en": "Apa Nama Populer Megohmmeter (Insulation Tester)?", "id": "Megger." },
  { "en": "Apa Itu Earth Tester (Penguji Bumi)?", "id": "Mengukur Resistansi Elektroda Pembumian." },
  { "en": "Apa Itu Tang Ampere (Clamp Meter)?", "id": "Mengukur Arus Tanpa Memutus Sirkuit." },
  { "en": "Prinsip Kerja Tang Ampere (Clamp Meter)?", "id": "Induksi Magnetik (Transformator Arus)." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Jadi Sinyal Listrik." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Mengubah Energi Satu Bentuk Ke Lain." },
  { "en": "Apakah Sensor Selalu Transduser?", "id": "Ya, Sensor Adalah Jenis Transduser." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Jadi Aksi Fisik." },
  { "en": "Apa Itu Pengkondisian Sinyal (Signal Conditioning)?", "id": "Memproses Sinyal Sensor Agar Sesuai." },
  { "en": "Apa Itu Penguatan (Amplification)?", "id": "Meningkatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Melemahkan Amplitudo Sinyal." },
  { "en": "Apa Itu Penyaringan (Filtering)?", "id": "Menghilangkan Komponen Frekuensi Tertentu." },
  { "en": "Apa Itu Linierisasi (Linearization)?", "id": "Membuat Respon Sensor Menjadi Linear." },
  { "en": "Apa Itu Isolasi (Isolation)?", "id": "Memisahkan Elektrikal Input Dan Output." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Sinyal Informasi Ke Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Ekstraksi Sinyal Informasi Dari Pembawa." },
  { "en": "Apa Itu Akuisisi Data (Data Acquisition) (DAQ)?", "id": "Pengumpulan Data Analog Ke Komputer." },
  { "en": "Apa Itu Telemetri (Telemetry)?", "id": "Pengukuran Jarak Jauh Lewat Komunikasi." },
  { "en": "Apa Itu Bahan Konduktor (Conductor)?", "id": "Mudah Hantarkan Listrik (Elektron Bebas)." },
  { "en": "Apa Itu Bahan Isolator (Insulator)?", "id": "Sulit Hantarkan Listrik (Celah Energi Besar)." },
  { "en": "Apa Itu Bahan Semikonduktor (Semiconductor)?", "id": "Konduktivitas Antara Konduktor, Isolator." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Ukuran Hambatan Intrinsik Material." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Ukuran Kemudahan Hantar Listrik." },
  { "en": "Apa Itu Efek Suhu Pada Resistivitas Logam?", "id": "Resistivitas Umumnya Meningkat Dengan Suhu." },
  { "en": "Apa Itu Superkonduktivitas (Superconductivity)?", "id": "Resistansi Nol Pada Suhu Sangat Rendah." },
  { "en": "Apa Itu Bahan Dielektrik (Dielectric)?", "id": "Isolator Digunakan Dalam Kapasitor." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Tegangan Maksimum Sebelum Breakdown." },
  { "en": "Apa Itu Bahan Magnetik (Magnetic Material)?", "id": "Bahan Berinteraksi Dengan Medan Magnet." },
  { "en": "Jenis Bahan Magnetik (Magnetic Material)?", "id": "Diamagnetik, Paramagnetik, Feromagnetik." },
  { "en": "Apa Itu Diamagnetisme (Diamagnetism)?", "id": "Sedikit Menolak Medan Magnet Eksternal." },
  { "en": "Apa Itu Paramagnetisme (Paramagnetism)?", "id": "Sedikit Tertarik Medan Magnet Eksternal." },
  { "en": "Apa Itu Feromagnetisme (Ferromagnetism)?", "id": "Sangat Tertarik Medan Magnet (Bisa Magnet Permanen)." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik (μ)?", "id": "Ukuran Kemampuan Hantarkan Fluks Magnet." },
  { "en": "Apa Itu Loop Histeresis (Hysteresis Loop)?", "id": "Grafik B-H Bahan Feromagnetik." },
  { "en": "Apa Itu Titik Curie (Curie Temperature)?", "id": "Suhu Hilangnya Sifat Feromagnetik." },
  { "en": "Apa Itu Bahan Magnet Lunak (Soft Magnetic)?", "id": "Mudah Dimagnetisasi, Didemagnetisasi (Inti Trafo)." },
  { "en": "Apa Itu Bahan Magnet Keras (Hard Magnetic)?", "id": "Sulit Dimagnetisasi, Didemagnetisasi (Magnet Permanen)." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Antarmuka Semikonduktor Tipe-P, Tipe-N." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Dasar Sambungan P-N." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Sekitar Sambungan Kurang Pembawa Muatan." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Dua Sambungan P-N (NPN Atau PNP)." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Mengontrol Arus Dengan Medan Listrik." },
  { "en": "Apa Itu Proses Fabrikasi (Fabrication Process) IC?", "id": "Langkah Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Litografi (Lithography)?", "id": "Proses Transfer Pola Ke Wafer." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Penghilangan Material Selektif." },
  { "en": "Apa Itu Deposisi (Deposition)?", "id": "Penambahan Lapisan Material Tipis." },
  { "en": "Apa Itu Doping (Doping)?", "id": "Penambahan Impuritas Kontrol Konduktivitas." },
  { "en": "Apa Itu Annealing (Annealing)?", "id": "Proses Pemanasan Perbaiki Struktur Kristal." },
  { "en": "Apa Itu Oksidasi (Oxidation)?", "id": "Pembentukan Lapisan Silikon Dioksida (SiO₂)." },
  { "en": "Apa Fungsi Lapisan SiO₂ (Silikon Dioksida)?", "id": "Sebagai Isolator Atau Masker Etsa." },
  { "en": "Apa Itu Implantasi Ion (Ion Implantation)?", "id": "Metode Doping Menembakkan Ion." },
  { "en": "Apa Itu Metalization (Metalisasi)?", "id": "Pembentukan Jalur Interkoneksi Logam." },
  { "en": "Logam Apa Umum Digunakan Metalization?", "id": "Aluminium Atau Tembaga." },
  { "en": "Apa Itu Planarisasi (Planarization) (CMP)?", "id": "Chemical Mechanical Polishing (Meratakan Permukaan)." },
  { "en": "Apa Kepanjangan CMP (Chemical Mechanical Polishing)?", "id": "Poles Kimia Mekanis." },
  { "en": "Apa Itu Yield (Yield) Fabrikasi?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Lingkungan Produksi Kontrol Partikel Ketat." },
  { "en": "Mengapa Perlu Ruang Bersih (Cleanroom)?", "id": "Partikel Debu Bisa Sebabkan Cacat IC." },
  { "en": "Apa Itu Pengujian Wafer (Wafer Testing)?", "id": "Pengujian Chip Saat Masih Di Wafer." },
  { "en": "Apa Itu Pengemasan (Packaging) IC?", "id": "Melindungi Chip, Menyediakan Koneksi Eksternal." },
  { "en": "Jenis Kemasan (Package) IC Through-Hole?", "id": "DIP (Dual In-line Package)." },
  { "en": "Jenis Kemasan (Package) IC Surface Mount?", "id": "SOIC, QFP, BGA." },
  { "en": "Apa Itu Wire Bonding (Ikatan Kawat)?", "id": "Menyambung Chip Ke Kaki Kemasan." },
  { "en": "Kawat Apa Digunakan Wire Bonding?", "id": "Kawat Emas Atau Aluminium Tipis." },
  { "en": "Apa Itu Flip Chip (Flip Chip)?", "id": "Metode Pemasangan Chip Langsung Ke Substrat." },
  { "en": "Apa Itu Ball Grid Array (BGA)?", "id": "Kemasan IC Kontak Bola Timah Bawah." },
  { "en": "Apa Keuntungan BGA (Ball Grid Array)?", "id": "Kepadatan Pin Tinggi, Kinerja Termal Baik." },
  { "en": "Apa Kerugian BGA (Ball Grid Array)?", "id": "Sulit Inspeksi Solderan, Rework Sulit." },
  { "en": "Apa Itu Pengujian Akhir (Final Test) IC?", "id": "Pengujian IC Setelah Dikemas." },
  { "en": "Apa Itu Binning (Binning) IC?", "id": "Klasifikasi IC Berdasarkan Kinerja Uji." },
  { "en": "Apa Itu Keandalan (Reliability) Semikonduktor?", "id": "Probabilitas IC Berfungsi Tanpa Gagal." },
  { "en": "Mekanisme Kegagalan (Failure Mechanism) IC Umum?", "id": "Elektromigrasi, Hot Carrier Injection, ESD." },
  { "en": "Apa Itu Elektromigrasi (Electromigration)?", "id": "Pergerakan Atom Logam Akibat Arus Tinggi." },
  { "en": "Apa Akibat Elektromigrasi (Electromigration)?", "id": "Jalur Logam Putus Atau Hubung Singkat." },
  { "en": "Apa Itu Hot Carrier Injection (HCI)?", "id": "Elektron Energi Tinggi Merusak Oksida Gerbang." },
  { "en": "Apa Kepanjangan HCI (Hot Carrier Injection)?", "id": "Injeksi Pembawa Panas." },
  { "en": "Apa Itu Time Dependent Dielectric Breakdown (TDDB)?", "id": "Kerusakan Isolator Oksida Seiring Waktu." },
  { "en": "Apa Kepanjangan TDDB (Time Dependent Dielectric Breakdown)?", "id": "Kerusakan Dielektrik Tergantung Waktu." },
  { "en": "Apa Itu Latch-Up (Latch-Up) CMOS?", "id": "Kondisi Hubung Singkat Parasitik Thyristor." },
  { "en": "Bagaimana Mencegah Latch-Up (Latch-Up)?", "id": "Desain Layout Hati-Hati (Guard Ring)." },
  { "en": "Apa Itu Radiasi (Radiation) Pengion?", "id": "Radiasi Energi Tinggi Rusak Semikonduktor." },
  { "en": "Contoh Radiasi (Radiation) Pengion?", "id": "Partikel Alfa, Sinar Gama, Neutron." },
  { "en": "Apa Itu Single Event Upset (SEU)?", "id": "Perubahan Keadaan Bit Memori Radiasi." },
  { "en": "Apa Kepanjangan SEU (Single Event Upset)?", "id": "Gangguan Kejadian Tunggal." },
  { "en": "Apa Itu Single Event Latch-Up (SEL)?", "id": "Latch-Up Dipicu Partikel Radiasi Tunggal." },
  { "en": "Apa Kepanjangan SEL (Single Event Latch-Up)?", "id": "Latch-Up Kejadian Tunggal." },
  { "en": "Apa Itu Radiation Hardening (Pengerasan Radiasi)?", "id": "Desain IC Tahan Efek Radiasi." },
  { "en": "Dimana IC Rad-Hard Digunakan?", "id": "Aplikasi Luar Angkasa, Nuklir." },
  { "en": "Apa Itu System-on-Chip (SoC)?", "id": "Integrasi Komponen Sistem Lengkap Chip Tunggal." },
  { "en": "Apa Komponen Umum Dalam SoC?", "id": "CPU, GPU, Memori, Peripheral I/O." },
  { "en": "Apa Itu Network-on-Chip (NoC)?", "id": "Infrastruktur Komunikasi Antar Blok SoC." },
  { "en": "Apa Kepanjangan NoC (Network-on-Chip)?", "id": "Jaringan Dalam Chip." },
  { "en": "Apa Itu IP (Intellectual Property) Core?", "id": "Blok Desain Logika Siap Pakai Lisensi." },
  { "en": "Jenis IP (Intellectual Property) Core?", "id": "Soft IP (Kode HDL), Hard IP (Layout Fisik)." },
  { "en": "Apa Itu Electronic Design Automation (EDA)?", "id": "Perangkat Lunak Desain Sirkuit Elektronik." },
  { "en": "Apa Kepanjangan EDA (Electronic Design Automation)?", "id": "Otomatisasi Desain Elektronik." },
  { "en": "Tahapan Desain IC (Integrated Circuit) Utama?", "id": "Desain Logika, Fisik, Verifikasi, Fabrikasi." },
  { "en": "Apa Itu Desain Logika (Logic Design)?", "id": "Implementasi Fungsi Menggunakan Gerbang Logika." },
  { "en": "Apa Itu Desain Fisik (Physical Design)?", "id": "Penempatan, Routing Komponen Layout Chip." },
  { "en": "Apa Itu Verifikasi (Verification) Desain?", "id": "Memastikan Desain Berfungsi Sesuai Spesifikasi." },
  { "en": "Metode Verifikasi (Verification) Desain?", "id": "Simulasi, Verifikasi Formal, Emulasi." },
  { "en": "Apa Itu Simulasi Logika (Logic Simulation)?", "id": "Memverifikasi Fungsionalitas Desain Logika." },
  { "en": "Apa Itu Verifikasi Formal (Formal Verification)?", "id": "Pembuktian Matematis Kebenaran Properti Desain." },
  { "en": "Apa Itu Emulasi Perangkat Keras (Hardware Emulation)?", "id": "Menjalankan Desain Hardware Khusus Cepat." },
  { "en": "Apa Itu Desain Untuk Pengujian (DFT)?", "id": "Design for Testability (DFT)." },
  { "en": "Apa Kepanjangan DFT (Design for Testability)?", "id": "Desain Untuk Kemudahan Pengujian." },
  { "en": "Mengapa DFT (Design for Testability) Penting?", "id": "Memudahkan Pengujian Chip Setelah Fabrikasi." },
  { "en": "Teknik DFT (Design for Testability) Umum?", "id": "Scan Chain, Built-In Self-Test (BIST)." },
  { "en": "Apa Itu Scan Chain (Rantai Pindai)?", "id": "Menghubungkan Flip-Flop Jadi Register Geser." },
  { "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Pengujian Internal Dalam Chip." },
  { "en": "Apa Kepanjangan BIST (Built-In Self-Test)?", "id": "Tes Diri Bawaan." },
  { "en": "Apa Itu Automatic Test Pattern Generation (ATPG)?", "id": "Pembuatan Pola Tes Otomatis." },
  { "en": "Apa Kepanjangan ATPG (Automatic Test Pattern Generation)?", "id": "Pembangkitan Pola Tes Otomatis." },
  { "en": "Apa Itu Boundary Scan (Pindaian Batas) (JTAG)?", "id": "Metode Tes Interkoneksi Antar IC." },
  { "en": "Apa Itu Yield (Yield) Manufaktur?", "id": "Persentase Produk Bagus Dari Total Produksi." },
  { "en": "Apa Itu Cacat (Defect) Manufaktur?", "id": "Ketidaksempurnaan Fisik Proses Produksi." },
  { "en": "Apa Itu Metrologi (Metrology) Semikonduktor?", "id": "Pengukuran Presisi Dimensi Proses Fabrikasi." },
  { "en": "Apa Itu Optika Komputasi (Computational Lithography)?", "id": "Teknik Komputasi Tingkatkan Resolusi Litografi." },
  { "en": "Apa Itu Pengemasan Lanjutan (Advanced Packaging)?", "id": "Teknologi Kemasan IC Inovatif." },
  { "en": "Contoh Pengemasan Lanjutan (Advanced Packaging)?", "id": "System-in-Package (SiP), 3D IC." },
  { "en": "Apa Itu System-in-Package (SiP)?", "id": "Integrasi Multi Chip Satu Kemasan." },
  { "en": "Apa Kepanjangan SiP (System-in-Package)?", "id": "Sistem Dalam Paket." },
  { "en": "Apa Itu 3D IC (Integrated Circuit) Integration?", "id": "Menumpuk Chip Secara Vertikal." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Menembus Chip Silikon." },
  { "en": "Apa Kepanjangan TSV (Through-Silicon Via)?", "id": "Via Melalui Silikon." },
  { "en": "Apa Itu Heterogeneous Integration (Integrasi Heterogen)?", "id": "Menggabungkan Chip Teknologi Berbeda Kemasan." },
  { "en": "Apa Itu Photonics (Fotonika) Silikon?", "id": "Integrasi Komponen Optik Chip Silikon." },
  { "en": "Aplikasi Photonics (Fotonika) Silikon?", "id": "Komunikasi Data Cepat, Sensor." },
  { "en": "Apa Itu Memristor (Memristor)?", "id": "Elemen Sirkuit Pasif Keempat (Teoretis)." },
  { "en": "Apa Sifat Utama Memristor (Memristor)?", "id": "Resistansi Tergantung Riwayat Arus/Tegangan." },
  { "en": "Potensi Aplikasi Memristor (Memristor)?", "id": "Memori Non-Volatil, Komputasi Neuromorfik." },
  { "en": "Apa Itu Komputasi Neuromorfik (Neuromorphic Computing)?", "id": "Komputasi Terinspirasi Struktur Otak." },
  { "en": "Apa Itu Spin-Transfer Torque (STT) MRAM?", "id": "Teknologi Memori MRAM Generasi Baru." },
  { "en": "Apa Kepanjangan STT (Spin-Transfer Torque)?", "id": "Torsi Transfer Spin." },
  { "en": "Apa Itu Phase Change Memory (PCM)?", "id": "Memori Non-Volatil Berbasis Perubahan Fasa Material." },
  { "en": "Apa Kepanjangan PCM (Phase Change Memory)?", "id": "Memori Perubahan Fasa." },
  { "en": "Material Apa Digunakan PCM (Phase Change Memory)?", "id": "Material Kalkogenida (Chalcogenide)." },
  { "en": "Apa Itu Resistive RAM (ReRAM/RRAM)?", "id": "Memori Non-Volatil Berbasis Perubahan Resistansi." },
  { "en": "Apa Kepanjangan ReRAM (Resistive Random Access Memory)?", "id": "Memori Akses Acak Resistif." },
  { "en": "Apa Itu Teknologi FinFET (Fin Field Effect Transistor)?", "id": "Struktur Transistor 3D Sirip (Fin)." },
  { "en": "Apa Keuntungan FinFET (Fin Field Effect Transistor)?", "id": "Kontrol Gerbang Lebih Baik, Arus Bocor Rendah." },
  { "en": "Apa Itu Teknologi Gate-All-Around (GAA)?", "id": "Struktur Transistor Gerbang Mengelilingi Saluran." },
  { "en": "Apa Kepanjangan GAA (Gate-All-Around)?", "id": "Gerbang Mengelilingi Semua." },
  { "en": "Apa Itu Extreme Ultraviolet (EUV) Lithography?", "id": "Litografi Gunakan Cahaya UV Ekstrem." },
  { "en": "Apa Kepanjangan EUV (Extreme Ultraviolet)?", "id": "Ultraviolet Ekstrem." },
  { "en": "Mengapa Perlu Litografi EUV (Extreme Ultraviolet)?", "id": "Mencetak Fitur Chip Sangat Kecil." },
  { "en": "Apa Itu More Moore (Lebih Moore)?", "id": "Melanjutkan Penskalaan Transistor Hukum Moore." },
  { "en": "Apa Itu More Than Moore (Lebih Dari Moore)?", "id": "Integrasi Fungsi Tambahan (Sensor, RF)." },
  { "en": "Apa Itu Sensor MEMS (Micro-Electro-Mechanical System)?", "id": "Sensor Miniatur Dibuat Proses Semikonduktor." },
  { "en": "Contoh Sensor MEMS (Micro-Electro-Mechanical System)?", "id": "Akselerometer, Giroskop, Mikrofon." },
  { "en": "Apa Itu Aktuator MEMS (Micro-Electro-Mechanical System)?", "id": "Aktuator Miniatur (Cermin Mikro DMD)." },
  { "en": "Apa Kepanjangan DMD (Digital Micromirror Device)?", "id": "Perangkat Mikrocermin Digital." },
  { "en": "Dimana DMD (Digital Micromirror Device) Digunakan?", "id": "Proyektor DLP (Digital Light Processing)." },
  { "en": "Apa Itu BioMEMS (Biomedical MEMS)?", "id": "Aplikasi MEMS Bidang Biomedis." },
  { "en": "Contoh Aplikasi BioMEMS (Biomedical MEMS)?", "id": "Lab-on-a-Chip, Sensor Diagnostik." },
  { "en": "Apa Itu Lab-on-a-Chip (Lab Dalam Chip)?", "id": "Integrasi Fungsi Laboratorium Chip Kecil." },
  { "en": "Apa Itu Elektronika Organik (Organic Electronics)?", "id": "Elektronika Berbasis Material Organik." },
  { "en": "Contoh Perangkat Elektronika Organik?", "id": "OLED (Organic LED), OFET (Organic FET)." },
  { "en": "Apa Kepanjangan OFET (Organic Field Effect Transistor)?", "id": "Transistor Efek Medan Organik." },
  { "en": "Apa Keunggulan Elektronika Organik?", "id": "Fleksibel, Ringan, Biaya Rendah Potensial." },
  { "en": "Apa Kerugian Elektronika Organik?", "id": "Umur Lebih Pendek, Kinerja Rendah." },
  { "en": "Apa Itu Elektronika Cetak (Printed Electronics)?", "id": "Mencetak Sirkuit Elektronik (Tinta Konduktif)." },
  { "en": "Apa Itu Elektronika Fleksibel (Flexible Electronics)?", "id": "Elektronika Dibangun Substrat Fleksibel." },
  { "en": "Apa Itu Elektronika Yang Dapat Dipakai (Wearable Electronics)?", "id": "Elektronika Terintegrasi Pakaian/Aksesoris." },
  { "en": "Contoh Elektronika Yang Dapat Dipakai?", "id": "Smartwatch (Jam Cerdas), Fitness Tracker." },
  { "en": "Apa Itu Sensor Biometrik (Biometric Sensor)?", "id": "Mengukur Ciri Fisiologis/Perilaku Manusia." },
  { "en": "Contoh Sensor Biometrik (Biometric Sensor)?", "id": "Sensor Sidik Jari, Detak Jantung." },
  { "en": "Apa Itu Sensor EKG/ECG (Electrocardiogram)?", "id": "Mengukur Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Sensor EEG (Electroencephalogram)?", "id": "Mengukur Aktivitas Listrik Otak." },
  { "en": "Apa Itu Sensor EMG (Electromyogram)?", "id": "Mengukur Aktivitas Listrik Otot." },
  { "en": "Apa Itu Sensor SpO2 (Oksimetri Nadi)?", "id": "Mengukur Saturasi Oksigen Darah." },
  { "en": "Bagaimana Sensor SpO2 (Oksimetri Nadi) Bekerja?", "id": "Mengukur Serapan Cahaya Merah, Inframerah." },
  { "en": "Apa Itu Bahan Pintar (Smart Material)?", "id": "Material Berubah Sifat Respons Stimulus." },
  { "en": "Contoh Stimulus Bahan Pintar?", "id": "Suhu, Cahaya, Medan Listrik/Magnet." },
  { "en": "Apa Itu Paduan Memori Bentuk (SMA)?", "id": "Kembali Bentuk Asli Saat Dipanaskan." },
  { "en": "Apa Itu Bahan Piezoelektrik (Piezoelectric Material)?", "id": "Hasilkan Listrik Stres Mekanis." },
  { "en": "Apa Itu Bahan Elektrokromik (Electrochromic Material)?", "id": "Berubah Warna Diberi Tegangan Listrik." },
  { "en": "Apa Itu Bahan Termokromik (Thermochromic Material)?", "id": "Berubah Warna Akibat Perubahan Suhu." },
  { "en": "Apa Itu Bahan Fotokromik (Photochromic Material)?", "id": "Berubah Warna Akibat Paparan Cahaya." },
  { "en": "Dimana Bahan Fotokromik (Photochromic Material) Digunakan?", "id": "Lensa Kacamata Transisi." },
  { "en": "Apa Itu Cairan Magnetoreologi (MR Fluid)?", "id": "Viskositas Berubah Medan Magnet." },
  { "en": "Apa Kepanjangan MR Fluid (Magnetorheological Fluid)?", "id": "Cairan Magnetoreologi." },
  { "en": "Aplikasi Cairan Magnetoreologi (MR Fluid)?", "id": "Peredam Kejut Adaptif, Kopling." },
  { "en": "Apa Itu Cairan Elektroreologi (ER Fluid)?", "id": "Viskositas Berubah Medan Listrik." },
  { "en": "Apa Kepanjangan ER Fluid (Electrorheological Fluid)?", "id": "Cairan Elektroreologi." },
  { "en": "Apa Itu Polimer Penghantar Listrik (Conducting Polymer)?", "id": "Polimer Dapat Menghantarkan Listrik." },
  { "en": "Apa Itu Gelombang Akustik Permukaan (SAW)?", "id": "Surface Acoustic Wave (SAW)." },
  { "en": "Apa Kepanjangan SAW (Surface Acoustic Wave)?", "id": "Gelombang Akustik Permukaan." },
  { "en": "Dimana Perangkat SAW (Surface Acoustic Wave) Digunakan?", "id": "Filter RF, Sensor." },
  { "en": "Apa Itu Gelombang Akustik Bulk (BAW)?", "id": "Bulk Acoustic Wave (BAW)." },
  { "en": "Apa Kepanjangan BAW (Bulk Acoustic Wave)?", "id": "Gelombang Akustik Bulk." },
  { "en": "Dimana Perangkat BAW (Bulk Acoustic Wave) Digunakan?", "id": "Filter RF Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Metamaterial (Metamaterial)?", "id": "Material Buatan Struktur Sub-Lambda." },
  { "en": "Apa Sifat Unik Metamaterial (Metamaterial)?", "id": "Indeks Bias Negatif, Permeabilitas Negatif." },
  { "en": "Aplikasi Potensial Metamaterial (Metamaterial)?", "id": "Antena Superdirektif, Jubah Tembus Pandang." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Ilmu Teknologi Hasilkan, Manipulasi Foton." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Perangkat Konversi Cahaya-Listrik." },
  { "en": "Apa Itu Kristal Fotonik (Photonic Crystal)?", "id": "Struktur Periodik Kontrol Aliran Cahaya." },
  { "en": "Apa Itu Celah Pita Fotonik (Photonic Bandgap)?", "id": "Rentang Frekuensi Cahaya Dilarang Lewat." },
  { "en": "Apa Itu Sirkuit Terpadu Fotonik (PIC)?", "id": "Photonic Integrated Circuit (PIC)." },
  { "en": "Apa Kepanjangan PIC (Photonic Integrated Circuit)?", "id": "Sirkuit Terpadu Fotonik." },
  { "en": "Apa Fungsi PIC (Photonic Integrated Circuit)?", "id": "Integrasi Komponen Optik Chip Tunggal." },
  { "en": "Apa Itu Fotonika Silikon (Silicon Photonics)?", "id": "Implementasi PIC (Photonic Integrated Circuit) Platform Silikon." },
  { "en": "Keuntungan Fotonika Silikon (Silicon Photonics)?", "id": "Integrasi Elektronik, Manufaktur Skala Besar." },
  { "en": "Apa Itu Modulator Optik (Optical Modulator)?", "id": "Mengubah Sifat Cahaya Sinyal Listrik." },
  { "en": "Jenis Modulator Optik (Optical Modulator)?", "id": "Elektro-Optik, Akusto-Optik, Termo-Optik." },
  { "en": "Apa Itu Modulator Mach-Zehnder (Mach-Zehnder Modulator)?", "id": "Modulator Optik Berbasis Interferometer." },
  { "en": "Apa Itu Efek Elektro-Optik (Electro-Optic Effect)?", "id": "Perubahan Indeks Bias Medan Listrik." },
  { "en": "Apa Itu Efek Akusto-Optik (Acousto-Optic Effect)?", "id": "Interaksi Cahaya Gelombang Akustik." },
  { "en": "Apa Itu Detektor Foto (Photodetector)?", "id": "Sensor Mengubah Cahaya Jadi Sinyal Listrik." },
  { "en": "Jenis Detektor Foto (Photodetector) Utama?", "id": "Fotodioda (PIN, APD), Fototransistor." },
  { "en": "Apa Itu Efisiensi Kuantum (Quantum Efficiency) Detektor?", "id": "Rasio Elektron Dihasilkan Per Foton Masuk." },
  { "en": "Apa Itu Responsivitas (Responsivity) Detektor?", "id": "Arus Output Per Daya Optik Input." },
  { "en": "Apa Itu Arus Gelap (Dark Current)?", "id": "Arus Dihasilkan Detektor Tanpa Cahaya." },
  { "en": "Apa Itu Noise (Derau) Detektor Foto?", "id": "Shot Noise, Thermal Noise, Dark Current Noise." },
  { "en": "Apa Itu Waktu Respon (Response Time) Detektor?", "id": "Kecepatan Detektor Merespon Perubahan Cahaya." },
  { "en": "Apa Itu Tampilan Kristal Cair (LCD)?", "id": "Liquid Crystal Display (LCD)." },
  { "en": "Prinsip Dasar LCD (Liquid Crystal Display)?", "id": "Kristal Cair Kontrol Polarisasi Cahaya." },
  { "en": "Apa Itu Lampu Latar (Backlight) LCD?", "id": "Sumber Cahaya Belakang Panel LCD." },
  { "en": "Jenis Lampu Latar (Backlight) Umum?", "id": "CCFL (Cold Cathode Fluorescent Lamp) (Lama), LED (Light Emitting Diode)." },
  { "en": "Apa Itu Tampilan LED (Light Emitting Diode)?", "id": "Tiap Piksel Adalah LED Kecil." },
  { "en": "Apa Beda LCD (Liquid Crystal Display) LED (Light Emitting Diode) Dan TV LED?", "id": "TV LED Gunakan Backlight LED Panel LCD." },
  { "en": "Apa Itu Tampilan OLED (Organic LED)?", "id": "Tampilan Berbasis Dioda Pemancar Cahaya Organik." },
  { "en": "Keunggulan OLED (Organic LED) Dibanding LCD?", "id": "Kontras Tak Hingga, Hitam Sempurna, Sudut Pandang." },
  { "en": "Apa Itu Burn-In (Burn-In) OLED?", "id": "Degradasi Permanen Piksel Akibat Gambar Statis." },
  { "en": "Apa Itu Tampilan Quantum Dot (QD)?", "id": "LCD (Liquid Crystal Display) Gunakan Lapisan Titik Kuantum." },
  { "en": "Manfaat Lapisan Quantum Dot (QD)?", "id": "Meningkatkan Rentang Warna, Kecerahan." },
  { "en": "Apa Itu Tampilan MicroLED (MicroLED)?", "id": "Tampilan Mirip OLED (Organic LED) Tapi Anorganik." },
  { "en": "Keunggulan MicroLED (MicroLED)?", "id": "Kecerahan Tinggi, Awet, Efisien." },
  { "en": "Apa Itu Tampilan E-Ink (E-Paper)?", "id": "Tampilan Bistabil Reflektif (Sangat Hemat Daya)." },
  { "en": "Apa Itu Bistabil (Bistable)?", "id": "Tetap Tampilkan Gambar Tanpa Daya." },
  { "en": "Dimana E-Ink (E-Paper) Digunakan?", "id": "E-Reader (Pembaca Buku Elektronik), Label Harga." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Resistif?", "id": "Deteksi Sentuhan Tekanan Dua Lapisan." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Kapasitif?", "id": "Deteksi Sentuhan Perubahan Medan Kapasitif." },
  { "en": "Jenis Layar Sentuh (Touchscreen) Mana Lebih Umum Smartphone?", "id": "Layar Sentuh Kapasitif (Capacitive Touchscreen)." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Proyeksi Kapasitif?", "id": "Projected Capacitive (PCAP) Umum Digunakan." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Gelombang Akustik Permukaan (SAW)?", "id": "Deteksi Sentuhan Gangguan Gelombang Akustik." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Inframerah (Infrared)?", "id": "Deteksi Sentuhan Pemutusan Sinar Inframerah." },
  { "en": "Apa Itu Haptics (Haptics)?", "id": "Teknologi Umpan Balik Sentuhan (Getaran)." },
  { "en": "Apa Itu Aktuator Haptik (Haptic Actuator)?", "id": "Komponen Penghasil Efek Getaran/Sentuhan." },
  { "en": "Jenis Aktuator Haptik (Haptic Actuator)?", "id": "ERM (Eccentric Rotating Mass), LRA (Linear Resonant Actuator)." },
  { "en": "Apa Kepanjangan ERM (Eccentric Rotating Mass)?", "id": "Massa Berputar Eksentrik." },
  { "en": "Apa Kepanjangan LRA (Linear Resonant Actuator)?", "id": "Aktuator Resonan Linear." },
  { "en": "Mana Umpan Balik Haptik Lebih Presisi?", "id": "LRA (Linear Resonant Actuator)." },
  { "en": "Apa Itu Pengenalan Suara (Speech Recognition)?", "id": "Komputer Mengerti Bahasa Lisan Manusia." },
  { "en": "Apa Itu Sintesis Suara (Speech Synthesis)?", "id": "Komputer Menghasilkan Suara Mirip Manusia." },
  { "en": "Nama Lain Sintesis Suara (Speech Synthesis)?", "id": "Text-to-Speech (TTS)." },
  { "en": "Apa Itu Akustik (Acoustics)?", "id": "Ilmu Tentang Suara." },
  { "en": "Apa Itu Psikoakustik (Psychoacoustics)?", "id": "Studi Persepsi Suara Manusia." },
  { "en": "Apa Itu Pembatalan Derau Aktif (ANC)?", "id": "Active Noise Cancellation (ANC)." },
  { "en": "Apa Kepanjangan ANC (Active Noise Cancellation)?", "id": "Pembatalan Derau Aktif." },
  { "en": "Bagaimana ANC (Active Noise Cancellation) Bekerja?", "id": "Menghasilkan Gelombang Anti-Fasa Derau." },
  { "en": "Apa Itu Mikrofon MEMS (Micro-Electro-Mechanical System)?", "id": "Mikrofon Miniatur Dibuat Proses IC." },
  { "en": "Apa Itu Kodek Audio (Audio Codec)?", "id": "Coder/Decoder Kompresi Dekompresi Audio." },
  { "en": "Apa Itu Audio Resolusi Tinggi (Hi-Res Audio)?", "id": "Audio Kualitas Lebih Tinggi Dari CD." },
  { "en": "Apa Itu Osiloskop Penyimpanan Digital (DSO)?", "id": "Osiloskop Menyimpan Bentuk Gelombang Digital." },
  { "en": "Apa Keuntungan DSO Dibanding Analog?", "id": "Penyimpanan, Analisis, Pengukuran Otomatis." },
  { "en": "Apa Itu Memori Akuisisi (Acquisition Memory) DSO?", "id": "Kedalaman Memori Simpan Sampel Gelombang." },
  { "en": "Apa Itu Interpolasi (Interpolation) Tampilan DSO?", "id": "Menghubungkan Titik Sampel Di Layar." },
  { "en": "Jenis Interpolasi (Interpolation) DSO?", "id": "Linear Dan Sin(x)/x." },
  { "en": "Apa Itu Mode Roll (Roll Mode) Osiloskop?", "id": "Tampilan Gelombang Bergerak Lambat Terus Menerus." },
  { "en": "Apa Itu Pengukuran Otomatis (Automatic Measurement) Osiloskop?", "id": "Mengukur Parameter (Vpp, Frekuensi) Otomatis." },
  { "en": "Apa Itu Fungsi Matematika (Math Function) Osiloskop?", "id": "Operasi Matematis Antar Kanal (Tambah, FFT)." },
  { "en": "Apa Itu Probe (Probe) Arus?", "id": "Probe Mengukur Arus Tanpa Putus Rangkaian." },
  { "en": "Prinsip Kerja Probe (Probe) Arus?", "id": "Efek Hall Atau Transformator Arus." },
  { "en": "Apa Itu Probe (Probe) Diferensial?", "id": "Mengukur Perbedaan Tegangan Dua Titik Apung." },
  { "en": "Kapan Probe (Probe) Diferensial Digunakan?", "id": "Pengukuran Tegangan Tidak Referensi Ground." },
  { "en": "Apa Itu Atenuasi (Attenuation) Probe?", "id": "Faktor Pembagi Tegangan Probe (1X, 10X)." },
  { "en": "Mengapa Gunakan Probe (Probe) 10X?", "id": "Impedansi Input Lebih Tinggi, Bandwidth Lebar." },
  { "en": "Apa Itu Kompensasi Probe (Probe Compensation)?", "id": "Menyesuaikan Kapasitansi Probe Osiloskop." },
  { "en": "Bagaimana Melakukan Kompensasi Probe (Probe Compensation)?", "id": "Gunakan Sinyal Kotak Referensi Osiloskop." },
  { "en": "Apa Itu Ground Lead (Kabel Ground) Probe?", "id": "Koneksi Referensi Ground Ke Rangkaian." },
  { "en": "Mengapa Ground Lead (Kabel Ground) Harus Pendek?", "id": "Mengurangi Induktansi Loop Ground (Noise)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Probe?", "id": "Frekuensi Maksimum Dapat Dilewatkan Probe." },
  { "en": "Apa Itu Tegangan Input Maksimum Probe?", "id": "Batas Tegangan Aman Untuk Probe." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Alat Akuisisi, Analisis Sinyal Digital." },
  { "en": "Apa Beda Osiloskop Dan Penganalisis Logika?", "id": "Osiloskop (Analog), Penganalisis (Logika Digital)." },
  { "en": "Apa Itu State Analysis (Analisis Keadaan)?", "id": "Menampilkan Data Sinkron Clock Sistem." },
  { "en": "Apa Itu Timing Analysis (Analisis Waktu)?", "id": "Menampilkan Data Asinkron Resolusi Waktu." },
  { "en": "Apa Itu Dekoder Protokol (Protocol Decoder)?", "id": "Fitur Penganalisis Logika Interpretasi Bus Serial." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Mengukur Distribusi Daya Sinyal Frekuensi." },
  { "en": "Apa Sumbu Horizontal Penganalisis Spektrum?", "id": "Frekuensi (Frequency)." },
  { "en": "Apa Sumbu Vertikal Penganalisis Spektrum?", "id": "Amplitudo Atau Daya (dBm)." },
  { "en": "Apa Itu Resolution Bandwidth (RBW)?", "id": "Lebar Pita Filter Penganalisis Spektrum." },
  { "en": "Apa Kepanjangan RBW (Resolution Bandwidth)?", "id": "Lebar Pita Resolusi." },
  { "en": "RBW (Resolution Bandwidth) Sempit Memberi Apa?", "id": "Resolusi Frekuensi Lebih Baik." },
  { "en": "Apa Itu Video Bandwidth (VBW)?", "id": "Lebar Pita Filter Detektor Video." },
  { "en": "Apa Kepanjangan VBW (Video Bandwidth)?", "id": "Lebar Pita Video." },
  { "en": "Apa Fungsi VBW (Video Bandwidth) Lebih Kecil?", "id": "Mengurangi Noise Tampilan (Averaging)." },
  { "en": "Apa Itu Noise Floor (Dasar Derau)?", "id": "Level Derau Internal Penganalisis Spektrum." },
  { "en": "Apa Itu Span (Rentang) Penganalisis Spektrum?", "id": "Rentang Frekuensi Ditampilkan Di Layar." },
  { "en": "Apa Itu Center Frequency (Frekuensi Tengah)?", "id": "Frekuensi Pusat Tampilan Penganalisis Spektrum." },
  { "en": "Apa Itu Tracking Generator (Generator Pelacak)?", "id": "Sumber Sinyal Sinkron Penganalisis Spektrum." },
  { "en": "Apa Fungsi Tracking Generator (Generator Pelacak)?", "id": "Mengukur Respon Frekuensi Komponen Pasif." },
  { "en": "Apa Itu Network Analyzer (Penganalisis Jaringan)?", "id": "Mengukur Parameter Jaringan (S-Parameter)." },
  { "en": "Apa Itu Vector Network Analyzer (VNA)?", "id": "Mengukur Magnitudo Dan Fasa (Kompleks)." },
  { "en": "Apa Itu Scalar Network Analyzer (SNA)?", "id": "Hanya Mengukur Magnitudo." },
  { "en": "Apa Itu Kalibrasi (Calibration) VNA?", "id": "Menghilangkan Efek Kesalahan Sistem Pengukuran." },
  { "en": "Standar Kalibrasi (Calibration Standard) VNA Umum?", "id": "Short, Open, Load, Thru (SOLT)." },
  { "en": "Apa Kepanjangan SOLT (Short Open Load Thru)?", "id": "Hubung Singkat, Terbuka, Beban, Lewat." },
  { "en": "Apa Itu Power Meter (Pengukur Daya) RF?", "id": "Mengukur Daya Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Sensor Daya (Power Sensor)?", "id": "Bagian Detektor Pengukur Daya RF." },
  { "en": "Jenis Sensor Daya (Power Sensor)?", "id": "Termokopel, Dioda, Termistor." },
  { "en": "Apa Itu Frequency Counter (Pencacah Frekuensi)?", "id": "Mengukur Frekuensi Sinyal Listrik Presisi." },
  { "en": "Apa Itu Time Base (Basis Waktu) Counter?", "id": "Osilator Referensi Internal Pencacah." },
  { "en": "Basis Waktu (Time Base) Apa Paling Stabil?", "id": "Osilator Kristal Kontrol Oven (OCXO)." },
  { "en": "Apa Itu Pengukuran Periode (Period Measurement)?", "id": "Mengukur Waktu Satu Siklus Sinyal." },
  { "en": "Apa Itu Pengukuran Interval Waktu (Time Interval)?", "id": "Mengukur Waktu Antara Dua Kejadian." },
  { "en": "Apa Itu LCR Meter (Pengukur LCR)?", "id": "Mengukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Bagaimana LCR Meter (Pengukur LCR) Bekerja?", "id": "Mengukur Impedansi Kompleks Frekuensi Tertentu." },
  { "en": "Apa Itu Faktor Disipasi (Dissipation Factor) (D)?", "id": "Ukuran Rugi Kapasitor (Diukur LCR)." },
  { "en": "Apa Itu Faktor Kualitas (Quality Factor) (Q)?", "id": "Ukuran Kualitas Induktor (Diukur LCR)." },
  { "en": "Apa Itu Hipot Tester (Penguji Hipot)?", "id": "Pengujian Kekuatan Isolasi Tegangan Tinggi." },
  { "en": "Apa Kepanjangan Hipot (High Potential)?", "id": "Potensial Tinggi." },
  { "en": "Apa Tujuan Tes Hipot (Hipot Test)?", "id": "Memastikan Isolasi Aman Tidak Tembus." },
  { "en": "Apa Itu Insulation Resistance Tester (Penguji Resistansi Isolasi)?", "id": "Mengukur Resistansi Isolasi (Megohmmeter)." },
  { "en": "Apa Tegangan Uji Insulation Tester?", "id": "Tegangan DC Tinggi (Ratusan/Ribuan Volt)." },
  { "en": "Apa Itu Earth Ground Tester (Penguji Bumi)?", "id": "Mengukur Resistansi Sistem Pembumian." },
  { "en": "Metode Pengujian Bumi (Earth Testing) Umum?", "id": "Metode Jatuh Potensial (Fall-of-Potential)." },
  { "en": "Apa Itu Beban Elektronik (Electronic Load)?", "id": "Perangkat Simulasi Beban Listrik Terprogram." },
  { "en": "Mode Operasi Beban Elektronik (Electronic Load)?", "id": "Arus Konstan (CC), Tegangan (CV), Resistansi (CR)." },
  { "en": "Apa Itu Sumber AC (AC Source) Terprogram?", "id": "Sumber Tegangan/Arus AC Dapat Diatur." },
  { "en": "Apa Itu Penganalisis Daya (Power Analyzer)?", "id": "Mengukur Parameter Daya Listrik Detail." },
  { "en": "Parameter Apa Diukur Penganalisis Daya?", "id": "Vrms, Irms, Watt, VA, VAR, PF, Harmonisa." },
  { "en": "Apa Itu Termografi Inframerah (Infrared Thermography)?", "id": "Pengambilan Gambar Distribusi Suhu Termal." },
  { "en": "Aplikasi Termografi (Thermography) Listrik?", "id": "Deteksi Koneksi Longgar, Overheating." },
  { "en": "Apa Itu Pengujian Ultrasonik (Ultrasonic Testing) Listrik?", "id": "Deteksi Corona, Arcing Frekuensi Tinggi." },
  { "en": "Apa Itu Partial Discharge (PD) Testing?", "id": "Deteksi Pelepasan Sebagian Isolasi HV." },
  { "en": "Apa Kepanjangan PD (Partial Discharge)?", "id": "Pelepasan Sebagian." },
  { "en": "Mengapa Pengujian PD (Partial Discharge) Penting?", "id": "Indikator Awal Kerusakan Isolasi HV." },
  { "en": "Apa Itu Automatic Test Equipment (ATE)?", "id": "Peralatan Pengujian Otomatis Produksi." },
  { "en": "Apa Kepanjangan ATE (Automatic Test Equipment)?", "id": "Peralatan Tes Otomatis." },
  { "en": "Apa Itu In-Circuit Test (ICT)?", "id": "Pengujian Komponen PCB Setelah Perakitan." },
  { "en": "Apa Kepanjangan ICT (In-Circuit Test)?", "id": "Tes Dalam Sirkuit." },
  { "en": "Apa Itu Functional Circuit Test (FCT)?", "id": "Pengujian Fungsi Keseluruhan PCB/Produk." },
  { "en": "Apa Kepanjangan FCT (Functional Circuit Test)?", "id": "Tes Sirkuit Fungsional." },
  { "en": "Apa Itu Bed of Nails Fixture (Fixture Ranjang Paku)?", "id": "Fixture ICT Kontak Pin Test Point." },
  { "en": "Apa Itu Flying Probe Tester (Penguji Probe Terbang)?", "id": "Pengujian PCB Tanpa Fixture (Probe Bergerak)." },
  { "en": "Apa Keuntungan Flying Probe Tester?", "id": "Fleksibel Untuk Prototipe, Volume Kecil." },
  { "en": "Apa Itu Boundary Scan (JTAG) Testing?", "id": "Pengujian Interkoneksi IC Menggunakan JTAG." },
  { "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Pengujian Internal Dalam IC." },
  { "en": "Apa Manfaat BIST (Built-In Self-Test)?", "id": "Memudahkan Pengujian Chip Kompleks." },
  { "en": "Apa Itu Design for Testability (DFT)?", "id": "Prinsip Desain Memudahkan Pengujian." },
  { "en": "Apa Itu Test Coverage (Cakupan Tes)?", "id": "Persentase Potensi Kegagalan Terdeteksi Tes." },
  { "en": "Apa Itu Yield (Hasil) Pengujian?", "id": "Persentase Unit Lulus Pengujian." },
  { "en": "Apa Itu Kalibrasi (Calibration) Alat Ukur?", "id": "Memastikan Akurasi Alat Sesuai Standar." },
  { "en": "Mengapa Kalibrasi (Calibration) Penting?", "id": "Menjamin Kepercayaan Hasil Pengukuran." },
  { "en": "Apa Itu Interval Kalibrasi (Calibration Interval)?", "id": "Periode Waktu Antar Kalibrasi." },
  { "en": "Apa Itu Ketertelusuran (Traceability) Kalibrasi?", "id": "Hubungan Ke Standar Pengukuran Nasional/Internasional." },
  { "en": "Apa Itu Standar Primer (Primary Standard)?", "id": "Standar Referensi Tertinggi Akurasinya." },
  { "en": "Apa Itu Standar Transfer (Transfer Standard)?", "id": "Standar Digunakan Transfer Akurasi." },
  { "en": "Apa Itu Standar Kerja (Working Standard)?", "id": "Standar Digunakan Kalibrasi Harian." },
  { "en": "Apa Itu Sertifikat Kalibrasi (Calibration Certificate)?", "id": "Dokumen Bukti Kalibrasi, Hasilnya." },
  { "en": "Apa Itu Akreditasi (Accreditation) Laboratorium Kalibrasi?", "id": "Pengakuan Kompetensi Laboratorium Oleh Badan." }




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
