# college-news-sharing-system
//A web based open-source platform to post and view college news updates
<!doctype html>
<html lang="en">
<head>
  <title>College News Sharing System</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg1:#0f172a;
      --bg2:#071124;
      --card:#0b1220;
      --accent:#7c3aed;
      --accent-2:#06b6d4;
      --glass: rgba(255,255,255,0.04);
      --muted: rgba(255,255,255,0.7);
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{background: radial-gradient(1200px 600px at 10% 10%, rgba(124,58,237,0.12), transparent),
                 radial-gradient(1000px 500px at 90% 90%, rgba(6,182,212,0.08), transparent),
                 linear-gradient(180deg,var(--bg1),var(--bg2));
         color:#e6eef8; -webkit-font-smoothing:antialiased; padding:40px;}

    /* floating decorative blobs */
    .blob{position:fixed;z-index:0;border-radius:50%;filter:blur(60px);opacity:.55}
    .blob.one{width:420px;height:420px;left:-120px;top:-80px;background:linear-gradient(45deg,var(--accent),transparent)}
    .blob.two{width:320px;height:320px;right:-100px;bottom:-120px;background:linear-gradient(45deg,var(--accent-2),transparent)}

    .container{position:relative;z-index:2;max-width:1100px;margin:0 auto;display:grid;grid-template-columns:420px 1fr;gap:28px}

    /* Header card */
    header{display:flex;align-items:center;gap:16px;padding:20px;border-radius:16px;background:linear-gradient(135deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));box-shadow:0 10px 30px rgba(2,6,23,0.6)}
    .logo{width:72px;height:72px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent-2));display:flex;align-items:center;justify-content:center;font-weight:800;font-size:28px;color:white}
    .title h1{margin:0;font-size:20px;letter-spacing:-0.3px}
    .title p{margin:2px 0 0;color:var(--muted);font-size:13px}

    /* Form */
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:18px;border-radius:14px;border:1px solid rgba(255,255,255,0.03);box-shadow: 0 6px 18px rgba(2,6,23,0.6)}
    form{display:flex;flex-direction:column;gap:12px}
    .field{position:relative}
    label.floating{position:absolute;left:12px;top:10px;pointer-events:none;transition:transform .18s ease, font-size .18s ease, opacity .18s ease;color:rgba(255,255,255,0.6)}
    input[type="text"], input[type="date"], textarea{width:100%;padding:16px 12px;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.04);color:inherit;font-size:14px;outline:none}
    textarea{min-height:120px;resize:vertical}
    /* when focus or has text */
    input:focus + label.floating, textarea:focus + label.floating, input:not(:placeholder-shown) + label.floating, textarea:not(:placeholder-shown) + label.floating{transform:translateY(-26px) scale(.95);font-size:12px;opacity:0.95}

    .submit-row{display:flex;gap:8px;align-items:center}
    button.primary{background:linear-gradient(90deg,var(--accent),var(--accent-2));border:none;padding:10px 14px;border-radius:10px;color:white;font-weight:600;cursor:pointer;box-shadow:0 8px 20px rgba(7,16,34,0.5);transition:transform .14s ease}
    button.primary:active{transform:translateY(2px)}
    button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px 12px;border-radius:10px;color:var(--muted);cursor:pointer}

    /* posts area */
    .posts{display:grid;gap:14px}
    .post{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:16px;border-radius:12px;border:1px solid rgba(255,255,255,0.03);display:flex;flex-direction:column;gap:8px;animation:cardIn .4s ease both}
    .post .meta{display:flex;justify-content:space-between;align-items:center}
    .post .meta .t{font-weight:700}
    .desc{color:rgba(230,238,248,0.92);line-height:1.45}
    .small{font-size:13px;color:var(--muted)}

    .controls{display:flex;gap:8px}
    .icon-btn{background:transparent;border:1px solid rgba(255,255,255,0.035);padding:6px 8px;border-radius:8px;color:var(--muted);cursor:pointer}

    /* hover effects */
    .post:hover{transform:translateY(-6px);transition:transform .18s ease, box-shadow .18s ease;box-shadow:0 18px 40px rgba(2,6,23,0.6)}

    @keyframes cardIn{from{opacity:0;transform:translateY(10px) scale(.995)}to{opacity:1;transform:none}}

    /* empty state */
    .empty{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px;border-radius:12px;border:1px dashed rgba(255,255,255,0.04);color:var(--muted)}

    /* responsive */
    @media (max-width:980px){.container{grid-template-columns:1fr;}}

    /* subtle marquee for header */
    .marquee{overflow:hidden;white-space:nowrap}
    .marquee span{display:inline-block;padding-right:40px;animation:mar 12s linear infinite}
    @keyframes mar{from{transform:translateX(0)}to{transform:translateX(-100%)}}

    /* animated textbox glow */
    .textbox-glow{position:relative}
    .textbox-glow:after{content:'';position:absolute;inset:-2px;border-radius:12px;background:linear-gradient(90deg,var(--accent),var(--accent-2));filter:blur(8px);opacity:0;transition:opacity .18s ease}
    input:focus ~ .textbox-glow:after, textarea:focus ~ .textbox-glow:after{opacity:0.6}

    /* small nice footer */
    footer{margin-top:18px;color:var(--muted);font-size:13px;text-align:center}
  </style>
