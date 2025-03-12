# turnos-exhibidor
Por favor, lea: Cómo predicar con los exhibidores de publicaciones Esta encuesta se ha diseñado para saber en qué horarios está disponible para el exhibidor, puede diligenciar la encuesta de manera individual para que se le asigne una pareja, o si lo desea puede diligenciar la encuesta con otro publicador. 
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selecciona tu Turno</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .turno { margin: 10px; padding: 10px; border: 1px solid black; display: inline-block; cursor: pointer; }
        .ocupado { background-color: lightgray; cursor: not-allowed; }
    </style>
</head>
<body>
    <h2>Selecciona tu Turno</h2>
    <div id="turnos"></div>

    <script>
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
            const turnosOcupados = JSON.parse(localStorage.getItem("turnosOcupados")) || [];
            
            turnos.forEach((turno, index) => {
                const div = document.createElement("div");
                div.className = "turno";
                div.innerText = `${turno.hora} - ${turno.punto}`;
                
                if (turnosOcupados.includes(index)) {
                    div.classList.add("ocupado");
                } else {
                    div.onclick = () => seleccionarTurno(index);
                }
                
                turnosContainer.appendChild(div);
            });
        }

        function seleccionarTurno(index) {
            let turnosOcupados = JSON.parse(localStorage.getItem("turnosOcupados")) || [];
            if (!turnosOcupados.includes(index)) {
                turnosOcupados.push(index);
                localStorage.setItem("turnosOcupados", JSON.stringify(turnosOcupados));
                cargarTurnos();
                alert("Turno seleccionado exitosamente.");
            }
        }

        cargarTurnos();
    </script>
</body>
</html>
