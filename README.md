<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Campaña de exhibidores del sábado 18 de abril</title>
<p>Elige el horario que te funcione y escribe los nombres de dos o tres personas que estarán presentes en ese turno (puedes incluirte).</p>
<p><strong>Importante:</strong> Los turnos que tengan solo un nombre serán eliminados.</p>
<p>Por favor leer: <a href="https://wol.jw.org/es/wol/d/r4/lp-s/202015126?q=exhibidor&p=doc">Cómo predicar con los exhibidores de publicaciones</a></p>

  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: #f0f4f0;
      padding: 16px;
    }

    h1 {
      text-align: center;
      color: #2e7d32;
      font-size: 20px;
      font-weight: 700;
      margin-bottom: 6px;
    }

    .subtitulo {
      text-align: center;
      color: #555;
      font-size: 13px;
      margin-bottom: 20px;
    }

    /* ── Contenedor principal ── */
    .contenedor {
      max-width: 600px;
      margin: auto;
    }

    /* ── Tarjeta de punto ── */
    .punto-card {
      background: #fff;
      border-radius: 12px;
      border: 1px solid #c8e6c9;
      margin-bottom: 16px;
      overflow: hidden;
    }

    .punto-header {
      background: #2e7d32;
      color: white;
      padding: 12px 16px;
      font-size: 15px;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .punto-header .icono {
      font-size: 16px;
    }

    .punto-body {
      padding: 12px;
    }

    /* ── Grid de turnos ── */
    .turnos-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
    }

    /* ── Tarjeta de turno ── */
    .turno {
      border-radius: 8px;
      padding: 10px;
      cursor: pointer;
      border: 1px solid transparent;
      transition: transform 0.1s;
      -webkit-tap-highlight-color: transparent;
    }

    .turno:active:not(.ocupado) { transform: scale(0.97); }

    .turno.ocupado {
      opacity: 0.75;
      cursor: not-allowed;
    }

    .turno-hora {
      font-size: 12px;
      font-weight: 600;
      color: #222;
      line-height: 1.3;
    }

    .turno-estado {
      font-size: 11px;
      color: #555;
      margin-top: 4px;
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .turno-dot {
      display: inline-block;
      width: 7px;
      height: 7px;
      border-radius: 50%;
      flex-shrink: 0;
    }

    .turno-nombres {
      font-size: 11px;
      color: #333;
      margin-top: 5px;
      line-height: 1.5;
      font-weight: 500;
    }

    /* Estados */
    .t-libre   { background: #ffebee; border-color: #ef9a9a; }
    .t-libre   .turno-dot { background: #e53935; }

    .t-ocupado { background: #c8e6c9; border-color: #81c784; }
    .t-ocupado .turno-dot { background: #2e7d32; }

    /* ── Leyenda ── */
    .leyenda {
      max-width: 600px;
      margin: 0 auto 20px;
      display: flex;
      gap: 16px;
      font-size: 12px;
      color: #555;
      padding: 10px 12px;
      background: #fff;
      border-radius: 8px;
      border: 1px solid #ddd;
    }

    .leyenda-item {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .leyenda-dot {
      width: 9px;
      height: 9px;
      border-radius: 50%;
    }

    /* ── Tabla de resumen ── */
    .resumen-titulo {
      text-align: center;
      color: #2e7d32;
      font-size: 17px;
      font-weight: 600;
      margin: 24px 0 12px;
    }

    .tabla-wrapper {
      max-width: 600px;
      margin: auto;
      overflow-x: auto;
      border-radius: 10px;
      border: 1px solid #ddd;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
      font-size: 13px;
      min-width: 320px;
    }

    th {
      background: #f2f2f2;
      padding: 10px 8px;
      text-align: center;
      font-weight: 600;
      border-bottom: 1px solid #ddd;
    }

    td {
      padding: 8px;
      text-align: center;
      border-bottom: 0.5px solid #eee;
    }

    tr:last-child td { border-bottom: none; }

    .estado-libre  { color: #e53935; font-weight: 600; }
    .estado-ocupado { color: #2e7d32; font-weight: 600; }

    /* ── Modal ── */
    .modal {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.5);
      z-index: 100;
      padding: 16px;
      align-items: center;
      justify-content: center;
    }

    .modal.visible { display: flex; }

    .modal-content {
      background: white;
      padding: 24px 20px;
      width: 100%;
      max-width: 340px;
      border-radius: 14px;
    }

    .modal-content h3 {
      color: #2e7d32;
      font-size: 17px;
      margin-bottom: 6px;
    }

    .modal-content p {
      font-size: 14px;
      color: #555;
      margin-bottom: 14px;
    }

    .modal-content input[type="text"] {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 15px;
      outline: none;
      transition: border-color 0.2s;
    }

    .modal-content input[type="text"]:focus {
      border-color: #2e7d32;
    }

    .modal-btns {
      display: flex;
      gap: 10px;
      margin-top: 16px;
    }

    .btn-confirmar {
      flex: 1;
      padding: 10px;
      background: #2e7d32;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
    }

    .btn-confirmar:active { background: #1b5e20; }

    .btn-cancelar {
      padding: 10px 16px;
      background: #eee;
      color: #444;
      border: none;
      border-radius: 8px;
      font-size: 15px;
      cursor: pointer;
    }

    .btn-cancelar:active { background: #ddd; }
  </style>
</head>

<body>

  <div class="contenedor">
    <h1>Turnos del Día</h1>
    <p class="subtitulo">Selecciona el punto y el horario de tu preferencia</p>
  </div>

  <div class="leyenda" style="max-width:600px;margin:0 auto 16px;">
    <div class="leyenda-item">
      <div class="leyenda-dot" style="background:#e53935"></div> Disponible
    </div>
    <div class="leyenda-item">
      <div class="leyenda-dot" style="background:#2e7d32"></div> Ocupado
    </div>
  </div>

  <div id="turnos" class="contenedor"></div>

  <p class="resumen-titulo">Resumen de turnos</p>
  <div class="tabla-wrapper">
    <table>
      <thead>
        <tr>
          <th>Punto</th>
          <th>Hora</th>
          <th>Estado</th>
        </tr>
      </thead>
      <tbody id="tablaTurnos"></tbody>
    </table>
  </div>

  <!-- Modal -->
  <div id="turnoModal" class="modal">
    <div class="modal-content">
      <h3>Confirmar turno</h3>
      <p id="turnoSeleccionadoTexto"></p>
      <input type="text" id="nombreInput" placeholder="Nombres de las personas">
      <div class="modal-btns">
        <button class="btn-confirmar" id="confirmarBtn">Confirmar</button>
        <button class="btn-cancelar" id="cancelarBtn">Cancelar</button>
      </div>
    </div>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getDatabase, ref, get, set, onValue } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

    // ── Firebase (sin cambios) ──
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
    const db  = getDatabase(app);

    // ── Datos (bug de coma en Afidro corregido) ──
    const turnos = [
      { hora: '07:00 - 09:00 a.m.', punto: 'Tibabuyes' },
      { hora: '09:00 - 11:00 a.m.', punto: 'Tibabuyes' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Tibabuyes' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Tibabuyes' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Tibabuyes' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Tibabuyes' },

      { hora: '07:00 - 09:00 a.m.', punto: 'Afidro' },
      { hora: '09:00 - 11:00 a.m.', punto: 'Afidro' },   // ← coma corregida
      { hora: '11:00 - 01:00 p.m.', punto: 'Afidro' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Afidro' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Afidro' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Afidro' },

      { hora: '07:00 - 09:00 a.m.', punto: 'Yaiti' },
      { hora: '09:00 - 11:00 a.m.', punto: 'Yaiti' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Yaiti' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Yaiti' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Yaiti' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Yaiti' },

      { hora: '07:00 - 09:00 a.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '09:00 - 11:00 a.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Adicional 1 - Cll 144 Cra 136a' },

      { hora: '07:00 - 09:00 a.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '09:00 - 11:00 a.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '11:00 - 01:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '01:00 - 03:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '03:00 - 05:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
      { hora: '05:00 - 07:00 p.m.', punto: 'Adicional 2 - Cll 143 Cra 127c' },
    ];

    let turnoSeleccionado = null;

    // ── Renderizado ──
    function cargarTurnos() {
      const contenedor = document.getElementById("turnos");
      const tabla      = document.getElementById("tablaTurnos");

      contenedor.innerHTML = "";
      tabla.innerHTML      = "";

      const grupos = {};
      turnos.forEach((t, i) => {
        if (!grupos[t.punto]) grupos[t.punto] = [];
        grupos[t.punto].push({ ...t, index: i });
      });

      get(ref(db, "turnosOcupados")).then(snapshot => {
        const ocupados = snapshot.val() || {};

        Object.keys(grupos).forEach(punto => {

          // Tarjeta del punto
          const card = document.createElement("div");
          card.className = "punto-card";

          const header = document.createElement("div");
          header.className = "punto-header";
          header.innerHTML = `<span class="icono">📍</span>${punto}`;

          const body = document.createElement("div");
          body.className = "punto-body";

          const grid = document.createElement("div");
          grid.className = "turnos-grid";

          grupos[punto].forEach(t => {
            const estaOcupado = !!ocupados[t.index];

            const div = document.createElement("div");
            div.className = `turno ${estaOcupado ? "t-ocupado ocupado" : "t-libre"}`;

            div.innerHTML = `
              <div class="turno-hora">${t.hora}</div>
              <div class="turno-estado">
                <span class="turno-dot"></span>
                ${estaOcupado ? "Ocupado" : "Disponible"}
              </div>
              ${estaOcupado ? `<div class="turno-nombres">${ocupados[t.index]}</div>` : ""}
            `;

            if (!estaOcupado) {
              div.onclick = () => abrirModal(t.index, t);
            }

            grid.appendChild(div);

            // Fila de la tabla
            const fila = document.createElement("tr");
            fila.innerHTML = `
              <td>${punto}</td>
              <td>${t.hora}</td>
              <td class="${estaOcupado ? "estado-ocupado" : "estado-libre"}">
                ${estaOcupado ? ocupados[t.index] : "Disponible"}
              </td>
            `;
            tabla.appendChild(fila);
          });

          body.appendChild(grid);
          card.appendChild(header);
          card.appendChild(body);
          contenedor.appendChild(card);
        });
      });
    }

    // ── Modal ──
    function abrirModal(index, turno) {
      turnoSeleccionado = index;
      document.getElementById("turnoSeleccionadoTexto").innerText =
        `${turno.punto} · ${turno.hora}`;
      document.getElementById("nombreInput").value = "";
      document.getElementById("turnoModal").classList.add("visible");
      setTimeout(() => document.getElementById("nombreInput").focus(), 100);
    }

    function cerrarModal() {
      document.getElementById("turnoModal").classList.remove("visible");
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
          alert("Este turno ya fue tomado.");
          cerrarModal();
        }
      });
    };

    document.getElementById("cancelarBtn").onclick = cerrarModal;

    // Cerrar modal al tocar el fondo
    document.getElementById("turnoModal").onclick = (e) => {
      if (e.target === document.getElementById("turnoModal")) cerrarModal();
    };

    // Escucha cambios en tiempo real
    onValue(ref(db, "turnosOcupados"), cargarTurnos);
  </script>

</body>
</html>
