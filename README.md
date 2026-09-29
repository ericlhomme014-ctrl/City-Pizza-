<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Carte de Fidélité - City Pizza</title>
    <!-- Simulation d'une icône pour l'écran d'accueil -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #121212;
            color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 10px;
            box-sizing: border-box;
        }
        .card {
            background: linear-gradient(135deg, #1c1c1c 0%, #0d0d0d 100%);
            border-radius: 20px;
            width: 100%;
            max-width: 340px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.7);
            border: 1px solid #333;
            position: relative;
            box-sizing: border-box;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 15px;
        }
        .logo-area h1 {
            margin: 0;
            font-size: 22px;
            color: #ff3333;
            font-style: italic;
            font-weight: bold;
        }
        .logo-area p {
            margin: 2px 0 0;
            font-size: 9px;
            color: #aaa;
        }
        .address {
            text-align: right;
            font-size: 8px;
            color: #888;
            line-height: 1.2;
        }
        .banner-img {
            width: 100%;
            height: 75px;
            background: url('https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=600&q=80') center/cover;
            border-radius: 10px;
            margin-bottom: 12px;
        }
        .card-title {
            text-align: center;
            font-size: 15px;
            font-weight: bold;
            font-style: italic;
            letter-spacing: 1px;
            margin-bottom: 12px;
            color: #ddd;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }
        .stamp {
            aspect-ratio: 1;
            border-radius: 50%;
            background-color: #2a2a2a;
            border: 2px solid #444;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 13px;
            font-weight: bold;
            color: #777;
            position: relative;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        .stamp.validated {
            background-color: #1a1a1a;
            border-color: #ff3333;
            color: #fff;
        }
        .stamp.validated::after {
            content: '✓';
            position: absolute;
            top: -4px;
            right: -4px;
            background: #ff3333;
            color: white;
            font-size: 9px;
            width: 15px;
            height: 15px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .promo-banner {
            background-color: #e51937;
            color: white;
            text-align: center;
            padding: 8px;
            border-radius: 8px;
            font-weight: bold;
            font-size: 12px;
            transform: rotate(-1deg);
            margin-bottom: 15px;
            box-shadow: 0 4px 10px rgba(229, 25, 55, 0.4);
        }
        .actions {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .btn {
            background-color: #000;
            color: white;
            border: 1px solid #444;
            width: 100%;
            padding: 10px;
            border-radius: 10px;
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
            text-align: center;
            box-sizing: border-box;
        }
        .btn-admin {
            background-color: #222;
            color: #aaa;
            font-size: 11px;
            border: 1px dashed #555;
            padding: 6px;
        }
    </style>
</head>
<body>

    <div class="card">
        <div class="header">
            <div class="logo-area">
                <h1>City Pizza</h1>
                <p>Le goût de l'Italie à Angoulême</p>
            </div>
            <div class="address">
                11 rue de Genève,<br>16000 Angoulême
            </div>
        </div>

        <div class="banner-img"></div>

        <div class="card-title">Carte de fidélité</div>

        <!-- Grille des 10 tampons -->
        <div class="grid" id="stampGrid">
            <!-- Généré par JavaScript -->
        </div>

        <div class="promo-banner">
            AU BOUT DE 10 PIZZAS<br>= 1 PIZZA OFFERTE !
        </div>

        <div class="actions">
            <button class="btn" onclick="installApp()">📲 Ajouter à l'écran d'accueil</button>
            <button class="btn btn-admin" onclick="adminAction()">🔒 Mode Pizzaiolo (Ajouter un tampon)</button>
        </div>
    </div>

    <script>
        // Gestion des tampons sauvegardés dans le téléphone du client
        let stamps = JSON.parse(localStorage.getItem('cityPizzaStamps')) || 2; // Par défaut 2 pour l'exemple

        function renderGrid() {
            const grid = document.getElementById('stampGrid');
            grid.innerHTML = '';
            for (let i = 1; i <= 10; i++) {
                const div = document.createElement('div');
                div.className = `stamp ${i <= stamps ? 'validated' : ''}`;
                div.innerText = i;
                grid.appendChild(div);
            }
        }

        // Fonction pour que le commerçant ajoute un tampon (sécurisé par code PIN simple, ex: "1234")
        function adminAction() {
            let pin = prompt("Entrez le code PIN du pizzaiolo :");
            if (pin === "1234") { // Vous pourrez changer ce code
                if (stamps < 10) {
                    stamps++;
                    localStorage.setItem('cityPizzaStamps', stamps);
                    renderGrid();
                    alert("Tampon ajouté avec succès !");
                } else {
                    if(confirm("La carte est pleine (10 pizzas) ! Réinitialiser la carte pour offrir la pizza ?")) {
                        stamps = 0;
                        localStorage.setItem('cityPizzaStamps', stamps);
                        renderGrid();
                    }
                }
            } else if (pin !== null) {
                alert("Code PIN incorrect.");
            }
        }

        // Message indicatif pour l'ajout à l'écran d'accueil
        function installApp() {
            alert("Pour installer la carte : \n- Sur iPhone : Appuyez sur le bouton de partage de Safari puis 'Sur l'écran d'accueil'.\n- Sur Android : Appuyez sur les 3 points du navigateur puis 'Ajouter à l'écran d'accueil'.");
        }

        // Initialisation de l'affichage au chargement
        renderGrid();
    </script>

</body>
</html>
