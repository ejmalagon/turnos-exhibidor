Esta encuesta se ha diseñado para saber en qué horarios estás disponible para el exhibidor, DEBES ESCRIBIR TU NOMBRE Y EL DE TUS COMPAÑEROS que estarán en ese turno.
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Turnos campaña exhibidor 12 de julio</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
    }

    .turno {
      margin: 10px;
      padding: 10px;
      border: 1px solid black;
      display: inline-block;
      cursor: pointer;
      white-space: pre-line;
      width: 200px;
    }

    .ocupado {
      background-color: lightgray;
      cursor: not-allowed;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      z-index: 999;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0, 0, 0, 0.5);
    }

    .modal-content {
      background-color: #fff;
      margin: 10% auto;
      padding: 20px;
      border: 1px solid #888;
      width: 300px;
      text-align: left;
      border-radius: 10px;
    }

    .modal-content input {
      width: 100%;
      padding: 8px;
      margin-top: 10px;
      margin-bottom: 10px;
    }

    .modal-content button {
      padding: 8px 16px;
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>Campaña de exhibidores del 12 de julio.</h1>
  <h2>Selecciona tu turno y escribe los nombres de las personas que estarán en ese turno.</h2>
  <div id="turnos"></div>

  <!-- Modal -->
  <div id="turnoModal" class="modal">
    <div class="modal-content">
      <h3>Confirmar Turno</h3>
      <p id="turnoSeleccionadoTexto"></p>
      <input type="text" id="nombreInput" placeholder="Escribe los nombres aquí">
      <br>
      <button id="confirmarBtn">Confirmar</button>
      <button id="cancelarBtn">Cancelar</button>
    </div>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getDatabase, ref, get, set, onValue } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyBnm4eifYOOoZ_H03Q0IOCmCs2E1ARPKQ0",
      authDomain: "exhibidores-37a1e.firebaseapp.com",
      databaseURL: "https://exhibidores-37a1e-default-rtdb.firebaseio.com/",
      projectId: "exhibidores-37a1e",
      storageBucket: "exhibidores-37a1e.appspot.com",
      messagingSenderId: "832454027212",
      appId: "1:832454027212:web:50dabadc51b3a559145f69",
      measurementId: "G-2BZD17QEL0"
    };

    const app = initializeApp(firebaseConfig);
    const database = getDatabase(app);

    const turnos = [
      { hora: '7:00 - 9:00', punto: 'Punto Tibabuyes' },
      { hora: '9:00 - 11:00', punto: 'Punto Tibabuyes' },
      { hora: '11:00 - 1:00 p.m.', punto: 'Punto Tibabuyes' },
      { hora: '1:00 - 3:00 p.m.', punto: 'Punto Tibabuyes' },
      { hora: '3:00 - 5:00 p.m.', punto: 'Punto Tibabuyes' },
      { hora: '5:00 - 7:00 p.m.', punto: 'Punto Tibabuyes' },
      { hora: '7:00 - 9:00', punto: 'Punto Afidro' },
      { hora: '9:00 - 11:00', punto: 'Punto Afidro' },
      { hora: '11:00 - 1:00 p.m.', punto: 'Punto Afidro' },
      { hora: '1:00 - 3:00 p.m.', punto: 'Punto Afidro' },
      { hora: '3:00 - 5:00 p.m.', punto: 'Punto Afidro' },
      { hora: '5:00 - 7:00 p.m.', punto: 'Punto Afidro' },
      { hora: '7:00 - 9:00', punto: 'Punto Yaiti' },
      { hora: '9:00 - 11:00', punto: 'Punto Yaiti' },
      { hora: '11:00 - 1:00 p.m.', punto: 'Punto Yaiti' },
      { hora: '1:00 - 3:00 p.m.', punto: 'Punto Yaiti' },
      { hora: '3:00 - 5:00 p.m.', punto: 'Punto Yaiti' },
      { hora: '5:00 - 7:00 p.m.', punto: 'Punto Yaiti' },
    ];

    let turnoActualSeleccionado = null;

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
            div.onclick = () => abrirModal(index);
          }

          turnosContainer.appendChild(div);
        });
      });
    }

    function abrirModal(index) {
      turnoActualSeleccionado = index;
      document.getElementById("turnoSeleccionadoTexto").innerText = `${turnos[index].hora} - ${turnos[index].punto}`;
      document.getElementById("nombreInput").value = "";
      document.getElementById("turnoModal").style.display = "block";
    }

    function cerrarModal() {
      document.getElementById("turnoModal").style.display = "none";
      document.getElementById("nombreInput").value = "";
      turnoActualSeleccionado = null;
    }

    // Confirmar selección
    document.getElementById("confirmarBtn").onclick = () => {
      const nombres = document.getElementById("nombreInput").value.trim();
      if (!nombres) {
        alert("Debes escribir los nombres de las dos o tres personas que estarán en el turno.");
        return;
      }

      const turnoRef = ref(database, `turnosOcupados/${turnoActualSeleccionado}`);
      get(turnoRef).then(snapshot => {
        if (!snapshot.exists()) {
          set(turnoRef, nombres).then(() => {
            cerrarModal();
            alert("Turno registrado con éxito.");
          });
        } else {
          cerrarModal();
          alert("Este turno ya ha sido ocupado.");
        }
      });
    };

    // Cancelar selección
    document.getElementById("cancelarBtn").onclick = () => {
      cerrarModal();
    };

    onValue(ref(database, "turnosOcupados"), cargarTurnos);
  </script>
</body>
</html>
