# Hoggs-dog.github.io

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fuel Card Price Analysis Dashboard</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell', sans-serif;
            background: linear-gradient(to bottom right, #f8fafc, #dbeafe);
            min-height: 100vh;
        }
        
        .container {
            max-width: 1280px;
            margin: 0 auto;
            padding: 1.5rem;
        }
        
        .header {
            margin-bottom: 2rem;
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            flex-wrap: wrap;
            gap: 1rem;
        }
        
        h1 {
            font-size: 2.25rem;
            font-weight: bold;
            color: #1f2937;
            margin-bottom: 0.5rem;
        }
        
        .subtitle {
            color: #4b5563;
        }
        
        .header-right {
            text-align: right;
        }
        
        .btn {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem 1rem;
            background-color: #3b82f6;
            color: white;
            border: none;
            border-radius: 0.375rem;
            cursor: pointer;
            font-size: 0.875rem;
            transition: background-color 0.2s;
        }
        
        .btn:hover:not(:disabled) {
            background-color: #2563eb;
        }
        
        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .btn-small {
            padding: 0.25rem 0.75rem;
            font-size: 0.875rem;
        }
        
        .update-time {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-top: 0.5rem;
            font-size: 0.875rem;
            color: #4b5563;
        }
        
        .card {
            background: white;
            border-radius: 0.5rem;
            box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1rem;
            margin-bottom: 1.5rem;
        }
        
        .stat-card {
            background: white;
            border-radius: 0.5rem;
            box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
            padding: 1.5rem;
        }
        
        .stat-label {
            font-size: 0.875rem;
            color: #4b5563;
            margin-bottom: 0.25rem;
        }
        
        .stat-value {
            font-size: 1.875rem;
            font-weight: bold;
        }
        
        .stat-value.blue { color: #3b82f6; }
        .stat-value.green { color: #10b981; }
        .stat-value.purple { color: #8b5cf6; }
        
        .charts-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
            gap: 1.5rem;
            margin-bottom: 1.5rem;
        }
        
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }
        
        .card-title {
            font-size: 1.25rem;
            font-weight: 600;
        }
        
        .chart-container {
            position: relative;
            height: 300px;
        }
        
        .table-container {
            overflow-x: auto;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        thead {
            background-color: #f9fafb;
        }
        
        th {
            padding: 0.5rem 1rem;
            text-align: left;
            font-size: 0.75rem;
            font-weight: 500;
            color: #374151;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }
        
        th.text-right {
            text-align: right;
        }
        
        td {
            padding: 0.5rem 1rem;
            font-size: 0.875rem;
            color: #111827;
            border-top: 1px solid #e5e7eb;
        }
        
        td.text-right {
            text-align: right;
        }
        
        td.font-semibold {
            font-weight: 600;
        }
        
        .row-highlight-green {
            background-color: #f0fdf4;
        }
        
        .row-highlight-blue {
            background-color: #eff6ff;
        }
        
        .row-highlight-yellow {
            background-color: #fefce8;
        }
        
        .error-box {
            background-color: #fef2f2;
            border: 1px solid #fecaca;
            border-radius: 0.5rem;
            padding: 1rem;
            margin-bottom: 1.5rem;
            color: #991b1b;
        }
        
        .loading-container {
            background: white;
            border-radius: 0.5rem;
            box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
            padding: 3rem;
            text-align: center;
        }
        
        .spinner {
            display: inline-block;
            width: 3rem;
            height: 3rem;
            border: 4px solid #3b82f6;
            border-top-color: transparent;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 1rem;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        @media (max-width: 768px) {
            h1 {
                font-size: 1.875rem;
            }
            
            .header {
                flex-direction: column;
            }
            
            .header-right {
                text-align: left;
                width: 100%;
            }
            
            .charts-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div id="root">
        <div class="loading-container">
            <div class="spinner"></div>
            <p style="color: #4b5563;">Loading dashboard...</p>
        </div>
    </div>

    <script>
        const SHEET_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vSH8koNGecLgCduEqSfurRr-hudolcyyCqvQaOSzCRk3dRhKia-rlslpLS1RAz2U8iwr8YjV8tT7X6n/pub?gid=1475315799&single=true&output=csv';
        const REFRESH_INTERVAL = 180000;
        
        let supplierChart = null;
        let cardTypeChart = null;
        let lastUpdated = null;
        let isLoading = false;

        const COLORS = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#ec4899', '#14b8a6', '#f97316'];

        function parseCSV(text) {
            const lines = text.trim().split('\n');
            if (lines.length === 0) return [];
            
            // Parse headers
            const headerLine = lines[0];
            const headers = [];
            let current = '';
            let inQuotes = false;
            
            for (let i = 0; i < headerLine.length; i++) {
                const char = headerLine[i];
                if (char === '"') {
                    inQuotes = !inQuotes;
                } else if (char === ',' && !inQuotes) {
                    headers.push(current.trim().replace(/^"|"$/g, ''));
                    current = '';
                } else {
                    current += char;
                }
            }
            headers.push(current.trim().replace(/^"|"$/g, ''));
            
            console.log('CSV Headers:', headers);
            console.log('Expected columns:', headers.length);
            
            // Parse data rows
            const rows = [];
            for (let lineIdx = 1; lineIdx < lines.length; lineIdx++) {
                const line = lines[lineIdx];
                if (!line.trim()) continue;
                
                const values = [];
                let current = '';
                let inQuotes = false;
                
                for (let i = 0; i < line.length; i++) {
                    const char = line[i];
                    if (char === '"') {
                        inQuotes = !inQuotes;
                    } else if (char === ',' && !inQuotes) {
                        values.push(current.trim().replace(/^"|"$/g, ''));
                        current = '';
                    } else {
                        current += char;
                    }
                }
                values.push(current.trim().replace(/^"|"$/g, ''));
                
                // Create row object
                const row = {};
                headers.forEach((header, i) => {
                    row[header] = values[i] || '';
                });
                
                // Only include rows that have at least some data
                if (Object.values(row).some(val => val !== '')) {
                    rows.push(row);
                }
            }
            
            console.log(`Parsed ${rows.length} rows from CSV`);
            console.log('First row:', rows[0]);
            return rows;
        }

        function analyzeData(data) {
            console.log('First row sample:', data[0]);
            
            let missingSupplier = 0;
            let missingPrice = 0;
            let bothValid = 0;
            
            const normalizedData = data.map(row => {
                // Get fuel card type, default to "Unknown" if blank
                const fuelCardType = row['What Type Of Fuel Card Do You Have'] || row['Other FC'] || 'Unknown';
                // Get supplier, default to empty string
                const supplier = row['Fuel Card Supplier'] || row['Other Supplier'] || '';
                // Get price, parse to number
                const pplString = row['Current ppl'] || '0';
                const ppl = parseFloat(pplString.toString().replace(/[£$,]/g, ''));
                
                return {
                    submittedAt: row['Submitted at'] || '',
                    fuelCardType: fuelCardType,
                    supplier: supplier,
                    ppl: ppl
                };
            }).filter(row => {
                // Only filter: must have price > 0 AND must have supplier
                const hasValidPrice = row.ppl > 0;
                const hasSupplier = row.supplier && row.supplier.trim() !== '';
                
                if (!hasSupplier) missingSupplier++;
                if (!hasValidPrice) missingPrice++;
                if (hasValidPrice && hasSupplier) bothValid++;
                
                return hasValidPrice && hasSupplier;
            });

            console.log(`Filter results: ${bothValid} valid, ${missingSupplier} missing supplier, ${missingPrice} missing price`);

            if (normalizedData.length === 0) {
                showError('No valid data found. Make sure your sheet has rows with both Supplier and Price filled in.');
                return null;
            }

            console.log(`Found ${normalizedData.length} valid entries`);

            const supplierStats = {};
            normalizedData.forEach(row => {
                if (!supplierStats[row.supplier]) {
                    supplierStats[row.supplier] = { total: 0, count: 0, prices: [] };
                }
                supplierStats[row.supplier].total += row.ppl;
                supplierStats[row.supplier].count += 1;
                supplierStats[row.supplier].prices.push(row.ppl);
            });

            const supplierAverages = Object.entries(supplierStats).map(([supplier, stats]) => ({
                supplier,
                avgPrice: stats.total / stats.count,
                submissions: stats.count,
                minPrice: Math.min(...stats.prices),
                maxPrice: Math.max(...stats.prices)
            })).sort((a, b) => a.avgPrice - b.avgPrice);

            const cardTypeStats = {};
            normalizedData.forEach(row => {
                if (!cardTypeStats[row.fuelCardType]) {
                    cardTypeStats[row.fuelCardType] = { total: 0, count: 0, prices: [] };
                }
                cardTypeStats[row.fuelCardType].total += row.ppl;
                cardTypeStats[row.fuelCardType].count += 1;
                cardTypeStats[row.fuelCardType].prices.push(row.ppl);
            });

            const cardTypeAverages = Object.entries(cardTypeStats).map(([cardType, stats]) => ({
                cardType,
                avgPrice: stats.total / stats.count,
                submissions: stats.count,
                minPrice: Math.min(...stats.prices),
                maxPrice: Math.max(...stats.prices)
            })).sort((a, b) => a.avgPrice - b.avgPrice);

            const combinationStats = {};
            normalizedData.forEach(row => {
                const key = `${row.supplier} - ${row.fuelCardType}`;
                if (!combinationStats[key]) {
                    combinationStats[key] = { 
                        supplier: row.supplier,
                        cardType: row.fuelCardType,
                        total: 0, 
                        count: 0, 
                        prices: [] 
                    };
                }
                combinationStats[key].total += row.ppl;
                combinationStats[key].count += 1;
                combinationStats[key].prices.push(row.ppl);
            });

            const combinations = Object.values(combinationStats).map(stats => ({
                combination: `${stats.supplier} - ${stats.cardType}`,
                supplier: stats.supplier,
                cardType: stats.cardType,
                avgPrice: stats.total / stats.count,
                submissions: stats.count,
                minPrice: Math.min(...stats.prices),
                maxPrice: Math.max(...stats.prices)
            })).sort((a, b) => a.avgPrice - b.avgPrice);

            return {
                supplierAverages,
                cardTypeAverages,
                combinations,
                totalSubmissions: normalizedData.length,
                uniqueSuppliers: Object.keys(supplierStats).length,
                uniqueCardTypes: Object.keys(cardTypeStats).length
            };
        }

        function showError(message) {
            document.getElementById('root').innerHTML = `
                <div class="container">
                    <div class="error-box">⚠️ ${message}</div>
                </div>
            `;
        }

        function formatLastUpdated() {
            if (!lastUpdated) return 'Never';
            const now = new Date();
            const diff = Math.floor((now - lastUpdated) / 1000);
            
            if (diff < 60) return `${diff} seconds ago`;
            if (diff < 3600) return `${Math.floor(diff / 60)} minutes ago`;
            return lastUpdated.toLocaleTimeString();
        }

        function updateTimeDisplay() {
            const timeEl = document.getElementById('update-time');
            if (timeEl) {
                timeEl.textContent = `Updated: ${formatLastUpdated()}`;
            }
        }

        function createCharts(analysis) {
            // Supplier chart
            if (supplierChart) supplierChart.destroy();
            const supplierCtx = document.getElementById('supplierChart');
            if (supplierCtx) {
                supplierChart = new Chart(supplierCtx, {
                    type: 'bar',
                    data: {
                        labels: analysis.supplierAverages.map(s => s.supplier),
                        datasets: [{
                            label: 'Average Price (ppl)',
                            data: analysis.supplierAverages.map(s => s.avgPrice),
                            backgroundColor: analysis.supplierAverages.map((_, i) => COLORS[i % COLORS.length]),
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: { display: false },
                            tooltip: {
                                callbacks: {
                                    label: (context) => `£${context.parsed.y.toFixed(2)}`
                                }
                            }
                        },
                        scales: {
                            y: {
                                beginAtZero: true,
                                title: { display: true, text: 'Price (ppl)' }
                            }
                        }
                    }
                });
            }

            // Card type chart
            if (cardTypeChart) cardTypeChart.destroy();
            const cardTypeCtx = document.getElementById('cardTypeChart');
            if (cardTypeCtx) {
                cardTypeChart = new Chart(cardTypeCtx, {
                    type: 'bar',
                    data: {
                        labels: analysis.cardTypeAverages.map(c => c.cardType),
                        datasets: [{
                            label: 'Average Price (ppl)',
                            data: analysis.cardTypeAverages.map(c => c.avgPrice),
                            backgroundColor: analysis.cardTypeAverages.map((_, i) => COLORS[i % COLORS.length]),
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: { display: false },
                            tooltip: {
                                callbacks: {
                                    label: (context) => `£${context.parsed.y.toFixed(2)}`
                                }
                            }
                        },
                        scales: {
                            y: {
                                beginAtZero: true,
                                title: { display: true, text: 'Price (ppl)' }
                            }
                        }
                    }
                });
            }
        }

        function renderDashboard(analysis) {
            document.getElementById('root').innerHTML = `
                <div class="container">
                    <div class="header">
                        <div>
                            <h1>Fuel Card Price Analysis</h1>
                            <p class="subtitle">Live data from user submissions - updated automatically</p>
                        </div>
                        <div class="header-right">
                            <button onclick="fetchData()" id="refresh-btn" class="btn">
                                🔄 Refresh Now
                            </button>
                            <div class="update-time">
                                🕐 <span id="update-time">Updated: ${formatLastUpdated()}</span>
                            </div>
                        </div>
                    </div>

                    <div class="stats-grid">
                        <div class="stat-card">
                            <h3 class="stat-label">Total Submissions</h3>
                            <p class="stat-value blue">${analysis.totalSubmissions}</p>
                        </div>
                        <div class="stat-card">
                            <h3 class="stat-label">Fuel Card Suppliers</h3>
                            <p class="stat-value green">${analysis.uniqueSuppliers}</p>
                        </div>
                        <div class="stat-card">
                            <h3 class="stat-label">Card Types</h3>
                            <p class="stat-value purple">${analysis.uniqueCardTypes}</p>
                        </div>
                    </div>

                    <div class="charts-grid">
                        <div class="card">
                            <h2 class="card-title">Average Price by Supplier</h2>
                            <div class="chart-container">
                                <canvas id="supplierChart"></canvas>
                            </div>
                        </div>

                        <div class="card">
                            <h2 class="card-title">Average Price by Card Type</h2>
                            <div class="chart-container">
                                <canvas id="cardTypeChart"></canvas>
                            </div>
                        </div>
                    </div>

                    <div class="card">
                        <h2 class="card-title">📉 Cheapest Suppliers (Average Price)</h2>
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Rank</th>
                                        <th>Supplier</th>
                                        <th class="text-right">Avg Price (ppl)</th>
                                        <th class="text-right">Min</th>
                                        <th class="text-right">Max</th>
                                        <th class="text-right">Submissions</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${analysis.supplierAverages.map((item, idx) => `
                                        <tr class="${idx < 3 ? 'row-highlight-green' : ''}">
                                            <td class="font-semibold">${idx + 1}</td>
                                            <td>${item.supplier}</td>
                                            <td class="text-right font-semibold">£${item.avgPrice.toFixed(2)}</td>
                                            <td class="text-right">£${item.minPrice.toFixed(2)}</td>
                                            <td class="text-right">£${item.maxPrice.toFixed(2)}</td>
                                            <td class="text-right">${item.submissions}</td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        </div>
                    </div>

                    <div class="card">
                        <h2 class="card-title">📊 Cheapest Card Types (Average Price)</h2>
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Rank</th>
                                        <th>Card Type</th>
                                        <th class="text-right">Avg Price (ppl)</th>
                                        <th class="text-right">Min</th>
                                        <th class="text-right">Max</th>
                                        <th class="text-right">Submissions</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${analysis.cardTypeAverages.map((item, idx) => `
                                        <tr class="${idx < 3 ? 'row-highlight-blue' : ''}">
                                            <td class="font-semibold">${idx + 1}</td>
                                            <td>${item.cardType}</td>
                                            <td class="text-right font-semibold">£${item.avgPrice.toFixed(2)}</td>
                                            <td class="text-right">£${item.minPrice.toFixed(2)}</td>
                                            <td class="text-right">£${item.maxPrice.toFixed(2)}</td>
                                            <td class="text-right">${item.submissions}</td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        </div>
                    </div>

                    <div class="card">
                        <h2 class="card-title">🏆 Best Supplier + Card Type Combinations</h2>
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Rank</th>
                                        <th>Supplier</th>
                                        <th>Card Type</th>
                                        <th class="text-right">Avg Price (ppl)</th>
                                        <th class="text-right">Submissions</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${analysis.combinations.slice(0, 10).map((item, idx) => `
                                        <tr class="${idx < 3 ? 'row-highlight-yellow' : ''}">
                                            <td class="font-semibold">${idx + 1}</td>
                                            <td>${item.supplier}</td>
                                            <td>${item.cardType}</td>
                                            <td class="text-right font-semibold">£${item.avgPrice.toFixed(2)}</td>
                                            <td class="text-right">${item.submissions}</td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            `;

            createCharts(analysis);
        }

        async function fetchData() {
            if (isLoading) return;
            isLoading = true;
            
            const btn = document.getElementById('refresh-btn');
            if (btn) {
                btn.disabled = true;
                btn.innerHTML = '⏳ Loading...';
            }
            
            try {
                const response = await fetch(SHEET_URL);
                
                if (!response.ok) {
                    throw new Error('Failed to fetch data from Google Sheets');
                }
                
                const text = await response.text();
                
                if (!text || text.trim().length === 0) {
                    throw new Error('Empty response from Google Sheets');
                }
                
                const data = parseCSV(text);
                const analysis = analyzeData(data);
                
                if (analysis) {
                    renderDashboard(analysis);
                    lastUpdated = new Date();
                }
            } catch (err) {
                showError(`Unable to load data: ${err.message}`);
                console.error('Error fetching data:', err);
            } finally {
                isLoading = false;
                const btn = document.getElementById('refresh-btn');
                if (btn) {
                    btn.disabled = false;
                    btn.innerHTML = '🔄 Refresh Now';
                }
            }
        }

        // Initial load
        if (typeof Chart !== 'undefined') {
            fetchData();
            setInterval(fetchData, REFRESH_INTERVAL);
            setInterval(updateTimeDisplay, 10000); // Update time every 10 seconds
        } else {
            showError('Chart library failed to load. Please refresh the page.');
        }
    </script>
</body>
</html>
