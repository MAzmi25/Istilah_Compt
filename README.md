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


  { "en": "Regulator Tegangan Positif 5V?", "id": "IC 7805." },
  { "en": "Regulator Tegangan Negatif 5V?", "id": "IC 7905." },
  { "en": "IC Membuat Berbagai Rangkaian Pewaktu?", "id": "IC Timer 555." },
  { "en": "Op-Amp Serbaguna Dan Populer?", "id": "IC 741." },
  { "en": "Transistor NPN Serbaguna?", "id": "Transistor 2N3904." },
  { "en": "Transistor PNP Serbaguna?", "id": "Transistor 2N3906." },
  { "en": "Dioda Penyearah Arus 1A?", "id": "Dioda 1N4007." },
  { "en": "Dioda Zener Tegangan 5.1V?", "id": "Dioda Zener 1N4733A." },
  { "en": "Gerbang Logika NOT Enam Inverter?", "id": "IC 7404." },
  { "en": "Gerbang Logika AND Empat Gerbang?", "id": "IC 7408." },
  { "en": "Gerbang Logika OR Empat Gerbang?", "id": "IC 7432." },
  { "en": "Mikrokontroler 8-bit Keluarga AVR?", "id": "ATmega328." },
  { "en": "Regulator Tegangan Yang Dapat Diatur?", "id": "LM317." },
  { "en": "Transistor Daya NPN Untuk Amplifier?", "id": "TIP31." },
  { "en": "Optocoupler Untuk Isolasi Sinyal?", "id": "PC817." },
  { "en": "Gerbang Logika NAND Empat Gerbang?", "id": "IC 7400." },
  { "en": "Penguat Audio Daya Rendah?", "id": "LM386." },
  { "en": "Transistor Efek Medan Kanal-N?", "id": "2N7000." },
  { "en": "Dioda Sinyal Kecil Switching Cepat?", "id": "Dioda 1N4148." },
  { "en": "LED (Light Emitting Diode) Merah Standar?", "id": "LED Merah 5mm." },
  { "en": "Kapasitor Keramik Nilai 100nF?", "id": "Kapasitor 104." },
  { "en": "Resistor Nilai 1 Kilo Ohm?", "id": "Resistor 1K." },
  { "en": "Potensiometer Putar Nilai 10K Ohm?", "id": "Potensiometer 10K." },
  { "en": "Regulator Tegangan Positif 12V?", "id": "IC 7812." },
  { "en": "Regulator Tegangan Negatif 12V?", "id": "IC 7912." },
  { "en": "Transistor Darlington NPN?", "id": "TIP120." },
  { "en": "Dioda Bridge Penyearah Gelombang Penuh?", "id": "Dioda Bridge DB107." },
  { "en": "Gerbang Logika NOR Empat Gerbang?", "id": "IC 7402." },
  { "en": "IC Counter Dekade?", "id": "IC 4017." },
  { "en": "IC Dekoder BCD Ke 7-Segment?", "id": "IC 7447." },
  { "en": "Papan Pengembangan Berbasis ATmega328?", "id": "Arduino Uno." },
  { "en": "Komputer Papan Tunggal Populer?", "id": "Raspberry Pi 4." },
  { "en": "Sensor Suhu Analog?", "id": "Sensor LM35." },
  { "en": "Sensor Jarak Ultrasonik?", "id": "Sensor HC-SR04." },
  { "en": "Modul Relay Untuk Mikrokontroler?", "id": "Modul Relay 5V." },
  { "en": "Motor Servo Posisi Sudut?", "id": "Motor Servo SG90." },
  { "en": "Layar LCD Karakter?", "id": "LCD 16x2." },
  { "en": "IC Penggeser Register 8-Bit?", "id": "IC 74HC595." },
  { "en": "Op-Amp Presisi Tinggi?", "id": "OP07." },
  { "en": "Transistor MOSFET Kanal-N Daya?", "id": "IRF540N." },
  { "en": "Dioda Schottky Penurunan Tegangan Rendah?", "id": "Dioda 1N5817." },
  { "en": "Regulator Switching Step-Down?", "id": "LM2596." },
  { "en": "Gerbang Logika XOR Empat Gerbang?", "id": "IC 7486." },
  { "en": "IC Multiplexer Analog 8-Channel?", "id": "IC 4051." },
  { "en": "Referensi Tegangan Presisi?", "id": "TL431." },
  { "en": "Transistor JFET Kanal-N?", "id": "J201." },
  { "en": "Kapasitor Elektrolit 1000uF 25V?", "id": "Elco 1000uF 25V." },
  { "en": "Kristal Osiator Untuk Mikrokontroler?", "id": "Kristal 16MHz." },
  { "en": "Buzzer Penghasil Suara?", "id": "Buzzer Aktif 5V." },
  { "en": "Resistor Variabel Peka Cahaya?", "id": "LDR (Light Dependent Resistor)." },
  { "en": "Mikrofon Kondenser Elektret?", "id": "Mikrofon Elektret." },
  { "en": "Thyristor Untuk Kontrol Daya AC?", "id": "SCR (Silicon Controlled Rectifier) TYN612." },
  { "en": "TRIAC Untuk Switching Beban AC?", "id": "TRIAC BT136." },
  { "en": "IC Driver Motor DC?", "id": "IC L293D." },
  { "en": "Sensor Suhu Dan Kelembaban Digital?", "id": "Sensor DHT11." },
  { "en": "LED (Light Emitting Diode) RGB (Red, Green, Blue) Umum?", "id": "LED RGB 5mm." },
  { "en": "Fototransistor Pendeteksi Cahaya?", "id": "Fototransistor." },
  { "en": "Saklar Tombol Tekan Sementara?", "id": "Push Button." },
  { "en": "Baterai Lithium-ion Isi Ulang?", "id": "Baterai 18650." },
  { "en": "Regulator Tegangan Positif 9V?", "id": "IC 7809." },
  { "en": "Transistor NPN Untuk Switching?", "id": "BC547." },
  { "en": "Transistor PNP Untuk Switching?", "id": "BC557." },
  { "en": "Dioda Zener Tegangan 12V?", "id": "Dioda Zener 1N4742A." },
  { "en": "Gerbang Logika NAND Schmitt Trigger?", "id": "IC 74LS132." },
  { "en": "Op-Amp Ganda (Dual Op-Amp)?", "id": "IC LM358." },
  { "en": "Komparator Tegangan Ganda?", "id": "IC LM393." },
  { "en": "Sensor Gerak Inframerah Pasif?", "id": "Sensor PIR HC-SR501." },
  { "en": "Modul Bluetooth Untuk Mikrokontroler?", "id": "Modul HC-05." },
  { "en": "Modul Wi-Fi Berbasis Mikrokontroler?", "id": "ESP8266." },
  { "en": "IC EEPROM Serial?", "id": "IC 24C02." },
  { "en": "IC Real-Time Clock (RTC)?", "id": "IC DS1307." },
  { "en": "Transistor MOSFET Kanal-P?", "id": "IRF9540N." },
  { "en": "Induktor Toroida Penyimpan Energi?", "id": "Induktor Ferit." },
  { "en": "Kapasitor Variabel Untuk Tuning?", "id": "Varco (Variable Capacitor)." },
  { "en": "Sekering Pelindung Arus Berlebih?", "id": "Fuse Kaca 1A." },
  { "en": "Thermistor Pengukur Suhu?", "id": "NTC 10K." },
  { "en": "Voltage Controlled Oscillator (VCO)?", "id": "IC 4046." },
  { "en": "IC Penguat RF (Radio Frequency)?", "id": "INA-02184." },
  { "en": "Dioda Varactor Untuk Tuning Frekuensi?", "id": "BB139." },
  { "en": "Filter Keramik Untuk Frekuensi Radio?", "id": "Filter IF 455KHz." },
  { "en": "IC Radio FM Chip Tunggal?", "id": "RDA5807M." },
  { "en": "Konektor Audio Stereo 3.5mm?", "id": "Jack Audio 3.5mm." },
  { "en": "Konektor Daya DC Barel?", "id": "Jack DC Barel." },
  { "en": "Header Pin Untuk Koneksi Papan?", "id": "Pin Header Male." },
  { "en": "Gerbang Logika AND Tiga Input?", "id": "IC 74LS11." },
  { "en": "IC Flip-Flop Tipe D Ganda?", "id": "IC 74LS74." },
  { "en": "IC ADC (Analog-to-Digital Converter) 8-bit?", "id": "ADC0804." },
  { "en": "IC DAC (Digital-to-Analog Converter) 8-bit?", "id": "DAC0808." },
  { "en": "Regulator Tegangan Negatif Yang Dapat Diatur?", "id": "LM337." },
  { "en": "Sensor Efek Hall Untuk Deteksi Magnet?", "id": "Sensor Hall A3144." },
  { "en": "Dioda Laser Merah?", "id": "Modul Laser KY-008." },
  { "en": "Papan Pengembangan Berbasis ESP32?", "id": "NodeMCU ESP32." },
  { "en": "IC Logika CMOS Gerbang NAND?", "id": "IC 4011." },
  { "en": "Transistor Unijunction (UJT)?", "id": "2N2646." },
  { "en": "Dioda PIN Untuk Switching RF?", "id": "Dioda BAR63." },
  { "en": "IC Penguat Operasional Quad?", "id": "LM324." },
  { "en": "Sensor Tekanan Barometrik?", "id": "BMP180." },
  { "en": "Modul Pembaca Kartu RFID?", "id": "MFRC522." },
  { "en": "LED (Light Emitting Diode) Inframerah Pemancar?", "id": "LED IR Pemancar." },
  { "en": "Penerima Inframerah Untuk Remote?", "id": "Sensor IR VS1838B." },
  { "en": "Op-Amp JFET-Input Ganda?", "id": "IC TL072." },
  { "en": "Regulator Tegangan 3.3V LDO?", "id": "AMS1117-3.3." },
  { "en": "Transistor Daya PNP Pelengkap TIP31?", "id": "TIP32C." },
  { "en": "Driver Motor H-Bridge Ganda?", "id": "IC L298N." },
  { "en": "Sensor Akselerometer Dan Giroskop 6-Axis?", "id": "MPU-6050." },
  { "en": "Mikrokontroler ARM Cortex-M3 Papan Biru?", "id": "STM32F103C8T6." },
  { "en": "Modul GPS Untuk Proyek Arduino?", "id": "Modul GPS NEO-6M." },
  { "en": "Gerbang Logika XNOR Empat Gerbang?", "id": "IC 74LS266." },
  { "en": "Dioda Zener Tegangan 3.3V?", "id": "Dioda Zener 1N4728A." },
  { "en": "Transistor NPN Frekuensi Radio?", "id": "2SC945." },
  { "en": "LED (Light Emitting Diode) Biru Terang?", "id": "LED Biru 5mm." },
  { "en": "LED (Light Emitting Diode) Hijau Standar?", "id": "LED Hijau 5mm." },
  { "en": "IC Dekoder Biner Ke Desimal?", "id": "IC 74LS42." },
  { "en": "Komparator Tegangan Quad (Empat)?", "id": "LM339." },
  { "en": "Optocoupler Dengan Output Darlington?", "id": "4N33." },
  { "en": "Regulator Tegangan Negatif 9V?", "id": "IC 7909." },
  { "en": "Sensor Suhu Digital Protokol I2C?", "id": "DS18B20." },
  { "en": "Modul Kartu SD Untuk Mikrokontroler?", "id": "Modul MicroSD." },
  { "en": "IC Penguat Audio Stereo Kelas AB?", "id": "TDA2030." },
  { "en": "Transistor MOSFET Kanal-N Mode Peningkatan?", "id": "BS170." },
  { "en": "Dioda Pemancar Cahaya Ultraviolet?", "id": "LED UV 5mm." },
  { "en": "IC Gerbang Logika AND Schmitt Trigger?", "id": "IC 74HC08." },
  { "en": "Papan Pengembangan WiFi Dan Bluetooth?", "id": "ESP32." },
  { "en": "Sensor Arus Listrik?", "id": "Sensor ACS712." },
  { "en": "IC Latch 8-Bit?", "id": "IC 74HC373." },
  { "en": "Potensiometer Geser (Slider)?", "id": "Slide Potentiometer 10K." },
  { "en": "Transistor Fotolistrik NPN?", "id": "L14F1." },
  { "en": "Dioda Bridge Miniatur 1A?", "id": "W04M." },
  { "en": "IC Counter Biner 4-Bit?", "id": "IC 74HC93." },
  { "en": "Regulator Tegangan Positif 15V?", "id": "IC 7815." },
  { "en": "Regulator Tegangan Negatif 15V?", "id": "IC 7915." },
  { "en": "Transistor Darlington PNP Daya?", "id": "TIP127." },
  { "en": "Dioda Zener Tegangan 9.1V?", "id": "Dioda Zener 1N4739A." },
  { "en": "Gerbang Logika OR Tiga Input?", "id": "IC 74LS27." },
  { "en": "Op-Amp Tunggal Presisi Tinggi?", "id": "OP27." },
  { "en": "Sensor Kelembaban Tanah Resistif?", "id": "Sensor YL-69." },
  { "en": "Modul Jam Digital Berbasis DS3231?", "id": "Modul RTC DS3231." },
  { "en": "IC Penguat Audio Kelas D Mono?", "id": "PAM8403." },
  { "en": "Transistor MOSFET Kanal-P Logika?", "id": "FQP27P06." },
  { "en": "IC Driver LED 16-Channel?", "id": "TLC5940." },
  { "en": "Sensor Getaran Piezoelektrik?", "id": "Sensor Piezo." },
  { "en": "Layar OLED Grafis Antarmuka I2C?", "id": "OLED 128x64." },
  { "en": "IC Multiplexer Digital 8-ke-1?", "id": "IC 74HC151." },
  { "en": "Regulator Switching Step-Up (Boost)?", "id": "Modul MT3608." },
  { "en": "Transistor NPN Daya Tinggi?", "id": "2N3055." },
  { "en": "Dioda Varactor Pengganti Varco?", "id": "BB910." },
  { "en": "Gerbang Logika NOR Tiga Input?", "id": "IC 4025." },
  { "en": "IC Antarmuka CAN Bus?", "id": "MCP2515." },
  { "en": "Sensor Warna Digital?", "id": "TCS3200." },
  { "en": "Modul Keypad Matriks 4x4?", "id": "Keypad 4x4." },
  { "en": "IC Buffer Non-Inverting?", "id": "IC 74HC244." },
  { "en": "Potensiometer Multi-Putaran (Trimpot)?", "id": "Trimpot 3296W." },
  { "en": "LED (Light Emitting Diode) Putih Super Terang?", "id": "LED Putih 5mm." },
  { "en": "Solenoida Pendorong Atau Penarik?", "id": "Solenoid Push-Pull 5V." },
  { "en": "Sensor Level Air?", "id": "Sensor Ketinggian Air." },
  { "en": "IC Penguat Instrumental?", "id": "INA125." },
  { "en": "Dioda Germanium Sinyal Radio?", "id": "Dioda 1N34A." },
  { "en": "Regulator Tegangan Positif 1.8V?", "id": "IC 7818." },
  { "en": "Transistor PNP Frekuensi Tinggi?", "id": "2SA733." },
  { "en": "Gerbang Logika CMOS Inverter Hex?", "id": "IC 4049." },
  { "en": "IC Op-Amp Audio Kualitas Tinggi?", "id": "NE5532." },
  { "en": "Sensor Gas Mudah Terbakar?", "id": "Sensor MQ-2." },
  { "en": "Modul Pembaca Sidik Jari?", "id": "Sensor FPM10A." },
  { "en": "IC Logika Flip-Flop JK Ganda?", "id": "IC 74HC109." },
  { "en": "Motor Stepper Bipolar?", "id": "Motor Stepper NEMA 17." },
  { "en": "Driver Motor Stepper?", "id": "Driver A4988." },
  { "en": "Dioda Penyearah Cepat (Fast Recovery)?", "id": "UF4007." },
  { "en": "IC ADC 10-bit SPI?", "id": "MCP3008." },
  { "en": "Regulator Tegangan Negatif 18V?", "id": "IC 7918." },
  { "en": "Sensor Api (Flame Sensor)?", "id": "Modul Sensor Api." },
  { "en": "LED (Light Emitting Diode) Kuning Standar?", "id": "LED Kuning 5mm." },
  { "en": "IC Komparator Empat Saluran?", "id": "IC LM2901." },
  { "en": "Transistor MOSFET Kanal-N SMD?", "id": "AO3400." },
  { "en": "IC Buffer Inverting Hex?", "id": "IC 74HC04." },
  { "en": "Dioda Zener Tegangan 24V?", "id": "Dioda Zener 1N4749A." },
  { "en": "IC Penguat Audio Jembatan (Bridge)?", "id": "TDA2822." },
  { "en": "Sensor Detak Jantung?", "id": "Sensor Pulse." },
  { "en": "Modul Ethernet Untuk Arduino?", "id": "Modul W5100." },
  { "en": "Gerbang Logika NAND Open-Collector?", "id": "IC 7401." },
  { "en": "Regulator Tegangan Positif 24V?", "id": "IC 7824." },
  { "en": "Transistor NPN Untuk Penguat Audio?", "id": "C945." },
  { "en": "Dioda Schottky Daya?", "id": "MBR10100." },
  { "en": "IC Switch Analog Empat Saluran?", "id": "IC 4066." },
  { "en": "Sensor Suhu Dan Kelembaban Akurasi Tinggi?", "id": "Sensor DHT22." },
  { "en": "Modul Joystick Analog?", "id": "Joystick KY-023." },
  { "en": "IC Penerjemah Level Logika?", "id": "Modul Logic Level Converter." },
  { "en": "Transistor IGBT (Insulated Gate Bipolar Transistor)?", "id": "FGA25N120." },
  { "en": "LED (Light Emitting Diode) Dot Matrix 8x8?", "id": "Modul Dot Matrix." },
  { "en": "IC Driver Tampilan 7-Segment?", "id": "MAX7219." },
  { "en": "Sensor Giroskop 3-Axis?", "id": "L3G4200D." },
  { "en": "Regulator Tegangan Negatif 24V?", "id": "IC 7924." },
  { "en": "Dioda TVS (Transient Voltage Suppressor)?", "id": "P6KE6.8A." },
  { "en": "IC Penguat Logaritmik?", "id": "AD8307." },
  { "en": "Sensor Tekanan Udara Presisi?", "id": "BMP280." },
  { "en": "Transistor NPN Darlington Daya?", "id": "TIP142." },
  { "en": "IC Pengontrol PWM?", "id": "TL494." },
  { "en": "Dioda Laser Daya Tinggi?", "id": "Dioda Laser 1W." },
  { "en": "Op-Amp FET-Input Tunggal?", "id": "CA3140." },
  { "en": "Sensor Kualitas Udara?", "id": "Sensor MQ-135." },
  { "en": "Modul Pengenalan Suara?", "id": "Modul Voice Recognition V3." },
  { "en": "Papan Prototipe Tanpa Solder?", "id": "Breadboard MB-102." },
  { "en": "Kabel Jumper Untuk Prototipe?", "id": "Kabel Jumper Male-to-Male." },
  { "en": "Transistor NPN Daya Sedang?", "id": "BD139." },
  { "en": "Transistor PNP Daya Sedang?", "id": "BD140." },
  { "en": "Dioda Zener Tegangan 4.7V?", "id": "Dioda Zener 1N4732A." },
  { "en": "Gerbang Logika Buffer Octal Tri-State?", "id": "IC 74LS245." },
  { "en": "IC Power Management Unit (PMU)?", "id": "AXP209." },
  { "en": "Op-Amp Output Rail-to-Rail?", "id": "MCP6002." },
  { "en": "Sensor Magnetometer 3-Axis?", "id": "HMC5883L." },
  { "en": "IC Driver Servo 16-Channel I2C?", "id": "PCA9685." },
  { "en": "LED (Light Emitting Diode) Addressable Individual?", "id": "LED WS2812B." },
  { "en": "Papan Pengembangan Mikrokontroler Sangat Kecil?", "id": "Arduino Pro Mini." },
  { "en": "Modul Komunikasi Jarak Jauh LoRa?", "id": "SX1278." },
  { "en": "Sensor Kelembaban Kapasitif Presisi?", "id": "SHT31." },
  { "en": "IC Penguat Audio Kelas D Efisiensi Tinggi?", "id": "TPA3116D2." },
  { "en": "Transistor NPN VHF/UHF?", "id": "BFR91A." },
  { "en": "LED (Light Emitting Diode) Inframerah Daya Tinggi?", "id": "LED IR 1W." },
  { "en": "IC Inverter Schmitt Trigger Hex?", "id": "IC 74HC14." },
  { "en": "Modul GSM/GPRS Untuk Komunikasi Seluler?", "id": "SIM800L." },
  { "en": "Sensor Debu Optik?", "id": "Sensor Sharp GP2Y10." },
  { "en": "Dioda Bridge Penyearah 3A?", "id": "KBL406." },
  { "en": "Transistor MOSFET Switching Cepat SMD?", "id": "2N7002." },
  { "en": "IC Pengontrol Baterai Lithium?", "id": "TP4056." },
  { "en": "Op-Amp JFET-Input Kuadrupel?", "id": "TL084." },
  { "en": "Sensor Efek Hall Latching?", "id": "US1881." },
  { "en": "IC Driver Tampilan VFD (Vacuum Fluorescent Display)?", "id": "PT6311." },
  { "en": "LED (Light Emitting Diode) Oranye Standar?", "id": "LED Oranye 5mm." },
  { "en": "IC Encoder Prioritas 8-ke-3?", "id": "IC 74LS148." },
  { "en": "Papan Pengembangan Berbasis ATtiny?", "id": "Digispark ATtiny85." },
  { "en": "Modul Pemutar MP3?", "id": "Modul DFPlayer Mini." },
  { "en": "Sensor Cahaya Ambient Digital?", "id": "BH1750." },
  { "en": "IC Penguat Audio Stereo Daya Sedang?", "id": "TDA7297." },
  { "en": "Transistor PNP RF (Radio Frequency)?", "id": "PN2907A." },
  { "en": "IC Counter Biner Sinkron 4-Bit?", "id": "IC 74HC161." },
  { "en": "Regulator Tegangan Positif 18V?", "id": "IC 7818." },
  { "en": "Dioda Zener Tegangan 15V?", "id": "Dioda Zener 1N4744A." },
  { "en": "IC Comparator Jendela (Window Comparator)?", "id": "ICL8038." },
  { "en": "Optocoupler Kecepatan Tinggi?", "id": "6N137." },
  { "en": "Sensor Akselerometer 3-Axis Analog?", "id": "ADXL335." },
  { "en": "Modul RFID Frekuensi Tinggi?", "id": "Modul PN532." },
  { "en": "LED (Light Emitting Diode) Bicolor Dua Warna?", "id": "LED Bicolor Merah Hijau." },
  { "en": "IC Demultiplexer 1-ke-8?", "id": "IC 74HC138." },
  { "en": "Transistor Array NPN?", "id": "ULN2003A." },
  { "en": "IC ADC 16-Bit Resolusi Tinggi?", "id": "ADS1115." },
  { "en": "Sensor Suhu Inframerah Non-Kontak?", "id": "MLX90614." },
  { "en": "Kapasitor Tantalum ESR (Equivalent Series Resistance) Rendah?", "id": "Kapasitor Tantalum 10uF." },
  { "en": "IC Gerbang AND Open-Collector?", "id": "IC 7409." },
  { "en": "Regulator Negatif Tegangan 3.3V?", "id": "LD1117V33." },
  { "en": "Transistor PNP Daya Darlington?", "id": "TIP147." },
  { "en": "IC Penguat Audio Mono 20W?", "id": "LM1875." },
  { "en": "Dioda Gunn Untuk Osilator Microwave?", "id": "Dioda Gunn MA4910." },
  { "en": "Sensor pH Untuk Mengukur Keasaman?", "id": "Sensor pH E-201C." },
  { "en": "Modul Pembaca Barcode?", "id": "Modul Barcode Scanner." },
  { "en": "IC Flip-Flop Tipe T?", "id": "CD4013B." },
  { "en": "LED (Light Emitting Diode) UV-C Untuk Sterilisasi?", "id": "LED UVC 275nm." },
  { "en": "IC Penguat RF (Radio Frequency) LNA (Low-Noise Amplifier)?", "id": "SPF5189Z." },
  { "en": "Relay Solid State (SSR)?", "id": "SSR-40DA." },
  { "en": "Dioda Penyearah Jembatan 35A?", "id": "Bridge Rectifier GBJ3510." },
  { "en": "IC ADC Audio Stereo?", "id": "PCM1802." },
  { "en": "Sensor Karbon Dioksida (CO2)?", "id": "MG811." },
  { "en": "Transistor Digital Dengan Resistor Internal?", "id": "DTC114EKA." },
  { "en": "IC Penguat Logaritmik Presisi?", "id": "LOG112." },
  { "en": "Varistor Pelindung Tegangan Lebih?", "id": "MOV (Metal Oxide Varistor)." },
  { "en": "IC Gerbang Logika NOR Open-Drain?", "id": "IC 74LS02." },
  { "en": "Sensor Sidik Jari Kapasitif?", "id": "Sensor FPC1020A." },
  { "en": "Modul Display E-Paper (Tinta Elektronik)?", "id": "Layar E-Ink 2.9 Inci." },
  { "en": "LED (Light Emitting Diode) Straw Hat Sudut Lebar?", "id": "LED Straw Hat 5mm." },
  { "en": "IC Phase-Locked Loop (PLL)?", "id": "IC 4046." },
  { "en": "Regulator Tegangan Switching Sinkron?", "id": "MP2307." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "IGBT (Insulated Gate Bipolar Transistor)." },
  { "en": "Dioda Pemulihan Langkah (Step Recovery)?", "id": "Dioda 1N5155." },
  { "en": "IC Penguat Transimpedansi?", "id": "OPA380." },
  { "en": "Sensor Oksigen Galvanik?", "id": "Sensor O2 KE-25." },
  { "en": "Modul Matriks LED RGB Addressable?", "id": "Panel LED WS2812B 8x8." },
  { "en": "IC Pengontrol Motor Servo Serial?", "id": "Maestro." },
  { "en": "Transistor NPN Frekuensi Radio Daya?", "id": "2SC1971." },
  { "en": "Dioda Zener Tegangan 6.2V?", "id": "Dioda Zener 1N4735A." },
  { "en": "Gerbang Logika NAND Tiga Input?", "id": "IC 74LS10." },
  { "en": "IC Penguat Video?", "id": "LMH6702." },
  { "en": "Sensor Kecepatan Angin Anemometer?", "id": "Anemometer." },
  { "en": "Modul Konverter USB Ke Serial?", "id": "CH340G." },
  { "en": "LED (Light Emitting Diode) Chip On Board (COB)?", "id": "LED COB 10W." },
  { "en": "IC DAC Audio Stereo Resolusi Tinggi?", "id": "PCM5102A." },
  { "en": "Transistor Efek Medan Gerbang Persimpangan?", "id": "JFET (Junction Field-Effect Transistor)." },
  { "en": "Dioda Tunnel Untuk Osilator?", "id": "Dioda Tunnel 1N3712." },
  { "en": "IC Multiplexer Video?", "id": "MAX4545." },
  { "en": "Sensor Getaran Merkuri?", "id": "Tilt Sensor SW-520D." },
  { "en": "Modul Pengukur Jarak Laser?", "id": "Sensor VL53L0X." },
  { "en": "IC Pembanding (Comparator) Dengan Referensi Internal?", "id": "LM311." },
  { "en": "Transistor PNP Komplementer BD139?", "id": "BD140." },
  { "en": "Dioda Penekan Tegangan Transien?", "id": "TVS Diode." },
  { "en": "Gerbang Logika Inverter Open-Collector?", "id": "IC 7405." },
  { "en": "Sensor Kelembaban Dan Suhu I2C?", "id": "Si7021." },
  { "en": "Modul Penguat Mikrofon?", "id": "Modul MAX9814." },
  { "en": "IC DAC Empat Saluran (Quad)?", "id": "DAC8564." },
  { "en": "Transistor Array PNP?", "id": "UDN2981A." },
  { "en": "IC Konverter Tegangan Ke Frekuensi?", "id": "AD654." },
  { "en": "Dioda Pemancar Laser (Laser Diode)?", "id": "LD Merah 650nm." },
  { "en": "Sensor Detak Jantung Dan Oksigen Darah?", "id": "MAX30100." },
  { "en": "LED (Light Emitting Diode) Inframerah Sisi?", "id": "Side-looking IR LED." },
  { "en": "Transistor NPN Penguat Daya Audio?", "id": "2SC5200." },
  { "en": "Transistor PNP Pasangan 2SC5200?", "id": "2SA1943." },
  { "en": "Regulator Tegangan Positif Adjustable L78?", "id": "L200." },
  { "en": "Dioda Zener Tegangan 7.5V?", "id": "Dioda Zener 1N4737A." },
  { "en": "Gerbang Logika NAND Tiga Input Kolektor Terbuka?", "id": "IC 7412." },
  { "en": "IC Penguat Daya Audio Kelas AB 100W?", "id": "LM3886." },
  { "en": "Transistor MOSFET Dual N-Channel?", "id": "Si4554DY." },
  { "en": "Dioda Bridge Tiga Fasa?", "id": "SQL100A." },
  { "en": "Sensor Giroskop Digital SPI/I2C?", "id": "L3GD20H." },
  { "en": "Modul Tampilan 7-Segment 4-Digit?", "id": "Modul TM1637." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Daya Tinggi?", "id": "LED UV 3W 395nm." },
  { "en": "IC Counter Up/Down Biner 4-Bit?", "id": "IC 74HC193." },
  { "en": "IC Driver Motor Stepper Bipolar?", "id": "DRV8825." },
  { "en": "Sensor Tekanan Fleksibel Piezo-Resistif?", "id": "Sensor FSR 402." },
  { "en": "Op-Amp Zero-Drift Presisi?", "id": "ADA4522." },
  { "en": "Regulator Tegangan Positif Ultra LDO?", "id": "MIC29302." },
  { "en": "Transistor NPN Fotodarlington?", "id": "2N5777." },
  { "en": "Papan Prototipe Dengan Jalur PCB?", "id": "Perfboard." },
  { "en": "Modul Sensor Suara Dengan Komparator?", "id": "Modul KY-038." },
  { "en": "IC Penguat Audio Mobil 4 Saluran?", "id": "TDA7388." },
  { "en": "Transistor PNP Epitaxial Silikon?", "id": "KSA1015." },
  { "en": "LED (Light Emitting Diode) Inframerah Kecepatan Tinggi?", "id": "SFH 4550." },
  { "en": "IC DAC Audio Stereo Kinerja Tinggi?", "id": "CS4398." },
  { "en": "Papan Pengembangan ARM Mbed?", "id": "NXP LPC1768." },
  { "en": "Sensor Gas Amonia (NH3)?", "id": "Sensor MQ-137." },
  { "en": "Transistor Array Darlington NPN Beban Tinggi?", "id": "ULN2803A." },
  { "en": "IC Penguat Lock-In?", "id": "AD630." },
  { "en": "Dioda IMPATT Untuk Osilator Microwave?", "id": "Dioda IMPATT." },
  { "en": "IC Penguat Isolasi?", "id": "ISO124." },
  { "en": "Sensor Giroskop Getar (Vibrating Structure Gyroscope)?", "id": "ENC-03R." },
  { "en": "LED (Light Emitting Diode) Superflux Piranha?", "id": "LED Piranha Putih." },
  { "en": "Gerbang Logika NAND CMOS Schmitt Trigger?", "id": "IC 4093." },
  { "en": "IC Pengontrol Motor Brushless DC (BLDC)?", "id": "DRV10983." },
  { "en": "Regulator Switching Buck-Boost?", "id": "LTC3780." },
  { "en": "Dioda Zener Tegangan 33V?", "id": "Dioda Zener 1N4752A." },
  { "en": "IC ADC Delta-Sigma Resolusi Tinggi?", "id": "ADS1256." },
  { "en": "Sensor IMU (Inertial Measurement Unit) 9-DOF?", "id": "BNO055." },
  { "en": "Transistor Germanium PNP?", "id": "AC128." },
  { "en": "IC Penguat Operasional Video?", "id": "AD811." },
  { "en": "Modul Kamera Untuk Raspberry Pi?", "id": "Modul Kamera Pi V2." },
  { "en": "IC Switch Analog Latch-Up Proof?", "id": "ADG5412." },
  { "en": "LED (Light Emitting Diode) UV-A Blacklight?", "id": "LED UVA 365nm." },
  { "en": "IC Multiplexer/Demultiplexer Analog Ganda?", "id": "CD4053B." },
  { "en": "Sensor Gerakan Microwave Doppler?", "id": "Modul RCWL-0516." },
  { "en": "Transistor Bipolar Heterojunction (HBT)?", "id": "HBT." },
  { "en": "Dioda Penyearah Jembatan 50A?", "id": "Bridge KBPC5010." },
  { "en": "IC DAC Audio Kinerja Tinggi?", "id": "AK4490." },
  { "en": "Sensor Gas Hidrogen Sulfida (H2S)?", "id": "Sensor ME3-H2S." },
  { "en": "IC Flip-Flop JK Dengan Preset Dan Clear?", "id": "IC 74HC76." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 0805?", "id": "LED SMD 0805." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Kelas E?", "id": "RFM69HCW." },
  { "en": "Regulator Tegangan Shunt Presisi?", "id": "LM4040." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "MOSFET." },
  { "en": "Dioda Supresor Tegangan Transien?", "id": "TVS Diode 1.5KE." },
  { "en": "IC Penguat Audio Portabel?", "id": "TPA6120A2." },
  { "en": "Sensor Kekeruhan Air (Turbidity)?", "id": "Sensor TSW-30." },
  { "en": "Modul Display LCD TFT Touchscreen?", "id": "Layar TFT 3.5 Inci." },
  { "en": "IC Gerbang AND Tiga Input CMOS?", "id": "IC 4073." },
  { "en": "Transistor NPN Untuk Aplikasi TV?", "id": "BU508." },
  { "en": "Dioda Zener Tegangan 2.7V?", "id": "Dioda Zener 1N4729A." },
  { "en": "IC Penguat Logaritmik RMS-to-DC?", "id": "AD8362." },
  { "en": "Sensor Getaran SW-420?", "id": "Modul Sensor Getar." },
  { "en": "LED (Light Emitting Diode) Strobo Xenon?", "id": "Lampu Flash." },
  { "en": "IC Switch Analog SPDT (Single Pole Double Throw)?", "id": "ADG706." },
  { "en": "Regulator Switching SEPIC?", "id": "LT8494." },
  { "en": "Transistor PNP Komplementer 2N3055?", "id": "MJ2955." },
  { "en": "Dioda Pemulihan Cepat Ultra Cepat?", "id": "MUR460." },
  { "en": "IC ADC SAR (Successive Approximation Register)?", "id": "ADS7830." },
  { "en": "Sensor Gas Karbon Monoksida (CO)?", "id": "Sensor MQ-7." },
  { "en": "Modul Display LCD Grafis?", "id": "LCD Grafis 128x64." },
  { "en": "IC Gerbang OR Kolektor Terbuka?", "id": "IC 74LS33." },
  { "en": "Transistor NPN Amplifier Kebisingan Rendah?", "id": "2SC2240." },
  { "en": "Dioda Penyearah Tegangan Tinggi?", "id": "NTE558." },
  { "en": "IC Penguat Audio Headphone?", "id": "TPA6120." },
  { "en": "Sensor Hujan?", "id": "Modul Sensor Hujan YL-83." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 1206?", "id": "LED SMD 1206." },
  { "en": "IC Penguat RF (Radio Frequency) Daya LDMOS?", "id": "BLF188XR." },
  { "en": "Regulator Tegangan Linear Kebisingan Rendah?", "id": "TPS7A4701." },
  { "en": "Transistor Efek Medan Statik Induksi?", "id": "SIT (Static Induction Transistor)." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge)?", "id": "Dioda ESD." },
  { "en": "IC Penguat Diferensial Penuh?", "id": "AD8138." },
  { "en": "Sensor pH Tanah?", "id": "Sensor pH Tanah." },
  { "en": "Modul Kamera Termal?", "id": "Modul Lepton FLIR." },
  { "en": "IC Gerbang NOR CMOS Tiga Input?", "id": "IC 4025B." },
  { "en": "Transistor PNP Amplifier Kebisingan Rendah?", "id": "2SA970." },
  { "en": "Dioda Zener Tegangan 47V?", "id": "Dioda Zener 1N4756A." },
  { "en": "IC Penguat Distribusi Video?", "id": "MAX4310." },
  { "en": "Sensor Kualitas Udara Dalam Ruangan?", "id": "Sensor CCS811." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Penglihatan Malam?", "id": "IR Illuminator." },
  { "en": "IC Switch Analog Video?", "id": "MAX4886." },
  { "en": "Regulator Switching Cuk Converter?", "id": "LM3478." },
  { "en": "Transistor Darlington Array NPN?", "id": "M-CSA04." },
  { "en": "Dioda Penyearah Jembatan Cepat?", "id": "GBU806." },
  { "en": "IC ADC Flash?", "id": "ADC0820." },
  { "en": "Sensor Gas Ozon (O3)?", "id": "Sensor MQ-131." },
  { "en": "Modul Pembaca E-KTP?", "id": "Card Reader E-KTP." },
  { "en": "IC Flip-Flop RS (Reset-Set)?", "id": "IC 4043." },
  { "en": "Papan Pengembangan Wearable (Bisa Dipakai)?", "id": "LilyPad Arduino." },
  { "en": "IC Penguat Operasional Umpan Balik Arus?", "id": "AD8009." },
  { "en": "Transistor MOSFET N-Channel Level Logika?", "id": "IRLZ44N." },
  { "en": "Dioda Penyearah Jembatan 1000V 50A?", "id": "GBPC5010." },
  { "en": "Sensor Akselerometer Dan Magnetometer?", "id": "LSM303DLHC." },
  { "en": "Modul Tampilan LCD Monokrom Jadul?", "id": "LCD Nokia 5110." },
  { "en": "LED (Light Emitting Diode) UV-B Untuk Aplikasi Medis?", "id": "LED UVB 310nm." },
  { "en": "IC Counter Johnson Dengan Output Dekode?", "id": "CD4022BE." },
  { "en": "IC Driver Motor DC Arus Tinggi?", "id": "VNH2SP30." },
  { "en": "Sensor Tekanan Udara Absolut (MAP)?", "id": "MPX4250AP." },
  { "en": "Op-Amp Kebisingan Sangat Rendah?", "id": "LT1028." },
  { "en": "Regulator Tegangan Negatif Adjustable LDO?", "id": "LT3015." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5779." },
  { "en": "Dioda Zener Tegangan 3.6V?", "id": "1N4730A." },
  { "en": "Gerbang Logika AND Schmitt-Trigger Quad?", "id": "HCF4081B." },
  { "en": "Kabel Pita Pelangi Untuk Koneksi?", "id": "Rainbow Ribbon Cable." },
  { "en": "Modul Sensor Suara Analog?", "id": "Modul KY-037." },
  { "en": "IC Penguat Audio Bluetooth?", "id": "TDA7492P." },
  { "en": "Transistor NPN Epitaxial Pelengkap KSA1015?", "id": "KSC2383." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Komunikasi Optik?", "id": "SFH 485 P." },
  { "en": "IC ADC Audio Kinerja Tinggi?", "id": "CS5361." },
  { "en": "Papan Pengembangan Berbasis FPGA?", "id": "Altera DE10-Nano." },
  { "en": "Sensor Gas Alkohol?", "id": "Sensor MQ-3." },
  { "en": "Transistor Array PNP Beban Tinggi?", "id": "UDN2987A." },
  { "en": "IC Konverter RMS-to-DC?", "id": "AD736." },
  { "en": "Dioda Kapasitansi Variabel (Varicap)?", "id": "BB204." },
  { "en": "IC Penguat Diferensial Video?", "id": "AD830." },
  { "en": "Sensor Kelembaban Dan Suhu SHT20?", "id": "SHT20." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 0603?", "id": "LED SMD 0603." },
  { "en": "Gerbang Logika OR CMOS Tiga Input?", "id": "IC 4075." },
  { "en": "IC Pengontrol Motor Servo Cerdas?", "id": "Dynamixel AX-12A." },
  { "en": "Regulator Switching Topologi Flyback?", "id": "UCC28600." },
  { "en": "Dioda Zener Tegangan 51V?", "id": "Dioda Zener 1N4757A." },
  { "en": "IC ADC Flash 8-Bit Paralel?", "id": "CA3318." },
  { "en": "Sensor IMU (Inertial Measurement Unit) 10-DOF?", "id": "GY-87." },
  { "en": "Transistor Germanium NPN?", "id": "AC127." },
  { "en": "IC Penguat Operasional Arus Tinggi?", "id": "L272." },
  { "en": "Modul Kamera Resolusi Tinggi?", "id": "Arducam." },
  { "en": "IC Switch Analog Multiplexer Video?", "id": "PI3V340." },
  { "en": "LED (Light Emitting Diode) Penuh Warna (Full Color)?", "id": "LED RGBW." },
  { "en": "IC Multiplexer/Demultiplexer CMOS 16-Channel?", "id": "CD74HC4067." },
  { "en": "Sensor Debu Laser?", "id": "Sensor PMS5003." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "IRG4PC50W." },
  { "en": "Dioda Penyearah Jembatan SMD?", "id": "MB6S." },
  { "en": "IC DAC Audio Resolusi Tinggi?", "id": "PCM1794A." },
  { "en": "Sensor VOC (Volatile Organic Compound)?", "id": "Sensor SGP30." },
  { "en": "IC Flip-Flop JK Master-Slave?", "id": "IC 4027." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 0402?", "id": "LED SMD 0402." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk Ponsel?", "id": "SKY77643." },
  { "en": "Regulator Tegangan Linear Arus Tinggi?", "id": "LT1083." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "IRFP460." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Array?", "id": "TPD4E05U06." },
  { "en": "IC Penguat Audio Kelas H?", "id": "STA540." },
  { "en": "Sensor Konduktivitas Air?", "id": "Sensor TDS." },
  { "en": "Modul Tampilan LCD TFT Dengan SD Card?", "id": "LCD 2.4 Inci ILI9341." },
  { "en": "IC Gerbang AND CMOS Empat Gerbang?", "id": "IC 4081." },
  { "en": "Transistor PNP Untuk Aplikasi TV?", "id": "BF423." },
  { "en": "Dioda Zener Tegangan 3.9V?", "id": "Dioda Zener 1N4731A." },
  { "en": "IC Penguat Logaritmik Kecepatan Tinggi?", "id": "AD8310." },
  { "en": "Sensor Air Hujan Optik?", "id": "Sensor Hujan Optik." },
  { "en": "LED (Light Emitting Diode) Ultraviolet-C (UVC)?", "id": "LED UVC." },
  { "en": "IC Switch Analog SP4T (Single Pole Four Throw)?", "id": "ADG704." },
  { "en": "Regulator Switching Zeta Converter?", "id": "LT3757." },
  { "en": "Transistor NPN Komplementer 2SA970?", "id": "2SC2240." },
  { "en": "Dioda Pemulihan Lunak (Soft Recovery)?", "id": "DSEI60-12A." },
  { "en": "IC ADC Pipa (Pipelined ADC)?", "id": "ADS5400." },
  { "en": "Sensor Gas Metana (CH4)?", "id": "Sensor MQ-4." },
  { "en": "Modul LCD OLED Fleksibel?", "id": "Layar OLED Fleksibel." },
  { "en": "IC Gerbang NOR CMOS Empat Gerbang?", "id": "IC 4001." },
  { "en": "Transistor NPN Amplifier Kebisingan Rendah?", "id": "BC847." },
  { "en": "Dioda Penyearah Tegangan Ultra Tinggi?", "id": "HV03-12." },
  { "en": "IC Penguat Headphone Kelas G?", "id": "MAX97220A." },
  { "en": "Sensor pH Digital?", "id": "Atlas Scientific pH." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 0201?", "id": "LED SMD 0201." },
  { "en": "IC Penguat RF (Radio Frequency) Doherty?", "id": "Doherty Amplifier." },
  { "en": "Regulator Tegangan Linear PSRR Tinggi?", "id": "LP5907." },
  { "en": "Transistor Efek Medan Semikonduktor Karbon?", "id": "Graphene FET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Polimer?", "id": "PolySurg." },
  { "en": "IC Penguat Diferensial Kebisingan Rendah?", "id": "ADA4937." },
  { "en": "Sensor Oksigen Terlarut?", "id": "Sensor DO." },
  { "en": "Modul Kamera Penglihatan Malam?", "id": "Modul Kamera NoIR." },
  { "en": "IC Gerbang XOR CMOS Empat Gerbang?", "id": "IC 4070." },
  { "en": "Transistor PNP Amplifier Kebisingan Rendah?", "id": "BC857." },
  { "en": "Dioda Zener Tegangan 6.8V?", "id": "Dioda Zener 1N4736A." },
  { "en": "IC Penguat Distribusi Kabel?", "id": "AD8129." },
  { "en": "Sensor Arus Efek Hall?", "id": "WCS1800." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Internal?", "id": "LED Lensa." },
  { "en": "IC Switch Analog Dengan Proteksi Kesalahan?", "id": "ADG5412F." },
  { "en": "Regulator Switching Topologi Forward?", "id": "UCC2897A." },
  { "en": "Transistor Darlington Array PNP?", "id": "UDN2982A." },
  { "en": "Dioda Penyearah Jembatan Arus Tinggi?", "id": "GBJ5010." },
  { "en": "IC ADC Sigma-Delta Audio?", "id": "CS5340." },
  { "en": "Sensor Kualitas Udara PM2.5?", "id": "SDS011." },
  { "en": "Modul Pengenalan Wajah?", "id": "Modul ESP32-CAM." },
  { "en": "IC Flip-Flop D-Type Octal?", "id": "74HC273." },
  { "en": "Papan Pengembangan Berbasis Python?", "id": "MicroPython PyBoard." },
  { "en": "IC Penguat Operasional Tegangan Tinggi?", "id": "OPA454." },
  { "en": "Transistor MOSFET N-Channel Mode Deplesi?", "id": "BF245C." },
  { "en": "Dioda Penyearah Jembatan Dengan Heatsink?", "id": "GBPC3510." },
  { "en": "Sensor Magnetometer Dan Kompas Digital?", "id": "QMC5883L." },
  { "en": "Modul Tampilan OLED Dua Warna?", "id": "OLED 0.96 Inci." },
  { "en": "LED (Light Emitting Diode) UV-C Daya Rendah?", "id": "LED UVC 5mW." },
  { "en": "IC Counter Biner 12-Tahap?", "id": "CD4040BE." },
  { "en": "IC Driver Motor DC Dengan Kontrol PWM?", "id": "BTS7960." },
  { "en": "Sensor Tekanan Vakum?", "id": "MPXV7002DP." },
  { "en": "Op-Amp BiFET Dengan Kebocoran Rendah?", "id": "LF356." },
  { "en": "Regulator Tegangan Negatif Arus Tinggi?", "id": "LT1086." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5778." },
  { "en": "Dioda Zener Tegangan 8.2V?", "id": "1N4738A." },
  { "en": "Gerbang Logika OR Schmitt-Trigger Quad?", "id": "CD4071B." },
  { "en": "Kabel Koaksial Untuk Sinyal RF?", "id": "Kabel RG58." },
  { "en": "Modul Sensor Warna Dengan Lensa?", "id": "ISL29125." },
  { "en": "IC Penguat Audio Kelas D Stereo?", "id": "TPA3118D2." },
  { "en": "Transistor NPN Untuk Switching Cepat?", "id": "2N2222A." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Sensor?", "id": "IR Emitter." },
  { "en": "IC ADC I2S Untuk Audio?", "id": "UDA1334A." },
  { "en": "Papan Pengembangan FPGA Dari Xilinx?", "id": "Spartan-6." },
  { "en": "Sensor Gas LPG Dan Propana?", "id": "Sensor MQ-6." },
  { "en": "Transistor Array Darlington Dengan Dioda?", "id": "ULN2004A." },
  { "en": "IC Konverter Frekuensi Ke Tegangan?", "id": "LM331." },
  { "en": "Dioda Pelindung Tegangan Berlebih?", "id": "Zener Diode." },
  { "en": "IC Penguat Diferensial Kebisingan Rendah?", "id": "AD620." },
  { "en": "Sensor Aliran Udara (Air Flow)?", "id": "Sensor AWM720P1." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 0805 Bicolor?", "id": "LED Bicolor 0805." },
  { "en": "Gerbang Logika XNOR CMOS Empat Gerbang?", "id": "IC 4077." },
  { "en": "IC Pengontrol Motor DC Tanpa Sikat?", "id": "A4960." },
  { "en": "Regulator Switching Topologi Sepic?", "id": "LT3757." },
  { "en": "Dioda Zener Tegangan 13V?", "id": "1N4743A." },
  { "en": "IC ADC I2C 16-Bit Empat Saluran?", "id": "ADS1115." },
  { "en": "Sensor IMU (Inertial Measurement Unit) 6-DOF?", "id": "LSM6DS33." },
  { "en": "Transistor PNP Frekuensi Radio?", "id": "2N4403." },
  { "en": "IC Penguat Operasional Dengan Shutdown?", "id": "OPA340." },
  { "en": "Modul Kamera Lensa Wide?", "id": "Kamera Lensa Lebar." },
  { "en": "IC Switch Analog Dengan Proteksi ESD?", "id": "ADG1411." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Diffused?", "id": "LED Diffused 5mm." },
  { "en": "IC Multiplexer CMOS 8-Channel?", "id": "CD4051BE." },
  { "en": "Sensor Kelembaban Dan Suhu SHT10?", "id": "SHT10." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "IGBT FGH60N60." },
  { "en": "Dioda Penyearah Jembatan Mini?", "id": "DF06M." },
  { "en": "IC DAC Audio Untuk Perangkat Portabel?", "id": "MAX98357A." },
  { "en": "Sensor Ketinggian Air Non-Kontak?", "id": "XKC-Y25-V." },
  { "en": "IC Flip-Flop D-Type Dengan Latch?", "id": "74HCT74." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3528?", "id": "LED SMD 3528." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk WiFi?", "id": "SE2576L." },
  { "en": "Regulator Tegangan Linear Kebisingan Sangat Rendah?", "id": "ADP150." },
  { "en": "Transistor Efek Medan Gerbang Ganda?", "id": "Dual-Gate MOSFET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Tegangan Rendah?", "id": "USBLC6-2SC6." },
  { "en": "IC Penguat Audio Kelas AB Daya Tinggi?", "id": "TDA7294." },
  { "en": "Sensor Total Dissolved Solids (TDS)?", "id": "Sensor TDS." },
  { "en": "Modul Tampilan LCD Karakter Dengan Backlight RGB?", "id": "LCD RGB 16x2." },
  { "en": "IC Gerbang AND CMOS Schmitt Trigger?", "id": "IC 4081B." },
  { "en": "Transistor PNP Komplementer TIP41?", "id": "TIP42." },
  { "en": "Dioda Zener Tegangan 20V?", "id": "1N4747A." },
  { "en": "IC Penguat Sampel Dan Tahan?", "id": "LF398." },
  { "en": "Sensor Suhu Dan Kelembaban AM2302?", "id": "DHT22." },
  { "en": "LED (Light Emitting Diode) Inframerah Sudut Sempit?", "id": "IR LED Narrow." },
  { "en": "IC Switch Analog SP8T (Single Pole Eight Throw)?", "id": "MAX395." },
  { "en": "Regulator Switching Untuk LED Driver?", "id": "LM3409." },
  { "en": "Transistor NPN Komplementer 2N2907?", "id": "2N2222." },
  { "en": "Dioda Pemulihan Cepat Tegangan Tinggi?", "id": "BYV26E." },
  { "en": "IC ADC Pipe-lined Kecepatan Tinggi?", "id": "AD9235." },
  { "en": "Sensor Gas Hidrogen?", "id": "Sensor MQ-8." },
  { "en": "Modul LCD OLED Transparan?", "id": "Layar OLED Transparan." },
  { "en": "IC Gerbang NOR CMOS Kolektor Terbuka?", "id": "CD4001UB." },
  { "en": "Transistor PNP Amplifier Kebisingan Sangat Rendah?", "id": "2SA1085." },
  { "en": "Dioda Penyearah Arus Sangat Tinggi?", "id": "Dioda Stud." },
  { "en": "IC Penguat Headphone Dengan Bass Boost?", "id": "TPA6132A2." },
  { "en": "Sensor Potensial Redoks (ORP)?", "id": "Sensor ORP." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 5050?", "id": "LED SMD 5050." },
  { "en": "IC Penguat RF (Radio Frequency) Gain Block?", "id": "ABA-52563." },
  { "en": "Regulator Tegangan Linear PSRR Sangat Tinggi?", "id": "TPS7A8300." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "IRLB8721." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Multi-Saluran?", "id": "USBLC6-4SC6." },
  { "en": "IC Penguat Diferensial Programmable Gain?", "id": "PGA281." },
  { "en": "Sensor Oksigen Terlarut Optik?", "id": "Sensor DO Optik." },
  { "en": "Modul Kamera Dengan Auto Fokus?", "id": "Kamera Auto Fokus." },
  { "en": "IC Gerbang XOR CMOS Open-Drain?", "id": "CD4077UB." },
  { "en": "Transistor NPN Amplifier Kebisingan Sangat Rendah?", "id": "2SC2547." },
  { "en": "Dioda Zener Tegangan 100V?", "id": "1N4764A." },
  { "en": "IC Penguat Distribusi ADSL?", "id": "AD816." },
  { "en": "Sensor Arus Dengan Output Tegangan?", "id": "ACS758." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Fokus?", "id": "LED Lensa Fokus." },
  { "en": "IC Switch Analog Video Lintas Titik?", "id": "AD8116." },
  { "en": "Regulator Switching Topologi Push-Pull?", "id": "UC3825." },
  { "en": "Transistor Darlington Array Dengan Dioda Zener?", "id": "ULN2003A." },
  { "en": "Dioda Penyearah Jembatan Daya Tinggi?", "id": "MDS100A." },
  { "en": "IC ADC Sigma-Delta Untuk Timbangan?", "id": "HX711." },
  { "en": "Sensor Partikulat PM10?", "id": "Sensor PM10." },
  { "en": "Modul Pengenalan Tulisan Tangan?", "id": "Modul OCR." },
  { "en": "IC Flip-Flop D-Type Dengan Clear?", "id": "74AC74." },
  { "en": "Papan Pengembangan Dengan Prosesor Dual-Core?", "id": "Raspberry Pi Pico." },
  { "en": "IC Penguat Operasional CMOS?", "id": "LMC6482." },
  { "en": "Transistor MOSFET N-Channel Dengan Dioda Zener?", "id": "BSS138." },
  { "en": "Dioda Penyearah Jembatan SMD Mini?", "id": "MB10S." },
  { "en": "Sensor Magnetometer Resolusi Tinggi?", "id": "LIS3MDL." },
  { "en": "Modul Tampilan LCD Grafis Serial?", "id": "Serial GLCD 128x64." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Germicidal?", "id": "LED UVC 254nm." },
  { "en": "IC Counter Biner 14-Tahap?", "id": "CD4060BE." },
  { "en": "IC Driver Setengah Jembatan (Half-Bridge)?", "id": "IR2110." },
  { "en": "Sensor Tekanan Untuk Aplikasi Medis?", "id": "MPX5010DP." },
  { "en": "Op-Amp BiFET Kebisingan Rendah?", "id": "LF411." },
  { "en": "Regulator Tegangan Negatif Tegangan Rendah?", "id": "LP2951." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5777." },
  { "en": "Dioda Zener Tegangan 22V?", "id": "1N4748A." },
  { "en": "Gerbang Logika OR Schmitt-Trigger Tunggal?", "id": "SN74LVC1G32." },
  { "en": "Kabel Serat Optik Multi-Mode?", "id": "Kabel Fiber Optik OM1." },
  { "en": "Modul Sensor Gestur Tangan?", "id": "PAJ7620U2." },
  { "en": "IC Penguat Audio Kelas AB Stereo?", "id": "TDA2050." },
  { "en": "Transistor PNP Untuk Switching Cepat?", "id": "2N2907A." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Remote Control?", "id": "IR Transmitter." },
  { "en": "IC ADC I2S 24-Bit Audio?", "id": "PCM1808." },
  { "en": "Papan Pengembangan FPGA Berbasis Cyclone?", "id": "Altera Cyclone II." },
  { "en": "Sensor Gas Udara Beracun?", "id": "Sensor MiCS-6814." },
  { "en": "Transistor Array Darlington Dengan Dioda Internal?", "id": "ULN2003." },
  { "en": "IC Konverter Tegangan Ke Arus?", "id": "XTR115." },
  { "en": "Dioda Pelindung Tegangan Lebih TVS Array?", "id": "SMF05C." },
  { "en": "IC Penguat Diferensial Programmable Gain?", "id": "MCP6G01." },
  { "en": "Sensor Aliran Air?", "id": "Sensor YF-S201." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 2835?", "id": "LED SMD 2835." },
  { "en": "Gerbang Logika XNOR CMOS Dengan Buffer?", "id": "CD4077BE." },
  { "en": "IC Pengontrol Motor DC Dengan Rem?", "id": "TB6612FNG." },
  { "en": "Regulator Switching Topologi Buck?", "id": "LM2576." },
  { "en": "Dioda Zener Tegangan 27V?", "id": "1N4750A." },
  { "en": "IC ADC SPI 12-Bit Delapan Saluran?", "id": "MCP3208." },
  { "en": "Sensor IMU (Inertial Measurement Unit) Dengan Filter Kalman?", "id": "Wit-Motion WT901." },
  { "en": "Transistor NPN Frekuensi Radio Daya Sedang?", "id": "2SC2078." },
  { "en": "IC Penguat Operasional Dengan Arus Bias Rendah?", "id": "LMP7721." },
  { "en": "Modul Kamera Lensa Fisheye?", "id": "Kamera Lensa Fisheye." },
  { "en": "IC Switch Analog Dengan Proteksi Tegangan Berlebih?", "id": "ADG5208." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Bening?", "id": "LED Bening 5mm." },
  { "en": "IC Multiplexer CMOS 16-Channel?", "id": "74HC4067." },
  { "en": "Sensor Kelembaban Dan Suhu SHTC3?", "id": "SHTC3." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "IXGN60N60C2D1." },
  { "en": "Dioda Penyearah Jembatan Daya Rendah?", "id": "DF04S." },
  { "en": "IC DAC Audio Untuk Aplikasi Profesional?", "id": "ES9018K2M." },
  { "en": "Sensor Salinitas Air?", "id": "Sensor Salinitas." },
  { "en": "IC Flip-Flop D-Type Dengan Preset Dan Clear?", "id": "74HC74." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 5630?", "id": "LED SMD 5630." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk RFID?", "id": "MFRC500." },
  { "en": "Regulator Tegangan Linear Arus Sangat Tinggi?", "id": "LT1084." },
  { "en": "Transistor Efek Medan Semikonduktor Galium Nitrida?", "id": "GaN FET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Kecepatan Tinggi?", "id": "SRV05-4." },
  { "en": "IC Penguat Audio Kelas T?", "id": "TA2024." },
  { "en": "Sensor Dissolved Oxygen (DO)?", "id": "Sensor Oksigen Terlarut." },
  { "en": "Modul Tampilan LCD Karakter Dengan Antarmuka I2C?", "id": "LCD I2C 20x4." },
  { "en": "IC Gerbang AND CMOS Kolektor Terbuka?", "id": "CD4081UB." },
  { "en": "Transistor PNP Komplementer TIP42?", "id": "TIP41." },
  { "en": "Dioda Zener Tegangan 30V?", "id": "1N4751A." },
  { "en": "IC Penguat Log-Ratio?", "id": "AD8302." },
  { "en": "Sensor Suhu Dan Kelembaban AM2320?", "id": "AM2320." },
  { "en": "LED (Light Emitting Diode) Inframerah Daya Sangat Tinggi?", "id": "IR LED 5W." },
  { "en": "IC Switch Analog Lintas Titik (Crosspoint)?", "id": "MT8816." },
  { "en": "Regulator Switching Untuk Pengisi Daya Baterai?", "id": "BQ24650." },
  { "en": "Transistor NPN Komplementer 2N4403?", "id": "2N4401." },
  { "en": "Dioda Pemulihan Cepat Arus Tinggi?", "id": "RHRG75120." },
  { "en": "IC ADC Sigma-Delta 24-Bit?", "id": "ADS1232." },
  { "en": "Sensor Gas Etanol?", "id": "Sensor MQ-303A." },
  { "en": "Modul LCD OLED Bundar?", "id": "Layar OLED Bulat." },
  { "en": "IC Gerbang NOR CMOS Schmitt Trigger?", "id": "CD4001B." },
  { "en": "Transistor PNP Amplifier Kebisingan Rendah?", "id": "2SA992." },
  { "en": "Dioda Penyearah Arus Rendah?", "id": "1N914." },
  { "en": "IC Penguat Headphone Virtual Surround?", "id": "CS4344." },
  { "en": "Sensor Potensial Hidrogen (pH) Industri?", "id": "Sensor pH Industri." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 5730?", "id": "LED SMD 5730." },
  { "en": "IC Penguat RF (Radio Frequency) Gain Variabel?", "id": "ADL5330." },
  { "en": "Regulator Tegangan Linear Arus Ultra Rendah?", "id": "TPS782." },
  { "en": "Transistor Efek Medan Semikonduktor Silikon Karbida?", "id": "SiC MOSFET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk HDMI?", "id": "TPD12S016." },
  { "en": "IC Penguat Diferensial Programmable Gain?", "id": "LTC6803-2." },
  { "en": "Sensor Oksigen Terlarut Fluoresen?", "id": "Sensor DO Fluoresen." },
  { "en": "Modul Kamera Stereo?", "id": "Kamera Stereo." },
  { "en": "IC Gerbang XOR CMOS Open-Drain Buffer?", "id": "CD4070UB." },
  { "en": "Transistor NPN Amplifier Kebisingan Rendah?", "id": "2SC1815." },
  { "en": "Dioda Zener Tegangan 110V?", "id": "1N4765A." },
  { "en": "IC Penguat Distribusi Serat Optik?", "id": "ONET8501T." },
  { "en": "Sensor Arus Dengan Isolasi?", "id": "ACS723." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Oval?", "id": "LED Lensa Oval." },
  { "en": "IC Switch Analog Video Definisi Tinggi?", "id": "ADG774." },
  { "en": "Regulator Switching Topologi Half-Bridge?", "id": "L6599." },
  { "en": "Transistor Darlington Array Dengan Output Arus Tinggi?", "id": "STA461C." },
  { "en": "Dioda Penyearah Jembatan Arus Sangat Tinggi?", "id": "SKB 30/12." },
  { "en": "IC ADC Sigma-Delta Untuk Suhu?", "id": "MAX31855." },
  { "en": "Sensor Partikulat PM1.0?", "id": "Sensor PM1.0." },
  { "en": "Modul Pengenalan Objek?", "id": "Modul Pixy2 CMUcam5." },
  { "en": "IC Flip-Flop D-Type Dengan Enable?", "id": "74LS374." },
  { "en": "Papan Pengembangan Berbasis CPLD?", "id": "Altera MAX II." },
  { "en": "IC Penguat Operasional Arus Sangat Rendah?", "id": "LMP2012." },
  { "en": "Transistor MOSFET N-Channel Mode Enhancement?", "id": "2N7000." },
  { "en": "Dioda Penyearah Jembatan 3A SMD?", "id": "MB3510." },
  { "en": "Sensor Magnetometer Dan Giroskop?", "id": "LSM9DS1." },
  { "en": "Modul Tampilan LCD Grafis Dengan Touchscreen?", "id": "LCD TFT Resistive." },
  { "en": "LED (Light Emitting Diode) UV-C Untuk Desinfeksi Air?", "id": "LED UVC Water Purifier." },
  { "en": "IC Counter Biner 8-Bit?", "id": "74HC590." },
  { "en": "IC Driver Full-Bridge Motor?", "id": "L298." },
  { "en": "Sensor Tekanan Ban Mobil (TPMS)?", "id": "Sensor TPMS." },
  { "en": "Op-Amp BiFET Tegangan Tinggi?", "id": "OPA129." },
  { "en": "Regulator Tegangan Negatif Arus Rendah?", "id": "LP2950." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5779." },
  { "en": "Dioda Zener Tegangan 16V?", "id": "1N4745A." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Tunggal?", "id": "74LVC1G132." },
  { "en": "Kabel Serat Optik Single-Mode?", "id": "Kabel Fiber Optik OS2." },
  { "en": "Modul Sensor Gerakan Optik?", "id": "ADNS-3050." },
  { "en": "IC Penguat Audio Kelas AB Daya Tinggi?", "id": "LM4766." },
  { "en": "Transistor NPN Untuk Switching Cepat?", "id": "MPS2222A." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Keamanan?", "id": "IR Security LED." },
  { "en": "IC ADC I2S Untuk Mikrofon?", "id": "ICS-43432." },
  { "en": "Papan Pengembangan FPGA Berbasis Zynq?", "id": "Digilent Arty Z7." },
  { "en": "Sensor Gas Campuran?", "id": "Sensor BME680." },
  { "en": "Transistor Array Darlington Dengan Dioda Freewheeling?", "id": "ULN2003A." },
  { "en": "IC Konverter Arus Ke Tegangan?", "id": "RVC1001." },
  { "en": "Dioda Pelindung Tegangan Berlebih Array?", "id": "SP0503BAHTG." },
  { "en": "IC Penguat Diferensial Arus Tinggi?", "id": "AD8130." },
  { "en": "Sensor Aliran Gas?", "id": "Sensor AWM5104VN." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3014?", "id": "LED SMD 3014." },
  { "en": "Gerbang Logika XNOR CMOS Dengan Output Buffer?", "id": "CD4077B." },
  { "en": "IC Pengontrol Motor DC Dengan Encoder?", "id": "VNH5019." },
  { "en": "Regulator Switching Topologi Boost?", "id": "LM2587." },
  { "en": "Dioda Zener Tegangan 36V?", "id": "1N4753A." },
  { "en": "IC ADC I2C 18-Bit?", "id": "MCP3421." },
  { "en": "Sensor IMU (Inertial Measurement Unit) Dengan GPS?", "id": "Adafruit Ultimate GPS." },
  { "en": "Transistor PNP Frekuensi Radio Daya Sedang?", "id": "2SA1213." },
  { "en": "IC Penguat Operasional Dengan Arus Bias Sangat Rendah?", "id": "OPA128." },
  { "en": "Modul Kamera Lensa Telefoto?", "id": "Kamera Lensa Tele." },
  { "en": "IC Switch Analog Dengan Proteksi Arus Berlebih?", "id": "ADG5433." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Fresnel?", "id": "LED Lensa Fresnel." },
  { "en": "IC Multiplexer CMOS 4-Channel Ganda?", "id": "CD4052BE." },
  { "en": "Sensor Kelembaban Dan Suhu SHT35?", "id": "SHT35." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "FGH40N60SMD." },
  { "en": "Dioda Penyearah Jembatan Untuk PCB?", "id": "RB156." },
  { "en": "IC DAC Audio Untuk Sistem Hi-Fi?", "id": "AK4493EQ." },
  { "en": "Sensor pH Digital Dengan Suhu?", "id": "DFRobot pH Sensor." },
  { "en": "IC Flip-Flop D-Type Dengan Set Asinkron?", "id": "74HC74." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3030?", "id": "LED SMD 3030." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk Satelit?", "id": "HMC451LP3." },
  { "en": "Regulator Tegangan Linear Arus Sangat Rendah?", "id": "ADP121." },
  { "en": "Transistor Efek Medan Semikonduktor Galium Arsenida?", "id": "GaAs FET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk USB?", "id": "TPD4S012." },
  { "en": "IC Penguat Audio Kelas G?", "id": "LM49251." },
  { "en": "Sensor Oksigen Terlarut Dengan Elektroda?", "id": "Sensor DO Elektroda." },
  { "en": "Modul Tampilan LCD Karakter Dengan Antarmuka SPI?", "id": "LCD SPI 16x2." },
  { "en": "IC Gerbang AND CMOS Kolektor Terbuka Buffer?", "id": "CD4081UB." },
  { "en": "Transistor NPN Komplementer 2N3906?", "id": "2N3904." },
  { "en": "Dioda Zener Tegangan 75V?", "id": "1N4761A." },
  { "en": "IC Penguat Logaritma Suhu?", "id": "MAX6675." },
  { "en": "Sensor Suhu Dan Kelembaban BME280?", "id": "BME280." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Penginderaan Jarak?", "id": "IR Proximity LED." },
  { "en": "IC Switch Analog Lintas Titik Audio?", "id": "AD75019." },
  { "en": "Regulator Switching Untuk Aplikasi Otomotif?", "id": "LM5085." },
  { "en": "Transistor NPN Komplementer 2SA992?", "id": "2SC1845." },
  { "en": "Dioda Pemulihan Cepat Arus Sangat Tinggi?", "id": "DSEI30-12A." },
  { "en": "IC ADC Sigma-Delta 16-Bit?", "id": "ADS1118." },
  { "en": "Sensor Gas Nitrogen Dioksida (NO2)?", "id": "Sensor MiCS-2714." },
  { "en": "Modul LCD OLED Fleksibel Transparan?", "id": "Layar OLED Fleksibel." },
  { "en": "IC Gerbang NOR CMOS Schmitt Trigger Buffer?", "id": "CD4001UB." },
  { "en": "Transistor PNP Amplifier Kebisingan Rendah?", "id": "2SA1015." },
  { "en": "Dioda Penyearah Arus Sangat Rendah?", "id": "BAT54." },
  { "en": "IC Penguat Headphone Dengan Kontrol Volume Digital?", "id": "TPA6130A2." },
  { "en": "Sensor pH Tanah Digital?", "id": "Sensor pH Tanah Digital." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 2016?", "id": "LED SMD 2016." },
  { "en": "IC Penguat RF (Radio Frequency) Gain Tetap?", "id": "MAX2633." },
  { "en": "Regulator Tegangan Linear Arus Diam Rendah?", "id": "MCP1700." },
  { "en": "Transistor Efek Medan Semikonduktor Organik?", "id": "OFET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk Ethernet?", "id": "TPD4E1U06." },
  { "en": "IC Penguat Diferensial Programmable Gain?", "id": "AD8221." },
  { "en": "Sensor Oksigen Terlarut Dengan Membran?", "id": "Sensor DO Membran." },
  { "en": "Modul Kamera Termal Resolusi Tinggi?", "id": "FLIR Lepton 3.5." },
  { "en": "IC Gerbang XOR CMOS Open-Drain Buffer?", "id": "CD4077UB." },
  { "en": "Transistor NPN Amplifier Kebisingan Rendah?", "id": "2SC2458." },
  { "en": "Dioda Zener Tegangan 200V?", "id": "1N4768A." },
  { "en": "IC Penguat Distribusi Serat Optik?", "id": "ADN2814." },
  { "en": "Sensor Arus Dengan Isolasi Galvanik?", "id": "ACPL-C87B." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Batwing?", "id": "LED Lensa Batwing." },
  { "en": "IC Switch Analog Video Definisi Standar?", "id": "ADG708." },
  { "en": "Regulator Switching Topologi Full-Bridge?", "id": "UCC28950." },
  { "en": "Transistor Darlington Array Dengan Output Arus Rendah?", "id": "M-CSA05." },
  { "en": "Dioda Penyearah Jembatan Arus Ultra Tinggi?", "id": "SKB 60/12." },
  { "en": "IC ADC Sigma-Delta Untuk Energi?", "id": "ADE7753." },
  { "en": "Sensor Partikulat PM4.0?", "id": "Sensor PM4.0." },
  { "en": "Modul Pengenalan Gerakan?", "id": "Modul APDS-9960." },
  { "en": "IC Flip-Flop D-Type Dengan Set Sinkron?", "id": "74LS175." },
  { "en": "IC Penguat Logaritmik Suhu?", "id": "MAX6675." },
  { "en": "IC Penguat Audio Kelas AB Mono 70W?", "id": "TDA7294." },
  { "en": "Transistor MOSFET N-Channel 600V?", "id": "IRFP460." },
  { "en": "Dioda Zener Tegangan 11V?", "id": "1N4741A." },
  { "en": "Gerbang Logika Buffer Non-Inverting Schmitt Trigger?", "id": "74HC244." },
  { "en": "Papan Pengembangan Berbasis ATtiny85?", "id": "Digispark." },
  { "en": "Sensor Kelembaban Tanah Kapasitif?", "id": "Capacitive Soil Moisture Sensor." },
  { "en": "IC Penguat Audio Stereo Dengan Mute?", "id": "TDA7265." },
  { "en": "Transistor PNP Penguat Daya Audio?", "id": "TIP42C." },
  { "en": "LED (Light Emitting Diode) Inframerah Kecepatan Tinggi?", "id": "SFH4550." },
  { "en": "IC ADC I2S Untuk Audio Resolusi Tinggi?", "id": "CS4272." },
  { "en": "Papan Pengembangan FPGA Berbasis ECP5?", "id": "TinyFPGA EX." },
  { "en": "Sensor Gas Amonia Dan Sulfida?", "id": "MQ-136." },
  { "en": "Transistor Array PNP Beban Tinggi?", "id": "UDN2981A." },
  { "en": "IC Konverter Arus Ke Tegangan Presisi?", "id": "IVC102." },
  { "en": "Dioda Pelindung Tegangan Berlebih Array 8-Saluran?", "id": "RClamp0524P." },
  { "en": "IC Penguat Diferensial Arus Tinggi?", "id": "AD8130." },
  { "en": "Sensor Aliran Udara Massal Otomotif?", "id": "MAF Sensor." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 1210?", "id": "LED SMD 1210." },
  { "en": "Gerbang Logika XNOR CMOS Dengan Output Schmitt?", "id": "CD4077." },
  { "en": "IC Pengontrol Motor DC Dengan Umpan Balik Arus?", "id": "MAX14870." },
  { "en": "Regulator Switching Topologi Forward Converter?", "id": "LM5025." },
  { "en": "Dioda Zener Tegangan 43V?", "id": "1N4755A." },
  { "en": "IC ADC I2C 24-Bit Untuk Timbangan?", "id": "HX711." },
  { "en": "Sensor IMU (Inertial Measurement Unit) Dengan Komunikasi CAN?", "id": "VN-200." },
  { "en": "Transistor NPN Frekuensi Radio Daya Sangat Tinggi?", "id": "MRF454." },
  { "en": "IC Penguat Operasional Arus Diam Sangat Rendah?", "id": "TLV2462." },
  { "en": "Modul Kamera Lensa Zoom Optik?", "id": "Optical Zoom Camera Module." },
  { "en": "IC Switch Analog Dengan Proteksi ESD?", "id": "ADG1212." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Datar Atas?", "id": "Flat Top LED." },
  { "en": "IC Multiplexer CMOS 2-Channel Ganda?", "id": "CD4053." },
  { "en": "Sensor Kelembaban Dan Suhu SHT21 I2C?", "id": "SHT21." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "NGB8207N." },
  { "en": "Dioda Penyearah Jembatan Daya Sangat Rendah?", "id": "A602." },
  { "en": "IC DAC Audio Untuk Sistem Mobil?", "id": "TDA1543." },
  { "en": "Sensor pH Digital Dengan Kalibrasi Otomatis?", "id": "Gravity pH Sensor Pro." },
  { "en": "IC Flip-Flop D-Type Dengan Set Asinkron?", "id": "74HCT74." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3030 Bicolor?", "id": "Bicolor 3030 LED." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk RFID?", "id": "AS3993." },
  { "en": "Regulator Tegangan Linear Arus Diam Sangat Rendah?", "id": "LT1761." },
  { "en": "Transistor Efek Medan Semikonduktor Galium Oksida?", "id": "Ga2O3 FET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk Audio?", "id": "ACF201." },
  { "en": "IC Penguat Audio Kelas H Stereo?", "id": "TDA7265." },
  { "en": "Sensor Oksigen Terlarut Dengan Elektroda Galvanik?", "id": "Galvanic DO Sensor." },
  { "en": "Modul Tampilan LCD Karakter Dengan Antarmuka Serial?", "id": "Serial LCD 16x2." },
  { "en": "IC Gerbang AND CMOS Kolektor Terbuka Buffer?", "id": "CD4081." },
  { "en": "Transistor NPN Komplementer 2N3905?", "id": "2N3903." },
  { "en": "Dioda Zener Tegangan 82V?", "id": "1N4762A." },
  { "en": "IC Penguat Logaritma Detektor RF?", "id": "AD8313." },
  { "en": "Sensor Suhu Dan Kelembaban HTU21D I2C?", "id": "HTU21D." },
  { "en": "LED (Light Emitting Diode) Inframerah Daya Sangat Tinggi?", "id": "IR LED 10W." },
  { "en": "IC Switch Analog Lintas Titik Video?", "id": "AD8113." },
  { "en": "Regulator Switching Untuk Aplikasi Daya Rendah?", "id": "LTC3588." },
  { "en": "Transistor NPN Komplementer 2SA1015?", "id": "2SC1815." },
  { "en": "Dioda Pemulihan Cepat Arus Sangat Tinggi?", "id": "HFA08TB60." },
  { "en": "IC ADC Sigma-Delta 18-Bit?", "id": "AD7799." },
  { "en": "Sensor Gas Etanol Dan Metana?", "id": "MQ-309A Sensor." },
  { "en": "Modul LCD OLED Transparan Biru?", "id": "Transparent Blue OLED." },
  { "en": "IC Gerbang NOR CMOS Schmitt Trigger Buffer?", "id": "CD4001." },
  { "en": "Transistor PNP Amplifier Kebisingan Rendah?", "id": "BC558." },
  { "en": "Dioda Penyearah Arus Sangat Rendah?", "id": "1N4148." },
  { "en": "IC Penguat Headphone Dengan Pompa Muatan?", "id": "TPA6110A2." },
  { "en": "Sensor Potensial Hidrogen (pH) Dengan Arduino?", "id": "Arduino pH Sensor Kit." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 2020?", "id": "SMD 2020 LED." },
  { "en": "IC Penguat RF (Radio Frequency) Gain Variabel?", "id": "VGA-563." },
  { "en": "Regulator Tegangan Linear Arus Diam Ultra Rendah?", "id": "MAX8881." },
  { "en": "Transistor Efek Medan Semikonduktor Organik?", "id": "Pentacene OFET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk HDMI?", "id": "IP4220CZ6." },
  { "en": "IC Penguat Diferensial Programmable Gain?", "id": "AD8231." },
  { "en": "Sensor Oksigen Terlarut Dengan Membran Optik?", "id": "Optical DO Sensor." },
  { "en": "Modul Kamera Termal Resolusi Rendah?", "id": "AMG8833." },
  { "en": "IC Gerbang XOR CMOS Open-Drain Buffer?", "id": "CD4077." },
  { "en": "Transistor NPN Amplifier Kebisingan Rendah?", "id": "BC549C." },
  { "en": "Dioda Zener Tegangan 120V?", "id": "1N4766A." },
  { "en": "IC Penguat Distribusi Serat Optik?", "id": "HMC799LP3E." },
  { "en": "Sensor Arus Dengan Isolasi Galvanik?", "id": "ACS711." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Batwing?", "id": "Batwing LED." },
  { "en": "IC Switch Analog Video Definisi Standar?", "id": "ADG708." },
  { "en": "Regulator Switching Topologi Full-Bridge?", "id": "ISL6752." },
  { "en": "Transistor Darlington Array Dengan Output Arus Rendah?", "id": "ULN2002A." },
  { "en": "Dioda Penyearah Jembatan Arus Ultra Tinggi?", "id": "SKB 72/12." },
  { "en": "IC ADC Sigma-Delta Untuk Energi?", "id": "MCP3905." },
  { "en": "Sensor Partikulat PM4.0?", "id": "SPS30." },
  { "en": "Modul Pengenalan Gerakan?", "id": "APDS-9960 Module." },
  { "en": "IC Flip-Flop D-Type Dengan Set Sinkron?", "id": "74LS175." },
  { "en": "IC Penguat Operasional JFET Presisi?", "id": "OPA627." },
  { "en": "Transistor NPN Amplifier VHF?", "id": "BFR96." },
  { "en": "Dioda Penyearah Arus 6A?", "id": "6A10." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Hex?", "id": "74HC132." },
  { "en": "Sensor Suhu Dan Kelembaban Akurasi Tinggi?", "id": "SHT31-D." },
  { "en": "Regulator Tegangan Negatif 1.2V-37V?", "id": "LM337T." },
  { "en": "Transistor MOSFET P-Channel Daya?", "id": "IRF4905." },
  { "en": "Dioda Zener Tegangan 2.4V?", "id": "1N4728A." },
  { "en": "IC Penguat Audio Kelas D Ganda?", "id": "TPA3110D2." },
  { "en": "Modul Sensor Warna Presisi Tinggi?", "id": "AS7262." },
  { "en": "IC Latch Transparan Octal?", "id": "74HC373." },
  { "en": "Papan Pengembangan Berbasis Teensy?", "id": "Teensy 4.0." },
  { "en": "Sensor Aliran Cairan?", "id": "YF-B1." },
  { "en": "Regulator Tegangan Positif 3A?", "id": "LM1085." },
  { "en": "IC Driver LED Matriks?", "id": "MAX7219CNG." },
  { "en": "Transistor PNP Amplifier Audio?", "id": "A1015." },
  { "en": "Dioda Schottky Penghalang Silikon?", "id": "1N5822." },
  { "en": "IC Penguat Audio Kelas D 300W Stereo?", "id": "TAS5630." },
  { "en": "Transistor MOSFET P-Channel -60V?", "id": "FQP47P06." },
  { "en": "Dioda Zener Tegangan 3.0V?", "id": "1N4727A." },
  { "en": "Gerbang Logika AND-OR-INVERT?", "id": "IC 74HC51." },
  { "en": "Papan Pengembangan Berbasis SAMD21?", "id": "Arduino Zero." },
  { "en": "Sensor Kualitas Udara VOC Dan eCO2?", "id": "SGP30." },
  { "en": "IC Prosesor Audio Dengan Kontrol I2C?", "id": "TDA7439." },
  { "en": "Transistor NPN Darlington Dengan Dioda Internal?", "id": "MJE700." },
  { "en": "LED (Light Emitting Diode) Inframerah Array Untuk Kamera?", "id": "IR LED Array." },
  { "en": "IC ADC Delta-Sigma 24-Bit Untuk Audio?", "id": "CS5381." },
  { "en": "Papan FPGA Berbasis MachXO2?", "id": "Lattice MachXO2 Breakout." },
  { "en": "IC Konverter Tegangan Ke Frekuensi Sinkron?", "id": "AD7740." },
  { "en": "Dioda Pelindung TVS Untuk Port Ethernet?", "id": "SP0503BAHTG." },
  { "en": "IC Penguat Diferensial Video Kecepatan Tinggi?", "id": "LMH6551." },
  { "en": "Sensor Jarak Time-of-Flight Presisi?", "id": "VL53L1X." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 1204?", "id": "LED SMD 1204." },
  { "en": "Gerbang Logika NAND Dengan Input 8?", "id": "IC 74HC30." },
  { "en": "IC Driver Motor Stepper Dengan Indeksator?", "id": "L6470." },
  { "en": "Regulator Switching Terisolasi Fly-Buck?", "id": "LM5017." },
  { "en": "Transistor PNP Amplifier RF (Radio Frequency)?", "id": "BFR92A." },
  { "en": "Dioda Penyearah Jembatan 6A?", "id": "GBU606." },
  { "en": "IC DAC Audio 24-bit?", "id": "PT8211." },
  { "en": "Sensor Resistivitas Tanah?", "id": "Soil Resistivity Sensor." },
  { "en": "IC Flip-Flop T-Type Ganda?", "id": "MC14013B." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Daya 10W?", "id": "UV LED 10W." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk ISM Band?", "id": "RFM95W." },
  { "en": "Regulator Tegangan Linear Referensi?", "id": "REF5025." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "FQP30N06L." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk SIM Card?", "id": "SMF05CT1G." },
  { "en": "IC Penguat Audio Kelas AB 50W?", "id": "LM4766." },
  { "en": "Sensor Konduktivitas Elektrik (EC)?", "id": "EC Sensor." },
  { "en": "Modul Tampilan LCD OLED Serial?", "id": "Serial OLED." },
  { "en": "IC Gerbang AND CMOS Delapan Input?", "id": "CD4068B." },
  { "en": "Transistor NPN Komplementer 2N5401?", "id": "2N5551." },
  { "en": "Dioda Zener Tegangan 39V?", "id": "1N4754A." },
  { "en": "IC Penguat Logaritma Dengan Output Tegangan?", "id": "LOG114." },
  { "en": "Sensor Suhu Dan Kelembaban HIH8120?", "id": "HIH8120." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Lidar?", "id": "Pulsed Laser Diode." },
  { "en": "IC Switch Analog Lintas Titik Resolusi Tinggi?", "id": "ADG2128." },
  { "en": "Regulator Switching Untuk Pengisi Daya USB-C?", "id": "TPS65982." },
  { "en": "Transistor NPN Komplementer 2SA1020?", "id": "2SC2655." },
  { "en": "Dioda Pemulihan Cepat Arus Ultra Tinggi?", "id": "RURG3060." },
  { "en": "IC ADC Sigma-Delta 20-Bit?", "id": "AD7705." },
  { "en": "Sensor Gas Aseton?", "id": "Sensor MQ-138." },
  { "en": "Modul LCD OLED Fleksibel Berwarna?", "id": "Flexible Color OLED." },
  { "en": "IC Gerbang NOR CMOS Schmitt Trigger Ganda?", "id": "CD4001B." },
  { "en": "Transistor PNP Amplifier Kebisingan Rendah?", "id": "2SA1084." },
  { "en": "Dioda Penyearah Arus Sangat Kecil?", "id": "1N4148W." },
  { "en": "IC Penguat Headphone Dengan Bypass?", "id": "LM4880." },
  { "en": "Sensor Potensial Hidrogen (pH) Laboratorium?", "id": "Lab Grade pH Probe." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 1608?", "id": "LED SMD 1608." },
  { "en": "IC Penguat RF (Radio Frequency) Gain Digital?", "id": "F0424." },
  { "en": "Regulator Tegangan Linear Arus Diam Sangat Rendah?", "id": "LTC1844." },
  { "en": "Transistor Efek Medan Semikonduktor Organik?", "id": "P3HT OFET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk CAN Bus?", "id": "PESD1CAN." },
  { "en": "IC Penguat Diferensial Programmable Gain?", "id": "PGA113." },
  { "en": "Sensor Oksigen Terlarut Dengan Membran Optik?", "id": "Optical DO Sensor." },
  { "en": "Modul Kamera Termal Resolusi Rendah?", "id": "AMG8833." },
  { "en": "IC Gerbang XOR CMOS Open-Drain Buffer?", "id": "CD4077." },
  { "en": "Transistor NPN Amplifier Kebisingan Rendah?", "id": "BC549C." },
  { "en": "Dioda Zener Tegangan 120V?", "id": "1N4766A." },
  { "en": "IC Penguat Distribusi Serat Optik?", "id": "HMC799LP3E." },
  { "en": "Sensor Arus Dengan Isolasi Galvanik?", "id": "ACS711." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Batwing?", "id": "Batwing LED." },
  { "en": "IC Switch Analog Video Definisi Standar?", "id": "ADG708." },
  { "en": "Regulator Switching Topologi Full-Bridge?", "id": "ISL6752." },
  { "en": "Transistor Darlington Array Dengan Output Arus Rendah?", "id": "ULN2002A." },
  { "en": "Dioda Penyearah Jembatan Arus Ultra Tinggi?", "id": "SKB 72/12." },
  { "en": "IC ADC Sigma-Delta Untuk Energi?", "id": "MCP3905." },
  { "en": "Sensor Partikulat PM4.0?", "id": "SPS30." },
  { "en": "Modul Pengenalan Gerakan?", "id": "APDS-9960 Module." },
  { "en": "IC Flip-Flop D-Type Dengan Set Sinkron?", "id": "74LS175." },
  { "en": "IC Penguat Operasional JFET Presisi?", "id": "OPA627." },
  { "en": "Transistor NPN Amplifier VHF?", "id": "BFR96." },
  { "en": "Dioda Penyearah Arus 6A?", "id": "6A10." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Hex?", "id": "74HC132." },
  { "en": "Sensor Suhu Dan Kelembaban Akurasi Tinggi?", "id": "SHT31-D." },
  { "en": "Regulator Tegangan Negatif 1.2V-37V?", "id": "LM337T." },
  { "en": "Transistor MOSFET P-Channel Daya?", "id": "IRF4905." },
  { "en": "Dioda Zener Tegangan 2.4V?", "id": "1N4728A." },
  { "en": "IC Penguat Audio Kelas D Ganda?", "id": "TPA3110D2." },
  { "en": "Modul Sensor Warna Presisi Tinggi?", "id": "AS7262." },
  { "en": "IC Latch Transparan Octal?", "id": "74HC373." },
  { "en": "Papan Pengembangan Berbasis Teensy?", "id": "Teensy 4.0." },
  { "en": "Sensor Aliran Cairan?", "id": "YF-B1." },
  { "en": "Regulator Tegangan Positif 3A?", "id": "LM1085." },
  { "en": "IC Driver LED Matriks?", "id": "MAX7219CNG." },
  { "en": "Transistor PNP Amplifier Audio?", "id": "A1015." },
  { "en": "Dioda Schottky Penghalang Silikon?", "id": "1N5822." },
  { "en": "Gerbang Logika OR-AND-INVERT?", "id": "MC14502B." },
  { "en": "IC Penerima GPS Presisi Tinggi?", "id": "ZED-F9P." },
  { "en": "Transistor JFET Kanal-P?", "id": "J175." },
  { "en": "Sensor Flex (Lentur)?", "id": "Spectra Symbol Flex." },
  { "en": "IC Penguat Audio OCL 150W?", "id": "Sanken 2SC3858." },
  { "en": "Dioda Zener Tegangan 1.8V?", "id": "1N4727A." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Phototherapy?", "id": "Phototherapy LED." },
  { "en": "IC Pengontrol Kipas PWM?", "id": "MAX6650." },
  { "en": "IC Pengontrol Power-Over-Ethernet (PoE)?", "id": "Si3402." },
  { "en": "Transistor NPN Amplifier RF Kebisingan Rendah?", "id": "BFP420." },
  { "en": "Dioda Zener Tegangan 8.7V?", "id": "1N5345B." },
  { "en": "Gerbang Logika NAND Dengan 13 Input?", "id": "CD4068B." },
  { "en": "Papan Pengembangan Berbasis BeagleBone?", "id": "BeagleBone Black." },
  { "en": "Sensor Muatan Listrik (Load Cell)?", "id": "TAL220." },
  { "en": "IC Audio Codec Stereo?", "id": "WM8731." },
  { "en": "Transistor Digital PNP Dengan Resistor?", "id": "DTA114E." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Encoder Optik?", "id": "HSDL-4261." },
  { "en": "IC ADC 22-Bit Resolusi Sangat Tinggi?", "id": "AD7705." },
  { "en": "Papan FPGA Berbasis ECP5?", "id": "ECP5 Versa." },
  { "en": "Sensor Gas Formaldehida (CH2O)?", "id": "ZE08-CH2O." },
  { "en": "IC Konverter Tegangan Ke Frekuensi Presisi?", "id": "VFC32." },
  { "en": "Dioda Pelindung TVS Untuk Video Line?", "id": "RClamp0504F." },
  { "en": "IC Penguat Diferensial Arus Sangat Tinggi?", "id": "LTC6268." },
  { "en": "Sensor Kecepatan Aliran Udara?", "id": "FS3000." },
  { "en": "Gerbang Logika NOR Dengan Input 4 Ganda?", "id": "74HC4002." },
  { "en": "IC Driver Motor Stepper Dengan Antarmuka SPI?", "id": "TMC2130." },
  { "en": "Regulator Switching Topologi Interleaved?", "id": "LTC3892." },
  { "en": "Dioda Penyearah Jembatan 25A?", "id": "GBJ2510." },
  { "en": "IC DAC Audio 32-bit Kinerja Tinggi?", "id": "ES9028Q2M." },
  { "en": "Sensor Getaran Dan Guncangan Piezo?", "id": "Piezo Vibration Sensor." },
  { "en": "IC Flip-Flop T-Type Tunggal Dengan Reset?", "id": "74LVC1G74." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Daya 50W?", "id": "UV LED 50W." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk Seluler?", "id": "QPA9903." },
  { "en": "Regulator Tegangan Linear Referensi Presisi?", "id": "ADR4525." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "CSD18534KCS." },
  { "en": "IC Penguat Audio Kelas AB 50W Ganda?", "id": "LM4766." },
  { "en": "IC Gerbang AND CMOS Delapan Input?", "id": "CD4068B." },
  { "en": "Dioda Zener Tegangan 39V?", "id": "1N4754A." },
  { "en": "IC Penguat Logaritma Dengan Output Tegangan?", "id": "LOG114." },
  { "en": "Sensor Suhu Dan Kelembaban HIH8120?", "id": "HIH8120." },
  { "en": "Regulator Switching Untuk Pengisi Daya USB-C?", "id": "TPS65982." },
  { "en": "Transistor NPN Komplementer 2SA1020?", "id": "2SC2655." },
  { "en": "Dioda Pemulihan Cepat Arus Ultra Tinggi?", "id": "RURG3060." },
  { "en": "Sensor Gas Aseton?", "id": "Sensor MQ-138." },
  { "en": "Gerbang Logika OR-AND-INVERT?", "id": "MC14502B." },
  { "en": "IC Penerima GPS Presisi Tinggi?", "id": "ZED-F9P." },
  { "en": "Transistor JFET Kanal-P?", "id": "J175." },
  { "en": "Sensor Flex (Lentur)?", "id": "Spectra Symbol Flex." },
  { "en": "IC Penguat Audio OCL 150W?", "id": "Sanken 2SC3858." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Phototherapy?", "id": "Phototherapy LED." },
  { "en": "IC Pengontrol Kipas PWM?", "id": "MAX6650." },
  { "en": "Papan Pengembangan Berbasis CPLD?", "id": "Altera MAX II." },
  { "en": "IC Penguat Operasional Arus Sangat Rendah?", "id": "LMP2012." },
  { "en": "Dioda Penyearah Jembatan 3A SMD?", "id": "MB3510." },
  { "en": "Sensor Magnetometer Dan Giroskop?", "id": "LSM9DS1." },
  { "en": "Modul Tampilan LCD Grafis Dengan Touchscreen?", "id": "LCD TFT Resistive." },
  { "en": "LED (Light Emitting Diode) UV-C Untuk Desinfeksi Air?", "id": "LED UVC Water Purifier." },
  { "en": "IC Counter Biner 8-Bit?", "id": "74HC590." },
  { "en": "IC Driver Full-Bridge Motor?", "id": "L298." },
  { "en": "Sensor Tekanan Ban Mobil (TPMS)?", "id": "Sensor TPMS." },
  { "en": "Op-Amp BiFET Tegangan Tinggi?", "id": "OPA129." },
  { "en": "Regulator Tegangan Negatif Arus Rendah?", "id": "LP2950." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5779." },
  { "en": "Dioda Zener Tegangan 16V?", "id": "1N4745A." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Tunggal?", "id": "74LVC1G132." },
  { "en": "Kabel Serat Optik Single-Mode?", "id": "Kabel Fiber Optik OS2." },
  { "en": "Modul Sensor Gerakan Optik?", "id": "ADNS-3050." },
  { "en": "IC Penguat Audio Kelas AB Daya Tinggi?", "id": "LM4766." },
  { "en": "Transistor NPN Untuk Switching Cepat?", "id": "MPS2222A." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Keamanan?", "id": "IR Security LED." },
  { "en": "IC ADC I2S Untuk Mikrofon?", "id": "ICS-43432." },
  { "en": "Papan Pengembangan FPGA Berbasis Zynq?", "id": "Digilent Arty Z7." },
  { "en": "Sensor Gas Campuran?", "id": "Sensor BME680." },
  { "en": "Transistor Array Darlington Dengan Dioda Freewheeling?", "id": "ULN2003A." },
  { "en": "IC Konverter Arus Ke Tegangan?", "id": "RVC1001." },
  { "en": "Dioda Pelindung Tegangan Berlebih Array?", "id": "SP0503BAHTG." },
  { "en": "IC Penguat Diferensial Arus Tinggi?", "id": "AD8130." },
  { "en": "Sensor Aliran Gas?", "id": "Sensor AWM5104VN." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3014?", "id": "LED SMD 3014." },
  { "en": "Gerbang Logika XNOR CMOS Dengan Output Buffer?", "id": "CD4077B." },
  { "en": "IC Pengontrol Motor DC Dengan Encoder?", "id": "VNH5019." },
  { "en": "Regulator Switching Topologi Boost?", "id": "LM2587." },
  { "en": "Dioda Zener Tegangan 36V?", "id": "1N4753A." },
  { "en": "IC ADC I2C 18-Bit?", "id": "MCP3421." },
  { "en": "Sensor IMU (Inertial Measurement Unit) Dengan GPS?", "id": "Adafruit Ultimate GPS." },
  { "en": "Transistor PNP Frekuensi Radio Daya Sedang?", "id": "2SA1213." },
  { "en": "IC Penguat Operasional Dengan Arus Bias Sangat Rendah?", "id": "OPA128." },
  { "en": "Modul Kamera Lensa Telefoto?", "id": "Kamera Lensa Tele." },
  { "en": "IC Switch Analog Dengan Proteksi Arus Berlebih?", "id": "ADG5433." },
  { "en": "LED (Light Emitting Diode) Dengan Lensa Fresnel?", "id": "LED Lensa Fresnel." },
  { "en": "IC Multiplexer CMOS 4-Channel Ganda?", "id": "CD4052BE." },
  { "en": "Sensor Kelembaban Dan Suhu SHT35?", "id": "SHT35." },
  { "en": "Transistor Bipolar Gerbang Terisolasi?", "id": "FGH40N60SMD." },
  { "en": "Dioda Penyearah Jembatan Untuk PCB?", "id": "RB156." },
  { "en": "IC DAC Audio Untuk Sistem Hi-Fi?", "id": "AK4493EQ." },
  { "en": "Sensor pH Digital Dengan Suhu?", "id": "DFRobot pH Sensor." },
  { "en": "IC Flip-Flop D-Type Dengan Set Asinkron?", "id": "74HC74." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3030?", "id": "LED SMD 3030." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk Satelit?", "id": "HMC451LP3." },
  { "en": "Regulator Tegangan Linear Arus Sangat Rendah?", "id": "ADP121." },
  { "en": "Transistor Efek Medan Semikonduktor Galium Arsenida?", "id": "GaAs FET." },
  { "en": "Dioda Penekan ESD (Electrostatic Discharge) Untuk USB?", "id": "TPD4S012." },
  { "en": "IC Penguat Audio Kelas G?", "id": "LM49251." },
  { "en": "Sensor Oksigen Terlarut Dengan Elektroda?", "id": "Sensor DO Elektroda." },
  { "en": "Modul Tampilan LCD Karakter Dengan Antarmuka SPI?", "id": "LCD SPI 16x2." },
  { "en": "IC Gerbang AND CMOS Kolektor Terbuka Buffer?", "id": "CD4081UB." },
  { "en": "Transistor NPN Komplementer 2N3906?", "id": "2N3904." },
  { "en": "IC Transceiver CAN Bus?", "id": "MCP2551." },
  { "en": "Transistor MOSFET N-Channel Dengan Logic Level Gate?", "id": "IRL540N." },
  { "en": "Dioda Zener Tegangan 6.8V?", "id": "1N4736A." },
  { "en": "Gerbang Logika AND Dengan 8 Input?", "id": "74HCT30." },
  { "en": "Papan Pengembangan Arduino Ukuran Besar?", "id": "Arduino Mega 2560." },
  { "en": "Sensor Indeks UV (Ultra Violet)?", "id": "VEML6070." },
  { "en": "IC Prosesor Gema Digital (Echo)?", "id": "PT2399." },
  { "en": "Transistor Digital NPN Dengan Resistor Internal?", "id": "FJN3303R." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Remote Control?", "id": "TSAL6200." },
  { "en": "IC ADC 24-Bit Dengan PGA?", "id": "ADS1220." },
  { "en": "Papan FPGA Berbasis iCE40 UltraPlus?", "id": "UPduino." },
  { "en": "Sensor Kualitas Udara VOC Dan Formaldehida?", "id": "CJMCU-8128." },
  { "en": "IC Konverter Tegangan Ke Frekuensi Presisi?", "id": "LM331." },
  { "en": "Dioda Pelindung TVS Untuk Port USB?", "id": "USBLC6-2SC6." },
  { "en": "IC Penguat Diferensial Arus Sangat Tinggi?", "id": "AD8138." },
  { "en": "Sensor Kecepatan Aliran Udara Anemometer?", "id": "FS1000A." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 0201?", "id": "LED SMD 0201." },
  { "en": "Gerbang Logika NOR Dengan Input 3 Ganda?", "id": "74HC27." },
  { "en": "IC Driver Motor Stepper Dengan Microstepping?", "id": "TMC2208." },
  { "en": "Regulator Switching Topologi Resonant LLC?", "id": "L6599." },
  { "en": "Transistor PNP Amplifier RF (Radio Frequency)?", "id": "BFG591." },
  { "en": "Dioda Penyearah Jembatan 25A?", "id": "GBJ2510." },
  { "en": "IC DAC Audio 32-bit Kinerja Tinggi?", "id": "ES9028Q2M." },
  { "en": "Sensor Getaran Dan Guncangan Piezo?", "id": "Piezo Vibration Sensor." },
  { "en": "IC Flip-Flop T-Type Tunggal Dengan Reset?", "id": "74LVC1G74." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Daya 50W?", "id": "UV LED 50W." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk Seluler?", "id": "QPA9903." },
  { "en": "Regulator Tegangan Linear Referensi Presisi?", "id": "ADR4525." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "CSD18534KCS." },
  { "en": "IC Penguat Audio Kelas AB 50W Ganda?", "id": "LM4766." },
  { "en": "Gerbang Logika AND CMOS Delapan Input?", "id": "CD4068B." },
  { "en": "Dioda Zener Tegangan 39V?", "id": "1N4754A." },
  { "en": "IC Penguat Logaritma Dengan Output Tegangan?", "id": "LOG114." },
  { "en": "Sensor Suhu Dan Kelembaban HIH8120?", "id": "HIH8120." },
  { "en": "Regulator Switching Untuk Pengisi Daya USB-C?", "id": "TPS65982." },
  { "en": "Transistor NPN Komplementer 2SA1020?", "id": "2SC2655." },
  { "en": "Dioda Pemulihan Cepat Arus Ultra Tinggi?", "id": "RURG3060." },
  { "en": "Sensor Gas Aseton?", "id": "Sensor MQ-138." },
  { "en": "IC Penguat Operasional JFET Presisi?", "id": "OPA627." },
  { "en": "Transistor NPN Amplifier VHF?", "id": "BFR96." },
  { "en": "Dioda Penyearah Arus 6A?", "id": "6A10." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Hex?", "id": "74HC132." },
  { "en": "Sensor Suhu Dan Kelembaban Akurasi Tinggi?", "id": "SHT31-D." },
  { "en": "Regulator Tegangan Negatif 1.2V-37V?", "id": "LM337T." },
  { "en": "Transistor MOSFET P-Channel Daya?", "id": "IRF4905." },
  { "en": "Dioda Zener Tegangan 2.4V?", "id": "1N4728A." },
  { "en": "IC Penguat Audio Kelas D Ganda?", "id": "TPA3110D2." },
  { "en": "Modul Sensor Warna Presisi Tinggi?", "id": "AS7262." },
  { "en": "IC Latch Transparan Octal?", "id": "74HC373." },
  { "en": "Papan Pengembangan Berbasis Teensy?", "id": "Teensy 4.0." },
  { "en": "Sensor Aliran Cairan?", "id": "YF-B1." },
  { "en": "Regulator Tegangan Positif 3A?", "id": "LM1085." },
  { "en": "IC Driver LED Matriks?", "id": "MAX7219CNG." },
  { "en": "Transistor PNP Amplifier Audio?", "id": "A1015." },
  { "en": "Dioda Schottky Penghalang Silikon?", "id": "1N5822." },
  { "en": "Gerbang Logika OR-AND-INVERT?", "id": "MC14502B." },
  { "en": "IC Penerima GPS Presisi Tinggi?", "id": "ZED-F9P." },
  { "en": "Transistor JFET Kanal-P?", "id": "J175." },
  { "en": "Sensor Flex (Lentur)?", "id": "Spectra Symbol Flex." },
  { "en": "IC Penguat Audio OCL 150W?", "id": "Sanken 2SC3858." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Phototherapy?", "id": "Phototherapy LED." },
  { "en": "IC Pengontrol Kipas PWM?", "id": "MAX6650." },
  { "en": "Papan Pengembangan Berbasis CPLD?", "id": "Altera MAX II." },
  { "en": "IC Penguat Operasional Arus Sangat Rendah?", "id": "LMP2012." },
  { "en": "Dioda Penyearah Jembatan 3A SMD?", "id": "MB3510." },
  { "en": "Sensor Magnetometer Dan Giroskop?", "id": "LSM9DS1." },
  { "en": "Modul Tampilan LCD Grafis Dengan Touchscreen?", "id": "LCD TFT Resistive." },
  { "en": "LED (Light Emitting Diode) UV-C Untuk Desinfeksi Air?", "id": "LED UVC Water Purifier." },
  { "en": "IC Counter Biner 8-Bit?", "id": "74HC590." },
  { "en": "IC Driver Full-Bridge Motor?", "id": "L298." },
  { "en": "Sensor Tekanan Ban Mobil (TPMS)?", "id": "Sensor TPMS." },
  { "en": "Op-Amp BiFET Tegangan Tinggi?", "id": "OPA129." },
  { "en": "Regulator Tegangan Negatif Arus Rendah?", "id": "LP2950." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5779." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Tunggal?", "id": "74LVC1G132." },
  { "en": "Kabel Serat Optik Single-Mode?", "id": "Kabel Fiber Optik OS2." },
  { "en": "Modul Sensor Gerakan Optik?", "id": "ADNS-3050." },
  { "en": "IC Penguat Audio Kelas AB Daya Tinggi?", "id": "LM4766." },
  { "en": "Transistor NPN Untuk Switching Cepat?", "id": "MPS2222A." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Keamanan?", "id": "IR Security LED." },
  { "en": "IC ADC I2S Untuk Mikrofon?", "id": "ICS-43432." },
  { "en": "Papan Pengembangan FPGA Berbasis Zynq?", "id": "Digilent Arty Z7." },
  { "en": "Sensor Gas Campuran?", "id": "Sensor BME680." },
  { "en": "Transistor Array Darlington Dengan Dioda Freewheeling?", "id": "ULN2003A." },
  { "en": "IC Konverter Arus Ke Tegangan?", "id": "RVC1001." },
  { "en": "Dioda Pelindung Tegangan Berlebih Array?", "id": "SP0503BAHTG." },
  { "en": "IC Penguat Diferensial Arus Tinggi?", "id": "AD8130." },
  { "en": "Sensor Aliran Gas?", "id": "Sensor AWM5104VN." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3014?", "id": "LED SMD 3014." },
  { "en": "Gerbang Logika XNOR CMOS Dengan Output Buffer?", "id": "CD4077B." },
  { "en": "IC Pengontrol Motor DC Dengan Encoder?", "id": "VNH5019." },
  { "en": "Regulator Switching Topologi Boost?", "id": "LM2587." },
  { "en": "Dioda Zener Tegangan 36V?", "id": "1N4753A." },
  { "en": "IC ADC I2C 18-Bit?", "id": "MCP3421." },
  { "en": "Sensor IMU (Inertial Measurement Unit) Dengan GPS?", "id": "Adafruit Ultimate GPS." },
  { "en": "Transistor PNP Frekuensi Radio Daya Sedang?", "id": "2SA1213." },
  { "en": "IC Penguat Operasional Dengan Arus Bias Sangat Rendah?", "id": "OPA128." },
  { "en": "Modul Kamera Lensa Telefoto?", "id": "Kamera Lensa Tele." },
  { "en": "IC Transceiver RS-485?", "id": "MAX485." },
  { "en": "Transistor MOSFET N-Channel Dengan Logic Level?", "id": "IRL520N." },
  { "en": "Dioda Zener Tegangan 7.5V?", "id": "1N4737A." },
  { "en": "Gerbang Logika Buffer/Line Driver Non-Inverting?", "id": "74HC244." },
  { "en": "Papan Pengembangan Berbasis ATmega2560?", "id": "Arduino Mega." },
  { "en": "Sensor Indeks UV (Ultra Violet) Analog?", "id": "GUVA-S12SD." },
  { "en": "IC Pengontrol Pengisian Baterai Li-Ion?", "id": "BQ24610." },
  { "en": "Transistor Digital PNP Dengan Resistor Bawaan?", "id": "FJN4303R." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Komunikasi Data?", "id": "IrDA LED." },
  { "en": "IC ADC 24-Bit Untuk Sensor Jembatan?", "id": "HX711." },
  { "en": "Papan FPGA Berbasis MachXO3?", "id": "Lattice MachXO3." },
  { "en": "Sensor Gas Karbon Monoksida (CO)?", "id": "MQ-9." },
  { "en": "IC Konverter Frekuensi Ke Tegangan?", "id": "TC9400." },
  { "en": "Dioda Pelindung TVS Untuk Port VGA?", "id": "SP0504BAHTG." },
  { "en": "IC Penguat Diferensial Untuk Video?", "id": "THS4509." },
  { "en": "Sensor Jarak Time-of-Flight Jarak Jauh?", "id": "TFmini Plus." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 1204?", "id": "LED SMD 1204." },
  { "en": "Gerbang Logika NAND Dengan Input 12?", "id": "74HCT133." },
  { "en": "IC Driver Motor Stepper Dengan Antarmuka Paralel?", "id": "SLA7062M." },
  { "en": "Regulator Switching Topologi Push-Pull?", "id": "UC3825." },
  { "en": "Transistor PNP Amplifier RF (Radio Frequency)?", "id": "BFG591." },
  { "en": "Dioda Penyearah Jembatan 25A?", "id": "GBJ2510." },
  { "en": "IC DAC Audio 32-bit Kinerja Tinggi?", "id": "ES9028Q2M." },
  { "en": "Sensor Getaran Dan Guncangan Piezo?", "id": "Piezo Vibration Sensor." },
  { "en": "IC Flip-Flop T-Type Tunggal Dengan Reset?", "id": "74LVC1G74." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Daya 50W?", "id": "UV LED 50W." },
  { "en": "IC Penguat Daya RF (Radio Frequency) Untuk Seluler?", "id": "QPA9903." },
  { "en": "Regulator Tegangan Linear Referensi Presisi?", "id": "ADR4525." },
  { "en": "Transistor Efek Medan Semikonduktor Oksida Logam?", "id": "CSD18534KCS." },
  { "en": "Gerbang Logika AND CMOS Delapan Input?", "id": "CD4068B." },
  { "en": "Dioda Zener Tegangan 39V?", "id": "1N4754A." },
  { "en": "IC Penguat Logaritma Dengan Output Tegangan?", "id": "LOG114." },
  { "en": "Sensor Suhu Dan Kelembaban HIH8120?", "id": "HIH8120." },
  { "en": "Regulator Switching Untuk Pengisi Daya USB-C?", "id": "TPS65982." },
  { "en": "Transistor NPN Komplementer 2SA1020?", "id": "2SC2655." },
  { "en": "Dioda Pemulihan Cepat Arus Ultra Tinggi?", "id": "RURG3060." },
  { "en": "Sensor Gas Aseton?", "id": "Sensor MQ-138." },
  { "en": "Papan Pengembangan Berbasis Teensy?", "id": "Teensy 4.1." },
  { "en": "Transistor NPN Amplifier VHF?", "id": "BFR96." },
  { "en": "Dioda Penyearah Arus 6A?", "id": "6A10." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Hex?", "id": "74HC132." },
  { "en": "Sensor Suhu Dan Kelembaban Akurasi Tinggi?", "id": "SHT31-D." },
  { "en": "Regulator Tegangan Negatif 1.2V-37V?", "id": "LM337T." },
  { "en": "Transistor MOSFET P-Channel Daya?", "id": "IRF4905." },
  { "en": "Dioda Zener Tegangan 2.4V?", "id": "1N4728A." },
  { "en": "IC Penguat Audio Kelas D Ganda?", "id": "TPA3110D2." },
  { "en": "Modul Sensor Warna Presisi Tinggi?", "id": "AS7262." },
  { "en": "IC Latch Transparan Octal?", "id": "74HC373." },
  { "en": "Sensor Aliran Cairan?", "id": "YF-B1." },
  { "en": "Regulator Tegangan Positif 3A?", "id": "LM1085." },
  { "en": "IC Driver LED Matriks?", "id": "MAX7219CNG." },
  { "en": "Transistor PNP Amplifier Audio?", "id": "A1015." },
  { "en": "Dioda Schottky Penghalang Silikon?", "id": "1N5822." },
  { "en": "Gerbang Logika OR-AND-INVERT?", "id": "MC14502B." },
  { "en": "IC Penerima GPS Presisi Tinggi?", "id": "ZED-F9P." },
  { "en": "Transistor JFET Kanal-P?", "id": "J175." },
  { "en": "Sensor Flex (Lentur)?", "id": "Spectra Symbol Flex." },
  { "en": "IC Penguat Audio OCL 150W?", "id": "Sanken 2SC3858." },
  { "en": "LED (Light Emitting Diode) UV (Ultra Violet) Phototherapy?", "id": "Phototherapy LED." },
  { "en": "IC Pengontrol Kipas PWM?", "id": "MAX6650." },
  { "en": "Papan Pengembangan Berbasis CPLD?", "id": "Altera MAX II." },
  { "en": "IC Penguat Operasional Arus Sangat Rendah?", "id": "LMP2012." },
  { "en": "Dioda Penyearah Jembatan 3A SMD?", "id": "MB3510." },
  { "en": "Sensor Magnetometer Dan Giroskop?", "id": "LSM9DS1." },
  { "en": "Modul Tampilan LCD Grafis Dengan Touchscreen?", "id": "LCD TFT Resistive." },
  { "en": "LED (Light Emitting Diode) UV-C Untuk Desinfeksi Air?", "id": "LED UVC Water Purifier." },
  { "en": "IC Counter Biner 8-Bit?", "id": "74HC590." },
  { "en": "IC Driver Full-Bridge Motor?", "id": "L298." },
  { "en": "Sensor Tekanan Ban Mobil (TPMS)?", "id": "Sensor TPMS." },
  { "en": "Op-Amp BiFET Tegangan Tinggi?", "id": "OPA129." },
  { "en": "Regulator Tegangan Negatif Arus Rendah?", "id": "LP2950." },
  { "en": "Transistor PNP Fotodarlington?", "id": "2N5779." },
  { "en": "Gerbang Logika NAND Schmitt-Trigger Tunggal?", "id": "74LVC1G132." },
  { "en": "Kabel Serat Optik Single-Mode?", "id": "Kabel Fiber Optik OS2." },
  { "en": "Modul Sensor Gerakan Optik?", "id": "ADNS-3050." },
  { "en": "IC Penguat Audio Kelas AB Daya Tinggi?", "id": "LM4766." },
  { "en": "Transistor NPN Untuk Switching Cepat?", "id": "MPS2222A." },
  { "en": "LED (Light Emitting Diode) Inframerah Untuk Keamanan?", "id": "IR Security LED." },
  { "en": "IC ADC I2S Untuk Mikrofon?", "id": "ICS-43432." },
  { "en": "Papan Pengembangan FPGA Berbasis Zynq?", "id": "Digilent Arty Z7." },
  { "en": "Sensor Gas Campuran?", "id": "Sensor BME680." },
  { "en": "Transistor Array Darlington Dengan Dioda Freewheeling?", "id": "ULN2003A." },
  { "en": "IC Konverter Arus Ke Tegangan?", "id": "RVC1001." },
  { "en": "Dioda Pelindung Tegangan Berlebih Array?", "id": "SP0503BAHTG." },
  { "en": "IC Penguat Diferensial Arus Tinggi?", "id": "AD8130." },
  { "en": "Sensor Aliran Gas?", "id": "Sensor AWM5104VN." },
  { "en": "LED (Light Emitting Diode) SMD Ukuran 3014?", "id": "LED SMD 3014." },
  { "en": "Gerbang Logika XNOR CMOS Dengan Output Buffer?", "id": "CD4077B." },
  { "en": "IC Pengontrol Motor DC Dengan Encoder?", "id": "VNH5019." },
  { "en": "Regulator Switching Topologi Boost?", "id": "LM2587." },
  { "en": "Dioda Zener Tegangan 36V?", "id": "1N4753A." },
  { "en": "IC ADC I2C 18-Bit?", "id": "MCP3421." },
  { "en": "Sensor IMU (Inertial Measurement Unit) Dengan GPS?", "id": "Adafruit Ultimate GPS." },
  { "en": "Transistor PNP Frekuensi Radio Daya Sedang?", "id": "2SA1213." },
  { "en": "IC Penguat Operasional Dengan Arus Bias Sangat Rendah?", "id": "OPA128." },
  { "en": "Modul Kamera Lensa Telefoto?", "id": "Kamera Lensa Tele." }



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
