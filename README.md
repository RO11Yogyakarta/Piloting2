# 📊 Dashboard Roleplay Interaktif

Dashboard ini dibuat untuk memvisualisasikan data **Roleplay KC/UKER** secara interaktif.  
Tujuan utama: memantau pencapaian roleplay rutin, total input, dan persentase capaian per unit kerja.

---

<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Dashboard Roleplay</title>
  <link rel="stylesheet" href="style.css">
  <!-- Plotly untuk grafik -->
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <!-- PapaParse untuk membaca CSV -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
</head>
<body>
  <h1>📊 Dashboard Roleplay RO 11 Yogyakarta</h1>

  <!-- Ringkasan -->
  <div id="summary"></div>

  <!-- Grafik -->
  <div id="chart" style="width:100%;height:500px;"></div>

  <!-- Tabel -->
  <div id="table"></div>

  <script src="script.js"></script>
</body>
</html>

body {
  font-family: Arial, sans-serif;
  margin: 20px;
  background-color: #f9f9f9;
}

h1 {
  text-align: center;
  color: #333;
}

#summary {
  margin: 20px 0;
  font-size: 18px;
  font-weight: bold;
}

table {
  border-collapse: collapse;
  width: 100%;
  margin-top: 20px;
  background: #fff;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
}

th {
  background-color: #007bff;
  color: white;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

// Baca file CSV
Papa.parse("data/roleplay.csv", {
  download: true,
  header: true,
  complete: function(results) {
    const data = results.data;

    // Ambil data UKER dan %
    const branches = data.map(d => d.UKER);
    const percentages = data.map(d => parseFloat(d["%"]));

    // Hitung ringkasan
    const avgPercent = (percentages.reduce((a,b) => a+b, 0) / percentages.length).toFixed(2);
    document.getElementById("summary").innerHTML = 
      `Rata-rata pencapaian roleplay: <b>${avgPercent}%</b>`;

    // Buat grafik bar
    Plotly.newPlot("chart", [{
      x: branches,
      y: percentages,
      type: "bar",
      marker: {color: "rgba(0,123,255,0.7)"}
    }], {
      title: "Persentase Roleplay per UKER",
      xaxis: {title: "UKER"},
      yaxis: {title: "% Roleplay"}
    });

    // Buat tabel interaktif
    let tableHTML = "<table><tr><th>UKER</th><th>%</th><th>Total Input</th></tr>";
    data.forEach(d => {
      if(d.UKER){ // hindari baris kosong
        tableHTML += `<tr>
          <td>${d.UKER}</td>
          <td>${d["%"]}</td>
          <td>${d["TOTAL INPUT"]}</td>
        </tr>`;
      }
    });
    tableHTML += "</table>";
    document.getElementById("table").innerHTML = tableHTML;
  }
});

No,Kode Branch,UKER,ROLEPLAY RUTIN,WROLEPLAY,DONE/BELUM,TARGET INPUT (3X SEMINGGU),TOTAL INPUT,UKO ROLEPLAY 3X,Uko Kurang Input,Uko Tidak Roleplay,%
1,409,KC YOGYAKARTA MLATI,3,0,0,1,1,0,0,0,100
2,153,Wonosari,23,0,0,1,1,0,0,0,100
3,152,Wates,23,0,0,1,1,0,0,0,100
4,1063,KC SOLO BARU,3,0,0,1,1,0,0,0,100
5,106,Cilacap,24,2,0,0.9583333333,1,0,0,0,95.83
6,410,KC YOGYAKARTA ADISUCIPTO,16,0,4,0.75,1,0,0,0,75
7,262,Parakan,11,2,2,0.7272727273,1,0,0,0,72.73

---

Dengan template ini, repo kamu akan terlihat profesional dan mudah dipahami.  

👉 Mau saya tambahkan juga **badge GitHub Pages** di README (misalnya status deploy otomatis)?
