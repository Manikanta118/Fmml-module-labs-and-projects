## 1. High-Level Application Architecture

```mermaid
flowchart TB
    subgraph Entry["⚡ Entry Point"]
        main["main.jsx"]
        chartReg["Chart.js Registration\n(CategoryScale, LinearScale,\nPointElement, LineElement,\nBarElement, ArcElement,\nTitle, Tooltip, Legend, Filler)"]
    end

    subgraph AppShell["🖥️ App Shell (App.jsx)"]
        router["BrowserRouter"]
        layout["Flex Layout\n(h-screen)"]
        sidebar["Sidebar"]
        mainArea["Main Content Area"]
        navbar["Navbar"]
        routes["Routes"]
    end

    subgraph Pages["📄 Pages"]
        dashboard["Dashboard\n(/dashboard)"]
        phishing["PhishingDetection\n(/phishing)"]
        ctmonitor["CTMonitor\n(/ct-monitor)"]
        alerts["AlertsPanel\n(/alerts)"]
    end

    subgraph SharedComponents["🧩 Shared Components"]
        card["Card"]
        btn["Button"]
        tbl["Table"]
        lineChart["LineChart"]
        doughnutChart["DoughnutChart"]
    end

    main --> chartReg --> router
    router --> layout
    layout --> sidebar
    layout --> mainArea
    mainArea --> navbar
    mainArea --> routes
    routes --> dashboard
    routes --> phishing
    routes --> ctmonitor
    routes --> alerts

    dashboard --> card & lineChart & tbl
    phishing --> btn
    ctmonitor --> btn & doughnutChart
    alerts --> tbl & btn

    style Entry fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style AppShell fill:#1e293b,stroke:#6366f1,color:#e2e8f0
    style Pages fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style SharedComponents fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
```

---

## 2. Routing & Navigation Flow

```mermaid
flowchart LR
    userVisits["User visits URL"] --> browserRouter["BrowserRouter"]

    browserRouter --> routeMatch{"Route Matching"}

    routeMatch -->|"/"| redirect["Navigate → /dashboard"]
    routeMatch -->|"/dashboard"| dashPage["Dashboard Page"]
    routeMatch -->|"/phishing"| phishPage["PhishingDetection Page"]
    routeMatch -->|"/ct-monitor"| ctPage["CTMonitor Page"]
    routeMatch -->|"/alerts"| alertsPage["AlertsPanel Page"]

    redirect --> dashPage

    subgraph SidebarNav["Sidebar Navigation (NavLink)"]
        nav1["📊 Dashboard\n→ /dashboard"]
        nav2["🔍 Phishing Detection\n→ /phishing"]
        nav3["📜 CT Monitor\n→ /ct-monitor"]
        nav4["🔔 Alerts Panel\n→ /alerts"]
    end

    nav1 -->|click| dashPage
    nav2 -->|click| phishPage
    nav3 -->|click| ctPage
    nav4 -->|click| alertsPage

    style routeMatch fill:#3b82f6,stroke:#60a5fa,color:#fff
    style redirect fill:#f59e0b,stroke:#fbbf24,color:#000
    style SidebarNav fill:#0f172a,stroke:#6366f1,color:#e2e8f0
```

---

## 3. Component Hierarchy Tree

```mermaid
flowchart TD
    root["ReactDOM.createRoot"]
    root --> strictMode["React.StrictMode"]
    strictMode --> app["App"]

    app --> router["BrowserRouter"]
    router --> flexContainer["div.flex.h-screen"]

    flexContainer --> sidebar["Sidebar"]
    flexContainer --> contentCol["div.flex-col"]

    sidebar --> logo["Shield Icon + 'CertEagle'"]
    sidebar --> navLinks["NavLink × 4\n(Dashboard, Phishing,\nCT Monitor, Alerts)"]
    sidebar --> userProfile["User Profile Card\n(Admin User / Security Analyst)"]

    contentCol --> navbar["Navbar"]
    contentCol --> mainContent["main (Routes)"]

    navbar --> searchBar["Search Input\n'Search endpoints, logs, or IPs...'"]
    navbar --> bellBtn["Bell Button\n(with ping animation)"]
    navbar --> settingsBtn["Settings Button"]

    mainContent --> dashRoute["/ → Dashboard"]
    mainContent --> phishRoute["/phishing → PhishingDetection"]
    mainContent --> ctRoute["/ct-monitor → CTMonitor"]
    mainContent --> alertRoute["/alerts → AlertsPanel"]

    dashRoute --> cards["Card × 4"]
    dashRoute --> lineChartComp["LineChart"]
    dashRoute --> threatMap["Threat Map Panel"]
    dashRoute --> eventsTable["Table (Recent Events)"]

    phishRoute --> urlInput["URL Input Form"]
    phishRoute --> scanBtn["Button (Analyze URL)"]
    phishRoute --> resultPanel["Result Panel\n(Score + Heuristics)"]

    ctRoute --> streamPanel["Live Stream Panel"]
    ctRoute --> doughnutComp["DoughnutChart"]
    ctRoute --> keywordPanel["Keyword Alerts Panel"]

    alertRoute --> statCards["Stat Cards × 3"]
    alertRoute --> tabBar["Tab Filter Bar"]
    alertRoute --> alertTable["Table (Alert List)"]

    style root fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style app fill:#1e293b,stroke:#6366f1,color:#e2e8f0
    style sidebar fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style navbar fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
```

