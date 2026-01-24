# Hoggs-dog.github.io

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fuel Card Price Analysis Dashboard</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://unpkg.com/recharts@2.5.0/dist/Recharts.js"></script>
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
            display: flex;
            align-items: center;
            gap: 0.5rem;
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
        
        .icon {
            width: 1rem;
            height: 1rem;
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
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;
        const { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, Cell } = Recharts;

        const RefreshIcon = ({ spinning }) => (
            <svg className={`icon ${spinning ? 'spinner' : ''}`} fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
            </svg>
        );

        const ClockIcon = () => (
            <svg className="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
        );

        const DownloadIcon = () => (
            <svg className="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
            </svg>
        );

        const TrendingDownIcon = () => (
            <svg className="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" style={{color: '#10b981', width: '1.25rem', height: '1.25rem'}}>
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6" />
            </svg>
        );

        const ChartIcon = () => (
            <svg className="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" style={{color: '#3b82f6', width: '1.25rem', height: '1.25rem'}}>
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
            </svg>
        );

        const FuelCardAnalyzer = () => {
            const [rawData, setRawData] = useState([]);
            const [analysis, setAnalysis] = useState(null);
            const [lastUpdated, setLastUpdated] = useState(null);
            const [isLoading, setIsLoading] = useState(false);
            const [error, setError] = useState(null);

            const SHEET_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vSH8koNGecLgCduEqSfurRr-hudolcyyCqvQaOSzCRk3dRhKia-rlslpLS1RAz2U8iwr8YjV8tT7X6n/pub?gid=1475315799&single=true&output=csv';
            const REFRESH_INTERVAL = 180000; // 3 minutes

            const parseCSV = (text) => {
                const lines = text.trim().split('\n');
                if (lines.length === 0) return [];
                
                const headers = lines[0].split(',').map(h => h.trim().replace(/^"|"$/g, ''));
                
                return lines.slice(1).map(line => {
                    const values = line.match(/(".*?"|[^,]+)(?=\s*,|\s*$)/g) || [];
                    const row = {};
                    headers.forEach((header, i) => {
                        row[header] = values[i]?.trim().replace(/^"|"$/g, '') || '';
                    });
                    return row;
                }).filter(row => Object.values(row).some(val => val !== ''));
            };

            const fetchData = async () => {
                setIsLoading(true);
                setError(null);
                
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
                    
                    if (data.length > 0) {
                        setRawData(data);
                        analyzeData(data);
                        setLastUpdated(new Date());
                    } else {
                        throw new Error('No valid data found in sheet');
                    }
                } catch (err) {
                    setError(`Unable to load data: ${err.message}`);
                    console.error('Error fetching data:', err);
                } finally {
                    setIsLoading(false);
                }
            };

            useEffect(() => {
                fetchData();
                const interval = setInterval(fetchData, REFRESH_INTERVAL);
                return () => clearInterval(interval);
            }, []);

            const analyzeData = (data) => {
                if (!data || data.length === 0) return;

                const normalizedData = data.map(row => {
                    const fuelCardType = row['What Type Of Fuel Card Do You Have'] || row['Other FC'] || '';
                    const supplier = row['Fuel Card Supplier'] || row['Other Supplier'] || '';
                    
                    return {
                        submittedAt: row['Submitted at'] || '',
                        fuelCardType: fuelCardType,
                        supplier: supplier,
                        ppl: parseFloat((row['Current ppl'] || '0').replace(/[£$,]/g, ''))
                    };
                }).filter(row => row.ppl > 0 && row.supplier && row.fuelCardType);

                if (normalizedData.length === 0) {
                    setError('No valid data found. Check your sheet format.');
                    return;
                }

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

                setAnalysis({
                    supplierAverages,
                    cardTypeAverages,
                    combinations,
                    totalSubmissions: normalizedData.length,
                    uniqueSuppliers: Object.keys(supplierStats).length,
                    uniqueCardTypes: Object.keys(cardTypeStats).length
                });
            };

            const formatLastUpdated = () => {
                if (!lastUpdated) return 'Never';
                const now = new Date();
                const diff = Math.floor((now - lastUpdated) / 1000);
                
                if (diff < 60) return `${diff} seconds ago`;
                if (diff < 3600) return `${Math.floor(diff / 60)} minutes ago`;
                return lastUpdated.toLocaleTimeString();
            };

            const exportToCSV = (dataArray, filename) => {
                if (!dataArray || dataArray.length === 0) return;
                
                const headers = Object.keys(dataArray[0]);
                const csv = [
                    headers.join(','),
                    ...dataArray.map(row => 
                        headers.map(header => {
                            const value = row[header];
                            return typeof value === 'number' ? value.toFixed(2) : `"${value}"`;
                        }).join(',')
                    )
                ].join('\n');

                const blob = new Blob([csv], { type: 'text/csv' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = filename;
                a.click();
                URL.revokeObjectURL(url);
            };

            const COLORS = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#ec4899', '#14b8a6', '#f97316'];

            return (
                <div className="container">
                    <div className="header">
                        <div>
                            <h1>Fuel Card Price Analysis</h1>
                            <p className="subtitle">Live data from user submissions - updated automatically</p>
                        </div>
                        <div className="header-right">
                            <button onClick={fetchData} disabled={isLoading} className="btn">
                                <RefreshIcon spinning={isLoading} />
                                Refresh Now
                            </button>
                            <div className="update-time">
                                <ClockIcon />
                                <span>Updated: {formatLastUpdated()}</span>
                            </div>
                        </div>
                    </div>

                    {error && (
                        <div className="error-box">
                            ⚠️ {error}
                        </div>
                    )}

                    {analysis && (
                        <>
                            <div className="stats-grid">
                                <div className="stat-card">
                                    <h3 className="stat-label">Total Submissions</h3>
                                    <p className="stat-value blue">{analysis.totalSubmissions}</p>
                                </div>
                                <div className="stat-card">
                                    <h3 className="stat-label">Fuel Card Suppliers</h3>
                                    <p className="stat-value green">{analysis.uniqueSuppliers}</p>
                                </div>
                                <div className="stat-card">
                                    <h3 className="stat-label">Card Types</h3>
                                    <p className="stat-value purple">{analysis.uniqueCardTypes}</p>
                                </div>
                            </div>

                            <div className="charts-grid">
                                <div className="card">
                                    <h2 className="card-title">Average Price by Supplier</h2>
                                    <ResponsiveContainer width="100%" height={300}>
                                        <BarChart data={analysis.supplierAverages}>
                                            <CartesianGrid strokeDasharray="3 3" />
                                            <XAxis dataKey="supplier" angle={-45} textAnchor="end" height={100} interval={0} fontSize={12} />
                                            <YAxis label={{ value: 'Price (ppl)', angle: -90, position: 'insideLeft' }} />
                                            <Tooltip formatter={(value) => `£${value.toFixed(2)}`} />
                                            <Bar dataKey="avgPrice">
                                                {analysis.supplierAverages.map((entry, index) => (
                                                    <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
                                                ))}
                                            </Bar>
                                        </BarChart>
                                    </ResponsiveContainer>
                                </div>

                                <div className="card">
                                    <h2 className="card-title">Average Price by Card Type</h2>
                                    <ResponsiveContainer width="100%" height={300}>
                                        <BarChart data={analysis.cardTypeAverages}>
                                            <CartesianGrid strokeDasharray="3 3" />
                                            <XAxis dataKey="cardType" angle={-45} textAnchor="end" height={100} interval={0} fontSize={12} />
                                            <YAxis label={{ value: 'Price (ppl)', angle: -90, position: 'insideLeft' }} />
                                            <Tooltip formatter={(value) => `£${value.toFixed(2)}`} />
                                            <Bar dataKey="avgPrice">
                                                {analysis.cardTypeAverages.map((entry, index) => (
                                                    <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
                                                ))}
                                            </Bar>
                                        </BarChart>
                                    </ResponsiveContainer>
                                </div>
                            </div>

                            <div className="card">
                                <h2 className="card-title">
                                    <TrendingDownIcon />
                                    Cheapest Suppliers (Average Price)
                                </h2>
                                <div className="table-container">
                                    <table>
                                        <thead>
                                            <tr>
                                                <th>Rank</th>
                                                <th>Supplier</th>
                                                <th className="text-right">Avg Price (ppl)</th>
                                                <th className="text-right">Min</th>
                                                <th className="text-right">Max</th>
                                                <th className="text-right">Submissions</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            {analysis.supplierAverages.map((item, idx) => (
                                                <tr key={idx} className={idx < 3 ? 'row-highlight-green' : ''}>
                                                    <td className="font-semibold">{idx + 1}</td>
                                                    <td>{item.supplier}</td>
                                                    <td className="text-right font-semibold">£{item.avgPrice.toFixed(2)}</td>
                                                    <td className="text-right">£{item.minPrice.toFixed(2)}</td>
                                                    <td className="text-right">£{item.maxPrice.toFixed(2)}</td>
                                                    <td className="text-right">{item.submissions}</td>
                                                </tr>
                                            ))}
                                        </tbody>
                                    </table>
                                </div>
                            </div>

                            <div className="card">
                                <h2 className="card-title">
                                    <ChartIcon />
                                    Cheapest Card Types (Average Price)
                                </h2>
                                <div className="table-container">
                                    <table>
                                        <thead>
                                            <tr>
                                                <th>Rank</th>
                                                <th>Card Type</th>
                                                <th className="text-right">Avg Price (ppl)</th>
                                                <th className="text-right">Min</th>
                                                <th className="text-right">Max</th>
                                                <th className="text-right">Submissions</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            {analysis.cardTypeAverages.map((item, idx) => (
                                                <tr key={idx} className={idx < 3 ? 'row-highlight-blue' : ''}>
                                                    <td className="font-semibold">{idx + 1}</td>
                                                    <td>{item.cardType}</td>
                                                    <td className="text-right font-semibold">£{item.avgPrice.toFixed(2)}</td>
                                                    <td className="text-right">£{item.minPrice.toFixed(2)}</td>
                                                    <td className="text-right">£{item.maxPrice.toFixed(2)}</td>
                                                    <td className="text-right">{item.submissions}</td>
                                                </tr>
                                            ))}
                                        </tbody>
                                    </table>
                                </div>
                            </div>

                            <div className="card">
                                <div className="card-header">
                                    <h2 className="card-title">Best Supplier + Card Type Combinations</h2>
                                    <button onClick={() => exportToCSV(analysis.combinations, 'combinations.csv')} className="btn btn-small">
                                        <DownloadIcon />
                                        Export
                                    </button>
                                </div>
                                <div className="table-container">
                                    <table>
                                        <thead>
                                            <tr>
                                                <th>Rank</th>
                                                <th>Supplier</th>
                                                <th>Card Type</th>
                                                <th className="text-right">Avg Price (ppl)</th>
                                                <th className="text-right">Submissions</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            {analysis.combinations.slice(0, 10).map((item, idx) => (
                                                <tr key={idx} className={idx < 3 ? 'row-highlight-yellow' : ''}>
                                                    <td className="font-semibold">{idx + 1}</td>
                                                    <td>{item.supplier}</td>
                                                    <td>{item.cardType}</td>
                                                    <td className="text-right font-semibold">£{item.avgPrice.toFixed(2)}</td>
                                                    <td className="text-right">{item.submissions}</td>
                                                </tr>
                                            ))}
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </>
                    )}

                    {!analysis && !error && (
                        <div className="loading-container">
                            {isLoading && <div className="spinner"></div>}
                            <p style={{color: '#4b5563'}}>Loading data from Google Sheets...</p>
                        </div>
                    )}
                </div>
            );
        };

        ReactDOM.render(<FuelCardAnalyzer />, document.getElementById('root'));
    </script>
</body>
</html>
