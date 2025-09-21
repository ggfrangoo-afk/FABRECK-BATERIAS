<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>FABRECK DO BRASIL - App de Garantia</title>
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

            /* Cores para o Modo Escuro Aprimorado */
            --dark-bg: #0d0d0d;
            --dark-card-bg: #1a1a1a;
            --dark-text: #E0E0E0;
            --dark-light-text: #B0B0B0;
            --dark-border: #333333;
            --dark-header-bg-start: #002244;
            --dark-header-bg-end: #000000;
            --dark-blue-light: #0066cc;
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
            overflow: hidden; /* Previne scroll no body, a rolagem será interna */
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

        /* --- LAYOUT PRINCIPAL DINÂMICO --- */
        #layoutContainer {
            display: flex;
            height: 100vh;
            width: 100%;
        }

        #sidebar {
            width: 260px;
            background: var(--fabreck-dark);
            color: var(--fabreck-white);
            display: none; /* Escondido por padrão, visível em desktop */
            flex-direction: column;
            padding: 20px;
            box-shadow: 5px 0 15px rgba(0,0,0,0.1);
            transition: background 0.3s ease;
        }
        
        #mainContentWrapper {
            flex: 1;
            overflow-y: auto; /* Permite scroll apenas na área de conteúdo */
            position: relative;
            padding: 10px;
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
        }
        
        /* Footer de navegação (Mobile) */
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
            transition: background 0.3s ease, border-top 0.3s ease;
        }
        
        /* Controle de páginas (abas) */
        .page {
            display: none;
            animation: fadeIn 0.3s ease-in-out;
        }
        .page.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- MODO DESKTOP (Telas > 992px) --- */
        @media (min-width: 992px) {
            #sidebar {
                display: flex; /* Mostra a sidebar */
            }
            .footer {
                display: none; /* Esconde o footer mobile */
            }
            #mainContentWrapper {
                padding: 20px;
            }
             .container {
                width: 100%;
                max-width: none;
                margin: 0;
            }
        }
        
        /* --- MODO MOBILE (Telas < 992px) --- */
        @media (max-width: 991px) {
            #mainContentWrapper {
                padding-bottom: 90px; /* Garante espaço para o footer não sobrepor o conteúdo */
            }
        }


        body.dark-mode #sidebar {
            background: var(--dark-card-bg);
            border-right: 1px solid var(--dark-border);
        }
        
        /* ... (restante do seu CSS permanece o mesmo) ... */
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


        /* Estilos para o Modo Escuro */
        body.dark-mode {
            background: var(--dark-bg);
            color: var(--dark-text);
        }

        body.dark-mode .status-badge {
            color: #FFFFFF; /* Garante que o texto seja sempre branco para contraste */
        }

        body.dark-mode .status-warranty,
        body.dark-mode .action-approved {
            background: #27ae60; /* Verde mais escuro e sólido */
            border: none;
        }
        
        body.dark-mode .status-expired,
        body.dark-mode .action-rejected {
            background: #c0392b; /* Vermelho mais escuro e sólido */
            border: none;
        }

        body.dark-mode .status-factory {
            background: #2980b9; /* Azul mais escuro e sólido */
            border: none;
        }
        
        body.dark-mode .status-in-analysis {
            background: #f39c12; /* Laranja sólido */
            border: none;
        }

        body.dark-mode .status-finalized,
        body.dark-mode .action-scrapped {
            background: #8e44ad; /* Roxo sólido */
            border: none;
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
        
        /* Estilos do cabeçalho */
        header {
            background: linear-gradient(135deg, var(--fabreck-blue), var(--fabreck-dark));
            padding: 15px;
            border-radius: 16px;
            margin-bottom: 15px;
            text-align: center;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
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
        
        .card {
            background: var(--fabreck-white);
            border-radius: 16px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.05);
            padding: 20px;
            border: 1px solid rgba(0, 0, 0, 0.05);
            transition: transform 0.3s ease, box-shadow 0.3s ease, background 0.3s ease, color 0.3s ease;
            margin-bottom: 15px;
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
        
        /* Estilos da tabela de relatório */
        .table-container {
            overflow-x: auto;
            margin-top: 15px;
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
            z-index: 1;
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
        
        .hidden {
            display: none;
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

        .form-section {
            margin-bottom: 20px;
            border-bottom: 1px solid #eee;
            padding-bottom: 15px;
        }
         body.dark-mode .form-section {
            border-bottom-color: var(--dark-border);
         }
        .form-section h3 {
            color: var(--fabreck-blue);
            margin-bottom: 15px;
            font-size: 16px;
        }
         body.dark-mode .form-section h3 {
            color: var(--dark-blue-light);
         }

        .checkbox-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 10px;
            margin-bottom: 15px;
        }
        .checkbox-group {
            display: flex;
            align-items: center;
        }
        .checkbox-group input {
            margin-right: 10px;
        }

        #laudoImagePreviews {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
            gap: 10px;
            margin-top: 15px;
        }
        .preview-container {
            position: relative;
            width: 100%;
            padding-top: 100%; /* Aspect ratio 1:1 */
            border: 1px dashed var(--fabreck-gray);
            border-radius: 8px;
            overflow: hidden;
        }
        .preview-container img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .remove-img-btn {
            position: absolute;
            top: 5px;
            right: 5px;
            background: rgba(231, 76, 60, 0.8);
            color: white;
            border: none;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            cursor: pointer;
            font-size: 14px;
            line-height: 24px;
            text-align: center;
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
            .total-box {
                min-width: 200px;
            }
            .filter-row {
                grid-template-columns: repeat(3, 1fr);
            }
        }

        /* --- NOVOS ESTILOS PARA WIDESCREEN OTIMIZADO --- */
        @media (min-width: 1200px) {
            /* Aplica um layout de 2 colunas para a página de Registro */
            #scanPage {
                display: grid;
                grid-template-columns: minmax(450px, 1.2fr) 1fr; /* Coluna do formulário um pouco maior */
                gap: 20px;
                align-items: start;
            }
            #scanPage .card {
                margin-bottom: 0;
            }
            
            #laudoPage .card {
                max-width: 900px;
                margin: 0 auto;
            }

            /* Reorganiza a página de Análise para melhor uso do espaço */
            #analysisPage > .card {
                display: grid;
                grid-template-columns: 300px 1fr; /* Coluna lateral para filtros e stats */
                gap: 20px;
                align-items: start;
            }
            /* Posiciona os elementos dentro do grid da página de Análise */
            #analysisPage .card-header { grid-column: 1 / -1; } /* Header ocupa a largura toda */
            #analysisPage .info-section { grid-column: 1 / 2; }
            #analysisPage .stat-cards { grid-column: 1 / 2; }
            #analysisPage .batch-actions { grid-column: 1 / 2; }
            #analysisPage .table-container {
                grid-column: 2 / 3;
                grid-row: 2 / 6; /* Ocupa as linhas ao lado dos filtros */
                margin-top: 0;
                max-height: 70vh; /* Permite que a tabela cresça com a altura da tela */
            }

            /* Aplica layout de 2 colunas para a página de Ajustes */
            #settingsPage {
                display: grid;
                grid-template-columns: 1fr 1fr;
                gap: 20px;
                align-items: start;
            }

            /* Melhora o layout dos filtros no relatório em telas muito largas */
            #reportPage .filter-row {
                grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
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
            <div style="margin-top: 15px; border-top: 1px solid #e0e0e0; padding-top: 15px;">
                 <button id="viewerLoginBtn" class="btn btn-info" style="margin-top:0;">Acesso Rápido (Somente Consulta)</button>
            </div>
        </div>
    </div>

    <div id="layoutContainer" class="hidden">
        <!-- Sidebar para Desktop -->
        <nav id="sidebar">
            <div class="sidebar-header">
                <img id="logo-img-sidebar" alt="Fabreck Logo" class="logo-img">
                <h3>Garantia</h3>
            </div>
            <div class="sidebar-nav">
                <a href="#" class="sidebar-btn active" data-page="scanPage"><i class="fas fa-bolt"></i><span>Registo</span></a>
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
            <div class="container">
                <header>
                    <div class="logo">
                        <img id="logo-img-header" alt="Fabreck Logo" class="logo-img">
                    </div>
                    <div class="system-title">Sistema de Controle de Garantia</div>
                    
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
            
                <!-- Página de Registo de Garantia -->
                <div id="scanPage" class="page active">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Registo de Garantia</h2>
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
                        
                        <div id="videoAnalysisOptions" class="hidden viewer-hidden">
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
                                <button id="addBtn" class="btn btn-primary viewer-hidden">
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
                        
                        <div class="form-row viewer-hidden" style="margin-top: 10px;">
                             <button id="clearFormBtn" class="btn btn-danger">
                                <i class="fas fa-eraser"></i> Limpar Código
                            </button>
                        </div>
                        
                        <div class="activity-log" id="activityLog"></div>
                    </div>

                    <!-- NOVO CARD COM O LOG DE ÚLTIMOS REGISTOS -->
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Últimos Registos</h2>
                            <div class="card-icon"><i class="fas fa-history"></i></div>
                        </div>
                        <div class="table-container" style="margin-top:0; max-height: 65vh;">
                            <table id="recentRegistrationsTable">
                                <thead>
                                    <tr>
                                        <th>Código</th>
                                        <th>Cliente</th>
                                        <th>Vendedor</th>
                                        <th>Data</th>
                                    </tr>
                                </thead>
                                <tbody id="recentRegistrationsBody"></tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- PÁGINA DE LAUDO TÉCNICO (AGORA SEM IA) -->
                <div id="laudoPage" class="page">
                    <div class="card">
                        <div class="card-header">
                            <h2 class="card-title">Gerador de Laudo Técnico Profissional</h2>
                            <div class="card-icon"><i class="fas fa-file-alt"></i></div>
                        </div>
                        <div class="info-section">
                            <p>Preencha os dados abaixo para gerar um laudo técnico detalhado em PDF para o cliente.</p>
                        </div>

                        <div class="form-section">
                            <h3>1. Dados de Identificação</h3>
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
                            <div class="form-group">
                                <label for="laudoBatteryModel">Modelo da Bateria</label>
                                <input type="text" id="laudoBatteryModel" class="form-control" placeholder="Ex: FA6AD">
                            </div>
                        </div>

                        <div class="form-section">
                            <h3>2. Parâmetros de Teste</h3>
                            <div class="form-row">
                                <div class="form-group">
                                    <label for="laudoBatteryCCA_Nominal">CA Nominal (Padrão)</label>
                                    <input type="number" id="laudoBatteryCCA_Nominal" class="form-control" value="120">
                                </div>
                                <div class="form-group">
                                    <label for="laudoCA_Medido">CA Medido (A)</label>
                                    <input type="number" id="laudoCA_Medido" class="form-control" placeholder="Valor do teste">
                                </div>
                            </div>
                            <div class="form-row">
                                <div class="form-group">
                                    <label for="laudoBatteryVoltage_Nominal">Tensão Nominal (V)</label>
                                    <input type="text" id="laudoBatteryVoltage_Nominal" class="form-control" value="12.4V" readonly>
                                </div>
                                <div class="form-group">
                                    <label for="laudoVoltage_Medida">Tensão Medida (V)</label>
                                    <input type="number" id="laudoVoltage_Medida" class="form-control" placeholder="Ex: 12.5">
                                </div>
                            </div>
                        </div>

                        <div class="form-section">
                            <h3>3. Inspeção Visual</h3>
                            <div class="checkbox-grid">
                                <div class="checkbox-group"><input type="checkbox" id="checkEstufada"> <label for="checkEstufada">Caixa Estufada/Danificada</label></div>
                                <div class="checkbox-group"><input type="checkbox" id="checkPolos"> <label for="checkPolos">Polos Danificados/Oxidados</label></div>
                                <div class="checkbox-group"><input type="checkbox" id="checkVazamento"> <label for="checkVazamento">Sinais de Vazamento</label></div>
                            </div>
                            <div class="form-group">
                                <label for="laudoVisualInspection">Outras Observações Visuais</label>
                                <textarea id="laudoVisualInspection" class="form-control" rows="2" placeholder="Ex: Caixa riscada, falta de etiqueta..."></textarea>
                            </div>
                        </div>

                        <div class="form-section">
                            <h3>4. Diagnósticos Técnicos Adicionais</h3>
                            <p class="info-section" style="margin-top:0; font-size:12px;">Selecione os diagnósticos relevantes. Eles serão formatados e adicionados ao laudo.</p>
                            <div class="checkbox-grid">
                                <div class="checkbox-group"><input type="checkbox" id="checkFugaCorrente"> <label for="checkFugaCorrente">Veículo com fuga de corrente</label></div>
                                <div class="checkbox-group"><input type="checkbox" id="checkSobretensao"> <label for="checkSobretensao">Sistema de recarga com sobretensão</label></div>
                                <div class="checkbox-group"><input type="checkbox" id="checkSubtensao"> <label for="checkSubtensao">Sistema de recarga com subtensão</label></div>
                                <div class="checkbox-group"><input type="checkbox" id="checkAplicacaoIncorreta"> <label for="checkAplicacaoIncorreta">Aplicação incorreta para o veículo</label></div>
                                <div class="checkbox-group"><input type="checkbox" id="checkLongoDesuso"> <label for="checkLongoDesuso">Sinais de longo período sem uso</label></div>
                            </div>
                             <div class="form-group">
                                <label for="laudoTechnicianNotes">Outras Notas (Opcional)</label>
                                <textarea id="laudoTechnicianNotes" class="form-control" rows="2" placeholder="Adicione aqui qualquer detalhe único não listado acima..."></textarea>
                            </div>
                        </div>
                        
                        <div class="form-section">
                            <h3>5. Evidências Fotográficas</h3>
                            <p class="info-section" style="margin-top:0; font-size:12px;">Adicione até 3 imagens do produto. Elas serão incluídas no PDF final.</p>
                            <button id="addLaudoImageBtn" class="btn btn-info"><i class="fas fa-camera"></i> Adicionar Imagens</button>
                            <input type="file" id="laudoImageUpload" accept="image/*" multiple style="display: none;">
                            <div id="laudoImagePreviews"></div>
                        </div>

                        <button id="generateLaudoPdfBtn" class="btn btn-primary" style="margin-top: 20px;"><i class="fas fa-file-pdf"></i> Visualizar Laudo em PDF</button>
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
                            <button id="batchAnalyzeBtn" class="btn btn-success viewer-hidden" disabled>
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
                                        <th class="actions-cell">Ação</th>
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
                            <p>Esta é a lista completa de <strong>todas</strong> as garantias registadas. Use os filtros abaixo para visualizar grupos específicos.</p>
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
                                    <label for="startDate">Data Inicial (Registo):</label>
                                    <input type="date" id="startDate" class="form-control">
                                </div>
                                <div class="filter-group">
                                    <label for="endDate">Data Final (Registo):</label>
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
                                        <th class="actions-cell">Ações</th>
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
                            <button id="clearBtn" class="btn btn-danger viewer-hidden"><i class="fas fa-trash-alt"></i> Limpar Tudo</button>
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
                        
                        <div class="form-row viewer-hidden">
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

                    <div class="card viewer-hidden">
                        <div class="card-header">
                            <h2 class="card-title">Segurança</h2>
                            <div class="card-icon"><i class="fas fa-shield-alt"></i></div>
                        </div>
                        <div class="info-section" style="margin-top:0;">
                            <p>Altere a sua palavra-passe de administrador regularmente para manter a segurança da sua conta.</p>
                        </div>
                        <button id="changePasswordBtn" class="btn btn-warning" style="margin-top: 20px;"><i class="fas fa-key"></i> Alterar Palavra-passe</button>
                    </div>

                </div>
            </div>
            
            <div class="footer">
                <div class="nav-btn active" data-page="scanPage">
                    <div><i class="fas fa-bolt"></i></div>
                    <span>Registo</span>
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
            <p style="text-align: center; font-size: 14px; margin-bottom: 15px;">A alteração será aplicada a todos os registos com estes nomes.</p>
            
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

    <!-- NOVO MODAL PARA ALTERAR PALAVRA-PASSE -->
    <div class="edit-modal" id="passwordModal">
        <div class="edit-content">
            <span class="edit-close" id="closePasswordModal">&times;</span>
            <h3 class="edit-title">Alterar Palavra-passe</h3>
            <form id="passwordChangeForm">
                <div class="form-group">
                    <label for="currentPassword">Palavra-passe Atual</label>
                    <input type="password" id="currentPassword" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="newPassword">Nova Palavra-passe</label>
                    <input type="password" id="newPassword" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="confirmNewPassword">Confirmar Nova Palavra-passe</label>
                    <input type="password" id="confirmNewPassword" class="form-control" required>
                </div>
                <button type="submit" id="savePasswordBtn" class="btn btn-success"><i class="fas fa-save"></i> Guardar Nova Palavra-passe</button>
            </form>
        </div>
    </div>


    <script>
        // #region Elementos DOM
        const clientNameInput = document.getElementById('clientName');
        const salesmanNameInput = document.getElementById('salesmanName');
        const serialCodeInput = document.getElementById('serialCode');
        const addBtn = document.getElementById('addBtn');
        const clearFormBtn = document.getElementById('clearFormBtn');
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

        // NOVO: Elemento para o log de registos
        const recentRegistrationsBody = document.getElementById('recentRegistrationsBody');

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
        
        // Elementos do Laudo Técnico
        const generateLaudoPdfBtn = document.getElementById('generateLaudoPdfBtn');
        
        // Elementos do Login e Layout
        const loginOverlay = document.getElementById('loginOverlay');
        const layoutContainer = document.getElementById('layoutContainer');
        const loginForm = document.getElementById('loginForm');
        const usernameInput = document.getElementById('usernameInput');
        const passwordInput = document.getElementById('passwordInput');
        const logoutBtn = document.getElementById('logoutBtn');
        const sidebarToggleBtn = document.getElementById('sidebar-toggle-btn');
        const viewerLoginBtn = document.getElementById('viewerLoginBtn');
        
        // Elementos de alteração de senha
        const changePasswordBtn = document.getElementById('changePasswordBtn');
        const passwordModal = document.getElementById('passwordModal');
        const closePasswordModal = document.getElementById('closePasswordModal');
        const passwordChangeForm = document.getElementById('passwordChangeForm');
        
        // Elementos do Laudo
        const addLaudoImageBtn = document.getElementById('addLaudoImageBtn');
        const laudoImageUpload = document.getElementById('laudoImageUpload');
        const laudoImagePreviews = document.getElementById('laudoImagePreviews');
        // #endregion

        // #region Estado do Sistema
        const DB_KEY = 'fabreck_battery_db_v21'; // Versão antiga para migração
        const ACTIVITY_KEY = 'fabreck_activity_log_v12';
        const LAST_SALESMAN_KEY = 'fabreck_last_salesman_v12';
        const LAST_CLIENT_KEY = 'fabreck_last_client_v12';
        const LAST_WARRANTY_TYPE_KEY = 'fabreck_last_warranty_type_v12';
        const DARK_MODE_KEY = 'fabreck_dark_mode'; 
        const INSTRUCTIONS_KEY = 'fabreck_instructions_v1';
        const SIDEBAR_COLLAPSED_KEY = 'fabreck_sidebar_collapsed_v1';
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
        let cloudDataToLoad = null; 
        let syncDebounceTimer = null;
        let laudoImages = []; // Array para guardar as imagens do laudo

        // Credenciais Chave
        const ADMIN_USERS_KEY = 'fabreck_admin_users';
        const FIXED_SYNC_KEY = '1418850998876823552';
        
        const logoBase64 = 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzIwIiBoZWlnaHQ9IjgyIiB2aWV3Qm94PSIwIDAgMzIwIDgyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxnIGlkPSJMb2dvIj48cmVjdCBpZD0iQm90dG9tQmFyIiB5PSI1NyIgd2lkdGg9IjMyMCIgaGVpZGg0PSIyNSIgZmlsbD0iIzAwNDdhYiIvPjxwYXRoIGlkPSJTdHJpcGUxMiIgZD0iTTI5MCA1N0gzMTVMMzA1IDgySDI4MFoiIGZpbGw9IndoaXRlIiAvPjxwYXRoIGlkPSJTdHJpcGUxMSIgZD0iTTI0NSA1N0gyNzBMMjYwIDgySDIzNVoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTEwIiBkPSJNMjAwIDU3SDIyNUwyMTUgODJIMTkwWiIgZmlsbD0id2hpdGUiLz48cGF0aCBpZD0iU3RyaXBlOSIgZD0iTTE1NSA1N0gxODBMMTcwIDgySDE0NVoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTgiIGQ9Ik0xMTAgNTdIMTM1TDEyNSA4MkgxMDBaIiBmaWxsPSJ3aGl0ZSIvPjxwYXRoIGlkPSJTdHJpcGU3IiBkPSJNNjUgNTdIOU9MDgwIDgySDQ1WiIgZmlsbD0id2hpdGUiLz48cGF0aCBpZD0iU3RyaXBlNiIgZD0iTTIwIDU3SDQ1TDM1IDgySDEwWiIgZmlsbD0id2hpdGUiLz48cmVjdCBpZD0iVG9wQmFyIiB3aWR0aD0iMzIwIiBoZWlnaHQ9IjI1IiBmaWxsPSIjMDA0N2FiIi8+PHBhdGggaWQ9IlN0cmlwZTUiIGQ9Ik0yOTAgMEgzMTVMMzA1IDI1SDI4MFoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTQiIGQ9Ik0yNDUgMEgyNzBMMjYwIDI1SDIzNVoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTMiIGQ9Ik0yMDAgMEgyMjVMMjE1IDI1SDE5MFoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTIiIGQ9Ik0xNTUgMEgxODBMMTcwIDI1SDE0NVoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTEiIGQ9Ik0xMTAgMEgxMzVMMTI1IDI1SDEwMFoiIGZpbGw9IndoaXRlIi8+PHBhdGggaWQ9IlN0cmlwZTAiIGQ9Ik02NSAwSDkwTDgwIDI1SDQ1WiIgZmlsbD0id2hpdGUiLz48cGF0aCBpZD0iU3RyaXBlLTEiIGQ9Ik0yMCAwSDQ1TDM1IDI1SDEwWiIgZmlsbD0id2hpdGUiLz48dGV4dCBpZD0iVGV4dCIgeD0iMTYwIiB5PSI1OCIgZm9udC1mYW1pbHk9IkltcGFjdCwgQXJpYWwgQmxhY2ssIHNhbnMtc2VyaWYiIGZvbnQtd2VpZ2h0PSI5MDAiIGZvbnQtcIzI9IjQ4IiBmaWxsPSIjRDIyNjMwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBzdHJva2U9IiNGRkZGRkYiIHN0cm9rZS13aWR0aD0iMi41IiBwYWludC1vcmRlcj0ic3Ryb2tlIj5GQUJSRUNLPC90ZXh0PjwvZz48L3N2Zz4=';

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
            applySidebarState();
            await dbPromise; // Garante que a BD está pronta antes de qualquer operação
            await initializeCredentials();
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
                applyUIRestrictions();
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

            if (loginForm) {
                loginForm.addEventListener('submit', async (e) => {
                    e.preventDefault();
                    const user = usernameInput.value.trim().toUpperCase();
                    const pass = passwordInput.value.trim();

                    const adminUsersData = await dbManager.get('settings', ADMIN_USERS_KEY);
                    const adminUsers = adminUsersData ? adminUsersData.value : [];
                    
                    const foundUser = adminUsers.find(admin => admin.user === user && admin.pass === pass);
                    
                    if (foundUser) {
                        sessionStorage.setItem('isAuthenticated', 'true');
                        sessionStorage.setItem('userType', 'admin');
                        sessionStorage.setItem('currentUser', user); // Guarda o utilizador atual
                        checkAuth();
                    } else {
                        showNotification('Utilizador ou palavra-passe incorretos.', 'error');
                    }
                });
            }

            if (viewerLoginBtn) {
                viewerLoginBtn.addEventListener('click', () => {
                    sessionStorage.setItem('isAuthenticated', 'true');
                    sessionStorage.setItem('userType', 'viewer');
                    checkAuth();
                });
            }

            if (logoutBtn) {
                logoutBtn.addEventListener('click', () => {
                    sessionStorage.removeItem('isAuthenticated');
                    sessionStorage.removeItem('userType');
                    sessionStorage.removeItem('currentUser');
                    checkAuth();
                });
            }
            
            navBtns.forEach(btn => {
                if (btn) btn.addEventListener('click', () => showPage(btn.getAttribute('data-page')));
            });

            sidebarBtns.forEach(btn => {
                if (btn) btn.addEventListener('click', (e) => {
                    e.preventDefault();
                    showPage(btn.getAttribute('data-page'));
                });
            });
            
            if (serialCodeInput) {
                serialCodeInput.addEventListener('input', handleSerialInput);
                serialCodeInput.addEventListener('keypress', e => { if (e.key === 'Enter') addBattery(); });
            }
            if (salesmanNameInput) {
                salesmanNameInput.addEventListener('input', (e) => e.target.value = e.target.value.toUpperCase());
                salesmanNameInput.addEventListener('blur', async () => {
                    const name = salesmanNameInput.value.trim().toUpperCase();
                    if (name) {
                        currentSalesman.textContent = name;
                        await dbManager.set('settings', { key: LAST_SALESMAN_KEY, value: name });
                    }
                });
            }
            if (clientNameInput) {
                clientNameInput.addEventListener('input', (e) => e.target.value = e.target.value.toUpperCase());
                clientNameInput.addEventListener('blur', async () => {
                    const name = clientNameInput.value.trim().toUpperCase();
                    if (name) await dbManager.set('settings', { key: LAST_CLIENT_KEY, value: name });
                });
            }
            
            if (clearFormBtn) clearFormBtn.addEventListener('click', clearCode);
            if (addBtn) addBtn.addEventListener('click', addBattery);
            
            if (factoryOption) factoryOption.addEventListener('click', () => selectWarrantyType('factory'));
            if (analyzedOption) analyzedOption.addEventListener('click', () => selectWarrantyType('analyzed'));
            
            if (applyFilterBtn) applyFilterBtn.addEventListener('click', applyFilters);
            if (clearFilterBtn) clearFilterBtn.addEventListener('click', clearFilters);
            if (previewPdfBtn) previewPdfBtn.addEventListener('click', previewPDF);
            if (pdfBtn) pdfBtn.addEventListener('click', downloadPDF);
            if (batchPdfBtn) batchPdfBtn.addEventListener('click', generateBatchPDFs);
            if (excelBtn) excelBtn.addEventListener('click', exportToFormattedExcel);
            if (clearBtn) clearBtn.addEventListener('click', clearData);
            if (codeFilter) codeFilter.addEventListener('input', applyFilters);
            
            if (backupBtn) backupBtn.addEventListener('click', exportData);
            if (restoreBtn) restoreBtn.addEventListener('click', () => restoreFileInput.click());

            if (restoreFileInput) {
                restoreFileInput.addEventListener('change', (e) => {
                    if (e.target.files.length > 0) {
                        fileToRestore = e.target.files[0];
                        if (restoreConfirmModal) restoreConfirmModal.classList.add('active');
                    }
                    e.target.value = ''; 
                });
            }

            if (cancelRestoreBtn) cancelRestoreBtn.addEventListener('click', () => {
                fileToRestore = null;
                if (restoreConfirmModal) restoreConfirmModal.classList.remove('active');
            });

            if (confirmRestoreBtn) confirmRestoreBtn.addEventListener('click', () => {
                if (restoreConfirmModal) restoreConfirmModal.classList.remove('active');
                if (fileToRestore) {
                    handleRestoreFile(fileToRestore);
                }
            });
            
            if (helpBtn) helpBtn.addEventListener('click', showRulesModal);
            if (closeRulesModal) closeRulesModal.addEventListener('click', () => rulesModal.classList.remove('active'));
            if (confirmRules) confirmRules.addEventListener('click', () => rulesModal.classList.remove('active'));
            
            if (closeAnalysisModal) closeAnalysisModal.addEventListener('click', () => analysisModal.classList.remove('active'));
            if (saveAnalysisBtn) saveAnalysisBtn.addEventListener('click', saveAnalysis);
            if (analysisClientFilter) analysisClientFilter.addEventListener('change', updateAnalysisTable);
            if (selectAllCheckbox) selectAllCheckbox.addEventListener('change', handleSelectAll);
            if (batchAnalyzeBtn) batchAnalyzeBtn.addEventListener('click', openBatchAnalysisModal);

            if (closeNameEditModal) closeNameEditModal.addEventListener('click', () => nameEditModal.classList.remove('active'));
            if (saveNameEditBtn) saveNameEditBtn.addEventListener('click', saveEditedNames);

            if (closeObservationModal) closeObservationModal.addEventListener('click', () => observationModal.classList.remove('active'));
            if (saveObservationBtn) saveObservationBtn.addEventListener('click', () => {
                if (observationCallback) observationCallback(observationText.value);
            });

            if (procedenteBtn) procedenteBtn.addEventListener('click', () => procedenteModal.classList.add('active'));
            if (closeProcedenteModal) closeProcedenteModal.addEventListener('click', () => procedenteModal.classList.remove('active'));
            if (capacidadeBaixaBtn) capacidadeBaixaBtn.addEventListener('click', () => {
                if(recommendationInput) recommendationInput.value = 'PROCEDENTE - CAPACIDADE BAIXA';
                if(procedenteModal) procedenteModal.classList.remove('active');
            });
            if (ccaBaixoBtn) ccaBaixoBtn.addEventListener('click', () => {
                if(recommendationInput) recommendationInput.value = 'PROCEDENTE - CCA BAIXO';
                if(procedenteModal) procedenteModal.classList.remove('active');
            });

            if (descarregadaBtn) descarregadaBtn.addEventListener('click', () => {
                if(recommendationInput) recommendationInput.value = 'RECARREGAR A BATERIA POR 6H EM CARGA LENTA E REFAZER O TESTE';
            });
            if (addObservationBtn) addObservationBtn.addEventListener('click', () => openObservationModal(tempObservation, (obs) => {
                tempObservation = obs;
                if(observationModal) observationModal.classList.remove('active');
                showNotification('Observação salva temporariamente.', 'info');
            }));
            if (analysisAddObservationBtn) analysisAddObservationBtn.addEventListener('click', async () => {
                 const battery = await dbManager.get('batteries', analyzingBatteryId);
                 if(battery) openObservationModal(battery.observations, async (obs) => {
                     battery.observations = obs;
                     await dbManager.set('batteries', battery);
                     if(observationModal) observationModal.classList.remove('active');
                     showNotification('Observação atualizada.', 'info');
                 });
            });

            if (toggleDarkModeBtn) toggleDarkModeBtn.addEventListener('click', toggleDarkMode);
            if (sidebarToggleBtn) sidebarToggleBtn.addEventListener('click', toggleSidebar);
            if (generateLaudoPdfBtn) generateLaudoPdfBtn.addEventListener('click', previewLaudoPDF);
            if (changePasswordBtn) changePasswordBtn.addEventListener('click', () => passwordModal.classList.add('active'));
            if (closePasswordModal) closePasswordModal.addEventListener('click', () => passwordModal.classList.remove('active'));
            if (passwordChangeForm) passwordChangeForm.addEventListener('submit', saveNewPassword);
            if (addLaudoImageBtn) addLaudoImageBtn.addEventListener('click', () => laudoImageUpload.click());
            if (laudoImageUpload) laudoImageUpload.addEventListener('change', handleLaudoImageUpload);
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

        function toggleSidebar() {
            const isCollapsed = document.body.classList.toggle('sidebar-collapsed');
            localStorage.setItem(SIDEBAR_COLLAPSED_KEY, isCollapsed);
        }

        function applySidebarState() {
            if (localStorage.getItem(SIDEBAR_COLLAPSED_KEY) === 'true') {
                document.body.classList.add('sidebar-collapsed');
            }
        }

        function applyUIRestrictions() {
            const userType = sessionStorage.getItem('userType');
            const isViewer = userType === 'viewer';

            // 1. Esconde todos os elementos de ação que têm a classe .viewer-hidden
            document.querySelectorAll('.viewer-hidden').forEach(el => {
                el.style.display = isViewer ? 'none' : '';
            });

            // 2. Esconde as colunas de ação nas tabelas
            document.querySelectorAll('.actions-cell').forEach(cell => {
                cell.style.display = isViewer ? 'none' : '';
            });
            
            // 3. Desativa os campos de formulário, exceto os de filtro, para o visualizador
            if (isViewer) {
                document.querySelectorAll('input, textarea, select').forEach(el => {
                    // Verifica se o elemento NÃO está dentro da área de filtros do relatório
                    if (!el.closest('.report-filters')) {
                        el.disabled = true;
                    }
                });
                 // Remove a interação de clique nas opções de tipo de garantia
                 document.querySelectorAll('.warranty-option').forEach(option => {
                    option.style.pointerEvents = 'none';
                    option.style.opacity = '0.7';
                 });
            } else {
                // Garante que os campos estão reativados para o admin
                 document.querySelectorAll('input, textarea, select').forEach(el => {
                    el.disabled = false;
                });
                 document.querySelectorAll('.warranty-option').forEach(option => {
                    option.style.pointerEvents = 'auto';
                    option.style.opacity = '1';
                });
            }

            // 4. Garante que todos os botões de navegação fiquem visíveis para todos os tipos de utilizador
            document.querySelectorAll('.sidebar-btn, .nav-btn').forEach(btn => {
                btn.style.display = '';
            });

            // 5. Define a página inicial
            if (isViewer) {
                showPage('reportPage'); // O visualizador começa nos relatórios, que é o mais útil para consulta
            } else {
                 // Garante que o admin comece na página de registo se nenhuma outra estiver ativa
                if (!document.querySelector('.page.active')) {
                     showPage('scanPage');
                }
            }
        }
        // #endregion

        // #region Lógica de Segurança e Credenciais
        async function initializeCredentials() {
            const adminUsers = await dbManager.get('settings', ADMIN_USERS_KEY);
            if (!adminUsers) {
                const initialAdmins = [
                    { user: 'JENILTON', pass: '32582190' },
                    { user: 'REGINALDO', pass: '123456' }
                ];
                await dbManager.set('settings', { key: ADMIN_USERS_KEY, value: initialAdmins });
                console.log('Credenciais de administrador iniciais definidas.');
            }
        }

        async function saveNewPassword(e) {
            e.preventDefault();
            const currentPasswordInput = document.getElementById('currentPassword');
            const newPasswordInput = document.getElementById('newPassword');
            const confirmNewPasswordInput = document.getElementById('confirmNewPassword');

            const currentPassword = currentPasswordInput.value;
            const newPassword = newPasswordInput.value;
            const confirmNewPassword = confirmNewPasswordInput.value;

            const loggedInUser = sessionStorage.getItem('currentUser');
            if (!loggedInUser) {
                showNotification('Erro: Utilizador não identificado. Por favor, inicie sessão novamente.', 'error');
                return;
            }

            const adminUsersData = await dbManager.get('settings', ADMIN_USERS_KEY);
            let adminUsers = adminUsersData.value;
            
            const userToUpdate = adminUsers.find(admin => admin.user === loggedInUser);

            if (!userToUpdate || currentPassword !== userToUpdate.pass) {
                showNotification('A palavra-passe atual está incorreta.', 'error');
                return;
            }
            if (newPassword.length < 6) {
                showNotification('A nova palavra-passe deve ter pelo menos 6 caracteres.', 'error');
                return;
            }
            if (newPassword !== confirmNewPassword) {
                showNotification('As novas palavras-passe não coincidem.', 'error');
                return;
            }

            // Atualiza a palavra-passe para o utilizador correto
            userToUpdate.pass = newPassword;

            await dbManager.set('settings', { key: ADMIN_USERS_KEY, value: adminUsers });
            showNotification('Palavra-passe alterada com sucesso!', 'success');
            passwordModal.classList.remove('active');
            currentPasswordInput.value = '';
            newPasswordInput.value = '';
            confirmNewPasswordInput.value = '';
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
        }

        async function loadFromDB() {
            batteryData = await dbManager.getAll('batteries');
            activityData = await dbManager.getAll('activity');
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
                showNotification('Este código de série já foi registado.', 'error');
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
                    newBattery.finalAction = 'ANALISADA NO REGISTO';
                }
                newBattery.technicalOpinionDate = new Date().toISOString();
                showNotification(`Bateria ${code} finalizada no registo.`, 'success');
                addActivity('fas fa-check-circle', `Bateria ${code} finalizada no registo.`);
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
                batteryData = [];
                activityData = [];
                updateUI();
                updateActivityLog();
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

                    showNotification(`${importedData.length} registos restaurados com sucesso! Os dados atuais foram substituídos.`, 'success');
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
        
        // #region Formulário e Leitura de Código
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
            updateRecentRegistrationsLog(); // Atualiza o novo log
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
        
        function updateRecentRegistrationsLog() {
            if (!recentRegistrationsBody) return;

            recentRegistrationsBody.innerHTML = '';
            const recentBatteries = [...batteryData]
                .sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp))
                .slice(0, 10); // Pega os últimos 10 registos

            if (recentBatteries.length === 0) {
                recentRegistrationsBody.innerHTML = `<tr><td colspan="4" style="text-align: center;">Nenhum registo ainda.</td></tr>`;
                return;
            }

            recentBatteries.forEach(battery => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${battery.code}</td>
                    <td>${battery.client}</td>
                    <td>${battery.salesman}</td>
                    <td>${new Date(battery.timestamp).toLocaleDateString('pt-BR')}</td>
                `;
                recentRegistrationsBody.appendChild(row);
            });
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
                const clientMatch = !currentFilters.client || (b.client && b.client.toLowerCase().includes(currentFilters.client.toLowerCase()));
                const statusMatch = !currentFilters.status || b.warranty_period_status === currentFilters.status;
                const workflowMatch = !currentFilters.workflowStatus || b.status === currentFilters.workflowStatus;
                const codeMatch = !currentFilters.code || (b.code && b.code.toLowerCase().includes(currentFilters.code.toLowerCase()));
                
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
                    <td class="actions-cell" style="display: flex; gap: 8px;">
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

            html += createSummaryLine('BATERIAS APROVADAS (PARA TROCA)', summary.approved);
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
                    <td class="actions-cell">
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
            showNotification('Nomes atualizados em todos os registos!', 'success');
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
            // A linha do logótipo foi removida para garantir estabilidade
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
                String(b.code || '-').substring(0, 20),
                String(b.batteryModel || '-').substring(0, 20),
                b.submissionDate ? new Date(b.submissionDate).toLocaleDateString('pt-BR') : '-',
                String(b.finalAction || (b.status === 'in_analysis' ? 'Em Análise' : '-')).substring(0, 40),
                String(b.recommendation || '-').substring(0, 100)
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
            try {
                if (!window.jspdf || !window.jspdf.jsPDF) {
                    showNotification('Erro: A biblioteca PDF (jsPDF) não carregou corretamente.', 'error');
                    console.error('jsPDF library is not available on window.jspdf');
                    return null;
                }
                const doc = new window.jspdf.jsPDF();
                if (typeof doc.autoTable !== 'function') {
                    showNotification('Erro: O plugin de tabelas para PDF (autoTable) não carregou.', 'error');
                    console.error('jsPDF autoTable plugin is not available on the doc instance.');
                    return null;
                }

                const filteredData = getFilteredData();
                if (filteredData.length === 0) {
                    showNotification('Nenhum dado no filtro para gerar o relatório.', 'error');
                    return null;
                }

                const validData = filteredData.filter(b => b && b.client);
                if (validData.length === 0) {
                    showNotification('Nenhum registo com cliente válido para gerar o relatório.', 'error');
                    return null;
                }

                const groupedByClient = validData.reduce((acc, battery) => {
                    (acc[battery.client] = acc[battery.client] || []).push(battery);
                    return acc;
                }, {});
                
                let firstClient = true;
                for (const clientName in groupedByClient) {
                    if (!firstClient) {
                        doc.addPage();
                    }
                    const clientBatteries = groupedByClient[clientName];
                    const salesmanName = clientBatteries.length > 0 ? (clientBatteries[0].salesman || 'N/A') : 'N/A';
                    const startY = drawPdfHeader(doc, clientName, salesmanName);
                    buildClientPDFPage(doc, clientName, clientBatteries, startY);
                    firstClient = false;
                }
                
                return doc;
            } catch (error) {
                console.error("Falha catastrófica ao gerar PDF:", error);
                showNotification(`Ocorreu um erro inesperado ao gerar o PDF. Verifique a consola.`, 'error');
                return null;
            }
        }
        
        function generateBatchPDFs() {
            try {
                if (!window.jspdf || !window.jspdf.jsPDF) {
                    showNotification('Erro: A biblioteca PDF (jsPDF) não carregou corretamente.', 'error');
                    console.error('jsPDF library is not available on window.jspdf');
                    return;
                }
                if (typeof (new window.jspdf.jsPDF()).autoTable !== 'function') {
                    showNotification('Erro: O plugin de tabelas para PDF (autoTable) não carregou.', 'error');
                    console.error('jsPDF autoTable plugin is not available on the doc instance.');
                    return;
                }

                const dataToProcess = batteryData;
                if (dataToProcess.length === 0) {
                    showNotification('Nenhum dado para gerar os relatórios.', 'error');
                    return;
                }

                const validData = dataToProcess.filter(b => b && b.client);
                 if (validData.length === 0) {
                    showNotification('Nenhum registo com cliente válido para gerar relatórios.', 'error');
                    return;
                }

                const groupedByClient = validData.reduce((acc, battery) => {
                    (acc[battery.client] = acc[battery.client] || []).push(battery);
                    return acc;
                }, {});

                showNotification(`Gerando ${Object.keys(groupedByClient).length} relatórios...`, 'info');

                for (const clientName in groupedByClient) {
                    const doc = new window.jspdf.jsPDF();
                    const clientBatteries = groupedByClient[clientName];
                    const salesmanName = clientBatteries.length > 0 ? (clientBatteries[0].salesman || 'N/A') : 'N/A';

                    const startY = drawPdfHeader(doc, clientName, salesmanName);
                    buildClientPDFPage(doc, clientName, clientBatteries, startY);
                    
                    const safeFileName = String(clientName).replace(/[^a-z0-9]/gi, '_').toLowerCase();
                    doc.save(`relatorio_garantia_${safeFileName}.pdf`);
                }
            } catch (error) {
                 console.error("Falha catastrófica ao gerar PDFs em lote:", error);
                showNotification(`Ocorreu um erro inesperado ao gerar os PDFs. Verifique a consola.`, 'error');
            }
        }

        function previewPDF() {
            const doc = generatePDF();
            if (doc) { 
                doc.output('dataurlnewwindow');
            }
        }
        
        function downloadPDF() {
            const doc = generatePDF();
            if (doc) { 
                doc.save(`relatorio_consolidado_${new Date().toISOString().slice(0,10)}.pdf`);
            }
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
                'Data de Registo': new Date(b.timestamp).toLocaleString('pt-BR'),
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

        // #region Lógica do Laudo Padrão (Offline)
        function getLaudoData() {
            const data = {
                client: document.getElementById('laudoClientName').value.trim().toUpperCase(),
                code: document.getElementById('laudoBatteryCode').value.trim().toUpperCase(),
                model: document.getElementById('laudoBatteryModel').value.trim().toUpperCase(),
                ccaNominal: document.getElementById('laudoBatteryCCA_Nominal').value,
                ccaMedido: document.getElementById('laudoCA_Medido').value,
                voltageNominal: document.getElementById('laudoBatteryVoltage_Nominal').value,
                voltageMedida: document.getElementById('laudoVoltage_Medida').value,
                visual: {
                    estufada: document.getElementById('checkEstufada').checked,
                    polos: document.getElementById('checkPolos').checked,
                    vazamento: document.getElementById('checkVazamento').checked,
                    outros: document.getElementById('laudoVisualInspection').value.trim()
                },
                diagnostics: {
                    fugaCorrente: document.getElementById('checkFugaCorrente').checked,
                    sobretensao: document.getElementById('checkSobretensao').checked,
                    subtensao: document.getElementById('checkSubtensao').checked,
                    aplicacaoIncorreta: document.getElementById('checkAplicacaoIncorreta').checked,
                    longoDesuso: document.getElementById('checkLongoDesuso').checked,
                },
                notes: document.getElementById('laudoTechnicianNotes').value.trim(),
                images: laudoImages, // Adiciona as imagens
            };

            // Validação
            if(!data.client || !data.code || !data.model || !data.ccaNominal || !data.ccaMedido || !data.voltageMedida) {
                showNotification('Por favor, preencha todos os campos do laudo.', 'error');
                return null;
            }
            return data;
        }

        function generateLaudoDiagnosis(data) {
            let analysis = [];
            let diagnosis = "Diagnóstico Pendente";

            const voltage = parseFloat(data.voltageMedida);
            const ccaNominal = parseFloat(data.ccaNominal);
            const ccaMedido = parseFloat(data.ccaMedido);

            // Análise da Tensão
            if (voltage < 10.5) {
                analysis.push("A Tensão em Circuito Aberto (VCA) encontra-se em nível crítico, sugerindo a possibilidade de uma ou mais células em curto-circuito ou em estado de descarga profunda irreversível.");
            } else if (voltage < 12.4) {
                analysis.push("A Tensão em Circuito Aberto (VCA) está abaixo do mínimo especificado (12.4V), o que indica um baixo Estado de Carga (SoC - State of Charge), requerendo análise aprofundada.");
            } else {
                analysis.push("A Tensão em Circuito Aberto (VCA) está em conformidade com os parâmetros nominais, indicando uma adequada capacidade de retenção de carga.");
            }

            // Análise do CA (CCA)
            if (ccaMedido < (ccaNominal * 0.7)) {
                 analysis.push(`A Corrente de Arranque (CA) medida de ${ccaMedido}A está significativamente abaixo do especificado (${ccaNominal}A), denotando uma perda acentuada e irrecuperável da capacidade de arranque, indicativo de sulfatação avançada das placas internas.`);
            } else {
                analysis.push(`A Corrente de Arranque (CA) medida de ${ccaMedido}A mostra-se compatível com o valor nominal de ${ccaNominal}A, demonstrando capacidade de arranque adequada.`);
            }

            // Análise Visual
            let visualProblems = [];
            if (data.visual.estufada) visualProblems.push("deformação ou estufamento da caixa");
            if (data.visual.polos) visualProblems.push("danos ou oxidação nos polos");
            if (data.visual.vazamento) visualProblems.push("sinais de vazamento de eletrólito");

            if (visualProblems.length > 0) {
                analysis.push(`Adicionalmente, a inspeção visual constatou: ${visualProblems.join(', ')}.`);
                diagnosis = `IMPROCEDENTE. Identificada avaria física (${visualProblems.join(', ')}), condição característica de falha no sistema de recarga do veículo (sobretensão/subtensão) ou manuseio inadequado, não configurando defeito de fabricação.`;
            } else if (voltage < 12.4 && ccaMedido >= (ccaNominal * 0.7)) {
                diagnosis = "IMPROCEDENTE. Nenhum defeito de fabricação foi detectado. A bateria apresenta-se apenas com baixo estado de carga (SoC). Recomenda-se a aplicação de recarga lenta conforme especificações técnicas e a verificação do sistema elétrico do veículo.";
            } else if (voltage >= 12.4 && ccaMedido < (ccaNominal * 0.7)) {
                diagnosis = "PROCEDENTE. A bateria exibe perda de capacidade de arranque (CA) não recuperável por processo de recarga, mesmo com tensão em repouso adequada, o que caracteriza um defeito de fabricação nas células internas.";
            } else if (voltage < 12.4 && ccaMedido < (ccaNominal * 0.7)) {
                 diagnosis = "PROCEDENTE. A bateria apresenta falha crítica e simultânea de retenção de carga e capacidade de arranque, caracterizando um defeito de fabricação conclusivo.";
            }
            else {
                diagnosis = "IMPROCEDENTE. Os parâmetros elétricos de Tensão em Circuito Aberto (VCA) e Corrente de Arranque (CA) estão em conformidade com as especificações técnicas do produto. Nenhuma anomalia de fabricação foi detectada durante os ensaios.";
            }

            return { analysis: analysis.join(' '), diagnosis };
        }

        function generateLaudoPDF(data) {
            try {
                if (!window.jspdf || !window.jspdf.jsPDF) {
                    showNotification('Erro: A biblioteca PDF (jsPDF) não carregou corretamente.', 'error');
                    return null;
                }
                const doc = new window.jspdf.jsPDF();
                const { analysis, diagnosis } = generateLaudoDiagnosis(data);

                // --- Cabeçalho ---
                doc.setFontSize(18);
                doc.text('FABRECK DO BRASIL', 200, 15, { align: 'right' });
                doc.setFontSize(12);
                doc.text('Laudo Técnico de Análise de Garantia', 200, 22, { align: 'right' });
                doc.setLineWidth(0.5);
                doc.line(15, 30, 200, 30);
                
                let y = 40;

                // --- Seção 1: Dados ---
                doc.setFontSize(12);
                doc.setFont(undefined, 'bold');
                doc.text('1. DADOS DE IDENTIFICAÇÃO', 15, y);
                y += 7;
                doc.setFontSize(10);
                doc.setFont(undefined, 'normal');
                doc.text(`Cliente: ${data.client}`, 17, y);
                doc.text(`Data: ${new Date().toLocaleDateString('pt-BR')}`, 200, y, {align: 'right'});
                y += 5;
                doc.text(`Bateria S/N: ${data.code}`, 17, y);
                doc.text(`Modelo: ${data.model}`, 100, y);
                y += 10;

                // --- Seção 2: Testes ---
                doc.setFontSize(12);
                doc.setFont(undefined, 'bold');
                doc.text('2. PARÂMETROS DE TESTE', 15, y);
                y += 5;
                doc.autoTable({
                    startY: y,
                    theme: 'grid',
                    head: [['Parâmetro', 'Valor Padrão', 'Valor Medido', 'Resultado']],
                    body: [
                        [`Tensão em Circuito Aberto (VCA)`, `${data.voltageNominal}`, `${data.voltageMedida}V`, parseFloat(data.voltageMedida) >= 12.4 ? 'CONFORME' : 'NÃO CONFORME'],
                        [`Corrente de Arranque (CA / CCA)`, `${data.ccaNominal}A`, `${data.ccaMedido}A`, parseFloat(data.ccaMedido) >= (parseFloat(data.ccaNominal) * 0.7) ? 'CONFORME' : 'NÃO CONFORME']
                    ],
                    headStyles: { fillColor: [22, 49, 72] },
                });
                y = doc.lastAutoTable.finalY + 10;

                // --- Seção 3: Inspeção Visual ---
                doc.setFontSize(12);
                doc.setFont(undefined, 'bold');
                doc.text('3. INSPEÇÃO VISUAL', 15, y);
                y += 7;
                doc.setFontSize(10);
                doc.setFont(undefined, 'normal');
                doc.text(`- Carcaça estufada/danificada: ${data.visual.estufada ? 'SIM' : 'NÃO'}`, 17, y);
                doc.text(`- Polos danificados/oxidados: ${data.visual.polos ? 'SIM' : 'NÃO'}`, 100, y);
                y += 5;
                doc.text(`- Sinais de vazamento de eletrólito: ${data.visual.vazamento ? 'SIM' : 'NÃO'}`, 17, y);
                y += 5;
                if (data.visual.outros) doc.text(`- Outras observações: ${data.visual.outros}`, 17, y);
                y += 10;

                // --- Seção 4: Diagnóstico ---
                doc.setFontSize(12);
                doc.setFont(undefined, 'bold');
                doc.text('4. ANÁLISE E DIAGNÓSTICO TÉCNICO', 15, y);
                y += 7;
                doc.setFontSize(10);
                doc.setFont(undefined, 'normal');
                doc.text(doc.splitTextToSize(analysis, 180), 17, y);
                y = doc.lastAutoTable.finalY > y ? doc.lastAutoTable.finalY + 10 : y + 20;

                // --- Seção 5: Parecer ---
                doc.setFontSize(12);
                doc.setFont(undefined, 'bold');
                doc.text('5. PARECER FINAL', 15, y);
                y += 7;
                doc.setFont(undefined, 'bold');
                doc.setFillColor(diagnosis.includes('PROCEDENTE') ? '#2ecc71' : '#e74c3c');
                doc.rect(17, y - 4, 60, 6, 'F');
                doc.setTextColor(255,255,255);
                doc.text(diagnosis.split('.')[0], 20, y);
                doc.setTextColor(0,0,0);
                y += 7;
                doc.setFont(undefined, 'normal');
                doc.text(doc.splitTextToSize(diagnosis, 180), 17, y);
                y = doc.lastAutoTable.finalY > y ? doc.lastAutoTable.finalY + 20 : y + 20;

                // --- Seção 6: Observações ---
                const diagnosticNotes = [];
                if (data.diagnostics.fugaCorrente) diagnosticNotes.push("- Veículo apresenta indícios de fuga de corrente, resultando em descarga prematura da bateria.");
                if (data.diagnostics.sobretensao) diagnosticNotes.push("- Sistema de recarga (alternador/retificador) opera com sobretensão, causando sobrecarga e danos internos à bateria.");
                if (data.diagnostics.subtensao) diagnosticNotes.push("- Sistema de recarga (alternador/retificador) opera com subtensão, impedindo a recarga completa da bateria.");
                if (data.diagnostics.aplicacaoIncorreta) diagnosticNotes.push("- A bateria instalada não corresponde à aplicação recomendada para o modelo do veículo.");
                if (data.diagnostics.longoDesuso) diagnosticNotes.push("- Identificada evidência de longo período sem uso, o que pode levar à descarga profunda e sulfatação.");
                if (data.notes) diagnosticNotes.push(`- Outras notas: ${data.notes}`);

                if (diagnosticNotes.length > 0) {
                     doc.setFontSize(12);
                    doc.setFont(undefined, 'bold');
                    doc.text('6. OBSERVAÇÕES TÉCNICAS ADICIONAIS', 15, y);
                    y += 7;
                    doc.setFontSize(10);
                    doc.setFont(undefined, 'normal');
                    doc.text(doc.splitTextToSize(diagnosticNotes.join('\n'), 180), 17, y);
                    y = doc.lastAutoTable.finalY > y ? doc.lastAutoTable.finalY + 15 : y + 15;
                }

                 // --- Seção 7: Evidências Fotográficas ---
                if (data.images && data.images.length > 0) {
                    if (y > 180) { // Adiciona nova página se não houver espaço suficiente
                        doc.addPage();
                        y = 20;
                    }
                    doc.setFontSize(12);
                    doc.setFont(undefined, 'bold');
                    doc.text('7. EVIDÊNCIAS FOTOGRÁFICAS', 15, y);
                    y += 10;
                    
                    const imgWidth = 55;
                    const imgHeight = 55;
                    let x = 15;

                    data.images.forEach((imgData, index) => {
                        doc.addImage(imgData.src, 'JPEG', x, y, imgWidth, imgHeight);
                        x += imgWidth + 5; // Move para a próxima imagem
                        if ((index + 1) % 3 === 0) { // Quebra a linha a cada 3 imagens
                            x = 15;
                            y += imgHeight + 5;
                        }
                    });
                    y += imgHeight + 10;
                }


                // --- Assinatura ---
                const signatureY = doc.internal.pageSize.height - 40;
                doc.line(60, signatureY, 150, signatureY);
                doc.setFontSize(10);
                doc.text('JENILTON CRUZ', 105, signatureY + 5, { align: 'center'});
                doc.setFontSize(8);
                doc.text('Técnico Responsável', 105, signatureY + 9, { align: 'center'});
                
                return doc;

            } catch (error) {
                console.error("Falha ao gerar o PDF do laudo:", error);
                showNotification(`Ocorreu um erro inesperado ao gerar o laudo.`, 'error');
                return null;
            }
        }
        
        function previewLaudoPDF() {
            const data = getLaudoData();
            if (!data) return;
            
            const doc = generateLaudoPDF(data);
            if(doc) {
                doc.output('dataurlnewwindow');
            }
        }
        
        function handleLaudoImageUpload(event) {
            const files = event.target.files;
            if (!files) return;

            if (laudoImages.length + files.length > 3) {
                showNotification('Pode adicionar no máximo 3 imagens.', 'error');
                return;
            }

            for (const file of files) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    laudoImages.push({ id: Date.now() + Math.random(), src: e.target.result });
                    renderLaudoImagePreviews();
                };
                reader.readAsDataURL(file);
            }
            // Limpa o input para permitir selecionar o mesmo ficheiro novamente
            event.target.value = '';
        }

        function renderLaudoImagePreviews() {
            laudoImagePreviews.innerHTML = '';
            laudoImages.forEach(image => {
                const container = document.createElement('div');
                container.className = 'preview-container';

                const img = document.createElement('img');
                img.src = image.src;

                const removeBtn = document.createElement('button');
                removeBtn.className = 'remove-img-btn';
                removeBtn.innerHTML = '&times;';
                removeBtn.onclick = () => {
                    laudoImages = laudoImages.filter(i => i.id !== image.id);
                    renderLaudoImagePreviews();
                };

                container.appendChild(img);
                container.appendChild(removeBtn);
                laudoImagePreviews.appendChild(container);
            });
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
            if (syncStatus) {
                syncStatus.textContent = 'A sincronizar...';
            }
            
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
                    if(syncStatus) {
                        syncStatus.textContent = `Sincronizado às ${syncTime}`;
                        syncStatus.style.color = '#2ECC71';
                    }
                } else {
                    showNotification('Dados atualizados na nuvem com sucesso!', 'success');
                }
                addActivity('fas fa-cloud-upload-alt', 'Dados sincronizados com a nuvem.');

            } catch (error) {
                console.error('Erro ao salvar na nuvem:', error);
                if (isSilent) {
                    if(syncStatus) {
                        syncStatus.textContent = 'Erro de sincronização';
                        syncStatus.style.color = '#E74C3C';
                    }
                }
                showNotification('Falha ao sincronizar os dados. Verifique a sua ligação.', 'error');
            }
        }

        async function loadFromCloud(isSilent = false) {
            const syncStatus = document.getElementById('syncStatus');
            if (syncStatus) {
                syncStatus.textContent = 'A carregar dados da nuvem...';
            }

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
                    if (cloudLoadConfirmModal) cloudLoadConfirmModal.classList.add('active');
                }

            } catch (error) {
                console.error('Erro ao carregar da nuvem:', error);
                showNotification(`Falha ao carregar: ${error.message}`, 'error');
                if (syncStatus) {
                    syncStatus.textContent = 'Falha ao carregar da nuvem';
                    syncStatus.style.color = '#E74C3C';
                }
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
                    showNotification(`${data.batteries.length} registos carregados da nuvem!`, 'success');
                }
                addActivity('fas fa-cloud-download-alt', 'Dados carregados da nuvem.');

                const syncStatus = document.getElementById('syncStatus');
                if (syncStatus) {
                    const syncTime = new Date(data.savedAt).toLocaleString('pt-BR');
                    syncStatus.textContent = `Sincronizado em ${syncTime}`;
                    syncStatus.style.color = '#2ECC71';
                }

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