---

## 4. Dashboard Page — Data Flow

```mermaid
flowchart TD
    subgraph StatsFlow["📊 Stats Cards"]
        statsArray["stats array\n(4 objects)"]
        statsMap["stats.map()"]
        cardRender["Card Component\n• title, value\n• icon (Lucide)\n• trend direction\n• gradient color"]
    end

    subgraph ChartFlow["📈 Line Chart"]
        trafficData["trafficData object"]
        labels["labels:\n00:00 → 24:00\n(4h intervals)"]
        ds1["Dataset 1: Threat Intensity\nred line, filled area"]
        ds2["Dataset 2: Certificates Logged\nblue line, no fill"]
        lineChartRender["LineChart Component\n(react-chartjs-2)"]
    end

    subgraph ThreatMap["🌍 Threat Map"]
        countryData["Country data array\n(Russia 45%, China 32%,\nBrazil 15%, USA 8%)"]
        progressBars["Progress bar\nper country"]
        geoBtn["'View Geographic Map'\nbutton"]
    end

    subgraph EventsFlow["📋 Recent Events Table"]
        alertsData["recentAlertsData\n(5 alert objects)"]
        tableRender["Table Component\n• Time\n• Domain / Indicator\n• Event Type\n• Severity\n• Actions"]
    end

    statsArray --> statsMap --> cardRender
    trafficData --> labels & ds1 & ds2
    labels & ds1 & ds2 --> lineChartRender
    countryData --> progressBars --> geoBtn
    alertsData --> tableRender

    style StatsFlow fill:#0f172a,stroke:#ef4444,color:#e2e8f0
    style ChartFlow fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style ThreatMap fill:#0f172a,stroke:#f59e0b,color:#e2e8f0
    style EventsFlow fill:#0f172a,stroke:#10b981,color:#e2e8f0
```

---

## 5. CT Monitor — Live Stream Flow

```mermaid
flowchart TD
    mount["Component Mounts"] --> checkStream{"isStreaming?"}

    checkStream -->|Yes| startInterval["setInterval\n(every 1500ms)"]
    checkStream -->|No| idle["Idle — no logs generated"]

    startInterval --> generateLog["generateLog()"]

    generateLog --> pickDomain["Random subdomain\n(auth., www., api., mail.,\napp., dev., test., secure.)"]
    generateLog --> pickBase["Random base domain\n(example.com, google.com,\nmicrosoft.com, internal.network,\nsuspicious-login.net)"]
    generateLog --> pickIssuer["Random issuer\n(Let's Encrypt, DigiCert,\nSectigo, Cloudflare, ZeroSSL)"]

    pickDomain & pickBase --> fullDomain["Combined domain string"]
    fullDomain --> riskCheck{"Contains 'suspicious'\nor 'auth.internal'?"}

    riskCheck -->|Yes| highRisk["risk: 'High'\ntype: 'Pre-cert (Suspicious)'"]
    riskCheck -->|No| lowRisk["risk: 'Low'\ntype: 'Pre-cert'"]

    highRisk & lowRisk --> newLog["New log object\n{id, domain, issuer,\ntimestamp, type, risk}"]

    newLog --> prependLogs["Prepend to logs state\n(newest first)"]
    prependLogs --> capLogs["Slice to 50 max entries"]
    capLogs --> renderStream["Render in Live Stream panel"]

    renderStream --> logEntry{"risk === 'High'?"}
    logEntry -->|Yes| dangerStyle["Red highlight\nbg-danger/10\nborder-danger/30"]
    logEntry -->|No| normalStyle["Dark style\nbg-dark-800/80\nborder-dark-700"]

    subgraph Controls["🎛️ User Controls"]
        toggleBtn["Play/Stop Button\n→ toggles isStreaming"]
        exportBtn["Export CSV Button"]
        filterInput["Filter domains input"]
    end

    toggleBtn --> checkStream

    subgraph SidePanel["📊 Side Panel"]
        doughnut["DoughnutChart\nIssuers Distribution\n(Let's Encrypt 65%,\nCloudflare 15%,\nDigiCert 10%,\nSectigo 5%, Other 5%)"]
        keywords["Keyword Alerts\n(paypal, apple, login,\nverify, update, secure, wallet)"]
        regex["Regex Pattern\n/(.*)(login|verify|auth)(.*)\\.(com|net|org)$/i"]
    end

    style Controls fill:#1e293b,stroke:#6366f1,color:#e2e8f0
    style SidePanel fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
    style riskCheck fill:#ef4444,stroke:#f87171,color:#fff
```

