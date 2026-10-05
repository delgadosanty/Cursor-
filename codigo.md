<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Saludo al estudiante</title>
  <style>
    :root {
      --bg: #0f172a;
      --card: #ffffff;
      --accent: #2563eb;
      --accent-dark: #1d4ed8;
      --text: #0f172a;
      --muted: #64748b;
      --ok: #059669;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: "Segoe UI", system-ui, sans-serif;
      background:
        radial-gradient(circle at top right, #38bdf8 0%, transparent 40%),
        radial-gradient(circle at bottom left, #6366f1 0%, transparent 42%),
        var(--bg);
      display: grid;
      place-items: center;
      padding: 24px;
      color: var(--text);
    }

    .card {
      width: min(440px, 100%);
      background: var(--card);
      border-radius: 24px;
      padding: 32px 28px;
      box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.45);
    }

    h1 { margin: 0 0 8px; font-size: 1.7rem; }

    .subtitle {
      margin: 0 0 24px;
      color: var(--muted);
      line-height: 1.4;
    }

    button {
      width: 100%;
      border: 0;
      border-radius: 14px;
      padding: 14px 16px;
      font-size: 1rem;
      font-weight: 700;
      cursor: pointer;
    }

    #solicitar { background: var(--accent); color: white; }
    #solicitar:hover { background: var(--accent-dark); }

    form { margin-top: 22px; display: grid; gap: 14px; }

    label {
      display: grid;
      gap: 6px;
      font-size: 0.9rem;
      font-weight: 600;
      color: #334155;
    }

    input, select {
      width: 100%;
      padding: 12px 14px;
      border: 1px solid #cbd5e1;
      border-radius: 12px;
      font-size: 1rem;
      background: #f8fafc;
    }

    input:focus, select:focus {
      outline: 3px solid #bfdbfe;
      border-color: var(--accent);
      background: white;
    }

    .hora-grupo {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .enviar { background: var(--ok); color: white; margin-top: 6px; }
    .enviar:hover { background: #047857; }

    #mensaje {
      margin: 22px 0 0;
      padding: 16px;
      border-radius: 16px;
      background: #eff6ff;
      color: #1e3a8a;
      font-weight: 600;
      line-height: 1.5;
      display: none;
    }

    #mensaje.visible { display: block; }
    .hidden { display: none; }
  </style>
</head>
<body>
  <main class="card">
    <h1>Saludo al estudiante</h1>
    <p class="subtitle">Haz clic para solicitar un saludo personalizado.</p>

    <button id="solicitar" type="button">Solicitar saludo</button>

    <form id="formulario" class="hidden">
      <label>
        Nombre
        <input id="nombre" required placeholder="Ej. Ana">
      </label>

      <label>
        Edad
        <input id="edad" type="number" min="1" required placeholder="Ej. 18">
      </label>

      <div class="hora-grupo">
        <label>
          Hora
          <input id="hora" type="number" min="1" max="12" required placeholder="1 a 12">
        </label>
        <label>
          AM / PM
          <select id="ampm" required>
            <option value="">Seleccione</option>
            <option value="AM">AM</option>
            <option value="PM">PM</option>
          </select>
        </label>
      </div>

      <button class="enviar" type="submit">Enviar datos</button>
    </form>

    <p id="mensaje"></p>
  </main>

  <script>
    const solicitar = document.getElementById("solicitar");
    const formulario = document.getElementById("formulario");
    const mensaje = document.getElementById("mensaje");

    solicitar.addEventListener("click", () => {
      formulario.classList.remove("hidden");
      solicitar.classList.add("hidden");
      mensaje.classList.remove("visible");
    });

    formulario.addEventListener("submit", (evento) => {
      evento.preventDefault();

      const nombre = document.getElementById("nombre").value.trim();
      const edad = document.getElementById("edad").value;
      const hora = document.getElementById("hora").value;
      const ampm = document.getElementById("ampm").value;
      const saludo = ampm === "AM" ? "Buenos días" : "Buenas tardes";

      mensaje.textContent =
        `${saludo}, ${nombre}. Tienes ${edad} años. Son las ${hora} ${ampm}.`;
      mensaje.classList.add("visible");
    });
  </script>
</body>
</html>
