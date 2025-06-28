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


  { "en": "Sensor untuk mengukur suhu?", "id": "Sensor Suhu (Termistor/Termokopel/RTD)." },
  { "en": "Sensor untuk mendeteksi cahaya?", "id": "Sensor Cahaya (LDR/Fotodioda)." },
  { "en": "Sensor untuk mengukur jarak tanpa sentuhan?", "id": "Sensor Jarak (Ultrasonik/Inframerah/Laser)." },
  { "en": "Sensor untuk mendeteksi adanya getaran?", "id": "Sensor Getar / Vibration Sensor." },
  { "en": "Sensor untuk mengukur percepatan atau guncangan?", "id": "Akselerometer / Accelerometer." },
  { "en": "Sensor untuk mengubah suara menjadi sinyal listrik?", "id": "Mikrofon / Microphone." },
  { "en": "Sensor untuk mendeteksi objek logam dari dekat?", "id": "Sensor Proximity Induktif." },
  { "en": "Sensor untuk mengukur kelembapan udara?", "id": "Sensor Kelembapan / Humidity Sensor." },
  { "en": "Sensor untuk mengukur tekanan udara atau cairan?", "id": "Sensor Tekanan / Pressure Sensor." },
  { "en": "Sensor untuk mendeteksi adanya medan magnet?", "id": "Sensor Efek Hall / Hall Effect Sensor." },
  { "en": "Sensor yang resistansinya berubah karena cahaya?", "id": "LDR (Light Dependent Resistor)." },
  { "en": "Sensor untuk mengukur orientasi dan kecepatan sudut?", "id": "Giroskop / Gyroscope." },
  { "en": "Sensor untuk mendeteksi panas tubuh manusia?", "id": "Sensor PIR (Passive Infrared)." },
  { "en": "Sensor untuk mengukur berat atau gaya tekan?", "id": "Load Cell." },
  { "en": "Sensor untuk mendeteksi adanya asap?", "id": "Sensor Asap / Smoke Detector." },
  { "en": "Sensor untuk mendeteksi kebocoran gas?", "id": "Sensor Gas." },
  { "en": "Sensor untuk mengukur laju aliran cairan?", "id": "Sensor Aliran / Flow Sensor." },
  { "en": "Sensor yang bekerja seperti kompas digital?", "id": "Magnetometer." },
  { "en": "Sensor untuk mendeteksi tetesan air hujan?", "id": "Sensor Hujan / Rain Sensor." },
  { "en": "Sensor untuk mengukur kelembapan dalam tanah?", "id": "Sensor Kelembapan Tanah / Soil Moisture." },
  { "en": "Sensor suhu dari dua jenis logam berbeda?", "id": "Termokopel / Thermocouple." },
  { "en": "Sensor untuk mendeteksi sentuhan pada layar?", "id": "Sensor Sentuh / Touch Sensor." },
  { "en": "Sensor untuk mengidentifikasi sidik jari?", "id": "Sensor Sidik Jari / Fingerprint." },
  { "en": "Sensor untuk mendeteksi warna suatu objek?", "id": "Sensor Warna / Color Sensor." },
  { "en": "Sensor untuk mengukur detak jantung?", "id": "Sensor Detak Jantung / Heart Rate." },
  { "en": "Sensor jarak yang menggunakan pantulan suara?", "id": "Sensor Ultrasonik." },
  { "en": "Sensor yang berfungsi sebagai saklar batas mekanis?", "id": "Limit Switch." },
  { "en": "Sensor yang mendeteksi berbagai jenis material dari dekat?", "id": "Sensor Proximity Kapasitif." },
  { "en": "Sensor suhu dari bahan semikonduktor?", "id": "Termistor (NTC/PTC)." },
  { "en": "Sensor untuk menangkap gambar digital?", "id": "Sensor Gambar (CMOS/CCD)." },
  { "en": "Sensor untuk mengukur regangan pada benda padat?", "id": "Strain Gauge." },
  { "en": "Sensor untuk mendeteksi tingkat keasaman (pH)?", "id": "Sensor pH." },
  { "en": "Sensor yang mendeteksi kemiringan suatu benda?", "id": "Tilt Sensor / Sensor Kemiringan." },
  { "en": "Sensor untuk mendeteksi nyala api?", "id": "Sensor Api / Flame Sensor." },
  { "en": "Sensor untuk mengukur posisi sudut putaran?", "id": "Encoder." },
  { "en": "Sensor untuk mengukur kadar alkohol di udara?", "id": "Sensor Alkohol." },
  { "en": "Sensor yang mendeteksi ketinggian permukaan air?", "id": "Sensor Level Air / Water Level Sensor." },
  { "en": "Sensor untuk mendeteksi sinar Ultraviolet (UV)?", "id": "Sensor UV." },
  { "en": "Sensor cahaya yang menghasilkan arus listrik?", "id": "Fotodioda / Photodiode." },
  { "en": "Sensor yang bekerja seperti saklar pelampung?", "id": "Float Switch." },
  { "en": "Sensor yang menghasilkan tegangan saat ditekan/ditekuk?", "id": "Sensor Piezoelektrik." },
  { "en": "Sensor untuk mengukur kualitas udara?", "id": "Sensor Kualitas Udara / Air Quality Sensor." },
  { "en": "Sensor yang mendeteksi arus listrik pada kabel?", "id": "Sensor Arus / Current Sensor." },
  { "en": "Sensor suhu yang sangat akurat dari platinum?", "id": "RTD (Resistance Temperature Detector)." },
  { "en": "Sensor jarak yang menggunakan sinar inframerah?", "id": "Sensor Jarak Inframerah (IR)." },
  { "en": "Sensor untuk mendeteksi Karbon Monoksida (CO)?", "id": "Sensor CO." },
  { "en": "Sensor yang mengukur kekeruhan air?", "id": "Sensor Kekeruhan / Turbidity Sensor." },
  { "en": "Sensor yang menggabungkan Akselerometer dan Giroskop?", "id": "IMU (Inertial Measurement Unit)." },
  { "en": "Sensor untuk mendeteksi posisi dengan tuas fleksibel?", "id": "Flex Sensor." },
  { "en": "Sensor suhu tanpa sentuhan?", "id": "Pirometer / Thermometer Inframerah." },
  { "en": "Sensor cahaya yang diperkuat oleh transistor?", "id": "Fototransistor / Phototransistor." },
  { "en": "Sensor untuk mengukur medan magnet bumi?", "id": "Magnetometer." },
  { "en": "Sensor yang mendeteksi keberadaan objek transparan?", "id": "Sensor Proximity Kapasitif." },
  { "en": "Sensor untuk mengukur rotasi layar ponsel?", "id": "Akselerometer / Giroskop." },
  { "en": "Sensor yang mendeteksi ketukan atau gukanan ringan?", "id": "Knock Sensor / Sensor Ketukan." },
  { "en": "Sensor yang dapat membaca barcode?", "id": "Barcode Scanner." },
  { "en": "Sensor untuk mendeteksi garis (misal: robot line follower)?", "id": "Sensor Garis / Line Sensor." },
  { "en": "Sensor untuk mengukur tegangan listrik?", "id": "Sensor Tegangan / Voltage Sensor." },
  { "en": "Sensor yang mendeteksi suara pada frekuensi tertentu?", "id": "Sound Detection Module." },
  { "en": "Sensor untuk mengukur posisi absolut suatu putaran?", "id": "Absolute Encoder." },
  { "en": "Sensor untuk mengukur perubahan posisi putaran?", "id": "Incremental Encoder." },
  { "en": "Sensor yang mendeteksi medan magnet dengan reed switch?", "id": "Reed Switch." },
  { "en": "Sensor untuk mengukur saturasi oksigen dalam darah?", "id": "Pulse Oximeter Sensor." },
  { "en": "Sensor untuk mengukur aktivitas otot (EMG)?", "id": "Sensor EMG." },
  { "en": "Sensor untuk mengukur aktivitas otak (EEG)?", "id": "Sensor EEG." },
  { "en": "Sensor jarak yang menggunakan teknologi laser?", "id": "Sensor Jarak Laser / LIDAR." },
  { "en": "Sensor untuk mengukur tingkat kebisingan (desibel)?", "id": "Sound Level Meter." },
  { "en": "Sensor untuk mendeteksi adanya hambatan di depan?", "id": "Obstacle Avoidance Sensor." },
  { "en": "Sensor yang mendeteksi perubahan resistansi karena tekanan?", "id": "Force Sensitive Resistor (FSR)." },
  { "en": "Sensor untuk mendeteksi gerakan di suatu area?", "id": "Motion Detector / Sensor Gerak." },
  { "en": "Sensor yang mendeteksi posisi menggunakan GPS?", "id": "Modul GPS." },
  { "en": "Sensor untuk membaca kartu RFID?", "id": "RFID Reader." },
  { "en": "Sensor untuk mengukur tekanan atmosfer?", "id": "Sensor Barometer." },
  { "en": "Sensor untuk mendeteksi debu atau partikel di udara?", "id": "Sensor Debu / Dust Sensor." },
  { "en": "Sensor yang mendeteksi objek dengan memancarkan dan menerima cahaya?", "id": "Photoelectric Sensor." },
  { "en": "Sensor yang mendeteksi perubahan medan kapasitif?", "id": "Sensor Proximity Kapasitif." },
  { "en": "Sensor yang mendeteksi perubahan medan induktif?", "id": "Sensor Proximity Induktif." },
  { "en": "Sensor untuk mengukur kecepatan angin?", "id": "Anemometer." },
  { "en": "Sensor untuk mengukur konduktivitas listrik cairan?", "id": "Conductivity Sensor." },
  { "en": "Sensor untuk mengukur radiasi nuklir?", "id": "Geiger Counter." },
  { "en": "Sensor untuk mengukur kelembapan dan suhu sekaligus?", "id": "Sensor DHT (e.g., DHT11/DHT22)." },
  { "en": "Sensor untuk mendeteksi gerakan mata?", "id": "Eye Tracker." },
  { "en": "Sensor yang mendeteksi gerakan menggunakan gelombang mikro?", "id": "Microwave Sensor / Radar Sensor." },
  { "en": "Sensor untuk membedakan antara siang dan malam?", "id": "LDR / Fotodioda." },
  { "en": "Sensor untuk sistem parkir mobil mundur?", "id": "Sensor Ultrasonik." },
  { "en": "Sensor pada pintu otomatis yang mendeteksi orang?", "id": "Sensor PIR / Microwave Sensor." },
  { "en": "Sensor untuk mengukur putaran roda kendaraan (ABS)?", "id": "Wheel Speed Sensor." },
  { "en": "Sensor pada mouse optik?", "id": "Sensor Optik / Optical Sensor." },
  { "en": "Sensor untuk mengetahui pintu terbuka atau tertutup?", "id": "Magnetic Contact Switch (Door Sensor)." },
  { "en": "Sensor untuk mengukur jumlah orang yang lewat?", "id": "People Counter Sensor." },
  { "en": "Sensor yang mendeteksi getaran kecil pada kaca?", "id": "Glass Break Detector." },
  { "en": "Sensor untuk mendeteksi posisi geografis?", "id": "Modul GPS." },
  { "en": "Sensor untuk mengukur kadar Oksigen (O2)?", "id": "Sensor Oksigen." },
  { "en": "Sensor yang mendeteksi keberadaan air atau banjir?", "id": "Water Leakage Sensor." },
  { "en": "Sensor untuk mengukur medan listrik statis?", "id": "Electrometer / Static Sensor." },
  { "en": "Sensor untuk mengetahui arah hadap?", "id": "Kompas / Magnetometer." },
  { "en": "Sensor untuk mengukur tinggi suatu tempat dari permukaan laut?", "id": "Altimeter / Sensor Barometer." },
  { "en": "Sensor untuk mengukur gaya putar atau torsi?", "id": "Sensor Torsi / Torque Sensor." },
  { "en": "Sensor yang mendeteksi objek dengan tirai cahaya pengaman?", "id": "Light Curtain / Tirai Cahaya." },
  { "en": "Sensor untuk mengukur pergeseran linear sangat presisi?", "id": "LVDT (Linear Variable Differential Transformer)." },
  { "en": "Sensor untuk mengukur tingkat kekentalan cairan?", "id": "Viscometer / Sensor Viskositas." },
  { "en": "Sensor untuk mengukur kadar glukosa dalam darah?", "id": "Sensor Glukosa." },
  { "en": "Sensor yang mendeteksi senyawa organik yang mudah menguap (VOC)?", "id": "Sensor VOC." },
  { "en": "Sensor untuk menganalisis spektrum cahaya?", "id": "Spektrometer." },
  { "en": "Sensor untuk mendeteksi kadar oksigen terlarut dalam air?", "id": "Dissolved Oxygen (DO) Sensor." },
  { "en": "Sensor yang mendeteksi sinyal listrik dari jantung?", "id": "Sensor EKG / ECG (Elektrokardiogram)." },
  { "en": "Sensor untuk mendeteksi kebocoran gas hidrogen sulfida (H2S)?", "id": "Sensor H2S." },
  { "en": "Sensor yang menggunakan reaksi kimia untuk mendeteksi gas?", "id": "Sensor Gas Elektrokimia." },
  { "en": "Sensor untuk mengukur intensitas radiasi matahari?", "id": "Piranometer / Solar Radiation Sensor." },
  { "en": "Sensor yang mengukur indeks bias suatu zat?", "id": "Refraktometer." },
  { "en": "Sensor untuk mendeteksi tekanan yang sangat rendah (vakum)?", "id": "Sensor Vakum / Vacuum Gauge." },
  { "en": "Sensor untuk mendeteksi posisi tuas gas pada kendaraan?", "id": "Throttle Position Sensor (TPS)." },
  { "en": "Sensor yang mendeteksi ketukan mesin (engine knocking)?", "id": "Knock Sensor." },
  { "en": "Sensor yang mengukur laju aliran massa udara ke mesin?", "id": "Mass Airflow (MAF) Sensor." },
  { "en": "Sensor yang menggunakan loop kawat untuk mendeteksi kendaraan?", "id": "Inductive Loop Detector." },
  { "en": "Sensor yang mengukur tingkat kepadatan (densitas) cairan?", "id": "Densitometer / Sensor Densitas." },
  { "en": "Sensor yang mendeteksi perubahan postur tubuh?", "id": "Sensor Postur." },
  { "en": "Sensor untuk mengukur laju pernapasan?", "id": "Sensor Pernapasan / Respiration Sensor." },
  { "en": "Sensor untuk mendeteksi keringat pada kulit (respon galvanik)?", "id": "GSR (Galvanic Skin Response) Sensor." },
  { "en": "Sensor yang mengukur sudut kemudi mobil?", "id": "Steering Angle Sensor." },
  { "en": "Sensor untuk mendeteksi partikel PM2.5 di udara?", "id": "Sensor PM2.5." },
  { "en": "Sensor yang berupa strip logam yang melengkung karena panas?", "id": "Sensor Bimetal / Bimetallic Strip." },
  { "en": "Sensor yang mendeteksi arah sumber suara?", "id": "Sound Localization Sensor." },
  { "en": "Sensor untuk mengukur resistivitas listrik suatu material?", "id": "Ohmmeter." },
  { "en": "Sensor untuk mengukur aliran panas?", "id": "Heat Flux Sensor." },
  { "en": "Sensor untuk mengukur suhu sangat dingin (kriogenik)?", "id": "Cryogenic Temperature Sensor." },
  { "en": "Sensor yang dapat 'melihat' distribusi panas?", "id": "Kamera Termal / Thermal Imager." },
  { "en": "Sensor untuk mengukur posisi piston dalam silinder hidrolik?", "id": "Cylinder Position Sensor." },
  { "en": "Sensor untuk mendeteksi kadar Ozon (O3) di atmosfer?", "id": "Sensor Ozon." },
  { "en": "Sensor yang mendeteksi ion di udara (ionizer)?", "id": "Ion Detector." },
  { "en": "Sensor untuk mendeteksi kebocoran amonia (NH3)?", "id": "Sensor Amonia." },
  { "en": "Sensor yang mendeteksi posisi menggunakan potensiometer?", "id": "Sensor Potensiometer." },
  { "en": "Sensor yang sangat sensitif untuk mengukur medan magnet lemah?", "id": "Fluxgate Magnetometer." },
  { "en": "Sensor yang mendeteksi satu foton cahaya?", "id": "SPAD (Single-Photon Avalanche Diode)." },
  { "en": "Sensor untuk mengukur polarisasi cahaya?", "id": "Sensor Polarisasi." },
  { "en": "Sensor untuk mendeteksi tegangan AC tanpa kontak fisik?", "id": "Non-Contact Voltage Tester." },
  { "en": "Sensor yang mendeteksi suara frekuensi sangat rendah (infrasonik)?", "id": "Infrasound Sensor." },
  { "en": "Sensor untuk mengukur keasaman atau kebasaan tanah?", "id": "Soil pH Meter." },
  { "en": "Sensor untuk mengukur getaran pada struktur bangunan?", "id": "Seismic Sensor / Seismometer." },
  { "en": "Sensor untuk mendeteksi objek dengan gelombang radar?", "id": "Sensor Radar." },
  { "en": "Sensor untuk mengetahui apakah suatu permukaan basah?", "id": "Sensor Kelembapan / Sensor Air." },
  { "en": "Sensor yang mendeteksi objek dengan membandingkan gambar?", "id": "Vision Sensor / Sensor Visi." },
  { "en": "Sensor untuk mengukur putaran per menit (RPM) suatu motor?", "id": "Tachometer." },
  { "en": "Sensor untuk mendeteksi posisi engkol mesin (crankshaft)?", "id": "Crankshaft Position Sensor." },
  { "en": "Sensor untuk mendeteksi posisi poros bubungan (camshaft)?", "id": "Camshaft Position Sensor." },
  { "en": "Sensor untuk mendeteksi tingkat oli mesin?", "id": "Oil Level Sensor." },
  { "en": "Sensor yang mengukur percepatan lateral (belokan)?", "id": "Lateral Accelerometer." },
  { "en": "Sensor yang membaca kode QR?", "id": "QR Code Scanner." },
  { "en": "Sensor untuk mendeteksi molekul biologis (antibodi/DNA)?", "id": "Biosensor." },
  { "en": "Sensor untuk mengukur hidrasi kulit?", "id": "Skin Hydration Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan nitrogen dioksida (NO2)?", "id": "Sensor NO2." },
  { "en": "Sensor untuk mendeteksi sulfur dioksida (SO2)?", "id": "Sensor SO2." },
  { "en": "Sensor untuk mendeteksi pergerakan cairan dalam tubuh (darah)?", "id": "Doppler Flow Sensor." },
  { "en": "Sensor untuk mendeteksi tingkat pengisian baterai?", "id": "Fuel Gauge IC." },
  { "en": "Sensor yang mengukur medan listrik di atmosfer?", "id": "Atmospheric Charge Sensor." },
  { "en": "Sensor yang mendeteksi perubahan sudut gravitasi?", "id": "Inklinometer / Tilt Sensor." },
  { "en": "Sensor untuk mengukur tekanan diferensial (perbedaan tekanan)?", "id": "Differential Pressure Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan uap air?", "id": "Water Vapor Sensor." },
  { "en": "Sensor yang mendeteksi radiasi elektromagnetik?", "id": "EMF (Electromagnetic Field) Meter." },
  { "en": "Sensor yang mendeteksi keberadaan objek di titik buta mobil?", "id": "Blind Spot Detector." },
  { "en": "Sensor untuk mengukur kedalaman air?", "id": "Depth Sensor / Echosounder." },
  { "en": "Sensor untuk mengukur salinitas (kadar garam) air?", "id": "Salinity Sensor." },
  { "en": "Sensor yang mendeteksi pergeseran tanah (longsor)?", "id": "Landslide Detector." },
  { "en": "Sensor untuk mendeteksi medan listrik yang dihasilkan otot?", "id": "Elektromiografi (EMG) Sensor." },
  { "en": "Sensor untuk mengukur paparan kebisingan personal?", "id": "Noise Dosimeter." },
  { "en": "Sensor yang mendeteksi perubahan panjang gelombang cahaya?", "id": "Wavelength Sensor." },
  { "en": "Sensor untuk mengukur ketebalan suatu lapisan cat?", "id": "Coating Thickness Gauge." },
  { "en": "Sensor untuk mendeteksi retakan mikro pada material?", "id": "Acoustic Emission Sensor." },
  { "en": "Sensor untuk mengukur kecepatan fluida dengan ultrasonik?", "id": "Ultrasonic Flowmeter." },
  { "en": "Sensor untuk mendeteksi molekul bau?", "id": "Electronic Nose (e-nose)." },
  { "en": "Sensor untuk mengukur kapasitas listrik suatu komponen?", "id": "Capacitance Meter." },
  { "en": "Sensor untuk mengukur induktansi suatu kumparan?", "id": "Inductance Meter (LCR Meter)." },
  { "en": "Sensor untuk mendeteksi keberadaan embun?", "id": "Dew Point Sensor." },
  { "en": "Sensor untuk mengukur potensi oksidasi-reduksi (ORP) air?", "id": "ORP Sensor." },
  { "en": "Sensor yang menggunakan efek Seebeck untuk mengukur suhu?", "id": "Termokopel." },
  { "en": "Sensor yang menggunakan efek piezoresistif untuk tekanan?", "id": "Piezoresistive Pressure Sensor." },
  { "en": "Sensor untuk mendeteksi gerakan dengan mengganggu medan elektromagnetik?", "id": "Capacitive Displacement Sensor." },
  { "en": "Sensor untuk mengukur jumlah langkah kaki?", "id": "Pedometer." },
  { "en": "Sensor untuk mengukur getaran yang disebabkan oleh detak jantung?", "id": "Ballistocardiograph (BCG)." },
  { "en": "Sensor untuk mendeteksi gelombang gravitasi?", "id": "Gravitational-Wave Detector (e.g., LIGO)." },
  { "en": "Sensor yang mendeteksi sinar kosmik?", "id": "Cosmic Ray Detector." },
  { "en": "Sensor untuk mengukur tingkat klorin dalam air?", "id": "Chlorine Sensor." },
  { "en": "Sensor untuk mengukur radiasi beta dan gamma?", "id": "Scintillation Counter." },
  { "en": "Sensor untuk mendeteksi kelelahan pengemudi dari mata?", "d": "Driver Drowsiness Detection Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan suatu senyawa kimia secara spesifik?", "id": "Chemiresistor." },
  { "en": "Sensor untuk mengukur kecepatan dan arah angin?", "id": "Ultrasonic Anemometer." },
  { "en": "Sensor untuk mengukur ketinggian awan?", "id": "Ceilometer." },
  { "en": "Sensor untuk mendeteksi petir?", "id": "Lightning Detector." },
  { "en": "Sensor untuk mengukur medan gravitasi bumi?", "id": "Gravimeter." },
  { "en": "Sensor untuk mendeteksi keberadaan air pada bahan bakar?", "id": "Water-in-Fuel Sensor." },
  { "en": "Sensor untuk memverifikasi keaslian sidik jari (deteksi nadi)?", "id": "Live Finger Detection Sensor." },
  { "en": "Sensor yang mengukur kadar kafein?", "id": "Caffeine Sensor." },
  { "en": "Sensor untuk mendeteksi tingkat kesegaran makanan?", "id": "Food Freshness Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan pestisida?", "id": "Pesticide Sensor." },
  { "en": "Sensor untuk mengukur kekasaran permukaan?", "id": "Profilometer / Surface Roughness Tester." },
  { "en": "Sensor untuk mendeteksi retak di bawah permukaan logam?", "id": "Eddy Current Sensor." },
  { "en": "Sensor untuk mengukur tingkat kilap (gloss) suatu permukaan?", "id": "Gloss Meter." },
  { "en": "Sensor yang bekerja berdasarkan efek Coriolis untuk mengukur aliran?", "id": "Coriolis Flowmeter." },
  { "en": "Sensor aliran yang menggunakan medan magnet pada cairan konduktif?", "id": "Magnetic Flowmeter." },
  { "en": "Sensor aliran yang mengukur pendinginan elemen yang dipanaskan?", "id": "Thermal Mass Flowmeter." },
  { "en": "Sensor yang meniru indra perasa manusia?", "id": "Electronic Tongue (e-tongue)." },
  { "en": "Sensor yang menggunakan resonansi plasmon untuk deteksi molekuler?", "id": "SPR (Surface Plasmon Resonance) Sensor." },
  { "en": "Sensor untuk menghitung partikel atau sel dalam cairan?", "id": "Particle Counter / Flow Cytometer." },
  { "en": "Sensor yang mendeteksi radiasi inframerah gelombang panjang?", "id": "Bolometer / Microbolometer." },
  { "en": "Sensor untuk mengukur tingkat kejernihan atau kabut suatu cairan?", "id": "Nephelometer / Transmissometer." },
  { "en": "Sensor yang mendeteksi posisi dengan navigasi inersia?", "id": "INS (Inertial Navigation System)." },
  { "en": "Sensor yang mengukur perubahan percepatan (jerk)?", "id": "Jerk Sensor." },
  { "en": "Sensor yang menggunakan interferensi cahaya untuk pengukuran presisi?", "id": "Interferometer." },
  { "en": "Sensor untuk mengukur kekerasan suatu material?", "id": "Hardness Tester." },
  { "en": "Sensor untuk mengukur konsentrasi DNA?", "id": "Nanospectrophotometer." },
  { "en": "Sensor yang menggunakan antibodi untuk mendeteksi protein?", "id": "Immunoassay Sensor." },
  { "en": "Sensor yang mendeteksi kebocoran pipa menggunakan suara?", "id": "Acoustic Leak Detector." },
  { "en": "Sensor aliran yang mendeteksi pusaran (vortex) fluida?", "id": "Vortex Flowmeter." },
  { "en": "Sensor untuk mengukur konsumsi daya listrik (watt)?", "id": "Wattmeter / Power Meter." },
  { "en": "Sensor untuk menganalisis kualitas daya listrik (harmonik)?", "id": "Power Quality Analyzer." },
  { "en": "Sensor yang bekerja berdasarkan efek Giant Magnetoresistance?", "id": "GMR Sensor." },
  { "en": "Sensor untuk mengukur arus listrik yang sangat kecil?", "id": "Galvanometer / Picoammeter." },
  { "en": "Sensor yang mendeteksi posisi berdasarkan perubahan induktansinya?", "id": "Variable Inductance Sensor." },
  { "en": "Sensor yang melacak gerakan mata pada layar?", "id": "Eye Gaze Tracker." },
  { "en": "Sensor untuk mendeteksi kebocoran gas metana (CH4)?", "id": "Methane Sensor." },
  { "en": "Sensor untuk mengukur ketebalan lapisan tipis?", "id": "Film Thickness Gauge." },
  { "en": "Kamera yang dapat melihat dalam kondisi cahaya sangat minim?", "id": "Intensified CCD (ICCD) Camera." },
  { "en": "Sensor pada keran air otomatis?", "id": "Sensor Inframerah Aktif." },
  { "en": "Sensor untuk memastikan sabuk pengaman mobil terpasang?", "id": "Buckle Switch." },
  { "en": "Sensor untuk sistem adaptive cruise control pada mobil?", "id": "Sensor Radar / LIDAR." },
  { "en": "Sensor yang mendeteksi kelembapan pada material kayu?", "id": "Wood Moisture Meter." },
  { "en": "Sensor yang bekerja berdasarkan efek fotolistrik?", "id": "Photoelectric Sensor." },
  { "en": "Sensor yang resistansinya sangat dipengaruhi medan magnet?", "id": "Magnetoresistor." },
  { "en": "Sensor untuk mengukur konsentrasi debu di atmosfer?", "id": "Aethalometer." },
  { "en": "Sensor untuk mendeteksi gelombang kejut (shockwave)?", "id": "Shock Sensor." },
  { "en": "Sensor yang mendeteksi kehadiran manusia dalam ruangan?", "id": "Occupancy Sensor." },
  { "en": "Sensor yang digunakan dalam tes kebohongan (poligraf)?", "id": "Polygraph Sensor (GSR/HR/Respiration)." },
  { "en": "Sensor yang memindai dan membuat peta 3D lingkungan?", "id": "3D Scanner / LIDAR." },
  { "en": "Sensor untuk mendeteksi keberadaan tegangan pada kawat pagar?", "id": "Electric Fence Voltage Sensor." },
  { "en": "Sensor untuk mengukur defleksi atau kelengkungan balok?", "id": "Deflection Sensor." },
  { "en": "Sensor yang mendeteksi perubahan resistansi akibat tekukan?", "id": "Flex Sensor." },
  { "en": "Sensor untuk mengukur sudut datang matahari?", "id": "Sun Angle Sensor." },
  { "en": "Sensor yang mengukur kadar karbon dioksida (CO2)?", "id": "Sensor CO2 / NDIR Sensor." },
  { "en": "Sensor yang mendeteksi hidrogen peroksida (H2O2)?", "id": "Hydrogen Peroxide Sensor." },
  { "en": "Sensor untuk mendeteksi tingkat radiasi pengion?", "id": "Ionizing Radiation Detector." },
  { "en": "Sensor untuk mengukur tingkat kekeruhan atmosfer?", "id": "Pyrheliometer." },
  { "en": "Sensor untuk mendeteksi uap pelarut organik?", "id": "Organic Solvent Vapor Sensor." },
  { "en": "Sensor untuk mengukur medan magnet sisa pada material?", "id": "Residual Field Indicator." },
  { "en": "Sensor untuk mendeteksi tingkat kelelahan material logam?", "id": "Fatigue Sensor." },
  { "en": "Sensor untuk mengukur tingkat korosi?", "id": "Corrosion Sensor." },
  { "en": "Sensor yang mendeteksi perbedaan fase antara dua sinyal?", "id": "Phase Detector." },
  { "en": "Sensor untuk mengukur permitivitas dielektrik suatu bahan?", "id": "Dielectric Sensor." },
  { "en": "Sensor yang mendeteksi pergerakan air tanah?", "id": "Groundwater Flow Sensor." },
  { "en": "Sensor untuk mendeteksi perubahan warna cairan secara otomatis?", "id": "Colorimetric Sensor." },
  { "en": "Sensor untuk mengukur visibilitas atau jarak pandang?", "id": "Visibility Sensor." },
  { "en": "Sensor untuk mengukur konsentrasi klorofil pada tanaman?", "id": "Chlorophyll Meter." },
  { "en": "Sensor yang mendeteksi keberadaan es di permukaan?", "id": "Ice Detector." },
  { "en": "Sensor untuk mendeteksi orientasi absolut terhadap bumi?", "id": "Attitude and Heading Reference System (AHRS)." },
  { "en": "Sensor yang mendeteksi getaran harmonik tertentu?", "id": "Resonant Sensor." },
  { "en": "Sensor untuk mengukur tekanan darah non-invasif?", "id": "Sphygmomanometer / Blood Pressure Sensor." },
  { "en": "Sensor untuk mengukur tingkat gula dalam buah?", "id": "Brix Sensor." },
  { "en": "Sensor untuk mengukur deformasi batuan?", "id": "Strainmeter / Deformeter." },
  { "en": "Sensor yang mendeteksi gelombang suara di bawah air?", "id": "Hidrofon / Hydrophone." },
  { "en": "Sensor untuk mendeteksi keberadaan merkuri?", "id": "Mercury Sensor." },
  { "en": "Sensor untuk mengukur tingkat radiasi fotosintetik (PAR)?", "id": "PAR Sensor." },
  { "en": "Sensor untuk mendeteksi perpindahan panas konvektif?", "d": "Convective Heat Transfer Sensor." },
  { "en": "Sensor yang mendeteksi dan mengklasifikasikan bau?", "id": "Electronic Nose (e-nose)." },
  { "en": "Sensor yang menggunakan serat optik untuk mengukur regangan?", "id": "Fiber Optic Strain Sensor." },
  { "en": "Sensor yang menggunakan efek pyroelektrik untuk deteksi panas?", "id": "Pyroelectric Sensor." },
  { "en": "Sensor untuk mengukur tingkat penguapan air?", "id": "Evaporation Sensor." },
  { "en": "Sensor yang mengukur medan listrik di bawah air?", "id": "Underwater Electric Field Sensor." },
  { "en": "Sensor untuk mendeteksi tingkat kesadahan air?", "id": "Water Hardness Sensor." },
  { "en": "Sensor untuk memverifikasi keaslian suatu dokumen?", "id": "Document Security Sensor." },
  { "en": "Sensor untuk mendeteksi kualitas tidur berdasarkan gerakan?", "id": "Sleep Tracker." },
  { "en": "Sensor untuk mengukur kepadatan fluks magnetik?", "id": "Gaussmeter." },
  { "en": "Sensor yang mendeteksi kerusakan pada bantalan (bearing) mesin?", "id": "Bearing Fault Detector." },
  { "en": "Sensor untuk mendeteksi medan listrik dari makhluk hidup?", "id": "Bio-electric Field Sensor." },
  { "en": "Sensor untuk mengukur tingkat kebulatan suatu objek?", "id": "Roundness Measurement Sensor." },
  { "en": "Sensor untuk mengukur kecepatan suara dalam suatu medium?", "id": "Sound Velocimeter." },
  { "en": "Sensor untuk mendeteksi tegangan permukaan cairan?", "id": "Tensiometer." },
  { "en": "Sensor yang mendeteksi tingkat hidrasi tubuh?", "id": "Hydration Sensor." },
  { "en": "Sensor untuk mengukur aktivitas elektrokimia di otak?", "id": "Neurotransmitter Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan bakteri?", "id": "Bacterial Sensor." },
  { "en": "Sensor untuk mengukur jumlah serbuk sari di udara?", "id": "Pollen Sensor." },
  { "en": "Sensor untuk mengukur tingkat stres pada tanaman?", "id": "Plant Stress Sensor." },
  { "en": "Sensor yang mendeteksi perubahan gravitasi mikro?", "id": "Microgravity Sensor." },
  { "en": "Sensor untuk mengukur tingkat kelembapan pada beton?", "id": "Concrete Moisture Meter." },
  { "en": "Sensor yang mendeteksi pembekuan cairan?", "id": "Freezing Point Sensor." },
  { "en": "Sensor untuk mengukur permeabilitas magnetik suatu material?", "id": "Permeameter." },
  { "en": "Sensor untuk mengukur tingkat kelelahan pengemudi?", "id": "Driver Fatigue Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan timbal (lead) dalam air?", "id": "Lead Sensor." },
  { "en": "Sensor yang mendeteksi perubahan kimia melalui warna?", "id": "Chemochromic Sensor." },
  { "en": "Sensor untuk mengukur tingkat paparan getaran pada tangan?", "id": "Hand-Arm Vibration Sensor." },
  { "en": "Sensor untuk mengukur ketebalan es di jalan?", "id": "Ice Thickness Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan jamur atau spora?", "id": "Mold Detector." },
  { "en": "Sensor yang memberi 'indra peraba' pada robot?", "id": "Tactile Sensor Array." },
  { "en": "Sensor untuk mengukur kesehatan tanaman dari citra drone?", "id": "Kamera Multispektral / Hiperspektral." },
  { "en": "Sensor untuk mengukur pertumbuhan diameter batang pohon?", "id": "Dendrometer." },
  { "en": "Sensor yang mendeteksi embun atau kebasahan pada daun?", "id": "Leaf Wetness Sensor." },
  { "en": "Sensor yang bisa ditelan untuk memonitor kondisi dalam tubuh?", "id": "Ingestible Sensor / Smart Pill." },
  { "en": "Sensor yang menganalisis napas untuk mendeteksi penyakit?", "id": "Breathalyzer Medis." },
  { "en": "Sensor untuk mengukur tekanan di dalam bola mata (glaukoma)?", "id": "Tonometer." },
  { "en": "Sensor yang memetakan distribusi tekanan pada telapak kaki?", "id": "Plantar Pressure Sensor." },
  { "en": "Sistem sensor untuk mendeteksi dan melacak tsunami di laut?", "id": "DART (Deep-ocean Assessment and Reporting of Tsunamis)." },
  { "en": "Sensor untuk mengukur total padatan terlarut dalam air?", "id": "TDS (Total Dissolved Solids) Meter." },
  { "en": "Sensor untuk mengukur kepadatan tanah?", "id": "Soil Compaction Tester / Penetrometer." },
  { "en": "Sensor untuk mengukur ketebalan lapisan salju?", "id": "Snow Depth Sensor." },
  { "en": "Sensor untuk memverifikasi campuran warna cat secara presisi?", "id": "Spektrofotometer." },
  { "en": "Sensor untuk mendeteksi lubang sangat kecil (pinhole) pada lembaran?", "id": "Pinhole Detector." },
  { "en": "Sensor untuk mengukur keselarasan (alignment) dua poros mesin?", "id": "Laser Alignment Tool." },
  { "en": "Sensor yang mengukur muatan listrik statis pada jalur produksi?", "id": "Static Charge Meter." },
  { "en": "Sensor yang bekerja berdasarkan efek Seebeck?", "id": "Termokopel." },
  { "en": "Sensor yang bekerja berdasarkan efek Piezoresistif?", "id": "Strain Gauge / Piezoresistive Sensor." },
  { "en": "Sensor yang bekerja berdasarkan efek Pyroelektrik?", "id": "Pyroelectric Sensor." },
  { "en": "Sensor untuk memonitor kesehatan dan lokasi ternak sapi?", "id": "Smart Cattle Collar / Bolus Sensor." },
  { "en": "Sensor untuk memonitor nutrisi dalam sistem hidroponik?", "id": "Hydroponic Nutrient Sensor." },
  { "en": "Sensor untuk mendeteksi kematangan buah dari gas etilen?", "id": "Ethylene Sensor." },
  { "en": "Sensor yang mendeteksi cacat mikroskopis pada wafer silikon?", "id": "Wafer Inspection System." },
  { "en": "Sensor untuk mengukur kekuatan genggaman robot?", "id": "Grip Force Sensor." },
  { "en": "Sistem sensor lengkap untuk memantau kondisi cuaca?", "id": "Stasiun Cuaca / Weather Station." },
  { "en": "Sensor untuk mengukur kandungan air dalam salju?", "id": "Snow Water Equivalent (SWE) Sensor." },
  { "en": "Sensor yang mendeteksi getaran awal dari longsoran salju?", "id": "Avalanche Detector." },
  { "en": "Sensor yang meniru cara kelelawar bernavigasi?", "id": "Sensor Ultrasonik." },
  { "en": "Sensor yang memberi 'mata' presisi pada lengan robot?", "id": "Vision Sensor / Kamera Industri." },
  { "en": "Sensor yang merasakan 'demam' mesin dari jauh?", "id": "Termometer Inframerah." },
  { "en": "Sensor yang 'mendengarkan' kesehatan mesin dari getarannya?", "id": "Vibration Analyzer." },
  { "en": "Sensor yang 'mencicipi' keasaman atau kebasaan larutan?", "id": "pH Meter." },
  { "en": "Sensor yang mendeteksi perubahan orientasi berdasarkan inersia?", "id": "Inertial Measurement Unit (IMU)." },
  { "en": "Sensor yang mengukur waktu tempuh bolak-balik pulsa cahaya?", "id": "Time-of-Flight (ToF) Sensor." },
  { "en": "Elemen inti pada sensor tekanan yang berubah bentuk?", "id": "Diafragma." },
  { "en": "Sensor yang mendeteksi posisi dengan sinyal satelit?", "id": "GNSS Receiver (GPS, GLONASS)." },
  { "en": "Sensor untuk mendeteksi turbulensi dalam aliran udara?", "id": "Turbulence Sensor." },
  { "en": "Sensor untuk mengukur kadar fosfat dalam air?", "id": "Phosphate Sensor." },
  { "en": "Sensor untuk mengukur kadar nitrat?", "id": "Nitrate Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan virus?", "id": "Viral Sensor." },
  { "en": "Sensor untuk menganalisis komposisi kimia suatu sampel?", "id": "Chemical Analyzer." },
  { "en": "Sensor untuk mengukur tingkat radiasi non-pengion (ponsel, Wi-Fi)?", "id": "RF (Radio Frequency) Detector." },
  { "en": "Sensor untuk mengukur tingkat erosi tanah?", "id": "Soil Erosion Sensor." },
  { "en": "Sensor yang mendeteksi tegangan pada sel syaraf?", "id": "Neuroprobe / Patch Clamp." },
  { "en": "Sensor untuk mengukur tingkat hidrasi pada tanaman?", "id": "Plant Hydration Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan logam berat dalam air?", "id": "Heavy Metal Sensor." },
  { "en": "Sensor untuk mengukur konsentrasi aerosol di udara?", "id": "Aerosol Monitor." },
  { "en": "Sensor yang mendeteksi perubahan kecil dalam medan gravitasi lokal?", "id": "Gravitasi Gradiometer." },
  { "en": "Sensor untuk mengukur indeks kenyamanan termal manusia?", "id": "Thermal Comfort Sensor." },
  { "en": "Sensor untuk memantau integritas struktural sebuah jembatan?", "id": "Structural Health Monitoring (SHM) Sensor." },
  { "en": "Sensor yang mendeteksi tingkat keausan pada rem kendaraan?", "id": "Brake Pad Wear Sensor." },
  { "en": "Sensor untuk mengukur tingkat kekosongan (vakum) dalam kemasan?", "id": "Package Leak Detector." },
  { "en": "Sensor yang mendeteksi kontaminasi mikroba pada permukaan?", "id": "Microbial Contamination Sensor." },
  { "en": "Sensor untuk mengukur elastisitas suatu material?", "id": "Elastometer / Modulus Sensor." },
  { "en": "Sensor untuk mendeteksi medan magnet yang dihasilkan oleh otak?", "id": "Magnetoencephalography (MEG) Sensor." },
  { "en": "Sensor untuk mengukur sudut kontak tetesan cairan?", "id": "Contact Angle Goniometer." },
  { "en": "Sensor yang mendeteksi keberadaan serbuk gergaji di udara?", "id": "Sawdust Sensor." },
  { "en": "Sensor untuk memverifikasi keberadaan hologram keamanan?", "id": "Hologram Detector." },
  { "en": "Sensor untuk mendeteksi perubahan tekanan di dalam telinga?", "id": "Tympanometer." },
  { "en": "Sensor yang mendeteksi perubahan komposisi gas buang mobil?", "id": "Exhaust Gas Sensor (Lambda/O2)." },
  { "en": "Sensor untuk mengukur tingkat kebengkokan rel kereta api?", "id": "Rail Track Geometry Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan plankton di air?", "id": "Plankton Counter." },
  { "en": "Sensor untuk mengukur tingkat kejernihan optik (misal: lensa)?", "id": "Optical Clarity Sensor." },
  { "en": "Sensor untuk mendeteksi pergerakan gletser?", "id": "Glacier Motion Sensor." },
  { "en": "Sensor untuk mengukur tingkat pemadatan aspal?", "id": "Asphalt Density Gauge." },
  { "en": "Sensor yang mendeteksi keberadaan organisme hasil rekayasa genetika?", "id": "GMO Detector." },
  { "en": "Sensor untuk mengukur tingkat kebisingan di bawah air?", "id": "Acoustic Noise Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan ranjau darat?", "id": "Landmine Detector." },
  { "en": "Sensor yang mengukur konsentrasi gas radon?", "id": "Radon Detector." },
  { "en": "Sensor untuk mendeteksi keaslian uang kertas?", "id": "Banknote Authenticator." },
  { "en": "Sensor yang mengukur kecepatan perambatan retak pada material?", "id": "Crack Propagation Gauge." },
  { "en": "Sensor untuk mendeteksi perubahan warna kulit (kesehatan)?", "id": "Skin Color sensor." },
  { "en": "Sensor untuk mengukur medan listrik di dalam air?", "id": "Underwater Electric Field Sensor." },
  { "en": "Sensor yang mengukur tingkat pelapukan batuan?", "id": "Weathering Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan benda asing dalam makanan?", "id": "Food Contaminant Detector (X-ray/Metal)." },
  { "en": "Sensor untuk mengukur tingkat kesegaran telur?", "id": "Egg Freshness Tester." },
  { "en": "Sensor untuk memantau aktivitas vulkanik?", "id": "Volcano Monitoring Sensor." },
  { "en": "Sensor yang mendeteksi ketidakseimbangan pada mesin berputar?", "id": "Imbalance Sensor." },
  { "en": "Sensor untuk mengukur tingkat hidrasi adonan?", "id": "Dough Hydration Sensor." },
  { "en": "Sensor yang mendeteksi tingkat stres vokal dalam suara?", "id": "Vocal Stress Analyzer." },
  { "en": "Sensor untuk mengukur tingkat luminesensi?", "id": "Luminometer." },
  { "en": "Sensor untuk mendeteksi perubahan tekanan yang sangat cepat?", "id": "Dynamic Pressure Sensor." },
  { "en": "Sensor untuk mengukur tingkat kelembapan pada biji-bijian?", "id": "Grain Moisture Meter." },
  { "en": "Sensor untuk mengukur tingkat radiasi ultraviolet yang diterima kulit?", "id": "Personal UV Dosimeter." },
  { "en": "Sensor yang mendeteksi keberadaan antibiotik?", "id": "Antibiotic Sensor." },
  { "en": "Sensor untuk mengukur tingkat kemanisan madu?", "id": "Honey Quality Sensor." },
  { "en": "Sensor untuk memantau kemacetan lalu lintas?", "id": "Traffic Flow Sensor." },
  { "en": "Sensor yang mendeteksi keausan pada mata bor?", "id": "Drill Bit Wear Sensor." },
  { "en": "Sensor untuk mengukur konsentrasi gas rumah kaca?", "id": "Greenhouse Gas Analyzer." },
  { "en": "Sensor untuk mendeteksi gelembung udara dalam cairan?", "id": "Bubble Detector." },
  { "en": "Sensor yang mengukur tegangan pada senar (alat musik)?", "id": "String Tension Sensor." },
  { "en": "Sensor untuk mendeteksi tingkat aktivitas seismik mikro?", "id": "Microseismic Sensor." },
  { "en": "Sensor yang mengukur tingkat pengisian silo dari luar?", "id": "Silo Level Sensor (Radar/Ultrasonic)." },
  { "en": "Sensor untuk mendeteksi keberadaan narkotika?", "id": "Narcotics Detector." },
  { "en": "Sensor untuk memverifikasi kualitas lasan?", "id": "Weld Inspection Sensor." },
  { "en": "Sensor yang mengukur tingkat degradasi oli pelumas?", "id": "Oil Quality Sensor." },
  { "en": "Sensor untuk mengukur tingkat kesuburan tanah?", "id": "Soil Fertility Sensor." },
  { "en": "Sensor yang menggunakan grafena untuk deteksi super sensitif?", "id": "Graphene Sensor." },
  { "en": "Sensor yang menggunakan tabung nano karbon untuk mendeteksi gas?", "id": "Carbon Nanotube Sensor." },
  { "en": "Sensor yang bekerja berdasarkan prinsip quantum tunneling?", "id": "Scanning Tunneling Microscope (STM)." },
  { "en": "Sensor untuk mendeteksi fonon (kuanta getaran)?", "id": "Phonon Detector." },
  { "en": "Sensor untuk mendeteksi partikel subatomik seperti muon?", "id": "Particle Detector / Cloud Chamber." },
  { "en": "Sensor yang membaca perintah dari gerakan tangan di udara?", "id": "Gesture Control Sensor." },
  { "en": "Sensor yang mendeteksi kehadiran orang dari detak jantungnya?", "id": "Radar-based Vital Sign Sensor." },
  { "en": "Sensor yang memonitor tingkat laktat pada atlet?", "id": "Lactate Sensor." },
  { "en": "Sensor yang mengukur tingkat kedipan mata untuk deteksi kelelahan?", "id": "Blink Sensor." },
  { "en": "Sensor untuk mendeteksi busa dalam proses industri?", "id": "Foam Sensor." },
  { "en": "Sensor untuk memastikan label tidak hilang dari produk?", "id": "Label Sensor." },
  { "en": "Sensor untuk memverifikasi keberadaan tutup botol?", "id": "Vision Sensor / Proximity Sensor." },
  { "en": "Sensor untuk mengukur tingkat pencahayaan alami di dalam ruangan?", "id": "Daylight Sensor." },
  { "en": "Sensor untuk memantau kesehatan terumbu karang?", "id": "Coral Reef Monitoring Sensor." },
  { "en": "Sensor untuk mengukur pergerakan lempeng tektonik?", "id": "Geodetic GPS / Strainmeter." },
  { "en": "Sensor untuk mengukur pH air hujan secara spesifik?", "id": "Acid Rain Monitor." },
  { "en": "Sensor untuk mengukur kualitas langit malam (polusi cahaya)?", "id": "Sky Quality Meter." },
  { "en": "Sensor yang bertindak sebagai 'penjaga gerbang' tak terlihat?", "id": "Safety Scanner / Light Curtain." },
  { "en": "Sensor yang 'berbisik' jika ada logam yang lewat?", "id": "Sensor Induktif." },
  { "en": "Sensor yang merasakan 'haus'-nya sebuah pot tanaman?", "id": "Soil Moisture Sensor." },
  { "en": "Sensor yang menghitung setiap pil yang jatuh dari dispenser?", "id": "Object Counter Sensor." },
  { "en": "Sensor yang memastikan warna kain sesuai standar?", "id": "Spectrophotometer." },
  { "en": "Sensor yang menjaga jarak aman antar kendaraan di jalan tol?", "id": "Long-Range Radar / LIDAR." },
  { "en": "Sensor yang mendeteksi jika sebuah lukisan di museum disentuh?", "id": "Capacitive Proximity Sensor." },
  { "en": "Sensor pada mesin penjual otomatis yang memvalidasi koin?", "id": "Coin Acceptor." },
  { "en": "Sensor untuk membuka pintu toilet umum tanpa sentuhan?", "id": "Active Infrared Sensor." },
  { "en": "Sensor untuk mengukur efisiensi mesin secara keseluruhan (OEE)?", "id": "OEE Monitoring System." },
  { "en": "Sensor untuk mendeteksi pola getaran yang tidak normal?", "id": "Anomaly Detection Sensor." },
  { "en": "Sensor yang menentukan orientasi 3D sebuah satelit?", "id": "Star Tracker / Sun Sensor." },
  { "en": "Sensor untuk mengukur tingkat 'kewaspadaan' seorang pilot?", "id": "Pilot Monitoring System." },
  { "en": "Sensor untuk mendeteksi kehalusan bubuk coklat?", "id": "Particle Size Analyzer." },
  { "en": "Sensor untuk mengukur tegangan pada jaring laba-laba?", "id": "Vibration Sensor (Pico-scale)." },
  { "en": "Sensor yang mendeteksi molekul DNA spesifik?", "id": "DNA Microarray." },
  { "en": "Sensor untuk mengukur putaran baling-baling drone?", "id": "Motor RPM Sensor." },
  { "en": "Sensor yang mendeteksi perubahan salinitas di muara sungai?", "id": "Estuary Salinity Sensor." },
  { "en": "Sensor untuk memverifikasi keberadaan tanda air pada dokumen?", "id": "Watermark Detector." },
  { "en": "Sensor untuk mengukur perubahan diameter pupil mata?", "id": "Pupilometer." },
  { "en": "Sensor yang mendeteksi tingkat kristalisasi gula?", "id": "Crystallization Sensor." },
  { "en": "Sensor untuk memantau tingkat radiasi di sekitar reaktor nuklir?", "id": "Dosimeter." },
  { "en": "Sensor untuk mendeteksi retakan rambut pada sayap pesawat?", "id": "Acoustic Emission Sensor." },
  { "en": "Sensor yang mengukur kadar air dalam madu?", "id": "Honey Moisture Tester." },
  { "en": "Sensor untuk mengukur tingkat kekosongan udara dalam panel surya?", "id": "Vacuum Sensor." },
  { "en": "Sensor yang mendeteksi perubahan kimia pada permukaan katalis?", "id": "Catalytic Sensor." },
  { "en": "Sensor untuk mengukur tingkat biodegradasi plastik?", "id": "Biodegradation Sensor." },
  { "en": "Sensor yang membedakan antara air tawar dan air asin?", "id": "Salinity Sensor." },
  { "en": "Sensor untuk mengukur gaya yang dibutuhkan untuk menusuk material?", "id": "Puncture Resistance Tester." },
  { "en": "Sensor yang mendeteksi keberadaan serangga di lumbung padi?", "id": "Acoustic Insect Detector." },
  { "en": "Sensor untuk mengukur tingkat deposisi debu industri?", "id": "Dustfall Monitor." },
  { "en": "Sensor untuk membedakan antara berbagai jenis kain?", "id": "Fabric Analyzer." },
  { "en": "Sensor yang mendeteksi tingkat stres pada ikan di akuakultur?", "id": "Fish Stress Monitor." },
  { "en": "Sensor untuk mengukur tingkat kesegaran kopi?", "id": "Coffee Freshness Sensor." },
  { "en": "Sensor yang mendeteksi pelepasan korona pada isolator listrik?", "id": "Corona Discharge Detector." },
  { "en": "Sensor untuk mengukur tingkat pemutihan pada kertas?", "id": "Paper Whiteness Meter." },
  { "en": "Sensor yang mendeteksi keberadaan organisme laut dalam air balas?", "id": "Ballast Water Monitoring Sensor." },
  { "en": "Sensor untuk mengukur kadar kapur dalam tanah?", "id": "Soil Lime Requirement Sensor." },
  { "en": "Sensor yang mendeteksi tingkat pemalsuan minyak zaitun?", "id": "Olive Oil Authenticity Sensor." },
  { "en": "Sensor untuk mengukur tingkat getaran yang dirasakan oleh penghuni gedung?", "id": "Human Comfort Vibration Meter." },
  { "en": "Sensor yang mendeteksi tanda tangan kimiawi dari bahan peledak?", "id": "Explosive Trace Detector." },
  { "en": "Sensor untuk mengukur tingkat kekeringan pada mata?", "id": "Tear Film Osmolarity Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan bakteri Legionella dalam air?", "id": "Legionella Sensor." },
  { "en": "Sensor untuk mengukur tingkat keselarasan serat dalam komposit?", "id": "Fiber Orientation Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan arsenik dalam air minum?", "id": "Arsenic Sensor." },
  { "en": "Sensor untuk mengukur tingkat karbon organik total (TOC) dalam air?", "id": "TOC Analyzer." },
  { "en": "Sensor yang mendeteksi tingkat kejenuhan warna pada layar display?", "id": "Colorimeter." },
  { "en": "Sensor untuk mengukur muatan elektrostatik pada tubuh manusia?", "id": "Static Charge Sensor for Personnel." },
  { "en": "Sensor yang mendeteksi keberadaan gluten dalam makanan?", "id": "Gluten Detector." },
  { "en": "Sensor untuk mengukur tingkat penyerapan suara suatu material?", "id": "Acoustic Absorption Meter." },
  { "en": "Sensor yang mendeteksi perubahan tekanan di dalam gunung berapi?", "id": "Volcanic Deformation Sensor." },
  { "en": "Sensor untuk mengukur tingkat radiasi dari ponsel?", "id": "SAR (Specific Absorption Rate) Meter." },
  { "en": "Sensor yang mendeteksi keberadaan lumut di permukaan?", "id": "Algae Sensor." },
  { "en": "Sensor untuk mengukur viskoelastisitas polimer?", "id": "Rheometer." },
  { "en": "Sensor yang mendeteksi keberadaan histamin dalam ikan?", "id": "Histamine Sensor." },
  { "en": "Sensor untuk mengukur tingkat paparan formaldehida di dalam ruangan?", "id": "Formaldehyde Detector." },
  { "en": "Sensor yang membedakan antara sentuhan dan tekanan?", "id": "Force/Tactile Sensor." },
  { "en": "Sensor untuk mendeteksi pergerakan janin dalam kandungan?", "id": "Fetal Movement Sensor." },
  { "en": "Sensor yang mengukur tingkat kekakuan kertas?", "id": "Stiffness Tester." },
  { "en": "Sensor untuk mendeteksi keberadaan aflatoksin pada kacang?", "id": "Aflatoxin Sensor." },
  { "en": "Sensor yang mengukur tingkat pelapisan anti-reflektif pada kacamata?", "id": "Anti-Reflection Coating Meter." },
  { "en": "Sensor untuk mendeteksi pergerakan magma di bawah tanah?", "id": "Magma Chamber Sensor." },
  { "en": "Sensor yang mengukur tingkat kohesi bubuk?", "id": "Powder Flow Tester." },
  { "en": "Sensor untuk mendeteksi keberadaan laktosa dalam susu?", "id": "Lactose Sensor." },
  { "en": "Sensor yang memverifikasi integritas segel kemasan?", "id": "Seal Integrity Tester." },
  { "en": "Sensor untuk mengukur tingkat kekeringan udara terkompresi?", "id": "Compressed Air Dew Point Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan alergen kacang?", "id": "Peanut Allergen Sensor." },
  { "en": "Sensor untuk mengukur gaya gesekan antara dua permukaan?", "id": "Tribometer." },
  { "en": "Sensor yang mendeteksi keberadaan medan magnet bolak-balik?", "id": "AC Magnetometer." },
  { "en": "Sensor untuk mengukur tingkat karbonasi dalam minuman?", "id": "Carbonation Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan minyak di permukaan air?", "id": "Oil-on-Water Sensor." },
  { "en": "Sensor untuk mengukur tingkat ketegangan pada kain tenun?", "id": "Warp Tension Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan kista parasit dalam air?", "id": "Parasite Cyst Detector." },
  { "en": "Sensor untuk mengukur tingkat kecerahan bintang di langit?", "id": "Astronomical Photometer." },
  { "en": "Sensor yang mendeteksi keberadaan sianida?", "id": "Cyanide Sensor." },
  { "en": "Sensor untuk mengukur tingkat keausan ban kendaraan?", "id": "Tire Tread Depth Sensor." },
  { "en": "Sensor yang mendeteksi keberadaan medan skalar (hipotetis)?", "id": "Scalar Field Detector." },
  { "en": "Sensor untuk memverifikasi komposisi paduan logam?", "id": "X-Ray Fluorescence (XRF) Analyzer." },
  { "en": "Sensor yang mengukur tingkat aktivitas enzim?", "id": "Enzyme Activity Sensor." },
  { "en": "Sensor untuk mendeteksi keberadaan polusi suara spesifik?", "id": "Noise Source Identifier." },
  { "en": "Sensor yang mendeteksi keberadaan kehidupan di planet lain?", "id": "Life Detection Instrument." }

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
