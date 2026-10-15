<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>App</title>
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<style>
/* ---------- GLOBAL RESET & BOX-SIZING FIX ---------- */
*, *::before, *::after {
  box-sizing: border-box;
}
html, body {
  overflow-x: hidden;
  width: 100%;
}

/* ---------- APP THEME COLOURS ---------- */
:root{
  --app-bg:#fff; --app-text:#111; --app-border:#e4e6eb;
  --nav-inactive:#1C204CE3; --nav-bg:#fff;
  --panel:rgba(255,255,255,0.85); --panel-text:#333; --panel-border:#f0f0f0;
  --search-bg:linear-gradient(to right,#e2e2e2,#c9d6ff);
  --input-bg:rgba(0,0,0,0.08); --soft-hover:rgba(0,0,0,0.08);
  --history-text:#444; --chip-bg:#f2f2f2; --chip-text:#666;
  --trend-bg:#fff; --card-bg:#f0f2f5;
}
:root[data-theme="dark"]{
  --app-bg:#18191a; --app-text:#e4e6eb; --app-border:#3e4042;
  --nav-inactive:#b0b3b8; --nav-bg:#000;
  --panel:rgba(36,37,38,0.92); --panel-text:#e4e6eb; --panel-border:#3e4042;
  --search-bg:linear-gradient(to right,#18191a,#232a45);
  --input-bg:rgba(255,255,255,0.1); --soft-hover:rgba(255,255,255,0.1);
  --history-text:#e4e6eb; --chip-bg:#3a3b3c; --chip-text:#b0b3b8;
  --trend-bg:#000; --card-bg:#111;
}
body{
  margin:0;
  background:var(--app-bg);
  color:var(--app-text);
  font-family:Arial,sans-serif;
  padding-bottom:80px;
  transition:background .25s,color .25s;
}

/* ================================================================
   TOP NAV  —  fixed icon sizing + clean active pill
   ================================================================ */
.top-nav{
  display:flex;
  justify-content:center;
  align-items:center;
  gap:4px;
  background:rgba(5,26,255,0.19);
  padding:8px 6px;
  position:sticky;
  top:0;
  z-index:10;
  backdrop-filter:blur(10px);
  -webkit-backdrop-filter:blur(10px);
  -webkit-tap-highlight-color:transparent;
  width: 100%;
}
.nav-btn{
  flex:1 1 0;
  max-width:104px;
  min-width:0;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  gap:5px;
  border:none;
  background:none;
  font-weight:600;
  font-size:11px;
  line-height:1;
  letter-spacing:.2px;
  color:var(--nav-inactive);
  padding:7px 4px;
  border-radius:14px;
  transition:color .2s, background .2s, transform .15s;
  cursor:pointer;
}
.nav-btn span{white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:100%}

.nav-btn svg{
  width:28px;
  height:28px;
  display:block;
  flex:0 0 auto;
}
.nav-btn .avatar-img{
  width:28px;
  height:28px;
  flex:0 0 auto;
  border-radius:50%;
  object-fit:cover;
  display:none;
  border:2px solid #007AFF;
  background:#8a8a8a;
}

.nav-btn.active{color:#007AFF;background:rgba(0,122,255,.12)}
.nav-btn:active{transform:scale(.94)}
.nav-btn.active .keep-white{fill:white !important}

/* ---------- BOTTOM NAV ---------- */
.bottom-nav{
  position:fixed;
  bottom:0;left:0;right:0;
  height:60px;
  background:var(--nav-bg);
  border-top:1px solid var(--app-border);
  display:flex;
  justify-content:space-around;
  align-items:center;
  z-index:999;
  width: 100%;
}
.nav-item{
  display:flex;
  flex-direction:column;
  align-items:center;
  gap:2px;
  color:#888;
  font-size:10px;
  font-weight:600;
  cursor:pointer;
  padding:6px 10px;
  border-radius:10px;
  transition:0.2s;
}
.nav-item svg{width:26px;height:26px}
.nav-item .icon{fill:currentColor}
.nav-item.active{color:#1877f2}
.nav-item:active{transform:scale(1.1)}

/* ---------- POST POPUP ---------- */
#popup{
  position:fixed;
  bottom:85px;
  left:50%;
  transform:translateX(-50%) translateY(20px);
  display:flex;
  gap:28px;
  padding:18px 32px;
  border-radius:18px;
  background:rgba(5,26,255,0.19);
  backdrop-filter:blur(10px);
  box-shadow:4px 5px 5px rgba(0,0,0,.5);
  z-index:100;
  opacity:0;
  pointer-events:none;
  transition:.32s cubic-bezier(.34,1.56,.64,1);
}
#popup.show{
  opacity:1;
  pointer-events:auto;
  transform:translateX(-50%) translateY(0);
}
.p-item{display:flex;flex-direction:column;align-items:center;gap:8px}
.p-btn{
  width:60px;height:60px;
  border:2px solid transparent;
  border-radius:12px;
  background:transparent;
  display:flex;justify-content:center;align-items:center;
  color:#38bdf8;
  transition:0.2s;
  cursor:pointer;
}
.p-btn:hover{border:2px solid #38bdf8;color:#033270;transform:scale(1.05)}
.p-label{color:#1877f2;font-family:monospace;font-size:15px;font-weight:600;letter-spacing:1.5px}
#postOverlay{position:fixed;inset:0;display:none;z-index:15}
#postOverlay.show{display:block}
#preview,#previewVideo{
  position:fixed;
  top:80px;left:50%;
  transform:translateX(-50%);
  max-width:200px;
  border-radius:12px;
  display:none;
  z-index:110;
}

/* ---------- MORE (VIEW-MORE) POPUP ---------- */
#moreOverlay{
  position:fixed;inset:0;
  background:rgba(0,0,0,0.4);
  display:none;
  z-index:9;
}
#moreOverlay.show{display:block}
.view-more{
  position:fixed;
  bottom:-100%;
  left:50%;
  transform:translateX(-50%);
  width:100%;
  max-width:380px;
  height:75vh;
  max-height:550px;
  background:var(--panel);
  backdrop-filter:blur(20px);
  border-radius:20px 20px 0 0;
  padding:12px;
  display:flex;
  flex-direction:column;
  z-index:10;
  transition:bottom 0.35s cubic-bezier(0.32,0.72,0,1);
  box-shadow:0 -10px 40px rgba(0,0,0,0.2);
  overflow-y:auto;
}
.view-more.show{bottom:70px}
.cancel{
  background:#ccc;
  width:40px !important;
  height:5px !important;
  border-radius:10px;
  margin:0 auto 10px auto !important;
  cursor:pointer;
  border:none;
  box-shadow:none !important;
  flex-shrink:0;
  padding:0;
}
.cancel:hover{background:var(--app-text);width:50px !important}
.line{background:var(--panel-border);width:100%;height:1px;margin-bottom:12px;flex-shrink:0}
.view-more button:not(.cancel){
  font-size:14px;
  width:100%;
  height:44px;
  color:var(--panel-text);
  background:transparent;
  box-shadow:0 1px 4px rgba(0,0,0,0.08);
  border-radius:12px;
  border:1px solid var(--panel-border);
  transition:0.2s;
  font-weight:500;
  cursor:pointer;
}
.view-more button:not(.cancel):hover{
  background:#0073C1;
  color:white;
  transform:translateY(-1px);
  box-shadow:0 4px 12px rgba(0,115,193,0.3);
}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}

/* ================================================================
   SEARCH PAGE  —  glass bar, perfect pill input, round icon buttons
   ================================================================ */
#searchPage{
  position:fixed;
  top:0;left:0;
  width:100%;height:100%;
  background:var(--search-bg);
  z-index:100;
  display:none;
  overflow-y:auto;
  padding:15px 15px 120px; /* Increased bottom padding for keyboard */
}
#searchPage.open{display:block}

.search-container{
  display:flex;
  align-items:center;
  gap:8px;
  padding:7px;
  border-radius:18px;
  max-width:700px;
  margin:0 auto 20px auto;
  background:var(--panel);
  border:1px solid var(--panel-border);
  box-shadow:0 6px 20px rgba(0,0,0,.08);
  backdrop-filter:blur(14px);
  -webkit-backdrop-filter:blur(14px);
  transition:box-shadow .25s, border-color .25s;
  width: 100%;
}
.search-container:focus-within{
  border-color:#6c7eff;
  box-shadow:0 6px 24px rgba(108,126,255,.25);
}