---

## 6. Phishing Detection — Scan Flow

```mermaid
flowchart TD
    start["User lands on\nPhishing URL Analyzer"] --> inputUrl["Enter URL in input field\n(type='url', required)"]

    inputUrl --> submitForm["Submit form\n(handleScan)"]

    submitForm --> validateUrl{"URL empty?"}
    validateUrl -->|Yes| doNothing["Return — no action"]
    validateUrl -->|No| startScan["setIsScanning(true)\nsetResult(null)"]

    startScan --> showSpinner["Show Loader2\nspinner animation"]

    showSpinner --> mockDelay["setTimeout\n(2000ms delay)"]

    mockDelay --> checkUrl{"URL contains\n'paypal', 'login',\n'update', or '-'?"}

    checkUrl -->|Yes| phishResult["isPhishing: true\nscore: 92"]
    checkUrl -->|No| safeResult["isPhishing: false\nscore: 12"]

    phishResult --> buildChecks1["Checks:\n❌ Domain Age: 3 days\n✅ SSL: Valid Let's Encrypt\n❌ Redirects: Obfuscated\n✅ Homoglyphs: None"]

    safeResult --> buildChecks2["Checks:\n✅ Domain Age: 5 years\n✅ SSL: Valid Let's Encrypt\n✅ Redirects: Clean direct\n✅ Homoglyphs: None"]

    buildChecks1 & buildChecks2 --> setResult["setResult(resultObj)\nsetIsScanning(false)"]

    setResult --> renderResult{"isPhishing?"}

    renderResult -->|Yes| dangerUI["🛡️ ShieldX icon (red)\nPing animation\n'Phishing Detected'\nThreat Score: 92/100\nred progress bar"]
    renderResult -->|No| safeUI["🛡️ ShieldCheck icon (green)\n'URL is Safe'\nThreat Score: 12/100\ngreen progress bar"]

    dangerUI --> heuristicsPanel["Heuristics Analysis\n(4 check rows with\nPass/Fail badges)"]
    safeUI --> heuristicsPanel

    heuristicsPanel --> actionBtns{"isPhishing?"}
    actionBtns -->|Yes| bothBtns["'Download Report' +\n'Block Domain' buttons"]
    actionBtns -->|No| reportBtn["'Download Report'\nbutton only"]

    style checkUrl fill:#f59e0b,stroke:#fbbf24,color:#000
    style renderResult fill:#6366f1,stroke:#818cf8,color:#fff
    style dangerUI fill:#7f1d1d,stroke:#ef4444,color:#fca5a5
    style safeUI fill:#064e3b,stroke:#10b981,color:#6ee7b7
```

---

## 7. Alerts Panel — Triage Flow

