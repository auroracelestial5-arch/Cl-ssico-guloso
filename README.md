<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Clássico Guloso</title>

<style>

body{
font-family:Arial;
background:#111;
color:white;
text-align:center;
}

input{
padding:5px;
margin:5px;
}

button{
padding:6px;
margin:5px;
cursor:pointer;
}

.jogo{
background:#222;
margin:5px;
padding:8px;
}

</style>

</head>

<body>

<h1>⚽ Clássico Guloso</h1>

<h2>Adicionar jogo</h2>

<input id="time1" placeholder="Time 1">
<input id="placar1" type="number" placeholder="Gols">

x

<input id="placar2" type="number" placeholder="Gols">
<input id="time2" placeholder="Time 2">

<br>

<button onclick="adicionarJogo()">Adicionar jogo</button>

<h2>Jogos</h2>

<div id="listaJogos"></div>

<h2>Artilheiro</h2>

<input id="nomeArtilheiro" placeholder="Nome do jogador">
<input id="golsArtilheiro" type="number" placeholder="Gols">

<button onclick="salvarArtilheiro()">Salvar artilheiro</button>

<div id="artilheiro"></div>

<h2>Estatísticas</h2>

<div id="estatisticas"></div>

<script>

let jogos = JSON.parse(localStorage.getItem("jogos")) || [];
let artilheiro = JSON.parse(localStorage.getItem("artilheiro")) || null;

function adicionarJogo(){

let time1 = document.getElementById("time1").value;
let time2 = document.getElementById("time2").value;
let placar1 = document.getElementById("placar1").value;
let placar2 = document.getElementById("placar2").value;

if(time1=="" || time2=="") return;

let jogo = {
time1,
time2,
placar1:Number(placar1),
placar2:Number(placar2)
};

jogos.push(jogo);

localStorage.setItem("jogos",JSON.stringify(jogos));

mostrarJogos();
atualizarEstatisticas();

}

function mostrarJogos(){

let lista = document.getElementById("listaJogos");

lista.innerHTML="";

jogos.forEach((jogo,index)=>{

lista.innerHTML+=`
<div class="jogo">
${jogo.time1} ${jogo.placar1} x ${jogo.placar2} ${jogo.time2}

<br>

<button onclick="excluirJogo(${index})">Excluir</button>

</div>
`;

});

}

function excluirJogo(i){

jogos.splice(i,1);

localStorage.setItem("jogos",JSON.stringify(jogos));

mostrarJogos();
atualizarEstatisticas();

}

function atualizarEstatisticas(){

let totalJogos = jogos.length;
let totalGols = 0;

jogos.forEach(j=>{

totalGols += j.placar1 + j.placar2;

});

let media = totalJogos ? (totalGols/totalJogos).toFixed(2) : 0;

document.getElementById("estatisticas").innerHTML=`

Jogos: ${totalJogos} <br>
Gols: ${totalGols} <br>
Média de gols: ${media}

`;

}

function salvarArtilheiro(){

let nome = document.getElementById("nomeArtilheiro").value;
let gols = document.getElementById("golsArtilheiro").value;

artilheiro = {nome,gols};

localStorage.setItem("artilheiro",JSON.stringify(artilheiro));

mostrarArtilheiro();

}

function mostrarArtilheiro(){

if(!artilheiro) return;

document.getElementById("artilheiro").innerHTML=`

🏆 ${artilheiro.nome} - ${artilheiro.gols} gols

`;

}

mostrarJogos();
atualizarEstatisticas();
mostrarArtilheiro();

</script>

</body>
</html>
