# Turnos campaña exhibidor 22 de marzo 
Esta encuesta se ha diseñado para saber en qué horarios estás disponible para el exhibidor, debes escribir tu nombre y el de los compañeros que estarán en ese turno. 

<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selecciona tu Turno</title>
    <script type="module">
        // Importar Firebase 9+
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
        import { getDatabase, ref, get, set, onValue } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

        // Configuración de Firebase (reemplaza con tus credenciales)
        const firebaseConfig = {
  apiKey: "AIzaSyBnm4eifYOOoZ_H03Q0IOCmCs2E1ARPKQ0",
  authDomain: "exhibidores-37a1e.firebaseapp.com",
   databaseURL: "https://exhibidores-37a1e-default-rtdb.firebaseio.com/",
  projectId: "exhibidores-37a1e",
  storageBucket: "exhibidores-37a1e.firebasestorage.app",
  messagingSenderId: "832454027212",
  appId: "1:832454027212:web:50dabadc51b3a559145f69",
  measurementId: "G-2BZD17QEL0"
};

       // Inicializar Firebase
        const app = initializeApp(firebaseConfig);
        const database = getDatabase(app);

        // Lista de turnos disponibles
        const turnos = [
            { hora: '07:00 - 09:00', punto: 'Punto Tibabuyes' },
            { hora: '09:00 - 11:00', punto: 'Punto Tibabuyes' },
            { hora: '11:00 - 1:00 p.m.', punto: 'Punto Tibabuyes' },
            { hora: '1:00 - 3:00 p.m.', punto: 'Punto Tibabuyes' },
            { hora: '3:00 - 5:00 p.m.', punto: 'Punto Tibabuyes' },
            { hora: '5:00 - 7:00 p.m.', punto: 'Punto Tibabuyes' },
            { hora: '07:00 - 09:00', punto: 'Punto Afidro' },
            { hora: '09:00 - 11:00', punto: 'Punto Afidro' },
            { hora: '11:00 - 1:00 p.m.', punto: 'Punto Afidro' },
            { hora: '1:00 - 3:00 p.m.', punto: 'Punto Afidro' },
            { hora: '3:00 - 5:00 p.m.', punto: 'Punto Afidro' },
            { hora: '5:00 - 7:00 p.m.', punto: 'Punto Afidro' },
            { hora: '07:00 - 09:00', punto: 'Punto Yaiti' },
            { hora: '09:00 - 11:00', punto: 'Punto Yaiti' },
            { hora: '11:00 - 1:00 p.m.', punto: 'Punto Yaiti' },
            { hora: '1:00 - 3:00 p.m.', punto: 'Punto Yaiti' },
            { hora: '3:00 - 5:00 p.m.', punto: 'Punto Yaiti' },
            { hora: '5:00 - 7:00 p.m.', punto: 'Punto Yaiti' },
            { hora: '07:00 - 09:00', punto: 'Punto Adicional 1' },
        ];

        // Cargar los turnos y mostrar los ocupados
        function cargarTurnos() {
            const turnosContainer = document.getElementById("turnos");
            turnosContainer.innerHTML = "";

            const turnosRef = ref(database, "turnosOcupados");

            get(turnosRef).then(snapshot => {
                const turnosOcupados = snapshot.val() || {};

                turnos.forEach((turno, index) => {
                    const div = document.createElement("div");
                    div.className = "turno";
                    div.innerText = `${turno.hora} - ${turno.punto}`;

                    if (turnosOcupados[index]) {
                        div.classList.add("ocupado");
                        div.innerText += `\nOcupado por: ${turnosOcupados[index]}`;
                    } else {
                        div.onclick = () => seleccionarTurno(index);
                    }

                    turnosContainer.appendChild(div);
                });
            });
        }

        // Seleccionar un turno
        function seleccionarTurno(index) {
            const nombre = document.getElementById("nombre").value.trim();
            if (!nombre) {
                alert("Por favor, ingresa tu nombre antes de seleccionar un turno.");
                return;
            }

            const turnoRef = ref(database, `turnosOcupados/${index}`);

            get(turnoRef).then(snapshot => {
                if (!snapshot.exists()) {
                    set(turnoRef, nombre).then(() => {
                        alert("Turno seleccionado exitosamente.");
                    });
                } else {
                    alert("Este turno ya ha sido ocupado.");
                }
            });
        }

        // Escuchar cambios en tiempo real
        onValue(ref(database, "turnosOcupados"), cargarTurnos);
    </script>

    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .turno { margin: 10px; padding: 10px; border: 1px solid black; display: inline-block; cursor: pointer; }
        .ocupado { background-color: lightgray; cursor: not-allowed; }
    </style>
</head>
<body>
    <h2>Selecciona tu Turno</h2>
    <input type="text" id="nombre" placeholder="Escribe los nombres" required>
    <div id="turnos"></div>
</body>
</html>