```mermaid
flowchart TD
    load["AlertsPanel Loads"] --> initState["useState: activeTab = 'all'"]

    initState --> renderStats["Render 3 Summary Cards"]

    renderStats --> card1["🔴 Critical Unresolved: 12\n(AlertCircle icon)"]
    renderStats --> card2["🟡 Investigating: 8\n(Shield icon)"]
    renderStats --> card3["🟢 Resolved 24h: 45\n(CheckCircle icon)"]

    initState --> renderTabs["Render Tab Filter Bar"]
    renderTabs --> tab1["All Alerts"]
    renderTabs --> tab2["High Severity"]
    renderTabs --> tab3["New Only"]

    tab1 -->|click| setAll["setActiveTab('all')"]
    tab2 -->|click| setHigh["setActiveTab('high')"]
    tab3 -->|click| setNew["setActiveTab('new')"]

    setAll & setHigh & setNew --> filterLogic{"Filter alertsData"}

    filterLogic -->|"tab === 'all'"| showAll["Show all 7 alerts"]
    filterLogic -->|"tab === 'high'"| filterHigh["Filter: severity === 'High'\n(4 alerts)"]
    filterLogic -->|"tab === 'new'"| filterNew["Filter: status === 'New'\n(3 alerts)"]

    showAll & filterHigh & filterNew --> mapData["Map alert objects to\nJSX-enriched rows"]

    mapData --> statusBadge["Status badge with color:\n• New → blue\n• Investigating → amber\n• Blocked → red\n• Closed → gray"]

    statusBadge --> renderTable["Table Component\nHeaders: ID, Domain, Source,\nDetection Type, Severity,\nStatus, Time, Actions"]

    subgraph AlertSources["📡 Alert Sources"]
        src1["CT Log Monitor"]
        src2["Phishing Engine"]
        src3["USER Report"]
        src4["Internal Scan"]
        src5["Threat Intel"]
    end

    subgraph ActionBtns["🎛️ Page Actions"]
        filtersBtn["Filters button\n(SlidersHorizontal)"]
        clearBtn["Clear Resolved button\n(Trash2, danger variant)"]
    end

    style filterLogic fill:#3b82f6,stroke:#60a5fa,color:#fff
    style AlertSources fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style ActionBtns fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
```

---

## 8. State Management Flow

```mermaid
flowchart LR
    subgraph CTMonitorState["CTMonitor State"]
        logsState["logs\nuseState([])\n—array of log objects—"]
        streamingState["isStreaming\nuseState(true)"]
    end

    subgraph PhishingState["PhishingDetection State"]
        urlState["url\nuseState('')"]
        scanState["isScanning\nuseState(false)"]
        resultState["result\nuseState(null)"]
    end

    subgraph AlertsState["AlertsPanel State"]
        tabState["activeTab\nuseState('all')"]
    end

    streamingState -->|controls| useEffectHook["useEffect\n(interval logic)"]
    useEffectHook -->|updates| logsState
    logsState -->|renders| streamUI["Live Stream UI"]

    urlState -->|input to| handleScan["handleScan()"]
    handleScan -->|toggles| scanState
    handleScan -->|sets after 2s| resultState
    resultState -->|renders| resultUI["Result Panel UI"]

    tabState -->|filters| filteredData["filteredData\n(derived, not state)"]
    filteredData -->|renders| tableUI["Alerts Table UI"]

    style CTMonitorState fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style PhishingState fill:#0f172a,stroke:#ef4444,color:#e2e8f0
    style AlertsState fill:#0f172a,stroke:#10b981,color:#e2e8f0
```

---

## 9. User Interaction Flow — End-to-End Journey

```mermaid
flowchart TD
    userOpens["🧑‍💻 User opens CertEagle"] --> autoRedirect["Auto-redirect\n/ → /dashboard"]

    autoRedirect --> viewDashboard["📊 Dashboard\n• View 4 stat cards\n• View threat vs cert chart\n• View threat map by country\n• View recent high-priority events"]

    viewDashboard --> choosePath{"User clicks\nSidebar link"}

    choosePath -->|"Phishing Detection"| phishFlow["🔍 Phishing Detection"]
    choosePath -->|"CT Monitor"| ctFlow["📜 CT Monitor"]
    choosePath -->|"Alerts Panel"| alertFlow["🔔 Alerts Panel"]

    phishFlow --> enterUrl["Enter suspicious URL"]
    enterUrl --> clickAnalyze["Click 'Analyze URL'"]
    clickAnalyze --> waitScan["Wait 2s scanning animation"]
    waitScan --> viewResult["View result:\n• Safe or Phishing verdict\n• Threat score bar\n• 4 heuristic checks"]
    viewResult --> phishActions{"Phishing detected?"}
    phishActions -->|Yes| blockOrReport["Block Domain /\nDownload Report"]
    phishActions -->|No| downloadReport["Download Report"]

    ctFlow --> viewStream["Watch live certificate stream\n(auto-updating every 1.5s)"]
    viewStream --> ctActions{"User action"}
    ctActions -->|"Stop/Play"| toggleStream["Toggle streaming\non/off"]
    ctActions -->|"Export"| exportCsv["Export CSV"]
    ctActions -->|"Filter"| filterDomains["Filter by domain name"]
    ctActions -->|"Add keyword"| addKeyword["Add keyword alert\n(paypal, login, etc.)"]

    alertFlow --> viewAlerts["View 3 summary cards\n(Critical, Investigating, Resolved)"]
    viewAlerts --> switchTab{"Switch tab"}
    switchTab -->|"All Alerts"| showAll["Show all alerts"]
    switchTab -->|"High Severity"| showHigh["Show high-severity only"]
    switchTab -->|"New Only"| showNew["Show new alerts only"]
    showAll & showHigh & showNew --> triageAlert["Review alert details\nin table"]

    style choosePath fill:#6366f1,stroke:#818cf8,color:#fff
    style phishActions fill:#ef4444,stroke:#f87171,color:#fff
    style ctActions fill:#3b82f6,stroke:#60a5fa,color:#fff
    style switchTab fill:#10b981,stroke:#34d399,color:#fff
```

