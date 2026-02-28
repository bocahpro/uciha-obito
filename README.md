# uciha-obito
yamete
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nihongo Master - AI Agent Belajar Bahasa Jepang</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&family=Noto+Sans:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans', sans-serif;
        }
        .jp-font {
            font-family: 'Noto Sans JP', sans-serif;
        }
        .gradient-text {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .card-hover {
            transition: all 0.3s ease;
        }
        .card-hover:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        .animate-float {
            animation: float 6s ease-in-out infinite;
        }
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }
        .progress-ring {
            transition: stroke-dashoffset 0.35s;
            transform: rotate(-90deg);
            transform-origin: 50% 50%;
        }
        .typing-cursor::after {
            content: '|';
            animation: blink 1s infinite;
        }
        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }
        .sakura {
            position: absolute;
            width: 20px;
            height: 20px;
            background: radial-gradient(circle, #ffb7c5 0%, #ffc0cb 100%);
            border-radius: 50% 0 50% 50%;
            animation: fall linear infinite;
            opacity: 0.6;
        }
        @keyframes fall {
            to {
                transform: translateY(100vh) rotate(360deg);
            }
        }
        .chat-bubble {
            position: relative;
            max-width: 80%;
            padding: 12px 16px;
            border-radius: 20px;
            margin: 8px 0;
        }
        .chat-bubble.user {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 4px;
        }
        .chat-bubble.bot {
            background: #f3f4f6;
            color: #1f2937;
            margin-right: auto;
            border-bottom-left-radius: 4px;
        }
    </style>
<base target="_blank">
</head>
<body class="bg-gray-50 min-h-screen overflow-x-hidden">

    <!-- Sakura Animation Container -->
    <div id="sakura-container" class="fixed inset-0 pointer-events-none z-0"></div>

    <!-- Navigation -->
    <nav class="bg-white/80 backdrop-blur-md shadow-sm sticky top-0 z-50 border-b border-gray-200">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16 items-center">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-gradient-to-br from-pink-500 to-purple-600 rounded-xl flex items-center justify-center text-white font-bold text-xl shadow-lg">
                        日
                    </div>
                    <span class="text-2xl font-bold bg-gradient-to-r from-pink-600 to-purple-600 bg-clip-text text-transparent">
                        Nihongo Master
                    </span>
                </div>
                <div class="hidden md:flex space-x-8">
                    <button onclick="showSection('dashboard')" class="nav-btn text-gray-700 hover:text-purple-600 font-medium transition">Dashboard</button>
                    <button onclick="showSection('lessons')" class="nav-btn text-gray-700 hover:text-purple-600 font-medium transition">Pelajaran</button>
                    <button onclick="showSection('practice')" class="nav-btn text-gray-700 hover:text-purple-600 font-medium transition">Latihan</button>
                    <button onclick="showSection('chat')" class="nav-btn text-gray-700 hover:text-purple-600 font-medium transition">AI Sensei</button>
                </div>
                <div class="flex items-center space-x-4">
                    <div class="text-sm text-gray-600">
                        <span class="font-semibold text-purple-600" id="streak-count">🔥 3 Hari</span>
                    </div>
                    <div class="w-10 h-10 rounded-full bg-gradient-to-r from-yellow-400 to-orange-500 flex items-center justify-center text-white font-bold shadow-md">
                        YOU
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 relative z-10">

        <!-- Dashboard Section -->
        <section id="dashboard" class="space-y-8">
            <!-- Hero -->
            <div class="bg-gradient-to-r from-purple-600 to-pink-600 rounded-3xl p-8 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute top-0 right-0 w-64 h-64 bg-white opacity-10 rounded-full -mr-20 -mt-20 animate-float"></div>
                <div class="relative z-10">
                    <h1 class="text-4xl font-bold mb-4">おはようございます! (Selamat Pagi!)</h1>
                    <p class="text-xl opacity-90 mb-6">Siap untuk menguasai bahasa Jepang hari ini?</p>
                    <div class="flex space-x-4">
                        <button onclick="showSection('lessons')" class="bg-white text-purple-600 px-6 py-3 rounded-full font-bold hover:bg-gray-100 transition shadow-lg">
                            Mulai Belajar
                        </button>
                        <button onclick="showSection('chat')" class="bg-transparent border-2 border-white text-white px-6 py-3 rounded-full font-bold hover:bg-white hover:text-purple-600 transition">
                            Ngobrol dengan AI
                        </button>
                    </div>
                </div>
            </div>

            <!-- Progress Overview -->
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
                <div class="bg-white rounded-2xl p-6 shadow-lg card-hover">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-bold text-gray-800">Level Saat Ini</h3>
                        <span class="text-2xl">🎯</span>
                    </div>
                    <p class="text-3xl font-bold text-purple-600 mb-1">N5</p>
                    <p class="text-sm text-gray-500">Pemula (Dasar)</p>
                    <div class="mt-4 w-full bg-gray-200 rounded-full h-2">
                        <div class="bg-purple-600 h-2 rounded-full" style="width: 35%"></div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-6 shadow-lg card-hover">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-bold text-gray-800">Kosakata</h3>
                        <span class="text-2xl">📚</span>
                    </div>
                    <p class="text-3xl font-bold text-pink-600 mb-1">245</p>
                    <p class="text-sm text-gray-500">kata dipelajari</p>
                    <div class="mt-4 w-full bg-gray-200 rounded-full h-2">
                        <div class="bg-pink-600 h-2 rounded-full" style="width: 12%"></div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-6 shadow-lg card-hover">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-bold text-gray-800">Kanji</h3>
                        <span class="text-2xl">🈳</span>
                    </div>
                    <p class="text-3xl font-bold text-blue-600 mb-1">52</p>
                    <p class="text-sm text-gray-500">karakter dikuasai</p>
                    <div class="mt-4 w-full bg-gray-200 rounded-full h-2">
                        <div class="bg-blue-600 h-2 rounded-full" style="width: 8%"></div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-6 shadow-lg card-hover">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-bold text-gray-800">Streak</h3>
                        <span class="text-2xl">🔥</span>
                    </div>
                    <p class="text-3xl font-bold text-orange-600 mb-1">3 Hari</p>
                    <p class="text-sm text-gray-500">belajar berturut-turut</p>
                    <div class="mt-4 w-full bg-gray-200 rounded-full h-2">
                        <div class="bg-orange-600 h-2 rounded-full" style="width: 60%"></div>
                    </div>
                </div>
            </div>

            <!-- Daily Quest -->
            <div class="bg-white rounded-2xl p-6 shadow-lg">
                <h2 class="text-2xl font-bold text-gray-800 mb-6">Misi Harian</h2>
                <div class="space-y-4">
                    <div class="flex items-center justify-between p-4 bg-gray-50 rounded-xl hover:bg-gray-100 transition cursor-pointer" onclick="showSection('lessons')">
                        <div class="flex items-center space-x-4">
                            <div class="w-12 h-12 bg-purple-100 rounded-full flex items-center justify-center text-2xl">📖</div>
                            <div>
                                <h3 class="font-bold text-gray-800">Pelajari 10 Kosakata Baru</h3>
                                <p class="text-sm text-gray-500">0/10 selesai</p>
                            </div>
                        </div>
                        <div class="flex items-center space-x-2">
                            <span class="text-yellow-500 font-bold">+20 XP</span>
                            <div class="w-6 h-6 rounded-full border-2 border-gray-300"></div>
                        </div>
                    </div>

                    <div class="flex items-center justify-between p-4 bg-gray-50 rounded-xl hover:bg-gray-100 transition cursor-pointer" onclick="showSection('practice')">
                        <div class="flex items-center space-x-4">
                            <div class="w-12 h-12 bg-pink-100 rounded-full flex items-center justify-center text-2xl">✍️</div>
                            <div>
                                <h3 class="font-bold text-gray-800">Latihan Kanji 15 Menit</h3>
                                <p class="text-sm text-gray-500">0/15 menit</p>
                            </div>
                        </div>
                        <div class="flex items-center space-x-2">
                            <span class="text-yellow-500 font-bold">+30 XP</span>
                            <div class="w-6 h-6 rounded-full border-2 border-gray-300"></div>
                        </div>
                    </div>

                    <div class="flex items-center justify-between p-4 bg-gray-50 rounded-xl hover:bg-gray-100 transition cursor-pointer" onclick="showSection('chat')">
                        <div class="flex items-center space-x-4">
                            <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center text-2xl">💬</div>
                            <div>
                                <h3 class="font-bold text-gray-800">Ngobrol dengan AI Sensei</h3>
                                <p class="text-sm text-gray-500">0/5 percakapan</p>
                            </div>
                        </div>
                        <div class="flex items-center space-x-2">
                            <span class="text-yellow-500 font-bold">+50 XP</span>
                            <div class="w-6 h-6 rounded-full border-2 border-gray-300"></div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Lessons Section -->
        <section id="lessons" class="hidden space-y-8">
            <div class="text-center mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">Pilih Level Anda</h2>
                <p class="text-gray-600">Dari N5 (Pemula) hingga N1 (Ahli)</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- N5 Card -->
                <div class="bg-white rounded-2xl shadow-lg overflow-hidden card-hover border-2 border-purple-200">
                    <div class="bg-gradient-to-br from-purple-500 to-purple-700 p-6 text-white">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-3xl font-bold mb-1">N5</h3>
                                <p class="opacity-90">Level Pemula</p>
                            </div>
                            <span class="bg-white/20 px-3 py-1 rounded-full text-sm">Aktif</span>
                        </div>
                    </div>
                    <div class="p-6">
                        <ul class="space-y-3 mb-6">
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-purple-100 rounded-full flex items-center justify-center text-purple-600 mr-3 text-xs font-bold">✓</span>
                                Hiragana & Katakana
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-purple-100 rounded-full flex items-center justify-center text-purple-600 mr-3 text-xs font-bold">✓</span>
                                100 Kanji Dasar
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-purple-100 rounded-full flex items-center justify-center text-purple-600 mr-3 text-xs font-bold">✓</span>
                                800 Kosakata
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-purple-100 rounded-full flex items-center justify-center text-purple-600 mr-3 text-xs font-bold">✓</span>
                                Grammar Dasar
                            </li>
                        </ul>
                        <button onclick="startLesson('n5')" class="w-full bg-purple-600 text-white py-3 rounded-xl font-bold hover:bg-purple-700 transition">
                            Lanjutkan Belajar
                        </button>
                    </div>
                </div>

                <!-- N4 Card -->
                <div class="bg-white rounded-2xl shadow-lg overflow-hidden card-hover opacity-75">
                    <div class="bg-gradient-to-br from-blue-500 to-blue-700 p-6 text-white">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-3xl font-bold mb-1">N4</h3>
                                <p class="opacity-90">Level Dasar</p>
                            </div>
                            <span class="bg-white/20 px-3 py-1 rounded-full text-sm">Terkunci</span>
                        </div>
                    </div>
                    <div class="p-6">
                        <ul class="space-y-3 mb-6">
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 mr-3 text-xs font-bold">•</span>
                                300 Kanji
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 mr-3 text-xs font-bold">•</span>
                                1,500 Kosakata
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 mr-3 text-xs font-bold">•</span>
                                Grammar Menengah
                            </li>
                        </ul>
                        <button class="w-full bg-gray-300 text-gray-500 py-3 rounded-xl font-bold cursor-not-allowed">
                            Selesaikan N5 Terlebih Dahulu
                        </button>
                    </div>
                </div>

                <!-- N3 Card -->
                <div class="bg-white rounded-2xl shadow-lg overflow-hidden card-hover opacity-75">
                    <div class="bg-gradient-to-br from-green-500 to-green-700 p-6 text-white">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-3xl font-bold mb-1">N3</h3>
                                <p class="opacity-90">Level Menengah</p>
                            </div>
                            <span class="bg-white/20 px-3 py-1 rounded-full text-sm">Terkunci</span>
                        </div>
                    </div>
                    <div class="p-6">
                        <ul class="space-y-3 mb-6">
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-green-100 rounded-full flex items-center justify-center text-green-600 mr-3 text-xs font-bold">•</span>
                                650 Kanji
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-green-100 rounded-full flex items-center justify-center text-green-600 mr-3 text-xs font-bold">•</span>
                                3,750 Kosakata
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-green-100 rounded-full flex items-center justify-center text-green-600 mr-3 text-xs font-bold">•</span>
                                Reading Comprehension
                            </li>
                        </ul>
                        <button class="w-full bg-gray-300 text-gray-500 py-3 rounded-xl font-bold cursor-not-allowed">
                            Selesaikan N4 Terlebih Dahulu
                        </button>
                    </div>
                </div>

                <!-- N2 Card -->
                <div class="bg-white rounded-2xl shadow-lg overflow-hidden card-hover opacity-75">
                    <div class="bg-gradient-to-br from-orange-500 to-orange-700 p-6 text-white">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-3xl font-bold mb-1">N2</h3>
                                <p class="opacity-90">Level Pra-Mahir</p>
                            </div>
                            <span class="bg-white/20 px-3 py-1 rounded-full text-sm">Terkunci</span>
                        </div>
                    </div>
                    <div class="p-6">
                        <ul class="space-y-3 mb-6">
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-orange-100 rounded-full flex items-center justify-center text-orange-600 mr-3 text-xs font-bold">•</span>
                                1,000 Kanji
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-orange-100 rounded-full flex items-center justify-center text-orange-600 mr-3 text-xs font-bold">•</span>
                                6,000 Kosakata
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-orange-100 rounded-full flex items-center justify-center text-orange-600 mr-3 text-xs font-bold">•</span>
                                Business Japanese
                            </li>
                        </ul>
                        <button class="w-full bg-gray-300 text-gray-500 py-3 rounded-xl font-bold cursor-not-allowed">
                            Selesaikan N3 Terlebih Dahulu
                        </button>
                    </div>
                </div>

                <!-- N1 Card -->
                <div class="bg-white rounded-2xl shadow-lg overflow-hidden card-hover opacity-75">
                    <div class="bg-gradient-to-br from-red-500 to-red-700 p-6 text-white">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-3xl font-bold mb-1">N1</h3>
                                <p class="opacity-90">Level Mahir</p>
                            </div>
                            <span class="bg-white/20 px-3 py-1 rounded-full text-sm">Terkunci</span>
                        </div>
                    </div>
                    <div class="p-6">
                        <ul class="space-y-3 mb-6">
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-red-100 rounded-full flex items-center justify-center text-red-600 mr-3 text-xs font-bold">•</span>
                                2,000+ Kanji
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-red-100 rounded-full flex items-center justify-center text-red-600 mr-3 text-xs font-bold">•</span>
                                10,000+ Kosakata
                            </li>
                            <li class="flex items-center text-gray-700">
                                <span class="w-6 h-6 bg-red-100 rounded-full flex items-center justify-center text-red-600 mr-3 text-xs font-bold">•</span>
                                Academic & Literary
                            </li>
                        </ul>
                        <button class="w-full bg-gray-300 text-gray-500 py-3 rounded-xl font-bold cursor-not-allowed">
                            Selesaikan N2 Terlebih Dahulu
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- Practice Section -->
        <section id="practice" class="hidden space-y-8">
            <div class="text-center mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">Latihan Interaktif</h2>
                <p class="text-gray-600">Pilih jenis latihan untuk mengasah kemampuan Anda</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Flashcard Mode -->
                <div class="bg-white rounded-2xl shadow-lg p-8 card-hover cursor-pointer" onclick="startFlashcard()">
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-16 h-16 bg-gradient-to-br from-yellow-400 to-orange-500 rounded-2xl flex items-center justify-center text-3xl shadow-lg">
                            🎴
                        </div>
                        <span class="bg-green-100 text-green-800 px-3 py-1 rounded-full text-sm font-semibold">Populer</span>
                    </div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-2">Flashcard</h3>
                    <p class="text-gray-600 mb-4">Hafalkan kosakata dan kanji dengan sistem kartu pintar yang menyesuaikan dengan kemampuan Anda.</p>
                    <div class="flex items-center text-purple-600 font-semibold">
                        Mulai Latihan <span class="ml-2">→</span>
                    </div>
                </div>

                <!-- Quiz Mode -->
                <div class="bg-white rounded-2xl shadow-lg p-8 card-hover cursor-pointer" onclick="startQuiz()">
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-16 h-16 bg-gradient-to-br from-blue-400 to-indigo-500 rounded-2xl flex items-center justify-center text-3xl shadow-lg">
                            ❓
                        </div>
                        <span class="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-semibold">Tantangan</span>
                    </div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-2">Kuis Cepat</h3>
                    <p class="text-gray-600 mb-4">Uji pemahaman Anda dengan berbagai pertanyaan pilihan ganda dan isian singkat.</p>
                    <div class="flex items-center text-purple-600 font-semibold">
                        Mulai Kuis <span class="ml-2">→</span>
                    </div>
                </div>

                <!-- Listening Mode -->
                <div class="bg-white rounded-2xl shadow-lg p-8 card-hover cursor-pointer" onclick="startListening()">
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-16 h-16 bg-gradient-to-br from-pink-400 to-rose-500 rounded-2xl flex items-center justify-center text-3xl shadow-lg">
                            🎧
                        </div>
                        <span class="bg-purple-100 text-purple-800 px-3 py-1 rounded-full text-sm font-semibold">Audio</span>
                    </div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-2">Listening</h3>
                    <p class="text-gray-600 mb-4">Latih kemampuan mendengar dengan audio native speaker dan berbagai konteks percakapan.</p>
                    <div class="flex items-center text-purple-600 font-semibold">
                        Mulai Listening <span class="ml-2">→</span>
                    </div>
                </div>

                <!-- Writing Mode -->
                <div class="bg-white rounded-2xl shadow-lg p-8 card-hover cursor-pointer" onclick="startWriting()">
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-16 h-16 bg-gradient-to-br from-green-400 to-teal-500 rounded-2xl flex items-center justify-center text-3xl shadow-lg">
                            ✍️
                        </div>
                        <span class="bg-orange-100 text-orange-800 px-3 py-1 rounded-full text-sm font-semibold">Pro</span>
                    </div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-2">Menulis</h3>
                    <p class="text-gray-600 mb-4">Praktik menulis kanji dan kalimat dengan deteksi stroke order yang akurat.</p>
                    <div class="flex items-center text-purple-600 font-semibold">
                        Mulai Menulis <span class="ml-2">→</span>
                    </div>
                </div>
            </div>

            <!-- Active Practice Area -->
            <div id="practice-area" class="hidden bg-white rounded-2xl shadow-lg p-8 min-h-[400px]">
                <!-- Content will be loaded dynamically -->
            </div>
        </section>

        <!-- AI Chat Section -->
        <section id="chat" class="hidden space-y-6">
            <div class="bg-gradient-to-r from-purple-600 to-pink-600 rounded-2xl p-6 text-white mb-6">
                <h2 class="text-2xl font-bold mb-2">AI Sensei - Asisten Pribadi Anda</h2>
                <p>Tanyakan apa saja tentang bahasa Jepang, berlatih percakapan, atau minta penjelasan grammar.</p>
            </div>

            <div class="bg-white rounded-2xl shadow-lg overflow-hidden" style="height: 600px;">
                <div id="chat-messages" class="h-[480px] overflow-y-auto p-6 space-y-4 bg-gray-50">
                    <!-- Welcome Message -->
                    <div class="chat-bubble bot">
                        <p class="font-bold mb-1">AI Sensei 先生</p>
                        <p>Konnichiwa! 👋 Saya adalah AI Sensei Anda. Saya bisa membantu Anda:</p>
                        <ul class="list-disc ml-5 mt-2 space-y-1 text-sm">
                            <li>Menjelaskan grammar dan pola kalimat</li>
                            <li>Berlatih percakapan sesuai level Anda</li>
                            <li>Mengoreksi pengucapan dan tulisan</li>
                            <li>Memberikan rekomendasi belajar personal</li>
                        </ul>
                        <p class="mt-2">Apa yang ingin Anda pelajari hari ini?</p>
                    </div>
                </div>

                <div class="p-4 bg-white border-t border-gray-200">
                    <div class="flex space-x-4">
                        <input type="text" id="chat-input" placeholder="Tulis pesan dalam Bahasa Indonesia atau Jepang..." 
                               class="flex-1 px-4 py-3 border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-purple-500"
                               onkeypress="if(event.key === 'Enter') sendMessage()">
                        <button onclick="sendMessage()" class="bg-purple-600 text-white px-6 py-3 rounded-xl font-bold hover:bg-purple-700 transition flex items-center">
                            <span>Kirim</span>
                            <svg class="w-5 h-5 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path>
                            </svg>
                        </button>
                    </div>
                    <div class="flex space-x-2 mt-3 overflow-x-auto pb-2">
                        <button onclick="quickChat('Bagaimana cara mengucapkan selamat pagi?')" class="whitespace-nowrap px-3 py-1 bg-gray-100 rounded-full text-sm text-gray-600 hover:bg-purple-100 hover:text-purple-600 transition">
                            🌅 Selamat pagi
                        </button>
                        <button onclick="quickChat('Jelaskan perbedaan wa dan ga')" class="whitespace-nowrap px-3 py-1 bg-gray-100 rounded-full text-sm text-gray-600 hover:bg-purple-100 hover:text-purple-600 transition">
                            📝 Partikel wa/ga
                        </button>
                        <button onclick="quickChat('Latihan percakapan: memperkenalkan diri')" class="whitespace-nowrap px-3 py-1 bg-gray-100 rounded-full text-sm text-gray-600 hover:bg-purple-100 hover:text-purple-600 transition">
                            👋 Perkenalan
                        </button>
                        <button onclick="quickChat('Berikan 5 kosakata tentang makanan')" class="whitespace-nowrap px-3 py-1 bg-gray-100 rounded-full text-sm text-gray-600 hover:bg-purple-100 hover:text-purple-600 transition">
                            🍜 Kosakata makanan
                        </button>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- Flashcard Modal -->
    <div id="flashcard-modal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl max-w-lg w-full p-8 relative">
            <button onclick="closeFlashcard()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                </svg>
            </button>
            
            <div class="text-center">
                <div class="mb-4 text-sm text-gray-500">Kartu <span id="card-current">1</span> dari <span id="card-total">10</span></div>
                
                <div id="flashcard-content" class="bg-gradient-to-br from-purple-50 to-pink-50 rounded-2xl p-12 mb-6 cursor-pointer transition-all duration-500 transform hover:scale-105" onclick="flipCard()">
                    <div id="card-front" class="jp-font text-6xl font-bold text-gray-800 mb-4">日本</div>
                    <div id="card-back" class="hidden">
                        <div class="text-3xl font-bold text-purple-600 mb-2">にほん</div>
                        <div class="text-xl text-gray-600">Jepang</div>
                        <div class="text-sm text-gray-500 mt-2">Nihon</div>
                    </div>
                </div>

                <p class="text-gray-500 mb-6 text-sm">Ketuk kartu untuk membalik</p>

                <div class="flex justify-center space-x-4">
                    <button onclick="nextCard(false)" class="px-6 py-3 bg-red-100 text-red-600 rounded-xl font-bold hover:bg-red-200 transition">
                        Sulit ↻
                    </button>
                    <button onclick="nextCard(true)" class="px-6 py-3 bg-green-100 text-green-600 rounded-xl font-bold hover:bg-green-200 transition">
                        Mudah ✓
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Create sakura petals
        function createSakura() {
            const container = document.getElementById('sakura-container');
            const petal = document.createElement('div');
            petal.className = 'sakura';
            petal.style.left = Math.random() * 100 + 'vw';
            petal.style.animationDuration = Math.random() * 3 + 2 + 's';
            petal.style.opacity = Math.random();
            container.appendChild(petal);
            
            setTimeout(() => petal.remove(), 5000);
        }

        setInterval(createSakura, 300);

        // Navigation
        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('main > section').forEach(section => {
                section.classList.add('hidden');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.remove('hidden');
            
            // Update nav buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-purple-600');
                btn.classList.add('text-gray-700');
            });
            
            // Scroll to top
            window.scrollTo(0, 0);
        }

        // Chat functionality
        function sendMessage() {
            const input = document.getElementById('chat-input');
            const message = input.value.trim();
            if (!message) return;

            // Add user message
            addMessage(message, 'user');
            input.value = '';

            // Simulate AI response
            setTimeout(() => {
                const response = generateAIResponse(message);
                addMessage(response, 'bot');
            }, 1000);
        }

        function quickChat(message) {
            document.getElementById('chat-input').value = message;
            sendMessage();
        }

        function addMessage(text, sender) {
            const container = document.getElementById('chat-messages');
            const bubble = document.createElement('div');
            bubble.className = `chat-bubble ${sender}`;
            
            if (sender === 'bot') {
                bubble.innerHTML = `<p class="font-bold mb-1">AI Sensei 先生</p><p class="typing-cursor">${text}</p>`;
            } else {
                bubble.innerHTML = `<p>${text}</p>`;
            }
            
            container.appendChild(bubble);
            container.scrollTop = container.scrollHeight;
            
            // Remove typing cursor after animation
            if (sender === 'bot') {
                setTimeout(() => {
                    const cursor = bubble.querySelector('.typing-cursor');
                    if (cursor) cursor.classList.remove('typing-cursor');
                }, 2000);
            }
        }

        function generateAIResponse(input) {
            const responses = [
                "Bagus pertanyaannya! Dalam bahasa Jepang, penggunaan partikel は (wa) dan が (ga) memang sering membingungkan pemula...",
                "Mari kita latihan! Coba ucapkan: わたしはがくせいです (Watashi wa gakusei desu) - Saya adalah pelajar.",
                "Untuk level N5, Anda perlu menghafal sekitar 100 kanji dan 800 kosakata. Saya bisa membantu membuat jadwal belajar!",
                "Konnichiwa! 🌸 Untuk mengucapkan selamat pagi, gunakan 'おはようございます' (Ohayou gozaimasu) sebelum jam 10 pagi.",
                "Perbedaan utama: は (wa) menandakan topik, sedangkan が (ga) menandakan subjek yang melakukan aksi."
            ];
            
            // Simple keyword matching
            if (input.includes('pagi')) return responses[3];
            if (input.includes('wa') || input.includes('ga')) return responses[4];
            if (input.includes('latih')) return responses[1];
            if (input.includes('kanji') || input.includes('kosakata')) return responses[2];
            
            return responses[Math.floor(Math.random() * responses.length)];
        }

        // Flashcard functionality
        let currentCard = 0;
        let isFlipped = false;
        
        const cards = [
            { kanji: '日本', kana: 'にほん', romaji: 'Nihon', meaning: 'Jepang' },
            { kanji: '人', kana: 'ひと', romaji: 'Hito', meaning: 'Orang' },
            { kanji: '水', kana: 'みず', romaji: 'Mizu', meaning: 'Air' },
            { kanji: '火', kana: 'ひ', romaji: 'Hi', meaning: 'Api' },
            { kanji: '木', kana: 'き', romaji: 'Ki', meaning: 'Pohon' },
            { kanji: '金', kana: 'かね', romaji: 'Kane', meaning: 'Uang' },
            { kanji: '土', kana: 'つち', romaji: 'Tsuchi', meaning: 'Tanah' },
            { kanji: '日', kana: 'ひ/にち', romaji: 'Hi/Nichi', meaning: 'Hari/Matahari' },
            { kanji: '月', kana: 'つき', romaji: 'Tsuki', meaning: 'Bulan' },
            { kanji: '山', kana: 'やま', romaji: 'Yama', meaning: 'Gunung' }
        ];

        function startFlashcard() {
            currentCard = 0;
            isFlipped = false;
            updateCard();
            document.getElementById('flashcard-modal').classList.remove('hidden');
        }

        function closeFlashcard() {
            document.getElementById('flashcard-modal').classList.add('hidden');
        }

        function updateCard() {
            document.getElementById('card-current').textContent = currentCard + 1;
            document.getElementById('card-total').textContent = cards.length;
            
            const card = cards[currentCard];
            document.getElementById('card-front').textContent = card.kanji;
            document.getElementById('card-back').innerHTML = `
                <div class="text-3xl font-bold text-purple-600 mb-2">${card.kana}</div>
                <div class="text-xl text-gray-600">${card.meaning}</div>
                <div class="text-sm text-gray-500 mt-2">${card.romaji}</div>
            `;
            
            // Reset flip
            isFlipped = false;
            document.getElementById('card-front').classList.remove('hidden');
            document.getElementById('card-back').classList.add('hidden');
        }

        function flipCard() {
            isFlipped = !isFlipped;
            if (isFlipped) {
                document.getElementById('card-front').classList.add('hidden');
                document.getElementById('card-back').classList.remove('hidden');
            } else {
                document.getElementById('card-front').classList.remove('hidden');
                document.getElementById('card-back').classList.add('hidden');
            }
        }

        function nextCard(known) {
            currentCard++;
            if (currentCard >= cards.length) {
                alert('Selamat! Anda telah menyelesaikan semua kartu. 🎉');
                closeFlashcard();
                return;
            }
            updateCard();
        }

        // Practice modes
        function startQuiz() {
            const area = document.getElementById('practice-area');
            area.classList.remove('hidden');
            area.innerHTML = `
                <div class="text-center">
                    <h3 class="text-2xl font-bold mb-6">Kuis: Pilih arti yang benar</h3>
                    <div class="jp-font text-4xl font-bold mb-8 text-purple-600">ありがとう</div>
                    <div class="grid grid-cols-2 gap-4 max-w-md mx-auto">
                        <button onclick="checkAnswer(this, false)" class="p-4 bg-gray-100 rounded-xl hover:bg-gray-200 transition font-bold">Selamat tinggal</button>
                        <button onclick="checkAnswer(this, true)" class="p-4 bg-gray-100 rounded-xl hover:bg-gray-200 transition font-bold">Terima kasih</button>
                        <button onclick="checkAnswer(this, false)" class="p-4 bg-gray-100 rounded-xl hover:bg-gray-200 transition font-bold">Permisi</button>
                        <button onclick="checkAnswer(this, false)" class="p-4 bg-gray-100 rounded-xl hover:bg-gray-200 transition font-bold">Maaf</button>
                    </div>
                    <div id="quiz-feedback" class="mt-6 text-lg font-bold"></div>
                </div>
            `;
        }

        function checkAnswer(btn, correct) {
            const feedback = document.getElementById('quiz-feedback');
            if (correct) {
                btn.classList.remove('bg-gray-100');
                btn.classList.add('bg-green-500', 'text-white');
                feedback.textContent = 'Benar! 🎉';
                feedback.className = 'mt-6 text-lg font-bold text-green-600';
            } else {
                btn.classList.remove('bg-gray-100');
                btn.classList.add('bg-red-500', 'text-white');
                feedback.textContent = 'Coba lagi! 💪';
                feedback.className = 'mt-6 text-lg font-bold text-red-600';
            }
        }

        function startListening() {
            const area = document.getElementById('practice-area');
            area.classList.remove('hidden');
            area.innerHTML = `
                <div class="text-center">
                    <h3 class="text-2xl font-bold mb-6">Listening Practice</h3>
                    <div class="bg-purple-100 rounded-2xl p-8 mb-6 inline-block">
                        <button onclick="playAudio()" class="w-20 h-20 bg-purple-600 rounded-full flex items-center justify-center text-white text-3xl hover:bg-purple-700 transition shadow-lg">
                            ▶
                        </button>
                    </div>
                    <p class="text-gray-600 mb-4">Dengarkan dan tulis apa yang Anda dengar:</p>
                    <input type="text" placeholder="Ketik dalam romaji atau hiragana..." 
                           class="w-full max-w-md px-4 py-3 border-2 border-purple-200 rounded-xl text-center text-xl focus:outline-none focus:border-purple-500">
                    <button onclick="checkListening()" class="mt-4 px-6 py-3 bg-purple-600 text-white rounded-xl font-bold hover:bg-purple-700 transition">
                        Periksa Jawaban
                    </button>
                </div>
            `;
        }

        function playAudio() {
            // Simulate audio with speech synthesis
            const utterance = new SpeechSynthesisUtterance('Konnichiwa');
            utterance.lang = 'ja-JP';
            speechSynthesis.speak(utterance);
        }

        function checkListening() {
            alert('Bagus! Jawaban yang benar adalah "Konnichiwa" (こんにちは)');
        }

        function startWriting() {
            const area = document.getElementById('practice-area');
            area.classList.remove('hidden');
            area.innerHTML = `
                <div class="text-center">
                    <h3 class="text-2xl font-bold mb-6">Latihan Menulis</h3>
                    <div class="mb-4">
                        <span class="text-4xl jp-font font-bold text-gray-800">あ</span>
                        <p class="text-gray-600 mt-2">Garis: 3 | Urutan: Top → Bottom</p>
                    </div>
                    <canvas id="writing-canvas" width="300" height="300" class="border-2 border-gray-300 rounded-xl mx-auto bg-white cursor-crosshair"></canvas>
                    <div class="mt-4 space-x-4">
                        <button onclick="clearCanvas()" class="px-4 py-2 bg-gray-200 rounded-lg hover:bg-gray-300 transition">Hapus</button>
                        <button onclick="checkWriting()" class="px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition">Periksa</button>
                    </div>
                </div>
            `;
            
            // Initialize canvas drawing
            setTimeout(initCanvas, 100);
        }

        let canvas, ctx, isDrawing = false;

        function initCanvas() {
            canvas = document.getElementById('writing-canvas');
            ctx = canvas.getContext('2d');
            ctx.lineWidth = 8;
            ctx.lineCap = 'round';
            ctx.strokeStyle = '#000';
            
            canvas.addEventListener('mousedown', startDrawing);
            canvas.addEventListener('mousemove', draw);
            canvas.addEventListener('mouseup', stopDrawing);
            canvas.addEventListener('mouseout', stopDrawing);
            
            // Touch support
            canvas.addEventListener('touchstart', (e) => {
                e.preventDefault();
                startDrawing(e.touches[0]);
            });
            canvas.addEventListener('touchmove', (e) => {
                e.preventDefault();
                draw(e.touches[0]);
            });
            canvas.addEventListener('touchend', stopDrawing);
        }

        function startDrawing(e) {
            isDrawing = true;
            const rect = canvas.getBoundingClientRect();
            ctx.beginPath();
            ctx.moveTo(e.clientX - rect.left, e.clientY - rect.top);
        }

        function draw(e) {
            if (!isDrawing) return;
            const rect = canvas.getBoundingClientRect();
            ctx.lineTo(e.clientX - rect.left, e.clientY - rect.top);
            ctx.stroke();
        }

        function stopDrawing() {
            isDrawing = false;
        }

        function clearCanvas() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        }

        function checkWriting() {
            alert('Bagus! Anda telah menulis あ (a) dengan benar! ✨');
        }

        function startLesson(level) {
            alert(`Memulai pelajaran ${level.toUpperCase()}! Fitur lengkap akan tersedia segera.`);
        }

        // Initialize
        showSection('dashboard');
    </script>
</body>
</html>
