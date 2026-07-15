<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>借支申请 · 双端分离（管理员登录）</title>
    <style>
        /* ===== 全局重置 & 基础 ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            background: #f1f5f9;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding: 30px 20px;
        }

        .app-container {
            max-width: 1100px;
            width: 100%;
            background: #ffffff;
            border-radius: 24px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.08);
            padding: 32px 36px 40px;
            transition: all 0.2s;
        }

        /* ===== 角色切换 ===== */
        .role-switcher {
            display: flex;
            justify-content: center;
            gap: 12px;
            margin-bottom: 28px;
            border-bottom: 2px solid #eef2f6;
            padding-bottom: 18px;
        }

        .role-btn {
            padding: 10px 30px;
            border: 2px solid #d1d9e6;
            border-radius: 40px;
            background: transparent;
            font-size: 16px;
            font-weight: 600;
            color: #475569;
            cursor: pointer;
            transition: all 0.2s;
        }

        .role-btn:hover {
            border-color: #94a3b8;
            background: #f8fafc;
        }

        .role-btn.active {
            background: #2563eb;
            border-color: #2563eb;
            color: #ffffff;
            box-shadow: 0 4px 14px rgba(37, 99, 235, 0.25);
        }

        .role-btn.active:hover {
            background: #1d4ed8;
        }

        /* ===== 头部 ===== */
        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 24px;
            padding-bottom: 16px;
            border-bottom: 2px solid #eef2f6;
        }

        .app-header h1 {
            font-size: 26px;
            font-weight: 700;
            color: #0f172a;
            letter-spacing: 0.5px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .app-header h1 .badge {
            font-size: 14px;
            font-weight: 500;
            background: #dbeafe;
            color: #1d4ed8;
            padding: 2px 14px;
            border-radius: 30px;
            letter-spacing: 0.3px;
        }

        .header-actions {
            display: flex;
            gap: 12px;
            align-items: center;
            flex-wrap: wrap;
        }

        .header-actions .stats {
            font-size: 14px;
            color: #475569;
            background: #f8fafc;
            padding: 6px 18px;
            border-radius: 30px;
            border: 1px solid #e2e8f0;
        }

        .header-actions .stats strong {
            color: #0f172a;
        }

        .local-badge {
            font-size: 12px;
            color: #2563eb;
            background: #eff6ff;
            padding: 4px 14px;
            border-radius: 30px;
            border: 1px solid #bfdbfe;
        }

        /* ===== 按钮通用 ===== */
        .btn {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 10px 24px;
            border: none;
            border-radius: 12px;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            text-decoration: none;
            white-space: nowrap;
        }

        .btn-primary {
            background: #2563eb;
            color: #ffffff;
            box-shadow: 0 4px 12px rgba(37, 99, 235, 0.25);
        }
        .btn-primary:hover {
            background: #1d4ed8;
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(37, 99, 235, 0.3);
        }
        .btn-primary:active {
            transform: translateY(0);
        }

        .btn-success {
            background: #059669;
            color: #ffffff;
            box-shadow: 0 4px 12px rgba(5, 150, 105, 0.25);
        }
        .btn-success:hover {
            background: #047857;
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(5, 150, 105, 0.3);
        }

        .btn-outline {
            background: transparent;
            color: #475569;
            border: 1.5px solid #cbd5e1;
        }
        .btn-outline:hover {
            background: #f8fafc;
            border-color: #94a3b8;
        }

        .btn-danger-outline {
            background: transparent;
            color: #dc2626;
            border: 1.5px solid #fca5a5;
        }
        .btn-danger-outline:hover {
            background: #fef2f2;
            border-color: #ef4444;
        }

        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
        }

        /* ===== 表单卡片 ===== */
        .form-card {
            background: #fafcff;
            border-radius: 18px;
            padding: 28px 30px 30px;
            border: 1px solid #e9eef3;
            margin-bottom: 24px;
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 20px;
            align-items: end;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .form-group label {
            font-size: 14px;
            font-weight: 600;
            color: #1e293b;
            letter-spacing: 0.3px;
        }

        .form-group label .required {
            color: #dc2626;
            margin-left: 2px;
        }

        .form-group input {
            padding: 12px 16px;
            border: 1.5px solid #d1d9e6;
            border-radius: 12px;
            font-size: 15px;
            background: #ffffff;
            transition: border-color 0.2s, box-shadow 0.2s;
            outline: none;
            width: 100%;
            color: #0f172a;
        }

        .form-group input:focus {
            border-color: #2563eb;
            box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.12);
        }

        .form-group input::placeholder {
            color: #94a3b8;
        }

        .form-group .hint {
            font-size: 12px;
            color: #94a3b8;
            margin-top: 4px;
        }

        .form-group .hint.error {
            color: #dc2626;
        }

        .form-group .hint.success {
            color: #059669;
        }

        .form-actions {
            display: flex;
            gap: 12px;
            align-items: center;
            padding-top: 6px;
        }

        .form-actions .btn {
            flex: 1;
            justify-content: center;
            padding: 12px 20px;
        }

        /* ===== 金额快捷按钮 ===== */
        .amount-quick {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin-top: 6px;
        }

        .amount-quick .chip {
            padding: 4px 14px;
            border-radius: 30px;
            font-size: 13px;
            font-weight: 500;
            border: 1.5px solid #d1d9e6;
            background: #ffffff;
            color: #334155;
            cursor: pointer;
            transition: all 0.15s;
        }

        .amount-quick .chip:hover {
            border-color: #2563eb;
            color: #2563eb;
            background: #eff6ff;
        }

        .amount-quick .chip.active {
            border-color: #2563eb;
            background: #2563eb;
            color: #ffffff;
        }

        .amount-input-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .amount-input-group .form-group {
            flex: 1;
            min-width: 120px;
        }

        .amount-input-group .amount-quick {
            flex: 1;
            min-width: 140px;
            align-items: center;
        }

        /* ===== 消息提示 ===== */
        .toast {
            padding: 12px 20px;
            border-radius: 12px;
            font-size: 14px;
            font-weight: 500;
            margin-bottom: 16px;
            display: none;
            align-items: center;
            gap: 10px;
            animation: slideDown 0.3s ease;
        }

        .toast.show {
            display: flex;
        }

        .toast-success {
            background: #ecfdf5;
            color: #065f46;
            border: 1px solid #a7f3d0;
        }

        .toast-error {
            background: #fef2f2;
            color: #991b1b;
            border: 1px solid #fca5a5;
        }

        .toast-info {
            background: #eff6ff;
            color: #1e40af;
            border: 1px solid #bfdbfe;
        }

        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-12px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* ===== 数据表格 ===== */
        .table-section {
            margin-top: 8px;
        }

        .table-header-tools {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 16px;
        }

        .table-header-tools h2 {
            font-size: 18px;
            font-weight: 600;
            color: #0f172a;
        }

        .table-wrapper {
            overflow-x: auto;
            border-radius: 14px;
            border: 1px solid #e9eef3;
            background: #ffffff;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 14px;
            min-width: 560px;
        }

        thead {
            background: #f8fafc;
            border-bottom: 2px solid #e2e8f0;
        }

        thead th {
            padding: 14px 18px;
            text-align: left;
            font-weight: 600;
            color: #1e293b;
            font-size: 13px;
            letter-spacing: 0.3px;
            text-transform: uppercase;
            white-space: nowrap;
        }

        tbody tr {
            border-bottom: 1px solid #f1f5f9;
            transition: background 0.15s;
        }

        tbody tr:hover {
            background: #fafcff;
        }

        tbody tr:last-child {
            border-bottom: none;
        }

        tbody td {
            padding: 14px 18px;
            color: #0f172a;
            vertical-align: middle;
        }

        tbody td .id-card-mask {
            font-family: "SF Mono", "Menlo", monospace;
            font-size: 13px;
            letter-spacing: 0.5px;
        }

        tbody td .amount-cell {
            font-weight: 600;
            color: #0f172a;
        }

        .empty-state {
            text-align: center;
            padding: 50px 20px;
            color: #94a3b8;
        }

        .empty-state .icon {
            font-size: 48px;
            margin-bottom: 12px;
            opacity: 0.6;
        }

        .empty-state p {
            font-size: 15px;
        }

        /* ===== 视图控制 ===== */
        .view-employee .admin-only {
            display: none !important;
        }

        .view-admin .employee-only {
            display: none !important;
        }

        /* ===== 管理员登录框 ===== */
        .login-overlay {
            background: #fafcff;
            border-radius: 18px;
            padding: 30px 28px;
            border: 1px solid #e9eef3;
            margin-bottom: 24px;
            max-width: 420px;
            margin-left: auto;
            margin-right: auto;
        }

        .login-overlay h3 {
            font-size: 20px;
            font-weight: 600;
            color: #0f172a;
            margin-bottom: 6px;
        }

        .login-overlay .sub {
            color: #64748b;
            font-size: 14px;
            margin-bottom: 20px;
        }

        .login-overlay .form-group {
            margin-bottom: 14px;
        }

        .login-overlay .form-actions {
            margin-top: 8px;
        }

        .login-overlay .form-actions .btn {
            flex: 1;
        }

        .login-error {
            color: #dc2626;
            font-size: 13px;
            margin-top: 6px;
            display: none;
        }

        .login-error.show {
            display: block;
        }

        /* ===== 响应式 ===== */
        @media (max-width: 820px) {
            .app-container {
                padding: 20px 16px;
            }

            .form-row {
                grid-template-columns: 1fr;
                gap: 16px;
            }

            .app-header {
                flex-direction: column;
                align-items: flex-start;
                gap: 14px;
            }

            .app-header h1 {
                font-size: 22px;
            }

            .header-actions {
                width: 100%;
                justify-content: space-between;
            }

            .form-actions {
                flex-direction: column;
            }

            .form-actions .btn {
                width: 100%;
            }

            .table-header-tools {
                flex-direction: column;
                align-items: stretch;
                gap: 10px;
            }

            .table-header-tools .btn {
                justify-content: center;
            }

            .role-switcher {
                flex-direction: column;
                align-items: stretch;
            }

            .login-overlay {
                max-width: 100%;
            }
        }

        @media (max-width: 480px) {
            .app-container {
                padding: 16px 12px;
            }

            .form-card {
                padding: 18px 16px;
            }

            thead th,
            tbody td {
                padding: 10px 12px;
                font-size: 13px;
            }

            .amount-quick .chip {
                font-size: 12px;
                padding: 3px 10px;
            }

            .app-header h1 {
                font-size: 19px;
            }
        }

        .spinner {
            display: inline-block;
            width: 18px;
            height: 18px;
            border: 2.5px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: #ffffff;
            animation: spin 0.7s linear infinite;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }

        /* 登录状态提示 */
        .login-status {
            font-size: 13px;
            color: #059669;
            background: #ecfdf5;
            padding: 4px 14px;
            border-radius: 30px;
            border: 1px solid #a7f3d0;
        }
    </style>