---

## 10. Data Rendering Pipeline

```mermaid
flowchart LR
    subgraph DataSources["📦 Data Sources"]
        mockStats["Mock Stats\n(hardcoded arrays)"]
        mockAlerts["Mock Alerts\n(hardcoded objects)"]
        generatedLogs["Generated Logs\n(setInterval + Math.random)"]
        scanResult["Scan Result\n(setTimeout mock)"]
    end

    subgraph Processing["⚙️ Processing"]
        mapTransform[".map() transforms"]
        filterTransform[".filter() by tab"]
        sliceTransform[".slice(0, 50) cap"]
        riskClassify["Risk classification\n(keyword matching)"]
    end

    subgraph UIComponents["🎨 UI Components"]
        cardUI["Card\n(icon + value + trend)"]
        tableUI["Table\n(headers + row iteration)"]
        lineChartUI["LineChart\n(Chart.js Line)"]
        doughnutUI["DoughnutChart\n(Chart.js Doughnut)"]
        customPanels["Custom Panels\n(Threat Map, Results,\nKeywords, Stream)"]
    end

    subgraph Styling["🎭 Styling Layer"]
        tailwind["TailwindCSS classes"]
        glassmorphism["glass-panel\n(custom class)"]
        animations["Animations\n(animate-ping,\nanimate-pulse,\nslide-in)"]
        conditionalStyle["Conditional styles\n(severity colors,\nrisk highlighting)"]
    end

    mockStats --> mapTransform --> cardUI
    mockAlerts --> filterTransform --> mapTransform
    mapTransform --> tableUI
    generatedLogs --> riskClassify --> sliceTransform --> customPanels
    scanResult --> customPanels

    cardUI & tableUI & lineChartUI & doughnutUI & customPanels --> tailwind & glassmorphism & animations & conditionalStyle

    style DataSources fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style Processing fill:#0f172a,stroke:#f59e0b,color:#e2e8f0
    style UIComponents fill:#0f172a,stroke:#10b981,color:#e2e8f0
    style Styling fill:#0f172a,stroke:#a855f7,color:#e2e8f0
```

---

## 11. Table Component — Conditional Cell Rendering

```mermaid
flowchart TD
    tableMount["Table receives\n{headers, data}"] --> renderHeaders["Render thead\nheaders.map() → th cells"]

    renderHeaders --> renderRows["Render tbody\ndata.map() → tr rows"]

    renderRows --> iterateCells["Object.entries(row)\n.map() → td cells"]

    iterateCells --> cellCheck{"What is the\ncell key?"}

    cellCheck -->|"key === 'severity'"| severityBranch["Severity Cell"]
    cellCheck -->|"key === 'actions'"| actionsBranch["Actions Cell"]
    cellCheck -->|"key === 'url' or 'domain'"| domainBranch["Domain Cell"]
    cellCheck -->|"any other key"| defaultBranch["Default Cell\n(render value as-is)"]

    severityBranch --> sevIcon{"value.toLowerCase()"}
    sevIcon -->|"'high'"| highIcon["🔺 AlertTriangle\n(text-danger)"]
    sevIcon -->|"'medium'"| medIcon["ℹ️ Info\n(text-warning)"]
    sevIcon -->|"'low'"| lowIcon["✅ CheckCircle2\n(text-accent)"]

    highIcon & medIcon & lowIcon --> sevLabel["Colored severity label\n• High → text-danger\n• Medium → text-warning\n• Low → text-accent"]

    actionsBranch --> extLink["ExternalLink icon button\nopacity-0 → opacity-100\non group-hover"]

    domainBranch --> monoText["font-mono text-xs\ntext-slate-200"]

    subgraph RowStyling["Row Hover Effect"]
        rowDefault["border-b border-dark-700/50"]
        rowHover["hover:bg-dark-800/30\ntransition-colors"]
        groupClass["class='group'\n(enables child hover reveal)"]
    end

    style cellCheck fill:#6366f1,stroke:#818cf8,color:#fff
    style severityBranch fill:#1e293b,stroke:#ef4444,color:#e2e8f0
    style actionsBranch fill:#1e293b,stroke:#3b82f6,color:#e2e8f0
    style domainBranch fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style RowStyling fill:#0f172a,stroke:#f59e0b,color:#e2e8f0
```

