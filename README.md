# tplc-lms<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Technical Support Training</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,300&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<style>

:root{--bg:#0a0e14;--bg2:#111720;--bg3:#172030;--accent:#00d4ff;--accent2:#ff6b2b;--accent3:#39e075;--accent4:#f5c842;--text:#e8f0fe;--text2:#8aa0c0;--border:rgba(0,212,255,0.12);--card:rgba(17,23,32,0.97);--glow:rgba(0,212,255,0.15);--admin:#aa88ff;}
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;}
body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(0,212,255,0.03) 1px,transparent 1px),linear-gradient(90deg,rgba(0,212,255,0.03) 1px,transparent 1px);background-size:40px 40px;pointer-events:none;z-index:0;}

/* AUTH */
.auth-wrap{position:fixed;inset:0;display:none;align-items:center;justify-content:center;z-index:1000;background:var(--bg);}
.auth-wrap.hidden{display:none;}
.auth-glow{position:absolute;width:600px;height:600px;border-radius:50%;background:radial-gradient(circle,rgba(0,212,255,0.07) 0%,transparent 70%);pointer-events:none;animation:glowPulse 6s ease-in-out infinite;}
@keyframes glowPulse{0%,100%{transform:scale(1);}50%{transform:scale(1.1);opacity:0.7;}}
.auth-card{background:var(--card);border:1px solid var(--border);border-radius:20px;padding:38px 42px;width:100%;max-width:440px;position:relative;z-index:1;animation:slideUp 0.45s ease;}
@keyframes slideUp{from{opacity:0;transform:translateY(22px);}to{opacity:1;transform:translateY(0);}}
.auth-logo{text-align:center;margin-bottom:26px;}
.auth-logo-icon{width:54px;height:54px;background:linear-gradient(135deg,var(--accent),#0077aa);border-radius:14px;display:flex;align-items:center;justify-content:center;margin:0 auto 11px;box-shadow:0 0 28px rgba(0,212,255,0.3);}
.auth-logo-icon svg{width:27px;height:27px;}
.auth-logo-title{font-family:'Bebas Neue',sans-serif;font-size:21px;letter-spacing:2px;color:var(--accent);}
.auth-logo-sub{font-size:10.5px;color:var(--text2);letter-spacing:2px;text-transform:uppercase;margin-top:2px;}
.auth-tabs{display:flex;background:rgba(255,255,255,0.04);border-radius:9px;padding:3px;margin-bottom:26px;gap:3px;}
.auth-tab{flex:1;text-align:center;padding:9px;border-radius:7px;font-size:13px;font-weight:500;cursor:pointer;color:var(--text2);transition:all 0.2s;}
.auth-tab.active{background:var(--accent);color:#0a0e14;}
.form-group{margin-bottom:14px;}
.form-label{font-size:11px;color:var(--text2);letter-spacing:0.4px;margin-bottom:5px;display:block;}
.form-input{width:100%;background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:8px;padding:10px 13px;color:var(--text);font-size:13px;font-family:'DM Sans',sans-serif;outline:none;transition:all 0.2s;}
.form-input:focus{border-color:rgba(0,212,255,0.4);background:rgba(0,212,255,0.04);}
.form-input::placeholder{color:rgba(138,160,192,0.5);}
.form-input.error{border-color:rgba(255,107,43,0.5);}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.form-select{width:100%;background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:8px;padding:10px 13px;color:var(--text);font-size:13px;font-family:'DM Sans',sans-serif;outline:none;cursor:pointer;appearance:none;background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='11' height='11' viewBox='0 0 24 24' fill='none' stroke='%238aa0c0' stroke-width='2'%3E%3Cpolyline points='6 9 12 15 18 9'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 11px center;transition:all 0.2s;}
.form-select:focus{border-color:rgba(0,212,255,0.4);}
.form-select option{background:#111720;}
.err-msg{font-size:10.5px;color:var(--accent2);margin-top:3px;display:none;}
.err-msg.show{display:block;}
.btn-auth{width:100%;padding:12px;background:var(--accent);color:#0a0e14;border:none;border-radius:8px;font-size:13.5px;font-weight:600;font-family:'DM Sans',sans-serif;cursor:pointer;margin-top:6px;transition:all 0.2s;}
.btn-auth:hover{background:#22e8ff;box-shadow:0 0 22px rgba(0,212,255,0.3);}
.auth-divider{text-align:center;color:var(--text2);font-size:10.5px;margin:14px 0;position:relative;}
.auth-divider::before,.auth-divider::after{content:'';position:absolute;top:50%;width:42%;height:1px;background:var(--border);}
.auth-divider::before{left:0;}.auth-divider::after{right:0;}
.demo-box{background:rgba(0,212,255,0.04);border:1px solid rgba(0,212,255,0.1);border-radius:9px;padding:12px 14px;margin-top:14px;}
.demo-title{font-size:9.5px;letter-spacing:1.5px;text-transform:uppercase;color:var(--text2);margin-bottom:9px;}
.demo-row{display:flex;align-items:center;gap:8px;margin-bottom:7px;}
.demo-row:last-child{margin-bottom:0;}
.demo-role{font-size:11px;font-weight:500;min-width:60px;}
.demo-role.a{color:var(--admin);}.demo-role.p{color:var(--accent3);}
.demo-cred{font-size:10.5px;font-family:'JetBrains Mono',monospace;color:var(--text2);flex:1;}
.demo-fill{font-size:10px;padding:3px 9px;background:rgba(255,255,255,0.05);border:1px solid var(--border);border-radius:5px;cursor:pointer;color:var(--text2);transition:all 0.18s;}
.demo-fill:hover{border-color:var(--accent);color:var(--accent);}

/* TOAST */
.toast{position:fixed;top:18px;right:18px;z-index:9999;padding:11px 16px;border-radius:9px;font-size:12.5px;font-weight:500;transform:translateX(120%);transition:transform 0.3s cubic-bezier(0.4,0,0.2,1);max-width:300px;}
.toast.show{transform:translateX(0);}
.toast.success{background:#0d2a18;border:1px solid rgba(57,224,117,0.3);color:var(--accent3);}
.toast.error{background:#2a0d0d;border:1px solid rgba(255,107,43,0.3);color:var(--accent2);}
.toast.info{background:#0d1e2a;border:1px solid rgba(0,212,255,0.3);color:var(--accent);}

/* APP */
.app-wrap{display:none;}
.app-wrap.visible{display:flex;min-height:100vh;}

/* SIDEBAR */
.sidebar{position:fixed;left:0;top:0;bottom:0;width:256px;background:var(--bg2);border-right:1px solid var(--border);display:flex;flex-direction:column;z-index:100;}
.logo-area{padding:24px 22px 16px;border-bottom:1px solid var(--border);}
.logo-icon{width:36px;height:36px;background:linear-gradient(135deg,var(--accent),#0077aa);border-radius:8px;display:flex;align-items:center;justify-content:center;margin-bottom:9px;box-shadow:0 0 16px rgba(0,212,255,0.3);}
.logo-icon svg{width:19px;height:19px;}
.logo-title{font-family:'Bebas Neue',sans-serif;font-size:18px;letter-spacing:1.5px;color:var(--accent);line-height:1;}
.logo-sub{font-size:9px;color:var(--text2);letter-spacing:2px;text-transform:uppercase;margin-top:2px;}
.nav-section{flex:1;overflow-y:auto;padding:12px 0;}
.nav-label{font-size:8.5px;letter-spacing:2.5px;text-transform:uppercase;color:var(--text2);padding:10px 22px 4px;opacity:0.55;}
.nav-label.admin-only,.nav-item.admin-only{display:none;}
.nav-label.admin-only.show,.nav-item.admin-only.show{display:block;}
.nav-item.admin-only.show{display:flex;}
.nav-item{display:flex;align-items:center;gap:10px;padding:9px 22px;cursor:pointer;border-left:3px solid transparent;transition:all 0.18s;font-size:12.5px;color:var(--text2);}
.nav-item:hover{background:rgba(0,212,255,0.04);color:var(--text);}
.nav-item.active{border-left-color:var(--accent);background:rgba(0,212,255,0.06);color:var(--accent);}
.nav-item svg{width:16px;height:16px;opacity:0.7;flex-shrink:0;}
.nav-item.active svg{opacity:1;}
.nbadge{margin-left:auto;font-size:9.5px;padding:2px 6px;border-radius:20px;font-family:'JetBrains Mono',monospace;}
.nbadge.orange{background:var(--accent2);color:#fff;}
.nbadge.green{background:var(--accent3);color:#0a2010;}
.nbadge.purple{background:var(--admin);color:#0a0e14;}
.sidebar-footer{padding:12px 22px 16px;border-top:1px solid var(--border);}
.user-card{display:flex;align-items:center;gap:9px;cursor:pointer;padding:7px;border-radius:9px;transition:background 0.18s;margin:-7px;margin-bottom:0;}
.user-card:hover{background:rgba(255,255,255,0.03);}
.u-av{width:32px;height:32px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:700;flex-shrink:0;}
.u-name{font-size:12.5px;font-weight:500;}
.u-role{font-size:10px;color:var(--text2);}
.role-pill{margin-left:auto;font-size:9px;padding:2px 7px;border-radius:10px;font-family:'JetBrains Mono',monospace;font-weight:600;}
.rp-admin{background:rgba(170,136,255,0.15);color:var(--admin);border:1px solid rgba(170,136,255,0.3);}
.rp-peserta{background:rgba(57,224,117,0.1);color:var(--accent3);border:1px solid rgba(57,224,117,0.3);}
.btn-logout{margin-top:9px;width:100%;padding:7px;background:transparent;border:1px solid rgba(255,107,43,0.2);border-radius:7px;color:rgba(255,107,43,0.7);font-size:11.5px;font-family:'DM Sans',sans-serif;cursor:pointer;transition:all 0.18s;display:flex;align-items:center;justify-content:center;gap:5px;}
.btn-logout:hover{border-color:var(--accent2);color:var(--accent2);background:rgba(255,107,43,0.05);}

/* MAIN */
.main{margin-left:256px;min-height:100vh;position:relative;z-index:1;}
.topbar{height:60px;background:rgba(10,14,20,0.92);border-bottom:1px solid var(--border);display:flex;align-items:center;padding:0 28px;gap:12px;backdrop-filter:blur(12px);position:sticky;top:0;z-index:50;}
.topbar-title{font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:1px;color:var(--text);flex:1;}
.topbar-title span{color:var(--accent);}
.search-bar{display:flex;align-items:center;gap:7px;background:var(--bg2);border:1px solid var(--border);border-radius:7px;padding:7px 12px;width:210px;}
.search-bar input{background:transparent;border:none;outline:none;color:var(--text);font-size:12.5px;font-family:'DM Sans',sans-serif;width:100%;}
.search-bar input::placeholder{color:var(--text2);}
.icon-btn{width:32px;height:32px;background:var(--bg2);border:1px solid var(--border);border-radius:7px;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:all 0.18s;position:relative;}
.icon-btn:hover{border-color:var(--accent);}
.icon-btn .dot{position:absolute;top:5px;right:5px;width:6px;height:6px;background:var(--accent2);border-radius:50%;border:1.5px solid var(--bg);}
.content{padding:24px 28px;}
.section{display:none;}
.section.active{display:block;}

/* COMPONENT STYLES */
.stats-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:13px;margin-bottom:20px;}
.stat-card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:17px 19px;position:relative;overflow:hidden;transition:transform 0.2s,box-shadow 0.2s;}
.stat-card:hover{transform:translateY(-2px);box-shadow:0 8px 26px rgba(0,0,0,0.3);}
.stat-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;}
.c1::before{background:var(--accent);}.c2::before{background:var(--accent2);}.c3::before{background:var(--accent3);}.c4::before{background:var(--accent4);}
.stat-icon{width:38px;height:38px;border-radius:8px;display:flex;align-items:center;justify-content:center;margin-bottom:11px;}
.c1 .stat-icon{background:rgba(0,212,255,0.1);}.c2 .stat-icon{background:rgba(255,107,43,0.1);}.c3 .stat-icon{background:rgba(57,224,117,0.1);}.c4 .stat-icon{background:rgba(245,200,66,0.1);}
.stat-value{font-family:'Bebas Neue',sans-serif;font-size:32px;letter-spacing:1px;line-height:1;margin-bottom:3px;}
.c1 .stat-value{color:var(--accent);}.c2 .stat-value{color:var(--accent2);}.c3 .stat-value{color:var(--accent3);}.c4 .stat-value{color:var(--accent4);}
.stat-label{font-size:11px;color:var(--text2);}
.stat-delta{margin-top:6px;font-size:10px;font-family:'JetBrains Mono',monospace;color:var(--accent3);}
.stat-delta.down{color:var(--accent2);}
.card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:19px;}
.card-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;}
.card-title{font-family:'Bebas Neue',sans-serif;font-size:14.5px;letter-spacing:1px;color:var(--text);}
.card-badge{font-size:9.5px;font-family:'JetBrains Mono',monospace;padding:3px 8px;border-radius:20px;border:1px solid var(--border);color:var(--text2);}
.card-badge.live{border-color:rgba(57,224,117,0.3);color:var(--accent3);animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:0.6;}}
.two-col{display:grid;grid-template-columns:2fr 1fr;gap:13px;margin-bottom:20px;}
.three-col{display:grid;grid-template-columns:repeat(3,1fr);gap:13px;margin-bottom:20px;}
.progress-item{margin-bottom:13px;}
.progress-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:5px;font-size:12px;}
.progress-bar{height:5px;background:rgba(255,255,255,0.06);border-radius:3px;overflow:hidden;}
.progress-fill{height:100%;border-radius:3px;transition:width 1.5s cubic-bezier(0.4,0,0.2,1);}
.activity-item{display:flex;gap:11px;padding:9px 0;border-bottom:1px solid rgba(255,255,255,0.04);align-items:flex-start;}
.activity-item:last-child{border-bottom:none;}
.act-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;margin-top:5px;}
.act-text{font-size:12px;line-height:1.5;}
.act-text .act-name{font-weight:500;color:var(--text);}
.act-time{font-size:10px;color:var(--text2);margin-top:1px;font-family:'JetBrains Mono',monospace;}
.bar-chart{display:flex;align-items:flex-end;gap:6px;height:100px;padding-top:8px;}
.bar-col{flex:1;display:flex;flex-direction:column;align-items:center;gap:5px;height:100%;justify-content:flex-end;}
.bar{width:100%;border-radius:3px 3px 0 0;background:linear-gradient(180deg,var(--accent),rgba(0,212,255,0.3));min-height:3px;}
.bar.orange{background:linear-gradient(180deg,var(--accent2),rgba(255,107,43,0.3));}
.bar-label{font-size:9px;color:var(--text2);font-family:'JetBrains Mono',monospace;}
.donut-wrap{display:flex;align-items:center;gap:18px;}
.legend-item{display:flex;align-items:center;gap:8px;margin-bottom:8px;font-size:11.5px;}
.legend-dot{width:7px;height:7px;border-radius:2px;flex-shrink:0;}
.legend-val{margin-left:auto;font-family:'JetBrains Mono',monospace;font-size:11px;}
.hero-banner{background:linear-gradient(135deg,#0a2030 0%,#0d1520 40%,#14202a 100%);border:1px solid var(--border);border-radius:14px;padding:24px 28px;margin-bottom:20px;display:flex;align-items:center;justify-content:space-between;gap:22px;position:relative;overflow:hidden;}
.hero-banner::before{content:'';position:absolute;right:-60px;top:-60px;width:220px;height:220px;border-radius:50%;background:radial-gradient(circle,rgba(0,212,255,0.07) 0%,transparent 70%);}
.hero-greeting{font-size:12px;color:var(--text2);margin-bottom:4px;}
.hero-name{font-family:'Bebas Neue',sans-serif;font-size:26px;letter-spacing:1px;margin-bottom:6px;}
.hero-name span{color:var(--accent);}
.hero-sub{font-size:12.5px;color:var(--text2);max-width:380px;line-height:1.5;}
.hero-stats{display:flex;gap:26px;z-index:1;}
.hero-stat-val{font-family:'Bebas Neue',sans-serif;font-size:28px;letter-spacing:1px;color:var(--accent);}
.hero-stat-lbl{font-size:10px;color:var(--text2);text-transform:uppercase;letter-spacing:1px;}
.course-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:13px;margin-bottom:20px;}
.course-card{background:var(--card);border:1px solid var(--border);border-radius:12px;overflow:hidden;transition:transform 0.2s,box-shadow 0.2s;cursor:pointer;}
.course-card:hover{transform:translateY(-3px);box-shadow:0 10px 32px rgba(0,0,0,0.4);border-color:rgba(0,212,255,0.2);}
.course-thumb{height:105px;position:relative;display:flex;align-items:center;justify-content:center;font-size:34px;}
.t1{background:linear-gradient(135deg,#0d2a3a,#0a4060);}.t2{background:linear-gradient(135deg,#2a1a0d,#603010);}.t3{background:linear-gradient(135deg,#0d2a15,#105028);}.t4{background:linear-gradient(135deg,#2a2a0d,#504010);}.t5{background:linear-gradient(135deg,#1a0d2a,#300860);}.t6{background:linear-gradient(135deg,#2a0d0d,#600010);}
.course-level{position:absolute;top:7px;right:7px;font-size:9px;padding:2px 6px;border-radius:4px;font-family:'JetBrains Mono',monospace;}
.level-basic{background:rgba(57,224,117,0.2);color:var(--accent3);border:1px solid rgba(57,224,117,0.3);}
.level-inter{background:rgba(245,200,66,0.2);color:var(--accent4);border:1px solid rgba(245,200,66,0.3);}
.level-adv{background:rgba(255,107,43,0.2);color:var(--accent2);border:1px solid rgba(255,107,43,0.3);}
.course-body{padding:13px;}
.course-cat{font-size:9px;color:var(--accent);font-family:'JetBrains Mono',monospace;letter-spacing:1px;text-transform:uppercase;margin-bottom:4px;}
.course-name{font-size:13px;font-weight:600;margin-bottom:6px;line-height:1.4;}
.course-meta{display:flex;gap:10px;font-size:10px;color:var(--text2);margin-bottom:9px;}
.course-prog-bar{height:3px;background:rgba(255,255,255,0.06);border-radius:2px;overflow:hidden;margin-bottom:3px;}
.course-prog-fill{height:100%;background:var(--accent);border-radius:2px;}
.course-prog-text{font-size:9px;color:var(--text2);font-family:'JetBrains Mono',monospace;}
.data-table{width:100%;border-collapse:collapse;font-size:12px;}
.data-table th{text-align:left;padding:8px 12px;font-size:9px;letter-spacing:1.5px;text-transform:uppercase;color:var(--text2);border-bottom:1px solid var(--border);font-weight:500;font-family:'JetBrains Mono',monospace;}
.data-table td{padding:11px 12px;border-bottom:1px solid rgba(255,255,255,0.04);vertical-align:middle;}
.data-table tr:last-child td{border-bottom:none;}
.data-table tr:hover td{background:rgba(0,212,255,0.02);}
.spill{display:inline-flex;align-items:center;gap:4px;font-size:10px;padding:2px 8px;border-radius:20px;font-family:'JetBrains Mono',monospace;}
.spill::before{content:'';width:4px;height:4px;border-radius:50%;background:currentColor;}
.spill.done{background:rgba(57,224,117,0.1);color:var(--accent3);border:1px solid rgba(57,224,117,0.2);}
.spill.prog{background:rgba(0,212,255,0.1);color:var(--accent);border:1px solid rgba(0,212,255,0.2);}
.spill.pend{background:rgba(245,200,66,0.1);color:var(--accent4);border:1px solid rgba(245,200,66,0.2);}
.sbar-wrap{display:flex;align-items:center;gap:8px;}
.sbar{flex:1;height:3px;background:rgba(255,255,255,0.06);border-radius:2px;overflow:hidden;}
.sbar-fill{height:100%;border-radius:2px;background:var(--accent);}
.cert-card{background:linear-gradient(135deg,var(--bg2),#0d1e2e);border:1px solid rgba(0,212,255,0.2);border-radius:12px;padding:18px;display:flex;gap:14px;margin-bottom:9px;align-items:center;transition:transform 0.2s;cursor:pointer;}
.cert-card:hover{transform:translateX(4px);}
.cert-icon{width:48px;height:48px;background:linear-gradient(135deg,rgba(0,212,255,0.15),rgba(0,212,255,0.05));border:1px solid rgba(0,212,255,0.2);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:22px;flex-shrink:0;}
.cert-info{flex:1;}
.cert-name{font-size:13.5px;font-weight:600;margin-bottom:3px;}
.cert-date{font-size:10px;color:var(--text2);font-family:'JetBrains Mono',monospace;}
.cert-score{font-family:'Bebas Neue',sans-serif;font-size:24px;color:var(--accent3);}
.quiz-card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:20px;margin-bottom:13px;}
.q-num{font-family:'JetBrains Mono',monospace;font-size:10px;color:var(--accent);margin-bottom:6px;}
.q-text{font-size:14px;font-weight:500;margin-bottom:14px;line-height:1.5;}
.q-options{display:flex;flex-direction:column;gap:8px;}
.q-option{padding:10px 14px;border-radius:7px;border:1px solid var(--border);cursor:pointer;font-size:12.5px;transition:all 0.15s;background:rgba(255,255,255,0.02);}
.q-option:hover{border-color:var(--accent);background:rgba(0,212,255,0.04);}
.q-option.selected{border-color:var(--accent);background:rgba(0,212,255,0.08);color:var(--accent);}
.sched-item{display:flex;gap:12px;padding
