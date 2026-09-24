<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BARGUNÇADOS ROCK BAR | Pub, Arcade & Metal</title>
  
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Montserrat:wght@400;600;800;900&family=Press+Start+2P&display=swap" rel="stylesheet">

  <style>
    /* =========================================
       1. RESET E VARIÁVEIS (Mobile-First)
       ========================================= */
    * { box-sizing: border-box; margin: 0; padding: 0; cursor: default; }

    :root {
      --bg-dark: #050505;
      /* Vidro Fosco Premium */
      --glass-bg: rgba(20, 20, 25, 0.45);
      --glass-border-top: rgba(255, 255, 255, 0.25);
      --glass-border-bot: rgba(255, 255, 255, 0.05);
      --glass-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.8);
      
      --c-pink: #e91e63; --c-yellow: #ffeb3b; --c-red: #d32f2f;
      --c-cyan: #00bcd4; --c-purple: #673ab7;
      --text-main: #f0f0f3; --text-muted: #a0a0b0;
      
      --font-title: 'Bebas Neue', sans-serif;
      --font-arcade: 'Press Start 2P', monospace;
      --font-body: 'Montserrat', sans-serif;
    }

    body {
      background-color: var(--bg-dark); color: var(--text-main); font-family: var(--font-body);
      overflow-x: hidden; font-size: 14px; scroll-behavior: smooth;
    }

    ::-webkit-scrollbar { width: 5px; height: 4px; }
    ::-webkit-scrollbar-track { background: transparent; }
    ::-webkit-scrollbar-thumb { background: var(--c-red); border-radius: 4px; }

    /* =========================================
       2. FUNDO MARCA D'ÁGUA E LUZES DE SHOW
       ========================================= */
    .bg-watermark {
      position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; z-index: -3;
      background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRCsoBmqPE2nWj7rFdVVvIETVDsDxHWzIqjU9d7JfjpTQ&s");
      background-position: center; background-size: cover; background-repeat: no-repeat;
      opacity: 0.15; filter: grayscale(10%) contrast(150%) blur(3px);
      animation: pulseBg 12s infinite alternate ease-in-out;
    }
    @keyframes pulseBg { 0% { transform: scale(1); opacity: 0.1; } 100% { transform: scale(1.1); opacity: 0.25; } }
    
    .stage-lights { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; pointer-events: none; z-index: -2; overflow: hidden; }
    .spotlight { position: absolute; top: -15vh; width: 60vw; height: 140vh; clip-path: polygon(50% 0%, 100% 100%, 0% 100%); mix-blend-mode: screen; opacity: 0.85; }
    .spotlight.cyan { left: -15vw; background: linear-gradient(to bottom, rgba(0, 188, 212, 0.5) 0%, transparent 100%); animation: swingLightLeft 4.5s ease-in-out infinite alternate; }
    .spotlight.pink { right: -15vw; background: linear-gradient(to bottom, rgba(233, 30, 99, 0.5) 0%, transparent 100%); animation: swingLightRight 5.5s ease-in-out infinite alternate-reverse; }
    .spotlight.yellow { left: 20vw; background: linear-gradient(to bottom, rgba(255, 235, 59, 0.3) 0%, transparent 100%); animation: swingLightCenter 6.5s ease-in-out infinite alternate; }

    @keyframes swingLightLeft { 0% { transform: rotate(40deg); } 100% { transform: rotate(5deg); } }
    @keyframes swingLightRight { 0% { transform: rotate(-40deg); } 100% { transform: rotate(-5deg); } }
    @keyframes swingLightCenter { 0% { transform: rotate(-15deg); } 100% { transform: rotate(15deg); } }

    #sparks-canvas { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; pointer-events: none; z-index: -1; opacity: 0.9; }

    /* EFEITO VIDRO FOSCO */
    .glass-panel {
      background: var(--glass-bg); backdrop-filter: blur(20px) saturate(200%); -webkit-backdrop-filter: blur(20px) saturate(200%);
      border-top: 1px solid var(--glass-border-top); border-left: 1px solid var(--glass-border-top);
      border-bottom: 1px solid var(--glass-border-bot); border-right: 1px solid var(--glass-border-bot);
      box-shadow: var(--glass-shadow); border-radius: 12px; transition: all 0.4s cubic-bezier(0.25, 0.8, 0.25, 1);
    }
    .glass-panel:hover { border-top-color: var(--c-cyan); box-shadow: 0 10px 50px rgba(0, 188, 212, 0.25); transform: translateY(-2px); }

    .app-wrapper { position: relative; z-index: 2; display: flex; flex-direction: column; min-height: 100vh; }

    /* =========================================
       3. RÁDIO E CABEÇALHO
       ========================================= */
    .radio-player-bar {
      border-radius: 0; border-top: none; border-left: none; border-right: none; border-bottom: 2px solid var(--c-cyan);
      display: flex; align-items: center; justify-content: space-between; padding: 10px 15px; position: relative; z-index: 100; flex-wrap: wrap; gap: 10px;
    }
    .radio-info-wrapper { display: flex; align-items: center; gap: 10px; flex: 1; }
    .radio-cover { width: 45px; height: 45px; border-radius: 50%; object-fit: cover; border: 2px solid var(--c-pink); animation: spinRecord 4s linear infinite paused; }
    @keyframes spinRecord { to { transform: rotate(360deg); } }
    .radio-text { display: flex; flex-direction: column; }
    .radio-station-name { color: var(--c-yellow); font-family: var(--font-title); font-size: 1.2rem; letter-spacing: 1px; }
    .radio-now-playing { color: #fff; font-size: 0.8rem; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 220px; }
    .radio-now-playing span { color: var(--c-cyan); text-transform: uppercase;}
    .play-pause-btn { background: rgba(0,0,0,0.6); border: 1px solid var(--c-cyan); color: var(--c-cyan); font-family: var(--font-title); font-size: 1rem; padding: 6px 15px; border-radius: 20px; transition: 0.3s; cursor:pointer;}
    .play-pause-btn:hover { background: var(--c-cyan); color: #000; box-shadow: 0 0 20px var(--c-cyan); transform: scale(1.05); }
    audio { display: none; }

    .promo-banner { background: #0a0000; color: #fff; padding: 8px; text-align: center; font-weight: 800; font-size: 0.8rem; text-transform: uppercase; border-bottom: 1px solid var(--c-red); }
    .promo-banner span { color: var(--c-yellow); border: 1px solid var(--c-yellow); padding: 1px 4px; border-radius: 3px; cursor: pointer; }

    header { padding: 1.5rem 1rem; text-align: center; display: flex; flex-direction: column; align-items: center; }
    .logo-img { max-width: 180px; width: 100%; border-radius: 50%; box-shadow: 0 0 25px rgba(233,30,99,0.6); border: 2px solid rgba(255,255,255,0.2); animation: floatLogo 4s ease-in-out infinite; cursor: crosshair;}
    @keyframes floatLogo { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); border-color: var(--c-yellow); box-shadow: 0 0 35px rgba(0,188,212,0.8); } }

    /* =========================================
       4. NAVEGAÇÃO
       ========================================= */
    .nav-container { background: rgba(5, 5, 8, 0.7); backdrop-filter: blur(15px); border-bottom: 1px solid rgba(255,255,255,0.1); position: sticky; top: 0; z-index: 1000; box-shadow: 0 5px 20px rgba(0,0,0,0.6); }
    nav.nav-tabs { display: flex; flex-wrap: wrap; justify-content: center; padding: 8px; gap: 6px; max-width: 900px; margin: 0 auto; }
    .tab-btn {
      flex: 1 1 calc(20% - 6px); background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255,255,255,0.05); color: var(--text-muted); 
      font-family: var(--font-title); font-size: 1.05rem; letter-spacing: 1px; padding: 8px 2px; 
      border-radius: 6px; transition: 0.3s; text-align: center; cursor: pointer; text-transform: uppercase;
    }
    .tab-btn:hover { background: rgba(255,255,255,0.15); color: var(--text-main); }
    .tab-btn.active { color: #fff; background: rgba(233, 30, 99, 0.2); border-color: var(--c-pink); box-shadow: inset 0 0 10px rgba(233,30,99,0.3); }

    /* =========================================
       5. TRANSIÇÃO 3D E ESTRUTURA PRINCIPAL
       ========================================= */
    main { flex: 1; max-width: 1000px; margin: 0 auto; width: 100%; padding: 1.5rem 5% 3rem; perspective: 1200px; }
    .tab-pane { display: none; }
    .tab-pane.active { display: block; animation: flipCube 0.6s cubic-bezier(0.25, 1, 0.5, 1) forwards; transform-origin: top; }
    @keyframes flipCube { 0% { opacity: 0; transform: rotateX(-90deg) translateY(-50px); } 100% { opacity: 1; transform: rotateX(0deg) translateY(0); } }

    .section-title { font-family: var(--font-title); font-size: 2rem; color: #fff; margin-bottom: 1.2rem; border-bottom: 1px solid rgba(255,255,255,0.1); padding-bottom: 5px; display: flex; align-items: center; gap: 8px; text-shadow: 0 0 10px rgba(255,255,255,0.3); }
    .section-title::before { content: '⚡'; color: var(--c-yellow); font-size: 0.8em; text-shadow: none; }

    .grid { display: grid; gap: 15px; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); }
    .btn-fire { display: inline-block; background: linear-gradient(90deg, var(--c-pink), var(--c-purple)); color: #fff; font-family: var(--font-title); font-size: 1.2rem; letter-spacing: 1px; padding: 10px 20px; border: none; border-radius: 6px; text-decoration: none; text-transform: uppercase; cursor: pointer; transition: 0.3s; box-shadow: 0 4px 15px rgba(233,30,99,0.4); width: 100%; text-align: center; margin-top: 10px; }
    .btn-fire:hover { filter: brightness(1.3); box-shadow: 0 6px 20px rgba(0,188,212,0.6); transform: scale(1.02); }

    /* =========================================
       6. CARDÁPIO (GRID 2x2 RIGOROSA)
       ========================================= */
    .menu-category { font-family: var(--font-title); font-size: 1.5rem; color: var(--c-yellow); margin: 1.5rem 0 0.8rem; border-bottom: 1px dashed rgba(255,235,59,0.3); padding-bottom: 5px; }
    .menu-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; }

    .product-card { display: flex; flex-direction: column; align-items: center; text-align: center; padding: 10px; cursor: pointer; position: relative; overflow: hidden; }
    .product-card::before { content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 3px; background: linear-gradient(90deg, var(--c-cyan), var(--c-pink)); opacity: 0; transition: 0.3s; }
    .product-card:hover::before { opacity: 1; }
    
    .product-img { width: 100%; height: 120px; object-fit: cover; border-radius: 6px; margin-bottom: 8px; border: 1px solid rgba(255,255,255,0.1); transition: 0.3s; }
    .product-card:hover .product-img { transform: scale(1.05); border-color: var(--c-cyan); }
    
    .m-title { font-size: 0.95rem; font-weight: 800; color: #fff; margin-bottom: 3px; text-transform: uppercase; line-height: 1.1; }
    .m-short { font-size: 0.75rem; color: var(--text-muted); line-height: 1.2; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; margin-bottom: 5px; }
    .menu-price { font-family: var(--font-title); font-size: 1.4rem; color: var(--c-cyan); font-weight: bold; margin-top: auto; text-shadow: 0 0 10px rgba(0,188,212,0.3); }

    /* =========================================
       7. MODAIS
       ========================================= */
    .modal { display: none; position: fixed; z-index: 3000; left: 0; top: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); backdrop-filter: blur(8px); align-items: center; justify-content: center; padding: 15px; }
    .modal-content { background: rgba(15,15,20,0.95); border: 1px solid var(--c-pink); border-radius: 12px; max-width: 350px; width: 100%; padding: 20px; position: relative; text-align: center; box-shadow: 0 15px 35px rgba(0,0,0,0.8), inset 0 0 20px rgba(233,30,99,0.1); max-height: 90vh; overflow-y: auto; animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
    @keyframes popIn { from { transform: scale(0.8); opacity: 0; } to { transform: scale(1); opacity: 1; } }
    .close-btn { position: absolute; right: 15px; top: 10px; font-size: 1.8rem; color: #fff; cursor: pointer; line-height: 1; transition: 0.2s; z-index: 10;}
    .close-btn:hover { color: var(--c-red); transform: scale(1.1); }
    
    #modalImg, #eventModalImg { width: 100%; height: 200px; object-fit: cover; border-radius: 8px; border: 1px solid var(--c-cyan); margin-bottom: 12px; }
    #modalTitle { font-size: 1.4rem; font-weight: 900; color: #fff; margin-bottom: 8px; text-transform: uppercase; }
    #modalDesc { font-size: 0.9rem; color: #ccc; line-height: 1.4; margin-bottom: 15px; text-align: left; }
    #modalPrice { font-family: var(--font-title); font-size: 2.2rem; color: var(--c-yellow); margin-bottom: 15px; text-shadow: 0 0 15px rgba(255,235,59,0.4); }

    #eventModalImg { border-color: var(--c-purple); max-height: 250px; }
    #e-title { font-family: var(--font-title); font-size: 2.5rem; color: #fff; line-height: 1;}

    #playerModalImg { width: 100px; height: 100px; border-radius: 50%; border: 3px solid var(--c-yellow); margin: 0 auto 15px; object-fit: cover; }
    #playerQuote { font-style: italic; color: #fff; font-size: 1.1rem; border-left: 3px solid var(--c-pink); padding-left: 10px; margin: 15px 0; background: rgba(255,255,255,0.05); padding: 10px; border-radius: 4px; }

    /* =========================================
       8. LISTAS DE RANKING COM DESTAQUE TOP 3
       ========================================= */
    .leaderboard-list { display: flex; flex-direction: column; gap: 8px; }
    .lb-item { display: flex; align-items: center; padding: 10px; border-radius: 8px; background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.05); transition: 0.2s; }
    .lb-item.clickable:hover { background: rgba(233,30,99,0.1); border-color: var(--c-pink); cursor: pointer; transform: translateX(5px); }
    
    .top3-item { border-color: rgba(255, 235, 59, 0.4); background: rgba(255, 235, 59, 0.05); box-shadow: inset 0 0 10px rgba(255,235,59,0.1); }
    .top3-item:hover { border-color: var(--c-yellow); background: rgba(255, 235, 59, 0.1); }
    .lb-inline-quote { color: #fff; font-style: italic; font-size: 0.8rem; margin-top: 5px; border-left: 2px solid var(--c-pink); padding-left: 6px; display: block; width: 100%;}
    
    .lb-pos { font-family: var(--font-arcade); color: var(--text-muted); font-size: 0.8rem; width: 30px; text-align: center; }
    .lb-ava { width: 35px; height: 35px; border-radius: 50%; object-fit: cover; border: 1px solid var(--c-cyan); margin: 0 10px; background: #111;}
    .lb-info { flex: 1; display: flex; flex-direction: column; }
    .lb-name { font-weight: bold; font-size: 0.95rem; color: #fff; }
    .lb-sub { font-size: 0.75rem; color: var(--c-cyan); }
    .lb-score { font-family: var(--font-title); font-size: 1.3rem; color: var(--c-pink); text-align: right; }
    .lb-prize { font-size: 0.65rem; background: #111; color: var(--c-cyan); padding: 2px 5px; border-radius: 3px; border: 1px solid var(--c-cyan); text-align: center; display: block; margin-top: 3px;}

    /* =========================================
       9. SHOWS E AGENDA MENSAL
       ========================================= */
    .featured-flyer-img { width: 100%; display: block; border-radius: 12px; border: 2px solid var(--c-red); box-shadow: 0 10px 30px rgba(211,47,47,0.5); cursor: pointer; transition: 0.3s; }
    .featured-flyer-img:hover { transform: scale(1.02); box-shadow: 0 10px 40px rgba(233,30,99,0.8); border-color: var(--c-pink); }
    .agenda-mini-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px; margin-top: 15px; }
    .mini-flyer { position: relative; border-radius: 8px; overflow: hidden; aspect-ratio: 1 / 1; cursor: pointer; border: 1px solid var(--glass-border-top); box-shadow: 0 4px 10px rgba(0,0,0,0.5); transition: 0.3s; background-size: cover; background-position: center; }
    .mini-flyer:hover { transform: scale(1.05); border-color: var(--c-cyan); box-shadow: 0 8px 20px rgba(0,188,212,0.4); z-index: 5;}
    .mini-flyer::after { content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: linear-gradient(to top, rgba(0,0,0,0.9) 0%, rgba(0,0,0,0.1) 100%); z-index: 1; }
    .mf-content { position: absolute; bottom: 0; left: 0; width: 100%; padding: 10px; z-index: 2; text-align: center; }
    .mf-date { background: var(--c-red); color: #fff; font-family: var(--font-title); padding: 2px 6px; border-radius: 4px; font-size: 0.85rem; display: inline-block; margin-bottom: 5px; }
    .mf-title { font-family: var(--font-title); font-size: 1.1rem; color: #fff; line-height: 1.1; text-transform: uppercase; text-shadow: 1px 1px 2px #000; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }

    /* =========================================
       10. DIRETORIA, PROMOTERS E CONTATO
       ========================================= */
    .dir-grid { display: flex; justify-content: center; flex-wrap: wrap; gap: 15px; margin-top: 10px;}
    
    /* Efeitos de Destaque na Diretoria */
    .dir-card { flex: 1 1 calc(33% - 15px); min-width: 250px; padding: 25px 15px; text-align: center; transition: all 0.4s ease; border-top: 2px solid transparent;}
    .dir-card:hover { transform: translateY(-8px); border-top-color: var(--card-color, var(--c-cyan)); box-shadow: 0 15px 40px rgba(0,0,0,0.8), 0 0 20px rgba(255,255,255,0.05); z-index: 5;}
    
    .dir-img { width: 100px; height: 100px; border-radius: 50%; object-fit: cover; border: 3px solid var(--card-color, var(--c-cyan)); margin: 0 auto 15px; display: block; box-shadow: 0 0 15px rgba(0,0,0,0.5); transition: all 0.4s ease;}
    .dir-card:hover .dir-img { transform: scale(1.1) rotate(5deg); box-shadow: 0 0 25px var(--card-color, var(--c-cyan)); }
    
    .dir-name { font-family: var(--font-title); font-size: 2rem; color: #fff; line-height: 1; margin-bottom: 5px;}
    .dir-role { font-size: 0.9rem; font-weight: bold; margin-bottom: 5px; text-transform: uppercase; }
    .dir-desc { color: #aaa; font-size: 0.95rem; line-height: 1.5; margin-top: 15px; }

    /* Botão Instagram Destaque */
    .ig-button {
      display: inline-block; background: rgba(0,0,0,0.5); border: 1px solid var(--card-color, var(--c-pink));
      padding: 8px 15px; border-radius: 30px; color: var(--card-color, var(--c-pink)); 
      text-decoration: none; font-family: var(--font-title); font-size: 1.1rem; letter-spacing: 1px;
      transition: all 0.3s ease; margin: 10px 0 5px; box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    }
    .ig-button span { color: #fff; font-family: var(--font-body); font-weight: bold; font-size: 0.85rem; letter-spacing: 0;}
    .ig-button:hover { background: var(--card-color, var(--c-pink)); color: #000 !important; box-shadow: 0 0 20px var(--card-color, var(--c-pink)); transform: scale(1.05); }
    .ig-button:hover span { color: #000 !important; }

    .contact-card { text-align: center; padding: 3rem 1rem; }
    .contact-link { display: flex; align-items: center; justify-content: center; gap: 15px; margin-bottom: 20px; text-decoration: none; color: #fff; font-size: 1.2rem; font-weight: bold; background: rgba(255,255,255,0.05); padding: 15px; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); transition: 0.3s;}
    .contact-link:hover { background: rgba(255,255,255,0.1); border-color: var(--c-cyan); transform: scale(1.02); }

    .map-container { border: 1px solid var(--glass-border-top); border-radius: 12px; overflow: hidden; height: 250px; }
    .map-container iframe { width: 100%; height: 100%; border: 0; filter: contrast(1.2) grayscale(0.2); }

    /* =========================================
       11. PAINEL DE ADMINISTRAÇÃO ACORDEÃO
       ========================================= */
    .admin-login-box { text-align: center; padding: 40px 15px; }
    
    .admin-accordion-btn {
      background: rgba(0,0,0,0.8); color: #fff; cursor: pointer; padding: 15px 20px; width: 100%; 
      border: none; text-align: left; outline: none; font-family: var(--font-title); font-size: 1.5rem; 
      border-left: 4px solid var(--c-pink); border-radius: 8px; margin-bottom: 8px; transition: 0.3s;
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
    }
    .admin-accordion-btn:hover { filter: brightness(1.2); }
    .admin-accordion-btn.active { border-bottom-left-radius: 0; border-bottom-right-radius: 0; margin-bottom: 0; border-bottom: 1px dashed rgba(255,255,255,0.2); }
    
    .admin-accordion-content {
      padding: 20px; display: none; background: rgba(0,0,0,0.6); 
      border-left: 4px solid var(--c-pink); border-bottom-left-radius: 8px; border-bottom-right-radius: 8px; 
      margin-bottom: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.5);
    }

    .admin-card { background: #111; border: 1px solid #333; padding: 15px; border-radius: 8px; margin-bottom: 15px; display: flex; flex-direction: column; gap: 10px; position: relative;}
    .admin-card-row { display: flex; gap: 10px; align-items: center; flex-wrap: wrap; }
    .admin-input { flex: 1; padding: 8px; background: #000; border: 1px solid #444; color: #fff; border-radius: 4px; font-family: var(--font-body); min-width: 120px; }
    .admin-textarea { width: 100%; padding: 8px; background: #000; border: 1px solid #444; color: #fff; border-radius: 4px; font-family: var(--font-body); resize: vertical; }
    
    .file-upload-wrapper { position: relative; overflow: hidden; display: inline-block; cursor: pointer; background: #333; padding: 8px 15px; border-radius: 4px; color: #fff; font-size: 0.85rem; font-weight: bold; text-align: center; flex: 1; border: 1px solid #555;}
    .file-upload-wrapper input[type=file] { font-size: 100px; position: absolute; left: 0; top: 0; opacity: 0; cursor: pointer; }
    
    .action-btn { background: var(--c-cyan); color: #000; border: none; padding: 8px 15px; font-weight: bold; border-radius: 4px; cursor: pointer; transition: 0.2s;}
    .action-btn:hover { filter: brightness(1.2); }
    .del-btn { background: var(--c-red); color: #fff;}

    footer { background: rgba(5,5,8,0.6); backdrop-filter: blur(10px); border-top: 1px solid var(--glass-border-top); padding: 1.5rem 1rem; text-align: center; margin-top: auto; }
    
    @media (max-width: 600px) {
      .tab-btn { flex: 1 1 calc(33.333% - 6px); font-size: 0.95rem; padding: 8px 2px; }
      .product-img { height: 120px; } 
      .dir-card { flex: 1 1 100%; }
      .menu-grid { grid-template-columns: repeat(2, 1fr); gap: 8px; }
      .agenda-mini-grid { grid-template-columns: repeat(2, 1fr); }
      .admin-card-row { flex-direction: column; align-items: stretch; }
      .admin-input { width: 100%; }
    }
  </style>
</head>
<body>

  <!-- EFEITOS DE FUNDO 3D E VIDRO -->
  <div class="stage-lights">
    <div class="spotlight cyan"></div>
    <div class="spotlight yellow"></div>
    <div class="spotlight pink"></div>
  </div>
  <div class="bg-watermark"></div>
  <canvas id="sparks-canvas"></canvas>

  <!-- MODAIS CLIENTE -->
  <div id="productModal" class="modal">
    <div class="modal-content glass-panel">
      <span class="close-btn" onclick="closeModal('productModal')">×</span>
      <img id="modalImg" src="" alt="Produto" onerror="fallbackImg(this)">
      <h3 id="modalTitle">Nome</h3>
      <p id="modalDesc">Descrição</p>
      <div id="modalPrice">R$ 00,00</div>
    </div>
  </div>

  <div id="eventModal" class="modal">
    <div class="modal-content glass-panel" style="border-color: var(--c-cyan);">
      <span class="close-btn" onclick="closeModal('eventModal')">×</span>
      <img id="eventModalImg" src="" alt="Evento" onerror="fallbackImg(this)">
      <div id="e-tag" style="background: var(--c-red); color: #fff; display: inline-block; padding: 5px 10px; font-weight: bold; border-radius: 4px; margin-bottom: 10px;">TAG</div>
      <h3 id="e-title" style="font-family: var(--font-title); font-size: 2.2rem; color: #fff; margin-bottom: 5px; text-transform: uppercase;">TITULO</h3>
      <p id="e-sub" style="color: var(--c-yellow); font-weight: bold; font-size: 1rem; margin-bottom: 15px;">SUBTITULO</p>
      <p id="e-desc" style="color: #ccc; font-size: 1rem; line-height: 1.5; margin-bottom: 20px; text-align: left;">DESCRIÇÃO</p>
    </div>
  </div>

  <div id="playerModal" class="modal">
    <div class="modal-content glass-panel" style="border-color: var(--c-yellow);">
      <span class="close-btn" onclick="closeModal('playerModal')">×</span>
      <img id="playerModalImg" src="" alt="Player" onerror="fallbackImg(this)">
      <h3 id="playerName" style="font-family: var(--font-title); font-size: 2.2rem; color: var(--c-yellow); margin-bottom: 5px;">Nome</h3>
      <a id="playerIg" href="#" target="_blank" style="color: var(--c-cyan); font-weight: bold; text-decoration: none; display: block; margin-bottom: 15px; font-size: 1.1rem;">@instagram</a>
      <div id="playerQuote">"A mensagem de provocação vai aqui."</div>
    </div>
  </div>

  <div class="app-wrapper">
    
    <!-- RÁDIO -->
    <div class="radio-player-bar glass-panel" style="border-radius: 0; border-top: none; border-left: none; border-right: none;">
      <div class="radio-info-wrapper">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRCsoBmqPE2nWj7rFdVVvIETVDsDxHWzIqjU9d7JfjpTQ&s" id="radio-cover" class="radio-cover" alt="Logo">
        <div class="radio-text">
          <div class="radio-station-name">📻 RÁDIO BARGUNÇADOS</div>
          <div class="radio-now-playing" id="now-playing">Aguardando... <span>INTERAJA NA TELA</span></div>
        </div>
      </div>
      <button class="play-pause-btn" id="play-pause-btn">▶ PLAY</button>
      <audio id="rockRadio" preload="none" crossorigin="anonymous">
        <source src="https://stream.radioparadise.com/rock-128" type="audio/mpeg">
      </audio>
    </div>

    <!-- BANNER COM EASTER EGG -->
    <div class="promo-banner">
      <span id="skull-btn" style="cursor:pointer;" ondblclick="triggerEasterEgg()">💀</span> <span id="view-top-promo-banner">SÓ ROCK, CERVEJA E CACHAÇA</span> <span id="skull-btn2" style="cursor:pointer;" ondblclick="triggerEasterEgg()">💀</span>
    </div>

    <header>
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRCsoBmqPE2nWj7rFdVVvIETVDsDxHWzIqjU9d7JfjpTQ&s" alt="Logo" class="logo-img" />
    </header>

    <!-- NAVEGAÇÃO -->
    <div class="nav-container">
      <nav class="nav-tabs">
        <button class="tab-btn active" onclick="switchTab('inicio')">O Bar</button>
        <button class="tab-btn" onclick="switchTab('bebidas')">Bebidas</button>
        <button class="tab-btn" onclick="switchTab('porcoes')">Porções</button>
        <button class="tab-btn" onclick="switchTab('shows')">Agenda</button>
        <button class="tab-btn" onclick="switchTab('fliperama')">Arcade</button>
        <button class="tab-btn" onclick="switchTab('bebedores')">Rankings</button>
        <button class="tab-btn" onclick="switchTab('diretoria')">Diretoria</button>
        <button class="tab-btn" onclick="switchTab('promoters')">Promoter</button>
        <button class="tab-btn" onclick="switchTab('contato')">Contato</button>
        <button class="tab-btn" onclick="switchTab('admin')" style="color: var(--c-red); font-weight: bold; border-color: var(--c-red);">⚙️ ADM</button>
      </nav>
    </div>

    <main>
      
      <!-- INÍCIO DINÂMICO -->
      <section id="inicio" class="tab-pane active">
        <h2 class="section-title">O Templo da Bargunça</h2>
        <div class="grid">
          <div class="glass-panel" style="padding: 1.5rem; margin-bottom: 15px;">
            <img id="home-img" src="" style="display:none; width:100%; border-radius:8px; margin-bottom:15px; border:1px solid rgba(255,255,255,0.1);">
            <h3 id="h-title" style="font-family:var(--font-title); font-size:1.6rem; color:var(--c-cyan); margin-bottom:10px;">SOM PESADO & BEBIDA TRINCANDO</h3>
            <p id="h-desc1" style="color:#ddd; line-height:1.5; margin-bottom:10px;">Bem-vindo ao <strong>Bargunçados Rock Bar</strong>. Aqui a regra é clara: som no talo, copo cheio e zero frescura.</p>
            <p id="h-desc2" style="color:#ddd; line-height:1.5;">O seu novo QG no Pimentas. Tocamos todos os estilos de Rock, do Indie ao Heavy Metal pesado.</p>
          </div>
          <div class="glass-panel" style="padding: 1.5rem; border-color: rgba(211,47,47,0.4);">
            <h3 id="h-promo-title" style="font-family:var(--font-title); font-size:1.6rem; color:var(--c-red); margin-bottom:10px;">🔥 DIVERSÃO GARANTIDA</h3>
            <ul style="list-style:none; line-height:1.6; margin-bottom:15px; color:#fff;">
              <li id="h-promo1">🎸 Agenda de Shows Coberta o Mês Todo</li>
              <li id="h-promo2">🕹️ Fliperama Arcades Liberados</li>
              <li id="h-promo3">🍻 Bebidas Baratas & Porções Brutas</li>
            </ul>
            <button class="btn-fire" onclick="switchTab('shows')">Ver Agenda Mensal</button>
          </div>
        </div>
      </section>

      <!-- BEBIDAS DINÂMICAS -->
      <section id="bebidas" class="tab-pane">
        <h2 class="section-title">Cardápio de Bebidas</h2>
        <p style="text-align: center; color: var(--c-yellow); font-weight: bold; margin-bottom: 15px; font-size: 0.85rem;">👇 CLIQUE NO PRODUTO PARA VER DETALHES 👇</p>
        <div id="dynamic-menu-bebidas"></div>
      </section>

      <!-- PORÇÕES DINÂMICAS -->
      <section id="porcoes" class="tab-pane">
        <h2 class="section-title">Larica Pesada</h2>
        <div id="dynamic-menu-porcoes"></div>
      </section>

      <!-- FLIPERAMA DINÂMICO -->
      <section id="fliperama" class="tab-pane">
        <img src="https://images.unsplash.com/photo-1550745165-9bc0b252726f?q=80&w=1200&auto=format&fit=crop" style="width: 100%; height: 160px; object-fit: cover; border-radius: 8px; border: 1px solid var(--glass-border-top); margin-bottom: 15px;" alt="Fliperama">
        <h2 class="section-title" style="font-size: 1.6rem; color: var(--c-yellow);">🏆 ARCADE RANKING TOP 10</h2>
        <p style="color: #ccc; margin-bottom: 15px; font-size: 0.9rem;">(Clique nos líderes TOP 3 para ver a provocação!)</p>
        <div class="leaderboard-list" id="dynamic-arcade-list"></div>
      </section>

      <!-- CACHACEIROS DINÂMICO -->
      <section id="bebedores" class="tab-pane">
        <h2 class="section-title" style="font-size: 1.6rem; color: var(--c-pink);">🍻 TOP 10 CACHACEIROS</h2>
        <p style="color: #ccc; margin-bottom: 15px; font-size: 0.9rem;">(Clique nos líderes TOP 3 para ver o recado!)</p>
        <div class="leaderboard-list" id="dynamic-cachaceiros-list"></div>
      </section>

      <!-- SHOWS / AGENDA -->
      <section id="shows" class="tab-pane">
        <h2 class="section-title">Agenda do Mês</h2>
        <p style="text-align: center; color: var(--c-yellow); font-weight: bold; margin-bottom: 15px; font-size: 0.85rem;">👇 CLIQUE NO FLYER PARA VER DETALHES 👇</p>
        
        <img src="" id="featured-flyer-img" class="featured-flyer-img" onclick="openAgendaModal('featured')" alt="Evento Destaque">
        
        <h3 style="font-family: var(--font-title); font-size: 1.5rem; color: #fff; margin-top: 2rem; border-bottom: 1px dashed #333; padding-bottom: 5px;">PRÓXIMOS EVENTOS</h3>
        <div id="dynamic-agenda-list" class="agenda-mini-grid"></div>
      </section>

      <!-- DIRETORIA DINÂMICA (Com Destaques de Redes Sociais e Efeitos) -->
      <section id="diretoria" class="tab-pane">
        <h2 class="section-title">A Diretoria</h2>
        <div class="dir-grid" id="dynamic-dir-list"></div>
      </section>

      <!-- PROMOTERS DINÂMICO -->
      <section id="promoters" class="tab-pane">
        <h2 class="section-title">Promoters</h2>
        <div class="dir-grid" id="dynamic-pro-list"></div>
      </section>

      <!-- CONTATO -->
      <section id="contato" class="tab-pane">
        <h2 class="section-title">Contato & Redes</h2>
        <div class="glass-panel contact-card">
          <h3 style="font-family: var(--font-title); font-size: 2.5rem; color: var(--c-pink); margin-bottom: 10px;">BARGUNÇADOS ROCK BAR</h3>
          <p style="color: #ccc; margin-bottom: 25px; font-size: 1rem;">Siga, mande mensagem e faça sua reserva.</p>
          <a href="https://wa.me/5511959427214" target="_blank" class="contact-link" style="border-left: 4px solid #25D366;"><span style="font-size: 1.5rem;">💬</span> WhatsApp: (11) 95942-7214</a>
          <a href="https://instagram.com/barguncadosrockbar" target="_blank" class="contact-link" style="border-left: 4px solid var(--c-pink);"><span style="font-size: 1.5rem;">📸</span> Instagram: @barguncadosrockbar</a>
        </div>
      </section>

      <!-- ÁREA DO ADMINISTRADOR TOTAL MANUAL EM BLOCOS EXPANSÍVEIS (ACORDEÃO) -->
      <section id="admin" class="tab-pane">
        <h2 class="section-title" style="color: var(--c-red);">Painel ADM Completo</h2>
        
        <div id="admin-login" class="glass-panel admin-login-box">
          <h3 style="font-family: var(--font-title); font-size: 2rem; color: #fff; margin-bottom: 10px;">Acesso Restrito</h3>
          <p style="color: #ccc; font-size: 1rem; margin-bottom: 25px;">Insira o E-mail Administrativo para liberar o painel.</p>
          <input type="email" id="manual-email-input" style="width: 100%; max-width: 300px; padding: 12px; font-size: 1rem; border: 1px solid var(--c-cyan); border-radius: 4px; margin-bottom: 15px; background: #000; color: #fff; text-align: center;" placeholder="Digite o E-mail Autorizado" autocomplete="off">
          <br>
          <button class="btn-fire" onclick="processManualLogin()" style="width: 100%; max-width: 300px; background: var(--c-cyan); color: #000;">Acessar Painel</button>
        </div>

        <div id="admin-dashboard" style="display: none;">
          
          <div style="background: rgba(0,255,0,0.1); border: 1px solid #0f0; color: #0f0; padding: 10px; border-radius: 6px; text-align: center; margin-bottom: 20px; font-weight: bold;">
            Acesso Liberado: Painel de Controle Ativo
          </div>

          <!-- ADM INICIO -->
          <button class="admin-accordion-btn" style="border-color: var(--c-cyan);" onclick="toggleAdminSection('adm-inicio')">🏠 Gerenciar Início</button>
          <div id="adm-inicio" class="admin-accordion-content" style="border-color: var(--c-cyan);">
            <div class="admin-card">
              
              <h5 style="color:#fff; margin-top:5px; margin-bottom: 5px;">Frase do Topo (Letreiro)</h5>
              <input type="text" class="admin-input" id="adm-h-promo-banner" placeholder="SÓ ROCK, CERVEJA E CACHAÇA" style="margin-bottom: 15px; border-color: var(--c-yellow);">

              <div class="file-upload-wrapper">
                <span>🖼️ UPLOAD BANNER CENTRAL (Opcional)</span>
                <input type="file" accept="image/*" onchange="updateHomeImg(this)">
              </div>
              <input type="text" class="admin-input" id="adm-h-title" placeholder="Título Principal" style="margin-top:10px;">
              <textarea class="admin-textarea" id="adm-h-desc1" rows="2" placeholder="Parágrafo 1" style="margin-top:10px;"></textarea>
              <textarea class="admin-textarea" id="adm-h-desc2" rows="2" placeholder="Parágrafo 2" style="margin-top:10px;"></textarea>
              
              <h5 style="color:#fff; margin-top:15px;">Destaques (Lista)</h5>
              <input type="text" class="admin-input" id="adm-h-promo-title" placeholder="Título Destaques" style="margin-top:10px;">
              <input type="text" class="admin-input" id="adm-h-promo1" placeholder="Item 1" style="margin-top:10px;">
              <input type="text" class="admin-input" id="adm-h-promo2" placeholder="Item 2" style="margin-top:10px;">
              <input type="text" class="admin-input" id="adm-h-promo3" placeholder="Item 3" style="margin-top:10px;">
              <button class="action-btn" onclick="saveHomeData()" style="margin-top:15px; background: var(--c-cyan);">💾 Salvar Página Inicial</button>
            </div>
          </div>

          <!-- ADM AGENDA -->
          <button class="admin-accordion-btn" style="border-color: var(--c-yellow);" onclick="toggleAdminSection('adm-agenda')">📅 Gerenciar Agenda</button>
          <div id="adm-agenda" class="admin-accordion-content" style="border-color: var(--c-yellow);">
            <div class="admin-card" style="border-color: var(--c-yellow);">
              <h5 style="color:#fff; font-family:var(--font-title); font-size:1.2rem; margin-bottom:5px;">Flyer Destaque (Principal)</h5>
              <div class="file-upload-wrapper">
                <span>🖼️ UPLOAD DO FLYER DESTAQUE</span>
                <input type="file" accept="image/*" onchange="updateFlyerMain(this)">
              </div>
              <div class="admin-card-row" style="margin-top: 10px;">
                <input type="text" class="admin-input" id="adm-f-tag" placeholder="Tag (Ex: SÁBADO)">
                <input type="text" class="admin-input" id="adm-f-title" placeholder="Nome da Banda">
                <input type="text" class="admin-input" id="adm-f-sub" placeholder="Subtítulo">
              </div>
              <textarea class="admin-textarea" id="adm-f-desc" rows="2" placeholder="Descrição"></textarea>
              <button class="action-btn" onclick="saveFlyerData()" style="margin-top: 5px;">💾 Salvar Dados Destaque</button>
            </div>

            <h5 style="color: #fff; font-family: var(--font-title); font-size: 1.2rem; margin-bottom: 10px;">Adicionar Mini Flyer</h5>
            <div class="admin-card" style="border-color: var(--c-cyan);">
              <div class="admin-card-row">
                <input type="text" class="admin-input" id="new-ev-date" placeholder="Data (15/Out)">
                <input type="text" class="admin-input" id="new-ev-title" placeholder="Título">
                <input type="text" class="admin-input" id="new-ev-desc" placeholder="Descrição">
              </div>
              <div class="admin-card-row" style="margin-top: 10px;">
                <div class="file-upload-wrapper">
                  <span>📷 Upload Foto</span>
                  <input type="file" id="new-ev-img" accept="image/*">
                </div>
                <button class="action-btn" onclick="addAgendaItem()">+ ADD Evento</button>
              </div>
            </div>
            
            <h5 style="color: #fff; font-family: var(--font-title); font-size: 1.2rem; margin-bottom: 10px;">Eventos Salvos (Mini Flyers)</h5>
            <div id="admin-agenda-list"></div>
          </div>

          <!-- ADM CARDÁPIO -->
          <button class="admin-accordion-btn" style="border-color: var(--c-pink);" onclick="toggleAdminSection('adm-cardapio')">🍔 Gerenciar Cardápio</button>
          <div id="adm-cardapio" class="admin-accordion-content" style="border-color: var(--c-pink);">
            
            <h5 style="color: #fff; font-family: var(--font-title); font-size: 1.2rem; margin-bottom: 10px;">Novo Item</h5>
            <div class="admin-card" style="border-color: var(--c-pink);">
              <div class="admin-card-row">
                <select class="admin-input" id="new-item-type">
                  <option value="drink">Destilado</option>
                  <option value="beer">Cerveja</option>
                  <option value="noalcohol">Sem Álcool</option>
                  <option value="portion">Porção</option>
                </select>
                <input type="text" class="admin-input" id="new-item-name" placeholder="Nome">
                <input type="text" class="admin-input" id="new-item-price" placeholder="Preço (Ex: 15,00)">
              </div>
              <input type="text" class="admin-input" id="new-item-short" placeholder="Resumo curto..." style="margin-top: 10px;">
              <textarea class="admin-textarea" id="new-item-full" rows="2" placeholder="Descrição completa..." style="margin-top: 10px;"></textarea>
              <div class="admin-card-row" style="margin-top: 10px;">
                <div class="file-upload-wrapper">
                  <span>📷 Upload Foto</span>
                  <input type="file" id="new-item-img" accept="image/*">
                </div>
                <button class="action-btn" onclick="addMenuItem()">+ ADD Item</button>
              </div>
            </div>

            <h5 style="color: #fff; font-family: var(--font-title); font-size: 1.2rem; margin-bottom: 10px;">Itens no Cardápio (Quadradinhos)</h5>
            <div id="admin-menu-list"></div>
          </div>

          <!-- ADM DIRETORIA -->
          <button class="admin-accordion-btn" style="border-color: var(--c-cyan);" onclick="toggleAdminSection('adm-equipe')">👔 Gerenciar Equipe (Diretores & Promoters)</button>
          <div id="adm-equipe" class="admin-accordion-content" style="border-color: var(--c-cyan);">
            <div id="admin-team-list"></div>
          </div>

          <!-- ADM ARCADE -->
          <button class="admin-accordion-btn" style="border-color: var(--c-purple);" onclick="toggleAdminSection('adm-arcade')">🕹️ Gerenciar Arcade Top 10</button>
          <div id="adm-arcade" class="admin-accordion-content" style="border-color: var(--c-purple);">
            <div id="admin-arcade-list"></div>
          </div>

          <!-- ADM CACHACEIROS -->
          <button class="admin-accordion-btn" style="border-color: #ff9800;" onclick="toggleAdminSection('adm-cachaceiros')">🍻 Gerenciar Cachaceiros Top 10</button>
          <div id="adm-cachaceiros" class="admin-accordion-content" style="border-color: #ff9800;">
            <div id="admin-cachaceiros-list"></div>
          </div>

        </div>
      </section>

    </main>

    <footer class="glass-panel" style="border-radius: 0; border-top: 1px solid var(--glass-border-top); padding: 1.5rem 1rem; text-align: center; margin-top: auto; border-bottom: none; border-left: none; border-right: none;">
      <p style="font-size: 1.4rem; font-family: var(--font-title); color: #fff; letter-spacing: 1px; margin-bottom: 5px;">BARGUNÇADOS ROCK BAR © <span id="easter-footer">2026</span></p>
      <p style="color: #aaa; font-size: 0.8rem; margin-bottom: 10px;">Rua Assis Abude, 197 - Guarulhos, SP</p>
      <p style="color: #555; font-size: 0.75rem; font-family: 'Courier New', monospace;">Otimizado via Vanilla JS. ADM Dinâmico com Storage Nativo.</p>
    </footer>

  </div>

  <script>
    // =========================================
    // BLINDADOR DE TEXTOS (Escaping)
    // =========================================
    function escapeHTML(str) {
      if (!str) return '';
      return str.toString().replace(/'/g, "\\'").replace(/"/g, "&quot;");
    }

    // =========================================
    // BANCO DE DADOS LOCAL COMPLETO (Versão _v4)
    // =========================================
    
    // HOME DATA
    const defaultHome = {
      promoBannerText: "SÓ ROCK, CERVEJA E CACHAÇA",
      title: "SOM PESADO & BEBIDA TRINCANDO",
      desc1: "Bem-vindo ao Bargunçados Rock Bar. Aqui a regra é clara: som no talo, copo cheio e zero frescura.",
      desc2: "O seu novo QG no Pimentas. Tocamos todos os estilos de Rock, do Indie ao Heavy Metal pesado.",
      promoTitle: "🔥 DIVERSÃO GARANTIDA",
      promo1: "🎸 Agenda de Shows Coberta o Mês Todo",
      promo2: "🕹️ Fliperama Arcades Liberados",
      promo3: "🍻 Bebidas Baratas & Porções Brutas",
      img: ""
    };

    const defaultMenu = [
      { id: 1, type: 'drink', name: 'Gin Sunset', short: 'Gin, laranja e especiarias.', full: 'Gin artesanal vibrante.', price: '8,00', img: 'https://images.unsplash.com/photo-1609951651556-5334e2706168?q=80&w=400' },
      { id: 2, type: 'drink', name: 'Caipirinha', short: 'Limão taiti, açúcar e cachaça.', full: 'O clássico nacional.', price: '10,00', img: 'https://images.unsplash.com/photo-1513558161293-cdaf765ed2fd?q=80&w=400' },
      { id: 3, type: 'drink', name: 'Caipiroska', short: 'Vodka premium e morango.', full: 'Visual brutal e sabor letal.', price: '14,00', img: 'https://images.unsplash.com/photo-1536935338788-846bb9981813?q=80&w=400' },
      { id: 4, type: 'drink', name: 'Bloody Mary', short: 'Vodka, tomate, pimenta.', full: 'Para curar a ressaca.', price: '18,00', img: 'https://images.unsplash.com/photo-1541544741938-0af808871cc0?q=80&w=400' },
      { id: 5, type: 'drink', name: 'Fenrir Whiskey', short: 'Whiskey on the rocks.', full: 'O combustível dos rockstars.', price: '20,00', img: 'https://images.unsplash.com/photo-1527281400683-1aae777175f8?q=80&w=400' },
      { id: 6, type: 'beer', name: 'Heineken (LN)', short: 'A verdinha trincando.', full: 'Garrafa 330ml gelada ao extremo.', price: '12,00', img: 'https://images.unsplash.com/photo-1600861194942-f883de0dfe96?q=80&w=400' },
      { id: 7, type: 'beer', name: 'Budweiser (LN)', short: 'Lager americana clássica.', full: 'Cerveja redonda e clássica.', price: '10,00', img: 'https://images.unsplash.com/photo-1620228892404-5858b9fbd81b?q=80&w=400' },
      { id: 8, type: 'beer', name: 'Skol Lata', short: 'Pilsen pra descer redondo.', full: 'Lata 350ml trincando.', price: '8,00', img: 'https://images.unsplash.com/photo-1605810230434-7631ac76ec81?q=80&w=400' },
      { id: 9, type: 'noalcohol', name: 'Mocktail', short: 'Morango, limão e soda.', full: 'Sem álcool. Muito gelo.', price: '15,00', img: 'https://images.unsplash.com/photo-1497534446932-c925b458314e?q=80&w=400' },
      { id: 10, type: 'portion', name: 'Batata Cheddar', short: 'Cheddar e bacon.', full: 'Batata rústica com avalanche de cheddar.', price: '36,00', img: 'https://images.unsplash.com/photo-1576107232684-1279f390859f?q=80&w=400' },
      { id: 11, type: 'portion', name: 'Calabresa Fire', short: 'Flambada na cachaça.', full: 'Calabresa com cebola roxa e pão de alho.', price: '38,00', img: 'https://images.unsplash.com/photo-1595295333158-4742f28fbd85?q=80&w=400' },
      { id: 12, type: 'portion', name: 'Tábua Churras', short: 'Iscas de carne e fritas.', full: 'A tábua cooperativa de respeito com chimichurri.', price: '65,00', img: 'https://images.unsplash.com/photo-1555939594-58d7cb561ad1?q=80&w=800' }
    ];

    const defaultAgenda = [
      { id: 1, date: '15/Out', title: 'Quinta Grunge', desc: 'Nirvana, Pearl Jam e Arctic Monkeys.', img: 'https://images.unsplash.com/photo-1514525253161-7a46d19cd819?w=400&q=80' },
      { id: 2, date: '16/Out', title: 'Sexta Classic', desc: 'AC/DC, Guns e Queen.', img: 'https://images.unsplash.com/photo-1459749411175-04bf5292ceea?w=400&q=80' },
      { id: 3, date: '18/Out', title: 'Domingo Acústico', desc: 'Voz e violão. Rock nacional.', img: 'https://images.unsplash.com/photo-1511192336575-5a79af67a629?w=400&q=80' }
    ];

    const defaultTeam = [
      { id: 'dir1', type: 'dir', name: 'TOURU', role: 'TI & Infra', ig: '@Diego_touru', desc: 'O cérebro tecnológico do bar. Responsável por toda a parte técnica, infraestrutura de TI, som digital de alta fidelidade e inovações tecnológicas que mantêm o Bargunçados rodando no talo sem cair a performance.', img: '169819.jpg' },
      { id: 'dir2', type: 'dir', name: 'EDINHO', role: 'Eventos', ig: '@Thrasher_eds', desc: 'O mestre do Marketing e Eventos. A mente estratégica por trás das noites épicas, campanhas de divulgação imbatíveis, parcerias com grandes bandas e a missão constante de lotar a casa todo fim de semana.', img: '169822.jpg' },
      { id: 'dir3', type: 'dir', name: 'ALISSON', role: 'Barman', ig: '@alisson_adler', desc: 'O Barman titular e alquimista das bebidas. Garante os drinks mais brutais e a execução perfeita dos clássicos, controlando o balcão com maestria para que nenhum guerreiro fique de copo vazio.', img: '169823.jpg' },
      { id: 'pro1', type: 'pro', name: 'KELVIN', role: 'Bandas & Mosh', ig: '@jkelvin_guitar', desc: 'O contato direto com a cena underground. Responsável por trazer as melhores bandas cover e autorais da região, agitar as redes sociais e garantir que o mosh pit não pare um segundo em cada evento.', img: 'https://unavatar.io/instagram/jkelvin_guitar' }
    ];

    const defaultArcade = Array.from({length: 10}, (_, i) => ({
      id: i+1, name: i===0?'Rodrigo "KOF-GOD"':`Player ${i+1}`, sub: 'KOF 2002', score: (1000 - i*50)+'K', prize: i===0?'TORRE CHOPP':'', 
      quote: i<3?'Aqui ninguém me ganha, traga sua ficha e chore!':'', ig: '@rocker', img: 'https://ui-avatars.com/api/?name=P'+(i+1)+'&background=e91e63&color=fff'
    }));

    const defaultCachaceiros = Array.from({length: 10}, (_, i) => ({
      id: i+1, name: i===0?'Alex "Tanque"':`Bebedor ${i+1}`, sub: '@rocker', score: (40 - i*2)+' L', prize: i===0?'JACK DANIELS':'', 
      quote: i<3?'Água é pro fígado fraco, desce mais uma dose!':'', img: 'https://ui-avatars.com/api/?name=B'+(i+1)+'&background=00bcd4&color=fff'
    }));

    let currentHome = JSON.parse(localStorage.getItem('brg_home_v4')) || defaultHome;
    let currentMenu = JSON.parse(localStorage.getItem('brg_menu_v4')) || defaultMenu;
    let currentAgenda = JSON.parse(localStorage.getItem('brg_agenda_v4')) || defaultAgenda;
    let currentTeam = JSON.parse(localStorage.getItem('brg_team_v4')) || defaultTeam;
    let currentArcade = JSON.parse(localStorage.getItem('brg_arcade_v4')) || defaultArcade;
    let currentCachaceiros = JSON.parse(localStorage.getItem('brg_cachaceiros_v4')) || defaultCachaceiros;
    let currentFlyer = JSON.parse(localStorage.getItem('brg_flyer_v4')) || { tag: 'SÁBADO INSANO', title: 'BANDA HELLFIRE', sub: 'ESPECIAL METALLICA E SLIPKNOT', desc: 'A noite mais pesada da região. Guitarras distorcidas e mosh pit liberado. 2h de show!', img: 'https://images.unsplash.com/photo-1540039155732-611116238b93?w=800&q=80' };

    // =========================================
    // RENDERIZAÇÃO DOM CLIENTE
    // =========================================
    function renderApp() {
      // HOME CLIENTE
      document.getElementById('view-top-promo-banner').innerText = currentHome.promoBannerText || "SÓ ROCK, CERVEJA E CACHAÇA";
      document.getElementById('h-title').innerText = currentHome.title;
      document.getElementById('h-desc1').innerText = currentHome.desc1;
      document.getElementById('h-desc2').innerText = currentHome.desc2;
      document.getElementById('h-promo-title').innerText = currentHome.promoTitle;
      document.getElementById('h-promo1').innerText = currentHome.promo1;
      document.getElementById('h-promo2').innerText = currentHome.promo2;
      document.getElementById('h-promo3').innerText = currentHome.promo3;
      if(currentHome.img) {
         document.getElementById('home-img').style.display = 'block';
         document.getElementById('home-img').src = currentHome.img;
      } else {
         document.getElementById('home-img').style.display = 'none';
      }

      // MENU
      const drinksContainer = document.getElementById('dynamic-menu-bebidas');
      const portionsContainer = document.getElementById('dynamic-menu-porcoes');
      drinksContainer.innerHTML = ''; portionsContainer.innerHTML = '';
      
      const catHTML = { drink: '<div class="menu-category">🔥 DESTILADOS & CLÁSSICOS</div><div class="menu-grid">', beer: '</div><div class="menu-category">🍺 CERVEJAS</div><div class="menu-grid">', noalcohol: '</div><div class="menu-category">🥤 SEM ÁLCOOL</div><div class="menu-grid">', portion: '<div class="menu-grid">' };
      
      let drinksStr = catHTML.drink;
      currentMenu.filter(i => i.type === 'drink').forEach(i => drinksStr += createCard(i));
      drinksStr += catHTML.beer;
      currentMenu.filter(i => i.type === 'beer').forEach(i => drinksStr += createCard(i));
      drinksStr += catHTML.noalcohol;
      currentMenu.filter(i => i.type === 'noalcohol').forEach(i => drinksStr += createCard(i));
      drinksStr += '</div>';
      drinksContainer.innerHTML = drinksStr;

      let portionsStr = catHTML.portion;
      currentMenu.filter(i => i.type === 'portion').forEach(i => portionsStr += createCard(i));
      portionsStr += '</div>';
      portionsContainer.innerHTML = portionsStr;

      // AGENDA MENSAL CLIENTE
      const agendaList = document.getElementById('dynamic-agenda-list');
      agendaList.innerHTML = '';
      currentAgenda.forEach(ev => {
        const fallimg = ev.img || 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=400&q=80';
        agendaList.innerHTML += `
          <div class="mini-flyer" style="background-image: url('${fallimg}');" onclick="openEventModalFromList('${escapeHTML(ev.title)}', '${escapeHTML(ev.date)}', '${escapeHTML(ev.desc)}', '${fallimg}')">
            <div class="mf-content">
              <div class="mf-date">${ev.date}</div>
              <div class="mf-title">${ev.title}</div>
            </div>
          </div>`;
      });

      // FLYER PRINCIPAL
      document.getElementById('featured-flyer-img').src = currentFlyer.img;
      document.getElementById('adm-f-tag').value = currentFlyer.tag;
      document.getElementById('adm-f-title').value = currentFlyer.title;
      document.getElementById('adm-f-sub').value = currentFlyer.sub;
      document.getElementById('adm-f-desc').value = currentFlyer.desc;

      // DIRETORIA E PROMOTERS CLIENTE (Textos completos & Efeitos Adicionados)
      const dirContainer = document.getElementById('dynamic-dir-list');
      const proContainer = document.getElementById('dynamic-pro-list');
      dirContainer.innerHTML = ''; proContainer.innerHTML = '';
      
      currentTeam.filter(t => t.type === 'dir').forEach(t => {
        let cColor = 'var(--c-pink)';
        if(t.id === 'dir2') cColor = 'var(--c-cyan)';
        if(t.id === 'dir3') cColor = 'var(--c-yellow)';
        
        dirContainer.innerHTML += `
          <div class="glass-panel dir-card" style="--card-color: ${cColor};">
            <img src="${t.img}" class="dir-img" alt="${t.name}" onerror="fallbackImg(this)">
            <h3 class="dir-name">${t.name}</h3>
            <p class="dir-role" style="color:${cColor};">${t.role}</p>
            <a href="https://instagram.com/${t.ig.replace('@','')}" class="ig-button" style="border-color:${cColor}; color:${cColor};" target="_blank">📸 INSTAGRAM: <span style="color:#fff;">${t.ig}</span></a>
            <p class="dir-desc">${t.desc}</p>
          </div>`;
      });
      
      currentTeam.filter(t => t.type === 'pro').forEach(t => {
        proContainer.innerHTML += `
          <div class="glass-panel dir-card" style="width:100%; max-width:450px; margin:0 auto; padding:2rem; --card-color: var(--c-purple);">
            <img src="${t.img}" class="dir-img" alt="${t.name}" style="width:130px; height:130px;" onerror="fallbackImg(this)">
            <h3 class="dir-name" style="font-size:2.5rem;">${t.name}</h3>
            <p class="dir-role" style="color:var(--c-purple); font-size:1rem;">${t.role}</p>
            <a href="https://instagram.com/${t.ig.replace('@','')}" class="ig-button" style="border-color:var(--c-purple); color:var(--c-purple); font-size: 1.1rem;" target="_blank">📸 INSTAGRAM: <span style="color:#fff;">${t.ig}</span></a>
            <p class="dir-desc" style="font-size:1rem; margin-top:15px;">${t.desc}</p>
          </div>`;
      });

      // RANKINGS CLIENTE (TOP 1, 2, E 3 FUNCIONANDO COM MENSAGEM)
      document.getElementById('dynamic-arcade-list').innerHTML = currentArcade.map((i, index) => {
        const isTop3 = index < 3;
        const safeQuote = i.quote || (isTop3 ? 'Traga sua ficha e tente a sorte!' : '');
        const safeName = escapeHTML(i.name);
        const safeIg = escapeHTML(i.ig || '@jogador');
        const safeQuoteStr = escapeHTML(safeQuote);
        
        const clickHTML = isTop3 ? `onclick="openPlayerModal('${safeName}', '${safeIg}', '${safeQuoteStr}', '${i.img}')"` : '';
        
        return `
        <div class="lb-item ${isTop3 ? 'top3-item clickable' : ''}" ${clickHTML}>
          <span class="lb-pos" style="${isTop3 ? 'color:var(--c-yellow); font-size:1rem;' : ''}">#${i.id}</span>
          ${isTop3 ? `<img class="lb-ava" src="${i.img}" onerror="fallbackImg(this)">` : ''}
          <div class="lb-info">
            <span class="lb-name">${i.name}</span>
            <span class="lb-sub">${i.sub}</span>
            ${isTop3 && safeQuote ? `<span class="lb-inline-quote">"${safeQuote}"</span>` : ''}
          </div>
          <div><div class="lb-score">${i.score}</div>${i.prize ? `<span class="lb-prize">${i.prize}</span>` : ''}</div>
        </div>`;
      }).join('');

      document.getElementById('dynamic-cachaceiros-list').innerHTML = currentCachaceiros.map((i, index) => {
        const isTop3 = index < 3;
        const safeQuote = i.quote || (isTop3 ? 'Água é pro fígado fraco, desce mais uma dose!' : '');
        const safeName = escapeHTML(i.name);
        const safeSub = escapeHTML(i.sub || '@jogador');
        const safeQuoteStr = escapeHTML(safeQuote);
        
        const clickHTML = isTop3 ? `onclick="openPlayerModal('${safeName}', '${safeSub}', '${safeQuoteStr}', '${i.img}')"` : '';
        
        return `
        <div class="lb-item ${isTop3 ? 'top3-item clickable' : ''}" ${clickHTML}>
          <span class="lb-pos" style="${isTop3 ? 'color:var(--c-yellow); font-size:1rem;' : ''}">#${i.id}</span>
          <img class="lb-ava" src="${i.img}" onerror="fallbackImg(this)">
          <div class="lb-info">
            <span class="lb-name">${i.name}</span>
            <span class="lb-sub">${i.sub}</span>
            ${isTop3 && safeQuote ? `<span class="lb-inline-quote">"${safeQuote}"</span>` : ''}
          </div>
          <div><div class="lb-score">${i.score}</div>${i.prize ? `<span class="lb-prize">${i.prize}</span>` : ''}</div>
        </div>`;
      }).join('');

      renderAdminLists();
    }

    function createCard(item) {
      const sName = escapeHTML(item.name);
      const sFull = escapeHTML(item.full);
      return `
        <div class="product-card glass-panel" onclick="openMenuModalObj('${sName}', '${sFull}', '${item.price}', '${item.img}')">
          <img src="${item.img}" class="product-img" onerror="fallbackImg(this)">
          <h4 class="m-title">${item.name}</h4>
          <p class="m-short">${item.short}</p>
          <div class="menu-price">R$ ${item.price}</div>
        </div>`;
    }

    // =========================================
    // ADMIN ACORDEÃO E LISTAS (Quadradinhos)
    // =========================================
    function toggleAdminSection(id) {
      const content = document.getElementById(id);
      const btn = content.previousElementSibling;
      if (content.style.display === "block") {
        content.style.display = "none";
        btn.classList.remove("active");
      } else {
        content.style.display = "block";
        btn.classList.add("active");
      }
    }

    function renderAdminLists() {
      // Menu Admin
      const menuList = document.getElementById('admin-menu-list');
      if(menuList) {
        menuList.innerHTML = currentMenu.map(i => `
          <div class="admin-card">
            <div class="admin-card-row">
                <input type="text" class="admin-input" id="name-${i.id}" value="${i.name}">
                <input type="text" class="admin-input" id="price-${i.id}" value="${i.price}" style="max-width:80px;">
            </div>
            <div class="admin-card-row" style="margin-top:5px;">
                <input type="text" class="admin-input" id="short-${i.id}" value="${i.short}">
            </div>
            <textarea class="admin-textarea" id="full-${i.id}" rows="2" style="margin-top:5px;">${i.full}</textarea>
            <div class="admin-card-row" style="margin-top:5px;">
                <img src="${i.img}" style="width:40px; height:40px; object-fit:cover; border-radius:4px; border:1px solid #444;">
                <div class="file-upload-wrapper" style="padding: 5px 10px;">
                  <span style="font-size:0.75rem;">📷 Upar Nova Foto</span>
                  <input type="file" accept="image/*" onchange="updateItemImg(${i.id}, this)">
                </div>
                <button class="action-btn" onclick="saveItemData(${i.id})">💾 Salvar</button>
                <button class="action-btn del-btn" onclick="removeMenuItem(${i.id})">🗑️</button>
            </div>
          </div>`).join('');
      }
      
      // Agenda Admin
      const agendaList = document.getElementById('admin-agenda-list');
      if(agendaList) {
        agendaList.innerHTML = currentAgenda.map(i => `
          <div class="admin-card">
            <div class="admin-card-row">
              <input type="text" class="admin-input" id="a-date-${i.id}" value="${i.date}" style="max-width:80px;">
              <input type="text" class="admin-input" id="a-title-${i.id}" value="${i.title}">
            </div>
            <textarea class="admin-textarea" id="a-desc-${i.id}" rows="2" style="margin-top:5px;">${i.desc}</textarea>
            <div class="admin-card-row" style="margin-top:5px;">
              <img src="${i.img || 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=400&q=80'}" style="width:40px; height:40px; object-fit:cover; border-radius:4px;">
              <div class="file-upload-wrapper" style="padding: 5px 10px;">
                  <span style="font-size:0.75rem;">📷 Upar Mini Flyer</span>
                  <input type="file" accept="image/*" onchange="updateAgendaImg(${i.id}, this)">
              </div>
              <button class="action-btn" onclick="saveAgendaData(${i.id})">💾</button>
              <button class="action-btn del-btn" onclick="removeAgendaItem(${i.id})">🗑️</button>
            </div>
          </div>`).join('');
      }

      // Equipe Admin
      const teamList = document.getElementById('admin-team-list');
      if(teamList) {
        teamList.innerHTML = currentTeam.map(t => `
          <div class="admin-card">
            <strong style="color:var(--c-cyan);">${t.name} (${t.type.toUpperCase()})</strong>
            <div class="admin-card-row">
              <input type="text" class="admin-input" id="tname-${t.id}" value="${t.name}" placeholder="Nome">
              <input type="text" class="admin-input" id="trole-${t.id}" value="${t.role}" placeholder="Cargo">
            </div>
            <input type="text" class="admin-input" id="tig-${t.id}" value="${t.ig}" placeholder="Instagram" style="margin-top:5px;">
            <textarea class="admin-textarea" id="tdesc-${t.id}" rows="2" style="margin-top:5px;">${t.desc}</textarea>
            <div class="admin-card-row" style="margin-top:5px;">
                <img src="${t.img}" style="width:40px; height:40px; object-fit:cover; border-radius:4px; border:1px solid #444;">
                <div class="file-upload-wrapper" style="padding: 5px 10px;">
                  <span style="font-size:0.75rem;">📷 Upar Avatar</span>
                  <input type="file" accept="image/*" onchange="updateTeamImg('${t.id}', this)">
                </div>
                <button class="action-btn" onclick="saveTeamData('${t.id}')">💾 Salvar</button>
            </div>
          </div>`).join('');
      }

      // Arcade Admin
      const arcList = document.getElementById('admin-arcade-list');
      if(arcList) {
        arcList.innerHTML = currentArcade.map(i => `
          <div class="admin-card" style="border-color:var(--c-purple);">
            <strong style="color:var(--c-purple);">Posição #${i.id} ${i.id <= 3 ? '(TOP 3)' : ''}</strong>
            <div class="admin-card-row">
              <input type="text" class="admin-input" id="arc-name-${i.id}" value="${i.name}" placeholder="Nome">
              <input type="text" class="admin-input" id="arc-sub-${i.id}" value="${i.sub}" placeholder="Jogo">
            </div>
            <div class="admin-card-row" style="margin-top:5px;">
              <input type="text" class="admin-input" id="arc-score-${i.id}" value="${i.score}" placeholder="Score">
              <input type="text" class="admin-input" id="arc-prize-${i.id}" value="${i.prize}" placeholder="Prêmio (vazio=sem prêmio)">
            </div>
            <div class="admin-card-row" style="margin-top:5px;">
              <input type="text" class="admin-input" id="arc-ig-${i.id}" value="${i.ig}" placeholder="Instagram (@)">
            </div>
            <textarea class="admin-textarea" id="arc-quote-${i.id}" rows="1" placeholder="Mensagem de Provocação..." style="margin-top:5px;">${i.quote}</textarea>
            <div class="admin-card-row" style="margin-top:5px;">
              ${i.id <= 3 ? `<img src="${i.img}" style="width:40px; height:40px; object-fit:cover; border-radius:4px; border:1px solid #444;">
              <div class="file-upload-wrapper" style="padding: 5px 10px;">
                <span style="font-size:0.75rem;">📷 Avatar Top 3</span>
                <input type="file" accept="image/*" onchange="updateArcadeImg(${i.id}, this)">
              </div>` : ''}
              <button class="action-btn" onclick="saveArcadeData(${i.id})">💾 Salvar Jogador</button>
            </div>
          </div>`).join('');
      }

      // Cachaceiros Admin
      const cachList = document.getElementById('admin-cachaceiros-list');
      if(cachList) {
        cachList.innerHTML = currentCachaceiros.map(i => `
          <div class="admin-card" style="border-color:#ff9800;">
            <strong style="color:#ff9800;">Posição #${i.id} ${i.id <= 3 ? '(TOP 3)' : ''}</strong>
            <div class="admin-card-row">
              <input type="text" class="admin-input" id="ca-name-${i.id}" value="${i.name}" placeholder="Nome">
              <input type="text" class="admin-input" id="ca-score-${i.id}" value="${i.score}" placeholder="Litros">
            </div>
            <div class="admin-card-row" style="margin-top:5px;">
              <input type="text" class="admin-input" id="ca-sub-${i.id}" value="${i.sub}" placeholder="Instagram (@)">
              <input type="text" class="admin-input" id="ca-prize-${i.id}" value="${i.prize}" placeholder="Prêmio (vazio=sem prêmio)">
            </div>
            <textarea class="admin-textarea" id="ca-quote-${i.id}" rows="1" placeholder="Mensagem de Provocação..." style="margin-top:5px;">${i.quote}</textarea>
            <div class="admin-card-row" style="margin-top:5px;">
              <img src="${i.img}" style="width:40px; height:40px; object-fit:cover; border-radius:4px;">
              <div class="file-upload-wrapper" style="padding: 5px 10px;">
                  <span style="font-size:0.75rem;">📷 Upar Avatar</span>
                  <input type="file" accept="image/*" onchange="updateCachaceiroImg(${i.id}, this)">
              </div>
              <button class="action-btn" onclick="saveCachaceiroData(${i.id})">💾 Salvar Bebedor #${i.id}</button>
            </div>
          </div>`).join('');
      }
    }

    // =========================================
    // FUNÇÕES DE ADMINISTRAÇÃO MANUAL
    // =========================================
    function processManualLogin() {
      const email = document.getElementById('manual-email-input').value.trim();
      if(email === 'diegotouru666@gmail.com') {
        document.getElementById('admin-login').style.display = 'none';
        document.getElementById('admin-dashboard').style.display = 'block';
        document.getElementById('manual-email-input').value = '';
        alert('Acesso Concedido. Bem-vindo ao painel da Diretoria!');
      } else {
        alert('Acesso Negado. E-mail não autorizado ou incorreto.');
      }
    }

    function handleImageUpload(file, callback) {
      if (!file) return;
      const reader = new FileReader();
      reader.onloadend = function() { callback(reader.result); }
      reader.readAsDataURL(file);
    }

    let tempNewItemImg = 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=400&q=80';
    let tempNewEvImg = 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=400&q=80';
    
    document.getElementById('new-item-img').addEventListener('change', e => { handleImageUpload(e.target.files[0], res => { tempNewItemImg = res; alert('Foto carregada!'); }); });
    document.getElementById('new-ev-img').addEventListener('change', e => { handleImageUpload(e.target.files[0], res => { tempNewEvImg = res; alert('Mini Flyer carregado!'); }); });

    function updateHomeImg(input) { handleImageUpload(input.files[0], res => { currentHome.img = res; saveToStorage(); alert('Banner Inicial Atualizado!'); }); }
    function updateFlyerMain(input) { handleImageUpload(input.files[0], res => { currentFlyer.img = res; saveToStorage(); alert('Flyer Destaque Atualizado!'); }); }
    
    function updateItemImg(id, input) { handleImageUpload(input.files[0], res => { const item = currentMenu.find(i => i.id === id); if(item) { item.img = res; saveToStorage(); alert('Imagem do produto salva!'); } }); }
    function updateAgendaImg(id, input) { handleImageUpload(input.files[0], res => { const item = currentAgenda.find(i => i.id === id); if(item) { item.img = res; saveToStorage(); alert('Mini Flyer atualizado!'); } }); }
    function updateTeamImg(id, input) { handleImageUpload(input.files[0], res => { const t = currentTeam.find(i => i.id === id); if(t) { t.img = res; saveToStorage(); alert('Avatar atualizado!'); } }); }
    function updateCachaceiroImg(id, input) { handleImageUpload(input.files[0], res => { const t = currentCachaceiros.find(i => i.id === id); if(t) { t.img = res; saveToStorage(); alert('Avatar atualizado!'); } }); }
    function updateArcadeImg(id, input) { handleImageUpload(input.files[0], res => { const t = currentArcade.find(i => i.id === id); if(t) { t.img = res; saveToStorage(); alert('Avatar Arcade atualizado!'); } }); }

    function saveHomeData() {
      currentHome.promoBannerText = document.getElementById('adm-h-promo-banner').value;
      currentHome.title = document.getElementById('adm-h-title').value;
      currentHome.desc1 = document.getElementById('adm-h-desc1').value;
      currentHome.desc2 = document.getElementById('adm-h-desc2').value;
      currentHome.promoTitle = document.getElementById('adm-h-promo-title').value;
      currentHome.promo1 = document.getElementById('adm-h-promo1').value;
      currentHome.promo2 = document.getElementById('adm-h-promo2').value;
      currentHome.promo3 = document.getElementById('adm-h-promo3').value;
      saveToStorage(); alert('Dados do Início Salvos!');
    }

    function saveItemData(id) {
       const item = currentMenu.find(i => i.id === id);
       if(item) {
          item.name = document.getElementById(`name-${id}`).value;
          item.price = document.getElementById(`price-${id}`).value;
          item.short = document.getElementById(`short-${id}`).value;
          item.full = document.getElementById(`full-${id}`).value;
          saveToStorage(); alert('Produto Salvo!');
       }
    }

    function saveAgendaData(id) {
       const item = currentAgenda.find(i => i.id === id);
       if(item) {
          item.date = document.getElementById(`a-date-${id}`).value;
          item.title = document.getElementById(`a-title-${id}`).value;
          item.desc = document.getElementById(`a-desc-${id}`).value;
          saveToStorage(); alert('Evento Salvo!');
       }
    }

    function saveTeamData(id) {
       const t = currentTeam.find(i => i.id === id);
       if(t) {
          t.name = document.getElementById(`tname-${id}`).value;
          t.role = document.getElementById(`trole-${id}`).value;
          t.ig = document.getElementById(`tig-${id}`).value;
          t.desc = document.getElementById(`tdesc-${id}`).value;
          saveToStorage(); alert('Membro salvo!');
       }
    }

    function saveArcadeData(id) {
       const t = currentArcade.find(i => i.id === id);
       if(t) {
          t.name = document.getElementById(`arc-name-${id}`).value;
          t.sub = document.getElementById(`arc-sub-${id}`).value;
          t.score = document.getElementById(`arc-score-${id}`).value;
          t.prize = document.getElementById(`arc-prize-${id}`).value;
          t.ig = document.getElementById(`arc-ig-${id}`).value;
          t.quote = document.getElementById(`arc-quote-${id}`).value;
          saveToStorage(); alert('Jogador salvo!');
       }
    }

    function saveCachaceiroData(id) {
       const t = currentCachaceiros.find(i => i.id === id);
       if(t) {
          t.name = document.getElementById(`ca-name-${id}`).value;
          t.sub = document.getElementById(`ca-sub-${id}`).value;
          t.score = document.getElementById(`ca-score-${id}`).value;
          t.prize = document.getElementById(`ca-prize-${id}`).value;
          t.quote = document.getElementById(`ca-quote-${id}`).value;
          saveToStorage(); alert('Cachaceiro salvo!');
       }
    }

    function addMenuItem() {
      const name = document.getElementById('new-item-name').value;
      const price = document.getElementById('new-item-price').value;
      if(!name || !price) return alert("Preencha nome e preço.");
      currentMenu.push({
        id: Date.now(), type: document.getElementById('new-item-type').value,
        name: name, short: document.getElementById('new-item-short').value,
        full: document.getElementById('new-item-full').value, price: price, img: tempNewItemImg
      });
      saveToStorage(); alert('Produto Adicionado ao Cardápio!');
    }

    function addAgendaItem() {
      const title = document.getElementById('new-ev-title').value;
      const date = document.getElementById('new-ev-date').value;
      if(!title || !date) return alert("Preencha data e título.");
      currentAgenda.push({ id: Date.now(), date: date, title: title, desc: document.getElementById('new-ev-desc').value, img: tempNewEvImg });
      saveToStorage(); alert('Evento Adicionado!');
    }

    function removeMenuItem(id) { if(confirm('Apagar item do cardápio?')) { currentMenu = currentMenu.filter(i => i.id !== id); saveToStorage(); } }
    function removeAgendaItem(id) { if(confirm('Apagar evento da agenda?')) { currentAgenda = currentAgenda.filter(i => i.id !== id); saveToStorage(); } }

    function saveFlyerData() {
      currentFlyer.tag = document.getElementById('adm-f-tag').value;
      currentFlyer.title = document.getElementById('adm-f-title').value;
      currentFlyer.sub = document.getElementById('adm-f-sub').value;
      currentFlyer.desc = document.getElementById('adm-f-desc').value;
      saveToStorage(); alert("Dados do Destaque Salvos!");
    }

    function saveToStorage() {
      localStorage.setItem('brg_home_v3', JSON.stringify(currentHome));
      localStorage.setItem('brg_menu_v3', JSON.stringify(currentMenu));
      localStorage.setItem('brg_agenda_v3', JSON.stringify(currentAgenda));
      localStorage.setItem('brg_team_v3', JSON.stringify(currentTeam));
      localStorage.setItem('brg_arcade_v3', JSON.stringify(currentArcade));
      localStorage.setItem('brg_cachaceiros_v3', JSON.stringify(currentCachaceiros));
      localStorage.setItem('brg_flyer_v3', JSON.stringify(currentFlyer));
      renderApp();
    }

    // =========================================
    // MODAIS E INTERAÇÕES DO CLIENTE
    // =========================================
    function openMenuModalObj(name, desc, price, img) {
      document.getElementById('modalTitle').innerText = name.replace('PROMO', '').trim();
      document.getElementById('modalDesc').innerText = desc;
      document.getElementById('modalPrice').innerText = 'R$ ' + price;
      document.getElementById('modalImg').src = img;
      document.getElementById('productModal').style.display = 'flex';
    }

    function openAgendaModal(type) {
      document.getElementById('e-tag').innerText = currentFlyer.tag;
      document.getElementById('e-title').innerText = currentFlyer.title;
      document.getElementById('e-sub').innerText = currentFlyer.sub;
      document.getElementById('e-desc').innerText = currentFlyer.desc;
      document.getElementById('eventModalImg').src = currentFlyer.img;
      document.getElementById('eventModal').style.display = 'flex';
    }

    function openEventModalFromList(title, date, desc, img) {
      document.getElementById('e-tag').innerText = date;
      document.getElementById('e-title').innerText = title;
      document.getElementById('e-sub').innerText = "Agenda Mensal";
      document.getElementById('e-desc').innerText = desc;
      document.getElementById('eventModalImg').src = img;
      document.getElementById('eventModal').style.display = 'flex';
    }

    function openPlayerModal(name, ig, quote, imgUrl) {
      document.getElementById('playerName').innerText = name;
      document.getElementById('playerIg').innerText = ig;
      document.getElementById('playerIg').href = "https://instagram.com/" + ig.replace('@','');
      document.getElementById('playerQuote').innerText = `"${quote}"`;
      document.getElementById('playerModalImg').src = imgUrl;
      document.getElementById('playerModal').style.display = 'flex';
    }

    function closeModal(modalId) { document.getElementById(modalId).style.display = 'none'; }
    window.onclick = function(event) { 
      if (event.target == document.getElementById('productModal')) closeModal('productModal'); 
      if (event.target == document.getElementById('eventModal')) closeModal('eventModal'); 
      if (event.target == document.getElementById('playerModal')) closeModal('playerModal');
    }

    function fallbackImg(el) { el.onerror = null; el.src = 'https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=400&q=80'; }

    // =========================================
    // RÁDIO AUTOPLAY BYPASS
    // =========================================
    const audio = document.getElementById('rockRadio');
    const playBtn = document.getElementById('play-pause-btn');
    const cover = document.getElementById('radio-cover');

    function forceAutoplay() {
      if(audio.paused) {
        audio.volume = 0.5;
        audio.play().then(() => {
          playBtn.innerHTML = '⏸ PAUSE'; cover.style.animationPlayState = 'running';
          ['click', 'touchstart', 'scroll', 'keydown'].forEach(evt => document.removeEventListener(evt, forceAutoplay));
        }).catch(() => {});
      }
    }
    forceAutoplay();
    ['click', 'touchstart', 'scroll', 'keydown'].forEach(evt => document.addEventListener(evt, forceAutoplay, { once: true }));

    playBtn.addEventListener('click', (e) => {
      e.stopPropagation(); 
      if(audio.paused) { audio.play(); playBtn.innerHTML = '⏸ PAUSE'; cover.style.animationPlayState = 'running'; } 
      else { audio.pause(); playBtn.innerHTML = '▶ PLAY'; cover.style.animationPlayState = 'paused'; }
    });

    async function fetchRadioData() {
      try {
        const response = await fetch('https://api.radioparadise.com/api/now_playing?chan=2');
        const data = await response.json();
        document.getElementById('now-playing').innerHTML = `TOCANDO: <span>${data.artist} - ${data.title}</span>`;
      } catch (err) { document.getElementById('now-playing').innerHTML = `TOCANDO: <span>ROCK MIX</span>`; }
    }
    setInterval(fetchRadioData, 15000); fetchRadioData();

    // =========================================
    // NAVEGAÇÃO 3D CUBE TABS
    // =========================================
    function switchTab(tabId) {
      document.querySelectorAll('.tab-pane').forEach(tab => { tab.classList.remove('active'); void tab.offsetWidth; });
      document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
      document.getElementById(tabId).classList.add('active');
      const clickedBtn = Array.from(document.querySelectorAll('.tab-btn')).find(btn => btn.getAttribute('onclick').includes(tabId));
      if (clickedBtn) clickedBtn.classList.add('active');
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    // =========================================
    // EASTER EGGS EXCLUSIVOS
    // =========================================
    function triggerEasterEgg() {
      alert("🔥 Easter Egg Encontrado! Você ativou o modo Grand Master. +50 AP liberado no MegaMU!");
      document.getElementById('skull-btn').innerText = "⚔️";
      document.getElementById('skull-btn2').innerText = "⚔️";
    }
    document.getElementById('easter-footer').addEventListener('dblclick', () => alert("Desenvolvido com ódio, café e magia de Soul Wizard."));

    // =========================================
    // CANVAS SPARKS LEVE
    // =========================================
    const canvas = document.getElementById('sparks-canvas');
    const ctx = canvas.getContext('2d');
    let width, height;
    function resize() { width = canvas.width = window.innerWidth; height = canvas.height = window.innerHeight; }
    window.addEventListener('resize', resize); resize();
    class Ember {
      constructor() { this.reset(true); }
      reset(initial = false) {
        this.x = Math.random() * width; this.y = initial ? Math.random() * height : height + Math.random() * 50;
        this.size = Math.random() * 2 + 0.5; this.speedY = Math.random() * 1.5 + 0.5; this.speedX = (Math.random() - 0.5);
        this.opacity = Math.random() * 0.8 + 0.2; this.fade = Math.random() * 0.015 + 0.005;
        const colors = ['233, 30, 99', '0, 188, 212', '255, 235, 59'];
        this.color = colors[Math.floor(Math.random() * colors.length)];
      }
      update() {
        this.y -= this.speedY; this.x += this.speedX; this.opacity -= this.fade;
        if (this.opacity <= 0 || this.y < -10) this.reset();
      }
      draw() {
        ctx.beginPath(); ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${this.color}, ${this.opacity})`; ctx.fill();
      }
    }
    const embers = Array.from({ length: window.innerWidth < 768 ? 20 : 40 }, () => new Ember());
    function animateEmbers() {
      ctx.clearRect(0, 0, width, height);
      embers.forEach(ember => { ember.update(); ember.draw(); });
      requestAnimationFrame(animateEmbers);
    }
    animateEmbers();

    // Inicializa a UI com os dados Locais
    renderApp();
  </script>
</body>
</html>
