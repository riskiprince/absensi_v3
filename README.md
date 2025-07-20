<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Absensi Digital SMK RAUDLATUSSALAM</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #f0f4f8;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .pulse {
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        .slide-in {
            animation: slideIn 0.5s ease-out;
        }
        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        .attendance-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }
        .tab-active {
            border-bottom: 3px solid #4f46e5;
            color: #4f46e5;
            font-weight: 600;
        }
        .print-only {
            display: none;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            .print-only {
                display: block;
            }
            body {
                background-color: white;
            }
            .print-break-inside-avoid {
                break-inside: avoid;
            }
        }
    </style>
</head>
<body>
    <div class="min-h-screen flex flex-col">
        <!-- Header -->
        <header class="bg-gradient-to-r from-indigo-600 to-purple-600 text-white shadow-lg no-print">
            <div class="container mx-auto px-4 py-3 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div id="logoContainer" class="w-12 h-12 bg-white rounded-full flex items-center justify-center overflow-hidden">
                        <img id="schoolLogo" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI4MCIgaGVpZ2h0PSI4MCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiM0MzM4Y2EiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIj48cGF0aCBkPSJNMiAzaDZhNCA0IDAgMCAxIDQgNHY0YTQgNCAwIDAgMS00IDRIMnYtMTJ6Ij48L3BhdGg+PHBhdGggZD0iTTIyIDNoLTZhNCA0IDAgMCAwLTQgNHY0YTQgNCAwIDAgMCA0IDRoNlYzeiI+PC9wYXRoPjxwYXRoIGQ9Ik0yIDE1aDIwdjJhNCA0IDAgMCAxLTQgNEg2YTQgNCAwIDAgMS00LTR2LTJ6Ij48L3BhdGg+PC9zdmc+" alt="Logo Sekolah" class="w-10 h-10">
                    </div>
                    <div>
                        <h1 class="text-xl font-bold">SMK RAUDLATUSSALAM</h1>
                        <p class="text-xs opacity-80">Sistem Absensi Digital</p>
                    </div>
                </div>
                <div class="flex items-center space-x-2">
                    <span id="currentDate" class="text-sm bg-white/20 px-3 py-1 rounded-full"></span>
                    <button id="printButton" class="bg-white text-indigo-700 px-3 py-1 rounded-md text-sm font-medium hover:bg-indigo-100 transition">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 17h2a2 2 0 002-2v-4a2 2 0 00-2-2H5a2 2 0 00-2 2v4a2 2 0 002 2h2m2 4h6a2 2 0 002-2v-4a2 2 0 00-2-2H9a2 2 0 00-2 2v4a2 2 0 002 2z" />
                        </svg>
                        Cetak
                    </button>
                </div>
            </div>
        </header>

        <!-- Navigation -->
        <nav class="bg-white shadow-md no-print">
            <div class="container mx-auto px-4">
                <div class="flex overflow-x-auto">
                    <button class="tab-button px-4 py-3 text-gray-700 hover:text-indigo-600 tab-active" data-tab="dashboard">Dashboard</button>
                    <button class="tab-button px-4 py-3 text-gray-700 hover:text-indigo-600" data-tab="attendance">Absensi Harian</button>
                    <button class="tab-button px-4 py-3 text-gray-700 hover:text-indigo-600" data-tab="students">Data Siswa</button>
                    <button class="tab-button px-4 py-3 text-gray-700 hover:text-indigo-600" data-tab="reports">Laporan Bulanan</button>
                </div>
            </div>
        </nav>

        <!-- Main Content -->
        <main class="flex-grow container mx-auto px-4 py-6">
            <!-- Dashboard Tab -->
            <div id="dashboard" class="tab-content">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                    <div class="bg-white rounded-lg shadow-md p-5">
                        <div class="flex justify-between items-center mb-4">
                            <h3 class="text-lg font-semibold text-gray-700">Kehadiran Hari Ini</h3>
                            <span class="text-xs text-gray-500" id="todayDate"></span>
                        </div>
                        <div class="flex justify-between items-center">
                            <div class="text-center">
                                <div class="text-3xl font-bold text-green-500" id="presentCount">0</div>
                                <div class="text-sm text-gray-500">Hadir</div>
                            </div>
                            <div class="text-center">
                                <div class="text-3xl font-bold text-yellow-500" id="lateCount">0</div>
                                <div class="text-sm text-gray-500">Terlambat</div>
                            </div>
                            <div class="text-center">
                                <div class="text-3xl font-bold text-red-500" id="absentCount">0</div>
                                <div class="text-sm text-gray-500">Tidak Hadir</div>
                            </div>
                            <div class="text-center">
                                <div class="text-3xl font-bold text-blue-500" id="permissionCount">0</div>
                                <div class="text-sm text-gray-500">Izin</div>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-md p-5">
                        <h3 class="text-lg font-semibold text-gray-700 mb-4">Kehadiran Mingguan</h3>
                        <canvas id="weeklyChart" height="150"></canvas>
                    </div>
                    <div class="bg-white rounded-lg shadow-md p-5">
                        <h3 class="text-lg font-semibold text-gray-700 mb-4">Persentase Kehadiran</h3>
                        <canvas id="attendanceDonut" height="150"></canvas>
                    </div>
                </div>

                <div class="bg-white rounded-lg shadow-md p-5 mb-6">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-lg font-semibold text-gray-700">Statistik Bulanan</h3>
                        <div class="flex space-x-2">
                            <select id="monthSelector" class="border rounded-md px-3 py-1 text-sm">
                                <option value="0">Januari</option>
                                <option value="1">Februari</option>
                                <option value="2">Maret</option>
                                <option value="3">April</option>
                                <option value="4">Mei</option>
                                <option value="5">Juni</option>
                                <option value="6">Juli</option>
                                <option value="7">Agustus</option>
                                <option value="8">September</option>
                                <option value="9">Oktober</option>
                                <option value="10">November</option>
                                <option value="11">Desember</option>
                            </select>
                            <select id="yearSelector" class="border rounded-md px-3 py-1 text-sm">
                                <option value="2023">2023</option>
                                <option value="2024" selected>2024</option>
                                <option value="2025">2025</option>
                                <option value="2026">2026</option>
                                <option value="2027">2027</option>
                                <option value="2028">2028</option>
                                <option value="2029">2029</option>
                                <option value="2030">2030</option>
                            </select>
                        </div>
                    </div>
                    <canvas id="monthlyChart" height="100"></canvas>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="bg-white rounded-lg shadow-md p-5">
                        <h3 class="text-lg font-semibold text-gray-700 mb-4">Siswa Terbaik (Kehadiran)</h3>
                        <div id="topStudents" class="space-y-3">
                            <!-- Top students will be populated here -->
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-md p-5">
                        <h3 class="text-lg font-semibold text-gray-700 mb-4">Kehadiran per Kelas</h3>
                        <canvas id="classChart" height="200"></canvas>
                    </div>
                </div>
            </div>

            <!-- Attendance Tab -->
            <div id="attendance" class="tab-content hidden">
                <div class="bg-white rounded-lg shadow-md p-5 mb-6">
                    <div class="flex flex-wrap justify-between items-center mb-6">
                        <h2 class="text-xl font-bold text-gray-800">Absensi Harian</h2>
                        <div class="flex flex-wrap gap-2 mt-2 sm:mt-0">
                            <input type="date" id="attendanceDate" class="border rounded-md px-3 py-2 text-sm">
                            <select id="attendanceClass" class="border rounded-md px-3 py-2 text-sm">
                                <option value="">Pilih Kelas</option>
                                <option value="10-TKJ">10 TKJ</option>
                                <option value="10-AKL">10 AKL</option>
                                <option value="10-TO">10 TO</option>
                                <option value="11-TKJ">11 TKJ</option>
                                <option value="11-AKL">11 AKL</option>
                                <option value="11-TO">11 TO</option>
                                <option value="12-TKJ">12 TKJ</option>
                                <option value="12-AKL">12 AKL</option>
                                <option value="12-TO">12 TO</option>
                            </select>
                            <button id="loadAttendanceBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                                Muat Data
                            </button>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="min-w-full bg-white">
                            <thead>
                                <tr class="bg-gray-100 text-gray-600 uppercase text-sm leading-normal">
                                    <th class="py-3 px-6 text-left">No</th>
                                    <th class="py-3 px-6 text-left">Nama Siswa</th>
                                    <th class="py-3 px-6 text-center">Status</th>
                                    <th class="py-3 px-6 text-center">Waktu</th>
                                    <th class="py-3 px-6 text-center">Keterangan</th>
                                </tr>
                            </thead>
                            <tbody id="attendanceTableBody" class="text-gray-600 text-sm">
                                <!-- Attendance data will be populated here -->
                            </tbody>
                        </table>
                    </div>

                    <div class="mt-6 flex justify-end">
                        <button id="saveAttendanceBtn" class="bg-green-600 text-white px-4 py-2 rounded-md text-sm hover:bg-green-700 transition">
                            Simpan Absensi
                        </button>
                    </div>
                </div>
            </div>

            <!-- Students Tab -->
            <div id="students" class="tab-content hidden">
                <div class="bg-white rounded-lg shadow-md p-5 mb-6">
                    <div class="flex flex-wrap justify-between items-center mb-6">
                        <h2 class="text-xl font-bold text-gray-800">Data Siswa</h2>
                        <button id="addStudentBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                            Tambah Siswa
                        </button>
                    </div>

                    <div class="flex flex-wrap gap-2 mb-4">
                        <select id="filterClass" class="border rounded-md px-3 py-2 text-sm">
                            <option value="">Semua Kelas</option>
                            <option value="10-TKJ">10 TKJ</option>
                            <option value="10-AKL">10 AKL</option>
                            <option value="10-TO">10 TO</option>
                            <option value="11-TKJ">11 TKJ</option>
                            <option value="11-AKL">11 AKL</option>
                            <option value="11-TO">11 TO</option>
                            <option value="12-TKJ">12 TKJ</option>
                            <option value="12-AKL">12 AKL</option>
                            <option value="12-TO">12 TO</option>
                        </select>
                        <input type="text" id="searchStudent" placeholder="Cari siswa..." class="border rounded-md px-3 py-2 text-sm flex-grow">
                    </div>

                    <div class="overflow-x-auto">
                        <table class="min-w-full bg-white">
                            <thead>
                                <tr class="bg-gray-100 text-gray-600 uppercase text-sm leading-normal">
                                    <th class="py-3 px-6 text-left">Foto</th>
                                    <th class="py-3 px-6 text-left">Nama</th>
                                    <th class="py-3 px-6 text-left">Kelas</th>
                                    <th class="py-3 px-6 text-left">Jenis Kelamin</th>
                                    <th class="py-3 px-6 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="studentsTableBody" class="text-gray-600 text-sm">
                                <!-- Student data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Reports Tab -->
            <div id="reports" class="tab-content hidden">
                <div class="bg-white rounded-lg shadow-md p-5 mb-6">
                    <div class="flex flex-wrap justify-between items-center mb-6">
                        <h2 class="text-xl font-bold text-gray-800">Laporan Bulanan</h2>
                        <div class="flex flex-wrap gap-2 mt-2 sm:mt-0">
                            <select id="reportMonth" class="border rounded-md px-3 py-2 text-sm">
                                <option value="0">Januari</option>
                                <option value="1">Februari</option>
                                <option value="2">Maret</option>
                                <option value="3">April</option>
                                <option value="4">Mei</option>
                                <option value="5">Juni</option>
                                <option value="6">Juli</option>
                                <option value="7">Agustus</option>
                                <option value="8">September</option>
                                <option value="9">Oktober</option>
                                <option value="10">November</option>
                                <option value="11">Desember</option>
                            </select>
                            <select id="reportYear" class="border rounded-md px-3 py-2 text-sm">
                                <option value="2023">2023</option>
                                <option value="2024" selected>2024</option>
                                <option value="2025">2025</option>
                                <option value="2026">2026</option>
                                <option value="2027">2027</option>
                                <option value="2028">2028</option>
                                <option value="2029">2029</option>
                                <option value="2030">2030</option>
                            </select>
                            <select id="reportClass" class="border rounded-md px-3 py-2 text-sm">
                                <option value="">Pilih Kelas</option>
                                <option value="10-TKJ">10 TKJ</option>
                                <option value="10-AKL">10 AKL</option>
                                <option value="10-TO">10 TO</option>
                                <option value="11-TKJ">11 TKJ</option>
                                <option value="11-AKL">11 AKL</option>
                                <option value="11-TO">11 TO</option>
                                <option value="12-TKJ">12 TKJ</option>
                                <option value="12-AKL">12 AKL</option>
                                <option value="12-TO">12 TO</option>
                            </select>
                            <button id="generateReportBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                                Tampilkan Laporan
                            </button>
                        </div>
                    </div>

                    <div id="reportContent">
                        <div class="text-center text-gray-500 py-10">
                            Pilih bulan, tahun, dan kelas untuk menampilkan laporan
                        </div>
                    </div>
                </div>
            </div>
        </main>

        <!-- Print Template -->
        <div id="printTemplate" class="hidden print-only">
            <div class="p-8">
                <div class="flex justify-between items-center mb-6">
                    <div class="flex items-center">
                        <img id="printLogo" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI4MCIgaGVpZ2h0PSI4MCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiM0MzM4Y2EiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIj48cGF0aCBkPSJNMiAzaDZhNCA0IDAgMCAxIDQgNHY0YTQgNCAwIDAgMS00IDRIMnYtMTJ6Ij48L3BhdGg+PHBhdGggZD0iTTIyIDNoLTZhNCA0IDAgMCAwLTQgNHY0YTQgNCAwIDAgMCA0IDRoNlYzeiI+PC9wYXRoPjxwYXRoIGQ9Ik0yIDE1aDIwdjJhNCA0IDAgMCAxLTQgNEg2YTQgNCAwIDAgMS00LTR2LTJ6Ij48L3BhdGg+PC9zdmc+" alt="Logo Sekolah" class="w-16 h-16 mr-4">
                        <div>
                            <h1 class="text-2xl font-bold">SMK RAUDLATUSSALAM</h1>
                            <p class="text-sm">Laporan Absensi Siswa</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p id="printDate" class="text-sm"></p>
                        <p id="printClass" class="text-sm font-semibold"></p>
                    </div>
                </div>

                <div id="printReportContent" class="mt-6">
                    <!-- Report content will be populated here -->
                </div>

                <div class="mt-10 text-right">
                    <p class="mb-16">Wali Kelas</p>
                    <p class="font-semibold">Mukhamad Khoirul Riski S.Kom</p>
                </div>
            </div>
        </div>

        <!-- Footer -->
        <footer class="bg-gray-800 text-white py-4 no-print">
            <div class="container mx-auto px-4 text-center">
                <p>© 2024 SMK RAUDLATUSSALAM - Sistem Absensi Digital</p>
                <p class="text-xs mt-1 text-gray-400">Dikembangkan oleh Tim IT SMK RAUDLATUSSALAM</p>
            </div>
        </footer>
    </div>

    <!-- Modals -->
    <!-- Add/Edit Student Modal -->
    <div id="studentModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden no-print">
        <div class="bg-white rounded-lg shadow-xl w-full max-w-md mx-4">
            <div class="border-b px-6 py-4">
                <h3 id="studentModalTitle" class="text-lg font-semibold text-gray-800">Tambah Siswa Baru</h3>
            </div>
            <div class="p-6">
                <form id="studentForm">
                    <input type="hidden" id="studentId">
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-medium mb-2" for="studentName">Nama Lengkap</label>
                        <input type="text" id="studentName" class="w-full border rounded-md px-3 py-2" required>
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-medium mb-2" for="studentClass">Kelas</label>
                        <select id="studentClass" class="w-full border rounded-md px-3 py-2" required>
                            <option value="">Pilih Kelas</option>
                            <option value="10-TKJ">10 TKJ</option>
                            <option value="10-AKL">10 AKL</option>
                            <option value="10-TO">10 TO</option>
                            <option value="11-TKJ">11 TKJ</option>
                            <option value="11-AKL">11 AKL</option>
                            <option value="11-TO">11 TO</option>
                            <option value="12-TKJ">12 TKJ</option>
                            <option value="12-AKL">12 AKL</option>
                            <option value="12-TO">12 TO</option>
                        </select>
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-medium mb-2" for="studentGender">Jenis Kelamin</label>
                        <select id="studentGender" class="w-full border rounded-md px-3 py-2" required>
                            <option value="">Pilih Jenis Kelamin</option>
                            <option value="Laki-laki">Laki-laki</option>
                            <option value="Perempuan">Perempuan</option>
                        </select>
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-medium mb-2" for="studentPhoto">Foto</label>
                        <div class="flex items-center space-x-4">
                            <div id="photoPreview" class="w-16 h-16 bg-gray-200 rounded-full flex items-center justify-center overflow-hidden">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                                </svg>
                            </div>
                            <input type="file" id="studentPhoto" class="hidden" accept="image/*">
                            <button type="button" id="uploadPhotoBtn" class="bg-gray-200 text-gray-700 px-3 py-1 rounded-md text-sm hover:bg-gray-300 transition">
                                Pilih Foto
                            </button>
                        </div>
                    </div>
                </form>
            </div>
            <div class="border-t px-6 py-4 flex justify-end space-x-2">
                <button id="cancelStudentBtn" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-md text-sm hover:bg-gray-300 transition">
                    Batal
                </button>
                <button id="saveStudentBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                    Simpan
                </button>
            </div>
        </div>
    </div>

    <!-- Attendance Status Modal -->
    <div id="attendanceModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden no-print">
        <div class="bg-white rounded-lg shadow-xl w-full max-w-md mx-4">
            <div class="border-b px-6 py-4">
                <h3 id="attendanceModalTitle" class="text-lg font-semibold text-gray-800">Isi Absensi</h3>
            </div>
            <div class="p-6">
                <form id="attendanceForm">
                    <input type="hidden" id="attendanceStudentId">
                    <div class="mb-4">
                        <p id="attendanceStudentName" class="font-medium text-lg"></p>
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-medium mb-2" for="attendanceStatus">Status Kehadiran</label>
                        <select id="attendanceStatus" class="w-full border rounded-md px-3 py-2" required>
                            <option value="hadir">Hadir</option>
                            <option value="terlambat">Terlambat</option>
                            <option value="izin">Izin</option>
                            <option value="sakit">Sakit</option>
                            <option value="alpha">Alpha (Tanpa Keterangan)</option>
                        </select>
                    </div>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-medium mb-2" for="attendanceNote">Keterangan (Opsional)</label>
                        <textarea id="attendanceNote" class="w-full border rounded-md px-3 py-2" rows="2"></textarea>
                    </div>
                </form>
            </div>
            <div class="border-t px-6 py-4 flex justify-end space-x-2">
                <button id="cancelAttendanceBtn" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-md text-sm hover:bg-gray-300 transition">
                    Batal
                </button>
                <button id="saveAttendanceStatusBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                    Simpan
                </button>
            </div>
        </div>
    </div>

    <!-- Logo Upload Modal -->
    <div id="logoModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden no-print">
        <div class="bg-white rounded-lg shadow-xl w-full max-w-md mx-4">
            <div class="border-b px-6 py-4">
                <h3 class="text-lg font-semibold text-gray-800">Ganti Logo Sekolah</h3>
            </div>
            <div class="p-6">
                <div class="mb-4 flex justify-center">
                    <div id="logoPreview" class="w-32 h-32 bg-gray-200 rounded-lg flex items-center justify-center overflow-hidden">
                        <img id="currentLogo" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI4MCIgaGVpZ2h0PSI4MCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiM0MzM4Y2EiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIj48cGF0aCBkPSJNMiAzaDZhNCA0IDAgMCAxIDQgNHY0YTQgNCAwIDAgMS00IDRIMnYtMTJ6Ij48L3BhdGg+PHBhdGggZD0iTTIyIDNoLTZhNCA0IDAgMCAwLTQgNHY0YTQgNCAwIDAgMCA0IDRoNlYzeiI+PC9wYXRoPjxwYXRoIGQ9Ik0yIDE1aDIwdjJhNCA0IDAgMCAxLTQgNEg2YTQgNCAwIDAgMS00LTR2LTJ6Ij48L3BhdGg+PC9zdmc+" alt="Logo Sekolah" class="w-full h-full object-contain">
                    </div>
                </div>
                <div class="flex justify-center">
                    <input type="file" id="logoFile" class="hidden" accept="image/*">
                    <button type="button" id="uploadLogoBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                        Pilih Logo
                    </button>
                </div>
            </div>
            <div class="border-t px-6 py-4 flex justify-end space-x-2">
                <button id="cancelLogoBtn" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-md text-sm hover:bg-gray-300 transition">
                    Batal
                </button>
                <button id="saveLogoBtn" class="bg-indigo-600 text-white px-4 py-2 rounded-md text-sm hover:bg-indigo-700 transition">
                    Simpan
                </button>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-4 right-4 bg-green-500 text-white px-6 py-3 rounded-md shadow-lg transform transition-transform duration-300 translate-y-20 opacity-0 z-50 no-print">
        <span id="toastMessage">Berhasil menyimpan data</span>
    </div>

    <script>
        // Initial data
        const initialStudents = [
            { id: 1, name: "Amelia Eksa Purianti", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 2, name: "Ana Citra Lestari", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 3, name: "Andre Maulana", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 4, name: "Aprilia Sofi Andrea", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 5, name: "Aurellia Shera Shakila", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 6, name: "Dwi Anisa Lailatul Hidayah", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 7, name: "Hasan Triasa Fu'adi", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 8, name: "Kalyca Nafi Prasetya", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 9, name: "M Fikni Mustofa", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 10, name: "M. Jazuli Ihsanudin", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 11, name: "Mohammad Faisal Kudhori", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 12, name: "Muhamad Ali Muzaki", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 13, name: "Muhammad Farel Ashraf", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 14, name: "Muhammad Hamdi Syafi'I", class: "10-TKJ", gender: "Laki-laki", photo: null },
            { id: 15, name: "Niken Mayasari", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 16, name: "Ninis Rahmawati", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 17, name: "Nur Azizah", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 18, name: "Ria Agustianingsih", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 19, name: "Siti Aishatun Nafizah", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 20, name: "Siti Munawaroh", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 21, name: "Umi Musyawaroh", class: "10-TKJ", gender: "Perempuan", photo: null },
            { id: 22, name: "Widiya Indah Lestari", class: "10-TKJ", gender: "Perempuan", photo: null }
        ];

        // Initialize data in localStorage if not exists
        if (!localStorage.getItem('students')) {
            localStorage.setItem('students', JSON.stringify(initialStudents));
        }
        if (!localStorage.getItem('attendance')) {
            localStorage.setItem('attendance', JSON.stringify({}));
        }

        // DOM Elements
        const tabButtons = document.querySelectorAll('.tab-button');
        const tabContents = document.querySelectorAll('.tab-content');
        const studentModal = document.getElementById('studentModal');
        const attendanceModal = document.getElementById('attendanceModal');
        const logoModal = document.getElementById('logoModal');
        const toast = document.getElementById('toast');

        // Current date display
        const currentDate = new Date();
        document.getElementById('currentDate').textContent = formatDate(currentDate);
        document.getElementById('todayDate').textContent = formatDate(currentDate);
        document.getElementById('attendanceDate').valueAsDate = currentDate;

        // Set month selector to current month
        document.getElementById('monthSelector').value = currentDate.getMonth();
        document.getElementById('reportMonth').value = currentDate.getMonth();

        // Tab navigation
        tabButtons.forEach(button => {
            button.addEventListener('click', () => {
                const tabId = button.getAttribute('data-tab');
                
                // Update active tab button
                tabButtons.forEach(btn => btn.classList.remove('tab-active'));
                button.classList.add('tab-active');
                
                // Show selected tab content
                tabContents.forEach(content => {
                    if (content.id === tabId) {
                        content.classList.remove('hidden');
                    } else {
                        content.classList.add('hidden');
                    }
                });

                // Load data for the selected tab
                if (tabId === 'students') {
                    loadStudents();
                } else if (tabId === 'dashboard') {
                    loadDashboard();
                }
            });
        });

        // Logo click event
        document.getElementById('logoContainer').addEventListener('click', () => {
            logoModal.classList.remove('hidden');
        });

        // Upload logo button
        document.getElementById('uploadLogoBtn').addEventListener('click', () => {
            document.getElementById('logoFile').click();
        });

        // Logo file change
        document.getElementById('logoFile').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (event) => {
                    document.getElementById('currentLogo').src = event.target.result;
                };
                reader.readAsDataURL(file);
            }
        });

        // Save logo
        document.getElementById('saveLogoBtn').addEventListener('click', () => {
            const logoSrc = document.getElementById('currentLogo').src;
            document.getElementById('schoolLogo').src = logoSrc;
            document.getElementById('printLogo').src = logoSrc;
            localStorage.setItem('schoolLogo', logoSrc);
            logoModal.classList.add('hidden');
            showToast('Logo berhasil diperbarui');
        });

        // Cancel logo
        document.getElementById('cancelLogoBtn').addEventListener('click', () => {
            logoModal.classList.add('hidden');
        });

        // Load saved logo if exists
        if (localStorage.getItem('schoolLogo')) {
            const savedLogo = localStorage.getItem('schoolLogo');
            document.getElementById('schoolLogo').src = savedLogo;
            document.getElementById('currentLogo').src = savedLogo;
            document.getElementById('printLogo').src = savedLogo;
        }

        // Add student button
        document.getElementById('addStudentBtn').addEventListener('click', () => {
            document.getElementById('studentModalTitle').textContent = 'Tambah Siswa Baru';
            document.getElementById('studentId').value = '';
            document.getElementById('studentName').value = '';
            document.getElementById('studentClass').value = '';
            document.getElementById('studentGender').value = '';
            document.getElementById('photoPreview').innerHTML = `
                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                </svg>
            `;
            studentModal.classList.remove('hidden');
        });

        // Upload photo button
        document.getElementById('uploadPhotoBtn').addEventListener('click', () => {
            document.getElementById('studentPhoto').click();
        });

        // Student photo change
        document.getElementById('studentPhoto').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (event) => {
                    document.getElementById('photoPreview').innerHTML = `
                        <img src="${event.target.result}" class="w-full h-full object-cover">
                    `;
                };
                reader.readAsDataURL(file);
            }
        });

        // Save student
        document.getElementById('saveStudentBtn').addEventListener('click', () => {
            const id = document.getElementById('studentId').value;
            const name = document.getElementById('studentName').value;
            const studentClass = document.getElementById('studentClass').value;
            const gender = document.getElementById('studentGender').value;
            
            if (!name || !studentClass || !gender) {
                showToast('Mohon lengkapi semua data', 'error');
                return;
            }

            let photoSrc = null;
            const photoPreview = document.getElementById('photoPreview');
            if (photoPreview.querySelector('img')) {
                photoSrc = photoPreview.querySelector('img').src;
            }

            const students = JSON.parse(localStorage.getItem('students'));
            
            if (id) {
                // Update existing student
                const index = students.findIndex(s => s.id === parseInt(id));
                if (index !== -1) {
                    students[index] = {
                        ...students[index],
                        name,
                        class: studentClass,
                        gender,
                        photo: photoSrc
                    };
                }
            } else {
                // Add new student
                const newId = students.length > 0 ? Math.max(...students.map(s => s.id)) + 1 : 1;
                students.push({
                    id: newId,
                    name,
                    class: studentClass,
                    gender,
                    photo: photoSrc
                });
            }

            localStorage.setItem('students', JSON.stringify(students));
            studentModal.classList.add('hidden');
            loadStudents();
            showToast('Data siswa berhasil disimpan');
        });

        // Cancel student modal
        document.getElementById('cancelStudentBtn').addEventListener('click', () => {
            studentModal.classList.add('hidden');
        });

        // Load students
        function loadStudents() {
            const students = JSON.parse(localStorage.getItem('students'));
            const filterClass = document.getElementById('filterClass').value;
            const searchTerm = document.getElementById('searchStudent').value.toLowerCase();
            
            const filteredStudents = students.filter(student => {
                const matchesClass = !filterClass || student.class === filterClass;
                const matchesSearch = !searchTerm || student.name.toLowerCase().includes(searchTerm);
                return matchesClass && matchesSearch;
            });
            
            const tableBody = document.getElementById('studentsTableBody');
            tableBody.innerHTML = '';
            
            filteredStudents.forEach(student => {
                const row = document.createElement('tr');
                row.className = 'border-b hover:bg-gray-50';
                
                const photoCell = document.createElement('td');
                photoCell.className = 'py-3 px-6';
                
                const photoDiv = document.createElement('div');
                photoDiv.className = 'w-10 h-10 rounded-full bg-gray-200 flex items-center justify-center overflow-hidden';
                
                if (student.photo) {
                    const img = document.createElement('img');
                    img.src = student.photo;
                    img.className = 'w-full h-full object-cover';
                    photoDiv.appendChild(img);
                } else {
                    photoDiv.innerHTML = `
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                        </svg>
                    `;
                }
                
                photoCell.appendChild(photoDiv);
                row.appendChild(photoCell);
                
                const nameCell = document.createElement('td');
                nameCell.className = 'py-3 px-6';
                nameCell.textContent = student.name;
                row.appendChild(nameCell);
                
                const classCell = document.createElement('td');
                classCell.className = 'py-3 px-6';
                classCell.textContent = student.class;
                row.appendChild(classCell);
                
                const genderCell = document.createElement('td');
                genderCell.className = 'py-3 px-6';
                genderCell.textContent = student.gender;
                row.appendChild(genderCell);
                
                const actionCell = document.createElement('td');
                actionCell.className = 'py-3 px-6 text-center';
                
                const actionDiv = document.createElement('div');
                actionDiv.className = 'flex justify-center space-x-2';
                
                const editButton = document.createElement('button');
                editButton.className = 'bg-blue-500 text-white px-2 py-1 rounded-md text-xs hover:bg-blue-600 transition';
                editButton.textContent = 'Edit';
                editButton.addEventListener('click', () => editStudent(student.id));
                
                const deleteButton = document.createElement('button');
                deleteButton.className = 'bg-red-500 text-white px-2 py-1 rounded-md text-xs hover:bg-red-600 transition';
                deleteButton.textContent = 'Hapus';
                deleteButton.addEventListener('click', () => deleteStudent(student.id));
                
                actionDiv.appendChild(editButton);
                actionDiv.appendChild(deleteButton);
                actionCell.appendChild(actionDiv);
                row.appendChild(actionCell);
                
                tableBody.appendChild(row);
            });
        }

        // Edit student
        function editStudent(id) {
            const students = JSON.parse(localStorage.getItem('students'));
            const student = students.find(s => s.id === id);
            
            if (student) {
                document.getElementById('studentModalTitle').textContent = 'Edit Data Siswa';
                document.getElementById('studentId').value = student.id;
                document.getElementById('studentName').value = student.name;
                document.getElementById('studentClass').value = student.class;
                document.getElementById('studentGender').value = student.gender;
                
                if (student.photo) {
                    document.getElementById('photoPreview').innerHTML = `
                        <img src="${student.photo}" class="w-full h-full object-cover">
                    `;
                } else {
                    document.getElementById('photoPreview').innerHTML = `
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                        </svg>
                    `;
                }
                
                studentModal.classList.remove('hidden');
            }
        }

        // Delete student
        function deleteStudent(id) {
            if (confirm('Apakah Anda yakin ingin menghapus data siswa ini?')) {
                const students = JSON.parse(localStorage.getItem('students'));
                const updatedStudents = students.filter(s => s.id !== id);
                localStorage.setItem('students', JSON.stringify(updatedStudents));
                loadStudents();
                showToast('Data siswa berhasil dihapus');
            }
        }

        // Filter students
        document.getElementById('filterClass').addEventListener('change', loadStudents);
        document.getElementById('searchStudent').addEventListener('input', loadStudents);

        // Load attendance
        document.getElementById('loadAttendanceBtn').addEventListener('click', () => {
            const date = document.getElementById('attendanceDate').value;
            const selectedClass = document.getElementById('attendanceClass').value;
            
            if (!date || !selectedClass) {
                showToast('Pilih tanggal dan kelas terlebih dahulu', 'error');
                return;
            }
            
            loadAttendanceData(date, selectedClass);
        });

        // Load attendance data
        function loadAttendanceData(date, selectedClass) {
            const students = JSON.parse(localStorage.getItem('students')).filter(s => s.class === selectedClass);
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            const dateKey = date;
            
            const tableBody = document.getElementById('attendanceTableBody');
            tableBody.innerHTML = '';
            
            students.forEach((student, index) => {
                const studentAttendance = attendance[dateKey] && attendance[dateKey][student.id] 
                    ? attendance[dateKey][student.id] 
                    : null;
                
                const row = document.createElement('tr');
                row.className = 'border-b hover:bg-gray-50';
                if (studentAttendance) {
                    row.classList.add('fade-in');
                }
                
                const numberCell = document.createElement('td');
                numberCell.className = 'py-3 px-6 text-left';
                numberCell.textContent = index + 1;
                row.appendChild(numberCell);
                
                const nameCell = document.createElement('td');
                nameCell.className = 'py-3 px-6 text-left';
                
                const nameDiv = document.createElement('div');
                nameDiv.className = 'flex items-center';
                
                const photoDiv = document.createElement('div');
                photoDiv.className = 'w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center overflow-hidden mr-3';
                
                if (student.photo) {
                    const img = document.createElement('img');
                    img.src = student.photo;
                    img.className = 'w-full h-full object-cover';
                    photoDiv.appendChild(img);
                } else {
                    photoDiv.innerHTML = `
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                        </svg>
                    `;
                }
                
                nameDiv.appendChild(photoDiv);
                nameDiv.appendChild(document.createTextNode(student.name));
                nameCell.appendChild(nameDiv);
                row.appendChild(nameCell);
                
                const statusCell = document.createElement('td');
                statusCell.className = 'py-3 px-6 text-center';
                
                if (studentAttendance) {
                    let statusClass = '';
                    let statusText = '';
                    
                    switch (studentAttendance.status) {
                        case 'hadir':
                            statusClass = 'bg-green-100 text-green-800';
                            statusText = 'Hadir';
                            break;
                        case 'terlambat':
                            statusClass = 'bg-yellow-100 text-yellow-800';
                            statusText = 'Terlambat';
                            break;
                        case 'izin':
                            statusClass = 'bg-blue-100 text-blue-800';
                            statusText = 'Izin';
                            break;
                        case 'sakit':
                            statusClass = 'bg-purple-100 text-purple-800';
                            statusText = 'Sakit';
                            break;
                        case 'alpha':
                            statusClass = 'bg-red-100 text-red-800';
                            statusText = 'Alpha';
                            break;
                    }
                    
                    const statusBadge = document.createElement('span');
                    statusBadge.className = `px-2 py-1 rounded-full text-xs ${statusClass}`;
                    statusBadge.textContent = statusText;
                    statusCell.appendChild(statusBadge);
                } else {
                    const statusButton = document.createElement('button');
                    statusButton.className = 'bg-gray-200 hover:bg-gray-300 text-gray-700 px-3 py-1 rounded-md text-xs transition';
                    statusButton.textContent = 'Isi Absensi';
                    statusButton.addEventListener('click', () => openAttendanceModal(student, date));
                    statusCell.appendChild(statusButton);
                }
                
                row.appendChild(statusCell);
                
                const timeCell = document.createElement('td');
                timeCell.className = 'py-3 px-6 text-center text-sm';
                timeCell.textContent = studentAttendance ? studentAttendance.time : '-';
                row.appendChild(timeCell);
                
                const noteCell = document.createElement('td');
                noteCell.className = 'py-3 px-6 text-center text-sm';
                
                if (studentAttendance && studentAttendance.note) {
                    noteCell.textContent = studentAttendance.note;
                } else {
                    noteCell.textContent = '-';
                }
                
                row.appendChild(noteCell);
                tableBody.appendChild(row);
            });
        }

        // Open attendance modal
        function openAttendanceModal(student, date) {
            document.getElementById('attendanceStudentId').value = student.id;
            document.getElementById('attendanceStudentName').textContent = student.name;
            document.getElementById('attendanceStatus').value = 'hadir';
            document.getElementById('attendanceNote').value = '';
            
            // Store date for later use
            attendanceModal.dataset.date = date;
            
            attendanceModal.classList.remove('hidden');
        }

        // Save attendance status
        document.getElementById('saveAttendanceStatusBtn').addEventListener('click', () => {
            const studentId = parseInt(document.getElementById('attendanceStudentId').value);
            const status = document.getElementById('attendanceStatus').value;
            const note = document.getElementById('attendanceNote').value;
            const date = attendanceModal.dataset.date;
            
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            if (!attendance[date]) {
                attendance[date] = {};
            }
            
            attendance[date][studentId] = {
                status,
                note,
                time: new Date().toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' })
            };
            
            localStorage.setItem('attendance', JSON.stringify(attendance));
            attendanceModal.classList.add('hidden');
            
            // Reload attendance data
            const selectedClass = document.getElementById('attendanceClass').value;
            loadAttendanceData(date, selectedClass);
            
            showToast('Absensi berhasil disimpan');
        });

        // Cancel attendance modal
        document.getElementById('cancelAttendanceBtn').addEventListener('click', () => {
            attendanceModal.classList.add('hidden');
        });

        // Save attendance button
        document.getElementById('saveAttendanceBtn').addEventListener('click', () => {
            const date = document.getElementById('attendanceDate').value;
            const selectedClass = document.getElementById('attendanceClass').value;
            
            if (!date || !selectedClass) {
                showToast('Pilih tanggal dan kelas terlebih dahulu', 'error');
                return;
            }
            
            const students = JSON.parse(localStorage.getItem('students')).filter(s => s.class === selectedClass);
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            let allFilled = true;
            students.forEach(student => {
                if (!attendance[date] || !attendance[date][student.id]) {
                    allFilled = false;
                }
            });
            
            if (!allFilled) {
                if (confirm('Beberapa siswa belum diisi absensinya. Tetap simpan?')) {
                    showToast('Absensi berhasil disimpan');
                    loadDashboard();
                }
            } else {
                showToast('Absensi berhasil disimpan');
                loadDashboard();
            }
        });

        // Generate report
        document.getElementById('generateReportBtn').addEventListener('click', () => {
            const month = parseInt(document.getElementById('reportMonth').value);
            const year = parseInt(document.getElementById('reportYear').value);
            const selectedClass = document.getElementById('reportClass').value;
            
            if (!selectedClass) {
                showToast('Pilih kelas terlebih dahulu', 'error');
                return;
            }
            
            generateMonthlyReport(month, year, selectedClass);
        });

        // Generate monthly report
        function generateMonthlyReport(month, year, selectedClass) {
            const students = JSON.parse(localStorage.getItem('students')).filter(s => s.class === selectedClass);
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            const monthName = new Date(year, month, 1).toLocaleString('id-ID', { month: 'long' });
            
            const reportContent = document.getElementById('reportContent');
            reportContent.innerHTML = `
                <div class="mb-4">
                    <h3 class="text-xl font-bold text-gray-800">Laporan Absensi Bulan ${monthName} ${year}</h3>
                    <p class="text-sm text-gray-600">Kelas: ${selectedClass}</p>
                </div>
                
                <div class="overflow-x-auto">
                    <table class="min-w-full bg-white border">
                        <thead>
                            <tr class="bg-gray-100 text-gray-600 uppercase text-sm leading-normal">
                                <th class="py-3 px-2 text-left border">No</th>
                                <th class="py-3 px-2 text-left border">Nama Siswa</th>
                                ${Array.from({ length: daysInMonth }, (_, i) => `
                                    <th class="py-3 px-2 text-center border w-10">${i + 1}</th>
                                `).join('')}
                                <th class="py-3 px-2 text-center border">H</th>
                                <th class="py-3 px-2 text-center border">T</th>
                                <th class="py-3 px-2 text-center border">I</th>
                                <th class="py-3 px-2 text-center border">S</th>
                                <th class="py-3 px-2 text-center border">A</th>
                            </tr>
                        </thead>
                        <tbody>
                            ${students.map((student, index) => {
                                let hadir = 0, terlambat = 0, izin = 0, sakit = 0, alpha = 0;
                                
                                const dayCells = Array.from({ length: daysInMonth }, (_, i) => {
                                    const day = i + 1;
                                    const dateStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                                    
                                    if (attendance[dateStr] && attendance[dateStr][student.id]) {
                                        const status = attendance[dateStr][student.id].status;
                                        let statusMark = '';
                                        let statusClass = '';
                                        
                                        switch (status) {
                                            case 'hadir':
                                                statusMark = 'H';
                                                statusClass = 'bg-green-100 text-green-800';
                                                hadir++;
                                                break;
                                            case 'terlambat':
                                                statusMark = 'T';
                                                statusClass = 'bg-yellow-100 text-yellow-800';
                                                terlambat++;
                                                break;
                                            case 'izin':
                                                statusMark = 'I';
                                                statusClass = 'bg-blue-100 text-blue-800';
                                                izin++;
                                                break;
                                            case 'sakit':
                                                statusMark = 'S';
                                                statusClass = 'bg-purple-100 text-purple-800';
                                                sakit++;
                                                break;
                                            case 'alpha':
                                                statusMark = 'A';
                                                statusClass = 'bg-red-100 text-red-800';
                                                alpha++;
                                                break;
                                        }
                                        
                                        return `<td class="py-1 px-2 text-center border"><span class="inline-block w-6 h-6 rounded-full ${statusClass} flex items-center justify-center text-xs font-medium">${statusMark}</span></td>`;
                                    } else {
                                        return `<td class="py-1 px-2 text-center border">-</td>`;
                                    }
                                }).join('');
                                
                                return `
                                    <tr class="border-b hover:bg-gray-50">
                                        <td class="py-2 px-2 text-left border">${index + 1}</td>
                                        <td class="py-2 px-2 text-left border">${student.name}</td>
                                        ${dayCells}
                                        <td class="py-2 px-2 text-center border font-medium text-green-600">${hadir}</td>
                                        <td class="py-2 px-2 text-center border font-medium text-yellow-600">${terlambat}</td>
                                        <td class="py-2 px-2 text-center border font-medium text-blue-600">${izin}</td>
                                        <td class="py-2 px-2 text-center border font-medium text-purple-600">${sakit}</td>
                                        <td class="py-2 px-2 text-center border font-medium text-red-600">${alpha}</td>
                                    </tr>
                                `;
                            }).join('')}
                        </tbody>
                    </table>
                </div>
                
                <div class="mt-6">
                    <h4 class="font-semibold mb-2">Keterangan:</h4>
                    <div class="flex flex-wrap gap-4">
                        <div class="flex items-center">
                            <span class="inline-block w-6 h-6 rounded-full bg-green-100 text-green-800 flex items-center justify-center text-xs font-medium mr-2">H</span>
                            <span>Hadir</span>
                        </div>
                        <div class="flex items-center">
                            <span class="inline-block w-6 h-6 rounded-full bg-yellow-100 text-yellow-800 flex items-center justify-center text-xs font-medium mr-2">T</span>
                            <span>Terlambat</span>
                        </div>
                        <div class="flex items-center">
                            <span class="inline-block w-6 h-6 rounded-full bg-blue-100 text-blue-800 flex items-center justify-center text-xs font-medium mr-2">I</span>
                            <span>Izin</span>
                        </div>
                        <div class="flex items-center">
                            <span class="inline-block w-6 h-6 rounded-full bg-purple-100 text-purple-800 flex items-center justify-center text-xs font-medium mr-2">S</span>
                            <span>Sakit</span>
                        </div>
                        <div class="flex items-center">
                            <span class="inline-block w-6 h-6 rounded-full bg-red-100 text-red-800 flex items-center justify-center text-xs font-medium mr-2">A</span>
                            <span>Alpha</span>
                        </div>
                    </div>
                </div>
                
                <div class="mt-8 text-right">
                    <p class="mb-16">Wali Kelas</p>
                    <p class="font-semibold">Mukhamad Khoirul Riski S.Kom</p>
                </div>
            `;

            // Update print template
            document.getElementById('printDate').textContent = `${monthName} ${year}`;
            document.getElementById('printClass').textContent = `Kelas: ${selectedClass}`;
            document.getElementById('printReportContent').innerHTML = reportContent.innerHTML;
        }

        // Print button
        document.getElementById('printButton').addEventListener('click', () => {
            window.print();
        });

        // Dashboard
        function loadDashboard() {
            const currentDate = new Date();
            const today = formatDateISO(currentDate);
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            // Today's attendance counts
            let presentCount = 0;
            let lateCount = 0;
            let absentCount = 0;
            let permissionCount = 0;
            
            if (attendance[today]) {
                Object.values(attendance[today]).forEach(record => {
                    switch (record.status) {
                        case 'hadir':
                            presentCount++;
                            break;
                        case 'terlambat':
                            lateCount++;
                            break;
                        case 'alpha':
                            absentCount++;
                            break;
                        case 'izin':
                        case 'sakit':
                            permissionCount++;
                            break;
                    }
                });
            }
            
            document.getElementById('presentCount').textContent = presentCount;
            document.getElementById('lateCount').textContent = lateCount;
            document.getElementById('absentCount').textContent = absentCount;
            document.getElementById('permissionCount').textContent = permissionCount;
            
            // Weekly chart
            const weekDays = [];
            const weekData = {
                present: [],
                late: [],
                absent: [],
                permission: []
            };
            
            for (let i = 6; i >= 0; i--) {
                const date = new Date();
                date.setDate(date.getDate() - i);
                const dateStr = formatDateISO(date);
                const dayName = date.toLocaleDateString('id-ID', { weekday: 'short' });
                
                weekDays.push(dayName);
                
                let dayPresent = 0;
                let dayLate = 0;
                let dayAbsent = 0;
                let dayPermission = 0;
                
                if (attendance[dateStr]) {
                    Object.values(attendance[dateStr]).forEach(record => {
                        switch (record.status) {
                            case 'hadir':
                                dayPresent++;
                                break;
                            case 'terlambat':
                                dayLate++;
                                break;
                            case 'alpha':
                                dayAbsent++;
                                break;
                            case 'izin':
                            case 'sakit':
                                dayPermission++;
                                break;
                        }
                    });
                }
                
                weekData.present.push(dayPresent);
                weekData.late.push(dayLate);
                weekData.absent.push(dayAbsent);
                weekData.permission.push(dayPermission);
            }
            
            const weeklyCtx = document.getElementById('weeklyChart').getContext('2d');
            if (window.weeklyChart) {
                window.weeklyChart.destroy();
            }
            window.weeklyChart = new Chart(weeklyCtx, {
                type: 'bar',
                data: {
                    labels: weekDays,
                    datasets: [
                        {
                            label: 'Hadir',
                            data: weekData.present,
                            backgroundColor: 'rgba(34, 197, 94, 0.7)',
                            borderColor: 'rgba(34, 197, 94, 1)',
                            borderWidth: 1
                        },
                        {
                            label: 'Terlambat',
                            data: weekData.late,
                            backgroundColor: 'rgba(234, 179, 8, 0.7)',
                            borderColor: 'rgba(234, 179, 8, 1)',
                            borderWidth: 1
                        },
                        {
                            label: 'Izin/Sakit',
                            data: weekData.permission,
                            backgroundColor: 'rgba(59, 130, 246, 0.7)',
                            borderColor: 'rgba(59, 130, 246, 1)',
                            borderWidth: 1
                        },
                        {
                            label: 'Alpha',
                            data: weekData.absent,
                            backgroundColor: 'rgba(239, 68, 68, 0.7)',
                            borderColor: 'rgba(239, 68, 68, 1)',
                            borderWidth: 1
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            stacked: true
                        },
                        y: {
                            stacked: true,
                            beginAtZero: true,
                            ticks: {
                                precision: 0
                            }
                        }
                    }
                }
            });
            
            // Attendance donut chart
            const totalStudents = JSON.parse(localStorage.getItem('students')).length;
            const attendancePercentage = totalStudents > 0 ? Math.round((presentCount + lateCount) / totalStudents * 100) : 0;
            const absentPercentage = totalStudents > 0 ? Math.round((absentCount + permissionCount) / totalStudents * 100) : 0;
            const notRecordedPercentage = 100 - attendancePercentage - absentPercentage;
            
            const donutCtx = document.getElementById('attendanceDonut').getContext('2d');
            if (window.donutChart) {
                window.donutChart.destroy();
            }
            window.donutChart = new Chart(donutCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Hadir', 'Tidak Hadir', 'Belum Direkam'],
                    datasets: [{
                        data: [attendancePercentage, absentPercentage, notRecordedPercentage],
                        backgroundColor: [
                            'rgba(34, 197, 94, 0.7)',
                            'rgba(239, 68, 68, 0.7)',
                            'rgba(209, 213, 219, 0.7)'
                        ],
                        borderColor: [
                            'rgba(34, 197, 94, 1)',
                            'rgba(239, 68, 68, 1)',
                            'rgba(209, 213, 219, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `${context.label}: ${context.raw}%`;
                                }
                            }
                        }
                    }
                }
            });
            
            // Monthly chart
            updateMonthlyChart();
            
            // Top students
            updateTopStudents();
            
            // Class chart
            updateClassChart();
        }

        // Update monthly chart
        function updateMonthlyChart() {
            const month = parseInt(document.getElementById('monthSelector').value);
            const year = parseInt(document.getElementById('yearSelector').value);
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            const days = Array.from({ length: daysInMonth }, (_, i) => i + 1);
            
            const monthData = {
                present: Array(daysInMonth).fill(0),
                late: Array(daysInMonth).fill(0),
                absent: Array(daysInMonth).fill(0),
                permission: Array(daysInMonth).fill(0)
            };
            
            for (let i = 0; i < daysInMonth; i++) {
                const day = i + 1;
                const dateStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                
                if (attendance[dateStr]) {
                    Object.values(attendance[dateStr]).forEach(record => {
                        switch (record.status) {
                            case 'hadir':
                                monthData.present[i]++;
                                break;
                            case 'terlambat':
                                monthData.late[i]++;
                                break;
                            case 'alpha':
                                monthData.absent[i]++;
                                break;
                            case 'izin':
                            case 'sakit':
                                monthData.permission[i]++;
                                break;
                        }
                    });
                }
            }
            
            const monthlyCtx = document.getElementById('monthlyChart').getContext('2d');
            if (window.monthlyChart) {
                window.monthlyChart.destroy();
            }
            window.monthlyChart = new Chart(monthlyCtx, {
                type: 'line',
                data: {
                    labels: days,
                    datasets: [
                        {
                            label: 'Hadir',
                            data: monthData.present,
                            backgroundColor: 'rgba(34, 197, 94, 0.1)',
                            borderColor: 'rgba(34, 197, 94, 1)',
                            borderWidth: 2,
                            tension: 0.3,
                            fill: true
                        },
                        {
                            label: 'Terlambat',
                            data: monthData.late,
                            backgroundColor: 'rgba(234, 179, 8, 0.1)',
                            borderColor: 'rgba(234, 179, 8, 1)',
                            borderWidth: 2,
                            tension: 0.3,
                            fill: true
                        },
                        {
                            label: 'Izin/Sakit',
                            data: monthData.permission,
                            backgroundColor: 'rgba(59, 130, 246, 0.1)',
                            borderColor: 'rgba(59, 130, 246, 1)',
                            borderWidth: 2,
                            tension: 0.3,
                            fill: true
                        },
                        {
                            label: 'Alpha',
                            data: monthData.absent,
                            backgroundColor: 'rgba(239, 68, 68, 0.1)',
                            borderColor: 'rgba(239, 68, 68, 1)',
                            borderWidth: 2,
                            tension: 0.3,
                            fill: true
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                precision: 0
                            }
                        }
                    }
                }
            });
        }

        // Month selector change
        document.getElementById('monthSelector').addEventListener('change', updateMonthlyChart);
        document.getElementById('yearSelector').addEventListener('change', updateMonthlyChart);

        // Update top students
        function updateTopStudents() {
            const students = JSON.parse(localStorage.getItem('students'));
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            const studentAttendance = {};
            
            // Count attendance for each student
            students.forEach(student => {
                studentAttendance[student.id] = {
                    id: student.id,
                    name: student.name,
                    photo: student.photo,
                    class: student.class,
                    present: 0,
                    total: 0
                };
            });
            
            // Calculate attendance
            Object.keys(attendance).forEach(date => {
                Object.keys(attendance[date]).forEach(studentId => {
                    const studentId_int = parseInt(studentId);
                    if (studentAttendance[studentId_int]) {
                        studentAttendance[studentId_int].total++;
                        
                        if (attendance[date][studentId].status === 'hadir') {
                            studentAttendance[studentId_int].present++;
                        }
                    }
                });
            });
            
            // Sort by attendance percentage
            const sortedStudents = Object.values(studentAttendance)
                .filter(s => s.total > 0)
                .sort((a, b) => {
                    const aPercentage = a.present / a.total;
                    const bPercentage = b.present / b.total;
                    return bPercentage - aPercentage;
                })
                .slice(0, 5);
            
            const topStudentsContainer = document.getElementById('topStudents');
            topStudentsContainer.innerHTML = '';
            
            if (sortedStudents.length === 0) {
                topStudentsContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-4">
                        Belum ada data absensi yang cukup
                    </div>
                `;
                return;
            }
            
            sortedStudents.forEach((student, index) => {
                const percentage = Math.round(student.present / student.total * 100);
                
                const studentDiv = document.createElement('div');
                studentDiv.className = 'flex items-center justify-between p-3 bg-gray-50 rounded-lg slide-in';
                studentDiv.style.animationDelay = `${index * 0.1}s`;
                
                const leftDiv = document.createElement('div');
                leftDiv.className = 'flex items-center';
                
                const rankSpan = document.createElement('span');
                rankSpan.className = 'w-6 h-6 rounded-full bg-indigo-600 text-white flex items-center justify-center text-sm font-medium mr-3';
                rankSpan.textContent = index + 1;
                
                const photoDiv = document.createElement('div');
                photoDiv.className = 'w-10 h-10 rounded-full bg-gray-200 flex items-center justify-center overflow-hidden mr-3';
                
                if (student.photo) {
                    const img = document.createElement('img');
                    img.src = student.photo;
                    img.className = 'w-full h-full object-cover';
                    photoDiv.appendChild(img);
                } else {
                    photoDiv.innerHTML = `
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                        </svg>
                    `;
                }
                
                const infoDiv = document.createElement('div');
                
                const nameP = document.createElement('p');
                nameP.className = 'font-medium';
                nameP.textContent = student.name;
                
                const classP = document.createElement('p');
                classP.className = 'text-xs text-gray-500';
                classP.textContent = student.class;
                
                infoDiv.appendChild(nameP);
                infoDiv.appendChild(classP);
                
                leftDiv.appendChild(rankSpan);
                leftDiv.appendChild(photoDiv);
                leftDiv.appendChild(infoDiv);
                
                const percentageDiv = document.createElement('div');
                percentageDiv.className = 'text-right';
                
                const percentageP = document.createElement('p');
                percentageP.className = 'text-lg font-bold text-indigo-600';
                percentageP.textContent = `${percentage}%`;
                
                const attendanceP = document.createElement('p');
                attendanceP.className = 'text-xs text-gray-500';
                attendanceP.textContent = `${student.present}/${student.total} hari`;
                
                percentageDiv.appendChild(percentageP);
                percentageDiv.appendChild(attendanceP);
                
                studentDiv.appendChild(leftDiv);
                studentDiv.appendChild(percentageDiv);
                
                topStudentsContainer.appendChild(studentDiv);
            });
        }

        // Update class chart
        function updateClassChart() {
            const students = JSON.parse(localStorage.getItem('students'));
            const attendance = JSON.parse(localStorage.getItem('attendance'));
            
            // Get unique classes
            const classes = [...new Set(students.map(s => s.class))].filter(c => c);
            
            // Count students per class
            const classStudents = {};
            classes.forEach(cls => {
                classStudents[cls] = students.filter(s => s.class === cls).length;
            });
            
            // Count attendance per class for today
            const today = formatDateISO(new Date());
            const classAttendance = {};
            classes.forEach(cls => {
                classAttendance[cls] = 0;
            });
            
            if (attendance[today]) {
                Object.keys(attendance[today]).forEach(studentId => {
                    const student = students.find(s => s.id === parseInt(studentId));
                    if (student && student.class && attendance[today][studentId].status === 'hadir') {
                        classAttendance[student.class]++;
                    }
                });
            }
            
            // Calculate attendance percentage
            const attendancePercentage = {};
            classes.forEach(cls => {
                attendancePercentage[cls] = classStudents[cls] > 0 
                    ? Math.round(classAttendance[cls] / classStudents[cls] * 100) 
                    : 0;
            });
            
            const classCtx = document.getElementById('classChart').getContext('2d');
            if (window.classChart) {
                window.classChart.destroy();
            }
            window.classChart = new Chart(classCtx, {
                type: 'bar',
                data: {
                    labels: classes,
                    datasets: [
                        {
                            label: 'Persentase Kehadiran',
                            data: classes.map(cls => attendancePercentage[cls]),
                            backgroundColor: 'rgba(79, 70, 229, 0.7)',
                            borderColor: 'rgba(79, 70, 229, 1)',
                            borderWidth: 1
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                callback: function(value) {
                                    return value + '%';
                                }
                            }
                        }
                    },
                    plugins: {
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    const cls = context.label;
                                    return `Kehadiran: ${context.raw}% (${classAttendance[cls]}/${classStudents[cls]} siswa)`;
                                }
                            }
                        }
                    }
                }
            });
        }

        // Helper functions
        function formatDate(date) {
            return date.toLocaleDateString('id-ID', {
                weekday: 'long',
                day: 'numeric',
                month: 'long',
                year: 'numeric'
            });
        }

        function formatDateISO(date) {
            return `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}-${String(date.getDate()).padStart(2, '0')}`;
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const toastMessage = document.getElementById('toastMessage');
            
            toastMessage.textContent = message;
            
            if (type === 'success') {
                toast.classList.remove('bg-red-500');
                toast.classList.add('bg-green-500');
            } else {
                toast.classList.remove('bg-green-500');
                toast.classList.add('bg-red-500');
            }
            
            toast.classList.remove('translate-y-20', 'opacity-0');
            
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }

        // Initialize dashboard on load
        loadDashboard();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9622146c804c2afe',t:'MTc1MzAxMDUzNi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

