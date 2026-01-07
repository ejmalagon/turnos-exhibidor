<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Turnos campaña exhibidor</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      text-align: center;
      padding: 10px;
    }

    h2 {
      margin-top: 40px;
    }

    #turnos {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }

    .turno {
      margin: 10px;
      padding: 12px;
      width: 210px;
      border-radius: 10px;
      background: #ffffff;
      border: 2px solid #ddd;
      cursor: pointer;
      white-space: pre-line;
      transition: all 0.3s ease;
      box-shadow: 0 2px 6px rgba(0,0,0,0.08);
    }

    .turno:hover {
      transform: translateY(-3px);
      box-shadow: 0 4px 10px rgba(0,0,0,0.15);
    }

/* Turno seleccionado (aún no confirmado) */
.turno.seleccionado {
  background-color: #e0e0e0;
  border-color: #777;
}

/* Turno ocupado (confirmado) */
.turno.ocupado {
  background-color: #d4f8d4;
  border-color: #2e7d32;
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
      background-color: rgba(0, 0, 0, 0.5);
    }

    .modal-content {
      background-color: #fff;
      margin: 12% auto;
      padding: 20px;
      width: 320px;
      border-radius: 12px;
      text-align: left;
    }

    .modal-content input {
      width: 100%;
      padding: 8px;
      margin: 10px 0;
    }

    .modal-content button {
      padding: 8px 14px;
      border-radius: 5px;
      border: none;
      cursor: pointer;
    }

    #confirmarBtn {
      background: #2e7d32;
      color: white;
    }

    #cancelarBtn {
      background: #ccc;
    }

    /* Tabla */
    #tablaTurnos {
      margin: auto;
      border-collapse: collapse;
      width: 90%;
      background: white;
    }

    #tablaTurnos th,
    #tablaTurnos td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: center;
    }

    #tablaTurnos th {
      background: #f2f2f2;
    }
  </style>
</head>

<body>

  <h1>Asignación de turnos – Campaña exhibidor</h1>

  <div id="turnos"></div>

  <h2>Tabla de turnos ocupados</h2>
  <table id="tablaTurnos">
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
      <input type="text" id="nombreInput" placeholder="Nombres de las personas">
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
      appId: "1:832454027212:web:50dabadc51b3a559145f69"
    };

    const app = initializeApp(firebaseConfig);
    const database = getDatabase(app);

    const turnos = [
      { hora: '07:00 - 09:00', punto: 'Tibabuyes' },
      { hora: '09:00 - 11:00', punto: 'Tibabuyes' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Tibabuyes' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Tibabuyes' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Tibabuyes' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Tibabuyes' },

      { hora: '07:00 - 09:00', punto: 'Afidro' },
      { hora: '09:00 - 11:00', punto: 'Afidro' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Afidro' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Afidro' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Afidro' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Afidro' },
    ];

    let turnoActualSeleccionado = null;

    function cargarTurnos() {
      const contenedor = document.getElementById("turnos");
      const tablaBody = document.querySelector("#tablaTurnos tbody");

      contenedor.innerHTML = "";
      tablaBody.innerHTML = "";

      const refTurnos = ref(database, "turnosOcupados");

      get(refTurnos).then(snapshot => {
        const ocupados = snapshot.val() || {};

        turnos.forEach((turno, index) => {
          const div = document.createElement("div");
          div.className = "turno";
          div.innerText = `${turno.hora}\n${turno.punto}`;

          if (ocupados[index]) {
            div.classList.add("ocupado");
            div.innerText += `\nOcupado`;
          } else {
            div.onclick = () => {
              document.querySelectorAll(".turno").forEach(t => t.classList.remove("seleccionado"));
              div.classList.add("seleccionado");
              abrirModal(index);
            };
          }

          contenedor.appendChild(div);

          const fila = document.createElement("tr");
          fila.innerHTML = `
            <td>${turno.hora}</td>
            <td>${turno.punto}</td>
            <td style="color:${ocupados[index] ? 'green' : 'red'}; font-weight: ${ocupados[index] ? 'bold' : 'normal'}">
              ${ocupados[index] || 'Disponible'}
            </td>
          `;
          tablaBody.appendChild(fila);
        });
      });
    }

    function abrirModal(index) {
      turnoActualSeleccionado = index;
      document.getElementById("turnoSeleccionadoTexto").innerText =
        `${turnos[index].hora} - ${turnos[index].punto}`;
      document.getElementById("nombreInput").value = "";
      document.getElementById("turnoModal").style.display = "block";
    }

    function cerrarModal() {
      document.getElementById("turnoModal").style.display = "none";
      document.getElementById("nombreInput").value = "";
      turnoActualSeleccionado = null;
      document.querySelectorAll(".turno").forEach(t => t.classList.remove("seleccionado"));
    }

    document.getElementById("confirmarBtn").onclick = () => {
      const nombres = document.getElementById("nombreInput").value.trim();
      if (!nombres) {
        alert("Debes escribir los nombres.");
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
          alert("Este turno ya fue ocupado.");
        }
      });
    };

    document.getElementById("cancelarBtn").onclick = cerrarModal;

    onValue(ref(database, "turnosOcupados"), cargarTurnos);
  </script>

</body>
</html>
