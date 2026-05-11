<!DOCTYPE html>

<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Connectivity360 — Tango</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700&family=DM+Mono:wght@400;500&display=swap');
:root {
  --teal:#0D7A6E; --teal-d:#09594F; --teal-l:#E6F4F2;
  --green:#2D9C5A; --green-l:#E8F6EE;
  --red:#D94040; --red-l:#FDEAEA;
  --amber:#E07B18; --amber-l:#FEF3E2;
  --purple:#5B3FC4; --blue:#2563EB; --blue-l:#EFF6FF;
  --g50:#F8F9FA; --g100:#F1F3F5; --g200:#E9ECEF;
  --g400:#9CA3AF; --g600:#4B5563; --g800:#1F2937;
  --bdr:#E5E7EB;
  --sh:0 1px 3px rgba(0,0,0,.06),0 1px 2px rgba(0,0,0,.04);
  --sh-md:0 4px 16px rgba(0,0,0,.09);
  --r:10px; --font:'DM Sans',sans-serif; --mono:'DM Mono',monospace;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:var(--font);font-size:13px;background:var(--g50);color:var(--g800);min-height:100vh;}

/* NAV */
.nav{background:#fff;border-bottom:1px solid var(–bdr);display:flex;align-items:center;padding:0 24px;height:60px;gap:20px;position:sticky;top:0;z-index:100;box-shadow:var(–sh);}
.logo{display:flex;align-items:center;gap:10px;flex-shrink:0;}
.logo-t{font-size:22px;font-weight:700;color:var(–teal);letter-spacing:-.5px;}
.logo-t span{color:#7DC242;}
.logo-div{width:1px;height:24px;background:var(–bdr);}
.logo-p{font-size:15px;font-weight:600;color:var(–g800);}
.search-area{flex:1;max-width:520px;display:flex;align-items:center;gap:8px;}
.search-lbl{font-size:11px;font-weight:500;color:var(–g600);white-space:nowrap;}
.search-box{position:relative;flex:1;}
.search-box svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);color:var(–g400);pointer-events:none;}
.s-input{width:100%;padding:8px 36px 8px 34px;border:1.5px solid var(–bdr);border-radius:8px;font-family:var(–font);font-size:13px;outline:none;transition:border-color .2s;}
.s-input:focus{border-color:var(–teal);}
.s-clear{position:absolute;right:8px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;color:var(–g400);font-size:14px;line-height:1;}
.s-btn{padding:8px 18px;background:var(–teal);color:#fff;border:none;border-radius:8px;font-family:var(–font);font-size:13px;font-weight:500;cursor:pointer;display:flex;align-items:center;gap:6px;flex-shrink:0;transition:background .15s;}
.s-btn:hover{background:var(–teal-d);}
.nav-live{display:flex;align-items:center;gap:6px;font-size:11px;color:var(–g600);}
.live-dot{width:7px;height:7px;border-radius:50%;background:var(–green);animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:.4;}}
.nav-r{margin-left:auto;display:flex;align-items:center;gap:16px;}
.nav-link{display:flex;align-items:center;gap:5px;font-size:13px;color:var(–g600);cursor:pointer;text-decoration:none;}
.nav-user{display:flex;align-items:center;gap:6px;font-size:13px;font-weight:500;cursor:pointer;position:relative;}
.role-badge{padding:2px 8px;border-radius:20px;font-size:10px;font-weight:700;}
.rb-admin{background:#FDEAEA;color:var(–red);}
.rb-l2{background:#FEF3E2;color:var(–amber);}
.rb-ro{background:var(–blue-l);color:var(–blue);}

/* ROLE SWITCHER */
.role-switcher{position:absolute;top:calc(100% + 8px);right:0;background:#fff;border:1px solid var(–bdr);border-radius:var(–r);box-shadow:var(–sh-md);padding:8px;min-width:200px;display:none;z-index:200;}
.role-switcher.open{display:block;}
.role-opt{display:flex;align-items:center;justify-content:space-between;padding:8px 10px;border-radius:7px;cursor:pointer;font-size:12px;}
.role-opt:hover{background:var(–g50);}
.role-opt.active{background:var(–teal-l);}

/* ALERT BANNER */
.alert-banner{display:flex;align-items:center;justify-content:space-between;padding:12px 24px;border-bottom:1px solid;gap:12px;flex-wrap:wrap;}
.alert-banner.warn{background:#FFFBEB;border-color:#FDE68A;}
.alert-banner.err{background:#FEF2F2;border-color:#FECACA;}
.alert-banner.ok{background:var(–green-l);border-color:#A7F3D0;}
.alert-left{display:flex;align-items:center;gap:10px;}
.alert-icon{width:32px;height:32px;border-radius:50%;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
.alert-icon.warn{background:#FEF3C7;}
.alert-icon.err{background:var(–red-l);}
.alert-title{font-size:13px;font-weight:700;}
.alert-title.warn{color:#92400E;}
.alert-title.err{color:var(–red);}
.alert-desc{font-size:12px;color:var(–g600);margin-top:1px;}
.alert-meta{font-size:11px;color:var(–g400);white-space:nowrap;}
.alert-cta{padding:7px 14px;border:1.5px solid var(–g200);border-radius:8px;background:#fff;font-size:12px;font-weight:500;cursor:pointer;display:flex;align-items:center;gap:5px;white-space:nowrap;}
.alert-cta:hover{border-color:var(–teal);color:var(–teal);}

/* MAIN */
.main{max-width:1380px;margin:0 auto;padding:16px 20px 40px;}

/* SUMMARY BAR */
.sum-bar{background:#fff;border:1px solid var(–bdr);border-radius:var(–r);display:flex;align-items:stretch;margin-bottom:14px;box-shadow:var(–sh);overflow:hidden;}
.sum-item{padding:12px 20px;border-right:1px solid var(–bdr);flex:1;min-width:0;}
.sum-item:last-child{border-right:none;flex:0 0 auto;}
.sum-lbl{font-size:10px;font-weight:500;color:var(–g400);text-transform:uppercase;letter-spacing:.05em;margin-bottom:4px;display:flex;align-items:center;gap:5px;}
.sum-val{font-size:14px;font-weight:600;font-family:var(–mono);}
.sum-val.teal{color:var(–teal);}
.sum-val.green{color:var(–green);}
.copy-btn{background:none;border:none;cursor:pointer;color:var(–g400);padding:0 0 0 5px;vertical-align:middle;}
.copy-btn:hover{color:var(–teal);}
.ont-badge{display:flex;align-items:center;gap:10px;padding:12px 20px;}
.ont-ico{width:38px;height:38px;border-radius:50%;background:var(–green);display:flex;align-items:center;justify-content:center;}
.ont-ico svg{width:18px;height:18px;stroke:#fff;fill:none;stroke-width:2;}
.ont-txt .ol{font-size:10px;font-weight:500;color:var(–g400);text-transform:uppercase;letter-spacing:.05em;}
.ont-txt .ov{font-size:13px;font-weight:700;color:var(–green);}

/* KPI ROW */
.kpi-row{display:grid;grid-template-columns:repeat(6,1fr);gap:12px;margin-bottom:14px;}
.kpi-card{background:#fff;border:1px solid var(–bdr);border-radius:var(–r);padding:14px 16px;box-shadow:var(–sh);display:flex;align-items:center;gap:12px;}
.kpi-icon{width:38px;height:38px;border-radius:10px;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
.kpi-icon.green{background:var(–green-l);}
.kpi-icon.red{background:var(–red-l);}
.kpi-icon.amber{background:var(–amber-l);}
.kpi-icon.blue{background:var(–blue-l);}
.kpi-icon.teal{background:var(–teal-l);}
.kpi-icon svg{width:18px;height:18px;fill:none;stroke-width:2;}
.kpi-icon.green svg{stroke:var(–green);}
.kpi-icon.red svg{stroke:var(–red);}
.kpi-icon.amber svg{stroke:var(–amber);}
.kpi-icon.blue svg{stroke:var(–blue);}
.kpi-icon.teal svg{stroke:var(–teal);}
.kpi-body{}
.kpi-lbl{font-size:10px;font-weight:500;color:var(–g400);text-transform:uppercase;letter-spacing:.05em;}
.kpi-val{font-size:18px;font-weight:700;font-family:var(–mono);line-height:1.2;}
.kpi-val.green{color:var(–green);}
.kpi-val.red{color:var(–red);}
.kpi-val.blue{color:var(–blue);}
.kpi-val.teal{color:var(–teal);}
.kpi-sub{font-size:10px;color:var(–g400);margin-top:1px;}
/* Health score */
.health-card{background:#fff;border:1px solid var(–bdr);border-radius:var(–r);padding:14px 16px;box-shadow:var(–sh);display:flex;align-items:center;gap:14px;}
.health-ring{position:relative;width:52px;height:52px;flex-shrink:0;}
.health-ring svg{transform:rotate(-90deg);}
.health-pct{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;color:var(–green);}
.health-body .hl{font-size:10px;font-weight:500;color:var(–g400);text-transform:uppercase;letter-spacing:.05em;}
.health-body .hv{font-size:14px;font-weight:700;color:var(–green);}
.health-body .hs{font-size:11px;color:var(–g400);}

/* CONTENT ROW */
.content-row{display:grid;grid-template-columns:1fr 320px;gap:14px;margin-bottom:14px;}
.content-left{}
.content-right{display:flex;flex-direction:column;gap:14px;}

/* SECTION CARD */
.sc{background:#fff;border:1px solid var(–bdr);border-radius:var(–r);box-shadow:var(–sh);overflow:hidden;}
.sc-hdr{padding:11px 16px;border-bottom:1px solid var(–bdr);display:flex;align-items:center;justify-content:space-between;}
.sc-title{font-size:11px;font-weight:700;color:var(–teal);text-transform:uppercase;letter-spacing:.08em;display:flex;align-items:center;gap:7px;}
.sc-title svg{width:13px;height:13px;stroke:var(–teal);fill:none;stroke-width:2;}
.sc-body{padding:20px 24px 16px;}

/* TOPOLOGY */
.topo-wrap{display:flex;align-items:flex-start;justify-content:center;}
.topo-node{display:flex;flex-direction:column;align-items:center;gap:5px;min-width:100px;}
.t-circle{width:62px;height:62px;border-radius:50%;border:2.5px solid;background:#fff;display:flex;align-items:center;justify-content:center;position:relative;transition:border-color .3s;}
.t-circle.ok{border-color:var(–green);}
.t-circle.ko{border-color:var(–red);}
.t-circle.warn{border-color:var(–amber);}
.t-circle svg{width:28px;height:28px;fill:none;stroke-width:1.6;}
.t-circle.ok svg{stroke:var(–teal);}
.t-circle.ko svg{stroke:var(–red);}
.t-circle.warn svg{stroke:var(–amber);}
.t-check{position:absolute;bottom:-2px;right:-2px;width:20px;height:20px;border-radius:50%;display:flex;align-items:center;justify-content:center;}
.t-check.ok{background:var(–green);}
.t-check.ko{background:var(–red);}
.t-check.warn{background:var(–amber);}
.t-check svg{width:10px;height:10px;stroke:#fff;fill:none;stroke-width:3;}
.t-conn{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:flex-start;padding-top:31px;min-width:36px;gap:3px;}
.t-line{height:2.5px;width:100%;border-radius:2px;transition:background .3s;}
.t-line.ok{background:var(–green);}
.t-line.ko{background:var(–red);}
.t-line.warn{background:var(–amber);}
.t-conn-lbl{font-size:9px;font-weight:600;text-transform:uppercase;letter-spacing:.06em;color:var(–g400);white-space:nowrap;}
.t-conn-state{font-size:9px;font-weight:600;white-space:nowrap;}
.t-conn-state.ok{color:var(–green);}
.t-conn-state.ko{color:var(–red);}
.t-conn-state.warn{color:var(–amber);}
.t-lbl{font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:.03em;color:var(–g800);}
.t-status{font-size:11px;font-weight:600;}
.t-status.ok{color:var(–green);}
.t-status.ko{color:var(–red);}
.t-status.warn{color:var(–amber);}
.t-btn{margin-top:7px;padding:6px 14px;border:none;border-radius:7px;font-family:var(–font);font-size:11px;font-weight:600;cursor:pointer;color:#fff;transition:opacity .15s,transform .1s;white-space:nowrap;display:flex;align-items:center;gap:5px;}
.t-btn:hover{opacity:.87;}
.t-btn:active{transform:scale(.97);}
.t-btn.blue{background:var(–purple);}
.t-btn.orange{background:var(–amber);}
.t-btn.red{background:var(–red);}
.t-btn:disabled{background:var(–g200);color:var(–g400);cursor:not-allowed;}

/* SIM BAR */
.sim-bar{display:flex;align-items:center;gap:16px;flex-wrap:wrap;padding:10px 16px;background:var(–g50);border-top:1px solid var(–bdr);font-size:11px;color:var(–g600);}
.tog{position:relative;width:34px;height:19px;flex-shrink:0;}
.tog input{opacity:0;width:0;height:0;}
.tog-sl{position:absolute;inset:0;border-radius:19px;cursor:pointer;transition:background .2s;background:var(–red);}
.tog-sl:before{content:’’;position:absolute;width:15px;height:15px;left:2px;top:2px;border-radius:50%;background:#fff;transition:transform .2s;}
.tog input:checked+.tog-sl{background:var(–green);}
.tog input:checked+.tog-sl:before{transform:translateX(15px);}
.tog-grp{display:flex;align-items:center;gap:6px;}

/* DATA GRID */
.data-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:14px;}
.dc{background:#fff;border:1px solid var(–bdr);border-radius:var(–r);box-shadow:var(–sh);overflow:hidden;}
.dc-hdr{padding:10px 14px;border-bottom:1px solid var(–bdr);display:flex;align-items:center;justify-content:space-between;}
.dc-title{font-size:11px;font-weight:700;color:var(–teal);text-transform:uppercase;letter-spacing:.07em;display:flex;align-items:center;gap:6px;}
.dc-title svg{width:13px;height:13px;stroke:var(–teal);fill:none;stroke-width:2;}
.dr{display:flex;justify-content:space-between;align-items:center;padding:6px 14px;border-bottom:1px solid var(–g100);gap:8px;}
.dr:last-child{border-bottom:none;}
.dk{font-size:12px;color:var(–g600);}
.dv{font-size:12px;font-weight:500;color:var(–g800);text-align:right;}
.dv.mono{font-family:var(–mono);font-size:11px;}
.dv.green{color:var(–green);font-weight:600;}
.dv.red{color:var(–red);font-weight:600;}
.dv.amber{color:var(–amber);font-weight:600;}
.dv.blue{color:var(–blue);font-weight:600;}
.dv.teal{color:var(–teal);font-weight:600;}
.pill{display:inline-flex;align-items:center;gap:3px;padding:2px 8px;border-radius:20px;font-size:10px;font-weight:600;}
.pill.green{background:var(–green-l);color:var(–green);}
.pill.red{background:var(–red-l);color:var(–red);}
.pill.amber{background:var(–amber-l);color:var(–amber);}
.pill.blue{background:var(–blue-l);color:var(–blue);}
.pill.gray{background:var(–g100);color:var(–g600);}
.pill.teal{background:var(–teal-l);color:var(–teal);}
.hors-seuil{padding:2px 6px;background:var(–red-l);color:var(–red);border:1px solid #FECACA;border-radius:4px;font-size:10px;font-weight:700;margin-left:5px;}

/* BOTTOM GRID */
.bottom-grid{display:grid;grid-template-columns:1fr 1.6fr 1fr;gap:14px;margin-bottom:14px;}

/* TIMELINE */
.tl-item{display:flex;align-items:flex-start;gap:10px;padding:8px 14px;border-bottom:1px solid var(–g100);}
.tl-item:last-child{border-bottom:none;}
.tl-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;margin-top:4px;}
.tl-dot.major{background:var(–red);}
.tl-dot.minor{background:var(–amber);}
.tl-dot.cleared{background:var(–green);}
.tl-body{flex:1;min-width:0;}
.tl-time{font-size:10px;color:var(–g400);font-family:var(–mono);}
.tl-desc{font-size:12px;color:var(–g800);}
.tl-sev{padding:1px 7px;border-radius:3px;font-size:10px;font-weight:700;}
.tl-sev.major{background:var(–red);color:#fff;}
.tl-sev.minor{background:var(–amber);color:#fff;}
.tl-sev.cleared{background:var(–green-l);color:var(–green);}

/* ALARMS */
.a-table{width:100%;border-collapse:collapse;}
.a-table th{text-align:left;padding:7px 12px;font-size:10px;font-weight:600;color:var(–g400);text-transform:uppercase;letter-spacing:.06em;border-bottom:1px solid var(–bdr);background:var(–g50);}
.a-table td{padding:7px 12px;border-bottom:1px solid var(–g100);font-size:12px;vertical-align:middle;}
.a-table tr:last-child td{border-bottom:none;}
.a-table tr:hover td{background:var(–g50);}
.sev-badge{display:inline-block;padding:2px 8px;border-radius:4px;font-size:10px;font-weight:700;}
.sev-badge.major{background:var(–red);color:#fff;}
.sev-badge.minor{background:var(–amber);color:#fff;}
.a-filters{display:flex;align-items:center;gap:6px;}
.f-btn{padding:4px 11px;border-radius:20px;border:1.5px solid var(–bdr);background:#fff;font-size:11px;font-weight:600;cursor:pointer;transition:all .15s;}
.f-btn.active-all{background:var(–teal);color:#fff;border-color:var(–teal);}
.f-btn.active-major{background:var(–red);color:#fff;border-color:var(–red);}
.f-btn.active-minor{background:var(–amber);color:#fff;border-color:var(–amber);}

/* DIAGNOSTIC */
.diag-box{border-radius:8px;padding:12px 14px;margin-bottom:10px;}
.diag-box.warn{background:var(–amber-l);border:1px solid #FDE68A;}
.diag-box.ok{background:var(–green-l);border:1px solid #A7F3D0;}
.diag-title{font-size:12px;font-weight:700;display:flex;align-items:center;gap:6px;margin-bottom:6px;}
.diag-title.warn{color:#92400E;}
.diag-title.ok{color:var(–teal-d);}
.diag-text{font-size:11px;color:var(–g600);line-height:1.6;}
.diag-reco{margin-top:8px;}
.diag-reco li{font-size:11px;color:var(–g600);margin-left:14px;margin-top:3px;}
.diag-test-btn{width:100%;margin-top:10px;padding:8px;background:var(–teal);color:#fff;border:none;border-radius:8px;font-family:var(–font);font-size:12px;font-weight:600;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:6px;}
.diag-test-btn:hover{background:var(–teal-d);}

/* REBOOTS */
.rb-item{padding:10px 14px;border-bottom:1px solid var(–g100);}
.rb-item:last-child{border-bottom:none;}
.rb-dot{width:8px;height:8px;border-radius:50%;display:inline-block;margin-right:6px;}
.rb-dot.green{background:var(–green);}
.rb-dot.amber{background:var(–amber);}
.rb-name{font-size:12px;font-weight:600;color:var(–g800);}
.rb-time{font-size:11px;color:var(–g400);font-family:var(–mono);margin-top:1px;}
.rb-reason{font-size:11px;color:var(–g600);margin-top:2px;}
.rb-more{display:block;width:100%;padding:8px;border:1px solid var(–bdr);border-radius:7px;background:#fff;font-family:var(–font);font-size:11px;color:var(–g600);cursor:pointer;margin:10px 14px;width:calc(100% - 28px);}

/* CHART */
.chart-wrap{padding:16px 16px 10px;}
.chart-legend{display:flex;gap:14px;margin-bottom:8px;}
.cl-item{display:flex;align-items:center;gap:5px;font-size:11px;color:var(–g600);}
.cl-dot{width:24px;height:3px;border-radius:2px;}
.chart-tabs{display:flex;gap:4px;}
.c-tab{padding:4px 11px;border-radius:6px;border:1.5px solid var(–bdr);background:#fff;font-size:11px;font-weight:600;cursor:pointer;color:var(–g600);}
.c-tab.active{background:var(–teal);color:#fff;border-color:var(–teal);}
.threshold-line{font-size:10px;color:var(–red);margin-top:4px;display:flex;align-items:center;gap:5px;}

/* MODAL */
.modal-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.4);z-index:300;align-items:center;justify-content:center;}
.modal-bg.open{display:flex;}
.modal-box{background:#fff;border-radius:14px;padding:24px;width:380px;box-shadow:0 24px 48px rgba(0,0,0,.18);}
.modal-title{font-size:15px;font-weight:600;margin-bottom:6px;display:flex;align-items:center;gap:8px;}
.modal-desc{font-size:13px;color:var(–g600);line-height:1.6;margin-bottom:14px;}
.modal-meta{background:var(–g50);border-radius:8px;padding:10px 12px;margin-bottom:14px;font-size:12px;font-family:var(–mono);}
.modal-meta div{display:flex;justify-content:space-between;padding:2px 0;}
.modal-meta .mk{color:var(–g400);}
.modal-meta .mv{color:var(–g800);font-weight:500;}
.modal-comment label{font-size:12px;font-weight:500;color:var(–g600);display:block;margin-bottom:5px;}
.modal-comment textarea{width:100%;padding:8px 10px;border:1.5px solid var(–bdr);border-radius:8px;font-family:var(–font);font-size:12px;resize:none;outline:none;height:64px;}
.modal-comment textarea:focus{border-color:var(–teal);}
.modal-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:14px;}
.btn-cancel{padding:8px 16px;background:var(–g100);border:none;border-radius:8px;font-size:13px;font-family:var(–font);cursor:pointer;color:var(–g800);}
.btn-confirm{padding:8px 18px;background:var(–red);color:#fff;border:none;border-radius:8px;font-size:13px;font-weight:600;font-family:var(–font);cursor:pointer;display:flex;align-items:center;gap:6px;}
.spin{width:13px;height:13px;border:2px solid rgba(255,255,255,.35);border-top-color:#fff;border-radius:50%;animation:sp .7s linear infinite;display:none;}
@keyframes sp{to{transform:rotate(360deg);}}

/* TOAST */
.toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:#1a1a1a;color:#fff;padding:10px 20px;border-radius:8px;font-size:13px;font-weight:500;opacity:0;transition:opacity .3s;z-index:400;pointer-events:none;white-space:nowrap;display:flex;align-items:center;gap:8px;}
.toast.show{opacity:1;}

/* FOOTER */
.footer{text-align:center;font-size:11px;color:var(–g400);padding:16px 0;border-top:1px solid var(–bdr);}

@media(max-width:1100px){.data-grid{grid-template-columns:1fr 1fr;}.bottom-grid{grid-template-columns:1fr;}.kpi-row{grid-template-columns:repeat(3,1fr);}.content-row{grid-template-columns:1fr;}}
</style>

</head>
<body>

<!-- NAV -->

<nav class="nav">
  <div class="logo">
    <span class="logo-t">tan<span>go</span></span>
    <div class="logo-div"></div>
    <span class="logo-p">Connectivity360</span>
  </div>
  <div class="search-area">
    <span class="search-lbl">Rechercher un SLID</span>
    <div class="search-box">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg>
      <input class="s-input" id="slid-inp" value="221100530437" placeholder="Ex. 221100530437">
      <button class="s-clear" onclick="document.getElementById('slid-inp').value=''">✕</button>
    </div>
    <button class="s-btn" onclick="showToast('✓ Données rafraîchies')">
      <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg>
      Rechercher
    </button>
  </div>
  <div class="nav-live">
    <span class="live-dot"></span>
    <span>Live<br><span style="font-size:10px;">Auto-refresh: 30s</span></span>
  </div>
  <div class="nav-r">
    <a class="nav-link" href="#">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 16v-4M12 8h.01"/></svg>
      Aide
    </a>
    <div class="nav-user" onclick="document.getElementById('role-sw').classList.toggle('open')">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
      <span id="user-name">Admin</span>
      <span class="role-badge rb-admin" id="role-label">ADMIN</span>
      <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg>
      <div class="role-switcher" id="role-sw">
        <div style="font-size:10px;font-weight:600;color:var(--g400);padding:4px 10px 6px;text-transform:uppercase;letter-spacing:.06em;">Simuler un rôle</div>
        <div class="role-opt active" onclick="setRole('ADMIN','Admin','rb-admin');event.stopPropagation()">
          <span>Admin</span><span class="role-badge rb-admin">ADMIN</span>
        </div>
        <div class="role-opt" onclick="setRole('SUPPORT_L2','J. Dupont','rb-l2');event.stopPropagation()">
          <span>J. Dupont</span><span class="role-badge rb-l2">SUPPORT_L2</span>
        </div>
        <div class="role-opt" onclick="setRole('READ_ONLY','M. Martin','rb-ro');event.stopPropagation()">
          <span>M. Martin</span><span class="role-badge rb-ro">READ_ONLY</span>
        </div>
      </div>
    </div>
  </div>
</nav>

<!-- ALERT BANNER -->

<div class="alert-banner warn" id="alert-banner">
  <div class="alert-left">
    <div class="alert-icon warn">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#D97706" stroke-width="2"><path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg>
    </div>
    <div>
      <div class="alert-title warn">⚡ FIBRE OPTIQUE DÉGRADÉE DÉTECTÉE</div>
      <div class="alert-desc">Rx Signal (<strong>-29.4 dBm</strong>) inférieur au seuil recommandé (-28.0 dBm). Vérifier la liaison fibre entre le PTO et l'ONT.</div>
    </div>
  </div>
  <div style="display:flex;align-items:center;gap:12px;flex-shrink:0;">
    <div class="alert-meta">Détecté le : 20/04/2024 11:37:22</div>
    <button class="alert-cta" onclick="switchToAlarms()">Voir le diagnostic <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="9 18 15 12 9 6"/></svg></button>
  </div>
</div>

<div class="main">

  <!-- SUMMARY BAR -->

  <div class="sum-bar">
    <div class="sum-item">
      <div class="sum-lbl">SLID</div>
      <div class="sum-val teal">221100530437 <button class="copy-btn" onclick="copyVal('221100530437','SLID')" title="Copier"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg></button></div>
    </div>
    <div class="sum-item">
      <div class="sum-lbl">ONT Serial</div>
      <div class="sum-val">ALCLFDF19610 <button class="copy-btn" onclick="copyVal('ALCLFDF19610','Serial')" title="Copier"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg></button></div>
    </div>
    <div class="sum-item">
      <div class="sum-lbl">Equipment ID</div>
      <div class="sum-val">11980209 <button class="copy-btn" onclick="copyVal('11980209','Equipment ID')" title="Copier"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg></button></div>
    </div>
    <div class="sum-item">
      <div class="sum-lbl">Status</div>
      <div class="sum-val green" style="display:flex;align-items:center;gap:6px;"><span style="width:8px;height:8px;border-radius:50%;background:var(--green);display:inline-block;"></span>Activated / ALIGNED</div>
    </div>
    <div class="sum-item">
      <div class="sum-lbl">Operational State</div>
      <div class="sum-val green">UP</div>
    </div>
    <div class="ont-badge">
      <div class="ont-ico"><svg viewBox="0 0 24 24" stroke-width="2"><path d="M5 12.55a11 11 0 0 1 14.08 0M1.42 9a16 16 0 0 1 21.16 0M8.53 16.11a6 6 0 0 1 6.95 0M12 20h.01"/></svg></div>
      <div class="ont-txt"><div class="ol">ONT</div><div class="ov">ONLINE</div></div>
    </div>
  </div>

  <!-- KPI ROW -->

  <div class="kpi-row">
    <div class="kpi-card">
      <div class="kpi-icon red"><svg viewBox="0 0 24 24"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg></div>
      <div class="kpi-body"><div class="kpi-lbl">RX SIGNAL</div><div class="kpi-val red">-29.4 <span style="font-size:12px;">dBm</span></div><div class="kpi-sub">⚠ Dégradé</div></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-icon green"><svg viewBox="0 0 24 24"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg></div>
      <div class="kpi-body"><div class="kpi-lbl">TX SIGNAL</div><div class="kpi-val green">5.756 <span style="font-size:12px;">dBm</span></div><div class="kpi-sub">Normal</div></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-icon blue"><svg viewBox="0 0 24 24"><rect x="2" y="2" width="20" height="8" rx="2"/><rect x="2" y="14" width="20" height="8" rx="2"/><line x1="6" y1="6" x2="6.01" y2="6"/><line x1="6" y1="18" x2="6.01" y2="18"/></svg></div>
      <div class="kpi-body"><div class="kpi-lbl">ETHERNET</div><div class="kpi-val blue">1G FD</div><div class="kpi-sub">Up</div></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-icon teal"><svg viewBox="0 0 24 24"><path d="M5 12.55a11 11 0 0 1 14.08 0M1.42 9a16 16 0 0 1 21.16 0M8.53 16.11a6 6 0 0 1 6.95 0M12 20h.01"/></svg></div>
      <div class="kpi-body"><div class="kpi-lbl">PPP (INTERNET)</div><div class="kpi-val teal">Up</div><div class="kpi-sub">Connecté</div></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-icon green"><svg viewBox="0 0 24 24"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg></div>
      <div class="kpi-body"><div class="kpi-lbl">ALIGNMENT</div><div class="kpi-val teal">ALIGNED</div><div class="kpi-sub">Bon</div></div>
    </div>
    <!-- Health Score -->
    <div class="health-card">
      <div class="health-ring">
        <svg width="52" height="52" viewBox="0 0 52 52">
          <circle cx="26" cy="26" r="22" fill="none" stroke="#E9ECEF" stroke-width="5"/>
          <circle cx="26" cy="26" r="22" fill="none" stroke="#2D9C5A" stroke-width="5" stroke-dasharray="138.2 138.2" stroke-dashoffset="24.9" stroke-linecap="round"/>
        </svg>
        <div class="health-pct">92%</div>
      </div>
      <div class="health-body"><div class="hl">HEALTH SCORE</div><div class="hv">92%</div><div class="hs">Très bon</div></div>
    </div>
  </div>

  <!-- TOPOLOGY + REBOOTS -->

  <div class="content-row">
    <div class="sc">
      <div class="sc-hdr">
        <div class="sc-title"><svg viewBox="0 0 24 24"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></svg>TOPOLOGIE</div>
        <span style="font-size:10px;color:var(--g400);" id="topo-update">Dernière mise à jour : à l'instant</span>
      </div>
      <div class="sc-body" style="padding:20px 16px 12px;">
        <div class="topo-wrap" id="topo"></div>
      </div>
      <div class="sim-bar" id="sim-bar">
        <strong style="color:var(--g800);">Simuler :</strong>
        <div class="tog-grp"><label class="tog"><input type="checkbox" id="tog-ont" checked onchange="renderTopo()"><span class="tog-sl"></span></label>Signal ONT</div>
        <div class="tog-grp"><label class="tog"><input type="checkbox" id="tog-rj45" checked onchange="renderTopo()"><span class="tog-sl"></span></label>Câble RJ45</div>
        <div class="tog-grp"><label class="tog"><input type="checkbox" id="tog-ppp" checked onchange="renderTopo()"><span class="tog-sl"></span></label>PPP Internet</div>
        <span style="margin-left:auto;font-size:10px;background:var(--red-l);color:var(--red);padding:2px 8px;border-radius:20px;font-weight:600;">🔴 ADMIN only</span>
      </div>
    </div>

```
<!-- REBOOTS -->
<div class="sc">
  <div class="sc-hdr"><div class="sc-title"><svg viewBox="0 0 24 24"><polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 1 1-2.12-9.36L23 10"/></svg>DERNIERS REBOOTS</div></div>
  <div class="rb-item">
    <div><span class="rb-dot green"></span><span class="rb-name">ONT Last Reboot</span></div>
    <div class="rb-time">18/04/2024 02:11:47</div>
    <div class="rb-reason">Raison : Remote reboot</div>
  </div>
  <div class="rb-item">
    <div><span class="rb-dot amber"></span><span class="rb-name">Fritzbox Last Reboot</span></div>
    <div class="rb-time">17/04/2024 16:27:01</div>
    <div class="rb-reason">Raison : Remote reboot</div>
  </div>
  <button class="rb-more">Voir l'historique complet →</button>
</div>
```

  </div>

  <!-- DATA GRID -->

  <div class="data-grid">
    <div class="dc">
      <div class="dc-hdr"><div class="dc-title"><svg viewBox="0 0 24 24"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>État & Mesures Optiques</div></div>
      <div class="dr"><span class="dk">ONT Up</span><span class="dv green">true</span></div>
      <div class="dr"><span class="dk">Rx Signal (dBm)</span><span class="dv red mono">-29.4<span class="hors-seuil">HORS SEUIL</span></span></div>
      <div class="dr"><span class="dk">Tx Signal (dBm)</span><span class="dv green mono">5.756</span></div>
      <div class="dr"><span class="dk">Tension module (V)</span><span class="dv mono">3.26</span></div>
      <div class="dr"><span class="dk">Laser Bias (µA)</span><span class="dv mono">16716</span></div>
      <div class="dr"><span class="dk">Température (°C)</span><span class="dv mono">43.5</span></div>
      <div class="dr"><span class="dk">Seuils (Low/High dBm)</span><span class="dv mono" style="font-size:10px;">-28.0 / -8.0</span></div>
    </div>
    <div class="dc">
      <div class="dc-hdr"><div class="dc-title"><svg viewBox="0 0 24 24"><rect x="2" y="2" width="20" height="8" rx="2"/><rect x="2" y="14" width="20" height="8" rx="2"/><line x1="6" y1="6" x2="6.01" y2="6"/><line x1="6" y1="18" x2="6.01" y2="18"/></svg>Port Ethernet</div></div>
      <div class="dr"><span class="dk">Ethernet Port Up</span><span class="dv green">true</span></div>
      <div class="dr"><span class="dk">Speed</span><span class="dv blue">1G FD</span></div>
      <div class="dr"><span class="dk">Port Type</span><span class="dv">FIBER</span></div>
      <div class="dr"><span class="dk">Duplex</span><span class="dv">Full</span></div>
      <div class="dr"><span class="dk">Link State</span><span class="dv green">Up</span></div>
      <div class="dr"><span class="dk">CPE Mac Address</span><span class="dv mono" style="font-size:10px;" id="mac-addr">44:4E:6D:DE:47:55</span></div>
      <div class="dr"><span class="dk">VLAN</span><span class="dv" style="color:var(--g400);">—</span></div>
    </div>
    <div class="dc">
      <div class="dc-hdr"><div class="dc-title"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><path d="M12 16v-4M12 8h.01"/></svg>Informations ONT</div></div>
      <div class="dr"><span class="dk">ONT Type</span><span class="dv">I-010G-U</span></div>
      <div class="dr"><span class="dk">ONT Serial</span><span class="dv mono">ALCLF81262AF</span></div>
      <div class="dr"><span class="dk">Vendor</span><span class="dv">XGS-PON</span></div>
      <div class="dr"><span class="dk">Model</span><span class="dv">XS-010X-Q</span></div>
      <div class="dr"><span class="dk">Software Version</span><span class="dv mono">V1.0.8B13</span></div>
      <div class="dr"><span class="dk">Alignment State</span><span class="dv green">ALIGNED</span></div>
      <div class="dr"><span class="dk">Status</span><span class="dv"><span class="pill green">Activated</span></span></div>
    </div>
    <div class="dc">
      <div class="dc-hdr"><div class="dc-title"><svg viewBox="0 0 24 24"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>Général & Service</div></div>
      <div class="dr"><span class="dk">Operational State</span><span class="dv green">UP</span></div>
      <div class="dr"><span class="dk">Service Status</span><span class="dv green">Activated</span></div>
      <div class="dr"><span class="dk">Temperature (°C)</span><span class="dv mono">43.5</span></div>
      <div class="dr"><span class="dk">Voltage (V)</span><span class="dv mono">3.26</span></div>
      <div class="dr"><span class="dk">ONT Uptime</span><span class="dv mono">12j 04h 32m</span></div>
      <div class="dr"><span class="dk">Last Change</span><span class="dv mono" style="font-size:10px;">20/04/2024 12:32:10</span></div>
      <div class="dr"><span class="dk">Activation Date</span><span class="dv mono" style="font-size:10px;">08/04/2024 14:15:22</span></div>
    </div>
  </div>

  <!-- BOTTOM GRID: Timeline | Alarmes | Diagnostic -->

  <div class="bottom-grid">

```
<!-- Timeline -->
<div class="sc">
  <div class="sc-hdr">
    <div class="sc-title"><svg viewBox="0 0 24 24" style="width:13px;height:13px;stroke:var(--teal);fill:none;stroke-width:2;"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>TIMELINE DES ÉVÉNEMENTS</div>
    <button style="font-size:11px;color:var(--teal);background:none;border:none;cursor:pointer;">Voir tout</button>
  </div>
  <div class="tl-item"><div class="tl-dot major"></div><div class="tl-body"><div class="tl-time">20/04/2024 11:37:22</div><div style="display:flex;align-items:center;justify-content:space-between;"><span class="tl-desc">LOS detected (Fibre optique)</span><span class="tl-sev major">Major</span></div></div></div>
  <div class="tl-item"><div class="tl-dot cleared"></div><div class="tl-body"><div class="tl-time">20/04/2024 11:41:58</div><div style="display:flex;align-items:center;justify-content:space-between;"><span class="tl-desc">Signal restored</span><span class="tl-sev cleared">Cleared</span></div></div></div>
  <div class="tl-item"><div class="tl-dot minor"></div><div class="tl-body"><div class="tl-time">20/04/2024 12:05:33</div><div style="display:flex;align-items:center;justify-content:space-between;"><span class="tl-desc">Ethernet link down</span><span class="tl-sev minor">Minor</span></div></div></div>
  <div class="tl-item"><div class="tl-dot cleared"></div><div class="tl-body"><div class="tl-time">20/04/2024 12:06:02</div><div style="display:flex;align-items:center;justify-content:space-between;"><span class="tl-desc">Ethernet link up</span><span class="tl-sev cleared">Cleared</span></div></div></div>
  <div class="tl-item"><div class="tl-dot minor"></div><div class="tl-body"><div class="tl-time">20/04/2024 12:25:10</div><div style="display:flex;align-items:center;justify-content:space-between;"><span class="tl-desc">PPP session flap</span><span class="tl-sev minor">Minor</span></div></div></div>
</div>

<!-- Alarmes -->
<div class="sc">
  <div class="sc-hdr">
    <div class="sc-title"><svg viewBox="0 0 24 24" style="width:13px;height:13px;stroke:var(--teal);fill:none;stroke-width:2;"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9M13.73 21a2 2 0 0 1-3.46 0"/></svg>HISTORIQUE DES ALARMES</div>
    <div class="a-filters">
      <button class="f-btn active-all" onclick="filterA('all',this)">Toutes</button>
      <button class="f-btn" onclick="filterA('major',this)">Major</button>
      <button class="f-btn" onclick="filterA('minor',this)">Minor</button>
      <button style="padding:5px 8px;border:1px solid var(--bdr);border-radius:6px;background:#fff;cursor:pointer;font-size:11px;color:var(--g600);" id="export-btn" title="Exporter CSV">
        <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
      </button>
    </div>
  </div>
  <div style="overflow-x:auto;">
    <table class="a-table"><thead><tr><th>Sév.</th><th>Event Time</th><th>Cleared</th><th>Source</th><th>Mném.</th><th>Cause</th></tr></thead>
    <tbody id="alarm-body"></tbody></table>
  </div>
</div>

<!-- Diagnostic -->
<div class="sc">
  <div class="sc-hdr"><div class="sc-title"><svg viewBox="0 0 24 24" style="width:13px;height:13px;stroke:var(--teal);fill:none;stroke-width:2;"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>DIAGNOSTIC AUTOMATIQUE</div></div>
  <div style="padding:12px 14px;">
    <div class="diag-box warn">
      <div class="diag-title warn">⚠ Problème potentiel détecté</div>
      <div class="diag-text">Perte optique élevée détectée. Possible problème sur la fibre entre le PTO et l'ONT.</div>
      <div class="diag-reco"><strong style="font-size:11px;color:var(--g800);">Recommandations :</strong>
        <ul>
          <li>Vérifier la propreté des connecteurs</li>
          <li>Vérifier la continuité de la fibre</li>
          <li>Si le problème persiste, escalader au N2</li>
        </ul>
      </div>
    </div>
    <button class="diag-test-btn" id="test-btn" onclick="launchTest()">
      <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg>
      Lancer un test de ligne
    </button>
  </div>
</div>
```

  </div>

  <!-- CHART -->

  <div class="sc" style="margin-bottom:14px;">
    <div class="sc-hdr">
      <div class="sc-title"><svg viewBox="0 0 24 24" style="width:13px;height:13px;stroke:var(--teal);fill:none;stroke-width:2;"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>HISTORIQUE DES SIGNAUX (dBm)</div>
      <div style="display:flex;align-items:center;gap:14px;">
        <div class="chart-legend">
          <div class="cl-item"><div class="cl-dot" style="background:var(--green);"></div>Rx Signal (dBm)</div>
          <div class="cl-item"><div class="cl-dot" style="background:var(--red);"></div>Tx Signal (dBm)</div>
        </div>
        <div class="chart-tabs">
          <button class="c-tab active" onclick="setRange('24H',this)">24H</button>
          <button class="c-tab" onclick="setRange('7D',this)">7D</button>
          <button class="c-tab" onclick="setRange('30D',this)">30D</button>
        </div>
      </div>
    </div>
    <div class="chart-wrap">
      <canvas id="sigChart" height="100"></canvas>
      <div class="threshold-line">
        <svg width="20" height="3"><line x1="0" y1="1.5" x2="20" y2="1.5" stroke="var(--red)" stroke-width="1.5" stroke-dasharray="4,2"/></svg>
        Seuil Low : -28.0 dBm
      </div>
    </div>
  </div>

  <div class="footer">Connectivity360 © 2024 — Tango. Tous droits réservés.</div>
</div>

<!-- MODAL -->

<div class="modal-bg" id="modal">
  <div class="modal-box">
    <p class="modal-title" id="m-title"></p>
    <p class="modal-desc" id="m-desc"></p>
    <div class="modal-meta">
      <div><span class="mk">SLID</span><span class="mv">221100530437</span></div>
      <div><span class="mk">ONT Serial</span><span class="mv">ALCLFDF19610</span></div>
    </div>
    <div class="modal-comment" id="m-comment">
      <label>Motif (obligatoire)</label>
      <textarea id="m-reason" placeholder="Ex: Client sans connexion depuis 30 min..."></textarea>
    </div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal()">Annuler</button>
      <button class="btn-confirm" id="m-confirm" onclick="confirmAction()">
        <span class="spin" id="spinner"></span>
        <span id="m-label">Confirmer</span>
      </button>
    </div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.0/chart.umd.min.js"></script>

<script>
// ─── ROLE ───────────────────────────────
let currentRole = 'ADMIN';
function setRole(role, name, badgeCls) {
  currentRole = role;
  document.getElementById('user-name').textContent = name;
  const lb = document.getElementById('role-label');
  lb.textContent = role;
  lb.className = 'role-badge ' + badgeCls;
  document.getElementById('role-sw').classList.remove('open');
  document.querySelectorAll('.role-opt').forEach(o => o.classList.remove('active'));
  event.target.closest('.role-opt').classList.add('active');
  applyRole();
}
function applyRole() {
  const canAct    = ['SUPPORT_L2','ADMIN'].includes(currentRole);
  const canSim    = currentRole === 'ADMIN';
  const canExport = ['SUPPORT_L2','ADMIN'].includes(currentRole);
  const canCopy   = ['SUPPORT_L2','ADMIN'].includes(currentRole);
  // Buttons
  document.querySelectorAll('.t-btn').forEach(b => {
    b.disabled = !canAct;
    b.title = canAct ? '' : 'Accès non autorisé pour votre rôle';
  });
  // Simulateur
  document.getElementById('sim-bar').style.display = canSim ? 'flex' : 'none';
  // Export
  const expBtn = document.getElementById('export-btn');
  if (expBtn) { expBtn.disabled = !canExport; expBtn.style.opacity = canExport ? '1' : '.4'; }
  // Copy buttons
  document.querySelectorAll('.copy-btn').forEach(b => { b.style.display = canCopy ? 'inline' : 'none'; });
  // MAC masking
  const mac = document.getElementById('mac-addr');
  if (mac) mac.textContent = currentRole === 'READ_ONLY' ? '44:4E:6D:XX:XX:XX' : '44:4E:6D:DE:47:55';
  // Test button
  const tb = document.getElementById('test-btn');
  if (tb) { tb.disabled = !canAct; tb.style.opacity = canAct ? '1' : '.5'; }
  showToast(`Rôle : ${currentRole}`);
}

// ─── TOPOLOGY ────────────────────────────
const C = {
  ok:   {cls:'ok',  st:'OK'},
  ko:   {cls:'ko',  st:'KO'},
  warn: {cls:'warn',st:'Dégradée'},
};
function chk(ko) {
  if (ko) return `<svg viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="3"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>`;
  return `<svg viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="3"><polyline points="20 6 9 17 4 12"/></svg>`;
}
function ico(path) { return `<svg viewBox="0 0 24 24" fill="none" stroke-width="1.6">${path}</svg>`; }
const ICONS = {
  pto:      ico(`<rect x="3" y="3" width="18" height="18" rx="3"/><circle cx="12" cy="12" r="4"/><line x1="12" y1="3" x2="12" y2="8"/><line x1="12" y1="16" x2="12" y2="21"/>`),
  ont:      ico(`<rect x="2" y="6" width="20" height="12" rx="2"/><line x1="6" y1="10" x2="6" y2="14"/><line x1="10" y1="9" x2="10" y2="15"/><circle cx="17" cy="12" r="2"/>`),
  fritz:    ico(`<path d="M5 12.55a11 11 0 0 1 14.08 0"/><path d="M1.42 9a16 16 0 0 1 21.16 0"/><path d="M8.53 16.11a6 6 0 0 1 6.95 0"/><line x1="12" y1="20" x2="12" y2="20" stroke-width="3" stroke-linecap="round"/>`),
  internet: ico(`<circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>`)
};

function node(icon, lbl, s, btnHtml) {
  return `<div class="topo-node">
    <div class="t-circle ${s.cls}">${icon}<div class="t-check ${s.cls}">${chk(s.cls==='ko')}</div></div>
    <div class="t-lbl">${lbl}</div>
    <div class="t-status ${s.cls}">${s.st}</div>
    ${btnHtml||''}
  </div>`;
}
function conn(cls, lbl, stateLbl) {
  return `<div class="t-conn"><div class="t-line ${cls}"></div><div class="t-conn-lbl">${lbl}</div><div class="t-conn-state ${cls}">${stateLbl}</div></div>`;
}

function renderTopo() {
  const ont  = document.getElementById('tog-ont').checked;
  const rj45 = document.getElementById('tog-rj45').checked;
  const ppp  = document.getElementById('tog-ppp').checked;
  const sOnt   = ont  ? C.ok : C.ko;
  const sFritz = rj45 ? C.ok : C.ko;
  const sPPP   = ppp  ? C.ok : C.ko;
  const fo = ont ? 'ok' : 'ko';
  const canAct = ['SUPPORT_L2','ADMIN'].includes(currentRole);
  const btnOnt   = `<button class="t-btn blue"   onclick="openModal('ont')"   ${canAct?'':'disabled'} title="${canAct?'':'Accès non autorisé'}">↺ Reset ONT</button>`;
  const btnFritz = `<button class="t-btn orange" onclick="openModal('fritz')" ${canAct?'':'disabled'} title="${canAct?'':'Accès non autorisé'}">↺ Reboot Fritzbox</button>`;
  document.getElementById('topo').innerHTML =
    node(ICONS.pto,      'PTO',      C.ok,   '') +
    conn(fo,             'FIBRE OPTIQUE', ont?'Up':'Dégradée') +
    node(ICONS.ont,      'ONT',      sOnt,   btnOnt) +
    conn(rj45?'ok':'ko', 'ETHERNET', rj45?'Up':'Down') +
    node(ICONS.fritz,    'FRITZBOX', sFritz, btnFritz) +
    conn(ppp?'ok':'ko',  'PPP / INTERNET', ppp?'Up':'Down') +
    node(ICONS.internet, 'INTERNET', sPPP,   '');
}
renderTopo();

// ─── ALARMS ──────────────────────────────
const alarms = [
  { sev:'major', ev:'20/04/2024 11:37:22', cl:'20/04/2024 11:41:58', src:'ONT:T2809…ONT44', mem:'LOS', cause:'Loss Of Signal' },
  { sev:'minor', ev:'19/04/2024 08:15:47', cl:'19/04/2024 09:02:13', src:'ETH Port', mem:'LOS', cause:'Loss Of Signal' },
  { sev:'minor', ev:'18/04/2024 18:06:10', cl:'18/04/2024 18:07:45', src:'ONT:T2809…ONT44', mem:'EOP', cause:'Equipment Malfunction' },
  { sev:'minor', ev:'17/04/2024 16:27:00', cl:'17/04/2024 16:31:32', src:'ONT:T2809…ONT44', mem:'LOS', cause:'Loss Of Signal' },
  { sev:'minor', ev:'16/04/2024 15:12:07', cl:'16/04/2024 16:02:44', src:'ONT:T2809…ONT44', mem:'LOF', cause:'Loss Of Frame' },
];
function renderAlarms(f) {
  const d = f==='all' ? alarms : alarms.filter(a=>a.sev===f);
  document.getElementById('alarm-body').innerHTML = d.length
    ? d.map(a=>`<tr>
        <td><span class="sev-badge ${a.sev}">${a.sev.charAt(0).toUpperCase()+a.sev.slice(1)}</span></td>
        <td style="font-size:11px;white-space:nowrap;font-family:var(--mono);">${a.ev}</td>
        <td style="font-size:11px;white-space:nowrap;color:var(--g400);font-family:var(--mono);">${a.cl}</td>
        <td style="font-size:11px;">${a.src}</td>
        <td><span class="pill ${a.sev==='major'?'red':'amber'}" style="font-size:10px;">${a.mem}</span></td>
        <td style="font-size:11px;">${a.cause}</td>
      </tr>`).join('')
    : `<tr><td colspan="6" style="text-align:center;padding:20px;color:var(--g400);">Aucune alarme</td></tr>`;
}
renderAlarms('all');
function filterA(f, btn) {
  document.querySelectorAll('.f-btn').forEach(b=>b.className='f-btn');
  btn.className = f==='all'?'f-btn active-all':f==='major'?'f-btn active-major':'f-btn active-minor';
  renderAlarms(f);
}
function switchToAlarms() { window.scrollTo({top:document.getElementById('alarm-body').closest('.sc').offsetTop-80,behavior:'smooth'}); }

// ─── CHART ───────────────────────────────
let chart;
function buildChart(range) {
  const n = range==='24H'?24:range==='7D'?7:30;
  const labels=[], rx=[], tx=[];
  const base = new Date('2024-04-19T08:00:00');
  const step = range==='24H'?3600000:86400000;
  for(let i=0;i<n;i++){
    const d=new Date(base.getTime()+i*step);
    labels.push(range==='24H'?d.toLocaleTimeString('fr-FR',{hour:'2-digit',minute:'2-digit'}):d.toLocaleDateString('fr-FR',{day:'2-digit',month:'2-digit'}));
    const v = -17 - Math.random()*15;
    rx.push(+v.toFixed(2));
    tx.push(+(5.5+Math.random()*.8).toFixed(2));
  }
  const ctx = document.getElementById('sigChart').getContext('2d');
  if(chart) chart.destroy();
  chart = new Chart(ctx, {
    type:'line',
    data:{labels, datasets:[
      {label:'Rx',data:rx,borderColor:'#2D9C5A',backgroundColor:'rgba(45,156,90,.08)',borderWidth:1.8,pointRadius:2,pointHoverRadius:4,tension:.4,fill:true},
      {label:'Tx',data:tx,borderColor:'#D94040',backgroundColor:'rgba(217,64,64,.05)',borderWidth:1.8,pointRadius:2,pointHoverRadius:4,tension:.4,fill:false},
    ]},
    options:{
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{backgroundColor:'#1a1a1a',titleFont:{family:"'DM Sans'",size:11},bodyFont:{family:"'DM Mono'",size:11}},
        annotation:{annotations:{threshold:{type:'line',yMin:-28,yMax:-28,borderColor:'rgba(217,64,64,.5)',borderWidth:1.5,borderDash:[4,3]}}}
      },
      scales:{
        x:{grid:{color:'rgba(0,0,0,.04)'},ticks:{font:{family:"'DM Sans'",size:10},color:'#9CA3AF'}},
        y:{grid:{color:'rgba(0,0,0,.04)'},ticks:{font:{family:"'DM Mono'",size:10},color:'#9CA3AF'}}
      }
    }
  });
}
buildChart('24H');
function setRange(r,btn){
  document.querySelectorAll('.c-tab').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active'); buildChart(r);
}

// ─── MODAL ───────────────────────────────
let currentAction=null;
function openModal(type){
  currentAction=type;
  const cfg={
    ont:{title:'↺  Réinitialiser l\'ONT',desc:'Cette action redémarre l\'ONT. La connexion sera interrompue ~2 minutes.',label:'Réinitialiser'},
    fritz:{title:'↺  Reboot Fritzbox',desc:'Cette action redémarre la Fritzbox du client. Service temporairement indisponible.',label:'Rebooter'}
  };
  const c=cfg[type];
  document.getElementById('m-title').textContent=c.title;
  document.getElementById('m-desc').textContent=c.desc;
  document.getElementById('m-label').textContent=c.label;
  document.getElementById('m-reason').value='';
  document.getElementById('modal').classList.add('open');
}
function closeModal(){ document.getElementById('modal').classList.remove('open'); }
function confirmAction(){
  const reason=document.getElementById('m-reason').value.trim();
  if(!reason){ document.getElementById('m-reason').style.borderColor='var(--red)'; document.getElementById('m-reason').focus(); return; }
  document.getElementById('m-reason').style.borderColor='';
  const sp=document.getElementById('spinner'), lb=document.getElementById('m-label'), btn=document.getElementById('m-confirm');
  sp.style.display='block'; lb.textContent='En cours…'; btn.disabled=true;
  setTimeout(()=>{ sp.style.display='none'; btn.disabled=false; lb.textContent='Confirmer'; closeModal();
    showToast(currentAction==='ont'?'✓ ONT réinitialisé avec succès':'✓ Fritzbox redémarrée avec succès'); },2200);
}

// ─── TEST DE LIGNE ────────────────────────
function launchTest(){
  const btn=document.getElementById('test-btn');
  btn.innerHTML='<svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="animation:sp .7s linear infinite"><polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 1 1-2.12-9.36L23 10"/></svg> Test en cours…';
  btn.disabled=true;
  setTimeout(()=>{ btn.innerHTML='<svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg> Lancer un test de ligne'; btn.disabled=false; showToast('✓ Test de ligne terminé — résultat : Dégradé'); },3000);
}

// ─── COPY ────────────────────────────────
function copyVal(val, label){ navigator.clipboard.writeText(val).then(()=>showToast(`✓ ${label} copié`)); }

// ─── TOAST ───────────────────────────────
function showToast(msg){
  const t=document.getElementById('toast'); t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),3000);
}

// Auto-refresh counter
let refreshCount=30;
setInterval(()=>{ refreshCount--; if(refreshCount<=0){ refreshCount=30; document.getElementById('topo-update').textContent='Dernière mise à jour : à l\'instant'; } },1000);
</script>

</body>
</html>