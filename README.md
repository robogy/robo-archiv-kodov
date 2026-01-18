# robo-archiv-kodov
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.css"/>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.js"></script>

<style>
    /* Základný obal pre celú Hero sekciu */
    .hero-wrapper {
        position: relative;
        width: 100%;
        height: 450px !important;
        background-color: #000;
        overflow: hidden;
    }

    #panorama {
        width: 100%;
        height: 100%;
        opacity: 1 !important;
    }

    /* Vaša funkčná "opona" (náhľad) */
    #custom-overlay {
        position: absolute;
        top: 0; left: 0;
        width: 100%; height: 100%;
        background: url('https://samko.robcentratest.h10s.eu/wp-content/uploads/2026/01/lomnica-360-hero-300x150.webp') no-repeat center center;
        background-size: cover;
        z-index: 100;
        opacity: 1;
        transition: opacity 3s ease-in-out;
        pointer-events: none;
    }

    /* VRSTVA PRE TEXT - s vysokou prioritou */
    #hero-text-layer {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 999; /* Musí byť nad všetkým */
        text-align: center;
        width: 100%;
        opacity: 0;
        visibility: hidden;
        transition: opacity 2s ease-in-out, visibility 2s;
        pointer-events: none; /* Umožňuje točiť panorámou aj cez text */
    }

    #hero-text-layer h2 {
        color: #ffffff !important;
        font-family: sans-serif;
        font-size: 2.5rem !important;
        font-weight: bold !important;
        text-shadow: 2px 2px 15px rgba(0,0,0,0.9) !important;
        margin: 0;
        padding: 0 20px;
        display: block !important;
    }

    /* Trieda, ktorú pridá JavaScript pre zobrazenie */
    .show-content {
        opacity: 1 !important;
        visibility: visible !important;
    }

    /* Skrytie originálnych Pannellum prvkov */
    .pnlm-load-box, .pnlm-preview-bg { 
        display: none !important; 
    }
</style>

<div class="hero-wrapper">
    <div id="panorama"></div>
    <div id="custom-overlay"></div>
    
    <div id="hero-text-layer">
        <h2>Vstupte do virtuálnej perspektívy</h2>
    </div>
</div>

<script>
    // Inicializácia Pannellum
    var viewer = pannellum.viewer('panorama', {
        "type": "equirectangular",
        "panorama": "https://samko.robcentratest.h10s.eu/wp-content/uploads/2026/01/lomnica-360-hero.webp",
        "autoLoad": true,
        "autoRotate": -20,
        "autoRotateInactivityDelay": 3000,
        "showControls": true
    });

    // Udalosť po načítaní scény
    viewer.on('load', function() {
        // 1. Postupné zmiznutie náhľadu
        var overlay = document.getElementById('custom-overlay');
        if (overlay) {
            overlay.style.opacity = '0';
            setTimeout(function() { 
                overlay.style.display = 'none'; 
            }, 3000);
        }

        // 2. Postupné vynorenie textu s malým oneskorením
        setTimeout(function() {
            var textLayer = document.getElementById('hero-text-layer');
            if (textLayer) {
                textLayer.classList.add('show-content');
            }
        }, 1500); 
    });

    // Poistka pre frontend - vynútenie prekreslenia
    window.addEventListener('load', function() {
        if(viewer) viewer.resize();
    });
</script>