</head>
<body>
  <div class="blob one" aria-hidden></div>
  <div class="blob two" aria-hidden></div>

  <div class="container">
    <div>
      <header class="card" style="margin-bottom:18px">
        <div class="logo">CS</div>
        <div class="title">
          <h1>College News Hub</h1>
          <p>Share announcements, results, events — instant & open</p>
        </div>
      </header>

      <section class="card">
        <form id="newsForm" autocomplete="off">
          <div class="field">
            <input id="title" type="text" placeholder=" " required />
            <label class="floating">Title</label>
            <div class="textbox-glow"></div>
          </div>

          <div class="field">
            <input id="date" type="date" placeholder=" " required />
            <label class="floating">Date</label>
            <div class="textbox-glow"></div>
          </div>

          <div class="field">
            <textarea id="description" placeholder=" " required></textarea>
            <label class="floating">Description</label>
            <div class="textbox-glow"></div>
          </div>

          <div class="submit-row">
            <button class="primary" type="submit">Post News</button>
            <button class="ghost" type="button" id="clearBtn">Clear</button>
            <div style="margin-left:auto;color:var(--muted);font-size:13px">Local only • Saved in your browser</div>
          </div>
        </form>
      </section>

      <footer>Built for college use • Edit & delete supported • Data stored in LocalStorage</footer>
    </div>

    <main>
      <div class="card" style="margin-bottom:14px;display:flex;flex-direction:column">
        <div style="display:flex;align-items:center;justify-content:space-between">
          <div>
            <h2 style="margin:0">Latest Updates</h2>
            <p class="small" style="margin:6px 0 0">Showing all posts (newest first)</p>
          </div>
          <div class="marquee small" style="width:48%;text-align:right"><span>🎓 Welcome to the College News Hub — post responsibly • Be kind and accurate</span></div>
        </div>

        <div id="postsWrap" style="margin-top:12px" class="posts"></div>
      </div>
    </main>
  </div>

  <script>
    // Storage key
    const STORAGE_KEY = 'collegeNewsPosts_v1';

    // Utils
    function uid(){return Math.random().toString(36).slice(2,9)}
    function formatDateISO(d){
      const dt = new Date(d);
      if (isNaN(dt)) return '';
      return dt.toLocaleDateString(undefined,{year:'numeric',month:'short',day:'numeric'});
    }

    // DOM
    const newsForm = document.getElementById('newsForm');
    const titleEl = document.getElementById('title');
    const dateEl = document.getElementById('date');
    const descEl = document.getElementById('description');
    const postsWrap = document.getElementById('postsWrap');
    const clearBtn = document.getElementById('clearBtn');

    let posts = [];
    let editId = null;

    // Initialize
    (function init(){
      // set date default to today
      const today = new Date().toISOString().slice(0,10);
      dateEl.value = today;

      loadPosts();
      renderPosts();
    })();

    // Load & Save
    function loadPosts(){
      try{
        const raw = localStorage.getItem(STORAGE_KEY);
        posts = raw ? JSON.parse(raw) : [];
      }catch(e){posts = []}
    }
    function savePosts(){
      localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
    }

    // Render
    function renderPosts(){
      postsWrap.innerHTML = '';
      if (!posts.length){
        const empty = document.createElement('div');
        empty.className = 'empty';
        empty.innerHTML = '<strong>No news yet</strong><div style="margin-top:8px;color:var(--muted)">Post the first update using the form on the left.</div>';
        postsWrap.appendChild(empty);
        return;
      }

      // sort newest first (by date then createdAt)
      const sorted = posts.slice().sort((a,b)=>{
        const da = new Date(a.date).getTime() || 0;
        const db = new Date(b.date).getTime() || 0;
        if (db !== da) return db - da;
        return (b.createdAt||0) - (a.createdAt||0);
      });

      sorted.forEach(p => postsWrap.appendChild(postCard(p)));
    }

    function postCard(p){
      const wrap = document.createElement('article');
      wrap.className = 'post';

      const meta = document.createElement('div'); meta.className='meta';
      const left = document.createElement('div'); left.innerHTML = <div class="t">${escapeHtml(p.title)}</div><div class="small">${formatDateISO(p.date)}</div>;
      const right = document.createElement('div');

      const controls = document.createElement('div'); controls.className='controls';
      const editBtn = document.createElement('button'); editBtn.className='icon-btn'; editBtn.textContent='Edit';
      const delBtn = document.createElement('button'); delBtn.className='icon-btn'; delBtn.textContent='Delete';

      editBtn.onclick = ()=> startEdit(p.id);
      delBtn.onclick = ()=> removePost(p.id);

      controls.appendChild(editBtn); controls.appendChild(delBtn);
      right.appendChild(controls);

      meta.appendChild(left); meta.appendChild(right);

      const desc = document.createElement('div'); desc.className='desc'; desc.innerText = p.description;

      wrap.appendChild(meta);
      wrap.appendChild(desc);

      // subtle animation delay based on createdAt
      if (p.createdAt){
        const t = (p.createdAt % 1000) / 1000; // 0-1
        wrap.style.animationDelay = (t/4)+'s';
      }

      return wrap;
    }

    // Add / Edit
    newsForm.addEventListener('submit', e=>{
      e.preventDefault();
      const title = titleEl.value.trim();
      const date = dateEl.value;
      const desc = descEl.value.trim();
      if (!title || !date || !desc) return alert('Please fill all fields');

      if (editId){
        const idx = posts.findIndex(x => x.id === editId);
        if (idx>-1){
          posts[idx].title = title;
          posts[idx].date = date;
          posts[idx].description = desc;
          posts[idx].updatedAt = Date.now();
          savePosts();
          editId = null;
          newsForm.querySelector('button.primary').textContent = 'Post News';
          clearForm();
          renderPosts();
          return;
        }
      }

      const newPost = {id: uid(), title, date, description: desc, createdAt: Date.now()};
      posts.push(newPost);
      savePosts();
      clearForm();
      renderPosts();

      // tiny success micro-interaction
      const btn = newsForm.querySelector('button.primary');
      btn.animate([{transform:'scale(1.02)'},{transform:'scale(1)'}],{duration:250});
    });

    function startEdit(id){
      const p = posts.find(x=>x.id===id);
      if(!p) return;
      editId = id;
      titleEl.value = p.title;
      dateEl.value = p.date;
      descEl.value = p.description;
      newsForm.querySelector('button.primary').textContent = 'Save Changes';
      // move focus
      titleEl.focus();
      // scroll into view for smaller screens
      titleEl.scrollIntoView({behavior:'smooth',block:'center'});
    }

    function removePost(id){
      if (!confirm('Delete this post?')) return;
      posts = posts.filter(x=>x.id!==id);
      savePosts();
      renderPosts();
    }

    clearBtn.addEventListener('click', ()=>{
      if (!confirm('Clear all saved news from your browser?')) return;
      posts = [];
      savePosts();
      renderPosts();
    });

    function clearForm(){
      titleEl.value = '';
      // keep date as today
      dateEl.value = new Date().toISOString().slice(0,10);
      descEl.value = '';
      editId = null;
      newsForm.querySelector('button.primary').textContent = 'Post News';
    }

    // small helper to avoid HTML injection
    function escapeHtml(str){
      return String(str).replace(/[&<>\"']/g, function(m){return {'&':'&amp;','<':'&lt;','>':'&gt;', '"':'&quot;', "'":'&#39;'}[m]});
    }

  </script>
</body>
</html>
