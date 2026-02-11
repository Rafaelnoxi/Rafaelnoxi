<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rafaelnoxi</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root{
  --bg:#0a0a0a;
  --card:#111111;
  --text:#eaeaea;
  --muted:#888;
  --border:rgba(255,255,255,0.08);
}

*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
}

body{
  background:var(--bg);
  color:var(--text);
  line-height:1.6;
}

.container{
  max-width:1100px;
  margin:auto;
  padding:60px 20px;
}

nav{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:30px 20px;
  max-width:1100px;
  margin:auto;
}

nav a{
  color:var(--muted);
  text-decoration:none;
  margin-left:25px;
  font-size:.9rem;
}

nav a:hover{color:#fff;}

.hero{
  padding:120px 0 80px 0;
}

.hero h1{
  font-size:3rem;
  font-weight:500;
}

.hero p{
  margin-top:20px;
  max-width:600px;
  color:var(--muted);
}

.section{
  padding:100px 0;
  border-top:1px solid var(--border);
}

.section h2{
  font-size:1.6rem;
  margin-bottom:40px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
  gap:30px;
}

.card{
  background:var(--card);
  border:1px solid var(--border);
  padding:30px;
  border-radius:16px;
}

.stat-number{
  font-size:2rem;
}

.stat-label{
  color:var(--muted);
  font-size:.9rem;
}

footer{
  padding:60px 0;
  text-align:center;
  border-top:1px solid var(--border);
  color:var(--muted);
  font-size:.9rem;
}
</style>
</head>

<body>

<nav>
  <strong>Rafaelnoxi</strong>
  <div>
    <a href="#sobre">Sobre</a>
    <a href="#projetos">Projetos</a>
    <a href="#contato">Contato</a>
  </div>
</nav>

<div class="container">

<section class="hero">
  <h1>Desenvolvedor focado em tecnologia moderna.</h1>
  <p>
    Construindo soluções eficientes, minimalistas e escaláveis.
  </p>
</section>

<section id="sobre" class="section">
  <h2>Sobre</h2>
  <p style="max-width:700px;color:#888">
    Desenvolvedor interessado em projetos web, automação e boas práticas.
  </p>
</section>

<section id="projetos" class="section">
  <h2>Estatísticas GitHub</h2>

  <div class="grid" style="margin-bottom:60px">
    <div class="card">
      <div class="stat-number" id="repoCount">0</div>
      <div class="stat-label">Repositórios</div>
    </div>
    <div class="card">
      <div class="stat-number" id="followers">0</div>
      <div class="stat-label">Seguidores</div>
    </div>
    <div class="card">
      <div class="stat-number" id="stars">0</div>
      <div class="stat-label">Estrelas Totais</div>
    </div>
  </div>

  <div class="grid">
    <div class="card">
      <h3>Projetos Relevantes</h3>
      <canvas id="reposChart"></canvas>
    </div>

    <div class="card">
      <h3>Linguagens</h3>
      <canvas id="languagesChart"></canvas>
    </div>
  </div>
</section>

<section id="contato" class="section">
  <h2>Contato</h2>
  <p style="color:#888">
    GitHub: github.com/Rafaelnoxi<br>
    Email: seuemail@email.com
  </p>
</section>

</div>

<footer>
© 2026 Rafaelnoxi
</footer>

<script>
const username="Rafaelnoxi";

async function loadData(){

  const userRes=await fetch(`https://api.github.com/users/${username}`);
  const user=await userRes.json();

  document.getElementById("repoCount").innerText=user.public_repos;
  document.getElementById("followers").innerText=user.followers;

  const reposRes=await fetch(`https://api.github.com/users/${username}/repos?per_page=100`);
  const repos=await reposRes.json();

  let totalStars=0;
  repos.forEach(r=>totalStars+=r.stargazers_count);
  document.getElementById("stars").innerText=totalStars;

  const topRepos=repos.sort((a,b)=>b.stargazers_count-a.stargazers_count).slice(0,5);

  new Chart(document.getElementById("reposChart"),{
    type:"bar",
    data:{
      labels:topRepos.map(r=>r.name),
      datasets:[{data:topRepos.map(r=>r.stargazers_count)}]
    },
    options:{plugins:{legend:{display:false}}}
  });

  const langCount={};
  repos.forEach(r=>{
    if(r.language){
      langCount[r.language]=(langCount[r.language]||0)+1;
    }
  });

  new Chart(document.getElementById("languagesChart"),{
    type:"doughnut",
    data:{
      labels:Object.keys(langCount),
      datasets:[{data:Object.values(langCount)}]
    }
  });

}

loadData();
</script>

</body>
</html>
