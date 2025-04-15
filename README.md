<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Unit Converter AI</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #1e1e2f, #2d2d44);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }

    .container {
      background: rgba(255, 255, 255, 0.05);
      padding: 2.5rem;
      border-radius: 25px;
      box-shadow: 0 0 30px rgba(0, 255, 230, 0.5);
      text-align: center;
      width: 400px;
      backdrop-filter: blur(10px);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }

    h1 {
      color: #00ffe0;
      font-size: 2rem;
      margin-bottom: 25px;
      text-shadow: 1px 1px 2px black;
    }

    select, input {
      width: 90%;
      padding: 12px;
      margin: 10px 0;
      border-radius: 12px;
      background-color: #2c2c3e;
      color: white;
      border: 1px solid #444;
      font-size: 1rem;
      outline: none;
      transition: 0.3s;
    }

    select:focus, input:focus {
      border-color: #00ffe0;
      background-color: #1f1f2e;
    }

    button {
      background: #00ffe0;
      color: #000;
      padding: 12px 18px;
      margin: 10px 5px;
      border: none;
      border-radius: 10px;
      font-weight: bold;
      font-size: 1rem;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.2s ease;
    }

    button:hover {
      background: #00d5c0;
      transform: scale(1.05);
    }

    #result {
      margin-top: 20px;
      font-size: 1.3rem;
      color: #00ffb3;
      text-shadow: 0 0 5px #00ffe0;
    }
  </style>
</head>
<body>
<div class="container">
  <h1>🌟 AI Unit Converter</h1>

  <select id="category" onchange="populateUnits()">
    <option value="">Select Category</option>
    <option value="length">Length</option>
    <option value="temperature">Temperature</option>
    <option value="frequency">Frequency</option>
  </select><br>

  <input type="number" id="valueInput" placeholder="Enter value" /><br>

  <select id="fromUnit"></select><br>
  <select id="toUnit"></select><br>

  <button onclick="convert()">Convert</button>

  <div id="result"></div>
</div>

<script>
  const units = {
    length: ['m', 'cm', 'km', 'inch', 'ft'],
    temperature: ['C', 'F', 'K'],
    frequency: ['Hz', 'kHz', 'MHz', 'GHz']
  };

  function populateUnits() {
    const category = document.getElementById("category").value;
    const from = document.getElementById("fromUnit");
    const to = document.getElementById("toUnit");

    from.innerHTML = "";
    to.innerHTML = "";

    if (units[category]) {
      units[category].forEach(unit => {
        from.innerHTML += `<option value="${unit}">${unit}</option>`;
        to.innerHTML += `<option value="${unit}">${unit}</option>`;
      });
    }
  }

  function convert() {
    const value = parseFloat(document.getElementById("valueInput").value);
    const from = document.getElementById("fromUnit").value;
    const to = document.getElementById("toUnit").value;
    const category = document.getElementById("category").value;

    if (isNaN(value) || !from || !to || !category) {
      document.getElementById("result").innerText = "❌ Please fill all fields properly!";
      return;
    }

    let result;

    if (category === "length") {
      const factors = {
        m: 1,
        cm: 0.01,
        km: 1000,
        inch: 0.0254,
        ft: 0.3048
      };
      result = value * factors[from] / factors[to];

    } else if (category === "temperature") {
      if (from === "C" && to === "F") result = (value * 9/5) + 32;
      else if (from === "F" && to === "C") result = (value - 32) * 5/9;
      else if (from === "C" && to === "K") result = value + 273.15;
      else if (from === "K" && to === "C") result = value - 273.15;
      else if (from === "F" && to === "K") result = (value - 32) * 5/9 + 273.15;
      else if (from === "K" && to === "F") result = (value - 273.15) * 9/5 + 32;
      else result = value;

    } else if (category === "frequency") {
      const factors = {
        Hz: 1,
        kHz: 1e3,
        MHz: 1e6,
        GHz: 1e9
      };
      result = value * factors[from] / factors[to];
    }

    document.getElementById("result").innerText = `✅ ${value} ${from} = ${result.toFixed(4)} ${to}`;
  }
</script>
</body>
</html>