.back-btn,
.search-btn{
  width:42px;
  height:42px;
  flex:0 0 42px;
  border-radius:50%;
  border:none;
  background:var(--input-bg);
  color:var(--app-text);
  display:flex;
  align-items:center;
  justify-content:center;
  cursor:pointer;
  -webkit-tap-highlight-color:transparent;
  transition:background .2s, color .2s, transform .15s;
}
.back-btn:hover,
.search-btn:hover{background:var(--soft-hover)}
.back-btn:active,
.search-btn:active{transform:scale(.9)}
.search-btn{color:#6c7eff}
.search-btn:hover{background:rgba(108,126,255,.15)}

.search-input-wrapper{flex:1;display:flex;align-items:center;min-width:0}
.search-input-wrapper input{
  width:100%;
  height:42px;
  padding:0 18px;
  font-size:16px;
  font-family:inherit;
  outline:none;
  border:none;
  background:var(--input-bg);
  color:var(--app-text);
  border-radius:21px;
  box-shadow:inset 0 0 0 1.5px transparent;
  transition:box-shadow .2s, background .2s;
  -webkit-appearance:none;
  appearance:none;
}
.search-input-wrapper input::placeholder{
  color:var(--chip-text);
  opacity:.85;
}
.search-input-wrapper input:focus{
  box-shadow:inset 0 0 0 1.5px #6c7eff, 0 0 0 4px rgba(108,126,255,.16);
}

hr{border:none;height:1px;background:var(--app-border);max-width:700px;margin:20px auto}
.search-section{max-width:370px;margin:0 auto;width:100%}
.history-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.history-header h2{font-size:14px;color:red;font-weight:500}
.clear-all{border:none;background:none;color:#6c7eff;font-size:13px;font-weight:500;cursor:pointer}
.search-history{
  display:flex;align-items:center;justify-content:space-between;
  background:#002EDE52;
  margin:8px 0;padding:12px 18px;
  border-radius:25px;font-size:14px;color:var(--history-text);
  box-shadow:0 2px 8px rgba(0,0,0,0.05);
  width: 100%;
}
.history-left{display:flex;align-items:center;gap:10px;flex:1;cursor:pointer;min-width:0}
.history-left::before{
  content:'';
  width:20px;height:20px;flex-shrink:0;
  background:currentColor;
  -webkit-mask:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23444' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Ccircle cx='12' cy='12' r='9'/%3E%3Cpath d='M12 7v5l3 2'/%3E%3C/svg%3E") center/contain no-repeat;
  mask:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23444' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Ccircle cx='12' cy='12' r='9'/%3E%3Cpath d='M12 7v5l3 2'/%3E%3C/svg%3E") center/contain no-repeat;
}
.remove-btn{
  border:none;background:var(--chip-bg);
  width:26px;height:26px;border-radius:50%;
  display:flex;align-items:center;justify-content:center;
  cursor:pointer;color:var(--history-text);
  flex-shrink:0;
  margin-left:10px;
}
.remove-btn svg{width:12px;height:12px;stroke:currentColor;stroke-width:2.5;stroke-linecap:round;fill:none}
.trends{margin-top:30px;padding:20px;border-radius:16px}
.trends h1{font-size:18px;font-weight:600;margin:0 0 15px;display:flex;align-items:center;gap:8px}
.trends h1 svg{width:20px;height:20px;fill:#e8590c}
.trends ul{margin:0;padding:0}
.trends li{list-style:none;padding:10px 0;border-bottom:1px solid var(--app-border);font-size:14px;cursor:pointer}
.trends li::before{content:'# ';color:#6c7eff;font-weight:600}

/* ---------- TRENDS PAGE ---------- */
#trendsPage{
  position:fixed;
  top:0;left:0;right:0;bottom:60px;
  background:var(--trend-bg);
  color:var(--app-text);
  z-index:80;
  display:none;
  overflow-y:auto;
}
#trendsPage.open{display:block}
.trends-header{
  position:sticky;top:0;
  height:48px;
  display:flex;align-items:center;justify-content:center;
  background:var(--trend-bg);
  border-bottom:1px solid var(--app-border);
  z-index:6;
}
.trends-header h2{font-size:16px;margin:0}
.top{
  box-sizing:border-box;
  width:100%;height:40px;
  display:flex;overflow-x:auto;gap:6px;
  padding:6px 10px;
  white-space:nowrap;
  position:sticky;top:48px;
  background:var(--trend-bg);
  z-index:5;
}
.top::-webkit-scrollbar{display:none}
.top button{background:var(--chip-bg);color:var(--chip-text);border:none;border-radius:16px;padding:0 14px;height:28px;flex-shrink:0;cursor:pointer}
.top button.active{background:var(--app-text);color:var(--app-bg)}
.video{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;padding:10px 10px 80px 10px}
.card{border-radius:10px;overflow:hidden;background:var(--card-bg)}
.card video{width:100%;height:260px;object-fit:cover;display:block}
.card .info{padding:6px 8px}
.card .title{font-size:12px;color:var(--app-text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.card .user{font-size:10px;color:#888}

/* ---------- ME SCREEN (profile + settings) ---------- */
:root{color-scheme:light;--bg:#f0f2f5; --card:#ffffff; --text:#050505; --text2:#65676b;
  --border:#e4e6eb; --blue:#1877f2; --green:#00d26a; --red:#e41e3f;
  --hover:#f2f3f5; --icon-bg:#e4e6eb;
}
:root[data-theme="dark"]{color-scheme:dark;
  --bg:#18191a; --card:#242526; --text:#e4e6eb; --text2:#b0b3b8;
  --border:#3e4042; --hover:#3a3b3c; --icon-bg:#3a3b3c;
}
#meScreen,#meScreen *{box-sizing:border-box}
#meScreen{margin:0;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;background:var(--bg);color:var(--text);transition:background .25s,color .25s}
#meScreen h1,#meScreen h2,#meScreen h3,#meScreen p{margin:0}
#meScreen .page{display:none;min-height:100vh}
#meScreen .page.active{display:block}
#meScreen .header{position:sticky;top:0;z-index:10;height:56px;display:flex;align-items:center;gap:12px;padding:0 16px;background:var(--card);color:var(--text);box-shadow:0 1px 2px rgba(0,0,0,.1)}
#meScreen .header b,#meScreen .header h1{font-size:18px;font-weight:700}
#meScreen .back{width:36px;height:36px;border-radius:50%;border:none;background:var(--bg);color:var(--text);display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0}
#meScreen .back svg{width:22px;height:22px;stroke:currentColor;stroke-width:2.2;fill:none;stroke-linecap:round;stroke-linejoin:round}
#meScreen .wrap{max-width:500px;margin:0 auto;padding:12px}
#meScreen .card{background:var(--card);border-radius:12px;box-shadow:0 1px 2px rgba(0,0,0,.08);overflow:hidden;margin-bottom:12px;transition:background .25s}
#meScreen .pad{padding:16px}
#meScreen .card h2{font-size:12px;color:var(--text2);margin-bottom:8px;text-transform:uppercase;letter-spacing:1px}
#meScreen .section{padding:14px 16px 6px;font-size:13px;color:var(--text2)}
#meScreen .note{padding:12px 16px;font-size:13px;color:var(--text2);line-height:1.4}
#meScreen .row{display:flex;align-items:center;justify-content:space-between;gap:12px;width:100%;padding:16px;background:transparent;border:none;color:inherit;font:inherit;text-align:left;cursor:pointer}
#meScreen .row+.row{border-top:1px solid var(--border)}
#meScreen .row:active{background:var(--hover)}
#meScreen .row b{font-size:15px;font-weight:500}
#meScreen .row p{font-size:13px;color:var(--text2);margin-top:3px}
#meScreen .row .chev{color:var(--text2);font-size:20px}
#meScreen .grow{flex:1;min-width:0}
#meScreen .int{width:100%;height:44px;border:1px solid var(--border);border-radius:10px;padding:0 14px;font-size:16px;background:var(--bg);color:var(--text);outline:none}
#meScreen .int:focus{border-color:var(--text)}
#meScreen .btn{border:none;background:var(--icon-bg);color:var(--text);padding:10px 18px;border-radius:8px;font-weight:600;font-size:14px;cursor:pointer}
#meScreen .btn:active{transform:scale(.96)}
#meScreen .btn.done{background:var(--blue);color:#fff}
#meScreen .btn.blue{background:var(--blue);color:#fff}
#meScreen .btn.red{background:var(--red);color:#fff}
#meScreen .btn.black{background:#111;color:#fff}
#meScreen .btn.green{background:var(--green);color:#fff}
#meScreen .btn.full{width:100%;height:44px;margin-top:10px}
#meScreen .btns{display:flex;gap:10px;margin-top:16px}
#meScreen .btns .btn{flex:1;padding:12px;font-size:15px}
#meScreen .icon{width:36px;height:36px;border-radius:50%;background:var(--icon-bg);display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0}
#meScreen .icon{color:var(--text)}
#meScreen .icon svg{width:20px;height:20px;fill:none;stroke:currentColor;stroke-width:1.8;stroke-linecap:round;stroke-linejoin:round}
#meScreen .left-icon{display:flex;align-items:center;gap:12px}
#meScreen .radio{width:22px;height:22px;border-radius:50%;border:2px solid #ccd0d5;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:.2s}
#meScreen .radio.on{border-color:var(--blue)}
#meScreen .radio.on::after{content:'';width:12px;height:12px;background:var(--blue);border-radius:50%}
#meScreen .switch{width:50px;height:28px;background:#ccd0d5;border-radius:14px;position:relative;cursor:pointer;transition:.25s;flex-shrink:0}
#meScreen .switch.on{background:var(--blue)}
#meScreen .switch::before{content:'OFF';position:absolute;left:0;right:0;top:0;line-height:28px;text-align:right;padding-right:8px;font-size:9px;font-weight:700;color:#fff;opacity:.9}
#meScreen .switch.on::before{content:'ON';text-align:left;padding-left:8px}
#meScreen .switch .knob{width:22px;height:22px;background:#fff;border-radius:50%;position:absolute;top:3px;left:3px;transition:.25s cubic-bezier(.4,0,.2,1);box-shadow:0 1px 3px rgba(0,0,0,.3);z-index:2}
#meScreen .switch.on .knob{left:25px}
#meScreen .badge{font-size:11px;font-weight:600;padding:3px 6px;border-radius:4px;margin-left:8px}
#meScreen .badge.green{background:#e7f3ff;color:var(--blue)}
#meScreen .badge.gray{background:var(--bg);color:var(--text2)}
#meScreen .modal{position:fixed;inset:0;background:rgba(0,0,0,.5);display:none;align-items:center;justify-content:center;padding:16px;z-index:1000}
#meScreen .modal.show{display:flex}
#meScreen .modal.bottom{align-items:flex-end;padding:0}
#meScreen .sheet{background:var(--card);color:var(--text);width:100%;max-width:500px;border-radius:16px 16px 0 0;padding:20px;animation:up .25s}
#meScreen .box{background:var(--card);color:var(--text);border-radius:12px;padding:20px;width:100%;max-width:340px}
#meScreen .box b{font-size:18px}
#meScreen .box p{font-size:14px;color:var(--text2);margin:10px 0 0;line-height:1.4}
@keyframes up{from{transform:translateY(100%)}to{transform:translateY(0)}}
#meScreen textarea{width:100%;height:100px;background:var(--bg);color:var(--text);border:none;border-radius:10px;padding:12px;font:inherit;font-size:15px;outline:none;margin-top:12px;resize:none}
#meScreen .toast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%) translateY(100px);background:#333;color:#fff;padding:12px 20px;border-radius:24px;font-size:14px;transition:.3s;z-index:2000;max-width:90%;text-align:center}
#meScreen .toast.show{transform:translateX(-50%) translateY(0)}
#meScreen .top-bar{display:flex;justify-content:space-between;align-items:center;padding:12px 15px}
#meScreen .setting-wrap{display:flex;flex-direction:column;align-items:center}
#meScreen .setting-btn{width:40px;height:40px;border-radius:50%;border:none;background:transparent;color:var(--text2);cursor:pointer;display:flex;align-items:center;justify-content:center}
#meScreen .setting-btn:hover{background:var(--hover)}
#meScreen .more-setting{font-size:11px;color:grey;margin-top:-5px}
#meScreen .profile-area{display:flex;flex-direction:column;align-items:center;margin-top:20px}
#meScreen .profile-btn{width:80px;height:80px;border-radius:50%;border:none;background:#8a8a8a;display:flex;align-items:center;justify-content:center;cursor:pointer}
#meScreen .profile-btn svg{width:50px;height:50px;color:#fff}
#meScreen .Name h1{margin:15px 0 5px;font-size:22px;color:var(--text2);font-weight:600}
#meScreen .btn-info{display:flex;justify-content:center;gap:10px;margin:20px 15px;flex-wrap:wrap}
#meScreen .btn-info button{padding:8px 16px;border:none;border-radius:20px;background:var(--card);color:var(--text);box-shadow:0 1px 3px rgba(0,0,0,.15);cursor:pointer;font-size:13px;min-width:80px}
#meScreen .postter{display:flex;gap:8px;overflow-x:auto;padding:15px;border-bottom:1px solid var(--border);border-top:1px solid var(--border);margin-top:20px}
#meScreen .postter::-webkit-scrollbar{display:none}
#meScreen .postter button{background:transparent;border:1px solid var(--border);padding:8px 14px;font-size:14px;border-radius:20px;cursor:pointer;white-space:nowrap;color:var(--text2)}
#meScreen .postter button:hover{background:var(--text);color:var(--card)}
#meScreen #page-settings{background:var(--bg);color:var(--text);padding:20px}
#meScreen #page-settings .back{background:var(--card);color:var(--text2);margin-bottom:10px}
#meScreen #page-settings h2{font-size:14px;color:var(--text2);margin:30px 0 12px;text-transform:uppercase;letter-spacing:1px}
#meScreen .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(105px,1fr));gap:12px}
#meScreen .item{background:var(--card);box-shadow:0 1px 2px rgba(0,0,0,.08);border-radius:12px;padding:18px 8px;text-align:center;cursor:pointer}
#meScreen .item:hover{background:var(--hover)}
#meScreen .item svg{width:28px;height:28px;stroke:currentColor;color:var(--text);stroke-width:1.7;fill:none}
#meScreen .item span{display:block;font-size:11px;margin-top:8px;color:var(--text2)}
#meScreen .profile-wrap{display:flex;justify-content:center;margin-top:10px}
#meScreen .profile{width:110px;height:110px;background:#8a8a8a;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer}
#meScreen .profile svg{width:70px;height:70px;color:#fff}
#meScreen .personal{list-style:none;margin:0;padding:10px}
#meScreen .personal li{padding:12px 10px;border-bottom:1px solid var(--border)}
#meScreen .personal li:last-child{border:none}
#meScreen .personal .int{margin-top:6px;height:38px;border-left:3px solid var(--text);font-weight:600}
#meScreen .add-row{display:flex;align-items:center;padding:12px}
#meScreen .profile-add{width:50px;height:50px;min-width:50px;background:#8a8a8a;border-radius:50%;display:flex;align-items:center;justify-content:center}
#meScreen .profile-add svg{width:30px;height:30px;color:#fff}
#meScreen .add-text{display:flex;flex-direction:column;margin-left:12px}
#meScreen .name-add{font-weight:700}
#meScreen .email-add{font-size:13px;color:var(--text2);margin-top:2px}
#meScreen .fab-wrap{position:fixed;bottom:20px;right:15px;z-index:999}
#meScreen .nav-add{width:56px;height:56px;border-radius:50%;background:var(--green);display:flex;justify-content:center;align-items:center;cursor:pointer;box-shadow:0 4px 12px rgba(0,0,0,.25);border:none}
#meScreen .nav-add:hover{transform:scale(1.05)}
#meScreen .section-title{margin:0 0 10px 5px;font-size:13px;color:var(--text2);text-transform:uppercase;font-weight:600}
#meScreen .profile-preview{width:80px;height:80px;border-radius:50%;background:#ddd;display:flex;align-items:center;justify-content:center;overflow:hidden;margin-top:10px}
#meScreen .profile-preview img{width:100%;height:100%;object-fit:cover}
#meScreen .label{font-size:15px;font-weight:600;display:flex;justify-content:space-between}
#meScreen input[type=range]{width:100%;margin:16px 0 8px}
#meScreen .sizes{display:flex;justify-content:space-between;align-items:flex-end;font-size:12px;color:var(--text2)}
#meScreen .preview-box{margin-top:16px;padding:16px;border:2px dashed var(--border);border-radius:10px;background:var(--bg)}
#meScreen .bar-wrap{height:6px;background:var(--icon-bg);border-radius:10px;margin-top:10px;overflow:hidden}
#meScreen .bar{height:100%;background:var(--blue);width:68%;border-radius:10px;transition:width .4s ease}
#meScreen .right{font-size:14px;font-weight:500;color:var(--text2)}
#meScreen .content{display:none;padding:0 16px 16px;font-size:14px;color:var(--text2);line-height:1.5}
#meScreen .content.show{display:block}
#meScreen .content h3{color:var(--text);font-size:14px;margin:12px 0 4px}
#meScreen .search{height:40px;background:var(--card);border-radius:20px;padding:0 16px;display:flex;align-items:center;gap:8px;margin:12px}
#meScreen .search svg{width:18px;height:18px;flex-shrink:0;fill:none;stroke:var(--text2);stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
#meScreen .search input{border:none;background:none;flex:1;font-size:15px;outline:none;color:var(--text)}
#meScreen{display:none;position:fixed;inset:0;overflow-y:auto;z-index:2000}
#meScreen.open{display:block}

/* ================================================================
   AUTH GATE  —  fixed login / register / phone inputs
   ================================================================ */
body.auth-active{overflow:hidden}
#authScreen{
  position:fixed; inset:0; z-index:5000;
  display:flex; justify-content:center; align-items:center;
  background:linear-gradient(to right,#e2e2e2,#c9d6ff);
  padding:15px; overflow-y:auto;
  font-family:'Segoe UI',Arial,sans-serif;
  box-sizing: border-box;
}
#authScreen .auth-container{
  margin:auto; 
  width:100%; 
  max-width:450px;
  box-sizing: border-box;
}
#authScreen .form-box{
  width:100%;
  padding:30px;
  border-radius:10px;
  background-color:#fff;
  box-shadow:0 0 10px rgba(0,0,0,0.1);
  display:none;
  box-sizing: border-box;
}
#authScreen .form-box.active{display:block}
#authScreen h2{font-size:34px;text-align:center;margin:0 0 22px;text-transform:capitalize;color:#333}

/* ---- the inputs / select ---- */
#authScreen input,
#authScreen select{
  width:100%;
  height:52px;
  padding:0 16px;
  background-color:#f4f6fb;
  border:1.5px solid #e2e6f0;
  border-radius:12px;
  outline:none;
  font-family:inherit;
  font-size:15px;
  color:#1c1e21;
  margin-bottom:15px;
  -webkit-appearance:none;
  appearance:none;
  transition:border-color .2s, background-color .2s, box-shadow .2s;
  box-sizing: border-box; /* Crucial for fixing the overflow */
}
#authScreen input::placeholder{color:#9aa0b4;opacity:1}
#authScreen input:hover,
#authScreen select:hover{border-color:#cfd6e8}
#authScreen input:focus,
#authScreen select:focus{
  background-color:#fff;
  border-color:#7494ec;
  box-shadow:0 0 0 4px rgba(116,148,236,.18);
}
/* kill Chrome's yellow autofill */
#authScreen input:-webkit-autofill,
#authScreen input:-webkit-autofill:hover,
#authScreen input:-webkit-autofill:focus{
  -webkit-box-shadow:0 0 0 1000px #f4f6fb inset;
  -webkit-text-fill-color:#1c1e21;
  transition:background-color 9999s ease-in-out 0s;
}

/* ---- custom chevron for the role select ---- */
#authScreen select{
  padding-right:44px;
  cursor:pointer;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%237494ec' stroke-width='2.5' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpolyline points='6 9 12 15 18 9'/%3E%3C/svg%3E");
  background-repeat:no-repeat;
  background-position:right 16px center;
  background-size:16px;
}

