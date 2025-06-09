<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel - Gaming Tournament</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* --- Dark Theme Variables --- */
        :root {
            --primary-bg: #0F172A; /* Slate 900 */
            --secondary-bg: #1E293B; /* Slate 800 */
            --card-bg: #1E293B; /* Slate 800 */
            --sidebar-bg: #111827; /* Gray 900 */
            --text-primary: #E2E8F0; /* Slate 200 */
            --text-secondary: #94A3B8; /* Slate 400 */
            --text-muted-custom: #64748B; /* Slate 500 - Improved visibility for muted text */
            --accent-color: #FACC15; /* Yellow 500 */
            --accent-gradient: linear-gradient(to right, #FACC15, #FBBF24);
            --primary-button-bg: #3B82F6; /* Blue 500 */
            --border-color: #334155; /* Slate 700 */
            --success-color: #10B981; /* Emerald 500 */
            --danger-color: #EF4444; /* Red 500 */
            --warning-color: #F59E0B; /* Amber 500 */
            --info-color: #60A5FA; /* Blue 400 */
            --link-color: #9CA3AF; /* Gray 400 */
            --link-hover-color: #E5E7EB; /* Gray 200 */
            --placeholder-bg: rgba(100, 116, 139, 0.15); /* Slate 500 with opacity */
        }
        body { background-color: var(--primary-bg); color: var(--text-primary); font-family: 'Poppins', sans-serif; padding-top: 60px; }
        .app-header { position: fixed; top: 0; left: 0; width: 100%; height: 60px; background-color: var(--secondary-bg); display: flex; align-items: center; justify-content: space-between; padding: 0 15px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); z-index: 1030; border-bottom: 1px solid var(--border-color);}
        .header-left { display: flex; align-items: center; gap: 10px;}
        .menu-toggle-btn { background: none; border: none; color: var(--text-primary); font-size: 1.5rem; padding: 0 5px; margin-left: -5px;}
        .header-logo { width: 35px; height: 35px; border-radius: 50%; background-color: #fff; object-fit: cover; border: 1px solid var(--accent-color);}
        .header-title { font-size: 1rem; font-weight: 600; line-height: 1.2;}
        .header-right { display: flex; align-items: center; gap: 15px;}
        .admin-email-header { font-size: 0.8rem; color: var(--text-secondary); display: none; }
        @media (min-width: 576px) { .admin-email-header { display: inline; } }
        .offcanvas-start { background-color: var(--sidebar-bg); color: var(--link-color); width: 260px !important; border-right: 1px solid var(--border-color);}
        .offcanvas-header { border-bottom: 1px solid var(--border-color); padding: 1rem 1.2rem;}
        .offcanvas-title h5 { color: var(--text-primary); margin-bottom: 0; font-size: 1.1rem; }
        .btn-close-white { filter: invert(1) grayscale(100%) brightness(200%);}
        .offcanvas-body { padding: 0; display: flex; flex-direction: column; height: calc(100% - 56px); }
        .offcanvas-body .nav { flex-grow: 1; overflow-y: auto; }
        .offcanvas-body .nav-link { color: var(--link-color); padding: 0.8rem 1.2rem; border-left: 3px solid transparent; font-size: 0.95rem;}
        .offcanvas-body .nav-link.active, .offcanvas-body .nav-link:hover { color: var(--link-hover-color); background-color: rgba(255, 255, 255, 0.05); border-left-color: var(--accent-color);}
        .offcanvas-body .nav-link i { margin-right: 12px; width: 20px; text-align: center; font-size: 1.1rem;}
        .offcanvas-logout-btn { margin: 1rem; border-top: 1px solid var(--border-color); padding-top: 1rem;}
        .main-content { padding: 20px; }
        .section { display: none; animation: fadeIn 0.5s; }
        .section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .section-title { font-size: 1.25rem; font-weight: 600; margin-bottom: 20px; }
        .login-container { max-width: 400px; margin: 10vh auto; background-color: var(--secondary-bg); padding: 2rem; border-radius: 8px; border: 1px solid var(--border-color); box-shadow: 0 4px 12px rgba(0,0,0,0.2);}
        #adminLoader { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); display: flex; justify-content: center; align-items: center; z-index: 9999; display: none; }
        .spinner-border { color: var(--accent-color); }
        .card { background-color: var(--card-bg); border: 1px solid var(--border-color); color: var(--text-primary); margin-bottom: 1.5rem; }
        .card-title { color: var(--text-primary); }
        .table-responsive { overflow-x: auto; }
        .table { background-color: var(--card-bg); color: var(--text-primary); border-color: var(--border-color); margin-bottom: 0;}
        .table > :not(caption) > * > * { background-color: transparent !important; border-bottom-color: var(--border-color); }
        .table th { color: var(--text-secondary); font-weight: 500; white-space: nowrap; padding: 0.8rem 1rem;}
        .table td { vertical-align: middle; white-space: nowrap; padding: 0.7rem 1rem;}
        .table td .text-muted { color: var(--text-muted-custom) !important; }
        .table td small.text-muted { font-size: 0.85em; }
        .table-hover > tbody > tr:hover > * { background-color: rgba(255, 255, 255, 0.03) !important; color: var(--text-primary); }
        .action-buttons button, .action-buttons a { margin-right: 5px; margin-bottom: 5px; }
        .modal-content { background-color: var(--secondary-bg); color: var(--text-primary); border: 1px solid var(--border-color); }
        .modal-header { border-bottom-color: var(--border-color); }
        .modal-footer { border-top-color: var(--border-color); }
        .modal-body label { margin-bottom: 0.5rem; font-weight: 500; color: var(--text-secondary); }
        .form-control, .form-select { background-color: var(--primary-bg); color: var(--text-primary); border: 1px solid var(--border-color); }
        .form-control:focus, .form-select:focus { background-color: var(--primary-bg); color: var(--text-primary); border-color: var(--accent-color); box-shadow: 0 0 0 0.2rem rgba(250, 204, 21, 0.25); }
        .form-control::placeholder { color: var(--text-secondary); opacity: 0.7; }
        .form-control:disabled, .form-select:disabled { background-color: #2d3748; opacity: 0.7; }
        .form-control[type="file"] { background-color: var(--primary-bg); color: var(--text-secondary); border: 1px solid var(--border-color); }
        .imgbb-upload-status { display: none; margin-top: 5px; font-size: 0.85rem; color: var(--text-secondary);}
        .table img.preview-img { max-width: 60px; height: 40px; object-fit: cover; border-radius: 4px; cursor: pointer; border: 1px solid var(--border-color); }
        .text-bg-primary { background-color: #3B82F6 !important; }
        .text-bg-info { background-color: #0ea5e9 !important; }
        .text-bg-warning { background-color: #f59e0b !important; }
        .text-bg-success { background-color: #10b981 !important; }
        .text-bg-danger { background-color: #ef4444 !important; }
        .text-bg-secondary { background-color: #6b7280 !important; }
        .text-bg-dark { background-color: #374151 !important; }
        .nav-tabs .nav-link { color: var(--text-secondary); border-color: var(--border-color); border-bottom-color: transparent; margin-bottom: -1px;}
        .nav-tabs .nav-link.active { color: var(--accent-color); background-color: var(--card-bg); border-color: var(--border-color); border-bottom-color: var(--card-bg); font-weight: 600; }
        .nav-tabs { border-bottom-color: var(--border-color); }
        textarea.form-control { min-height: 120px; }
        .copy-btn { cursor: pointer; opacity: 0.6; transition: opacity 0.2s; }
        .copy-btn:hover { opacity: 1; }
        .status-badge { font-size: 0.8em; padding: 0.3em 0.6em; border-radius: 0.25rem; }
        .loading-placeholder td > div { background-color: var(--placeholder-bg); border-radius: 4px; min-height: 1.2em; animation: placeholder-glow 2s ease-in-out infinite; }
        @keyframes placeholder-glow { 0% { background-color: var(--placeholder-bg); } 50% { background-color: rgba(100, 116, 139, 0.2); } 100% { background-color: var(--placeholder-bg); } }
        .dash-card-icon { font-size: 1.8rem; opacity: 0.6; position: absolute; right: 15px; top: 50%; transform: translateY(-50%); }
        .card-body { position: relative; }
    </style>
</head>
<body>
    <!-- ... (All your HTML code from your previous post, unchanged) ... -->
    <!-- For brevity, the HTML is not repeated here. Use your own full HTML as above. -->
    <!-- Place the following script just before the closing </body> tag: -->

    <!-- Bootstrap JS (required for modals/offcanvas) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <!-- Demo JavaScript for Section Switching, Demo Data, and Modal Reset -->
    <script>
    // Section Switching
    document.querySelectorAll('.offcanvas-body .nav-link').forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            document.querySelectorAll('.offcanvas-body .nav-link').forEach(l => l.classList.remove('active'));
            this.classList.add('active');
            let sectionId = this.getAttribute('data-section');
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            // Update header title
            document.getElementById('adminPageTitle').textContent = this.textContent.trim();
            // Close offcanvas on mobile
            let sidebar = bootstrap.Offcanvas.getOrCreateInstance(document.getElementById('adminSidebar'));
            sidebar.hide();
        });
    });

    // Demo Data for Dashboard
    function populateDemoStats() {
        document.getElementById('statTotalUsers').textContent = '1245';
        document.getElementById('statActiveTournaments').textContent = '3';
        document.getElementById('statPendingWithdrawals').textContent = '2';
        document.getElementById('statCompletedWithdrawals').textContent = '120';
        document.getElementById('statRejectedWithdrawals').textContent = '4';
        document.getElementById('statTotalGames').textContent = '8';
        document.getElementById('statTotalPromotions').textContent = '5';
        document.getElementById('statFinishedTournaments').textContent = '18';
        document.getElementById('pendingWithdrawalCountBadge').style.display = 'inline-block';
        document.getElementById('pendingWithdrawalCountBadge').textContent = '2';
    }
    // Demo Data for Tables
    function populateDemoTables() {
        // Games Table
        document.getElementById('gamesTableBody').innerHTML = `
            <tr>
                <td><img src="https://via.placeholder.com/60x40?text=BGMI" class="preview-img"></td>
                <td>BGMI</td>
                <td>game_bgmi</td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-primary"><i class="bi bi-pencil"></i></button>
                    <button class="btn btn-sm btn-danger"><i class="bi bi-trash"></i></button>
                </td>
            </tr>
            <tr>
                <td><img src="https://via.placeholder.com/60x40?text=Free+Fire" class="preview-img"></td>
                <td>Free Fire</td>
                <td>game_ff</td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-primary"><i class="bi bi-pencil"></i></button>
                    <button class="btn btn-sm btn-danger"><i class="bi bi-trash"></i></button>
                </td>
            </tr>
        `;
        // Promotions Table
        document.getElementById('promotionsTableBody').innerHTML = `
            <tr>
                <td><img src="https://via.placeholder.com/60x40?text=Promo1" class="preview-img"></td>
                <td><a href="https://t.me/example" target="_blank">https://t.me/example</a></td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-primary"><i class="bi bi-pencil"></i></button>
                    <button class="btn btn-sm btn-danger"><i class="bi bi-trash"></i></button>
                </td>
            </tr>
        `;
        // Tournaments Table
        document.getElementById('tournamentsTableBody').innerHTML = `
            <tr>
                <td>BGMI Solo</td>
                <td>BGMI</td>
                <td>₹20</td>
                <td>₹500</td>
                <td>2025-06-10 18:00</td>
                <td><span class="badge text-bg-info">Upcoming</span></td>
                <td>85/100</td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-primary"><i class="bi bi-pencil"></i></button>
                    <button class="btn btn-sm btn-danger"><i class="bi bi-trash"></i></button>
                </td>
            </tr>
        `;
        // Users Table
        document.getElementById('usersTableBody').innerHTML = `
            <tr>
                <td>UID1234</td>
                <td>PlayerOne</td>
                <td>player1@example.com</td>
                <td>₹120.00</td>
                <td><span class="badge text-bg-success">Active</span></td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-primary"><i class="bi bi-eye"></i></button>
                </td>
            </tr>
            <tr>
                <td>UID5678</td>
                <td>PlayerTwo</td>
                <td>player2@example.com</td>
                <td>₹0.00</td>
                <td><span class="badge text-bg-danger">Blocked</span></td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-primary"><i class="bi bi-eye"></i></button>
                </td>
            </tr>
        `;
        // Withdrawals Tables
        document.getElementById('pendingWithdrawalsTableBody').innerHTML = `
            <tr>
                <td>2025-06-09 12:30</td>
                <td>PlayerOne</td>
                <td>₹100</td>
                <td>UPI</td>
                <td class="action-buttons">
                    <button class="btn btn-sm btn-success"><i class="bi bi-check"></i></button>
                    <button class="btn btn-sm btn-danger"><i class="bi bi-x"></i></button>
                </td>
            </tr>
        `;
        document.getElementById('completedWithdrawalsTableBody').innerHTML = `
            <tr>
                <td>2025-06-08 15:00</td>
                <td>2025-06-08 16:00</td>
                <td>PlayerTwo</td>
                <td>₹200</td>
                <td>Paytm</td>
                <td>Paid</td>
            </tr>
        `;
        document.getElementById('rejectedWithdrawalsTableBody').innerHTML = `
            <tr>
                <td>2025-06-07 10:00</td>
                <td>2025-06-07 11:00</td>
                <td>PlayerThree</td>
                <td>₹50</td>
                <td>UPI</td>
                <td>Invalid UPI</td>
            </tr>
        `;
    }

    // Demo: Show admin area by default (no login)
    document.getElementById('auth-container').style.display = 'none';
    document.getElementById('admin-main-area').style.display = 'block';
    populateDemoStats();
    populateDemoTables();

    // Add Demo Data button
    document.getElementById('addDemoDataBtn').addEventListener('click', function() {
        populateDemoStats();
        populateDemoTables();
        alert('Demo data added!');
    });

    // Modal Reset (clear forms on open)
    document.querySelectorAll('.modal').forEach(modalEl => {
        modalEl.addEventListener('show.bs.modal', function() {
            let forms = modalEl.querySelectorAll('form');
            forms.forEach(f => f.reset());
            let statuses = modalEl.querySelectorAll('.imgbb-upload-status, .mt-2, .mt-3');
            statuses.forEach(s => s.innerHTML = '');
        });
    });

    // Copy to clipboard for UID/referral code/request ID
    document.querySelectorAll('.copy-btn').forEach(btn => {
        btn.addEventListener('click', function() {
            let target = document.querySelector(this.getAttribute('data-target'));
            if (target) {
                navigator.clipboard.writeText(target.textContent.trim());
                this.title = 'Copied!';
                setTimeout(() => { this.title = 'Copy'; }, 1000);
            }
        });
    });

    // Search Users (demo, client-side)
    document.getElementById('userSearchInput').addEventListener('input', function() {
        let val = this.value.toLowerCase();
        document.querySelectorAll('#usersTableBody tr').forEach(row => {
            let show = Array.from(row.children).some(td => td.textContent.toLowerCase().includes(val));
            row.style.display = show ? '' : 'none';
        });
    });
    </script>
</body>
</html![Screenshot_20250609_165439](https://github.com/user-attachments/assets/81a48456-1016-418b-b854-028c29de3e7c)
