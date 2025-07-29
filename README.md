#<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Absensi Digital Sekolah</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .slide-in {
            animation: slideIn 0.3s ease-out;
        }
        @keyframes slideIn {
            from { transform: translateX(-100%); }
            to { transform: translateX(0); }
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }
        .modal.show {
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1001;
            padding: 12px 24px;
            border-radius: 8px;
            color: white;
            font-weight: 500;
            transform: translateX(400px);
            transition: transform 0.3s ease;
        }
        .notification.show {
            transform: translateX(0);
        }
        .notification.success { background-color: #10b981; }
        .notification.error { background-color: #ef4444; }
        .notification.info { background-color: #3b82f6; }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
    <!-- Notification Container -->
    <div id="notification" class="notification"></div>

    <!-- Landing Page -->
    <div id="landingPage" class="min-h-screen flex flex-col">
        <!-- Header -->
        <header class="bg-white shadow-lg">
            <div class="container mx-auto px-4 py-4 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div id="schoolLogo" class="w-12 h-12 bg-blue-600 rounded-full flex items-center justify-center text-white font-bold text-xl">
                        🏫
                    </div>
                    <div>
                        <h1 class="text-2xl font-bold text-gray-800">SMA Digital</h1>
                        <p class="text-sm text-gray-600">Sistem Absensi Digital</p>
                    </div>
                </div>
                <button onclick="showAdminPanel()" class="text-sm text-blue-600 hover:text-blue-800">Admin Panel</button>
            </div>
        </header>

        <!-- Hero Section -->
        <main class="flex-1 flex items-center justify-center px-4">
            <div class="text-center max-w-4xl mx-auto">
                <h2 class="text-4xl md:text-6xl font-bold text-gray-800 mb-6 fade-in">
                    Sistem Absensi Digital Terpadu
                </h2>
                <p class="text-xl text-gray-600 mb-12 fade-in">
                    Kelola absensi siswa dengan teknologi Face ID dan GPS tracking
                </p>
                
                <!-- Login Buttons -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <button onclick="showLogin('siswa')" class="bg-blue-600 hover:bg-blue-700 text-white py-4 px-6 rounded-xl shadow-lg transform hover:scale-105 transition-all duration-200">
                        <div class="text-3xl mb-2">👨‍🎓</div>
                        <div class="font-semibold">Login Siswa</div>
                    </button>
                    <button onclick="showLogin('guru')" class="bg-green-600 hover:bg-green-700 text-white py-4 px-6 rounded-xl shadow-lg transform hover:scale-105 transition-all duration-200">
                        <div class="text-3xl mb-2">👨‍🏫</div>
                        <div class="font-semibold">Login Guru</div>
                    </button>
                    <button onclick="showLogin('tatatertib')" class="bg-orange-600 hover:bg-orange-700 text-white py-4 px-6 rounded-xl shadow-lg transform hover:scale-105 transition-all duration-200">
                        <div class="text-3xl mb-2">👮‍♂️</div>
                        <div class="font-semibold">Tata Tertib</div>
                    </button>
                    <button onclick="showLogin('orangtua')" class="bg-purple-600 hover:bg-purple-700 text-white py-4 px-6 rounded-xl shadow-lg transform hover:scale-105 transition-all duration-200">
                        <div class="text-3xl mb-2">👨‍👩‍👧‍👦</div>
                        <div class="font-semibold">Orang Tua</div>
                    </button>
                </div>

                <div class="text-center">
                    <p class="text-gray-600 mb-4">Belum punya akun?</p>
                    <button onclick="showRegister()" class="bg-indigo-600 hover:bg-indigo-700 text-white py-3 px-8 rounded-lg font-semibold shadow-lg">
                        Daftar Sekarang
                    </button>
                </div>
            </div>
        </main>
    </div>

    <!-- Login Modal -->
    <div id="loginModal" class="modal">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md mx-4">
            <div class="text-center mb-6">
                <h3 id="loginTitle" class="text-2xl font-bold text-gray-800 mb-2">Login</h3>
                <div id="loginIcon" class="text-4xl mb-4">👤</div>
            </div>
            <form id="loginForm" onsubmit="handleLogin(event)">
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Username/Email</label>
                    <input type="text" id="loginUsername" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                </div>
                <div class="mb-6">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Password</label>
                    <input type="password" id="loginPassword" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                </div>
                <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg transition-colors">
                    Masuk
                </button>
            </form>
            <button onclick="closeModal('loginModal')" class="w-full mt-4 text-gray-600 hover:text-gray-800">Batal</button>
        </div>
    </div>

    <!-- Register Modal -->
    <div id="registerModal" class="modal">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md mx-4 max-h-[90vh] overflow-y-auto">
            <div class="text-center mb-6">
                <h3 class="text-2xl font-bold text-gray-800 mb-2">Daftar Akun Baru</h3>
                <div class="text-4xl mb-4">📝</div>
            </div>
            <form id="registerForm" onsubmit="handleRegister(event)">
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Tipe Akun</label>
                    <select id="registerType" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                        <option value="">Pilih Tipe Akun</option>
                        <option value="siswa">Siswa</option>
                        <option value="orangtua">Orang Tua</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Nama Lengkap</label>
                    <input type="text" id="registerName" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Email</label>
                    <input type="email" id="registerEmail" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Password</label>
                    <input type="password" id="registerPassword" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                </div>
                <div id="siswaFields" class="hidden">
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-bold mb-2">NIS</label>
                        <input type="text" id="registerNIS" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500">
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-bold mb-2">Kelas</label>
                        <select id="registerClass" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500">
                            <option value="">Pilih Kelas</option>
                            <option value="X-1">X-1</option>
                            <option value="X-2">X-2</option>
                            <option value="XI-IPA-1">XI-IPA-1</option>
                            <option value="XI-IPA-2">XI-IPA-2</option>
                            <option value="XII-IPA-1">XII-IPA-1</option>
                            <option value="XII-IPA-2">XII-IPA-2</option>
                        </select>
                    </div>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">No. Telepon</label>
                    <input type="tel" id="registerPhone" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500" required>
                </div>
                <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-4 rounded-lg transition-colors">
                    Daftar
                </button>
            </form>
            <button onclick="closeModal('registerModal')" class="w-full mt-4 text-gray-600 hover:text-gray-800">Batal</button>
        </div>
    </div>

    <!-- Admin Panel Modal -->
    <div id="adminModal" class="modal">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-6xl mx-4 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold text-gray-800">Admin Panel</h3>
                <button onclick="closeModal('adminModal')" class="text-gray-500 hover:text-gray-700 text-2xl">&times;</button>
            </div>
            
            <!-- Admin Tabs -->
            <div class="border-b border-gray-200 mb-6">
                <nav class="flex space-x-8">
                    <button onclick="showAdminTab('logo')" class="admin-tab-btn py-2 px-1 border-b-2 border-blue-500 text-blue-600 font-medium">Logo Sekolah</button>
                    <button onclick="showAdminTab('users')" class="admin-tab-btn py-2 px-1 border-b-2 border-transparent text-gray-500 hover:text-gray-700">Kelola User</button>
                    <button onclick="showAdminTab('attendance')" class="admin-tab-btn py-2 px-1 border-b-2 border-transparent text-gray-500 hover:text-gray-700">Data Absensi</button>
                    <button onclick="showAdminTab('monitoring')" class="admin-tab-btn py-2 px-1 border-b-2 border-transparent text-gray-500 hover:text-gray-700">Monitoring Lokasi</button>
                </nav>
            </div>

            <!-- Logo Tab -->
            <div id="logoTab" class="admin-tab-content">
                <div class="bg-gray-50 p-6 rounded-lg">
                    <h4 class="text-lg font-semibold mb-4">Upload Logo Sekolah</h4>
                    <div class="flex items-center space-x-6">
                        <div class="current-logo">
                            <p class="text-sm text-gray-600 mb-2">Logo Saat Ini:</p>
                            <div id="currentLogo" class="w-20 h-20 bg-blue-600 rounded-full flex items-center justify-center text-white font-bold text-2xl">
                                🏫
                            </div>
                        </div>
                        <div class="flex-1">
                            <input type="file" id="logoUpload" accept="image/*" class="mb-4" onchange="previewLogo(event)">
                            <button onclick="updateLogo()" class="bg-blue-600 hover:bg-blue-700 text-white px-6 py-2 rounded-lg">
                                Update Logo
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Users Tab -->
            <div id="usersTab" class="admin-tab-content hidden">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <!-- Add User Form -->
                    <div class="bg-gray-50 p-6 rounded-lg">
                        <h4 class="text-lg font-semibold mb-4">Tambah User Baru</h4>
                        <form onsubmit="addUser(event)">
                            <div class="mb-4">
                                <label class="block text-gray-700 text-sm font-bold mb-2">Tipe User</label>
                                <select id="newUserType" class="w-full px-3 py-2 border border-gray-300 rounded-lg" required>
                                    <option value="">Pilih Tipe</option>
                                    <option value="siswa">Siswa</option>
                                    <option value="guru">Guru</option>
                                    <option value="tatatertib">Tata Tertib</option>
                                    <option value="bk">Guru BK</option>
                                </select>
                            </div>
                            <div class="mb-4">
                                <label class="block text-gray-700 text-sm font-bold mb-2">Nama</label>
                                <input type="text" id="newUserName" class="w-full px-3 py-2 border border-gray-300 rounded-lg" required>
                            </div>
                            <div class="mb-4">
                                <label class="block text-gray-700 text-sm font-bold mb-2">Email</label>
                                <input type="email" id="newUserEmail" class="w-full px-3 py-2 border border-gray-300 rounded-lg" required>
                            </div>
                            <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white py-2 px-4 rounded-lg">
                                Tambah User
                            </button>
                        </form>
                    </div>

                    <!-- User List -->
                    <div class="bg-gray-50 p-6 rounded-lg">
                        <h4 class="text-lg font-semibold mb-4">Daftar User</h4>
                        <div id="userList" class="space-y-2 max-h-64 overflow-y-auto">
                            <!-- User list will be populated here -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Attendance Tab -->
            <div id="attendanceTab" class="admin-tab-content hidden">
                <div class="bg-gray-50 p-6 rounded-lg">
                    <div class="flex justify-between items-center mb-4">
                        <h4 class="text-lg font-semibold">Data Absensi</h4>
                        <button onclick="downloadAttendance()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg flex items-center space-x-2">
                            <span>📊</span>
                            <span>Download Excel</span>
                        </button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full border-collapse border border-gray-300">
                            <thead>
                                <tr class="bg-gray-200">
                                    <th class="border border-gray-300 px-4 py-2">Nama</th>
                                    <th class="border border-gray-300 px-4 py-2">Kelas</th>
                                    <th class="border border-gray-300 px-4 py-2">Tanggal</th>
                                    <th class="border border-gray-300 px-4 py-2">Masuk</th>
                                    <th class="border border-gray-300 px-4 py-2">Pulang</th>
                                    <th class="border border-gray-300 px-4 py-2">Status</th>
                                </tr>
                            </thead>
                            <tbody id="attendanceTable">
                                <!-- Attendance data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Monitoring Tab -->
            <div id="monitoringTab" class="admin-tab-content hidden">
                <div class="bg-gray-50 p-6 rounded-lg">
                    <h4 class="text-lg font-semibold mb-4">Monitoring Lokasi Siswa</h4>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="locationMonitoring">
                        <!-- Location monitoring cards will be populated here -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Student Dashboard -->
    <div id="studentDashboard" class="hidden min-h-screen bg-gray-50">
        <header class="bg-white shadow-sm">
            <div class="container mx-auto px-4 py-4 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center text-white font-bold">
                        👨‍🎓
                    </div>
                    <div>
                        <h2 class="font-semibold text-gray-800" id="studentName">Dashboard Siswa</h2>
                        <p class="text-sm text-gray-600" id="studentClass">Kelas</p>
                    </div>
                </div>
                <button onclick="logout()" class="text-red-600 hover:text-red-800">Logout</button>
            </div>
        </header>

        <main class="container mx-auto px-4 py-8">
            <!-- Quick Actions -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                <button onclick="showFaceIdAbsen()" class="bg-green-600 hover:bg-green-700 text-white p-6 rounded-xl shadow-lg text-center">
                    <div class="text-3xl mb-2">📱</div>
                    <div class="font-semibold">Absen Masuk</div>
                </button>
                <button onclick="showFaceIdAbsen('pulang')" class="bg-blue-600 hover:bg-blue-700 text-white p-6 rounded-xl shadow-lg text-center">
                    <div class="text-3xl mb-2">🏠</div>
                    <div class="font-semibold">Absen Pulang</div>
                </button>
                <button onclick="showIzinForm()" class="bg-orange-600 hover:bg-orange-700 text-white p-6 rounded-xl shadow-lg text-center">
                    <div class="text-3xl mb-2">📝</div>
                    <div class="font-semibold">Ajukan Izin</div>
                </button>
                <button onclick="showConsultation()" class="bg-purple-600 hover:bg-purple-700 text-white p-6 rounded-xl shadow-lg text-center">
                    <div class="text-3xl mb-2">💬</div>
                    <div class="font-semibold">Konsultasi BK</div>
                </button>
            </div>

            <!-- Attendance Status -->
            <div class="bg-white rounded-xl shadow-lg p-6 mb-8">
                <h3 class="text-xl font-semibold mb-4">Status Absensi Hari Ini</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="text-center">
                        <div class="text-2xl mb-2" id="masukStatus">⏰</div>
                        <div class="font-semibold">Masuk</div>
                        <div class="text-gray-600" id="masukTime">Belum absen</div>
                    </div>
                    <div class="text-center">
                        <div class="text-2xl mb-2" id="pulangStatus">⏰</div>
                        <div class="font-semibold">Pulang</div>
                        <div class="text-gray-600" id="pulangTime">Belum absen</div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Face ID Absen Modal -->
    <div id="faceIdModal" class="modal">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md mx-4">
            <div class="text-center">
                <h3 class="text-2xl font-bold text-gray-800 mb-4" id="faceIdTitle">Absen Masuk</h3>
                <div class="w-48 h-48 mx-auto bg-gray-200 rounded-full flex items-center justify-center mb-6">
                    <div class="text-6xl" id="faceIdIcon">📷</div>
                </div>
                <p class="text-gray-600 mb-6">Posisikan wajah Anda di dalam frame</p>
                <div class="space-y-4">
                    <button onclick="simulateFaceId()" class="w-full bg-blue-600 hover:bg-blue-700 text-white py-3 px-4 rounded-lg">
                        Mulai Scan Wajah
                    </button>
                    <button onclick="closeModal('faceIdModal')" class="w-full text-gray-600 hover:text-gray-800">
                        Batal
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Izin Form Modal -->
    <div id="izinModal" class="modal">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md mx-4 max-h-[90vh] overflow-y-auto">
            <h3 class="text-2xl font-bold text-gray-800 mb-6 text-center">Ajukan Izin</h3>
            <form onsubmit="submitIzin(event)">
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Jenis Izin</label>
                    <select id="izinType" class="w-full px-4 py-3 border border-gray-300 rounded-lg" required>
                        <option value="">Pilih Jenis Izin</option>
                        <option value="sakit">Sakit</option>
                        <option value="izin">Izin</option>
                        <option value="terlambat">Terlambat</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Tanggal</label>
                    <input type="date" id="izinDate" class="w-full px-4 py-3 border border-gray-300 rounded-lg" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Alasan</label>
                    <textarea id="izinReason" class="w-full px-4 py-3 border border-gray-300 rounded-lg h-24" required></textarea>
                </div>
                <div class="mb-6">
                    <label class="block text-gray-700 text-sm font-bold mb-2">Upload Surat Dokter/Orang Tua</label>
                    <input type="file" id="izinFile" accept="image/*,.pdf" class="w-full px-4 py-3 border border-gray-300 rounded-lg">
                </div>
                <button type="submit" class="w-full bg-orange-600 hover:bg-orange-700 text-white py-3 px-4 rounded-lg">
                    Kirim Izin
                </button>
            </form>
            <button onclick="closeModal('izinModal')" class="w-full mt-4 text-gray-600 hover:text-gray-800">Batal</button>
        </div>
    </div>

    <!-- Consultation Modal -->
    <div id="consultationModal" class="modal">
        <div class="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-2xl mx-4 max-h-[90vh] overflow-y-auto">
            <h3 class="text-2xl font-bold text-gray-800 mb-6 text-center">Konsultasi dengan Guru BK</h3>
            <div class="bg-gray-50 rounded-lg p-4 mb-6 h-64 overflow-y-auto" id="chatMessages">
                <div class="text-center text-gray-500 py-8">
                    Mulai percakapan dengan guru BK. Semua pesan bersifat rahasia.
                </div>
            </div>
            <div class="flex space-x-2">
                <input type="text" id="chatInput" placeholder="Ketik pesan Anda..." class="flex-1 px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500">
                <button onclick="sendMessage()" class="bg-purple-600 hover:bg-purple-700 text-white px-6 py-3 rounded-lg">
                    Kirim
                </button>
            </div>
            <button onclick="closeModal('consultationModal')" class="w-full mt-4 text-gray-600 hover:text-gray-800">Tutup</button>
        </div>
    </div>

    <!-- Tata Tertib Dashboard -->
    <div id="tatatertibDashboard" class="hidden min-h-screen bg-gray-50">
        <header class="bg-white shadow-sm">
            <div class="container mx-auto px-4 py-4 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-orange-600 rounded-full flex items-center justify-center text-white font-bold">
                        👮‍♂️
                    </div>
                    <h2 class="font-semibold text-gray-800">Dashboard Tata Tertib</h2>
                </div>
                <button onclick="logout()" class="text-red-600 hover:text-red-800">Logout</button>
            </div>
        </header>

        <main class="container mx-auto px-4 py-8">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <!-- Late Students -->
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-xl font-semibold mb-4">Siswa Terlambat Hari Ini</h3>
                    <div id="lateStudentsList" class="space-y-3">
                        <!-- Late students will be populated here -->
                    </div>
                </div>

                <!-- Quick Actions -->
                <div class="bg-white rounded-xl shadow-lg p-6">
                    <h3 class="text-xl font-semibold mb-4">Aksi Cepat</h3>
                    <div class="space-y-3">
                        <button onclick="markStudentLate()" class="w-full bg-red-600 hover:bg-red-700 text-white py-3 px-4 rounded-lg text-left">
                            ⏰ Tandai Siswa Terlambat
                        </button>
                        <button onclick="viewAllAttendance()" class="w-full bg-blue-600 hover:bg-blue-700 text-white py-3 px-4 rounded-lg text-left">
                            📊 Lihat Semua Absensi
                        </button>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Parent Dashboard -->
    <div id="parentDashboard" class="hidden min-h-screen bg-gray-50">
        <header class="bg-white shadow-sm">
            <div class="container mx-auto px-4 py-4 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-purple-600 rounded-full flex items-center justify-center text-white font-bold">
                        👨‍👩‍👧‍👦
                    </div>
                    <h2 class="font-semibold text-gray-800">Dashboard Orang Tua</h2>
                </div>
                <button onclick="logout()" class="text-red-600 hover:text-red-800">Logout</button>
            </div>
        </header>

        <main class="container mx-auto px-4 py-8">
            <div class="bg-white rounded-xl shadow-lg p-6 mb-8">
                <h3 class="text-xl font-semibold mb-4">Status Anak Hari Ini</h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="text-center p-4 bg-green-50 rounded-lg">
                        <div class="text-2xl mb-2">✅</div>
                        <div class="font-semibold">Status Kehadiran</div>
                        <div class="text-green-600" id="childAttendanceStatus">Hadir</div>
                    </div>
                    <div class="text-center p-4 bg-blue-50 rounded-lg">
                        <div class="text-2xl mb-2">📍</div>
                        <div class="font-semibold">Lokasi Terakhir</div>
                        <div class="text-blue-600" id="childLocation">Di Sekolah</div>
                    </div>
                    <div class="text-center p-4 bg-orange-50 rounded-lg">
                        <div class="text-2xl mb-2">⏰</div>
                        <div class="font-semibold">Waktu Masuk</div>
                        <div class="text-orange-600" id="childEntryTime">07:15</div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script>
        // Global variables
        let currentUser = null;
        let currentUserType = null;
        let attendanceData = [];
        let userData = {
            admin: { username: 'admin', password: 'admin' },
            siswa: [],
            guru: [],
            tatatertib: [],
            bk: [],
            orangtua: []
        };

        // Initialize sample data
        function initializeData() {
            // Sample students
            userData.siswa = [
                { id: 1, name: 'Ahmad Rizki', email: 'ahmad@student.com', password: '123456', nis: '2024001', class: 'X-1', phone: '081234567890' },
                { id: 2, name: 'Siti Nurhaliza', email: 'siti@student.com', password: '123456', nis: '2024002', class: 'X-2', phone: '081234567891' }
            ];

            // Sample teachers
            userData.guru = [
                { id: 1, name: 'Pak Budi', email: 'budi@teacher.com', password: '123456' },
                { id: 2, name: 'Bu Ani', email: 'ani@teacher.com', password: '123456' }
            ];

            // Sample tata tertib
            userData.tatatertib = [
                { id: 1, name: 'Pak Joko', email: 'joko@tatatertib.com', password: '123456' }
            ];

            // Sample BK
            userData.bk = [
                { id: 1, name: 'Bu Sari', email: 'sari@bk.com', password: '123456' }
            ];

            // Sample parents
            userData.orangtua = [
                { id: 1, name: 'Bapak Ahmad', email: 'bapak.ahmad@parent.com', password: '123456', childId: 1 }
            ];

            // Sample attendance data
            attendanceData = [
                { studentId: 1, name: 'Ahmad Rizki', class: 'X-1', date: '2024-01-15', masuk: '07:15', pulang: '15:30', status: 'Hadir' },
                { studentId: 2, name: 'Siti Nurhaliza', class: 'X-2', date: '2024-01-15', masuk: '07:45', pulang: '', status: 'Terlambat' }
            ];

            updateUserList();
            updateAttendanceTable();
        }

        // Show notification
        function showNotification(message, type = 'info') {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type} show`;
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Modal functions
        function showLogin(type) {
            const modal = document.getElementById('loginModal');
            const title = document.getElementById('loginTitle');
            const icon = document.getElementById('loginIcon');
            
            const icons = {
                siswa: '👨‍🎓',
                guru: '👨‍🏫',
                tatatertib: '👮‍♂️',
                orangtua: '👨‍👩‍👧‍👦'
            };
            
            const titles = {
                siswa: 'Login Siswa',
                guru: 'Login Guru',
                tatatertib: 'Login Tata Tertib',
                orangtua: 'Login Orang Tua'
            };
            
            title.textContent = titles[type];
            icon.textContent = icons[type];
            modal.dataset.userType = type;
            modal.classList.add('show');
        }

        function showRegister() {
            document.getElementById('registerModal').classList.add('show');
        }

        function showAdminPanel() {
            const username = prompt('Username:');
            const password = prompt('Password:');
            
            if (username === 'admin' && password === 'admin') {
                document.getElementById('adminModal').classList.add('show');
                showAdminTab('logo');
            } else {
                showNotification('Username atau password salah!', 'error');
            }
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('show');
        }

        // Register form handling
        document.getElementById('registerType').addEventListener('change', function() {
            const siswaFields = document.getElementById('siswaFields');
            if (this.value === 'siswa') {
                siswaFields.classList.remove('hidden');
            } else {
                siswaFields.classList.add('hidden');
            }
        });

        // Handle registration
        function handleRegister(event) {
            event.preventDefault();
            
            const type = document.getElementById('registerType').value;
            const name = document.getElementById('registerName').value;
            const email = document.getElementById('registerEmail').value;
            const password = document.getElementById('registerPassword').value;
            const phone = document.getElementById('registerPhone').value;
            
            const newUser = {
                id: Date.now(),
                name,
                email,
                password,
                phone
            };
            
            if (type === 'siswa') {
                newUser.nis = document.getElementById('registerNIS').value;
                newUser.class = document.getElementById('registerClass').value;
            }
            
            userData[type].push(newUser);
            
            showNotification('Registrasi berhasil! Silakan login.', 'success');
            closeModal('registerModal');
            document.getElementById('registerForm').reset();
        }

        // Handle login
        function handleLogin(event) {
            event.preventDefault();
            
            const username = document.getElementById('loginUsername').value;
            const password = document.getElementById('loginPassword').value;
            const userType = document.getElementById('loginModal').dataset.userType;
            
            // Check admin login
            if (userType === 'admin' && username === 'admin' && password === 'admin') {
                showNotification('Login admin berhasil!', 'success');
                closeModal('loginModal');
                return;
            }
            
            // Check user login
            const users = userData[userType] || [];
            const user = users.find(u => (u.email === username || u.username === username) && u.password === password);
            
            if (user) {
                currentUser = user;
                currentUserType = userType;
                showNotification(`Login berhasil! Selamat datang ${user.name}`, 'success');
                closeModal('loginModal');
                showDashboard(userType);
            } else {
                showNotification('Username atau password salah!', 'error');
            }
        }

        // Show dashboard based on user type
        function showDashboard(userType) {
            document.getElementById('landingPage').classList.add('hidden');
            
            switch(userType) {
                case 'siswa':
                    document.getElementById('studentDashboard').classList.remove('hidden');
                    document.getElementById('studentName').textContent = currentUser.name;
                    document.getElementById('studentClass').textContent = currentUser.class;
                    break;
                case 'tatatertib':
                    document.getElementById('tatatertibDashboard').classList.remove('hidden');
                    updateLateStudentsList();
                    break;
                case 'orangtua':
                    document.getElementById('parentDashboard').classList.remove('hidden');
                    updateParentDashboard();
                    break;
            }
        }

        // Logout function
        function logout() {
            currentUser = null;
            currentUserType = null;
            
            // Hide all dashboards
            document.getElementById('studentDashboard').classList.add('hidden');
            document.getElementById('tatatertibDashboard').classList.add('hidden');
            document.getElementById('parentDashboard').classList.add('hidden');
            
            // Show landing page
            document.getElementById('landingPage').classList.remove('hidden');
            
            showNotification('Logout berhasil!', 'success');
        }

        // Face ID simulation
        function showFaceIdAbsen(type = 'masuk') {
            document.getElementById('faceIdTitle').textContent = type === 'masuk' ? 'Absen Masuk' : 'Absen Pulang';
            document.getElementById('faceIdModal').classList.add('show');
            document.getElementById('faceIdModal').dataset.type = type;
        }

        function simulateFaceId() {
            const icon = document.getElementById('faceIdIcon');
            const type = document.getElementById('faceIdModal').dataset.type;
            
            // Simulate face scanning
            icon.textContent = '🔍';
            setTimeout(() => {
                icon.textContent = '✅';
                
                // Get current location (simulated)
                if (navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(
                        (position) => {
                            processAttendance(type, position.coords);
                        },
                        () => {
                            // Fallback if location is denied
                            processAttendance(type, { latitude: -6.2088, longitude: 106.8456 });
                        }
                    );
                } else {
                    processAttendance(type, { latitude: -6.2088, longitude: 106.8456 });
                }
            }, 2000);
        }

        function processAttendance(type, coords) {
            const now = new Date();
            const time = now.toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' });
            const date = now.toLocaleDateString('id-ID');
            
            // Check if within school area (simulated)
            const schoolLat = -6.2088;
            const schoolLng = 106.8456;
            const distance = calculateDistance(coords.latitude, coords.longitude, schoolLat, schoolLng);
            
            if (distance > 0.5) { // More than 500m from school
                showNotification('Anda berada di luar area sekolah!', 'error');
                closeModal('faceIdModal');
                return;
            }
            
            // Update attendance status
            const statusElement = document.getElementById(type + 'Status');
            const timeElement = document.getElementById(type + 'Time');
            
            statusElement.textContent = '✅';
            timeElement.textContent = time;
            
            // Add to attendance data
            const existingRecord = attendanceData.find(a => a.studentId === currentUser.id && a.date === date);
            if (existingRecord) {
                existingRecord[type] = time;
            } else {
                attendanceData.push({
                    studentId: currentUser.id,
                    name: currentUser.name,
                    class: currentUser.class,
                    date: date,
                    masuk: type === 'masuk' ? time : '',
                    pulang: type === 'pulang' ? time : '',
                    status: 'Hadir'
                });
            }
            
            showNotification(`Absen ${type} berhasil pada ${time}`, 'success');
            closeModal('faceIdModal');
            updateAttendanceTable();
        }

        // Calculate distance between two coordinates
        function calculateDistance(lat1, lon1, lat2, lon2) {
            const R = 6371; // Earth's radius in km
            const dLat = (lat2 - lat1) * Math.PI / 180;
            const dLon = (lon2 - lon1) * Math.PI / 180;
            const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
                    Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
                    Math.sin(dLon/2) * Math.sin(dLon/2);
            const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
            return R * c;
        }

        // Show izin form
        function showIzinForm() {
            document.getElementById('izinModal').classList.add('show');
        }

        // Submit izin
        function submitIzin(event) {
            event.preventDefault();
            
            const type = document.getElementById('izinType').value;
            const date = document.getElementById('izinDate').value;
            const reason = document.getElementById('izinReason').value;
            const file = document.getElementById('izinFile').files[0];
            
            // Simulate izin submission
            showNotification('Permohonan izin berhasil dikirim!', 'success');
            closeModal('izinModal');
            document.getElementById('izinModal').querySelector('form').reset();
        }

        // Show consultation
        function showConsultation() {
            document.getElementById('consultationModal').classList.add('show');
        }

        // Send message to BK
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const messages = document.getElementById('chatMessages');
            
            if (input.value.trim()) {
                // Add user message
                const userMsg = document.createElement('div');
                userMsg.className = 'bg-blue-100 p-3 rounded-lg mb-2 ml-8';
                userMsg.innerHTML = `<div class="font-semibold text-sm text-blue-800">Anda</div><div>${input.value}</div>`;
                messages.appendChild(userMsg);
                
                // Simulate BK response
                setTimeout(() => {
                    const bkMsg = document.createElement('div');
                    bkMsg.className = 'bg-gray-100 p-3 rounded-lg mb-2 mr-8';
                    bkMsg.innerHTML = `<div class="font-semibold text-sm text-gray-800">Guru BK</div><div>Terima kasih atas pesannya. Saya akan membantu Anda dengan masalah ini.</div>`;
                    messages.appendChild(bkMsg);
                    messages.scrollTop = messages.scrollHeight;
                }, 1000);
                
                input.value = '';
                messages.scrollTop = messages.scrollHeight;
            }
        }

        // Admin panel functions
        function showAdminTab(tabName) {
            // Hide all tabs
            document.querySelectorAll('.admin-tab-content').forEach(tab => {
                tab.classList.add('hidden');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.admin-tab-btn').forEach(btn => {
                btn.classList.remove('border-blue-500', 'text-blue-600');
                btn.classList.add('border-transparent', 'text-gray-500');
            });
            
            // Show selected tab
            document.getElementById(tabName + 'Tab').classList.remove('hidden');
            
            // Add active class to selected button
            event.target.classList.remove('border-transparent', 'text-gray-500');
            event.target.classList.add('border-blue-500', 'text-blue-600');
        }

        // Logo management
        function previewLogo(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    document.getElementById('currentLogo').innerHTML = `<img src="${e.target.result}" class="w-full h-full object-cover rounded-full">`;
                };
                reader.readAsDataURL(file);
            }
        }

        function updateLogo() {
            const logoUpload = document.getElementById('logoUpload');
            if (logoUpload.files[0]) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    // Update logo in header
                    document.getElementById('schoolLogo').innerHTML = `<img src="${e.target.result}" class="w-full h-full object-cover rounded-full">`;
                    showNotification('Logo berhasil diupdate!', 'success');
                };
                reader.readAsDataURL(logoUpload.files[0]);
            }
        }

        // User management
        function addUser(event) {
            event.preventDefault();
            
            const type = document.getElementById('newUserType').value;
            const name = document.getElementById('newUserName').value;
            const email = document.getElementById('newUserEmail').value;
            
            const newUser = {
                id: Date.now(),
                name,
                email,
                password: '123456' // Default password
            };
            
            userData[type].push(newUser);
            updateUserList();
            showNotification('User berhasil ditambahkan!', 'success');
            
            // Reset form
            event.target.reset();
        }

        function updateUserList() {
            const userList = document.getElementById('userList');
            userList.innerHTML = '';
            
            Object.keys(userData).forEach(type => {
                if (type !== 'admin') {
                    userData[type].forEach(user => {
                        const userItem = document.createElement('div');
                        userItem.className = 'flex justify-between items-center p-3 bg-white rounded-lg border';
                        userItem.innerHTML = `
                            <div>
                                <div class="font-semibold">${user.name}</div>
                                <div class="text-sm text-gray-600">${user.email} (${type})</div>
                            </div>
                            <button onclick="deleteUser('${type}', ${user.id})" class="text-red-600 hover:text-red-800">
                                🗑️
                            </button>
                        `;
                        userList.appendChild(userItem);
                    });
                }
            });
        }

        function deleteUser(type, userId) {
            userData[type] = userData[type].filter(user => user.id !== userId);
            updateUserList();
            showNotification('User berhasil dihapus!', 'success');
        }

        // Attendance management
        function updateAttendanceTable() {
            const table = document.getElementById('attendanceTable');
            table.innerHTML = '';
            
            attendanceData.forEach(record => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="border border-gray-300 px-4 py-2">${record.name}</td>
                    <td class="border border-gray-300 px-4 py-2">${record.class}</td>
                    <td class="border border-gray-300 px-4 py-2">${record.date}</td>
                    <td class="border border-gray-300 px-4 py-2">${record.masuk || '-'}</td>
                    <td class="border border-gray-300 px-4 py-2">${record.pulang || '-'}</td>
                    <td class="border border-gray-300 px-4 py-2">
                        <span class="px-2 py-1 rounded text-sm ${record.status === 'Hadir' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'}">
                            ${record.status}
                        </span>
                    </td>
                `;
                table.appendChild(row);
            });
        }

        function downloadAttendance() {
            // Simulate Excel download
            const csvContent = "data:text/csv;charset=utf-8," 
                + "Nama,Kelas,Tanggal,Masuk,Pulang,Status\n"
                + attendanceData.map(record => 
                    `${record.name},${record.class},${record.date},${record.masuk || '-'},${record.pulang || '-'},${record.status}`
                ).join("\n");
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "data_absensi.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showNotification('File Excel berhasil didownload!', 'success');
        }

        // Location monitoring
        function updateLocationMonitoring() {
            const monitoring = document.getElementById('locationMonitoring');
            monitoring.innerHTML = '';
            
            userData.siswa.forEach(student => {
                const card = document.createElement('div');
                card.className = 'bg-white p-4 rounded-lg border';
                card.innerHTML = `
                    <div class="font-semibold">${student.name}</div>
                    <div class="text-sm text-gray-600">${student.class}</div>
                    <div class="mt-2">
                        <div class="text-sm">📍 Lokasi: Di Sekolah</div>
                        <div class="text-sm">⏰ Update: ${new Date().toLocaleTimeString('id-ID')}</div>
                    </div>
                `;
                monitoring.appendChild(card);
            });
        }

        // Tata tertib functions
        function updateLateStudentsList() {
            const list = document.getElementById('lateStudentsList');
            list.innerHTML = '';
            
            const lateStudents = attendanceData.filter(record => record.status === 'Terlambat');
            
            if (lateStudents.length === 0) {
                list.innerHTML = '<div class="text-gray-500 text-center py-4">Tidak ada siswa terlambat hari ini</div>';
                return;
            }
            
            lateStudents.forEach(student => {
                const item = document.createElement('div');
                item.className = 'flex justify-between items-center p-3 bg-red-50 rounded-lg border border-red-200';
                item.innerHTML = `
                    <div>
                        <div class="font-semibold">${student.name}</div>
                        <div class="text-sm text-gray-600">${student.class} - Masuk: ${student.masuk}</div>
                    </div>
                    <button onclick="markStudentLate(${student.studentId})" class="bg-red-600 hover:bg-red-700 text-white px-3 py-1 rounded text-sm">
                        Tandai Telat
                    </button>
                `;
                list.appendChild(item);
            });
        }

        function markStudentLate(studentId) {
            const studentName = prompt('Masukkan nama siswa yang terlambat:');
            if (studentName) {
                const now = new Date();
                const time = now.toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' });
                const date = now.toLocaleDateString('id-ID');
                
                attendanceData.push({
                    studentId: Date.now(),
                    name: studentName,
                    class: 'X-1', // Default class
                    date: date,
                    masuk: time,
                    pulang: '',
                    status: 'Terlambat'
                });
                
                updateLateStudentsList();
                updateAttendanceTable();
                showNotification(`${studentName} telah ditandai terlambat`, 'info');
            }
        }

        // Parent dashboard functions
        function updateParentDashboard() {
            // Simulate child data
            document.getElementById('childAttendanceStatus').textContent = 'Hadir';
            document.getElementById('childLocation').textContent = 'Di Sekolah';
            document.getElementById('childEntryTime').textContent = '07:15';
        }

        // Chat input enter key handler
        document.getElementById('chatInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });

        // Initialize the application
        document.addEventListener('DOMContentLoaded', function() {
            initializeData();
            updateLocationMonitoring();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'966dc43ef49bdf69',t:'MTc1MzgwNDE3MC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