---

## 12. Chart.js Configuration Pipeline

```mermaid
flowchart TD
    subgraph Registration["⚡ Global Registration (main.jsx)"]
        chartJS["Chart.js Core"]
        scales["Scales:\nCategoryScale\nLinearScale"]
        elements["Elements:\nPointElement\nLineElement\nBarElement\nArcElement"]
        plugins["Plugins:\nTitle\nTooltip\nLegend\nFiller"]
        register["ChartJS.register(...)"]
    end

    chartJS --> register
    scales --> register
    elements --> register
    plugins --> register

    register --> lineChartComp["LineChart Component"]
    register --> doughnutComp["DoughnutChart Component"]

    subgraph LineConfig["📈 LineChart Options"]
        lineResponsive["responsive: true\nmaintainAspectRatio: false"]
        lineLegend["Legend:\nposition: top\ncolor: #94a3b8\nusePointStyle: true"]
        lineTooltip["Tooltip:\nbg: #1e293b\nborder: #334155\npadding: 12"]
        lineYAxis["Y Axis:\ngrid: dashed #334155\nbeginAtZero: true"]
        lineXAxis["X Axis:\ngrid: hidden"]
        lineInteraction["Interaction:\nmode: index\nintersect: false"]
        lineElements["Elements:\ntension: 0.4 (smooth curves)\npoint radius: 0\nhover radius: 6"]
    end

    lineChartComp --> lineResponsive & lineLegend & lineTooltip & lineYAxis & lineXAxis & lineInteraction & lineElements

    subgraph DoughnutConfig["🍩 DoughnutChart Options"]
        doughResponsive["responsive: true\nmaintainAspectRatio: false"]
        doughCutout["cutout: 75%\n(thin ring)"]
        doughLegend["Legend:\nposition: bottom\npadding: 20\npointStyle: circle"]
        doughTooltip["Tooltip:\nbg: #1e293b\nborder: #334155"]
        doughBorder["borderWidth: 0\n(seamless segments)"]
        centerOverlay["Center overlay:\nTotal count (bold)\n'Total Logs' label"]
    end

    doughnutComp --> doughResponsive & doughCutout & doughLegend & doughTooltip & doughBorder & centerOverlay

    subgraph DataFlow["📊 Data Inputs"]
        dashData["Dashboard provides:\n• labels (time intervals)\n• Dataset 1: Threat (red, filled)\n• Dataset 2: Certs (blue, line)"]
        ctData["CTMonitor provides:\n• labels (issuer names)\n• Single dataset (5 colors)\n• borderWidth: 0"]
    end

    dashData --> lineChartComp
    ctData --> doughnutComp

    style Registration fill:#0f172a,stroke:#f59e0b,color:#e2e8f0
    style LineConfig fill:#1e293b,stroke:#3b82f6,color:#e2e8f0
    style DoughnutConfig fill:#1e293b,stroke:#a855f7,color:#e2e8f0
    style DataFlow fill:#0f172a,stroke:#10b981,color:#e2e8f0
```

---

## 13. Threat Classification & Severity System

