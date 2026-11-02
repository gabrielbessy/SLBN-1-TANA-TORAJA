
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SIAS - Absensi Siswa SLBN 1 Tana Toraja</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial;background:#f1f5f9;color:#1e293b}
header{background:#123b61;color:white;padding:22px;text-align:center}
header h2{margin:0 0 8px}
.container{max-width:1000px;margin:25px auto;padding:15px}
.card{background:white;padding:20px;border-radius:12px;margin-bottom:18px;box-shadow:0 2px 8px #0001}
input,select,button{padding:11px;border:1px solid #cbd5e1;border-radius:7px;margin:5px 0;font-size:14px}
input,select{max-width:100%}
button{background:#1769aa;color:white;border:0;cursor:pointer}
button:hover{background:#0c4a80}
table{width:100%;border-collapse:collapse;margin-top:12px}
th,td{padding:10px;border:1px solid #ddd;text-align:left}
th{background:#e2e8f0}
.stats{display:flex;gap:10px;flex-wrap:wrap}
.stat{background:#e0f2fe;padding:15px;border-radius:8px;flex:1;min-width:110px}
.stat strong{display:block;font-size:25px}
.actions{display:flex;gap:8px;flex-wrap:wrap}
@media(max-width:600px){table{font-size:12px}th,td{padding:6px}}
</style>
</head>
<body>
<header>
<h2>SIAS - ABSENSI SISWA</h2>
<p>UPT SLB NEGERI 1 TANA TORAJA</p>
<p>Sistem Informasi Absensi Siswa</p>
</header>

<div class="container">
<div class="card">
<h3>📊 Dashboard Absensi</h3>
<div class="stats">
<div class="stat">Hadir<strong id="hadir">0</strong></div>
<div class="stat">Izin<strong id="izin">0</strong></div>
<div class="stat">Sakit<strong id="sakit">0</strong></div>
<div class="stat">Alpa<strong id="alpa">0</strong></div>
</div>
</div>

<div class="card">
<h3>👨‍🎓 Tambah Data Siswa</h3>
<input id="nama" placeholder="Nama siswa">
<input id="kelas" placeholder="Kelas">
<button onclick="tambahSiswa()">Simpan Siswa</button>
</div>

<div class="card">
<h3>📝 Absensi Harian</h3>
<label>Tanggal:</label>
<input type="date" id="tanggal">
<table>
<thead>
<tr><th>Nama</th><th>Kelas</th><th>Kehadiran</th></tr>
</thead>
<tbody id="daftar"></tbody>
</table>
<button onclick="simpanAbsensi()">Simpan Absensi</button>
</div>

<div class="card">
<h3>📋 Rekap Absensi</h3>
<button onclick="rekap()">Tampilkan Rekap</button>
<button onclick="window.print()">Cetak Laporan</button>
<div id="hasil"></div>
</div>
</div>

<script>
let siswa=JSON.parse(localStorage.getItem("siswa")||"[]");
let absensi=JSON.parse(localStorage.getItem("absensi")||"[]");

document.getElementById("tanggal").value=new Date().toISOString().slice(0,10);

function tambahSiswa(){
let nama=document.getElementById("nama").value.trim();
let kelas=document.getElementById("kelas").value.trim();
if(!nama||!kelas){alert("Isi nama dan kelas!");return}
siswa.push({nama,kelas});
localStorage.setItem("siswa",JSON.stringify(siswa));
document.getElementById("nama").value="";
document.getElementById("kelas").value="";
tampil();
}

function tampil(){
let html="";
siswa.forEach((s,i)=>{
html+=`<tr><td>${s.nama}</td><td>${s.kelas}</td><td>
<select id="status${i}">
<option>Hadir</option><option>Izin</option>
<option>Sakit</option><option>Alpa</option>
</select></td></tr>`;
});
document.getElementById("daftar").innerHTML=html;
}

function simpanAbsensi(){
let tanggal=document.getElementById("tanggal").value;
if(!tanggal){alert("Pilih tanggal!");return}
siswa.forEach((s,i)=>{
absensi.push({nama:s.nama,kelas:s.kelas,tanggal,status:document.getElementById("status"+i).value});
});
localStorage.setItem("absensi",JSON.stringify(absensi));
alert("Absensi berhasil disimpan!");
hitung();
}

function hitung(){
["Hadir","Izin","Sakit","Alpa"].forEach(st=>{
document.getElementById(st.toLowerCase()).innerText=
absensi.filter(a=>a.status===st).length;
});
}

function rekap(){
let html="<table><tr><th>Tanggal</th><th>Nama</th><th>Kelas</th><th>Status</th></tr>";
absensi.forEach(a=>{
html+=`<tr><td>${a.tanggal}</td><td>${a.nama}</td><td>${a.kelas}</td><td>${a.status}</td></tr>`;
});
html+="</table>";
document.getElementById("hasil").innerHTML=html;
}

tampil();
hitung();
</script>
</body>
</html>
