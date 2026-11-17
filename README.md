<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultime index pokémon</title>
    <link rel="icon" type="image/jpeg" href="https://files.catbox.moe/k8u0g5.jpg">
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800;900&display=swap');
        
        :root {
            --primary: #10B981;
            --primary-dark: #059669;
            --accent: #F59E0B;
            --bg-dark: #0F172A;
            --card-bg: #1E293B;
        }

        body {
            font-family: 'Outfit', sans-serif;
            background-color: var(--bg-dark);
            color: #F8FAFC;
            background-image: 
                radial-gradient(at 0% 0%, rgba(16, 185, 129, 0.15) 0px, transparent 50%),
                radial-gradient(at 100% 100%, rgba(245, 158, 11, 0.15) 0px, transparent 50%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        .glass-panel {
            background: rgba(30, 41, 59, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .glow-effect {
            box-shadow: 0 0 25px -5px rgba(16, 185, 129, 0.4);
        }

        .dynamax-aura {
            animation: pulseGlow 2s infinite alternate;
        }

        @keyframes pulseGlow {
            0% { box-shadow: 0 0 15px #3B82F6, inset 0 0 15px #3B82F6; }
            100% { box-shadow: 0 0 35px #60A5FA, inset 0 0 25px #60A5FA; }
        }

        .gigamax-aura {
            animation: gigaGlow 1.5s infinite alternate;
        }

        @keyframes gigaGlow {
            0% { box-shadow: 0 0 20px #FF007F, 0 0 40px #FF007F; border-color: #FF007F; }
            100% { box-shadow: 0 0 40px #FF7700, 0 0 80px #FF7700; border-color: #FF7700; }
        }

        .type-badge {
            font-weight: 600;
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.85rem;
            text-shadow: 0 1px 2px rgba(0,0,0,0.5);
        }

        .custom-scrollbar::-webkit-scrollbar {
            width: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #0F172A;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #10B981;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen">

    <!-- Header -->
    <header class="glass-panel sticky top-0 z-50 border-b border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="relative">
                    <img src="https://files.catbox.moe/k8u0g5.jpg" alt="Logo" class="w-12 h-12 rounded-full border-2 border-emerald-500 object-cover shadow-lg hover:scale-105 transition-transform">
                    <div class="absolute -bottom-1 -right-1 bg-emerald-500 text-slate-900 rounded-full p-1 text-xs font-bold shadow">
                        <i class="fa-solid fa-bolt"></i>
                    </div>
                </div>
                <div>
                    <h1 class="text-2xl font-black tracking-wider bg-gradient-to-r from-emerald-400 via-teal-300 to-amber-400 bg-clip-text text-transparent">
                        ULTIME INDEX POKÉMON
                    </h1>
                    <p class="text-xs text-slate-400 flex items-center gap-2">
                        <span class="inline-block w-2 h-2 rounded-full bg-emerald-500 animate-ping"></span>
                        Synchronisé PokéAPI & Formes Spéciales
                    </p>
                </div>
            </div>
            <div class="flex items-center gap-2">
                <button id="sync-btn" onclick="app.forceSync()" class="p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-emerald-400 transition-colors border border-slate-700 flex items-center gap-2 text-sm font-semibold shadow">
                    <i class="fa-solid fa-rotate animate-spin" id="sync-spinner" style="animation-duration: 3s;"></i>
                    <span class="hidden sm:inline">Actualiser API</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Dashboard Container -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-8">
        
        <!-- Stats Banner -->
        <section class="grid grid-cols-2 md:grid-cols-4 gap-4">
            <div class="glass-panel p-4 rounded-2xl border-l-4 border-emerald-500 flex items-center justify-between">
                <div>
                    <p class="text-xs font-medium text-slate-400">Pokémon Chargés</p>
                    <h3 id="stat-total" class="text-2xl font-black text-white mt-1">1025</h3>
                </div>
                <div class="p-3 bg-emerald-500/10 text-emerald-400 rounded-xl">
                    <i class="fa-solid fa-folder-open text-xl"></i>
                </div>
            </div>
            <div class="glass-panel p-4 rounded-2xl border-l-4 border-amber-500 flex items-center justify-between">
                <div>
                    <p class="text-xs font-medium text-slate-400">Variantes Chromatiques</p>
                    <h3 id="stat-shiny" class="text-2xl font-black text-amber-400 mt-1">1025+</h3>
                </div>
                <div class="p-3 bg-amber-500/10 text-amber-400 rounded-xl">
                    <i class="fa-solid fa-sparkles text-xl"></i>
                </div>
            </div>
            <div class="glass-panel p-4 rounded-2xl border-l-4 border-blue-500 flex items-center justify-between">
                <div>
                    <p class="text-xs font-medium text-slate-400">Formes Dynamax</p>
                    <h3 class="text-2xl font-black text-blue-400 mt-1">1025</h3>
                </div>
                <div class="p-3 bg-blue-500/10 text-blue-400 rounded-xl">
                    <i class="fa-solid fa-hurricane text-xl"></i>
                </div>
            </div>
            <div class="glass-panel p-4 rounded-2xl border-l-4 border-fuchsia-500 flex items-center justify-between">
                <div>
                    <p class="text-xs font-medium text-slate-400">Formes Gigamax</p>
                    <h3 class="text-2xl font-black text-fuchsia-400 mt-1">32</h3>
                </div>
                <div class="p-3 bg-fuchsia-500/10 text-fuchsia-400 rounded-xl">
                    <i class="fa-solid fa-bolt-lightning text-xl"></i>
                </div>
            </div>
        </section>

        <!-- Search & Control Bar -->
        <section class="glass-panel p-4 sm:p-6 rounded-3xl glow-effect space-y-4">
            <div class="flex flex-col md:flex-row gap-4 items-center justify-between">
                <div class="relative w-full md:w-2/3">
                    <i class="fa-solid fa-magnifying-glass absolute left-4 top-1/2 -translate-y-1/2 text-slate-400 text-lg"></i>
                    <input type="text" id="search-input" placeholder="Rechercher par nom (ex: Salamèche) ou numéro (#0004)..." class="w-full bg-slate-900/80 border border-slate-700 rounded-2xl pl-12 pr-4 py-3.5 text-white placeholder-slate-400 focus:outline-none focus:border-emerald-500 transition-colors shadow-inner text-base">
                    <button onclick="app.clearSearch()" class="absolute right-4 top-1/2 -translate-y-1/2 text-slate-400 hover:text-white">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <div class="flex flex-wrap items-center gap-2 w-full md:w-auto justify-end">
                    <div class="inline-flex rounded-xl bg-slate-900 border border-slate-700 p-1 w-full sm:w-auto">
                        <button onclick="app.setGlobalMode('normal')" id="btn-mode-normal" class="flex-1 sm:flex-initial px-3 py-1.5 rounded-lg text-xs font-bold transition-all bg-emerald-500 text-slate-900 shadow">Normal</button>
                        <button onclick="app.setGlobalMode('shiny')" id="btn-mode-shiny" class="flex-1 sm:flex-initial px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-white">Shiny ✨</button>
                        <button onclick="app.setGlobalMode('dynamax')" id="btn-mode-dynamax" class="flex-1 sm:flex-initial px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-white flex items-center justify-center gap-1">Dyna 🌀</button>
                        <button onclick="app.setGlobalMode('gigamax')" id="btn-mode-gigamax" class="flex-1 sm:flex-initial px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-white flex items-center justify-center gap-1">Giga ⚡</button>
                    </div>
                </div>
            </div>

            <!-- Chips Filters -->
            <div class="pt-4 border-t border-slate-800/80 flex flex-col gap-3">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-bold text-slate-400 uppercase tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-filter text-emerald-400"></i> Filtres Rapides
                    </span>
                    <button onclick="app.resetFilters()" class="text-xs text-emerald-400 hover:underline font-semibold">Réinitialiser</button>
                </div>
                
                <div class="flex flex-wrap gap-2">
                    <button onclick="app.filterByEvent()" id="chip-event" class="px-3.5 py-1.5 rounded-xl text-xs font-bold bg-amber-500/10 border border-amber-500/30 text-amber-400 hover:bg-amber-500/20 transition-all flex items-center gap-1.5">
                        <i class="fa-solid fa-ticket"></i> Formes Événement / Distribution 🎟️
                    </button>
                </div>

                <div class="flex items-center gap-2 overflow-x-auto pb-2 custom-scrollbar text-xs" id="type-filters">
                    <button onclick="app.filterByType('all')" class="type-btn px-3 py-1 rounded-lg bg-slate-800 text-white font-bold border border-slate-700 whitespace-nowrap active-type" data-type="all">Tous les Types</button>
                </div>
            </div>
        </section>

        <!-- Pokemon Grid Section -->
        <section class="space-y-4">
            <div class="flex items-center justify-between px-2">
                <div class="flex items-center gap-3">
                    <h2 class="text-xl font-bold flex items-center gap-2">
                        <span>Pokédex National</span>
                        <span id="pokemon-count-badge" class="px-2.5 py-0.5 rounded-full bg-slate-800 border border-slate-700 text-xs font-mono text-emerald-400">40 / 1025</span>
                    </h2>
                </div>
                <div class="flex items-center gap-2">
                    <span class="text-xs text-slate-400">Trier par:</span>
                    <select id="sort-select" onchange="app.sortPokemon(this.value)" class="bg-slate-900 border border-slate-700 text-white rounded-xl px-3 py-1.5 text-xs font-semibold focus:outline-none focus:border-emerald-500">
                        <option value="id-asc">Numéro (Croissant)</option>
                        <option value="id-desc">Numéro (Décroissant)</option>
                        <option value="pc-desc">PC Max (Décroissant)</option>
                        <option value="name-asc">Nom (A-Z)</option>
                    </select>
                </div>
            </div>

            <!-- Grid -->
            <div id="pokemon-grid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-6">
                <!-- Rempli instantanément par le script -->
            </div>

            <!-- Load More Controls -->
            <div class="pt-8 text-center space-y-4">
                <button id="load-more-btn" onclick="app.loadMore()" class="glass-panel px-8 py-3.5 rounded-2xl font-bold text-sm bg-emerald-500/10 border border-emerald-500/30 text-emerald-400 hover:bg-emerald-500/20 hover:scale-105 transition-all shadow-lg flex items-center gap-3 mx-auto">
                    <span>Afficher plus de Pokémon</span>
                    <i class="fa-solid fa-chevron-down text-xs"></i>
                </button>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="mt-auto glass-panel border-t border-slate-800 py-6 px-4">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4 text-center md:text-left">
            <div class="flex items-center space-x-3">
                <img src="https://files.catbox.moe/k8u0g5.jpg" alt="Logo" class="w-10 h-10 rounded-full border border-emerald-500 object-cover">
                <div>
                    <h4 class="font-black tracking-wider text-base bg-gradient-to-r from-emerald-400 to-teal-200 bg-clip-text text-transparent">
                        ULTIME INDEX POKÉMON
                    </h4>
                    <p class="text-xs text-slate-500">Base de données complète temps-réel</p>
                </div>
            </div>
            
            <div class="flex items-center justify-center gap-2 bg-slate-900 border border-slate-800 px-4 py-2 rounded-2xl shadow-inner">
                <span class="text-xs text-slate-400 font-medium">Créateur & Partenaire :</span>
                <a href="https://www.youtube.com/@AJC14" target="_blank" class="flex items-center gap-1.5 text-rose-500 hover:text-rose-400 font-extrabold text-sm transition-colors group">
                    <i class="fa-brands fa-youtube text-xl group-hover:scale-110 transition-transform text-red-500"></i>
                    <span>YouTube AJC14</span>
                </a>
            </div>

            <p class="text-xs text-slate-500">
                Données fournies by <a href="https://pokeapi.co/" target="_blank" class="text-slate-400 hover:underline">PokéAPI</a>. Non affilié à Nintendo/Game Freak.
            </p>
        </div>
    </footer>

    <!-- Detailed Modal -->
    <div id="pokemon-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-md hidden opacity-0 transition-opacity duration-300">
        <div class="glass-panel w-full max-w-2xl rounded-3xl overflow-hidden shadow-2xl border border-slate-700 flex flex-col max-h-[90vh]">
            <!-- Modal Header -->
            <div id="modal-header-bg" class="p-6 relative bg-gradient-to-b from-slate-800 to-slate-900 text-white">
                <button onclick="app.closeModal()" class="absolute top-4 right-4 w-9 h-9 rounded-full bg-slate-950/50 hover:bg-slate-950/80 flex items-center justify-center text-slate-300 hover:text-white transition-colors border border-slate-700">
                    <i class="fa-solid fa-xmark"></i>
                </button>
                <div class="flex items-center gap-4">
                    <span id="modal-id" class="text-3xl font-black text-white/40 font-mono">#0000</span>
                    <div>
                        <h2 id="modal-name" class="text-3xl font-black uppercase tracking-wide">Pokémon</h2>
                        <div id="modal-types" class="flex gap-2 mt-2"></div>
                    </div>
                </div>
            </div>

            <!-- Modal Body -->
            <div class="p-6 overflow-y-auto custom-scrollbar flex-grow space-y-6">
                <!-- Visual Showcase Grid -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div class="glass-panel p-3 rounded-2xl text-center border border-slate-700/80 bg-slate-900/50 flex flex-col items-center justify-between">
                        <span class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-2 w-full text-left">NORMAL</span>
                        <div class="h-28 flex items-center justify-center my-1">
                            <img id="modal-img-normal" src="" alt="Normal" class="max-h-full object-contain filter drop-shadow">
                        </div>
                        <span class="text-xs font-mono font-bold text-white mt-2 bg-slate-800 px-2 py-0.5 rounded w-full text-center">PC <span id="modal-pc-normal">0</span></span>
                    </div>

                    <div class="glass-panel p-3 rounded-2xl text-center border border-amber-500/30 bg-amber-500/5 flex flex-col items-center justify-between relative overflow-hidden group">
                        <span class="text-[10px] font-bold text-amber-400 uppercase tracking-widest mb-2 flex items-center gap-1 w-full text-left"><i class="fa-solid fa-sparkles"></i> SHINY</span>
                        <div class="h-28 flex items-center justify-center my-1">
                            <img id="modal-img-shiny" src="" alt="Shiny" class="max-h-full object-contain filter drop-shadow group-hover:scale-110 transition-transform">
                        </div>
                        <span class="text-xs font-mono font-bold text-amber-400 mt-2 bg-amber-500/10 px-2 py-0.5 rounded w-full text-center border border-amber-500/20">PC <span id="modal-pc-shiny">0</span></span>
                    </div>

                    <div class="glass-panel p-3 rounded-2xl text-center border border-blue-500/30 bg-blue-500/5 flex flex-col items-center justify-between relative overflow-hidden group">
                        <span class="text-[10px] font-bold text-blue-400 uppercase tracking-widest mb-2 flex items-center gap-1 w-full text-left"><i class="fa-solid fa-hurricane"></i> DYNAMAX</span>
                        <div class="h-28 flex items-center justify-center my-1 relative">
                            <div class="absolute inset-0 rounded-full dynamax-aura opacity-50 pointer-events-none"></div>
                            <img id="modal-img-dynamax" src="" alt="Dynamax" class="max-h-full object-contain relative z-10 filter drop-shadow group-hover:scale-110 transition-transform">
                        </div>
                        <span class="text-xs font-mono font-bold text-blue-400 mt-2 bg-blue-500/10 px-2 py-0.5 rounded w-full text-center border border-blue-500/20">PC <span id="modal-pc-dynamax">0</span></span>
                    </div>

                    <div class="glass-panel p-3 rounded-2xl text-center border border-fuchsia-500/30 bg-fuchsia-500/5 flex flex-col items-center justify-between relative overflow-hidden group">
                        <span class="text-[10px] font-bold text-fuchsia-400 uppercase tracking-widest mb-2 flex items-center gap-1 w-full text-left"><i class="fa-solid fa-bolt-lightning"></i> GIGAMAX</span>
                        <div class="h-28 flex items-center justify-center my-1 relative">
                            <div class="absolute inset-0 rounded-full gigamax-aura opacity-50 pointer-events-none"></div>
                            <img id="modal-img-gigamax" src="" alt="Gigamax" class="max-h-full object-contain relative z-10 filter drop-shadow group-hover:scale-110 transition-transform">
                        </div>
                        <span class="text-xs font-mono font-bold text-fuchsia-400 mt-2 bg-fuchsia-500/10 px-2 py-0.5 rounded w-full text-center border border-fuchsia-500/20">PC <span id="modal-pc-gigamax">0</span></span>
                    </div>
                </div>

                <!-- Stats & Info -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="glass-panel p-4 rounded-2xl border border-slate-700/80 space-y-3">
           