/* ---- password field + Show/Hide pill ---- */
#authScreen .pass-wrap{
  position:relative;
  margin-bottom:15px;
  width: 100%;
}
#authScreen .pass-wrap input{
  margin-bottom:0;
  padding-right:70px; /* Make room for the Show text */
}
#authScreen .pass-wrap span{
  position:absolute;
  right:8px;
  top:50%;
  transform:translateY(-50%);
  cursor:pointer;
  color:#7494ec;
  font-size:12px;
  font-weight:700;
  letter-spacing:.3px;
  user-select:none;
  padding:7px 10px;
  border-radius:9px;
  transition:background .2s, color .2s;
}
#authScreen .pass-wrap span:hover{background:rgba(116,148,236,.14)}
#authScreen .pass-wrap span:active{background:rgba(116,148,236,.24)}

#authScreen button{width:100%;padding:12px;background:#7494ec;border-radius:6px;border:none;cursor:pointer;font-size:16px;color:#fff;font-weight:500;margin-bottom:15px;transition:0.3s;box-sizing: border-box;}
#authScreen button:hover{background:#6884d3}
#authScreen p{font-size:14.5px;text-align:center;margin-bottom:10px;color:#333}
#authScreen p a{color:#7494ec;text-decoration:none;font-weight:500}
#authScreen p a:hover{text-decoration:underline}
#authScreen .error{background:#ffe0e0;color:#d93025;padding:8px 12px;border-radius:6px;font-size:13px;margin-bottom:15px;display:none}

/* profile picture propagation */
.nav-btn .avatar-img{width:28px;height:28px;border-radius:50%;object-fit:cover;display:none}
#meScreen .profile-btn{overflow:hidden}
#meScreen .profile{overflow:hidden}
#meScreen .profile-add{overflow:hidden}
</style>
</head>

<body id="appBody" class="auth-active">

<!-- AUTH GATE (login / register / phone) -->
<div id="authScreen">
  <div class="auth-container">

    <!-- LOGIN -->
    <div class="form-box active" id="login-form">
      <form onsubmit="return authLoginSubmit()">
        <h2>Login</h2>
        <input type="email" id="login-email" name="email" placeholder="Enter your Email" required>
        <div class="pass-wrap">
          <input type="password" id="login-pass" name="password" placeholder="Enter your Password" required>
          <span onclick="authTogglePass('login-pass', this)">Show</span>
        </div>
        <div id="login-error" class="error"></div>
        <button type="submit" name="login">Login</button>
        <p>Don't have an account? <a href="#" onclick="authShowForm('register-form'); return false;">Register.</a></p>
      </form>
    </div>

    <!-- REGISTER -->
    <div class="form-box" id="register-form">
      <form onsubmit="return authValidateRegister()">
        <h2>Register</h2>
        <input type="text" name="name" placeholder="Enter your name" required>
        <input type="email" name="email" placeholder="Enter your Email" required>

        <div class="pass-wrap">
          <input type="password" id="reg-pass" name="password" placeholder="Password (8-16 letters+numbers)" required>
          <span onclick="authTogglePass('reg-pass', this)">Show</span>
        </div>

        <div class="pass-wrap">
          <input type="password" id="confirm-pass" name="confirm-password" placeholder="Confirm Password" required>
          <span onclick="authTogglePass('confirm-pass', this)">Show</span>
        </div>

        <div id="reg-error" class="error"></div>

        <select name="role" required>
          <option value="">-- Select Role --</option>
          <option value="personal">Personal</option>
          <option value="business">Business</option>
        </select>
        <button type="submit" name="register">Register</button>
        <p>Already have an account? <a href="#" onclick="authShowForm('login-form'); return false;">Login.</a></p>
      </form>
    </div>

    <!-- PHONE -->
    <div class="form-box" id="phone-form">
      <form onsubmit="enterApp(); return false;">
        <h2>Phone</h2>
        <input type="tel" name="Phone" placeholder="Enter your number" value="+" required>
        <button type="submit" name="login">Next</button>
        <p>Don't have an account? <a href="#" onclick="authShowForm('register-form'); return false;">Register.</a></p>
      </form>
    </div>

  </div>
