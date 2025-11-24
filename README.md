![src/header.png](src/header.png)

<!---
KleberVales/KleberVales is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

<br>

<!-- Typing animation -->
[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=4b0082&center=true&vCenter=true&width=600&lines=Hi+there+👋,+I'm+Kleber+Vales;+Welcome+to+My+Profile!;Over+5+years+of+programming+experience;Always+learning+new+things)](https://git.io/typing-svg)

## 👨‍💻 About Me

Hello!  I'm Kleber Vales, Back-end Software Engineer with solid experience in modern software development. I specialize in building robust, scalable, and efficient applications, applying best practices and leveraging up-to-date technologies to deliver high-quality solutions. Passionate about problem-solving and system design, I enjoy working on projects that combine technical challenges with real-world impact. 

🏆**OCA: Java SE 7 Programmer** 🏆**MTA: Software Development** 🏆**Scrum Certified** 🏆**OCI 2025: DevOps Professional**

### 🎓 Bachelor's Degree in Computer Science  
### 🎓 MBA in Web Software Development 

<br>
Welcome to my profile! 🚀

---

## 📊 GitHub Stats

<p>Follow my journey on GitHub: commits, projects, most used languages, and key moments in my professional development!</p>



<details><summary>Click to expand GitHub Stats 📈</summary>

<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>GitHub Card — sem "tempo no GitHub"</title>
  <style>
    :root{--bg:#0b1220;--card:#0f1724;--muted:#94a3b8;--accent:#2dd4bf}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,Arial;background:linear-gradient(180deg,#071024 0%, #081127 100%);display:grid;place-items:center;color:#e6eef6}
    .card{width:420px;padding:20px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));box-shadow:0 8px 30px rgba(2,6,23,0.6);border-radius:14px;border:1px solid rgba(255,255,255,0.03)}
    .header{display:flex;gap:14px;align-items:center}
    .avatar{width:72px;height:72px;border-radius:12px;flex:0 0 72px;background:#071024;overflow:hidden}
    .avatar img{width:100%;height:100%;object-fit:cover}
    .meta{flex:1}
    .name{font-size:18px;font-weight:700}
    .login{color:var(--muted);font-size:13px;margin-top:4px}
    .bio{margin-top:12px;color:var(--muted);font-size:13px}
    .stats{display:flex;gap:10px;margin-top:16px}
    .stat{flex:1;background:rgba(255,255,255,0.02);padding:12px;border-radius:10px;text-align:center}
    .stat .num{font-weight:700;font-size:16px}
    .stat .label{font-size:12px;color:var(--muted);margin-top:6px}
    .actions{display:flex;gap:8px;margin-top:16px}
    .btn{flex:1;padding:10px;border-radius:10px;border:0;background:transparent;color:var(--accent);cursor:pointer;font-weight:600}
    .small{font-size:12px;color:var(--muted);margin-top:8px;text-align:center}
    .inputRow{display:flex;gap:8px;margin-bottom:12px}
    .inputRow input{flex:1;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit}
    .spinner{width:18px;height:18px;border:2px solid rgba(255,255,255,0.08);border-top-color:var(--accent);border-radius:50%;animation:spin 1s linear infinite}
    @keyframes spin{to{transform:rotate(360deg)}}
    .error{color:#ffb4b4;background:rgba(255,180,180,0.05);padding:8px;border-radius:8px;margin-top:10px;font-size:13px}
  </style>
</head>
<body>
  <div class="card" id="card">
    <div class="inputRow">
      <input id="username" placeholder="Informe o usuário (ex: klebervales)" value="klebervales" />
      <button class="btn" id="loadBtn">Carregar</button>
    </div>

    <div id="content">
      <div class="header">
        <div class="avatar" id="avatar"><img src="" alt="avatar" /></div>
        <div class="meta">
          <div class="name" id="name">—</div>
          <div class="login" id="login">@—</div>
        </div>
      </div>

      <div class="bio" id="bio">Perfil GitHub</div>

      <div class="stats">
        <div class="stat">
          <div class="num" id="repos">—</div>
          <div class="label">Repositórios públicos</div>
        </div>
        <div class="stat">
          <div class="num" id="followers">—</div>
          <div class="label">Seguidores</div>
        </div>
        <div class="stat">
          <div class="num" id="stars">—</div>
          <div class="label">Estrelas totais</div>
        </div>
      </div>

      <div class="actions">
        <button class="btn" id="viewProfile">Ver no GitHub</button>
        <button class="btn" id="refresh">Atualizar</button>
      </div>

      <div class="small">Este cartão NÃO exibe há quanto tempo você está no GitHub (data de criação omitida).</div>
      <div id="error" style="display:none" class="error"></div>
    </div>
  </div>

  <script>
    const el = id => document.getElementById(id)
    const usernameInput = el('username')
    const loadBtn = el('loadBtn')
    const avatarImg = el('avatar').querySelector('img')
    const nameEl = el('name'), loginEl = el('login'), bioEl = el('bio')
    const reposEl = el('repos'), followersEl = el('followers'), starsEl = el('stars')
    const viewBtn = el('viewProfile'), refreshBtn = el('refresh'), errorEl = el('error')

    async function fetchUser(user){
      const userRes = await fetch(`https://api.github.com/users/${user}`)
      if(!userRes.ok) throw new Error('Usuário não encontrado')
      return userRes.json()
    }

    async function fetchRepos(user, page=1, per_page=100){
      const res = await fetch(`https://api.github.com/users/${user}/repos?per_page=${per_page}&page=${page}`)
      if(!res.ok) throw new Error('Erro ao buscar repositórios')
      return res.json()
    }

    async function load(user){
      errorEl.style.display='none'
      // show spinner on load button
      loadBtn.disabled=true
      const orig = loadBtn.textContent
      loadBtn.innerHTML = '<span class="spinner"></span>'
      try{
        const u = await fetchUser(user)
        avatarImg.src = u.avatar_url
        nameEl.textContent = u.name || u.login
        loginEl.textContent = '@' + u.login
        bioEl.textContent = u.bio || '—'
        reposEl.textContent = u.public_repos
        followersEl.textContent = u.followers

        // sum stars (first 300 repos, 3 pages of 100)
        let totalStars = 0
        for(let p=1;p<=3;p++){
          const r = await fetchRepos(user,p)
          if(r.length===0) break
          totalStars += r.reduce((s,repo)=>s+ (repo.stargazers_count||0),0)
          if(r.length < 100) break
        }
        starsEl.textContent = totalStars

        viewBtn.onclick = ()=> window.open(u.html_url,'_blank')

      }catch(err){
        errorEl.style.display='block'
        errorEl.textContent = err.message
      }finally{
        loadBtn.disabled=false
        loadBtn.textContent = orig
      }
    }

    loadBtn.addEventListener('click', ()=> load(usernameInput.value.trim()||'klebervales'))
    refreshBtn.addEventListener('click', ()=> load(usernameInput.value.trim()||'klebervales'))

    // load default
    load(usernameInput.value.trim())
  </script>
</body>
</html>


---

### 🔄 DevOps & CI/CD
<img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions" /> <img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" alt="Terraform" /> <img src="https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white" alt="Jenkins" /> <img src="https://img.shields.io/badge/Cloud_DevOps-0078D7?style=for-the-badge&logo=clouddevops&logoColor=white" alt="Cloud DevOps" /> <img src="https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white" alt="Ansible" /> <img src="https://img.shields.io/badge/Helm-0F1689?style=for-the-badge&logo=helm&logoColor=white" alt="Helm" /> <img src="https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white" alt="Prometheus" /> <img src="https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white" alt="Grafana" />

### 💻 Languages  
<img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=OpenJDK&logoColor=white" alt="Java" /> <img src="https://img.shields.io/badge/JavaScript-FFFF00?style=for-the-badge&logo=JavaScript&logoColor=black" alt="JavaScript" /> <img src="https://img.shields.io/badge/Kotlin-7F52FF?style=for-the-badge&logo=Kotlin&logoColor=white" alt="Kotlin" />

### 🧩 Frameworks 
<img src="https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=Spring%20Boot&logoColor=white" alt="Spring Boot" /> <img src="https://img.shields.io/badge/Hibernate-59666C?style=for-the-badge&logo=Hibernate&logoColor=white" alt="Hibernate" /> <!-- JavaFX --> <img src="https://img.shields.io/badge/JavaFX-1C2833?style=for-the-badge&logo=OpenJFX&logoColor=white" alt="JavaFX" /> <!-- JUnit --> <img src="https://img.shields.io/badge/JUnit-25A162?style=for-the-badge&logo=JUnit5&logoColor=white" alt="JUnit" /> <!-- Mockito --> <img src="https://img.shields.io/badge/Mockito-45C6B0?style=for-the-badge&logo=Mockito&logoColor=white" alt="Mockito" /> <!-- React --> <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=React&logoColor=61DAFB" alt="React" />

### 🛢️ Databases  
<img src="https://img.shields.io/badge/MySQL-0000FF?style=for-the-badge&logo=MySQL&logoColor=white" alt="MySQL" /> <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" /> <img src="https://img.shields.io/badge/MongoDB-32CD32?style=for-the-badge&logo=MongoDB&logoColor=white" alt="MongoDB" /> <img src="https://img.shields.io/badge/SQLite-000080?style=for-the-badge&logo=SQLite&logoColor=white" alt="SQLite" /> 

### 📡 Messaging and Communication
<img src="https://img.shields.io/badge/Apache_Kafka-231F20?style=for-the-badge&logo=Apache%20Kafka&logoColor=white" alt="Apache Kafka" /> <img src="https://img.shields.io/badge/RESTful_APIs-6E6E6E?style=for-the-badge&logo=api&logoColor=white" alt="RESTful APIs" /> <img src="https://img.shields.io/badge/AWS_SDK_for_Java-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS SDK for Java" />

### 🔧 Tools  
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=Docker&logoColor=white" alt="Docker" /> <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=Kubernetes&logoColor=white" alt="Kubernetes" /> <img src="https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=Gradle&logoColor=white" alt="Gradle" /> <img src="https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=Postman&logoColor=white" alt="Postman" /> <img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=Git&logoColor=white" alt="Git" /> <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=GitHub&logoColor=white" alt="GitHub" />

### 🎨 Design Patterns
<img src="https://img.shields.io/badge/Dependency%20Injection-026E82?style=for-the-badge&logo=spring&logoColor=white" alt="Dependency Injection" /> <img src="https://img.shields.io/badge/Singleton%20Pattern-367D89?style=for-the-badge&logo=apache&logoColor=white" alt="Singleton Pattern" /> <img src="https://img.shields.io/badge/Factory%20Method%20Pattern-4CAF50?style=for-the-badge&logo=java&logoColor=white" alt="Factory Method Pattern" /> <img src="https://img.shields.io/badge/Observer%20Pattern-FF9800?style=for-the-badge&logo=ReactiveX&logoColor=white" alt="Observer Pattern" /> <img src="https://img.shields.io/badge/Repository%20Pattern-3F51B5?style=for-the-badge&logo=java&logoColor=white" alt="Repository Pattern" />

### 📋 Project Management
<img src="https://img.shields.io/badge/PMBOK_(PMI)-026E82?style=for-the-badge&logo=BookStack&logoColor=white" alt="PMBOK (PMI)" /> <img src="https://img.shields.io/badge/🌀_Scrum_Framework-6DB33F?style=for-the-badge" alt="Scrum Framework" />

---

## 📚 Courses and Improvements

![badges_alura.png](src/badges_alura.png)

---


## 🏆 HackerRank Profile Trophy

<p align="center"> 
  <a href="https://www.hackerrank.com/klebervales" target="_blank" rel="noopener noreferrer">
    <img src="./src/badges_hackerrank.png" alt="HackerRank Badges" width="57%">
  </a>
  <a href="https://www.hackerrank.com/klebervales" target="_blank" rel="noopener noreferrer">
    <img src="./src/hackerrank-logo.jpg" alt="HackerRank Logo" width="30%">
  </a>
</p>

---

## 🚀 GitHub Trophies

[![GitHub Trophy](https://github-profile-trophy.vercel.app/?username=klebervales)](https://github.com/ryo-ma/github-profile-trophy)

---

## 📈 GitHub Contribution Graph

![GitHub Activity Graph](https://raw.githubusercontent.com/BEPb/BEPb/output/github-contribution-grid-snake.svg)

---

[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=4b0082&center=true&vCenter=true&width=600&lines=Thanks+for+visiting)](https://git.io/typing-svg)

---
<br>


![src/footer.png](src/footer.png)

<br>
