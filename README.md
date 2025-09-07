<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Demande de Maquette Club</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            scroll-behavior: smooth;
        }

        .step {
            transition: all 0.3s ease;
        }

        .step-active {
            border-color: #4f46e5;
            color: #4f46e5;
            font-weight: 600;
        }

        .step-completed {
            border-color: #16a34a;
            color: #16a34a;
            background-color: #f0fdf4;
        }

        .step-line {
            transition: all 0.3s ease;
            background-color: #e5e7eb;
        }

        .step-line-completed {
            background-color: #16a34a;
        }

        .color-swatch {
            transition: all 0.2s ease-in-out;
            cursor: pointer;
        }

        .color-swatch:hover {
            transform: scale(1.1);
        }

        .color-swatch.selected {
            box-shadow: 0 0 0 3px #4f46e5;
            transform: scale(1.1);
        }
        
        .modal { 
            display: none; 
            transition: opacity 0.3s ease;
        }
        .modal.is-open { 
            display: flex; 
        }

        [data-step-content] {
            display: none;
        }

        [data-step-content].active {
            display: block;
            animation: fadeIn 0.5s;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .style-label input:checked + span {
            border-color: #4f46e5;
            background-color: #eef2ff;
        }

        details > summary {
            list-style: none;
        }
        details > summary::-webkit-details-marker {
            display: none;
        }

        .loader {
            border: 4px solid #f3f3f3;
            border-radius: 50%;
            border-top: 4px solid #4f46e5;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Improved Notification Toast */
        .toast-notification {
            transition: opacity 0.3s, transform 0.3s;
            transform: translateY(20px);
            opacity: 0;
            pointer-events: none;
        }

        .toast-notification.show {
            transform: translateY(0);
            opacity: 1;
            pointer-events: auto;
        }
    </style>
</head>

<body class="bg-gray-50 text-gray-800">

    <div class="container mx-auto p-4 sm:p-6 lg:p-8">
        <header class="text-center mb-8">
            <h1 class="text-4xl font-extrabold text-gray-800 tracking-tight">Générateur de Maquette Club</h1>
            <p class="mt-2 text-lg text-gray-500">Créez votre demande de design personnalisé.</p>
        </header>

        <!-- Stepper -->
        <div id="stepper" class="w-full max-w-5xl mx-auto mb-12">
            <div class="flex items-center">
                <div class="step step-active flex-1 flex flex-col items-center border-b-4 pb-2" data-step="1">
                    <div class="w-8 h-8 rounded-full bg-indigo-600 text-white flex items-center justify-center">1</div>
                    <p class="mt-1 text-xs sm:text-sm">Infos Club</p>
                </div>
                <div class="step-line flex-1 h-1"></div>
                <div class="step flex-1 flex flex-col items-center border-b-4 border-gray-200 pb-2 text-gray-400" data-step="2">
                    <div class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center">2</div>
                    <p class="mt-1 text-xs sm:text-sm" data-step-label="2">Style & Couleurs</p>
                </div>
                <div class="step-line flex-1 h-1"></div>
                <div class="step flex-1 flex flex-col items-center border-b-4 border-gray-200 pb-2 text-gray-400" data-step="3">
                    <div class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center">3</div>
                    <p class="mt-1 text-xs sm:text-sm" data-step-label="3">Générateur IA</p>
                </div>
                <div class="step-line flex-1 h-1"></div>
                <div class="step flex-1 flex flex-col items-center border-b-4 border-gray-200 pb-2 text-gray-400" data-step="4">
                    <div class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center">4</div>
                    <p class="mt-1 text-xs sm:text-sm" data-step-label="4">Choix Articles</p>
                </div>
                 <div class="step-line flex-1 h-1"></div>
                <div class="step flex-1 flex flex-col items-center border-b-4 border-gray-200 pb-2 text-gray-400" data-step="5">
                    <div class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center">5</div>
                    <p class="mt-1 text-xs sm:text-sm" data-step-label="5">Finalisation</p>
                </div>
            </div>
        </div>

        <main class="bg-white p-6 rounded-xl shadow-lg mt-6 space-y-8">
            <!-- Step 1: Club Info -->
            <div data-step-content="1" class="active">
                <div class="flex justify-between items-center border-b pb-3 mb-6">
                    <h2 class="text-2xl font-bold text-gray-800">Étape 1: Informations sur le club</h2>
                    <button id="reset-app-btn" class="px-4 py-2 bg-red-600 text-white text-sm font-medium rounded-md hover:bg-red-700">Réinitialiser</button>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div>
                        <label for="clubName" class="block text-sm font-medium text-gray-700">Nom du Club <span class="text-red-500">*</span></label>
                        <input type="text" id="clubName" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">
                        <p id="clubName-error" class="text-red-600 text-sm mt-1 h-4"></p>
                    </div>
                    <div>
                        <label for="clientNumber" class="block text-sm font-medium text-gray-700">N° client <span class="text-red-500">*</span></label>
                        <input type="text" id="clientNumber" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">
                         <p id="clientNumber-error" class="text-red-600 text-sm mt-1 h-4"></p>
                    </div>
                     <div>
                        <label for="requestDate" class="block text-sm font-medium text-gray-700">Date du jour</label>
                        <input type="date" id="requestDate" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 bg-gray-100" readonly>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700">Sport Principal <span class="text-red-500">*</span></label>
                        <div class="mt-2 flex flex-wrap gap-x-4 gap-y-2">
                             <div class="flex items-center">
                                <input id="sport-competition" name="sport" type="radio" value="cyclisme_competition" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500" checked>
                                <label for="sport-competition" class="ml-2 block text-sm font-medium text-gray-700">CYCLISME COMPÉTITION</label>
                            </div>
                            <div class="flex items-center">
                                <input id="sport-touring" name="sport" type="radio" value="cyclotourisme" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-touring" class="ml-2 block text-sm font-medium text-gray-700">CYCLOTOURISME</label>
                            </div>
                             <div class="flex items-center">
                                <input id="sport-mtb" name="sport" type="radio" value="vtt" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-mtb" class="ml-2 block text-sm font-medium text-gray-700">VTT</label>
                            </div>
                             <div class="flex items-center">
                                <input id="sport-gravel" name="sport" type="radio" value="gravel" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-gravel" class="ml-2 block text-sm font-medium text-gray-700">GRAVEL</label>
                            </div>
                            <div class="flex items-center">
                                <input id="sport-running" name="sport" type="radio" value="running" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-running" class="ml-2 block text-sm font-medium text-gray-700">RUNNING</label>
                            </div>
                            <div class="flex items-center">
                                <input id="sport-trail" name="sport" type="radio" value="trail" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-trail" class="ml-2 block text-sm font-medium text-gray-700">TRAIL</label>
                            </div>
                            <div class="flex items-center">
                                <input id="sport-athletisme" name="sport" type="radio" value="athletisme" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-athletisme" class="ml-2 block text-sm font-medium text-gray-700">ATHLETISME</label>
                            </div>
                            <div class="flex items-center">
                                <input id="sport-triathlon" name="sport" type="radio" value="triathlon" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-triathlon" class="ml-2 block text-sm font-medium text-gray-700">TRIATHLON</label>
                            </div>
                             <div class="flex items-center">
                                <input id="sport-marche" name="sport" type="radio" value="marche" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="sport-marche" class="ml-2 block text-sm font-medium text-gray-700">MARCHE</label>
                            </div>
                        </div>
                    </div>
                     <div class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-700">Type de Demande <span class="text-red-500">*</span></label>
                        <div class="mt-2 flex flex-wrap gap-x-4 gap-y-2">
                             <div class="flex items-center">
                                <input id="request-type-new" name="request-type" type="radio" value="creation" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500" checked>
                                <label for="request-type-new" class="ml-2 block text-sm font-medium text-gray-700">Nouvelle Création</label>
                            </div>
                            <div class="flex items-center">
                                <input id="request-type-mod" name="request-type" type="radio" value="modification" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="request-type-mod" class="ml-2 block text-sm font-medium text-gray-700">Modification d'une Maquette Existante</label>
                            </div>
                            <div class="flex items-center">
                                <input id="request-type-provided" name="request-type" type="radio" value="fournie" class="h-4 w-4 text-indigo-600 border-gray-300 focus:ring-indigo-500">
                                <label for="request-type-provided" class="ml-2 block text-sm font-medium text-gray-700">Maquette Fournie par le Client</label>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Step 2: Design & Colors -->
            <div data-step-content="2">
                <h2 class="text-2xl font-bold text-gray-800 border-b pb-3 mb-6">Étape 2: Style, Couleurs & Logos</h2>
                <div class="space-y-8">
                    <!-- Color & Design Options -->
                    <div class="space-y-6">
                        <div>
                            <h3 class="text-lg font-semibold mb-2 text-gray-700">Palette de couleurs</h3>
                            <p class="text-sm text-gray-500 mb-4">Cliquez sur une famille pour voir les nuances, puis cliquez sur une nuance pour la sélectionner.</p>
                            <div id="color-picker" class="space-y-3">
                                <!-- Color pickers will be injected here -->
                            </div>
                        </div>
                        <div>
                            <h3 class="text-lg font-semibold mb-2 text-gray-700">Styles de Design</h3>
                            <p class="text-sm text-gray-500 mb-4">Sélectionnez un ou plusieurs styles qui vous inspirent.</p>
                            <div id="style-selection-container" class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-3">
                                <!-- Styles will be injected here -->
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                             <div>
                                <h3 class="text-lg font-semibold text-gray-700">Logos & Placement</h3>
                                <p class="text-sm text-gray-500 mb-4">Importez les logos et indiquez leurs emplacements pour la création IA.</p>
                                <input type="file" id="logo-upload" multiple class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100">
                                <div id="logo-preview-area" class="mt-4 grid grid-cols-2 sm:grid-cols-3 gap-4">
                                    <!-- Logo previews will be shown here -->
                                </div>
                            </div>
                            <div>
                                 <h3 class="text-lg font-semibold text-gray-700 invisible">Instructions</h3>
                                 <p class="text-sm text-gray-500 mb-4 invisible">-</p>
                                 <div id="logo-instructions-area" class="space-y-4">
                                    <p class="text-center text-gray-400 p-4 border-2 border-dashed rounded-lg">Importez des logos pour ajouter des instructions.</p>
                                 </div>
                            </div>
                        </div>
                         <div>
                            <h3 class="text-lg font-semibold mb-2 text-gray-700">Images d'Inspiration</h3>
                            <p class="text-sm text-gray-500 mb-4">Importez des images (maillots, motifs...) qui représentent le style que vous recherchez.</p>
                            <input type="file" id="inspiration-upload" multiple class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100">
                            <div id="inspiration-preview-area" class="mt-4 grid grid-cols-2 sm:grid-cols-4 gap-4">
                                <!-- Inspiration previews will be shown here -->
                            </div>
                        </div>
                        <div>
                            <h3 class="text-lg font-semibold text-gray-700">Arrière-plan pour l'IA</h3>
                             <label for="ia-background-input" class="block text-sm font-medium text-gray-500 mb-2">Décrivez l'arrière-plan souhaité pour l'image (ex: "sur une route de montagne", "dans un studio photo", "flou et neutre").</label>
                             <input type="text" id="ia-background-input" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500" value="sur un mannequin, fond de studio neutre">
                        </div>
                         <div>
                            <h3 class="text-lg font-semibold mb-2 text-gray-700">Brief Créatif</h3>
                            <label for="design-notes" class="block text-sm font-medium text-gray-500 mb-2">Décrivez le style que vous recherchez, donnez des exemples (ex: "design moderne avec des lignes géométriques", "style sobre et classique", "inspiré du maillot de l'équipe X").</label>
                            <textarea id="design-notes" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500"></textarea>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 3: AI Generator -->
            <div data-step-content="3">
                <h2 class="text-2xl font-bold text-gray-800 border-b pb-3 mb-6">Étape 3: Préparation pour l'IA</h2>
                 <div class="space-y-6">
                    <div>
                        <h3 class="text-lg font-semibold text-gray-700">1. Générer et Copier le Prompt</h3>
                        <p class="text-sm text-gray-500 mb-4">Cliquez pour créer une description textuelle basée sur vos choix. Copiez ce texte pour l'utiliser dans l'outil de création d'image.</p>
                        <div class="flex gap-4">
                            <button id="generate-prompt-btn" class="w-1/2 px-4 py-2 bg-indigo-600 text-white font-medium rounded-md hover:bg-indigo-700">Générer le Prompt</button>
                            <button id="copy-prompt-btn" class="w-1/2 px-4 py-2 bg-gray-600 text-white font-medium rounded-md hover:bg-gray-700 transition-colors duration-300">Copier le Prompt</button>
                        </div>
                        <textarea id="ia-prompt-textarea" rows="8" class="mt-3 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500" placeholder="Le prompt pour l'IA apparaîtra ici..."></textarea>
                    </div>
                    <div>
                        <h3 class="text-lg font-semibold text-gray-700">2. Lancer l'outil de création</h3>
                        <p class="text-sm text-gray-500 mb-4">Cliquez sur le bouton ci-dessous pour ouvrir l'application Canva dans un nouvel onglet. Vous pourrez y coller votre prompt pour générer vos visuels.</p>
                         <a href="https://www.canva.com/dream-lab?adj=eyJBIjoiQSIsIkIiOjF9" target="_blank" id="open-canvas-btn" class="w-full text-center block px-4 py-3 bg-green-600 text-white font-bold rounded-md hover:bg-green-700">Ouvrir l'IA de Création d'Image (Canva)</a>
                    </div>
                    <div>
                        <h3 class="text-lg font-semibold text-gray-700">3. Importer votre visuel (Optionnel)</h3>
                        <p class="text-sm text-gray-500 mb-4">Une fois votre visuel créé sur Canva, vous pouvez le télécharger puis l'importer ici pour le joindre à votre demande.</p>
                        <input type="file" id="ia-image-upload" multiple class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100">
                        <div id="ia-image-result-area" class="mt-4 grid grid-cols-2 sm:grid-cols-4 gap-4 min-h-[120px]">
                            <!-- IA image previews will be shown here -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 4: Product Selection OR Modification -->
            <div data-step-content="4">
                <!-- Content will be injected by JS based on request type -->
            </div>

            <!-- Step 5: Finalization -->
            <div data-step-content="5">
                 <h2 class="text-2xl font-bold text-gray-800 border-b pb-3 mb-6">Étape 5: Finalisation</h2>
                 <div class="space-y-6">
                    <div>
                        <h3 class="text-lg font-semibold mb-2 text-gray-700">Options de validation</h3>
                        <div class="flex items-center">
                            <input id="bat-tissu" type="checkbox" class="h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500">
                            <label for="bat-tissu" class="ml-3 text-sm text-gray-600">Envoyer un BAT tissu au club (si applicable)</label>
                        </div>
                    </div>
                    <div>
                        <h3 class="text-lg font-semibold mb-2 text-gray-700">Impératif de livraison</h3>
                        <label for="deadline-date" class="block text-sm font-medium text-gray-500 mb-2">Y a-t-il une date impérative pour la livraison ? Si oui, précisez-la.</label>
                        <input type="date" id="deadline-date" class="mt-1 block w-full md:w-1/2 rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">
                    </div>
                    <div>
                        <h3 class="text-lg font-semibold mb-2 text-gray-700">Informations sur la dernière maquette (si applicable)</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                             <div>
                                <label for="last-mockup-date" class="block text-sm font-medium text-gray-500 mb-2">Date de la maquette</label>
                                <input type="date" id="last-mockup-date" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">
                             </div>
                             <div>
                                 <label for="last-mockup-link" class="block text-sm font-medium text-gray-500 mb-2">Lien vers le PDF</label>
                                 <input type="url" id="last-mockup-link" placeholder="https://..." class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">
                             </div>
                        </div>
                    </div>
                 </div>
                 <div class="mt-8">
                      <h3 class="text-lg font-semibold mb-2 text-gray-700">Commentaires finaux</h3>
                      <label for="final-comments" class="block text-sm font-medium text-gray-500 mb-2">Ajoutez ici toute autre information pertinente pour la création de votre maquette.</label>
                      <textarea id="final-comments" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500"></textarea>
                 </div>
            </div>

            <!-- Navigation -->
            <div class="mt-8 pt-6 border-t flex justify-between items-center">
                <button id="prev-btn" class="px-6 py-2 bg-gray-600 text-white font-medium rounded-md hover:bg-gray-700 disabled:bg-gray-300 disabled:cursor-not-allowed">Précédent</button>
                <button id="next-btn" class="px-6 py-2 bg-indigo-600 text-white font-medium rounded-md hover:bg-indigo-700 disabled:bg-gray-300 disabled:cursor-not-allowed">Suivant</button>
                <button id="submit-btn" class="hidden px-8 py-3 bg-green-600 text-white font-bold rounded-md hover:bg-green-700">Envoyer la Demande</button>
            </div>
        </main>
    </div>

    <!-- Summary Modal -->
    <div id="summary-modal" class="modal fixed inset-0 bg-black bg-opacity-60 z-50 justify-center items-start pt-10 overflow-y-auto">
        <div class="bg-white rounded-lg shadow-xl p-6 sm:p-8 w-full max-w-3xl relative my-8">
            <button id="summary-modal-close-btn" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600">
                <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
            </button>
            <h3 class="text-2xl font-bold text-gray-800 mb-6 text-center">Récapitulatif de votre demande</h3>
            <div id="summary-modal-body" class="text-gray-600 max-h-[70vh] overflow-y-auto pr-4 space-y-6">
                <!-- Summary content will be injected here -->
            </div>
            <div class="mt-8 flex justify-end gap-4">
                <button id="cancel-send-btn" class="px-6 py-2 bg-gray-300 text-gray-800 font-medium rounded-md hover:bg-gray-400">Annuler</button>
                <button id="print-btn" class="px-8 py-2 bg-green-600 text-white font-bold rounded-md hover:bg-green-700 cursor-pointer">Valider et Préparer l'envoi</button>
            </div>

            <!-- PDF Generation Loader -->
            <div id="pdf-loader" class="absolute inset-0 bg-white bg-opacity-80 flex-col justify-center items-center rounded-lg hidden">
                <div class="loader"></div>
                <p class="mt-4 text-gray-600 font-semibold">Génération du PDF en cours...</p>
            </div>
        </div>
    </div>

    <!-- Reset Confirmation Modal -->
    <div id="reset-modal" class="modal fixed inset-0 bg-black bg-opacity-60 z-50 justify-center items-center">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-sm">
            <h3 class="text-lg font-bold text-gray-800 mb-4">Confirmer la réinitialisation</h3>
            <p class="text-sm text-gray-600 mb-6">Toutes les données saisies seront définitivement effacées. Voulez-vous continuer ?</p>
            <div class="flex justify-end gap-4">
                <button id="cancel-reset-btn" class="px-4 py-2 bg-gray-300 text-gray-800 font-medium rounded-md hover:bg-gray-400">Annuler</button>
                <button id="confirm-reset-btn" class="px-4 py-2 bg-red-600 text-white font-bold rounded-md hover:bg-red-700">Réinitialiser</button>
            </div>
        </div>
    </div>
    
    <!-- Local Storage Restore Notification -->
    <div id="restore-notification" class="toast-notification fixed bottom-5 right-5 bg-indigo-600 text-white py-3 px-5 rounded-lg shadow-lg flex items-center gap-3">
        <p>Progression restaurée. N'oubliez pas de réimporter vos fichiers.</p>
        <button id="restore-notification-close" class="font-bold text-lg">&times;</button>
    </div>


    <script type="module">
        // --- DATA ---
        const allAvailableProducts = [
            { name: 'MAILLOT CLASSIQUE HOMME CONFORT MC', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT CLASSIQUE FEMME CONFORT MC', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT MIXTE CONFORT SANS MANCHE', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT MIXTE PERFORMANCE MC', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT MIXTE VTT CONFORT MC', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT VTT/DESCENTE MIXTE CONFORT MC (Très ample)', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT MIXTE AERO MC', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Courtes' },
            { name: 'MAILLOT MI-SAISON HOMME CONFORT ML', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Longues' },
            { name: 'MAILLOT MI-SAISON FEMME CONFORT ML', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Longues' },
            { name: 'MAILLOT BMX MIXTE CONFORT ML (Très ample)', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Longues' },
            { name: 'MAILLOT MI-SAISON MIXTE AERO ML', category: 'CYCLISME', type: 'haut', subtype: 'Maillots Manches Longues' },
            { name: 'MAILLOT PLUIE MIXTE AERO MC (non sublimé, marquage DTF)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'MAILLOT PLUIE MIXTE AERO ML (non sublimé, marquage DTF)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'GILET COUPE-VENT LEGER MIXTE (vent et pluie fine, sans poche dos)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'GILET COUPE-VENT MI-SAISON MIXTE (dos ajouré)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'GILET COUPE-VENT HIVER MIXTE (tout membranné)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'COUPE-VENT LEGER MIXTE CONFORT (vent et pluie fine)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'COUPE-VENT LEGER DEPERLANT MIXTE CONFORT (avec membranne)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'VESTE MI-SAISON MIXTE CONFORT (membranne coupe-vent + mi-saison)', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'VESTE MI-SAISON MIXTE CONFORT avec -6cm aux ML', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'VESTE HIVER HOMME CONFORT', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'VESTE HIVER FEMME CONFORT', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'VESTE HIVER THERMIQUE HOMME CONFORT', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'VESTE HIVER THERMIQUE FEMME CONFORT', category: 'CYCLISME', type: 'haut', subtype: 'Essentiels et Vestes' },
            { name: 'CUISSARD A BRETELLES HOMME CONFORT Peau LANDSCAPE', category: 'CYCLISME', type: 'bas', subtype: 'Cuissards Courts' },
            { name: 'CUISSARD A BRETELLES FEMME CONFORT Peau LANDSCAPE', category: 'CYCLISME', type: 'bas', subtype: 'Cuissards Courts' },
            { name: 'CUISSARD FEMME SANS BRETELLES CONFORT Peau LANDSCAPE', category: 'CYCLISME', type: 'bas', subtype: 'Cuissards Courts' },
            { name: 'CUISSARD HOMME AERO Peau CERVINO', category: 'CYCLISME', type: 'bas', subtype: 'Cuissards Courts' },
            { name: 'CUISSARD FEMME AERO Peau CERVINO', category: 'CYCLISME', type: 'bas', subtype: 'Cuissards Courts' },
            { name: 'SHORT VTT FOND Peau ENDURANCE 2.5', category: 'CYCLISME', type: 'bas', subtype: 'Cuissards Courts' },
            { name: 'CORSAIRE HOMME A BRETELLES CONFORT', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'CORSAIRE FEMME SANS BRETELLES CONFORT', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COLLANT HIVER A BRETELLES HOMME CONFORT', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COLLANT HIVER A BRETELLES FEMME CONFORT', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COLLANT HIVER FEMME SANS BRETELLES CONFORT', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COLLANT HIVER HOMME AERO Peau CERVINO', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COLLANT HIVER FEMME AERO Peau CERVINO', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COLLANT MIXTE ECHAUFFEMENT', category: 'CYCLISME', type: 'bas', subtype: 'Corsaires/Collants' },
            { name: 'COMBINAISON ROUTE MANCHES COURTES HOMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON ROUTE MANCHES COURTES FEMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO ROUTE MANCHES COURTES HOMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO ROUTE MANCHES COURTES FEMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO ROUTE MANCHES LONGUES HOMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO ROUTE MANCHES LONGUES FEMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO PISTE MANCHES COURTES HOMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO PISTE MANCHES COURTES FEMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO PISTE MANCHES LONGUES HOMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CHRONO PISTE MANCHES LONGUES FEMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CYCLO-CROSS MANCHES LONGUES HOMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'COMBINAISON CYCLO-CROSSMANCHES LONGUES FEMME AERO', category: 'CYCLISME', type: 'haut', subtype: 'Combinaisons' },
            { name: 'MAILLOT RUNNING HOMME', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'MAILLOT RUNNING FEMME', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'MAILLOT TRAIL HOMME MANCHES COURTES', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'MAILLOT TRAIL FEMME MANCHES COURTES', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'DEBARDEUR ATHLETISME HOMME', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'DEBARDEUR ATHLETISME FEMME', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'BRASSIERE RUNNING FEMME', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'MAILLOT RUNNING HIVER HOMME MANCHES LONGUES', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'MAILLOT RUNNING HIVER FEMME MANCHES LONGUES', category: 'RUNNING', type: 'haut', subtype: 'Hauts Running' },
            { name: 'GILET COUPE-VENT LEGER MIXTE', category: 'RUNNING', type: 'haut', subtype: 'Vestes Running' },
            { name: 'GILET COUPE-VENT MI-SAISON MIXTE', category: 'RUNNING', type: 'haut', subtype: 'Vestes Running' },
            { name: 'GILET COUPE-VENT HIVER MIXTE', category: 'RUNNING', type: 'haut', subtype: 'Vestes Running' },
            { name: 'COUPE-VENT LEGER MIXTE CONFORT', category: 'RUNNING', type: 'haut', subtype: 'Vestes Running' },
            { name: 'VESTE MI-SAISON HOMME CONFORT', category: 'RUNNING', type: 'haut', subtype: 'Vestes Running' },
            { name: 'VESTE MI-SAISON FEMME CONFORT', category: 'RUNNING', type: 'haut', subtype: 'Vestes Running' },
            { name: 'SHORT RUNNING MIXTE', category: 'RUNNING', type: 'bas', subtype: 'Bas Running' },
            { name: 'SHORTY FEMME RUNNING', category: 'RUNNING', type: 'bas', subtype: 'Bas Running' },
            { name: 'CUISSARD RUNNING HOMME', category: 'RUNNING', type: 'bas', subtype: 'Bas Running' },
            { name: 'COLLANT RUNNING MIXTE', category: 'RUNNING', type: 'bas', subtype: 'Bas Running' },
            { name: 'TRIFONCTION HOMME COURTE DISTANCE Peau TRI GEL', category: 'RUNNING', type: 'haut', subtype: 'Trifonctions' },
            { name: 'TRIFONCTION FEMME COURTE DISTANCE Peau TRI GEL', category: 'RUNNING', type: 'haut', subtype: 'Trifonctions' },
            { name: 'TRIFONCTION HOMME HALF Peau TRI GEL, ZIP DEVANT OU DOS', category: 'RUNNING', type: 'haut', subtype: 'Trifonctions' },
            { name: 'TRIFONCTION FEMME HALF Peau TRI GEL, ZIP DEVANT OU DOS', category: 'RUNNING', type: 'haut', subtype: 'Trifonctions' },
            { name: 'BANDANA ÉTÉ', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'BANDEAU', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'TOUR DE COU', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'PASSE MONTAGNE', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'MANCHETTES HIVER VELO/RUNNING', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'JAMBIERES', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'GANTS ÉTÉ', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'TAPIS DE TRANSITION MULTISPORTS', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'CHAUSSETTES AERO MIXTE 18cm', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'CHAUSSETTES VELO/COURSE A PIED Mixte Tige 13 ou 17cm', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'GAPETTE VELO', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERSONNALISÉS' },
            { name: 'SOUS-MAILLOT SANS MANCHES', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERMANENTS' },
            { name: 'SOUS-MAILLOT MI-SAISON MANCHES COURTES', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERMANENTS' },
            { name: 'SOUS-MAILLOT HIVER MANCHES LONGUES', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERMANENTS' },
            { name: 'SOUS CASQUE', category: 'ACCESSOIRES', type: 'accessoire', subtype: 'ACCESSOIRES PERMANENTS' },
            { name: 'MAILLOT ENFANT PERFORMANCE MC', category: 'ENFANTS', type: 'enfant', subtype: 'Cyclisme Enfant' },
            { name: 'MAILLOT VTT/DESCENTE ENFANT CONFORT MC', category: 'ENFANTS', type: 'enfant', subtype: 'Cyclisme Enfant' },
            { name: 'MAILLOT RUNNING ENFANT MANCHES COURTES', category: 'ENFANTS', type: 'enfant', subtype: 'Running Enfants' },
            { name: 'DEBARDEUR ATHLETISME ENFANT', category: 'ENFANTS', type: 'enfant', subtype: 'Running Enfants' },
            { name: 'CUISSARD RUNNING ENFANT', category: 'ENFANTS', type: 'enfant', subtype: 'Running Enfants' },
            { name: 'TRIFONCTION ENFANT COURTE DISTANCE', category: 'ENFANTS', type: 'enfant', subtype: 'Running Enfants' },
        ];
        
        const colorPalette = {
            "Gris / Noir": { "Noir": "#000000", "Gris Anthracite": "#374151", "Gris Moyen": "#6B7280", "Gris Clair": "#D1D5DB", "Argent Métallique": "#C0C0C0", "Blanc": "#FFFFFF" },
            "Rouges & Roses": { "Rouge Vif": "#EF4444", "Rouge Foncé": "#B91C1C", "Bordeaux": "#991B1B", "Corail": "#F87171", "Rose Vif": "#EC4899", "Framboise": "#E30B5D", "Brique": "#B22222", "Vermillon": "#FF4500", "Rose Pâle": "#FFE4E1", "Fuchsia": "#E879F9", "Rose Poudré": "#FBCFE8" },
            "Bleus": { "Bleu Roi": "#3B82F6", "Bleu Marine": "#1E3A8A", "Bleu Ciel": "#60A5FA", "Turquoise": "#2DD4BF", "Cyan": "#06B6D4", "Bleu Klein": "#002FA7", "Aigue-marine": "#7FFFD4", "Bleu Nuit": "#000080", "Bleu Canard": "#005F69", "Bleu Azur": "#007FFF", "Bleu Pétrole": "#1D4E89" },
            "Verts": { "Vert Émeraude": "#10B981", "Vert Forêt": "#166534", "Vert Citron": "#A3E635", "Vert Kaki": "#65A30D", "Vert d'Eau": "#A7F3D0", "Vert Olive": "#808000", "Vert Menthe": "#98FF98", "Vert Sapin": "#01796F", "Vert Tilleul": "#B0E05C", "Vert Amande": "#90EE90" },
            "Jaunes & Oranges": { "Jaune Vif": "#EAB308", "Jaune Pâle": "#FDE047", "Or Métallique": "#FFD700", "Orange": "#F97316", "Orange Sanguine": "#EA580C", "Or": "#D97706", "Ambre": "#FFBF00", "Safran": "#F4C430", "Mandarine": "#F28500", "Jaune Moutarde": "#FFDB58", "Abricot": "#FBCEB1", "Ocre": "#CC7722", "Corail": "#FF7F50" },
            "Violets & Mauves": { "Violet": "#8B5CF6", "Lavande": "#C4B5FD", "Magenta": "#D946EF", "Indigo": "#4B0082", "Lilas": "#C8A2C8", "Parme": "#CFA0E9", "Mauve": "#E0B0FF", "Prune": "#8E4585", "Aubergine": "#472C4C" },
            "Marrons & Terres": { "Chocolat": "#7B3F00", "Marron Glacé": "#8D6E63", "Beige": "#F5F5DC", "Sable": "#C2B280", "Taupe": "#483C32", "Terre de Sienne": "#E97451" },
            "Couleurs Fluo": { "Jaune Fluo": "#CCFF00", "Vert Fluo": "#39FF14", "Rose Fluo": "#FF007F", "Orange Fluo": "#FF5F00", "Bleu Fluo": "#00FFFF" }
        };

        const designStyles = [
            "Grunge", "Dégradé", "Trame (Halftone)", "Aquarelle", "Galaxie", "Radial", "Lumineux (Glow)",
            "Géométrique", "Minimaliste", "Organique / Nature", "Vintage / Rétro", "Lignes / Rayures", "Typographique", "Urbain / Street",
            "Art Déco", "Psychédélique", "Tribal", "Futuriste", "Technologique", "Asymétrique", "Abstrait", "Art Nouveau", "Pixel Art", "Vaporwave", "Coup de pinceau", "Marbré",
            "Camouflage", "Animalier", "Pop Art", "Bauhaus", "Topographique", "Mosaïque", "Fibre de Carbone", "Ondes Sonores", "Op Art", "Memphis", "Néon", "Holographique", "Métallique", "Esquisse / Croquis"
        ];

        // --- APP STATE ---
        const state = {
            currentStep: 1,
            clubInfo: {
                name: '',
                clientNumber: '',
                date: '',
                sport: 'cyclisme_competition',
                requestType: 'creation' // 'creation' or 'modification' or 'fournie'
            },
            selectedArticles: [],
            design: {
                colors: [],
                styles: [],
                inspirationImages: [], // For inspiration uploads
                notes: '',
                background: 'sur un mannequin, fond de studio neutre',
            },
            modification: {
                existingVisuals: [],
                instructions: {
                    hauts: '',
                    bas: '',
                    combinaisons: '',
                    accessoires: ''
                },
                supportingFiles: []
            },
            production: {
                productionFiles: [],
                productionNotes: ''
            },
            iaGenerator: {
                prompt: '',
                uploadedImages: [],
            },
            logos: [], // { file: File, instructions: { hauts: '', bas: ''} }
            finalComments: '',
            finalization: {
                batTissu: false,
                deadline: '',
                lastMockupDate: '',
                lastMockupLink: ''
            }
        };

        // --- DOM ELEMENTS ---
        const dom = {
            stepper: document.getElementById('stepper'),
            steps: document.querySelectorAll('.step'),
            stepContents: document.querySelectorAll('[data-step-content]'),
            prevBtn: document.getElementById('prev-btn'),
            nextBtn: document.getElementById('next-btn'),
            submitBtn: document.getElementById('submit-btn'),
            productSelectionContainer: document.getElementById('product-selection-container'),
            colorPickerContainer: document.getElementById('color-picker'),
            styleSelectionContainer: document.getElementById('style-selection-container'),
            inspirationUpload: document.getElementById('inspiration-upload'),
            inspirationPreviewArea: document.getElementById('inspiration-preview-area'),
            logoUpload: document.getElementById('logo-upload'),
            logoPreviewArea: document.getElementById('logo-preview-area'),
            logoInstructionsArea: document.getElementById('logo-instructions-area'),
            summaryModal: document.getElementById('summary-modal'),
            summaryModalBody: document.getElementById('summary-modal-body'),
            summaryModalCloseBtn: document.getElementById('summary-modal-close-btn'),
            pdfLoader: document.getElementById('pdf-loader'),
            finalCloseBtn: document.getElementById('final-close-btn'),
            cancelSendBtn: document.getElementById('cancel-send-btn'),
            // Step 2
            iaBackgroundInput: document.getElementById('ia-background-input'),
            // Step 3 - AI Generator
            generatePromptBtn: document.getElementById('generate-prompt-btn'),
            iaPromptTextarea: document.getElementById('ia-prompt-textarea'),
            copyPromptBtn: document.getElementById('copy-prompt-btn'),
            iaImageUpload: document.getElementById('ia-image-upload'),
            iaImageResultArea: document.getElementById('ia-image-result-area'),
            // Notifications
            restoreNotification: document.getElementById('restore-notification'),
            restoreNotificationClose: document.getElementById('restore-notification-close'),
            // Reset Modal
            resetModal: document.getElementById('reset-modal'),
            confirmResetBtn: document.getElementById('confirm-reset-btn'),
            cancelResetBtn: document.getElementById('cancel-reset-btn'),
        };

        // --- RENDER FUNCTIONS ---

        function renderStep() {
            const isCreation = state.clubInfo.requestType === 'creation';
            const finalStep = 5;

            // Show/Hide steps based on request type
            document.querySelector('[data-step="2"]').style.display = isCreation ? 'flex' : 'none';
            document.querySelector('[data-step="3"]').style.display = isCreation ? 'flex' : 'none';
            document.querySelectorAll('.step-line')[0].style.display = isCreation ? 'block' : 'none';
            document.querySelectorAll('.step-line')[1].style.display = isCreation ? 'block' : 'none';
            
            // Update Step 4 Label
            const step4Label = document.querySelector('[data-step-label="4"]');
            if (state.clubInfo.requestType === 'creation') {
                 step4Label.textContent = "Choix Articles";
            } else if (state.clubInfo.requestType === 'modification') {
                 step4Label.textContent = "Modifications";
            } else {
                 step4Label.textContent = "Fichiers Production";
            }


            dom.steps.forEach(step => {
                const stepNum = parseInt(step.dataset.step);
                step.classList.remove('step-active', 'step-completed');
                step.querySelector('div:first-child').classList.remove('bg-indigo-600', 'text-white', 'bg-green-600');
                step.querySelector('div:first-child').classList.add('bg-gray-200');

                if (stepNum < state.currentStep) {
                    step.classList.add('step-completed');
                     step.querySelector('div:first-child').classList.add('bg-green-600', 'text-white');
                } else if (stepNum === state.currentStep) {
                    step.classList.add('step-active');
                     step.querySelector('div:first-child').classList.add('bg-indigo-600', 'text-white');
                }
            });
            
             document.querySelectorAll('.step-line').forEach((line, index) => {
                 line.classList.remove('step-line-completed');
                 let adjustedCurrentStep = state.currentStep;
                 // This logic simplifies the line completion when skipping steps for non-creation flows
                 if (state.clubInfo.requestType !== 'creation' && index > 0) {
                     if (state.currentStep >= 4) { // After step 1, we are on step 4 or 5
                        line.classList.add('step-line-completed');
                     }
                 } else if (index < state.currentStep -1) {
                     line.classList.add('step-line-completed');
                 }
             });

            dom.stepContents.forEach(content => {
                content.classList.remove('active');
                if (parseInt(content.dataset.stepContent) === state.currentStep) {
                    content.classList.add('active');
                }
            });
            
            renderStep4Content();

            dom.prevBtn.disabled = state.currentStep === 1;
            dom.nextBtn.style.display = state.currentStep === finalStep ? 'none' : 'block';
            dom.submitBtn.style.display = state.currentStep === finalStep ? 'block' : 'none';
        }

        function renderProductSelection() {
            const groupedProducts = allAvailableProducts.reduce((acc, p) => {
                const key = `${p.category} - ${p.subtype}`;
                if (!acc[key]) acc[key] = [];
                acc[key].push(p);
                return acc;
            }, {});

            let html = `
                <h2 class="text-2xl font-bold text-gray-800 border-b pb-3 mb-6">Étape 4: Articles souhaités pour la maquette</h2>
                <p class="text-sm text-gray-600 mb-4">Cochez tous les articles pour lesquels vous désirez une proposition de design.</p>
                <div class="space-y-4 max-h-[60vh] overflow-y-auto pr-4">
            `;
            for (const group in groupedProducts) {
                html += `
                    <details class="bg-gray-50 rounded-lg p-3" ${group.startsWith('CYCLISME - Maillots') ? 'open' : ''}>
                        <summary class="font-semibold cursor-pointer text-gray-700">${group}</summary>
                        <div class="mt-2 pl-4 space-y-2">
                `;
                groupedProducts[group].forEach(p => {
                    const isChecked = state.selectedArticles.includes(p.name);
                    html += `
                        <div class="flex items-center">
                            <input id="prod-${p.name}" type="checkbox" value="${p.name}" class="product-checkbox h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500" ${isChecked ? 'checked' : ''}>
                            <label for="prod-${p.name}" class="ml-3 text-sm text-gray-600">${p.name}</label>
                        </div>
                    `;
                });
                html += `</div></details>`;
            }
            html += `</div>`;
            return html;
        }

        function renderModificationForm() {
            return `
                <h2 class="text-2xl font-bold text-gray-800 border-b pb-3 mb-6">Étape 4: Modification d'une maquette existante</h2>
                <div class="space-y-6">
                     <div>
                        <h3 class="text-lg font-semibold text-gray-700">Visuel Existant</h3>
                        <p class="text-sm text-gray-500 mb-4">Importez le ou les visuels de la maquette à modifier (PDF, PNG, JPG...).</p>
                        <input type="file" id="existing-visual-upload" multiple class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100">
                        <div id="existing-visual-preview" class="mt-4 grid grid-cols-2 sm:grid-cols-4 gap-4">
                             <div class="col-span-full text-center text-gray-400 p-4 border-2 border-dashed rounded-lg">Aucun visuel chargé</div>
                        </div>
                    </div>
                    <div>
                        <h3 class="text-lg font-semibold text-gray-700">Instructions de modification</h3>
                        <div class="space-y-4 mt-2">
                             <div>
                                <label for="mod-hauts" class="block text-sm font-medium text-gray-700">LES HAUTS</label>
                                <textarea id="mod-hauts" data-mod-type="hauts" class="mod-instruction mt-1 block w-full rounded-md border-gray-300 shadow-sm" rows="3">${state.modification.instructions.hauts}</textarea>
                            </div>
                             <div>
                                <label for="mod-bas" class="block text-sm font-medium text-gray-700">LES BAS</label>
                                <textarea id="mod-bas" data-mod-type="bas" class="mod-instruction mt-1 block w-full rounded-md border-gray-300 shadow-sm" rows="3">${state.modification.instructions.bas}</textarea>
                            </div>
                             <div>
                                <label for="mod-combis" class="block text-sm font-medium text-gray-700">LES COMBINAISONS</label>
                                <textarea id="mod-combis" data-mod-type="combinaisons" class="mod-instruction mt-1 block w-full rounded-md border-gray-300 shadow-sm" rows="3">${state.modification.instructions.combinaisons}</textarea>
                            </div>
                            <div>
                                <label for="mod-accessoires" class="block text-sm font-medium text-gray-700">LES ACCESSOIRES</label>
                                <textarea id="mod-accessoires" data-mod-type="accessoires" class="mod-instruction mt-1 block w-full rounded-md border-gray-300 shadow-sm" rows="3">${state.modification.instructions.accessoires}</textarea>
                            </div>
                        </div>
                    </div>
                     <div>
                        <h3 class="text-lg font-semibold text-gray-700">Fichiers d'illustration</h3>
                        <p class="text-sm text-gray-500 mb-4">Pour illustrer vos demandes (croquis, exemples...), ajoutez vos fichiers ici.</p>
                        <input type="file" id="supporting-files-upload" multiple class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100">
                         <div id="supporting-files-preview-area" class="mt-4 grid grid-cols-2 sm:grid-cols-4 gap-4"></div>
                    </div>
                </div>
            `;
        }
        
        function renderProductionForm() {
             return `
                <h2 class="text-2xl font-bold text-gray-800 border-b pb-3 mb-6">Étape 4: Maquette Fournie</h2>
                <div class="space-y-6">
                     <div>
                        <h3 class="text-lg font-semibold text-gray-700">Fichiers de Production</h3>
                        <p class="text-sm text-gray-500 mb-4">Importez tous les fichiers de production fournis par le client (PDF, AI, EPS, etc.).</p>
                        <input type="file" id="production-files-upload" multiple class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100">
                        <div id="production-files-preview-area" class="mt-4 grid grid-cols-2 sm:grid-cols-4 gap-4"></div>
                    </div>
                    <div>
                        <h3 class="text-lg font-semibold text-gray-700">Instructions de production</h3>
                        <label for="production-notes" class="block text-sm font-medium text-gray-500 mb-2">Ajoutez ici toute information pertinente pour la production (références Pantone, détails spécifiques, etc.).</label>
                        <textarea id="production-notes" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">${state.production.productionNotes}</textarea>
                    </div>
                </div>
            `;
        }

        function renderStep4Content() {
            const step4Content = document.querySelector('[data-step-content="4"]');
            if (state.clubInfo.requestType === 'creation') {
                step4Content.innerHTML = renderProductSelection();
            } else if (state.clubInfo.requestType === 'modification') {
                step4Content.innerHTML = renderModificationForm();
            } else { // 'fournie'
                step4Content.innerHTML = renderProductionForm();
            }
        }
        
        function renderColorPicker() {
            let html = '';
            for (const familyName in colorPalette) {
                const colors = colorPalette[familyName];
                const firstColorHex = Object.values(colors)[0];

                html += `
                <details class="bg-gray-50 rounded-lg">
                    <summary class="p-3 font-semibold cursor-pointer text-gray-700 flex items-center gap-3">
                        <div class="w-6 h-6 rounded-full border" style="background-color: ${firstColorHex};"></div>
                        ${familyName}
                    </summary>
                    <div class="p-4 border-t border-gray-200">
                        <div class="flex flex-wrap gap-2">
                            ${Object.entries(colors).map(([name, hex]) => `
                                <div class="color-swatch w-8 h-8 rounded-full border-2 border-white" 
                                     style="background-color: ${hex};" 
                                     title="${name}"
                                     data-color-name="${name}">
                                </div>
                            `).join('')}
                        </div>
                    </div>
                </details>`;
            }
            dom.colorPickerContainer.innerHTML = html;
        }

        function renderStyleSelection() {
            dom.styleSelectionContainer.innerHTML = designStyles.map(style => {
                const styleId = `style-${style.toLowerCase().replace(/[^a-z0-9]/g, '-')}`;
                return `
                <label for="${styleId}" class="style-label cursor-pointer">
                    <input id="${styleId}" type="checkbox" value="${style}" class="style-checkbox sr-only">
                    <span class="flex items-center justify-center p-3 border-2 rounded-lg text-center text-sm font-medium text-gray-700 hover:bg-indigo-50 transition-colors">
                        ${style}
                    </span>
                </label>
            `;
            }).join('');
        }

        function updateColorSelectionUI() {
            document.querySelectorAll('.color-swatch').forEach(swatch => {
                if (state.design.colors.includes(swatch.dataset.colorName)) {
                    swatch.classList.add('selected');
                } else {
                    swatch.classList.remove('selected');
                }
            });
        }
        
        function renderLogoPreviews() {
            dom.logoPreviewArea.innerHTML = '';
            dom.logoInstructionsArea.innerHTML = '';
            
            if (state.logos.length === 0) {
                 dom.logoInstructionsArea.innerHTML = `<p class="text-center text-gray-400 p-4 border-2 border-dashed rounded-lg">Importez des logos pour ajouter des instructions.</p>`;
                 return;
            }

            state.logos.forEach((logo, index) => {
                const file = logo.file;
                const previewWrapper = document.createElement('div');
                previewWrapper.className = 'relative group';
                
                const previewContentDiv = document.createElement('div');
                previewContentDiv.className = 'preview-content';
                
                if (file.type === 'application/pdf') {
                    const reader = new FileReader();
                    reader.onload = e => {
                        previewContentDiv.innerHTML = `<embed src="${e.target.result}" type="application/pdf" class="w-full h-24 border rounded-md">`;
                    };
                    reader.readAsDataURL(file);
                } else if (file.type.startsWith('image/')) {
                    const reader = new FileReader();
                    reader.onload = e => {
                        previewContentDiv.innerHTML = `<img src="${e.target.result}" alt="${file.name}" class="w-full h-24 object-contain border rounded-md p-1">`;
                    };
                    reader.readAsDataURL(file);
                } else {
                    previewContentDiv.innerHTML = `<div class="w-full h-24 border rounded-md p-2 flex flex-col items-center justify-center bg-gray-100 text-center">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                            <path fill-rule="evenodd" d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4zm2 6a1 1 0 011-1h6a1 1 0 110 2H7a1 1 0 01-1-1zm1 3a1 1 0 100 2h6a1 1 0 100-2H7z" clip-rule="evenodd" />
                        </svg>
                        <span class="text-xs text-gray-500 mt-1 truncate w-full">${file.name}</span>
                       </div>`;
                }

                previewWrapper.innerHTML = `
                    <button data-logo-index="${index}" class="remove-logo-btn absolute top-0 right-0 bg-red-500 text-white rounded-full w-5 h-5 flex items-center justify-center text-xs opacity-0 group-hover:opacity-100 transition-opacity">&times;</button>
                    <p class="text-xs text-center truncate" title="${file.name}">${file.name}</p>
                `;
                previewWrapper.prepend(previewContentDiv);
                dom.logoPreviewArea.appendChild(previewWrapper);


                const instructionDiv = document.createElement('div');
                instructionDiv.innerHTML = `
                    <label class="block text-sm font-medium text-gray-700">${file.name}</label>
                    <div class="mt-1 space-y-2">
                        <input type="text" data-logo-index="${index}" data-placement="hauts" value="${logo.instructions.hauts}" class="logo-instruction-input block w-full rounded-md border-gray-300 shadow-sm text-sm" placeholder="Placement sur HAUTS...">
                        <input type="text" data-logo-index="${index}" data-placement="bas" value="${logo.instructions.bas}" class="logo-instruction-input block w-full rounded-md border-gray-300 shadow-sm text-sm" placeholder="Placement sur BAS...">
                    </div>
                `;
                dom.logoInstructionsArea.appendChild(instructionDiv);
            });
        }
        
        function renderInspirationPreviews() {
            dom.inspirationPreviewArea.innerHTML = '';
            
            if (state.design.inspirationImages.length === 0) return;

            state.design.inspirationImages.forEach((img, index) => {
                const reader = new FileReader();
                reader.onload = e => {
                    const previewWrapper = document.createElement('div');
                    previewWrapper.className = 'relative group';
                    previewWrapper.innerHTML = `
                        <img src="${e.target.result}" alt="${img.file.name}" class="w-full h-24 object-contain border rounded-md p-1">
                        <button data-inspiration-index="${index}" class="remove-inspiration-btn absolute top-0 right-0 bg-red-500 text-white rounded-full w-5 h-5 flex items-center justify-center text-xs opacity-0 group-hover:opacity-100 transition-opacity">&times;</button>
                        <p class="text-xs text-center truncate" title="${img.file.name}">${img.file.name}</p>
                    `;
                    dom.inspirationPreviewArea.appendChild(previewWrapper);
                };
                reader.readAsDataURL(img.file);
            });
        }

        function renderIaImagePreviews() {
            dom.iaImageResultArea.innerHTML = '';
            if (state.iaGenerator.uploadedImages.length === 0) {
                dom.iaImageResultArea.innerHTML = `<div class="col-span-full text-center text-gray-400 p-4 border-2 border-dashed rounded-lg flex items-center justify-center min-h-[120px]">L'aperçu de vos images apparaîtra ici.</div>`;
                return;
            }

            state.iaGenerator.uploadedImages.forEach((file, index) => {
                const reader = new FileReader();
                reader.onload = e => {
                    const previewWrapper = document.createElement('div');
                    previewWrapper.className = 'relative group';
                    previewWrapper.innerHTML = `
                        <img src="${e.target.result}" alt="${file.name}" class="w-full h-24 object-contain border rounded-md p-1">
                        <button data-ia-image-index="${index}" class="remove-ia-image-btn absolute top-0 right-0 bg-red-500 text-white rounded-full w-5 h-5 flex items-center justify-center text-xs opacity-0 group-hover:opacity-100 transition-opacity">&times;</button>
                        <p class="text-xs text-center truncate" title="${file.name}">${file.name}</p>
                    `;
                    dom.iaImageResultArea.appendChild(previewWrapper);
                };
                reader.readAsDataURL(file);
            });
        }

        function renderFilePreviews(filesArray, containerElement, arrayName) {
            containerElement.innerHTML = '';
            if (!filesArray || filesArray.length === 0) {
                 if (containerElement.id === 'existing-visual-preview') {
                     containerElement.innerHTML = `<div class="col-span-full text-center text-gray-400 p-4 border-2 border-dashed rounded-lg">Aucun visuel chargé</div>`;
                 }
                return;
            }

            filesArray.forEach((file, index) => {
                const previewWrapper = document.createElement('div');
                previewWrapper.className = 'relative group';
                
                const previewContentDiv = document.createElement('div');
                previewContentDiv.className = 'preview-content';
                
                if (file.type === 'application/pdf') {
                    const reader = new FileReader();
                    reader.onload = e => {
                        previewContentDiv.innerHTML = `<embed src="${e.target.result}" type="application/pdf" class="w-full h-24 border rounded-md">`;
                    };
                    reader.readAsDataURL(file);
                }
                else if (file.type.startsWith('image/')) {
                    const reader = new FileReader();
                    reader.onload = e => {
                        previewContentDiv.innerHTML = `<img src="${e.target.result}" alt="${file.name}" class="w-full h-24 object-contain border rounded-md p-1">`;
                    };
                    reader.readAsDataURL(file);
                } else {
                    previewContentDiv.innerHTML = `<div class="w-full h-24 border rounded-md p-2 flex flex-col items-center justify-center bg-gray-100 text-center">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                            <path fill-rule="evenodd" d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4zm2 6a1 1 0 011-1h6a1 1 0 110 2H7a1 1 0 01-1-1zm1 3a1 1 0 100 2h6a1 1 0 100-2H7z" clip-rule="evenodd" />
                        </svg>
                        <span class="text-xs text-gray-500 mt-1 truncate w-full">${file.name}</span>
                       </div>`;
                }

                previewWrapper.innerHTML = `
                       <p class="text-xs text-center truncate" title="${file.name}">${file.name}</p>
                    <button data-array-name="${arrayName}" data-file-index="${index}" class="remove-file-btn absolute top-0 right-0 -mt-2 -mr-2 bg-red-500 text-white rounded-full w-5 h-5 flex items-center justify-center text-xs opacity-0 group-hover:opacity-100 transition-opacity">&times;</button>
                `;
                previewWrapper.prepend(previewContentDiv);
                containerElement.appendChild(previewWrapper);
            });
        }

        // --- EVENT HANDLERS ---

        function handleNavClick(direction) {
            collectStepData();
            if (direction === 'next' && !validateStep()) return;
            
            let nextStep = state.currentStep + (direction === 'next' ? 1 : -1);

            if (state.clubInfo.requestType !== 'creation') {
                 if (direction === 'next' && state.currentStep === 1) nextStep = 4;
                 if (direction === 'prev' && state.currentStep === 4) nextStep = 1;
            }
            
            state.currentStep = nextStep;

            if(state.currentStep < 1) state.currentStep = 1;
            if(state.currentStep > 5) state.currentStep = 5;
            
            saveStateToLocalStorage();
            window.scrollTo(0, 0);
            renderStep();
        }
        
        function validateStep() {
            let isValid = true;
            // Clear previous errors
            document.getElementById('clubName-error').textContent = '';
            document.getElementById('clientNumber-error').textContent = '';
            document.getElementById('clubName').classList.remove('border-red-500');
            document.getElementById('clientNumber').classList.remove('border-red-500');

            if (state.currentStep === 1) {
                const name = document.getElementById('clubName').value.trim();
                const clientNumber = document.getElementById('clientNumber').value.trim();
                if (!name) {
                    document.getElementById('clubName-error').textContent = 'Ce champ est requis.';
                    document.getElementById('clubName').classList.add('border-red-500');
                    isValid = false;
                }
                if (!clientNumber) {
                    document.getElementById('clientNumber-error').textContent = 'Ce champ est requis.';
                    document.getElementById('clientNumber').classList.add('border-red-500');
                    isValid = false;
                }
            }
            return isValid;
        }

        function collectStepData() {
            if (state.currentStep === 1) {
                state.clubInfo.name = document.getElementById('clubName').value.trim();
                state.clubInfo.clientNumber = document.getElementById('clientNumber').value.trim();
                state.clubInfo.date = document.getElementById('requestDate').value;
                state.clubInfo.sport = document.querySelector('input[name="sport"]:checked').value;
                state.clubInfo.requestType = document.querySelector('input[name="request-type"]:checked').value;
            } else if (state.currentStep === 2) {
                 state.design.notes = document.getElementById('design-notes').value.trim();
                 state.design.styles = Array.from(document.querySelectorAll('.style-checkbox:checked')).map(cb => cb.value);
                 state.design.background = dom.iaBackgroundInput.value;
            } else if (state.currentStep === 3) {
                state.iaGenerator.prompt = dom.iaPromptTextarea.value;
            } else if (state.currentStep === 4) {
                if(state.clubInfo.requestType === 'creation') {
                    state.selectedArticles = Array.from(document.querySelectorAll('.product-checkbox:checked')).map(cb => cb.value);
                } else if (state.clubInfo.requestType === 'modification') {
                    document.querySelectorAll('.mod-instruction').forEach(textarea => {
                        state.modification.instructions[textarea.dataset.modType] = textarea.value;
                    });
                } else { // 'fournie'
                    state.production.productionNotes = document.getElementById('production-notes').value;
                }
            } else if (state.currentStep === 5) {
                 state.finalComments = document.getElementById('final-comments').value.trim();
                 state.finalization.batTissu = document.getElementById('bat-tissu').checked;
                 state.finalization.deadline = document.getElementById('deadline-date').value;
                 state.finalization.lastMockupDate = document.getElementById('last-mockup-date').value;
                 state.finalization.lastMockupLink = document.getElementById('last-mockup-link').value.trim();
            }
            saveStateToLocalStorage();
        }

        function handleColorClick(e) {
            if (!e.target.classList.contains('color-swatch')) return;
            const { colorName } = e.target.dataset;
            const index = state.design.colors.indexOf(colorName);

            if (index > -1) {
                state.design.colors.splice(index, 1);
            } else {
                state.design.colors.push(colorName);
            }
            updateColorSelectionUI();
            saveStateToLocalStorage();
        }
        
        function handleLogoUpload(e) {
            const newFiles = Array.from(e.target.files);
            newFiles.forEach(file => {
                if (!state.logos.some(logo => logo.file.name === file.name)) {
                    state.logos.push({ file: file, instructions: { hauts: '', bas: '' } });
                }
            });
            renderLogoPreviews();
        }

        function handleInspirationUpload(e) {
            const newFiles = Array.from(e.target.files);
            newFiles.forEach(file => {
                if (!state.design.inspirationImages.some(img => img.file.name === file.name)) {
                    state.design.inspirationImages.push({ file: file });
                }
            });
            renderInspirationPreviews();
        }

        function handleGeneratePrompt() {
             const styleDescriptions = {
                "Grunge": "Intégrer des textures usées, des éclaboussures de peinture, un aspect volontairement vieilli, des typographies brutes, et un rendu chaotique et superposé.",
                "Dégradé": "Utiliser des transitions de couleurs fluides, harmonieuses et parfaitement fondues, que ce soit de manière subtile ou audacieuse.",
                "Trame (Halftone)": "Appliquer un motif de points visible de type bande dessinée ou journal pour donner un aspect rétro et graphique.",
                "Aquarelle": "Simuler un effet de peinture à l'eau réaliste avec des couleurs diluées, des taches, des coulures et des superpositions transparentes.",
                "Galaxie": "Créer un design cosmique immersif avec des nébuleuses, des constellations, des étoiles scintillantes et des couleurs profondes comme le bleu nuit et le violet.",
                "Radial": "Le design doit impérativement émaner d'un point central, créant des motifs et des lignes qui rayonnent de manière explosive ou douce vers l'extérieur.",
                "Lumineux (Glow)": "Ajouter des effets de lumière et de néon prononcés pour faire ressortir et briller certains éléments clés du design.",
                "Géométrique": "Construire le design à partir de formes claires et audacieuses, de motifs répétitifs et de lignes nettes pour un look structuré et moderne.",
                "Minimaliste": "Utiliser des lignes extrêmement épurées, un espace négatif important, très peu de couleurs (deux ou trois maximum) pour un look sobre, chic et élégant.",
                "Organique / Nature": "S'inspirer de formes fluides et non-linéaires trouvées dans la nature : nervures de feuilles, vagues, motifs cellulaires, pour un rendu harmonieux.",
                "Vintage / Rétro": "S'inspirer fidèlement des designs des années 70, 80 ou 90, avec des palettes de couleurs et des polices de caractères typiques de l'époque choisie.",
                "Lignes / Rayures": "Jouer avec des lignes de différentes épaisseurs, orientations et couleurs pour structurer le design et créer du rythme et du mouvement.",
                "Typographique": "Le texte et les lettres deviennent l'élément visuel principal du design, avec un travail artistique sur la police, la taille, la déformation et la disposition.",
                "Urbain / Street": "Incorporer des éléments de la culture urbaine : graffitis, pochoirs, textures de béton ou de brique, et un style brut et énergique.",
                "Art Déco": "Utiliser des formes géométriques symétriques, des dorures, des noirs profonds et des lignes luxueuses inspirées de l'architecture des années 1920.",
                "Psychédélique": "Créer des motifs hypnotiques et immersifs avec des couleurs très vives, des formes ondulantes, des fractales et des illusions d'optique.",
                "Tribal": "Intégrer des motifs ethniques audacieux, des symboles traditionnels et des formes primitives avec des contours marqués.",
                "Futuriste": "Imaginer un design avant-gardiste avec des formes aérodynamiques, des matériaux métalliques, des interfaces holographiques et une esthétique de science-fiction.",
                "Technologique": "Penser à des motifs complexes comme des circuits imprimés, des lignes de données néon, des grilles digitales et un aspect fibre de carbone.",
                "Asymétrique": "Créer un déséquilibre visuel contrôlé et intentionnel pour un design percutant, moderne et dynamique.",
                "Abstrait": "Utiliser des formes et des couleurs non figuratives pour exprimer une émotion ou une idée de manière artistique et non littérale.",
                "Art Nouveau": "S'inspirer des lignes courbes, des arabesques élégantes et des motifs floraux et végétaux stylisés de la fin du 19ème siècle.",
                "Pixel Art": "Composer le design à partir de carrés visibles (pixels), comme dans les jeux vidéo rétro, pour un look nostalgique et numérique.",
                "Vaporwave": "Mélanger une esthétique rétro des années 80/90 avec des éléments de la culture internet, des couleurs pastel, des néons et des motifs de grille.",
                "Coup de pinceau": "Simuler des touches de peinture épaisses et visibles pour un rendu artistique, expressif et texturé.",
                "Marbré": "Imiter les veines fluides et les motifs organiques du marbre pour un look luxueux, élégant et naturel.",
                "Camouflage": "Utiliser des motifs de camouflage, qu'ils soient classiques (forêt, désert) ou stylisés avec des couleurs inattendues.",
                "Animalier": "Incorporer des motifs de peau d'animaux (zèbre, léopard, serpent) de manière réaliste ou stylisée pour un look audacieux.",
                "Pop Art": "S'inspirer du mouvement artistique des années 60 avec des couleurs vives en aplat, des contours noirs épais et des motifs répétés.",
                "Bauhaus": "Appliquer les principes stricts du Bauhaus : formes géométriques simples (carré, cercle, triangle), couleurs primaires et fonctionnalité pure.",
                "Topographique": "Utiliser des courbes de niveau de cartes topographiques pour créer des motifs de lignes complexes et organiques qui suggèrent le relief.",
                "Mosaïque": "Assembler de petites pièces de couleur (tesselles) pour former une image, comme une mosaïque antique ou moderne.",
                "Fibre de Carbone": "Imiter la texture tissée, la brillance subtile et l'aspect high-tech de la fibre de carbone.",
                "Ondes Sonores": "Visualiser des ondes sonores ou des fréquences radio pour un design rythmé, technologique et dynamique.",
                "Op Art": "Créer de fortes illusions d'optique avec des formes géométriques en noir et blanc ou en couleur qui semblent bouger ou vibrer.",
                "Memphis": "Utiliser des formes géométriques colorées, des motifs ludiques (zigzags, pois) et des couleurs vives et contrastées inspirés du design italien des années 1980.",
                "Néon": "Utiliser des couleurs extrêmement vives et saturées qui semblent briller et émettre de la lumière comme des enseignes au néon.",
                "Holographique": "Créer un effet irisé et changeant qui reflète la lumière avec les couleurs de l'arc-en-ciel, comme un hologramme.",
                "Métallique": "Simuler des textures de métal brossé, chromé, doré ou cuivré, avec des reflets réalistes.",
                "Esquisse / Croquis": "Donner l'impression que le design a été dessiné rapidement à la main, avec des lignes de crayon, de fusain ou de stylo visibles."
            };


            const sportArticleMap = {
                cyclisme_competition: "le cyclisme de compétition",
                cyclotourisme: "le cyclotourisme",
                vtt: "le VTT",
                gravel: "le gravel",
                triathlon: "le triathlon",
                running: "la course à pied",
                trail: "le trail",
                athletisme: "l'athlétisme",
                marche: "la marche"
            };
            const sportName = sportArticleMap[state.clubInfo.sport] || "le sport";

            const line1 = `Conception d'un maillot pour ${sportName}. Présentation catalogue de 4 maillots différents sur une mise en page de catalogue de sport, concept de fiche produit. Qualité professionnelle.`;
            
            let detailedStyles = "";
            if (state.design.styles.length > 0) {
                detailedStyles = state.design.styles.map(style => styleDescriptions[style] || style).join(' ');
            }
            const designLine = `Détails du Design et du Style : style ${state.design.styles.join(' et ')}. ${detailedStyles}`;

            const colorsText = state.design.colors.length > 0 
                ? state.design.colors.join(', ') 
                : "Noir, Blanc, Gris Clair, Jaune Fluo";
            const colorLine = `Palette de Couleurs : ${colorsText}`;

            const qualityBase = "HYPERREALISTIC, rendu professionnel, Rendu ultra-réaliste, qualité studio haute résolution, photographique. Style de rendu digne d'un lookbook de marque de sport haut de gamme. Éclairage professionnel pour sublimer les textures et les volumes.";
            const backgroundText = state.design.background ? `en arrière plan ${state.design.background}.` : "";
            const qualityLine = `Qualité et Rendu :\n${qualityBase} ${backgroundText}`;
            
            const creativeBriefLine = state.design.notes ? `Brief Créatif Complémentaire :\n${state.design.notes}` : "";

            const finalInstruction = "Faire que le prompt me donne des images ultraréalistes comme dans un catalogue de sport.";

            const finalPrompt = [
                designLine,
                line1,
                colorLine,
                qualityLine,
                creativeBriefLine,
                finalInstruction
            ].filter(Boolean).join('\n\n');

            state.iaGenerator.prompt = finalPrompt;
            dom.iaPromptTextarea.value = finalPrompt;
            saveStateToLocalStorage();
        }
        
        function handleCopyPrompt() {
            const promptTextarea = dom.iaPromptTextarea;
            if (!promptTextarea.value) return;

            const showSuccess = () => {
                const originalText = dom.copyPromptBtn.innerHTML;
                dom.copyPromptBtn.innerHTML = `
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 inline-block mr-2" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                    </svg>
                    Copié !`;
                dom.copyPromptBtn.classList.add('bg-green-600', 'hover:bg-green-700');
                setTimeout(() => {
                    dom.copyPromptBtn.innerHTML = originalText;
                    dom.copyPromptBtn.classList.remove('bg-green-600', 'hover:bg-green-700');
                }, 2000);
            };

            promptTextarea.select();
            promptTextarea.setSelectionRange(0, 99999); 

            try {
                // Using the Clipboard API as the primary method
                navigator.clipboard.writeText(promptTextarea.value).then(showSuccess, () => {
                    // Fallback for when the promise rejects (e.g., permissions)
                    document.execCommand('copy');
                    showSuccess();
                });
            } catch (err) {
                // Fallback for older browsers that might not have the Clipboard API
                try {
                    document.execCommand('copy');
                    showSuccess();
                } catch (copyErr) {
                    console.error('Failed to copy with any method:', copyErr);
                }
            }
        }

        
        function handleIaImageUpload(e) {
            const newFiles = Array.from(e.target.files);
            newFiles.forEach(file => {
                if (!state.iaGenerator.uploadedImages.some(img => img.name === file.name)) {
                    state.iaGenerator.uploadedImages.push(file);
                }
            });
            renderIaImagePreviews();
        }

        function generateEmailBody() {
             return `Bonjour Aude,\n\nVeuillez trouver en pièce jointe le PDF récapitulatif d'une nouvelle demande de maquette.\n\nCordialement,`;
        }

        function handleSubmit() {
            collectStepData();
            
            const sportDisplayNames = {
                cyclisme_competition: 'CYCLISME COMPÉTITION',
                cyclotourisme: 'CYCLOTOURISME',
                vtt: 'VTT',
                gravel: 'GRAVEL',
                running: 'RUNNING',
                trail: 'TRAIL',
                athletisme: 'ATHLETISME',
                triathlon: 'TRIATHLON',
                marche: 'MARCHE'
            };
            const sportName = sportDisplayNames[state.clubInfo.sport] || state.clubInfo.sport;

            let summaryHtml = `
                <div class="border-b pb-4">
                    <h4 class="font-bold text-lg text-indigo-700">Informations Club</h4>
                    <p><strong>Nom du Club:</strong> ${state.clubInfo.name}</p>
                    <p><strong>N° client:</strong> ${state.clubInfo.clientNumber}</p>
                    <p><strong>Date de la demande:</strong> ${new Date(state.clubInfo.date).toLocaleDateString('fr-FR')}</p>
                    <p><strong>Sport:</strong> ${sportName}</p>
                    <p><strong>Type de demande:</strong> ${state.clubInfo.requestType === 'creation' ? 'Nouvelle Création' : (state.clubInfo.requestType === 'modification' ? 'Modification' : 'Maquette Fournie')}</p>
                </div>
            `;

            if (state.clubInfo.requestType === 'creation') {
                 summaryHtml += `
                     <div class="border-b pb-4">
                         <h4 class="font-bold text-lg text-indigo-700 mt-4">Style & Couleurs</h4>
                         <p><strong>Styles souhaités:</strong> ${state.design.styles.join(', ') || 'Aucun'}</p>
                         <p><strong>Couleurs sélectionnées:</strong> ${state.design.colors.join(', ') || 'Aucune'}</p>
                         <p class="mt-2"><strong>Notes sur le design:</strong><br><span class="text-sm italic">${state.design.notes || 'Aucune'}</span></p>
                         <p class="mt-2"><strong>Images d'inspiration:</strong> ${state.design.inspirationImages.length} image(s) fournie(s).</p>
                     </div>
                      <div class="border-b pb-4">
                         <h4 class="font-bold text-lg text-indigo-700 mt-4">Visuel(s) de Référence (Canva)</h4>
                         ${generateFilePreviewHtml(state.iaGenerator.uploadedImages)}
                     </div>
                     <div class="border-b pb-4">
                         <h4 class="font-bold text-lg text-indigo-700 mt-4">Articles Sélectionnés</h4>
                         <ul class="list-disc list-inside">${state.selectedArticles.map(item => `<li>${item}</li>`).join('') || "<li>Aucun</li>"}</ul>
                     </div>
                `;
            } else if (state.clubInfo.requestType === 'modification') {
                 summaryHtml += `
                       <div class="border-b pb-4">
                         <h4 class="font-bold text-lg text-indigo-700 mt-4">Détails de la Modification</h4>
                         <p><strong>Visuel(s) existant(s):</strong></p>
                         ${generateFilePreviewHtml(state.modification.existingVisuals)}
                         <p class="mt-4"><strong>Instructions Hauts:</strong><br><span class="text-sm italic">${state.modification.instructions.hauts || 'Aucune'}</span></p>
                         <p class="mt-2"><strong>Instructions Bas:</strong><br><span class="text-sm italic">${state.modification.instructions.bas || 'Aucune'}</span></p>
                         <p class="mt-2"><strong>Instructions Combinaisons:</strong><br><span class="text-sm italic">${state.modification.instructions.combinaisons || 'Aucune'}</span></p>
                         <p class="mt-2"><strong>Instructions Accessoires:</strong><br><span class="text-sm italic">${state.modification.instructions.accessoires || 'Aucune'}</span></p>
                          <p class="mt-4"><strong>Fichiers joints:</strong></p>
                          ${generateFilePreviewHtml(state.modification.supportingFiles)}
                     </div>
                `;
            } else { // 'fournie'
                summaryHtml += `
                    <div class="border-b pb-4">
                        <h4 class="font-bold text-lg text-indigo-700 mt-4">Fichiers de Production Fournis</h4>
                        <p><strong>Fichiers pour production:</strong></p>
                        ${generateFilePreviewHtml(state.production.productionFiles)}
                        <p class="mt-4"><strong>Notes de production:</strong><br><span class="text-sm italic">${state.production.productionNotes || 'Aucune'}</span></p>
                    </div>
                `;
            }

            summaryHtml += `
                <div class="border-b pb-4">
                     <h4 class="font-bold text-lg text-indigo-700 mt-4">Logos & Placement</h4>
                     ${state.logos.length > 0 ? state.logos.map(logo => `
                         <div class="pl-4 border-l-2 my-2">
                             <p><strong>${logo.file.name}</strong></p>
                             <p class="text-xs ml-2">Placement Hauts: ${logo.instructions.hauts || '<i>Non spécifié</i>'}</p>
                             <p class="text-xs ml-2">Placement Bas: ${logo.instructions.bas || '<i>Non spécifié</i>'}</p>
                         </div>
                     `).join('') : '<p>Aucun logo importé.</p>'}
                </div>
                <div>
                     <h4 class="font-bold text-lg text-indigo-700 mt-4">Finalisation</h4>
                     <p><strong>BAT Tissu demandé :</strong> ${state.finalization.batTissu ? 'Oui' : 'Non'}</p>
                     <p><strong>Date impérative :</strong> ${state.finalization.deadline ? new Date(state.finalization.deadline).toLocaleDateString('fr-FR') : 'Aucune'}</p>
                     <p><strong>Date dernière maquette :</strong> ${state.finalization.lastMockupDate ? new Date(state.finalization.lastMockupDate).toLocaleDateString('fr-FR') : 'Non renseignée'}</p>
                     <p><strong>Lien dernière maquette :</strong> ${state.finalization.lastMockupLink ? `<a href="${state.finalization.lastMockupLink}" target="_blank" class="text-indigo-600 hover:underline break-all">${state.finalization.lastMockupLink}</a>` : 'Aucun'}</p>
                     <p class="mt-2"><strong>Commentaires finaux:</strong><br><span class="text-sm italic">${state.finalComments || 'Aucun'}</span></p>
                </div>
            `;
            
            dom.summaryModalBody.innerHTML = summaryHtml;
            dom.summaryModal.classList.add('is-open');
        }
        
        function generateFilePreviewHtml(filesArray) {
            if (!filesArray || filesArray.length === 0) {
                return '<p class="text-sm text-gray-500">Aucun fichier fourni.</p>';
            }
            let html = '<div class="grid grid-cols-4 gap-2 mt-2">';
            filesArray.forEach(file => {
                const url = URL.createObjectURL(file);
                if (file.type.startsWith('image/')) {
                    html += `<div class="text-center"><img src="${url}" class="w-full h-20 object-contain border rounded-md"><p class="text-xs truncate mt-1">${file.name}</p></div>`;
                } else {
                     html += `<div class="text-center border rounded-md p-2 flex flex-col justify-center items-center h-full"><svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-gray-400" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4zm2 6a1 1 0 011-1h6a1 1 0 110 2H7a1 1 0 01-1-1zm1 3a1 1 0 100 2h6a1 1 0 100-2H7z" clip-rule="evenodd" /></svg><p class="text-xs truncate mt-1 w-full">${file.name}</p></div>`;
                }
            });
            html += '</div>';
            return html;
        }
        
        // --- LOCAL STORAGE FUNCTIONS ---

        function saveStateToLocalStorage() {
            // Create a version of the state that can be safely stringified
            // (omitting File objects which cannot be stored)
            const serializableState = {
                currentStep: state.currentStep,
                clubInfo: state.clubInfo,
                selectedArticles: state.selectedArticles,
                design: {
                    colors: state.design.colors,
                    styles: state.design.styles,
                    notes: state.design.notes,
                    background: state.design.background,
                },
                modification: {
                    instructions: state.modification.instructions,
                },
                production: {
                    productionNotes: state.production.productionNotes,
                },
                iaGenerator: {
                    prompt: state.iaGenerator.prompt,
                },
                finalComments: state.finalComments,
                finalization: state.finalization,
            };
            localStorage.setItem('maquetteFormData', JSON.stringify(serializableState));
        }

        function loadStateFromLocalStorage() {
            const savedData = localStorage.getItem('maquetteFormData');
            if (!savedData) return;

            try {
                const parsedData = JSON.parse(savedData);

                // Deep merge the loaded data into the current state
                Object.assign(state.clubInfo, parsedData.clubInfo);
                Object.assign(state.design, parsedData.design);
                Object.assign(state.modification, parsedData.modification);
                Object.assign(state.production, parsedData.production);
                Object.assign(state.iaGenerator, parsedData.iaGenerator);
                if(parsedData.finalization) Object.assign(state.finalization, parsedData.finalization);
                state.selectedArticles = parsedData.selectedArticles || [];
                state.finalComments = parsedData.finalComments || '';
                state.currentStep = parsedData.currentStep || 1;

                // Update the UI with the loaded data
                updateUIFromLoadedState();

                // Show notification
                dom.restoreNotification.classList.add('show');
                setTimeout(() => {
                    dom.restoreNotification.classList.remove('show');
                }, 5000);

            } catch (e) {
                console.error("Failed to load or parse state from localStorage:", e);
                localStorage.removeItem('maquetteFormData'); // Clear corrupted data
            }
        }
        
        function updateUIFromLoadedState() {
             // Step 1
            document.getElementById('clubName').value = state.clubInfo.name || '';
            document.getElementById('clientNumber').value = state.clubInfo.clientNumber || '';
            if (state.clubInfo.sport) {
                document.querySelector(`input[name="sport"][value="${state.clubInfo.sport}"]`).checked = true;
            }
            if (state.clubInfo.requestType) {
                document.querySelector(`input[name="request-type"][value="${state.clubInfo.requestType}"]`).checked = true;
            }
            // Step 2
            document.getElementById('design-notes').value = state.design.notes || '';
            document.getElementById('ia-background-input').value = state.design.background || '';
            updateColorSelectionUI();
            document.querySelectorAll('.style-checkbox').forEach(cb => {
                cb.checked = state.design.styles.includes(cb.value);
            });
            // Step 3
            dom.iaPromptTextarea.value = state.iaGenerator.prompt || '';
            // Step 5
            document.getElementById('final-comments').value = state.finalComments || '';
            if (state.finalization) { // Check if it exists for backward compatibility
                document.getElementById('bat-tissu').checked = state.finalization.batTissu || false;
                document.getElementById('deadline-date').value = state.finalization.deadline || '';
                document.getElementById('last-mockup-date').value = state.finalization.lastMockupDate || '';
                document.getElementById('last-mockup-link').value = state.finalization.lastMockupLink || '';
            }

        }

        // --- INITIALIZATION & EVENT LISTENERS ---

        function init() {
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('requestDate').value = today;
            state.clubInfo.date = today;

            // Render dynamic components
            renderColorPicker();
            renderStyleSelection();
            renderIaImagePreviews(); // To show the initial placeholder
            
            // Try to load saved data
            loadStateFromLocalStorage();

            // Initial render of the current step
            renderStep();

            // --- Event Listeners ---
            
            // Navigation
            dom.prevBtn.addEventListener('click', () => handleNavClick('prev'));
            dom.nextBtn.addEventListener('click', () => handleNavClick('next'));
            dom.submitBtn.addEventListener('click', handleSubmit);

            // Step 1 Input Validation
            document.getElementById('clubName').addEventListener('input', () => validateStep());
            document.getElementById('clientNumber').addEventListener('input', () => validateStep());

            // Color Picker
            dom.colorPickerContainer.addEventListener('click', handleColorClick);
            
            // Main event delegation for dynamically added/changed content
            document.querySelector('main').addEventListener('change', (e) => {
                if(e.target.name === 'request-type' || e.target.name === 'sport') {
                    state.clubInfo[e.target.name] = e.target.value;
                    renderStep(); // Re-render for conditional steps
                    saveStateToLocalStorage();
                } else if(e.target.id === 'existing-visual-upload') {
                    state.modification.existingVisuals.push(...Array.from(e.target.files));
                    renderFilePreviews(state.modification.existingVisuals, document.getElementById('existing-visual-preview'), 'existingVisuals');
                } else if (e.target.id === 'production-files-upload') {
                    state.production.productionFiles.push(...Array.from(e.target.files));
                    renderFilePreviews(state.production.productionFiles, document.getElementById('production-files-preview-area'), 'productionFiles');
                }
                else if(e.target.id === 'supporting-files-upload') {
                     state.modification.supportingFiles.push(...Array.from(e.target.files));
                     renderFilePreviews(state.modification.supportingFiles, document.getElementById('supporting-files-preview-area'), 'supportingFiles');
                } else if(e.target.id === 'ia-image-upload') {
                    handleIaImageUpload(e);
                } else if(e.target.classList.contains('style-checkbox') || e.target.id === 'bat-tissu' || e.target.id === 'deadline-date') {
                    collectStepData(); // Collects and saves state
                }
            });
             document.querySelector('main').addEventListener('input', (e) => {
                  if (e.target.classList.contains('mod-instruction') || e.target.id === 'production-notes' || e.target.id === 'design-notes' || e.target.id === 'final-comments') {
                     collectStepData(); // Collects and saves state
                  }
             });
             document.querySelector('main').addEventListener('click', (e) => {
                if (e.target.id === 'reset-app-btn') {
                    dom.resetModal.classList.add('is-open');
                }

                const removeBtn = e.target.closest('.remove-file-btn');
                if (removeBtn) {
                    const { arrayName, fileIndex } = removeBtn.dataset;
                    const index = parseInt(fileIndex);
                    
                    if (arrayName === 'newLogos') {
                        state.modification.newLogos.splice(index, 1);
                        renderFilePreviews(state.modification.newLogos, document.getElementById('new-logo-preview-area'), 'newLogos');
                    } else if (arrayName === 'supportingFiles') {
                        state.modification.supportingFiles.splice(index, 1);
                        renderFilePreviews(state.modification.supportingFiles, document.getElementById('supporting-files-preview-area'), 'supportingFiles');
                    } else if (arrayName === 'productionFiles') {
                        state.production.productionFiles.splice(index, 1);
                        renderFilePreviews(state.production.productionFiles, document.getElementById('production-files-preview-area'), 'productionFiles');
                    } else if (arrayName === 'existingVisuals') {
                        state.modification.existingVisuals.splice(index, 1);
                        renderFilePreviews(state.modification.existingVisuals, document.getElementById('existing-visual-preview'), 'existingVisuals');
                    }
                }
            });


            // Logo Uploads
            dom.logoUpload.addEventListener('change', handleLogoUpload);
            dom.logoPreviewArea.addEventListener('click', (e) => {
                if(e.target.classList.contains('remove-logo-btn')) {
                    const index = parseInt(e.target.dataset.logoIndex);
                    state.logos.splice(index, 1);
                    renderLogoPreviews();
                }
            });
            dom.logoInstructionsArea.addEventListener('input', (e) => {
                 if(e.target.classList.contains('logo-instruction-input')) {
                    const index = parseInt(e.target.dataset.logoIndex);
                    const placement = e.target.dataset.placement;
                    state.logos[index].instructions[placement] = e.target.value;
                 }
            });
            
            // Inspiration Uploads
            dom.inspirationUpload.addEventListener('change', handleInspirationUpload);
            dom.inspirationPreviewArea.addEventListener('click', (e) => {
                if(e.target.classList.contains('remove-inspiration-btn')) {
                    const index = parseInt(e.target.dataset.inspirationIndex);
                    state.design.inspirationImages.splice(index, 1);
                    renderInspirationPreviews();
                }
            });

            // AI Generator listeners
            dom.generatePromptBtn.addEventListener('click', handleGeneratePrompt);
            dom.copyPromptBtn.addEventListener('click', handleCopyPrompt);
            dom.iaImageResultArea.addEventListener('click', (e) => {
                const removeBtn = e.target.closest('.remove-ia-image-btn');
                if (removeBtn) {
                    const index = parseInt(removeBtn.dataset.iaImageIndex);
                    state.iaGenerator.uploadedImages.splice(index, 1);
                    renderIaImagePreviews();
                }
            });


            // Modal
            dom.summaryModalCloseBtn.addEventListener('click', () => dom.summaryModal.classList.remove('is-open'));

            const generateAndOpenEmail = () => {
                const subject = `Demande de Maquette - ${state.clubInfo.name} (${state.clubInfo.clientNumber})`;
                const body = `Bonjour,

Veuillez trouver ci-joint ma demande de maquette.

Merci.

---
Récapitulatif :
Club : ${state.clubInfo.name}
N° Client : ${state.clubInfo.clientNumber}
---
`;
                const mailtoLink = `mailto:aude@noret.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
                window.location.href = mailtoLink;
            };

            const handlePrintAndEmail = () => {
                const modalBody = dom.summaryModalBody;
                const printWindow = window.open('', '_blank');
                printWindow.document.write('<html><head><title>Récapitulatif de la demande</title>');
                printWindow.document.write('<script src="https://cdn.tailwindcss.com"><\/script>');
                printWindow.document.write('<style>body { font-family: "Inter", sans-serif; padding: 20px; } img { max-width: 100% !important; page-break-inside: avoid; } .grid { display: grid !important; } .grid-cols-4 { grid-template-columns: repeat(4, minmax(0, 1fr)) !important; } .gap-2 { gap: 0.5rem !important; } .text-center { text-align: center !important; } .border { border-width: 1px !important; } .rounded-md { border-radius: 0.375rem !important; } .object-contain { object-fit: contain !important; } .w-full { width: 100% !important; } .h-20 { height: 5rem !important; } .mt-1 { margin-top: 0.25rem !important; } .truncate { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; } .text-xs { font-size: 0.75rem !important; line-height: 1rem !important; }</style>');
                printWindow.document.write('</head><body>');
                printWindow.document.write(modalBody.innerHTML);
                printWindow.document.write('</body></html>');
                printWindow.document.close();
                printWindow.focus();
                
                setTimeout(() => {
                    printWindow.print();
                    
                    // The 'afterprint' event is a more reliable way to know when the print dialog is closed.
                    printWindow.addEventListener('afterprint', () => {
                        printWindow.close();
                        generateAndOpenEmail();
                    });

                    // As a fallback for some browsers that might not fire 'afterprint' correctly
                    // for "Save as PDF", we'll still trigger the email after a delay.
                    // The user can simply close the extra email window if it opens.
                    let emailTriggered = false;
                    const fallbackTimeout = setTimeout(() => {
                        if (!emailTriggered) {
                            generateAndOpenEmail();
                        }
                    }, 2000); 

                    printWindow.onafterprint = () => {
                        emailTriggered = true;
                        clearTimeout(fallbackTimeout);
                        printWindow.close();
                        generateAndOpenEmail();
                    };


                }, 500);

                dom.summaryModal.classList.remove('is-open');
                localStorage.removeItem('maquetteFormData');
            };

            document.getElementById('print-btn').addEventListener('click', handlePrintAndEmail);

            dom.cancelSendBtn.addEventListener('click', () => dom.summaryModal.classList.remove('is-open'));

            // Notification close button
            dom.restoreNotificationClose.addEventListener('click', () => {
                dom.restoreNotification.classList.remove('show');
            });

            // Reset Modal Listeners
            dom.cancelResetBtn.addEventListener('click', () => {
                dom.resetModal.classList.remove('is-open');
            });
            dom.confirmResetBtn.addEventListener('click', () => {
                localStorage.removeItem('maquetteFormData');
                window.location.reload();
            });
        }

        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>

</html>

