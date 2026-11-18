
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Coleta de Dados Meteorológicos</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, Helvetica, sans-serif;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #0f172a, #164e63);
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 25px;
            color: #1e293b;
        }

        .container {
            width: 100%;
            max-width: 480px;
            background: #ffffff;
            padding: 35px;
            border-radius: 18px;
            box-shadow: 0 15px 45px rgba(0,0,0,0.25);
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .icon {
            font-size: 48px;
            margin-bottom: 15px;
        }

        h1 {
            font-size: 25px;
            color: #0f172a;
            margin-bottom: 10px;
        }

        .subtitle {
            color: #64748b;
            font-size: 14px;
            line-height: 1.6;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-weight: bold;
            font-size: 14px;
            margin-bottom: 8px;
            color: #334155;
        }

        input {
            width: 100%;
            padding: 14px;
            border: 1px solid #cbd5e1;
            border-radius: 9px;
            font-size: 15px;
            outline: none;
            transition: 0.3s;
        }

        input:focus {
            border-color: #0891b2;
            box-shadow: 0 0 0 3px rgba(8,145,178,0.12);
        }

        .location-btn {
            width: 100%;
            padding: 13px;
            background: #e0f2fe;
            color: #0369a1;
            border: none;
            border-radius: 9px;
            font-weight: bold;
            cursor: pointer;
            margin-bottom: 25px;
            transition: 0.3s;
        }

        .location-btn:hover {
            background: #bae6fd;
        }

        .submit-btn {
            width: 100%;
            padding: 15px;
            background: #0891b2;
            color: white;
            border: none;
            border-radius: 9px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
        }

        .submit-btn:hover {
            background: #0e7490;
        }

        .submit-btn:disabled {
            opacity: 0.6;
            cursor: wait;
        }

        .status {
            margin-top: 18px;
            padding: 12px;
            border-radius: 8px;
            font-size: 13px;
            text-align: center;
            display: none;
            line-height: 1.5;
        }

        .status.success {
            display: block;
            background: #dcfce7;
            color: #166534;
        }

        .status.error {
            display: block;
            background: #fee2e2;
            color: #991b1b;
        }

        .footer {
            text-align: center;
            margin-top: 25px;
            color: #94a3b8;
            font-size: 12px;
        }

        @media (max-width: 480px) {
            .container {
                padding: 25px 20px;
            }

            h1 {
                font-size: 22px;
            }
        }
    </style>
</head>

<body>

    <main class="container">

        <header class="header">
            <div class="icon">🌤️</div>

            <h1>Coleta de Dados Meteorológicos</h1>

            <p class="subtitle">
                Informe a localização para registrar os
                dados geográficos da coleta.
            </p>
        </header>

        <!-- Formulário integrado ao Google Forms -->
        <form
            id="weatherForm"
            action="https://docs.google.com/forms/d/e/1FAIpQLSc_FqQw0pq1i7iShMcOR9n4bZcNmt_ifBzMESSwUIm8egn8oA/formResponse"
            method="POST"
            target="hiddenFrame"
        >

            <div class="form-group">
                <label for="local">Nome do Local *</label>

                <input
                    type="text"
                    id="local"
                    name="entry.315772519"
                    placeholder="Ex: Campinas - SP"
                    required
                >
            </div>

            <button
                type="button"
                class="location-btn"
                id="gpsButton"
            >
                📍 Obter minha localização automaticamente
            </button>

            <div class="form-group">
                <label for="latitude">Latitude *</label>

                <input
                    type="number"
                    step="any"
                    id="latitude"
                    name="entry.1115222331"
                    placeholder="Ex: -22.9056"
                    min="-90"
                    max="90"
                    required
                >
            </div>

            <div class="form-group">
                <label for="longitude">Longitude *</label>

                <input
                    type="number"
                    step="any"
                    id="longitude"
                    name="entry.1367989623"
                    placeholder="Ex: -47.0608"
                    min="-180"
                    max="180"
                    required
                >
            </div>

            <button
                type="submit"
                class="submit-btn"
                id="submitButton"
            >
                Enviar informações
            </button>

            <div id="status" class="status" role="status" aria-live="polite"></div>

        </form>

        <!-- Recebe a resposta sem sair da página -->
        <iframe
            name="hiddenFrame"
            id="hiddenFrame"
            style="display:none"
        ></iframe>

        <footer class="footer">
            Coleta de Dados Meteorológicos
            <br>
            Registro de localização geográfica
        </footer>

    </main>

    <script>

        const form = document.getElementById("weatherForm");

        const gpsButton = document.getElementById("gpsButton");

        const submitButton = document.getElementById("submitButton");

        const status = document.getElementById("status");

        const localInput = document.getElementById("local");

        const latitudeInput = document.getElementById("latitude");

        const longitudeInput = document.getElementById("longitude");


        // Obtém localização pelo GPS do dispositivo

        gpsButton.addEventListener("click", function() {

            if (!navigator.geolocation) {

                mostrarMensagem(
                    "Seu navegador não suporta geolocalização.",
                    "error"
                );

                return;
            }

            gpsButton.disabled = true;
            gpsButton.textContent = "Obtendo localização...";

            navigator.geolocation.getCurrentPosition(

                function(position) {

                    const latitude = position.coords.latitude;

                    const longitude = position.coords.longitude;

                    latitudeInput.value = latitude.toFixed(6);

                    longitudeInput.value = longitude.toFixed(6);

                    mostrarMensagem(
                        "Localização obtida com sucesso!",
                        "success"
                    );

                    gpsButton.disabled = false;

                    gpsButton.textContent =
                        "📍 Localização obtida";

                },

                function(error) {

                    let mensagem = "Não foi possível obter sua localização.";

                    if (error.code === 1) {
                        mensagem = "Permita o acesso à localização no navegador.";
                    }

                    if (error.code === 2) {
                        mensagem = "Localização indisponível no momento.";
                    }

                    if (error.code === 3) {
                        mensagem = "Tempo esgotado ao buscar localização.";
                    }

                    mostrarMensagem(mensagem, "error");

                    gpsButton.disabled = false;

                    gpsButton.textContent =
                        "📍 Tentar novamente";

                },

                {
                    enableHighAccuracy: true,
                    timeout: 15000,
                    maximumAge: 0
                }

            );

        });


        // Envio dos dados para o Google Forms

        form.addEventListener("submit", function(event) {

            const latitude = Number(latitudeInput.value);

            const longitude = Number(longitudeInput.value);

            if (
                !Number.isFinite(latitude) ||
                latitude < -90 ||
                latitude > 90
            ) {
                event.preventDefault();

                mostrarMensagem("Informe uma latitude válida.", "error");

                return;
            }

            if (
                !Number.isFinite(longitude) ||
                longitude < -180 ||
                longitude > 180
            ) {
                event.preventDefault();

                mostrarMensagem("Informe uma longitude válida.", "error");

                return;
            }

            submitButton.disabled = true;

            submitButton.textContent = "Enviando...";

            mostrarMensagem(
                "Enviando informações para o Google Forms...",
                "success"
            );

            // Aguarda o envio ao iframe antes de liberar o formulário.
            document.getElementById("hiddenFrame").onload = function() {

                mostrarMensagem(
                    "Solicitação enviada! Verifique o registro no Google Forms.",
                    "success"
                );

                submitButton.disabled = false;

                submitButton.textContent = "Enviar informações";

                form.reset();

                gpsButton.textContent =
                    "📍 Obter minha localização automaticamente";

            };

        });


        // Exibe mensagens na tela

        function mostrarMensagem(mensagem, tipo) {

            status.textContent = mensagem;

            status.className = "status " + tipo;

        }

    </script>

</body>
</html>
