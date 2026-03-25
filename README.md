<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>Pro Edit</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:#0a0a0f;font-family:'Segoe UI',sans-serif;color:#fff;min-height:100vh}
.app{max-width:430px;margin:0 auto;min-height:100vh;display:flex;flex-direction:column}
.header{padding:16px 20px;background:rgba(255,255,255,0.03);border-bottom:1px solid rgba(255,255,255,0.07);display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100}
.logo{display:flex;align-items:center;gap:10px}
.logo-box{width:36px;height:36px;border-radius:10px;background:linear-gradient(135deg,#a855f7,#3b82f6);display:flex;align-items:center;justify-content:center;font-weight:900;font-size:18px}
.logo-text{font-size:20px;font-weight:800;background:linear-gradient(90deg,#c084fc,#60a5fa);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.login-btn{background:linear-gradient(135deg,#a855f7,#3b82f6);border:none;border-radius:20px;padding:6px 16px;color:#fff;font-weight:700;font-size:13px;cursor:pointer}
.notif{position:fixed;top:70px;left:50%;transform:translateX(-50%);background:linear-gradient(135deg,#a855f7,#3b82f6);border-radius:30px;padding:10px 24px;font-size:13px;font-weight:700;z-index:9999;white-space:nowrap;display:none;box-shadow:0 4px 24px rgba(168,85,247,0.5)}
.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,0.85);display:none;align-items:center;justify-content:center;z-index:500;padding:24px}
.modal{background:linear-gradient(135deg,#1a0030,#001a2e);border:1px solid rgba(168,85,247,0.3);border-radius:24px;padding:28px;width:100%;max-width:360px}
.modal h2{font-size:22px;font-weight:800;margin-bottom:20px;background:linear-gradient(90deg,#c084fc,#60a5fa);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
input,textarea{display:block;width:100%;margin-bottom:12px;background:rgba(255,255,255,0.07);border:1px solid rgba(168,85,247,0.3);border-radius:12px;color:#fff;padding:12px 14px;font-size:14px;outline:none;font-family:inherit;box-sizing:border-box}
input::placeholder,textarea::placeholder{color:#555}
textarea{resize:none;height:60px}
.btn{display:block;width:100%;padding:12px;background:linear-gradient(135deg,#a855f7,#3b82f6);border:none;border-radius:12px;color:#fff;font-weight:700;font-size:14px;cursor:pointer;margin-bottom:8px;font-family:inherit}
.btn-o{display:block;width:100%;padding:12px;background:transparent;border:1px solid rgba(168,85,247,0.4);border-radius:12px;color:#c084fc;font-weight:700;font-size:14px;cursor:pointer;margin-bottom:8px;font-family:inherit}
.content{flex:1;overflow-y:auto;padding-bottom:80px}
.page{padding:20px;display:none}
.page.on{display:block}
.card{background:rgba(255,255,255,0.04);border:1px solid rgba(255,255,255,0.07);border-radius:18px;padding:18px;margin-bottom:16px}
.upload{background:linear-gradient(135deg,rgba(168,85,247,0.15),rgba(59,130,246,0.15));border:2px dashed rgba(168,85,247,0.4);border-radius:20px;padding:32px;text-align:center;cursor:pointer;margin-bottom:20px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px}
.qbtn{border-radius:16px;padding:14px 12px;display:flex;align-items:center;gap:8px;cursor:pointer;font-weight:600;font-size:14px}
.recent{background:rgba(255,255,255,0.04);border-radius:14px;padding:12px 16px;margin-bottom:8px;display:flex;align-items:center;justify-content:space-between;cursor:pointer}
.preview{background:#000;border-radius:18px;height:190px;display:flex;align-items:center;justify-content:center;margin-bottom:16px;position:relative;overflow:hidden;border:1px solid rgba(168,85,247,0.3)}
.pbar{position:absolute;bottom:10px;left:16px;right:16px}
.pbg{background:rgba(168,85,247,0.3);border-radius:4px;height:4px;margin-bottom:6px}
.pfill{background:#a855f7;width:35%;height:100%;border-radius:4px}
.badge{position:absolute;top:8px;border-radius:10px;padding:3px 8px;font-size:11px}
.tools{display:grid;grid-template-columns:repeat(4,1fr);gap:8px}
.tool{background:rgba(255,255,255,0.05);border-radius:14px;padding:10px 4px;text-align:center;cursor:pointer;border:1px solid rgba(255,255,255,0.07)}
.tool.on{background:linear-gradient(135deg,rgba(168,85,247,0.4),rgba(59,130,246,0.4))}
.fx-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
.fx{background:rgba(255,255,255,0.05);border-radius:12px;padding:10px 6px;text-align:center;cursor:pointer;border:1px solid rgba(255,255,255,0.07);font-size:12px}
.fx.on{background:linear-gradient(135deg,#a855f7,#3b82f6);border:none;font-weight:700}
.toggle{width:50px;height:28px;border-radius:14px;background:rgba(255,255,255,0.15);position:relative;cursor:pointer;flex-shrink:0}
.toggle.on{background:#a855f7}
.dot{position:absolute;top:3px;left:3px;width:22px;height:22px;border-radius:50%;background:#fff;transition:0.2s}
.toggle.on .dot{left:25px}
.nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:430px;background:rgba(10,10,15,0.97);border-top:1px solid rgba(255,255,255,0.07);display:flex;z-index:200}
.ni{flex:1;padding:10px 0 8px;text-align:center;cursor:pointer;border-top:2px solid transparent}
.ni.on{border-top-color:#a855f7}
.ni .ic{font-size:20px}
.ni .lb{font-size:9px;color:#555;font-weight:600;margin-top:2px}
.ni.on .lb{color:#a855f7}
.media-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.media-item{background:rgba(255,255,255,0.04);border-radius:16px;overflow:hidden;cursor:pointer;border:1px solid rgba(255,255,255,0.07)}
.media-thumb{height:90px;display:flex;align-items:center;justify-content:center;font-size:40px;position:relative;background:rgba(168,85,247,0.1)}
.song-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.song-btn{padding:10px;border-radius:10px;border:1px solid rgba(59,130,246,0.3);background:rgba(59,130,246,0.1);color:#93c5fd;cursor:pointer;font-size:13px;font-family:inherit}
.stat{background:rgba(255,255,255,0.05);border-radius:14px;padding:14px 8px;text-align:center}
.si{display:flex;align-items:center;justify-content:space-between;padding:12px 0;border-bottom:1px solid rgba(255,255,255,0.06);cursor:pointer}
</style>
</head>
<body>
<div class="app">
<div class="header">
  <div class="logo">
    <div class="logo-box">P</div>
    <span class="logo-text">Pro Edit</span>
  </div>
  <button class="login-btn" id="lbtn" onclick="openAuth('login')">Login</button>
</div>
<div class="notif" id="notif"></div>

<!-- Auth Modal -->
<div class="modal-bg" id="amodal" style="display:none">
<div class="modal">
  <h2 id="atitle">🔐 Login</h2>
  <div id="aform">
    <input id="auser" placeholder="Username"/>
    <input id="apass" type="password" placeholder="Password"/>
    <input id="aphone" type="tel" placeholder="Phone Number"/>
  </div>
  <div id="otpform" style="display:none">
    <div id="otp1"><input id="ophone" type="tel" placeholder="Phone Number"/><button class="btn" onclick="sendOtp()">📤 Send OTP</button></div>
    <div id="otp2" style="display:none"><p style="color:#a3a3a3;font-size:13px;margin-bottom:10px">OTP sent! Use: 1234</p><input id="ocode" type="number" placeholder="Enter OTP"/><button class="btn" onclick="verifyOtp()">✅ Verify OTP</button></div>
  </div>
  <div id="abtns"></div>
  <button style="background:transparent;border:none;color:#666;cursor:pointer;font-size:14px;width:100%;padding:8px;font-family:inherit" onclick="closeAuth()">✕ Close</button>
</div>
</div>

<div class="content">

<!-- HOME -->
<div class="page on" id="p-home">
  <div style="font-size:24px;font-weight:800;margin-bottom:4px">Hello <span id="gname">👋</span>!</div>
  <div style="color:#a3a3a3;font-size:14px;margin-bottom:20px">Professional video editing at your fingertips</div>
  <div class="upload" onclick="uploadVid()">
    <div style="font-size:48px;margin-bottom:8px">📹</div>
    <div style="font-weight:700;font-size:16px">Upload Video / Image</div>
    <div style="color:#a3a3a3;font-size:13px">Tap to select from gallery</div>
  </div>
  <div style="font-size:16px;font-weight:700;margin-bottom:12px">⚡ Quick Actions</div>
  <div class="grid2">
    <div class="qbtn" style="background:linear-gradient(135deg,#a855f722,#a855f711);border:1px solid #a855f744" onclick="goEdit('Short Video')"><span style="font-size:22px">📱</span>Short Video</div>
    <div class="qbtn" style="background:linear-gradient(135deg,#3b82f622,#3b82f611);border:1px solid #3b82f644" onclick="goEdit('Long Video')"><span style="font-size:22px">📺</span>Long Video</div>
    <div class="qbtn" style="background:linear-gradient(135deg,#ec489922,#ec489911);border:1px solid #ec489944" onclick="goEdit('Song Edit')"><span style="font-size:22px">🎵</span>Song Edit</div>
    <div class="qbtn" style="background:linear-gradient(135deg,#f59e0b22,#f59e0b11);border:1px solid #f59e0b44" onclick="goEdit('Movie Clips')"><span style="font-size:22px">🎬</span>Movie Clips</div>
    <div class="qbtn" style="background:linear-gradient(135deg,#10b98122,#10b98111);border:1px solid #10b98144" onclick="goEdit('Thumbnail')"><span style="font-size:22px">🖼️</span>Thumbnail</div>
    <div class="qbtn" style="background:linear-gradient(135deg,#ef444422,#ef444411);border:1px solid #ef444444" onclick="goEdit('4K Export')"><span style="font-size:22px">4️⃣K</span>4K Export</div>
  </div>
  <div style="font-size:16px;font-weight:700;margin-bottom:12px">🕐 Recent Projects</div>
  <div class="recent" onclick="openProj('Project_01.mp4')"><div style="display:flex;align-items:center;gap:12px"><span style="font-size:26px">🎞️</span><div><div style="font-weight:600;font-size:14px">Project_01.mp4</div><div style="color:#666;font-size:12px">Edited recently</div></div></div><span style="color:#a855f7">▶</span></div>
  <div class="recent" onclick="openProj('My_Vlog.mp4')"><div style="display:flex;align-items:center;gap:12px"><span style="font-size:26px">🎞️</span><div><div style="font-weight:600;font-size:14px">My_Vlog.mp4</div><div style="color:#666;font-size:12px">Edited recently</div></div></div><span style="color:#a855f7">▶</span></div>
  <div class="recent" onclick="openProj('Short_Reel.mp4')"><div style="display:flex;align-items:center;gap:12px"><span style="font-size:26px">🎞️</span><div><div style="font-weight:600;font-size:14px">Short_Reel.mp4</div><div style="color:#666;font-size:12px">Edited recently</div></div></div><span style="color:#a855f7">▶</span></div>
</div>

<!-- EDIT -->
<div class="page" id="p-edit">
  <div class="preview" id="prev">
    <div id="pempty" style="text-align:center;color:#555"><div style="font-size:48px;margin-bottom:8px">📭</div><div>No video selected</div></div>
    <div id="pvid" style="display:none;width:100%;height:100%;position:relative;align-items:center;justify-content:center;flex-direction:column">
      <span style="font-size:60px">🎬</span>
      <div class="pbar"><div class="pbg"><div class="pfill"></div></div><div style="display:flex;justify-content:space-between;font-size:11px;color:#ccc"><span>0:08</span><span id="vname">video.mp4</span><span>0:23</span></div></div>
      <div class="badge" id="bgbadge" style="background:#10b981;right:8px;display:none">BG Removed</div>
      <div class="badge" id="k4badge" style="background:#ef4444;left:8px;display:none">4K</div>
    </div>
  </div>
  <div style="display:flex;gap:8px;margin-bottom:16px">
    <button id="bshort" class="btn" onclick="setType('short')" style="flex:1;margin:0">📱 Short</button>
    <button id="blong" class="btn-o" onclick="setType('long')" style="flex:1;margin:0">📺 Long</button>
  </div>
  <div class="card">
    <div style="font-weight:700;margin-bottom:10px">📝 Text → Video</div>
    <textarea id="txtin" placeholder="Type script here..."></textarea>
    <button class="btn" onclick="genVideo()">Generate Video</button>
  </div>
  <div style="font-weight:700;margin-bottom:12px">🛠️ Editing Tools</div>
  <div class="tools" id="toolsgrid"></div>
</div>

<!-- EFFECTS -->
<div class="page" id="p-effects">
  <div style="font-size:20px;font-weight:800;margin-bottom:4px">✨ Effects Library</div>
  <div style="color:#a3a3a3;font-size:13px;margin-bottom:16px">1000+ effects for all videos</div>
  <div class="fx-grid" id="fxgrid"></div>
</div>

<!-- AUDIO -->
<div class="page" id="p-audio">
  <div style="font-size:20px;font-weight:800;margin-bottom:20px">🎵 Audio Studio</div>
  <div class="card">
    <div style="font-weight:700;margin-bottom:12px">🔊 Voice Thickness</div>
    <div style="display:flex;align-items:center;gap:12px;margin-bottom:8px">
      <span style="font-size:12px;color:#a3a3a3">Thin</span>
      <input type="range" min="0" max="100" value="50" id="thick" oninput="updThick(this.value)" style="flex:1;accent-color:#a855f7;border:none;background:transparent;padding:0;margin:0"/>
      <span style="font-size:12px;color:#a3a3a3">Thick</span>
    </div>
    <div id="thickval" style="text-align:center;font-size:13px;color:#a855f7">50% — Normal</div>
  </div>
  <div class="card">
    <div style="font-weight:700;margin-bottom:12px">🎙️ Professional Voice</div>
    <div style="display:flex;gap:8px;flex-wrap:wrap">
      <button onclick="notify('Normal voice!')" style="flex:1;padding:8px;border-radius:10px;border:none;background:rgba(168,85,247,0.2);color:#c084fc;cursor:pointer;font-size:11px;font-weight:600;min-width:50px;font-family:inherit">Normal</button>
      <button onclick="notify('Pro voice!')" style="flex:1;padding:8px;border-radius:10px;border:none;background:rgba(168,85,247,0.2);color:#c084fc;cursor:pointer;font-size:11px;font-weight:600;min-width:50px;font-family:inherit">Pro</button>
      <button onclick="notify('Clean voice!')" style="flex:1;padding:8px;border-radius:10px;border:none;background:rgba(168,85,247,0.2);color:#c084fc;cursor:pointer;font-size:11px;font-weight:600;min-width:50px;font-family:inherit">Clean</button>
      <button onclick="notify('Deep voice!')" style="flex:1;padding:8px;border-radius:10px;border:none;background:rgba(168,85,247,0.2);color:#c084fc;cursor:pointer;font-size:11px;font-weight:600;min-width:50px;font-family:inherit">Deep</button>
      <button onclick="notify('High voice!')" style="flex:1;padding:8px;border-radius:10px;border:none;background:rgba(168,85,247,0.2);color:#c084fc;cursor:pointer;font-size:11px;font-weight:600;min-width:50px;font-family:inherit">High</button>
    </div>
  </div>
  <div class="card">
    <div style="display:flex;align-items:center;justify-content:space-between">
      <div><div style="font-weight:700">🧹 Clean Voice</div><div style="color:#a3a3a3;font-size:13px;margin-top:4px">Remove noise</div></div>
      <div class="toggle" id="ctoggle" onclick="togClean()"><div class="dot"></div></div>
    </div>
  </div>
  <div class="card">
    <div style="font-weight:700;margin-bottom:8px">🎤 Remove BG Voice</div>
    <div style="display:flex;gap:8px">
      <button class="btn" style="flex:1;margin:0" onclick="notify('Vocals isolated! 🎤')">Keep Vocals</button>
      <button class="btn-o" style="flex:1;margin:0" onclick="notify('Music isolated! 🎵')">Keep Music</button>
    </div>
  </div>
  <div class="card">
    <div style="font-weight:700;margin-bottom:12px">🎶 Song Editor</div>
    <div class="song-grid">
      <button class="song-btn" onclick="notify('Tempo!')">Tempo</button>
      <button class="song-btn" onclick="notify('Pitch!')">Pitch</button>
      <button class="song-btn" onclick="notify('Bass!')">Bass</button>
      <button class="song-btn" onclick="notify('Treble!')">Treble</button>
      <button class="song-btn" onclick="notify('Echo!')">Echo</button>
      <button class="song-btn" onclick="notify('Reverb!')">Reverb</button>
      <button class="song-btn" onclick="notify('Fade In!')">Fade In</button>
      <button class="song-btn" onclick="notify('Fade Out!')">Fade Out</button>
    </div>
  </div>
</div>

<!-- SEARCH -->
<div class="page" id="p-search">
  <div style="font-size:20px;font-weight:800;margin-bottom:16px">🔍 Search Media</div>
  <div style="position:relative;margin-bottom:20px">
    <span style="position:absolute;left:14px;top:50%;transform:translateY(-50%);font-size:18px">🔍</span>
    <input style="padding-left:44px;margin-bottom:0" id="srch" placeholder="Search videos, images..." oninput="filterMedia(this.value)"/>
  </div>
  <div class="media-grid" id="mgrid"></div>
  <div id="nores" style="display:none;text-align:center;color:#555;margin-top:40px"><div style="font-size:48px">🔭</div><div>No results found</div></div>
</div>

<!-- PROFILE -->
<div class="page" id="p-profile">
  <div style="font-size:20px;font-weight:800;margin-bottom:20px">👤 Profile</div>
  <div id="pout" style="text-align:center;padding-top:40px">
    <div style="font-size:64px;margin-bottom:16px">🔐</div>
    <div style="font-size:18px;font-weight:700;margin-bottom:8px">Login Required</div>
    <div style="color:#a3a3a3;margin-bottom:24px">Create account to save projects</div>
    <button class="btn" onclick="openAuth('login')">Login / Sign Up</button>
    <button class="btn-o" onclick="openAuth('otp')">📱 Verify Phone (OTP)</button>
  </div>
  <div id="pin" style="display:none">
    <div style="text-align:center;margin-bottom:24px">
      <div style="width:80px;height:80px;border-radius:50%;background:linear-gradient(135deg,#a855f7,#3b82f6);display:flex;align-items:center;justify-content:center;font-size:32px;margin:0 auto 12px" id="avi">U</div>
      <div style="font-size:20px;font-weight:700" id="pname">User</div>
      <div style="color:#a855f7;font-size:14px;margin-top:4px">Pro Editor</div>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-bottom:20px">
      <div class="stat"><div style="font-size:22px;font-weight:800;color:#a855f7">12</div><div style="font-size:12px;color:#a3a3a3">Videos</div></div>
      <div class="stat"><div style="font-size:22px;font-weight:800;color:#a855f7">48</div><div style="font-size:12px;color:#a3a3a3">Edits</div></div>
      <div class="stat"><div style="font-size:22px;font-weight:800;color:#a855f7">3</div><div style="font-size:12px;color:#a3a3a3">Projects</div></div>
    </div>
    <div class="card">
      <div style="font-weight:700;margin-bottom:12px">⚙️ Settings</div>
      <div class="si" onclick="notify('Edit Profile!')"><span>Edit Profile</span><span style="color:#555">›</span></div>
      <div class="si" onclick="notify('Notifications!')"><span>Notifications</span><span style="color:#555">›</span></div>
      <div class="si" onclick="notify('Storage!')"><span>Storage</span><span style="color:#555">›</span></div>
      <div class="si" onclick="notify('Privacy!')"><span>Privacy</span><span style="color:#555">›</span></div>
      <div class="si" onclick="notify('Help!')"><span>Help & Support</span><span style="color:#555">›</span></div>
      <div class="si" onclick="notify('New Features!')"><span>Update / New Features</span><span style="color:#555">›</span></div>
    </div>
    <button class="btn" style="background:rgba(239,68,68,0.2);border:1px solid rgba(239,68,68,0.3)" onclick="logout()">🚪 Logout</button>
  </div>
</div>

</div>

<!-- Nav -->
<div class="nav">
  <div class="ni on" onclick="sw('home',this)"><div class="ic">🏠</div><div class="lb">Home</div></div>
  <div class="ni" onclick="sw('edit',this)"><div class="ic">✂️</div><div class="lb">Edit</div></div>
  <div class="ni" onclick="sw('effects',this)"><div class="ic">✨</div><div class="lb">Effects</div></div>
  <div class="ni" onclick="sw('audio',this)"><div class="ic">🎵</div><div class="lb">Audio</div></div>
  <div class="ni" onclick="sw('search',this)"><div class="ic">🔍</div><div class="lb">Search</div></div>
  <div class="ni" onclick="sw('profile',this)"><div class="ic">👤</div><div class="lb">Profile</div></div>
</div>
</div>

<script>
let loggedIn=false,curUser='',vidLoaded=false,bgRm=false,is4k=false,actTool=null,actFx=null;
const EFFECTS=['Cinematic','Glitch','Neon','Vintage','VHS','Bloom','Film Grain','Chromatic',