</div>

<!-- global hidden input for setting the profile picture from anywhere -->
<input type="file" id="mainProfileInput" accept="image/*" style="display:none">

<!-- MORE POPUP -->
<div id="moreOverlay" onclick="hideMore()"></div>

<div class="view-more" id="viewMore">
  <button class="cancel" onclick="hideMore()" title="Close" aria-label="Close"></button>
  <div class="line"></div>
  <div class="grid">
    <button onclick="alert('live')">Live</button>
    <button onclick="goTo('News')">News</button>
    <button onclick="goTo('Sports')">Sports</button>
    <button onclick="goTo('Kids')">Kids</button>
    <button onclick="goTo('Comedy')">Comedy</button>
    <button onclick="goTo('Food')">Food</button>
    <button onclick="goTo('Fashion')">Fashion</button>
    <button onclick="goTo('Technology')">Technology</button>
    <button onclick="goTo('Motivation')">Motivation</button>
    <button onclick="goTo('Drama')">Drama</button>
    <button onclick="goTo('Challenge')">Challenge</button>
    <button onclick="goTo('History')">History</button>
    <button onclick="goTo('Education')">Education</button>
    <button onclick="goTo('Animation')">Animation</button>
    <button onclick="goTo('Dance')">Dance</button>
    <button onclick="goTo('Picture')">Picture</button>
  </div>
</div>

<!-- SEARCH -->
<div id="searchPage">
  <div class="search-container">
    <button class="back-btn" onclick="closeSearch()" aria-label="Back">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 12H5"/><path d="M12 19l-7-7 7-7"/></svg>
    </button>
    <div class="search-input-wrapper"><input type="search" placeholder="Search..." id="search"></div>
    <button class="search-btn" onclick="doSearch()" aria-label="Search"><svg width="22" height="22" viewBox="0 0 24 24" fill="currentColor"><path d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/></svg></button>
  </div>
  <hr>
  <div class="search-section">
    <div class="history-header"><h2>Recent searches</h2><button class="clear-all" onclick="clearAll()">Clear all</button></div>
    <div id="history-list">
      <div class="search-history"><div class="history-left" onclick="fillSearch(this)">android development</div><button class="remove-btn" onclick="removeItem(this)" aria-label="Remove"><svg viewBox="0 0 24 24"><path d="M6 6l12 12M18 6L6 18"/></svg></button></div>
      <div class="search-history"><div class="history-left" onclick="fillSearch(this)">linux tutorial</div><button class="remove-btn" onclick="removeItem(this)" aria-label="Remove"><svg viewBox="0 0 24 24"><path d="M6 6l12 12M18 6L6 18"/></svg></button></div>
      <div class="search-history"><div class="history-left" onclick="fillSearch(this)">how to build own os</div><button class="remove-btn" onclick="removeItem(this)" aria-label="Remove"><svg viewBox="0 0 24 24"><path d="M6 6l12 12M18 6L6 18"/></svg></button></div>
    </div>
    <div class="trends">
      <h1><svg viewBox="0 0 24 24"><path d="M13.5.67s.74 2.65.74 4.8c0 2.06-1.35 3.73-3.41 3.73-2.07 0-3.63-1.67-3.63-3.73l.03-.36C5.21 7.51 4 10.62 4 14c0 4.42 3.58 8 8 8s8-3.58 8-8C20 8.61 17.41 3.8 13.5.67z"/></svg>Trending searches</h1>
      <ul>
        <li onclick="fillSearch(this)">flutter vs react native</li>
        <li onclick="fillSearch(this)">best coding laptops 2026</li>
        <li onclick="fillSearch(this)">chatgpt prompts</li>
        <li onclick="fillSearch(this)">android 16 features</li>
      </ul>
    </div>
  </div>
</div>

<!-- TOP NAV -->
<div class="top-nav">
  <button class="nav-btn active" onclick="setTopActive(this)">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="2.5" y="2.5" width="19" height="19" rx="5.5" fill="none"/><line x1="2.5" y1="9" x2="21.5" y2="9"/><line x1="8.5" y1="2.5" x2="8.5" y2="9"/><line x1="15.5" y1="2.5" x2="15.5" y2="9"/><path d="M9.5 12 L9.5 18 L15 15 Z" fill="currentColor" stroke="none"/></svg>
    <span>Reels</span>
  </button>
  <button class="nav-btn" onclick="setTopActive(this)">
    <svg viewBox="0 0 32 24"><path d="M27 5.8 Q27.5 7.5 27.5 12 Q27.5 16.5 27 18.2 Q26.6 19.6 25.1 20 Q22.8 20.5 16 20.5 Q9.2 20.5 6.9 20 Q5.4 19.6 5 18.2 Q4.5 16.5 4.5 12 Q4.5 7.5 5 5.8 Q5.4 4.4 6.9 4 Q9.2 3.5 16 3.5 Q22.8 3.5 25.1 4 Q26.6 4.4 27 5.8 Z" fill="currentColor"/><path class="keep-white" d="M13 16.5 L20.5 12 L13 7.5 Z" fill="#005578AB"/></svg>
    <span>Movies</span>
  </button>
  <button class="nav-btn" onclick="setTopActive(this)">
    <svg viewBox="0 0 24 24" fill="currentColor"><path d="M14 3.2 L21 2 V14.6 c0 1.9 -1.6 3.4 -3.5 3.4 c-1.9 0 -3.5 -1.5 -3.5 -3.4 s1.6 -3.4 3.5 -3.4 c0.5 0 1 0.1 1.5 0.3 V6.1 L9 8 V17.6 c0 1.9 -1.6 3.4 -3.5 3.4 C3.6 21 2 19.5 2 17.6 S3.6 14.2 5.5 14.2 c0.5 0 1 0.1 1.5 0.3 V4.8 c0 -0.5 0.4 -0.9 0.9 -1 L14 3.2 Z"/></svg>
    <span>Music</span>
  </button>
  <button class="nav-btn" onclick="setTopActive(this);openSearch()">
    <svg viewBox="0 0 24 24" fill="currentColor"><path d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/></svg>
    <span>Search</span>
  </button>
  <button class="nav-btn" onclick="setTopActive(this);openMe()">
    <svg id="meNavIcon" viewBox="0 0 24 24" fill="currentColor"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
    <img id="meNavImg" class="avatar-img" alt="">
    <span>Me</span>
  </button>
</div>

<!-- POST POPUP -->
<div id="postOverlay" onclick="closePopup()"></div>
<img id="preview" alt="Selected image preview" onclick="this.style.display='none'">
<video id="previewVideo" controls muted onclick="event.stopPropagation()"></video>

<div id="popup">
  <div class="p-item">
    <input type="file" id="galleryInput" accept="image/*,video/*" style="display:none" onchange="showPreview(event)">
    <button class="p-btn" aria-label="Gallery" onclick="document.getElementById('galleryInput').click()"><svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="3"/><circle cx="8.5" cy="8.5" r="1.5" fill="currentColor" stroke="none"/><path d="M3 15 L7 11 L11 15 L13.5 12.5 L21 19"/></svg></button>
    <span class="p-label">gallery</span>
  </div>
  <div class="p-item">
    <button class="p-btn" aria-label="Go live" onclick="alert('Go Live')"><svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="7" width="14" height="10" rx="2.5"/><path d="M16 9 L21 6.5 L21 17.5 L16 15 Z" fill="currentColor"/></svg></button>
    <span class="p-label">live</span>
  </div>
  <div class="p-item">
    <button class="p-btn" aria-label="Music" onclick="alert('Music')"><svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18V6l10-2v12"/><circle cx="7" cy="18" r="3" fill="currentColor" stroke="none"/><circle cx="17" cy="16" r="3" fill="currentColor" stroke="none"/></svg></button>
    <span class="p-label">music</span>
  </div>
</div>

<!-- TRENDS VIDEOS -->
<div id="trendsPage">
  <div class="trends-header"><h2>Trending</h2></div>
  <div class="top" id="topBar">
    <button class="active" data-cat="all">All</button>
    <button data-cat="entertainment">Entertainment</button>
    <button data-cat="news">News</button>
    <button data-cat="sports">Sports</button>
    <button data-cat="anime">Anime/comics</button>
    <button data-cat="life">Life</button>
    <button data-cat="fashion">Fashion</button>
    <button data-cat="gaming">Gaming</button>
  </div>
  <div class="video" id="trendsVideos">
    <div class="card" data-cat="entertainment"><video src="https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4" controls muted loop></video><div class="info"><div class="title">Big Buck Bunny - Trending</div><div class="user">@nature • 2.3M</div></div></div>
    <div class="card" data-cat="anime"><video src="https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4" controls muted loop></video><div class="info"><div class="title">Elephant Dream</div><div class="user">@animation • 1.1M</div></div></div>
    <div class="card" data-cat="sports"><video src="https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerJoyrides.mp4" controls muted loop></video><div class="info"><div class="title">Joyrides - Trending</div><div class="user">@sports • 2M</div></div></div>
    <div class="card" data-cat="gaming"><video src="https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerMeltdowns.mp4" controls muted loop></video><div class="info"><div class="title">Epic Meltdowns</div><div class="user">@gaming • 1.9M</div></div></div>
    <div class="card" data-cat="anime"><video src="https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4" controls muted loop></video><div class="info"><div class="title">Sintel - Fantasy</div><div class="user">@anime • 5.2M</div></div></div>
  </div>
</div>

