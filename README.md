<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Malla Medicina Veterinaria UC</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>
body{
  font-family:'Poppins',sans-serif;
  background:linear-gradient(135deg,#fde4ff,#e0f7fa);
  margin:0;
  padding:20px;
}

h1{
  text-align:center;
  color:#6a1b9a;
  margin-bottom:30px;
}

.malla{
  display:grid;
  grid-template-columns:repeat(6,1fr);
  gap:20px;
}

.anio{
  background:#ffffffcc;
  border-radius:18px;
  padding:14px;
}

.anio h2{
  text-align:center;
  color:#8e24aa;
  font-size:18px;
  margin-bottom:10px;
}

.semestre{
  background:#f3e5f5;
  border-radius:14px;
  padding:10px;
  margin-bottom:12px;
}

.semestre h3{
  font-size:14px;
  color:#00838f;
  margin-bottom:8px;
  text-align:center;
}

.ramo{
  background:#ede7f6;
  border-radius:10px;
  padding:8px;
  margin-bottom:6px;
  font-size:12px;
  cursor:pointer;
  transition:0.2s;
  position:relative;
}

.ramo:hover{transform:scale(1.03);}

.ramo.aprobado{
  background:#c8e6c9;
  text-decoration:line-through;
}

.ramo.bloqueado{
  background:#eeeeee;
  color:#888;
  cursor:not-allowed;
}

.creditos{
  font-size:10px;
  color:#555;
}

.tooltip{
  display:none;
  position:absolute;
  bottom:110%;
  left:50%;
  transform:translateX(-50%);
  background:#6a1b9a;
  color:white;
  padding:6px;
  border-radius:8px;
  font-size:11px;
  width:200px;
  z-index:10;
}

.ramo.bloqueado:hover .tooltip{
  display:block;
}
</style>
</head>

<body>

<h1>Malla Interactiva – Medicina Veterinaria UC</h1>
<div class="malla" id="malla"></div>

<script>
const aprobados = JSON.parse(localStorage.getItem("aprobados")||"[]");

function ramoHTML(cod,nombre,cred,req){
  const d=document.createElement("div");
  d.className="ramo";
  d.innerHTML=`<strong>${nombre}</strong><br>${cod}<div class="creditos">${cred} créditos</div>`;
  const faltan=req.filter(r=>!aprobados.includes(r));

  if(req.length && faltan.length){
    d.classList.add("bloqueado");
    const t=document.createElement("div");
    t.className="tooltip";
    t.innerText="Falta aprobar: "+faltan.join(", ");
    d.appendChild(t);
  } else {
    if(aprobados.includes(cod)) d.classList.add("aprobado");
    d.onclick=()=>{
      if(aprobados.includes(cod))
        aprobados.splice(aprobados.indexOf(cod),1);
      else aprobados.push(cod);
      localStorage.setItem("aprobados",JSON.stringify(aprobados));
      location.reload();
    }
  }
  return d;
}

const data = [
{
anio:"1° Año",
semestres:[
{nombre:"Semestre 1", r:[
["VET0010","Introducción a la Medicina Veterinaria",5,[]],
["QUIM201M","Química",10,[]],
["MAT1023","Cálculo",10,[]],
["FIS119V","Física Biomédica",10,[]],
["FIL2001","Filosofía",10,[]],
["VET0140","Biología y Diversidad Animal",10,[]]
]},
{nombre:"Semestre 2", r:[
["VET0110","Anatomía Vet. I",10,["VET0140"]],
["BIO239M","Biología Molecular",10,["QUIM201M"]],
["VET0100","Histología y Embriología",10,[]],
["VET0130","Bioestadística",10,["MAT1023"]],
["VET0020","Ética Veterinaria",5,[]],
["ELECTIVO1","Electivo FG",5,[]]
]}
]
},
{
anio:"2° Año",
semestres:[
{nombre:"Semestre 3", r:[
["VET0120","Anatomía Vet. II",10,["VET0110"]],
["VET1100","Fisiología Animal",10,["BIO239M","VET0100","FIS119V"]],
["VET1400","Ecología Veterinaria",5,["VET0140"]],
["VET0500","Etología y Bienestar Animal",5,["VET0140"]],
["ELECTIVO2","Electivo FG",5,[]]
]},
{nombre:"Semestre 4", r:[
["VET1110","Microbiología e Inmunología",10,["BIO239M"]],
["VET1130","Fisiopatología",10,["VET1100"]],
["VET1120","Genética y Evolución",10,["BIO239M","VET0130"]],
["VET0510","Manejo Animal",5,["VET0500","VET0120","VET0020"]],
["ELECTIVO3","Electivo FG",5,[]],
["ELECTIVO4","Electivo FG",5,[]]
]}
]
},
{
anio:"3° Año",
semestres:[
{nombre:"Semestre 5", r:[
["VET1140","Enfermedades Parasitarias",10,["VET1110","VET1130"]],
["VET1150","Patología General",10,["VET1100","VET0120","VET1130"]],
["AGZ341","Nutrición Animal",10,["VET1100","VET1120"]],
["AGZ331","Alimentos para Animales",10,["VET1100","VET1120"]],
["ELECTIVO5","Electivo FG",5,[]]
]},
{nombre:"Semestre 6", r:[
["VET1160","Enfermedades Infecciosas",10,["VET1110","VET1150"]],
["VET1180","Farmacología",10,["VET0130","VET1130"]],
["VET1170","Reproducción Animal",10,["VET1150"]],
["ELECTIVO6","Electivo FG",5,[]],
["ELECTIVO7","Electivo FG",5,[]]
]}
]
},
{
anio:"4° Año",
semestres:[
{nombre:"Semestre 7", r:[
["VET1800","Semiología",10,["VET0120","VET0510"]],
["VET1810","Patología Clínica",10,["VET1130","VET1160","VET1140"]],
["VET1190","Ginecología y Obstetricia",10,["VET1170"]],
["VET1600","Salud Pública",5,["VET0130"]],
["VET1300","Producción Monogástricos",10,["VET1120","AGZ341","AGZ331"]]
]},
{nombre:"Semestre 8", r:[
["VET1830","Medicina Interna I",10,["VET1800","VET1810","VET1180"]],
["VET2820","Imagenología",5,["VET1800"]],
["VET1310","Producción Rumiantes",10,["VET1120","VET1300","AGZ341","AGZ331"]],
["VET1000","Economía Veterinaria",5,["VET0130"]],
["VET1410","Manejo Vida Silvestre",5,["VET1400","VET1130","VET1160"]]
]}
]
},
{
anio:"5° Año",
semestres:[
{nombre:"Semestre 9", r:[
["VET2830","Medicina Interna II",10,["VET1830"]],
["VET2800","Cirugía y Anestesia",10,["VET1830","VET1810"]],
["VET2000","Evaluación de Proyectos",5,["VET1000","VET1310"]],
["OPT1","Optativo Profundización",5,[]],
["OPT2","Optativo Profundización",5,[]]
]},
{nombre:"Semestre 10", r:[
["VET1700","Calidad e Inocuidad",5,["VET1600"]],
["VET2810","Cirugía Avanzada",10,["VET2800"]],
["OPT3","Optativo Profundización",5,[]],
["OPT4","Optativo Profundización",5,[]]
]}
]
},
{
anio:"6° Año",
semestres:[
{nombre:"Semestre 11-12", r:[
["INT1","Internado Caninos/Felinos",30,[]],
["INT2","Internado Equinos",30,[]]
]}
]
}
];

const cont=document.getElementById("malla");

data.forEach(a=>{
  const anio=document.createElement("div");
  anio.className="anio";
  anio.innerHTML=`<h2>${a.anio}</h2>`;
  a.semestres.forEach(s=>{
    const sem=document.createElement("div");
    sem.className="semestre";
    sem.innerHTML=`<h3>${s.nombre}</h3>`;
    s.r.forEach(x=>sem.appendChild(ramoHTML(...x)));
    anio.appendChild(sem);
  });
  cont.appendChild(anio);
});
</script>

</body>
</html>
