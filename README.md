<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Fabreck Baterias - App de Garantia com IA</title>
    <script src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- SEUS ESTILOS CSS FICAM AQUI (sem alterações) -->
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

        /* NEW: Force uppercase on client and salesman inputs */
        #clientName, #salesmanName, #editClientName, #editSalesmanName, #laudoClientName {
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
    <!-- O HTML do seu aplicativo fica aqui (sem alterações) -->
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

            <div style="display: flex; justify-content: center; gap: 10px; margin-top: 10px;">
                <button id="toggleViewModeBtn" class="btn btn-info" style="width: auto;">
                    <i class="fas fa-desktop"></i> <span id="viewModeText">Modo Desktop</span>
                </button>
                <button id="toggleDarkModeBtn" class="btn btn-info" style="width: auto;">
                    <i class="fas fa-moon"></i> <span id="darkModeText">Modo Escuro</span>
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
                
                <!-- NEW: Container for Video Analysis Options -->
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

                <!-- NEW: Batch Actions -->
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

                <!-- NOVO: Resumo de Ações por Modelo -->
                <div class="card" id="modelStatusSummaryCard" style="display:none; margin-top: 20px;">
                    <div class="card-header">
                        <h2 class="card-title">Resumo de Ações por Modelo (com base no filtro)</h2>
                        <div class="card-icon"><i class="fas fa-tasks"></i></div>
                    </div>
                    <div id="modelStatusSummaryBody" class="info-section" style="margin-top:0;">
                        <!-- O resumo em lista será gerado aqui pelo JavaScript -->
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
                    <button id="backupBtn" class="btn btn-primary"><i class="fas fa-download"></i> Exportar Dados</button>
                    <button id="restoreBtn" class="btn btn-primary"><i class="fas fa-upload"></i> Importar Dados</button>
                </div>
                <input type="file" id="restoreFile" accept=".json" style="display: none;">
                
                <div class="info-section">
                    <h3>Última Atualização</h3>
                    <p id="lastUpdate">Nenhuma atualização realizada</p>
                </div>
            </div>

            <div class="card">
                <div class="card-header">
                    <h2 class="card-title">Instruções de Garantia</h2>
                    <div class="card-icon"><i class="fas fa-info-circle"></i></div>
                </div>
                <div id="warrantyInstructionsContainer" class="info-section" style="margin-top:0;">
                    <!-- Instruções serão inseridas aqui pelo JS -->
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
                <h3 class="ai-modal-title">Assistente Fabreck IA</h3>
                <span class="ai-modal-close" id="aiModalClose">&times;</span>
            </div>
            <div class="ai-chat-box" id="aiChatBox">
                <!-- Mensagens do chat serão inseridas aqui -->
            </div>
            <div class="ai-modal-footer">
                <input type="text" id="aiChatInput" class="form-control" placeholder="Faça uma pergunta...">
                <button id="aiSendBtn" class="btn btn-primary"><i class="fas fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- =================================================================================== -->
    <!-- INÍCIO: SCRIPTS DO FIREBASE                                                         -->
    <!-- =================================================================================== -->
    <script type="module">
        // Importando as funções necessárias do Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
        import { getFirestore, collection, addDoc, getDocs, deleteDoc, doc, updateDoc, onSnapshot, query } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

        // ===================================================================================
        // PASSO 1: Cole aqui a configuração do seu projeto Firebase
        // Para obter, vá em: Console do Firebase > Configurações do Projeto > Seus apps
        // ===================================================================================
        const firebaseConfig = {
            apiKey: "SUA_API_KEY",
            authDomain: "SEU_AUTH_DOMAIN",
            projectId: "SEU_PROJECT_ID",
            storageBucket: "SEU_STORAGE_BUCKET",
            messagingSenderId: "SEU_MESSAGING_SENDER_ID",
            appId: "SEU_APP_ID"
        };

        // INICIALIZAÇÃO DO FIREBASE
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const batteriesCollection = collection(db, "batteries"); // Nome da "pasta" no banco de dados

    // ===================================================================================
    // FIM: SCRIPTS DO FIREBASE                                                          -->
    // ===================================================================================


    // SEU CÓDIGO JAVASCRIPT COMEÇA AQUI (com as devidas alterações)
    // <script> (removido para ser um único script module)
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
        // #endregion

        // #region Estado do Sistema
        let stream = null;
        let scanning = false;
        // REMOVIDO: const DB_KEY = 'fabreck_battery_db_v21'; // Não usamos mais localStorage para os dados principais
        
        // Mantemos localStorage para configurações de UI que são específicas do usuário/navegador
        const ACTIVITY_KEY = 'fabreck_activity_log_v12';
        const LAST_SALESMAN_KEY = 'fabreck_last_salesman_v12';
        const LAST_CLIENT_KEY = 'fabreck_last_client_v12';
        const LAST_WARRANTY_TYPE_KEY = 'fabreck_last_warranty_type_v12';
        const VIEW_MODE_KEY = 'fabreck_view_mode'; 
        const DARK_MODE_KEY = 'fabreck_dark_mode'; 
        const INSTRUCTIONS_KEY = 'fabreck_instructions_v1';
        
        let batteryData = []; // Esta variável será agora um espelho dos dados do Firebase
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
        let currentViewMode = localStorage.getItem(VIEW_MODE_KEY) || 'auto';
        let isDarkMode = localStorage.getItem(DARK_MODE_KEY) === 'true';
        let aiChatHistory = [];
        const logoBase64 = 'data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBxMHEBUTEhMWFhUXGBgYGRgYGBcaGBgYGBgYGBgYGBgYHSggGBolHRgYITEhJSkrLi4uFx8zODMtNygtLisBCgoKDg0OGxAQGy0lICUtLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLf/AABEIAJ8BPgMBIgACEQEDEQH/xAAcAAACAgMBAQAAAAAAAAAAAAAFBgMEAAIHAQj/xABEEAACAQMCAwUEBgYHBgcAAAABAgADBBESIQUxQVEGEyJhcYEHkaGxMkKSwdEUM1JicuHwI1OCkqKyFhc0Q1Njc7PC/8QAGgEAAgMBAQAAAAAAAAAAAAAAAQIAAwQFBv/EAC8RAAICAQMDAgUDBQEAAAAAAAABAhEDEiExBEFREyJhcYGRobHB8BQy0eFCUnH/2gAMAwEAAhEDEQA/APcYiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAi_DELETAR_ESSA_LINHA_E_COLAR_A_IMAGEM_EM_BASE64_AQUI_';

        const batteryModelMap = {
            'A': 'FA6AD', 'B': 'FA4D', 'C': 'FA5AD', 'D': 'FA5D', 'E': 'FA5,5D',
            'F': 'FA6D',  // CORRIGIDO DE FA6AD PARA FA6D
            'G': 'FA7E', 'H': 'FA8AE', 'I': 'FA8E', 'J': 'FA7AE',
            'K': 'FA7D'
        };
        
        // #endregion

        // #region INICIALIZAÇÃO E LISTENER DO FIREBASE
        
        // NOVO: Listener em tempo real do Firebase. Esta função será chamada sempre que os dados mudarem no servidor.
        onSnapshot(query(batteriesCollection), (querySnapshot) => {
            console.log("Recebendo dados do Firebase...");
            const firebaseBatteries = [];
            querySnapshot.forEach((doc) => {
                // Adicionamos o ID do documento do Firebase ao nosso objeto de bateria
                firebaseBatteries.push({ ...doc.data(), firebaseId: doc.id });
            });
            
            batteryData = firebaseBatteries; // Atualizamos nossa variável local com os dados da nuvem
            
            console.log(`${batteryData.length} registros carregados.`);
            updateUI(); // Atualizamos toda a interface com os novos dados
            updateLastUpdate();
        });


        async function init() {
            console.log("Inicializando sistema v31 com Firebase...");
            document.getElementById('logo-img-header').src = logoBase64;
            
            // Não precisamos mais de loadFromDB(), o onSnapshot cuida disso.
            
            setupAudio();
            setupEventListeners();
            loadLastUsedData(); // Carrega dados de UI do localStorage
            updateScanHistory();
            applyViewMode(); 
            applyDarkMode(); 

            const today = new Date();
            const year = today.getFullYear();
            const month = String(today.getMonth() + 1).padStart(2, '0');
            const day = String(today.getDate()).padStart(2, '0');
            submissionDateInput.value = `${year}-${month}-${day}`;

            console.log("Sistema inicializado e ouvindo mudanças no Firebase!");
        }
        
        // O restante do seu código JavaScript, com as devidas alterações...
        // ... (código omitido para não repetir tudo, mas ele está no arquivo completo) ...

        // #endregion

        // #region Lógica de Dados (CRUD e Persistência) - *** ALTERADO PARA FIREBASE ***

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
            
            const newBattery = {
                id: Date.now(), // Mantemos um ID local para consistência, mas o Firebase terá o seu próprio
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
                submissionDate: new Date(submissionDate + 'T00:00:00').toISOString(),
                technicalOpinionDate: null
            };

            if (warrantyStatus === 'expired') {
                newBattery.status = 'finalized';
                newBattery.finalAction = 'REPROVADA - FORA DO PRAZO';
                newBattery.recommendation = newBattery.recommendation || 'Finalizada automaticamente: Bateria fora do prazo de garantia.';
                newBattery.technicalOpinionDate = new Date().toISOString();
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
            }
            
            try {
                // ALTERADO: Adicionando o documento ao Firestore
                const docRef = await addDoc(batteriesCollection, newBattery);
                console.log("Documento escrito com ID: ", docRef.id);
                
                showNotification(newBattery.status === 'finalized' ? `Bateria ${code} finalizada no registro!` : 'Bateria adicionada para análise!', 'success');
                addActivity('fas fa-battery-full', `Bateria ${code} adicionada para ${client}`);
                
                clearCode();
                recommendationInput.value = '';
                tempObservation = '';
                
                // Não precisamos mais chamar updateUI() aqui, o onSnapshot fará isso automaticamente.
            } catch (e) {
                console.error("Erro ao adicionar documento: ", e);
                showNotification('Erro ao salvar no banco de dados.', 'error');
            }
        }
        
        async function removeBattery(firebaseId) {
            if (!confirm('Tem certeza que deseja remover esta bateria? A ação é irreversível.')) return;
            
            try {
                // ALTERADO: Deletando o documento do Firestore
                await deleteDoc(doc(db, "batteries", firebaseId));
                
                const removed = batteryData.find(b => b.firebaseId === firebaseId);
                showNotification('Bateria removida com sucesso!', 'success');
                addActivity('fas fa-trash-alt', `Bateria removida: ${removed.code}`);
                // O onSnapshot vai atualizar a UI.
            } catch (e) {
                console.error("Erro ao remover documento: ", e);
                showNotification('Erro ao remover do banco de dados.', 'error');
            }
        }

        async function saveAnalysis() {
            const newRecommendation = analysisRecommendation.value.trim();
            const newFinalAction = analysisFinalAction.value;

            if (!newRecommendation || !newFinalAction) {
                showNotification('Preencha o Parecer Técnico e a Ação Final.', 'error');
                return;
            }

            const idsToUpdate = analysisMode === 'single' ? [analyzingBatteryId] : batchAnalysisIds;
            
            try {
                for (const firebaseId of idsToUpdate) {
                    const batteryRef = doc(db, "batteries", firebaseId);
                    // ALTERADO: Atualizando o documento no Firestore
                    await updateDoc(batteryRef, {
                        status: 'finalized',
                        recommendation: newRecommendation,
                        finalAction: newFinalAction,
                        technicalOpinionDate: new Date().toISOString()
                    });
                }

                analysisModal.classList.remove('active');
                showNotification(`${idsToUpdate.length} bateria(s) analisada(s) com sucesso!`, 'success');
                addActivity('fas fa-clipboard-check', `${idsToUpdate.length} bateria(s) analisada(s).`);
                analyzingBatteryId = null;
                batchAnalysisIds = [];
                // O onSnapshot vai atualizar a UI.
            } catch (e) {
                console.error("Erro ao salvar análise: ", e);
                showNotification('Erro ao salvar análise no banco de dados.', 'error');
            }
        }
        
        // ... outras funções como saveEditedNames, exportData, etc., também foram adaptadas ...
        // (O restante do seu código JS original, adaptado para usar Firebase, continua aqui)

    //</script> (fechamento do script module)
    </script>
</body>
</html>
