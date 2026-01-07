<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      padding: 15px;
      text-align: center;
    }

    h1 {
      margin-bottom: 10px;
    }

    h3 {
      width: 100%;
      text-align: left;
      max-width: 1000px;
      margin: 30px auto 10px;
      color: #2e7d32;
      border-bottom: 2px solid #c8e6c9;
      padding-bottom: 4px;
    }

    #turnos {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      max-width: 1000px;
      margin: auto;
    }

    .turno {
      margin: 8px;
      padding: 12px;
      width: 180px;
      border-radius: 10px;
      background: #ffffff;
      border: 2px solid #ddd;
      cursor: pointer;
      transition: all 0.25s ease;
      box-shadow: 0 2px 6px rgba(0,0,0,0.08);
      font-weight: bold;
    }

    .turno:hover {
      transform: translateY(-3px);
      box-shadow: 0 4px 10px rgba(0,0,0,0.15);
    }

    /* Seleccionado */
    .turno.seleccionado {
      background: #e0e0e0;
      border-color: #777;
    }

    /* Ocupado */
    .turno.ocupado {
      background: #d4f8d4;
      border-color: #2e7d32;
      cursor: not-allowed;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.5);
      z-index: 1000;
    }

    .modal-content {
      background: white;
      margin: 15% auto;
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
      border-radius: 6px;
      border: none;
      cursor: pointer;
    }

    #confirmarBtn {
      background: #2e7d32;
      color: white;
    }

    #cancelarBtn {
      background: #ccc;
      margin-left: 5px;
    }

    /* Tabla */
    table {
      margin: 40px auto;
      border-collapse: collapse;
      width: 90%;
      max-width: 1000px;
      background: white;
    }

    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: center;
    }

    th {
      background: #f2f2f2;
    }
  </style>
</head>

<body>

<h1>Asignación de turnos – Campaña exhibidor</h1>

<div id="turnos"></div>

<h2>Resumen de turnos</h2>

<table>
  <thead>
    <tr>
      <th>Hora</th>
      <th>Punto</th>
      <th>Estado</th>
    </tr>
  </thead>
  <tbody id="tablaTurnos"></tbody>
</table>

<!-- Modal -->
<div id="turnoModal" class="modal">
  <div class="modal-content">
    <h3>Confirmar turno</h3>
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
  const db = getDatabase(app);

  const turnos = [
    { hora: '07:00 - 09:00 a.m.', punto: 'Tibabuyes' },
    { hora: '09:00 - 11:00 a.m.', punto: 'Tibabuyes' },
    { hora: '11:00 - 1:00 p.m.', punto: 'Tibabuyes' },
    { hora: '1:00 - 3:00 p.m.', punto: 'Tibabuyes' },
    { hora: '3:00 - 5:00 p.m.', punto: 'Tibabuyes' },
    { hora: '5:00 - 7:00 p.m.', punto: 'Tibabuyes' },

    { hora: '07:00 - 09:00 a.m.', punto: 'Afidro' },
    { hora: '09:00 - 11:00 a.m,', punto: 'Afidro' },
    { hora: '11:00 - 1:00 p.m.', punto: 'Afidro' },
    { hora: '1:00 - 3:00 p.m.', punto: 'Afidro' },
    { hora: '3:00 - 5:00 p.m.', punto: 'Afidro' },
    { hora: '5:00 - 7:00 p.m.', punto: 'Afidro' },

    { hora: '07:00 - 09:00 a.m.', punto: 'Yaiti' }, 
  { hora: '09:00 - 11:00 a.m.', punto: 'Yaiti' }, 
  { hora: '11:00 - 01:00 p.m.', punto: 'Yaiti' }, 
  { hora: '01:00 - 03:00 p.m.', punto: 'Yaiti' }, 
  { hora: '03:00 - 05:00 p.m.', punto: 'Yaiti' }, 
  { hora: '05:00 - 07:00 p.m.', punto: 'Yaiti' }, 
  
  { hora: '07:00 - 09:00 a.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' }, 
  { hora: '09:00 - 11:00 a.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' }, 
  { hora: '11:00 - 01:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' }, 
  { hora: '01:00 - 03:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' }, 
  { hora: '03:00 - 05:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' }, 
  { hora: '05:00 - 07:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' }, 
  
  { hora: '07:00 - 09:00 a.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' }, 
  { hora: '09:00 - 11:00 a.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' }, 
  { hora: '11:00 - 01:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' }, 
  { hora: '01:00 - 03:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' }, 
  { hora: '03:00 - 05:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' }, 
  { hora: '05:00 - 07:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
  ];

  let turnoSeleccionado = null;

  function cargarTurnos() {
    const contenedor = document.getElementById("turnos");
    const tabla = document.getElementById("tablaTurnos");

    contenedor.innerHTML = "";
    tabla.innerHTML = "";

    const grupos = {};
    turnos.forEach((t, i) => {
      if (!grupos[t.punto]) grupos[t.punto] = [];
      grupos[t.punto].push({ ...t, index: i });
    });

    get(ref(db, "turnosOcupados")).then(snapshot => {
      const ocupados = snapshot.val() || {};

      Object.keys(grupos).forEach(punto => {
        const titulo = document.createElement("h3");
        titulo.innerText = punto;
        contenedor.appendChild(titulo);

        grupos[punto].forEach(t => {
          const div = document.createElement("div");
          div.className = "turno";
          div.innerText = t.hora;

          if (ocupados[t.index]) {
            div.classList.add("ocupado");
          } else {
            div.onclick = () => abrirModal(t.index, t);
          }

          contenedor.appendChild(div);

          const fila = document.createElement("tr");
          fila.innerHTML = `
            <td>${t.hora}</td>
            <td>${punto}</td>
            <td style="color:${ocupados[t.index] ? 'green' : 'red'}; font-weight:bold">
              ${ocupados[t.index] || 'Disponible'}
            </td>
          `;
          tabla.appendChild(fila);
        });
      });
    });
  }

  function abrirModal(index, turno) {
    turnoSeleccionado = index;
    document.getElementById("turnoSeleccionadoTexto").innerText =
      `${turno.hora} – ${turno.punto}`;
    document.getElementById("nombreInput").value = "";
    document.getElementById("turnoModal").style.display = "block";
  }

  function cerrarModal() {
    document.getElementById("turnoModal").style.display = "none";
    turnoSeleccionado = null;
  }

  document.getElementById("confirmarBtn").onclick = () => {
    const nombres = document.getElementById("nombreInput").value.trim();
    if (!nombres) return alert("Escribe los nombres.");

    const refTurno = ref(db, `turnosOcupados/${turnoSeleccionado}`);
    get(refTurno).then(snap => {
      if (!snap.exists()) {
        set(refTurno, nombres).then(() => {
          cerrarModal();
          cargarTurnos();
        });
      } else {
        alert("Turno ya ocupado.");
        cerrarModal();
      }
    });
  };

  document.getElementById("cancelarBtn").onclick = cerrarModal;

  onValue(ref(db, "turnosOcupados"), cargarTurnos);
</script>

</body>
</html>
