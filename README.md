<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCDA - সেন্ট্রাল ফিক্সড অ্যাসেট রেজিস্টার ও লগইন সিস্টেম</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        .login-body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #f4f6f9; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .login-container { background: white; padding: 30px; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); width: 380px; text-align: center; border-top: 5px solid #28a745; }
        .login-container h2 { color: #333; margin-bottom: 5px; font-size: 22px; }
        .login-container p { color: #666; font-size: 14px; margin-bottom: 25px; }
        .form-group { text-align: left; margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; color: #444; font-weight: bold; font-size: 14px; }
        .form-group select, .form-group input { width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 4px; box-sizing: border-box; font-size: 14px; }
        .login-btn { width: 100%; padding: 12px; background-color: #28a745; border: none; color: white; font-size: 16px; font-weight: bold; border-radius: 4px; cursor: pointer; margin-top: 10px; }
        .login-btn:hover { background-color: #218838; }
        .footer-text { margin-top: 20px; font-size: 12px; color: #888; }

        .app-body { background-color: #f4f7f6; font-family: 'SolaimanLipi', Arial, sans-serif; color: #000; }
        .app-container { max-width: 1800px; margin: 30px auto; background: #fff; padding: 30px; border-radius: 10px; box-shadow: 0 4px 20px rgba(0,0,0,0.08); }
        .header-title { text-align: center; margin-bottom: 20px; }
        .ccda-main-title { color: #b30000 !important; font-weight: 800; margin-bottom: 5px; text-align: center; }
        .ccda-sub-title { color: #000 !important; font-weight: bold; }
        .page-heading { color: #000 !important; font-weight: bold; }
        .notice-board { background: #fff3cd; border: 1px solid #ffeeba; color: #856404; padding: 10px 15px; border-radius: 6px; font-weight: bold; margin-bottom: 25px; display: flex; align-items: center; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .notice-title { background: #856404; color: #fff; padding: 3px 10px; border-radius: 4px; font-size: 13px; margin-right: 12px; white-space: nowrap; }
        .notice-marquee { width: 100%; overflow: hidden; }
        .table-bordered { border: 2px solid #444; }
        .table-bordered th, .table-bordered td { border: 1px solid #777; vertical-align: middle; font-size: 13px; color: #000; }
        .table th { background-color: #e9ecef; color: #000; font-weight: bold; text-align: center; }
        .col-asset-name { min-width: 220px; text-align: left !important; }
        .table input, .table select { width: 100%; border: none; background: transparent; text-align: center; font-weight: 700; color: #000 !important; padding: 6px 4px; }
        .table input:focus, .table select:focus { outline: 2px solid #0d6efd; background: #fff; border-radius: 4px; }
        
        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }

        .editable-cell { background-color: #fffdf0 !important; border: 1px dashed #ffc107 !important; }
        .category-header { background: linear-gradient(135deg, #0dcaf0, #0d6efd) !important; color: white !important; font-weight: bold; text-align: left; padding-left: 15px !important; }
        .grand-total-row { font-weight: bold; background-color: #d1e7dd !important; color: #000; }
        .row-hidden { background-color: #f8d7da !important; opacity: 0.4; }
        .row-hidden input { color: #842029 !important; text-decoration: line-through; }
        .btn-hide-active { background-color: #842029 !important; color: white !important; border-color: #842029 !important; }
        .qty-clickable { cursor: pointer; color: #0d6efd; text-decoration: underline; }
        .qty-clickable:hover { color: #0056b3; }
        .signature-section { display: flex; justify-content: space-between; margin-top: 220px; text-align: center; }
        .signature-box { width: 30%; border-top: 2px solid #000; padding-top: 5px; font-weight: bold; }
        .print-only-header { display: none; }

        @media print {
            @page { size: legal landscape; margin: 8mm; }
            body { background: #fff; font-size: 10pt; color: #000; }
            .no-print { display: none !important; }
            .print-only-header { display: block !important; }
            .app-container { box-shadow: none; margin: 0; padding: 0; width: 100%; max-width: 100%; }
            .table { font-size: 9pt; width: 100% !important; color: #000; }
            .row-hidden { display: none !important; }
            .signature-section { display: flex !important; flex-direction: row !important; justify-content: space-between !important; page-break-inside: avoid; margin-top: 200px; }
        }
    </style>
</head>
<body id="mainBody" class="login-body">

    <!-- এমবেডেড ডেটা হোল্ডার -->
    <script id="embeddedDataStore" type="application/json">/*EMBEDDED_DATA_PLACEHOLDER*/</script>

    <!-- LOGIN VIEW CONTAINER -->
    <div id="loginViewContainer" class="login-container">
        <h2>CCDA SMART Fixed Assets Register</h2>
        <p>স্থাবর সম্পত্তি ব্যবস্থাপনা সিস্টেম (১২০ শাখা ও হেড অফিস)</p>
        
        <form onsubmit="handleLoginSubmit(event)">
            <div class="form-group">
                <label for="loginBranchSelect">শাখা বা হেড অফিস নির্বাচন করুন:</label>
                <select id="loginBranchSelect" name="branch_code" required>
                    <option value="">-- অপশন সিলেক্ট করুন --</option>
                    <option value="00">Head Office (00) - অ্যাডমিন প্যানেল</option>
                </select>
            </div>

            <div class="form-group">
                <label for="username">ইউজার আইডি (User ID):</label>
                <input type="text" id="username" name="username" placeholder="উদা: branch001 বা admin" required>
            </div>

            <div class="form-group">
                <label for="password">পাসওয়ার্ড:</label>
                <input type="password" id="password" name="password" placeholder="পাসওয়ার্ড দিন" required>
            </div>

            <button type="submit" class="login-btn">সিস্টেমে প্রবেশ করুন (Login)</button>
        </form>

        <div class="footer-text">
            সিস্টেম ১০০% আপডেট ও প্রস্তুত (১২০ ব্রাঞ্চ ও হেড অফিস সেন্ট্রাল সিঙ্ক এক্টিভ)
        </div>
    </div>

    <!-- MAIN APP VIEW CONTAINER -->
    <div id="appViewContainer" style="display: none;" class="w-100">
        <div class="app-container">
            <div class="header-title no-print">
                <h3 class="fw-bold ccda-main-title">Centre for Community Development Assistance (CCDA)</h3>
                <p class="ccda-sub-title mb-1">House-1/8, Block-G, Lalmatia Housing Estate, Dhaka-1207, Bangladesh</p>
                <h5 class="text-uppercase page-heading" id="pageTitleHeading">Fixed Assets Register</h5>
            </div>

            <div class="notice-board no-print">
                <span class="notice-title">📢 নোটিশ বোর্ড</span>
                <marquee behavior="scroll" direction="left" class="notice-marquee">
                    বিশেষ প্রয়োজনে Md. Hafizur Rahman (01734251323) নম্বরে যোগাযোগ করুন। 
                </marquee>
            </div>

            <div id="syncStatus" class="alert alert-secondary py-1 px-3 mb-3 no-print text-center fw-bold" style="font-size: 12px;">
                🟢 সিস্টেম ১০০% আপডেট ও প্রস্তুত
            </div>

            <div class="row mb-4 no-print bg-light p-3 rounded border" id="topControlPanelRow">
                <div class="col-md-3" id="headOfficeSelectorWrapper" style="display: none;">
                    <label class="form-label fw-bold text-danger">Head Office: Select Branch (001 to 120):</label>
                    <select id="branchSelector" class="form-select bg-white text-dark fw-bold border-danger" onchange="changeBranchCode(this.value)">
                    </select>
                </div>
                <div class="col-md-3">
                    <label class="form-label fw-bold">Name Of Branch & Code:</label>
                    <input type="text" id="branchName" class="form-control bg-white text-dark" value="Branch --" oninput="updateMetaInfo('branchName', this.value)">
                </div>
                <div class="col-md-3">
                    <label class="form-label fw-bold">Financial Year:</label>
                    <input type="text" id="finYear" class="form-control bg-white text-dark" value="2026-2027" oninput="updateMetaInfo('finYear', this.value)">
                </div>
                <div class="col-md-3">
                    <label class="form-label fw-bold">Duration:</label>
                    <input type="text" id="duration" class="form-control bg-white text-dark" value="01/07/2026 To 30/06/2027" oninput="updateMetaInfo('duration', this.value)">
                </div>
            </div>

            <!-- Year Tabs -->
            <ul class="nav nav-pills mb-4 no-print gap-1 align-items-center" id="yearTabsList" role="tablist">
            </ul>

            <div class="mb-3 no-print d-flex justify-content-between action-toolbar">
                <div>
                    <button class="btn btn-success btn-sm fw-bold me-1" onclick="addRow(0)">+ আসবাবপত্র সারি</button>
                    <button class="btn btn-success btn-sm fw-bold me-1" onclick="addRow(1)">+ ইকুইপমেন্ট সারি</button>
                    <button class="btn btn-success btn-sm fw-bold" onclick="addRow(2)">+ কম্পিউটার সারি</button>
                </div>
                <div>
                    <button class="btn btn-warning btn-sm fw-bold me-2 text-dark" onclick="unhideAllRows()">👁️ Show All (Unhide All)</button>
                    <button class="btn btn-info btn-sm fw-bold me-2 text-white" onclick="refreshData()">⚡ Data Refresh</button>
                    <button class="btn btn-danger btn-sm fw-bold me-2" onclick="resetAllData()">🔄 সব ডেটা রিসেট করুন</button>
                    <button class="btn btn-success btn-sm fw-bold me-2" onclick="saveDraft()">💾 ফাইল ডেটাসহ সেভ করুন (Download)</button>
                    <button class="btn btn-secondary btn-sm fw-bold me-2" onclick="window.print()">🖨️ প্রিন্ট করুন (Legal Page)</button>
                    <button class="btn btn-dark btn-sm fw-bold" onclick="logoutSystem()">🚪 লগআউট</button>
                </div>
            </div>

            <div id="tab-container"></div>

            <div class="signature-section">
                <div class="signature-box">Prepared by</div>
                <div class="signature-box">Checked by</div>
                <div class="signature-box">Approved by</div>
            </div>
        </div>
    </div>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // ফাইল ওপেন হওয়ার সাথে সাথে এমবেড করা ডেটা লোড করার স্ক্রিপ্ট
        (function restoreEmbeddedData() {
            try {
                let scriptTag = document.getElementById('embeddedDataStore');
                if (scriptTag && scriptTag.textContent.trim() !== '' && scriptTag.textContent.trim() !== '/*EMBEDDED_DATA_PLACEHOLDER*/') {
                    let decodedJson = decodeURIComponent(escape(window.atob(scriptTag.textContent.trim())));
                    let parsedData = JSON.parse(decodedJson);
                    for (let key in parsedData) {
                        localStorage.setItem(key, parsedData[key]);
                    }
                }
            } catch(e) { console.error('Data Restore Error:', e); }
        })();

        document.addEventListener('wheel', function(e) {
            if (document.activeElement.type === 'number') {
                document.activeElement.blur();
            }
        }, { passive: true });

        const loginBranchSelect = document.getElementById('loginBranchSelect');
        for (let i = 1; i <= 120; i++) {
            let bCode = String(i).padStart(3, '0');
            let opt = document.createElement('option');
            opt.value = bCode;
            opt.textContent = 'Branch ' + (i < 10 ? '0' + i : i) + ' (Code: ' + bCode + ')';
            loginBranchSelect.appendChild(opt);
        }

        function getYearMetaInfo(yearIndex) {
            let startYear = 2026 + (yearIndex - 1);
            let endYear = startYear + 1;
            let finYear = startYear + '-' + endYear;
            let duration = '01/07/' + startYear + ' To 30/06/' + endYear;
            let yStr = yearIndex < 10 ? '0' + yearIndex : yearIndex;
            let tabDisplayName = 'Year-' + yStr + ' (' + finYear + ')';
            return { finYear: finYear, duration: duration, tabDisplayName: tabDisplayName };
        }

        let currentBranch = 'Branch_001';
        let currentUserRole = 'branch';

        function initYearTabs() {
            let tabsContainer = document.getElementById('yearTabsList');
            if(!tabsContainer) return;
            let tabsHtml = '';
            for(let i=1; i<=20; i++) {
                let metaInfo = getYearMetaInfo(i);
                let activeClass = (i === 1) ? 'active' : '';
                tabsHtml += '<li class="nav-item" role="presentation"><button class="nav-link ' + activeClass + ' fw-bold px-2 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(' + i + ')">' + metaInfo.tabDisplayName + '</button></li>';
            }
            tabsHtml += '<li class="nav-item ms-2" role="presentation"><button class="nav-link bg-primary text-white fw-bold px-3 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(\'ProductSubSheet\')">📦 প্রডাক্ট এন্ট্রি সাব-সিট</button></li>';
            tabsHtml += '<li class="nav-item" role="presentation"><button class="nav-link bg-info text-dark fw-bold px-3 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(\'IdWiseDepreciationRegister\')">📊 আইডি ভিত্তিক অবচয় রেজিস্টার</button></li>';
            tabsHtml += '<li class="nav-item" role="presentation"><button class="nav-link bg-danger text-white fw-bold px-3 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(\'DisposalManager\')">🔴 সম্পদ বাতিল মডিউল</button></li>';
            tabsHtml += '<li class="nav-item" role="presentation"><button class="nav-link bg-warning text-dark fw-bold px-3 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(\'Top\')">Summary Report</button></li>';
            
            if (currentUserRole === 'head_office') {
                tabsHtml += '<li class="nav-item" role="presentation" id="consolidatedTabLi"><button class="nav-link bg-success text-white fw-bold px-3 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(\'AllBranchesConsolidated\')">🏢 ১২০ ব্রাঞ্চ সেন্ট্রাল রেজিস্টার</button></li>';
            }

            tabsHtml += '<li class="nav-item" role="presentation"><button class="nav-link bg-secondary text-white fw-bold px-3 py-1 mb-1" data-bs-toggle="tab" data-bs-target="#tab-container" type="button" onclick="setActiveTab(\'RestoreBin\')">♻️ Deleted Items Restore</button></li>';
            tabsContainer.innerHTML = tabsHtml;
        }
        initYearTabs();

        function handleLoginSubmit(e) {
            e.preventDefault();
            let selectedCode = loginBranchSelect.value;
            let usernameInput = document.getElementById('username').value.trim();
            let passwordInput = document.getElementById('password').value.trim();

            if(!selectedCode) return alert('দয়া করে শাখা বা হেড অফিস নির্বাচন করুন!');

            if(selectedCode === '00') {
                if(usernameInput === 'admin' && passwordInput === '000') {
                    currentUserRole = 'head_office';
                    currentBranch = 'Branch_001';
                } else {
                    return alert('সঠিক ইউজার আইডি ও পাসওয়ার্ড দিন।');
                }
            } else {
                let expectedUsername = 'branch' + selectedCode;
                if(usernameInput === expectedUsername && passwordInput === '123') {
                    currentUserRole = 'branch';
                    currentBranch = 'Branch_' + selectedCode;
                } else {
                    return alert('এই শাখার জন্য সঠিক ইউজার আইডি ও পাসওয়ার্ড দিন।');
                }
            }
            
            document.getElementById('mainBody').className = 'app-body';
            document.getElementById('loginViewContainer').style.display = 'none';
            document.getElementById('appViewContainer').style.display = 'block';

            let hoWrapper = document.getElementById('headOfficeSelectorWrapper');
            let branchSelectorEl = document.getElementById('branchSelector');
            if(currentUserRole === 'head_office') {
                hoWrapper.style.display = 'block';
                populateBranchSelector();
                branchSelectorEl.disabled = false;
                branchSelectorEl.value = currentBranch;
            } else {
                hoWrapper.style.display = 'none';
            }

            initYearTabs();
            initData();
            setActiveTab(1);
        }

        function logoutSystem() {
            currentUserRole = 'branch';
            document.getElementById('mainBody').className = 'login-body';
            document.getElementById('appViewContainer').style.display = 'none';
            document.getElementById('loginViewContainer').style.display = 'block';
            document.getElementById('username').value = '';
            document.getElementById('password').value = '';
            loginBranchSelect.value = '';
        }

        function populateBranchSelector() {
            let select = document.getElementById('branchSelector');
            let html = '';
            for(let b=1; b<=120; b++) {
                let bCode = String(b).padStart(3, '0');
                html += '<option value="Branch_' + bCode + '">Branch ' + b + ' (Code: ' + bCode + ') - CCDA</option>';
            }
            select.innerHTML = html;
            select.value = currentBranch;
        }

        function changeBranchCode(bVal) {
            currentBranch = bVal;
            initData();
            setActiveTab(currentYearTab);
        }

        const baseCategories = [
            { cat: 'আসবাবপত্র মোটঃ', items: [
                "হাতলসহ চেয়ার (কাঠের)", "হাতলছাড়া চেয়ার (কাঠের)", "প্লাষ্টিক চেয়ার", "হাফ সেক্রেটারি টেবিল", "টেবিল কাঠের বড়",
                "টেবিল কাঠের সাধারণ", "আলমারি", "ফাইল কেবিনেট", "খাট", "আলনা", "বেঞ্চ", "মিটসেফ", "রেক", "কম্পিউটার টেবিল",
                "টিভি বক্স", "গ্যাসের চুলা", "গ্যাস সিলেন্ডার", "প্লাষ্টিক টেবিল", "ব্লাক বোর্ড", "ডিসপ্লে বোর্ড"
            ]},
            { cat: 'ইকুইপমেন্ট মোটঃ', items: ["সিলিং ফ্যান", "টেলিভিশন", "প্রিন্টার", "মোবাইল", "ইউপিএস"]},
            { cat: 'কম্পিউটার মোটঃ', items: ["ল্যাপটপ কম্পিউটার", "ডেস্কটপ কম্পিউটার", "প্রিন্টার"]}
        ];

        let currentYearTab = 1;

        function getKeys() {
            return {
                master: 'ccda_master_' + currentBranch,
                meta: 'ccda_meta_' + currentBranch,
                products: 'ccda_products_' + currentBranch,
                disposals: 'ccda_disposals_' + currentBranch
            };
        }

        function initData() {
            let keys = getKeys();
            if (!localStorage.getItem(keys.master)) {
                let master = [];
                baseCategories.forEach((group, gIdx) => {
                    let gItems = [];
                    group.items.forEach(itemName => {
                        let rate = (gIdx === 2) ? 25 : 10;
                        gItems.push({
                            name: itemName, qty: {}, price: '', rate: {}, defaultRate: rate,
                            newPurchase: {}, overrideCurrDep: {}, overrideYear1PrevVal: '', overrideYear1PrevTotDep: '',
                            isHidden: {}, isDeleted: {}
                        });
                    });
                    master.push({ cat: group.cat, items: gItems });
                });
                localStorage.setItem(keys.master, JSON.stringify(master));
            }

            if (!localStorage.getItem(keys.meta)) {
                let meta = {};
                for (let i = 1; i <= 20; i++) {
                    let yInfo = getYearMetaInfo(i);
                    meta[i] = { branchName: currentBranch, finYear: yInfo.finYear, duration: yInfo.duration };
                }
                meta['Top'] = meta[1];
                meta['DisposalManager'] = meta[1];
                meta['RestoreBin'] = meta[1];
                meta['ProductSubSheet'] = meta[1];
                meta['IdWiseDepreciationRegister'] = meta[1];
                meta['AllBranchesConsolidated'] = meta[1];
                localStorage.setItem(keys.meta, JSON.stringify(meta));
            }

            if (!localStorage.getItem(keys.products)) {
                localStorage.setItem(keys.products, JSON.stringify([]));
            }

            if (!localStorage.getItem(keys.disposals)) {
                localStorage.setItem(keys.disposals, JSON.stringify([]));
            }
        }

        function getMasterData() { return JSON.parse(localStorage.getItem(getKeys().master)); }
        function saveMasterData(data) { localStorage.setItem(getKeys().master, JSON.stringify(data)); autoSaveNotification(); }
        function getMetaData() { return JSON.parse(localStorage.getItem(getKeys().meta)); }
        function saveMetaData(data) { localStorage.setItem(getKeys().meta, JSON.stringify(data)); autoSaveNotification(); }
        function getProductData() { return JSON.parse(localStorage.getItem(getKeys().products)); }
        function saveProductData(data) { localStorage.setItem(getKeys().products, JSON.stringify(data)); syncProductsToMaster(); autoSaveNotification(); }
        function getDisposalData() { return JSON.parse(localStorage.getItem(getKeys().disposals)); }
        function saveDisposalData(data) { localStorage.setItem(getKeys().disposals, JSON.stringify(data)); syncProductsToMaster(); autoSaveNotification(); }

        function autoSaveNotification() {
            let statusDiv = document.getElementById('syncStatus');
            if(statusDiv) {
                statusDiv.innerHTML = "💾 " + currentBranch + " এর ডেটা স্বয়ংক্রিয়ভাবে সেভ হয়েছে";
                setTimeout(() => {
                    statusDiv.innerHTML = "🟢 সিস্টেম ১০০% আপডেট ও প্রস্তুত";
                }, 2500);
            }
        }

        function formatDateToDisplay(dateStr) {
            if(!dateStr) return '';
            if(dateStr.includes('/')) {
                let p = dateStr.split('/');
                if(p[0].length === 4) return p[2] + '/' + p[1] + '/' + p[0];
                return p[0] + '/' + p[1] + '/' + p[2];
            }
            let parts = dateStr.split('-');
            if(parts.length === 3 && parts[0].length === 4) {
                return parts[2] + '/' + parts[1] + '/' + parts[0];
            }
            return dateStr;
        }

        function formatDateToInput(dateStr) {
            if(!dateStr) return '';
            if(dateStr.includes('-') && dateStr.split('-')[0].length === 4) return dateStr;
            let parts = dateStr.split('/');
            if(parts.length === 3) {
                if(parts[0].length === 4) return parts[0] + '-' + parts[1] + '-' + parts[2];
                return parts[2] + '-' + parts[1] + '-' + parts[0];
            }
            return dateStr;
        }

        function calculateMonthsFromPurchaseDate(purchaseDateStr) {
            if(!purchaseDateStr) return 12;
            let isoDateStr = formatDateToInput(purchaseDateStr);
            let pDate = new Date(isoDateStr);
            if(isNaN(pDate.getTime())) return 12;

            let pMonth = pDate.getMonth() + 1; 
            let pDay = pDate.getDate();

            let effectiveStartMonth = (pDay <= 15) ? pMonth : (pMonth + 1);
            let startMonthNormalized = effectiveStartMonth < 7 ? effectiveStartMonth + 12 : effectiveStartMonth;
            let targetEndNormalized = 6 + 12;
            
            let monthsDiff = targetEndNormalized - startMonthNormalized + 1;
            if(monthsDiff > 12) monthsDiff = 12;
            if(monthsDiff < 0) monthsDiff = 0;

            return monthsDiff > 0 ? monthsDiff : 12;
        }

        function syncProductsToMaster() {
            let products = getProductData();
            let master = getMasterData();

            master.forEach(group => {
                group.items.forEach(item => {
                    item.productSubBuyTotal = {};
                    item.productSubBuyQty = {};
                    item.productSubOldQty = {};
                    item.productSubOldWdv = {};
                    item.productSubOldAccDep = {};
                    for(let y=1; y<=20; y++) {
                        item.productSubBuyTotal[y] = 0;
                        item.productSubBuyQty[y] = 0;
                        item.productSubOldQty[y] = 0;
                        item.productSubOldWdv[y] = 0;
                        item.productSubOldAccDep[y] = 0;
                    }
                });
            });

            products.forEach(p => {
                let g = parseInt(p.gIdx), i = parseInt(p.iIdx), y = parseInt(p.yearNum), qty = parseInt(p.qty) || 1;
                if(master[g] && master[g].items[i]) {
                    if(p.entryType === 'purchase') {
                        master[g].items[i].productSubBuyTotal[y] += Math.round(parseFloat(p.price) || 0);
                        master[g].items[i].productSubBuyQty[y] += qty;
                    } else if(p.entryType === 'old_purchase') {
                        master[g].items[i].productSubOldQty[y] += qty;
                        master[g].items[i].productSubOldWdv[y] += Math.round(parseFloat(p.openingWdv) || 0);
                        master[g].items[i].productSubOldAccDep[y] += Math.round(parseFloat(p.openingAccDep) || 0);
                    }
                }
            });

            localStorage.setItem(getKeys().master, JSON.stringify(master));
        }

        let currentProductFilter = 'all';

        function setActiveTab(tab) {
            if (tab === 'AllBranchesConsolidated' && currentUserRole !== 'head_office') {
                alert('দুঃখিত! এই অপশনটি শুধুমাত্র হেড অফিস এডমিন ইউজারের জন্য সংরক্ষিত।');
                return;
            }

            currentYearTab = tab;
            const toolbar = document.querySelector('.action-toolbar');
            const heading = document.getElementById('pageTitleHeading');
            
            let meta = getMetaData();
            
            if(typeof tab === 'number' && tab >= 1 && tab <= 20) {
                let yInfo = getYearMetaInfo(tab);
                if(!meta[tab]) meta[tab] = {};
                meta[tab].finYear = yInfo.finYear;
                meta[tab].duration = yInfo.duration;
                if(!meta[tab].branchName) meta[tab].branchName = currentBranch;
                saveMetaData(meta);
            }

            if(!meta[tab]) {
                meta[tab] = meta[1] || { branchName: currentBranch, finYear: "2026-2027", duration: "01/07/2026 To 30/06/2027" };
                saveMetaData(meta);
            }

            document.getElementById('branchName').value = meta[tab].branchName;
            document.getElementById('finYear').value = meta[tab].finYear;
            document.getElementById('duration').value = meta[tab].duration;

            if (tab === 'Top') {
                toolbar.style.display = 'none';
                heading.innerText = 'Fixed Assets Register - Summary Report (' + currentBranch + ')';
                renderTopSheet();
            } else if (tab === 'AllBranchesConsolidated') {
                toolbar.style.display = 'none';
                heading.innerText = '🏢 120 Branches Central Consolidated Assets Register';
                renderAllBranchesConsolidatedPage();
            } else if (tab === 'DisposalManager') {
                toolbar.style.display = 'none';
                heading.innerText = 'Asset Disposal Manager & Real-time Auto Journals (' + currentBranch + ')';
                renderDisposalManagerPage();
            } else if (tab === 'RestoreBin') {
                toolbar.style.display = 'none';
                heading.innerText = 'Deleted Items Restore Bin (' + currentBranch + ')';
                renderRestoreBinPage();
            } else if (tab === 'ProductSubSheet') {
                toolbar.style.display = 'none';
                heading.innerText = 'Product Entry Sub-Sheet (' + currentBranch + ')';
                renderProductSubSheetPage(currentProductFilter);
            } else if (tab === 'IdWiseDepreciationRegister') {
                toolbar.style.display = 'none';
                heading.innerText = 'ID-wise Depreciation Register (' + currentBranch + ')';
                renderIdWiseDepreciationRegisterPage();
            } else if (tab === 'ItemDetailModal') {
                toolbar.style.display = 'none';
                heading.innerText = 'Item Specific Asset Details';
            } else {
                toolbar.style.display = 'flex';
                let yInfo = getYearMetaInfo(tab);
                heading.innerText = 'Fixed Assets Register - ' + currentBranch + ' (' + yInfo.tabDisplayName + ')';
                renderYearRegister(tab);
            }
        }

        function updateMetaInfo(field, val) {
            let meta = getMetaData();
            if (!meta[currentYearTab]) meta[currentYearTab] = {};
            meta[currentYearTab][field] = val;
            saveMetaData(meta);
        }

        function formatVal(val) {
            if (val === undefined || val === null || isNaN(val) || val === 0) return '';
            let rounded = Math.round(val);
            return rounded === 0 ? '' : rounded;
        }

        function calculateFullChainForBranch(branchKey) {
            let savedMaster = localStorage.getItem('ccda_master_' + branchKey);
            let savedDisposals = localStorage.getItem('ccda_disposals_' + branchKey);
            let savedProducts = localStorage.getItem('ccda_products_' + branchKey);
            if(!savedMaster) return null;

            let master = JSON.parse(savedMaster);
            let disposals = savedDisposals ? JSON.parse(savedDisposals) : [];
            let products = savedProducts ? JSON.parse(savedProducts) : [];
            let yearlyCalculations = [];

            let disposalMap = {}; 
            disposals.forEach(d => { disposalMap[d.productId] = parseInt(d.disposalYear); });

            let disposalModuleDepMap = {};
            disposals.forEach(d => {
                let prod = products.find(p => p.productId === d.productId);
                if(prod) {
                    let key = prod.gIdx + '_' + prod.iIdx + '_' + d.disposalYear;
                    disposalModuleDepMap[key] = (disposalModuleDepMap[key] || 0) + (parseFloat(d.accDep) || 0);
                }
            });

            for (let y = 1; y <= 20; y++) {
                let yearGroups = JSON.parse(JSON.stringify(master));
                
                yearGroups.forEach((group, gIdx) => {
                    group.items.forEach((item, iIdx) => {
                        let rate = (item.rate && item.rate[y] !== undefined && item.rate[y] !== '') ? parseFloat(item.rate[y]) : item.defaultRate;
                        
                        let activeQtyCount = 0;
                        products.forEach(p => {
                            if(p.gIdx == gIdx && p.iIdx == iIdx) {
                                let pStartYear = parseInt(p.yearNum) || 1;
                                let dYear = disposalMap[p.productId];
                                if(pStartYear <= y && (!dYear || dYear > y)) {
                                    activeQtyCount += (parseInt(p.qty) || 1);
                                }
                            }
                        });

                        let manualQty = (item.qty && item.qty[y] !== undefined && item.qty[y] !== '') ? parseInt(item.qty[y]) : null;
                        let currentQty = '';
                        if (products.filter(p => p.gIdx == gIdx && p.iIdx == iIdx).length > 0) {
                            currentQty = activeQtyCount > 0 ? activeQtyCount : '';
                        } else {
                            if (y === 1) {
                                currentQty = (manualQty !== null ? manualQty : 0);
                            } else {
                                let prevQty = yearlyCalculations[y - 2][gIdx].items[iIdx].qty;
                                currentQty = (prevQty !== '' ? parseInt(prevQty) : 0);
                            }
                        }

                        let itemDisposedThisYearVal = 0;
                        products.forEach(p => {
                            if(p.gIdx == gIdx && p.iIdx == iIdx) {
                                let dYear = disposalMap[p.productId];
                                if(dYear === y) {
                                    let pCost = (p.entryType === 'old_purchase') ? (parseFloat(p.openingWdv||0) + parseFloat(p.openingAccDep||0)) : (parseFloat(p.price||0));
                                    itemDisposedThisYearVal += Math.round(pCost);
                                }
                            }
                        });

                        let manualNewBuy = (item.newPurchase && item.newPurchase[y] !== undefined && item.newPurchase[y] !== '') ? Math.round(parseFloat(item.newPurchase[y])) : 0;
                        let productSubBuy = 0;
                        products.forEach(p => {
                            if(p.gIdx == gIdx && p.iIdx == iIdx && parseInt(p.yearNum) === y && p.entryType === 'purchase') {
                                productSubBuy += Math.round(parseFloat(p.price) || 0);
                            }
                        });
                        let newBuy = manualNewBuy + productSubBuy; 

                        let customDep = (item.overrideCurrDep && item.overrideCurrDep[y] !== undefined && item.overrideCurrDep[y] !== '') ? Math.round(parseFloat(item.overrideCurrDep[y])) : null;
                        
                        let openingCost = 0, openingTotDep = 0, openingWdv = 0;
                        let subOldWdv = 0, subOldAccDep = 0;
                        products.forEach(p => {
                            if(p.gIdx == gIdx && p.iIdx == iIdx && parseInt(p.yearNum) === y && p.entryType === 'old_purchase') {
                                subOldWdv += Math.round(parseFloat(p.openingWdv) || 0);
                                subOldAccDep += Math.round(parseFloat(p.openingAccDep) || 0);
                            }
                        });

                        if (y === 1) {
                            let manualPrevWdv = (item.overrideYear1PrevVal !== undefined && item.overrideYear1PrevVal !== '' && !isNaN(item.overrideYear1PrevVal)) ? Math.round(parseFloat(item.overrideYear1PrevVal)) : 0;
                            let manualPrevTotDep = (item.overrideYear1PrevTotDep !== undefined && item.overrideYear1PrevTotDep !== '' && !isNaN(item.overrideYear1PrevTotDep)) ? Math.round(parseFloat(item.overrideYear1PrevTotDep)) : 0;
                            openingWdv = manualPrevWdv + subOldWdv;
                            openingTotDep = manualPrevTotDep + subOldAccDep;
                            openingCost = openingWdv + openingTotDep;
                        } else {
                            let prevYearItem = yearlyCalculations[y - 2][gIdx]?.items[iIdx];
                            openingCost = prevYearItem ? (prevYearItem.price !== '' ? Math.round(parseFloat(prevYearItem.price)) : 0) : 0;
                            openingTotDep = prevYearItem ? (prevYearItem.cumDep !== '' ? Math.round(parseFloat(prevYearItem.cumDep)) : 0) : 0;
                            openingWdv = prevYearItem ? (prevYearItem.currVal !== '' ? Math.round(parseFloat(prevYearItem.currVal)) : 0) : 0;
                        }

                        let totalAvailable = openingCost + newBuy;
                        let disposedVal = itemDisposedThisYearVal;
                        if (disposedVal > totalAvailable) disposedVal = totalAvailable;

                        let closingCost = Math.max(0, totalAvailable - disposedVal);
                        let adjustedOpeningWdv = Math.max(0, openingWdv); 
                        
                        let calculatedCurrDep = 0;
                        if(customDep !== null) {
                            calculatedCurrDep = customDep;
                        } else {
                            if(adjustedOpeningWdv > 0) {
                                calculatedCurrDep = Math.round((adjustedOpeningWdv * rate) / 100);
                            }
                            products.forEach(p => {
                                if(p.gIdx == gIdx && p.iIdx == iIdx && parseInt(p.yearNum) === y && p.entryType === 'purchase') {
                                    let pCost = parseFloat(p.price) || 0;
                                    let months = calculateMonthsFromPurchaseDate(p.purchaseDate);
                                    calculatedCurrDep += Math.round((pCost / 12) * months * (rate / 100));
                                }
                            });
                        }

                        let currDep = calculatedCurrDep;
                        let depReductionOnDisposal = 0;
                        if (openingCost > 0 && disposedVal > 0) {
                            depReductionOnDisposal = (disposedVal >= totalAvailable) ? openingTotDep : openingTotDep * (disposedVal / openingCost);
                        }

                        let remainingOpeningTotDep = Math.max(0, openingTotDep - Math.round(depReductionOnDisposal));
                        let isDisposedYear = products.some(p => p.gIdx == gIdx && p.iIdx == iIdx && disposalMap[p.productId] === y);
                        let disposalModuleAccDep = disposalModuleDepMap[gIdx + '_' + iIdx + '_' + y] || 0;

                        let cumDep = 0;
                        if (isDisposedYear) {
                            cumDep = Math.max(0, openingTotDep + currDep - disposalModuleAccDep);
                        } else {
                            cumDep = remainingOpeningTotDep + currDep;
                        }

                        if (disposedVal >= totalAvailable && totalAvailable > 0 && y > (disposalMap[item.productId] || 999)) cumDep = 0;

                        let closingWdv = Math.max(0, closingCost - cumDep);
                        let finalCurrVal = (disposedVal >= totalAvailable && totalAvailable > 0 && y >= (disposalMap[item.productId] || 999)) ? 0 : closingWdv;

                        item.qty = currentQty > 0 ? currentQty : '';
                        item.price = closingCost > 0 ? closingCost : '';
                        item.activeRate = rate;
                        item.prevVal = openingWdv > 0 ? openingWdv : '';
                        item.prevTotDep = openingTotDep > 0 ? openingTotDep : '';
                        item.currDep = customDep !== null ? customDep : (currDep > 0 ? currDep : '');
                        item.cumDep = cumDep > 0 ? cumDep : '';
                        item.currVal = finalCurrVal > 0 ? finalCurrVal : '';
                        item.effectiveNewBuy = newBuy;
                        item.effectiveDisposed = disposedVal;
                    });
                });
                yearlyCalculations.push(yearGroups);
            }
            return yearlyCalculations;
        }

        function calculateFullChain() {
            syncProductsToMaster();
            return calculateFullChainForBranch(currentBranch);
        }

        function openItemDetailModal(gIdx, iIdx, yearNum) {
            currentYearTab = 'ItemDetailModal';
            let container = document.getElementById('tab-container');
            let master = getMasterData();
            let item = master[gIdx].items[iIdx];
            let catName = master[gIdx].cat.replace('মোটঃ', '');
            let products = getProductData();
            let disposals = getDisposalData();
            
            let disposalMap = {};
            disposals.forEach(d => { disposalMap[d.productId] = parseInt(d.disposalYear); });

            let matchingProducts = products.filter(p => p.gIdx == gIdx && p.iIdx == iIdx);
            let yInfo = getYearMetaInfo(yearNum);
            let prodRows = '';
            let activeCount = 0;

            matchingProducts.forEach(p => {
                let dYear = disposalMap[p.productId];
                let pStartYear = parseInt(p.yearNum) || 1;

                if (pStartYear > yearNum) return;
                if (dYear && dYear < yearNum) return;

                activeCount++;
                let pType = p.entryType === 'old_purchase' ? '🟡 পুরাতন ক্রয়' : '🟢 নতুন ক্রয়';
                let pVal = (p.entryType === 'old_purchase') ? Math.round(parseFloat(p.openingWdv||0) + parseFloat(p.openingAccDep||0)) : Math.round(parseFloat(p.price||0));
                let statusBadge = (dYear && dYear === yearNum) ? '<span class="badge bg-danger">Disposed This Year</span>' : '<span class="badge bg-success">Active</span>';
                
                prodRows += '<tr>' +
                    '<td>' + activeCount + '</td>' +
                    '<td class="fw-bold text-primary">' + (p.productId || 'N/A') + '</td>' +
                    '<td>' + yInfo.tabDisplayName + '</td>' +
                    '<td>' + (formatDateToDisplay(p.purchaseDate) || '-') + '</td>' +
                    '<td>' + pType + '</td>' +
                    '<td>' + (p.qty || 1) + '</td>' +
                    '<td>৳' + pVal.toLocaleString() + '</td>' +
                    '<td>' + (p.remarks || '-') + '</td>' +
                    '<td>' + statusBadge + '</td>' +
                '</tr>';
            });

            if(activeCount === 0) {
                prodRows = '<tr><td colspan="9" class="text-center text-muted py-3">এই আইটেমের স্টকে বর্তমানে কোনো সক্রিয় প্রডাক্ট মজুদ নেই।</td></tr>';
            }

            container.innerHTML = '<div class="card border-primary shadow-sm mb-4">' +
                '<div class="card-header bg-primary text-white fw-bold d-flex justify-content-between align-items-center">' +
                    '<span>🔍 সক্রিয় মজুদ প্রডাক্ট তালিকা: [' + catName + '] ' + item.name + ' (' + yInfo.tabDisplayName + ') - [' + currentBranch + ']</span>' +
                    '<button class="btn btn-light btn-sm fw-bold text-primary" onclick="setActiveTab(' + yearNum + ')">⬅️ রেজিস্টারে ফিরে যান</button>' +
                '</div>' +
                '<div class="card-body bg-light">' +
                    '<div class="table-responsive">' +
                        '<table class="table table-bordered align-middle text-center bg-white">' +
                            '<thead class="table-secondary"><tr><th>ক্রমিক</th><th>প্রডাক্ট আইডি / কোড</th><th>অর্থ বছর</th><th>ক্রয়ের তারিখ</th><th>এন্ট্রির ধরন</th><th>QTY</th><th>মূল্য (TK.)</th><th>মন্তব্য</th><th>স্ট্যাটাস</th></tr></thead>' +
                            '<tbody>' + prodRows + '</tbody>' +
                        '</table>' +
                    '</div>' +
                    '<div class="text-end mt-3">' +
                        '<button class="btn btn-secondary fw-bold" onclick="setActiveTab(' + yearNum + ')">ফিরে যান</button>' +
                    '</div>' +
                '</div>' +
            '</div>';
        }

        function renderAllBranchesConsolidatedPage() {
            if (currentUserRole !== 'head_office') {
                alert('অনুমোদিত নয়!');
                setActiveTab(1);
                return;
            }

            let container = document.getElementById('tab-container');
            let branchTotals = [];

            for(let b=1; b<=120; b++) {
                let bCode = String(b).padStart(3, '0');
                let bKey = 'Branch_' + bCode;
                let chain = calculateFullChainForBranch(bKey);

                let bAssetVal = 0, bDepVal = 0, bWdvVal = 0;
                if(chain) {
                    let targetYearIdx = (typeof currentConsolidatedYear !== 'undefined' ? currentConsolidatedYear : 1) - 1;
                    let yearData = chain[targetYearIdx] || chain[0];
                    yearData.forEach(group => {
                        group.items.forEach(item => {
                            bAssetVal += (item.price !== '' ? Math.round(parseFloat(item.price)) : 0);
                            bDepVal += (item.cumDep !== '' ? Math.round(parseFloat(item.cumDep)) : 0);
                            bWdvVal += (item.currVal !== '' ? Math.round(parseFloat(item.currVal)) : 0);
                        });
                    });
                }
                branchTotals.push({ bCode: bCode, bKey: bKey, bAssetVal: bAssetVal, bDepVal: bDepVal, bWdvVal: bWdvVal });
            }

            let totalSum = 0, totalDepSum = 0, totalWdvSum = 0;
            let rowsHtml = branchTotals.map((bt, idx) => {
                totalSum += bt.bAssetVal;
                totalDepSum += bt.bDepVal;
                totalWdvSum += bt.bWdvVal;
                return '<tr>' +
                    '<td>' + (idx + 1) + '</td>' +
                    '<td class="fw-bold text-primary">Branch Code: ' + bt.bCode + '</td>' +
                    '<td>৳' + bt.bAssetVal.toLocaleString() + '</td>' +
                    '<td>৳' + bt.bDepVal.toLocaleString() + '</td>' +
                    '<td>৳' + bt.bWdvVal.toLocaleString() + '</td>' +
                    '<td><button class="btn btn-outline-primary btn-sm fw-bold" onclick="switchBranchFromHO(\'' + bt.bKey + '\')">এই শাখার রেজিস্টারে যান</button></td>' +
                '</tr>';
            }).join('');

            let yearOptionsHtml = '';
            for(let i=1; i<=20; i++) {
                let sel = (typeof currentConsolidatedYear !== 'undefined' && currentConsolidatedYear === i) ? 'selected' : '';
                yearOptionsHtml += '<option value="' + i + '" ' + sel + '>' + getYearMetaInfo(i).tabDisplayName + '</option>';
            }

            let yearSelectorHtml = '<div class="row mb-3 align-items-center">' +
                '<div class="col-md-4">' +
                    '<label class="form-label fw-bold">সমন্বিত রিপোর্টের বছর সিলেক্ট করুন:</label>' +
                    '<select id="consolidatedYearSelect" class="form-select fw-bold border-success" onchange="changeConsolidatedYear(this.value)">' + yearOptionsHtml + '</select>' +
                '</div>' +
            '</div>';

            container.innerHTML = '<div class="card border-success shadow-sm mb-4">' +
                '<div class="card-header bg-success text-white fw-bold">' +
                    '<span>🏢 ১২০টি শাখার সমন্বিত স্থায়ী সম্পদ ব্যবস্থাপনা রেজিস্টার (Head Office Central View)</span>' +
                '</div>' +
                '<div class="card-body bg-light">' +
                    yearSelectorHtml +
                    '<div class="row mb-4">' +
                        '<div class="col-md-4"><div class="p-3 bg-white border rounded shadow-sm text-center"><h6 class="text-muted mb-1">মোট সম্পদ মূল্য:</h6><h4 class="fw-bold text-success">৳' + totalSum.toLocaleString() + '</h4></div></div>' +
                        '<div class="col-md-4"><div class="p-3 bg-white border rounded shadow-sm text-center"><h6 class="text-muted mb-1">মোট ক্রমপুঞ্জীভূত অবচয়:</h6><h4 class="fw-bold text-danger">৳' + totalDepSum.toLocaleString() + '</h4></div></div>' +
                        '<div class="col-md-4"><div class="p-3 bg-white border rounded shadow-sm text-center"><h6 class="text-muted mb-1">মোট বর্তমান WDV মূল্য:</h6><h4 class="fw-bold text-primary">৳' + totalWdvSum.toLocaleString() + '</h4></div></div>' +
                    '</div>' +
                    '<div class="table-responsive">' +
                        '<table class="table table-bordered align-middle text-center bg-white">' +
                            '<thead class="table-secondary"><tr><th>ক্রমিক</th><th>ব্রাঞ্চ কোড</th><th>মোট সম্পদের মূল্য (TK.)</th><th>মোট অবচয় (TK.)</th><th>বর্তমান WDV (TK.)</th><th>অ্যাকশন</th></tr></thead>' +
                            '<tbody>' + rowsHtml + '</tbody>' +
                            '<tfoot><tr class="table-success fw-bold"><td colspan="2" class="text-end">সর্বমোট (১২০ শাখা মিলে):</td><td>৳' + totalSum.toLocaleString() + '</td><td>৳' + totalDepSum.toLocaleString() + '</td><td>৳' + totalWdvSum.toLocaleString() + '</td><td>-</td></tr></tfoot>' +
                        '</table>' +
                    '</div>' +
                '</div>' +
            '</div>';
        }

        let currentConsolidatedYear = 1;
        function changeConsolidatedYear(yr) {
            currentConsolidatedYear = parseInt(yr);
            renderAllBranchesConsolidatedPage();
        }

        function switchBranchFromHO(bKey) {
            currentBranch = bKey;
            let sel = document.getElementById('branchSelector');
            if(sel) sel.value = bKey;
            initData();
            setActiveTab(1);
        }

        function calculateSingleProductSchedule(p, disposals, branchKey) {
            let pStartYear = parseInt(p.yearNum) || 1;
            let initialCost = (p.entryType === 'old_purchase') ? (parseFloat(p.openingWdv||0) + parseFloat(p.openingAccDep||0)) : parseFloat(p.price||0);
            let initialOpeningAccDep = (p.entryType === 'old_purchase') ? parseFloat(p.openingAccDep||0) : 0;
            
            let master = JSON.parse(localStorage.getItem('ccda_master_' + branchKey) || '[]');
            let gIdx = parseInt(p.gIdx), iIdx = parseInt(p.iIdx);
            let defaultRate = (gIdx === 2) ? 25 : 10;
            
            let dispObj = disposals.find(d => d.productId === p.productId);
            let disposalYear = dispObj ? parseInt(dispObj.disposalYear) : null;

            let schedule = [];
            let currentWdv = (p.entryType === 'old_purchase') ? parseFloat(p.openingWdv||0) : initialCost;
            let runningAccDep = initialOpeningAccDep;

            for(let y=1; y<=20; y++) {
                if(y < pStartYear || (disposalYear && y > disposalYear)) {
                    schedule.push({ year: y, openingWdv: 0, dep: 0, cumDep: 0, closingWdv: 0, status: 'Inactive' });
                    continue;
                }

                let rate = defaultRate;
                if(master[gIdx] && master[gIdx].items[iIdx] && master[gIdx].items[iIdx].rate && master[gIdx].items[iIdx].rate[y] !== undefined && master[gIdx].items[iIdx].rate[y] !== '') {
                    rate = parseFloat(master[gIdx].items[iIdx].rate[y]);
                }

                let openingWdv = currentWdv;
                let currentDep = 0;
                
                if(y === pStartYear && p.entryType === 'purchase') {
                    let months = calculateMonthsFromPurchaseDate(p.purchaseDate);
                    let basePrice = parseFloat(p.price) || 0;
                    currentDep = Math.round((basePrice / 12) * months * (rate / 100));
                } else {
                    currentDep = (openingWdv > 0) ? Math.round((openingWdv * rate) / 100) : 0;
                }

                if(disposalYear && y === disposalYear) {
                    let moduleAccDep = dispObj ? (parseFloat(dispObj.accDep) || 0) : 0;
                    runningAccDep = Math.max(0, runningAccDep + currentDep - moduleAccDep);
                } else {
                    runningAccDep += currentDep;
                }

                let closingWdv = Math.max(0, openingWdv - currentDep);
                currentWdv = closingWdv;

                let status = (disposalYear && y === disposalYear) ? 'Disposed' : 'Active';
                schedule.push({ year: y, openingWdv: Math.round(openingWdv), dep: Math.round(currentDep), cumDep: Math.round(runningAccDep), closingWdv: Math.round(closingWdv), status: status });

                if(disposalYear && y >= disposalYear) break;
            }
            return { initialCost: initialCost, runningAccDep: runningAccDep, schedule: schedule, disposalYear: disposalYear };
        }

        function getProductExactDisposalDetails(productId, disposalYear) {
            let products = getProductData();
            let disposals = getDisposalData();
            let p = products.find(item => item.productId === productId);
            if(!p) return { cost: 0, accDep: 0, loss: 0 };

            let scheduleData = calculateSingleProductSchedule(p, disposals, currentBranch);
            let targetYearEntry = scheduleData.schedule.find(s => s.year === disposalYear);

            let totalCost = Math.round(scheduleData.initialCost);
            let totalAccDep = targetYearEntry ? targetYearEntry.cumDep : scheduleData.runningAccDep;
            let calculatedLoss = Math.max(0, totalCost - totalAccDep);

            return { cost: totalCost, accDep: Math.round(totalAccDep), loss: Math.round(calculatedLoss) };
        }

        function renderDisposalManagerPage() {
            let container = document.getElementById('tab-container');
            let products = getProductData();
            let disposals = getDisposalData();
            let master = getMasterData();

            let disposalTableRows = '';
            let realTimeJournalsHtml = '';

            disposals.forEach((d, idx) => {
                let p = products.find(item => item.productId === d.productId);
                let assetName = 'অজানা সম্পদ';
                let catName = '';
                let purchaseYearName = '-';
                let purchaseDateStr = '-';

                if(p) {
                    let mGroup = master[p.gIdx];
                    if(mGroup && mGroup.items[p.iIdx]) {
                        assetName = mGroup.items[p.iIdx].name;
                        catName = mGroup.cat.replace('মোটঃ', '');
                    }
                    purchaseYearName = getYearMetaInfo(p.yearNum).tabDisplayName;
                    purchaseDateStr = formatDateToDisplay(p.purchaseDate) || '-';
                }

                let dYInfo = getYearMetaInfo(d.disposalYear);

                disposalTableRows += '<tr>' +
                    '<td>' + (idx + 1) + '</td>' +
                    '<td class="fw-bold text-primary">' + d.productId + '</td>' +
                    '<td>' + catName + '</td>' +
                    '<td>' + assetName + '</td>' +
                    '<td>' + purchaseYearName + '</td>' +
                    '<td>' + purchaseDateStr + '</td>' +
                    '<td>' + dYInfo.tabDisplayName + '</td>' +
                    '<td>' + (formatDateToDisplay(d.disposalDate) || '-') + '</td>' +
                    '<td>৳' + d.disposalValue.toLocaleString() + '</td>' +
                    '<td>৳' + d.accDep.toLocaleString() + '</td>' +
                    '<td>৳' + d.lossVal.toLocaleString() + '</td>' +
                    '<td><span class="badge bg-danger">Disposed</span></td>' +
                    '<td><button class="btn btn-outline-danger btn-sm fw-bold py-0 px-2" onclick="removeDisposal(\'' + d.productId + '\')">Restore</button></td>' +
                '</tr>';

                realTimeJournalsHtml += '<div class="card mb-3 border-dark shadow-sm">' +
                    '<div class="card-header bg-dark text-white fw-bold d-flex justify-content-between align-items-center">' +
                        '<span>📝 প্রডাক্ট কোড: ' + d.productId + ' (' + assetName + ') - রিয়েল-টাইম অটো জেনারেটেড জার্নাল</span>' +
                        '<span class="badge bg-warning text-dark">তারিখ: ' + (formatDateToDisplay(d.disposalDate) || 'N/A') + '</span>' +
                    '</div>' +
                    '<div class="card-body bg-white">' +
                        '<table class="table table-bordered table-sm mb-0 text-center align-middle">' +
                            '<thead class="table-secondary"><tr><th>তারিখ</th><th>একাউন্টস শিরোনাম / বিবরণ</th><th>L.F.</th><th>ডেবিট (৳)</th><th>ক্রেডিট (৳)</th></tr></thead>' +
                            '<tbody>' +
                                '<tr>' +
                                    '<td>' + (formatDateToDisplay(d.disposalDate) || '-') + '</td>' +
                                    '<td class="text-start fw-bold">Accumulated Depreciation A/c ... Dr.<br>Loss on Asset Disposal A/c ... Dr.<br>&nbsp;&nbsp;&nbsp;&nbsp;To Fixed Assets (' + assetName + ' - Code: ' + d.productId + ') A/c</td>' +
                                    '<td>-</td>' +
                                    '<td class="fw-bold">৳' + d.accDep.toLocaleString() + '<br>৳' + d.lossVal.toLocaleString() + '<br>-</td>' +
                                    '<td class="fw-bold">-<br>-<br>৳' + d.disposalValue.toLocaleString() + '</td>' +
                                '</tr>' +
                            '</tbody>' +
                        '</table>' +
                    '</div>' +
                '</div>';
            });

            let disposedProductIds = disposals.map(d => d.productId);
            let activeProductsForDisposal = products.filter(p => !disposedProductIds.includes(p.productId));

            let productOptions = '<option value="">-- প্রডাক্ট কোড সিলেক্ট করুন --</option>';
            activeProductsForDisposal.forEach(p => {
                let mGroup = master[p.gIdx];
                let name = mGroup && mGroup.items[p.iIdx] ? mGroup.items[p.iIdx].name : '';
                let pCost = (p.entryType === 'old_purchase') ? (parseFloat(p.openingWdv||0) + parseFloat(p.openingAccDep||0)) : (parseFloat(p.price||0));
                productOptions += '<option value="' + p.productId + '">[' + p.productId + '] - ' + name + ' (মূল্য: ৳' + Math.round(pCost) + ')</option>';
            });

            let yearOptHtml = '';
            for(let i=1; i<=20; i++) {
                yearOptHtml += '<option value="' + i + '">' + getYearMetaInfo(i).tabDisplayName + '</option>';
            }

            container.innerHTML = '<div class="row">' +
                '<div class="col-md-6">' +
                    '<div class="card border-danger shadow-sm mb-4">' +
                        '<div class="card-header bg-danger text-white fw-bold"><span>🔴 সম্পদ বাতিল করুন (Disposal Entry Form) - [' + currentBranch + ']</span></div>' +
                        '<div class="card-body bg-light">' +
                            '<form onsubmit="handleDisposalSubmit(event)">' +
                                '<div class="mb-3"><label class="form-label fw-bold">প্রডাক্ট আইডি সিলেক্ট করুন:</label><select id="disposalProductId" class="form-select editable-cell fw-bold" required onchange="onDisposalProductSelect(this)">' + productOptions + '</select></div>' +
                                '<div class="mb-3"><label class="form-label fw-bold">বাতিলের বছর সিলেক্ট করুন:</label><select id="disposalYearNum" class="form-select editable-cell fw-bold" required onchange="onDisposalYearChange()">' + yearOptHtml + '</select></div>' +
                                '<div class="mb-3"><label class="form-label fw-bold">বাতিলের তারিখ (DD/MM/YYYY):</label><input type="date" id="disposalDateInput" class="form-control fw-bold editable-cell" required></div>' +
                                '<div class="mb-3"><label class="form-label fw-bold">বাতিলকৃত মোট মূল্য (৳):</label><input type="number" id="disposalValueInput" class="form-control fw-bold bg-white" readonly required></div>' +
                                '<div class="text-end"><button type="submit" class="btn btn-danger fw-bold w-100">সম্পদ বাতিল নিশ্চিত করুন</button></div>' +
                            '</form>' +
                        '</div>' +
                    '</div>' +
                '</div>' +
                '<div class="col-md-12">' +
                    '<div class="card border-success shadow-sm mb-4">' +
                        '<div class="card-header bg-success text-white fw-bold"><span>📦 বাতিলকৃত প্রডাক্ট ও সম্পদের বিস্তারিত বিবরণ তালিকা</span></div>' +
                        '<div class="card-body bg-white">' +
                            '<div class="table-responsive">' +
                                '<table class="table table-bordered align-middle text-center" style="font-size: 12pt;">' +
                                    '<thead class="table-secondary"><tr><th>ক্রমিক</th><th>প্রডাক্ট আইডি / কোড</th><th>ক্যাটাগরি</th><th>সম্পদের নাম</th><th>ক্রয়ের বছর</th><th>তারিখ</th><th>বাতিলের বছর</th><th>তারিখ</th><th>মূল্য</th><th>অবচয়</th><th>ক্ষতি</th><th>স্ট্যাটাস</th><th>অ্যাকশন</th></tr></thead>' +
                                    '<tbody>' + (disposalTableRows.length > 0 ? disposalTableRows : '<tr><td colspan="13" class="text-center text-muted py-3">কোনো সম্পদ বাতিল করা হয়নি।</td></tr>') + '</tbody>' +
                                '</table>' +
                            '</div>' +
                        '</div>' +
                    '</div>' +
                    '<div class="card border-dark shadow-sm mb-4">' +
                        '<div class="card-header bg-dark text-white fw-bold"><span>📊 রিয়েল-টাইম অটো জেনারেটেড জার্নালসমূহ</span></div>' +
                        '<div class="card-body bg-light">' + (realTimeJournalsHtml.length > 0 ? realTimeJournalsHtml : '<div class="text-center text-muted py-3">কোনো বাতিল এন্ট্রি নেই।</div>') + '</div>' +
                    '</div>' +
                '</div>' +
            '</div>';
        }

        function onDisposalProductSelect(selectElem) {
            let prodId = selectElem.value;
            if(prodId) {
                let products = getProductData();
                let p = products.find(item => item.productId === prodId);
                if(p) {
                    document.getElementById('disposalYearNum').value = p.yearNum;
                    updateDisposalValuesPreview();
                }
            } else {
                document.getElementById('disposalValueInput').value = '';
            }
        }

        function onDisposalYearChange() {
            updateDisposalValuesPreview();
        }

        function updateDisposalValuesPreview() {
            let productId = document.getElementById('disposalProductId').value;
            let yearNum = parseInt(document.getElementById('disposalYearNum').value);
            if(productId && yearNum) {
                let exactDetails = getProductExactDisposalDetails(productId, yearNum);
                document.getElementById('disposalValueInput').value = exactDetails.cost;
            }
        }

        function handleDisposalSubmit(event) {
            event.preventDefault();
            let productId = document.getElementById('disposalProductId').value;
            let yearNum = parseInt(document.getElementById('disposalYearNum').value);
            let disposalDate = document.getElementById('disposalDateInput').value;
            
            let products = getProductData();
            let p = products.find(item => item.productId === productId);
            if(!p) return alert('সঠিক প্রডাক্ট কোড পাওয়া যায়নি!');

            let exactDetails = getProductExactDisposalDetails(productId, yearNum);
            let disposals = getDisposalData().filter(d => d.productId !== productId);

            disposals.push({
                productId: productId, disposalYear: yearNum, disposalDate: disposalDate,
                disposalValue: exactDetails.cost, accDep: exactDetails.accDep, lossVal: exactDetails.loss
            });

            saveDisposalData(disposals);
            alert('সফলভাবে সম্পদ বাতিল করা হয়েছে!');
            renderDisposalManagerPage();
        }

        function removeDisposal(productId) {
            if(confirm('বাতিল এন্ট্রি রিস্টোর করতে চান?')) {
                let disposals = getDisposalData().filter(d => d.productId !== productId);
                saveDisposalData(disposals);
                renderDisposalManagerPage();
            }
        }

        function getMasterOptionsHtml(selG, selI) {
            let master = getMasterData();
            let html = '';
            master.forEach((group, gIdx) => {
                group.items.forEach((item, iIdx) => {
                    let isSel = (gIdx == selG && iIdx == selI) ? 'selected' : '';
                    html += '<option value="' + gIdx + '_' + iIdx + '" ' + isSel + '>' + item.name + '</option>';
                });
            });
            return html;
        }

        function renderProductSubSheetPage(filterType = 'all') {
            currentProductFilter = filterType;
            let container = document.getElementById('tab-container');
            let products = getProductData();
            let master = getMasterData();

            let rowsHtml = '';
            products.forEach((p, idx) => {
                let mGroup = master[p.gIdx];
                let catName = mGroup ? mGroup.cat.replace('মোটঃ','') : '';
                let typeVal = p.entryType || 'purchase';
                let isOld = (typeVal === 'old_purchase');
                let displayPrice = Math.round(isOld ? (parseFloat(p.openingWdv||0) + parseFloat(p.openingAccDep||0)) : (parseFloat(p.price||0)));
                
                let selectedProductYear = parseInt(p.yearNum) || 1;
                if (selectedProductYear > 1 && isOld) return;

                let yearOptStr = '';
                for(let i=1; i<=20; i++) {
                    let sel = (p.yearNum === i) ? 'selected' : '';
                    yearOptStr += '<option value="' + i + '" ' + sel + '>' + getYearMetaInfo(i).tabDisplayName + '</option>';
                }

                rowsHtml += '<tr>' +
                    '<td>' + (idx + 1) + '</td>' +
                    '<td><input type="text" value="' + (p.productId || '') + '" oninput="updateProductField(' + idx + ', \'productId\', this.value)" class="editable-cell fw-bold text-center"></td>' +
                    '<td>' + catName + '</td>' +
                    '<td><select onchange="updateProductItemRef(' + idx + ', this.value)" class="form-select form-select-sm editable-cell fw-bold">' + getMasterOptionsHtml(p.gIdx, p.iIdx) + '</select></td>' +
                    '<td><select onchange="updateProductType(' + idx + ', this.value)" class="form-select form-select-sm editable-cell fw-bold ' + (isOld ? 'text-warning' : 'text-success') + '"><option value="purchase" ' + (typeVal === 'purchase' ? 'selected' : '') + '>🟢 নতুন ক্রয়</option>' + (selectedProductYear === 1 ? '<option value="old_purchase" ' + (typeVal === 'old_purchase' ? 'selected' : '') + '>🟡 পুরাতন ক্রয়</option>' : '') + '</select></td>' +
                    '<td><select onchange="updateProductYear(' + idx + ', this.value)" class="form-select form-select-sm editable-cell fw-bold">' + yearOptStr + '</select></td>' +
                    '<td><input type="date" value="' + (p.purchaseDate || '') + '" oninput="updateProductField(' + idx + ', \'purchaseDate\', this.value)" class="editable-cell fw-bold text-center"></td>' +
                    '<td><input type="number" value="' + (p.qty !== undefined ? p.qty : 1) + '" oninput="updateProductField(' + idx + ', \'qty\', this.value)" class="editable-cell fw-bold text-center" style="width: 70px;"></td>' +
                    '<td><input type="number" value="' + displayPrice + '" oninput="updateProductField(' + idx + ', \'price\', this.value)" class="editable-cell fw-bold text-success" ' + (isOld ? 'readonly' : '') + '></td>' +
                    '<td><input type="number" value="' + Math.round(p.openingWdv || 0) + '" oninput="updateProductField(' + idx + ', \'openingWdv\', this.value)" class="editable-cell fw-bold text-dark ' + (isOld ? '' : 'bg-light text-muted') + '" ' + (isOld ? '' : 'disabled') + '></td>' +
                    '<td><input type="number" value="' + Math.round(p.openingAccDep || 0) + '" oninput="updateProductField(' + idx + ', \'openingAccDep\', this.value)" class="editable-cell fw-bold text-dark ' + (isOld ? '' : 'bg-light text-muted') + '" ' + (isOld ? '' : 'disabled') + '></td>' +
                    '<td><input type="text" value="' + (p.remarks || '') + '" oninput="updateProductField(' + idx + ', \'remarks\', this.value)" class="editable-cell"></td>' +
                    '<td><button class="btn btn-outline-danger btn-sm py-0 px-2 fw-bold" onclick="deleteProductRow(' + idx + ')">❌</button></td>' +
                '</tr>';
            });

            container.innerHTML = '<div class="card border-primary shadow-sm mb-4">' +
                '<div class="card-header bg-primary text-white fw-bold d-flex justify-content-between align-items-center">' +
                    '<span>📦 প্রডাক্ট এন্ট্রি সাব-সিট (Product Entry Sub-Sheet) - [' + currentBranch + ']</span>' +
                    '<button class="btn btn-light btn-sm fw-bold text-primary" onclick="addProductRow()">+ নতুন প্রডাক্ট এন্ট্রি যোগ করুন</button>' +
                '</div>' +
                '<div class="card-body bg-light">' +
                    '<div class="table-responsive">' +
                        '<table class="table table-bordered align-middle text-center bg-white">' +
                            '<thead><tr class="table-secondary"><th>ক্রমিক</th><th>প্রডাক্ট আইডি / কোড</th><th>ক্যাটাগরি</th><th>সম্পদের নাম</th><th>এন্ট্রির ধরণ</th><th>অর্থ বছর</th><th>তারিখ</th><th>QTY</th><th>মূল্য</th><th>প্রারম্ভিক WDV</th><th>প্রারম্ভিক অবচয়</th><th>মন্তব্য</th><th>অ্যাকশন</th></tr></thead>' +
                            '<tbody>' + (rowsHtml.length > 0 ? rowsHtml : '<tr><td colspan="13" class="text-center text-muted py-3">কোনো প্রডাক্ট এন্ট্রি পাওয়া যায়নি।</td></tr>') + '</tbody>' +
                        '</table>' +
                    '</div>' +
                '</div>' +
            '</div>';
        }

        function renderIdWiseDepreciationRegisterPage() {
            let container = document.getElementById('tab-container');
            let products = getProductData();
            let master = getMasterData();
            let disposals = getDisposalData();
            let productSchedulesHtml = '';

            products.forEach(p => {
                let mGroup = master[p.gIdx];
                let assetName = (mGroup && mGroup.items[p.iIdx]) ? mGroup.items[p.iIdx].name : 'অজানা সম্পদ';
                let scheduleData = calculateSingleProductSchedule(p, disposals, currentBranch);

                let schedRows = '';
                scheduleData.schedule.forEach(s => {
                    if(s.status === 'Inactive') return;
                    let sYInfo = getYearMetaInfo(s.year);
                    let statusBadge = s.status === 'Disposed' ? '<span class="badge bg-danger">Disposed</span>' : '<span class="badge bg-success">Active</span>';
                    schedRows += '<tr><td>' + sYInfo.tabDisplayName + '</td><td>৳' + s.openingWdv.toLocaleString() + '</td><td>৳' + s.dep.toLocaleString() + '</td><td>৳' + s.cumDep.toLocaleString() + '</td><td>৳' + s.closingWdv.toLocaleString() + '</td><td>' + statusBadge + '</td></tr>';
                });

                let pYInfo = getYearMetaInfo(p.yearNum);
                let pDateStr = formatDateToDisplay(p.purchaseDate) || '-';

                productSchedulesHtml += '<div class="card border-secondary shadow-sm mb-4">' +
                    '<div class="card-header bg-secondary text-white fw-bold d-flex justify-content-between align-items-center">' +
                        '<span>📦 প্রডাক্ট আইডি: <span class="text-warning">' + (p.productId || 'N/A') + '</span> | সম্পদ: ' + assetName + '</span>' +
                        '<span>ক্রয়ের তারিখ: ' + pDateStr + ' | ক্রয়ের বছর: ' + pYInfo.tabDisplayName + ' | মূল্য: ৳' + scheduleData.initialCost.toLocaleString() + '</span>' +
                    '</div>' +
                    '<div class="card-body bg-light">' +
                        '<div class="table-responsive">' +
                            '<table class="table table-bordered table-sm align-middle text-center bg-white">' +
                                '<thead class="table-dark"><tr><th>অর্থ বছর</th><th>প্রারম্ভিক WDV</th><th>চলতি বছরের অবচয়</th><th>ক্রমপুঞ্জীভূত অবচয়</th><th>সমাপনী WDV</th><th>স্ট্যাটাস</th></tr></thead>' +
                                '<tbody>' + schedRows + '</tbody>' +
                            '</table>' +
                        '</div>' +
                    '</div>' +
                '</div>';
            });

            container.innerHTML = '<div class="card border-info shadow-sm mb-4"><div class="card-header bg-info text-dark fw-bold">📊 আইডি ভিত্তিক অবচয় রেজিস্টার - [' + currentBranch + ']</div><div class="card-body bg-light">' + productSchedulesHtml + '</div></div>';
        }

        function addProductRow() {
            let products = getProductData();
            let newId = 'PRD-' + (1001 + products.length);
            products.push({ productId: newId, gIdx: 0, iIdx: 0, entryType: 'purchase', yearNum: 1, purchaseDate: '', qty: 1, price: 0, openingWdv: 0, openingAccDep: 0, remarks: '' });
            saveProductData(products);
            renderProductSubSheetPage(currentProductFilter);
        }

        function updateProductField(idx, field, val) {
            let products = getProductData();
            if(['price', 'qty', 'openingWdv', 'openingAccDep', 'yearNum'].includes(field)) {
                products[idx][field] = val === '' ? 0 : Math.round(parseFloat(val));
            } else {
                products[idx][field] = val;
            }
            saveProductData(products);
        }

        function updateProductYear(idx, val) {
            let products = getProductData();
            let targetYear = parseInt(val) || 1;
            if(targetYear > 1) {
                products[idx].entryType = 'purchase';
                products[idx].openingWdv = 0;
                products[idx].openingAccDep = 0;
            }
            products[idx].yearNum = targetYear;
            saveProductData(products);
            renderProductSubSheetPage(currentProductFilter);
        }

        function updateProductType(idx, val) {
            let products = getProductData();
            if(val === 'old_purchase' && parseInt(products[idx].yearNum) > 1) {
                alert('Year-01 এর পর শুধুমাত্র নতুন ক্রয় করা সম্ভব!');
                return;
            }
            products[idx].entryType = val;
            if(val === 'purchase') {
                products[idx].openingWdv = 0;
                products[idx].openingAccDep = 0;
            }
            saveProductData(products);
            renderProductSubSheetPage(currentProductFilter);
        }

        function updateProductItemRef(idx, val) {
            let parts = val.split('_');
            let products = getProductData();
            products[idx].gIdx = parseInt(parts[0]);
            products[idx].iIdx = parseInt(parts[1]);
            saveProductData(products);
        }

        function deleteProductRow(idx) {
            let products = getProductData();
            products.splice(idx, 1);
            saveProductData(products);
            renderProductSubSheetPage(currentProductFilter);
        }

        function renderRestoreBinPage() {
            let master = getMasterData();
            let container = document.getElementById('tab-container');
            let deleted = [];
            master.forEach((group, gIdx) => {
                group.items.forEach((item, iIdx) => {
                    if(item.isDeleted) {
                        for(let y=1; y<=20; y++) {
                            if(item.isDeleted[y]) deleted.push({ gIdx: gIdx, iIdx: iIdx, y: y, name: item.name });
                        }
                    }
                });
            });
            let html = '';
            if(deleted.length === 0) {
                html = '<div class="alert alert-warning text-center fw-bold">♻️ কোনো ডিলিট করা সারি নেই।</div>';
            } else {
                let liItems = '';
                deleted.forEach(d => {
                    let yInfo = getYearMetaInfo(d.y);
                    liItems += '<li class="list-group-item d-flex justify-content-between align-items-center"><span>[' + yInfo.tabDisplayName + '] <strong>' + d.name + '</strong></span><button class="btn btn-sm btn-success fw-bold" onclick="restoreRow(' + d.y + ', ' + d.gIdx + ', ' + d.iIdx + ')">Restore</button></li>';
                });
                html = '<ul class="list-group">' + liItems + '</ul>';
            }
            container.innerHTML = '<div class="card border-info shadow-sm"><div class="card-header bg-info text-dark fw-bold">♻️ Restore Bin (' + currentBranch + ')</div><div class="card-body">' + html + '</div></div>';
        }

        function renderYearRegister(yearNum) {
            let allYearsData = calculateFullChain();
            let yearData = allYearsData[yearNum - 1];
            let container = document.getElementById('tab-container');

            let html = '<div class="table-responsive">' +
                '<table class="table table-bordered align-middle text-center">' +
                    '<thead>' +
                        '<tr>' +
                            '<th>ক্রমিক নং</th>' +
                            '<th class="col-asset-name">সম্পদের নাম</th>' +
                            '<th>মজুদ (QTY)</th>' +
                            '<th>মোট মূল্য (TK.)</th>' +
                            '<th>নতুন ক্রয় (TK.)</th>' +
                            '<th>সম্পদ বাতিল (TK.)</th>' +
                            '<th>বিগত বছরের অবচয় বিয়োগকৃত মূল্য(WDV)</th>' +
                            '<th>বিগত বছর পর্যন্ত মোট অবচয়</th>' +
                            '<th>অবচয় হার (%)</th>' +
                            '<th>চলতি বছরের অবচয়</th>' +
                            '<th>ক্রমপুঞ্জীভূত অবচয়</th>' +
                            '<th>অবচয় বিয়োগকৃত মূল্য</th>' +
                            '<th class="no-print">Action</th>' +
                        '</tr>' +
                    '</thead>' +
                    '<tbody>';

            let gCount = 1;
            let grandP = 0, grandPV = 0, grandPTD = 0, grandCD = 0, grandCumD = 0, grandCV = 0, grandNew = 0, grandDisposed = 0;

            yearData.forEach((group, gIdx) => {
                html += '<tr class="category-header"><td colspan="13">' + group.cat + ' (' + currentBranch + ')</td></tr>';
                let tPrice = 0, tPrevVal = 0, tPrevTotDep = 0, tCurrDep = 0, tCumDep = 0, tCurrVal = 0, tNewBuy = 0, tDisposed = 0;

                group.items.forEach((item, iIdx) => {
                    let masterItem = getMasterData()[gIdx].items[iIdx];
                    if (masterItem.isDeleted && masterItem.isDeleted[yearNum] === true) return;
                    let isHidden = masterItem.isHidden && masterItem.isHidden[yearNum] === true;

                    if (!isHidden) {
                        tPrice += (item.price !== '' ? Math.round(parseFloat(item.price)) : 0);
                        tPrevVal += (item.prevVal !== '' ? Math.round(parseFloat(item.prevVal)) : 0);
                        tPrevTotDep += (item.prevTotDep !== '' ? Math.round(parseFloat(item.prevTotDep)) : 0);
                        tCurrDep += (item.currDep !== '' ? Math.round(parseFloat(item.currDep)) : 0);
                        tCumDep += (item.cumDep !== '' ? Math.round(parseFloat(item.cumDep)) : 0);
                        tCurrVal += (item.currVal !== '' ? Math.round(parseFloat(item.currVal)) : 0);
                        tNewBuy += (item.effectiveNewBuy || 0);
                        tDisposed += (item.effectiveDisposed || 0);
                    }

                    let currentRateVal = (masterItem.rate && masterItem.rate[yearNum] !== undefined) ? masterItem.rate[yearNum] : item.activeRate;
                    let currentCustomDep = (masterItem.overrideCurrDep && masterItem.overrideCurrDep[yearNum] !== undefined) ? masterItem.overrideCurrDep[yearNum] : formatVal(item.currDep);
                    let qtyVal = item.qty !== '' ? item.qty : '';

                    let products = getProductData();
                    let hasNewPurchase = products.some(p => p.gIdx == gIdx && p.iIdx == iIdx && parseInt(p.yearNum) === yearNum && p.entryType === 'purchase');
                    let isDepEditable = (hasNewPurchase || (item.effectiveNewBuy && item.effectiveNewBuy > 0) || (item.prevVal !== '' && parseFloat(item.prevVal) > 0));

                    let rowClass = isHidden ? 'row-hidden' : '';
                    html += '<tr class="' + rowClass + '">' +
                        '<td>' + (gCount++) + '</td>' +
                        '<td class="col-asset-name"><input type="text" value="' + item.name + '" oninput="updateMasterItemName(' + gIdx + ', ' + iIdx + ', this.value)" class="editable-cell text-start fw-bold"></td>' +
                        '<td><span class="qty-clickable fw-bold" onclick="openItemDetailModal(' + gIdx + ', ' + iIdx + ', ' + yearNum + ')">' + (qtyVal !== '' ? qtyVal : '-') + '</span></td>' +
                        '<td>' + formatVal(item.price) + '</td>' +
                        '<td><input type="text" value="' + (item.effectiveNewBuy > 0 ? item.effectiveNewBuy : '') + '" readonly class="fw-bold"></td>' +
                        '<td><input type="text" value="' + (item.effectiveDisposed > 0 ? item.effectiveDisposed : '') + '" readonly class="fw-bold text-danger"></td>' +
                        '<td><input type="text" value="' + formatVal(item.prevVal) + '" readonly class="fw-bold"></td>' +
                        '<td><input type="text" value="' + formatVal(item.prevTotDep) + '" readonly class="fw-bold"></td>' +
                        '<td><input type="text" value="' + currentRateVal + '" oninput="updateRate(' + yearNum + ', ' + gIdx + ', ' + iIdx + ', this.value)" class="editable-cell fw-bold"></td>' +
                        '<td><input type="text" value="' + (isDepEditable ? currentCustomDep : formatVal(item.currDep)) + '" readonly class="fw-bold"></td>' +
                        '<td>' + formatVal(item.cumDep) + '</td>' +
                        '<td class="fw-bold">' + formatVal(item.currVal) + '</td>' +
                        '<td class="no-print">' +
                            '<button class="btn ' + (isHidden ? 'btn-secondary btn-hide-active' : 'btn-outline-secondary') + ' btn-sm py-0 px-1 fw-bold mb-1" onclick="toggleHideRow(' + yearNum + ', ' + gIdx + ', ' + iIdx + ')">' + (isHidden ? 'Unhide' : 'Hide') + '</button>' +
                            '<button class="btn btn-outline-danger btn-sm py-0 px-1 fw-bold" onclick="deleteRow(' + yearNum + ', ' + gIdx + ', ' + iIdx + ')">Delete</button>' +
                        '</td>' +
                    '</tr>';
                });

                grandP += tPrice; grandPV += tPrevVal; grandPTD += tPrevTotDep; grandCD += tCurrDep; grandCumD += tCumDep; grandCV += tCurrVal; grandNew += tNewBuy; grandDisposed += tDisposed;
            });

            html += '<tr class="grand-total-row">' +
                '<td colspan="3" class="text-end">সর্বমোট (' + currentBranch + ')ঃ</td>' +
                '<td>' + formatVal(grandP) + '</td><td>' + formatVal(grandNew) + '</td><td>' + formatVal(grandDisposed) + '</td>' +
                '<td>' + formatVal(grandPV) + '</td><td>' + formatVal(grandPTD) + '</td><td>-</td>' +
                '<td>' + formatVal(grandCD) + '</td><td>' + formatVal(grandCumD) + '</td><td>' + formatVal(grandCV) + '</td>' +
                '<td class="no-print"></td>' +
            '</tr>';
            html += '</tbody></table></div>';
            container.innerHTML = html;
        }

        function updateMasterItemName(gIdx, iIdx, newName) {
            let m = getMasterData();
            if(m[gIdx] && m[gIdx].items[iIdx]) {
                m[gIdx].items[iIdx].name = newName;
                saveMasterData(m);
            }
        }

        function renderTopSheet() {
            let allYearsData = calculateFullChain();
            let master = getMasterData();
            let disposals = getDisposalData();
            let container = document.getElementById('tab-container');

            let categoryNames = ["আসবাবপত্র", "ইকুইপমেন্ট", "কম্পিউটার"];
            let catYearPrices = [[], [], []];
            let totalAssetYears = [], totalDepYears = [], netWdvYears = [], totalDisposedYears = [], totalLossYears = [];

            for (let y = 1; y <= 20; y++) {
                let yrTotalAsset = 0, yrTotalDep = 0, yrNetWdv = 0, yrDisposedVal = 0, yrLossVal = 0;

                categoryNames.forEach((catName, gIdx) => {
                    let gTotal = 0;
                    allYearsData[y-1][gIdx].items.forEach((item, iIdx) => {
                        let delObj = master[gIdx].items[iIdx].isDeleted;
                        let hidObj = master[gIdx].items[iIdx].isHidden;
                        if (!(delObj && delObj[y]) && !(hidObj && hidObj[y])) {
                            gTotal += (item.price !== '' ? Math.round(parseFloat(item.price)) : 0);
                            yrTotalDep += (item.cumDep !== '' ? Math.round(parseFloat(item.cumDep)) : 0);
                            yrNetWdv += (item.currVal !== '' ? Math.round(parseFloat(item.currVal)) : 0);
                            yrDisposedVal += (item.effectiveDisposed || 0);
                        }
                    });
                    catYearPrices[gIdx].push(gTotal);
                    yrTotalAsset += gTotal;
                });

                disposals.forEach(d => {
                    if(parseInt(d.disposalYear) === y) {
                        yrLossVal += Math.round(parseFloat(d.lossVal) || 0);
                    }
                });

                totalAssetYears.push(yrTotalAsset);
                totalDepYears.push(yrTotalDep);
                netWdvYears.push(yrNetWdv);
                totalDisposedYears.push(yrDisposedVal);
                totalLossYears.push(yrLossVal);
            }

            let thYears = '';
            for(let i=1; i<=20; i++) {
                thYears += '<th>' + getYearMetaInfo(i).tabDisplayName + '</th>';
            }

            let r1 = catYearPrices[0].map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r2 = catYearPrices[1].map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r3 = catYearPrices[2].map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r4 = totalAssetYears.map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r5 = totalDepYears.map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r6 = netWdvYears.map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r7 = totalDisposedYears.map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');
            let r8 = totalLossYears.map(function(val) { return '<td>' + formatVal(val) + '</td>'; }).join('');

            container.innerHTML = '<div class="card border-warning shadow-sm mb-4">' +
                '<div class="card-header bg-warning text-dark fw-bold"><span>📊 স্থায়ী সম্পদ সমাপনী ও অবচয় সামারি রিপোর্ট (Summary Report) - [' + currentBranch + ']</span></div>' +
                '<div class="card-body bg-light">' +
                    '<div class="table-responsive">' +
                        '<table class="table table-bordered align-middle text-center bg-white">' +
                            '<thead><tr class="table-secondary"><th>ক্রমিক নং</th><th class="text-start ps-3">বিবরণ / ক্যাটাগরি</th>' + thYears + '</tr></thead>' +
                            '<tbody>' +
                                '<tr><td>1</td><td class="text-start ps-3 fw-bold">আসবাবপত্র মোটঃ</td>' + r1 + '</tr>' +
                                '<tr><td>2</td><td class="text-start ps-3 fw-bold">ইকুইপমেন্ট মোটঃ</td>' + r2 + '</tr>' +
                                '<tr><td>3</td><td class="text-start ps-3 fw-bold">কম্পিউটার মোটঃ</td>' + r3 + '</tr>' +
                                '<tr class="table-success fw-bold"><td>4</td><td class="text-start ps-3">সর্বমোট সম্পদ (Total Assets):</td>' + r4 + '</tr>' +
                                '<tr class="table-danger fw-bold text-danger"><td>5</td><td class="text-start ps-3 text-danger">মোট অবচয়:</td>' + r5 + '</tr>' +
                                '<tr class="table-primary fw-bold text-primary"><td>6</td><td class="text-start ps-3 text-primary">অবচয় বিয়োগকৃত মূল্য:</td>' + r6 + '</tr>' +
                                '<tr class="table-secondary fw-bold text-danger"><td>7</td><td class="text-start ps-3 text-danger">এ পর্যন্ত সম্পদ বাতিল:</td>' + r7 + '</tr>' +
                                '<tr class="some-cls fw-bold text-dark table-secondary"><td>8</td><td class="text-start ps-3 text-danger">এ পর্যন্ত সম্পদ বাতিল জনিত ক্ষতি:</td>' + r8 + '</tr>' +
                            '</tbody>' +
                        '</table>' +
                    '</div>' +
                '</div>' +
            '</div>';
        }

        function toggleHideRow(y, g, i) {
            let m = getMasterData();
            m[g].items[i].isHidden = m[g].items[i].isHidden || {};
            m[g].items[i].isHidden[y] = !m[g].items[i].isHidden[y];
            saveMasterData(m);
            renderYearRegister(y);
        }
        function deleteRow(y, g, i) {
            let m = getMasterData();
            m[g].items[i].isDeleted = m[g].items[i].isDeleted || {};
            m[g].items[i].isDeleted[y] = true;
            saveMasterData(m);
            renderYearRegister(y);
        }
        function restoreRow(y, g, i) {
            let m = getMasterData();
            if(m[g].items[i].isDeleted) m[g].items[i].isDeleted[y] = false;
            saveMasterData(m);
            renderRestoreBinPage();
        }
        function updateRate(y, g, i, v) { 
            let m = getMasterData(); 
            m[g].items[i].rate = m[g].items[i].rate || {}; 
            m[g].items[i].rate[y] = v === '' ? '' : parseFloat(v); 
            saveMasterData(m); 
        }
        
        function refreshData() {
            setActiveTab(currentYearTab);
            alert('ডেটা সফলভাবে রিফ্রেস হয়েছে!');
        }

        function unhideAllRows() {
            let m = getMasterData();
            m.forEach(g => g.items.forEach(i => { if(i.isHidden) i.isHidden[currentYearTab] = false; }));
            saveMasterData(m);
            renderYearRegister(currentYearTab);
        }
        function addRow(g) {
            let m = getMasterData();
            m[g].items.push({ name: "নতুন সম্পদ", qty: {}, price: {}, rate: {}, defaultRate: (g===2?25:10), isHidden:{}, isDeleted:{} });
            saveMasterData(m);
            if(!['Top', 'DisposalManager', 'RestoreBin', 'ProductSubSheet', 'IdWiseDepreciationRegister'].includes(currentYearTab)) renderYearRegister(currentYearTab);
        }
        function resetAllData() {
            if(confirm('সব মুছে ফেলতে চান?')) { 
                localStorage.removeItem(getKeys().master); 
                localStorage.removeItem(getKeys().products); 
                localStorage.removeItem(getKeys().disposals); 
                initData(); 
                setActiveTab(1); 
            }
        }

        // স্থায়ীভাবে ডেটা ফাইলের ভেতরে এমবেড করে ডাউনলোড করার ফাংশন
        function saveDraft() {
            let allStorageData = {};
            for (let i = 0; i < localStorage.length; i++) {
                let key = localStorage.key(i);
                if (key.startsWith('ccda_')) {
                    allStorageData[key] = localStorage.getItem(key);
                }
            }

            let jsonString = JSON.stringify(allStorageData);
            let base64Data = btoa(unescape(encodeURIComponent(jsonString)));

            let fullHtml = document.documentElement.outerHTML;
            let placeholderMark = '/*EMBEDDED_DATA_PLACEHOLDER*/';
            
            if (fullHtml.includes(placeholderMark)) {
                fullHtml = fullHtml.replace(placeholderMark, base64Data);
            } else {
                fullHtml = fullHtml.replace('<script id="embeddedDataStore" type="application/json">', '<script id="embeddedDataStore" type="application/json">' + base64Data);
            }

            let blob = new Blob([fullHtml], { type: 'text/html;charset=utf-8' });
            let url = URL.createObjectURL(blob);
            let a = document.createElement('a'); 
            a.href = url; 
            a.download = 'CCDA_Asset_Register_' + currentBranch + '.html'; 
            a.click();
            URL.revokeObjectURL(url);
        }
    </script>
</body>
</html>
