<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Turnos campaña exhibidor</title>
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
    #tablaTurnos th, #tablaTurnos td {
  border: 1px solid black;
  padding: 8px;
  text-align: center;
}
#tablaTurnos th {
  background-color: #f2f2f2;
}
  </style>
</head>
<body>
  <h1>Campaña de exhibidores del lunes 13 de octubre</h1>
<p>Elige el horario que te funcione y escribe los nombres de dos o tres personas que estarán presentes en ese turno (puedes incluirte).</p>
<p>⚠️ Importante: los turnos que tengan solo un nombre serán eliminados.</p>
  <div id="turnos"></div>
<h2>Tabla de turnos ocupados</h2>
<table id="tablaTurnos" border="1" style="margin:auto; border-collapse: collapse; width: 90%;">
  <thead>
    <tr>
      <th>Hora</th>
      <th>Punto</th>
      <th>Personas asignadas</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

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
      { hora: '07:00 - 09:00', punto: 'Tibabuyes' },
      { hora: '09:00 - 11:00', punto: 'Tibabuyes' },
      { hora: '11:00 - 1:00 p.m.', punto: 'Tibabuyes' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Tibabuyes' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Tibabuyes' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Tibabuyes' },
      { hora: '07:00 - 09:00', punto: 'Afidro' },
      { hora: '09:00 - 11:00', punto: 'Afidro' },
      { hora: '11:00 - 1:00 p.m.', punto: 'Afidro' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Afidro' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Afidro' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Afidro' },
      { hora: '07:00 - 09:00', punto: 'Yaiti' },
      { hora: '09:00 - 11:00', punto: 'Yaiti' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Yaiti' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Yaiti' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Yaiti' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Yaiti' },
      { hora: '07:00 - 09:00', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '09:00 - 11:00', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '07:00 - 09:00', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '09:00 - 11:00', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
    ];

    let turnoActualSeleccionado = null;

    function cargarTurnos() {
  const turnosContainer = document.getElementById("turnos");
  const tablaBody = document.querySelector("#tablaTurnos tbody");

  turnosContainer.innerHTML = "";
  tablaBody.innerHTML = "";

  const turnosRef = ref(database, "turnosOcupados");

  get(turnosRef).then(snapshot => {
    const turnosOcupados = snapshot.val() || {};

    turnos.forEach((turno, index) => {
      // -------- Bloques visuales --------
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

      // -------- Tabla --------
      const fila = document.createElement("tr");
      const celdaHora = document.createElement("td");
      const celdaPunto = document.createElement("td");
      const celdaPersonas = document.createElement("td");

      celdaHora.textContent = turno.hora;
      celdaPunto.textContent = turno.punto;
 if (turnosOcupados[index]) {
        celdaPersonas.textContent = turnosOcupados[index];
        celdaPersonas.style.color = "red";  
      } else {
        celdaPersonas.textContent = "Disponible";
        celdaPersonas.style.color = "green";
      }

      fila.appendChild(celdaHora);
      fila.appendChild(celdaPunto);
      fila.appendChild(celdaPersonas);
      tablaBody.appendChild(fila);
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
