[kalkulator_bep_retail.html](https://github.com/user-attachments/files/33374706/kalkulator_bep_retail.html)<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulator BEP Penjualan Retail</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts Inter & JetBrains Mono -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@500;700&display=swap" rel="stylesheet">
    <!-- Chart.js CDN for Visual Analysis -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8FAFC;
        }
        .font-mono {
            font-family: 'JetBrains Mono', monospace;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            .print-area {
                border: none !important;
                box-shadow: none !important;
                width: 100% !important;
                max-width: 100% !important;
            }
        }
    </style>
</head>
<body class="min-h-screen text-slate-800 pb-12">

    <!-- Header Navigation Bar -->
    <header class="no-print bg-indigo-700 text-white shadow-md mb-6">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="bg-white/20 p-2 rounded-xl">
                    <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path>
                    </svg>
                </div>
                <div>
                    <h1 class="font-extrabold text-lg sm:text-xl tracking-tight">BEP Nutrilife</h1>
                    <p class="text-indigo-200 text-xs hidden sm:block">Hitung Titik Impas (Break-Even Point) & Target Keuntungan Usaha Retail</p>
                </div>
            </div>
            
            <div class="flex items-center gap-2">
                <button type="button" onclick="loadSampleData()" class="text-xs bg-indigo-800 hover:bg-indigo-900 text-indigo-100 px-3 py-2 rounded-xl transition font-medium border border-indigo-500/50 flex items-center gap-1.5 active:scale-95">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
                    </svg>
                    Isi Contoh Data
                </button>
                <button type="button" onclick="resetForm()" class="text-xs bg-slate-800 hover:bg-slate-900 text-slate-200 px-3 py-2 rounded-xl transition font-medium border border-slate-700 flex items-center gap-1.5 active:scale-95">
                    Reset
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Layout -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        
        <!-- Error Alert Banner (Hidden by Default) -->
        <div id="errorBanner" class="hidden mb-6 p-4 bg-rose-50 border-l-4 border-rose-500 rounded-r-xl text-rose-800 text-xs sm:text-sm flex items-center justify-between">
            <div class="flex items-center gap-2">
                <svg class="w-5 h-5 text-rose-500 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                </svg>
                <span id="errorMessage">Harga jual per unit harus lebih besar dari biaya variabel (HPP) per unit agar usaha mendapatkan margin keuntungan.</span>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">
            
            <!-- LEFT COLUMN: Form Inputs (5 Columns) -->
            <div class="no-print lg:col-span-5 space-y-5">
                
                <!-- Card 1: Harga Jual & Biaya Variabel Dinamis -->
                <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm space-y-4">
                    <div class="flex justify-between items-center border-b border-slate-100 pb-2.5">
                        <h2 class="text-sm font-bold text-slate-800 flex items-center gap-2">
                            <span class="w-2.5 h-2.5 rounded-full bg-indigo-600"></span>
                            1. Penjualan & Biaya Variabel / Unit
                        </h2>
                        <button type="button" onclick="addVariableCostRow()" class="text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-700 font-bold px-2.5 py-1 rounded-lg border border-indigo-200 transition flex items-center gap-1 active:scale-95">
                            + Tambah Item
                        </button>
                    </div>

                    <div class="space-y-3">
                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">Harga Jual per Unit (Rp) <span class="text-rose-500">*</span></label>
                            <input type="number" id="sellingPrice" oninput="calculateBEP()" min="0" placeholder="Contoh: 150000" class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2.5 text-xs font-semibold focus:ring-2 focus:ring-indigo-600 focus:outline-none transition">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">Rincian Biaya Variabel / HPP per Unit (Rp)</label>
                            <div id="variableCostContainer" class="space-y-2">
                                <!-- Dynamic Variable Cost Rows will be added here -->
                            </div>
                        </div>

                        <div class="pt-2 border-t border-slate-100 flex justify-between items-center text-xs font-bold text-slate-800">
                            <span>Total Biaya Variabel / Unit:</span>
                            <span id="totalVariableCostDisplay" class="text-indigo-700 font-mono">Rp 0</span>
                        </div>
                    </div>
                </div>

                <!-- Card 2: Biaya Tetap Operasional Dinamis (Fixed Costs) -->
                <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm space-y-4">
                    <div class="flex justify-between items-center border-b border-slate-100 pb-2.5">
                        <h2 class="text-sm font-bold text-slate-800 flex items-center gap-2">
                            <span class="w-2.5 h-2.5 rounded-full bg-indigo-600"></span>
                            2. Biaya Tetap (Fixed Costs) / Bulan
                        </h2>
                        <button type="button" onclick="addFixedCostRow()" class="text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-700 font-bold px-2.5 py-1 rounded-lg border border-indigo-200 transition flex items-center gap-1 active:scale-95">
                            + Tambah Item
                        </button>
                    </div>

                    <div class="space-y-3">
                        <div id="fixedCostContainer" class="space-y-2">
                            <!-- Dynamic Fixed Cost Rows will be added here -->
                        </div>

                        <div class="pt-2 border-t border-slate-100 flex justify-between items-center text-xs font-bold text-slate-800">
                            <span>Total Biaya Tetap Bulanan:</span>
                            <span id="totalFixedCostDisplay" class="text-indigo-700 font-mono">Rp 0</span>
                        </div>
                    </div>
                </div>

                <!-- Card 3: Target Keuntungan (Opsional) -->
                            <span id="totalFixedCostDisplay" class="text-indigo-700 font-mono">Rp 0</span>
                        </div>
                    </div>
                </div>

                <!-- Card 3: Target Keuntungan (Optional) -->
                <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm space-y-4">
                    <div class="flex justify-between items-center border-b border-slate-100 pb-2.5">
                        <h2 class="text-sm font-bold text-slate-800 flex items-center gap-2">
                            <span class="w-2.5 h-2.5 rounded-full bg-emerald-600"></span>
                            3. Target Keuntungan Bersih (Opsional)
                        </h2>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-slate-600 mb-1">Target Laba Bersih Bulanan (Rp)</label>
                        <input type="number" id="targetProfit" oninput="calculateBEP()" min="0" placeholder="Contoh: 10000000" class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2.5 text-xs font-semibold focus:ring-2 focus:ring-emerald-600 focus:outline-none transition">
                        <p class="text-[10px] text-slate-400 mt-1">Estimasi unit yang harus dijual untuk mencapai laba bersih ini.</p>
                    </div>
                </div>

            </div>

            <!-- RIGHT COLUMN: Dashboard & Visual Analytics (7 Columns) -->
            <div class="lg:col-span-7 space-y-5 print-area">
                
                <!-- Action Header Bar -->
                <div class="no-print bg-slate-900 text-white rounded-2xl p-4 flex flex-wrap items-center justify-between gap-3 shadow-md">
                    <div>
                        <p class="font-bold text-slate-200 text-xs sm:text-sm">Hasil Analisis Break-Even Point</p>
                        <p class="text-slate-400 text-[11px]">Ringkasan performa finansial produk retail</p>
                    </div>
                    <button type="button" onclick="window.print()" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold px-4 py-2 rounded-xl text-xs transition flex items-center gap-1.5 active:scale-95 shadow-sm">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 17h2a2 2 0 002-2v-4a2 2 0 00-2-2H5a2 2 0 00-2 2v4a2 2 0 002 2h2m2 4h6a2 2 0 002-2v-4a2 2 0 00-2-2H9a2 2 0 00-2 2v4a2 2 0 002 2zm8-12V5a2 2 0 00-2-2H9a2 2 0 00-2 2v4h10z"></path>
                        </svg>
                        Cetak Laporan
                    </button>
                </div>

                <!-- Main Metric Cards Grid -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    
                    <!-- Card BEP Unit -->
                    <div class="bg-indigo-900 text-white rounded-2xl p-5 shadow-sm space-y-1 relative overflow-hidden">
                        <div class="absolute -right-4 -bottom-4 w-24 h-24 bg-indigo-800/50 rounded-full blur-xl pointer-events-none"></div>
                        <span class="text-[10px] font-bold uppercase tracking-wider text-indigo-300 block">BEP Penjualan (Unit)</span>
                        <div class="flex items-baseline gap-1.5">
                            <span id="bepUnitDisplay" class="text-2xl sm:text-3xl font-black font-mono">0</span>
                            <span class="text-xs text-indigo-200 font-medium">Pcs / Unit</span>
                        </div>
                        <p class="text-[11px] text-indigo-200/80 pt-1 border-t border-indigo-800">
                            Jumlah minimum unit yang harus terjual agar tidak rugi.
                        </p>
                    </div>

                    <!-- Card BEP Rupiah -->
                    <div class="bg-emerald-900 text-white rounded-2xl p-5 shadow-sm space-y-1 relative overflow-hidden">
                        <div class="absolute -right-4 -bottom-4 w-24 h-24 bg-emerald-800/50 rounded-full blur-xl pointer-events-none"></div>
                        <span class="text-[10px] font-bold uppercase tracking-wider text-emerald-300 block">BEP Omnset (Rupiah)</span>
                        <div class="flex items-baseline gap-1.5">
                            <span id="bepRupiahDisplay" class="text-xl sm:text-2xl font-black font-mono">Rp 0</span>
                        </div>
                        <p class="text-[11px] text-emerald-200/80 pt-1 border-t border-emerald-800">
                            Total nilai penjualan minimum untuk balik modal.
                        </p>
                    </div>

                </div>

                <!-- Secondary Financial Metrics Row -->
                <div class="grid grid-cols-2 sm:grid-cols-3 gap-3">
                    <div class="bg-white p-3.5 rounded-2xl border border-slate-200 text-center">
                        <span class="text-[10px] font-bold text-slate-400 block uppercase">Margin Kontribusi / Unit</span>
                        <span id="marginUnitDisplay" class="text-xs sm:text-sm font-extrabold text-slate-800 font-mono mt-1 block">Rp 0</span>
                    </div>

                    <div class="bg-white p-3.5 rounded-2xl border border-slate-200 text-center">
                        <span class="text-[10px] font-bold text-slate-400 block uppercase">Rasio Margin Kontribusi</span>
                        <span id="marginRatioDisplay" class="text-xs sm:text-sm font-extrabold text-indigo-600 font-mono mt-1 block">0%</span>
                    </div>

                    <div class="bg-white p-3.5 rounded-2xl border border-slate-200 text-center col-span-2 sm:col-span-1">
                        <span class="text-[10px] font-bold text-slate-400 block uppercase">Target Penjualan Unit</span>
                        <span id="targetUnitDisplay" class="text-xs sm:text-sm font-extrabold text-emerald-700 font-mono mt-1 block">0 Pcs</span>
                    </div>
                </div>

                <!-- Interactive Chart & Profit Simulator -->
                <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm space-y-4">
                    <div class="flex flex-wrap justify-between items-center border-b border-slate-100 pb-3 gap-2">
                        <div>
                            <h3 class="text-xs sm:text-sm font-bold text-slate-800">Grafik Visualiasi Break-Even Point</h3>
                            <p class="text-[10px] text-slate-400">Titik potong Pendapatan (Hijau) dan Total Biaya (Merah)</p>
                        </div>
                    </div>

                    <!-- Chart Canvas -->
                    <div class="relative w-full h-64">
                        <canvas id="bepChart"></canvas>
                    </div>
                </div>

                <!-- Simulation Slider Card -->
                <div class="no-print bg-indigo-50/60 rounded-2xl p-5 border border-indigo-100 space-y-3">
                    <div class="flex justify-between items-center flex-wrap gap-2">
                        <h3 class="text-xs font-bold text-indigo-900 flex items-center gap-1.5">
                            <svg class="w-4 h-4 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"></path>
                            </svg>
                            Simulasi Keuntungan berdasarkan Volume Penjualan
                        </h3>
                        <div class="flex items-center gap-2">
                            <label class="text-[11px] text-indigo-800 font-semibold">Qty Simulasi:</label>
                            <input type="number" id="simQtyInput" min="0" max="10000" value="0" oninput="syncSimInput(this.value)" class="w-24 bg-white border border-indigo-300 rounded-lg px-2 py-1 text-xs font-mono font-bold text-center focus:ring-1 focus:ring-indigo-600 focus:outline-none">
                            <span id="simQtyBadge" class="text-xs font-mono font-bold bg-indigo-600 text-white px-2.5 py-1 rounded-full">0 Unit</span>
                        </div>
                    </div>

                    <div>
                        <input type="range" id="simQtySlider" min="0" max="10000" value="0" step="1" oninput="syncSimSlider(this.value)" class="w-full accent-indigo-600 cursor-pointer">
                        <div class="flex justify-between text-[10px] text-slate-400 mt-1">
                            <span>0 Unit</span>
                            <span id="simMidLabel">5,000 Unit</span>
                            <span id="simMaxLabel">10,000 Unit</span>
                        </div>
                    </div>

                    <!-- Simulation Profit Output -->
                    <div class="bg-white p-3.5 rounded-xl border border-indigo-100 flex justify-between items-center text-xs">
                        <div>
                            <span class="text-[11px] text-slate-500 block">Perkiraan Hasil Operasional:</span>
                            <span id="simStatusBadge" class="inline-block mt-0.5 text-[10px] font-extrabold px-2 py-0.5 rounded uppercase bg-slate-100 text-slate-700">IMPÁS / BEP</span>
                        </div>
                        <div class="text-right">
                            <span class="text-[10px] text-slate-400 block">Laba / (Rugi) Bersih:</span>
                            <span id="simProfitDisplay" class="font-mono font-bold text-sm text-slate-800">Rp 0</span>
                        </div>
                    </div>
                </div>

                <!-- Explanation Formula Modal / Guide -->
                <div class="bg-slate-50 rounded-2xl p-4 border border-slate-200 text-xs text-slate-600 space-y-2">
                    <p class="font-bold text-slate-800 flex items-center gap-1">
                        <svg class="w-4 h-4 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                        Rumus Perhitungan BEP Retail:
                    </p>
                    <ul class="list-disc list-inside space-y-1 text-[11px] leading-relaxed text-slate-500">
                        <li><strong>BEP (Unit)</strong> = Biaya Tetap ÷ (Harga Jual per Unit - Biaya Variabel per Unit)</li>
                        <li><strong>BEP (Rupiah)</strong> = BEP (Unit) × Harga Jual per Unit</li>
                        <li><strong>Margin Kontribusi</strong> = Harga Jual per Unit - Biaya Variabel per Unit</li>
                    </ul>
                </div>

            </div>

        </div>
    </main>

    <!-- JavaScript Logic & Chart Engine -->
    <script>
        let bepChartInstance = null;

        // Format Currency Helper
        function formatRupiah(number) {
            return "Rp " + new Intl.NumberFormat("id-ID").format(Math.round(number));
        }

        // Parse inputs cleanly
        function getInputValue(id) {
            const el = document.getElementById(id);
            if (!el) return 0;
            const val = parseFloat(el.value);
            return isNaN(val) ? 0 : val;
        }

        // Dynamic Variable Cost Row Management
        function addVariableCostRow(name = '', value = '') {
            const container = document.getElementById('variableCostContainer');
            const rowId = 'var_row_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4);
            const row = document.createElement('div');
            row.id = rowId;
            row.className = 'flex items-center gap-2';
            row.innerHTML = `
                <input type="text" value="${name}" placeholder="Nama Biaya (mis. Modal / Kemasan)" class="w-1/2 bg-slate-50 border border-slate-300 rounded-xl px-2.5 py-1.5 text-xs focus:ring-1 focus:ring-indigo-600 focus:outline-none">
                <input type="number" value="${value}" min="0" placeholder="Rp" oninput="calculateBEP()" class="variable-cost-item w-1/2 bg-slate-50 border border-slate-300 rounded-xl px-2.5 py-1.5 text-xs font-medium focus:ring-1 focus:ring-indigo-600 focus:outline-none">
                <button type="button" onclick="removeCostRow('${rowId}')" class="text-slate-400 hover:text-rose-600 p-1 transition" title="Hapus">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                </button>
            `;
            container.appendChild(row);
            calculateBEP();
        }

        // Dynamic Fixed Cost Row Management
        function addFixedCostRow(name = '', value = '') {
            const container = document.getElementById('fixedCostContainer');
            const rowId = 'fix_row_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4);
            const row = document.createElement('div');
            row.id = rowId;
            row.className = 'flex items-center gap-2';
            row.innerHTML = `
                <input type="text" value="${name}" placeholder="Nama Biaya (mis. Sewa / Gaji)" class="w-1/2 bg-slate-50 border border-slate-300 rounded-xl px-2.5 py-1.5 text-xs focus:ring-1 focus:ring-indigo-600 focus:outline-none">
                <input type="number" value="${value}" min="0" placeholder="Rp" oninput="calculateBEP()" class="fixed-cost-item w-1/2 bg-slate-50 border border-slate-300 rounded-xl px-2.5 py-1.5 text-xs font-medium focus:ring-1 focus:ring-indigo-600 focus:outline-none">
                <button type="button" onclick="removeCostRow('${rowId}')" class="text-slate-400 hover:text-rose-600 p-1 transition" title="Hapus">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                </button>
            `;
            container.appendChild(row);
            calculateBEP();
        }

        // Remove row helper
        function removeCostRow(rowId) {
            const el = document.getElementById(rowId);
            if (el) {
                el.remove();
                calculateBEP();
            }
        }

        // Sum Variable Costs per Unit
        function getTotalVariableCost() {
            let total = 0;
            const items = document.querySelectorAll('.variable-cost-item');
            items.forEach(input => {
                const val = parseFloat(input.value);
                if (!isNaN(val)) total += val;
            });
            return total;
        }

        // Sum Fixed Costs
        function getTotalFixedCost() {
            let total = 0;
            const items = document.querySelectorAll('.fixed-cost-item');
            items.forEach(input => {
                const val = parseFloat(input.value);
                if (!isNaN(val)) total += val;
            });
            return total;
        }

        // Main Calculation Function
        function calculateBEP() {
            const sellingPrice = getInputValue('sellingPrice');
            const variableCost = getTotalVariableCost();
            const totalFixedCost = getTotalFixedCost();
            const targetProfit = getInputValue('targetProfit');

            const errorBanner = document.getElementById('errorBanner');
            const totalFixedCostDisplay = document.getElementById('totalFixedCostDisplay');
            const totalVariableCostDisplay = document.getElementById('totalVariableCostDisplay');

            if (totalFixedCostDisplay) totalFixedCostDisplay.innerText = formatRupiah(totalFixedCost);
            if (totalVariableCostDisplay) totalVariableCostDisplay.innerText = formatRupiah(variableCost);

            // Validation
            if (sellingPrice > 0 && variableCost >= sellingPrice) {
                errorBanner.classList.remove('hidden');
                document.getElementById('errorMessage').innerText = "Harga jual per unit harus lebih tinggi dari total biaya variabel (HPP). Margin kontribusi bernilai nol atau minus.";
                resetOutputs();
                return;
            } else {
                errorBanner.classList.add('hidden');
            }

            const contributionMargin = sellingPrice - variableCost;
            const contributionRatio = sellingPrice > 0 ? (contributionMargin / sellingPrice) * 100 : 0;

            let bepUnit = 0;
            let bepRupiah = 0;
            let targetUnit = 0;

            if (contributionMargin > 0 && totalFixedCost > 0) {
                bepUnit = totalFixedCost / contributionMargin;
                bepRupiah = bepUnit * sellingPrice;

                if (targetProfit > 0) {
                    targetUnit = (totalFixedCost + targetProfit) / contributionMargin;
                }
            }

            // Update Metric Displays
            document.getElementById('bepUnitDisplay').innerText = Math.ceil(bepUnit).toLocaleString('id-ID');
            document.getElementById('bepRupiahDisplay').innerText = formatRupiah(bepRupiah);
            document.getElementById('marginUnitDisplay').innerText = formatRupiah(contributionMargin);
            document.getElementById('marginRatioDisplay').innerText = contributionRatio.toFixed(1) + "%";
            document.getElementById('targetUnitDisplay').innerText = Math.ceil(targetUnit).toLocaleString('id-ID') + " Pcs";

            // Update Simulation Slider Range
            const slider = document.getElementById('simQtySlider');
            const simInput = document.getElementById('simQtyInput');
            
            // Set minimal batas atas simulasi ke 10.000 unit (atau 2x BEP jika BEP lebih tinggi)
            const maxVal = Math.max(Math.ceil(bepUnit * 2), 10000);
            slider.max = maxVal;
            if (simInput) simInput.max = maxVal;

            document.getElementById('simMidLabel').innerText = Math.round(maxVal / 2).toLocaleString('id-ID') + " Unit";
            document.getElementById('simMaxLabel').innerText = maxVal.toLocaleString('id-ID') + " Unit";

            updateSimulation();
            renderChart(bepUnit, totalFixedCost, sellingPrice, variableCost);
        }

        // Sync slider ke input box
        function syncSimSlider(val) {
            const simInput = document.getElementById('simQtyInput');
            if (simInput) simInput.value = val;
            updateSimulation();
        }

        // Sync input box ke slider
        function syncSimInput(val) {
            const slider = document.getElementById('simQtySlider');
            let num = parseFloat(val) || 0;
            if (slider) slider.value = num;
            updateSimulation();
        }

        // Reset outputs to zero
        function resetOutputs() {
            document.getElementById('bepUnitDisplay').innerText = "0";
            document.getElementById('bepRupiahDisplay').innerText = "Rp 0";
            document.getElementById('marginUnitDisplay').innerText = "Rp 0";
            document.getElementById('marginRatioDisplay').innerText = "0%";
            document.getElementById('targetUnitDisplay').innerText = "0 Pcs";
            document.getElementById('simProfitDisplay').innerText = "Rp 0";
        }

        // Update Profit Simulation Slider values
        function updateSimulation() {
            const simQty = parseFloat(document.getElementById('simQtySlider').value) || 0;
            const sellingPrice = getInputValue('sellingPrice');
            const variableCost = getTotalVariableCost();
            const totalFixedCost = getTotalFixedCost();

            document.getElementById('simQtyBadge').innerText = simQty.toLocaleString('id-ID') + " Unit";
            const simInput = document.getElementById('simQtyInput');
            if (simInput && document.activeElement !== simInput) {
                simInput.value = simQty;
            }

            const revenue = simQty * sellingPrice;
            const totalCost = totalFixedCost + (simQty * variableCost);
            const profit = revenue - totalCost;

            const profitDisplay = document.getElementById('simProfitDisplay');
            const statusBadge = document.getElementById('simStatusBadge');

            profitDisplay.innerText = formatRupiah(profit);

            if (profit > 0) {
                profitDisplay.className = "font-mono font-bold text-sm text-emerald-600";
                statusBadge.innerText = "UNTUNG / LABA";
                statusBadge.className = "inline-block mt-0.5 text-[10px] font-extrabold px-2 py-0.5 rounded uppercase bg-emerald-100 text-emerald-800";
            } else if (profit < 0) {
                profitDisplay.className = "font-mono font-bold text-sm text-rose-600";
                statusBadge.innerText = "RUGI";
                statusBadge.className = "inline-block mt-0.5 text-[10px] font-extrabold px-2 py-0.5 rounded uppercase bg-rose-100 text-rose-800";
            } else {
                profitDisplay.className = "font-mono font-bold text-sm text-slate-800";
                statusBadge.innerText = "IMPAS / BEP";
                statusBadge.className = "inline-block mt-0.5 text-[10px] font-extrabold px-2 py-0.5 rounded uppercase bg-slate-100 text-slate-700";
            }
        }

        // Render or Update Chart.js BEP Visualization
        function renderChart(bepUnit, fixedCost, price, varCost) {
            const ctx = document.getElementById('bepChart').getContext('2d');

            const maxUnits = Math.max(Math.ceil(bepUnit * 2), 50);
            const step = Math.ceil(maxUnits / 5);
            
            const labels = [];
            const revenueData = [];
            const totalCostData = [];
            const fixedCostData = [];

            for (let u = 0; u <= maxUnits; u += step) {
                labels.push(u + " Unit");
                revenueData.push(u * price);
                totalCostData.push(fixedCost + (u * varCost));
                fixedCostData.push(fixedCost);
            }

            if (bepChartInstance) {
                bepChartInstance.destroy();
            }

            bepChartInstance = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Pendapatan (Revenue)',
                            data: revenueData,
                            borderColor: '#10B981',
                            backgroundColor: 'rgba(16, 185, 129, 0.05)',
                            borderWidth: 2.5,
                            fill: true,
                            tension: 0.1
                        },
                        {
                            label: 'Total Biaya (Fixed + Variable)',
                            data: totalCostData,
                            borderColor: '#EF4444',
                            backgroundColor: 'rgba(239, 68, 68, 0.05)',
                            borderWidth: 2.5,
                            fill: true,
                            tension: 0.1
                        },
                        {
                            label: 'Biaya Tetap (Fixed Cost)',
                            data: fixedCostData,
                            borderColor: '#94A3B8',
                            borderWidth: 1.5,
                            borderDash: [5, 5],
                            pointRadius: 0,
                            fill: false
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                font: { size: 10, family: 'Inter' },
                                boxWidth: 12
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return context.dataset.label + ': ' + formatRupiah(context.raw);
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            ticks: { font: { size: 9, family: 'Inter' } },
                            grid: { display: false }
                        },
                        y: {
                            ticks: {
                                font: { size: 9, family: 'Inter' },
                                callback: function(value) {
                                    return 'Rp ' + (value / 1000) + 'k';
                                }
                            }
                        }
                    }
                }
            });
        }

        // Load Sample Data with Dynamic Rows
        function loadSampleData() {
            document.getElementById('sellingPrice').value = 75000;
            
            // Re-populate Variable Costs
            document.getElementById('variableCostContainer').innerHTML = '';
            addVariableCostRow('HPP / Modal Barang', 40000);
            addVariableCostRow('Kemasan & Stiker', 5000);

            // Re-populate Fixed Costs
            document.getElementById('fixedCostContainer').innerHTML = '';
            addFixedCostRow('Sewa Tempat / Toko', 2500000);
            addFixedCostRow('Gaji Karyawan Tetap', 4000000);
            addFixedCostRow('Listrik, Air & Utilitas', 1000000);
            addFixedCostRow('Pemasaran & Iklan', 500000);

            document.getElementById('targetProfit').value = 5000000;

            calculateBEP();
        }

        // Reset all inputs
        function resetForm() {
            document.getElementById('sellingPrice').value = '';
            document.getElementById('variableCostContainer').innerHTML = '';
            document.getElementById('fixedCostContainer').innerHTML = '';
            document.getElementById('targetProfit').value = '';

            addVariableCostRow('HPP Modal Barang', '');
            addFixedCostRow('Sewa Tempat', '');

            calculateBEP();
        }

        // On DOM Load
        document.addEventListener('DOMContentLoaded', () => {
            loadSampleData();
        });
    </script>
</body>
</html>
