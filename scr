<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Forest Watch - Deforestation Reporter</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Inter Font -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0fdf4; /* Very Light Green/Off-White */
        }
        .header-bg {
            background-color: #166534; /* Dark Forest Green */
        }
        .text-green-900 {
            color: #14532d; /* Even darker green for strong text */
        }
        .btn-submit {
            background-color: #dc2626; /* Red for urgent action/alert */
            transition: all 0.2s;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
        }
        .btn-submit:hover:not(:disabled) {
            background-color: #b91c1c;
        }
        .btn-submit:disabled {
            background-color: #9ca3af; /* Gray when disabled */
            cursor: not-allowed;
        }
        .card-shadow {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        
        /* Report Status Styling */
        .report-status-pending { border-color: #fbbf24; background-color: #fffbeb; color: #92400e; }
        .report-status-verified { border-color: #10b981; background-color: #ecfdf5; color: #047857; }
        .report-status-rejected { border-color: #ef4444; background-color: #fef2f2; color: #b91c1c; }
        
        /* Navigation Tabs */
        .nav-tab {
            cursor: pointer;
            padding: 8px 16px;
            border-radius: 8px;
            color: #d1d5db; /* Light gray */
            font-weight: 600;
        }
        .nav-tab.active {
            background-color: #ecfdf5; /* Light mint */
            color: #14532d; /* Dark green */
            display: inline-block;
        }
        .nav-tab:not(.active) {
            display: inline-block;
        }
        .nav-tab:hover {
            background-color: #15803d; /* Lighter green */
        }
        
        /* Local Doughnut Chart Styling */
        .doughnut-chart {
            width: 300px;
            height: 300px;
            border-radius: 50%;
            position: relative;
            /* Placeholder gradient - updated by JS */
            background: conic-gradient(#fbbf24 0% 33%, #10b981 33% 66%, #ef4444 66% 100%); 
            transition: background 0.5s ease-in-out;
        }
        .doughnut-chart::after {
            content: '';
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            width: 70%;
            height: 70%;
            background: #fff;
            border-radius: 50%;
        }
        .chart-center-text {
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            z-index: 1;
            text-align: center;
        }

        /* Global Bar Chart Styling */
        .bar-chart-container {
            display: flex;
            align-items: flex-end;
            gap: 16px;
            height: 250px;
            padding-bottom: 20px;
            border-bottom: 2px solid #e5e7eb;
        }
        .bar-wrapper {
            flex: 1;
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            position: relative;
        }
        .bar-year-label {
            font-size: 0.8rem;
            color: #4b5563;
            margin-top: 5px;
        }
        .bar-segment {
            width: 100%;
            transition: height 1s ease-out;
            border-radius: 4px 4px 0 0;
            position: relative;
        }
        .bar-loss {
            background-color: #dc2626; /* Red for loss */
        }
        .bar-remaining {
            background-color: #10b981; /* Green for remaining/gain */
        }
        .bar-tooltip {
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.75rem;
            white-space: nowrap;
            opacity: 0;
            transition: opacity 0.2s;
            pointer-events: none;
        }
        .bar-wrapper:hover .bar-tooltip {
            opacity: 1;
        }
        
        /* Hide dashboard tab until reports are submitted */
        #nav-dashboard {
            display: none;
        }
    </style>
</head>
<body class="min-h-screen">
    <header class="header-bg p-4 sticky top-0 z-10">
        <div class="max-w-7xl mx-auto flex justify-between items-center">
            <!-- Left Side: Title -->
            <h1 class="text-2xl font-bold text-white flex items-center">
                <svg class="w-6 h-6 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                Forest Watch
            </h1>
            
            <!-- Center: Navigation -->
            <nav class="flex space-x-2">
                <div id="nav-feed" class="nav-tab active" onclick="showFeedView()">Report Feed</div>
                <div id="nav-dashboard" class="nav-tab" onclick="showDashboardView()">Dashboard</div>
            </nav>
            
            <!-- Right Side: User Info -->
            <div class="flex items-center space-x-4">
                <span id="user-info" class="text-sm text-gray-300 truncate max-w-xs">Connecting...</span>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-6 lg:p-10">
        <!-- Message Box Container -->
        <div id="message-box-container" class="fixed top-20 right-5 z-20 w-full max-w-sm"></div>

        <!-- View 1: Report Feed (Default View) -->
        <div id="feed-view">
            <h2 class="text-4xl font-extrabold text-green-900 mb-8">Report Deforestation</h2>
        
            <!-- Report Submission Section -->
            <div class="bg-white p-6 rounded-xl mb-12 card-shadow">
                <h3 class="text-2xl font-semibold mb-4 text-green-900">Submit a New Incident</h3>
                <div class="space-y-4">
                    <input id="report-title" type="text" placeholder="Title (e.g., Logging near Amazon border)" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500">
                    <input id="report-location" type="text" placeholder="Location (e.g., GPS Coordinates or Area Name)" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500">
                    <input id="report-image-url" type="url" placeholder="Evidence Photo URL (Paste link to image)" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500">
                    <textarea id="report-description" placeholder="Describe the incident (Who, What, When, Impact)" rows="4" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500"></textarea>
                    <button id="submit-button" onclick="submitReport()" class="btn-submit text-white font-bold py-3 px-6 rounded-lg w-full md:w-auto" disabled>
                        Authenticating...
                    </button>
                </div>
            </div>

            <!-- Admin Panel (Conditional Display) -->
            <section id="admin-panel" class="mb-12 hidden">
                <h3 class="text-3xl font-extrabold text-red-700 mb-6">ADMIN REVIEW PANEL</h3>
                <div id="admin-reports-feed" class="space-y-6">
                    <p class="text-gray-500 text-center">Loading all reports for review...</p>
                </div>
            </section>
            
            <!-- Public Feed Section -->
            <section class="mb-12">
                <h3 class="text-3xl font-extrabold text-green-900 mb-6">Verified Deforestation Reports</h3>
                <div id="public-feed" class="space-y-6">
                    <!-- Verified reports will be dynamically loaded here -->
                    <p id="loading-message-public" class="text-gray-500 text-center">Connecting to server and loading reports...</p>
                </div>
            </section>
        </div>
        
        <!-- View 2: Dashboard (Hidden by Default) -->
        <div id="dashboard-view" class="hidden">
            <h2 class="text-4xl font-extrabold text-green-900 mb-8">Reports Dashboard</h2>
            
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                <!-- Column 1: Local Report Status -->
                <div class="lg:col-span-1 bg-white p-6 md:p-8 rounded-xl card-shadow">
                    <h3 class="text-xl font-bold mb-6 text-green-900 text-center border-b pb-4">Local Report Status</h3>
                    
                    <div class="flex flex-col justify-center items-center gap-6">
                        <!-- Chart -->
                        <div class="relative">
                            <div id="doughnut-chart" class="doughnut-chart"></div>
                            <div class="chart-center-text">
                                <span id="total-reports-text" class="text-4xl font-bold text-green-900">0</span>
                                <span class="block text-gray-500 text-sm">Total Reports</span>
                            </div>
                        </div>
                        
                        <!-- Legend -->
                        <div class="chart-legend">
                            <ul class="space-y-2">
                                <li class="flex items-center">
                                    <div class="w-3 h-3 rounded-full bg-yellow-400 mr-2"></div>
                                    <span id="legend-pending" class="text-gray-700">Pending: 0 (0%)</span>
                                </li>
                                <li class="flex items-center">
                                    <div class="w-3 h-3 rounded-full bg-green-500 mr-2"></div>
                                    <span id="legend-verified" class="text-gray-700">Verified: 0 (0%)</span>
                                </li>
                                <li class="flex items-center">
                                    <div class="w-3 h-3 rounded-full bg-red-500 mr-2"></div>
                                    <span id="legend-rejected" class="text-gray-700">Rejected: 0 (0%)</span>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>

                <!-- Column 2: Global Context & Trend Chart -->
                <div class="lg:col-span-2 bg-white p-6 rounded-xl card-shadow flex flex-col justify-between">
                    <div>
                        <h3 class="text-xl font-bold mb-4 text-red-700 border-b pb-2">Global Deforestation Trend (2021-2025)</h3>
                        <div id="global-data-container" class="mb-6">
                            <p class="text-gray-500 animate-pulse">Fetching current global trends and context...</p>
                        </div>

                        <!-- Bar Chart Area -->
                        <div id="global-trend-chart">
                            <!-- Chart will be rendered here -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
    </main>

    <!-- Firebase Imports -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, query, orderBy, onSnapshot, doc, addDoc, updateDoc, Timestamp, where, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // 1. ENVIRONMENT VARIABLES & GLOBAL SETUP
        
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}'); 
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        
        window.firebase = {
            app: null,
            db: null,
            auth: null,
            userId: null,
            // For demo purposes, we set the initial admin ID to the user's eventual UID
            adminId: null, 
            dbReady: false,
            appId: appId,
            totalReports: 0
        };

        const userInfoEl = document.getElementById('user-info');
        const submitButtonEl = document.getElementById('submit-button');
        
        if (Object.keys(firebaseConfig).length > 0) {
            window.firebase.app = initializeApp(firebaseConfig);
            window.firebase.db = getFirestore(window.firebase.app);
            window.firebase.auth = getAuth(window.firebase.app);

            setLogLevel('Debug');

            // 2. AUTHENTICATION & INITIAL SIGN-IN
            onAuthStateChanged(window.firebase.auth, async (user) => {
                if (user) {
                    // User is signed in
                    window.firebase.userId = user.uid;
                    // If adminId hasn't been set yet (first sign-in), set it now
                    if (!window.firebase.adminId) {
                        window.firebase.adminId = user.uid;
                    }
                    const isCurrentUserAdmin = user.uid === window.firebase.adminId;
                    
                    userInfoEl.textContent = isCurrentUserAdmin 
                        ? `ADMIN: ${user.uid.substring(0, 8)}...`
                        : `User ID: ${user.uid.substring(0, 8)}...`;
                        
                    // *** CRITICAL FIX: Set dbReady and enable button after successful auth ***
                    window.firebase.dbReady = true;
                    submitButtonEl.disabled = false;
                    submitButtonEl.textContent = 'Submit Alert (Needs Admin Verification)';
                    console.log("Firebase initialized and user authenticated:", user.uid);
                    
                    startDataListeners(isCurrentUserAdmin);

                } else {
                    // User is not signed in (Initial load state). Attempt sign-in.
                    try {
                        userInfoEl.textContent = 'Authenticating...';
                        if (initialAuthToken) {
                            await signInWithCustomToken(window.firebase.auth, initialAuthToken);
                        } else {
                            // If no token, sign in anonymously
                            await signInAnonymously(window.firebase.auth);
                        }
                        // Note: A successful sign-in will trigger onAuthStateChanged again with the user object.
                    } catch (error) {
                        console.error("Authentication failed:", error);
                        userInfoEl.textContent = 'Auth Failed! Check console.';
                        submitButtonEl.disabled = true;
                        submitButtonEl.textContent = 'Authentication Failed';
                    }
                }
            });
        } else {
            // Handle missing Firebase config
            document.getElementById('loading-message-public').textContent = 'Firebase configuration missing. Cannot load real-time data.';
            userInfoEl.textContent = 'Config Error';
            submitButtonEl.disabled = true;
            submitButtonEl.textContent = 'Config Error';
            console.error('Firebase configuration not found.');
        }

        // 3. FIRESTORE COLLECTION REFERENCE
        const getReportsCollectionRef = () => {
            if (!window.firebase.dbReady || !window.firebase.db) {
                // If not ready, return null. The calling functions must handle this.
                return null;
            }
            // Public data path: /artifacts/{appId}/public/data/reports
            return collection(window.firebase.db, `artifacts/${window.firebase.appId}/public/data/reports`);
        };

        // 4. REAL-TIME DATA LISTENERS
        function startDataListeners(isCurrentUserAdmin) {
            const reportsRef = getReportsCollectionRef();
            if (!reportsRef) {
                // This shouldn't happen if called from onAuthStateChanged with user, but safe check.
                userInfoEl.textContent = 'DB Connection Error';
                return; 
            }

            // Listener 1: For the PUBLIC FEED (ONLY verified reports)
            const publicQ = query(reportsRef, where('status', '==', 'verified')); 

            onSnapshot(publicQ, (snapshot) => {
                const feedEl = document.getElementById('public-feed');
                
                let reports = [];
                snapshot.forEach(doc => reports.push({ id: doc.id, ...doc.data() }));
                // Sort by creation date (newest first)
                reports.sort((a, b) => b.createdAt.seconds - a.createdAt.seconds);

                feedEl.innerHTML = '';
                if (reports.length === 0) {
                     feedEl.innerHTML = '<p class="text-gray-500 text-center">No verified reports yet. Stay vigilant!</p>';
                } else {
                    reports.forEach(report => {
                        feedEl.appendChild(createReportCard(report.id, report, false));
                    });
                }
            }, (error) => {
                console.error("Error fetching public reports:", error);
                document.getElementById('public-feed').innerHTML = '<p class="text-red-500 text-center">Error loading public reports. Check security rules.</p>';
            });

            // Listener 2: For ALL reports (Admin Panel & Dashboard Chart data)
            const allReportsQ = query(reportsRef, orderBy('createdAt', 'desc'));

            onSnapshot(allReportsQ, (snapshot) => {
                const adminFeedEl = document.getElementById('admin-reports-feed');
                
                let reportsForAdmin = [];
                let stats = { total: 0, pending: 0, verified: 0, rejected: 0 };

                snapshot.forEach(doc => {
                    const data = doc.data();
                    reportsForAdmin.push({ id: doc.id, ...data });

                    // Calculate stats for dashboard
                    stats.total++;
                    if (data.status === 'pending') stats.pending++;
                    else if (data.status === 'verified') stats.verified++;
                    else if (data.status === 'rejected') stats.rejected++;
                });

                window.firebase.totalReports = stats.total;
                
                // 1. Show Dashboard tab if there are reports
                if (window.firebase.totalReports > 0) {
                    document.getElementById('nav-dashboard').style.display = 'inline-block';
                    // Only fetch global data if the dashboard is currently visible to avoid unnecessary API calls
                    if (document.getElementById('dashboard-view').classList.contains('hidden') === false) {
                        window.fetchGlobalDeforestationTrend();
                    }
                }

                // 2. Update the Dashboard Chart (for ALL users)
                updateLocalDashboard(stats);

                // 3. Update the Admin Panel (ONLY if user is admin)
                if (isCurrentUserAdmin) {
                    document.getElementById('admin-panel').classList.remove('hidden');
                    if (snapshot.empty) {
                        adminFeedEl.innerHTML = '<p class="text-gray-500 text-center">No reports in the system.</p>';
                        return;
                    }
                    adminFeedEl.innerHTML = '';
                    reportsForAdmin.forEach(report => {
                        // Pass 'true' to indicate Admin View, enabling controls
                        adminFeedEl.appendChild(createReportCard(report.id, report, true)); 
                    });
                }
                
            }, (error) => {
                console.error("Error fetching all reports:", error);
                if (isCurrentUserAdmin) {
                    document.getElementById('admin-reports-feed').innerHTML = '<p class="text-red-500 text-center">Error loading admin reports.</p>';
                }
            });
        }
        
        // 5. LOCAL DASHBOARD (Doughnut Chart) UPDATE FUNCTION
        window.updateLocalDashboard = (stats) => {
            const { total, pending, verified, rejected } = stats;
            
            const chartEl = document.getElementById('doughnut-chart');
            const totalTextEl = document.getElementById('total-reports-text');
            const legendPendingEl = document.getElementById('legend-pending');
            const legendVerifiedEl = document.getElementById('legend-verified');
            const legendRejectedEl = document.getElementById('legend-rejected');

            if (total === 0) {
                chartEl.style.background = '#e5e7eb';
                totalTextEl.textContent = '0';
                legendPendingEl.textContent = 'Pending: 0 (0%)';
                legendVerifiedEl.textContent = 'Verified: 0 (0%)';
                legendRejectedEl.textContent = 'Rejected: 0 (0%)';
                return;
            }

            const pPending = (pending / total) * 100;
            const pVerified = (verified / total) * 100;
            const pRejected = (rejected / total) * 100;

            const gradPending = ` #fbbf24 0% ${pPending}%`;
            const gradVerified = `#10b981 ${pPending}% ${pPending + pVerified}%`;
            const gradRejected = `#ef4444 ${pPending + pVerified}% 100%`;
            
            chartEl.style.background = `conic-gradient(${gradPending}, ${gradVerified}, ${gradRejected})`;
            
            totalTextEl.textContent = total;
            legendPendingEl.textContent = `Pending: ${pending} (${pPending.toFixed(1)}%)`;
            legendVerifiedEl.textContent = `Verified: ${verified} (${pVerified.toFixed(1)}%)`;
            legendRejectedEl.textContent = `Rejected: ${rejected} (${pRejected.toFixed(1)}%)`;
        };
        
        // 6. GLOBAL DATA FETCHING & RENDERING (Uses Gemini API for context)
        
        // Simulated 5-year trend data for reliable chart rendering
        const GLOBAL_DEFORESTATION_DATA = [
            { year: 2021, loss_percent: 15, remaining_percent: 85, loss_value: '10.3M ha' },
            { year: 2022, loss_percent: 16, remaining_percent: 84, loss_value: '11.1M ha' },
            { year: 2023, loss_percent: 14, remaining_percent: 86, loss_value: '9.5M ha' },
            { year: 2024, loss_percent: 17, remaining_percent: 83, loss_value: '11.8M ha' },
            { year: 2025, loss_percent: 16.5, remaining_percent: 83.5, loss_value: '11.5M ha' }
        ];

        window.fetchGlobalDeforestationTrend = async () => {
            // Render the trend chart immediately with the representative data
            renderGlobalTrendChart(GLOBAL_DEFORESTATION_DATA);

            const dataContainer = document.getElementById('global-data-container');
            const apiKey = ""; 
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
            
            dataContainer.innerHTML = '<p class="text-gray-500 animate-pulse">Fetching global trend context...</p>';

            const systemPrompt = "You are a data journalist. Provide a concise, single-paragraph summary of the global deforestation trend between 2021 and 2025. Focus on the overall change in annual loss and any key drivers. Do not include any made-up numbers, only factual trends from the search results.";
            const userQuery = "Summarize the global deforestation trend from 2021 to 2025, including annual loss and key drivers.";

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                tools: [{ "google_search": {} }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };

            let response;
            for (let i = 0; i < 3; i++) {
                try {
                    response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (response.ok) break;
                    await new Promise(resolve => setTimeout(resolve, Math.pow(2, i) * 1000));
                } catch (e) {
                    await new Promise(resolve => setTimeout(resolve, Math.pow(2, i) * 1000));
                }
            }
            
            if (!response || !response.ok) {
                dataContainer.innerHTML = '<p class="text-red-500">Could not fetch live global trend context.</p>';
                return;
            }

            try {
                const result = await response.json();
                const candidate = result.candidates?.[0];

                if (candidate && candidate.content?.parts?.[0]?.text) {
                    const text = candidate.content.parts[0].text;
                    let sources = [];
                    const groundingMetadata = candidate.groundingMetadata;

                    if (groundingMetadata && groundingMetadata.groundingAttributions) {
                        sources = groundingMetadata.groundingAttributions
                            .map(attribution => ({
                                uri: attribution.web?.uri,
                                title: attribution.web?.title,
                            }))
                            .filter(source => source.uri && source.title); 
                    }

                    let sourceHtml = '';
                    if (sources.length > 0) {
                        sourceHtml = `<p class="text-xs mt-3 text-gray-500">Context Source: <a href="${sources[0].uri}" target="_blank" class="underline hover:text-red-600">${sources[0].title.substring(0, 50)}...</a></p>`;
                    }

                    dataContainer.innerHTML = `
                        <p class="text-base font-medium text-gray-700 leading-relaxed">${text}</p>
                        ${sourceHtml}
                    `;
                } else {
                    dataContainer.innerHTML = '<p class="text-red-500">Context result was empty.</p>';
                }
            } catch (e) {
                console.error("Error processing Gemini API response:", e);
                dataContainer.innerHTML = '<p class="text-red-500">Error parsing global context.</p>';
            }
        };

        // Renders the bar chart for the 5-year trend
        window.renderGlobalTrendChart = (data) => {
            const chartEl = document.getElementById('global-trend-chart');
            chartEl.innerHTML = '';
            
            const legend = `
                <div class="flex justify-center space-x-6 text-sm font-medium mb-4">
                    <div class="flex items-center">
                        <span class="w-3 h-3 rounded-full bg-red-600 mr-2"></span> Forest Loss
                    </div>
                    <div class="flex items-center">
                        <span class="w-3 h-3 rounded-full bg-green-500 mr-2"></span> Forest Remaining/Gain
                    </div>
                </div>
            `;
            
            const chartContent = document.createElement('div');
            chartContent.className = 'bar-chart-container';

            data.forEach(item => {
                // Determine heights for visualization
                const lossHeight = item.loss_percent * 2.5; // Max 250px
                const remainingHeight = item.remaining_percent * 2.5;

                const wrapper = document.createElement('div');
                wrapper.className = 'bar-wrapper';
                
                // Loss Bar (Red)
                const lossBar = document.createElement('div');
                lossBar.className = 'bar-segment bar-loss';
                lossBar.style.height = `${lossHeight}px`; 
                
                const lossTooltip = document.createElement('div');
                lossTooltip.className = 'bar-tooltip';
                lossTooltip.textContent = `Loss: ${item.loss_value} (${item.loss_percent.toFixed(1)}%)`;
                lossBar.appendChild(lossTooltip);

                // Remaining/Gain Bar (Green)
                const remainingBar = document.createElement('div');
                remainingBar.className = 'bar-segment bar-remaining';
                remainingBar.style.height = `${remainingHeight}px`;

                const remainingTooltip = document.createElement('div');
                remainingTooltip.className = 'bar-tooltip';
                remainingTooltip.textContent = `Remaining: ${item.remaining_percent.toFixed(1)}%`;
                remainingBar.appendChild(remainingTooltip);

                // Year Label
                const yearLabel = document.createElement('div');
                yearLabel.className = 'bar-year-label';
                yearLabel.textContent = item.year;

                wrapper.appendChild(remainingBar); // Flipped order for stacked effect (green on bottom)
                wrapper.appendChild(lossBar); 
                wrapper.appendChild(yearLabel);

                chartContent.appendChild(wrapper);
            });
            
            chartEl.innerHTML = legend;
            chartEl.appendChild(chartContent);
        };


        // =========================================================================
        // DOM MANIPULATION & INTERACTION FUNCTIONS (Made global)
        // =========================================================================
        
        // --- Navigation ---
        window.showFeedView = () => {
            document.getElementById('feed-view').classList.remove('hidden');
            document.getElementById('dashboard-view').classList.add('hidden');
            document.getElementById('nav-feed').classList.add('active');
            document.getElementById('nav-dashboard').classList.remove('active');
        };
        
        window.showDashboardView = () => {
            if (window.firebase.totalReports === 0) {
                window.showMessage('Submit your first report to unlock the Global Dashboard!', 'info');
                return;
            }
            
            document.getElementById('feed-view').classList.add('hidden');
            document.getElementById('dashboard-view').classList.remove('hidden');
            document.getElementById('nav-feed').classList.remove('active');
            document.getElementById('nav-dashboard').classList.add('active');
            
            // Re-fetch global data when the dashboard is viewed
            window.fetchGlobalDeforestationTrend();
        };
        
        // --- Report Card Creation ---
        window.createReportCard = (id, data, isAdminView) => {
            const card = document.createElement('div');
            card.id = `report-${id}`;
            const statusClass = `report-status-${data.status}`;
            
            card.className = `bg-white p-6 rounded-xl border-l-4 card-shadow ${statusClass}`;
            
            // Use the full userId since it's the only identifier
            const userId = data.userId; 

            let adminControls = '';
            if (isAdminView) {
                adminControls = `
                    <div class="mt-4 pt-4 border-t border-gray-200">
                        <p class="font-semibold text-sm mb-2 text-green-900">Admin Actions:</p>
                        <div class="flex space-x-2 flex-wrap gap-2">
                            <button onclick="updateReportStatus('${id}', 'verified')" class="px-3 py-1 bg-green-500 text-white rounded-lg hover:bg-green-600 text-sm">Verify (Publish)</button>
                            <button onclick="updateReportStatus('${id}', 'rejected')" class="px-3 py-1 bg-red-500 text-white rounded-lg hover:bg-red-600 text-sm">Reject</button>
                            <button onclick="updateReportStatus('${id}', 'pending')" class="px-3 py-1 bg-yellow-500 text-white rounded-lg hover:bg-yellow-600 text-sm">Set Pending</button>
                        </div>
                    </div>
                `;
            }

            card.innerHTML = `
                <div class="flex items-start justify-between">
                    <div>
                        <span class="text-xs font-bold uppercase rounded-full px-2 py-1 ${statusClass} inline-block mb-2">${data.status}</span>
                        <h4 class="text-xl font-bold text-green-900">${data.title}</h4>
                        <p class="text-sm text-gray-500 mb-2">
                            Location: <span class="font-medium text-green-900">${data.location}</span>
                        </p>
                        <p class="text-xs text-gray-500 break-all">
                            Reported by <span class="font-medium text-green-900">${userId}</span>
                            on ${new Date(data.createdAt.seconds * 1000).toLocaleDateString()}
                        </p>
                    </div>
                </div>

                <p class="text-gray-700 my-4 whitespace-pre-wrap">${data.description}</p>
                
                ${data.imageUrl ? `<img src="${data.imageUrl}" onerror="this.onerror=null;this.src='https://placehold.co/400x200/f0fdf4/374151?text=Image+Not+Found';" class="w-full max-h-64 object-cover rounded-lg mb-4" alt="Evidence Photo">` : ''}

                ${adminControls}
            `;
            return card;
        }

        // --- Firestore Actions ---
        window.submitReport = async () => {
            const title = document.getElementById('report-title').value.trim();
            const location = document.getElementById('report-location').value.trim();
            const imageUrl = document.getElementById('report-image-url').value.trim();
            const description = document.getElementById('report-description').value.trim();
            
            // Check dbReady *before* allowing the function to proceed
            if (!window.firebase.dbReady) {
                 return window.showMessage('Authentication is still loading. Please wait until the submit button is active.', 'error');
            }
            if (!title || !description || !location) return window.showMessage('Title, Location, and Description are required!', 'error');

            const isFirstReport = window.firebase.totalReports === 0;

            try {
                const reportsRef = getReportsCollectionRef();
                // Check if reportsRef is null (due to DB not ready - though button is disabled, this is a final guard)
                if (!reportsRef) return window.showMessage('Database connection is not fully ready. Please try again in a moment.', 'error');

                await addDoc(reportsRef, {
                    title,
                    description,
                    location,
                    imageUrl: imageUrl || '',
                    userId: window.firebase.userId,
                    status: 'pending', 
                    createdAt: Timestamp.now(),
                });
                
                // Clear fields on success
                document.getElementById('report-title').value = '';
                document.getElementById('report-location').value = '';
                document.getElementById('report-image-url').value = '';
                document.getElementById('report-description').value = '';
                window.showMessage('Report submitted successfully! It is now awaiting admin review.', 'success');

                // If this was the first report, show the dashboard tab
                if (isFirstReport) {
                    window.showMessage('First report submitted! Global Dashboard is now unlocked.', 'info');
                    // Add a slight delay before switching views
                    setTimeout(() => {
                        window.showDashboardView();
                    }, 500);
                }

            } catch (e) {
                console.error("Error adding report: ", e);
                window.showMessage('Failed to submit report. Check console for details.', 'error');
            }
        };

        window.updateReportStatus = async (reportId, newStatus) => {
            if (!window.firebase.dbReady) return window.showMessage('Authentication not ready.', 'error');
            if (window.firebase.userId !== window.firebase.adminId) return window.showMessage('Access Denied: Only the designated admin can change report status.', 'error');

            const reportDocRef = doc(getReportsCollectionRef(), reportId);
            
            try {
                await updateDoc(reportDocRef, {
                    status: newStatus
                });
                window.showMessage(`Report ${reportId.substring(0, 4)}... status updated to: ${newStatus.toUpperCase()}`, 'info');

            } catch (e) {
                console.error("Error updating report status: ", e);
                window.showMessage('Failed to update report status. Check console.', 'error');
            }
        };

        
        // --- Utilities ---
        window.showMessage = (message, type = 'info') => {
            const container = document.getElementById('message-box-container');
            const colorMap = {
                success: 'bg-green-100 border-green-400 text-green-700',
                error: 'bg-red-100 border-red-400 text-red-700',
                info: 'bg-blue-100 border-blue-400 text-blue-700'
            };

            const box = document.createElement('div');
            box.className = `p-4 border-l-4 rounded-lg shadow-lg mb-4 ${colorMap[type]} transition-all duration-300 ease-in-out transform translate-x-0`;
            box.innerHTML = `<p class="font-bold">${type.charAt(0).toUpperCase() + type.slice(1)}:</p><p class="text-sm">${message}</p>`;

            container.prepend(box);

            setTimeout(() => {
                box.classList.add('opacity-0');
                setTimeout(() => box.remove(), 500);
            }, 5000);
        };
        
    </script>
</body>
</html>