<!-- BOTTOM NAV -->
<div class="bottom-nav">
  <div class="nav-item active" id="navHome" onclick="goHome()"><svg viewBox="0 0 24 24"><path class="icon" d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/></svg><span>Home</span></div>
  <div class="nav-item" id="navTrends" onclick="openTrends()"><svg viewBox="0 0 24 24"><path class="icon" d="M13.5.67s.74 2.65.74 4.8c0 2.06-1.35 3.73-3.41 3.73-2.07 0-3.63-1.67-3.63-3.73l.03-.36C5.21 7.51 4 10.62 4 14c0 4.42 3.58 8 8 8s8-3.58 8-8C20 8.61 17.41 3.8 13.5.67z"/></svg><span>Trends</span></div>
  <div class="nav-item" id="navPost" onclick="setBottomActive(this);togglePopup()"><svg viewBox="0 0 24 24"><path class="icon" d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/></svg><span>Post</span></div>
  <div class="nav-item" id="navCommunity" onclick="setBottomActive(this)"><svg viewBox="0 0 24 24"><path class="icon" d="M16 11c1.66 0 2.99-1.34 2.99-3S17.66 5 16 5c-1.66 0-3 1.34-3 3s1.34 3 3 3zm-8 0c1.66 0 2.99-1.34 2.99-3S9.66 5 8 5C6.34 5 5 6.34 5 8s1.34 3 3 3zm0 2c-2.33 0-7 1.17-7 3.5V19h14v-2.5c0-2.33-4.67-3.5-7-3.5zm8 0c-.29 0-.62.02-.97.05 1.16.84 1.97 1.97 1.97 3.45V19h6v-2.5c0-2.33-4.67-3.5-7-3.5z"/></svg><span>Community</span></div>
  <div class="nav-item" id="navMore" onclick="showMore()"><svg viewBox="0 0 24 24"><path class="icon" d="M6 10c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm12 0c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm-6 0c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2z"/></svg><span>More</span></div>
</div>

<!-- ME SCREEN -->
<div id="meScreen">
<!-- shared icon -->
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <symbol id="i-back" viewBox="0 0 24 24"><path d="M15 19l-7-7 7-7"/></symbol>
  <symbol id="i-user" viewBox="0 0 24 24"><path fill="currentColor" stroke="none" d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></symbol>
  <symbol id="i-sun" viewBox="0 0 24 24"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"/></symbol>
  <symbol id="i-moon" viewBox="0 0 24 24"><path d="M21 12.8A9 9 0 1 1 11.2 3a7 7 0 0 0 9.8 9.8z"/></symbol>
  <symbol id="i-monitor" viewBox="0 0 24 24"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8M12 17v4"/></symbol>
  <symbol id="i-search" viewBox="0 0 24 24"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.3-4.3"/></symbol>
  <symbol id="i-phone" viewBox="0 0 24 24"><path d="M22 16.9v3a2 2 0 0 1-2.2 2 19.8 19.8 0 0 1-8.6-3.1 19.5 19.5 0 0 1-6-6A19.8 19.8 0 0 1 2.1 4.2 2 2 0 0 1 4.1 2h3a2 2 0 0 1 2 1.7c.1 1 .4 1.9.7 2.8a2 2 0 0 1-.5 2.1L8.1 9.9a16 16 0 0 0 6 6l1.3-1.3a2 2 0 0 1 2.1-.4c.9.3 1.8.6 2.8.7a2 2 0 0 1 1.7 2z"/></symbol>
  <symbol id="i-mail" viewBox="0 0 24 24"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M22 7l-10 6L2 7"/></symbol>
  <symbol id="i-chat" viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></symbol>
  <symbol id="i-whatsapp" viewBox="0 0 24 24"><path d="M21 11.5a8.4 8.4 0 0 1-12.4 7.4L3 20.5l1.6-5.4A8.4 8.4 0 1 1 21 11.5z"/></symbol>
  <symbol id="i-alert" viewBox="0 0 24 24"><path d="M10.3 3.9L1.8 18a2 2 0 0 0 1.7 3h17a2 2 0 0 0 1.7-3L13.7 3.9a2 2 0 0 0-3.4 0z"/><path d="M12 9v4M12 17h.01"/></symbol>
</svg>

<!-- PROFILE -->
<div class="page active" id="page-profile">
  <div class="top-bar">
    <button class="back" aria-label="Back" onclick="closeMe()"><svg><use href="#i-back"/></svg></button>
    <div class="setting-wrap">
      <button class="setting-btn" aria-label="More" onclick="showPage('page-settings')"><svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none"><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/></svg></button>
      <span class="more-setting">More</span>
    </div>
  </div>
  <div class="profile-area">
    <button class="profile-btn profile-pic-target" aria-label="Profile picture" onclick="document.getElementById('mainProfileInput').click()"><svg viewBox="0 0 24 24"><use href="#i-user"/></svg></button>
    <div class="Name"><h1><u>Name</u></h1></div>
  </div>
  <div class="btn-info">
    <button>following</button><button>followers</button><button>friends</button>
  </div>
  <div class="postter">
    <button>video</button><button>Music</button><button>images</button><button>likes</button><button>gifts</button><button>Share</button><button>link</button>
  </div>
</div>

<!-- SETTINGS -->
<div class="page" id="page-settings">
  <button class="back" aria-label="Back" onclick="showPage('page-profile')"><svg><use href="#i-back"/></svg></button>

  <h2><u>Account</u></h2>
  <div class="grid">
    <div class="item" onclick="showPage('page-account-detail')"><svg viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg><span>Account</span></div>
    <div class="item" onclick="showPage('page-menu')"><svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><circle cx="20" cy="7" r="2"/></svg><span>Control</span></div>
  </div>

  <h2><u>Activity</u></h2>
  <div class="grid">
    <div class="item" onclick="showPage('page-theme')"><svg viewBox="0 0 24 24"><rect x="2" y="2" width="20" height="20" rx="2"/><circle cx="8.5" cy="8.5" r="1.5" fill="currentColor"/><polyline points="21 15 16 10 5 21"/></svg><span>Theme</span></div>
    <div class="item" onclick="soon()"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><polygon points="10 8 16 12 10 16 10 8" fill="currentColor" stroke="none"/></svg><span>ADS</span></div>
    <div class="item" onclick="showPage('page-text')"><svg viewBox="0 0 24 24"><circle cx="12" cy="7" r="4"/><path d="M12 11v6"/><path d="M9 14l-4 2 8 3 8-3-4-2"/></svg><span>Accessibility</span></div>
  </div>

  <h2><u>Cache</u></h2>
  <div class="grid">
    <div class="item" onclick="showPage('page-storage')"><svg viewBox="0 0 24 24"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg><span>Free up space</span></div>
    <div class="item" onclick="showPage('page-data')"><svg viewBox="0 0 24 24"><path d="M12 20v-6M6 20V10M18 20V4"/><path d="M2 6l3-3 3 3"/><path d="M22 18l-3 3-3-3"/></svg><span>Data Saver</span></div>
    <div class="item" onclick="showPage('page-downloads')"><svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg><span>Downloads</span></div>
  </div>

  <h2><u>Support</u></h2>
  <div class="grid">
    <div class="item" onclick="showPage('page-help')"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg><span>Help Centre</span></div>
    <div class="item" onclick="showPage('page-report')"><svg viewBox="0 0 24 24"><path d="M4 15s1-1 4-1 5 2 8 2 4-1 4-1V3s-1 1-4 1-5-2-8-2-4 1-4 1z"/><line x1="4" y1="22" x2="4" y2="15"/></svg><span>Report</span></div>
    <div class="item" onclick="showPage('page-contact')"><svg viewBox="0 0 24 24"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg><span>Contact help line</span></div>
  </div>

  <h2><u>About</u></h2>
  <div class="grid">
    <div class="item" onclick="soon()"><svg viewBox="0 0 24 24"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/><circle cx="12" cy="16" r="1" fill="currentColor" stroke="none"/></svg><span>privacy &amp; services</span></div>
    <div class="item" onclick="showPage('page-terms')"><svg viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/></svg><span>Terms and policies</span></div>
  </div>

  <h2><u>Changes</u></h2>
  <div class="grid">
    <div class="item" onclick="showPage('page-blocked')"><svg viewBox="0 0 24 24"><circle cx="10" cy="8" r="4.5"/><path d="M2 21a8 8 0 0 1 10-7.7"/><circle cx="18" cy="14" r="6"/><line x1="14" y1="10" x2="22" y2="18"/></svg><span>Blocked accounts</span></div>
    <div class="item" onclick="soon()"><svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><line x1="20" y1="8" x2="20" y2="14"/><line x1="23" y1="11" x2="17" y2="11"/></svg><span>Account Manager</span></div>
    <div class="item" onclick="doLogout()"><svg viewBox="0 0 24 24"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg><span>Log out</span></div>
  </div>
</div>

<!-- ACCOUNT DETAIL -->
<div class="page" id="page-account-detail">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Account</b>
  </div>
  <div class="wrap">
    <div class="profile-wrap">
      <div class="profile profile-pic-target" onclick="showPage('page-account')"><svg viewBox="0 0 24 24"><use href="#i-user"/></svg></div>
    </div>
    <div class="card" style="margin-top:20px">
      <ul class="personal">
        <li>Name<input class="int" type="text" value="John Mungai" placeholder="Enter your name" required></li>
        <li>Email<input class="int" type="email" value="Email@gmail.com" readonly></li>
        <li>Phone<input class="int" type="tel" value="+1234567891" placeholder="phone" required></li>
      </ul>
    </div>
    <div class="card add-row">
      <div class="profile-add profile-pic-target"><svg viewBox="0 0 24 24"><use href="#i-user"/></svg></div>
      <div class="add-text"><div class="name-add">Name</div><div class="email-add">email@gmail.com</div></div>
    </div>
  </div>
  <div class="fab-wrap">
    <button class="nav-add" aria-label="Add account" onclick="showToast('Add account is not available yet')">
      <svg width="28" height="28" viewBox="0 0 24 24" fill="white"><path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/></svg>
    </button>
  </div>
</div>

<!-- CONTROL MENU -->
<div id="page-menu" class="page">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <h1>Control</h1>
  </div>
  <div class="wrap">
    <h4 class="section-title">Information</h4>
    <div class="card">
      <button class="row" onclick="showPage('page-account')"><span>Account Information</span><span class="chev">›</span></button>
      <button class="row" onclick="showPage('page-password')"><span>Password</span><span class="chev">›</span></button>
      <button class="row" onclick="showPage('page-business')"><span>Personal or Business</span><span class="chev">›</span></button>
    </div>
  </div>
</div>

<!-- ACCOUNT INFORMATION -->
<div id="page-account" class="page">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-menu')"><svg><use href="#i-back"/></svg></button>
    <h1>Account Information</h1>
  </div>
  <div class="wrap">
    <div class="card pad"><h2>Profile</h2><input type="file" accept="image/*" id="profileInput"><div class="profile-preview profile-pic-target" id="avatarPreview"><svg viewBox="0 0 24 24" width="40" height="40" style="color:#999"><use href="#i-user"/></svg></div></div>
    <div class="card pad"><h2>Name</h2><input class="int" type="text" value="John Mungai" readonly></div>
    <div class="card pad"><h2>Email</h2><input class="int" type="email" value="email@gmail.com" readonly></div>
    <div class="card pad"><h2>Date of birth</h2><input class="int" type="date" value="2000-01-01"></div>
    <div class="card pad"><h2>Date of Registration</h2><input class="int" type="text" value="18 Sep 2026" readonly></div>
  </div>
