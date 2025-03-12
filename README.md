# Turnos campaña exhibidor 22 de marzo 
<!DOCTYPE html>
Esta encuesta se ha diseñado para saber en qué horarios está disponible para el exhibidor, puede diligenciar la encuesta de manera individual para que se le asigne una pareja, o si lo desea puede diligenciar la encuesta con otro publicador. 

<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selecciona tu Turno</title>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .turno { margin: 10px; padding: 10px; border: 1px solid black; display: inline-block; cursor: pointer; }
        .ocupado { background-color: lightgray; cursor: not-allowed; }
    </style>
</head>
<body>
    <h2>Selecciona tu Turno</h2>
    <input type="text" id="nombre" placeholder="Tu Nombre" required>
    <div id="turnos"></div>

    <script>
       const firebaseConfig = {
  apiKey: "AIzaSyBnm4eifYOOoZ_H03Q0IOCmCs2E1ARPKQ0",
  authDomain: "exhibidores-37a1e.firebaseapp.com",
  projectId: "exhibidores-37a1e",
  storageBucket: "exhibidores-37a1e.firebasestorage.app",
  messagingSenderId: "832454027212",
  appId: "1:832454027212:web:50dabadc51b3a559145f69",
  measurementId: "G-2BZD17QEL0"
};

        
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        const turnos = [
            { hora: '07:00 - 09:00', punto: 'Punto 1' },
            { hora: '09:00 - 11:00', punto: 'Punto 1' },
            { hora: '11:00 - 13:00', punto: 'Punto 1' },
            { hora: '07:00 - 09:00', punto: 'Punto 2' },
            { hora: '09:00 - 11:00', punto: 'Punto 2' },
            { hora: '11:00 - 13:00', punto: 'Punto 2' },
            { hora: '07:00 - 09:00', punto: 'Punto 3' },
            { hora: '09:00 - 11:00', punto: 'Punto 3' },
            { hora: '11:00 - 13:00', punto: 'Punto 3' }
        ];

        function cargarTurnos() {
            const turnosContainer = document.getElementById("turnos");
            turnosContainer.innerHTML = "";
            
            database.ref("turnosOcupados").once("value", snapshot => {
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

        function seleccionarTurno(index) {
            const nombre = document.getElementById("nombre").value.trim();
            if (!nombre) {
                alert("Por favor, ingresa tu nombre antes de seleccionar un turno.");
                return;
            }
            
            database.ref(`turnosOcupados/${index}`).once("value", snapshot => {
                if (!snapshot.exists()) {
                    database.ref(`turnosOcupados/${index}`).set(nombre);
                    cargarTurnos();
                    alert("Turno seleccionado exitosamente.");
                } else {
                    alert("Este turno ya ha sido ocupado.");
                }
            });
        }

        database.ref("turnosOcupados").on("value", cargarTurnos);
    </script>
</body>
</html>
