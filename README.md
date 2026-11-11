# Conexao-Humana-Da-Celula-a-Sociedade
Trabalho de Culminância Eletiva - Conexão Humana: Da célula a sociedade
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Prontuário Médico - Diagnóstico Clínico</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Courier+Prime:ital,wght@0,400;0,700;1,400&family=Inter:wght@300;400;600;700;800&family=Special+Elite&display=swap" rel="stylesheet">
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <style>
    /* Font Families */
    .font-medical {
      font-family: 'Courier Prime', monospace;
    }
    .font-stamp {
      font-family: 'Special Elite', cursive, monospace;
    }
    .font-sans-clean {
      font-family: 'Inter', sans-serif;
    }

    /* Clipboard & Paper Textures */
    .clipboard-bg {
      background: #231c19;
      background-image: radial-gradient(#3a2e29 1px, transparent 1px), linear-gradient(135deg, #2c231f 0%, #1a1412 100%);
      background-size: 20px 20px, 100% 100%;
      box-shadow: inset 0 0 100px rgba(0,0,0,0.8), 0 25px 50px -12px rgba(0, 0, 0, 0.7);
    }

    .paper-sheet {
      background-color: #faf6ee;
      background-image: 
        linear-gradient(#e2dcd0 1px, transparent 1px),
        linear-gradient(90deg, rgba(239, 68, 68, 0.15) 1px, transparent 1px);
      background-size: 100% 1.8rem, 100% 100%;
      background-position: 0 0, 2.5rem 0;
      box-shadow: 
        0 10px 25px -5px rgba(0, 0, 0, 0.3),
        0 8px 10px -6px rgba(0, 0, 0, 0.2),
        inset 0 0 40px rgba(210, 190, 160, 0.2);
    }

    /* Rubber Stamp Effect */
    .stamp {
      display: inline-block;
      padding: 0.35rem 1rem;
      text-transform: uppercase;
      font-weight: 800;
      border-width: 4px;
      border-style: solid;
      border-radius: 6px;
      mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.8" result="noise"/><feColorMatrix type="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -9" opacity="0.7"/></filter><rect width="100%" height="100%" filter="url(%23n)"/></svg>');
      -webkit-mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.8" result="noise"/><feColorMatrix type="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -9" opacity="0.7"/></filter><rect width="100%" height="100%" filter="url(%23n)"/></svg>');
    }

    .stamp-approved {
      color: #15803d;
      border-color: #15803d;
      background-color: rgba(34, 197, 94, 0.08);
      transform: rotate(-5deg);
      animation: stampSlam 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
    }

    .stamp-rejected {
      color: #b91c1c;
      border-color: #b91c1c;
      background-color: rgba(239, 68, 68, 0.08);
      transform: rotate(6deg);
      animation: stampSlam 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
    }

    @keyframes stampSlam {
      0% {
        opacity: 0;
        transform: scale(3) rotate(15deg);
      }
      100% {
        opacity: 0.95;
        transform: scale(1) rotate(var(--rotation, -5deg));
      }
    }

    /* ECG Screen Animation */
    .ecg-line {
      stroke-dasharray: 1000;
      stroke-dashoffset: 1000;
      animation: ecgSweep 2.5s linear infinite;
    }

    @keyframes ecgSweep {
      0% { stroke-dashoffset: 1000; }
      100% { stroke-dashoffset: 0; }
    }

    /* Custom Scrollbar for Clinical Details */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #f1ebd9;
    }
    ::-webkit-scrollbar-thumb {
      background: #c5bba8;
      border-radius: 3px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #9e937e;
    }
  </style>
</head>
<body class="bg-stone-900 text-stone-800 font-sans-clean min-h-screen py-4 px-2 sm:px-4 md:py-8 flex flex-col items-center justify-center select-none">

  <!-- MAIN APP CONTAINER -->
  <div class="w-full max-w-4xl mx-auto relative">

    <!-- TOP CONTROL BAR (SOUND, RANKING QUICK BTN) -->
    <div class="flex justify-between items-center mb-3 px-2">
      <div class="flex items-center space-x-2">
        <span class="inline-flex items-center px-2.5 py-1 rounded-md text-xs font-semibold bg-emerald-950 text-emerald-400 border border-emerald-800">
          <span class="w-2 h-2 rounded-full bg-emerald-500 mr-1.5 animate-pulse"></span>
          PRONTUÁRIO DIGITAL v2.5
        </span>
      </div>
      <div class="flex items-center space-x-2">
        <button id="soundToggleBtn" onclick="toggleSound()" class="px-3 py-1.5 bg-stone-800 hover:bg-stone-700 text-stone-300 rounded-lg text-xs font-medium border border-stone-700 flex items-center gap-1.5 transition">
          <i data-lucide="volume-2" class="w-4 h-4 text-emerald-400" id="soundIcon"></i>
          <span id="soundText">Som: LIGADO</span>
        </button>
        <button onclick="showRankingModal()" class="px-3 py-1.5 bg-amber-950/80 hover:bg-amber-900 text-amber-300 rounded-lg text-xs font-medium border border-amber-800 flex items-center gap-1.5 transition">
          <i data-lucide="trophy" class="w-4 h-4 text-amber-400"></i>
          <span>Ver Ranking</span>
        </button>
      </div>
    </div>

    <!-- THE PHYSICAL CLIPBOARD CONTAINER -->
    <div class="clipboard-bg rounded-2xl p-3 sm:p-6 md:p-8 relative border-2 border-amber-950/40 shadow-2xl">
      
      <!-- METAL CLIP AT TOP -->
      <div class="absolute -top-6 left-1/2 -translate-x-1/2 z-20 w-48 sm:w-64 h-12 bg-gradient-to-b from-stone-300 via-stone-100 to-stone-400 rounded-t-lg border-t-2 border-x-2 border-stone-200 shadow-lg flex flex-col items-center justify-center">
        <!-- Metal Clip Spring/Hole Details -->
        <div class="w-24 h-2 bg-stone-700 rounded-full mb-1 shadow-inner"></div>
        <div class="flex justify-between w-36 px-4">
          <div class="w-3 h-3 rounded-full bg-stone-500 border border-stone-300 shadow"></div>
          <div class="w-12 h-1 bg-stone-500 rounded"></div>
          <div class="w-3 h-3 rounded-full bg-stone-500 border border-stone-300 shadow"></div>
        </div>
      </div>

      <!-- MEDICAL PAPER SHEET -->
      <div class="paper-sheet rounded-lg p-4 sm:p-8 min-h-[650px] relative mt-4 border border-amber-200/60 flex flex-col justify-between">
        
        <!-- WATERMARK STETHOSCOPE (BACKGROUND DECORATION) -->
        <div class="absolute inset-0 pointer-events-none opacity-[0.03] flex items-center justify-center overflow-hidden">
          <i data-lucide="activity" class="w-[400px] h-[400px] text-stone-900"></i>
        </div>

        <!-- APP SCREENS INJECTED HERE -->
        <div id="screenContainer" class="relative z-10 flex-1 flex flex-col">
          <!-- Dynamic Content Rendered Here -->
        </div>

        <!-- PAPER FOOTER DECORATION -->
        <div class="mt-6 pt-3 border-t border-stone-300/80 flex justify-between items-center text-[10px] sm:text-xs text-stone-500 font-medical">
          <div>HOSPITAL CENTRAL • SERVIÇO DE INFECTOLOGIA</div>
          <div>DOCUMENTO MÉDICO OFICIAL • CONFIDENCIAL</div>
        </div>

      </div>
    </div>
  </div>

  <!-- RANKING MODAL -->
  <div id="rankingModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-3">
    <div class="bg-stone-900 border border-amber-900/50 rounded-2xl w-full max-w-2xl overflow-hidden shadow-2xl text-stone-200">
      <div class="bg-stone-950 px-6 py-4 border-b border-stone-800 flex justify-between items-center">
        <div class="flex items-center gap-2">
          <i data-lucide="trophy" class="w-6 h-6 text-amber-400"></i>
          <h2 class="text-lg font-bold text-amber-100 font-medical tracking-wide">RANKING DE DIAGNOSTICISTAS</h2>
        </div>
        <button onclick="closeRankingModal()" class="text-stone-400 hover:text-white p-1 rounded-lg hover:bg-stone-800">
          <i data-lucide="x" class="w-6 h-6"></i>
        </button>
      </div>

      <div class="p-6 max-h-[60vh] overflow-y-auto">
        <div id="rankingListContainer">
          <!-- Ranking Table Rendered Here -->
        </div>
      </div>

      <div class="bg-stone-950 px-6 py-4 border-t border-stone-800 flex justify-between items-center flex-wrap gap-3">
        <button onclick="confirmResetRanking()" class="px-4 py-2 bg-red-950/80 hover:bg-red-900 text-red-200 rounded-lg text-xs font-semibold border border-red-800 flex items-center gap-2 transition">
          <i data-lucide="trash-2" class="w-4 h-4 text-red-400"></i>
          <span>Resetar Ranking</span>
        </button>
        <button onclick="closeRankingModal()" class="px-5 py-2 bg-stone-800 hover:bg-stone-700 text-white rounded-lg text-xs font-semibold transition">
          Fechar
        </button>
      </div>
    </div>
  </div>

  <!-- CONFIRM RESET MODAL -->
  <div id="resetConfirmModal" class="fixed inset-0 bg-black/90 backdrop-blur-md z-[60] hidden flex items-center justify-center p-4">
    <div class="bg-stone-900 border-2 border-red-800 rounded-xl p-6 max-w-md w-full text-stone-200 shadow-2xl">
      <div class="flex items-center gap-3 text-red-400 mb-4">
        <i data-lucide="alert-triangle" class="w-8 h-8"></i>
        <h3 class="text-lg font-bold font-medical">CONFIRMAR ZERAMENTO</h3>
      </div>
      <p class="text-sm text-stone-300 mb-6">
        Tem certeza que deseja apagar todos os registros do ranking? Esta ação é irreversível e removerá todos os históricos salvos.
      </p>
      <div class="flex justify-end gap-3">
        <button onclick="closeResetConfirmModal()" class="px-4 py-2 bg-stone-800 hover:bg-stone-700 text-stone-300 rounded-lg text-xs font-medium">
          Cancelar
        </button>
        <button onclick="executeResetRanking()" class="px-4 py-2 bg-red-600 hover:bg-red-700 text-white font-bold rounded-lg text-xs transition">
          Sim, Resetar Tudo
        </button>
      </div>
    </div>
  </div>

  <!-- GAME LOGIC SCRIPT -->
  <script>
    /* ==========================================================================
       DATASET - EXACT QUESTIONS & OPTIONS (INTERLEAVED 1 TO 25)
       ========================================================================== */
    const ALL_QUESTIONS = [
      // CASO 1 (Peste Negra - Caso 1)
      {
        category: "Peste Negra",
        caseTitle: "Caso 1 – O homem com febre (67 anos)",
        patientAge: "67 anos",
        patientSex: "Masculino",
        patientId: "CASO-01",
        questionText: "Um homem de 67 anos começou a apresentar febre muito alta, calafrios, dor de cabeça intensa e fraqueza. Depois de dois dias, percebeu o aparecimento de caroços muito doloridos na região da virilha e das axilas.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 3, // D
        explanation: "Os caroços doloridos na virilha e axilas são 'bubões', inflamações dos linfonodos causadas pela bactéria Yersinia pestis na Peste Bubônica."
      },

      // CASO 2 (H1N1 - Caso 1)
      {
        category: "H1N1",
        caseTitle: "Caso 2 – Paciente de 19 anos",
        patientAge: "19 anos",
        patientSex: "Feminino",
        patientId: "CASO-02",
        questionText: "Uma jovem procura atendimento médico relatando dor de garganta, espirros, coriza e uma leve dor de cabeça. Ela conta que começou a se sentir indisposta há dois dias, mas continua realizando suas atividades normalmente. Apresenta temperatura de 37,8 °C e relata que, na noite anterior, sentiu calafrios e dores leves nas pernas.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 2, // C
        explanation: "Sintomas respiratórios agudos e febrícula com mialgia em paciente jovem são típicos de Influenza A (H1N1)."
      },

      // CASO 3 (Tuberculose - Caso 1)
      {
        category: "Tuberculose",
        caseTitle: "Caso 3 – O homem de 45 anos",
        patientAge: "45 anos",
        patientSex: "Masculino",
        patientId: "CASO-03",
        questionText: "Um homem de 45 anos procura o posto de saúde com tosse que já dura quatro semanas, acompanhada de catarro, febre baixa no final da tarde e suor noturno. Ele relata ter perdido 5 kg sem fazer dieta. A radiografia de tórax mostrou uma mancha cavitada.\nQual é a forma de tuberculose apresentada pelo paciente?",
        options: [
          "A) Tuberculose pulmonar",
          "B) Tuberculose ganglionar",
          "C) Tuberculose pleural",
          "D) Tuberculose óssea (Mal de Pott)",
          "E) Tuberculose militar"
        ],
        correctIndex: 0, // A
        explanation: "Tosse por mais de 3 semanas, febre vespertina, sudorese noturna e cavitação radiológica definem a Tuberculose Pulmonar."
      },

      // CASO 4 (Varíola - Caso 1)
      {
        category: "Varíola",
        caseTitle: "Caso 4 – A camareira de hospital",
        patientAge: "Não informada",
        patientSex: "Feminino",
        patientId: "CASO-04",
        questionText: "Em um hospital, uma camareira fica doente após manusear roupas, lençóis e objetos de um paciente. Apesar de ser incomum, o que pode ser?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 0, // A
        explanation: "O vírus da Varíola era contagioso por fômites (roupas de cama e vestuário contendo cascas das lesões)."
      },

      // CASO 5 (Cólera - Caso 1)
      {
        category: "Cólera",
        caseTitle: "Caso 5 – O paciente desidratado",
        patientAge: "35 anos",
        patientSex: "Masculino",
        patientId: "CASO-05",
        questionText: "Um homem de 35 anos chega ao hospital relatando diarreia intensa, com fezes muito líquidas. Ele afirma que os sintomas começaram há aproximadamente 12 horas e que também apresenta vômitos frequentes. Durante o atendimento, o médico percebe que o paciente está muito fraco, com a boca seca e sinais de desidratação. O homem conta que, nos últimos dias, consumiu água de um poço sem tratamento.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 4, // E
        explanation: "Diarreia profusa hídrica aquosa e vômitos decorrentes da água não tratada são a clássica manifestação da Cólera."
      },

      // CASO 6 (Tuberculose - Caso 3)
      {
        category: "Tuberculose",
        caseTitle: "Caso 6 – O jovem com dor no peito",
        patientAge: "Jovem",
        patientSex: "Masculino",
        patientId: "CASO-06",
        questionText: "Um jovem dá entrada na emergência com dor no peito ao respirar fundo e falta de ar há duas semanas. O médico identifica um acúmulo de líquido ao redor do pulmão (derrame pleural). A análise desse líquido revelou uma quantidade elevada de linfócitos e nível alto da enzima Adenosina Deaminase (ADA).\nEsse achado no líquido pleural é muito característico de qual condição?",
        options: [
          "A) Tuberculose pulmonar",
          "B) Tuberculose ganglionar",
          "C) Tuberculose pleural",
          "D) Tuberculose óssea (Mal de Pott)",
          "E) Tuberculose militar"
        ],
        correctIndex: 2, // C
        explanation: "Derrame pleural exsudativo rico em linfócitos com elevação de ADA confirma Tuberculose Pleural."
      },

      // CASO 7 (Peste Negra - Caso 4)
      {
        category: "Peste Negra",
        caseTitle: "Caso 7 – O viajante adoecido (42 anos)",
        patientAge: "42 anos",
        patientSex: "Masculino",
        patientId: "CASO-07",
        questionText: "Um homem de 42 anos chegou a uma cidade apresentando febre, calafrios, dores musculares e inchaços dolorosos no pescoço. Ele contou que havia passado por uma região onde muitos ratos estavam sendo encontrados mortos.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 3, // D
        explanation: "A morte massiva de roedores (epizootia) antecede surtos humanos da Peste Negra transmitida pelas pulgas."
      },

      // CASO 8 (Varíola - Caso 2)
      {
        category: "Varíola",
        caseTitle: "Caso 8 – A criança com erupções",
        patientAge: "Pediatria",
        patientSex: "Infantil",
        patientId: "CASO-08",
        questionText: "Uma criança doente possui febre alta, dores de cabeça, dores no corpo, abatimento e calafrios. Após quatro dias, a febre reduziu, porém surgiram erupções na pele.\nEstes sintomas se relacionam a qual doença infecciosa?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 0, // A
        explanation: "O aparecimento característico das erupções cutâneas ocorria logo após a queda temporária da febre inicial na Varíola."
      },

      // CASO 9 (Cólera - Caso 3)
      {
        category: "Cólera",
        caseTitle: "Caso 9 – A criança em estado grave",
        patientAge: "8 anos",
        patientSex: "Infantil",
        patientId: "CASO-09",
        questionText: "Uma criança de 8 anos é levada ao pronto-socorro após apresentar diarreia muito intensa e vômitos repetidos. A mãe relata que a criança perdeu bastante líquido em poucas horas e agora apresenta olhos fundos, muita sede, fraqueza e pouca produção de urina. A família mora em uma região sem saneamento básico adequado.\nQual é a principal doença suspeita?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 4, // E
        explanation: "Sinais de desidratação grave em poucas horas (olhos fundos, oligúria) em local sem saneamento indicam Cólera."
      },

      // CASO 10 (H1N1 - Caso 2)
      {
        category: "H1N1",
        caseTitle: "Caso 10 – Paciente de 52 anos",
        patientAge: "52 anos",
        patientSex: "Masculino",
        patientId: "CASO-10",
        questionText: "Um homem chega à unidade de saúde com febre de 39,5 °C, tosse persistente e dores musculares intensas. Relata que começou a sentir os sintomas repentinamente e que, em menos de 24 horas, passou de uma sensação de indisposição para um quadro de fraqueza intensa. Também apresenta dor de cabeça e dificuldade para respirar durante pequenos esforços.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 2, // C
        explanation: "Instalação abrupta de febre alta, tosse e prostração intensa é marca do vírus H1N1."
      },

      // CASO 11 (Peste Negra - Caso 2)
      {
        category: "Peste Negra",
        caseTitle: "Caso 11 – A mulher com manchas na pele (24 anos)",
        patientAge: "24 anos",
        patientSex: "Feminino",
        patientId: "CASO-11",
        questionText: "Uma mulher de 24 anos apresentou febre, vômitos, dores pelo corpo e manchas escuras na pele. Também sentia muito cansaço e seu estado de saúde piorava rapidamente.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 3, // D
        explanation: "As manchas escuras na pele caracterizam a necrose tecidual decorrente da Peste Septicêmica."
      },

      // CASO 12 (Tuberculose - Caso 4)
      {
        category: "Tuberculose",
        caseTitle: "Caso 12 – Dor crônica nas costas",
        patientAge: "Adulto",
        patientSex: "Masculino",
        patientId: "CASO-12",
        questionText: "Um paciente apresenta dor crônica nas costas (coluna lombar) há mais de três meses, associada a febre baixa e perda de peso. A ressonância magnética mostrou a destruição das vértebras L2 e L3 e a formação de um abscesso na região (conhecido como \"abscesso frio\").\nComo é chamada essa complicação da tuberculose nos ossos/coluna?",
        options: [
          "A) Tuberculose pulmonar",
          "B) Tuberculose ganglionar",
          "C) Tuberculose pleural",
          "D) Tuberculose óssea (Mal de Pott)",
          "E) Tuberculose militar"
        ],
        correctIndex: 3, // D
        explanation: "A infecção tuberculosa dos corpos vertebrais e discos com abscesso frio é o Mal de Pott (Tuberculose Óssea)."
      },

      // CASO 13 (Cólera - Caso 2)
      {
        category: "Cólera",
        caseTitle: "Caso 13 – O surto na comunidade",
        patientAge: "Vários",
        patientSex: "Comunidade",
        patientId: "CASO-13",
        questionText: "Uma médica é chamada para atender uma pequena comunidade onde várias pessoas começaram a apresentar sintomas semelhantes. Os moradores relatam diarreia abundante, vômitos e dores abdominais. Após investigar, a médica descobre que todos utilizam a mesma fonte de água, que recentemente foi contaminada por esgoto. Algumas pessoas já apresentam sinais de desidratação grave.\nQual doença pode estar causando esse surto?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 4, // E
        explanation: "Surtos decorrentes de fontes de água contaminadas por dejetos/esgoto são a forma clássica de disseminação do Vibrio cholerae."
      },

      // CASO 14 (Varíola - Caso 5)
      {
        category: "Varíola",
        caseTitle: "Caso 14 – Mulher em estado grave",
        patientAge: "Adulta",
        patientSex: "Feminino",
        patientId: "CASO-14",
        questionText: "Uma mulher chega com uma doença cujo período de incubação é de aproximadamente 12 dias e que, depois de quatro dias, progride para uma forma mais grave.\nEla possui qual doença?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 0, // A
        explanation: "O período de incubação típico do vírus Variola variava de 10 a 14 dias (média de 12 dias)."
      },

      // CASO 15 (H1N1 - Caso 4)
      {
        category: "H1N1",
        caseTitle: "Caso 15 – Paciente de 34 anos",
        patientAge: "34 anos",
        patientSex: "Feminino",
        patientId: "CASO-15",
        questionText: "Uma mulher procura atendimento médico relatando cansaço excessivo, dor de cabeça, tosse seca e sensação de frio, mesmo em ambientes aquecidos. Ela afirma que não teve febre muito elevada, mas percebeu uma piora gradual do seu estado geral ao longo de três dias. Também apresenta congestão nasal e dores leves nas articulações.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 2, // C
        explanation: "O quadro da síndrome gripal por H1N1 envolve tosse seca, astenia, congestão nasal e artralgia."
      },

      // CASO 16 (Tuberculose - Caso 2)
      {
        category: "Tuberculose",
        caseTitle: "Caso 16 – A jovem com caroço no pescoço",
        patientAge: "22 anos",
        patientSex: "Feminino",
        patientId: "CASO-16",
        questionText: "Uma jovem de 22 anos notou o aparecimento de um \"caroço\" no pescoço há cerca de um mês. O nódulo não dói, cresceu devagar e, nos últimos dias, começou a expelir uma secreção esbranquiçada e espessa pela pele (aspecto de queijo/caseoso). A biópsia do local mostrou a presença de granuloma com necrose.\nQual é a forma de tuberculose apresentada por esta paciente?",
        options: [
          "A) Tuberculose pulmonar",
          "B) Tuberculose ganglionar",
          "C) Tuberculose pleural",
          "D) Tuberculose óssea (Mal de Pott)",
          "E) Tuberculose militar"
        ],
        correctIndex: 1, // B
        explanation: "Acometimento de linfonodos cervicais com fistulização de necrose caseosa é a Tuberculose Ganglionar."
      },

      // CASO 17 (Peste Negra - Caso 3)
      {
        category: "Peste Negra",
        caseTitle: "Caso 17 – O jovem com dificuldade para respirar (19 anos)",
        patientAge: "19 anos",
        patientSex: "Masculino",
        patientId: "CASO-17",
        questionText: "Um jovem de 19 anos começou a apresentar febre alta, tosse intensa, dor no peito e dificuldade para respirar. Sua tosse passou a apresentar secreção com sangue.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 3, // D
        explanation: "A febre associada à escarro hemoptóico (com sangue) de instalação fulminante aponta para a Peste Pneumônica."
      },

      // CASO 18 (Cólera - Caso 4)
      {
        category: "Cólera",
        caseTitle: "Caso 18 – A viagem inesperada",
        patientAge: "42 anos",
        patientSex: "Masculino",
        patientId: "CASO-18",
        questionText: "Um homem de 42 anos retorna de uma viagem a uma região onde existem registros de casos de cólera. Durante a viagem, consumiu alimentos vendidos na rua e bebeu água de procedência desconhecida. Dois dias depois, começou a apresentar diarreia aquosa abundante, vômitos e cãibras musculares. Ele procura atendimento médico preocupado com os sintomas.\nQual é a hipótese mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 4, // E
        explanation: "Cãibras dolorosas (devido ao desequilíbrio de potássio/sódio) e diarreia aquosa pós-viagem confirmam o diagnóstico de Cólera."
      },

      // CASO 19 (Varíola - Caso 3)
      {
        category: "Varíola",
        caseTitle: "Caso 19 – Tratamento de suporte",
        patientAge: "Adulto",
        patientSex: "Masculino",
        patientId: "CASO-19",
        questionText: "Um homem está realizando um tratamento de suporte para uma doença que não possui cura específica. O tratamento inclui hidratação, analgésicos para dor, antitérmicos, higienização de lesões e cuidados com a pele.\nEle possui qual doença?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 0, // A
        explanation: "A Varíola exigia cuidados essenciais de suporte à pele e hidratação, visto que não havia antiviral específico."
      },

      // CASO 20 (H1N1 - Caso 3)
      {
        category: "H1N1",
        caseTitle: "Caso 20 – Paciente de 7 anos",
        patientAge: "7 anos",
        patientSex: "Infantil",
        patientId: "CASO-20",
        questionText: "Uma criança é levada pelos responsáveis ao hospital após apresentar episódios de vômito, dor abdominal e febre moderada. Os pais relatam que ela também está com tosse ocasional, pouco apetite e sonolência maior que o habitual. Não apresenta muitas queixas de dor muscular, mas está mais quieta e indisposta.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 2, // C
        explanation: "Em crianças, a H1N1 pode manifestar-se com vômitos e dores abdominais associados a sintomas respiratórios."
      },

      // CASO 21 (Tuberculose - Caso 5)
      {
        category: "Tuberculose",
        caseTitle: "Caso 21 – Aspecto de grãos de milho",
        patientAge: "Imunodeprimido",
        patientSex: "Indefinido",
        patientId: "CASO-21",
        questionText: "Um paciente com imunidade baixa apresenta febre diária, falta de ar e fraqueza há três semanas. A radiografia de tórax revelou vários pontinhos bem pequenos espalhados por todo o pulmão, lembrando vários \"grãos de milho\".\nEssa apresentação grave e espalhada pelo sangue é chamada de qual forma de tuberculose?",
        options: [
          "A) Tuberculose pulmonar",
          "B) Tuberculose ganglionar",
          "C) Tuberculose pleural",
          "D) Tuberculose óssea (Mal de Pott)",
          "E) Tuberculose militar"
        ],
        correctIndex: 4, // E
        explanation: "O padrão micronodular em grãos de milho disseminado pelo sangue é a Tuberculose Miliar."
      },

      // CASO 22 (Peste Negra - Caso 5)
      {
        category: "Peste Negra",
        caseTitle: "Caso 22 – O paciente em estado grave (76 anos)",
        patientAge: "76 anos",
        patientSex: "Masculino",
        patientId: "CASO-22",
        questionText: "Um homem de 76 anos foi levado ao hospital com febre muito alta, fraqueza extrema, dores abdominais e vômitos. Seus dedos começaram a apresentar manchas escuras, indicando possíveis problemas na circulação sanguínea.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 3, // D
        explanation: "A necrose periférica das extremidades (dedos negros) é o motivo do termo histórico 'Peste Negra'."
      },

      // CASO 23 (Cólera - Caso 5)
      {
        category: "Cólera",
        caseTitle: "Caso 23 – A investigação do médico",
        patientAge: "56 anos",
        patientSex: "Feminino",
        patientId: "CASO-23",
        questionText: "Uma paciente de 56 anos chega ao hospital apresentando diarreia intensa, vômitos e cãibras nas pernas. Ela relata que outras pessoas de sua família também ficaram doentes após uma refeição realizada em casa. O médico suspeita de uma doença bacteriana transmitida pela ingestão de alimentos contaminados. Para confirmar sua hipótese, solicita exames laboratoriais e inicia a reposição de líquidos imediatamente.\nQual alternativa apresenta a doença suspeita?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 4, // E
        explanation: "A reposição hidroeletrolítica vigorosa imediata é a prioridade vital no combate à Cólera."
      },

      // CASO 24 (Varíola - Caso 4)
      {
        category: "Varíola",
        caseTitle: "Caso 24 – Estudo histórico de pandemia",
        patientAge: "Histórico",
        patientSex: "N/A",
        patientId: "CASO-24",
        questionText: "Uma historiadora estuda uma doença que chegou a nível mundial. Esta doença teve seu auge no século XVIII, calcula-se que tenha matado mais de 300 milhões de pessoas e foi totalmente erradicada em 1980.\nQual pandemia global é essa?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 0, // A
        explanation: "A Varíola foi a primeira e única doença humana viral erradicada no mundo através de vacinação (1980)."
      },

      // CASO 25 (H1N1 - Caso 5)
      {
        category: "H1N1",
        caseTitle: "Caso 25 – Paciente de 70 anos",
        patientAge: "70 anos",
        patientSex: "Masculino",
        patientId: "CASO-25",
        questionText: "Um idoso é levado ao pronto-socorro após apresentar confusão mental leve, perda de apetite e fraqueza. A família relata que ele começou a tossir há alguns dias e apresentou febre baixa, mas não reclamou de dores no corpo. Durante a avaliação, os profissionais percebem uma respiração mais acelerada que o habitual e uma queda na disposição para realizar atividades simples.\nQual é a doença mais provável?",
        options: [
          "A) Varíola",
          "B) Tuberculose",
          "C) H1N1",
          "D) Peste Negra",
          "E) Cólera"
        ],
        correctIndex: 2, // C
        explanation: "Nos idosos, infecções virais agudas como H1N1 cursam frequentemente com atipias como desorientação e taquipneia."
      }
    ];

    /* ==========================================================================
       STATE MANAGEMENT & AUDIO
       ========================================================================== */
    let gameState = {
      playerName: "",
      currentQuestionIndex: 0,
      score: 0,
      correctCount: 0,
      userAnswers: [],
      startTime: 0,
      questionTimer: 30,
      timerInterval: null,
      soundEnabled: true
    };

    // Web Audio API Synthesizer
    let audioCtx = null;

    function getAudioContext() {
      if (!audioCtx) {
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        if (AudioContext) audioCtx = new AudioContext();
      }
      if (audioCtx && audioCtx.state === 'suspended') {
        audioCtx.resume();
      }
      return audioCtx;
    }

    function playSound(type) {
      if (!gameState.soundEnabled) return;
      const ctx = getAudioContext();
      if (!ctx) return;

      try {
        if (type === 'beep') {
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = 'sine';
          osc.frequency.setValueAtTime(880, ctx.currentTime);
          gain.gain.setValueAtTime(0.05, ctx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.08);
          osc.connect(gain);
          gain.connect(ctx.destination);
          osc.start();
          osc.stop(ctx.currentTime + 0.08);
        } else if (type === 'stamp') {
          // Low heavy thud stamp sound
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(140, ctx.currentTime);
          osc.frequency.exponentialRampToValueAtTime(30, ctx.currentTime + 0.15);
          gain.gain.setValueAtTime(0.4, ctx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.15);
          osc.connect(gain);
          gain.connect(ctx.destination);
          osc.start();
          osc.stop(ctx.currentTime + 0.15);
        } else if (type === 'correct') {
          // Upward medical success tone
          const now = ctx.currentTime;
          const osc1 = ctx.createOscillator();
          const osc2 = ctx.createOscillator();
          const gain = ctx.createGain();

          osc1.type = 'sine';
          osc2.type = 'sine';
          osc1.frequency.setValueAtTime(523.25, now); // C5
          osc1.frequency.setValueAtTime(659.25, now + 0.1); // E5
          osc2.frequency.setValueAtTime(783.99, now + 0.1); // G5

          gain.gain.setValueAtTime(0.15, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.35);

          osc1.connect(gain);
          osc2.connect(gain);
          gain.connect(ctx.destination);

          osc1.start(now);
          osc2.start(now + 0.1);
          osc1.stop(now + 0.35);
          osc2.stop(now + 0.35);
        } else if (type === 'incorrect') {
          // Warning error buzzer
          const now = ctx.currentTime;
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(150, now);
          osc.frequency.setValueAtTime(110, now + 0.1);
          gain.gain.setValueAtTime(0.2, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.3);
          osc.connect(gain);
          gain.connect(ctx.destination);
          osc.start(now);
          osc.stop(now + 0.3);
        }
      } catch (e) {
        console.error("Audio playback error", e);
      }
    }

    function toggleSound() {
      gameState.soundEnabled = !gameState.soundEnabled;
      const icon = document.getElementById('soundIcon');
      const text = document.getElementById('soundText');
      
      if (gameState.soundEnabled) {
        icon.setAttribute('data-lucide', 'volume-2');
        icon.className = 'w-4 h-4 text-emerald-400';
        text.innerText = 'Som: LIGADO';
        playSound('beep');
      } else {
        icon.setAttribute('data-lucide', 'volume-x');
        icon.className = 'w-4 h-4 text-red-400';
        text.innerText = 'Som: DESLIGADO';
      }
      lucide.createIcons();
    }

    /* ==========================================================================
       SCREEN RENDERING & NAVIGATION
       ========================================================================== */
    const screenContainer = document.getElementById('screenContainer');

    // 1. START SCREEN
    function renderStartScreen() {
      clearInterval(gameState.timerInterval);
      
      screenContainer.innerHTML = `
        <!-- HEADER DO HOSPITAL -->
        <div class="border-b-2 border-stone-800 pb-4 mb-6">
          <div class="flex items-center justify-between flex-wrap gap-2">
            <div class="flex items-center space-x-3">
              <div class="w-12 h-12 rounded-full bg-red-700 text-white flex items-center justify-center font-bold text-xl shadow-md border-2 border-red-900">
                +
              </div>
              <div>
                <h1 class="text-xl sm:text-2xl font-black font-medical text-stone-900 tracking-wider">HOSPITAL CENTRAL</h1>
                <p class="text-xs text-stone-600 font-semibold tracking-widest uppercase">DEPARTAMENTO DE EPIDEMIOLOGIA E INFECTOLOGIA</p>
              </div>
            </div>
            <div class="text-right text-xs font-medical text-stone-600 hidden sm:block">
              <div>SETOR: TRIAGEM E EMERGÊNCIA</div>
              <div>PROTOCOLO: EVAL-2026</div>
            </div>
          </div>
        </div>

        <!-- CLINICAL WELCOME SHEET -->
        <div class="flex-1 flex flex-col justify-between">
          <div class="space-y-6">
            
            <div class="bg-amber-100/60 border border-amber-300 rounded-lg p-4 text-stone-800 text-sm font-medical space-y-2">
              <div class="font-bold text-stone-900 flex items-center gap-2">
                <i data-lucide="clipboard-list" class="w-5 h-5 text-amber-800"></i>
                EXAME DE QUALIFICAÇÃO CLÍNICA
              </div>
              <p class="leading-relaxed">
                Bem-vindo ao sistema de avaliação de prontuários. Você analisará <strong class="text-stone-950">25 casos clínicos reais</strong> englobando as seguintes doenças: 
                <span class="font-bold text-red-900">Peste Negra, Varíola, H1N1, Cólera e Tuberculose</span>.
              </p>
              <p class="text-xs text-stone-600 italic">
                * Avalie os sintomas, histórico dos pacientes e responda com o diagnóstico correto para receber suas pontuações de triagem.
              </p>
            </div>

            <!-- DOCTOR NAME INPUT FORM -->
            <div class="bg-stone-100 p-5 rounded-lg border border-stone-300 shadow-inner space-y-4">
              <label class="block font-bold text-stone-800 font-medical text-sm">
                IDENTIFICAÇÃO DO PROFISSIONAL RESPONSÁVEL:
              </label>
              <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-stone-500 font-medical font-bold">
                  Dr(a).
                </span>
                <input 
                  type="text" 
                  id="doctorNameInput" 
                  placeholder="Seu Nome ou Sobrenome..." 
                  maxlength="25"
                  class="w-full pl-16 pr-4 py-3 bg-white border-2 border-stone-400 rounded-md text-stone-900 font-bold font-medical focus:outline-none focus:border-emerald-700 transition"
                  value="${gameState.playerName || ''}"
                />
              </div>
            </div>

            <!-- INSTRUCTIONS SUMMARY BOX -->
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-3 text-xs font-medical text-stone-700">
              <div class="bg-white/80 p-3 rounded border border-stone-300 flex items-center gap-2">
                <i data-lucide="help-circle" class="w-5 h-5 text-blue-700 flex-shrink-0"></i>
                <span><strong>25 Casos</strong> com alternativas objetivas</span>
              </div>
              <div class="bg-white/80 p-3 rounded border border-stone-300 flex items-center gap-2">
                <i data-lucide="timer" class="w-5 h-5 text-amber-700 flex-shrink-0"></i>
                <span><strong>30s por caso</strong> + bônus de agilidade</span>
              </div>
              <div class="bg-white/80 p-3 rounded border border-stone-300 flex items-center gap-2">
                <i data-lucide="award" class="w-5 h-5 text-emerald-700 flex-shrink-0"></i>
                <span><strong>Ranking salvo</strong> com histórico local</span>
              </div>
            </div>

          </div>

          <!-- START BUTTON -->
          <div class="pt-6 mt-6 border-t border-stone-300 flex flex-col sm:flex-row gap-3 justify-end items-center">
            <button onclick="showRankingModal()" class="w-full sm:w-auto px-5 py-3 bg-stone-200 hover:bg-stone-300 text-stone-800 font-bold font-medical rounded-md text-sm transition flex items-center justify-center gap-2 border border-stone-400">
              <i data-lucide="trophy" class="w-4 h-4 text-amber-700"></i>
              VER RANKING ATUAL
            </button>
            <button onclick="startGame()" class="w-full sm:w-auto px-8 py-3.5 bg-emerald-800 hover:bg-emerald-900 text-white font-bold font-medical rounded-md text-base shadow-lg transition flex items-center justify-center gap-2 border border-emerald-950">
              <i data-lucide="play-circle" class="w-5 h-5"></i>
              INICIAR TRIAGEM DOS CASOS
            </button>
          </div>
        </div>
      `;
      lucide.createIcons();
    }

    // START GAME INITIALIZATION
    function startGame() {
      const nameInput = document.getElementById('doctorNameInput');
      let name = nameInput ? nameInput.value.trim() : "";
      if (!name) name = "Médico Anônimo";
      
      gameState.playerName = name;
      gameState.currentQuestionIndex = 0;
      gameState.score = 0;
      gameState.correctCount = 0;
      gameState.userAnswers = [];
      gameState.startTime = Date.now();

      renderQuestionScreen();
    }

    // 2. QUESTION SCREEN
    function renderQuestionScreen() {
      clearInterval(gameState.timerInterval);
      const q = ALL_QUESTIONS[gameState.currentQuestionIndex];
      gameState.questionTimer = 30;

      screenContainer.innerHTML = `
        <!-- VITAL SIGNS MONITOR HEADER -->
        <div class="bg-stone-900 rounded-lg p-3 text-emerald-400 mb-4 border border-stone-800 shadow-md">
          <div class="flex justify-between items-center text-xs font-medical mb-1">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-ping"></span>
              <span class="text-emerald-300 font-bold">MONITOR DE SINAIS VITAIS</span>
            </div>
            <div class="text-stone-400">
              MÉDICO: <span class="text-white font-bold">Dr(a). ${escapeHtml(gameState.playerName)}</span>
            </div>
          </div>
          
          <div class="grid grid-cols-12 gap-2 items-center">
            <!-- Animated ECG Pulse Line -->
            <div class="col-span-7 sm:col-span-8 bg-black/60 rounded p-1 h-10 border border-emerald-900 flex items-center overflow-hidden">
              <svg class="w-full h-full text-emerald-500" viewBox="0 0 500 50" preserveAspectRatio="none">
                <path class="ecg-line" d="M 0,25 L 100,25 L 110,10 L 120,40 L 130,5 L 140,45 L 150,25 L 250,25 L 260,10 L 270,40 L 280,5 L 290,45 L 300,25 L 400,25 L 410,10 L 420,40 L 430,5 L 440,45 L 450,25 L 500,25" fill="none" stroke="currentColor" stroke-width="2.5"/>
              </svg>
            </div>
            
            <!-- Timer Display -->
            <div class="col-span-5 sm:col-span-4 bg-black/80 rounded p-1.5 border border-emerald-800 text-center flex items-center justify-between px-3">
              <span class="text-[10px] text-emerald-600 font-bold">TEMPO:</span>
              <span id="timerText" class="text-xl font-bold font-medical text-amber-400 animate-pulse">30s</span>
            </div>
          </div>
        </div>

        <!-- PROGRESS & METADATA BAR -->
        <div class="flex justify-between items-center text-xs font-medical text-stone-700 border-b border-stone-300 pb-2 mb-4">
          <div>
            PRONTUÁRIO Nº: <span class="font-bold text-stone-900">${q.patientId}</span>
          </div>
          <div>
            CASO <span class="font-bold text-red-900">${gameState.currentQuestionIndex + 1}</span> DE <span class="font-bold">25</span>
          </div>
          <div class="hidden sm:block">
            CATEGORIA: <span class="font-bold uppercase text-amber-900">${q.category}</span>
          </div>
        </div>

        <!-- PATIENT CLINICAL CHART SHEET -->
        <div class="bg-white/90 rounded-lg p-4 sm:p-5 border border-stone-300 shadow-sm mb-4 space-y-3 font-medical">
          
          <!-- Case Header & Title -->
          <div class="flex justify-between items-start border-b border-stone-200 pb-2 flex-wrap gap-2">
            <h2 class="text-base sm:text-lg font-bold text-stone-900 font-medical">${q.caseTitle}</h2>
            <div class="text-xs bg-stone-100 text-stone-700 px-2 py-1 rounded border border-stone-300">
              Idade: ${q.patientAge} | Sexo: ${q.patientSex}
            </div>
          </div>

          <!-- Question Description Text -->
          <div class="text-sm text-stone-800 leading-relaxed font-medical whitespace-pre-line bg-stone-50/80 p-3 rounded border border-stone-200">
${q.questionText}
          </div>

        </div>

        <!-- DIAGNOSTIC OPTIONS LIST -->
        <div class="space-y-2 mb-6">
          <div class="text-xs font-bold font-medical text-stone-700 uppercase tracking-wide mb-1">
            SELEÇÃO DE DIAGNÓSTICO HIPOTÉTICO:
          </div>
          ${q.options.map((opt, idx) => `
            <button 
              onclick="selectAnswer(${idx})" 
              class="w-full text-left p-3.5 bg-stone-100 hover:bg-amber-100/80 active:bg-amber-200 border-2 border-stone-300 hover:border-amber-700 rounded-lg transition font-medical font-bold text-sm text-stone-800 flex items-center justify-between group shadow-sm"
            >
              <span class="flex items-center gap-3">
                <span class="w-6 h-6 rounded-full border-2 border-stone-400 group-hover:border-amber-800 group-hover:bg-amber-700 group-hover:text-white flex items-center justify-center text-xs font-bold transition">
                  ${String.fromCharCode(65 + idx)}
                </span>
                <span>${escapeHtml(opt)}</span>
              </span>
              <i data-lucide="chevron-right" class="w-4 h-4 text-stone-400 group-hover:text-amber-800 transition"></i>
            </button>
          `).join('')}
        </div>
      `;

      lucide.createIcons();

      // Start Countdown Timer for this question
      gameState.timerInterval = setInterval(() => {
        gameState.questionTimer--;
        const timerText = document.getElementById('timerText');
        if (timerText) {
          timerText.innerText = `${gameState.questionTimer}s`;
          if (gameState.questionTimer <= 5) {
            timerText.className = "text-xl font-bold font-medical text-red-500 animate-bounce";
            playSound('beep');
          }
        }

        if (gameState.questionTimer <= 0) {
          clearInterval(gameState.timerInterval);
          selectAnswer(-1); // Time expired -> Auto incorrect
        }
      }, 1000);
    }

    // 3. ANSWER SELECTION & FEEDBACK SCREEN
    function selectAnswer(selectedIndex) {
      clearInterval(gameState.timerInterval);
      
      const q = ALL_QUESTIONS[gameState.currentQuestionIndex];
      const isCorrect = (selectedIndex === q.correctIndex);
      const timeBonus = isCorrect ? Math.max(5, gameState.questionTimer * 3) : 0;
      const questionPoints = isCorrect ? (100 + timeBonus) : 0;

      if (isCorrect) {
        gameState.score += questionPoints;
        gameState.correctCount++;
        playSound('correct');
      } else {
        playSound('incorrect');
      }

      playSound('stamp');

      gameState.userAnswers.push({
        questionIndex: gameState.currentQuestionIndex,
        selectedIndex: selectedIndex,
        isCorrect: isCorrect,
        points: questionPoints
      });

      // Render Feedback Overlay with Stamp
      screenContainer.innerHTML = `
        <!-- FEEDBACK HEADER -->
        <div class="flex justify-between items-center border-b-2 border-stone-800 pb-3 mb-4 font-medical">
          <div>
            <span class="text-xs text-stone-500">PARECER CLÍNICO PARA:</span>
            <div class="font-bold text-stone-900">${q.caseTitle}</div>
          </div>
          <div class="text-right">
            <span class="text-xs text-stone-500">PONTUAÇÃO ACUMULADA:</span>
            <div class="font-bold text-emerald-800 text-lg">${gameState.score} PTS</div>
          </div>
        </div>

        <!-- STAMP & EVALUATION SECTION -->
        <div class="bg-white/95 rounded-lg p-5 border border-stone-300 shadow-md mb-6 relative overflow-hidden font-medical">
          
          <!-- DYNAMIC RUBBER STAMP -->
          <div class="flex justify-center my-4">
            ${isCorrect ? `
              <div class="stamp stamp-approved font-stamp text-2xl sm:text-3xl tracking-widest" style="--rotation: -4deg">
                DIAGNÓSTICO CONFIRMADO
              </div>
            ` : `
              <div class="stamp stamp-rejected font-stamp text-2xl sm:text-3xl tracking-widest" style="--rotation: 5deg">
                DIAGNÓSTICO INCORRETO
              </div>
            `}
          </div>

          <!-- DIAGNOSTIC RESULT DETAIL -->
          <div class="space-y-3 text-sm text-stone-800 mt-4 border-t border-stone-200 pt-4">
            <div class="flex items-start gap-2">
              <span class="font-bold text-stone-600 min-w-[140px]">Sua Escolha:</span>
              <span class="font-bold ${isCorrect ? 'text-green-800' : 'text-red-700'}">
                ${selectedIndex >= 0 ? escapeHtml(q.options[selectedIndex]) : 'Tempo Esgotado! Sem resposta.'}
              </span>
            </div>
            
            ${!isCorrect ? `
              <div class="flex items-start gap-2">
                <span class="font-bold text-stone-600 min-w-[140px]">Diagnóstico Correto:</span>
                <span class="font-bold text-green-800">
                  ${escapeHtml(q.options[q.correctIndex])}
                </span>
              </div>
            ` : ''}

            <!-- MEDICAL EXPLANATION -->
            <div class="bg-amber-50/80 p-3.5 rounded-md border border-amber-200 mt-3 text-xs leading-relaxed">
              <div class="font-bold text-amber-900 mb-1 flex items-center gap-1.5">
                <i data-lucide="file-text" class="w-4 h-4"></i>
                FUNDAMENTAÇÃO CLÍNICA:
              </div>
              <p class="text-stone-800 font-sans-clean">${escapeHtml(q.explanation)}</p>
            </div>
          </div>

        </div>

        <!-- NEXT BUTTON -->
        <div class="flex justify-end pt-2 font-medical">
          <button 
            onclick="nextQuestion()" 
            class="px-8 py-3 bg-stone-900 hover:bg-stone-800 text-white font-bold rounded-md text-sm transition shadow-lg flex items-center gap-2"
          >
            <span>${gameState.currentQuestionIndex < 24 ? 'PRÓXIMO PACIENTE' : 'CONCLUIR AVALIAÇÃO FINAL'}</span>
            <i data-lucide="arrow-right" class="w-4 h-4"></i>
          </button>
        </div>
      `;

      lucide.createIcons();
    }

    function nextQuestion() {
      gameState.currentQuestionIndex++;
      if (gameState.currentQuestionIndex < 25) {
        renderQuestionScreen();
      } else {
        renderResultsScreen();
      }
    }

    // 4. RESULTS SCREEN
    function renderResultsScreen() {
      clearInterval(gameState.timerInterval);
      
      const totalQuestions = 25;
      const accuracyPct = Math.round((gameState.correctCount / totalQuestions) * 100);
      const totalTimeSec = Math.round((Date.now() - gameState.startTime) / 1000);

      // Title Title Assignment
      let doctorGrade = "";
      let gradeColor = "";
      if (accuracyPct === 100) {
        doctorGrade = "Chefe de Infectologia (Doutor Honoris Causa)";
        gradeColor = "text-emerald-700";
      } else if (accuracyPct >= 80) {
        doctorGrade = "Médico Especialista em Epidemias";
        gradeColor = "text-emerald-600";
      } else if (accuracyPct >= 60) {
        doctorGrade = "Médico Residente de Infectologia";
        gradeColor = "text-blue-700";
      } else if (accuracyPct >= 40) {
        doctorGrade = "Interno de Medicina Geral";
        gradeColor = "text-amber-700";
      } else {
        doctorGrade = "Estagiário de Pronto-Socorro";
        gradeColor = "text-red-700";
      }

      // Save Score to LocalStorage Ranking
      saveScoreToLeaderboard({
        name: gameState.playerName,
        score: gameState.score,
        correct: gameState.correctCount,
        accuracy: accuracyPct,
        grade: doctorGrade,
        timeSeconds: totalTimeSec,
        date: new Date().toLocaleDateString('pt-BR')
      });

      screenContainer.innerHTML = `
        <!-- HOSPITAL OFFICIAL LAUDO HEADER -->
        <div class="border-b-2 border-stone-800 pb-4 mb-4 text-center">
          <div class="text-xs font-bold tracking-widest text-stone-500 uppercase font-medical">LAUDO FINAL DE DESEMPENHO MÉDICO</div>
          <h2 class="text-2xl font-black font-medical text-stone-900 mt-1">PRONTUÁRIO DE AVALIAÇÃO CONCLUÍDO</h2>
        </div>

        <!-- DOCTOR SUMMARY CARD -->
        <div class="bg-white/95 rounded-lg p-5 border border-stone-300 shadow-sm mb-6 font-medical space-y-4">
          <div class="flex justify-between items-center border-b border-stone-200 pb-3 flex-wrap gap-2">
            <div>
              <span class="text-xs text-stone-500">MÉDICO AVALIADO:</span>
              <div class="text-lg font-bold text-stone-900">Dr(a). ${escapeHtml(gameState.playerName)}</div>
            </div>
            <div class="text-right">
              <span class="text-xs text-stone-500">TITULAÇÃO CONFERIDA:</span>
              <div class="text-sm font-bold ${gradeColor}">${doctorGrade}</div>
            </div>
          </div>

          <!-- METRICS GRID -->
          <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 text-center pt-2">
            <div class="bg-stone-50 p-3 rounded border border-stone-200">
              <div class="text-2xl font-bold text-emerald-800">${gameState.score}</div>
              <div class="text-[10px] text-stone-500 font-bold uppercase">Pontuação Total</div>
            </div>
            <div class="bg-stone-50 p-3 rounded border border-stone-200">
              <div class="text-2xl font-bold text-stone-900">${gameState.correctCount} / 25</div>
              <div class="text-[10px] text-stone-500 font-bold uppercase">Acertos</div>
            </div>
            <div class="bg-stone-50 p-3 rounded border border-stone-200">
              <div class="text-2xl font-bold text-amber-700">${accuracyPct}%</div>
              <div class="text-[10px] text-stone-500 font-bold uppercase">Taxa de Precisão</div>
            </div>
            <div class="bg-stone-50 p-3 rounded border border-stone-200">
              <div class="text-2xl font-bold text-blue-800">${totalTimeSec}s</div>
              <div class="text-[10px] text-stone-500 font-bold uppercase">Tempo Decorrido</div>
            </div>
          </div>
        </div>

        <!-- DETAILED RECAP LIST -->
        <div class="flex-1 overflow-hidden flex flex-col mb-4">
          <div class="text-xs font-bold font-medical text-stone-700 uppercase tracking-wide mb-2 flex justify-between items-center">
            <span>RESUMO DOS CASOS AVALIADOS:</span>
            <span class="text-stone-500 font-normal">Verifique seu histórico individual</span>
          </div>

          <div class="flex-1 overflow-y-auto max-h-[220px] bg-white/80 border border-stone-300 rounded-lg p-3 space-y-2 font-medical text-xs">
            ${gameState.userAnswers.map((ans, idx) => {
              const q = ALL_QUESTIONS[idx];
              return `
                <div class="p-2 rounded border flex justify-between items-center gap-2 ${ans.isCorrect ? 'bg-green-50/80 border-green-200 text-green-900' : 'bg-red-50/80 border-red-200 text-red-900'}">
                  <div class="truncate">
                    <span class="font-bold">${idx + 1}. ${q.caseTitle}</span>
                    <span class="text-[10px] opacity-75 block">${q.category}</span>
                  </div>
                  <div class="flex items-center gap-2 flex-shrink-0">
                    <span class="font-bold">${ans.isCorrect ? '+ ' + ans.points + ' pts' : '0 pts'}</span>
                    <i data-lucide="${ans.isCorrect ? 'check-circle' : 'x-circle'}" class="w-4 h-4 ${ans.isCorrect ? 'text-green-700' : 'text-red-700'}"></i>
                  </div>
                </div>
              `;
            }).join('')}
          </div>
        </div>

        <!-- ACTION BUTTONS -->
        <div class="pt-3 border-t border-stone-300 flex flex-col sm:flex-row gap-3 justify-between items-center font-medical">
          <button onclick="showRankingModal()" class="w-full sm:w-auto px-5 py-3 bg-amber-900 hover:bg-amber-950 text-amber-100 font-bold rounded-md text-xs transition flex items-center justify-center gap-2">
            <i data-lucide="trophy" class="w-4 h-4 text-amber-400"></i>
            VER RANKING DE LÍDERES
          </button>
          <button onclick="renderStartScreen()" class="w-full sm:w-auto px-8 py-3 bg-stone-900 hover:bg-stone-800 text-white font-bold rounded-md text-xs transition flex items-center justify-center gap-2">
            <i data-lucide="rotate-ccw" class="w-4 h-4"></i>
            NOVO DIAGNÓSTICO
          </button>
        </div>
      `;

      lucide.createIcons();
    }

    /* ==========================================================================
       RANKING & LOCALSTORAGE LOGIC
       ========================================================================== */
    const RANKING_STORAGE_KEY = 'prontuario_medico_ranking_v1';

    function getLeaderboard() {
      try {
        const data = localStorage.getItem(RANKING_STORAGE_KEY);
        return data ? JSON.parse(data) : [];
      } catch (e) {
        return [];
      }
    }

    function saveScoreToLeaderboard(entry) {
      const list = getLeaderboard();
      list.push(entry);
      // Sort by score descending, then by time ascending
      list.sort((a, b) => {
        if (b.score !== a.score) return b.score - a.score;
        return a.timeSeconds - b.timeSeconds;
      });
      // Keep top 50 entries
      const trimmed = list.slice(0, 50);
      try {
        localStorage.setItem(RANKING_STORAGE_KEY, JSON.stringify(trimmed));
      } catch (e) {
        console.error("Storage error", e);
      }
    }

    function showRankingModal() {
      const modal = document.getElementById('rankingModal');
      const listContainer = document.getElementById('rankingListContainer');
      const list = getLeaderboard();

      if (list.length === 0) {
        listContainer.innerHTML = `
          <div class="text-center py-12 text-stone-500 font-medical">
            <i data-lucide="clipboard-x" class="w-12 h-12 mx-auto mb-3 text-stone-600"></i>
            <p class="text-sm font-bold">Nenhum registro de diagnóstico encontrado.</p>
            <p class="text-xs text-stone-600 mt-1">Conclua uma triagem para registrar sua pontuação!</p>
          </div>
        `;
      } else {
        listContainer.innerHTML = `
          <table class="w-full text-left font-medical text-xs text-stone-300">
            <thead class="bg-stone-950 text-amber-400 uppercase border-b border-stone-800">
              <tr>
                <th class="p-2.5 text-center">#</th>
                <th class="p-2.5">Médico</th>
                <th class="p-2.5 text-center">Pontos</th>
                <th class="p-2.5 text-center">Precisão</th>
                <th class="p-2.5 text-center">Tempo</th>
                <th class="p-2.5 text-right">Data</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-stone-800">
              ${list.map((item, idx) => `
                <tr class="${idx === 0 ? 'bg-amber-950/30 text-amber-200 font-bold' : 'hover:bg-stone-800/50'}">
                  <td class="p-2.5 text-center font-bold ${idx === 0 ? 'text-amber-400' : 'text-stone-500'}">
                    ${idx === 0 ? '🥇 1' : idx === 1 ? '🥈 2' : idx === 2 ? '🥉 3' : (idx + 1)}
                  </td>
                  <td class="p-2.5 font-bold text-stone-100">
                    Dr(a). ${escapeHtml(item.name)}
                    <span class="block text-[10px] text-stone-500 font-normal">${escapeHtml(item.grade || '')}</span>
                  </td>
                  <td class="p-2.5 text-center font-bold text-emerald-400">${item.score}</td>
                  <td class="p-2.5 text-center">${item.accuracy}%</td>
                  <td class="p-2.5 text-center">${item.timeSeconds}s</td>
                  <td class="p-2.5 text-right text-stone-500">${item.date}</td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        `;
      }

      modal.classList.remove('hidden');
      lucide.createIcons();
    }

    function closeRankingModal() {
      document.getElementById('rankingModal').classList.add('hidden');
    }

    function confirmResetRanking() {
      document.getElementById('resetConfirmModal').classList.remove('hidden');
    }

    function closeResetConfirmModal() {
      document.getElementById('resetConfirmModal').classList.add('hidden');
    }

    function executeResetRanking() {
      try {
        localStorage.removeItem(RANKING_STORAGE_KEY);
      } catch (e) {
        console.error("Clear storage error", e);
      }
      closeResetConfirmModal();
      showRankingModal();
      playSound('incorrect');
    }

    // Helper: Escape HTML string safely
    function escapeHtml(str) {
      if (!str) return '';
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#039;');
    }

    // Initialize application on window load
    window.onload = function() {
      renderStartScreen();
    };
  </script>
</body>
</html>