</div>

<!-- PASSWORD -->
<div id="page-password" class="page">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-menu')"><svg><use href="#i-back"/></svg></button>
    <h1>Password</h1>
  </div>
  <div class="wrap">
    <div class="card pad">
      <input class="int" id="pwd" type="password" value="1234594idkqjdjd" readonly style="letter-spacing:1px">
      <div class="btns">
        <button class="btn black" id="showBtn" onclick="togglePassword()">Show</button>
        <button class="btn green" id="copyBtn" onclick="copyPassword()">Copy</button>
      </div>
      <p id="msg" style="font-size:12px;color:var(--text2);margin-top:10px"></p>
    </div>
  </div>
</div>

<!-- BUSINESS -->
<div id="page-business" class="page">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-menu')"><svg><use href="#i-back"/></svg></button>
    <h1>Account Type</h1>
  </div>
  <div class="wrap">
    <div class="card pad"><h2>Personal</h2><p style="font-size:14px;line-height:1.5">For friends, privacy, simple posting. No analytics or ads.</p><button class="btn black full" onclick="showToast('Personal selected')">Use Personal</button></div>
    <div class="card pad"><h2>Business</h2><p style="font-size:14px;line-height:1.5">For selling, growth, ads, insights, shop, contact button.</p><button class="btn green full" onclick="showToast('Business selected')">Use Business</button></div>
  </div>
</div>

<!-- THEME -->
<div id="page-theme" class="page">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <h1>Dark mode</h1>
  </div>
  <div class="section">Display preferences</div>
  <div class="wrap" style="padding-top:0">
    <div class="card">
      <button class="row" onclick="setTheme('light')">
        <div class="left-icon"><div class="icon"><svg viewBox="0 0 24 24"><use href="#i-sun"/></svg></div><div><b>Off</b><p>Light mode</p></div></div>
        <div class="radio" id="r-light"></div>
      </button>
      <button class="row" onclick="setTheme('dark')">
        <div class="left-icon"><div class="icon"><svg viewBox="0 0 24 24"><use href="#i-moon"/></svg></div><div><b>On</b><p>Dark mode</p></div></div>
        <div class="radio" id="r-dark"></div>
      </button>
      <button class="row" onclick="setTheme('auto')">
        <div class="left-icon"><div class="icon"><svg viewBox="0 0 24 24"><use href="#i-monitor"/></svg></div><div><b>Use system settings</b><p>We’ll adjust your appearance based on your device settings</p></div></div>
        <div class="radio" id="r-auto"></div>
      </button>
    </div>
  </div>
</div>

<!-- TEXT SIZE -->
<div class="page" id="page-text">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Text Size</b>
  </div>
  <div class="wrap">
    <div class="card pad">
      <div class="label"><span>Adjust Text Size</span><span id="sizeLabel">Normal</span></div>
      <input type="range" min="1" max="4" value="2" id="sizeRange" oninput="setSize(this.value)">
      <div class="sizes"><span style="font-size:11px">A</span><span style="font-size:13px">A</span><span style="font-size:16px">A</span><span style="font-size:20px">A</span></div>
      <div class="preview-box">
        <b>Preview</b>
        <p style="margin-top:8px;color:var(--text2);line-height:1.4">The quick brown fox jumps over the lazy dog. This is how text will look.</p>
      </div>
    </div>
  </div>
</div>

<!-- DATA SAVER -->
<div class="page" id="page-data">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Data Saver</b>
  </div>
  <div class="wrap">
    <div class="section">Manage data usage</div>
    <div class="card">
      <div class="row" style="cursor:default">
        <div><b>Data Saver</b><p id="mainText">On • Saving data</p></div>
        <div class="switch on" id="main" onclick="toggleSwitch('main')"><div class="knob"></div></div>
      </div>
    </div>
    <div class="section">Options</div>
    <div class="card" id="opts">
      <div class="row" style="cursor:default">
        <div><b>Less data for videos</b><p>Lower quality</p></div>
        <div class="switch on" id="s1" onclick="toggleSwitch('s1')"><div class="knob"></div></div>
      </div>
      <div class="row" style="cursor:default">
        <div><b>Auto-play on mobile data</b><p>Stop auto-play</p></div>
        <div class="switch on" id="s2" onclick="toggleSwitch('s2')"><div class="knob"></div></div>
      </div>
      <div class="row" style="cursor:default">
        <div><b>Reduce image quality</b><p>Lower quality images</p></div>
        <div class="switch" id="s3" onclick="toggleSwitch('s3')"><div class="knob"></div></div>
      </div>
    </div>
  </div>
</div>

<!-- STORAGE -->
<div class="page" id="page-storage">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Storage</b>
  </div>
  <div class="wrap">
    <div class="card">
      <div class="row" style="cursor:default">
        <div class="grow">
          <b>Cache</b>
          <p id="cacheText">20.8 MB used on this device</p>
          <div class="bar-wrap"><div class="bar" id="bar"></div></div>
        </div>
        <div class="right" id="mb">20.8 MB</div>
      </div>
      <div class="row" style="cursor:default">
        <div><b>Clear Cache</b><p>Free up space, your data is safe</p></div>
        <button class="btn" id="clearBtn" onclick="clearCache()">Clear</button>
      </div>
    </div>
  </div>
</div>

<!-- DOWNLOAD QUALITY -->
<div class="page" id="page-downloads">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Download Quality</b>
  </div>
  <div class="note">Choose the quality for videos you download for offline viewing. Higher quality uses more storage.</div>
  <div class="wrap" style="padding-top:0">
    <div class="card">
      <div class="row" onclick="selectQ(0)">
        <div><div style="display:flex;align-items:center"><b>Data Saver</b><span class="badge gray">~ 30 MB / hour</span></div><p>Lower quality, saves data and storage</p></div>
        <div class="radio" id="r0"></div>
      </div>
      <div class="row" onclick="selectQ(1)">
        <div><div style="display:flex;align-items:center"><b>Standard</b><span class="badge green">Recommended</span></div><p>Good quality, balanced storage</p></div>
        <div class="radio on" id="r1"></div>
      </div>
      <div class="row" onclick="selectQ(2)">
        <div><div style="display:flex;align-items:center"><b>High</b><span class="badge gray">~ 120 MB / hour</span></div><p>Better quality, uses more storage</p></div>
        <div class="radio" id="r2"></div>
      </div>
      <div class="row" onclick="selectQ(3)">
        <div><div style="display:flex;align-items:center"><b>Ultra High</b><span class="badge gray">~ 300 MB / hour</span></div><p>Best quality, uses most storage</p></div>
        <div class="radio" id="r3"></div>
      </div>
    </div>
    <div class="card pad">
      <b style="font-size:13px">Storage used</b>
      <p id="storageInfo" style="font-size:13px;color:var(--text2);margin-top:6px;line-height:1.4"></p>
    </div>
  </div>
</div>

<!-- REPORT -->
<div class="page" id="page-report">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Report a Problem</b>
  </div>
  <div class="wrap">
    <div class="section" style="padding-left:4px">Select a problem to report</div>
    <div class="card">
      <button class="row" onclick="openReport('Storage issue')"><b>Storage issue</b><span class="chev">›</span></button>
      <button class="row" onclick="openReport('Download not working')"><b>Download not working</b><span class="chev">›</span></button>
      <button class="row" onclick="openReport('Data Saver not working')"><b>Data Saver not working</b><span class="chev">›</span></button>
      <button class="row" onclick="openReport('Text size problem')"><b>Text size problem</b><span class="chev">›</span></button>
      <button class="row" onclick="openReport('Something not working')"><b>Something not working</b><span class="chev">›</span></button>
      <button class="row" onclick="openReport('Other issue')"><b>Other issue</b><span class="chev">›</span></button>
    </div>
  </div>
</div>

<!-- HELP CENTRE -->
<div class="page" id="page-help">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Video Help</b>
  </div>
  <div class="search"><svg viewBox="0 0 24 24"><use href="#i-search"/></svg><input id="helpSearch" placeholder="Search for help..." oninput="filterHelp(this.value)"></div>
  <div class="wrap" style="padding-top:0">
    <div class="card" id="helpList">
      <button class="row" onclick="showHelp('play')"><div><b>Play/Pause Issues</b><p>Video not playing or pausing</p></div><span class="chev">›</span></button>
      <button class="row" onclick="showHelp('quality')"><div><b>Video Quality</b><p>Change quality or fix blurry videos</p></div><span class="chev">›</span></button>
      <button class="row" onclick="showHelp('download')"><div><b>Downloads</b><p>Manage downloaded videos</p></div><span class="chev">›</span></button>
      <button class="row" onclick="showHelp('subtitles')"><div><b>Subtitles</b><p>Turn subtitles on/off</p></div><span class="chev">›</span></button>
      <button class="row" onclick="showHelp('audio')"><div><b>Audio Issues</b><p>No sound or audio lag</p></div><span class="chev">›</span></button>
      <button class="row" onclick="showPage('page-report')"><div><b>Report a Problem</b><p>Let us know if something isn't working</p></div><span class="chev">›</span></button>
    </div>
  </div>
</div>

<!-- CONTACT -->
<div class="page" id="page-contact">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Contact Us</b>
  </div>
  <div class="wrap">
    <div class="card">
      <button class="row" onclick="location.href='tel:+1234567890'"><div class="left-icon"><div class="icon" style="background:#e7f3ff;color:#222"><svg viewBox="0 0 24 24"><use href="#i-phone"/></svg></div><div><b>Call Helpline</b><p>+1 234 567 890 • 24/7</p></div></div></button>
      <button class="row" onclick="location.href='mailto:support@videoplayer.com'"><div class="left-icon"><div class="icon" style="background:#e7ffe7;color:#222"><svg viewBox="0 0 24 24"><use href="#i-mail"/></svg></div><div><b>Email Support</b><p>support@videoplayer.com</p></div></div></button>
      <button class="row" onclick="showToast('Opening chat...')"><div class="left-icon"><div class="icon" style="background:#fff0e7;color:#222"><svg viewBox="0 0 24 24"><use href="#i-chat"/></svg></div><div><b>Live Chat</b><p>Usually replies in 2 min</p></div></div></button>
      <button class="row" onclick="window.open('https://wa.me/1234567890','_blank')"><div class="left-icon"><div class="icon" style="background:#e7ffe7;color:#222"><svg viewBox="0 0 24 24"><use href="#i-whatsapp"/></svg></div><div><b>WhatsApp</b><p>Message us directly</p></div></div></button>
    </div>
    <div class="card">
      <button class="row" onclick="showPage('page-report')"><div class="left-icon"><div class="icon" style="background:#ffe7e7;color:#222"><svg viewBox="0 0 24 24"><use href="#i-alert"/></svg></div><div><b>Report a Problem</b><p>Video not playing, crash, bug</p></div></div></button>
    </div>
  </div>
