
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vai de Ônibus | Passagens</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f3f6fb;color:#17243b}
header{background:#0756a5;color:white;padding:20px 7%;display:flex;justify-content:space-between;align-items:center}
.logo{font-size:25px;font-weight:bold}
header a{color:white;text-decoration:none}
.hero{background:linear-gradient(120deg,#064b96,#1685dc);padding:65px 20px;text-align:center;color:white}
.hero h1{font-size:38px;margin:0 0 12px}
.hero p{font-size:17px}
.container{max-width:950px;margin:-25px auto 40px;padding:0 15px}
.panel{background:white;padding:25px;border-radius:15px;box-shadow:0 8px 30px #102c501a}
.form{display:grid;grid-template-columns:repeat(2,1fr);gap:15px}
label{font-weight:bold;font-size:14px}
input,select{width:100%;padding:14px;border:1px solid #ccd5e1;border-radius:8px;margin-top:7px;font-size:15px}
button{border:0;border-radius:8px;background:#0876d1;color:white;padding:15px;font-size:16px;font-weight:bold;cursor:pointer}
button:hover{background:#0756a5}
.search{width:100%;margin-top:20px}
#results{margin-top:25px}
.bus{background:white;border:1px solid #dce4ee;padding:20px;border-radius:12px;margin:12px 0;display:flex;justify-content:space-between;align-items:center;gap:15px}
.price{font-size:23px;color:#0876d1;font-weight:bold}
.small{color:#65748a;font-size:13px}
.seats{display:grid;grid-template-columns:repeat(4,48px);gap:10px;margin:20px 0}
.seat{padding:12px 5px;background:#e2eaf2;color:#17314b}
.seat.selected{background:#0876d1;color:white}
.seat.taken{background:#aaa;color:white;cursor:not-allowed}
.hidden{display:none}
footer{text-align:center;padding:25px;background:#102d4c;color:white}
@media(max-width:600px){
 .hero h1{font-size:29px}
 .form{grid-template-columns:1fr}
 .bus{flex-direction:column;align-items:flex-start}
 .bus button{width:100%}
}
</style>
</head>
<body>

<header>
 <div class="logo">🚌 Vai de Ônibus</div>
 <a href="#busca">Comprar passagem</a>
</header>

<section class="hero">
 <h1>Viaje com conforto e economia!</h1>
 <p>Encontre sua próxima viagem em poucos cliques.</p>
</section>

<main class="container">
 <section class="panel" id="busca">
  <h2>🔎 Buscar passagens</h2>
  <div class="form">
   <label>Origem
    <input id="origem" placeholder="Ex: Dois Vizinhos">
   </label>
   <label>Destino
    <input id="destino" placeholder="Ex: Curitiba">
   </label>
   <label>Data da viagem
    <input type="date" id="data">
   </label>
   <label>Passageiros
    <select id="quantidade">
     <option>1</option><option>2</option>
     <option>3</option><option>4</option>
    </select>
   </label>
  </div>
  <button class="search" onclick="buscar()">BUSCAR PASSAGENS</button>
 </section>

 <section id="results"></section>

 <section class="panel hidden" id="compra">
  <h2>Escolha sua poltrona</h2>
  <p class="small">Poltronas cinzas estão ocupadas. Selecione uma disponível.</p>
  <div class="seats" id="seats"></div>
  <p id="seatInfo">Nenhuma poltrona selecionada.</p>
  <h3>Dados do passageiro</h3>
  <label>Nome completo
   <input id="nome" placeholder="Seu nome">
  </label><br><br>
  <label>CPF
   <input id="cpf" placeholder="000.000.000-00">
  </label><br><br>
  <label>E-mail
   <input id="email" type="email" placeholder="seuemail@email.com">
  </label><br><br>
  <button onclick="finalizar()">CONTINUAR COMPRA</button>
  <p id="confirmacao"></p>
 </section>
</main>

<footer>
 © 2026 Vai de Ônibus — Sua viagem começa aqui!
</footer>

<script>
let preco = 0;
let poltrona = null;
let viagem = {};

document.getElementById('data').min =
 new Date().toISOString().split('T')[0];

function buscar(){
 const origem=document.getElementById('origem').value.trim();
 const destino=document.getElementById('destino').value.trim();
 const data=document.getElementById('data').value;
 const qtd=Number(document.getElementById('quantidade').value);

 if(!origem || !destino || !data){
  alert('Preencha origem, destino e data.');
  return;
 }
 if(origem.toLowerCase()===destino.toLowerCase()){
  alert('Origem e destino precisam ser diferentes.');
  return;
 }
 if(data < document.getElementById('data').min){
  alert('Escolha uma data válida.');
  return;
 }

 viagem={origem,destino,data,qtd};
 preco=129.90;

 document.getElementById('results').innerHTML=`
 <h2>Passagens encontradas</h2>
 <div class="bus">
  <div><b>🚌 Expresso Vai de Ônibus</b>
  <p>${origem} → ${destino}</p>
  <span class="small">08:00 — 16:00 | Executivo</span></div>
  <div><div class="price">R$ 129,90</div>
  <button onclick="escolher()">Escolher</button></div>
 </div>
 <div class="bus">
  <div><b>🚌 Viação Conforto</b>
  <p>${origem} → ${destino}</p>
  <span class="small">14:00 — 22:00 | Convencional</span></div>
  <div><div class="price">R$ 99,90</div>
  <button onclick="escolher(99.90)">Escolher</button></div>
 </div>`;
 document.getElementById('compra').classList.add('hidden');
}

function escolher(valor=129.90){
 preco=valor;
 poltrona=null;
 document.getElementById('compra').classList.remove('hidden');
 document.getElementById('confirmacao').innerText='';
 document.getElementById('seatInfo').innerText='Nenhuma poltrona selecionada.';
 const area=document.getElementById('seats');
 area.innerHTML='';
 for(let i=1;i<=20;i++){
  const b=document.createElement('button');
  b.textContent=i;
  b.className='seat';
  if([3,7,12,16].includes(i)){
   b.classList.add('taken');
   b.disabled=true;
  }else{
   b.onclick=()=>{
    document.querySelectorAll('.seat').forEach(s=>s.classList.remove('selected'));
    b.classList.add('selected');
    poltrona=i;
    document.getElementById('seatInfo').innerText=
      'Poltrona escolhida: '+i+
      ' | Total: R$ '+(preco*viagem.qtd).toFixed(2);
   };
  }
  area.appendChild(b);
 }
 document.getElementById('compra').scrollIntoView({behavior:'smooth'});
}

function finalizar(){
 const nome=document.getElementById('nome').value.trim();
 const cpf=document.getElementById('cpf').value.trim();
 const email=document.getElementById('email').value.trim();

 if(!poltrona){alert('Escolha uma poltrona.');return;}
 if(!nome||!cpf||!email){alert('Preencha todos os dados.');return;}
 if(!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)){
  alert('Digite um e-mail válido.');return;
 }

 document.getElementById('confirmacao').innerHTML=
 '✅ Solicitação de compra registrada na demonstração!<br>'+
 'Passageiro: '+nome+
 '<br>Viagem: '+viagem.origem+' → '+viagem.destino+
 '<br>Poltrona: '+poltrona+
 '<br>Total: R$ '+(preco*viagem.qtd).toFixed(2)+
 '<br><br>Para confirmar a compra, é necessário integrar um sistema de pagamento.';
}
</script>
</body>
</html>
