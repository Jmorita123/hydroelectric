<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>HIDROMINA - Monitoreo Presa</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <header><h1>HIDROMINA</h1></header>
    <main>
      <div class="panel">
        <button id="connect">🔌 Conectar Presa</button>
        <p id="status">Estado: Desconectado</p>
      </div>
      <div class="data-grid">
        <div><span class="label">📏 Distancia:</span> <span id="dist">--</span> cm</div>
        <div><span class="label">🌡️ Temperatura:</span> <span id="temp">--</span> °C</div>
        <div><span class="label">💧 Humedad:</span> <span id="hum">--</span> %</div>
        <div><span class="label">🌧️ Lluvia:</span> <span id="rain">--</span></div>
        <div><span class="label">📟 Valor analógico:</span> <span id="rainAO">--</span></div>
      </div>
    </main>
    <section class="info">
      <h2>Componentes</h2>
      <ul>
        <li><strong>HC‑SR04:</strong> sensor de distancia ultrasónico.</li>
        <li><strong>DHT11:</strong> sensor de temperatura y humedad.</li>
        <li><strong>Sensor de lluvia:</strong> detecta presencia de agua.</li>
        <li><strong>Bomba DC:</strong> controla nivel de agua con ULN2003.</li>
      </ul>
    </section>
  </div>

  <script>
    const SERVICE = '4fafc201-1fb5-459e-8fcc-c5c9c331914b';
    const CHAR = 'beb5483e-36e1-4688-b7f5-e9a123456789';
    let characteristic;

    document.getElementById('connect').onclick = async () => {
      try {
        const device = await navigator.bluetooth.requestDevice({
          filters: [{ name: 'Presa_HIDRO' }],
          optionalServices: [SERVICE]
        });
        document.getElementById('status').textContent = 'Estado: Conectando…';
        const server = await device.gatt.connect();
        const service = await server.getPrimaryService(SERVICE);
        characteristic = await service.getCharacteristic(CHAR);
        await characteristic.startNotifications();
        characteristic.addEventListener('characteristicvaluechanged', onData);
        document.getElementById('status').textContent = 'Estado: Conectado';
      } catch {
        document.getElementById('status').textContent = 'Error al conectar';
      }
    };

    function onData(event) {
      const txt = new TextDecoder().decode(event.target.value);
      const [d,t,h,lr,ao] = txt.split(',');
      document.getElementById('dist').textContent = d;
      document.getElementById('temp').textContent = t;
      document.getElementById('hum').textContent = h;
      document.getElementById('rain').textContent = lr === '1' ? 'SI' : 'NO';
      document.getElementById('rainAO').textContent = ao;
    }
  </script>
</body>
</html>
body {
  margin: 0; padding: 0;
  font-family: 'Segoe UI', sans-serif;
  color: #333;
  background: url('https://images.unsplash.com/photo-1503264116251-35a269479413?auto=format&fit=crop&w=1350&q=80') no-repeat center center fixed;
  background-size: cover;
}
.container {
  max-width: 800px;
  margin: auto;
  background: rgba(255,255,255,0.9);
  padding: 20px;
  border-radius: 8px;
}
header h1 {
  margin: 0;
  text-align: center;
  color: #045a8d;
}
.panel {
  text-align: center;
  margin: 20px 0;
}
#connect {
  padding: 10px 20px;
  font-size: 16px;
  background: #045a8d;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
#status {
  margin-top: 10px;
  font-style: italic;
}
.data-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
  margin-bottom: 20px;
}
.label {
  font-weight: bold;
}
.info h2 {
  color: #045a8d;
}
.info ul {
  list-style: none;
  padding-left: 0;
}
.info li {
  margin: 6px 0;
}