</head>
<body>
    <div class="app-container" id="app">
        <!-- 角色切换 -->
        <div class="role-switcher">
            <button class="role-btn active" data-role="employee" id="roleEmployee">🧑‍💼 员工端</button>
            <button class="role-btn" data-role="admin" id="roleAdmin">👨‍💻 管理员端</button>
        </div>

        <!-- 通用提示 -->
        <div class="toast" id="toast"></div>

        <!-- ===== 员工端视图 ===== -->
        <div class="view-employee" id="employeeView">
            <header class="app-header employee-only">
                <h1>
                    📋 借支申请
                    <span class="badge">员工</span>
                </h1>
                <div class="header-actions">
                    <span class="local-badge">💾 数据保存在本地</span>
                </div>
            </header>

            <section class="form-card">
                <form id="submitForm" novalidate>
                    <div class="form-row">
                        <div class="form-group">
                            <label>
                                姓名 <span class="required">*</span>
                            </label>
                            <input type="text" id="name" placeholder="请输入员工姓名" maxlength="20" autocomplete="name" required />
                            <div class="hint" id="nameHint"></div>
                        </div>
                        <div class="form-group">
                            <label>
                                身份证号 <span class="required">*</span>
                            </label>
                            <input type="text" id="idCard" placeholder="18 位身份证号码" maxlength="18" autocomplete="off" required />
                            <div class="hint" id="idCardHint">请输入 18 位身份证号（末尾可为 X）</div>
                        </div>
                        <div class="form-group">
                            <label>
                                借支金额 (元) <span class="required">*</span>
                            </label>
                            <div class="amount-input-group">
                                <input type="number" id="amount" placeholder="0.00" min="0.01" step="100" required />
                                <div class="amount-quick" id="quickAmounts">
                                    <span class="chip" data-value="300">300</span>
                                    <span class="chip" data-value="500">500</span>
                                </div>
                            </div>
                            <div class="hint" id="amountHint"></div>
                        </div>
                    </div>

                    <div class="form-actions" style="margin-top:18px;">
                        <button type="submit" class="btn btn-primary" id="submitBtn">
                            ✅ 提交申请
                        </button>
                        <button type="reset" class="btn btn-outline" id="resetBtn">
                            ↺ 清空
                        </button>
                    </div>
                </form>
            </section>

            <div class="employee-only" style="text-align:center; color:#94a3b8; padding:20px 0 10px; border-top:1px solid #eef2f6; margin-top:10px;">
                <p style="font-size:15px;">📌 提交后请等待管理员审核，您无法查看他人记录。</p>
            </div>
        </div>

        <!-- ===== 管理员端视图 ===== -->
        <div class="view-admin admin-only" id="adminView" style="display:none;">
            <!-- 登录框（默认显示，登录后隐藏） -->
            <div class="login-overlay" id="loginBox">
                <h3>🔐 管理员登录</h3>
                <div class="sub">请输入管理员账号和密码</div>
                <form id="loginForm">
                    <div class="form-group">
                        <label>账号</label>
                        <input type="text" id="loginUsername" placeholder="请输入账号" value="admin" />
                    </div>
                    <div class="form-group">
                        <label>密码</label>
                        <input type="password" id="loginPassword" placeholder="请输入密码" value="123456" />
                    </div>
                    <div class="login-error" id="loginError">❌ 账号或密码错误，请重试</div>
                    <div class="form-actions">
                        <button type="submit" class="btn btn-primary" id="loginBtn">登录</button>
                    </div>
                    <div style="margin-top:12px; font-size:13px; color:#94a3b8; text-align:center;">
                    </div>
                </form>
            </div>

            <!-- 管理内容（登录后显示） -->
            <div id="adminContent" style="display:none;">
                <header class="app-header">
                    <h1>
                        📋 借支管理
                        <span class="badge" style="background:#fef3c7;color:#b45309;">管理员</span>
                    </h1>
                    <div class="header-actions">
                        <span class="login-status">✅ 已登录</span>
                        <span class="stats">
                            📊 总记录：<strong id="totalCount">0</strong>
                        </span>
                        <button class="btn btn-success" id="exportBtn" title="导出为 CSV 表格">
                            ⬇️ 导出表格
                        </button>
                        <button class="btn btn-outline" id="logoutBtn" title="退出登录">🚪 退出</button>
                    </div>
                </header>

                <section class="table-section">
                    <div class="table-header-tools">
                        <h2>📄 全部借支记录</h2>
                        <div style="display:flex; gap:10px; flex-wrap:wrap;">
                            <button class="btn btn-danger-outline" id="clearAllBtn" title="清空所有记录">
                                🗑️ 清空全部
                            </button>
                        </div>
                    </div>

                    <div class="table-wrapper">
                        <table>
                            <thead>
                                <tr>
                                    <th style="width:50px;">#</th>
                                    <th>姓名</th>
                                    <th>身份证号</th>
                                    <th style="text-align:right;">金额 (元)</th>
                                    <th style="text-align:center;">提交时间</th>
                                </tr>
                            </thead>
                            <tbody id="recordsBody">
                                <!-- 动态渲染 -->
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>
        </div>
    </div>

    <script>
        // ================================================================
        //  双端分离 + 管理员登录（纯前端演示）
        // ================================================================
        const STORAGE_KEY = 'loanRecords';
        const SESSION_KEY = 'adminLoggedIn';

        // 默认管理员凭证
        const ADMIN_CRED = { username: 'admin', password: '123456' };

        // 当前角色
        let currentRole = 'employee'; // 'employee' | 'admin'

        // DOM 引用
        const roleEmployee = document.getElementById('roleEmployee');
        const roleAdmin = document.getElementById('roleAdmin');
        const employeeView = document.getElementById('employeeView');
        const adminView = document.getElementById('adminView');
        const appContainer = document.getElementById('app');

        const loginBox = document.getElementById('loginBox');
        const adminContent = document.getElementById('adminContent');
        const loginForm = document.getElementById('loginForm');
        const loginUsername = document.getElementById('loginUsername');
        const loginPassword = document.getElementById('loginPassword');
        const loginError = document.getElementById('loginError');
        const loginBtn = document.getElementById('loginBtn');
        const logoutBtn = document.getElementById('logoutBtn');

        const form = document.getElementById('submitForm');
        const nameInput = document.getElementById('name');
        const idCardInput = document.getElementById('idCard');
        const amountInput = document.getElementById('amount');
        const submitBtn = document.getElementById('submitBtn');
        const resetBtn = document.getElementById('resetBtn');
        const exportBtn = document.getElementById('exportBtn');
        const clearAllBtn = document.getElementById('clearAllBtn');
        const recordsBody = document.getElementById('recordsBody');
        const totalCount = document.getElementById('totalCount');
        const toast = document.getElementById('toast');
        const nameHint = document.getElementById('nameHint');
        const idCardHint = document.getElementById('idCardHint');
        const amountHint = document.getElementById('amountHint');
        const quickChips = document.querySelectorAll('.chip');

        let records = [];
        let isSubmitting = false;

        // ================================================================
        //  工具函数
        // ================================================================
        function formatTime(iso) {
            if (!iso) return '--';
            try {
                const d = new Date(iso);
                const pad = n => String(n).padStart(2, '0');
                return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())} ${pad(d.getHours())}:${pad(d.getMinutes())}`;
            } catch {
                return iso;
            }
        }

        function formatMoney(val) {
            return Number(val).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',');
        }

        function maskIdCard(id) {
            if (!id || id.length < 10) return id;
            return id.slice(0, 6) + '********' + id.slice(-4);
        }

        function showToast(message, type = 'info') {
            toast.className = 'toast show toast-' + type;
            toast.textContent = message;
            clearTimeout(toast._timer);
            toast._timer = setTimeout(() => {
                toast.classList.remove('show');
            }, 4000);
        }

        function setLoading(btn, loading) {
            if (loading) {
                btn.disabled = true;
                btn._origText = btn.innerHTML;
                btn.innerHTML = '<span class="spinner"></span> 处理中...';
            } else {
                btn.disabled = false;
                if (btn._origText) btn.innerHTML = btn._origText;
            }
        }

        // ================================================================
        //  数据管理
        // ================================================================
        function loadRecords() {
            try {
                const local = localStorage.getItem(STORAGE_KEY);
                records = local ? JSON.parse(local) : [];
            } catch {
                records = [];
            }
            renderAdminTable();
            updateStats();
        }

        function saveRecords() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(records));
        }

        function addRecord(record) {
            record.createdAt = new Date().toISOString();
            records.push(record);
            saveRecords();
            renderAdminTable();
            updateStats();
        }

        function clearAllRecords() {
            if (records.length === 0) {
                showToast('暂无记录可清空', 'info');
                return;
            }
            if (!confirm('⚠️ 确定要清空所有借支记录吗？此操作不可恢复！')) return;
            records = [];
            saveRecords();
            renderAdminTable();
            updateStats();
            showToast('✅ 已清空所有记录', 'success');
        }

        // ================================================================
        //  导出 CSV
        // ================================================================
        function exportCSV() {
            if (records.length === 0) {
                showToast('⚠️ 没有记录可导出', 'error');
                return;
            }
            const headers = ['序号', '姓名', '身份证号', '金额(元)', '提交时间'];
            const rows = [];
            const sorted = [...records];
            sorted.forEach((r, idx) => {
                rows.push([
                    idx + 1,
                    r.name,
                    r.idCard,
                    Number(r.amount).toFixed(2),
                    formatTime(r.createdAt)
                ]);
            });

            let csvContent = headers.join(',') + '\n';
            rows.forEach(row => {
                const escaped = row.map(field => {
                    if (typeof field === 'string' && (field.includes(',') || field.includes('"'))) {
                        return `"${field.replace(/"/g, '""')}"`;
                    }
                    return field;
                });
                csvContent += escaped.join(',') + '\n';
            });

            const blob = new Blob(['\uFEFF' + csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            const url = URL.createObjectURL(blob);
            link.href = url;
            link.download = `借支记录_${new Date().toISOString().slice(0,10)}.csv`;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
            showToast('✅ 导出成功！', 'success');
        }

        // ================================================================
        //  渲染表格（管理员）
        // ================================================================
        function renderAdminTable() {
            if (!recordsBody) return;
            if (records.length === 0) {
                recordsBody.innerHTML = `
                    <tr>
                        <td colspan="5">
                            <div class="empty-state">
                                <div class="icon">📭</div>
                                <p>暂无借支记录</p>
                            </div>
                        </td>
                    </tr>
                `;
                return;
            }
            const sorted = [...records].reverse();
            let html = '';
            sorted.forEach((r, idx) => {
                const realIdx = records.length - idx;
                html += `
                    <tr>
                        <td style="text-align:center;font-weight:500;color:#64748b;">${realIdx}</td>
                        <td><strong>${escapeHtml(r.name)}</strong></td>
                        <td><span class="id-card-mask">${escapeHtml(maskIdCard(r.idCard))}</span></td>
                        <td style="text-align:right;font-weight:600;">¥ ${formatMoney(r.amount)}</td>
                        <td style="text-align:center;color:#64748b;font-size:13px;">${formatTime(r.createdAt)}</td>
                    </tr>
                `;
            });
            recordsBody.innerHTML = html;
        }

        function updateStats() {
            if (totalCount) totalCount.textContent = records.length;
        }

        function escapeHtml(text) {
            if (!text) return '';
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        // ================================================================
        //  表单验证
        // ================================================================
        function validateName(name) {
            if (!name || name.trim() === '') return { valid: false, msg: '请输入姓名' };
            if (name.trim().length < 2) return { valid: false, msg: '姓名至少 2 个字符' };
            return { valid: true, msg: '✅' };
        }

        function validateIdCard(id) {
            const val = id.trim().toUpperCase();
            if (!val) return { valid: false, msg: '请输入身份证号' };
            const regex = /^[1-9]\d{5}(18|19|20)?\d{2}(0[1-9]|1[0-2])(0[1-9]|[12]\d|3[01])\d{3}[\dX]$/;
            if (!regex.test(val)) return { valid: false, msg: '格式有误，请检查位数和校验码' };
            const year = parseInt(val.slice(6, 10));
            const month = parseInt(val.slice(10, 12));
            const day = parseInt(val.slice(12, 14));
            const now = new Date();
            const currentYear = now.getFullYear();
            if (year < 1900 || year > currentYear) return { valid: false, msg: '出生年份不合理' };
            if (month < 1 || month > 12) return { valid: false, msg: '出生月份不合理' };
            const maxDay = new Date(year, month, 0).getDate();
            if (day < 1 || day > maxDay) return { valid: false, msg: '出生日期不合理' };
            return { valid: true, msg: '✅ 格式正确' };
        }

        function validateAmount(val) {
            const num = parseFloat(val);
            if (isNaN(num) || num <= 0) return { valid: false, msg: '请输入大于 0 的金额' };
            if (num > 99999999) return { valid: false, msg: '金额过大，请重新输入' };
            return { valid: true, msg: '✅' };
        }

        function getFormData() {
            return {
                name: nameInput.value.trim(),
                idCard: idCardInput.value.trim().toUpperCase(),
                amount: amountInput.value.trim()
            };
        }

        function validateForm() {
            const { name, idCard, amount } = getFormData();
            const nameResult = validateName(name);
            const idResult = validateIdCard(idCard);
            const amountResult = validateAmount(amount);

            nameHint.textContent = nameResult.msg;
            nameHint.className = 'hint ' + (nameResult.valid ? 'success' : 'error');

            idCardHint.textContent = idResult.msg;
            idCardHint.className = 'hint ' + (idResult.valid ? 'success' : 'error');

            amountHint.textContent = amountResult.msg;
            amountHint.className = 'hint ' + (amountResult.valid ? 'success' : 'error');

            return nameResult.valid && idResult.valid && amountResult.valid;
        }

        // ================================================================
        //  登录 / 登出
        // ================================================================
        function isLoggedIn() {
            return sessionStorage.getItem(SESSION_KEY) === 'true';
        }

        function setLoggedIn(status) {
            if (status) {
                sessionStorage.setItem(SESSION_KEY, 'true');
            } else {
                sessionStorage.removeItem(SESSION_KEY);
            }
        }

        function showAdminContent(show) {
            if (show) {
                loginBox.style.display = 'none';
                adminContent.style.display = 'block';
            } else {
                loginBox.style.display = 'block';
                adminContent.style.display = 'none';
                loginError.classList.remove('show');
                // 清空密码框
                loginPassword.value = '';
            }
        }

        function handleLogin(e) {
            e.preventDefault();
            const username = loginUsername.value.trim();
            const password = loginPassword.value.trim();

            if (username === ADMIN_CRED.username && password === ADMIN_CRED.password) {
                setLoggedIn(true);
                showAdminContent(true);
                // 渲染数据
                renderAdminTable();
                updateStats();
                showToast('✅ 登录成功！', 'success');
                loginError.classList.remove('show');
            } else {
                loginError.classList.add('show');
                showToast('❌ 账号或密码错误', 'error');
            }
        }

        function handleLogout() {
            setLoggedIn(false);
            showAdminContent(false);
            showToast('已退出登录', 'info');
            // 如果当前在管理员视图，但未登录，显示登录框
            if (currentRole === 'admin') {
                // 保持视图，但内容已切换为登录框
            }
        }

        // ================================================================
        //  角色切换
        // ================================================================
        function setRole(role) {
            currentRole = role;
            roleEmployee.classList.toggle('active', role === 'employee');
            roleAdmin.classList.toggle('active', role === 'admin');

            if (role === 'employee') {
                employeeView.style.display = 'block';
                adminView.style.display = 'none';
                appContainer.classList.add('view-employee');
                appContainer.classList.remove('view-admin');
            } else {
                employeeView.style.display = 'none';
                adminView.style.display = 'block';
                appContainer.classList.add('view-admin');
                appContainer.classList.remove('view-employee');

                // 检查登录状态
                if (isLoggedIn()) {
                    showAdminContent(true);
                    renderAdminTable();
                    updateStats();
                } else {
                    showAdminContent(false);
                    // 清空错误
                    loginError.classList.remove('show');
                }
            }
            toast.classList.remove('show');
        }

        // ================================================================
        //  事件绑定
        // ================================================================

        // 角色切换
        roleEmployee.addEventListener('click', () => setRole('employee'));
        roleAdmin.addEventListener('click', () => setRole('admin'));

        // 登录
        loginForm.addEventListener('submit', handleLogin);

        // 登出
        logoutBtn.addEventListener('click', handleLogout);

        // 提交表单（员工端）
        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            if (isSubmitting) return;
            if (!validateForm()) {
                showToast('⚠️ 请完善表单信息后再提交', 'error');
                return;
            }
            const { name, idCard, amount } = getFormData();
            isSubmitting = true;
            setLoading(submitBtn, true);

            try {
                const record = { name, idCard, amount: parseFloat(amount) };
                addRecord(record);
                showToast('✅ 借支申请提交成功！', 'success');
                amountInput.value = '';
                quickChips.forEach(c => c.classList.remove('active'));
                amountHint.textContent = '';
                amountHint.className = 'hint';
                amountInput.focus();
            } catch (err) {
                showToast('❌ 提交异常：' + err.message, 'error');
            } finally {
                isSubmitting = false;
                setLoading(submitBtn, false);
            }
        });

        // 重置
        resetBtn.addEventListener('click', () => {
            form.reset();
            quickChips.forEach(c => c.classList.remove('active'));
            nameHint.textContent = '';
            nameHint.className = 'hint';
            idCardHint.textContent = '请输入 18 位身份证号（末尾可为 X）';
            idCardHint.className = 'hint';
            amountHint.textContent = '';
            amountHint.className = 'hint';
            nameInput.focus();
        });

        // 金额快捷
        quickChips.forEach(chip => {
            chip.addEventListener('click', () => {
                const value = chip.dataset.value;
                if (value) {
                    amountInput.value = value;
                    quickChips.forEach(c => c.classList.remove('active'));
                    chip.classList.add('active');
                    validateForm();
                    amountInput.focus();
                }
            });
        });

        // 实时验证
        nameInput.addEventListener('blur', validateForm);
        idCardInput.addEventListener('blur', validateForm);
        amountInput.addEventListener('blur', validateForm);

        nameInput.addEventListener('input', () => {
            if (nameInput.value.trim().length >= 2) validateForm();
        });
        idCardInput.addEventListener('input', () => {
            if (idCardInput.value.trim().length >= 18) validateForm();
        });
        amountInput.addEventListener('input', () => {
            if (amountInput.value.trim() !== '') {
                validateForm();
                quickChips.forEach(c => c.classList.remove('active'));
            }
        });

        // 导出（管理员）
        exportBtn.addEventListener('click', exportCSV);

        // 清空（管理员）
        clearAllBtn.addEventListener('click', clearAllRecords);

        // Ctrl+Enter 提交
        document.addEventListener('keydown', (e) => {
            if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
                e.preventDefault();
                if (!submitBtn.disabled) {
                    form.dispatchEvent(new Event('submit'));
                }
            }
        });

        // ================================================================
        //  初始化
        // ================================================================
        function init() {
            loadRecords();
            // 默认员工端
            setRole('employee');
            // 如果 session 中有登录状态，但在员工端不影响，切换到管理员时会自动登录
            if (isLoggedIn()) {
                // 预先设置登录状态，但员工端不显示
                console.log('管理员已登录（会话有效）');
            }
            nameInput.focus();
        }

        init();
    </script>
</body>
</html>
