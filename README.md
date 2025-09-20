<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>FABRECK DO BRASIL - App de Garantia com IA</title>
    <script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Variáveis de cores Fabreck */
        :root {
            --fabreck-red: #D22630;
            --fabreck-blue: #0047AB;
            --fabreck-white: #FFFFFF;
            --fabreck-light: #F5F7FA;
            --fabreck-dark: #1A2A3A;
            --fabreck-gray: #7F8C8D;
            --fabreck-success: #2ECC71;
            --fabreck-warning: #F39C12;
            --fabreck-danger: #E74C3C;

            /* Cores para o Modo Escuro */
            --dark-bg: #121212;
            --dark-card-bg: #1E1E1E;
            --dark-text: #E0E0E0;
            --dark-light-text: #B0B0B0;
            --dark-border: #333333;
            --dark-header-bg-start: #003366;
            --dark-header-bg-end: #0A0A0A;
            --dark-blue-light: #0056B3;
        }
        
        /* Reset e fontes */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent; /* Remove highlight em mobile */
        }
        
        html, body {
            height: 100%;
            overflow: hidden; /* Previne scroll no body */
        }

        body {
            background: var(--fabreck-light);
            color: var(--fabreck-dark);
            line-height: 1.6;
            transition: background 0.3s ease, color 0.3s ease;
        }

        /* --- ESTILOS DO LOGIN --- */
        .login-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--fabreck-blue), var(--fabreck-dark));
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 3000;
            transition: opacity 0.3s ease;
        }
        .login-overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }
        .login-card {
            background: var(--fabreck-white);
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            width: 90%;
            max-width: 400px;
            text-align: center;
        }
        body.dark-mode .login-card {
            background: var(--dark-card-bg);
        }
        .login-card .logo-img {
            width: 180px;
            margin-bottom: 20px;
        }
        .login-card h2 {
            color: var(--fabreck-blue);
            margin-bottom: 20px;
        }
        body.dark-mode .login-card h2 {
             color: var(--dark-blue-light);
        }
        .login-card .form-group {
            text-align: left;
        }
         /* --- FIM ESTILOS DO LOGIN --- */

        /* --- LAYOUT PRINCIPAL (DESKTOP/WEB) --- */
        #layoutContainer {
            display: flex;
            height: 100vh;
            width: 100%;
        }

        #sidebar {
            width: 260px;
            background: var(--fabreck-dark);
            color: var(--fabreck-white);
            display: none; /* Escondido por padrão em mobile */
            flex-direction: column;
            padding: 20px;
            box-shadow: 5px 0 15px rgba(0,0,0,0.1);
            transition: background 0.3s ease;
        }
        body.dark-mode #sidebar {
            background: var(--dark-card-bg);
            border-right: 1px solid var(--dark-border);
        }

        .sidebar-header {
            text-align: center;
            margin-bottom: 30px;
        }
        .sidebar-header .logo-img {
            width: 150px;
            margin-bottom: 10px;
        }
        .sidebar-header h3 {
            font-size: 16px;
            opacity: 0.8;
        }

        .sidebar-nav {
            flex-grow: 1;
        }
        .sidebar-btn {
            display: flex;
            align-items: center;
            padding: 15px;
            color: var(--fabreck-white);
            opacity: 0.7;
            text-decoration: none;
            border-radius: 12px;
            margin-bottom: 8px;
            font-weight: 500;
            transition: all 0.3s ease;
        }
        body.dark-mode .sidebar-btn {
            color: var(--dark-text);
        }
        .sidebar-btn i {
            font-size: 20px;
            width: 30px;
            margin-right: 15px;
        }
        .sidebar-btn:hover {
            opacity: 1;
            background: rgba(255, 255, 255, 0.1);
        }
        .sidebar-btn.active {
            opacity: 1;
            background: var(--fabreck-blue);
            color: var(--fabreck-white);
            font-weight: 600;
        }
        body.dark-mode .sidebar-btn.active {
             background: var(--dark-blue-light);
        }
        .sidebar-footer {
            font-size: 12px;
            text-align: center;
            opacity: 0.5;
            padding-top: 20px;
            border-top: 1px solid rgba(255,255,255,0.1);
        }
        body.dark-mode .sidebar-footer {
            border-top-color: var(--dark-border);
        }

        #mainContentWrapper {
            flex: 1;
            overflow-y: auto; /* Permite scroll do conteúdo principal */
            position: relative;
            padding: 10px;
        }
        /* --- FIM LAYOUT PRINCIPAL --- */

        /* Estilos para o Modo Escuro */
        body.dark-mode {
            background: var(--dark-bg);
            color: var(--dark-text);
        }

        body.dark-mode header {
            background: linear-gradient(135deg, var(--dark-header-bg-start), var(--dark-header-bg-end));
            border: 1px solid var(--dark-border);
        }

        body.dark-mode .fabreck-pattern {
            background: 
                linear-gradient(135deg, var(--dark-blue-light) 25%, transparent 25%) -50px 0,
                linear-gradient(225deg, var(--dark-blue-light) 25%, transparent 25%) -50px 0,
                linear-gradient(315deg, var(--fabreck-red) 25%, transparent 25%),
                linear-gradient(45deg, var(--fabreck-red) 25%, transparent 25%);
        }

        body.dark-mode .logo h1,
        body.dark-mode .system-title,
        body.dark-mode .company-info p,
        body.dark-mode .active-salesman {
            color: var(--dark-text);
        }

        body.dark-mode .card {
            background: var(--dark-card-bg);
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.3);
            border: 1px solid var(--dark-border);
        }

        body.dark-mode .card-header {
            border-bottom: 2px solid rgba(0, 71, 171, 0.3);
        }

        body.dark-mode .card-title {
            color: var(--dark-blue-light);
        }

        body.dark-mode .form-group label {
            color: var(--dark-light-text);
        }

        body.dark-mode .form-control {
            background: var(--dark-bg);
            color: var(--dark-text);
            border: 1px solid var(--dark-border);
        }

        body.dark-mode .form-control:focus {
            border-color: var(--dark-blue-light);
            box-shadow: 0 0 0 3px rgba(0, 71, 171, 0.4);
        }

        body.dark-mode .btn-primary,
        body.dark-mode .btn-info {
            background: var(--dark-blue-light);
        }

        body.dark-mode .btn-danger {
            background: var(--fabreck-danger);
        }

        body.dark-mode .btn-success {
            background: var(--fabreck-success);
        }

        body.dark-mode .btn-warning {
            background: var(--fabreck-warning);
        }

        body.dark-mode .code-preview {
            color: var(--dark-light-text);
        }

        body.dark-mode .code-preview.valid {
            color: var(--fabreck-success); /* Keep success color bright */
        }
        body.dark-mode .code-preview.invalid {
            color: var(--fabreck-danger); /* Keep danger color bright */
        }

        body.dark-mode .scanner-status {
            background: rgba(0, 0, 0, 0.7);
            color: var(--dark-text);
        }

        body.dark-mode .scanned-history {
            background: rgba(0, 71, 171, 0.1);
            border: 1px solid rgba(0, 71, 171, 0.2);
        }

        body.dark-mode .scanned-item {
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        body.dark-mode .scanned-code {
            color: var(--dark-blue-light);
        }

        body.dark-mode .scanned-time {
            color: var(--dark-light-text);
        }

        body.dark-mode .activity-log {
            background: var(--dark-card-bg);
            border: 1px solid var(--dark-border);
        }

        body.dark-mode .activity-item {
            border-bottom: 1px dashed rgba(255, 255, 255, 0.1);
        }

        body.dark-mode .activity-icon {
            background: rgba(0, 71, 171, 0.2);
            color: var(--dark-blue-light);
        }

        body.dark-mode .activity-content {
            color: var(--dark-light-text);
        }

        body.dark-mode .activity-time {
            color: var(--dark-gray);
        }

        body.dark-mode .info-section {
            background: rgba(0, 71, 171, 0.1);
            border: 1px solid rgba(0, 71, 171, 0.2);
            color: var(--dark-light-text);
        }

        body.dark-mode .info-section h3 {
            color: var(--dark-blue-light);
        }

        body.dark-mode .report-filters,
        body.dark-mode .batch-actions {
            background: rgba(0, 71, 171, 0.1);
            border: 1px solid rgba(0, 71, 171, 0.2);
        }

        body.dark-mode table {
            color: var(--dark-light-text);
        }

        body.dark-mode th {
            background: rgba(0, 71, 171, 0.2);
            color: var(--dark-blue-light);
        }

        body.dark-mode tr:nth-child(even) {
            background: rgba(0, 71, 171, 0.05);
        }

        body.dark-mode .table-footer {
            background: rgba(0, 71, 171, 0.2);
            border-top: 2px solid var(--dark-blue-light);
        }

        body.dark-mode .total-box {
            background: var(--dark-card-bg);
            border: 1px solid var(--dark-border);
        }

        body.dark-mode .total-title {
            color: var(--dark-blue-light);
        }

        body.dark-mode .total-value {
            color: var(--dark-text);
        }

        body.dark-mode .footer {
            background: var(--dark-card-bg);
            border-top: 1px solid var(--dark-border);
            box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.3);
        }

        body.dark-mode .nav-btn {
            color: var(--dark-light-text);
        }

        body.dark-mode .nav-btn.active {
            color: var(--dark-blue-light);
            background: rgba(0, 71, 171, 0.2);
        }

        body.dark-mode .rules-modal .rules-content,
        body.dark-mode .edit-modal .edit-content {
            background: var(--dark-card-bg);
            border: 2px solid var(--dark-blue-light);
        }

        body.dark-mode .rules-title,
        body.dark-mode .edit-title {
            color: var(--dark-blue-light);
        }

        body.dark-mode .rules-list li,
        body.dark-mode .edit-modal label {
            color: var(--dark-light-text);
        }

        body.dark-mode .rules-highlight {
            background: rgba(0, 71, 171, 0.2);
            color: var(--dark-blue-light);
            border: 1px solid rgba(0, 71, 171, 0.3);
        }

        body.dark-mode .warranty-option {
            background: rgba(0, 71, 171, 0.1);
            border: 1px solid rgba(0, 71, 171, 0.2);
            color: var(--dark-light-text);
        }

        body.dark-mode .warranty-option.selected {
            background: var(--dark-blue-light);
            color: var(--fabreck-white);
            border-color: var(--dark-blue-light);
        }

        body.dark-mode .persisted-field::after {
            color: var(--fabreck-success); /* Keep success color bright */
            background: rgba(46, 204, 113, 0.2);
        }
        
        .container {
            max-width: 100%; /* Default for mobile */
            margin: 0 auto;
            padding-bottom: 80px; /* Espaço para o footer */
        }
        
        /* Estilos do cabeçalho */
        header {
            background: linear-gradient(135deg, var(--fabreck-blue), var(--fabreck-dark));
            padding: 15px;
            border-radius: 16px;
            margin-bottom: 15px;
            text-align: center;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
            position: sticky;
            top: 0;
            z-index: 10;
            border: 1px solid rgba(255, 255, 255, 0.1);
            overflow: hidden;
            position: relative;
        }

        /* Padrão de fundo no cabeçalho */
        .fabreck-pattern {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.03;
            pointer-events: none;
            background: 
                linear-gradient(135deg, var(--fabreck-blue) 25%, transparent 25%) -50px 0,
                linear-gradient(225deg, var(--fabreck-blue) 25%, transparent 25%) -50px 0,
                linear-gradient(315deg, var(--fabreck-red) 25%, transparent 25%),
                linear-gradient(45deg, var(--fabreck-red) 25%, transparent 25%);
            background-size: 100px 100px;
            background-color: transparent;
        }
        
        .logo {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
        }
        
        .logo-img {
            width: 150px; 
            height: auto;
        }

        .logo h1 {
            font-size: 22px;
            font-weight: 700;
            color: var(--fabreck-white);
            letter-spacing: 0.5px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        
        .system-title {
            font-size: 16px;
            opacity: 0.9;
            color: rgba(255, 255, 255, 0.85);
            margin-top: 5px;
            font-weight: 500;
            position: relative;
            z-index: 1;
        }
        
        .company-info {
            margin-top: 10px;
            padding: 10px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.15);
            font-size: 13px;
            backdrop-filter: blur(5px);
            position: relative;
            z-index: 1;
        }
        
        .company-info p {
            margin: 3px 0;
            color: var(--fabreck-white);
        }
        
        .active-salesman {
            font-weight: bold;
            color: var(--fabreck-white);
            margin-top: 5px;
            display: block;
            font-size: 13px;
            background: var(--fabreck-red);
            padding: 4px 8px;
            border-radius: 20px;
            display: inline-block;
        }
        
        /* Estilos para o conteúdo principal */
        .main-content {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .card {
            background: var(--fabreck-white);
            border-radius: 16px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.05);
            padding: 20px;
            border: 1px solid rgba(0, 0, 0, 0.05);
            transition: transform 0.3s ease, box-shadow 0.3s ease, background 0.3s ease, color 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
        }
        
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 12px;
            border-bottom: 2px solid rgba(0, 71, 171, 0.1);
        }
        
        .card-title {
            font-size: 18px;
            color: var(--fabreck-blue);
            font-weight: 700;
            letter-spacing: 0.3px;
        }
        
        .card-icon {
            font-size: 24px;
            color: var(--fabreck-white);
            background: var(--fabreck-red);
            width: 40px;
            height: 40px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 3px 8px rgba(210, 38, 48, 0.3);
        }
        
        /* Estilos de formulário */
        .form-group {
            margin-bottom: 15px;
            position: relative;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: var(--fabreck-blue);
            font-size: 14px;
            transition: color 0.3s ease;
        }
        
        .form-control {
            width: 100%;
            padding: 14px 16px;
            border: 1px solid rgba(0, 0, 0, 0.1);
            border-radius: 12px;
            font-size: 16px;
            background: var(--fabreck-white);
            color: var(--fabreck-dark);
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.05);
            transition: border 0.3s, box-shadow 0.3s, background 0.3s ease, color 0.3s ease;
        }

        #clientName, #salesmanName, #editClientName, #editSalesmanName, #laudoClientName, #usernameInput {
            text-transform: uppercase;
        }
        
        .form-control:focus {
            border-color: var(--fabreck-blue);
            outline: none;
            box-shadow: 0 0 0 3px rgba(0, 71, 171, 0.2);
        }
        
        .form-row {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
        }
        
        /* Estilos dos botões */
        .btn {
            padding: 14px;
            border: none;
            border-radius: 12px;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            margin-top: 10px;
            width: 100%;
            font-size: 16px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            transition: all 0.2s;
        }

        .btn:disabled {
            background-color: var(--fabreck-gray);
            cursor: not-allowed;
            opacity: 0.7;
        }
        
        .btn:active {
            transform: translateY(2px);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        
        .btn-primary {
            background: var(--fabreck-blue);
            color: var(--fabreck-white);
        }
        
        .btn-success {
            background: var(--fabreck-success);
            color: var(--fabreck-white);
        }
        
        .btn-warning {
            background: var(--fabreck-warning);
            color: var(--fabreck-white);
        }
        
        .btn-danger {
            background: var(--fabreck-danger);
            color: var(--fabreck-white);
        }
        
        .btn-info {
            background: #3498DB;
            color: var(--fabreck-white);
        }
        
        /* Estilos da câmera */
        .camera-container {
            position: relative;
            width: 100%;
            height: 250px;
            background: #000;
            border-radius: 12px;
            overflow: hidden;
            margin-bottom: 15px;
            border: 2px solid var(--fabreck-blue);
        }
        
        #video {
            width: 100%;
            height: 100%;
            display: block;
            object-fit: cover;
        }
        
        .scanner-status {
            position: absolute;
            bottom: 10px;
            left: 10px;
            font-size: 14px;
            color: var(--fabreck-white);
            display: flex;
            align-items: center;
            gap: 8px;
            z-index: 10;
            background: rgba(0, 0, 0, 0.5);
            padding: 6px 12px;
            border-radius: 20px;
        }
        
        .scanner-status-indicator {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: var(--fabreck-danger);
        }
        
        .scanner-status-indicator.active {
            background: var(--fabreck-success);
            box-shadow: 0 0 8px var(--fabreck-success);
        }
        
        .camera-controls {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
        }
        
        /* Estilos da tabela de relatório */
        .table-container {
            overflow-x: auto;
            margin-top: 15px;
            max-height: 500px;
            overflow-y: auto;
            border: 1px solid rgba(0, 0, 0, 0.1);
            border-radius: 12px;
            padding: 3px;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 14px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid rgba(0, 0, 0, 0.08);
            white-space: nowrap; 
            overflow: hidden;    
            text-overflow: ellipsis; 
        }

        th:first-child, td:first-child {
            width: 40px;
            text-align: center;
        }
        
        th {
            background: rgba(0, 71, 171, 0.1);
            color: var(--fabreck-blue);
            font-weight: 700;
            position: sticky;
            top: 0;
            font-size: 13px;
        }
        
        tr:nth-child(even) {
            background: rgba(0, 71, 171, 0.03);
        }
        
        /* Badges de status */
        .status-badge {
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 700;
            display: inline-block;
            text-align: center;
        }
        
        .status-warranty {
            background: rgba(46, 204, 113, 0.15);
            color: var(--fabreck-success);
            border: 1px solid rgba(46, 204, 113, 0.3);
        }
        
        .status-expired {
            background: rgba(231, 76, 60, 0.15);
            color: var(--fabreck-danger);
            border: 1px solid rgba(231, 76, 60, 0.3);
        }
        
        .status-factory {
            background: rgba(52, 152, 219, 0.15);
            color: #3498DB;
            border: 1px solid rgba(52, 152, 219, 0.3);
        }
        
        .status-analyzed {
            background: rgba(155, 89, 182, 0.15);
            color: #9B59B6;
            border: 1px solid rgba(155, 89, 182, 0.3);
        }

        .status-in-analysis {
            background: rgba(243, 156, 18, 0.15);
            color: var(--fabreck-warning);
            border: 1px solid rgba(243, 156, 18, 0.3);
        }

        .status-finalized {
            background: rgba(142, 68, 173, 0.15); 
            color: #8E44AD;
            border: 1px solid rgba(142, 68, 173, 0.3);
        }

        /* Badges para Ação Final */
        .action-approved {
            background: rgba(46, 204, 113, 0.2);
            color: var(--fabreck-success);
            border: 1px solid rgba(46, 204, 113, 0.4);
        }
        .action-rejected {
            background: rgba(231, 76, 60, 0.2);
            color: var(--fabreck-danger);
            border: 1px solid rgba(231, 76, 60, 0.4);
        }
        .action-scrapped {
            background: rgba(127, 140, 141, 0.2);
            color: var(--fabreck-gray);
            border: 1px solid rgba(127, 140, 141, 0.4);
        }
        
        /* Cartões de estatísticas */
        .stat-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .stat-card {
            background: var(--fabreck-white);
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            border: 1px solid rgba(0, 0, 0, 0.05);
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.03);
            transition: background 0.3s ease, color 0.3s ease;
        }
        
        .stat-title {
            font-size: 12px;
            color: var(--fabreck-blue);
            margin-bottom: 5px;
            font-weight: 600;
        }
        
        .stat-value {
            font-size: 24px;
            font-weight: 800;
            margin: 5px 0;
            color: var(--fabreck-blue);
        }
        
        /* Footer de navegação */
        .footer {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: var(--fabreck-white);
            display: flex;
            justify-content: space-around;
            padding: 10px 5px;
            border-top: 1px solid rgba(0, 0, 0, 0.08);
            z-index: 100;
            box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.05);
            transition: background 0.3s ease, border-top 0.3s ease, box-shadow 0.3s ease;
        }
        
        .nav-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 8px;
            color: var(--fabreck-gray);
            font-size: 11px;
            text-align: center;
            border-radius: 12px;
            transition: all 0.3s;
            flex-basis: 0;
            flex-grow: 1;
        }
        
        .nav-btn.active {
            color: var(--fabreck-blue);
            background: rgba(0, 71, 171, 0.1);
            transform: translateY(-5px);
        }
        
        .nav-btn i {
            font-size: 20px;
            margin-bottom: 3px;
        }
        
        /* Notificações */
        .notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: auto;
            min-width: 300px;
            max-width: 90%;
            padding: 15px 20px;
            border-radius: 12px;
            color: var(--fabreck-white);
            font-weight: 600;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
            z-index: 2100;
            transform: translate(-50%, -120px);
            transition: transform 0.4s ease;
            text-align: center;
        }
        
        .notification.show {
            transform: translate(-50%, 0);
        }
        
        .notification.success {
            background: var(--fabreck-success);
            border-left: 5px solid rgba(0, 0, 0, 0.1);
        }
        
        .notification.error {
            background: var(--fabreck-danger);
            border-left: 5px solid rgba(0, 0, 0, 0.1);
        }
        
        .notification.info {
            background: var(--fabreck-blue);
            border-left: 5px solid rgba(0, 0, 0, 0.1);
        }
        
        /* Seções de informação */
        .info-section {
            background: rgba(0, 71, 171, 0.05);
            padding: 15px;
            border-radius: 12px;
            margin-top: 15px;
            border: 1px solid rgba(0, 71, 171, 0.1);
            font-size: 14px;
            transition: background 0.3s ease, border 0.3s ease, color 0.3s ease;
        }
        
        /* Filtros de relatório */
        .report-filters {
            background: rgba(0, 71, 171, 0.05);
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 15px;
            border: 1px solid rgba(0, 71, 171, 0.1);
            transition: background 0.3s ease, border 0.3s ease;
        }
        
        .filter-row {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        .filter-group {
            margin-bottom: 8px;
        }
        
        .filter-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-top: 8px;
        }
        
        /* Controle de páginas */
        .page {
            display: none;
        }
        
        .page.active {
            display: block;
        }
        
        .hidden {
            display: none;
        }
        
        /* Indicador de câmera */
        .camera-indicator {
            position: absolute;
            top: 10px;
            right: 10px;
            background: rgba(0, 0, 0, 0.5);
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
        }
        
        /* Pré-visualização do código */
        .code-preview {
            font-size: 20px;
            text-align: center;
            font-weight: bold;
            margin-top: 10px;
            letter-spacing: 2px;
            color: var(--fabreck-blue);
            font-family: monospace;
            transition: color 0.3s ease;
        }
        
        /* Histórico de escaneamentos */
        .scanned-history {
            max-height: 150px;
            overflow-y: auto;
            margin-top: 15px;
            border: 1px solid rgba(0, 71, 171, 0.1);
            border-radius: 12px;
            padding: 10px;
            background: rgba(0, 71, 171, 0.03);
            transition: background 0.3s ease, border 0.3s ease;
        }
        
        .scanned-item {
            padding: 10px;
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
            font-size: 14px;
            display: flex;
            justify-content: space-between;
            border-radius: 8px;
            transition: background 0.2s, border-bottom 0.3s ease;
        }
        
        .scanned-item:hover {
            background: rgba(0, 71, 171, 0.05);
        }
        
        .scanned-item:last-child {
            border-bottom: none;
        }
        
        .scanned-code {
            font-weight: bold;
            color: var(--fabreck-blue);
            font-family: monospace;
        }
        
        .scanned-time {
            color: var(--fabreck-gray);
            font-size: 12px;
        }
        
        /* Ícone de ajuda */
        .help-icon {
            position: absolute;
            right: 10px;
            top: 35px;
            color: var(--fabreck-blue);
            cursor: pointer;
            background: rgba(0, 71, 171, 0.1);
            width: 24px;
            height: 24px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        /* Modal de regras */
        .rules-modal, .edit-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s;
        }
        
        .rules-modal.active, .edit-modal.active {
            opacity: 1;
            pointer-events: all;
        }
        
        .rules-content, .edit-content {
            background: var(--fabreck-white);
            border-radius: 16px;
            padding: 25px;
            width: 90%;
            max-width: 500px;
            border: 2px solid var(--fabreck-blue);
            position: relative;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            transition: background 0.3s ease, border 0.3s ease;
        }
        
        .rules-close, .edit-close {
            position: absolute;
            top: 15px;
            right: 15px;
            color: var(--fabreck-red);
            font-size: 24px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .rules-close:hover, .edit-close:hover {
            transform: scale(1.1);
        }
        
        .rules-title, .edit-title {
            color: var(--fabreck-blue);
            margin-bottom: 15px;
            text-align: center;
            font-size: 20px;
            font-weight: 700;
        }
        
        .rules-list {
            padding-left: 20px;
            margin-bottom: 20px;
        }
        
        .rules-list li {
            margin-bottom: 12px;
            line-height: 1.5;
        }
        
        .rules-highlight {
            background: rgba(0, 71, 171, 0.1);
            padding: 2px 8px;
            border-radius: 4px;
            font-weight: bold;
            color: var(--fabreck-blue);
            border: 1px solid rgba(0, 71, 171, 0.2);
            transition: background 0.3s ease, border 0.3s ease, color 0.3s ease;
        }
        
        /* Opções de tipo de garantia */
        .warranty-type {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 15px;
        }
        
        .warranty-option {
            display: flex;
            align-items: center;
            background: rgba(0, 71, 171, 0.05);
            padding: 12px 15px;
            border-radius: 12px;
            border: 1px solid rgba(0, 71, 171, 0.1);
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .warranty-option.selected {
            background: var(--fabreck-blue);
            color: var(--fabreck-white);
            border-color: var(--fabreck-blue);
        }
        
        .warranty-option input {
            margin-right: 10px;
        }

        /* Campo persistente (salvo automaticamente) */
        .persisted-field {
            position: relative;
        }

        .persisted-field::after {
            content: 'Salvo';
            position: absolute;
            right: 10px;
            top: 40px;
            font-size: 10px;
            color: var(--fabreck-success);
            background: rgba(46, 204, 113, 0.1);
            padding: 2px 5px;
            border-radius: 10px;
        }
        
        /* Campo de parecer técnico */
        #recommendationField {
            display: none; /* Hide by default */
        }
        
        /* Linha de input de código */
        .code-input-row {
            display: flex;
            gap: 10px;
        }
        
        .code-input-row .form-control {
            flex: 1;
        }
        
        .code-input-row .btn {
            width: auto;
            flex: 0 0 auto;
            margin-top: 0;
            padding: 14px 20px;
        }
        
        /* Footer da tabela */
        .table-footer {
            font-weight: bold;
            background: rgba(0, 71, 171, 0.1);
        }
        
        .table-footer td {
            padding: 10px 15px;
            border-top: 2px solid var(--fabreck-blue);
        }
        
        .table-footer .total-label {
            text-align: right;
            padding-right: 20px;
        }
        
        /* Log de atividades */
        .activity-log {
            margin-top: 15px;
            max-height: 200px;
            overflow-y: auto;
            border: 1px solid rgba(0, 0, 0, 0.05);
            border-radius: 12px;
            padding: 10px;
            background: var(--fabreck-white);
            transition: background 0.3s ease, border 0.3s ease;
        }
        
        .activity-item {
            padding: 12px;
            border-bottom: 1px dashed rgba(0, 0, 0, 0.08);
            display: flex;
            gap: 10px;
            align-items: center;
            border-radius: 8px;
            transition: background 0.2s, border-bottom 0.3s ease;
        }
        
        .activity-item:hover {
            background: rgba(0, 71, 171, 0.03);
        }
        
        .activity-icon {
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            background: rgba(0, 71, 171, 0.1);
            color: var(--fabreck-blue);
            flex-shrink: 0;
        }
        
        .activity-content {
            flex: 1;
            font-size: 14px;
        }
        
        .activity-time {
            color: var(--fabreck-gray);
            font-size: 12px;
            white-space: nowrap;
        }

        /* Overlay de captura da câmera */
        .capture-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 5;
            cursor: pointer;
        }
        
        .capture-indicator {
            width: 70px;
            height: 70px;
            border: 5px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(210, 38, 48, 0.7);
            transition: transform 0.2s;
        }
        
        .capture-indicator:active {
            transform: scale(0.9);
        }
        
        .capture-indicator i {
            font-size: 30px;
            color: white;
        }
        
        .scanner-guide {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            height: 80px;
            border: 3px dashed rgba(255, 255, 255, 0.5);
            border-radius: 10px;
            z-index: 2;
            pointer-events: none;
        }
        
        /* Estilos para os totais do relatório */
        .totals-container {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        .total-box {
            flex: 1;
            min-width: 150px;
            background: var(--fabreck-white);
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
            border: 1px solid rgba(0, 0, 0, 0.08);
            transition: background 0.3s ease, border 0.3s ease, color 0.3s ease;
        }
        
        .total-title {
            font-size: 14px;
            font-weight: 600;
            color: var(--fabreck-blue);
            margin-bottom: 5px;
        }
        
        .total-value {
            font-size: 24px;
            font-weight: 800;
        }
        
        .total-warranty {
            color: var(--fabreck-success);
        }
        
        .total-expired {
            color: var(--fabreck-danger);
        }
        
        .total-all {
            color: var(--fabreck-blue);
        }

        /* Estilos para o modal de edição */
        .edit-modal .form-group {
            margin-bottom: 10px;
        }

        .edit-modal .warranty-option {
            margin-bottom: 5px;
        }

        /* Informações da bateria no modal de análise */
        .analysis-info {
            background: rgba(0, 71, 171, 0.05);
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 15px;
            border: 1px solid rgba(0, 71, 171, 0.1);
            font-size: 14px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        .analysis-info p {
            margin: 0;
        }

        .analysis-info p strong {
            color: var(--fabreck-blue);
            display: block;
            font-size: 12px;
        }
        
        /* --- ESTILOS PARA LAUDO TÉCNICO --- */
        #laudoResultContainer {
            margin-top: 20px;
            background-color: var(--fabreck-light);
            border: 1px solid rgba(0, 71, 171, 0.2);
            border-radius: 12px;
            padding: 20px;
        }
        
        body.dark-mode #laudoResultContainer {
             background-color: var(--dark-bg);
             border-color: var(--dark-border);
        }
        
        #laudoResult {
            white-space: pre-wrap; /* Mantém a formatação do texto */
            font-family: monospace;
            font-size: 14px;
            line-height: 1.6;
            color: var(--fabreck-dark);
            background-color: transparent;
            border: none;
            width: 100%;
            min-height: 200px;
        }
        
        body.dark-mode #laudoResult {
            color: var(--dark-text);
        }

        .laudo-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .laudo-title {
            font-size: 18px;
            color: var(--fabreck-blue);
            font-weight: 700;
        }

        body.dark-mode .laudo-title {
            color: var(--dark-blue-light);
        }

        /* --- NOVOS ESTILOS PARA ASSISTENTE DE IA --- */
        .ai-assistant-fab {
            position: fixed;
            bottom: 90px;
            right: 20px;
            width: 60px;
            height: 60px;
            background-color: var(--fabreck-blue);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 28px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            cursor: pointer;
            z-index: 1050;
            transition: transform 0.2s ease;
        }

        .ai-assistant-fab:hover {
            transform: scale(1.1);
        }

        .ai-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2050;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .ai-modal.active {
            opacity: 1;
            pointer-events: all;
        }

        .ai-modal-content {
            width: 90%;
            max-width: 500px;
            height: 70vh;
            background: var(--fabreck-white);
            border-radius: 16px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        
        body.dark-mode .ai-modal-content {
             background: var(--dark-card-bg);
        }

        .ai-modal-header {
            padding: 15px 20px;
            border-bottom: 1px solid rgba(0,0,0,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        body.dark-mode .ai-modal-header {
            border-bottom: 1px solid var(--dark-border);
        }

        .ai-modal-title {
            color: var(--fabreck-blue);
            font-size: 18px;
            font-weight: 700;
        }
        
        body.dark-mode .ai-modal-title {
            color: var(--dark-blue-light);
        }

        .ai-modal-close {
            font-size: 24px;
            color: var(--fabreck-gray);
            cursor: pointer;
        }
        
        .ai-chat-box {
            flex-grow: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .ai-chat-message {
            padding: 10px 15px;
            border-radius: 12px;
            max-width: 80%;
            line-height: 1.5;
        }
        
        .ai-chat-message.user {
            background-color: var(--fabreck-blue);
            color: white;
            align-self: flex-end;
            border-bottom-right-radius: 4px;
        }
        
        body.dark-mode .ai-chat-message.user {
            background-color: var(--dark-blue-light);
        }
        
        .ai-chat-message.assistant {
            background-color: var(--fabreck-light);
            color: var(--fabreck-dark);
            align-self: flex-start;
            border-bottom-left-radius: 4px;
        }
        
        body.dark-mode .ai-chat-message.assistant {
            background-color: var(--dark-bg);
            color: var(--dark-text);
        }
        
        .ai-chat-message.assistant.thinking {
            font-style: italic;
        }
        
        .ai-chat-message.assistant.thinking .dot {
            display: inline-block;
            animation: aiblink 1.4s infinite;
        }
        .ai-chat-message.assistant.thinking .dot:nth-child(2) { animation-delay: 0.2s; }
        .ai-chat-message.assistant.thinking .dot:nth-child(3) { animation-delay: 0.4s; }

        @keyframes aiblink {
            0%, 80%, 100% { opacity: 0; }
            40% { opacity: 1; }
        }
        
        .ai-modal-footer {
            padding: 15px;
            border-top: 1px solid rgba(0,0,0,0.1);
            display: flex;
            gap: 10px;
        }
        
        body.dark-mode .ai-modal-footer {
            border-top: 1px solid var(--dark-border);
        }

        #ai-chat-input {
            flex-grow: 1;
            border-radius: 20px;
            padding: 10px 15px;
        }
        
        #ai-send-btn {
            width: 50px;
            height: 40px;
            margin-top: 0;
            border-radius: 20px;
            padding: 0;
        }
        /* --- FIM DOS ESTILOS DE IA --- */

        /* --- NOVOS ESTILOS PARA ANÁLISE EM LOTE --- */
        .batch-actions {
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 15px;
            border: 1px solid rgba(0, 71, 171, 0.1);
            background: rgba(0, 71, 171, 0.05);
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        @media (min-width: 600px) {
            .batch-actions {
                flex-direction: row;
                align-items: center;
                justify-content: space-between;
            }
            .batch-actions .form-group {
                flex-grow: 1;
                margin-bottom: 0;
            }
            .batch-actions .btn {
                width: auto;
                margin-top: 0;
            }
        }
        /* --- FIM DOS ESTILOS DE LOTE --- */

        /* Media query para tablets (a partir de 600px) */
        @media (min-width: 600px) {
            .form-row {
                grid-template-columns: 1fr 1fr;
            }
            .filter-row {
                grid-template-columns: 1fr 1fr;
            }
            #finalizadoPage .stat-cards {
                grid-template-columns: repeat(4, 1fr);
            }
        }

        /* Media query para telas de desktop (a partir de 992px) - MODO WEB */
        @media (min-width: 992px) {
            #sidebar {
                display: flex;
            }
            #mainContentWrapper {
                padding: 20px;
            }
            .container {
                max-width: 1200px;
                padding-bottom: 20px; /* Reduz padding do footer em desktop */
            }
            header {
                position: static; /* Header normal no fluxo da página */
                top: auto;
            }
            .footer {
                display: none; /* Esconde o footer de navegação mobile */
            }
            .ai-assistant-fab {
                bottom: 20px; /* Ajusta posição do FAB sem o footer */
            }
            .card {
                padding: 25px;
            }
            .card-title {
                font-size: 20px;
            }
            .form-control {
                padding: 16px 18px;
                font-size: 17px;
            }
            .btn {
                padding: 16px;
                font-size: 17px;
            }
            table th, table td {
                font-size: 15px;
                padding: 14px 18px;
            }
            .total-box {
                min-width: 200px;
            }
            .filter-row {
                grid-template-columns: repeat(3, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="login-overlay" id="loginOverlay">
        <div class="login-card">
            <img id="logo-img-login" alt="Fabreck Logo" class="logo-img">
            <h2>Acesso Restrito</h2>
            <form id="loginForm">
                <div class="form-group">
                    <label for="usernameInput">Utilizador</label>
                    <input type="text" id="usernameInput" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="passwordInput">Palavra-passe</label>
                    <input type="password" id="passwordInput" class="form-control" required>
                </div>
                <button type="submit" class="btn btn-primary">Entrar</button>
            </form>
        </div>
    </div>

    <div id="layoutContainer" class="hidden">
        <!-- Sidebar para Desktop -->
        <nav id="sidebar">
            <div class="sidebar-header">
                <img id="logo-img-sidebar" alt="Fabreck Logo" class="logo-img">
                <h3>Garantia IA</h3>
            </div>
            <div class="sidebar-nav">
                <a href="#" class="sidebar-btn active" data-page="scanPage"><i class="fas fa-bolt"></i><span>Rápido</span></a>
                <a href="#" class="sidebar-btn" data-page="laudoPage"><i class="fas fa-file-invoice"></i><span>Laudo</span></a>
                <a href="#" class="sidebar-btn" data-page="analysisPage"><i class="fas fa-clipboard-check"></i><span>Análise</span></a>
                <a href="#" class="sidebar-btn" data-page="finalizadoPage"><i class="fas fa-check-double"></i><span>Finalizadas</span></a>
                <a href="#" class="sidebar-btn" data-page="reportPage"><i class="fas fa-chart-bar"></i><span>Geral</span></a>
                <a href="#" class="sidebar-btn" data-page="settingsPage"><i class="fas fa-cog"></i><span>Ajustes</span></a>
            </div>
            <div class="sidebar-footer">
                <p>&copy; 2024 FABRECK DO BRASIL</p>
            </div>
        </nav>

        <!-- Conteúdo Principal -->
        <div id="mainContentWrapper">
            <div class="fabreck-pattern"></div>
            
            <div class="container">
                <header>
                    <div class="logo">
                        <img id="logo-img-header" alt="Fabreck Logo" class="logo-img">
                    </div>
                    <div class="system-title">Sistema de Controle de Garantia com IA</div>
                    
                    <div class="company-info">
                        <p><strong>Encarregado de Produção: Reginaldo</strong></p>
                        <p>Análise da Garantia: Jenilton Cruz</p>
                        <p class="active-salesman">Vendedor Ativo: <span id="currentSalesman">N/A</span></p>
                    </div>
                    <div id="syncStatus" style="color: white; font-size: 12px; margin-top: 8px; opacity: 0.8; font-weight: 600;">A ligar...</div>

                    <div style="display: flex; justify-content: center; gap: 10px; margin-top: 10px;">
                        <button id="toggleDarkModeBtn" class="btn btn-info" style="width: auto;">
                            <i class="fas fa-moon"></i> <span id="darkModeText">Modo Escuro</span>
                        </button>
                        <button id="logoutBtn" class="btn btn-danger" style="width: auto;">
                            <i class="fas fa-sign-out-alt"></i> Sair
                        </button>
                    </div>
                </header>
            
                <!-- Página de Registro Rápido -->
                <div id="scanPage" class="page active">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Registro Rápido</h2>
                            <div class="card-icon"><i class="fas fa-bolt"></i></div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group autocomplete-container">
                                <label for="clientName">Nome do Cliente</label>
                                <input type="text" id="clientName" class="form-control" placeholder="Nome completo" autocomplete="off">
                            </div>
                            
                            <div class="form-group autocomplete-container">
                                <label for="salesmanName">Nome do Vendedor</label>
                                <input type="text" id="salesmanName" class="form-control" placeholder="Nome do vendedor" autocomplete="off">
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="submissionDateInput">Data do Envio</label>
                            <input type="date" id="submissionDateInput" class="form-control">
                        </div>

                        <div class="warranty-type">
                            <div class="warranty-option" id="factoryOption">
                                <input type="radio" id="factoryRadio" name="warrantyType" value="factory" style="display:none;">
                                <label for="factoryRadio" style="width:100%; cursor:pointer;">Garantia p/ Fábrica</label>
                            </div>
                            
                            <div class="warranty-option" id="analyzedOption">
                                <input type="radio" id="analyzedRadio" name="warrantyType" value="analyzed" style="display:none;">
                                <label for="analyzedRadio" style="width:100%; cursor:pointer;">Análise por Vídeo</label>
                            </div>
                        </div>
                        
                        <div id="videoAnalysisOptions" class="hidden">
                            <div class="form-group">
                                <label>Parecer Rápido (Análise por Vídeo)</label>
                                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                                    <button id="procedenteBtn" class="btn btn-success" style="margin-top:0;">Procedente</button>
                                    <button id="descarregadaBtn" class="btn btn-warning" style="margin-top:0;">Descarregada</button>
                                </div>
                            </div>
                             <div class="form-group">
                                <label for="recommendation">Parecer Técnico (Obrigatório)</label>
                                <textarea id="recommendation" class="form-control" placeholder="Clique em uma opção acima ou digite o parecer."></textarea>
                            </div>
                            <button id="addObservationBtn" class="btn btn-info" style="margin-top: -10px; margin-bottom: 10px;"><i class="fas fa-comment-alt"></i> Adicionar Observações</button>
                        </div>

                        <div class="form-group">
                            <label for="serialCode">Código de Série
                                <i class="fas fa-question-circle help-icon" id="helpBtn"></i>
                            </label>
                            <div class="code-input-row">
                                <div style="position: relative; flex: 1;">
                                    <input type="text" id="serialCode" class="form-control" placeholder="Ex: 3524A2623" maxlength="9">
                                </div>
                                <button id="addBtn" class="btn btn-primary">
                                    <i class="fas fa-plus-circle"></i> Adicionar
                                </button>
                            </div>
                            <div id="codePreview" class="code-preview"></div>
                            <div id="warrantyDebugInfo" class="info-section" style="margin-top: 10px; display: none;">
                                <p><strong>Informações de Cálculo da Garantia:</strong></p>
                                <p>Fabricação: <span id="debugManufDate"></span></p>
                                <p>Fim da Garantia: <span id="debugWarrantyEndDate"></span></p>
                                <p>Status Calculado: <span id="debugCalculatedStatus"></span></p>
                            </div>
                        </div>
                        
                        <div class="form-row" style="margin-top: 10px;">
                            <button id="scanBtn" class="btn btn-primary">
                                <i class="fas fa-camera"></i> Usar Câmera com IA
                            </button>
                            <button id="clearFormBtn" class="btn btn-danger">
                                <i class="fas fa-eraser"></i> Limpar Código
                            </button>
                        </div>
                        
                        <div class="activity-log" id="activityLog"></div>
                    </div>
                    
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Leitor por Câmera (IA)</h2>
                            <div class="card-icon"><i class="fas fa-robot"></i></div>
                        </div>
                        
                        <div class="camera-container">
                            <video id="video" autoplay playsinline></video>
                            <div class="scanner-guide"></div>
                            <div class="scanner-status">
                                <div class="scanner-status-indicator" id="scannerIndicator"></div>
                                <span id="scannerStatusText">Câmera desativada</span>
                            </div>
                            <div class="capture-overlay hidden" id="captureOverlay">
                                <div class="capture-indicator">
                                    <i class="fas fa-camera"></i>
                                </div>
                            </div>
                        </div>
                        
                        <div class="camera-controls">
                            <button id="switchCamera" class="btn" style="display: none; background: rgba(0,0,0,0.1); color: var(--fabreck-dark);">
                                <i class="fas fa-sync-alt"></i> Alternar
                            </button>
                            <button id="startCamera" class="btn btn-primary">
                                <i class="fas fa-play"></i> Iniciar
                            </button>
                            <button id="stopCamera" class="btn btn-danger" disabled>
                                <i class="fas fa-stop"></i> Parar
                            </button>
                        </div>
                        
                        <div class="scanned-history" id="scannedHistory"></div>
                    </div>
                </div>

                <!-- ***** NOVA PÁGINA DE LAUDO TÉCNICO ***** -->
                <div id="laudoPage" class="page">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Gerador de Laudo Técnico com IA</h2>
                            <div class="card-icon"><i class="fas fa-file-invoice"></i></div>
                        </div>
                        <div class="info-section">
                            <p>Preencha os dados abaixo para que a Inteligência Artificial gere um laudo técnico profissional e detalhado.</p>
                        </div>
                        <div class="form-row">
                            <div class="form-group">
                                <label for="laudoClientName">Nome do Cliente</label>
                                <input type="text" id="laudoClientName" class="form-control" placeholder="Nome completo do cliente">
                            </div>
                            <div class="form-group">
                                <label for="laudoBatteryCode">Código de Série da Bateria</label>
                                <input type="text" id="laudoBatteryCode" class="form-control" placeholder="Ex: 3524A2623" maxlength="9">
                            </div>
                        </div>
                         <div class="form-row">
                            <div class="form-group">
                                <label for="laudoBatteryModel">Modelo da Bateria</label>
                                <input type="text" id="laudoBatteryModel" class="form-control" placeholder="Ex: FA6AD">
                            </div>
                            <div class="form-group">
                                <label for="laudoCA">CA Medido (A)</label>
                                <input type="number" id="laudoCA" class="form-control" placeholder="Valor do teste de Cranking Amps">
                            </div>
                        </div>
                         <div class="form-row">
                             <div class="form-group">
                                <label for="laudoVoltage">Voltagem em Repouso (V)</label>
                                <input type="number" id="laudoVoltage" class="form-control" placeholder="Ex: 12.5">
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="laudoVisualInspection">Inspeção Visual</label>
                            <textarea id="laudoVisualInspection" class="form-control" rows="3" placeholder="Ex: Caixa estufada, polos oxidados, vazamento de solução..."></textarea>
                        </div>
                        <div class="form-group">
                            <label for="laudoTechnicianNotes">Observações Adicionais do Técnico</label>
                            <textarea id="laudoTechnicianNotes" class="form-control" rows="3" placeholder="Qualquer outra informação relevante sobre o teste."></textarea>
                        </div>
                        <button id="generateLaudoBtn" class="btn btn-primary"><i class="fas fa-cogs"></i> Gerar Laudo com IA</button>

                        <div id="laudoResultContainer" class="hidden">
                            <div class="laudo-header">
                                <h3 class="laudo-title">Laudo Técnico Gerado</h3>
                                <button id="copyLaudoBtn" class="btn btn-info" style="width:auto; margin-top:0;"><i class="fas fa-copy"></i> Copiar</button>
                            </div>
                            <textarea id="laudoResult" class="form-control" rows="15" readonly></textarea>
                        </div>
                    </div>
                </div>

                <!-- Página de Análise Técnica -->
                <div id="analysisPage" class="page">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Fila de Análise Técnica</h2>
                            <div class="card-icon"><i class="fas fa-clipboard-check"></i></div>
                        </div>
                        <div class="info-section">
                            <p>Baterias que precisam de um parecer técnico para finalizar o processo de garantia. Use o filtro e as caixas de seleção para analisar várias baterias de um mesmo cliente em lote.</p>
                        </div>
                        <div class="stat-cards">
                            <div class="stat-card">
                                <div class="stat-title">Aguardando Análise</div>
                                <div class="stat-value" id="inAnalysisCount">0</div>
                            </div>
                        </div>

                        <div class="batch-actions">
                            <div class="form-group">
                                <label for="analysisClientFilter">Agrupar por Cliente:</label>
                                <select id="analysisClientFilter" class="form-control"></select>
                            </div>
                            <button id="batchAnalyzeBtn" class="btn btn-success" disabled>
                                <i class="fas fa-layer-group"></i> Analisar Selecionados
                            </button>
                        </div>

                        <div class="table-container">
                            <table id="analysisTable">
                                <thead>
                                    <tr>
                                        <th><input type="checkbox" id="selectAllCheckbox"></th>
                                        <th>Código</th>
                                        <th>Cliente</th>
                                        <th>Modelo</th>
                                        <th>Data Envio</th>
                                        <th>Status Garantia</th>
                                        <th>Ação</th>
                                    </tr>
                                </thead>
                                <tbody id="analysisBody"></tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- ***** NOVA PÁGINA DE FINALIZADOS ***** -->
                <div id="finalizadoPage" class="page">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Garantias Finalizadas</h2>
                            <div class="card-icon"><i class="fas fa-check-double"></i></div>
                        </div>
                        <div class="info-section">
                            <p>Lista de todas as garantias que já foram concluídas, com um resumo dos resultados.</p>
                        </div>
                         <div class="stat-cards">
                            <div class="stat-card">
                                <div class="stat-title">Total Finalizadas</div>
                                <div class="stat-value" id="finalizadoCount">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-title">Aprovadas</div>
                                <div class="stat-value" id="finalizadoAprovadaCount">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-title">Reprovadas (Prazo)</div>
                                <div class="stat-value" id="finalizadoReprovadaPrazoCount">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-title">Reprovadas (Fora)</div>
                                <div class="stat-value" id="finalizadoReprovadaForaCount">0</div>
                            </div>
                        </div>
                        <div class="table-container">
                            <table id="finalizadoTable">
                                <thead>
                                    <tr>
                                        <th>Código</th>
                                        <th>Cliente</th>
                                        <th>Data da Análise</th>
                                        <th>Ação Final</th>
                                        <th>Parecer</th>
                                    </tr>
                                </thead>
                                <tbody id="finalizadoBody"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
                
                <!-- Página de Relatórios -->
                <div id="reportPage" class="page">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Relatório Geral</h2>
                            <div class="card-icon"><i class="fas fa-chart-bar"></i></div>
                        </div>
                        <div class="info-section">
                            <p>Esta é a lista completa de <strong>todas</strong> as garantias registradas. Use os filtros abaixo para visualizar grupos específicos.</p>
                        </div>
                        <div class="stat-cards">
                            <div class="stat-card">
                                <div class="stat-title">Total Geral</div>
                                <div class="stat-value" id="totalCount">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-title">Em Análise</div>
                                <div class="stat-value" id="inAnalysisCountReport">0</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-title">Finalizadas</div>
                                <div class="stat-value" id="finalizedCountReport">0</div>
                            </div>
                        </div>
                        
                        <div class="report-filters">
                            <div class="filter-row">
                                <div class="filter-group">
                                    <label for="clientFilter">Filtrar por Cliente:</label>
                                    <select id="clientFilter" class="form-control">
                                        <option value="">Todos os Clientes</option>
                                    </select>
                                </div>
                                <div class="filter-group">
                                    <label for="statusFilter">Status da Garantia (Prazo):</label>
                                    <select id="statusFilter" class="form-control">
                                        <option value="">Todos</option>
                                        <option value="warranty">Em Garantia</option>
                                        <option value="expired">Fora do Prazo</option>
                                    </select>
                                </div>
                                <div class="filter-group">
                                    <label for="workflowStatusFilter">Status do Processo:</label>
                                    <select id="workflowStatusFilter" class="form-control">
                                        <option value="">Todos</option>
                                        <option value="in_analysis">Em Análise</option>
                                        <option value="finalized">Finalizado</option>
                                    </select>
                                </div>
                            </div>
                            
                            <div class="filter-row">
                                <div class="filter-group">
                                    <label for="codeFilter">Filtrar por Código:</label>
                                    <input type="text" id="codeFilter" class="form-control" placeholder="Código de série">
                                </div>
                                <div class="filter-group">
                                    <label for="startDate">Data Inicial (Registro):</label>
                                    <input type="date" id="startDate" class="form-control">
                                </div>
                                <div class="filter-group">
                                    <label for="endDate">Data Final (Registro):</label>
                                    <input type="date" id="endDate" class="form-control">
                                </div>
                            </div>
                            
                            <div class="filter-buttons">
                                <button id="applyFilter" class="btn btn-primary">Aplicar Filtros</button>
                                <button id="clearFilter" class="btn btn-danger">Limpar Filtros</button>
                            </div>
                        </div>
                        
                        <div class="table-container">
                            <table id="reportTable">
                                <thead>
                                    <tr>
                                        <th>Código</th>
                                        <th>Modelo</th>
                                        <th>Cliente</th>
                                        <th>Vendedor</th>
                                        <th>Status Prazo</th>
                                        <th>Status Processo</th>
                                        <th>Ação Final</th>
                                        <th>Parecer Técnico</th>
                                        <th>Data Análise</th>
                                        <th>Ações</th>
                                    </tr>
                                </thead>
                                <tbody id="reportBody"></tbody>
                            </table>
                        </div>
                        
                        <div class="totals-container" id="totalsContainer"></div>

                        <div class="card" id="modelStatusSummaryCard" style="display:none; margin-top: 20px;">
                            <div class="card-header">
                                <h2 class="card-title">Resumo de Ações por Modelo (com base no filtro)</h2>
                                <div class="card-icon"><i class="fas fa-tasks"></i></div>
                            </div>
                            <div id="modelStatusSummaryBody" class="info-section" style="margin-top:0;">
                            </div>
                        </div>
                        
                        <div class="action-buttons" style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 20px;">
                            <button id="previewPdfBtn" class="btn btn-info"><i class="fas fa-eye"></i> Visualizar PDF Único</button>
                            <button id="pdfBtn" class="btn btn-primary"><i class="fas fa-file-pdf"></i> Baixar PDF Único</button>
                            <button id="batchPdfBtn" class="btn btn-success"><i class="fas fa-file-archive"></i> Baixar Todos (PDF por Cliente)</button>
                            <button id="excelBtn" class="btn btn-primary"><i class="fas fa-file-excel"></i> Baixar Excel</button>
                            <button id="clearBtn" class="btn btn-danger"><i class="fas fa-trash-alt"></i> Limpar Tudo</button>
                        </div>
                    </div>
                </div>
                
                <!-- Página de Configurações -->
                <div id="settingsPage" class="page">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Banco de Dados</h2>
                            <div class="card-icon"><i class="fas fa-database"></i></div>
                        </div>
                        
                        <div class="form-row">
                            <button id="backupBtn" class="btn btn-primary"><i class="fas fa-download"></i> Exportar Dados (Backup Local)</button>
                            <button id="restoreBtn" class="btn btn-primary"><i class="fas fa-upload"></i> Importar Dados (Restaurar Backup)</button>
                        </div>
                        <input type="file" id="restoreFile" accept=".json" style="display: none;">

                        <div class="info-section">
                            <h3>Última Atualização Local</h3>
                            <p id="lastUpdate">Nenhuma atualização realizada</p>
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Instruções de Garantia</h2>
                            <div class="card-icon"><i class="fas fa-info-circle"></i></div>
                        </div>
                        <div id="warrantyInstructionsContainer" class="info-section" style="margin-top:0;">
                        </div>
                    </div>

                </div>
            </div>
            
            <div class="footer">
                <div class="nav-btn active" data-page="scanPage">
                    <div><i class="fas fa-bolt"></i></div>
                    <span>Rápido</span>
                </div>
                <div class="nav-btn" data-page="laudoPage">
                    <div><i class="fas fa-file-invoice"></i></div>
                    <span>Laudo</span>
                </div>
                <div class="nav-btn" data-page="analysisPage">
                    <div><i class="fas fa-clipboard-check"></i></div>
                    <span>Análise</span>
                </div>
                <div class="nav-btn" data-page="finalizadoPage">
                    <div><i class="fas fa-check-double"></i></div>
                    <span>Finalizadas</span>
                </div>
                <div class="nav-btn" data-page="reportPage">
                    <div><i class="fas fa-chart-bar"></i></div>
                    <span>Geral</span>
                </div>
                <div class="nav-btn" data-page="settingsPage">
                    <div><i class="fas fa-cog"></i></div>
                    <span>Ajustes</span>
                </div>
            </div>
        </div>
    </div>
    
    <div id="notification" class="notification"></div>
    
    <!-- Modal de Regras do Código de Série -->
    <div class="rules-modal" id="rulesModal">
        <div class="rules-content">
            <span class="rules-close" id="closeRulesModal">&times;</span>
            <h3 class="rules-title">Formato do Código de Série</h3>
            <ul class="rules-list">
                <li>O código deve ter exatamente <span class="rules-highlight">9 caracteres</span></li>
                <li>Os primeiros 4 caracteres devem ser <span class="rules-highlight">números</span></li>
                <li>O quinto caractere deve ser uma <span class="rules-highlight">letra</span></li>
                <li>Os últimos 4 caracteres devem ser <span class="rules-highlight">números</span></li>
                <li>Exemplo válido: <span class="rules-highlight">3524A2623</span></li>
            </ul>
            <button class="btn btn-primary" id="confirmRules" style="width: 100%;">Entendi</button>
        </div>
    </div>

    <!-- Modal de Análise Técnica (agora para single e batch) -->
    <div class="edit-modal" id="analysisModal">
        <div class="edit-content">
            <span class="edit-close" id="closeAnalysisModal">&times;</span>
            <h3 class="edit-title" id="analysisModalTitle">Analisar Bateria</h3>
            
            <div id="analysisSingleInfo">
                <div class="analysis-info">
                    <p><strong>Cliente:</strong> <span id="analysisClientName"></span></p>
                    <p><strong>Vendedor:</strong> <span id="analysisSalesmanName"></span></p>
                    <p><strong>Modelo:</strong> <span id="analysisBatteryModel"></span></p>
                    <p><strong>Status Garantia:</strong> <span id="analysisWarrantyStatus"></span></p>
                </div>
            </div>

            <div class="form-group">
                <label for="analysisRecommendation">Parecer Técnico Final</label>
                <textarea id="analysisRecommendation" class="form-control" placeholder="Ex: Capacidade baixa, teste de CCA falhou..."></textarea>
            </div>

            <div class="form-group">
                <label for="analysisFinalAction">Ação Final</label>
                <select id="analysisFinalAction" class="form-control">
                    <option value="">-- Selecione uma ação --</option>
                    <option value="APROVADA - ENVIAR NOVA">APROVADA - ENVIAR NOVA</option>
                    <option value="REPROVADA - DEVOLVER AO CLIENTE">REPROVADA - DEVOLVER AO CLIENTE</option>
                    <option value="REPROVADA - SUCATEAR">REPROVADA - SUCATEAR</option>
                </select>
            </div>
             <button id="analysisAddObservationBtn" class="btn btn-info" style="margin-top: -10px; margin-bottom: 10px;"><i class="fas fa-comment-alt"></i> Adicionar Observações</button>

            <button id="saveAnalysisBtn" class="btn btn-success">
                <i class="fas fa-save"></i> Salvar Análise e Finalizar
            </button>
        </div>
    </div>

    <!-- Modal para Editar Nomes -->
    <div class="edit-modal" id="nameEditModal">
        <div class="edit-content">
            <span class="edit-close" id="closeNameEditModal">&times;</span>
            <h3 class="edit-title">Editar Nomes</h3>
            <p style="text-align: center; font-size: 14px; margin-bottom: 15px;">A alteração será aplicada a todos os registros com estes nomes.</p>
            
            <div class="form-group">
                <label for="editClientName">Nome do Cliente</label>
                <input type="text" id="editClientName" class="form-control">
            </div>

            <div class="form-group">
                <label for="editSalesmanName">Nome do Vendedor</label>
                <input type="text" id="editSalesmanName" class="form-control">
            </div>

            <button id="saveNameEditBtn" class="btn btn-success">
                <i class="fas fa-save"></i> Salvar Alterações
            </button>
        </div>
    </div>

    <!-- Modal para Observações -->
    <div class="edit-modal" id="observationModal">
        <div class="edit-content">
            <span class="edit-close" id="closeObservationModal">&times;</span>
            <h3 class="edit-title">Observações para o Cliente</h3>
             <div class="form-group">
                <label for="observationText">Observação</label>
                <textarea id="observationText" class="form-control" rows="4" placeholder="Digite aqui informações adicionais para o cliente..."></textarea>
            </div>
            <button id="saveObservationBtn" class="btn btn-primary">
                <i class="fas fa-check"></i> Confirmar
            </button>
        </div>
    </div>

    <!-- Modal para Opções de 'Procedente' -->
    <div class="edit-modal" id="procedenteModal">
        <div class="edit-content">
            <span class="edit-close" id="closeProcedenteModal">&times;</span>
            <h3 class="edit-title">Motivo - Procedente</h3>
            <p style="text-align: center; font-size: 14px; margin-bottom: 15px;">Selecione o motivo pelo qual a garantia foi aprovada.</p>
            <div style="display: grid; grid-template-columns: 1fr; gap: 10px;">
                 <button id="capacidadeBaixaBtn" class="btn btn-success">Capacidade Baixa</button>
                 <button id="ccaBaixoBtn" class="btn btn-success">CCA Baixo</button>
            </div>
        </div>
    </div>

    <!-- Modal de Confirmação de Restauração -->
    <div class="edit-modal" id="restoreConfirmModal">
        <div class="edit-content">
            <h3 class="edit-title">Confirmar Restauração</h3>
            <p style="text-align: center; font-size: 14px; margin-bottom: 15px;">
                <strong>Atenção!</strong> Esta ação substituirá <strong>TODOS</strong> os dados atuais pelos dados do arquivo de backup. A operação não pode ser desfeita.
            </p>
            <p style="text-align: center; font-size: 14px; margin-bottom: 20px;">Deseja continuar?</p>
            <div style="display: flex; gap: 10px;">
                 <button id="cancelRestoreBtn" class="btn btn-info" style="margin-top:0;">Cancelar</button>
                 <button id="confirmRestoreBtn" class="btn btn-danger" style="margin-top:0;">Sim, Substituir Tudo</button>
            </div>
        </div>
    </div>

    <!-- Botão Flutuante do Assistente de IA -->
    <div class="ai-assistant-fab" id="aiAssistantFab">
        <i class="fas fa-robot"></i>
    </div>

    <!-- Modal do Assistente de IA -->
    <div class="ai-modal" id="aiModal">
        <div class="ai-modal-content">
            <div class="ai-modal-header">
                <h3 class="ai-modal-title">Assistente FABRECK DO BRASIL</h3>
                <span class="ai-modal-close" id="aiModalClose">&times;</span>
            </div>
            <div class="ai-chat-box" id="aiChatBox">
            </div>
            <div class="ai-modal-footer">
                <input type="text" id="aiChatInput" class="form-control" placeholder="Faça uma pergunta...">
                <button id="aiSendBtn" class="btn btn-primary"><i class="fas fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <div class="edit-modal" id="cloudLoadConfirmModal">
        <div class="edit-content">
            <h3 class="edit-title">Confirmar Carregamento</h3>
            <p style="text-align: center; font-size: 14px; margin-bottom: 15px;">
                <strong>Atenção!</strong> Esta ação substituirá <strong>TODOS</strong> os dados locais pelos dados salvos na nuvem. A operação não pode ser desfeita.
            </p>
            <p style="text-align: center; font-size: 14px; margin-bottom: 20px;">Deseja continuar?</p>
            <div style="display: flex; gap: 10px;">
                 <button id="cancelCloudLoadBtn" class="btn btn-info" style="margin-top:0;">Cancelar</button>
                 <button id="confirmCloudLoadBtn" class="btn btn-danger" style="margin-top:0;">Sim, Substituir Dados</button>
            </div>
        </div>
    </div>


    <script>
        // #region Elementos DOM
        const video = document.getElementById('video');
        const startCameraBtn = document.getElementById('startCamera');
        const stopCameraBtn = document.getElementById('stopCamera');
        const switchCameraBtn = document.getElementById('switchCamera');
        const clientNameInput = document.getElementById('clientName');
        const salesmanNameInput = document.getElementById('salesmanName');
        const serialCodeInput = document.getElementById('serialCode');
        const addBtn = document.getElementById('addBtn');
        const clearFormBtn = document.getElementById('clearFormBtn');
        const scanBtn = document.getElementById('scanBtn');
        const pdfBtn = document.getElementById('pdfBtn');
        const previewPdfBtn = document.getElementById('previewPdfBtn');
        const batchPdfBtn = document.getElementById('batchPdfBtn');
        const clearBtn = document.getElementById('clearBtn');
        const backupBtn = document.getElementById('backupBtn');
        const restoreBtn = document.getElementById('restoreBtn');
        const restoreFileInput = document.getElementById('restoreFile');
        const reportBody = document.getElementById('reportBody');
        const totalCount = document.getElementById('totalCount');
        const lastUpdate = document.getElementById('lastUpdate');
        const clientFilter = document.getElementById('clientFilter');
        const statusFilter = document.getElementById('statusFilter'); 
        const workflowStatusFilter = document.getElementById('workflowStatusFilter');
        const applyFilterBtn = document.getElementById('applyFilter');
        const clearFilterBtn = document.getElementById('clearFilter');
        const navBtns = document.querySelectorAll('.nav-btn');
        const sidebarBtns = document.querySelectorAll('.sidebar-btn');
        const pages = document.querySelectorAll('.page');
        const scannerIndicator = document.getElementById('scannerIndicator');
        const scannerStatusText = document.getElementById('scannerStatusText');
        const scannedHistory = document.getElementById('scannedHistory');
        const codePreview = document.getElementById('codePreview');
        const activityLog = document.getElementById('activityLog');
        const currentSalesman = document.getElementById('currentSalesman');
        const startDateInput = document.getElementById('startDate');
        const endDateInput = document.getElementById('endDate');
        const excelBtn = document.getElementById('excelBtn');
        const helpBtn = document.getElementById('helpBtn');
        const rulesModal = document.getElementById('rulesModal');
        const closeRulesModal = document.getElementById('closeRulesModal');
        const confirmRules = document.getElementById('confirmRules');
        const factoryRadio = document.getElementById('factoryRadio');
        const analyzedRadio = document.getElementById('analyzedRadio');
        const factoryOption = document.getElementById('factoryOption');
        const analyzedOption = document.getElementById('analyzedOption');
        const codeFilter = document.getElementById('codeFilter');
        const recommendationInput = document.getElementById('recommendation');
        const captureOverlay = document.getElementById('captureOverlay');
        const totalsContainer = document.getElementById('totalsContainer');
        const warrantyDebugInfo = document.getElementById('warrantyDebugInfo');
        const debugManufDate = document.getElementById('debugManufDate');
        const debugWarrantyEndDate = document.getElementById('debugWarrantyEndDate');
        const debugCalculatedStatus = document.getElementById('debugCalculatedStatus');
        const submissionDateInput = document.getElementById('submissionDateInput');
        const toggleDarkModeBtn = document.getElementById('toggleDarkModeBtn');
        const darkModeText = document.getElementById('darkModeText');
        const warrantyInstructionsContainer = document.getElementById('warrantyInstructionsContainer');

        // Elementos da página de Análise
        const analysisBody = document.getElementById('analysisBody');
        const inAnalysisCount = document.getElementById('inAnalysisCount');
        const analysisClientFilter = document.getElementById('analysisClientFilter');
        const selectAllCheckbox = document.getElementById('selectAllCheckbox');
        const batchAnalyzeBtn = document.getElementById('batchAnalyzeBtn');

        // Elementos da página Finalizado e Relatório
        const finalizadoBody = document.getElementById('finalizadoBody');
        const finalizadoCount = document.getElementById('finalizadoCount');
        const finalizedCountReport = document.getElementById('finalizedCountReport');
        const inAnalysisCountReport = document.getElementById('inAnalysisCountReport');
        const finalizadoAprovadaCount = document.getElementById('finalizadoAprovadaCount');
        const finalizadoReprovadaPrazoCount = document.getElementById('finalizadoReprovadaPrazoCount');
        const finalizadoReprovadaForaCount = document.getElementById('finalizadoReprovadaForaCount');


        // Elementos do Modal de Análise
        const analysisModal = document.getElementById('analysisModal');
        const closeAnalysisModal = document.getElementById('closeAnalysisModal');
        const analysisModalTitle = document.getElementById('analysisModalTitle');
        const analysisSingleInfo = document.getElementById('analysisSingleInfo');
        const analysisClientName = document.getElementById('analysisClientName');
        const analysisSalesmanName = document.getElementById('analysisSalesmanName');
        const analysisBatteryModel = document.getElementById('analysisBatteryModel');
        const analysisWarrantyStatus = document.getElementById('analysisWarrantyStatus');
        const analysisRecommendation = document.getElementById('analysisRecommendation');
        const analysisFinalAction = document.getElementById('analysisFinalAction');
        const saveAnalysisBtn = document.getElementById('saveAnalysisBtn');
        const analysisAddObservationBtn = document.getElementById('analysisAddObservationBtn');

        // Elementos do Modal de Edição de Nomes
        const nameEditModal = document.getElementById('nameEditModal');
        const closeNameEditModal = document.getElementById('closeNameEditModal');
        const editClientNameInput = document.getElementById('editClientName');
        const editSalesmanNameInput = document.getElementById('editSalesmanName');
        const saveNameEditBtn = document.getElementById('saveNameEditBtn');

        // Elementos do Modal de Observações
        const observationModal = document.getElementById('observationModal');
        const closeObservationModal = document.getElementById('closeObservationModal');
        const observationText = document.getElementById('observationText');
        const saveObservationBtn = document.getElementById('saveObservationBtn');

        // Elementos de Análise por Vídeo
        const videoAnalysisOptions = document.getElementById('videoAnalysisOptions');
        const procedenteBtn = document.getElementById('procedenteBtn');
        const descarregadaBtn = document.getElementById('descarregadaBtn');
        const addObservationBtn = document.getElementById('addObservationBtn');

        // Elementos do Modal Procedente
        const procedenteModal = document.getElementById('procedenteModal');
        const closeProcedenteModal = document.getElementById('closeProcedenteModal');
        const capacidadeBaixaBtn = document.getElementById('capacidadeBaixaBtn');
        const ccaBaixoBtn = document.getElementById('ccaBaixoBtn');

        // Elementos do Modal de Restauração
        const restoreConfirmModal = document.getElementById('restoreConfirmModal');
        const confirmRestoreBtn = document.getElementById('confirmRestoreBtn');
        const cancelRestoreBtn = document.getElementById('cancelRestoreBtn');

        // Elementos do Assistente de IA
        const aiAssistantFab = document.getElementById('aiAssistantFab');
        const aiModal = document.getElementById('aiModal');
        const aiModalClose = document.getElementById('aiModalClose');
        const aiChatBox = document.getElementById('aiChatBox');
        const aiChatInput = document.getElementById('aiChatInput');
        const aiSendBtn = document.getElementById('aiSendBtn');
        
        // Elementos do Laudo Técnico
        const generateLaudoBtn = document.getElementById('generateLaudoBtn');
        const laudoResultContainer = document.getElementById('laudoResultContainer');
        const laudoResult = document.getElementById('laudoResult');
        const copyLaudoBtn = document.getElementById('copyLaudoBtn');
        
        // Elementos do Login e Layout
        const loginOverlay = document.getElementById('loginOverlay');
        const layoutContainer = document.getElementById('layoutContainer');
        const loginForm = document.getElementById('loginForm');
        const usernameInput = document.getElementById('usernameInput');
        const passwordInput = document.getElementById('passwordInput');
        const logoutBtn = document.getElementById('logoutBtn');
        // #endregion

        // #region Estado do Sistema
        let stream = null;
        let scanning = false;
        const DB_KEY = 'fabreck_battery_db_v21'; // Versão antiga para migração
        const ACTIVITY_KEY = 'fabreck_activity_log_v12';
        const LAST_SALESMAN_KEY = 'fabreck_last_salesman_v12';
        const LAST_CLIENT_KEY = 'fabreck_last_client_v12';
        const LAST_WARRANTY_TYPE_KEY = 'fabreck_last_warranty_type_v12';
        const DARK_MODE_KEY = 'fabreck_dark_mode'; 
        const INSTRUCTIONS_KEY = 'fabreck_instructions_v1';
        let batteryData = [];
        let activityData = [];
        let currentFilters = {
            client: '',
            status: '',
            workflowStatus: '',
            startDate: '',
            endDate: '',
            code: ''
        };
        let warrantyInstructions = {};
        let currentFacingMode = 'environment';
        let scanHistory = [];
        let audioContext = null;
        let beepSound = null;
        let rulesShown = localStorage.getItem('rulesShown') === 'true';
        let analysisMode = 'single'; // 'single' or 'batch'
        let analyzingBatteryId = null; 
        let fileToRestore = null;
        
        let batchAnalysisIds = []; 

        let editingBatteryId = null;
        let observationCallback = null; // Callback para salvar observação
        let tempObservation = '';
        let isDarkMode = localStorage.getItem(DARK_MODE_KEY) === 'true';
        let aiChatHistory = [];
        let cloudDataToLoad = null; 
        let syncDebounceTimer = null;

        // Credenciais e Chave Fixa
        const ADMIN_USER = 'ADMIN';
        const ADMIN_PASS = 'FABRECK2024';
        const FIXED_SYNC_KEY = '1418850998876823552';
        
        const logoBase64 = 'data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNDAiIGhlaWdodD0iNDAiIHZpZXdCb3g9IjAgMCAyNDAgNDAiPjxyZWN0IHdpZHRoPSIyNDAiIGhlaWdodD0iNDAiIGZpbGw9IiMwMDQ3QUIiLz48dGV4dCB4PSIxMjAiIHk9IjI1IiBmb250LWZhbWlseT0iU2Vnb2UgVUksIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iMjAiIGZvbnQtd2VpZ-Gh0PSJib2xkIiBmaWxsPSIjRkZGRkZGIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5GQUJSRUNLIERPIEJSQVNJTDwvdGV4dD48L3N2Zz4=';

        const batteryModelMap = {
            'A': 'FA6AD', 'B': 'FA4D', 'C': 'FA5AD', 'D': 'FA5D', 'E': 'FA5,5D',
            'F': 'FA6D',
            'G': 'FA7E', 'H': 'FA8AE', 'I': 'FA8E', 'J': 'FA7AE',
            'K': 'FA7D'
        };
        
        let db;
        // #endregion

        // #region IndexedDB Helper
        const dbPromise = new Promise((resolve, reject) => {
            const request = indexedDB.open('FabreckDB', 1);

            request.onerror = (event) => {
                console.error("Erro ao abrir IndexedDB:", event.target.error);
                reject("Erro de base de dados");
            };

            request.onupgradeneeded = (event) => {
                const db = event.target.result;
                if (!db.objectStoreNames.contains('batteries')) {
                    db.createObjectStore('batteries', { keyPath: 'id' });
                }
                 if (!db.objectStoreNames.contains('activity')) {
                    db.createObjectStore('activity', { autoIncrement: true });
                }
                 if (!db.objectStoreNames.contains('settings')) {
                    db.createObjectStore('settings', { keyPath: 'key' });
                }
            };

            request.onsuccess = (event) => {
                console.log("Base de dados aberta com sucesso.");
                db = event.target.result;
                resolve(db);
            };
        });

        const dbManager = {
            async get(storeName, key) {
                const db = await dbPromise;
                return new Promise((resolve, reject) => {
                    const transaction = db.transaction(storeName, 'readonly');
                    const store = transaction.objectStore(storeName);
                    const request = store.get(key);
                    request.onsuccess = () => resolve(request.result);
                    request.onerror = (event) => reject(event.target.error);
                });
            },
            async getAll(storeName) {
                const db = await dbPromise;
                return new Promise((resolve, reject) => {
                    const transaction = db.transaction(storeName, 'readonly');
                    const store = transaction.objectStore(storeName);
                    const request = store.getAll();
                    request.onsuccess = () => resolve(request.result);
                    request.onerror = (event) => reject(event.target.error);
                });
            },
            async set(storeName, value) {
                const db = await dbPromise;
                return new Promise((resolve, reject) => {
                    const transaction = db.transaction(storeName, 'readwrite');
                    const store = transaction.objectStore(storeName);
                    const request = store.put(value);
                    request.onsuccess = () => resolve(request.result);
                    request.onerror = (event) => reject(event.target.error);
                    transaction.oncomplete = () => resolve();
                });
            },
            async delete(storeName, key) {
                const db = await dbPromise;
                 return new Promise((resolve, reject) => {
                    const transaction = db.transaction(storeName, 'readwrite');
                    const store = transaction.objectStore(storeName);
                    const request = store.delete(key);
                    request.onsuccess = () => resolve();
                    request.onerror = (event) => reject(event.target.error);
                });
            },
            async clear(storeName) {
                const db = await dbPromise;
                return new Promise((resolve, reject) => {
                    const transaction = db.transaction(storeName, 'readwrite');
                    const store = transaction.objectStore(storeName);
                    const request = store.clear();
                    request.onsuccess = () => resolve();
                    request.onerror = (event) => reject(event.target.error);
                });
            }
        };
        // #endregion

        // #region Inicialização e Autenticação
        async function init() {
            console.log("Inicializando sistema...");
            document.getElementById('logo-img-header').src = logoBase64;
            document.getElementById('logo-img-login').src = logoBase64;
            document.getElementById('logo-img-sidebar').src = logoBase64;

            setupEventListeners();
            await dbPromise; // Garante que a BD está pronta antes de qualquer operação
            checkAuth(); // Verifica o login e inicia o carregamento de dados
        }

        async function initializeAppData() {
            showNotification('Autenticado. A carregar dados da nuvem...', 'info');
            
            await loadFromCloud(true); // Carrega os dados silenciosamente da nuvem primeiro

            // A migração e o carregamento do DB local servem como fallback
            await migrateFromLocalStorage();
            await loadFromDB(); 

            // Atualiza a UI com os dados carregados (da nuvem ou locais)
            updateUI();
            updateLastUpdate();
            setupAudio();
            loadLastUsedData(); 
            updateScanHistory();
            applyDarkMode(); 

            const today = new Date();
            const year = today.getFullYear();
            const month = String(today.getMonth() + 1).padStart(2, '0');
            const day = String(today.getDate()).padStart(2, '0');
            submissionDateInput.value = `${year}-${month}-${day}`;
        }

        function checkAuth() {
            if (sessionStorage.getItem('isAuthenticated') === 'true') {
                loginOverlay.classList.add('hidden');
                layoutContainer.classList.remove('hidden');
                if (batteryData.length === 0) { // Só inicializa se os dados ainda não estiverem na memória
                     initializeAppData();
                }
            } else {
                loginOverlay.classList.remove('hidden');
                layoutContainer.classList.add('hidden');
            }
        }
        
        async function loadLastUsedData() {
            const lastSalesman = await dbManager.get('settings', LAST_SALESMAN_KEY);
            if (lastSalesman) {
                salesmanNameInput.value = lastSalesman.value;
                currentSalesman.textContent = lastSalesman.value;
            }
            
            const lastClient = await dbManager.get('settings', LAST_CLIENT_KEY);
            if (lastClient) {
                clientNameInput.value = lastClient.value;
            }
            
            const lastWarrantyType = await dbManager.get('settings', LAST_WARRANTY_TYPE_KEY);
            selectWarrantyType(lastWarrantyType ? lastWarrantyType.value : 'factory');
        }
        
        function setupEventListeners() {
            console.log("Configurando event listeners...");

            loginForm.addEventListener('submit', async (e) => {
                e.preventDefault();
                const user = usernameInput.value.trim().toUpperCase();
                const pass = passwordInput.value.trim();
                if (user === ADMIN_USER && pass === ADMIN_PASS) {
                    sessionStorage.setItem('isAuthenticated', 'true');
                    await initializeAppData();
                    checkAuth();
                } else {
                    showNotification('Utilizador ou palavra-passe incorretos.', 'error');
                }
            });

            logoutBtn.addEventListener('click', () => {
                sessionStorage.removeItem('isAuthenticated');
                checkAuth();
            });
            
            navBtns.forEach(btn => {
                btn.addEventListener('click', () => showPage(btn.getAttribute('data-page')));
            });

            sidebarBtns.forEach(btn => {
                btn.addEventListener('click', (e) => {
                    e.preventDefault();
                    showPage(btn.getAttribute('data-page'));
                });
            });
            
            serialCodeInput.addEventListener('input', handleSerialInput);
            serialCodeInput.addEventListener('keypress', e => { if (e.key === 'Enter') addBattery(); });
            salesmanNameInput.addEventListener('input', (e) => e.target.value = e.target.value.toUpperCase());
            clientNameInput.addEventListener('input', (e) => e.target.value = e.target.value.toUpperCase());
            salesmanNameInput.addEventListener('blur', async () => {
                const name = salesmanNameInput.value.trim().toUpperCase();
                if (name) {
                    currentSalesman.textContent = name;
                    await dbManager.set('settings', { key: LAST_SALESMAN_KEY, value: name });
                }
            });
            clientNameInput.addEventListener('blur', async () => {
                const name = clientNameInput.value.trim().toUpperCase();
                if (name) await dbManager.set('settings', { key: LAST_CLIENT_KEY, value: name });
            });
            scanBtn.addEventListener('click', handleScanClick);
            clearFormBtn.addEventListener('click', clearCode);
            addBtn.addEventListener('click', addBattery);
            
            factoryOption.addEventListener('click', () => selectWarrantyType('factory'));
            analyzedOption.addEventListener('click', () => selectWarrantyType('analyzed'));
            
            startCameraBtn.addEventListener('click', startCamera);
            stopCameraBtn.addEventListener('click', stopCamera);
            switchCameraBtn.addEventListener('click', switchCamera);
            captureOverlay.addEventListener('click', captureImage);
            
            applyFilterBtn.addEventListener('click', applyFilters);
            clearFilterBtn.addEventListener('click', clearFilters);
            previewPdfBtn.addEventListener('click', previewPDF);
            pdfBtn.addEventListener('click', downloadPDF);
            batchPdfBtn.addEventListener('click', generateBatchPDFs);
            excelBtn.addEventListener('click', exportToFormattedExcel);
            clearBtn.addEventListener('click', clearData);
            codeFilter.addEventListener('input', applyFilters);
            
            backupBtn.addEventListener('click', exportData);
            restoreBtn.addEventListener('click', () => restoreFileInput.click());

            restoreFileInput.addEventListener('change', (e) => {
                if (e.target.files.length > 0) {
                    fileToRestore = e.target.files[0];
                    restoreConfirmModal.classList.add('active');
                }
                e.target.value = ''; 
            });

            cancelRestoreBtn.addEventListener('click', () => {
                fileToRestore = null;
                restoreConfirmModal.classList.remove('active');
            });

            confirmRestoreBtn.addEventListener('click', () => {
                restoreConfirmModal.classList.remove('active');
                if (fileToRestore) {
                    handleRestoreFile(fileToRestore);
                }
            });
            
            helpBtn.addEventListener('click', showRulesModal);
            closeRulesModal.addEventListener('click', () => rulesModal.classList.remove('active'));
            confirmRules.addEventListener('click', () => rulesModal.classList.remove('active'));
            
            closeAnalysisModal.addEventListener('click', () => analysisModal.classList.remove('active'));
            saveAnalysisBtn.addEventListener('click', saveAnalysis);
            analysisClientFilter.addEventListener('change', updateAnalysisTable);
            selectAllCheckbox.addEventListener('change', handleSelectAll);
            batchAnalyzeBtn.addEventListener('click', openBatchAnalysisModal);

            closeNameEditModal.addEventListener('click', () => nameEditModal.classList.remove('active'));
            saveNameEditBtn.addEventListener('click', saveEditedNames);

            closeObservationModal.addEventListener('click', () => observationModal.classList.remove('active'));
            saveObservationBtn.addEventListener('click', () => {
                if (observationCallback) observationCallback(observationText.value);
            });

            procedenteBtn.addEventListener('click', () => procedenteModal.classList.add('active'));
            closeProcedenteModal.addEventListener('click', () => procedenteModal.classList.remove('active'));
            capacidadeBaixaBtn.addEventListener('click', () => {
                recommendationInput.value = 'PROCEDENTE - CAPACIDADE BAIXA';
                procedenteModal.classList.remove('active');
            });
            ccaBaixoBtn.addEventListener('click', () => {
                recommendationInput.value = 'PROCEDENTE - CCA BAIXO';
                procedenteModal.classList.remove('active');
            });

            descarregadaBtn.addEventListener('click', () => {
                recommendationInput.value = 'RECARREGAR A BATERIA POR 6H EM CARGA LENTA E REFAZER O TESTE';
            });
            addObservationBtn.addEventListener('click', () => openObservationModal(tempObservation, (obs) => {
                tempObservation = obs;
                observationModal.classList.remove('active');
                showNotification('Observação salva temporariamente.', 'info');
            }));
            analysisAddObservationBtn.addEventListener('click', async () => {
                 const battery = await dbManager.get('batteries', analyzingBatteryId);
                 if(battery) openObservationModal(battery.observations, async (obs) => {
                     battery.observations = obs;
                     await dbManager.set('batteries', battery);
                     observationModal.classList.remove('active');
                     showNotification('Observação atualizada.', 'info');
                 });
            });

            toggleDarkModeBtn.addEventListener('click', toggleDarkMode);

            aiAssistantFab.addEventListener('click', openAIAssistant);
            aiModalClose.addEventListener('click', closeAIAssistant);
            aiSendBtn.addEventListener('click', sendAIChatMessage);
            aiChatInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') sendAIChatMessage();
            });
            
            generateLaudoBtn.addEventListener('click', generateLaudo);
            copyLaudoBtn.addEventListener('click', copyLaudo);
        }
        // #endregion

        // #region Lógica de UI (Páginas, Modos, Notificações)
        function applyDarkMode() {
            document.body.classList.toggle('dark-mode', isDarkMode);
            darkModeText.textContent = isDarkMode ? 'Modo Claro' : 'Modo Escuro';
            toggleDarkModeBtn.querySelector('i').className = isDarkMode ? 'fas fa-sun' : 'fas fa-moon';
        }

        function toggleDarkMode() {
            isDarkMode = !isDarkMode;
            localStorage.setItem(DARK_MODE_KEY, isDarkMode);
            applyDarkMode();
        }
        
        function showPage(pageId) {
            pages.forEach(page => page.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            
            navBtns.forEach(btn => {
                btn.classList.toggle('active', btn.getAttribute('data-page') === pageId);
            });

            sidebarBtns.forEach(btn => {
                btn.classList.toggle('active', btn.getAttribute('data-page') === pageId);
            });
            
            if (['reportPage', 'analysisPage', 'finalizadoPage', 'settingsPage'].includes(pageId)) {
                updateUI();
            }
            if (pageId !== 'scanPage' && stream) {
                stopCamera();
            }
        }

        function showNotification(message, type) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type} show`;
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        function showRulesModal() {
            rulesModal.classList.add('active');
        }
        // #endregion

        // #region Lógica de Dados (CRUD e Persistência)
        async function migrateFromLocalStorage() {
            const oldData = localStorage.getItem(DB_KEY);
            if (oldData) {
                try {
                    const dataToMigrate = JSON.parse(oldData);
                    if (Array.isArray(dataToMigrate)) {
                        await dbManager.clear('batteries');
                        for (const item of dataToMigrate) {
                            await dbManager.set('batteries', item);
                        }
                        localStorage.removeItem(DB_KEY);
                        console.log('Migração de localStorage para IndexedDB concluída.');
                        showNotification('Dados migrados para a nova base de dados segura!', 'success');
                    }
                } catch (e) {
                    console.error("Erro na migração de dados:", e);
                }
            }
            const oldScanHistory = localStorage.getItem('fabreck_scan_history');
            if (oldScanHistory) {
                try {
                    const historyToMigrate = JSON.parse(oldScanHistory);
                    if (Array.isArray(historyToMigrate)) {
                        await dbManager.set('settings', { key: 'fabreck_scan_history', value: historyToMigrate });
                        localStorage.removeItem('fabreck_scan_history');
                        console.log('Migração do histórico de scan concluída.');
                    }
                } catch (e) {
                    console.error("Erro na migração do histórico de scan:", e);
                }
            }
        }

        async function loadFromDB() {
            batteryData = await dbManager.getAll('batteries');
            activityData = await dbManager.getAll('activity');
            const savedScanHistory = await dbManager.get('settings', 'fabreck_scan_history');
            if (savedScanHistory) {
                scanHistory = savedScanHistory.value;
            }
            const instructions = await dbManager.get('settings', INSTRUCTIONS_KEY);
            if(instructions) {
                warrantyInstructions = instructions.value;
            } else {
                 warrantyInstructions = {
                    approved: 'Bateria com defeito de fabricação. Enviar uma nova unidade para o cliente.',
                    rejected_return: 'Bateria sem defeito, apenas descarregada ou com falha externa. Devolver ao cliente.',
                    rejected_scrap: 'Bateria com falha por mau uso (ex: sobrecarga, caixa danificada) ou fora do prazo. Sucatear.'
                };
                await dbManager.set('settings', {key: INSTRUCTIONS_KEY, value: warrantyInstructions});
            }
        }
        
        async function addBattery() {
            const client = clientNameInput.value.trim().toUpperCase();
            const salesman = salesmanNameInput.value.trim().toUpperCase();
            const code = serialCodeInput.value.trim().toUpperCase();
            const submissionDate = submissionDateInput.value;
            const warrantyTypeEl = document.querySelector('input[name="warrantyType"]:checked');
            const recommendation = recommendationInput.value.trim();
            
            if (!client || !salesman || !code || !submissionDate || !warrantyTypeEl) {
                showNotification('Preencha Cliente, Vendedor, Código, Data e Tipo de Garantia.', 'error');
                return;
            }
            if (warrantyTypeEl.value === 'analyzed' && !recommendation) {
                showNotification('Para Análise por Vídeo, o Parecer Técnico é obrigatório.', 'error');
                return;
            }
            if (!validateSerialCode(code)) {
                showNotification('Formato de código inválido.', 'error');
                return;
            }
            if (batteryData.some(b => b.code === code)) {
                showNotification('Este código de série já foi registrado.', 'error');
                return;
            }
            
            const week = parseInt(code.substring(0, 2));
            const year = parseInt("20" + code.substring(2, 4));
            const warrantyStatus = calculateWarrantyStatus(week, year);

            const dateParts = submissionDate.split('-');
            const submissionDateObj = new Date(Date.UTC(dateParts[0], dateParts[1] - 1, dateParts[2]));
            
            const newBattery = {
                id: Date.now(),
                client,
                salesman,
                code,
                batteryModel: getBatteryModelFromCode(code),
                manufDate: getManufacturingDate(week, year).toLocaleDateString('pt-BR'),
                warranty_period_status: warrantyStatus,
                status: 'in_analysis',
                warrantyType: warrantyTypeEl.value,
                recommendation: recommendation,
                observations: tempObservation,
                finalAction: '',
                timestamp: new Date().toISOString(),
                submissionDate: submissionDateObj.toISOString(),
                technicalOpinionDate: null
            };

            if (warrantyStatus === 'expired') {
                newBattery.status = 'finalized';
                newBattery.finalAction = 'REPROVADA - FORA DO PRAZO';
                newBattery.recommendation = newBattery.recommendation || 'Finalizada automaticamente: Bateria fora do prazo de garantia.';
                newBattery.technicalOpinionDate = new Date().toISOString();
                showNotification(`Bateria ${code} finalizada automaticamente (Fora do Prazo).`, 'warning');
                addActivity('fas fa-clock', `Bateria ${code} finalizada (Fora do Prazo).`);
            } else if (recommendation) {
                newBattery.status = 'finalized';
                if (recommendation.toUpperCase().includes('PROCEDENTE')) {
                    newBattery.finalAction = 'APROVADA - ENVIAR NOVA';
                } else if (recommendation.toUpperCase().includes('RECARREGAR')) {
                    newBattery.finalAction = 'REPROVADA - DEVOLVER AO CLIENTE';
                } else {
                    newBattery.finalAction = 'ANALISADA NO REGISTRO';
                }
                newBattery.technicalOpinionDate = new Date().toISOString();
                showNotification(`Bateria ${code} finalizada no registro.`, 'success');
                addActivity('fas fa-check-circle', `Bateria ${code} finalizada no registro.`);
            } else {
                newBattery.status = 'in_analysis';
                showNotification('Bateria adicionada para análise!', 'success');
                addActivity('fas fa-battery-full', `Bateria ${code} adicionada para ${client}`);
            }

            await dbManager.set('batteries', newBattery);
            batteryData.push(newBattery);
            
            clearCode();
            recommendationInput.value = '';
            tempObservation = '';
            
            updateUI();
            triggerAutoSync();
        }

        async function removeBattery(id) {
            if (!confirm('Tem certeza que deseja remover esta bateria? A ação é irreversível.')) return;
            
            const removed = batteryData.find(b => b.id === id);
            await dbManager.delete('batteries', id);
            batteryData = batteryData.filter(b => b.id !== id);
            updateUI();
            showNotification('Bateria removida com sucesso!', 'success');
            addActivity('fas fa-trash-alt', `Bateria removida: ${removed.code}`);
            triggerAutoSync();
        }

        async function clearData() {
            if (batteryData.length === 0) {
                showNotification('Não há dados para limpar', 'info');
                return;
            }
            if (confirm('TEM CERTEZA? Todos os dados de garantia serão apagados permanentemente.')) {
                await dbManager.clear('batteries');
                await dbManager.clear('activity');
                await dbManager.delete('settings', 'fabreck_scan_history');
                batteryData = [];
                activityData = [];
                scanHistory = [];
                updateUI();
                updateActivityLog();
                updateScanHistory();
                lastUpdate.textContent = "Nenhuma atualização";
                showNotification('Todos os dados foram removidos.', 'success');
                addActivity('fas fa-trash-alt', 'Todos os dados foram removidos');
                triggerAutoSync();
            }
        }
        
        async function exportData() {
            const dataToExport = await dbManager.getAll('batteries');
            if (dataToExport.length === 0) {
                showNotification('Não há dados para exportar', 'info');
                return;
            }
            const dataStr = JSON.stringify(dataToExport);
            const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
            const exportFileDefaultName = `fabreck_backup_${new Date().toISOString().slice(0,10)}.json`;
            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
            showNotification('Dados exportados com sucesso!', 'success');
            addActivity('fas fa-download', 'Dados exportados');
        }
        
        function handleRestoreFile(file) {
            if (!file) return;

            const reader = new FileReader();
            reader.onload = async function(event) {
                try {
                    const importedData = JSON.parse(event.target.result);
                    if (!Array.isArray(importedData)) {
                        throw new Error('Estrutura de dados inválida: o arquivo não contém um array.');
                    }
                    
                    await dbManager.clear('batteries');
                    await dbManager.clear('activity');
                    
                    for(const item of importedData) {
                        await dbManager.set('batteries', item);
                    }

                    batteryData = importedData;
                    activityData = [];

                    updateUI();
                    updateActivityLog();

                    showNotification(`${importedData.length} registros restaurados com sucesso! Os dados atuais foram substituídos.`, 'success');
                    addActivity('fas fa-upload', `${importedData.length} baterias restauradas do backup.`);
                    triggerAutoSync();

                } catch (error) {
                    showNotification('Erro ao importar. Arquivo inválido ou incompatível.', 'error');
                    console.error('Erro na restauração:', error);
                } finally {
                    fileToRestore = null;
                }
            };
            reader.readAsText(file);
        }
        // #endregion

        // #region Lógica de Negócio (Garantia, Código)
        function validateSerialCode(code) {
            return /^\d{4}[A-Za-z]\d{4}$/.test(code);
        }

        function getBatteryModelFromCode(code) {
            if (code && code.length >= 5) {
                const fifthChar = code[4].toUpperCase();
                return batteryModelMap[fifthChar] || 'Desconhecido';
            }
            return 'N/A';
        }

        function getManufacturingDate(week, year) {
            const date = new Date(year, 0, 1 + (week - 1) * 7);
            return date;
        }
        
        function calculateWarrantyStatus(week, year) {
            const manufDate = getManufacturingDate(week, year);
            const warrantyEnd = new Date(manufDate);
            warrantyEnd.setFullYear(warrantyEnd.getFullYear() + 1);
            warrantyEnd.setDate(warrantyEnd.getDate() + 7); // 1 semana de tolerância
            return new Date() <= warrantyEnd ? 'warranty' : 'expired';
        }

        function updateWarrantyDebugInfo(code) {
            if (validateSerialCode(code)) {
                const week = parseInt(code.substring(0, 2));
                const year = parseInt("20" + code.substring(2, 4));
                const manufDate = getManufacturingDate(week, year);
                const warrantyEnd = new Date(manufDate);
                warrantyEnd.setFullYear(warrantyEnd.getFullYear() + 1);
                warrantyEnd.setDate(warrantyEnd.getDate() + 7);

                debugManufDate.textContent = manufDate.toLocaleDateString('pt-BR');
                debugWarrantyEndDate.textContent = warrantyEnd.toLocaleDateString('pt-BR');
                debugCalculatedStatus.textContent = new Date() <= warrantyEnd ? 'Em Garantia' : 'Fora do Prazo';
                warrantyDebugInfo.style.display = 'block';
            } else {
                warrantyDebugInfo.style.display = 'none';
            }
        }
        // #endregion

        // #region Atualização da UI (Tabelas, Stats)
        function updateUI() {
            updateClientFilter();
            updateReportTable();
            updateAnalysisClientFilter();
            updateAnalysisTable();
            updateFinalizadoTable();
            updateStats();
            updateWarrantyInstructionsUI();
        }
        
        function updateStats() {
            const inAnalysis = batteryData.filter(b => b.status === 'in_analysis').length;
            const finalized = batteryData.filter(b => b.status === 'finalized').length;
            
            if (totalCount) totalCount.textContent = batteryData.length;
            if (inAnalysisCountReport) inAnalysisCountReport.textContent = inAnalysis;
            if (finalizedCountReport) finalizedCountReport.textContent = finalized;

            if (inAnalysisCount) inAnalysisCount.textContent = inAnalysis;

            if (finalizadoCount) finalizadoCount.textContent = finalized;

            if(finalizadoAprovadaCount) {
                const approvedCount = batteryData.filter(b => b.status === 'finalized' && b.finalAction.includes('APROVADA')).length;
                finalizadoAprovadaCount.textContent = approvedCount;
            }
            if(finalizadoReprovadaPrazoCount) {
                const rejectedInWarrantyCount = batteryData.filter(b => 
                    b.status === 'finalized' && 
                    b.finalAction.includes('REPROVADA') && 
                    b.warranty_period_status === 'warranty'
                ).length;
                finalizadoReprovadaPrazoCount.textContent = rejectedInWarrantyCount;
            }
            if(finalizadoReprovadaForaCount) {
                const rejectedExpiredCount = batteryData.filter(b => 
                    b.status === 'finalized' && 
                    b.warranty_period_status === 'expired'
                ).length;
                finalizadoReprovadaForaCount.textContent = rejectedExpiredCount;
            }
        }
        
        function updateClientFilter() {
            const currentVal = clientFilter.value;
            clientFilter.innerHTML = '<option value="">Todos os Clientes</option>';
            const uniqueClients = [...new Set(batteryData.map(b => b.client))];
            uniqueClients.sort().forEach(client => {
                const option = document.createElement('option');
                option.value = client;
                option.textContent = client;
                clientFilter.appendChild(option);
            });
            clientFilter.value = currentVal;
        }

        function updateAnalysisClientFilter() {
            const currentVal = analysisClientFilter.value;
            analysisClientFilter.innerHTML = '<option value="">Todos os Clientes</option>';
            const uniqueClientsInAnalysis = [...new Set(batteryData.filter(b => b.status === 'in_analysis').map(b => b.client))];
            uniqueClientsInAnalysis.sort().forEach(client => {
                const option = document.createElement('option');
                option.value = client;
                option.textContent = client;
                analysisClientFilter.appendChild(option);
            });
            analysisClientFilter.value = currentVal;
        }

        function updateWarrantyInstructionsUI() {
            warrantyInstructionsContainer.innerHTML = `
                <p><strong>APROVADA - ENVIAR NOVA:</strong><br>${warrantyInstructions.approved}</p>
                <hr style="margin: 10px 0; border-color: rgba(0,0,0,0.1);">
                <p><strong>REPROVADA - DEVOLVER AO CLIENTE:</strong><br>${warrantyInstructions.rejected_return}</p>
                 <hr style="margin: 10px 0; border-color: rgba(0,0,0,0.1);">
                <p><strong>REPROVADA - SUCATEAR:</strong><br>${warrantyInstructions.rejected_scrap}</p>
            `;
        }
        
        function getFilteredData() {
            return batteryData.filter(b => {
                const clientMatch = !currentFilters.client || b.client.toLowerCase().includes(currentFilters.client.toLowerCase());
                const statusMatch = !currentFilters.status || b.warranty_period_status === currentFilters.status;
                const workflowMatch = !currentFilters.workflowStatus || b.status === currentFilters.workflowStatus;
                const codeMatch = !currentFilters.code || b.code.toLowerCase().includes(currentFilters.code.toLowerCase());
                
                let dateMatch = true;
                if (currentFilters.startDate && currentFilters.endDate) {
                    const batteryDate = new Date(b.timestamp);
                    const startDate = new Date(currentFilters.startDate + 'T00:00:00');
                    const endDate = new Date(currentFilters.endDate + 'T23:59:59');
                    dateMatch = batteryDate >= startDate && batteryDate <= endDate;
                }
                return clientMatch && statusMatch && workflowMatch && codeMatch && dateMatch;
            });
        }
        
        function updateReportTable() {
            const filteredData = getFilteredData();
            filteredData.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
            
            reportBody.innerHTML = '';
            if (filteredData.length === 0) {
                reportBody.innerHTML = `<tr><td colspan="10" style="text-align: center;">Nenhum resultado encontrado</td></tr>`;
                totalsContainer.innerHTML = '';
                document.getElementById('modelStatusSummaryCard').style.display = 'none';
                return;
            }
            
            filteredData.forEach(battery => {
                const row = document.createElement('tr');
                const warrantyStatus = battery.warranty_period_status === 'warranty' ? '<span class="status-badge status-warranty">Em Garantia</span>' : '<span class="status-badge status-expired">Fora do Prazo</span>';
                const workflowStatus = battery.status === 'in_analysis' ? '<span class="status-badge status-in-analysis">Em Análise</span>' : '<span class="status-badge status-finalized">Finalizado</span>';
                
                let finalActionBadge = '-';
                if (battery.finalAction) {
                    let actionClass = '';
                    if (battery.finalAction.includes('APROVADA')) actionClass = 'action-approved';
                    else if (battery.finalAction.includes('DEVOLVER')) actionClass = 'action-rejected';
                    else if (battery.finalAction.includes('SUCATEAR') || battery.finalAction.includes('PRAZO') || battery.finalAction.includes('VÍDEO')) actionClass = 'action-scrapped';
                    finalActionBadge = `<span class="status-badge ${actionClass}">${battery.finalAction}</span>`;
                }

                row.innerHTML = `
                    <td>${battery.code}</td>
                    <td>${battery.batteryModel}</td>
                    <td>${battery.client}</td>
                    <td>${battery.salesman}</td>
                    <td>${warrantyStatus}</td>
                    <td>${workflowStatus}</td>
                    <td>${finalActionBadge}</td>
                    <td style="white-space: normal; max-width: 150px;">${battery.recommendation || '-'}</td>
                    <td>${battery.technicalOpinionDate ? new Date(battery.technicalOpinionDate).toLocaleDateString('pt-BR') : '-'}</td>
                    <td style="display: flex; gap: 8px;">
                        <button class="name-edit-btn btn btn-info" data-id="${battery.id}" title="Editar Nomes" style="padding: 5px 10px; width: auto; margin: 0;">
                            <i class="fas fa-pencil-alt"></i>
                        </button>
                        <button class="remove-btn btn btn-danger" data-id="${battery.id}" title="Remover" style="padding: 5px 10px; width: auto; margin: 0;">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </td>
                `;
                reportBody.appendChild(row);
            });

            document.querySelectorAll('.remove-btn').forEach(btn => {
                btn.addEventListener('click', function() { removeBattery(parseInt(this.getAttribute('data-id'))); });
            });
            document.querySelectorAll('.name-edit-btn').forEach(btn => {
                btn.addEventListener('click', function() { openNameEditModal(parseInt(this.getAttribute('data-id'))); });
            });

            updateReportTotals(filteredData);
            updateModelStatusSummary(filteredData);
        }

        function updateReportTotals(data) {
            const warrantyCount = data.filter(b => b.warranty_period_status === 'warranty').length;
            const expiredCount = data.filter(b => b.warranty_period_status === 'expired').length;
            
            totalsContainer.innerHTML = `
                <div class="total-box">
                    <div class="total-title">Total (Filtro)</div>
                    <div class="total-value total-all">${data.length}</div>
                </div>
                <div class="total-box">
                    <div class="total-title">Em Garantia</div>
                    <div class="total-value total-warranty">${warrantyCount}</div>
                </div>
                <div class="total-box">
                    <div class="total-title">Fora do Prazo</div>
                    <div class="total-value total-expired">${expiredCount}</div>
                </div>
            `;
        }

        function updateModelStatusSummary(data) {
            const summaryCard = document.getElementById('modelStatusSummaryCard');
            const summaryBody = document.getElementById('modelStatusSummaryBody');

            const finalizedData = data.filter(b => b.status === 'finalized');

            if (finalizedData.length === 0) {
                summaryCard.style.display = 'none';
                return;
            }

            const summary = {
                approved: {},
                rejected: {},
                recharge: {},
                expired: {}
            };

            finalizedData.forEach(battery => {
                const model = battery.batteryModel || 'N/A';
                if (battery.finalAction.includes('APROVADA')) {
                    summary.approved[model] = (summary.approved[model] || 0) + 1;
                } else if (battery.finalAction.includes('FORA DO PRAZO')) {
                    summary.expired[model] = (summary.expired[model] || 0) + 1;
                } else if (battery.recommendation && battery.recommendation.toUpperCase().includes('RECARREGAR')) {
                    summary.recharge[model] = (summary.recharge[model] || 0) + 1;
                } else if (battery.finalAction.includes('REPROVADA')) {
                    summary.rejected[model] = (summary.rejected[model] || 0) + 1;
                }
            });

            if (Object.keys(summary.approved).length === 0 && Object.keys(summary.rejected).length === 0 && Object.keys(summary.recharge).length === 0 && Object.keys(summary.expired).length === 0) {
                summaryCard.style.display = 'none';
                return;
            }

            summaryCard.style.display = 'block';

            let html = '';

            const createSummaryLine = (title, data) => {
                if (Object.keys(data).length > 0) {
                    const items = Object.entries(data).map(([model, count]) => `${model} = ${String(count).padStart(2, '0')}`).join(' / ');
                    return `<div style="margin-bottom: 10px;"><h4>${title}</h4><p>${items}</p></div>`;
                }
                return '';
            };

            html += createSummaryLine('BATERIAS APROVADAS (TROCA)', summary.approved);
            html += createSummaryLine('BATERIAS REPROVADAS', summary.rejected);
            html += createSummaryLine('BATERIAS PARA RECARREGAR', summary.recharge);
            html += createSummaryLine('BATERIAS FORA DO PRAZO', summary.expired);

            summaryBody.innerHTML = html;
        }


        function updateAnalysisTable() {
            const selectedClient = analysisClientFilter.value;
            let dataToAnalyze = batteryData.filter(b => b.status === 'in_analysis');

            if (selectedClient) {
                dataToAnalyze = dataToAnalyze.filter(b => b.client === selectedClient);
            }

            dataToAnalyze.sort((a, b) => new Date(a.submissionDate) - new Date(b.submissionDate));
            analysisBody.innerHTML = '';
            selectAllCheckbox.checked = false;

            if (dataToAnalyze.length === 0) {
                analysisBody.innerHTML = `<tr><td colspan="7" style="text-align: center;">Nenhuma bateria na fila de análise.</td></tr>`;
                return;
            }

            dataToAnalyze.forEach(battery => {
                const row = document.createElement('tr');
                const warrantyStatus = battery.warranty_period_status === 'warranty' ? '<span class="status-badge status-warranty">Em Garantia</span>' : '<span class="status-badge status-expired">Fora do Prazo</span>';
                row.innerHTML = `
                    <td><input type="checkbox" class="analysis-checkbox" data-id="${battery.id}"></td>
                    <td>${battery.code}</td>
                    <td>${battery.client}</td>
                    <td>${battery.batteryModel}</td>
                    <td>${new Date(battery.submissionDate).toLocaleDateString('pt-BR')}</td>
                    <td>${warrantyStatus}</td>
                    <td>
                        <button class="analyze-btn btn btn-primary" data-id="${battery.id}" style="padding: 8px 12px; width: auto; margin: 0;">
                            <i class="fas fa-edit"></i> Analisar
                        </button>
                    </td>
                `;
                analysisBody.appendChild(row);
            });

            document.querySelectorAll('.analyze-btn').forEach(btn => {
                btn.addEventListener('click', function() { openSingleAnalysisModal(parseInt(this.getAttribute('data-id'))); });
            });
            document.querySelectorAll('.analysis-checkbox').forEach(box => {
                box.addEventListener('change', updateBatchAnalyzeButtonState);
            });
            updateBatchAnalyzeButtonState();
        }

        function updateFinalizadoTable() {
            const dataFinalizada = batteryData.filter(b => b.status === 'finalized');
            dataFinalizada.sort((a, b) => new Date(b.technicalOpinionDate) - new Date(a.technicalOpinionDate));
            finalizadoBody.innerHTML = '';

            if (dataFinalizada.length === 0) {
                finalizadoBody.innerHTML = `<tr><td colspan="5" style="text-align: center;">Nenhuma garantia finalizada.</td></tr>`;
                return;
            }

            dataFinalizada.forEach(battery => {
                const row = document.createElement('tr');
                 let finalActionBadge = '-';
                if (battery.finalAction) {
                    let actionClass = '';
                    if (battery.finalAction.includes('APROVADA')) actionClass = 'action-approved';
                    else if (battery.finalAction.includes('DEVOLVER')) actionClass = 'action-rejected';
                    else if (battery.finalAction.includes('SUCATEAR') || battery.finalAction.includes('PRAZO') || battery.finalAction.includes('VÍDEO')) actionClass = 'action-scrapped';
                    finalActionBadge = `<span class="status-badge ${actionClass}">${battery.finalAction}</span>`;
                }
                row.innerHTML = `
                    <td>${battery.code}</td>
                    <td>${battery.client}</td>
                    <td>${new Date(battery.technicalOpinionDate).toLocaleDateString('pt-BR')}</td>
                    <td>${finalActionBadge}</td>
                    <td style="white-space: normal; max-width: 200px;">${battery.recommendation}</td>
                `;
                finalizadoBody.appendChild(row);
            });
        }
        // #endregion

        // #region Lógica de Análise e Edição (Single e Batch)
        async function openSingleAnalysisModal(id) {
            const battery = await dbManager.get('batteries', id);
            if (!battery) return;

            analysisMode = 'single';
            analyzingBatteryId = id;
            
            analysisModalTitle.textContent = `Analisar Bateria: ${battery.code}`;
            analysisSingleInfo.style.display = 'block';

            analysisClientName.textContent = battery.client;
            analysisSalesmanName.textContent = battery.salesman;
            analysisBatteryModel.textContent = battery.batteryModel;
            analysisWarrantyStatus.innerHTML = battery.warranty_period_status === 'warranty' ? '<span class="status-badge status-warranty">Em Garantia</span>' : '<span class="status-badge status-expired">Fora do Prazo</span>';
            analysisRecommendation.value = battery.recommendation || '';
            analysisFinalAction.value = '';

            analysisModal.classList.add('active');
        }

        function openBatchAnalysisModal() {
            const selectedCheckboxes = document.querySelectorAll('.analysis-checkbox:checked');
            if (selectedCheckboxes.length === 0) {
                showNotification('Nenhuma bateria selecionada para análise em lote.', 'error');
                return;
            }

            batchAnalysisIds = [];
            selectedCheckboxes.forEach(checkbox => {
                batchAnalysisIds.push(parseInt(checkbox.getAttribute('data-id')));
            });

            analysisMode = 'batch';
            analysisModalTitle.textContent = `Analisar ${batchAnalysisIds.length} Baterias em Lote`;
            analysisSingleInfo.style.display = 'none'; // Oculta informações individuais
            analysisRecommendation.value = '';
            analysisFinalAction.value = '';
            
            analysisModal.classList.add('active');
        }

        function saveAnalysis() {
            if (analysisMode === 'single') {
                saveSingleAnalysis();
            } else if (analysisMode === 'batch') {
                saveBatchAnalysis();
            }
        }

        async function saveSingleAnalysis() {
            if (analyzingBatteryId === null) return;

            const battery = await dbManager.get('batteries', analyzingBatteryId);
            if (!battery) return;

            const newRecommendation = analysisRecommendation.value.trim();
            const newFinalAction = analysisFinalAction.value;

            if (!newRecommendation || !newFinalAction) {
                showNotification('Preencha o Parecer Técnico e a Ação Final.', 'error');
                return;
            }

            battery.status = 'finalized';
            battery.recommendation = newRecommendation;
            battery.finalAction = newFinalAction;
            battery.technicalOpinionDate = new Date().toISOString();

            await dbManager.set('batteries', battery);
            batteryData = await dbManager.getAll('batteries');
            
            analysisModal.classList.remove('active');
            showNotification(`Análise da bateria ${battery.code} salva!`, 'success');
            addActivity('fas fa-clipboard-check', `Análise da bateria ${battery.code} finalizada.`);
            analyzingBatteryId = null;
            updateUI();
            triggerAutoSync();
        }

        async function saveBatchAnalysis() {
            const newRecommendation = analysisRecommendation.value.trim();
            const newFinalAction = analysisFinalAction.value;

            if (!newRecommendation || !newFinalAction) {
                showNotification('Preencha o Parecer Técnico e a Ação Final para o lote.', 'error');
                return;
            }

            let updatedCount = 0;
            for (const batteryId of batchAnalysisIds) {
                const battery = await dbManager.get('batteries', batteryId);
                if (battery) {
                    battery.status = 'finalized';
                    battery.recommendation = newRecommendation;
                    battery.finalAction = newFinalAction;
                    battery.technicalOpinionDate = new Date().toISOString();
                    await dbManager.set('batteries', battery);
                    updatedCount++;
                }
            }

            if (updatedCount > 0) {
                batteryData = await dbManager.getAll('batteries');
                analysisModal.classList.remove('active');
                showNotification(`${updatedCount} baterias analisadas em lote com sucesso!`, 'success');
                addActivity('fas fa-layer-group', `${updatedCount} baterias analisadas em lote.`);
                batchAnalysisIds = [];
                updateUI();
                triggerAutoSync();
            }
        }

        function handleSelectAll() {
            const checkboxes = document.querySelectorAll('.analysis-checkbox');
            checkboxes.forEach(checkbox => {
                checkbox.checked = selectAllCheckbox.checked;
            });
            updateBatchAnalyzeButtonState();
        }

        function updateBatchAnalyzeButtonState() {
            const selectedCount = document.querySelectorAll('.analysis-checkbox:checked').length;
            batchAnalyzeBtn.disabled = selectedCount === 0;
            batchAnalyzeBtn.textContent = selectedCount > 0 ? `Analisar ${selectedCount} Selecionados` : 'Analisar Selecionados';
        }

        async function openNameEditModal(id) {
            const battery = await dbManager.get('batteries', id);
            if (!battery) return;

            editingBatteryId = id;
            editClientNameInput.value = battery.client;
            editSalesmanNameInput.value = battery.salesman;
            nameEditModal.classList.add('active');
        }

        async function saveEditedNames() {
            if (editingBatteryId === null) return;

            const originalBattery = await dbManager.get('batteries', editingBatteryId);
            if (!originalBattery) return;

            const oldClientName = originalBattery.client;
            const oldSalesmanName = originalBattery.salesman;

            const newClientName = editClientNameInput.value.trim().toUpperCase();
            const newSalesmanName = editSalesmanNameInput.value.trim().toUpperCase();

            if (!newClientName || !newSalesmanName) {
                showNotification('Os nomes não podem estar em branco.', 'error');
                return;
            }

            const allBatteries = await dbManager.getAll('batteries');
            for (const battery of allBatteries) {
                let updated = false;
                if (battery.client === oldClientName) {
                    battery.client = newClientName;
                    updated = true;
                }
                if (battery.salesman === oldSalesmanName) {
                    battery.salesman = newSalesmanName;
                    updated = true;
                }
                if(updated) await dbManager.set('batteries', battery);
            }

            batteryData = await dbManager.getAll('batteries');
            updateUI();
            nameEditModal.classList.remove('active');
            showNotification('Nomes atualizados em todos os registros!', 'success');
            addActivity('fas fa-pencil-alt', `Nomes '${oldClientName}/${oldSalesmanName}' atualizados.`);
            editingBatteryId = null;
            triggerAutoSync();
        }

        function openObservationModal(currentObservation, callback) {
            observationText.value = currentObservation || '';
            observationCallback = callback;
            observationModal.classList.add('active');
        }
        // #endregion

        // #region Formulário e Câmera com IA
        function handleSerialInput() {
            const code = this.value.trim().toUpperCase();
            codePreview.textContent = code;
            codePreview.style.color = validateSerialCode(code) ? 'var(--fabreck-blue)' : 'var(--fabreck-danger)';
            updateWarrantyDebugInfo(code);
            if (!rulesShown && code.length >= 4 && !validateSerialCode(code)) {
                showRulesModal();
                rulesShown = true;
                localStorage.setItem('rulesShown', 'true');
            }
        }
        
        function clearCode() {
            serialCodeInput.value = '';
            codePreview.textContent = '';
            warrantyDebugInfo.style.display = 'none';
            serialCodeInput.focus();
        }

        async function selectWarrantyType(type) {
            factoryRadio.checked = type === 'factory';
            analyzedRadio.checked = type === 'analyzed';
            factoryOption.classList.toggle('selected', type === 'factory');
            analyzedOption.classList.toggle('selected', type === 'analyzed');
            videoAnalysisOptions.classList.toggle('hidden', type !== 'analyzed');
            await dbManager.set('settings', { key: LAST_WARRANTY_TYPE_KEY, value: type });
        }

        function handleScanClick() {
            if (stream) {
                showNotification('Toque na tela da câmera para capturar.', 'info');
            } else {
                startCamera();
            }
        }
        
        async function startCamera() {
            try {
                if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) throw new Error('API de câmera não suportada');
                if (stream) stream.getTracks().forEach(track => track.stop());
                
                const constraints = { video: { facingMode: currentFacingMode, width: { ideal: 1280 }, height: { ideal: 720 } } };
                stream = await navigator.mediaDevices.getUserMedia(constraints);
                video.srcObject = stream;
                
                startCameraBtn.disabled = true;
                stopCameraBtn.disabled = false;
                switchCameraBtn.style.display = 'block';
                captureOverlay.classList.remove('hidden');
                scannerIndicator.classList.add('active');
                scannerStatusText.textContent = 'Toque na tela para capturar';
                scanning = true;
            } catch (err) {
                console.error('Erro ao acessar a câmera:', err);
                showNotification('Erro ao acessar a câmera. Verifique as permissões.', 'error');
                stopCamera();
            }
        }
        
        function stopCamera() {
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
                stream = null;
            }
            startCameraBtn.disabled = false;
            stopCameraBtn.disabled = true;
            switchCameraBtn.style.display = 'none';
            captureOverlay.classList.add('hidden');
            scannerIndicator.classList.remove('active');
            scannerStatusText.textContent = 'Câmera desativada';
            scanning = false;
        }
        
        function switchCamera() {
            currentFacingMode = currentFacingMode === 'environment' ? 'user' : 'environment';
            stopCamera();
            setTimeout(startCamera, 100);
        }
        
        async function captureImage() {
            if (!stream || !scanning) return;
            
            scannerStatusText.textContent = 'Processando com IA...';
            
            try {
                const canvas = document.createElement('canvas');
                canvas.width = video.videoWidth;
                canvas.height = video.videoHeight;
                canvas.getContext('2d').drawImage(video, 0, 0, canvas.width, canvas.height);
                
                const base64ImageData = canvas.toDataURL('image/png').split(',')[1];
                
                const prompt = "Extraia o número de série de 9 caracteres desta imagem. O formato é 4 dígitos, 1 letra e 4 dígitos (ex: 3524A2623). Forneça apenas o código, sem texto adicional.";

                const payload = {
                    contents: [{
                        parts: [
                            { text: prompt },
                            { inlineData: { mimeType: "image/png", data: base64ImageData } }
                        ]
                    }]
                };
                
                const apiKey = ""; // A chave será injetada pelo ambiente
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    throw new Error(`Erro na API: ${response.statusText}`);
                }

                const result = await response.json();
                const text = result?.candidates?.[0]?.content?.parts?.[0]?.text?.trim();

                if (text && validateSerialCode(text)) {
                    serialCodeInput.value = text;
                    handleSerialInput();
                    playBeep();
                    await addToScanHistory(text);
                    scannerStatusText.textContent = 'Código reconhecido!';
                    showNotification(`Código reconhecido pela IA: ${text}`, 'success');
                    clientNameInput.focus();
                } else {
                    scannerStatusText.textContent = 'Código não encontrado. Tente novamente.';
                    showNotification('IA não encontrou um código válido. Melhore a iluminação ou o enquadramento.', 'error');
                }

            } catch (error) {
                console.error('Erro no OCR com IA:', error);
                scannerStatusText.textContent = 'Erro no reconhecimento';
                showNotification('Ocorreu um erro ao processar a imagem.', 'error');
            } finally {
                setTimeout(() => { if (scanning) scannerStatusText.textContent = 'Toque na tela para capturar'; }, 2000);
            }
        }
        // #endregion

        // #region Histórico e Log de Atividades
        function setupAudio() {
            try {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                beepSound = () => {
                    const oscillator = audioContext.createOscillator();
                    const gainNode = audioContext.createGain();
                    oscillator.connect(gainNode);
                    gainNode.connect(audioContext.destination);
                    oscillator.type = 'sine';
                    oscillator.frequency.value = 800;
                    gainNode.gain.value = 0.1;
                    oscillator.start();
                    setTimeout(() => oscillator.stop(), 150);
                };
            } catch (e) {
                console.log('Web Audio API não suportada');
            }
        }
        
        function playBeep() {
            if (beepSound) beepSound();
        }
        
        function updateLastUpdate() {
            if(batteryData.length > 0) {
                 lastUpdate.textContent = new Date(Math.max(...batteryData.map(b => new Date(b.timestamp)))).toLocaleString('pt-BR');
            } else {
                 lastUpdate.textContent = "Nenhuma atualização";
            }
        }
        
        function updateActivityLog() {
            activityLog.innerHTML = '';
            if (activityData.length === 0) {
                activityLog.innerHTML = `<div class="activity-item"><div class="activity-icon"><i class="fas fa-info-circle"></i></div><div class="activity-content">Nenhuma atividade recente</div></div>`;
                return;
            }
            activityData.slice(-5).reverse().forEach(activity => {
                const item = document.createElement('div');
                item.className = 'activity-item';
                item.innerHTML = `<div class="activity-icon"><i class="${activity.icon}"></i></div><div class="activity-content">${activity.message}</div><div class="activity-time">${activity.time}</div>`;
                activityLog.appendChild(item);
            });
        }
        
        async function addActivity(icon, message) {
            const newActivity = { icon, message, time: new Date().toLocaleTimeString('pt-BR') };
            await dbManager.set('activity', newActivity);
            activityData.push(newActivity);
            if (activityData.length > 50) activityData.shift();
            updateActivityLog();
        }

        async function addToScanHistory(code) {
            scanHistory.unshift({ code, time: new Date().toLocaleTimeString('pt-BR') });
            if (scanHistory.length > 10) scanHistory.pop();
            await dbManager.set('settings', { key: 'fabreck_scan_history', value: scanHistory });
            updateScanHistory();
        }

        function updateScanHistory() {
            scannedHistory.innerHTML = '';
            if (scanHistory.length === 0) {
                scannedHistory.innerHTML = '<div class="scanned-item"><span>Nenhum código escaneado</span></div>';
                return;
            }
            scanHistory.forEach(item => {
                const div = document.createElement('div');
                div.className = 'scanned-item';
                div.innerHTML = `<span class="scanned-code">${item.code}</span><span class="scanned-time">${item.time}</span>`;
                scannedHistory.appendChild(div);
            });
        }
        // #endregion

        // #region Filtros e PDF/Excel
        function applyFilters() {
            currentFilters = {
                client: clientFilter.value,
                status: statusFilter.value,
                workflowStatus: workflowStatusFilter.value,
                startDate: startDateInput.value,
                endDate: endDateInput.value,
                code: codeFilter.value
            };
            updateReportTable();
            showNotification('Filtros aplicados!', 'success');
        }
        
        function clearFilters() {
            clientFilter.value = '';
            statusFilter.value = '';
            workflowStatusFilter.value = '';
            startDateInput.value = '';
            endDateInput.value = '';
            codeFilter.value = '';
            currentFilters = { client: '', status: '', workflowStatus: '', startDate: '', endDate: '', code: '' };
            updateReportTable();
            showNotification('Filtros limpos!', 'success');
        }

        function drawPdfHeader(doc, clientName, salesmanName) {
            if (logoBase64) {
                doc.addImage(logoBase64, 'SVG', 15, 10, 80, 16);
            }
            doc.setFontSize(18);
            doc.text('FABRECK DO BRASIL', 195, 15, { align: 'right' });
            doc.setFontSize(12);
            doc.text('Relatório de Análise de Garantia', 195, 22, { align: 'right' });
            doc.setFontSize(10);
            doc.text(`Data de Emissão: ${new Date().toLocaleDateString('pt-BR')}`, 195, 28, { align: 'right' });
            
            doc.setFontSize(12);
            doc.text(`Cliente: ${clientName}`, 15, 45);
            doc.text(`Vendedor: ${salesmanName}`, 15, 51);

            doc.setLineWidth(0.5);
            doc.line(15, 58, 195, 58);

            doc.setFontSize(9);
            doc.setFont(undefined, 'bold');
            doc.text('RESPONSÁVEL TÉCNICO: REGINALDO SANTOS', 15, 65);
            doc.text('ANÁLISE DE GARANTIA: JENILTON CRUZ', 195, 65, { align: 'right' });
            doc.setFont(undefined, 'normal');

            return 75;
        }

        function buildClientPDFPage(doc, clientName, clientBatteries, startY) {
            let currentY = startY;

            const summary = {
                approved: {},
                rejected: {},
                recharge: {},
                expired: {}
            };

            const finalizedData = clientBatteries.filter(b => b.status === 'finalized');

            finalizedData.forEach(battery => {
                const model = battery.batteryModel || 'N/A';
                if (battery.finalAction.includes('APROVADA')) {
                    summary.approved[model] = (summary.approved[model] || 0) + 1;
                } else if (battery.finalAction.includes('FORA DO PRAZO')) {
                    summary.expired[model] = (summary.expired[model] || 0) + 1;
                } else if (battery.recommendation && battery.recommendation.toUpperCase().includes('RECARREGAR')) {
                    summary.recharge[model] = (summary.recharge[model] || 0) + 1;
                } else if (battery.finalAction.includes('REPROVADA')) {
                    summary.rejected[model] = (summary.rejected[model] || 0) + 1;
                }
            });

            doc.setFontSize(14);
            doc.setFont(undefined, 'bold');
            doc.text('Resumo para Separação', 15, currentY);
            currentY += 10;

            const createSummaryList = (title, data, color) => {
                if (Object.keys(data).length > 0) {
                    if (currentY > 260) {
                        doc.addPage();
                        currentY = 20;
                    }
                    const totalCount = Object.values(data).reduce((sum, count) => sum + count, 0);
                    doc.setFontSize(12);
                    doc.setFont(undefined, 'bold');
                    doc.setTextColor(color[0], color[1], color[2]);
                    doc.text(`${title} - TOTAL: ${String(totalCount).padStart(2, '0')}`, 15, currentY);
                    currentY += 8;

                    doc.setFontSize(10);
                    doc.setFont(undefined, 'normal');
                    doc.setTextColor(0, 0, 0);
                    const items = Object.entries(data).map(([model, count]) => `${model} = ${String(count).padStart(2, '0')}`).join('  /  ');
                    doc.text(items, 15, currentY, { maxWidth: 180 });
                    currentY += 15;
                }
            };
            
            doc.setTextColor(0, 0, 0);
            createSummaryList('BATERIAS APROVADAS (PARA TROCA)', summary.approved, [39, 174, 96]);
            createSummaryList('BATERIAS PARA RECARREGAR', summary.recharge, [243, 156, 18]);
            createSummaryList('BATERIAS REPROVADAS', summary.rejected, [231, 76, 60]);
            createSummaryList('BATERIAS FORA DO PRAZO', summary.expired, [127, 140, 141]);

            if (currentY > 240) {
                doc.addPage();
                currentY = 20;
            } else {
                doc.setLineWidth(0.2);
                doc.line(15, currentY - 5, 195, currentY - 5);
            }
            
            doc.setFontSize(14);
            doc.setFont(undefined, 'bold');
            doc.setTextColor(0,0,0);
            doc.text('Detalhamento das Baterias', 15, currentY);
            currentY += 10;

            const tableBodyData = clientBatteries.map(b => [
                b.code,
                b.batteryModel,
                new Date(b.submissionDate).toLocaleDateString('pt-BR'),
                b.finalAction || (b.status === 'in_analysis' ? 'Em Análise' : '-'),
                b.recommendation || '-'
            ]);

            doc.autoTable({
                startY: currentY,
                head: [['Código', 'Modelo', 'Data Envio', 'Ação Final', 'Parecer Técnico']],
                body: tableBodyData,
                theme: 'striped',
                headStyles: { fillColor: [22, 49, 72] },
                bodyStyles: { fontSize: 8 },
                columnStyles: { 4: { cellWidth: 60 } },
                 didDrawPage: (data) => {
                    currentY = data.cursor.y; 
                }
            });
            currentY = doc.lastAutoTable.finalY + 15;

            if (currentY > 240) {
                doc.addPage();
                currentY = 20;
            }
            doc.setFontSize(12);
            doc.setFont(undefined, 'bold');
            doc.text('Critérios para Análise de Garantia', 15, currentY);
            currentY += 8;
            
            doc.setFontSize(9);
            doc.setFont(undefined, 'normal');
            doc.text(`APROVADA (TROCA): ${warrantyInstructions.approved}`, 17, currentY, { maxWidth: 180 });
            currentY += 12;
            doc.text(`REPROVADA: ${warrantyInstructions.rejected_return}`, 17, currentY, { maxWidth: 180 });
            currentY += 12;
             doc.text(`SUCATEAR: ${warrantyInstructions.rejected_scrap}`, 17, currentY, { maxWidth: 180 });
        }


        function generatePDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            const filteredData = getFilteredData();

            const groupedByClient = filteredData.reduce((acc, battery) => {
                (acc[battery.client] = acc[battery.client] || []).push(battery);
                return acc;
            }, {});
            
            let firstClient = true;
            for (const clientName in groupedByClient) {
                if (!firstClient) {
                    doc.addPage();
                }
                const clientBatteries = groupedByClient[clientName];
                const salesmanName = clientBatteries.length > 0 ? clientBatteries[0].salesman : 'N/A';
                const startY = drawPdfHeader(doc, clientName, salesmanName);
                buildClientPDFPage(doc, clientName, clientBatteries, startY);
                firstClient = false;
            }
            
            return doc;
        }
        
        function generateBatchPDFs() {
            const filteredData = getFilteredData();
             if (filteredData.length === 0) {
                showNotification('Nenhum dado para gerar os relatórios.', 'error');
                return;
            }

            const groupedByClient = filteredData.reduce((acc, battery) => {
                (acc[battery.client] = acc[battery.client] || []).push(battery);
                return acc;
            }, {});

            showNotification(`Gerando ${Object.keys(groupedByClient).length} relatórios...`, 'info');

            for (const clientName in groupedByClient) {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF();
                const clientBatteries = groupedByClient[clientName];
                const salesmanName = clientBatteries.length > 0 ? clientBatteries[0].salesman : 'N/A';

                const startY = drawPdfHeader(doc, clientName, salesmanName);
                buildClientPDFPage(doc, clientName, clientBatteries, startY);
                
                const safeFileName = clientName.replace(/[^a-z0-9]/gi, '_').toLowerCase();
                doc.save(`relatorio_garantia_${safeFileName}.pdf`);
            }
        }

        function previewPDF() {
            if (getFilteredData().length === 0) {
                showNotification('Nenhum dado para gerar o relatório.', 'error');
                return;
            }
            const doc = generatePDF();
            doc.output('dataurlnewwindow');
        }
        
        function downloadPDF() {
             if (getFilteredData().length === 0) {
                showNotification('Nenhum dado para gerar o relatório.', 'error');
                return;
            }
            const doc = generatePDF();
            doc.save(`relatorio_consolidado_${new Date().toISOString().slice(0,10)}.pdf`);
        }
        
        function exportToFormattedExcel() {
            const data = getFilteredData();
            if (data.length === 0) {
                showNotification('Não há dados para exportar', 'error');
                return;
            }

            const mappedData = data.map(b => ({
                'Código': b.code,
                'Modelo': b.batteryModel,
                'Cliente': b.client,
                'Vendedor': b.salesman,
                'Data de Registro': new Date(b.timestamp).toLocaleString('pt-BR'),
                'Data de Envio': b.submissionDate ? new Date(b.submissionDate).toLocaleDateString('pt-BR') : '',
                'Status da Garantia': b.warranty_period_status === 'warranty' ? 'Em Garantia' : 'Fora do Prazo',
                'Status da Análise': b.status === 'in_analysis' ? 'Em Análise' : 'Finalizado',
                'Parecer Técnico': b.recommendation || '',
                'Ação Final': b.finalAction || '',
                'Data da Análise': b.technicalOpinionDate ? new Date(b.technicalOpinionDate).toLocaleString('pt-BR') : ''
            }));

            const ws = XLSX.utils.json_to_sheet(mappedData);
            
            ws['!autofilter'] = { ref: XLSX.utils.encode_range(XLSX.utils.decode_range(ws['!ref'])) };

            const colWidths = [];
            for (const key in mappedData[0]) {
                colWidths.push({ wch: Math.max(key.length, ...mappedData.map(row => row[key] ? row[key].toString().length : 0)) + 2 });
            }
            ws['!cols'] = colWidths;

            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Relatório de Garantias");

            XLSX.writeFile(wb, `relatorio_garantias_${new Date().toISOString().slice(0,10)}.xlsx`);
        }
        // #endregion

        // #region Lógica do Assistente e Laudo de IA
        function openAIAssistant() {
            aiModal.classList.add('active');
            if (aiChatBox.children.length === 0) {
                const welcomeMessage = "Olá! Sou o assistente técnico da FABRECK DO BRASIL. Posso fornecer informações detalhadas sobre tipos de bateria, diagnósticos, manutenção e as regras de garantia. Como posso ajudar?";
                addMessageToAIChat('assistant', welcomeMessage);
                aiChatHistory.push({ role: 'model', parts: [{ text: welcomeMessage }] });
            }
        }

        function closeAIAssistant() {
            aiModal.classList.remove('active');
        }

        function addMessageToAIChat(sender, message) {
            const messageEl = document.createElement('div');
            messageEl.classList.add('ai-chat-message', sender);
            
            if (sender === 'assistant' && message === 'thinking') {
                messageEl.classList.add('thinking');
                messageEl.innerHTML = `<span class="dot">.</span><span class="dot">.</span><span class="dot">.</span>`;
            } else {
                messageEl.textContent = message;
            }
            
            aiChatBox.appendChild(messageEl);
            aiChatBox.scrollTop = aiChatBox.scrollHeight;
            return messageEl;
        }

        async function sendAIChatMessage() {
            const userInput = aiChatInput.value.trim();
            if (!userInput) return;

            addMessageToAIChat('user', userInput);
            aiChatHistory.push({ role: 'user', parts: [{ text: userInput }] });
            aiChatInput.value = '';
            
            const thinkingEl = addMessageToAIChat('assistant', 'thinking');

            const knowledgeBase = `
                **Sobre a FABRECK DO BRASIL:** A FABRECK DO BRASIL é especialista em baterias de alta performance para motocicletas, utilizando tecnologia de ponta para garantir durabilidade e confiança.
                **Tipos de Bateria de Moto:** Convencional (Chumbo-Ácido), AGM (Selada VRLA), Gel, Lítio (LiFePO4).
                **Termos Técnicos:** Voltagem (V), Amperagem (Ah), CA (Cranking Amps), CCA (Cold Cranking Amps).
                **Diagnóstico de Problemas:** Causas para a bateria não segurar carga (sulfatação, fim da vida útil, problema na moto), passos para diagnosticar moto que não liga (terminais, voltagem, teste de CCA).
                **Dicas de Manutenção:** Limpeza de terminais, uso de carregador inteligente, voltagem de recarga ideal (13.5V-14.5V), armazenamento correto.
                **Regras de Garantia Fabreck:** Código de 9 caracteres (4 números, 1 letra, 4 números), garantia de 1 ano da fabricação + 7 dias de tolerância.
            `;

            const systemPrompt = `Você é o "IA FABRECK DO BRASIL", um assistente técnico especialista em baterias de motocicleta. Responda de forma profissional e amigável, usando a base de conhecimento a seguir. Se a pergunta for fora do escopo, informe educadamente que só pode ajudar com tópicos relacionados a baterias FABRECK DO BRASIL. Base de Conhecimento: ${knowledgeBase}`;

            const fullHistory = [
                { role: 'user', parts: [{ text: systemPrompt }] },
                { role: 'model', parts: [{ text: 'Entendido. Sou o IA FABRECK DO BRASIL, especialista em baterias. Estou pronto para ajudar.' }] },
                ...aiChatHistory
            ];


            try {
                const payload = { contents: fullHistory };
                const apiKey = ""; 
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) throw new Error(`API Error: ${response.statusText}`);
                
                const result = await response.json();
                const aiResponse = result?.candidates?.[0]?.content?.parts?.[0]?.text;
                
                if (aiResponse) {
                    thinkingEl.remove();
                    addMessageToAIChat('assistant', aiResponse);
                    aiChatHistory.push({ role: 'model', parts: [{ text: aiResponse }] });
                } else {
                    throw new Error("No response from AI.");
                }

            } catch (error) {
                console.error("Erro no chat com IA:", error);
                thinkingEl.remove();
                addMessageToAIChat('assistant', 'Desculpe, ocorreu um erro. Tente novamente mais tarde.');
            }
        }
        
        async function generateLaudo() {
            const client = document.getElementById('laudoClientName').value.trim();
            const code = document.getElementById('laudoBatteryCode').value.trim();
            const model = document.getElementById('laudoBatteryModel').value.trim();
            const ca = document.getElementById('laudoCA').value;
            const voltage = document.getElementById('laudoVoltage').value;
            const visual = document.getElementById('laudoVisualInspection').value.trim();
            const notes = document.getElementById('laudoTechnicianNotes').value.trim();

            if(!client || !code || !model || !ca || !voltage || !visual) {
                showNotification('Por favor, preencha todos os campos do laudo.', 'error');
                return;
            }

            const btn = document.getElementById('generateLaudoBtn');
            btn.disabled = true;
            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Gerando...';
            
            laudoResultContainer.classList.remove('hidden');
            laudoResult.value = "A IA está a redigir o laudo técnico com base nos dados fornecidos. Por favor, aguarde...";

            const prompt = `
                Aja como um engenheiro técnico especialista em baterias da FABRECK DO BRASIL.
                Crie um laudo técnico profissional, formal e bem estruturado em português do Brasil.
                A análise é feita em bancada, na fábrica, sem acesso à motocicleta do cliente.
                Use os seguintes dados brutos:
                - Cliente: ${client}
                - Código da Bateria: ${code}
                - Modelo da Bateria: ${model}
                - Teste de CA (Cranking Amps): ${ca} A
                - Voltagem em Repouso: ${voltage} V
                - Análise Visual: ${visual}
                - Observações do Técnico: ${notes}

                O laudo deve seguir esta estrutura:
                1.  **CABEÇALHO:** "LAUDO TÉCNICO DE ANÁLISE DE BATERIA - FABRECK DO BRASIL"
                2.  **DADOS DE IDENTIFICAÇÃO:** Cliente, Código e Modelo.
                3.  **PROCEDIMENTOS DE TESTE:** Descreva os testes (inspeção visual, medição de tensão, teste de CA).
                4.  **RESULTADOS OBTIDOS:** Apresente os valores e observações.
                5.  **ANÁLISE TÉCNICA:** Faça uma análise profissional. Compare a voltagem com o padrão (>12.4V). Explique o que os resultados significam.
                6.  **DIAGNÓSTICO FINAL:** Conclua com um diagnóstico focado apenas na bateria (ex: "Bateria com desgaste natural", "Nenhum defeito de fabricação encontrado, bateria apenas descarregada").
                7.  **RECOMENDAÇÃO:** Dê uma recomendação técnica. Adicione a observação padrão: "É crucial que o sistema de recarga da motocicleta (alternador/retificador) seja verificado por um profissional qualificado antes da instalação de uma nova bateria, para evitar danos recorrentes."
                8.  **RODAPÉ:** "Laudo gerado por IA FABRECK DO BRASIL em ${new Date().toLocaleDateString('pt-BR')}. Análise realizada em bancada."

                Seja objetivo e use terminologia técnica apropriada.
            `;
            
            try {
                const payload = { contents: [{ parts: [{ text: prompt }] }] };
                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) throw new Error(`API Error: ${response.statusText}`);
                
                const result = await response.json();
                const aiResponse = result?.candidates?.[0]?.content?.parts?.[0]?.text;

                if(aiResponse) {
                    laudoResult.value = aiResponse;
                } else {
                    throw new Error("Resposta da IA vazia.");
                }

            } catch(error) {
                console.error("Erro ao gerar laudo:", error);
                laudoResult.value = "Ocorreu um erro ao comunicar com a IA. Por favor, tente novamente.";
                showNotification("Erro ao gerar laudo.", 'error');
            } finally {
                btn.disabled = false;
                btn.innerHTML = '<i class="fas fa-cogs"></i> Gerar Laudo com IA';
            }
        }

        function copyLaudo() {
            laudoResult.select();
            document.execCommand('copy');
            showNotification('Laudo copiado para a área de transferência!', 'success');
        }
        // #endregion

        // #region Sincronização em Nuvem
        function triggerAutoSync() {
            debounce(() => saveToCloud(true), 2500);
        }

        function debounce(func, delay) {
            clearTimeout(syncDebounceTimer);
            const syncStatus = document.getElementById('syncStatus');
            syncStatus.textContent = 'A guardar alterações...';
            syncStatus.style.color = '#F39C12';
            syncDebounceTimer = setTimeout(func, delay);
        }

        async function saveToCloud(isSilent = false) {
            const syncStatus = document.getElementById('syncStatus');
            syncStatus.textContent = 'A sincronizar...';
            
            const dataToSave = {
                batteries: await dbManager.getAll('batteries'),
                savedAt: new Date().toISOString()
            };
            
            try {
                const response = await fetch(`https://jsonblob.com/api/jsonBlob/${FIXED_SYNC_KEY}`, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json',
                        'Accept': 'application/json'
                    },
                    body: JSON.stringify(dataToSave)
                });

                if (!response.ok) throw new Error(`Erro no servidor: ${response.statusText}`);

                if (isSilent) {
                    const syncTime = new Date().toLocaleTimeString('pt-BR');
                    syncStatus.textContent = `Sincronizado às ${syncTime}`;
                    syncStatus.style.color = '#2ECC71';
                } else {
                    showNotification('Dados atualizados na nuvem com sucesso!', 'success');
                }
                addActivity('fas fa-cloud-upload-alt', 'Dados sincronizados com a nuvem.');

            } catch (error) {
                console.error('Erro ao salvar na nuvem:', error);
                if (isSilent) {
                    syncStatus.textContent = 'Erro de sincronização';
                    syncStatus.style.color = '#E74C3C';
                }
                showNotification('Falha ao sincronizar os dados. Verifique a sua ligação.', 'error');
            }
        }

        async function loadFromCloud(isSilent = false) {
            const syncStatus = document.getElementById('syncStatus');
            syncStatus.textContent = 'A carregar dados da nuvem...';

            try {
                const response = await fetch(`https://jsonblob.com/api/jsonBlob/${FIXED_SYNC_KEY}`);

                if (!response.ok) {
                    if (response.status === 404) throw new Error('Nenhum dado encontrado na nuvem.');
                    throw new Error(`Erro no servidor: ${response.statusText}`);
                }

                const data = await response.json();

                if (!data || !Array.isArray(data.batteries)) {
                    throw new Error('O formato dos dados na nuvem é inválido.');
                }

                if (isSilent) {
                    await applyCloudData(data);
                } else {
                    cloudDataToLoad = data;
                    cloudLoadConfirmModal.classList.add('active');
                }

            } catch (error) {
                console.error('Erro ao carregar da nuvem:', error);
                showNotification(`Falha ao carregar: ${error.message}`, 'error');
                syncStatus.textContent = 'Falha ao carregar da nuvem';
                syncStatus.style.color = '#E74C3C';
            }
        }

        async function applyCloudData(data) {
            try {
                await dbManager.clear('batteries');
                
                for (const battery of data.batteries) {
                    await dbManager.set('batteries', battery);
                }

                await loadFromDB();
                updateUI();

                if (data.batteries.length > 0) {
                    showNotification(`${data.batteries.length} registros carregados da nuvem!`, 'success');
                }
                addActivity('fas fa-cloud-download-alt', 'Dados carregados da nuvem.');

                const syncStatus = document.getElementById('syncStatus');
                const syncTime = new Date(data.savedAt).toLocaleString('pt-BR');
                syncStatus.textContent = `Sincronizado em ${syncTime}`;
                syncStatus.style.color = '#2ECC71';

            } catch (error) {
                console.error('Erro ao aplicar dados da nuvem:', error);
                showNotification('Ocorreu um erro ao aplicar os dados carregados.', 'error');
            } finally {
                cloudDataToLoad = null;
            }
        }
        // #endregion

        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>

