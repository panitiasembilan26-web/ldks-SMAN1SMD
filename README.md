<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LDKS Command Center - SMAN 1 Sumedang</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f1f5f9; color: #1e293b; }
        .pulse-red-card { animation: pulseRedCard 1s infinite; }
        @keyframes pulseRedCard {
            0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.5); }
            70% { box-shadow: 0 0 0 18px rgba(239, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
        }
        .pulse-yellow-card { animation: pulseYellowCard 2s infinite; }
        @keyframes pulseYellowCard {
            0% { box-shadow: 0 0 0 0 rgba(234, 179, 8, 0.4); }
            70% { box-shadow: 0 0 0 15px rgba(234, 179, 8, 0); }
            100% { box-shadow: 0 0 0 0 rgba(234, 179, 8, 0); }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-sky-500 selection:text-white">

    <!-- Header Resmi Aksen Biru -->
    <header class="bg-white border-b border-slate-200 px-6 py-3.5 shadow-sm sticky top-0 z-30 flex justify-between items-center">
        <div class="flex items-center space-x-3">
            <div class="flex items-center space-x-2">
                <div class="w-10 h-10 rounded-xl bg-sky-700 flex items-center justify-center text-white font-black text-xs shadow-md border border-sky-800">
                    SMA
                </div>
                <div>
                    <h1 class="font-extrabold text-sm md:text-base text-slate-800 tracking-tight leading-tight">LDKS Command Center</h1>
                    <p class="text-[11px] text-slate-500 font-medium">SMA Negeri 1 Sumedang</p>
                </div>
            </div>
        </div>

        <div class="flex items-center space-x-3">
            <span id="roleBadge" class="px-3 py-1 bg-slate-100 text-slate-600 text-xs font-bold rounded-full border border-slate-200">Mode: Siswa</span>
            <button type="button" onclick="bukaModalLogin()" id="authBtn" class="bg-sky-700 hover:bg-sky-800 active:scale-95 text-white text-xs font-bold px-4 py-2 rounded-xl transition shadow-sm cursor-pointer">Login Panitia</button>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-4xl w-full mx-auto p-4 md:p-6 flex flex-col justify-center gap-6">

        <!-- VIEW 1: FORM LOGIN SISWA -->
        <section id="studentLoginView" class="bg-white border border-slate-200/80 rounded-3xl p-8 md:p-10 max-w-md w-full mx-auto shadow-xl shadow-slate-200/50 flex flex-col gap-6 my-auto">
            <div class="text-center flex flex-col items-center">
                <div class="w-16 h-16 bg-sky-50 text-sky-700 rounded-2xl flex items-center justify-center text-2xl font-black mb-3 border border-sky-100 shadow-inner">
                    🛡️
                </div>
                <h2 class="text-xl md:text-2xl font-black text-slate-900 tracking-tight">Portal Pemantauan Zona</h2>
                <p class="text-xs text-slate-500 mt-1 max-w-xs">Masukkan Kode Unik atau Nama Peserta Anda untuk melihat status pengawasan secara real-time.</p>
            </div>
            
            <div class="flex flex-col gap-4">
                <div>
                    <label class="text-[11px] font-bold text-slate-500 uppercase tracking-wider block mb-2">Kode Unik / Nama Peserta</label>
                    <input type="text" id="studentCodeInput" placeholder="Contoh: 01 atau Ahmad Fauzi" class="w-full bg-slate-50 border border-slate-200 rounded-2xl px-4 py-3.5 text-sm text-slate-800 placeholder-slate-400 focus:outline-none focus:border-sky-600 focus:bg-white transition shadow-sm">
                </div>
                <button type="button" onclick="loginSiswa()" class="w-full bg-sky-700 hover:bg-sky-800 active:scale-[0.98] text-white font-bold py-3.5 rounded-2xl transition shadow-lg shadow-sky-700/20 text-sm cursor-pointer">Masuk Dashboard</button>
            </div>

            <div class="border-t border-slate-100 pt-4 text-center">
                <button type="button" onclick="tarikDataCloudSiswa()" class="text-xs text-sky-700 font-bold hover:underline inline-flex items-center gap-1.5">
                    <span>🔄</span> Refresh Data Dari Sheets
                </button>
            </div>
        </section>

        <!-- VIEW 2: DASHBOARD PERSONAL & GLOBAL -->
        <div id="dashboardView" class="hidden flex flex-col gap-6 w-full">
            
            <!-- Banner Identitas Peserta -->
            <div id="studentIdentityCard" class="hidden bg-white border border-slate-200 rounded-2xl p-4 md:p-5 shadow-sm flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-sky-50 flex items-center justify-center text-sky-700 font-bold text-sm border border-sky-100">
                        👤
                    </div>
                    <div>
                        <p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider">Peserta Aktif</p>
                        <h3 id="currentStudentName" class="text-base font-extrabold text-slate-800">Nama Peserta</h3>
                    </div>
                </div>
                <button type="button" onclick="logoutSiswa()" class="px-3.5 py-2 bg-slate-100 hover:bg-slate-200 text-xs rounded-xl text-slate-700 font-semibold transition cursor-pointer border border-slate-200">Ganti Akun</button>
            </div>

            <!-- Kartu Status Zona Utama -->
            <section id="globalZoneCard" class="rounded-3xl p-8 md:p-12 text-center shadow-xl border transition-all duration-500 bg-white border-slate-200">
                <div class="inline-flex items-center gap-2 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider mb-4 border shadow-xs" id="globalZoneBadgeContainer">
                    <span id="statusIndicatorDot" class="w-2.5 h-2.5 rounded-full"></span>
                    <span id="globalZoneBadgeText">STATUS ZONA ANDA</span>
                </div>
                <h2 id="globalZoneTitle" class="text-3xl md:text-5xl font-black tracking-tight uppercase mb-3 text-slate-900">ZONA AMAN</h2>
                <p id="globalZoneDesc" class="text-slate-600 text-sm md:text-base max-w-lg mx-auto font-medium leading-relaxed">Jangan lengah! Tetap pertahankan kedisiplinan dan tingkatkan konsentrasi hingga akhir.</p>
            </section>

            <!-- Panel Kontrol Panitia -->
            <div id="panitiaPanel" class="hidden bg-white border border-slate-200 rounded-3xl p-6 md:p-8 shadow-xl flex flex-col gap-6">
                <div class="flex flex-wrap justify-between items-center border-b border-slate-100 pb-4 gap-4">
                    <div class="flex items-center space-x-2">
                        <div class="w-8 h-8 rounded-lg bg-sky-50 flex items-center justify-center text-sky-700 font-bold">🛠️</div>
                        <h3 class="font-bold text-slate-800 text-base">Panel Kontrol Panitia</h3>
                    </div>
                    <div class="flex flex-wrap items-center gap-2">
                        <button type="button" onclick="syncToCloud()" id="syncBtn" class="bg-emerald-600 hover:bg-emerald-700 text-white text-xs px-3.5 py-2 rounded-xl font-semibold transition cursor-pointer shadow-sm">☁️ Simpan Ke Sheets</button>
                        <button type="button" onclick="logoutPanitia()" class="bg-rose-50 text-rose-700 hover:bg-rose-100 border border-rose-200 text-xs px-3.5 py-2 rounded-xl font-semibold transition cursor-pointer">Keluar Panitia</button>
                    </div>
                </div>

                <!-- Global Zone Switcher -->
                <div>
                    <label class="text-[11px] font-bold text-slate-400 uppercase tracking-wider block mb-2.5">Ubah Zona Global Layar Utama:</label>
                    <div class="grid grid-cols-3 gap-3">
                        <button type="button" onclick="setGlobalZona('MERAH')" class="bg-rose-50 border border-rose-200 hover:bg-rose-600 hover:text-white text-rose-700 font-bold py-3 rounded-2xl transition text-xs md:text-sm cursor-pointer shadow-xs">🔴 ZONA MERAH</button>
                        <button type="button" onclick="setGlobalZona('KUNING')" class="bg-amber-50 border border-amber-200 hover:bg-amber-500 hover:text-white text-amber-700 font-bold py-3 rounded-2xl transition text-xs md:text-sm cursor-pointer shadow-xs">🟡 ZONA KUNING</button>
                        <button type="button" onclick="setGlobalZona('HIJAU')" class="bg-emerald-50 border border-emerald-200 hover:bg-emerald-600 hover:text-white text-emerald-700 font-bold py-3 rounded-2xl transition text-xs md:text-sm cursor-pointer shadow-xs">🟢 ZONA HIJAU</button>
                    </div>
                </div>

                <!-- Add Peserta Form -->
                <div class="bg-slate-50 p-4 md:p-5 rounded-2xl border border-slate-200/70">
                    <h4 class="text-xs font-bold text-slate-700 uppercase mb-3">Tambah Peserta Baru Manual</h4>
                    <div class="flex flex-col md:flex-row gap-3">
                        <input type="text" id="inputKode" placeholder="Kode (mis: 01)..." class="w-full md:w-32 bg-white border border-slate-200 rounded-xl px-3.5 py-2 text-xs text-slate-800 focus:outline-none focus:border-sky-600 shadow-xs">
                        <input type="text" id="inputNama" placeholder="Nama Lengkap Peserta..." class="flex-1 bg-white border border-slate-200 rounded-xl px-3.5 py-2 text-xs text-slate-800 focus:outline-none focus:border-sky-600 shadow-xs">
                        <select id="inputZona" class="bg-white border border-slate-200 rounded-xl px-3.5 py-2 text-xs text-slate-800 focus:outline-none focus:border-sky-600 shadow-xs">
                            <option value="HIJAU">Zona Hijau</option>
                            <option value="KUNING">Zona Kuning</option>
                            <option value="MERAH">Zona Merah</option>
                        </select>
                        <button type="button" onclick="tambahPeserta()" class="bg-sky-700 hover:bg-sky-800 text-white font-bold px-5 py-2 rounded-xl text-xs transition cursor-pointer shadow-sm">Tambah</button>
                    </div>
                </div>
            </div>

            <!-- Daftar Status Seluruh Peserta (Khusus Admin) -->
            <div id="pesertaListSection" class="hidden bg-white border border-slate-200 rounded-3xl p-6 md:p-8 shadow-xl flex flex-col gap-4">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4 border-b border-slate-100 pb-4">
                    <h3 class="font-bold text-base text-slate-800">Daftar Status Seluruh Peserta</h3>
                    <input type="text" id="searchBox" oninput="renderPeserta()" placeholder="Cari nama atau kode peserta..." class="w-full md:w-72 bg-slate-50 border border-slate-200 rounded-xl px-4 py-2 text-xs text-slate-800 focus:outline-none focus:border-sky-600 shadow-xs">
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse text-xs md:text-sm">
                        <thead>
                            <tr class="border-b border-slate-100 text-slate-400 font-semibold">
                                <th class="py-3 px-3">Kode</th>
                                <th class="py-3 px-3">Nama Peserta</th>
                                <th class="py-3 px-3">Status Zona</th>
                                <th class="py-3 px-3 text-right">Aksi Cepat</th>
                            </tr>
                        </thead>
                        <tbody id="tabelPeserta" class="divide-y divide-slate-100">
                            <!-- Rendered by JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </main>

    <!-- Modal Login Panitia -->
    <div id="authModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm hidden items-center justify-center p-4 z-[999]">
        <div class="bg-white border border-slate-200 rounded-3xl p-6 md:p-8 max-w-sm w-full shadow-2xl flex flex-col gap-4 relative z-[1000]">
            <div class="flex justify-between items-center">
                <h3 class="font-extrabold text-base text-slate-900">🔐 Login Akses Panitia</h3>
                <button type="button" onclick="tutupModalLogin()" class="text-slate-400 hover:text-slate-600 text-sm font-bold">✕</button>
            </div>
            <p class="text-xs text-slate-500">Masukkan password panitia untuk membuka panel kontrol administratif.</p>
            <input type="password" id="passwordInput" placeholder="Password Panitia..." class="bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-xs text-slate-800 focus:outline-none focus:border-sky-600 shadow-xs">
            <div class="flex justify-end gap-2 mt-2">
                <button type="button" onclick="tutupModalLogin()" class="px-4 py-2.5 bg-slate-100 hover:bg-slate-200 text-xs font-semibold rounded-xl text-slate-700 transition cursor-pointer">Batal</button>
                <button type="button" onclick="prosesLogin()" class="px-4 py-2.5 bg-sky-700 hover:bg-sky-800 text-white text-xs font-semibold rounded-xl transition cursor-pointer shadow-sm">Masuk</button>
            </div>
        </div>
    </div>

    <!-- Script Javascript -->
    <script>
        let isPanitia = false;
        let currentStudent = null;
        // LINK APPS SCRIPT SUDAH TERTANAM PERMANEN
        let scriptUrl = 'https://script.google.com/macros/s/AKfycbyipYOXWChY79wlhIsP-tCMc-Z_hHsvrPNtiKJEXzjB5e9OIm0A3t5CrMXgs4JuNt3a9A/exec';
        let globalZona = 'HIJAU';
        let audioCtx = null;
        let pesertaList = [];

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        function playSound(type) {
            try {
                initAudio();
                if (!audioCtx) return;

                const now = audioCtx.currentTime;

                if (type === 'MERAH') {
                    for (let i = 0; i < 3; i++) {
                        let startTime = now + (i * 0.35);
                        const osc = audioCtx.createOscillator();
                        const gain = audioCtx.createGain();
                        osc.type = 'sawtooth';
                        
                        osc.frequency.setValueAtTime(700, startTime);
                        osc.frequency.linearRampToValueAtTime(1400, startTime + 0.17);
                        osc.frequency.linearRampToValueAtTime(700, startTime + 0.34);
                        
                        gain.gain.setValueAtTime(0.3, startTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, startTime + 0.34);

                        osc.connect(gain);
                        gain.connect(audioCtx.destination);
                        osc.start(startTime);
                        osc.stop(startTime + 0.34);
                    }
                } 
                else if (type === 'KUNING') {
                    for (let i = 0; i < 2; i++) {
                        let startTime = now + (i * 0.45);
                        const osc = audioCtx.createOscillator();
                        const gain = audioCtx.createGain();
                        osc.type = 'triangle';
                        
                        osc.frequency.setValueAtTime(500, startTime);
                        osc.frequency.linearRampToValueAtTime(800, startTime + 0.22);
                        osc.frequency.linearRampToValueAtTime(500, startTime + 0.44);

                        gain.gain.setValueAtTime(0.25, startTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, startTime + 0.44);

                        osc.connect(gain);
                        gain.connect(audioCtx.destination);
                        osc.start(startTime);
                        osc.stop(startTime + 0.44);
                    }
                } 
                else {
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'sine';
                    
                    osc.frequency.setValueAtTime(523.25, now);
                    osc.frequency.setValueAtTime(659.25, now + 0.15);

                    gain.gain.setValueAtTime(0.15, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);

                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now);
                    osc.stop(now + 0.3);
                }
            } catch(e) {}
        }

        function tarikDataCloudSiswa() {
            initAudio();
            fetch(scriptUrl)
                .then(res => res.json())
                .then(data => {
                    if (Array.isArray(data) && data.length > 0) {
                        pesertaList = data;
                        if (isPanitia) renderPeserta();
                        if (currentStudent) {
                            let updated = pesertaList.find(p => String(p.kode).toLowerCase() === String(currentStudent.kode).toLowerCase() || String(p.nama).toLowerCase() === String(currentStudent.nama).toLowerCase());
                            if (updated) {
                                currentStudent = updated;
                                updateGlobalDisplay();
                            }
                        }
                    }
                })
                .catch(err => console.log('Gagal narik data: ' + err));
        }

        function loginSiswa() {
            initAudio();
            const inputVal = document.getElementById('studentCodeInput').value.trim().toLowerCase();
            if (!inputVal) return alert('Masukkan kode unik atau nama Anda terlebih dahulu!');

            if (pesertaList.length === 0) {
                fetch(scriptUrl)
                    .then(res => res.json())
                    .then(data => {
                        pesertaList = data;
                        eksekusiLoginSiswa(inputVal);
                    })
                    .catch(err => alert('Gagal terhubung ke database. Coba lagi!'));
            } else {
                eksekusiLoginSiswa(inputVal);
            }
        }

        function eksekusiLoginSiswa(inputVal) {
            let found = pesertaList.find(p => String(p.kode).toLowerCase() === inputVal || String(p.nama).toLowerCase().includes(inputVal));
            if (!found) {
                return alert('Kode atau Nama peserta tidak ditemukan dalam database. Hubungi panitia!');
            }

            currentStudent = found;
            document.getElementById('studentLoginView').classList.add('hidden');
            document.getElementById('dashboardView').classList.remove('hidden');
            document.getElementById('studentIdentityCard').classList.remove('hidden');
            document.getElementById('currentStudentName').innerText = `${found.kode} - ${found.nama}`;
            document.getElementById('roleBadge').innerText = "Mode: Siswa";
            
            document.getElementById('pesertaListSection').classList.add('hidden');
            updateGlobalDisplay();
        }

        function logoutSiswa() {
            initAudio();
            currentStudent = null;
            document.getElementById('studentLoginView').classList.remove('hidden');
            document.getElementById('dashboardView').classList.add('hidden');
            document.getElementById('studentIdentityCard').classList.add('hidden');
            document.getElementById('studentCodeInput').value = '';
        }

        function setGlobalZona(zona) {
            initAudio();
            globalZona = zona;
            playSound(zona);
            updateGlobalDisplay();
        }

        function updateGlobalDisplay() {
            const card = document.getElementById('globalZoneCard');
            const badgeContainer = document.getElementById('globalZoneBadgeContainer');
            const badgeText = document.getElementById('globalZoneBadgeText');
            const title = document.getElementById('globalZoneTitle');
            const desc = document.getElementById('globalZoneDesc');
            const dot = document.getElementById('statusIndicatorDot');

            card.className = "rounded-3xl p-8 md:p-12 text-center shadow-xl border transition-all duration-500 bg-white";
            
            let activeZona = globalZona;
            if (currentStudent) {
                activeZona = currentStudent.zona;
            }

            if (activeZona === 'MERAH') {
                card.classList.add('pulse-red-card', 'border-rose-500');
                badgeContainer.className = "inline-flex items-center gap-2 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider mb-4 border bg-rose-50 border-rose-200 text-rose-700 shadow-xs";
                dot.className = "w-2.5 h-2.5 rounded-full bg-rose-600 animate-ping";
                badgeText.innerText = "PERINGATAN KRITIS BAHAYA";
                title.className = "text-3xl md:text-5xl font-black tracking-tight uppercase mb-3 text-rose-600 animate-pulse";
                title.innerText = "🚨 ZONA MERAH BAHAYA";
                desc.innerText = "Tetap aktif dan berjuang! Jangan menyerah, segera ambil tindakan cepat dan perbaiki keadaan!";
            } else if (activeZona === 'KUNING') {
                card.classList.add('pulse-yellow-card', 'border-amber-400');
                badgeContainer.className = "inline-flex items-center gap-2 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider mb-4 border bg-amber-50 border-amber-200 text-amber-700 shadow-xs";
                dot.className = "w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse";
                badgeText.innerText = "STATUS WASPADA";
                title.className = "text-3xl md:text-5xl font-black tracking-tight uppercase mb-3 text-amber-600";
                title.innerText = "⚠️ ZONA KUNING WASPADA";
                desc.innerText = "Semangat dan terus berjuang! Jadikan peringatan ini sebagai picu untuk lebih fokus dan bangkit.";
            } else {
                card.classList.add('border-emerald-200');
                badgeContainer.className = "inline-flex items-center gap-2 px-4 py-1.5 rounded-full text-xs font-bold uppercase tracking-wider mb-4 border bg-emerald-50 border-emerald-200 text-emerald-700 shadow-xs";
                dot.className = "w-2.5 h-2.5 rounded-full bg-emerald-500";
                badgeText.innerText = "KONDISI KONDUSIF";
                title.className = "text-3xl md:text-5xl font-black tracking-tight uppercase mb-3 text-emerald-600";
                title.innerText = "🟢 ZONA AMAN KONDUSIF";
                desc.innerText = "Jangan lengah! Tetap pertahankan kedisiplinan dan tingkatkan konsentrasi hingga akhir.";
            }
        }

        function renderPeserta() {
            if (!isPanitia) return;
            const tbody = document.getElementById('tabelPeserta');
            const keyword = document.getElementById('searchBox').value.toLowerCase();
            tbody.innerHTML = '';

            const filtered = pesertaList.filter(p => String(p.nama).toLowerCase().includes(keyword) || String(p.kode).toLowerCase().includes(keyword));

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" class="py-6 text-center text-slate-400 text-xs">Tidak ada data peserta ditemukan.</td></tr>`;
                return;
            }

            filtered.forEach((p) => {
                let badgeColor = p.zona === 'MERAH' ? 'bg-rose-50 text-rose-700 border-rose-200' : p.zona === 'KUNING' ? 'bg-amber-50 text-amber-700 border-amber-200' : 'bg-emerald-50 text-emerald-700 border-emerald-200';
                
                let tr = document.createElement('tr');
                tr.className = "border-b border-slate-50 hover:bg-slate-50/80 transition";
                
                tr.innerHTML = `
                    <td class="py-3.5 px-3 text-slate-500 font-mono font-bold">${p.kode}</td>
                    <td class="py-3.5 px-3 font-bold text-slate-800">${p.nama}</td>
                    <td class="py-3.5 px-3"><span class="px-2.5 py-1 text-[11px] font-extrabold rounded-full border ${badgeColor}">${p.zona}</span></td>
                    <td class="py-3.5 px-3 text-right flex justify-end gap-1.5">
                        <button type="button" onclick="ubahZonaIndividu('${p.kode}', 'HIJAU')" class="px-2 py-1 bg-emerald-100 text-emerald-800 text-[10px] font-bold rounded-lg hover:bg-emerald-600 hover:text-white transition cursor-pointer">H</button>
                        <button type="button" onclick="ubahZonaIndividu('${p.kode}', 'KUNING')" class="px-2 py-1 bg-amber-100 text-amber-800 text-[10px] font-bold rounded-lg hover:bg-amber-500 hover:text-white transition cursor-pointer">K</button>
                        <button type="button" onclick="ubahZonaIndividu('${p.kode}', 'MERAH')" class="px-2 py-1 bg-rose-100 text-rose-800 text-[10px] font-bold rounded-lg hover:bg-rose-600 hover:text-white transition cursor-pointer">M</button>
                        <button type="button" onclick="hapusPeserta('${p.kode}')" class="px-2 py-1 bg-slate-100 text-slate-500 text-[10px] rounded-lg hover:bg-rose-600 hover:text-white transition cursor-pointer">✕</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function bukaModalLogin() {
            initAudio();
            const modal = document.getElementById('authModal');
            modal.classList.remove('hidden');
            modal.classList.add('flex');
            setTimeout(() => document.getElementById('passwordInput').focus(), 100);
        }

        function tutupModalLogin() {
            const modal = document.getElementById('authModal');
            modal.classList.add('hidden');
            modal.classList.remove('flex');
        }

        function prosesLogin() {
            initAudio();
            const pass = document.getElementById('passwordInput').value;
            if (pass === 'ldks2026') {
                isPanitia = true;
                tutupModalLogin();
                document.getElementById('passwordInput').value = '';
                
                document.getElementById('studentLoginView').classList.add('hidden');
                document.getElementById('dashboardView').classList.remove('hidden');
                document.getElementById('panitiaPanel').classList.remove('hidden');
                document.getElementById('pesertaListSection').classList.remove('hidden');
                document.getElementById('roleBadge').innerText = "Mode: Panitia (Admin)";
                document.getElementById('roleBadge').className = "px-3 py-1 bg-sky-100 text-sky-900 text-xs font-extrabold rounded-full border border-sky-200";
                document.getElementById('authBtn').classList.add('hidden');
                renderPeserta();
            } else {
                alert('Password salah!');
            }
        }

        function logoutPanitia() {
            initAudio();
            isPanitia = false;
            document.getElementById('panitiaPanel').classList.add('hidden');
            document.getElementById('pesertaListSection').classList.add('hidden');
            document.getElementById('roleBadge').innerText = "Mode: Siswa";
            document.getElementById('roleBadge').className = "px-3 py-1 bg-slate-100 text-slate-600 text-xs font-bold rounded-full border border-slate-200";
            document.getElementById('authBtn').classList.remove('hidden');
            
            if (!currentStudent) {
                document.getElementById('dashboardView').classList.add('hidden');
                document.getElementById('studentLoginView').classList.remove('hidden');
            }
        }

        function tambahPeserta() {
            initAudio();
            const kode = document.getElementById('inputKode').value.trim();
            const nama = document.getElementById('inputNama').value.trim();
            const zona = document.getElementById('inputZona').value;
            if (!kode || !nama) return alert('Kode dan Nama peserta tidak boleh kosong!');

            pesertaList.push({ kode, nama, zona });
            document.getElementById('inputKode').value = '';
            document.getElementById('inputNama').value = '';
            renderPeserta();
        }

        function ubahZonaIndividu(kode, zonaBaru) {
            initAudio();
            const p = pesertaList.find(item => String(item.kode).toLowerCase() === String(kode).toLowerCase());
            if (p) {
                p.zona = zonaBaru;
                renderPeserta();
                if (currentStudent && String(currentStudent.kode).toLowerCase() === String(kode).toLowerCase()) {
                    currentStudent.zona = zonaBaru;
                    updateGlobalDisplay();
                    playSound(zonaBaru);
                }
            }
        }

        function hapusPeserta(kode) {
            initAudio();
            pesertaList = pesertaList.filter(item => String(item.kode).toLowerCase() !== String(kode).toLowerCase());
            renderPeserta();
        }

        function syncToCloud() {
            initAudio();
            const btn = document.getElementById('syncBtn');
            btn.innerText = "Menyinkronkan...";
            
            fetch(scriptUrl, {
                method: 'POST',
                mode: 'no-cors',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ action: 'syncAll', pesertaList: pesertaList })
            }).then(() => {
                btn.innerText = "☁️ Simpan Ke Sheets";
                alert('Berhasil menyimpan data ke Google Sheets!');
            }).catch(err => {
                btn.innerText = "☁️ Simpan Ke Sheets";
                alert('Gagal sinkron: ' + err);
            });
        }

        // Jalankan saat pertama dibuka
        tarikDataCloudSiswa();
        updateGlobalDisplay();
    </script>
</body>
</html>
