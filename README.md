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

        body {
            background: var(--fabreck-light);
            color: var(--fabreck-dark);
            line-height: 1.6;
            min-height: 100vh;
            padding: 10px; /* Default padding for mobile */
            overflow-x: hidden; /* Evita rolagem horizontal */
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
            position: sticky; /* Fixa o cabeçalho no topo */
            top: 10px;
            z-index: 10;
            border: 1px solid rgba(255, 255, 255, 0.1);
            overflow: hidden; /* Para o padrão de fundo */
            position: relative;
        }

        /* Padrão de fundo no cabeçalho */
        .fabreck-pattern {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.03; /* Transparência */
            pointer-events: none; /* Não interfere com cliques */
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
            position: relative; /* Para ficar acima do padrão */
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
            backdrop-filter: blur(5px); /* Efeito de desfoque */
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
            grid-template-columns: 1fr; /* Default to single column for very small screens */
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
            overflow-x: auto; /* Permite rolagem horizontal em telas pequenas */
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
            width: 40px; /* Largura para o checkbox */
            text-align: center;
        }
        
        th {
            background: rgba(0, 71, 171, 0.1);
            color: var(--fabreck-blue);
            font-weight: 700;
            position: sticky; /* Fixa o cabeçalho da tabela */
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
            background: rgba(243, 156, 18, 0.15); /* fabreck-warning */
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
            background: rgba(46, 204, 113, 0.2); /* Verde mais forte */
            color: var(--fabreck-success);
            border: 1px solid rgba(46, 204, 113, 0.4);
        }
        .action-rejected {
            background: rgba(231, 76, 60, 0.2); /* Vermelho mais forte */
            color: var(--fabreck-danger);
            border: 1px solid rgba(231, 76, 60, 0.4);
        }
        .action-scrapped {
            background: rgba(127, 140, 141, 0.2); /* Cinza */
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
            left: 10px;
            right: 10px;
            padding: 15px 20px;
            border-radius: 12px;
            color: var(--fabreck-white);
            font-weight: 600;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
            z-index: 2100;
            transform: translateY(-120px);
            transition: transform 0.4s ease;
            text-align: center;
        }
        
        .notification.show {
            transform: translateY(0);
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

        /* Media query para tablets e telas maiores (a partir de 600px) */
        @media (min-width: 600px) {
            .form-row {
                grid-template-columns: 1fr 1fr; /* Duas colunas para telas mais largas */
            }
            .filter-row {
                grid-template-columns: 1fr 1fr;
            }
            #finalizadoPage .stat-cards {
                grid-template-columns: repeat(4, 1fr);
            }
        }

        /* Media query para telas de desktop (a partir de 992px) */
        @media (min-width: 992px) {
            body {
                padding: 20px; /* Mais padding nas laterais para desktop */
            }
            .container {
                max-width: 1200px; /* Largura máxima para o conteúdo em desktop */
            }
            header {
                padding: 20px;
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

        /* Estilos aplicados quando o modo desktop é forçado via botão */
        body[data-view-mode="desktop"] {
            padding: 20px; /* Padding consistente quando forçado */
        }

        body[data-view-mode="desktop"] .container {
            max-width: 1200px; /* Força a largura de desktop */
            padding: 0; /* O padding já está no body */
        }

        body[data-view-mode="desktop"] .form-row {
            grid-template-columns: 1fr 1fr; /* Força duas colunas */
        }

        body[data-view-mode="desktop"] .stat-cards {
            grid-template-columns: repeat(3, 1fr); /* FIX: Changed to 3 for the main report */
        }

        body[data-view-mode="desktop"] #finalizadoPage .stat-cards {
            grid-template-columns: repeat(4, 1fr); /* Keep 4 for the finalized page */
        }

        body[data-view-mode="desktop"] table th,
        body[data-view-mode="desktop"] table td {
            font-size: 15px;
            padding: 14px 18px;
        }
        body[data-view-mode="desktop"] .filter-row {
            grid-template-columns: repeat(3, 1fr);
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

    <div id="appWrapper" class="hidden">
        <div class="fabreck-pattern"></div>
        
        <div class="container">
             <header>
                <div class="logo">
                    <img id="logo-img-header" alt="Fabreck Logo" class="logo-img">
                </div>
                <div class="system-title">Sistema de Controle de Garantia na Nuvem com IA</div>
                <div class="company-info">
                    <p><strong>Encarregado de Produção: Reginaldo</strong></p>
                    <p>Análise da Garantia: Jenilton Cruz</p>
                    <p class="active-salesman">Vendedor Ativo: <span id="currentSalesman">N/A</span></p>
                </div>
                <div style="display: flex; justify-content: center; gap: 10px; margin-top: 10px;">
                    <button id="toggleViewModeBtn" class="btn btn-info" style="width: auto;"><i class="fas fa-desktop"></i> <span id="viewModeText">Modo Desktop</span></button>
                    <button id="toggleDarkModeBtn" class="btn btn-info" style="width: auto;"><i class="fas fa-moon"></i> <span id="darkModeText">Modo Escuro</span></button>
                    <button id="logoutBtn" class="btn btn-danger" style="width: auto;"><i class="fas fa-sign-out-alt"></i> Sair</button>
                </div>
            </header>
            
            <!-- Páginas do App (o HTML de cada página está aqui) -->
            <div id="scanPage" class="page active">
               <!-- ... conteúdo da página de scan ... -->
            </div>
            <div id="laudoPage" class="page">
               <!-- ... conteúdo da página de laudo ... -->
            </div>
            <div id="analysisPage" class="page">
                <!-- ... conteúdo da página de análise ... -->
            </div>
            <div id="finalizadoPage" class="page">
               <!-- ... conteúdo da página de finalizados ... -->
            </div>
            <div id="reportPage" class="page">
               <!-- ... conteúdo da página de relatórios ... -->
            </div>
            <div id="settingsPage" class="page">
                <div class="card">
                    <div class="card-header">
                        <h2 class="card-title">Banco de Dados na Nuvem</h2>
                        <div class="card-icon"><i class="fas fa-cloud"></i></div>
                    </div>
                    <div class="info-section">
                       <p>Seus dados estão sendo salvos em tempo real na nuvem. Funções de backup e importação estão disponíveis para maior segurança.</p>
                    </div>
                    <div class="form-row">
                        <button id="backupBtn" class="btn btn-primary"><i class="fas fa-download"></i> Exportar Dados (Backup)</button>
                        <button id="importBtn" class="btn btn-success"><i class="fas fa-upload"></i> Importar Backup Manual</button>
                    </div>
                    <input type="file" id="importFile" accept=".json" style="display: none;">
                </div>
                <div class="card">
                    <div class="card-header">
                        <h2 class="card-title">Instruções de Garantia</h2>
                        <div class="card-icon"><i class="fas fa-info-circle"></i></div>
                    </div>
                    <div id="warrantyInstructionsContainer" class="info-section" style="margin-top:0;"></div>
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
    
    <div id="notification" class="notification"></div>
    <!-- Modais (HTML dos modais está aqui) -->
    
    <!-- Firebase SDK -->
    <script type="module">
        // Import Firebase modules
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";
        import { 
            getFirestore, collection, doc, addDoc, getDoc, setDoc, 
            updateDoc, deleteDoc, onSnapshot, query, where, getDocs, writeBatch 
        } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

        // #region Elementos DOM (Omitido para brevidade)
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
        const importBtn = document.getElementById('importBtn'); 
        const importFileInput = document.getElementById('importFile');
        const reportBody = document.getElementById('reportBody');
        const totalCount = document.getElementById('totalCount');
        const clientFilter = document.getElementById('clientFilter');
        const statusFilter = document.getElementById('statusFilter');
        const workflowStatusFilter = document.getElementById('workflowStatusFilter');
        const applyFilterBtn = document.getElementById('applyFilter');
        const clearFilterBtn = document.getElementById('clearFilter');
        const navBtns = document.querySelectorAll('.nav-btn');
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
        const toggleViewModeBtn = document.getElementById('toggleViewModeBtn');
        const viewModeText = document.getElementById('viewModeText');
        const toggleDarkModeBtn = document.getElementById('toggleDarkModeBtn');
        const darkModeText = document.getElementById('darkModeText');
        const warrantyInstructionsContainer = document.getElementById('warrantyInstructionsContainer');
        const analysisBody = document.getElementById('analysisBody');
        const inAnalysisCount = document.getElementById('inAnalysisCount');
        const analysisClientFilter = document.getElementById('analysisClientFilter');
        const selectAllCheckbox = document.getElementById('selectAllCheckbox');
        const batchAnalyzeBtn = document.getElementById('batchAnalyzeBtn');
        const finalizadoBody = document.getElementById('finalizadoBody');
        const finalizadoCount = document.getElementById('finalizadoCount');
        const finalizedCountReport = document.getElementById('finalizedCountReport');
        const inAnalysisCountReport = document.getElementById('inAnalysisCountReport');
        const finalizadoAprovadaCount = document.getElementById('finalizadoAprovadaCount');
        const finalizadoReprovadaPrazoCount = document.getElementById('finalizadoReprovadaPrazoCount');
        const finalizadoReprovadaForaCount = document.getElementById('finalizadoReprovadaForaCount');
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
        const nameEditModal = document.getElementById('nameEditModal');
        const closeNameEditModal = document.getElementById('closeNameEditModal');
        const editClientNameInput = document.getElementById('editClientName');
        const editSalesmanNameInput = document.getElementById('editSalesmanName');
        const saveNameEditBtn = document.getElementById('saveNameEditBtn');
        const observationModal = document.getElementById('observationModal');
        const closeObservationModal = document.getElementById('closeObservationModal');
        const observationText = document.getElementById('observationText');
        const saveObservationBtn = document.getElementById('saveObservationBtn');
        const videoAnalysisOptions = document.getElementById('videoAnalysisOptions');
        const procedenteBtn = document.getElementById('procedenteBtn');
        const descarregadaBtn = document.getElementById('descarregadaBtn');
        const addObservationBtn = document.getElementById('addObservationBtn');
        const procedenteModal = document.getElementById('procedenteModal');
        const closeProcedenteModal = document.getElementById('closeProcedenteModal');
        const capacidadeBaixaBtn = document.getElementById('capacidadeBaixaBtn');
        const ccaBaixoBtn = document.getElementById('ccaBaixoBtn');
        const aiAssistantFab = document.getElementById('aiAssistantFab');
        const aiModal = document.getElementById('aiModal');
        const aiModalClose = document.getElementById('aiModalClose');
        const aiChatBox = document.getElementById('aiChatBox');
        const aiChatInput = document.getElementById('aiChatInput');
        const aiSendBtn = document.getElementById('aiSendBtn');
        const generateLaudoBtn = document.getElementById('generateLaudoBtn');
        const laudoResultContainer = document.getElementById('laudoResultContainer');
        const laudoResult = document.getElementById('laudoResult');
        const copyLaudoBtn = document.getElementById('copyLaudoBtn');
        const loginOverlay = document.getElementById('loginOverlay');
        const appWrapper = document.getElementById('appWrapper');
        const loginForm = document.getElementById('loginForm');
        const usernameInput = document.getElementById('usernameInput');
        const passwordInput = document.getElementById('passwordInput');
        const logoutBtn = document.getElementById('logoutBtn');
        // #endregion

        // #region Firebase & Estado do Sistema
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_AUTH_DOMAIN",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_STORAGE_BUCKET",
            messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
            appId: "YOUR_APP_ID"
        };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        
        let dbRefs = {};
        let userId = null;
        let stream = null;
        let scanning = false;
        const VIEW_MODE_KEY = 'fabreck_view_mode';
        const DARK_MODE_KEY = 'fabreck_dark_mode';
        const MIGRATION_KEY = `fabreck_migration_v9_${appId}`;
        let batteryData = [];
        let activityData = [];
        let currentFilters = { client: '', status: '', workflowStatus: '', startDate: '', endDate: '', code: '' };
        let warrantyInstructions = {};
        let currentFacingMode = 'environment';
        let scanHistory = [];
        let audioContext = null;
        let beepSound = null;
        let rulesShown = localStorage.getItem('rulesShown') === 'true';
        let analysisMode = 'single';
        let analyzingBatteryId = null;
        let batchAnalysisIds = [];
        let editingBatteryInfo = null;
        let observationCallback = null;
        let tempObservation = '';
        let currentViewMode = localStorage.getItem(VIEW_MODE_KEY) || 'auto';
        let isDarkMode = localStorage.getItem(DARK_MODE_KEY) === 'true';
        let aiChatHistory = [];
        const logoBase64 = 'data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNDAiIGhlaWdodD0iNDAiIHZpZXdCb3g9IjAgMCAyNDAgNDAiPjxyZWN0IHdpZHRoPSIyNDAiIGhlaWdodD0iNDAiIGZpbGw9IiMwMDQ3QUIiLz48dGV4dCB4PSIxMjAiIHk9IjI1IiBmb250LWZhbWlseT0iU2Vnb2UgVUksIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iMjAiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjRkZGRkZGIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5GQUJSRUNLIERPIEJSQVNJTDwvdGV4dD48L3N2Zz4=';
        const batteryModelMap = { 'A': 'FA6AD', 'B': 'FA4D', 'C': 'FA5AD', 'D': 'FA5D', 'E': 'FA5,5D', 'F': 'FA6D', 'G': 'FA7E', 'H': 'FA8AE', 'I': 'FA8E', 'J': 'FA7AE', 'K': 'FA7D' };
        let unsubscribeBatteries = null;
        let unsubscribeActivity = null;
        const ADMIN_USER = 'JENILTON';
        const ADMIN_PASS = '2582';
        // #endregion

        // #region Funções de UI e Auxiliares
        function showNotification(message, type, duration = 3000) { /* ... */ }
        function applyViewMode() { /* ... */ }
        function toggleViewMode() { /* ... */ }
        function applyDarkMode() { /* ... */ }
        function toggleDarkMode() { /* ... */ }
        function showPage(pageId) { /* ... */ }
        function showRulesModal() { /* ... */ }
        function setupAudio() { /* ... */ }
        function playBeep() { /* ... */ }
        function updateActivityLog() { /* ... */ }
        async function addActivity(icon, message) { /* ... */ }
        async function addToScanHistory(code) { /* ... */ }
        function updateScanHistory() { /* ... */ }
        function validateSerialCode(code) { /* ... */ }
        function getBatteryModelFromCode(code) { /* ... */ }
        function getManufacturingDate(week, year) { /* ... */ }
        function calculateWarrantyStatus(week, year) { /* ... */ }
        function updateWarrantyDebugInfo(code) { /* ... */ }
        function updateUI() { /* ... */ }
        function updateStats() { /* ... */ }
        function updateClientFilter() { /* ... */ }
        function updateAnalysisClientFilter() { /* ... */ }
        function getFilteredData() { /* ... */ }
        function updateReportTable(filteredData) { /* ... */ }
        function updateAnalysisTable() { /* ... */ }
        function updateFinalizadoTable() { /* ... */ }
        function updateReportTotals(data) { /* ... */ }
        function updateModelStatusSummary(data) { /* ... */ }
        function handleSelectAll() { /* ... */ }
        function updateBatchAnalyzeButtonState() { /* ... */ }
        function updateWarrantyInstructionsUI() { /* ... */ }
        async function openSingleAnalysisModal(id) { /* ... */ }
        function openBatchAnalysisModal() { /* ... */ }
        function saveAnalysis() { /* ... */ }
        async function saveSingleAnalysis() { /* ... */ }
        async function saveBatchAnalysis() { /* ... */ }
        async function openNameEditModal(id) { /* ... */ }
        async function saveEditedNames() { /* ... */ }
        function openObservationModal(currentObservation, callback) { /* ... */ }
        function handleSerialInput() { /* ... */ }
        function clearCode() { /* ... */ }
        async function selectWarrantyType(type) { /* ... */ }
        function handleScanClick() { /* ... */ }
        async function startCamera() { /* ... */ }
        function stopCamera() { /* ... */ }
        function switchCamera() { /* ... */ }
        async function captureImage() { /* ... */ }
        function applyFilters() { /* ... */ }
        function clearFilters() { /* ... */ }
        function drawPdfHeader(doc, clientName, salesmanName) { /* ... */ }
        function buildClientPDFPage(doc, clientName, clientBatteries, startY) { /* ... */ }
        function generatePDF() { /* ... */ }
        function generateBatchPDFs() { /* ... */ }
        function previewPDF() { /* ... */ }
        function downloadPDF() { /* ... */ }
        function exportToFormattedExcel() { /* ... */ }
        function openAIAssistant() { /* ... */ }
        function closeAIAssistant() { /* ... */ }
        function addMessageToAIChat(sender, message) { /* ... */ }
        async function sendAIChatMessage() { /* ... */ }
        async function generateLaudo() { /* ... */ }
        function copyLaudo() { /* ... */ }
        // #endregion

        // #region Inicialização e Lógica Principal
        document.addEventListener('DOMContentLoaded', () => {
            console.log("Inicializando sistema...");
            document.getElementById('logo-img-header').src = logoBase64;
            document.getElementById('logo-img-login').src = logoBase64;
            setupEventListeners();
            checkAuth(); 
        });

        function checkAuth() {
            if (sessionStorage.getItem('isAuthenticated') === 'true') {
                loginOverlay.classList.add('hidden');
                appWrapper.classList.remove('hidden');
                initializeAppLogic();
            } else {
                loginOverlay.classList.remove('hidden');
                appWrapper.classList.add('hidden');
            }
        }
        
        async function initializeAppLogic() {
             onAuthStateChanged(auth, async (user) => {
                if (user) {
                    userId = user.uid;
                    console.log(`Usuário autenticado: ${userId}`);
                    
                    dbRefs.batteries = collection(db, `/artifacts/${appId}/public/data/batteries`);
                    dbRefs.activity = collection(db, `/artifacts/${appId}/public/data/activity`);
                    dbRefs.settings = collection(db, `/artifacts/${appId}/users/${userId}/settings`);
                    
                    await loadUserSettings();
                    attachFirestoreListeners();
                    setupAudio();
                    applyViewMode();
                    applyDarkMode();

                    const today = new Date();
                    submissionDateInput.value = `${today.getFullYear()}-${String(today.getMonth() + 1).padStart(2, '0')}-${String(today.getDate()).padStart(2, '0')}`;
                    
                    showNotification("Conectado à nuvem com sucesso!", "success");
                } else {
                     try {
                        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                            await signInWithCustomToken(auth, __initial_auth_token);
                        } else {
                            await signInAnonymously(auth);
                        }
                    } catch (error) {
                        console.error("Erro na autenticação anônima:", error);
                        showNotification("Falha ao conectar à nuvem.", "error");
                    }
                }
            });
        }

        function setupEventListeners() {
            loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const user = usernameInput.value.trim().toUpperCase();
                const pass = passwordInput.value.trim();
                if (user === ADMIN_USER && pass === ADMIN_PASS) {
                    sessionStorage.setItem('isAuthenticated', 'true');
                    checkAuth();
                } else {
                    showNotification('Utilizador ou palavra-passe incorretos.', 'error');
                }
            });

            logoutBtn.addEventListener('click', () => {
                sessionStorage.removeItem('isAuthenticated');
                checkAuth();
            });
            // ... (restante dos event listeners)
        }

        function attachFirestoreListeners() {
            if (unsubscribeBatteries) unsubscribeBatteries();
            unsubscribeBatteries = onSnapshot(query(dbRefs.batteries), async (snapshot) => {
                batteryData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                console.log(`[Firestore] ${batteryData.length} baterias carregadas.`);
                await runInitialMigration();
                updateUI();
            }, error => console.error("Erro ao ouvir atualizações:", error));

            if (unsubscribeActivity) unsubscribeActivity();
            unsubscribeActivity = onSnapshot(query(dbRefs.activity), (snapshot) => {
                activityData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                updateActivityLog();
            });
        }

        async function loadUserSettings() {
            // ... (código inalterado)
        }
        
        async function runInitialMigration() {
            const migrationDone = localStorage.getItem(MIGRATION_KEY);
            if (migrationDone) {
                console.log("Migração de dados já foi realizada.");
                return;
            }
            
            showNotification(`Iniciando migração de 724 registros...`, "info", 10000);
            
            const preloadedData = [{"id":1753585232692,"client":"MOTOCRISS","salesman":"VITOR","code":"3124A0124","batteryModel":"FA6AD","manufDate":"29/07/2024","warranty_period_status":"expired","status":"finalized","warrantyType":"factory","recommendation":"Fora do prazo","finalAction":"REPROVADA - SUCATEAR","observations":"","timestamp":"2025-07-27T03:00:32.692Z","submissionDate":"2025-08-01T05:26:21.594Z","technicalOpinionDate":"2025-08-19T11:27:02.478Z"}]; // Adicione todos os 724 registros aqui

            try {
                const batch = writeBatch(db);
                let addedCount = 0;

                for (const battery of preloadedData) {
                    const q = query(dbRefs.batteries, where("code", "==", battery.code));
                    const querySnapshot = await getDocs(q);
                    if (querySnapshot.empty) {
                        const { id, ...batteryDataToImport } = battery;
                        const newDocRef = doc(collection(db, dbRefs.batteries.path));
                        batch.set(newDocRef, batteryDataToImport);
                        addedCount++;
                    }
                }

                if (addedCount > 0) {
                    await batch.commit();
                }

                localStorage.setItem(MIGRATION_KEY, 'true');
                showNotification(`Migração concluída! ${addedCount} novos registros adicionados.`, "success");
                if (addedCount > 0) addActivity('fas fa-cloud-upload-alt', `${addedCount} baterias migradas.`);
            } catch (error) {
                console.error("Falha na migração:", error);
                showNotification("Erro na migração automática.", "error");
            }
        }
        // ... (resto do código JS, funções de CRUD, UI, etc.)
    </script>
</body>
</html>

