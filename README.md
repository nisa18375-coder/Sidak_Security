# Sidak_Security
index.html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sidak Security Bank Enterprise v4.6</title>
    <!-- Tailwind CSS -->
    <script src="https://tailwindcss.com"></script>
    <!-- html2pdf.js -->
    <script src="https://cloudflare.com"></script>
</head>
<body class="bg-slate-900 min-h-screen text-gray-800 font-sans antialiased md:py-8" id="body-bg">

    <!-- Container Utama Aplikasi (Mobile Responsive) -->
    <div class="max-w-md mx-auto bg-white min-h-screen md:min-h-[90vh] md:rounded-2xl shadow-2xl flex flex-col justify-between overflow-hidden transition-all duration-300" id="cetak-area">
        
        <!-- Konten Utama Form -->
        <div class="p-5 space-y-6">
            
            <!-- Header Dokumen Formal & Network Indicator -->
            <div class="text-center border-b-4 border-blue-900 pb-3 relative" id="header-dokumen">
                <div id="network-status" class="absolute -top-3 left-1/2 -translate-x-1/2 text-[9px] font-extrabold px-3 py-0.5 rounded-full transition-all duration-300 bg-emerald-600 text-white animate-pulse">ONLINE MODE</div>
                <h1 class="text-xl font-black text-blue-900 tracking-wider transition-colors pt-2" id="text-title">E-SIDAK VENDOR SECURITY</h1>
                <p class="text-xs uppercase tracking-widest text-gray-500 font-bold mt-1" id="text-subtitle">Sistem Audit Internal Perbankan</p>
            </div>

            <!-- Tab Navigasi Konten -->
            <div class="flex border-b border-slate-200 text-[11px] font-bold" id="nav-tabs">
                <button onclick="switchTab('form')" id="tab-form" class="flex-1 text-center py-2.5 border-b-2 border-blue-700 text-blue-700">Form</button>
                <button onclick="switchTab('draf')" id="tab-draf" class="flex-1 text-center py-2.5 text-slate-500 hover:text-slate-800">Draf (<span id="count-draf">0</span>)</button>
                <button onclick="switchTab('riwayat')" id="tab-riwayat" class="flex-1 text-center py-2.5 text-slate-500 hover:text-slate-800">Arsip (<span id="count-riwayat">0</span>)</button>
                <button onclick="switchTab('pengaturan')" id="tab-pengaturan" class="flex-1 text-center py-2.5 text-slate-500 hover:text-slate-800">⚙️ Kustom</button>
            </div>

            <!-- VIEW 1: FORM INSPEKSI UTAMA -->
            <div id="view-form" class="space-y-6">
                <div id="draf-alert-zone" class="hidden bg-amber-50 border border-amber-300 rounded-xl p-3 text-xs flex justify-between items-center">
                    <span class="text-amber-800 font-medium">✏️ Anda sedang mengedit <b>Draf Tersimpan</b></span>
                    <button onclick="keluarDariEditDraf()" class="bg-amber-200 text-amber-900 px-2 py-1 rounded font-bold hover:bg-amber-300">Form Baru</button>
                </div>

                <!-- Bagian Metadata Informasi Umum -->
                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3.5 text-xs shadow-sm transition-colors" id="meta-container">
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block font-bold text-slate-700">Hari / Tanggal *</label>
                            <input type="date" id="tanggal" onchange="saveToLocalStorage()" class="w-full mt-1 p-2.5 bg-white border border-slate-300 rounded-lg font-medium focus:ring-2 focus:ring-blue-500 focus:outline-none required-field">
                        </div>
                        <div>
                            <label class="block font-bold text-slate-700">Shift / Regu *</label>
                            <select id="shift" onchange="saveToLocalStorage()" class="w-full mt-1 p-2.5 bg-white border border-slate-300 rounded-lg font-medium focus:ring-2 focus:ring-blue-500 focus:outline-none">
                                <option value="Pagi">Pagi (08.00 - 20.00)</option>
                                <option value="Malam">Malam (20.00 - 08.00)</option>
                            </select>
                        </div>
                    </div>
                    <div>
                        <label class="block font-bold text-slate-700">Wilayah Operasional *</label>
                        <input type="text" id="wilayah" oninput="saveToLocalStorage()" placeholder="Contoh: Wilayah Sinjai - Bank BSI" class="w-full mt-1 p-2.5 bg-white border border-slate-300 rounded-lg font-medium focus:ring-2 focus:ring-blue-500 focus:outline-none required-field">
                    </div>
                    <div>
                        <label class="block font-bold text-slate-700">Nama Personil (Anggota Diperiksa) *</label>
                        <input type="text" id="personil" oninput="saveToLocalStorage()" placeholder="Nama lengkap anggota" class="w-full mt-1 p-2.5 bg-white border border-slate-300 rounded-lg font-medium focus:ring-2 focus:ring-blue-500 focus:outline-none required-field">
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block font-bold text-slate-700">Waktu Inspeksi *</label>
                            <input type="text" id="waktu" oninput="saveToLocalStorage()" placeholder="Contoh: 10.15 WITA" class="w-full mt-1 p-2.5 bg-white border border-slate-300 rounded-lg font-medium focus:ring-2 focus:ring-blue-500 focus:outline-none required-field">
                        </div>
                        <div>
                            <label class="block font-bold text-slate-700">Status Kehadiran *</label>
                            <select id="kehadiran" onchange="saveToLocalStorage()" class="w-full mt-1 p-2.5 bg-white border border-slate-300 rounded-lg font-medium focus:ring-2 focus:ring-blue-500 focus:outline-none">
                                <option value="Absen">Hadir / Dinas</option>
                                <option value="Izin">Izin Resmi</option>
                                <option value="Alfa">Mangkir / Alfa</option>
                                <option value="Tanpa Keterangan">Tanpa Keterangan</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Score Live Dashboard Banner -->
                <div class="bg-blue-50 border-2 border-blue-200 rounded-xl p-4 shadow-sm transition-all duration-300" id="score-banner">
                    <div class="flex justify-between items-center">
                        <div>
                            <h3 class="text-xs font-bold text-blue-900 uppercase tracking-wider" id="score-title">Skor Kepatuhan Total</h3>
                            <p class="text-3xl font-black text-blue-900 mt-0.5" id="live-score">100%</p>
                        </div>
                        <div class="text-right">
                            <span id="status-badge" class="px-3 py-1.5 rounded-full text-xs font-extrabold bg-green-100 text-green-800 uppercase">LAYAK OPERASI</span>
                        </div>
                    </div>
                    <div id="sla-penalty-panel" class="hidden mt-2 p-2 bg-rose-100 border border-rose-300 rounded-lg text-[10px] text-rose-900 font-semibold">
                        ⚠️ Terdeteksi Pelanggaran SLA: Rekomendasi Surat Peringatan (SP) / Pemotongan Pinalti Vendor Terbuka.
                    </div>
                    <div class="mt-3 pt-3 border-t border-blue-200/60 text-[10px] space-y-2 text-slate-600 font-medium" id="sub-scores-display"></div>
                </div>

                <!-- Kontainer Poin Kriteria Penilaian -->
                <div class="space-y-5" id="checklist-container"></div>

                <!-- VI. Tindakan Temuan -->
                <div class="space-y-2">
                    <h2 class="text-xs font-bold bg-amber-600 text-white p-2.5 rounded-lg shadow-sm block-header" data-bg="bg-amber-600">VI. TINDAKAN TEMUAN & REKOMENDASI</h2>
                    <textarea id="tindakan-temuan" oninput="saveToLocalStorage()" rows="3" placeholder="Tuliskan temuan penyimpangan di lapangan..." class="w-full p-3 border border-slate-300 text-sm rounded-xl focus:ring-2 focus:ring-amber-500 focus:outline-none"></textarea>
                </div>

                <!-- VII. Modul Kamera Multi-Foto -->
                <div class="space-y-2 bg-slate-50 p-4 rounded-xl border border-slate-200 container-box">
                    <h2 class="text-xs font-bold text-slate-700 uppercase tracking-wide">VII. FOTO BUKTI SIDAK LAPANGAN (MAKSIMAL 3 FOTO)</h2>
                    <input type="file" id="foto-input" accept="image/*" capture="environment" onchange="prosesMultiWatermarkFoto(event)" class="w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-bold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100">
                    <div class="grid grid-cols-3 gap-2 mt-3" id="multi-foto-container"></div>
                </div>

                <!-- VIII. Modul Tanda Tangan Ganda Canvas (Koreksi Layout Simetris) -->
                <div class="space-y-4 bg-slate-50 p-4 rounded-xl border border-slate-200 html2pdf__page-break container-box" id="signature-block">
                    <h2 class="text-xs font-bold text-slate-700 uppercase tracking-wide">VIII. PENGESAHAN TANDA TANGAN BERSAMA *</h2>
                    
                    <div class="grid grid-cols-2 gap-3">
                        <div class="space-y-1">
                            <label class="block text-[11px] font-bold text-slate-600 truncate">Tanda Tangan Korlap</label>
                            <div class="border border-slate-300 bg-white rounded-xl relative shadow-inner overflow-hidden">
                            
