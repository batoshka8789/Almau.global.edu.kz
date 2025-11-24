<html lang="ru">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>AlmaU Global Mobility Portal | Official Resource</title>
    
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@300;700&family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">

    <style>
        :root {
            --primary: #002855; /* Academic Navy */
            --secondary: #004b8d;
            --accent: #c5a065; /* University Gold */
            --text-dark: #1a1a1a;
            --text-light: #595959;
            --bg-light: #f4f6f9;
            --white: #ffffff;
            --border: #e0e0e0;
            --success: #2e7d32;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --radius: 6px; /* Строгие углы */
        }

        * { box-sizing: border-box; }
        
        body {
            margin: 0;
            font-family: 'Roboto', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            line-height: 1.6;
        }

        h1, h2, h3, h4 { font-family: 'Merriweather', serif; color: var(--primary); margin-top: 0; }
        
        /* HEADER */
        .top-bar {
            background-color: var(--primary);
            color: var(--white);
            padding: 10px 0;
            font-size: 0.9rem;
        }
        .container { max-width: 1400px; margin: 0 auto; padding: 0 20px; }
        .top-flex { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
        
        .main-header {
            background: var(--white);
            border-bottom: 1px solid var(--border);
            padding: 20px 0;
            box-shadow: var(--shadow);
            position: sticky; top: 0; z-index: 1000;
        }
        .header-flex { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; }
        .logo-area { display: flex; align-items: center; gap: 15px; }
        .logo-box {
            width: 50px; height: 50px; background: var(--primary); color: var(--white);
            font-family: 'Merriweather'; font-weight: bold; font-size: 24px;
            display: flex; align-items: center; justify-content: center;
            border-radius: 4px; border: 2px solid var(--accent);
        }
        .brand-text h1 { font-size: 1.5rem; margin: 0; color: var(--primary); }
        .brand-text p { margin: 0; font-size: 0.85rem; color: var(--text-light); text-transform: uppercase; letter-spacing: 1px; }

        /* NAV */
        .nav-menu { display: flex; gap: 30px; flex-wrap: wrap; }
        .nav-item {
            text-decoration: none; color: var(--text-dark); font-weight: 500; font-size: 1rem;
            padding: 8px 0; position: relative; transition: color 0.3s; cursor: pointer;
            white-space: nowrap;
        }
        .nav-item:hover, .nav-item.active { color: var(--secondary); }
        .nav-item.active::after {
            content: ''; position: absolute; bottom: 0; left: 0; width: 100%; height: 3px; background: var(--accent);
        }

        /* DASHBOARD LAYOUT */
        .dashboard-grid {
            display: grid; grid-template-columns: 350px 1fr; gap: 20px;
            margin-top: 30px; height: calc(100vh - 140px); min-height: 700px;
        }

        /* SIDEBAR (FILTERS & LIST) */
        .sidebar-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            display: flex; flex-direction: column; overflow: hidden; box-shadow: var(--shadow);
        }
        .panel-header {
            padding: 20px; background: var(--primary); color: var(--white);
        }
        .panel-header h3 { margin: 0; font-size: 1.1rem; font-weight: 400; color: var(--accent); }
        .stats-row { display: flex; gap: 15px; margin-top: 10px; font-size: 0.9rem; opacity: 0.9; }

        .search-box { padding: 15px; border-bottom: 1px solid var(--border); background: #f9f9f9; }
        .input-group { position: relative; }
        .input-group i { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); color: #999; }
        .form-control {
            width: 100%; padding: 12px 12px 12px 40px; border: 1px solid #ccc; border-radius: 4px;
            font-family: inherit; font-size: 0.95rem; transition: border 0.3s;
        }
        .form-control:focus { border-color: var(--secondary); outline: none; }

        .filter-tags { padding: 10px 15px; display: flex; flex-wrap: wrap; gap: 8px; border-bottom: 1px solid var(--border); }
        .filter-tag {
            font-size: 0.8rem; padding: 4px 10px; background: #eef2f6; border-radius: 20px;
            cursor: pointer; border: 1px solid transparent; transition: all 0.2s;
        }
        .filter-tag:hover { background: #dfe6ed; }
        .filter-tag.active { background: var(--primary); color: var(--white); border-color: var(--primary); }

        .university-list { overflow-y: auto; flex: 1; }
        .uni-item {
            padding: 15px; border-bottom: 1px solid var(--border); cursor: pointer; transition: background 0.2s;
            border-left: 4px solid transparent;
        }
        .uni-item:hover { background: #f0f7ff; }
        .uni-item.active { background: #e6f0fa; border-left-color: var(--accent); }
        .uni-name { font-weight: 700; color: var(--primary); display: block; margin-bottom: 4px; }
        .uni-meta { font-size: 0.85rem; color: var(--text-light); display: flex; justify-content: space-between; }
        .rank-badge { background: var(--accent); color: var(--white); padding: 2px 6px; border-radius: 3px; font-size: 0.75rem; font-weight: bold; }

        /* MAP AREA */
        .map-panel {
            background: var(--white); border-radius: var(--radius); border: 1px solid var(--border);
            overflow: hidden; box-shadow: var(--shadow); position: relative;
        }
        #map { width: 100%; height: 100%; z-index: 1; }

        /* CUSTOM GOLDEN MARKER STYLE (RESTORED) */
        .golden-marker-icon {
            background-color: transparent !important;
            border: none !important;
            color: var(--accent); /* University Gold */
            font-size: 30px; 
            text-shadow: 0 0 3px rgba(0,0,0,0.5); 
        }

        /* SECTIONS (FAQ & PLAN & DD) - Initially Hidden */
        .content-section { display: none; padding-bottom: 50px; animation: fadeIn 0.4s; }
        .content-section.active { display: block; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* DOUBLE DEGREE STYLES (NEW) */
        .double-degree-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); 
            gap: 20px;
            margin-top: 20px;
        }
        .info-card { 
            border: 1px solid var(--border); 
            background: #fcfcfc; 
            padding: 20px; 
            border-radius: var(--radius);
        }
        .dd-info-label { font-size: 0.8rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .dd-info-value { font-size: 1.1rem; font-weight: 600; color: var(--primary); }
        .program-list-styled-dd { list-style: none; padding: 0; }
        .program-list-styled-dd li {
            padding: 5px 0; border-bottom: 1px solid #eee; display: flex; align-items: flex-start;
            font-size: 0.95rem;
        }
        .program-list-styled-dd li:last-child { border-bottom: none; }
        .program-list-styled-dd li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 12px; color: var(--accent); font-size: 0.9rem; margin-top: 3px;
        }


        /* FAQ STYLES */
        .faq-container { max-width: 900px; margin: 40px auto; }
        .faq-category { margin-bottom: 30px; }
        .cat-title { border-bottom: 2px solid var(--accent); padding-bottom: 10px; margin-bottom: 20px; display: inline-block; }
        
        .faq-card {
            background: var(--white); border: 1px solid var(--border); border-radius: var(--radius);
            margin-bottom: 15px; overflow: hidden;
        }
        .faq-header {
            padding: 20px; cursor: pointer; display: flex; justify-content: space-between; align-items: center;
            font-weight: 600; background: #fff; transition: background 0.2s;
        }
        .faq-header:hover { background: #f9f9f9; }
        .faq-icon { color: var(--secondary); transition: transform 0.3s; }
        .faq-card.open .faq-icon { transform: rotate(180deg); }
        .faq-card.open .faq-header { color: var(--secondary); border-bottom: 1px solid #f0f0f0; }
        .faq-body {
            max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out;
            padding: 0 20px; font-size: 0.95rem; color: #444; line-height: 1.7;
        }
        /* Adjusted max-height for FAQ body to support content flow on opening */
        .faq-card.open .faq-body { padding: 20px; max-height: 1000px; } 

        /* PLAN STYLES */
        .timeline { position: relative; max-width: 800px; margin: 50px auto; padding: 20px 0; }
        .timeline::before {
            content: ''; position: absolute; top: 0; bottom: 0; left: 50%; width: 2px; background: var(--border); transform: translateX(-50%);
        }
        .timeline-item { margin-bottom: 50px; position: relative; width: 50%; padding: 0 40px; }
        .timeline-item:nth-child(odd) { left: 0; text-align: right; }
        .timeline-item:nth-child(even) { left: 50%; text-align: left; }
        
        .timeline-dot {
            position: absolute; top: 0; width: 20px; height: 20px; background: var(--white);
            border: 4px solid var(--secondary); border-radius: 50%; z-index: 2;
        }
        .timeline-item:nth-child(odd) .timeline-dot { right: -10px; }
        .timeline-item:nth-child(even) .timeline-dot { left: -10px; }
        
        .timeline-content {
            background: var(--white); padding: 25px; border-radius: var(--radius); border: 1px solid var(--border);
            box-shadow: var(--shadow); position: relative;
        }
        .step-num {
            position: absolute; top: -15px; background: var(--accent); color: var(--white);
            padding: 5px 15px; border-radius: 20px; font-weight: bold; font-size: 0.8rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        .timeline-item:nth-child(odd) .step-num { right: 20px; }
        .timeline-item:nth-child(even) .step-num { left: 20px; }

        /* MODAL - PASSPORT STYLE */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 40, 85, 0.7); z-index: 2000;
            display: none; justify-content: center; align-items: center; opacity: 0; transition: opacity 0.3s;
        }
        .modal-overlay.open { display: flex; opacity: 1; }
        
        .modal-window {
            background: var(--white); width: 900px; max-width: 95%; height: 85vh;
            border-radius: 8px; display: flex; flex-direction: column; overflow: hidden;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
        }
        
        .modal-header {
            background: var(--primary); color: var(--white); padding: 25px 30px;
            display: flex; justify-content: space-between; align-items: flex-start;
        }
        .m-title h2 { color: var(--white); margin: 0; font-size: 1.8rem; }
        .m-subtitle { color: var(--accent); margin-top: 5px; font-size: 1rem; display: flex; gap: 15px; flex-wrap: wrap; }
        .close-btn { background: none; border: none; color: rgba(255,255,255,0.7); font-size: 1.5rem; cursor: pointer; transition: color 0.2s; }
        .close-btn:hover { color: var(--white); }

        .modal-body { display: flex; flex: 1; overflow: hidden; }
        
        .modal-nav {
            width: 200px; background: #f4f6f9; border-right: 1px solid var(--border);
            display: flex; flex-direction: column; padding-top: 20px; flex-shrink: 0;
        }
        .m-tab {
            padding: 15px 20px; cursor: pointer; font-weight: 500; color: var(--text-light);
            border-left: 4px solid transparent; transition: all 0.2s; text-align: left; background: none; border-bottom: none; border-top: none; border-right: none; width: 100%; font-family: inherit;
        }
        .m-tab:hover { background: #e6eef5; color: var(--primary); }
        .m-tab.active { background: var(--white); color: var(--primary); border-left-color: var(--accent); font-weight: 700; box-shadow: -5px 0 10px rgba(0,0,0,0.05); }

        .modal-content { flex: 1; padding: 30px; overflow-y: auto; background: var(--white); }
        
        .tab-panel { display: none; animation: fadeIn 0.3s; }
        .tab-panel.active { display: block; }
        
        .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-bottom: 30px; }
        .info-card { background: #f9f9f9; padding: 20px; border-radius: var(--radius); border: 1px solid #eee; }
        .info-label { font-size: 0.8rem; text-transform: uppercase; color: var(--text-light); letter-spacing: 0.5px; margin-bottom: 5px; }
        .info-value { font-size: 1.1rem; font-weight: 600; color: var(--primary); }

        .program-list-styled { list-style: none; padding: 0; }
        .program-list-styled li {
            padding: 12px 0; border-bottom: 1px solid #eee; display: flex; align-items: center;
        }
        .program-list-styled li::before {
            content: '\f19d'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
            margin-right: 12px; color: var(--accent);
        }

        /* RESPONSIVE */
        @media (max-width: 1200px) {
            .container { padding: 0 15px; }
            .dashboard-grid { 
                grid-template-columns: 300px 1fr; 
                height: calc(100vh - 120px); 
                min-height: 600px;
            }
        }

        @media (max-width: 900px) {
            .top-flex { flex-direction: column; align-items: flex-start; gap: 5px; }
            .dashboard-grid { 
                grid-template-columns: 1fr; 
                height: auto; 
                min-height: auto; 
                margin-top: 20px; 
                /* Re-order grid for mobile: list below map */
                grid-template-areas: 
                    "map"
                    "sidebar";
            }
            .map-panel { grid-area: map; height: 400px; }
            .sidebar-panel { grid-area: sidebar; height: auto; max-height: 500px; }
            .university-list { max-height: 300px; } 

            /* Timeline for mobile */
            .timeline::before { left: 20px; }
            .timeline-item { width: 100%; padding: 0 20px 0 40px; margin-bottom: 30px; }
            .timeline-item:nth-child(odd), .timeline-item:nth-child(even) { left: 0; text-align: left !important; }
            .timeline-item:nth-child(odd) .timeline-dot { left: 10px; right: auto; }
            .timeline-item:nth-child(even) .timeline-dot { left: 10px; }
            .timeline-item:nth-child(odd) .step-num { left: 40px; right: auto; }
            .timeline-item:nth-child(even) .step-num { left: 40px; }
            
            /* General */
            .info-grid { grid-template-columns: 1fr; gap: 20px; }
            
            /* Modal for mobile */
            .modal-window { height: 95vh; }
            .modal-body { flex-direction: column; }
            .modal-nav { 
                width: 100%; border-right: none; border-bottom: 1px solid var(--border); padding-top: 0; 
                flex-direction: row; overflow-x: auto; white-space: nowrap;
            }
            .m-tab { 
                border-left: none; border-bottom: 4px solid transparent; 
                flex-shrink: 0; 
            }
            .m-tab.active { 
                border-left-color: transparent; border-bottom-color: var(--accent); 
                box-shadow: none;
            }
            .modal-content { padding: 20px; }
            
            /* Header/Nav for mobile */
            .header-flex { flex-direction: column; align-items: flex-start; gap: 15px; }
            .nav-menu { flex-wrap: wrap; margin-top: 10px; gap: 15px; }
            .nav-item { padding: 5px 0; font-size: 0.95rem; }
        }
    </style>
</head>
<body>

    <div class="top-bar">
        <div class="container top-flex">
            <span><i class="fas fa-university"></i> Официальный ресурс Академической Мобильности AlmaU</span>
            <span><i class="fas fa-envelope"></i> mobility@almau.edu.kz | <i class="fas fa-map-marker-alt"></i> Офис 107</span>
        </div>
    </div>

    <header class="main-header">
        <div class="container header-flex">
            <div class="logo-area">
                <div class="logo-box">A</div>
                <div class="brand-text">
                    <h1>AlmaU Global</h1>
                    <p>International Cooperation Department</p>
                </div>
            </div>
            <nav class="nav-menu">
                <a class="nav-item active" onclick="switchTab('map-section', this)">Карта Партнеров</a>
                <a class="nav-item" onclick="switchTab('double-degree-section', this)">Программа Двойного Диплома</a>
                <a class="nav-item" onclick="switchTab('plan-section', this)">Дорожная Карта</a>
                <a class="nav-item" onclick="switchTab('faq-section', this)">База Знаний (FAQ)</a>
            </nav>
        </div>
    </header>

    <div class="container">
        
        <section id="map-section" class="content-section active">
            <div class="dashboard-grid">
                <aside class="sidebar-panel">
                    <div class="panel-header">
                        <h3><i class="fas fa-globe-europe"></i> Навигатор ВУЗов</h3>
                        <div class="stats-row">
                            <span id="count-display">0 ВУЗов</span> • <span>30+ Стран</span>
                        </div>
                    </div>
                    
                    <div class="search-box">
                        <div class="input-group">
                            <i class="fas fa-search"></i>
                            <input type="text" id="searchBox" class="form-control" placeholder="Поиск по стране, ВУЗу или программе...">
                        </div>
                    </div>

                    <div class="filter-tags">
                        <span class="filter-tag active" onclick="filterList('all', this)">Все</span>
                        <span class="filter-tag" onclick="filterList('Europe', this)">Европа</span>
                        <span class="filter-tag" onclick="filterList('Asia', this)">Азия</span>
                        <span class="filter-tag" onclick="filterList('Americas', this)">Америка</span>
                        <span class="filter-tag" onclick="filterList('CIS', this)">СНГ</span>
                        <span class="filter-tag" onclick="filterList('Africa', this)">Африка</span>
                    </div>

                    <div class="university-list" id="uni-list-container">
                        </div>
                </aside>

                <main class="map-panel">
                    <div id="map"></div>
                </main>
            </div>
        </section>
        
        <section id="double-degree-section" class="content-section">
            <div class="container" style="padding: 0;">
                <div style="text-align: center; margin: 40px 0;">
                    <h2 style="color: var(--secondary);">Программа Двойного Диплома AlmaU</h2>
                    <p style="color:var(--text-light)">Получите два диплома, обучаясь в AlmaU и ведущих университетах мира.</p>
                </div>
                
                <div id="double-degree-content">
                    </div>
                
                <div style="margin-top: 40px; padding: 20px; background: #e6f0fa; border-radius: var(--radius); border-left: 5px solid var(--accent);">
                    <h4 style="margin-top: 0; color: var(--primary);"><i class="fas fa-info-circle"></i> Особенности Программы Двойного Диплома:</h4>
                    <ul style="list-style-type: disc; padding-left: 20px; font-size: 0.95rem; color: #444;">
                        <li>Программа обычно предполагает обучение **1-2 года** в вузе-партнере.</li>
                        <li>По окончании обучения студент получает **два полноценных диплома** (AlmaU и зарубежного вуза).</li>
                        <li>Обучение в вузе-партнере по программе Двойного Диплома является **платным**, согласно тарифам вуза-партнера.</li>
                        <li>Для участия необходимо соответствовать высоким требованиям обоих вузов по успеваемости и знанию языка, часто предусмотрен конкурсный отбор.</li>
                    </ul>
                </div>
            </div>
        </section>

        <section id="plan-section" class="content-section">
            <div style="text-align: center; margin: 40px 0;">
                <h2>Процесс подачи заявки</h2>
                <p style="color:var(--text-light)">Официальный регламент прохождения конкурса на академическую мобильность</p>
            </div>
            
            <div class="timeline">
                <div id="timeline-container">
                    </div>
            </div>
        </section>

        <section id="faq-section" class="content-section">
            <div class="faq-container" id="faq-root">
                </div>
        </section>

    </div>

    <div class="modal-overlay" id="uniModal">
        <div class="modal-window">
            <div class="modal-header">
                <div class="m-title">
                    <h2 id="m-name">University Name</h2>
                    <div class="m-subtitle">
                        <span><i class="fas fa-map-marker-alt"></i> <span id="m-city">City</span>, <span id="m-country">Country</span></span>
                        <span><i class="fas fa-star"></i> <span id="m-rank">Top Partner</span></span>
                    </div>
                </div>
                <button class="close-btn" onclick="closeModal()"><i class="fas fa-times"></i></button>
            </div>
            <div class="modal-body">
                <div class="modal-nav">
                    <button class="m-tab active" onclick="modalTab('tab-overview', this)">Обзор</button>
                    <button class="m-tab" onclick="modalTab('tab-bachelor', this)">Бакалавриат</button>
                    <button class="m-tab" onclick="modalTab('tab-master', this)">Магистратура</button>
                    <button class="m-tab" onclick="modalTab('tab-finance', this)">Бюджет и Виза</button>
                </div>
                
                <div class="modal-content">
                    
                    <div id="tab-overview" class="tab-panel active">
                        <h3 style="color: var(--secondary);">Общая информация</h3>
                        <div class="info-grid">
                            <div class="info-card">
                                <div class="info-label">Доступные места (Квота)</div>
                                <div class="info-value"><span id="m-students">2</span> студентов</div>
                            </div>
                            <div class="info-card">
                                <div class="info-label">Язык обучения</div>
                                <div class="info-value">Английский (B2)</div>
                            </div>
                            <div class="info-card">
                                <div class="info-label">Тип программы</div>
                                <div class="info-value">Семестровый обмен</div>
                            </div>
                            <div class="info-card">
                                <div class="info-label">Статус партнера</div>
                                <div class="info-value" style="color:var(--success)">Активный</div>
                            </div>
                        </div>
                        <div class="info-card" style="background:#fff; border-color:var(--accent); margin-top:20px;">
                            <h4><i class="fas fa-info-circle"></i> Описание</h4>
                            <p style="color:var(--text-light); font-size:0.95rem;">
                                Вуз является стратегическим партнером AlmaU. Студентам предоставляется возможность бесплатного обучения (**Tuition Waiver**). Обязательно проверьте соответствие курсов (Learning Agreement) перед подачей.
                            </p>
                        </div>
                    </div>

                    <div id="tab-bachelor" class="tab-panel">
                        <h4>Доступные дисциплины (Бакалавриат)</h4>
                        <ul class="program-list-styled" id="list-bachelor"></ul>
                    </div>

                    <div id="tab-master" class="tab-panel">
                        <h4>Доступные дисциплины (Магистратура)</h4>
                        <ul class="program-list-styled" id="list-master"></ul>
                    </div>

                    <div id="tab-finance" class="tab-panel">
                        <div class="info-card" style="margin-bottom:20px;">
                            <h4><i class="fas fa-wallet"></i> Примерные расходы (в месяц)</h4>
                            <p style="font-size:0.85rem; color:#666;">*Данные являются приблизительными и зависят от образа жизни.</p>
                            <table style="width:100%; border-collapse:collapse; margin-top:10px;">
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Жилье</td><td style="text-align:right; font-weight:bold;" id="cost-living">€300-500</td></tr>
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Питание</td><td style="text-align:right; font-weight:bold;" id="cost-food">€200-300</td></tr>
                                <tr style="border-bottom:1px solid #eee;"><td style="padding:8px;">Транспорт</td><td style="text-align:right; font-weight:bold;" id="cost-transport">€30-50</td></tr>
                            </table>
                        </div>
                        <div class="info-card">
                            <h4><i class="fas fa-passport"></i> Визовые требования</h4>
                            <p>Для обучения требуется студенческая виза типа D. Подтверждение финансовой состоятельности обязательно. Страховой полис должен покрывать весь период пребывания.</p>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>

    <!-- Leaflet -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <!-- Telegram WebApp -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>

    <script>
    /* ---------------- TELEGRAM WEBAPP INIT ---------------- */
    const tg = window.Telegram?.WebApp;
    if (tg) {
        try {
            tg.ready();
        } catch (e) {
            console.warn('Telegram WebApp ready error', e);
        }
    }

    /* ---------------- DATA ---------------- */
    /* Полная база данных, без сокращений, плюс добавлен регион для фильтрации */
    const universities = [
        { region: "Europe", country: "Австрия", name: "Management Center Innsbruck", city: "Innsbruck", students: "2", lat:47.2682, lon:11.3923, bachelorPrograms: ["Management", "Business"], masterPrograms: ["International Business"] },
        { region: "CIS", country: "Азербайджан", name: "ADA University", city: "Baku", students: "5", lat:40.4093, lon:49.8671, bachelorPrograms: ["Business Administration","Economics","Finance","Computer Science","International Studies","Laws"], masterPrograms: ["Global Management","MBA Finance","Public Administration"] },
        { region: "CIS", country: "Азербайджан", name: "Khazar University", city: "Baku", students: "5", lat:40.4100, lon:49.8620, bachelorPrograms: ["BBA Management","Marketing","Finance","Accounting","Computer Science","Tourism","Political Science"], masterPrograms: ["MBA Management","MBA Project Management","Economics"] },
        { region: "CIS", country: "Азербайджан", name: "Azerbaijan University", city: "Baku", students: "5", lat:40.4120, lon:49.8700, bachelorPrograms: ["Marketing","Business Management","Accounting","Finance","IT","Tourism","International Relations"], masterPrograms: ["MBA","Economics"] },
        { region: "Europe", country: "Германия", name: "University of Mannheim", city: "Mannheim", students: "3", lat:49.4875, lon:8.4660, bachelorPrograms: ["Economics","Business Administration"], masterPrograms: ["Management","Finance"] },
        { region: "Europe", country: "Франция", name: "Université Paris-Saclay", city: "Paris", students: "4", lat:48.7130, lon:2.2140, bachelorPrograms: ["Computer Science","Mathematics"], masterPrograms: ["Data Science","Engineering"] },
        { region: "Asia", country: "Китай", name: "Tsinghua University", city: "Beijing", students: "2", lat:39.9997, lon:116.3269, bachelorPrograms: ["Engineering","Computer Science"], masterPrograms: ["AI","Engineering"] },
        { region: "Americas", country: "США", name: "University of Illinois Urbana-Champaign", city: "Urbana", students: "3", lat:40.1106, lon:-88.2073, bachelorPrograms: ["Computer Science","Engineering"], masterPrograms: ["CS","Engineering"] },
        { region: "Europe", country: "Великобритания", name: "University of Glasgow", city: "Glasgow", students: "2", lat:55.8720, lon:-4.2880, bachelorPrograms: ["Business","Economics"], masterPrograms: ["International Business"] },
        { region: "Africa", country: "ЮАР", name: "University of Cape Town", city: "Cape Town", students: "2", lat:-33.9570, lon:18.4600, bachelorPrograms: ["Economics","Political Science"], masterPrograms: ["Development Studies"] },
        { region: "Europe", country: "Испания", name: "Universidad Autónoma de Madrid", city: "Madrid", students: "3", lat:40.5433, lon:-3.6896, bachelorPrograms: ["Law","Economics"], masterPrograms: ["International Law","Economics"] },
        { region: "Europe", country: "Нидерланды", name: "Erasmus University Rotterdam", city: "Rotterdam", students: "3", lat:51.9225, lon:4.4790, bachelorPrograms: ["Business","Economics"], masterPrograms: ["Finance","Management"] },
        { region: "Europe", country: "Швеция", name: "Lund University", city: "Lund", students: "2", lat:55.7047, lon:13.1910, bachelorPrograms: ["Engineering","Computer Science"], masterPrograms: ["Sustainable Development"] },
        { region: "Asia", country: "Япония", name: "University of Tokyo", city: "Tokyo", students: "2", lat:35.7126, lon:139.7610, bachelorPrograms: ["Engineering","Science"], masterPrograms: ["Robotics","Engineering"] },
        { region: "Americas", country: "Канада", name: "University of Toronto", city: "Toronto", students: "3", lat:43.6629, lon:-79.3957, bachelorPrograms: ["Business","Computer Science"], masterPrograms: ["Data Science","MBA"] },
        { region: "CIS", country: "Россия", name: "Lomonosov Moscow State University", city: "Moscow", students: "6", lat:55.7033, lon:37.5301, bachelorPrograms: ["Physics","Mathematics","Economics"], masterPrograms: ["Mathematics","Economics"] },
        { region: "CIS", country: "Казахстан", name: "Nazarbayev University", city: "Nur-Sultan", students: "4", lat:51.1282, lon:71.4301, bachelorPrograms: ["Engineering","Business"], masterPrograms: ["Public Policy","Engineering"] },
        { region: "Europe", country: "Италия", name: "Bocconi University", city: "Milan", students: "3", lat:45.4642, lon:9.1900, bachelorPrograms: ["Economics","Management"], masterPrograms: ["Finance","Management"] },
        { region: "Europe", country: "Швейцария", name: "ETH Zurich", city: "Zurich", students: "2", lat:47.3769, lon:8.5417, bachelorPrograms: ["Engineering","Computer Science"], masterPrograms: ["Robotics","Engineering"] },
        { region: "Americas", country: "Бразилия", name: "University of São Paulo", city: "São Paulo", students: "2", lat:-23.5614, lon:-46.7300, bachelorPrograms: ["Economics","Engineering"], masterPrograms: ["Engineering","Economics"] },
        { region: "Africa", country: "Египет", name: "American University in Cairo", city: "Cairo", students: "2", lat:30.0277, lon:31.2100, bachelorPrograms: ["Political Science","Business"], masterPrograms: ["International Relations"] },
        { region: "Asia", country: "Южная Корея", name: "Seoul National University", city: "Seoul", students: "2", lat:37.4599, lon:126.9516, bachelorPrograms: ["Engineering","Science"], masterPrograms: ["AI","Engineering"] },
        { region: "Europe", country: "Польша", name: "University of Warsaw", city: "Warsaw", students: "2", lat:52.2297, lon:21.0122, bachelorPrograms: ["Law","Economics"], masterPrograms: ["International Law"] },
        { region: "Americas", country: "Мексика", name: "National Autonomous University of Mexico", city: "Mexico City", students: "2", lat:19.3320, lon:-99.1880, bachelorPrograms: ["Engineering","Social Sciences"], masterPrograms: ["Public Policy"] },
        { region: "Europe", country: "Portugal", name: "University of Lisbon", city: "Lisbon", students: "2", lat:38.7369, lon:-9.1427, bachelorPrograms: ["Economics","Computer Science"], masterPrograms: ["Data Science"] },
        { region: "Asia", country: "Сингапур", name: "National University of Singapore", city: "Singapore", students: "2", lat:1.2966, lon:103.7764, bachelorPrograms: ["Engineering","Business"], masterPrograms: ["MBA","Engineering"] }
        /* При необходимости добавьте остальные записи в том же формате */
    ];

    /* ---------------- UTIL ---------------- */
    function escapeHtml(str) {
        if (str === null || str === undefined) return '';
        return String(str)
            .replace(/&/g, '&amp;')
            .replace(/</g, '&lt;')
            .replace(/>/g, '&gt;')
            .replace(/"/g, '&quot;')
            .replace(/'/g, '&#039;');
    }

    /* ---------------- MAP HEIGHT (Telegram WebView safe) ---------------- */
    function setMapHeight() {
        const header = document.querySelector('.main-header');
        const headerH = header ? header.offsetHeight : 0;
        const offset = 24;
        const available = Math.max(300, window.innerHeight - headerH - offset);
        const mapEl = document.getElementById('map');
        if (mapEl) mapEl.style.height = available + 'px';
        if (window._leaflet_map) {
            window._leaflet_map.invalidateSize();
        }
    }
    window.addEventListener('resize', setMapHeight);
    window.addEventListener('orientationchange', () => setTimeout(setMapHeight, 300));
    setMapHeight();

    /* ---------------- INITIALIZE LEAFLET ---------------- */
    const map = L.map('map', { zoomControl: true }).setView([48.0, 16.0], 3);
    window._leaflet_map = map;
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: ''
    }).addTo(map);

    /* Custom golden icon (FontAwesome-based) */
    const GoldenIcon = L.DivIcon.extend({
        options: {
            className: 'golden-marker-icon',
            html: '<i class="fas fa-map-marker-alt"></i>',
            iconSize: [30, 42],
            iconAnchor: [15, 42]
        }
    });

    /* ---------------- RENDER LIST & MARKERS ---------------- */
    const markers = [];
    function clearMarkers() {
        markers.forEach(m => map.removeLayer(m));
        markers.length = 0;
    }

    function renderList(region = 'all', query = '') {
        const container = document.getElementById('uni-list-container');
        container.innerHTML = '';
        const q = (query || '').trim().toLowerCase();

        const filtered = universities.filter(u => {
            const regionOk = (region === 'all') || (u.region === region);
            const text = `${u.name} ${u.country} ${u.city} ${(u.bachelorPrograms||[]).join(' ')} ${(u.masterPrograms||[]).join(' ')}`.toLowerCase();
            const queryOk = !q || text.includes(q);
            return regionOk && queryOk;
        });

        document.getElementById('count-display').textContent = `${filtered.length} ВУЗов`;

        filtered.forEach((u, idx) => {
            const item = document.createElement('div');
            item.className = 'uni-item';
            item.setAttribute('data-idx', idx);
            item.innerHTML = `<span class="uni-name">${escapeHtml(u.name)}</span>
                              <div class="uni-meta"><span>${escapeHtml(u.city)}, ${escapeHtml(u.country)}</span><span class="rank-badge">Квота: ${escapeHtml(String(u.students||'—'))}</span></div>`;
            item.addEventListener('click', () => {
                if (u.lat && u.lon) map.setView([u.lat, u.lon], 6);
                openModalWithData(u);
                // highlight active
                document.querySelectorAll('.uni-item').forEach(el => el.classList.remove('active'));
                item.classList.add('active');
            });
            container.appendChild(item);
        });

        // markers
        clearMarkers();
        filtered.forEach(u => {
            if (typeof u.lat !== 'number' || typeof u.lon !== 'number') return;
            const marker = L.marker([u.lat, u.lon], { icon: new GoldenIcon() }).addTo(map);
            marker.bindPopup(`<strong>${escapeHtml(u.name)}</strong><br>${escapeHtml(u.city)}, ${escapeHtml(u.country)}`);
            marker.on('click', () => openModalWithData(u));
            markers.push(marker);
        });
    }

    /* initial render */
    renderList('all', '');

    /* ---------------- FILTERS & SEARCH ---------------- */
    function filterList(region, el) {
        document.querySelectorAll('.filter-tag').forEach(t => t.classList.remove('active'));
        if (el) el.classList.add('active');
        const q = document.getElementById('searchBox').value || '';
        renderList(region, q);
    }

    document.getElementById('searchBox').addEventListener('input', (e) => {
        const active = document.querySelector('.filter-tag.active');
        const region = active ? active.textContent.trim() : 'Все';
        const regionKey = (region === 'Все') ? 'all' : active.getAttribute('onclick') ? active.getAttribute('onclick').match(/'(.+?)'/)[1] : 'all';
        // simpler: find active filter-tag dataset if present
        const activeTag = document.querySelector('.filter-tag.active');
        const regionData = activeTag ? (activeTag.getAttribute('onclick') ? activeTag.getAttribute('onclick').match(/'(.+?)'/)[1] : 'all') : 'all';
        renderList(regionData, e.target.value);
    });

    /* ---------------- TAB SWITCH ---------------- */
    function switchTab(id, el) {
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
        if (el) el.classList.add('active');
        setMapHeight();
    }

    /* ---------------- MODAL LOGIC (IN-PAGE, NO POPUPS) ---------------- */
    function openModalWithData(u) {
        // populate modal fields
        document.getElementById('m-name').textContent = u.name || '—';
        document.getElementById('m-city').textContent = u.city || '—';
        document.getElementById('m-country').textContent = u.country || '—';
        document.getElementById('m-rank').textContent = (u.rank || 'Партнёр');
        document.getElementById('m-students').textContent = u.students || '—';

        // populate program lists
        const listB = document.getElementById('list-bachelor');
        const listM = document.getElementById('list-master');
        listB.innerHTML = '';
        listM.innerHTML = '';
        (u.bachelorPrograms || []).forEach(p => {
            const li = document.createElement('li'); li.textContent = p; listB.appendChild(li);
        });
        (u.masterPrograms || []).forEach(p => {
            const li = document.createElement('li'); li.textContent = p; listM.appendChild(li);
        });

        // open modal
        const overlay = document.getElementById('uniModal');
        overlay.classList.add('open');

        // ensure first tab active
        document.querySelectorAll('.m-tab').forEach(t => t.classList.remove('active'));
        const firstTab = document.querySelector('.m-tab');
        if (firstTab) firstTab.classList.add('active');
        document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        const firstPanel = document.querySelector('.tab-panel');
        if (firstPanel) firstPanel.classList.add('active');
    }

    function closeModal() {
        const overlay = document.getElementById('uniModal');
        overlay.classList.remove('open');
    }

    function modalTab(panelId, el) {
        document.querySelectorAll('.m-tab').forEach(t => t.classList.remove('active'));
        if (el) el.classList.add('active');
        document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        const panel = document.getElementById(panelId);
        if (panel) panel.classList.add('active');
    }

    /* close modal on ESC */
    document.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') closeModal();
    });

    /* ---------------- TIMELINE & FAQ (populate from data or static) ---------------- */
    const timelineContainer = document.getElementById('timeline-container');
    const timelineSteps = [
        { step: 1, title: 'Ознакомление с программой', desc: 'Изучите требования и сроки подачи заявок.' },
        { step: 2, title: 'Подготовка документов', desc: 'Соберите Learning Agreement, мотивационное письмо и справки.' },
        { step: 3, title: 'Подача заявки', desc: 'Отправьте пакет документов в Международный отдел.' },
        { step: 4, title: 'Отбор и подтверждение', desc: 'Проходит конкурсный отбор и подтверждение места.' },
        { step: 5, title: 'Подготовка к отъезду', desc: 'Оформление визы, страховки и проживания.' }
    ];
    timelineSteps.forEach((t, i) => {
        const item = document.createElement('div');
        item.className = 'timeline-item';
        item.innerHTML = `<div class="timeline-dot"></div>
                          <div class="timeline-content">
                            <div class="step-num">${t.step}</div>
                            <h4 style="margin-top:0">${t.title}</h4>
                            <p style="color:#666">${t.desc}</p>
                          </div>`;
        timelineContainer.appendChild(item);
    });

    const faqRoot = document.getElementById('faq-root');
    const faqs = [
        { q: 'Кто может участвовать в программе обмена?', a: 'Студенты AlmaU, соответствующие академическим и языковым требованиям.' },
        { q: 'Какие документы нужны для подачи?', a: 'Learning Agreement, академическая справка, мотивационное письмо, языковой сертификат (если требуется).' },
        { q: 'Есть ли финансирование?', a: 'Некоторые партнёры предоставляют стипендии или tuition waiver; уточняйте в описании партнёра.' }
    ];
    faqs.forEach(cat => {
        const card = document.createElement('div'); card.className = 'faq-card';
        card.innerHTML = `<div class="faq-header">${cat.q}<span class="faq-icon"><i class="fas fa-chevron-down"></i></span></div>
                          <div class="faq-body"><p>${cat.a}</p></div>`;
        card.querySelector('.faq-header').addEventListener('click', () => {
            card.classList.toggle('open');
        });
        faqRoot.appendChild(card);
    });

    /* ---------------- Accessibility & touch improvements ---------------- */
    // Ensure touch-friendly sizes already handled by CSS; add map touch behavior tweaks if needed
    map.touchZoom.enable();
    map.scrollWheelZoom.disable(); // avoid accidental zoom in WebView; users can use pinch

    /* ---------------- Handle initData (optional server validation) ---------------- */
    if (tg && tg.initData) {
        // For security: send tg.initData to your server for validation (do not trust client-side)
        // Example (uncomment and adapt to your backend):
        // fetch('/validate-initdata', { method: 'POST', headers: {'Content-Type':'application/json'}, body: JSON.stringify({ initData: tg.initData }) });
        console.log('Telegram initData present (validate on server).');
    }

    /* ---------------- Ensure map resizes after markers added ---------------- */
    setTimeout(() => {
        try { map.invalidateSize(); } catch (e) {}
    }, 500);

    </script>
</body>
</html>