```mermaid
flowchart TD
    subgraph InputSources["🌐 Threat Input Sources"]
        ctLogs["CT Log Stream\n(CTMonitor page)"]
        urlScan["URL Scan\n(PhishingDetection page)"]
        alertFeed["Alert Feed\n(AlertsPanel page)"]
        dashEvents["Dashboard Events\n(Dashboard page)"]
    end

    subgraph CTClassification["CT Log Classification"]
        ctDomain["Domain string assembled"]
        ctKeywordCheck{"Contains 'suspicious'\nor 'auth.internal'?"}
        ctHigh["risk: High\ntype: Pre-cert Suspicious"]
        ctLow["risk: Low\ntype: Pre-cert"]
        ctDomain --> ctKeywordCheck
        ctKeywordCheck -->|Yes| ctHigh
        ctKeywordCheck -->|No| ctLow
    end

    subgraph PhishClassification["Phishing URL Classification"]
        urlInput["URL string"]
        urlKeywordCheck{"Contains 'paypal',\n'login', 'update',\nor '-'?"}
        urlPhish["isPhishing: true\nscore: 92/100"]
        urlSafe["isPhishing: false\nscore: 12/100"]
        urlInput --> urlKeywordCheck
        urlKeywordCheck -->|Yes| urlPhish
        urlKeywordCheck -->|No| urlSafe

        urlPhish --> checks1["❌ Domain Age: 3 days\n❌ Redirects: Obfuscated"]
        urlSafe --> checks2["✅ Domain Age: 5 years\n✅ Redirects: Clean"]
    end

    subgraph AlertClassification["Alert Severity & Status"]
        alertObj["Alert object"]
        sevLevel{"severity"}
        sevHigh["🔴 High\ntext-danger\nAlertTriangle icon"]
        sevMed["🟡 Medium\ntext-warning\nInfo icon"]
        sevLow["🟢 Low\ntext-accent\nCheckCircle2 icon"]

        alertObj --> sevLevel
        sevLevel -->|High| sevHigh
        sevLevel -->|Medium| sevMed
        sevLevel -->|Low| sevLow

        statusLevel{"status"}
        stNew["🔵 New\nbg-blue-500/10"]
        stInvest["🟡 Investigating\nbg-warning/10"]
        stBlocked["🔴 Blocked\nbg-danger/10"]
        stClosed["⚫ Closed\nbg-dark-700"]

        alertObj --> statusLevel
        statusLevel -->|New| stNew
        statusLevel -->|Investigating| stInvest
        statusLevel -->|Blocked| stBlocked
        statusLevel -->|Closed| stClosed
    end

    subgraph DetectionTypes["🔎 Detection Type Taxonomy"]
        dt1["High Risk Keyword"]
        dt2["Heuristics Match"]
        dt3["Confirmed Phishing"]
        dt4["Suspicious TLD"]
        dt5["Malware Distribution"]
        dt6["Policy Violation"]
        dt7["Known Malicious IP"]
    end

    ctLogs --> CTClassification
    urlScan --> PhishClassification
    alertFeed --> AlertClassification
    dashEvents --> AlertClassification

    style InputSources fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style CTClassification fill:#1e293b,stroke:#ef4444,color:#e2e8f0
    style PhishClassification fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
    style AlertClassification fill:#1e293b,stroke:#6366f1,color:#e2e8f0
    style DetectionTypes fill:#0f172a,stroke:#10b981,color:#e2e8f0
```

---

## 14. Sidebar Navigation — Active State & UI Feedback

```mermaid
flowchart TD
    sidebarRender["Sidebar renders\n4 NavLink items"] --> navLinkMap["links.map() iteration"]

    navLinkMap --> link1["LayoutDashboard\n/dashboard"]
    navLinkMap --> link2["Search\n/phishing"]
    navLinkMap --> link3["FileText\n/ct-monitor"]
    navLinkMap --> link4["Bell\n/alerts"]

    link1 & link2 & link3 & link4 --> navLinkComp["NavLink Component\n(react-router-dom)"]

    navLinkComp --> isActiveCheck{"isActive?\n(current URL matches\nlink path)"}

    isActiveCheck -->|Yes| activeStyles["Active Styles Applied"]
    isActiveCheck -->|No| inactiveStyles["Inactive Styles Applied"]

    activeStyles --> activeBg["bg-primary/10"]
    activeStyles --> activeText["text-primary"]
    activeStyles --> activeBar["Left accent bar\nw-1 h-8 bg-primary\nrounded-r-full\n(positioned -ml-4)"]
    activeStyles --> activeIcon["Icon: text-primary"]

    inactiveStyles --> inactiveBg["transparent bg"]
    inactiveStyles --> inactiveText["text-slate-400"]
    inactiveStyles --> inactiveIcon["Icon: text-slate-500"]

    inactiveStyles --> hoverEffects["Hover Effects:\nhover:bg-dark-700\nhover:text-slate-200\nicon: group-hover:text-slate-300"]

    subgraph SidebarLayout["📐 Sidebar Layout"]
        header["Header (h-16)\nShield icon + 'CertEagle'\nborder-b border-dark-700"]
        navSection["Navigation Section\nflex-1 overflow-y-auto\npy-6 px-4"]
        navLabel["'NAVIGATION' label\ntext-xs uppercase\ntext-slate-500"]
        footer["Footer\nborder-t border-dark-700"]
        userCard["User Profile Card\nglass-panel rounded-xl"]
        avatar["Avatar: gradient ring\nfrom-primary to-accent\ninitials 'AD'"]
        userInfo["'Admin User'\n'Security Analyst'"]
    end

    header --> navSection --> navLabel
    navSection --> navLinkMap
    footer --> userCard --> avatar & userInfo

    style isActiveCheck fill:#3b82f6,stroke:#60a5fa,color:#fff
    style activeStyles fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style inactiveStyles fill:#1e293b,stroke:#64748b,color:#e2e8f0
    style SidebarLayout fill:#0f172a,stroke:#6366f1,color:#e2e8f0
```

