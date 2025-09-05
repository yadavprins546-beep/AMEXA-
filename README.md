<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Amexa Grocery — Single-file Advanced PWA</title>
<meta name="theme-color" content="#16a34a">
<style>
:root{
  --bg:#f7fff5; --card:#fff; --text:#06221b;
  --accent:#16a34a; --accent2:#f6d84a; --muted:#566b61; --line:#e6efe9;
  --panel-shadow:0 10px 28px rgba(6,40,20,0.08);
  --radius:12px;
  --max-width:1100px;
}
*{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial}
body{margin:0;background:var(--bg);color:var(--text);-webkit-font-smoothing:antialiased}
.container{max-width:var(--max-width);margin:12px auto;padding:12px}
.header{display:flex;align-items:center;gap:12px}
.brandLogo{width:64px;height:64px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent2));display:flex;align-items:center;justify-content:center;color:#fff;font-weight:800;font-size:26px;overflow:hidden}
.title{font-size:18px;font-weight:700}
.subtitle{color:var(--muted);font-size:13px}
.topbar{display:flex;gap:8px;align-items:center;margin:12px 0}
.search{flex:1;display:flex;gap:8px}
.input{padding:10px;border-radius:10px;border:1px solid var(--line);width:100%}
.btn{background:var(--accent);color:#fff;padding:10px 12px;border-radius:10px;border:0;cursor:pointer}
.btn.ghost{background:#fff;color:var(--accent);border:1px solid var(--line)}
.layout{display:grid;grid-template-columns:1fr 380px;gap:12px}
@media(max-width:920px){ .layout{grid-template-columns:1fr} .rightPanel{order:-1} }
.card{background:var(--card);border-radius:var(--radius);padding:12px;box-shadow:var(--panel-shadow);border:1px solid var(--line)}
.tabs{display:flex;gap:8px;overflow:auto;margin-bottom:10px}
.tab{padding:8px 10px;background:#fff;border-radius:999px;border:1px solid var(--line);cursor:pointer;white-space:nowrap}
.tab.active{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#fff}
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
@media(max-width:600px){ .grid{grid-template-columns:1fr} }
.prod{border-radius:12px;overflow:hidden;border:1px solid var(--line);background:#fff;display:flex}
.prod .img{width:40%;min-height:120px;background:#f1f6f2;display:flex;align-items:center;justify-content:center}
.prod .meta{padding:10px;width:60%}
.mrp{text-decoration:line-through;color:var(--muted);font-size:13px}
.price{font-weight:700;font-size:16px}
.small{font-size:13px;color:var(--muted)}
.cartList{max-height:320px;overflow:auto}
.tableRow{display:flex;justify-content:space-between;align-items:center;padding:8px;border-bottom:1px dashed var(--line)}
.iconBtn{background:#fff;border:1px solid var(--line);padding:6px;border-radius:8px;cursor:pointer}
.footer{margin-top:12px;text-align:center;color:var(--muted);font-size:13px}
.bottomNav{position:fixed;right:12px;left:12px;bottom:12px;background:var(--card);border-radius:16px;padding:8px;display:flex;justify-content:space-around;align-items:center;box-shadow:var(--panel-shadow);border:1px solid var(--line);max-width:520px;margin:0 auto;z-index:60;display:none}
.navBtn{display:flex;flex-direction:column;align-items:center;font-size:12px;color:var(--muted);cursor:pointer}
.navBtn.active{color:var(--accent)}
.profileSheet{position:fixed;left:0;right:0;bottom:-100%;background:#fff;border-radius:12px 12px 0 0;box-shadow:var(--panel-shadow);padding:12px;transition:bottom .28s;z-index:70}
.profileSheet.active{bottom:0}
.profileItem{padding:10px;border-bottom:1px dashed var(--line);cursor:pointer}
.modal{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.35);z-index:80;display:none}
.adminPanel{width:980px;max-width:95vw;background:#fff;border-radius:12px;padding:12px;box-shadow:var(--panel-shadow);border:1px solid var(--line);max-height:85vh;overflow:auto}
.formGrid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.kv{display:flex;justify-content:space-between;padding:8px 0;border-bottom:1px dashed var(--line)}
.saleBanner{background:linear-gradient(90deg,#fff2c2,#f6d84a);padding:12px;border-radius:10px;margin:10px 0;display:flex;justify-content:space-between;align-items:center}
.smallNote{font-size:12px;color:var(--muted)}
.input-file{border:1px dashed var(--line);padding:8px;border-radius:8px;background:#fcfff8}
.note{font-size:13px;color:var(--muted)}
.uploader{display:flex;gap:8px;align-items:center}
.table{width:100%;border-collapse:collapse}
.table td, .table th{padding:8px;border-bottom:1px dashed var(--line)}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <div id="brandLogo" class="brandLogo">A</div>
    <div style="flex:1">
      <div class="title">Amexa Grocery <span id="greeting" style="font-size:14px;color:var(--muted);margin-left:8px"></span></div>
      <div class="subtitle">Smart • Local • Raebareli Only</div>
    </div>
    <div style="display:flex;gap:8px;align-items:center">
      <button id="btnOpenAdmin" class="btn ghost">Admin</button>
      <button id="btnOpenLogin" class="btn">Login</button>
    </div>
  </div>

  <!-- Login -->
  <div id="loginCard" class="card" style="max-width:480px">
    <h3 style="margin:0 0 8px 0">Login / Register</h3>
    <div class="small note">Mobile OTP simulated + Raebareli PIN required</div>
    <div style="margin-top:8px" class="formGrid">
      <input id="mobile" class="input" placeholder="10-digit mobile" maxlength="10">
      <input id="pincode" class="input" placeholder="6-digit PIN (Raebareli)" maxlength="6">
    </div>
    <div style="margin-top:8px;display:flex;gap:8px">
      <button class="btn" onclick="handleLogin()">Continue</button>
      <button class="btn ghost" onclick="prefillAdmin()">Use demo admin</button>
    </div>
    <div id="loginMsg" class="small" style="margin-top:8px;color:crimson"></div>
  </div>

  <!-- Main view -->
  <div id="mainView" style="display:none;margin-top:12px">
    <div id="saleBannerArea"></div>

    <div class="topbar">
      <div style="width:220px"><select id="deliveryMode" class="input"><option value="30">Delivery: 30 minutes</option></select></div>
      <div class="search"><input id="q" class="input" placeholder="Search products or categories..."><button class="btn" onclick="runSearch()">Search</button></div>
      <div style="display:flex;align-items:center;gap:8px">
        <img src="https://cdn-icons-png.flaticon.com/512/1170/1170576.png" width="22">
        <div>Coins: <b id="coins">0</b></div>
      </div>
    </div>

    <div class="layout">
      <!-- left -->
      <div>
        <div class="tabs" id="tabs"></div>
        <div id="grid" class="grid"></div>
        <div id="ordersArea" style="margin-top:12px"></div>
      </div>

      <!-- right -->
      <div class="card rightPanel">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div><b>Cart</b></div>
          <div><button class="btn ghost" onclick="clearCart()">Clear</button></div>
        </div>
        <div id="cartArea" class="cartList" style="margin-top:8px"></div>
        <div id="priceBreak" style="margin-top:10px"></div>
        <div style="margin-top:10px"><button class="btn" onclick="checkout()">Place Order (COD)</button></div>
      </div>
    </div>
  </div>

  <div class="footer">Amexa — Demo • LocalStorage-based • For production use server & HTTPS</div>
</div>

<!-- bottom nav -->
<div id="bottomNav" class="bottomNav">
  <div id="navOrders" class="navBtn" onclick="showOrders(); setActiveNav('navOrders')">📦<div>Orders</div></div>
  <div id="navCart" class="navBtn active" onclick="openCart(); setActiveNav('navCart')">🛒<div>Cart</div></div>
  <div id="navProfile" class="navBtn" onclick="openProfile(); setActiveNav('navProfile')">👤<div>Profile</div></div>
  <div id="navSale" class="navBtn" onclick="openSalePage(); setActiveNav('navSale')">🔥<div>Sale</div></div>
</div>

<!-- profile sheet -->
<div id="profileSheet" class="profileSheet">
  <h3>Profile</h3>
  <div class="kv"><div><b id="pfName">—</b><div class="small" id="pfPhone">—</div></div><div><button class="btn ghost" onclick="logout()">Logout</button></div></div>

  <div style="margin-top:8px">
    <h4>Personal Details</h4>
    <div style="display:flex;gap:8px;align-items:center"><input id="editName" class="input" placeholder="Name"><input id="editEmail" class="input" placeholder="Email"></div>
    <button class="btn" style="margin-top:8px" onclick="savePersonal()">Save</button>
  </div>

  <div style="margin-top:12px">
    <h4>Addresses</h4>
    <div id="addrList"></div>
    <div style="margin-top:8px"><input id="newAddr" class="input" placeholder="Add address"><button class="btn" style="margin-top:6px" onclick="addAddress()">Add</button></div>
  </div>

  <div style="margin-top:12px">
    <h4>Cards</h4>
    <div id="cardsList" class="small">None</div>
    <input id="newCard" class="input" placeholder="Card (xxxx-xxxx-1234)" style="margin-top:8px"><button class="btn" style="margin-top:6px" onclick="addCard()">Add Card</button>
  </div>

  <div style="margin-top:12px">
    <h4>Help & Support</h4>
    <div class="small">Phone: <b>8081408019</b></div>
    <div class="small">Email: <b>yadavprins546@gmail.com</b></div>
    <div style="margin-top:8px"><button class="btn ghost" onclick="openAdminModal()">Admin Login</button></div>
  </div>
</div>

<!-- admin modal -->
<div id="adminModal" class="modal">
  <div class="adminPanel">
    <h3>Admin — Sign In / Panel</h3>
    <div class="small">Enter admin phone (pre-approved) or manage admins using owner password</div>

    <div style="margin-top:8px" class="formGrid">
      <input id="adminPhone" class="input" placeholder="Admin phone (10 digits)">
      <input id="ownerPassForAdd" class="input" placeholder="Owner password (for adding admins)">
    </div>

    <div style="margin-top:8px;display:flex;gap:8px">
      <button class="btn" onclick="adminSignIn()">Sign In</button>
      <button class="btn ghost" onclick="closeAdminModal()">Close</button>
    </div>

    <div id="adminMsg" class="small" style="margin-top:8px;color:crimson"></div>

    <div id="adminPanelArea" style="display:none;margin-top:12px">
      <h4>Admin Panel</h4>
      <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:8px">
        <button class="btn" onclick="showAdminSection('logo')">Logo & Theme</button>
        <button class="btn" onclick="showAdminSection('products')">Products</button>
        <button class="btn" onclick="showAdminSection('categories')">Categories</button>
        <button class="btn ghost" onclick="showAdminSection('orders')">Orders</button>
        <button class="btn ghost" onclick="showAdminSection('users')">Users</button>
        <button class="btn ghost" onclick="showAdminSection('admins')">Admins</button>
        <button class="btn ghost" onclick="showAdminSection('sale')">Sale</button>
      </div>
      <div id="adminSection"></div>
    </div>
  </div>
</div>

<script>
/* =========================
   Amexa Single-file App JS
   ========================= */

/* ---------- localStorage helpers ---------- */
function lsGet(k){ try{ return JSON.parse(localStorage.getItem(k)) }catch(e){ return null } }
function lsSet(k,v){ localStorage.setItem(k, JSON.stringify(v)) }

/* ---------- constants & defaults ---------- */
const OWNER_PASS = 'amexa-owner'
const PRE_APPROVED_ADMINS = ['8081408019','9839731130','9579241364','9511120372']
const ALLOWED_PINCODES = ['229001','229002','229201','229301','229401']

let DB = lsGet('amx_db') || {
  cats:[
    {id:'grocery',name:'Grocery',icon:'https://cdn-icons-png.flaticon.com/512/716/716784.png',color:'#16a34a'},
    {id:'snacks',name:'Snacks',icon:'https://cdn-icons-png.flaticon.com/512/1631/1631630.png',color:'#f97316'},
    {id:'dairy',name:'Dairy',icon:'https://cdn-icons-png.flaticon.com/512/135/135763.png',color:'#0ea5e9'},
    {id:'monthly',name:'Monthly Pack',icon:'https://cdn-icons-png.flaticon.com/512/833/833314.png',color:'#f59e0b'}
  ],
  products:[
    {id:'p1',name:'Aashirvaad Atta 5kg',cat:'grocery',mrp:320,price:285,gst:1,plat:2,delivery:10,desc:'Premium wheat atta'},
    {id:'p2',name:'Fortune Oil 1L',cat:'grocery',mrp:180,price:155,gst:1,plat:2,delivery:15,desc:'Refined oil'},
    {id:'p3',name:'Amexa Mixture 400g',cat:'snacks',mrp:110,price:85,gst:1,plat:3,delivery:7,desc:'Tasty mixture'},
    {id:'p4',name:'Monthly Veg Pack',cat:'monthly',mrp:850,price:799,gst:1,plat:0,delivery:0,desc:'Monthly veg pack',isPack:true,components:['p1','p3']}
  ],
  users:{}, orders:{}, admins: PRE_APPROVED_ADMINS.slice(),
  settings:{logoData:null,theme:{accent:'#16a34a',accent2:'#f6d84a'}}, sale:null
}
lsSet('amx_db', DB)

/* runtime */
let CURRENT_USER = lsGet('amx_user') || null
let CART = lsGet('amx_cart') || {}
let ACTIVE_CAT = 'grocery'
let CURRENT_ADMIN = null

/* helpers */
function money(v){ return '₹'+Math.round(v) }

/* ---------- theme/logo ---------- */
function applyTheme(){
  const t = DB.settings.theme || {}
  if(t.accent) document.documentElement.style.setProperty('--accent', t.accent)
  if(t.accent2) document.documentElement.style.setProperty('--accent2', t.accent2)
  const brandLogo = document.getElementById('brandLogo')
  if(DB.settings.logoData){
    brandLogo.innerHTML = `<img src="${DB.settings.logoData}" style="width:100%;height:100%;object-fit:cover;border-radius:12px">`
  } else {
    brandLogo.innerText = 'A'
    brandLogo.style.background = `linear-gradient(135deg, ${t.accent||'#16a34a'}, ${t.accent2||'#4cd964'})`
  }
}
applyTheme()

/* ---------- render tabs & grid ---------- */
function renderTabs(){
  const tabs = document.getElementById('tabs'); tabs.innerHTML = ''
  DB.cats.forEach(c=>{
    const d = document.createElement('div'); d.className = 'tab' + (ACTIVE_CAT===c.id ? ' active' : '')
    d.innerHTML = `<img src="${c.icon}" width="28" style="border-radius:8px;margin-right:8px;vertical-align:middle">${c.name}`
    d.onclick = ()=>{ ACTIVE_CAT = c.id; renderTabs(); renderGrid() }
    tabs.appendChild(d)
  })
}
function getCatIcon(catId){ const c = DB.cats.find(x=>x.id===catId); return c?.icon || 'https://cdn-icons-png.flaticon.com/512/833/833300.png' }
function renderGrid(items){
  const grid = document.getElementById('grid'); grid.innerHTML = ''
  const prods = items || DB.products.filter(p=>p.cat===ACTIVE_CAT)
  if(!prods.length){ grid.innerHTML = '<div class="card small">No items</div>'; return }
  prods.forEach(p=>{
    const el = document.createElement('div'); el.className = 'prod'
    el.innerHTML = `
      <div class="img">${p.img?`<img src="${p.img}" style="width:100%;height:100%;object-fit:cover">`:`<img src="${getCatIcon(p.cat)}" width="80">`}</div>
      <div class="meta">
        <h4>${p.name}</h4>
        <div class="small">${p.desc||''}</div>
        <div style="display:flex;justify-content:space-between;align-items:center;margin-top:8px">
          <div><div class="mrp">MRP ${money(p.mrp)}</div><div class="price">${money(p.price)}</div></div>
          <div class="small">GST ${p.gst}% • Plat ${p.plat}% • Del ${p.delivery?money(p.delivery):'Free'}</div>
        </div>
        <div style="margin-top:8px;display:flex;gap:8px;align-items:center">
          <button class="btn ghost" onclick="changeQty('${p.id}',-1)">−</button>
          <div id="q_${p.id}" style="min-width:28px;text-align:center">${CART[p.id]||0}</div>
          <button class="btn" onclick="changeQty('${p.id}',1)">Add</button>
          <button class="btn ghost" style="margin-left:auto" onclick="viewProduct('${p.id}')">View</button>
        </div>
      </div>`
    grid.appendChild(el)
  })
}

/* ---------- cart & price ---------- */
function changeQty(id,delta){
  const p = DB.products.find(x=>x.id===id); if(!p) return
  CART[id] = (CART[id]||0) + delta
  if(CART[id] <= 0) delete CART[id]
  lsSet('amx_cart', CART)
  const qel = document.getElementById('q_'+id); if(qel) qel.innerText = CART[id]||0
  renderCart(); renderPrice()
}
function renderCart(){
  const area = document.getElementById('cartArea'); area.innerHTML = ''
  const keys = Object.keys(CART); if(!keys.length){ area.innerHTML = '<div class="cartList">Cart empty</div>'; return }
  keys.forEach(id=>{
    const p = DB.products.find(x=>x.id===id)
    const row = document.createElement('div'); row.className = 'tableRow'
    row.innerHTML = `<div><b>${p.name}</b><div class="small">${money(p.price)} × ${CART[id]}</div></div><div style="display:flex;gap:6px"><button class="iconBtn" onclick="changeQty('${id}',-1)">−</button><button class="iconBtn" onclick="changeQty('${id}',1)">+</button></div>`
    area.appendChild(row)
  })
}
function renderPrice(){
  const keys = Object.keys(CART); const pb = document.getElementById('priceBreak'); if(!pb) return
  if(!keys.length){ pb.innerHTML = ''; return }
  let item=0,gst=0,plat=0,del=0,packDisc=0
  keys.forEach(id=>{
    const p = DB.products.find(x=>x.id===id); const q = CART[id]; const base = p.price*q
    item += base; gst += base*(p.gst||1)/100; plat += base*(p.plat||0)/100; del += (p.delivery||0)*q
    if(p.isPack) packDisc += 50*q
  })
  const total = item + gst + plat + del - packDisc
  pb.innerHTML = `<div class="small">Items: ${money(item)}</div><div class="small">GST: ${money(gst)}</div><div class="small">Platform: ${money(plat)}</div><div class="small">Delivery: ${money(del)}</div><div class="small">Pack Discount: -${money(packDisc)}</div><hr><div style="font-weight:700">Total: ${money(total)}</div>`
}
function clearCart(){ CART = {}; lsSet('amx_cart',CART); renderCart(); renderPrice() }

/* ---------- login / logout ---------- */
function handleLogin(){
  const m = document.getElementById('mobile').value.trim()
  const pin = document.getElementById('pincode').value.trim()
  const msg = document.getElementById('loginMsg'); msg.innerText=''
  if(!/^\d{10}$/.test(m)){ msg.innerText='Enter valid 10-digit mobile'; return }
  if(!ALLOWED_PINCODES.includes(pin)){ msg.innerText='Service only for Raebareli'; return }
  DB.users = DB.users || {}
  const user = DB.users[m] || { phone:m, name:'User', email:'', coins:0, addresses:[], cards:[] }
  DB.users[m] = user; lsSet('amx_db', DB)
  CURRENT_USER = user; lsSet('amx_user', CURRENT_USER)
  document.getElementById('loginCard').style.display='none'; document.getElementById('mainView').style.display='block'
  document.getElementById('greeting').innerText = `Hello, ${CURRENT_USER.name} 👋`
  document.getElementById('bottomNav').style.display = 'flex'
  renderTabs(); renderGrid(); renderCart(); renderPrice(); renderProfileInfo()
  if(DB.sale && DB.sale.active) { showSaleBanner(); handleIncomingSale(DB.sale) }
  alert('Login successful — GST & full details now shown')
}
function logout(){ CURRENT_USER = null; lsSet('amx_user', null); CART={}; lsSet('amx_cart',CART); document.getElementById('loginCard').style.display='block'; document.getElementById('mainView').style.display='none'; document.getElementById('greeting').innerText=''; document.getElementById('bottomNav').style.display='none' }

/* ---------- profile ---------- */
function openProfile(){ document.getElementById('profileSheet').classList.add('active') }
function closeProfile(){ document.getElementById('profileSheet').classList.remove('active') }
function renderProfileInfo(){
  if(!CURRENT_USER) return
  document.getElementById('pfName').innerText = CURRENT_USER.name || 'User'
  document.getElementById('pfPhone').innerText = CURRENT_USER.phone
  document.getElementById('editName').value = CURRENT_USER.name
  document.getElementById('editEmail').value = CURRENT_USER.email || ''
  const al = document.getElementById('addrList'); al.innerHTML = ''
  (CURRENT_USER.addresses||[]).forEach((a,i)=>{ const d = document.createElement('div'); d.className='profileItem'; d.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><div>${a}</div><div><button class="btn ghost" onclick="removeAddress(${i})">Remove</button></div></div>`; al.appendChild(
