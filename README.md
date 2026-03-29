<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Pro Edit</title>
  <style>
    *{margin:0;padding:0;box-sizing:border-box}
    body{background:linear-gradient(135deg,#0a0a0f,#12001a,#000d1a);font-family:'Segoe UI',sans-serif;color:#fff;min-height:100vh}
    .app{max-width:430px;margin:0 auto;min-height:100vh;display:flex;flex-direction:column;position:relative}
    
    /* Header */
    .header{padding:18px 20px 12px;background:rgba(255,255,255,0.03);border-bottom:1px solid rgba(255,255,255,0.07);display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100;backdrop-filter:blur(12px)}
    .logo{display:flex;align-items:center;gap:10}
    .logo-icon{width:36px;height:36px;border-radius:10px;background:linear-gradient(135deg,#a855f7,#3b82f6);display:flex;align-items:center;justify-content:center;font-size:18px;font-weight:900}
    .logo-text{font-size:20px;font-weight:800;background:linear-gradient(90deg,#c084fc,#60a5fa);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
    .login-btn{background:linear-gradient(135deg,#a855f7,#3b82f6);border-radius:20px;padding:5px 14px;font-size:13px;font-weight:700;cursor:pointer;border:none;color:#fff}

    /* Notification */
    .notif{position:fixed;top:70px;left:50%;transform:translateX(-50%);background:linear-gradient(135deg,#a855f7,#3b82f6);border-radius:30px;padding:10px 24px;font-size:13px;font-weight:700;z-index:9999;white-space:nowrap;box-shadow:0 4px 24px rgba(168,85,247,0.5);display:none}

    /* Modal */
    .modal-bg{position:fixed;inset:0;background:rgba(0,0,0,0.85);display:none;align-items:center;justify-content:center;z-index:500;padding:24px}
    .modal-bg.show{display:flex}
    .modal{background:linear-gradient(135deg,#1a0030,#001a2e);border:1px solid rgba(168,85,247,0.3);border-radius:24px;padding:28px;width:100%;max-width:360px}
    .modal-title{font-size:22px;font-weight:800;margin-bottom:20px;background:linear-gradient(90deg,#c084fc,#60a5fa);-webkit-background-clip:text;-webkit-text-fill-color:transparent}

    /* Content */
    .content{flex:1;overflow-y:auto;padding-bottom:80px}
    .page{padding:20px;display:none}
    .page.active{display:block}

    /* Inputs */
    input,textarea{display:block;width:100%;margin-bottom:12px;background:rgba(255,255,255,0.07);border:1px solid rgba(168,85,247,0.3);border-radius:12px;color:#fff;padding:12px 14px;font-size:14px;outline:none;font-family:inherit;box-sizing:border-box}
    input::placeholder,textarea::placeholder{color:#555}
    textarea{resize:none;height:60px}

    /* Buttons */
    .btn{display:block;width:100%;padding:13px 0;background:linear-gradient(135deg,#a855f7,#3b82f6);border:none;border-radius:12px;color:#fff;font-weight:700;font-size:15px;cursor:pointer;margin-bottom:10px;font-family:inherit}
    .btn-out{display:block;width:100%;padding:13px 0;background:transparent;border:1px solid rgba(168,85,247,0.4);border-radius:12px;color:#c084fc;font-weight:700;font-size:15px;cursor:pointer;margin-bottom:10px;font-family:inherit}
    .btn-close{background:transparent;border:none;color:#666;cursor:pointer;font-size:14px;font-family:inherit;width:100%;padding:8px}

    /* Card */
    .card{background:rgba(255,255,255,0.04);border:1px solid rgba(255,255,255,0.07);border-radius:18px;padding:18px;margin-bottom:16px}
    .card-title{font-weight:700;margin-bottom:12px;font-size:15px}

    /* Upload Box */
    .upload-box{background:linear-gradient(135deg,rgba(168,85,247,0.15),rgba(59,130,246,0.15));border:2px dashed rgba(168,85,247,0.4);border-radius:20px;padding:32px;text-align:center;cursor:pointer;margin-bottom:20px}
    .upload-box:hover{border-color:rgba(168,85,247,0.8)}
    .upload-icon{font-size:48px;margin-bottom:8px}
    .upload-title{font-weight:700;font-size:16px}
    .upload-sub{color:#a3a3a3;font-size:13px}

    /* Quick Actions */
    .section-title{font-size:16px;font-weight:700;margin-bottom:12px}
    .quick-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:24px}
    .quick-item{border-radius:16px;padding:16px 12px;display:flex;align-items:center;gap:10;cursor:pointer;font-weight:600;font-size:14px}

    /* Recent */
    .recent-item{background:rgba(255,255,255,0.04);border-radius:14px;padding:12px 16px;margin-bottom:8px;display:flex;align-items:center;justify-content:space-between;cursor:pointer}
    .recent-info{display:flex;align-items:center;gap:12px}
    .recent-icon{font-size:28px}
    .recent-name{font-weight:600;font-size:14px}
    .recent-sub{color:#666;font-size:12px}
    .recent-play{color:#a855f7;font-size:18px}

    /* Video Preview */
    .preview{background:#000;border-radius:18px;height:200px;display:flex;align-items:center;justify-content:center;margin-bottom:16px;position:relative;overflow:hidden;border:1px solid rgba(168,85,247,0.3)}
    .preview-big{font-size:64px}
    .preview-bar{position:absolute;bottom:10px;left:16px;right:16px}
    .progress-bg{background:rgba(168,85,247,0.3);border-radius:4px;height:4px;margin-bottom:8px}
    .progress-fill{background:#a855f7;width:35%;height:100%;border-radius:4px}
    .preview-time{display:flex;justify-content:space-between;font-size:12px;color:#ccc}
    .badge{position:absolute;top:8px;border-radius:10px;padding:3px 8px;font-size:11px}
    .badge-green{background:#10b981;right:8px}
    .badge-red{background:#ef4444;left:8px}

    /* Video Type */
    .type-row{display:flex;gap:8px;margin-bottom:16px}
    .type-btn{flex:1;padding:10px 0;border-radius:12px;border:none;background:rgba(255,255,255,0.07);color:#fff;font-weight:700;cursor:pointer;font-size:14px;font-family:inherit}
    .type-btn.active{background:linear-gradient(135deg,#a855f7,#3b82f6)}

    /* Tools */
    .tools-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}
    .tool-item{background:rgba(255,255,255,0.05);border-radius:14px;padding:12px 4px;text-align:center;cursor:pointer;border:1px solid rgba(255,255,255,0.07)}
    .tool-item.active{background:linear-gradient(135deg,rgba(168,85,247,0.4),rgba(59,130,246,0.4))}
    .tool-icon{font-size:22px}
    .tool-label{font-size:10px;color:#ccc;margin-top:4px}

    /* Effects */
    .effects-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
    .effect-item{background:rgba(255,255,255,0.05);border-radius:12px;padding:12px 8px;text-align:center;cursor:pointer;border:1px solid rgba(255,255,255,0.07);font-size:13px}
    .effect-item.active{background:linear-gradient(135deg,#a855f7,#3b82f6);border:none;font-weight:700}
    .effects-more{background:rgba(168,85,247,0.1);border:1px dashed rgba(168,85,247,0.3);border-radius:12px;padding:12px 8px;text-align:center;grid-column:span 3;color:#a855f7;font-size:13px}

    /* Range */
    input[type=range]{padding:0;border:none;background:transparent;height:auto;margin-bottom:0}
    .range-row{display:flex;align-items:center;gap:12px;margin-bottom:8px}
    .range-label{font-size:12px;color:#a3a3a3}
    .range-val{text-align:center;font-size:13px;color:#a855f7;margin-top:4px}

    /* Toggle */
    .toggle-row{display:flex;align-items:center;justify-content:space-between}
    .toggle{width:50px;height:28px;border-radius:14px;background:rgba(255,255,255,0.15);position:relative;cursor:pointer;transition:0.2s;flex-shrink:0}
    .toggle.on{background:#a855f7}
    .toggle-dot{position:absolute;top:3px;left:3px;width:22px;height:22px;border-radius:50%;background:#fff;transition:0.2s}
    .toggle.on .toggle-dot{left:25px}

    /* Voice Btns */
    .voice-row{display:flex;gap:8px;flex-wrap:wrap}
    .voice-btn{flex:1;padding:8px 0;border-radius:10px;border:none;background:rgba(168,85,247,0.2);color:#c084fc;cursor:pointer;font-size:11px;font-weight:600;min-width:50px;font-family:inherit}

    /* Song btns */
    .song-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
    .song-btn{padding:10px 8px;border-radius:10px;border:1px solid rgba(59,130,246,0.3);background:rgba(59,130,246,0.1);color:#93c5fd;cursor:pointer;font-size:13px;font-family:inherit}

    /* Search */
    .search-wrap{position:relative;margin-bottom:20px}
    .search-icon{position:absolute;left:14px;top:50%;transform:translateY(-50%);font-size:18px}
    .search-input{padding-left:44px!important;margin-bottom:0!important}
    .media-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    .media-item{background:rgba(255,255,255,0.04);border-radius:16px;overflow:hidden;cursor:pointer;border:1px solid rgba(255,255,255,0.07)}
    .media-thumb{height:100px;display:flex;align-items:center;justify-content:center;font-size:48px;position:relative;background:rgba(168,85,247,0.1)}
    .media-dur{position:absolute;bottom:6px;right:8px;background:rgba(0,0,0,0.7);border-radius:8px;padding:2px 6px;font-size:11px}
    .media-type{position:absolute;top:6px;left:8px;border-radius:8px;padding:2px 6px;font-size:10px}
    .media-type.video{background:rgba(168,85,247,0.8)}
    .media-type.image{background:rgba(59,130,246,0.8)}
    .media-name{padding:10px 12px;font-size:13px;font-weight:600}
    .no-results{text-align:center;color:#555;margin-top:40px}

    /* Profile */
    .profile-center{text-align:center;margin-bottom:24px}
    .avatar{width:80px;height:80px;border-radius:50%;background:linear-gradient(135deg,#a855f7,#3b82f6);display:flex;align-items:center;justify-content:center;font-size:32px;margin:0 auto 12px}
    .profile-name{font-size:20px;font-weight:700}
    .profile-role{color:#a855f7;font-size:14px;margin-top:4px}
    .stats-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-bottom:20px}
    .stat-item{background:rgba(255,255,255,0.05);border-radius:14px;padding:16px 8px;text-align:center}
    .stat-num{font-size:22px;font-weight:800;color:#a855f7}
    .stat-label{font-size:12px;color:#a3a3a3}
    .setting-item{display:flex;align-items:center;justify-content:space-between;padding:12px 0;border-bottom:1px solid rgba(255,255,255,0.06);cursor:pointer}
    .setting-arrow{color:#555}
    .btn-danger{background:rgba(239,68,68,0.2);border:1px solid rgba(239,68,68,0.3)}
    .login-prompt{text-align:center;padding-top:40px}
    .login-big{font-size:64px;margin-bottom:16px}

    /* Bottom Nav */
    .bottom-nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:430px;background:rgba(10,10,15,0.97);border-top:1px solid rgba(255,255,255,0.07);display:flex;z-index:200}
    .nav-item{flex:1;padding:10px 0 8px;text-align:center;cursor:pointer;border-top:2px solid transparent}
    .nav-item.active{border-top-color:#a855f7}
    .nav-icon{font-size:20px}
    .nav-label{font-size:9px;color:#555;font-weight:600;margin-top:2px}
    .nav-item.active .nav-label{color:#a855f7}

    /* Glow */
    .glow1{position:fixed;top:-80px;left:-80px;width:300px;height:300px;border-radius:50%;background:radial-gradient(circle,rgba(138,43,226,0.18),transparent 70%);pointer-events:none}
    .glow2{position:fixed;bottom:60px;right:-60px;width:260px;height:260px;border-radius:50%;background:radial-gradient(circle,rgba(0,200,255,0.12),transparent 70%);pointer-events:none}
  </style>
</head>
<body>
<div class="app">
  <div class="glow1"></div>
  <div class="glow2"></div>

  <!-- Header -->
  <div class="header">
    <div class="logo">
      <div class="logo-icon">P</div>
      <span class="logo-text">Pro Edit</span>
    </div>
    <button class="login-btn" id="loginBtn" onclick="openAuth('login')">Login</button>
  </div>

  <!-- Notification -->
  <div class="notif" id="notif"></div>

  <!-- Auth Modal -->
  <div class="modal-bg" id="authModal">
    <div class="modal">
      <div class="modal-title" id="authTitle">🔐 Login</div>
      <div id="authForm">
        <input type="text" id="authUser" placeholder="Username"/>
        <input type="password" id="authPass" placeholder="Password"/>
        <input type="tel" id="authPhone" placeholder="Phone Number"/>
      </div>
      <div id="otpForm" style="display:none">
        <div id="otpStep1">
          <input type="tel" id="otpPhone" placeholder="Phone Number"/>
          <button class="btn" onclick="sendOtp()">📤 Send OTP</button>
        </div>
        <div id="otpStep2" style="display:none">
          <p style="color:#a3a3a3;font-size:13px;margin-bottom:10px">OTP sent! Use: 1234</p>
          <input type="number" id="otpCode" placeholder="Enter OTP"/>
          <button class="btn" onclick="verifyOtp()">✅ Verify OTP</button>
        </div>
      </div>
      <div id="authBtns"></div>
      <button class="btn-close" onclick="closeAuth()">✕ Close</button>
    </div>
  </div>

  <!-- Content -->
  <div class="content">

    <!-- HOME -->
    <div class="page active" id="page-home">
      <div style="font-size:24px;font-weight:800;margin-bottom:4px">Hello <span id="greetName">👋</span>!</div>
      <div style="color:#a3a3a3;font-size:14px;margin-bottom:24px">Professional video editing at your fingertips</div>
      <div class="upload-box" onclick="uploadVideo()">
        <div class="upload-icon">📹</div>
        <div class="upload-title">Upload Video / Image</div>
        <div class="upload-sub">Tap to select from gallery</div>
      </div>
      <div class="section-title">⚡ Quick Actions</div>
      <div class="quick-grid">
        <div class="quick-item" style="background:linear-gradient(135deg,#a855f722,#a855f711);border:1px solid #a855f744" onclick="goEdit('Short Video')"><span style="font-size:24px">📱</span> Short Video</div>
        <div class="quick-item" style="background:linear-gradient(135deg,#3b82f622,#3b82f611);border:1px solid #3b82f644" onclick="goEdit('Long Video')"><span style="font-size:24px">📺</span> Long Video</div>
        <div class="quick-item" style="background:linear-gradient(135deg,#ec489922,#ec489911);border:1px solid #ec489944" onclick="goEdit('Song Edit')"><span style="font-size:24px">🎵</span> Song Edit</div>
        <div class="quick-item" style="background:linear-gradient(135deg,#f59e0b22,#f59e0b11);border:1px solid #f59e0b44" onclick="goEdit('Movie Clips')"><span style="font-size:24px">🎬</span> Movie Clips</div>
        <div class="quick-item" style="background:linear-gradient(135deg,#10b98122,#10b98111);border:1px solid #10b98144" onclick="goEdit('Thumbnail')"><span style="font-size:24px">🖼️</span> Thumbnail</div>
        <div class="quick-item" style="background:linear-gradient(135deg,#ef444422,#ef444411);border:1px solid #ef444444" onclick="goEdit('4K Export')"><span style="font-size:24px">4️⃣K</span> 4K Export</div>
      </div>
      <div class="section-title">🕐 Recent Projects</div>
      <div class="recent-item" onclick="openProject('Project_01.mp4')">
        <div class="recent-info"><span class="recent-icon">🎞️</span><div><div class="recent-name">Project_01.mp4</div><div class="recent-sub">Edited recently</div></div></div>
        <span class="recent-play">▶</span>
      </div>
      <div class="recent-item" onclick="openProject('My_Vlog.mp4')">
        <div class="recent-info"><span class="recent-icon">🎞️</span><div><div class="recent-name">My_Vlog.mp4</div><div class="recent-sub">Edited recently</div></div></div>
        <span class="recent-play">▶</span>
      </div>
      <div class="recent-item" onclick="openProject('Short_Reel.mp4')">
        <div class="recent-info"><span class="recent-icon">🎞️</span><div><div class="recent-name">Short_Reel.mp4</div><div class="recent-sub">Edited recently</div></div></div>
        <span class="recent-play">▶</span>
      </div>
    </div>

    <!-- EDIT -->
    <div class="page" id="page-edit">
      <div class="preview">
        <div id="previewEmpty" style="text-align:center;color:#555"><div style="font-size:48px;margin-bottom:8px">📭</div><div>No video selected</div></div>
        <div id="previewVideo" style="display:none;width:100%;height:100%;position:relative;display:none;align-items:center;justify-content:center">
          <span style="font-size:64px">🎬</span>
          <div class="preview-bar">
            <div class="progress-bg"><div class="progress-fill"></div></div>
            <div class="preview-time"><span>0:08</span><span id="vidName">▶ video.mp4</span><span>0:23</span></div>
          </div>
          <div class="badge badge-green" id="bgBadge" style="display:none">BG Removed</div>
          <div class="badge badge-red" id="k4Badge" style="display:none">4K</div>
        </div>
      </div>
      <div class="type-row">
        <button class="type-btn active" id="btnShort" onclick="setType('short')">📱 Short</button>
        <button class="type-btn" id="btnLong" onclick="setType('long')">📺 Long</button>
      </div>
      <div class="card">
        <div class="card-title">📝 Text → Video</div>
        <textarea id="txtInput" placeholder="Type script here... video will be generated"></textarea>
        <button class="btn" onclick="generateVideo()">Generate Video</button>
      </div>
      <div class="section-title">🛠️ Editing Tools</div>
      <div class="tools-grid" id="toolsGrid"></div>
    </div>

    <!-- EFFECTS -->
    <div class="page" id="page-effects">
      <div style="font-size:20px;font-weight:800;margin-bottom:4px">✨ Effects Library</div>
      <div style="color:#a3a3a3;font-size:13px;margin-bottom:16px">1000+ effects for all videos</div>
      <div class="effects-grid" id="effectsGrid"></div>
    </div>

    <!-- AUDIO -->
    <div class="page" id="page-audio">
      <div style="font-size:20px;font-weight:800;margin-bottom:20px">🎵 Audio Studio</div>
      <div class="card">
        <div class="card-title">🔊 Voice Thickness</div>
        <div class="range-row">
          <span class="range-label">Thin</span>
          <input type="range" min="0" max="100" value="50" id="thickRange" oninput="updateThick(this.value)" style="flex:1;accent-color:#a855f7"/>
          <span class="range-label">Thick</span>
        </div>
        <div class="range-val" id="thickVal">50% — Normal</div>
      </div>
      <div class="card">
        <div class="card-title">🎙️ Professional Voice</div>
        <div class="voice-row">
          <button class="voice-btn" onclick="notify('Normal voice applied!')">Normal</button>
          <button class="voice-btn" onclick="notify('Pro voice applied!')">Pro</button>
          <button class="voice-btn" onclick="notify('Clean voice applied!')">Clean</button>
          <button class="voice-btn" onclick="notify('Deep voice applied!')">Deep</button>
          <button class="voice-btn" onclick="notify('High voice applied!')">High</button>
        </div>
      </div>
      <div class="card">
        <div class="toggle-row">
          <div><div style="font-weight:700">🧹 Clean Voice</div><div style="color:#a3a3a3;font-size:13px;margin-top:4px">Remove background noise</div></div>
          <div class="toggle" id="cleanToggle" onclick="toggleClean()"><div class="toggle-dot"></div></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">🎤 Remove Background Voice</div>
        <div style="display:flex;gap:8px">
          <button class="btn" style="flex:1;margin:0" onclick="notify('Vocals isolated! 🎤')">Keep Vocals</button>
          <button class="btn-out" style="flex:1;margin:0" onclick="notify('Music isolated! 🎵')">Keep Music</button>
        </div>
      </div>
      <div class="card">
        <div class="card-title">🎶 Song Editor</div>
        <div class="song-grid">
          <button class="song-btn" onclick="notify('Tempo adjusted!')">Tempo</button>
          <button class="song-btn" onclick="notify('Pitch adjusted!')">Pitch</button>
          <button class="song-btn" onclick="notify('Bass adjusted!')">Bass</button>
          <button class="song-btn" onclick="notify('Treble adjusted!')">Treble</button>
          <button class="song-btn" onclick="notify('Echo adjusted!')">Echo</button>
          <button class="song-btn" onclick="notify('Reverb adjusted!')">Reverb</button>
          <button class="song-btn" onclick="not
