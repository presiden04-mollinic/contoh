<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Rekrutmen & SPK SAW - Toko Putrastore</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0fdfa',
                            100: '#ccfbf1',
                            500: '#14b8a6',
                            600: '#0d9488',
                            700: '#0f766e',
                            800: '#115e59',
                            900: '#134e4a',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        @media print {
            .no-print {
                display: none !important;
            }
            .print-only {
                display: block !important;
            }
            body {
                background: white !important;
                color: black !important;
            }
            .print-card {
                box-shadow: none !important;
                border: 1px solid #ccc !important;
            }
        }
        .animate-fade-in {
            animation: fadeIn 0.3s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(6px); }
            to { opacity: 1; transform: translateY(0); }
        }
        /* Custom scrollbar for tables */
        ::-webkit-scrollbar {
            height: 6px;
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
    </style>
</head>
<body class="bg-slate-100 text-slate-800 font-sans min-h-screen flex flex-col antialiased">

    <!-- TOP HEADER -->
    <header class="bg-gradient-to-r from-teal-800 via-teal-700 to-indigo-800 text-white shadow-lg sticky top-0 z-40 no-print">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <!-- Branding -->
                <div class="flex items-center space-x-3 cursor-pointer" onclick="resetToPublicHome()">
                    <div class="w-12 h-12 rounded-xl bg-white/10 backdrop-blur-md flex items-center justify-center border border-white/20 shadow-inner">
                        <i class="fa-solid fa-store text-2xl text-teal-300"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-xl sm:text-2xl tracking-tight leading-tight flex items-center gap-2">
                            PutraStore ID
                            <span class="text-xs font-normal px-2.5 py-0.5 rounded-full bg-teal-500/30 border border-teal-300/30 text-teal-100">
                                Official System
                            </span>
                        </h1>
                        <p class="text-xs sm:text-sm text-teal-100/90 font-medium">
                            Portal Pendaftaran & Rekrutmen Karyawan Baru
                        </p>
                    </div>
                </div>

                <!-- Mode Indicator & Login / Logout Button -->
                <div class="flex items-center gap-3">
                    <!-- Status Badge -->
                    <div id="mode-badge" class="hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-full text-xs font-semibold bg-white/10 border border-white/20 text-teal-100">
                        <span id="mode-dot" class="w-2.5 h-2.5 rounded-full bg-emerald-400 animate-pulse"></span>
                        <span id="mode-text">Mode Publik</span>
                    </div>

                    <!-- Action Button -->
                    <div id="auth-button-container">
                        <button onclick="openLoginModal()" id="btn-login-admin" class="px-4 py-2 bg-amber-500 hover:bg-amber-600 active:bg-amber-700 text-slate-950 font-semibold text-sm rounded-xl shadow-md hover:shadow-lg transition flex items-center gap-2 border border-amber-300">
                            <i class="fa-solid fa-key text-xs"></i>
                            <span>Login Admin</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- ADMIN NAVIGATION TABS (Strictly hidden in Public Mode) -->
        <nav id="admin-nav-bar" class="hidden bg-teal-900/90 backdrop-blur-md border-t border-white/10 shadow-inner">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex flex-wrap items-center justify-start gap-1 sm:gap-2 py-2 overflow-x-auto">
                    
                    <button onclick="switchAdminTab('applicants')" id="tab-btn-applicants" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-semibold transition flex items-center gap-2 text-white/80 hover:text-white hover:bg-white/10">
                        <i class="fa-solid fa-users text-teal-300"></i>
                        <span>Calon Karyawan</span>
                        <span id="applicant-count-badge" class="ml-1 text-xs px-2 py-0.5 rounded-full bg-teal-500/40 text-teal-200 border border-teal-400/30">0</span>
                    </button>

                    <button onclick="switchAdminTab('criteria')" id="tab-btn-criteria" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-semibold transition flex items-center gap-2 text-white/80 hover:text-white hover:bg-white/10">
                        <i class="fa-solid fa-sliders text-teal-300"></i>
                        <span>Kriteria SAW</span>
                    </button>

                    <button onclick="switchAdminTab('saw')" id="tab-btn-saw" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-semibold transition flex items-center gap-2 text-white/80 hover:text-white hover:bg-white/10">
                        <i class="fa-solid fa-calculator text-teal-300"></i>
                        <span>Perhitungan SAW</span>
                    </button>

                    <button onclick="switchAdminTab('add_applicant')" id="tab-btn-add_applicant" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-semibold transition flex items-center gap-2 text-white/80 hover:text-white hover:bg-white/10">
                        <i class="fa-solid fa-user-plus text-teal-300"></i>
                        <span>Tambah Manual</span>
                    </button>

                    <button onclick="switchAdminTab('settings')" id="tab-btn-settings" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-semibold transition flex items-center gap-2 text-white/80 hover:text-white hover:bg-white/10">
                        <i class="fa-solid fa-gear text-teal-300"></i>
                        <span>Pengaturan</span>
                    </button>

                </div>
            </div>
        </nav>
    </header>

    <!-- MAIN CONTENT CONTAINER -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- ========================================================= -->
        <!-- PUBLIC VIEW 1: REGISTRATION FORM (Tampilan Pendaftaran) -->
        <!-- ========================================================= -->
        <section id="view-public-form" class="animate-fade-in max-w-4xl mx-auto space-y-6">
            
            <!-- Banner Informatif -->
            <div class="bg-gradient-to-r from-teal-600 to-emerald-700 text-white p-6 sm:p-8 rounded-2xl shadow-xl relative overflow-hidden">
                <div class="absolute -right-10 -bottom-10 opacity-15 text-white pointer-events-none">
                    <i class="fa-solid fa-user-tie text-9xl"></i>
                </div>
                <div class="relative z-10 max-w-2xl">
                    <span class="inline-block px-3 py-1 rounded-full bg-white/20 text-xs font-semibold tracking-wide uppercase mb-3 backdrop-blur-sm">
                        Open Recruitment
                    </span>
                    <h2 class="text-2xl sm:text-3xl font-extrabold tracking-tight">
                        Formulir Pendaftaran Calon Karyawan (KASIR)
                    </h2>
                    <p class="mt-2 text-sm sm:text-base text-teal-100 leading-relaxed">
                        Bergabunglah bersama Toko PutraStore ID, Silakan isi formulir Pendaftaran di bawah ini.
                    </p>
                </div>
            </div>

            <!-- Registration Form Card -->
            <form id="form-registration" onsubmit="handleRegistrationSubmit(event)" class="bg-white rounded-2xl shadow-lg border border-slate-200/80 p-6 sm:p-8 space-y-8">
                
                <!-- Section 1: Identitas Diri -->
                <div>
                    <h3 class="text-lg font-bold text-slate-900 border-b pb-3 mb-5 flex items-center gap-2">
                        <i class="fa-solid fa-id-card text-teal-600"></i>
                        <span>1. Informasi Identitas Pelamar</span>
                    </h3>
                    
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                        <!-- Nama Lengkap -->
                        <div class="sm:col-span-2">
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Nama Lengkap <span class="text-rose-500">*</span>
                            </label>
                            <input type="text" id="reg-nama" required placeholder="Contoh: Putra Edo"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm transition shadow-sm">
                        </div>

                        <!-- No WhatsApp / HP -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                No. WhatsApp / HP <span class="text-rose-500">*</span>
                            </label>
                            <input type="tel" id="reg-phone" required placeholder="081236215830"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm transition shadow-sm">
                        </div>

                        <!-- Umur -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Usia / Umur (Tahun) <span class="text-rose-500">*</span>
                            </label>
                            <input type="number" id="reg-umur" required min="17" max="40" placeholder="min 17-40"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm transition shadow-sm">
                        </div>

                        <!-- Domisili -->
                        <div class="sm:col-span-2">
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Alamat Domisili <span class="text-rose-500">*</span>
                            </label>
                            <input type="text" id="reg-domisili" required placeholder="Kota / Jl.Samratulangi III"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm transition shadow-sm">
                        </div>
                    </div>
                </div>

                <!-- Section 2: Kualifikasi & Penilaian SAW -->
                <div>
                    <h3 class="text-lg font-bold text-slate-900 border-b pb-3 mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-award text-teal-600"></i>
                        <span>2. Kualifikasi & Penilaian Kriteria</span>
                    </h3>
                    <p class="text-xs text-slate-500 mb-5">
                        Pilihlah dengan jujur kriteria yang paling mencerminkan latar belakang dan keahlian Anda untuk perhitungan keputusan SAW.
                    </p>

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
                        
                        <!-- Pendidikan Terakhir (C1) -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Pendidikan Terakhir <span class="text-rose-500">*</span>
                            </label>
                            <select id="reg-c1" required class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm bg-white transition shadow-sm">
                                <option value="" disabled selected>-- Pilih Pendidikan --</option>
                                <option value="2">SMA / SMK Sederajat </option>
                                <option value="3">Diploma (D3)</option>
                                <option value="4">Sarjana (S1)</option>
                                <option value="5">Magister (S2)</option>
                            </select>
                        </div>

                        <!-- Pengalaman Kerja (C2) -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Pengalaman Kerja <span class="text-rose-500">*</span>
                            </label>
                            <select id="reg-c2" required class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm bg-white transition shadow-sm">
                                <option value="" disabled selected>-- Pengalaman Sebagai Kasir --</option>
                                <option value="1">Belum Ada Pengalaman / Fresh Graduate </option>
                                <option value="2">Kurang dari 1 Tahun </option>
                                <option value="3">1 - 2 Tahun </option>
                                <option value="4">3 - 4 Tahun </option>
                                <option value="5">Lebih dari 4 Tahun </option>
                            </select>
                        </div>

                        <!-- Nilai Tes Keterampilan / Soft Skill (C3) -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Kemampuan Operasional / Kasir <span class="text-rose-500">*</span>
                            </label>
                            <select id="reg-c3" required class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm bg-white transition shadow-sm">
                                <option value="" disabled selected>-- Self Assessment Keterampilan --</option>
                                <option value="2">Cukup Baik </option>
                                <option value="3">Baik / Menguasai Dasar</option>
                                <option value="4">Sangat Baik / Terbiasa Dengan Sistem </option>
                                <option value="5">Ahli / Mahir </option>
                            </select>
                        </div>

                        <!-- Sikap / Attitude & Komunikasi (C4) -->
                        <div>
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Kesiapan Kerja & Kedisiplinan <span class="text-rose-500">*</span>
                            </label>
                            <select id="reg-c4" required class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm bg-white transition shadow-sm">
                                <option value="" disabled selected>-- Pilih Kesediaan Shift --</option>
                                <option value="3">Shift malam jam 19.00 - 07.00</option>
                                <option value="3">Shift siang jam 07.00 - 19.00</option>
                                <option value="4">Siap Full Time & Lembur </option>
                                <option value="5">Sangat Fleksibel & Siap Kerja Segera </option>
                            </select>
                        </div>

                        <!-- Jarak / Aksesibilitas Domisili (C5 - Cost/Benefit) -->
                        <div class="sm:col-span-2">
                            <label class="block text-sm font-semibold text-slate-700 mb-1">
                                Jarak Tempuh ke Toko Putrastoreid <span class="text-rose-500">*</span>
                            </label>
                            <select id="reg-c5" required class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-teal-500 focus:border-teal-500 text-sm bg-white transition shadow-sm">
                                <option value="" disabled selected>-- Pilih Estimasi Jarak --</option>
                                <option value="5">Sangat Dekat (< 3 km) </option>
                                <option value="4">Dekat (3 - 7 km) </option>
                                <option value="3">Sedang (7 - 12 km) </option>
                                <option value="2">Jauh (> 12 km) </option>
                            </select>
                        </div>

                    </div>
                </div>

                <!-- Submit Button -->
                <div class="pt-4 border-t flex flex-col sm:flex-row items-center justify-between gap-4">
                    <p class="text-xs text-slate-500">
                        * Pastikan seluruh data diisi dengan jujur dan teliti.
                    </p>
                    <button type="submit" class="w-full sm:w-auto px-8 py-3.5 bg-gradient-to-r from-teal-600 to-emerald-600 hover:from-teal-700 hover:to-emerald-700 text-white font-bold text-base rounded-xl shadow-lg hover:shadow-xl transition transform active:scale-95 flex items-center justify-center gap-2">
                        <i class="fa-solid fa-paper-plane"></i>
                        <span>Kirim Pendaftaran Sekarang</span>
                    </button>
                </div>

            </form>

        </section>


        <!-- ========================================================= -->
        <!-- PUBLIC VIEW 2: REGISTRATION TICKET / STRUK BUKTI          -->
        <!-- ========================================================= -->
        <section id="view-public-ticket" class="hidden animate-fade-in max-w-2xl mx-auto space-y-6">
            
            <div id="ticket-card" class="bg-white rounded-3xl shadow-2xl border border-slate-200 overflow-hidden print-card">
                
                <!-- Ticket Header -->
                <div class="bg-gradient-to-r from-teal-700 to-teal-900 text-white p-6 sm:p-8 text-center relative">
                    <div class="w-16 h-16 rounded-2xl bg-white/10 backdrop-blur-md border border-white/20 flex items-center justify-center mx-auto mb-3 shadow-inner">
                        <i class="fa-solid fa-circle-check text-3xl text-emerald-300"></i>
                    </div>
                    <h2 class="text-2xl font-bold">Bukti Pendaftaran Berhasil</h2>
                    <p class="text-xs sm:text-sm text-teal-200 mt-1">Toko PutraStore ID - Sistem Rekrutmen SAW</p>
                    <div class="mt-4 inline-block px-4 py-1.5 rounded-full bg-emerald-500/30 border border-emerald-300/40 text-emerald-100 text-xs font-semibold">
                        Status: Menunggu Seleksi Sistem SAW
                    </div>
                </div>

                <!-- Ticket Body -->
                <div class="p-6 sm:p-8 space-y-6">
                    
                    <div class="flex flex-col sm:flex-row justify-between items-center gap-4 bg-slate-50 p-4 rounded-2xl border border-slate-200/80">
                        <div>
                            <span class="text-xs text-slate-400 font-semibold uppercase tracking-wider block">Nomor Registrasi</span>
                            <span id="ticket-id" class="text-xl font-mono font-extrabold text-teal-800">PUTRA-2026-0001</span>
                        </div>
                        <div class="text-right">
                            <span class="text-xs text-slate-400 font-semibold uppercase tracking-wider block">Tanggal Daftar</span>
                            <span id="ticket-date" class="text-sm font-semibold text-slate-700">28 Sep 2026</span>
                        </div>
                    </div>

                    <div class="space-y-3 border-t pt-4 text-sm">
                        <div class="flex justify-between py-1 border-b border-slate-100">
                            <span class="text-slate-500">Nama Pelamar</span>
                            <span id="ticket-nama" class="font-semibold text-slate-800">-</span>
                        </div>
                        <div class="flex justify-between py-1 border-b border-slate-100">
                            <span class="text-slate-500">No. WhatsApp</span>
                            <span id="ticket-phone" class="font-semibold text-slate-800">-</span>
                        </div>
                        <div class="flex justify-between py-1 border-b border-slate-100">
                            <span class="text-slate-500">Domisili</span>
                            <span id="ticket-domisili" class="font-semibold text-slate-800">-</span>
                        </div>
                        <div class="flex justify-between py-1 border-b border-slate-100">
                            <span class="text-slate-500">Pendidikan Terakhir</span>
                            <span id="ticket-pendidikan" class="font-semibold text-slate-800">-</span>
                        </div>
                        <div class="flex justify-between py-1">
                            <span class="text-slate-500">Pengalaman Kerja</span>
                            <span id="ticket-pengalaman" class="font-semibold text-slate-800">-</span>
                        </div>
                    </div>

                    <!-- Informative Note -->
                    <div class="bg-amber-50 border border-amber-200 text-amber-800 text-xs rounded-xl p-4 flex gap-3 items-start">
                        <i class="fa-solid fa-circle-info text-amber-600 mt-0.5 text-base"></i>
                        <p class="leading-relaxed">
                            Simpan atau cetak tiket ini sebagai bukti pendaftaran Anda. Tim HRD Toko PutraStore ID akan memproses penilaian dengan metode Simple Additive Weighting (SAW) dan mengumumkan hasil seleksi via WhatsApp.
                        </p>
                    </div>

                </div>

                <!-- Ticket Footer / Actions -->
                <div class="bg-slate-100 px-6 py-4 border-t flex flex-wrap gap-3 justify-between items-center no-print">
                    <button onclick="resetToPublicHome()" class="px-4 py-2 text-slate-600 hover:text-slate-800 text-sm font-semibold flex items-center gap-2">
                        <i class="fa-solid fa-arrow-left"></i>
                        <span>Kembali ke Form</span>
                    </button>
                    <button onclick="window.print()" class="px-5 py-2.5 bg-teal-700 hover:bg-teal-800 text-white rounded-xl font-semibold text-sm shadow-md transition flex items-center gap-2">
                        <i class="fa-solid fa-print"></i>
                        <span>Cetak Struk Tiket</span>
                    </button>
                </div>

            </div>

        </section>


        <!-- ========================================================= -->
        <!-- ADMIN TAB 1: CALON KARYAWAN (Applicant Management)        -->
        <!-- ========================================================= -->
        <section id="view-admin-applicants" class="hidden animate-fade-in space-y-6">
            
            <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-4 bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <i class="fa-solid fa-users text-teal-600"></i>
                        <span>Daftar Data Calon Karyawan</span>
                    </h2>
                    <p class="text-xs text-slate-500 mt-1">
                        Kelola seluruh pendaftar, ubah status kelulusan, atau hapus data pelamar.
                    </p>
                </div>
                <div class="flex flex-wrap items-center gap-3">
                    <div class="relative min-w-[220px]">
                        <input type="text" id="search-applicant" oninput="renderApplicantsTable()" placeholder="Cari nama / HP..."
                            class="w-full pl-9 pr-4 py-2 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500 focus:outline-none">
                        <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-sm"></i>
                    </div>
                    <button onclick="clearAllApplicants()" class="px-3.5 py-2 text-rose-600 hover:bg-rose-50 border border-rose-200 rounded-xl text-xs font-semibold transition flex items-center gap-1.5">
                        <i class="fa-solid fa-trash-can"></i>
                        <span>Reset Data</span>
                    </button>
                </div>
            </div>

            <!-- Table Card -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-800 text-slate-200 text-xs font-bold uppercase tracking-wider border-b">
                                <th class="p-4">No / Reg ID</th>
                                <th class="p-4">Nama Pelamar</th>
                                <th class="p-4">Kontak & Domisili</th>
                                <th class="p-4 text-center">Pendidikan</th>
                                <th class="p-4 text-center">Pengalaman</th>
                                <th class="p-4 text-center">Status</th>
                                <th class="p-4 text-center no-print">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="table-applicants-body" class="divide-y divide-slate-200 text-sm">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

        </section>


        <!-- ========================================================= -->
        <!-- ADMIN TAB 2: KRITERIA SAW MANAGEMENT                     -->
        <!-- ========================================================= -->
        <section id="view-admin-criteria" class="hidden animate-fade-in space-y-6">
            
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 flex flex-col md:flex-row md:items-center justify-between gap-4">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <i class="fa-solid fa-sliders text-teal-600"></i>
                        <span>Pengaturan Kriteria & Bobot SAW</span>
                    </h2>
                    <p class="text-xs text-slate-500 mt-1">
                        Atur kriteria penilaian, tipe (Benefit/Cost), dan bobot kepentingan ($W_j$). Total bobot disarankan $= 1.00$ ($100\%$).
                    </p>
                </div>
                <div id="criteria-total-badge" class="px-4 py-2 rounded-xl text-xs font-bold bg-teal-50 border border-teal-200 text-teal-800">
                    Total Bobot: <span id="sum-weights-display">1.00</span>
                </div>
            </div>

            <!-- Criteria Table Card -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-800 text-slate-200 text-xs font-bold uppercase tracking-wider border-b">
                                <th class="p-4">Kode</th>
                                <th class="p-4">Nama Kriteria</th>
                                <th class="p-4 text-center">Atribut / Tipe</th>
                                <th class="p-4 text-center">Bobot ($W_j$)</th>
                                <th class="p-4 text-center">Persentase</th>
                                <th class="p-4 text-center">Aksi Edit</th>
                            </tr>
                        </thead>
                        <tbody id="table-criteria-body" class="divide-y divide-slate-200 text-sm">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

        </section>


        <!-- ========================================================= -->
        <!-- ADMIN TAB 3: PERHITUNGAN SAW & RANKING                    -->
        <!-- ========================================================= -->
        <section id="view-admin-saw" class="hidden animate-fade-in space-y-8">
            
            <!-- Header section for calculations -->
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 flex flex-col sm:flex-row items-center justify-between gap-4">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <i class="fa-solid fa-calculator text-teal-600"></i>
                        <span>Hasil Perhitungan Simple Additive Weighting (SAW)</span>
                    </h2>
                    <p class="text-xs text-slate-500 mt-1">
                        Metode penjumlahan terbobot dari matriks keputusan yang telah dinormalisasi.
                    </p>
                </div>
                <button onclick="window.print()" class="px-4 py-2 bg-slate-800 hover:bg-slate-900 text-white rounded-xl text-xs font-semibold shadow transition flex items-center gap-2">
                    <i class="fa-solid fa-file-pdf"></i>
                    <span>Cetak Laporan SAW</span>
                </button>
            </div>

            <!-- STEP 1: MATRIKS KEPUTUSAN (X) -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-4">
                <h3 class="text-base font-bold text-slate-800 flex items-center gap-2 border-b pb-3">
                    <span class="w-6 h-6 rounded-full bg-teal-100 text-teal-800 text-xs flex items-center justify-center font-bold">1</span>
                    <span>Matriks Keputusan Pasangan Candidate ($X_{ij}$)</span>
                </h3>
                <div class="overflow-x-auto">
                    <table class="w-full text-sm border-collapse">
                        <thead>
                            <tr class="bg-slate-100 text-slate-700 text-xs font-semibold border-b">
                                <th class="p-3 text-left">Pelamar</th>
                                <th class="p-3 text-center">C1 (Pendidikan)</th>
                                <th class="p-3 text-center">C2 (Pengalaman)</th>
                                <th class="p-3 text-center">C3 (Keterampilan)</th>
                                <th class="p-3 text-center">C4 (Sikap/Disiplin)</th>
                                <th class="p-3 text-center">C5 (Jarak Domisili)</th>
                            </tr>
                        </thead>
                        <tbody id="table-saw-matrix-x" class="divide-y divide-slate-200">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- STEP 2: MATRIKS NORMALISASI (R) -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-4">
                <h3 class="text-base font-bold text-slate-800 flex items-center gap-2 border-b pb-3">
                    <span class="w-6 h-6 rounded-full bg-teal-100 text-teal-800 text-xs flex items-center justify-center font-bold">2</span>
                    <span>Matriks Ternormalisasi ($R_{ij}$)</span>
                </h3>
                <p class="text-xs text-slate-500">
                    Formula Benefit: $r_{ij} = x_{ij} / \max(x_{ij})$ | Formula Cost: $r_{ij} = \min(x_{ij}) / x_{ij}$
                </p>
                <div class="overflow-x-auto">
                    <table class="w-full text-sm border-collapse">
                        <thead>
                            <tr class="bg-slate-100 text-slate-700 text-xs font-semibold border-b">
                                <th class="p-3 text-left">Pelamar</th>
                                <th class="p-3 text-center">R1</th>
                                <th class="p-3 text-center">R2</th>
                                <th class="p-3 text-center">R3</th>
                                <th class="p-3 text-center">R4</th>
                                <th class="p-3 text-center">R5</th>
                            </tr>
                        </thead>
                        <tbody id="table-saw-matrix-r" class="divide-y divide-slate-200 font-mono text-xs">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- STEP 3: HASIL SKOR AKHIR & RANKING (V) -->
            <div class="bg-white rounded-2xl shadow-md border-2 border-teal-500/30 p-6 space-y-4">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3 border-b pb-3">
                    <h3 class="text-lg font-bold text-teal-900 flex items-center gap-2">
                        <span class="w-7 h-7 rounded-full bg-teal-600 text-white text-xs flex items-center justify-center font-bold">3</span>
                        <span>Hasil Akhir Preferensi ($V_i$) & Peringkat Kelulusan</span>
                    </h3>
                    <span class="text-xs px-3 py-1 rounded-full bg-emerald-100 text-emerald-800 font-semibold border border-emerald-300">
                        Top Ranking Merekomendasikan Hasil Terbaik
                    </span>
                </div>

                <!-- INFO BOX ALASAN PRIORITAS -->
                <div class="bg-amber-50 border border-amber-200 text-amber-800 text-sm rounded-xl p-4 flex gap-3 items-start no-print">
                    <i class="fa-solid fa-circle-info text-amber-600 mt-0.5 text-base"></i>
                    <div class="space-y-1">
                        <p class="font-bold text-amber-900">Kenapa pelamar peringkat teratas diprioritaskan?</p>
                        <ul class="list-disc list-inside space-y-1 ml-1 text-amber-800/90 text-xs sm:text-sm">
                            <li><strong>Nilai Preferensi Tertinggi:</strong> Pelamar teratas memiliki skor akumulasi tertinggi ($V_i$) dari perkalian bobot ($W_j$) dan nilai matriks normalisasi ($R_{ij}$).</li>
                            <li><strong>Kriteria Paling Unggul:</strong> Menandakan pelamar paling memenuhi standar kerja dalam segi Pendidikan, Pengalaman, Keterampilan, Kedisiplinan, dan Jarak Domisili.</li>
                            <li><strong>Sistem yang Objektif:</strong> SAW merangking pelamar murni secara matematis sehingga rekomendasi yang diberikan ("Sangat Direkomendasikan") adalah keputusan paling optimal untuk direkrut.</li>
                        </ul>
                    </div>
                </div>
                <!-- END INFO BOX -->
                
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-teal-900 text-white text-xs font-bold uppercase tracking-wider">
                                <th class="p-4 text-center">Peringkat</th>
                                <th class="p-4">Nama Pelamar</th>
                                <th class="p-4">No. HP</th>
                                <th class="p-4 text-center">Nilai Preferensi ($V_i$)</th>
                                <th class="p-4 text-center">Rekomendasi System</th>
                                <th class="p-4 text-center no-print">Aksi WhatsApp</th>
                            </tr>
                        </thead>
                        <tbody id="table-saw-ranking" class="divide-y divide-slate-200 text-sm">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

        </section>


        <!-- ========================================================= -->
        <!-- ADMIN TAB 4: PENGATURAN (Settings & Password Management) -->
        <!-- ========================================================= -->
        <section id="view-admin-settings" class="hidden animate-fade-in space-y-6 max-w-4xl mx-auto">
            
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                    <i class="fa-solid fa-gear text-teal-600"></i>
                    <span>Pengaturan Sistem Admin</span>
                </h2>
                <p class="text-xs text-slate-500 mt-1">
                    Atur visibilitas menu admin dan ubah password PIN akses admin.
                </p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Card 1: Pengaturan Password Admin -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-5">
                    <h3 class="text-lg font-bold text-slate-900 border-b pb-3 flex items-center gap-2">
                        <i class="fa-solid fa-key text-amber-500"></i>
                        <span>Ubah Password / PIN Admin</span>
                    </h3>

                    <form onsubmit="handleChangePassword(event)" class="space-y-4">
                        <div>
                            <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">
                                PIN Saat Ini <span class="text-rose-500">*</span>
                            </label>
                            <input type="password" id="change-pin-current" required placeholder="Masukkan PIN saat ini"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500 focus:outline-none">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">
                                PIN Baru <span class="text-rose-500">*</span>
                            </label>
                            <input type="password" id="change-pin-new" required placeholder="Masukkan PIN baru"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500 focus:outline-none">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">
                                Konfirmasi PIN Baru <span class="text-rose-500">*</span>
                            </label>
                            <input type="password" id="change-pin-confirm" required placeholder="Ulangi PIN baru"
                                class="w-full px-4 py-2.5 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500 focus:outline-none">
                        </div>

                        <div id="msg-password-status" class="hidden text-xs p-3 rounded-xl font-semibold"></div>

                        <button type="submit" class="w-full py-2.5 bg-teal-700 hover:bg-teal-800 text-white font-bold rounded-xl shadow transition flex items-center justify-center gap-2 text-sm">
                            <i class="fa-solid fa-floppy-disk"></i>
                            <span>Simpan PIN Baru</span>
                        </button>
                    </form>
                </div>

                <!-- Card 2: Pengaturan Menu Admin -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-5">
                    <h3 class="text-lg font-bold text-slate-900 border-b pb-3 flex items-center gap-2">
                        <i class="fa-solid fa-bars-staggered text-teal-600"></i>
                        <span>Pengaturan Kelola Menu Admin</span>
                    </h3>

                    <p class="text-xs text-slate-500">
                        Aktifkan atau nonaktifkan menu yang ingin ditampilkan pada navigasi Admin.
                    </p>

                    <div class="space-y-4">
                        <div class="flex items-center justify-between p-3.5 rounded-xl bg-slate-50 border border-slate-200">
                            <div class="flex items-center gap-3">
                                <i class="fa-solid fa-users text-teal-600 text-lg"></i>
                                <div>
                                    <div class="font-bold text-sm text-slate-800">Calon Karyawan</div>
                                    <div class="text-xs text-slate-500">Menu kelola data pendaftar</div>
                                </div>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="toggle-menu-applicants" class="sr-only peer" onchange="handleSaveMenuSettings()">
                                <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-teal-600"></div>
                            </label>
                        </div>

                        <div class="flex items-center justify-between p-3.5 rounded-xl bg-slate-50 border border-slate-200">
                            <div class="flex items-center gap-3">
                                <i class="fa-solid fa-sliders text-teal-600 text-lg"></i>
                                <div>
                                    <div class="font-bold text-sm text-slate-800">Kriteria SAW</div>
                                    <div class="text-xs text-slate-500">Menu bobot & kriteria penilaian</div>
                                </div>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="toggle-menu-criteria" class="sr-only peer" onchange="handleSaveMenuSettings()">
                                <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-teal-600"></div>
                            </label>
                        </div>

                        <div class="flex items-center justify-between p-3.5 rounded-xl bg-slate-50 border border-slate-200">
                            <div class="flex items-center gap-3">
                                <i class="fa-solid fa-calculator text-teal-600 text-lg"></i>
                                <div>
                                    <div class="font-bold text-sm text-slate-800">Perhitungan SAW</div>
                                    <div class="text-xs text-slate-500">Menu matriks & perangkingan SAW</div>
                                </div>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="toggle-menu-saw" class="sr-only peer" onchange="handleSaveMenuSettings()">
                                <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-teal-600"></div>
                            </label>
                        </div>

                        <div class="flex items-center justify-between p-3.5 rounded-xl bg-slate-50 border border-slate-200">
                            <div class="flex items-center gap-3">
                                <i class="fa-solid fa-user-plus text-teal-600 text-lg"></i>
                                <div>
                                    <div class="font-bold text-sm text-slate-800">Tambah Manual</div>
                                    <div class="text-xs text-slate-500">Menu pendaftaran pendaftar baru</div>
                                </div>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="toggle-menu-add_applicant" class="sr-only peer" onchange="handleSaveMenuSettings()">
                                <div class="w-11 h-6 bg-slate-300 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-teal-600"></div>
                            </label>
                        </div>
                    </div>

                    <div id="msg-menu-status" class="hidden text-xs p-3 rounded-xl font-semibold bg-emerald-50 text-emerald-800 border border-emerald-200">
                        Pengaturan menu berhasil disimpan!
                    </div>
                </div>
            </div>

        </section>

    </main>


    <!-- ========================================================= -->
    <!-- MODAL: ADMIN LOGIN PIN                                    -->
    <!-- ========================================================= -->
    <div id="modal-login" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4 animate-fade-in">
        <div class="bg-white rounded-3xl shadow-2xl border border-slate-200 max-w-sm w-full p-6 sm:p-8 relative">
            
            <button onclick="closeLoginModal()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600 transition">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>

            <div class="text-center space-y-3">
                <div class="w-14 h-14 rounded-2xl bg-teal-100 text-teal-700 flex items-center justify-center mx-auto shadow-sm">
                    <i class="fa-solid fa-user-shield text-2xl"></i>
                </div>
                <h3 class="text-xl font-extrabold text-slate-900">Autentikasi Admin</h3>
                <p class="text-xs text-slate-500">Masukkan PIN Admin untuk mengakses panel kelola & perhitungan SAW.</p>
            </div>

            <form onsubmit="handleAdminLogin(event)" class="mt-6 space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1">
                        PIN Akses Admin
                    </label>
                    <input type="password" id="input-admin-pin" required maxlength="10" placeholder="Masukan Password Admin" autofocus
                        class="w-full px-4 py-3 rounded-xl border border-slate-300 text-center font-mono text-lg tracking-widest focus:ring-2 focus:ring-teal-500 focus:outline-none">
                    <p class="text-[11px] text-teal-600 mt-1.5 text-center font-medium">
                    </p>
                </div>

                <div id="login-error-msg" class="hidden text-xs text-rose-600 bg-rose-50 border border-rose-200 rounded-lg p-2.5 text-center font-semibold">
                    PIN Salah! Silakan coba lagi.
                </div>

                <button type="submit" class="w-full py-3 bg-teal-700 hover:bg-teal-800 text-white font-bold rounded-xl shadow-md transition">
                    Masuk Mode Admin
                </button>
            </form>

        </div>
    </div>


    <!-- MODAL: EDIT CRITERIA WEIGHT -->
    <div id="modal-edit-criteria" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl shadow-2xl border border-slate-200 max-w-md w-full p-6 space-y-5">
            <div class="flex justify-between items-center border-b pb-3">
                <h3 class="font-bold text-slate-800 text-lg flex items-center gap-2">
                    <i class="fa-solid fa-pen-to-square text-teal-600"></i>
                    <span>Edit Kriteria & Bobot SAW</span>
                </h3>
                <button onclick="closeEditCriteriaModal()" class="text-slate-400 hover:text-slate-600">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <form onsubmit="handleSaveCriteria(event)" class="space-y-4">
                <input type="hidden" id="edit-crit-id">
                
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Nama Kriteria</label>
                    <input type="text" id="edit-crit-name" required class="w-full px-3.5 py-2 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500">
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Tipe Atribut</label>
                    <select id="edit-crit-type" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500">
                        <option value="benefit">Benefit (Makin besar makin bagus)</option>
                        <option value="cost">Cost (Makin kecil makin bagus)</option>
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Bobot ($W_j$) (Skala 0.05 - 0.50)</label>
                    <input type="number" step="0.01" min="0.01" max="1.0" id="edit-crit-weight" required class="w-full px-3.5 py-2 rounded-xl border border-slate-300 text-sm focus:ring-2 focus:ring-teal-500">
                </div>

                <div class="pt-3 flex gap-2 justify-end">
                    <button type="button" onclick="closeEditCriteriaModal()" class="px-4 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-xl text-sm font-semibold">
                        Batal
                    </button>
                    <button type="submit" class="px-5 py-2 bg-teal-700 hover:bg-teal-800 text-white rounded-xl text-sm font-bold shadow">
                        Simpan Perubahan
                    </button>
                </div>
            </form>
        </div>
    </div>


    <!-- ========================================================= -->
    <!-- JAVASCRIPT APP LOGIC & STATE                              -->
    <!-- ========================================================= -->
    <script>
        // DEFAULT INITIAL DATA
        const DEFAULT_CRITERIA = [
            { id: 'c1', code: 'C1', name: 'Pendidikan Terakhir', type: 'benefit', weight: 0.20 },
            { id: 'c2', code: 'C2', name: 'Pengalaman Kerja', type: 'benefit', weight: 0.25 },
            { id: 'c3', code: 'C3', name: 'Keterampilan Ritel/Kasir', type: 'benefit', weight: 0.25 },
            { id: 'c4', code: 'C4', name: 'Kesiapan & Sikap Kerja', type: 'benefit', weight: 0.15 },
            { id: 'c5', code: 'C5', name: 'Jarak Domisili', type: 'benefit', weight: 0.15 }
        ];

        const INITIAL_MOCK_APPLICANTS = [
            {
                id: 'PUTRA-2026-0001',
                nama: 'Dian Bria',
                phone: '081298765432',
                umur: 23,
                domisili: 'Kupang Barat',
                pendidikanLabel: 'Sarjana (S1)',
                pengalamanLabel: '1 - 2 Tahun',
                c1: 4, c2: 3, c3: 4, c4: 5, c5: 4,
                status: 'Lolos Seleksi',
                date: '28 Sep 2026'
            },
            {
                id: 'PUTRA-2026-0002',
                nama: 'Sucitra',
                phone: '085311223344',
                umur: 21,
                domisili: 'Oebobo',
                pendidikanLabel: 'SMA / SMK Sederajat',
                pengalamanLabel: '3 - 4 Tahun',
                c1: 2, c2: 4, c3: 5, c4: 4, c5: 5,
                status: 'Lolos Seleksi',
                date: '28 Sep 2026'
            },
            {
                id: 'PUTRA-2026-0003',
                nama: 'Trivena Menoh',
                phone: '087899001122',
                umur: 25,
                domisili: 'Maulafa',
                pendidikanLabel: 'Diploma (D3)',
                pengalamanLabel: 'Belum Ada Pengalaman',
                c1: 3, c2: 1, c3: 3, c4: 3, c5: 3,
                status: 'Menunggu',
                date: '28 Sep 2026'
            }
        ];

        // GLOBAL APP STATE
        let isAdmin = false;
        let activeAdminTab = 'applicants'; // 'applicants', 'criteria', 'saw', 'add_applicant', 'settings'
        let criteria = JSON.parse(localStorage.getItem('putra_saw_criteria')) || DEFAULT_CRITERIA;
        let applicants = JSON.parse(localStorage.getItem('putra_saw_applicants')) || INITIAL_MOCK_APPLICANTS;
        let adminPin = localStorage.getItem('putra_saw_pin') || '1234';
        let menuSettings = JSON.parse(localStorage.getItem('putra_saw_menu_settings')) || {
            applicants: true,
            criteria: true,
            saw: true,
            add_applicant: true
        };

        // INITIALIZE APP
        window.addEventListener('DOMContentLoaded', () => {
            saveToLocalStorage();
            updateUIState();
        });

        function saveToLocalStorage() {
            localStorage.setItem('putra_saw_criteria', JSON.stringify(criteria));
            localStorage.setItem('putra_saw_applicants', JSON.stringify(applicants));
            localStorage.setItem('putra_saw_pin', adminPin);
            localStorage.setItem('putra_saw_menu_settings', JSON.stringify(menuSettings));
        }

        // CONTROL VISIBILITY ACCORDING TO USER ROLE & MENU SETTINGS
        function updateUIState() {
            const adminNavBar = document.getElementById('admin-nav-bar');
            const authContainer = document.getElementById('auth-button-container');
            const modeBadge = document.getElementById('mode-badge');
            const modeText = document.getElementById('mode-text');
            const modeDot = document.getElementById('mode-dot');

            // Hide all sections first
            document.getElementById('view-public-form').classList.add('hidden');
            document.getElementById('view-public-ticket').classList.add('hidden');
            document.getElementById('view-admin-applicants').classList.add('hidden');
            document.getElementById('view-admin-criteria').classList.add('hidden');
            document.getElementById('view-admin-saw').classList.add('hidden');
            document.getElementById('view-admin-settings').classList.add('hidden');

            // Manage menu tab visibilities based on menuSettings
            const btnApplicants = document.getElementById('tab-btn-applicants');
            const btnCriteria = document.getElementById('tab-btn-criteria');
            const btnSaw = document.getElementById('tab-btn-saw');
            const btnAddApplicant = document.getElementById('tab-btn-add_applicant');

            if (btnApplicants) btnApplicants.style.display = menuSettings.applicants ? 'inline-flex' : 'none';
            if (btnCriteria) btnCriteria.style.display = menuSettings.criteria ? 'inline-flex' : 'none';
            if (btnSaw) btnSaw.style.display = menuSettings.saw ? 'inline-flex' : 'none';
            if (btnAddApplicant) btnAddApplicant.style.display = menuSettings.add_applicant ? 'inline-flex' : 'none';

            if (!isAdmin) {
                // PUBLIC MODE
                adminNavBar.classList.add('hidden'); // CRITICAL: Completely hide admin tabs
                
                modeText.innerText = 'Mode Publik';
                modeDot.className = 'w-2.5 h-2.5 rounded-full bg-emerald-400 animate-pulse';
                modeBadge.className = 'hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-full text-xs font-semibold bg-white/10 border border-white/20 text-teal-100';

                authContainer.innerHTML = `
                    <button onclick="openLoginModal()" class="px-4 py-2 bg-amber-500 hover:bg-amber-600 active:bg-amber-700 text-slate-950 font-semibold text-sm rounded-xl shadow-md hover:shadow-lg transition flex items-center gap-2 border border-amber-300">
                        <i class="fa-solid fa-key text-xs"></i>
                        <span>Login Admin</span>
                    </button>
                `;

                // Show default public form view
                document.getElementById('view-public-form').classList.remove('hidden');

            } else {
                // ADMIN MODE
                adminNavBar.classList.remove('hidden'); // Show navigation tabs bar for admin
                
                modeText.innerText = 'Mode Admin Logged In';
                modeDot.className = 'w-2.5 h-2.5 rounded-full bg-amber-400 animate-ping';
                modeBadge.className = 'hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-full text-xs font-semibold bg-amber-500/20 border border-amber-400/40 text-amber-200';

                authContainer.innerHTML = `
                    <button onclick="handleAdminLogout()" class="px-4 py-2 bg-rose-600 hover:bg-rose-700 active:bg-rose-800 text-white font-semibold text-sm rounded-xl shadow-md transition flex items-center gap-2 border border-rose-400">
                        <i class="fa-solid fa-right-from-bracket text-xs"></i>
                        <span>Keluar Admin</span>
                    </button>
                `;

                // Update active admin tab buttons
                document.querySelectorAll('.tab-btn').forEach(btn => {
                    btn.classList.remove('bg-white/20', 'text-white', 'shadow-sm');
                    btn.classList.add('text-white/80');
                });

                const activeBtn = document.getElementById(`tab-btn-${activeAdminTab}`);
                if (activeBtn) {
                    activeBtn.classList.add('bg-white/20', 'text-white', 'shadow-sm');
                    activeBtn.classList.remove('text-white/80');
                }

                // Render specific admin tab view
                if (activeAdminTab === 'applicants') {
                    document.getElementById('view-admin-applicants').classList.remove('hidden');
                    renderApplicantsTable();
                } else if (activeAdminTab === 'criteria') {
                    document.getElementById('view-admin-criteria').classList.remove('hidden');
                    renderCriteriaTable();
                } else if (activeAdminTab === 'saw') {
                    document.getElementById('view-admin-saw').classList.remove('hidden');
                    renderSAWCalculations();
                } else if (activeAdminTab === 'add_applicant') {
                    document.getElementById('view-public-form').classList.remove('hidden');
                } else if (activeAdminTab === 'settings') {
                    document.getElementById('view-admin-settings').classList.remove('hidden');
                    renderSettingsView();
                }
            }

            // Update badge count
            document.getElementById('applicant-count-badge').innerText = applicants.length;
        }

        // AUTHENTICATION FUNCTIONS
        function openLoginModal() {
            document.getElementById('input-admin-pin').value = '';
            document.getElementById('login-error-msg').classList.add('hidden');
            document.getElementById('modal-login').classList.remove('hidden');
        }

        function closeLoginModal() {
            document.getElementById('modal-login').classList.add('hidden');
        }

        function handleAdminLogin(event) {
            event.preventDefault();
            const pin = document.getElementById('input-admin-pin').value;
            if (pin === adminPin) {
                isAdmin = true;
                activeAdminTab = 'applicants';
                closeLoginModal();
                updateUIState();
            } else {
                document.getElementById('login-error-msg').classList.remove('hidden');
            }
        }

        function handleAdminLogout() {
            isAdmin = false;
            updateUIState();
        }

        function switchAdminTab(tabName) {
            if (!isAdmin) return;
            activeAdminTab = tabName;
            updateUIState();
        }

        function resetToPublicHome() {
            document.getElementById('view-public-ticket').classList.add('hidden');
            if (isAdmin) {
                switchAdminTab('applicants');
            } else {
                updateUIState();
            }
        }

        // REGISTRATION FORM SUBMISSION
        function handleRegistrationSubmit(event) {
            event.preventDefault();

            const nama = document.getElementById('reg-nama').value.trim();
            const phone = document.getElementById('reg-phone').value.trim();
            const umur = parseInt(document.getElementById('reg-umur').value);
            const domisili = document.getElementById('reg-domisili').value.trim();

            const c1Select = document.getElementById('reg-c1');
            const c2Select = document.getElementById('reg-c2');
            const c3Select = document.getElementById('reg-c3');
            const c4Select = document.getElementById('reg-c4');
            const c5Select = document.getElementById('reg-c5');

            const c1 = parseInt(c1Select.value);
            const c2 = parseInt(c2Select.value);
            const c3 = parseInt(c3Select.value);
            const c4 = parseInt(c4Select.value);
            const c5 = parseInt(c5Select.value);

            const regId = 'PUTRA-' + new Date().getFullYear() + '-' + String(Math.floor(1000 + Math.random() * 9000));
            const dateStr = new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'short', year: 'numeric' });

            const newApplicant = {
                id: regId,
                nama: nama,
                phone: phone,
                umur: umur,
                domisili: domisili,
                pendidikanLabel: c1Select.options[c1Select.selectedIndex].text.split('(')[0].trim(),
                pengalamanLabel: c2Select.options[c2Select.selectedIndex].text.split('(')[0].trim(),
                c1: c1,
                c2: c2,
                c3: c3,
                c4: c4,
                c5: c5,
                status: 'Menunggu',
                date: dateStr
            };

            applicants.unshift(newApplicant);
            saveToLocalStorage();

            // Populate Ticket Details
            document.getElementById('ticket-id').innerText = newApplicant.id;
            document.getElementById('ticket-date').innerText = newApplicant.date;
            document.getElementById('ticket-nama').innerText = newApplicant.nama;
            document.getElementById('ticket-phone').innerText = newApplicant.phone;
            document.getElementById('ticket-domisili').innerText = newApplicant.domisili;
            document.getElementById('ticket-pendidikan').innerText = newApplicant.pendidikanLabel;
            document.getElementById('ticket-pengalaman').innerText = newApplicant.pengalamanLabel;

            // Reset Form fields
            document.getElementById('form-registration').reset();

            // Switch to ticket view
            document.getElementById('view-public-form').classList.add('hidden');
            document.getElementById('view-public-ticket').classList.remove('hidden');

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // ADMIN: APPLICANTS TABLE RENDER
        function renderApplicantsTable() {
            const tbody = document.getElementById('table-applicants-body');
            const search = document.getElementById('search-applicant').value.toLowerCase();

            const filtered = applicants.filter(a => 
                a.nama.toLowerCase().includes(search) || 
                a.phone.includes(search) || 
                a.id.toLowerCase().includes(search)
            );

            if (filtered.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="7" class="p-8 text-center text-slate-400">
                            <i class="fa-solid fa-folder-open text-3xl mb-2 block"></i>
                            Belum ada data calon karyawan yang sesuai.
                        </td>
                    </tr>
                `;
                return;
            }

            tbody.innerHTML = filtered.map((a, index) => {
                let statusBadge = '';
                if (a.status === 'Lolos Seleksi') {
                    statusBadge = `<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800 border border-emerald-200">Lolos</span>`;
                } else if (a.status === 'Tidak Lolos') {
                    statusBadge = `<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-rose-100 text-rose-800 border border-rose-200">Tidak Lolos</span>`;
                } else {
                    statusBadge = `<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-amber-100 text-amber-800 border border-amber-200">Menunggu</span>`;
                }

                return `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="p-4 font-mono font-semibold text-xs text-slate-600">
                            ${a.id}<br>
                            <span class="text-[10px] text-slate-400 font-sans">${a.date}</span>
                        </td>
                        <td class="p-4 font-bold text-slate-900">${escapeHtml(a.nama)}</td>
                        <td class="p-4">
                            <div class="text-xs text-slate-700"><i class="fa-brands fa-whatsapp text-emerald-600 mr-1"></i>${escapeHtml(a.phone)}</div>
                            <div class="text-xs text-slate-400"><i class="fa-solid fa-location-dot mr-1"></i>${escapeHtml(a.domisili)}</div>
                        </td>
                        <td class="p-4 text-center text-xs font-medium text-slate-700">${escapeHtml(a.pendidikanLabel)}</td>
                        <td class="p-4 text-center text-xs text-slate-600">${escapeHtml(a.pengalamanLabel)}</td>
                        <td class="p-4 text-center">${statusBadge}</td>
                        <td class="p-4 text-center no-print">
                            <div class="flex items-center justify-center gap-1.5">
                                <button onclick="toggleStatus('${a.id}')" title="Ubah Status" class="p-1.5 text-teal-700 hover:bg-teal-50 rounded-lg transition">
                                    <i class="fa-solid fa-rotate"></i>
                                </button>
                                <button onclick="deleteApplicant('${a.id}')" title="Hapus Data" class="p-1.5 text-rose-600 hover:bg-rose-50 rounded-lg transition">
                                    <i class="fa-solid fa-trash"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        function toggleStatus(id) {
            const app = applicants.find(a => a.id === id);
            if (app) {
                if (app.status === 'Menunggu') app.status = 'Lolos Seleksi';
                else if (app.status === 'Lolos Seleksi') app.status = 'Tidak Lolos';
                else app.status = 'Menunggu';
                saveToLocalStorage();
                renderApplicantsTable();
            }
        }

        function deleteApplicant(id) {
            if (confirm('Apakah Anda yakin ingin menghapus data calon karyawan ini?')) {
                applicants = applicants.filter(a => a.id !== id);
                saveToLocalStorage();
                renderApplicantsTable();
                document.getElementById('applicant-count-badge').innerText = applicants.length;
            }
        }

        function clearAllApplicants() {
            if (confirm('PERINGATAN: Apakah Anda yakin ingin menghapus SEMUA data pendaftar?')) {
                applicants = [];
                saveToLocalStorage();
                renderApplicantsTable();
                document.getElementById('applicant-count-badge').innerText = 0;
            }
        }

        // ADMIN: CRITERIA TABLE RENDER & EDIT
        function renderCriteriaTable() {
            const tbody = document.getElementById('table-criteria-body');
            let sumWeight = 0;

            tbody.innerHTML = criteria.map(c => {
                sumWeight += parseFloat(c.weight);
                const pct = (parseFloat(c.weight) * 100).toFixed(0);
                const badgeType = c.type === 'benefit' 
                    ? `<span class="px-2.5 py-1 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-800">Benefit</span>`
                    : `<span class="px-2.5 py-1 rounded-full text-xs font-semibold bg-amber-100 text-amber-800">Cost</span>`;

                return `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="p-4 font-mono font-bold text-teal-800">${c.code}</td>
                        <td class="p-4 font-semibold text-slate-800">${c.name}</td>
                        <td class="p-4 text-center">${badgeType}</td>
                        <td class="p-4 text-center font-mono font-bold text-slate-700">${parseFloat(c.weight).toFixed(2)}</td>
                        <td class="p-4 text-center font-semibold text-slate-600">${pct}%</td>
                        <td class="p-4 text-center">
                            <button onclick="openEditCriteriaModal('${c.id}')" class="px-3 py-1.5 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-lg text-xs font-semibold transition">
                                <i class="fa-solid fa-pen-to-square mr-1"></i> Edit
                            </button>
                        </td>
                    </tr>
                `;
            }).join('');

            document.getElementById('sum-weights-display').innerText = sumWeight.toFixed(2);
        }

        function openEditCriteriaModal(id) {
            const crit = criteria.find(c => c.id === id);
            if (!crit) return;
            document.getElementById('edit-crit-id').value = crit.id;
            document.getElementById('edit-crit-name').value = crit.name;
            document.getElementById('edit-crit-type').value = crit.type;
            document.getElementById('edit-crit-weight').value = crit.weight;
            document.getElementById('modal-edit-criteria').classList.remove('hidden');
        }

        function closeEditCriteriaModal() {
            document.getElementById('modal-edit-criteria').classList.add('hidden');
        }

        function handleSaveCriteria(event) {
            event.preventDefault();
            const id = document.getElementById('edit-crit-id').value;
            const name = document.getElementById('edit-crit-name').value.trim();
            const type = document.getElementById('edit-crit-type').value;
            const weight = parseFloat(document.getElementById('edit-crit-weight').value);

            const crit = criteria.find(c => c.id === id);
            if (crit) {
                crit.name = name;
                crit.type = type;
                crit.weight = weight;
                saveToLocalStorage();
                renderCriteriaTable();
                closeEditCriteriaModal();
            }
        }

        // ADMIN: SAW MATHEMATICAL CALCULATION ENGINE
        function renderSAWCalculations() {
            const tableX = document.getElementById('table-saw-matrix-x');
            const tableR = document.getElementById('table-saw-matrix-r');
            const tableRanking = document.getElementById('table-saw-ranking');

            if (applicants.length === 0) {
                const emptyRow = `<tr><td colspan="6" class="p-6 text-center text-slate-400">Belum ada data pelamar untuk dihitung.</td></tr>`;
                tableX.innerHTML = emptyRow;
                tableR.innerHTML = emptyRow;
                tableRanking.innerHTML = `<tr><td colspan="6" class="p-6 text-center text-slate-400">Belum ada data pelamar.</td></tr>`;
                return;
            }

            // 1. Render Matriks Keputusan (X)
            tableX.innerHTML = applicants.map(a => `
                <tr class="hover:bg-slate-50">
                    <td class="p-3 font-semibold text-slate-800">${escapeHtml(a.nama)}</td>
                    <td class="p-3 text-center font-mono">${a.c1}</td>
                    <td class="p-3 text-center font-mono">${a.c2}</td>
                    <td class="p-3 text-center font-mono">${a.c3}</td>
                    <td class="p-3 text-center font-mono">${a.c4}</td>
                    <td class="p-3 text-center font-mono">${a.c5}</td>
                </tr>
            `).join('');

            // Calculate Max & Min for Each Criteria
            const cMax = {
                c1: Math.max(...applicants.map(a => a.c1)),
                c2: Math.max(...applicants.map(a => a.c2)),
                c3: Math.max(...applicants.map(a => a.c3)),
                c4: Math.max(...applicants.map(a => a.c4)),
                c5: Math.max(...applicants.map(a => a.c5)),
            };

            const cMin = {
                c1: Math.min(...applicants.map(a => a.c1)),
                c2: Math.min(...applicants.map(a => a.c2)),
                c3: Math.min(...applicants.map(a => a.c3)),
                c4: Math.min(...applicants.map(a => a.c4)),
                c5: Math.min(...applicants.map(a => a.c5)),
            };

            // 2. Compute Normalization Matrix R & Final Score V
            const normalizedApplicants = applicants.map(a => {
                const getCritType = (code) => {
                    const found = criteria.find(c => c.code.toLowerCase() === code);
                    return found ? found.type : 'benefit';
                };

                const r1 = getCritType('c1') === 'benefit' ? (a.c1 / cMax.c1) : (cMin.c1 / a.c1);
                const r2 = getCritType('c2') === 'benefit' ? (a.c2 / cMax.c2) : (cMin.c2 / a.c2);
                const r3 = getCritType('c3') === 'benefit' ? (a.c3 / cMax.c3) : (cMin.c3 / a.c3);
                const r4 = getCritType('c4') === 'benefit' ? (a.c4 / cMax.c4) : (cMin.c4 / a.c4);
                const r5 = getCritType('c5') === 'benefit' ? (a.c5 / cMax.c5) : (cMin.c5 / a.c5);

                const w1 = criteria[0] ? criteria[0].weight : 0.20;
                const w2 = criteria[1] ? criteria[1].weight : 0.25;
                const w3 = criteria[2] ? criteria[2].weight : 0.25;
                const w4 = criteria[3] ? criteria[3].weight : 0.15;
                const w5 = criteria[4] ? criteria[4].weight : 0.15;

                const scoreV = (r1 * w1) + (r2 * w2) + (r3 * w3) + (r4 * w4) + (r5 * w5);

                return {
                    ...a,
                    r1, r2, r3, r4, r5,
                    scoreV
                };
            });

            // Render Matriks Ternormalisasi (R)
            tableR.innerHTML = normalizedApplicants.map(a => `
                <tr class="hover:bg-slate-50">
                    <td class="p-3 font-semibold text-slate-800 font-sans">${escapeHtml(a.nama)}</td>
                    <td class="p-3 text-center">${a.r1.toFixed(3)}</td>
                    <td class="p-3 text-center">${a.r2.toFixed(3)}</td>
                    <td class="p-3 text-center">${a.r3.toFixed(3)}</td>
                    <td class="p-3 text-center">${a.r4.toFixed(3)}</td>
                    <td class="p-3 text-center">${a.r5.toFixed(3)}</td>
                </tr>
            `).join('');

            // Sort descending by score V
            const ranked = [...normalizedApplicants].sort((a, b) => b.scoreV - a.scoreV);

            // Render Final SAW Ranking Table
            tableRanking.innerHTML = ranked.map((a, idx) => {
                const rankNum = idx + 1;
                let rankBadge = '';
                if (rankNum === 1) {
                    rankBadge = `<span class="w-8 h-8 rounded-full bg-amber-400 text-slate-900 font-black flex items-center justify-center mx-auto shadow"><i class="fa-solid fa-trophy text-xs"></i></span>`;
                } else if (rankNum === 2) {
                    rankBadge = `<span class="w-8 h-8 rounded-full bg-slate-300 text-slate-900 font-black flex items-center justify-center mx-auto">2</span>`;
                } else if (rankNum === 3) {
                    rankBadge = `<span class="w-8 h-8 rounded-full bg-amber-700 text-white font-black flex items-center justify-center mx-auto">3</span>`;
                } else {
                    rankBadge = `<span class="w-8 h-8 rounded-full bg-slate-100 text-slate-700 font-bold flex items-center justify-center mx-auto">${rankNum}</span>`;
                }

                const isRecommended = rankNum <= 2;
                const recBadge = isRecommended 
                    ? `<span class="px-3 py-1 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800 border border-emerald-300"><i class="fa-solid fa-check-double mr-1"></i>Sangat Direkomendasikan</span>`
                    : `<span class="px-3 py-1 rounded-full text-xs font-medium bg-slate-100 text-slate-600">Pertimbangan Cadangan</span>`;

                // Format nomor HP ke 62...
                let cleanPhone = (a.phone || '').replace(/[^0-9]/g, '');
                if (cleanPhone.startsWith('0')) {
                    cleanPhone = '62' + cleanPhone.slice(1);
                }

                const msgLolos = encodeURIComponent(`Halo ${a.nama}, selamat! Anda dinyatakan LOLOS seleksi penerimaan karyawan di Toko PutraStore ID berdasarkan hasil penilaian sistem SAW (Nilai Preferensi: ${a.scoreV.toFixed(4)}). Silakan hubungi kami untuk informasi tahap selanjutnya.`);
                const msgGagal = encodeURIComponent(`Halo ${a.nama}, mohon maaf Anda dinyatakan TIDAK LOLOS seleksi penerimaan karyawan di Toko PutraStore ID. Terima kasih banyak atas partisipasi Anda.`);

                const waLolosUrl = `https://wa.me/${cleanPhone}?text=${msgLolos}`;
                const waGagalUrl = `https://wa.me/${cleanPhone}?text=${msgGagal}`;

                const waActionButtons = `
                    <div class="flex flex-col sm:flex-row items-center justify-center gap-1.5 no-print">
                        <a href="${waLolosUrl}" target="_blank" class="px-2.5 py-1 bg-emerald-600 hover:bg-emerald-700 text-white rounded-lg text-xs font-semibold shadow-sm transition flex items-center gap-1">
                            <i class="fa-brands fa-whatsapp text-xs"></i> Lolos
                        </a>
                        <a href="${waGagalUrl}" target="_blank" class="px-2.5 py-1 bg-rose-600 hover:bg-rose-700 text-white rounded-lg text-xs font-semibold shadow-sm transition flex items-center gap-1">
                            <i class="fa-brands fa-whatsapp text-xs"></i> Tidak Lolos
                        </a>
                    </div>
                `;

                return `
                    <tr class="hover:bg-teal-50/50 transition">
                        <td class="p-4 text-center">${rankBadge}</td>
                        <td class="p-4 font-bold text-slate-900">${escapeHtml(a.nama)}</td>
                        <td class="p-4 text-xs text-slate-600">${escapeHtml(a.phone)}</td>
                        <td class="p-4 text-center font-mono font-extrabold text-base text-teal-800">${a.scoreV.toFixed(4)}</td>
                        <td class="p-4 text-center">${recBadge}</td>
                        <td class="p-4 text-center">${waActionButtons}</td>
                    </tr>
                `;
            }).join('');
        }

        // ADMIN: SETTINGS HANDLERS
        function renderSettingsView() {
            document.getElementById('toggle-menu-applicants').checked = menuSettings.applicants;
            document.getElementById('toggle-menu-criteria').checked = menuSettings.criteria;
            document.getElementById('toggle-menu-saw').checked = menuSettings.saw;
            document.getElementById('toggle-menu-add_applicant').checked = menuSettings.add_applicant;
        }

        function handleSaveMenuSettings() {
            menuSettings.applicants = document.getElementById('toggle-menu-applicants').checked;
            menuSettings.criteria = document.getElementById('toggle-menu-criteria').checked;
            menuSettings.saw = document.getElementById('toggle-menu-saw').checked;
            menuSettings.add_applicant = document.getElementById('toggle-menu-add_applicant').checked;

            saveToLocalStorage();
            updateUIState();

            const msg = document.getElementById('msg-menu-status');
            msg.innerText = "Pengaturan menu berhasil diperbarui!";
            msg.classList.remove('hidden');
            setTimeout(() => {
                msg.classList.add('hidden');
            }, 3000);
        }

        function handleChangePassword(event) {
            event.preventDefault();
            const currentPin = document.getElementById('change-pin-current').value;
            const newPin = document.getElementById('change-pin-new').value;
            const confirmPin = document.getElementById('change-pin-confirm').value;
            const statusMsg = document.getElementById('msg-password-status');

            if (currentPin !== adminPin) {
                statusMsg.className = "text-xs p-3 rounded-xl font-semibold bg-rose-50 text-rose-800 border border-rose-200";
                statusMsg.innerText = "PIN saat ini salah!";
                statusMsg.classList.remove('hidden');
                return;
            }

            if (!newPin || newPin.length < 4) {
                statusMsg.className = "text-xs p-3 rounded-xl font-semibold bg-amber-50 text-amber-800 border border-amber-200";
                statusMsg.innerText = "PIN baru minimal harus 4 karakter!";
                statusMsg.classList.remove('hidden');
                return;
            }

            if (newPin !== confirmPin) {
                statusMsg.className = "text-xs p-3 rounded-xl font-semibold bg-rose-50 text-rose-800 border border-rose-200";
                statusMsg.innerText = "Konfirmasi PIN baru tidak cocok!";
                statusMsg.classList.remove('hidden');
                return;
            }

            adminPin = newPin;
            saveToLocalStorage();

            statusMsg.className = "text-xs p-3 rounded-xl font-semibold bg-emerald-50 text-emerald-800 border border-emerald-200";
            statusMsg.innerText = "Password / PIN Admin berhasil diubah!";
            statusMsg.classList.remove('hidden');

            // Reset form
            document.getElementById('change-pin-current').value = '';
            document.getElementById('change-pin-new').value = '';
            document.getElementById('change-pin-confirm').value = '';

            setTimeout(() => {
                statusMsg.classList.add('hidden');
            }, 3000);
        }

        // HELPER UTILITY
        function escapeHtml(text) {
            if (!text) return '';
            return text
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