</div>

<!-- TERMS -->
<div class="page" id="page-terms">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Terms &amp; Policies</b>
  </div>
  <div class="wrap">
    <div class="card">
      <button class="row" onclick="toggleTerm('t1')"><b>Terms of Service</b><span class="chev" id="i-t1">›</span></button>
      <div class="content" id="t1">
        <h3>1. Acceptance</h3><p>By using Video Player, you agree to these terms. If you don't agree, please uninstall.</p>
        <h3>2. Use License</h3><p>For personal, non-commercial use only. Don't copy, modify, or redistribute videos.</p>
        <h3>3. Updates</h3><p>We may update Data Saver, quality, and features to improve performance.</p>
      </div>
      <button class="row" onclick="toggleTerm('t2')"><b>Privacy Policy</b><span class="chev" id="i-t2">›</span></button>
      <div class="content" id="t2">
        <h3>What we collect</h3><p>Cache size (e.g. 20.8 MB), watch history, settings like text size. No data is sold.</p>
        <h3>Storage</h3><p>All videos stay on device. You can clear cache anytime in Settings &gt; Storage.</p>
        <h3>Contact</h3><p>Email privacy@videoplayer.com for any privacy request.</p>
      </div>
      <button class="row" onclick="toggleTerm('t3')"><b>Data Usage Policy</b><span class="chev" id="i-t3">›</span></button>
      <div class="content" id="t3">
        <h3>Data Saver</h3><p>When ON, we reduce video quality and stop auto-play to save mobile data.</p>
        <h3>Your responsibility</h3><p>If Data Saver is OFF, HD videos may use more data. We are not liable for carrier charges.</p>
      </div>
      <button class="row" onclick="toggleTerm('t4')"><b>Refund Policy</b><span class="chev" id="i-t4">›</span></button>
      <div class="content" id="t4"><p>Premium purchases are refundable within 7 days if not used. Contact support@videoplayer.com</p></div>
      <button class="row" onclick="toggleTerm('t5')"><b>Community Guidelines</b><span class="chev" id="i-t5">›</span></button>
      <div class="content" id="t5"><p>No spam, illegal content, or copyright violation in comments or uploads. Violations may lead to account ban.</p></div>
    </div>
    <div style="padding:16px;text-align:center;font-size:12px;color:var(--text2)">
      Version 2.4.1 • Last updated Sep 19, 2026<br>© 2026 Video Player
    </div>
  </div>
</div>

<!-- BLOCKED ACCOUNTS -->
<div class="page" id="page-blocked">
  <div class="header">
    <button class="back" aria-label="Back" onclick="showPage('page-settings')"><svg><use href="#i-back"/></svg></button>
    <b>Blocked Accounts</b>
  </div>
  <div class="wrap">
    <div class="card" id="blockList">
      <button class="row" onclick="unblock(this,'@movie_lover')"><div class="left-icon"><div class="icon"><svg viewBox="0 0 24 24"><use href="#i-user"/></svg></div><div><b>movie_lover</b><p>Tap to unblock</p></div></div><span style="color:var(--blue);font-size:14px;font-weight:600">Unblock</span></button>
      <button class="row" onclick="unblock(this,'@spam_account')"><div class="left-icon"><div class="icon"><svg viewBox="0 0 24 24"><use href="#i-user"/></svg></div><div><b>spam_account</b><p>Tap to unblock</p></div></div><span style="color:var(--blue);font-size:14px;font-weight:600">Unblock</span></button>
    </div>
    <div class="card pad">
      <div style="display:flex;gap:10px">
        <input class="int" id="blockUser" placeholder="@username to block" style="flex:1">
        <button class="btn black" onclick="blockUser()">Block</button>
      </div>
    </div>
  </div>
</div>

<!-- MODALS + TOAST (outside pages so they can always show) -->
<div class="modal bottom" id="reportModal" onclick="if(event.target===this)closeReport()">
  <div class="sheet">
    <b id="issueTitle">Report</b>
    <textarea id="details" placeholder="Describe what happened..."></textarea>
    <div class="btns">
      <button class="btn" onclick="closeReport()">Cancel</button>
      <button class="btn blue" onclick="submitReport()">Send Report</button>
    </div>
  </div>
</div>

<div class="modal" id="blockModal" onclick="if(event.target===this)closeBlock()">
  <div class="box">
    <b id="mtitle">Block account?</b>
    <p id="mtext"></p>
    <div class="btns">
      <button class="btn" onclick="closeBlock()">Cancel</button>
      <button class="btn red" id="maction" onclick="confirmAction()">Block</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

</div>

<script>
const $ = id => document.getElementById(id);

/* ---------- nav highlight ---------- */
function setTopActive(el){
  document.querySelectorAll('.top-nav .nav-btn').forEach(b=>b.classList.remove('active'));
  el.classList.add('active');
}
function setBottomActive(el){
  document.querySelectorAll('.bottom-nav .nav-item').forEach(b=>b.classList.remove('active'));
  el.classList.add('active');
}
// after a popup closes, highlight whichever screen is actually showing
function syncBottomActive(){
  setBottomActive($(trendsOpen ? 'navTrends' : 'navHome'));
}
function goHome(){
  closeTrends();
  closePopup();
  hideMore();
  setBottomActive($('navHome'));
}

/* ---------- post popup ---------- */
function togglePopup(){
  if($('popup').classList.contains('show')){ closePopup(); }
  else{ $('popup').classList.add('show'); $('postOverlay').classList.add('show'); }
}
function closePopup(){
  $('popup').classList.remove('show');
  $('postOverlay').classList.remove('show');
  syncBottomActive();
}
let previewURL = null;
function showPreview(e){
  const f = e.target.files[0];
  if(!f) return;
  if(previewURL) URL.revokeObjectURL(previewURL);
  previewURL = URL.createObjectURL(f);
  const img = $('preview'), vid = $('previewVideo');
  img.style.display = 'none';
  vid.style.display = 'none';
  if(f.type.startsWith('video/')){
    vid.src = previewURL; vid.style.display = 'block';
  }else{
    img.src = previewURL; img.style.display = 'block';
  }
  e.target.value = '';
  closePopup();
}

/* ---------- more popup ---------- */
function showMore(){
  setBottomActive($('navMore'));
  $('viewMore').classList.add('show');
  $('moreOverlay').classList.add('show');
}
function hideMore(){
  $('viewMore').classList.remove('show');
  $('moreOverlay').classList.remove('show');
  syncBottomActive();
}
function goTo(category){
  alert('Go to: ' + category);
  hideMore();
}
document.addEventListener('keydown', e=>{
  if(e.key === 'Escape'){ hideMore(); closePopup(); closeSearch(); }
});

/* ---------- search ---------- */
const searchPage = $('searchPage');
const searchInput = $('search');
function openSearch(){ searchPage.classList.add('open'); searchInput.focus(); }
function closeSearch(){
  searchPage.classList.remove('open');
  setTopActive(document.querySelector('.top-nav .nav-btn')); // back to Reels
}
function fillSearch(el){ searchInput.value = el.textContent.trim(); doSearch(); }
function removeItem(btn){ btn.parentElement.remove(); }
function clearAll(){ $('history-list').innerHTML = ''; }
function doSearch(){
  const q = searchInput.value.trim();
  if(!q) return;
  addToHistory(q);
  closeSearch();
  alert('Searching for: ' + q);
}
function addToHistory(q){
  const list = $('history-list');
  const exists = [...list.querySelectorAll('.history-left')].some(el=>el.textContent.trim().toLowerCase() === q.toLowerCase());
  if(exists) return;
  const div = document.createElement('div');
  div.className = 'search-history';
  div.innerHTML = '<div class="history-left" onclick="fillSearch(this)"></div>' +
    '<button class="remove-btn" onclick="removeItem(this)" aria-label="Remove"><svg viewBox="0 0 24 24"><path d="M6 6l12 12M18 6L6 18"/></svg></button>';
  div.querySelector('.history-left').textContent = q;   // textContent: no HTML injection
  list.prepend(div);
}
searchInput.addEventListener('keydown', e=>{ if(e.key === 'Enter') doSearch(); });

/* ---------- trends ---------- */
let trendsOpen = false;
function openTrends(){
  trendsOpen = true;
  $('trendsPage').classList.add('open');
  closePopup();
  hideMore();
  setBottomActive($('navTrends'));
}
function closeTrends(){
  trendsOpen = false;
  $('trendsPage').classList.remove('open');
  document.querySelectorAll('#trendsVideos video').forEach(v=>v.pause());
}
document.querySelectorAll('#topBar button').forEach(b=>{
  b.onclick = ()=>{
    document.querySelectorAll('#topBar button').forEach(x=>x.classList.remove('active'));
    b.classList.add('active');
    const cat = b.dataset.cat;
    document.querySelectorAll('#trendsVideos .card').forEach(c=>{
      c.style.display = (cat === 'all' || c.dataset.cat === cat) ? '' : 'none';
    });
  };
});
</script>

<script>
/* ---------- in-memory storage (kept for this session only; no browser storage APIs here) ---------- */
const memoryStore = {};
const store = {
  get(k, d){ return Object.prototype.hasOwnProperty.call(memoryStore, k) ? memoryStore[k] : d; },
  set(k, v){ memoryStore[k] = v; }
};

/* ---------- navigation + toast ---------- */
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  const el = document.getElementById(id);
  if(el) el.classList.add('active');
  document.getElementById('meScreen').scrollTop = 0;
}
let toastTimer;
function showToast(text){
  const t = document.getElementById('toast');
  t.textContent = text;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(()=>t.classList.remove('show'), 2500);
}
function soon(){ showToast('Coming soon'); }

/* ---------- profile picture preview ---------- */
const profileInput = document.getElementById('profileInput');
profileInput.addEventListener('change', ()=>{
  const f = profileInput.files[0];
  if(!f) return;
  applyProfilePicture(URL.createObjectURL(f));
  profileInput.value = '';
});