---

## 15. Application Lifecycle & Effect Cleanup

```mermaid
flowchart TD
    subgraph AppBoot["🚀 Application Boot"]
        htmlLoad["index.html loads\n(div#root)"]
        mainJsx["main.jsx executes"]
        registerCharts["ChartJS.register()\n10 plugins/scales"]
        createRoot["ReactDOM.createRoot()"]
        renderApp["Render App in\nReact.StrictMode"]
    end

    htmlLoad --> mainJsx --> registerCharts --> createRoot --> renderApp

    renderApp --> routerInit["BrowserRouter initializes\nURL listener attached"]

    routerInit --> initialRoute{"Current URL?"}
    initialRoute -->|"/"| redirectEffect["Navigate component\nreplace: true\n→ /dashboard"]
    initialRoute -->|"/dashboard"| mountDash["Mount Dashboard"]
    initialRoute -->|"/phishing"| mountPhish["Mount PhishingDetection"]
    initialRoute -->|"/ct-monitor"| mountCT["Mount CTMonitor"]
    initialRoute -->|"/alerts"| mountAlerts["Mount AlertsPanel"]

    redirectEffect --> mountDash

    subgraph CTLifecycle["🔄 CTMonitor Lifecycle"]
        ctMount["Component mounts\nlogs=[], isStreaming=true"]
        ctEffect["useEffect runs\n(dependency: isStreaming)"]
        ctCheck{"isStreaming\n=== true?"}
        ctStart["setInterval created\n(1500ms cycle)"]
        ctGenerate["generateLog() called\neach interval tick"]
        ctUpdate["setLogs(prev =>\n[newLog, ...prev].slice(0,50))"]
        ctCleanup["Cleanup function:\nclearInterval()"]
        ctToggle["User clicks\nStop/Start button"]
        ctRerun["Effect re-runs\nwith new isStreaming value"]
    end

    mountCT --> ctMount --> ctEffect --> ctCheck
    ctCheck -->|Yes| ctStart --> ctGenerate --> ctUpdate
    ctUpdate -->|"loop"| ctGenerate
    ctCheck -->|No| ctCleanup
    ctToggle --> ctCleanup --> ctRerun --> ctCheck

    subgraph PhishLifecycle["🔍 PhishingDetection Lifecycle"]
        phishMount["Component mounts\nurl='', isScanning=false,\nresult=null"]
        phishInput["User types URL\n→ setUrl()"]
        phishSubmit["Form submit\n→ handleScan()"]
        phishTimer["setTimeout(2000)\ncreated"]
        phishResult["Callback fires:\nsetIsScanning(false)\nsetResult(resultObj)"]
        phishUnmount["Component unmounts\n⚠️ No timer cleanup\n(potential memory leak)"]
    end

    mountPhish --> phishMount --> phishInput --> phishSubmit --> phishTimer --> phishResult
    phishSubmit -.->|"unmount during scan"| phishUnmount

    subgraph AlertsLifecycle["🔔 AlertsPanel Lifecycle"]
        alertMount["Component mounts\nactiveTab='all'"]
        alertRender["Render with\nfilteredData derived\nfrom alertsData"]
        alertTab["User clicks tab\n→ setActiveTab()"]
        alertRerender["Component re-renders\nfilteredData recomputed"]
    end

    mountAlerts --> alertMount --> alertRender
    alertTab --> alertRerender --> alertRender

    subgraph Unmount["🧹 Unmount Behavior"]
        navAway["User navigates\nto different page"]
        oldUnmount["Old page unmounts\n• CT: clearInterval\n• Phishing: timer orphaned\n• Alerts: no cleanup needed"]
        newMount["New page mounts\nfresh state initialized"]
    end

    navAway --> oldUnmount --> newMount

    style AppBoot fill:#0f172a,stroke:#3b82f6,color:#e2e8f0
    style CTLifecycle fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style PhishLifecycle fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
    style AlertsLifecycle fill:#1e293b,stroke:#6366f1,color:#e2e8f0
    style Unmount fill:#1e293b,stroke:#ef4444,color:#e2e8f0
    style phishUnmount fill:#7f1d1d,stroke:#ef4444,color:#fca5a5
```