/* ---------- password ---------- */
const pwd = document.getElementById('pwd');
const showBtn = document.getElementById('showBtn');
const copyBtn = document.getElementById('copyBtn');
const msg = document.getElementById('msg');
function togglePassword(){
  const hidden = pwd.type === 'password';
  pwd.type = hidden ? 'text' : 'password';
  showBtn.textContent = hidden ? 'Hide' : 'Show';
}
function copyPassword(){
  const done = ()=>{
    msg.textContent = 'Copied';
    copyBtn.textContent = 'Copied';
    setTimeout(()=>{ copyBtn.textContent = 'Copy'; msg.textContent = ''; }, 2000);
  };
  const fallback = ()=>{
    const prev = pwd.type;
    pwd.type = 'text'; pwd.select(); pwd.setSelectionRange(0, 99999);
    let ok = false;
    try{ ok = document.execCommand('copy'); }catch(e){}
    pwd.type = prev;
    if(ok) done(); else msg.textContent = 'Could not copy';
  };
  if(navigator.clipboard && window.isSecureContext){
    navigator.clipboard.writeText(pwd.value).then(done, fallback);
  }else{
    fallback();
  }
}

/* ---------- theme ---------- */
const darkQuery = window.matchMedia('(prefers-color-scheme: dark)');
function setTheme(t){
  ['light','dark','auto'].forEach(k=>document.getElementById('r-'+k).classList.remove('on'));
  document.getElementById('r-'+t).classList.add('on');
  store.set('nexus_theme', t);
  const dark = t === 'dark' || (t === 'auto' && darkQuery.matches);
  document.documentElement.setAttribute('data-theme', dark ? 'dark' : 'light');
}
darkQuery.addEventListener('change', ()=>{ if(store.get('nexus_theme','light') === 'auto') setTheme('auto'); });
setTheme(store.get('nexus_theme','light'));

/* ---------- text size ---------- */
let size = parseInt(store.get('nexus_textSize', '2'), 10);
function applySize(){
  const zoom = [0.9, 1, 1.15, 1.35];
  const labels = ['Small','Normal','Large','Extra Large'];
  document.getElementById('appBody').style.zoom = zoom[size-1];
  document.getElementById('sizeLabel').textContent = labels[size-1];
  document.getElementById('sizeRange').value = size;
  store.set('nexus_textSize', size);
}
function setSize(v){ size = parseInt(v, 10); applySize(); }
applySize();

/* ---------- data saver ---------- */
let ds;
try{ ds = JSON.parse(store.get('nexus_ds', '')); }catch(e){}
if(!ds) ds = {main:true, s1:true, s2:true, s3:false};
function renderSwitches(){
  for(const k in ds){ document.getElementById(k).className = 'switch' + (ds[k] ? ' on' : ''); }
  document.getElementById('mainText').textContent = ds.main ? 'On • Saving data' : 'Off';
  const opts = document.getElementById('opts');
  opts.style.opacity = ds.main ? '1' : '0.4';
  opts.style.pointerEvents = ds.main ? 'auto' : 'none';
  store.set('nexus_ds', JSON.stringify(ds));
}
function toggleSwitch(k){ ds[k] = !ds[k]; renderSwitches(); if(navigator.vibrate) navigator.vibrate(15); }
renderSwitches();

/* ---------- cache ---------- */
let cacheMB = 20.8;
function clearCache(){
  const btn = document.getElementById('clearBtn');
  btn.textContent = 'Clearing...';
  btn.disabled = true;
  setTimeout(()=>{
    cacheMB = 0;
    document.getElementById('mb').textContent = '0 MB';
    document.getElementById('cacheText').textContent = '0 MB used - Cache cleared';
    document.getElementById('bar').style.width = '0%';
    btn.textContent = 'Cleared';
    btn.classList.add('done');
    updateStorageInfo();
    setTimeout(()=>{ btn.textContent = 'Clear'; btn.disabled = false; btn.classList.remove('done'); }, 2000);
  }, 700);
}

/* ---------- download quality ---------- */
const qualities = [
  {name:'Data Saver', mb:30},
  {name:'Standard', mb:70},
  {name:'High', mb:120},
  {name:'Ultra High', mb:300}
];
let currentQ = parseInt(store.get('nexus_downloadQ','1'), 10);
function updateStorageInfo(){
  const q = qualities[currentQ];
  document.getElementById('storageInfo').textContent =
    `${q.name} • About ${q.mb} MB per hour • Current cache: ${cacheMB === 0 ? '0' : cacheMB} MB`;
}
function selectQ(i){
  currentQ = i;
  qualities.forEach((_, n)=>document.getElementById('r'+n).classList.toggle('on', n === i));
  store.set('nexus_downloadQ', i);
  updateStorageInfo();
  if(navigator.vibrate) navigator.vibrate(20);
}
selectQ(currentQ);

/* ---------- report ---------- */
function openReport(title){
  document.getElementById('issueTitle').textContent = title;
  document.getElementById('reportModal').classList.add('show');
}
function closeReport(){ document.getElementById('reportModal').classList.remove('show'); }
function submitReport(){
  closeReport();
  document.getElementById('details').value = '';
  showToast('Report sent. Thank you!');
}

/* ---------- help ---------- */
const helpAnswers = {
  play:'Check your connection, then close and reopen the video.',
  quality:'Turn off Data Saver or pick a higher Download Quality.',
  download:'Manage downloads and quality in Settings > Downloads.',
  subtitles:'Open the video menu and choose Subtitles.',
  audio:'Check your volume and any connected Bluetooth devices.'
};
function showHelp(key){ showToast(helpAnswers[key]); }
function filterHelp(q){
  q = q.trim().toLowerCase();
  document.querySelectorAll('#helpList .row').forEach(r=>{
    r.style.display = r.textContent.toLowerCase().includes(q) ? '' : 'none';
  });
}

/* ---------- terms accordion ---------- */
function toggleTerm(id){
  const open = document.getElementById(id).classList.toggle('show');
  document.getElementById('i-'+id).textContent = open ? '⌄' : '›';
}

/* ---------- blocked accounts ---------- */
let blockTarget = null, blockAction = 'block', blockName = '';
function makeBlockedRow(name){
  const row = document.createElement('button');
  row.className = 'row';
  row.innerHTML = '<div class="left-icon"><div class="icon"><svg viewBox="0 0 24 24"><use href="#i-user"/></svg></div><div><b></b><p>Tap to unblock</p></div></div>' +
                  '<span style="color:var(--blue);font-size:14px;font-weight:600">Unblock</span>';
  row.querySelector('b').textContent = name.replace(/^@/, '');
  row.onclick = ()=>unblock(row, name);
  return row;
}
function blockUser(){
  const name = document.getElementById('blockUser').value.trim();
  if(!name) return;
  blockName = name.startsWith('@') ? name : '@' + name;
  blockAction = 'block';
  document.getElementById('mtitle').textContent = 'Block ' + blockName + '?';
  document.getElementById('mtext').textContent = "They won't be able to comment or message you. We won't notify them.";
  document.getElementById('maction').textContent = 'Block';
  document.getElementById('blockModal').classList.add('show');
}
function unblock(el, name){
  blockTarget = el; blockName = name; blockAction = 'unblock';
  document.getElementById('mtitle').textContent = 'Unblock ' + name + '?';
  document.getElementById('mtext').textContent = 'They will be able to see your comments again.';
  document.getElementById('maction').textContent = 'Unblock';
  document.getElementById('blockModal').classList.add('show');
}
function closeBlock(){ document.getElementById('blockModal').classList.remove('show'); }
function confirmAction(){
  if(blockAction === 'block'){
    document.getElementById('blockList').appendChild(makeBlockedRow(blockName));
    document.getElementById('blockUser').value = '';
  }else if(blockTarget){
    blockTarget.remove();
    blockTarget = null;
  }
  closeBlock();
}

/* ---------- open / close the Me screen ---------- */
function openMe(){
  document.getElementById('meScreen').classList.add('open');
  showPage('page-profile');
}
function closeMe(){
  document.getElementById('meScreen').classList.remove('open');
  setTopActive(document.querySelector('.top-nav .nav-btn')); // back to Reels
}
</script>

<script>
/* ================= AUTH GATE ================= */
function authShowForm(formId){
  document.querySelectorAll('#authScreen .form-box').forEach(f=>f.classList.remove('active'));
  document.getElementById(formId).classList.add('active');
}
function authTogglePass(inputId, el){
  const input = document.getElementById(inputId);
  if(input.type === 'password'){ input.type = 'text'; el.textContent = 'Hide'; }
  else{ input.type = 'password'; el.textContent = 'Show'; }
}
function authLoginSubmit(){
  const email = document.getElementById('login-email').value.trim();
  const pass = document.getElementById('login-pass').value;
  const err = document.getElementById('login-error');
  if(!email || !pass){
    err.textContent = 'Please enter your email and password.';
    err.style.display = 'block';
    return false;
  }
  err.style.display = 'none';
  authShowForm('phone-form');
  return false;
}
function authValidateRegister(){
  const pass = document.getElementById('reg-pass').value;
  const confirm = document.getElementById('confirm-pass').value;
  const error = document.getElementById('reg-error');
  const regex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,16}$/;

  if(!regex.test(pass)){
    error.textContent = 'Password must be 8-16 chars with both letters and numbers.';
    error.style.display = 'block';
    return false;
  }
  if(pass !== confirm){
    error.textContent = 'Passwords do not match!';
    error.style.display = 'block';
    return false;
  }
  error.style.display = 'none';
  authShowForm('phone-form');
  return false;
}
function enterApp(){
  document.getElementById('authScreen').style.display = 'none';
  document.body.classList.remove('auth-active');
  showToast('Welcome!');
}
function doLogout(){
  closeMe();
  goHome();
  authShowForm('login-form');
  document.getElementById('login-email').value = '';
  document.getElementById('login-pass').value = '';
  document.getElementById('login-error').style.display = 'none';
  document.getElementById('authScreen').style.display = 'flex';
  document.body.classList.add('auth-active');
}

/* ================= PROFILE PICTURE PROPAGATION ================= */
let profilePictureURL = null;
function applyProfilePicture(url){
  profilePictureURL = url;
  document.querySelectorAll('.profile-pic-target').forEach(el=>{
    el.innerHTML = '<img src="' + url + '" alt="Profile picture" style="width:100%;height:100%;object-fit:cover;border-radius:50%">';
  });
  const meIcon = document.getElementById('meNavIcon');
  const meImg = document.getElementById('meNavImg');
  if(meIcon) meIcon.style.display = 'none';
  if(meImg){ meImg.src = url; meImg.style.display = 'block'; }
}
const mainProfileInput = document.getElementById('mainProfileInput');
mainProfileInput.addEventListener('change', ()=>{
  const f = mainProfileInput.files[0];
  if(!f) return;
  applyProfilePicture(URL.createObjectURL(f));
  mainProfileInput.value = '';
});
</script>
</body>
</html># View-
Video, movies and music streaming app
