<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sopaan · BODMAS: Order of Operations</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,700&family=Source+Sans+3:wght@400;600;700&family=IBM+Plex+Mono:wght@500;600&display=swap">
<style>
  html,body{margin:0;} img{max-width:100%;} [hidden]{display:none !important;}
  :root{
    --navy:#16264A; --navy-2:#1F3B6B; --gold:#C79A3E; --gold-soft:#F3E7C9;
    --paper:#FBF9F4; --paper-2:#F1EAD8; --card:#FFFFFF;
    --ink:#211E1A; --ink-soft:#655D4C; --rule:#E4DCC8;
    --success:#2E8B57; --success-soft:#E1F2E7;
    --danger:#BF4B45; --danger-soft:#FAE3E1;
    --locked:#C7BEA9;
    --focus:#1F3B6B;
    --accent-text:#1F3B6B; --retry-text:#8A661F;
    color-scheme:light;
  }
  @media (prefers-color-scheme: dark){
    :root:not([data-theme="light"]){
      --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
      --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
      --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
      --success:#7BC79A; --success-soft:#1B2E22;
      --danger:#E38884; --danger-soft:#331D1B;
      --locked:#4A4436;
      --focus:#E0B75B;
      --accent-text:#E0B75B; --retry-text:#E0B75B;
      color-scheme:dark;
    }
  }
  :root[data-theme="dark"]{
    --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
    --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
    --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
    --success:#7BC79A; --success-soft:#1B2E22;
    --danger:#E38884; --danger-soft:#331D1B;
    --locked:#4A4436;
    --focus:#E0B75B;
    --accent-text:#E0B75B; --retry-text:#E0B75B;
    color-scheme:dark;
  }

  *{box-sizing:border-box;}
  body{background:var(--paper); color:var(--ink); font-family:'Source Sans 3',-apple-system,'Segoe UI',sans-serif; padding-inline:0; padding-block:0;}
  h1,h2,h3{font-family:'Fraunces',Georgia,serif; text-wrap:balance; margin:0;}
  .num{font-family:'IBM Plex Mono',ui-monospace,monospace; font-variant-numeric:tabular-nums;}

  header.brand{
    background:linear-gradient(155deg,var(--navy) 0%, var(--navy-2) 100%);
    color:#F4EFDF; padding-block:calc(20px + env(safe-area-inset-top,0px)) 18px; padding-inline:20px;
  }
  .brand-row{display:flex; align-items:center; gap:12px; max-width:900px; margin:0 auto;}
  .crest{
    width:42px; height:42px; border-radius:10px; background:var(--gold); color:var(--navy);
    display:flex; align-items:center; justify-content:center; font-family:'Fraunces',serif; font-weight:700; font-size:18px; flex-shrink:0;
  }
  .brand-name{font-size:12px; letter-spacing:.08em; text-transform:uppercase; color:var(--gold); font-weight:700;}
  .series-name{font-size:19px; font-weight:700; font-family:'Fraunces',serif;}
  .chapter-eyebrow{max-width:900px; margin:14px auto 0; font-size:12px; letter-spacing:.06em; text-transform:uppercase; color:#AEB9D6;}
  .chapter-title{font-size:28px; max-width:900px; margin:2px auto 0;}
  .chapter-sub{font-size:14.5px; color:var(--gold); font-weight:700; max-width:900px; margin:3px auto 0;}

  .chapter-progress{max-width:900px; margin:14px auto 0; display:flex; align-items:center; gap:10px;}
  .cp-track{flex:1; height:6px; border-radius:99px; background:rgba(255,255,255,.18); overflow:hidden;}
  .cp-fill{height:100%; background:var(--gold); border-radius:99px; transition:width .3s ease;}
  .cp-label{font-size:11.5px; color:#CFD7EA; white-space:nowrap;}

  .tabbar{position:sticky; top:env(safe-area-inset-top,0px); z-index:15; background:var(--paper); border-bottom:1px solid var(--rule); overflow-x:auto; white-space:nowrap;}
  .tabbar-inner{max-width:900px; margin:0 auto; display:flex; padding-inline:16px;}
  .tab-btn{font-family:inherit; font-size:14px; font-weight:600; color:var(--ink-soft); background:none; border:none; padding:13px 16px; cursor:pointer; position:relative; flex-shrink:0;}
  .tab-btn.active{color:var(--accent-text);}
  .tab-btn.active::after{content:''; position:absolute; left:12px; right:12px; bottom:0; height:3px; background:var(--gold); border-radius:3px 3px 0 0;}
  .tab-count{font-size:11px; color:var(--ink-soft); margin-left:5px;}

  .slide-progress{max-width:900px; margin:0 auto; padding:10px 16px 0; display:flex; align-items:center; gap:10px;}
  .sp-track{flex:1; height:5px; border-radius:99px; background:var(--rule); overflow:hidden;}
  .sp-fill{height:100%; background:var(--accent-text); border-radius:99px; transition:width .3s ease;}
  .sp-label{font-size:12px; color:var(--ink-soft); white-space:nowrap; font-weight:600;}

  .wrap{max-width:900px; margin:0 auto; padding:16px 16px calc(110px + env(safe-area-inset-bottom,0px));}

  .qcard{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px;}
  .qhead{display:flex; align-items:flex-start; gap:10px; margin-bottom:10px;}
  .qnum{width:32px; height:32px; border-radius:8px; background:var(--gold-soft); color:var(--accent-text); display:flex; align-items:center; justify-content:center; font-weight:700; font-family:'Fraunces',serif; flex-shrink:0; font-size:14px;}
  .qtext{font-size:16px; line-height:1.55; flex:1;}
  .qtag{font-size:10.5px; color:var(--gold); background:var(--gold-soft); padding:2px 7px; border-radius:99px; font-weight:700; margin-left:6px; white-space:nowrap;}
  .marks-pill{font-size:11px; font-weight:700; color:var(--ink-soft); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; margin-left:auto; flex-shrink:0; white-space:nowrap;}
  .step-badge{font-size:11px; font-weight:700; color:var(--accent-text); background:var(--paper-2); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; display:inline-block; margin-bottom:12px;}

  .options{display:flex; flex-direction:column; gap:8px; margin-top:10px;}
  .opt{display:flex; align-items:center; gap:10px; padding:11px 12px; border:1.5px solid var(--rule); border-radius:10px; cursor:pointer; font-size:15px; background:var(--paper);}
  .opt input{accent-color:var(--navy-2);}
  .opt.is-correct{border-color:var(--success); background:var(--success-soft);}
  .opt.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .opt.locked{cursor:not-allowed;}

  .subpart-label{font-weight:700; font-size:12.5px; color:var(--accent-text); text-transform:uppercase; letter-spacing:.03em; margin:14px 0 6px;}
  .or-divider{text-align:center; font-size:11px; font-weight:700; letter-spacing:.08em; color:var(--ink-soft); margin:8px 0;}

  .steps{display:flex; flex-direction:column; gap:10px;}
  .step-line{font-size:14.5px; line-height:1.9; background:var(--paper); border:1px dashed var(--rule); border-radius:10px; padding:9px 12px;}
  .step-line.resolved{opacity:.9;}
  .blank-input{font-family:'IBM Plex Mono',monospace; font-size:14px; border:1.5px solid var(--rule); border-radius:6px; padding:3px 8px; width:120px; background:var(--card); color:var(--ink); margin:0 2px;}
  .blank-input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .blank-input.is-correct{border-color:var(--success); background:var(--success-soft);}
  .blank-input.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .blank-input[disabled]{opacity:.85;}
  .reveal-note{font-size:12px; color:var(--danger); padding-left:2px; margin-top:-2px;}

  .feedback{font-size:13.5px; padding:10px 12px; border-radius:9px; margin-top:14px; display:none;}
  .feedback.show{display:block;}
  .feedback.ok{background:var(--success-soft); color:var(--success);}
  .feedback.retry{background:var(--gold-soft); color:var(--retry-text);}
  .feedback.reveal{background:var(--danger-soft); color:var(--danger);}
  .feedback.err{background:var(--danger-soft); color:var(--danger);}
  .attempts-dots{display:inline-flex; gap:4px; margin-left:2px;}
  .attempts-dots span{width:6px; height:6px; border-radius:50%; background:var(--ink-soft); opacity:.3; display:inline-block;}
  .attempts-dots span.used{opacity:1; background:var(--danger);}

  .ar-box{background:var(--paper-2); border-radius:10px; padding:10px 12px; margin-bottom:12px; font-size:13px; color:var(--ink-soft);}

  .navbar{
    position:fixed; left:0; right:0; bottom:0; z-index:30; background:var(--paper);
    border-top:1px solid var(--rule); padding:10px 16px calc(10px + env(safe-area-inset-bottom,0px));
  }
  .navbar-inner{max-width:900px; margin:0 auto; display:flex; gap:8px; flex-wrap:wrap; align-items:center;}
  .btn{font-family:inherit; font-size:13.5px; font-weight:700; padding:10px 14px; border-radius:9px; border:1.5px solid var(--rule); background:var(--card); color:var(--ink); cursor:pointer;}
  .btn:hover{border-color:var(--navy-2);}
  .btn:disabled{opacity:.4; cursor:not-allowed;}
  .btn-primary{background:var(--navy-2); color:#fff; border-color:var(--navy-2);}
  .navbar .spacer{flex:1;}

  .fab{position:fixed; right:16px; bottom:calc(74px + env(safe-area-inset-bottom,0px)); background:var(--gold); color:var(--navy); border:none; border-radius:999px; padding:11px 16px; font-family:inherit; font-weight:700; font-size:13px; display:flex; align-items:center; gap:7px; cursor:pointer; box-shadow:0 6px 16px rgba(0,0,0,.18); z-index:35;}
  .fab-badge{background:rgba(255,255,255,.55); border-radius:99px; padding:1px 7px; font-size:11px;}

  .overlay{position:fixed; inset:0; background:rgba(20,18,14,.45); z-index:50; display:none; align-items:flex-end; justify-content:center;}
  .overlay.show{display:flex;}
  .palette{background:var(--card); width:min(480px,100%); max-height:78vh; overflow-y:auto; border-radius:18px 18px 0 0; padding:18px 18px calc(18px + env(safe-area-inset-bottom,0px)); border:1px solid var(--rule); border-bottom:none;}
  @media (min-width:640px){ .overlay{align-items:center;} .palette{border-radius:18px; border-bottom:1px solid var(--rule); max-height:74vh;} }
  .palette-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:6px;}
  .palette-head h2{font-size:16px;}
  .palette-close{background:none; border:none; font-size:20px; color:var(--ink-soft); cursor:pointer; padding:4px;}
  .palette-legend{display:flex; gap:12px; flex-wrap:wrap; font-size:11px; color:var(--ink-soft); margin:10px 0 14px;}
  .palette-legend span{display:inline-flex; align-items:center; gap:5px;}
  .dot{width:9px; height:9px; border-radius:50%; display:inline-block;}
  .dot.correct{background:var(--success);} .dot.revealed{background:var(--danger);}
  .dot.skipped{background:var(--gold);} .dot.locked{background:var(--locked);}
  .dot.current{background:var(--navy-2);}
  .chip-grid{display:grid; grid-template-columns:repeat(auto-fill,minmax(48px,1fr)); gap:8px;}
  .chip{border:1.5px solid var(--rule); border-radius:9px; padding:8px 4px; text-align:center; font-family:'IBM Plex Mono',monospace; font-size:12.5px; font-weight:600; cursor:pointer; background:var(--paper); color:var(--ink);}
  .chip[data-status="correct"]{border-color:var(--success); background:var(--success-soft); color:var(--success);}
  .chip[data-status="revealed"]{border-color:var(--danger); background:var(--danger-soft); color:var(--danger);}
  .chip[data-status="skipped"]{border-color:var(--gold); background:var(--gold-soft); color:var(--retry-text);}
  .chip[data-status="locked"]{border-color:var(--rule); color:var(--locked); cursor:not-allowed; background:var(--paper-2);}
  .chip[data-status="current"]{outline:2px solid var(--navy-2); outline-offset:1px;}

  .toast{position:fixed; left:50%; bottom:calc(140px + env(safe-area-inset-bottom,0px)); transform:translateX(-50%); background:var(--ink); color:var(--paper); padding:9px 16px; border-radius:99px; font-size:13px; opacity:0; pointer-events:none; transition:opacity .25s ease; z-index:60;}
  .toast.show{opacity:1;}

  .done-card{text-align:center; padding:36px 20px;}
  .done-card .big{font-size:38px; margin-bottom:6px;}
  .score-pills{display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin:16px 0;}
  .score-pill{padding:8px 16px; border-radius:99px; font-size:13px; font-weight:700;}

  footer.brandfoot{max-width:900px; margin:14px auto 0; padding:0 16px calc(110px + env(safe-area-inset-bottom,0px)); text-align:center; font-size:12px; color:var(--ink-soft);}

  .mode-pill{font-family:inherit; font-size:12.5px; font-weight:700; color:var(--navy); background:var(--gold); border:none; border-radius:99px; padding:6px 14px; cursor:pointer; margin-top:12px; display:inline-flex; align-items:center; gap:6px;}
  .mode-pick{display:flex; flex-direction:column; gap:14px; padding:8px 0 24px;}
  .mode-card{background:var(--card); border:1.5px solid var(--rule); border-radius:16px; padding:22px; cursor:pointer; text-align:left;}
  .mode-card:hover{border-color:var(--navy-2);}
  .mode-card .mode-icon{font-size:26px; margin-bottom:8px;}
  .mode-card h3{font-size:18px; margin-bottom:6px;}
  .mode-card p{font-size:13.5px; color:var(--ink-soft); line-height:1.5; margin:0;}
  .quiz-hint{font-size:12.5px; color:var(--ink-soft); background:var(--paper-2); border-radius:9px; padding:8px 12px; margin-bottom:14px;}
  .review-row{border:1px solid var(--rule); border-radius:10px; padding:11px 13px; margin-bottom:10px;}
  .review-tag{font-size:11.5px; font-weight:700; margin-bottom:4px; display:block;}
  .review-tag.ok{color:var(--success);} .review-tag.bad{color:var(--danger);} .review-tag.na{color:var(--ink-soft);}
  .review-q{font-size:13.5px; margin-bottom:6px; color:var(--ink-soft);}
  .review-ans{font-size:13px; margin-bottom:2px;}

  .who-row{max-width:900px; margin:12px auto 0; display:flex; flex-wrap:wrap; gap:8px; align-items:center;}
  .who-row .mode-pill{margin-top:0;}
  .who{display:inline-flex; flex-wrap:wrap; align-items:center; gap:6px; font-size:12.5px; color:#CFD7EA;}
  .who b{color:#F4EFDF;}
  .who button{font-family:inherit; font-size:12px; font-weight:700; color:#F4EFDF; background:rgba(255,255,255,.12); border:1px solid rgba(255,255,255,.22); border-radius:99px; padding:5px 11px; cursor:pointer;}
  .who button:hover{background:rgba(255,255,255,.2);}
  .login-card{max-width:460px; margin:8px auto 0; background:var(--card); border:1px solid var(--rule); border-radius:16px; padding:22px;}
  .login-card h2{font-size:21px; margin-bottom:4px;}
  .login-card p.lead{font-size:13.5px; color:var(--ink-soft); margin:0 0 16px; line-height:1.5;}
  .fld{display:flex; flex-direction:column; gap:5px; margin-bottom:12px;}
  .fld label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .fld input{font-family:inherit; font-size:15px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:9px; background:var(--paper); color:var(--ink);}
  .fld input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .fld-row{display:grid; grid-template-columns:1fr 1fr; gap:10px;}
  .login-err{font-size:13px; color:var(--danger); background:var(--danger-soft); border-radius:8px; padding:8px 10px; margin-bottom:12px;}
  .login-note{font-size:12px; color:var(--ink-soft); margin-top:12px; line-height:1.5;}
  .known{margin:0 0 16px; display:flex; flex-direction:column; gap:6px;}
  .known-label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .known-btn{display:flex; justify-content:space-between; align-items:center; gap:10px; text-align:left; font-family:inherit; font-size:14.5px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:10px; background:var(--paper); color:var(--ink); cursor:pointer;}
  .known-btn:hover{border-color:var(--navy-2);}
  .known-btn small{font-size:12px; color:var(--ink-soft);}
  .rec-card{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px; margin-bottom:14px;}
  .rec-card h2{font-size:19px; margin-bottom:10px;}
  .rec-card h3{font-size:15px; margin:4px 0 8px;}
  .rec-table-wrap{overflow-x:auto;}
  .rec-table{width:100%; border-collapse:collapse; font-size:13.5px; font-variant-numeric:tabular-nums;}
  .rec-table th{text-align:left; font-size:11.5px; text-transform:uppercase; letter-spacing:.04em; color:var(--ink-soft); padding:6px 8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-table td{padding:8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-bar{display:inline-block; width:70px; height:6px; border-radius:99px; background:var(--rule); overflow:hidden; vertical-align:middle; margin-right:6px;}
  .rec-bar i{display:block; height:100%; background:var(--success);}
  .rec-empty{font-size:13.5px; color:var(--ink-soft);}
  .sec-sub{max-width:900px; margin:0 auto; padding:10px 16px 0; font-size:12.5px; font-weight:700; color:var(--accent-text); letter-spacing:.02em;}
  .opt{flex-wrap:wrap;}
  .opt-l{font-family:'IBM Plex Mono',monospace; font-size:13px; font-weight:600; color:var(--ink-soft);}
  .opt-t{flex:1; min-width:0;}
  .opt.is-chosen:not(.is-correct):not(.is-wrong){border-color:var(--navy-2); background:var(--gold-soft);}
  .opt-tag{font-size:11px; font-weight:700; padding:2px 8px; border-radius:99px; background:var(--card); border:1px solid currentColor; white-space:nowrap;}
  .opt.is-correct .opt-tag{color:var(--success);} .opt.is-wrong .opt-tag{color:var(--danger);} .opt.is-chosen:not(.is-correct):not(.is-wrong) .opt-tag{color:var(--accent-text);}
  .chosen{font-size:13.5px; margin-top:10px; padding:8px 12px; border-radius:9px;}
  .chosen.ok{background:var(--success-soft); color:var(--success);} .chosen.bad{background:var(--danger-soft); color:var(--danger);}
  .chosen + .chosen{margin-top:6px;}
  .solution{margin-top:14px; border:1px solid var(--rule); border-left:4px solid var(--gold); background:var(--paper-2); border-radius:10px; padding:12px 14px;}
  .sol-h{font-family:'Fraunces',Georgia,serif; font-weight:700; font-size:14px; color:var(--accent-text); margin-bottom:6px;}
  .sol-line{font-size:14.5px; line-height:1.7;}
  .rev-sol summary{cursor:pointer; font-size:12.5px; font-weight:700; color:var(--accent-text); margin-top:6px;}
  .rev-sol .solution{margin-top:8px;}
  .chapter-credit{max-width:900px; margin:4px auto 0; font-size:12px; color:#CFD7EA;}
  footer.brandfoot a{color:inherit;}
  .blank-input.wide{width:260px; max-width:100%; font-family:'Source Sans 3',sans-serif;}
  .blank-input.expr{width:170px; max-width:100%;}
  /*fix-layout*/ .cp-fill,.sp-fill{width:0;} .fld{min-width:0;} .fld input{width:100%; min-width:0; box-sizing:border-box;}
  .fig{margin:6px 0 10px; display:flex; justify-content:center;}
  .figsvg{width:100%; max-width:340px; height:auto; overflow:visible;}
  .figsvg .ln,.figsvg .arm{stroke:var(--ink); stroke-width:1.6; fill:none;}
  .figsvg .arm{stroke:var(--accent-text); stroke-width:2;}
  .figsvg .pt{fill:var(--ink);}
  .figsvg .lb{fill:var(--ink); font:600 13px 'Source Sans 3',sans-serif;}
  .figsvg .al{fill:var(--accent-text); font:700 12px 'Source Sans 3',sans-serif;}
  .figsvg .wg{fill:var(--gold-soft); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .wg2{fill:rgba(199,154,62,.35); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .ra{fill:none; stroke:var(--ink); stroke-width:1.2;}
  .figsvg .ahd{fill:var(--ink);}
  .figsvg .pr{fill:var(--paper-2); stroke:var(--ink-soft); stroke-width:1.2;}
  .figsvg .tk{stroke:var(--ink-soft); stroke-width:.8;}
  .figsvg .po{fill:var(--ink); font:600 8.5px 'IBM Plex Mono',monospace;}
  .figsvg .pi{fill:var(--danger); font:600 7.5px 'IBM Plex Mono',monospace;}
  .figsvg .sh{fill:var(--gold-soft); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh2{fill:rgba(199,154,62,.38); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh3{fill:rgba(199,154,62,.22); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .none{fill:none;}
  .figsvg .hid{stroke-dasharray:5 4; fill:none;}
  .figsvg .tick{stroke:var(--ink); stroke-width:1.4; fill:none;}
  .figsvg .shd{fill:var(--accent-text); fill-opacity:.55;}
  .figsvg .cell{fill:var(--card); stroke:var(--ink); stroke-width:1.1;}
  .figsvg .dr{fill:var(--danger);} .figsvg .dw{fill:var(--card); stroke:var(--ink); stroke-width:1.2;}
  .figsvg .dotfill{fill:var(--danger);}
  .tt{border-collapse:collapse; font-size:13.5px; margin:4px auto; font-variant-numeric:tabular-nums;} .tt th,.tt td{border:1px solid var(--rule); padding:4px 9px; text-align:left;} .tt th{background:var(--paper-2); font-weight:700;} .ttw{overflow-x:auto; max-width:100%;}
  .fq{display:inline-flex; flex-direction:column; vertical-align:middle; text-align:center; font-size:.82em; line-height:1.12; margin:0 2px;}
  .fq>span:first-child{border-bottom:1.5px solid currentColor; padding:0 2px;}
  .fq>span:last-child{padding:0 2px;}
  .mx{white-space:nowrap;}
  .blank-input.fr{width:110px;}
  @media (prefers-reduced-motion:reduce){ *{transition:none !important;} }
.datline{display:block;margin-top:8px;padding:8px 10px;border-radius:8px;background:var(--paper-2);font:500 14px/1.7 'IBM Plex Mono',monospace;word-spacing:2px;overflow-wrap:anywhere;}

  .theory{display:flex;flex-direction:column;gap:16px;padding-bottom:40px;}
  .hub{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .hub{grid-template-columns:repeat(3,1fr);} }
  .hub-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:16px;display:flex;flex-direction:column;gap:8px;}
  .hub-card h3{font-family:'Fraunces',serif;font-size:18px;margin:0;color:var(--accent-text);}
  .hub-card p{margin:0;font-size:14px;color:var(--ink-soft);line-height:1.5;}
  .hub-btns{display:flex;flex-wrap:wrap;gap:8px;margin-top:auto;}
  .hub-btn{min-height:40px;border:1.5px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:10px;padding:8px 12px;font:600 14px 'Source Sans 3',sans-serif;cursor:pointer;text-align:left;}
  .hub-btn:hover{border-color:var(--gold);}
  .hub-btn.primary{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .note{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:18px;scroll-margin-top:120px;}
  .note h2{font-family:'Fraunces',serif;font-size:22px;margin:0 0 4px;color:var(--ink);}
  .note .lt{font-size:13.5px;color:var(--ink-soft);margin:0 0 12px;}
  .note .lt b{color:var(--accent-text);}
  .note h4{font:700 15px 'Source Sans 3',sans-serif;margin:16px 0 6px;color:var(--accent-text);}
  .note p,.note li{font-size:15.5px;line-height:1.6;}
  .note ul{padding-left:20px;margin:6px 0;}
  .keybox{background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:10px 12px;margin:10px 0;font-size:15px;line-height:1.6;}
  .keybox b{color:var(--ink);}
  .ex{background:var(--paper-2);border-radius:10px;padding:12px;margin:10px 0;}
  .ex .exh{font-family:'Fraunces',serif;font-weight:700;color:var(--accent-text);margin-bottom:4px;}
  .ex .exl{font-size:15px;line-height:1.7;}
  .ttab{width:100%;border-collapse:collapse;font-size:14.5px;margin:8px 0;}
  .ttab th,.ttab td{border:1px solid var(--rule);padding:7px 8px;text-align:left;vertical-align:top;}
  .ttab th{background:var(--paper-2);}
  .tscroll{overflow-x:auto;}
  .mono{font-family:'IBM Plex Mono',monospace;font-size:.92em;}
  @media (max-width:480px){ .ttab{font-size:13.5px;} .ttab th,.ttab td{padding:6px;} }
  .note .figsvg{max-width:100%;height:auto;display:block;margin:8px auto;}


  .rep-top{display:flex;flex-wrap:wrap;gap:18px;align-items:center;justify-content:center;margin-top:8px;}
  .rep-legend{flex:1;min-width:220px;display:flex;flex-direction:column;gap:8px;}
  .rep-cap{font:700 13px 'Source Sans 3',sans-serif;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.05em;}
  .rep-li{display:flex;align-items:center;gap:6px;font-size:15px;}
  .rep-n{margin-left:auto;white-space:nowrap;padding-left:8px;font-family:'IBM Plex Mono',monospace;font-weight:600;}
  .sw{display:inline-block;width:12px;height:12px;border-radius:3px;flex:none;}
  .rep-grid{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .rep-grid{grid-template-columns:1fr 1fr;} }
  .rep-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:14px;display:flex;flex-direction:column;gap:10px;}
  .rep-h{display:flex;align-items:center;gap:8px;font:700 16px 'Source Sans 3',sans-serif;}
  .crit{display:inline-grid;place-items:center;width:28px;height:28px;border-radius:50%;color:var(--card);font:700 15px Fraunces,serif;flex:none;}
  .rep-row{display:flex;gap:14px;align-items:center;}
  .rep-stats{display:flex;flex-direction:column;gap:5px;font-size:14.5px;}
  .rep-stats div{display:flex;align-items:center;gap:6px;}
  .rep-lvl{margin-top:4px;color:var(--accent-text);} .rep-lvl span{color:var(--ink-soft);font-size:13px;}
  .rep-note{font-size:13px;color:var(--ink-soft);}


  .skip-chips{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin:12px 0;}
  .skip-chips .chip{min-width:48px;min-height:40px;cursor:pointer;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--retry-text);border-radius:10px;font:600 14px 'IBM Plex Mono',monospace;}
  .skip-btns{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-top:12px;}


  .kb-toggle{border:none;background:transparent;font-size:15px;cursor:pointer;padding:0 2px;vertical-align:middle;opacity:.7;}
  .vkb{position:fixed;left:0;right:0;bottom:0;z-index:80;background:var(--paper-2);border-top:1px solid var(--rule);box-shadow:0 -6px 20px rgba(0,0,0,.12);padding:6px 6px 8px;display:none;}
  .vkb.show{display:block;}
  .vkb-top{display:flex;justify-content:space-between;align-items:center;max-width:640px;margin:0 auto 4px;font:600 12px 'Source Sans 3',sans-serif;color:var(--ink-soft);}
  .vkb-link{border:none;background:none;color:var(--accent-text);font:600 12px 'Source Sans 3',sans-serif;cursor:pointer;text-decoration:underline;}
  .vkb-row{display:flex;gap:5px;max-width:640px;margin:0 auto 5px;}
  .vkb-k{flex:1;min-height:42px;border:1px solid var(--rule);border-radius:8px;background:var(--card);color:var(--ink);font:600 17px 'IBM Plex Mono',monospace;cursor:pointer;padding:0;}
  .vkb-k.fn{font:600 13px 'Source Sans 3',sans-serif;background:var(--gold-soft);}
  .vkb-k.done{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .vkb-k:active{transform:scale(.96);}
  body.kb-open .wrap{padding-bottom:360px;}
  body.kb-open .fab{display:none;}
  .tool-bar{display:flex;gap:8px;flex-wrap:wrap;margin:-4px 0 10px;}
  .tool-btn{min-height:36px;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--ink);border-radius:18px;padding:4px 12px;font:600 13.5px 'Source Sans 3',sans-serif;cursor:pointer;}
  .tool-panel{position:fixed;z-index:85;background:var(--card);border:1px solid var(--rule);border-radius:14px;box-shadow:0 10px 30px rgba(0,0,0,.25);display:none;}
  .tool-panel.show{display:block;}
  .tp-head{display:flex;align-items:center;gap:8px;padding:8px 10px;border-bottom:1px solid var(--rule);font:600 14px 'Source Sans 3',sans-serif;}
  .tp-note{font-size:12px;color:var(--ink-soft);font-weight:500;}
  .tp-x{margin-left:auto;border:1px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:8px;min-height:32px;padding:2px 10px;cursor:pointer;font:600 13px 'Source Sans 3',sans-serif;}
  .tp-x + .tp-x{margin-left:0;}
  .calc{right:12px;bottom:84px;width:min(330px,calc(100vw - 24px));}
  body.calc-open .wrap{padding-bottom:440px;}
  body.calc-open .fab{display:none;}
  @media (max-width:640px){ .calc{left:6px;right:6px;width:auto;} .calc-keys button{min-height:36px;} }
  .calc-disp{padding:8px 12px;text-align:right;background:var(--paper-2);}
  .calc-expr{font:500 13px 'IBM Plex Mono',monospace;color:var(--ink-soft);min-height:18px;word-break:break-all;}
  .calc-res{font:600 24px 'IBM Plex Mono',monospace;color:var(--ink);word-break:break-all;}
  .calc-keys{display:grid;grid-template-columns:repeat(6,1fr);gap:5px;padding:8px;}
  .calc-keys button{min-height:40px;border:1px solid var(--rule);border-radius:8px;background:var(--card);color:var(--ink);font:600 14px 'IBM Plex Mono',monospace;cursor:pointer;padding:0;}
  .calc-keys button.fn{background:var(--gold-soft);font-family:'Source Sans 3',sans-serif;font-size:13px;}
  .calc-keys button.eq{grid-column:span 2;background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .desmos{left:50%;top:50%;transform:translate(-50%,-50%);width:min(760px,calc(100vw - 16px));height:min(560px,calc(100vh - 90px));flex-direction:column;}
  .desmos.show{display:flex;}
  #desmosBox{flex:1;min-height:0;border-radius:0 0 14px 14px;overflow:hidden;}
  .desmos-msg{padding:24px;text-align:center;color:var(--ink-soft);}
  .chip[data-status="skipped"]{cursor:pointer;}
  .chip[data-status="answered"]{border-color:var(--accent-text);color:var(--accent-text);}


  button.crest-home{border:none;padding:0;position:relative;cursor:pointer;transition:transform .15s ease;}
  button.crest-home:hover,button.crest-home:focus-visible{transform:scale(1.06);outline:2px solid var(--gold);outline-offset:2px;}
  .crest-h{position:absolute;right:-7px;bottom:-7px;width:18px;height:18px;border-radius:50%;background:#F4EFDF;color:var(--navy);font:700 12px/18px 'Source Sans 3',sans-serif;text-align:center;box-shadow:0 1px 3px rgba(0,0,0,.3);}
  .brand .brand-row{position:relative;}
  .bmasw{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);display:flex;flex-shrink:0;white-space:nowrap;align-items:center;gap:6px;background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.25);border-radius:99px;padding:5px 14px;color:#F4EFDF;}
  .bmasw-ic{font-size:16px;} .bmasw-t{font:600 20px 'IBM Plex Mono',monospace;letter-spacing:.02em;min-width:62px;text-align:center;}
  .bmasw-b{border:none;background:rgba(255,255,255,.18);color:#F4EFDF;border-radius:50%;width:30px;height:30px;cursor:pointer;font-size:13px;}
  .bmasw.off{display:none;}
  @media (max-width:560px){ .bmasw{position:static;transform:none;margin-left:auto;padding:4px 5px 4px 8px;gap:4px;} .bmasw-ic{display:none;} .bmasw-t{font-size:16px;min-width:48px;} .bmasw-b{width:26px;height:26px;} .brand .series-name{font-size:15px;} .brand .brand-name{font-size:10.5px;} .brand .brand-row>div:not(.bmasw){min-width:0;} }
  .bmasw-done{font:600 15px 'IBM Plex Mono',monospace;color:var(--accent-text);margin:4px 0 8px;}
  .sp-fab{position:fixed;left:14px;bottom:84px;z-index:60;border:1.5px solid var(--navy-2);background:var(--card);color:var(--ink);border-radius:99px;padding:9px 14px;font:700 14px 'Source Sans 3',sans-serif;box-shadow:0 4px 14px rgba(0,0,0,.15);cursor:pointer;}
  .sp-fab.on{background:var(--navy);color:#F4EFDF;}
  body.kb-open .sp-fab{display:none;}
  .sp-canvas{position:absolute;z-index:55;display:none;touch-action:none;cursor:crosshair;}
  .sp-canvas.passive{pointer-events:none;cursor:default;}
  .sp-bar{position:fixed;top:8px;left:8px;right:8px;margin:0 auto;width:max-content;z-index:90;display:none;align-items:center;gap:6px;background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:6px 8px;box-shadow:0 6px 20px rgba(0,0,0,.2);max-width:calc(100vw - 16px);flex-wrap:wrap;justify-content:center;}
  .sp-lbl{font:700 13px 'Source Sans 3',sans-serif;color:var(--ink-soft);margin-right:2px;}
  .sp-pen,.sp-tool{width:38px;height:38px;border-radius:10px;border:1.5px solid var(--rule);background:var(--paper);cursor:pointer;display:inline-flex;align-items:center;justify-content:center;font-size:17px;padding:0;color:var(--ink);}
  .sp-pen i{width:20px;height:20px;border-radius:50%;display:block;}
  .sp-pen.on,.sp-tool.on{border-color:var(--navy-2);box-shadow:0 0 0 3px var(--gold-soft);}
  @media (max-width:560px){ .sp-lbl{display:none;} .sp-pen,.sp-tool{width:34px;height:34px;} .sp-fab span{display:none;} }

  /* larger reading sizes */
  .chapter-eyebrow{font-size:13.5px;}
  .chapter-title{font-size:36px;line-height:1.15;}
  .chapter-sub{font-size:16.5px;}
  .tab-btn{font-size:15.5px;}
  .sec-sub{font-size:15px;}
  .qtext{font-size:19px;line-height:1.55;}
  .qnum{width:36px;height:36px;font-size:16px;}
  .opt{font-size:17.5px;padding:12px 14px;}
  .step-line{font-size:17.5px;line-height:2;}
  .blank-input{font-size:16px;}
  .sol-line{font-size:16.5px;}
  .feedback{font-size:15px;}
  .note h2{font-size:28px;}
  .note h4{font-size:19px;}
  .note p,.note li{font-size:17.5px;line-height:1.65;}
  .note .lt{font-size:15.5px;}
  .ex .exh{font-size:18px;} .ex .exl{font-size:16.5px;}
  .keybox{font-size:16.5px;}
  .ttab{font-size:15.5px;}
  .hub-card h3{font-size:21px;} .hub-card p{font-size:15.5px;} .hub-btn{font-size:15px;}
  @media (max-width:480px){ .chapter-title{font-size:30px;} .qtext{font-size:18px;} .opt,.step-line{font-size:16.5px;} .note h2{font-size:24px;} .note p,.note li{font-size:16.5px;} }

  /* v4 celebration */
  .v4conf{position:fixed;inset:0;width:100vw;height:100vh;pointer-events:none;z-index:9999;}
  .v4ban{background:linear-gradient(135deg,var(--gold-soft),var(--card));border:2px solid var(--gold);border-radius:18px;padding:18px 14px 16px;margin:-4px 0 18px;text-align:center;animation:v4pop .55s cubic-bezier(.2,1.6,.4,1);}
  @keyframes v4pop{0%{transform:scale(.6);opacity:0}100%{transform:scale(1);opacity:1}}
  .v4trophy{font-size:54px;line-height:1;animation:v4bob 1.4s ease-in-out infinite;}
  @keyframes v4bob{0%,100%{transform:translateY(0) rotate(-6deg)}50%{transform:translateY(-6px) rotate(6deg)}}
  .v4h{font-size:28px;font-weight:800;color:var(--accent-text);margin-top:6px;}
  .v4m{font-size:19px;color:var(--ink);margin-top:4px;}
  .v4pills{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin-top:12px;}
  .v4p{font-size:16px;font-weight:700;border-radius:999px;padding:6px 12px;}
  .v4p.g{background:var(--success-soft);color:var(--success);} .v4p.o{background:var(--gold-soft);color:var(--retry-text);}
  .v4p.r{background:var(--danger-soft);color:var(--danger);} .v4p.s{background:var(--rule);color:var(--ink);}
  .v4next{margin-top:14px;} .v4go{font-size:18px !important;padding:13px 20px !important;}
  .v4rec td.v4g{color:var(--success);font-weight:700;} .v4rec td.v4o{color:var(--retry-text);font-weight:700;} .v4rec td.v4r{color:var(--danger);font-weight:700;}
  .v4rec tr.v4tot td{font-weight:800;border-top:2px solid var(--rule);}
  .v4rec th{font-size:12.5px;padding:6px 3px !important;white-space:nowrap;} .v4rec td{font-size:15px;padding:7px 3px !important;text-align:center;}
  .v4rec td:first-child,.v4rec th:first-child{text-align:left;white-space:normal;min-width:92px;max-width:150px;font-size:14.5px;}
  @media(max-width:460px){ .v4left{display:none;} }
  .v4leg{font-size:14.5px;color:var(--ink-soft);margin:2px 0 8px;line-height:1.5;}
  body.v4done .fab-q,body.v4done #qFab{display:none;}
  .v4sum{font-size:16px;margin-top:10px;}
  /* v4 larger reading sizes */
  body{font-size:18px;}
  .chapter-title{font-size:38px !important;line-height:1.15;}
  .chapter-sub{font-size:17px !important;}
  .tab-btn{font-size:16.5px !important;}
  .sec-sub{font-size:16.5px !important;}
  .qtext{font-size:21px !important;line-height:1.6 !important;}
  .qnum{width:38px !important;height:38px !important;font-size:17px !important;}
  .opt{font-size:19.5px !important;padding:13px 15px !important;}
  .step-line{font-size:19.5px !important;line-height:2.1 !important;}
  .blank-input{font-size:18px !important;min-height:40px;}
  .sol-line{font-size:18px !important;line-height:1.6;}
  .feedback{font-size:16.5px !important;}
  .note h2{font-size:29px !important;}
  .note h3{font-size:22px !important;}
  .note h4{font-size:20px !important;}
  .note p,.note li,.note td,.note th{font-size:18.5px !important;line-height:1.7 !important;}
  .ex .exh{font-size:19px !important;} .ex .exl{font-size:18px !important;}
  .keybox{font-size:18px !important;}
  .hub-card p{font-size:17.5px !important;} .hub-btn{font-size:16.5px !important;}
  .review-q,.review-ans{font-size:17.5px !important;}
  .rec-table td{font-size:16px;}
  .vkb-k{font-size:19px !important;}
  .fq{font-size:0.95em;}

  header.brand{background:linear-gradient(180deg,rgba(20,20,20,.55),rgba(20,20,20,.78)),url('data:image/jpeg;base64,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') center/cover no-repeat, #2b2b2b !important;}
  .crest,button.crest-home{width:58px !important;height:54px !important;background:transparent !important;border-radius:12px !important;}
  .crest img{width:58px;height:53px;display:block;filter:drop-shadow(0 1px 2px rgba(0,0,0,.5));}
  button.crest-home .crest-h{right:-4px;bottom:-4px;}
  .brand-name{color:#F4EFDF !important;font-family:'Fraunces',Georgia,serif;font-size:13px !important;letter-spacing:.06em;}
  .series-name{color:#E8D9A8;}
  .chapter-eyebrow{color:#D9D4C7 !important;}
  .chapter-sub{color:#F1D58A !important;}
</style>
</head>
<body>
<header class="brand">
  <div class="brand-row">
    <div class="crest"><img src="data:image/png;base64,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" alt="Brain and Mind"></div>
    <div>
      <div class="brand-name">Brain &amp; Mind Academy</div>
      <div class="series-name">Sopaan <span style="font-weight:400;font-size:14px;color:#CFD7EA;">Practice Series</span></div>
    </div>
  </div>
  <div class="chapter-eyebrow">Mathematics · Grade 6–7 · Topic sheet</div>
  <div class="chapter-title">BODMAS: Order of Operations</div>
  <div class="chapter-sub">Theory Notes · Step-by-step Practice · Test</div><div class="chapter-credit">Theory · Step-by-step practice · Learning assessment</div>
  <div class="chapter-progress">
    <div class="cp-track"><div class="cp-fill" id="cpFill"></div></div>
    <div class="cp-label" id="cpLabel">0% complete</div>
  </div>
  <div class="who-row"><button class="mode-pill" id="modePill" hidden>Choose a mode</button><span class="who" id="whoBar" hidden></span></div>
</header>

<nav class="tabbar"><div class="tabbar-inner" id="tabbar"></div></nav>
<div class="sec-sub" id="secSub"></div><div class="slide-progress"><div class="sp-track"><div class="sp-fill" id="spFill"></div></div><div class="sp-label" id="spLabel">Question 1 of 49</div></div>
<div class="wrap" id="wrap"></div>
<footer class="brandfoot">Brain &amp; Mind Academy · Sopaan Practice Series<br>BODMAS / BIDMAS — theory notes, questions and worked solutions written by Brain &amp; Mind Academy.</footer>

<div class="navbar"><div class="navbar-inner" id="navbarInner"></div></div>
<button class="fab" id="paletteFab"><span>Questions</span><span class="fab-badge" id="fabBadge">0/49</span></button>

<div class="overlay" id="overlay">
  <div class="palette" role="dialog" aria-label="Question palette">
    <div class="palette-head"><h2 id="paletteTitle">Question palette</h2><button class="palette-close" id="paletteClose">&times;</button></div>
    <div class="palette-legend">
      <span><i class="dot current"></i>Current</span><span><i class="dot correct"></i>Correct</span>
      <span><i class="dot revealed"></i>Revealed</span><span><i class="dot skipped"></i>Skipped</span>
      <span><i class="dot locked"></i>Locked</span>
    </div>
    <div class="chip-grid" id="paletteBody"></div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script>
(function(){
"use strict";

/* ================= audio ================= */
var audioCtx=null;
function ensureAudio(){ if(!audioCtx){ try{ audioCtx=new (window.AudioContext||window.webkitAudioContext)(); }catch(e){} } return audioCtx; }
function playTone(freqs,dur,type){
  var ctx=ensureAudio(); if(!ctx) return;
  try{
    var t0=ctx.currentTime;
    freqs.forEach(function(f,i){
      var o=ctx.createOscillator(), g=ctx.createGain();
      o.type=type||'sine'; o.frequency.value=f;
      var start=t0+i*dur;
      g.gain.setValueAtTime(0.0001,start);
      g.gain.exponentialRampToValueAtTime(0.16,start+0.02);
      g.gain.exponentialRampToValueAtTime(0.0001,start+dur);
      o.connect(g); g.connect(ctx.destination);
      o.start(start); o.stop(start+dur+0.02);
    });
  }catch(e){}
}
function playSuccess(){ playTone([523.25,659.25,783.99],0.11,'sine'); }
function playWrong(){ playTone([220,185],0.14,'square'); }
function playReveal(){ playTone([300,220],0.16,'triangle'); }

/* ================= helpers ================= */
function norm(s){
  return String(s===undefined||s===null?'':s).trim().toLowerCase().replace(/\s+/g,'')
    .replace(/[×∗*·]/g,'x').replace(/÷/g,'/').replace(/–|—|−/g,'-')
    .replace(/[²]/g,'^2').replace(/[³]/g,'^3').replace(/[⁴]/g,'^4').replace(/[⁵]/g,'^5')
    .replace(/[⁶]/g,'^6').replace(/[⁷]/g,'^7').replace(/[⁸]/g,'^8').replace(/[⁹]/g,'^9')
    .replace(/\^/g,'');
}
function parseNum(s){
  if(s===undefined||s===null) return null;
  var t=String(s).trim().replace(/,/g,'').replace(/[−–—]/g,'-').replace(/\s+/g,''); if(t==='') return null;
  var neg=false; if(t[0]==='-'){neg=true;t=t.slice(1);}
  var m=t.match(/^(\d+)\/(\d+)$/);
  if(m){ var v=parseInt(m[1],10)/parseInt(m[2],10); return neg?-v:v; }
  if(/^\d+(\.\d+)?$/.test(t)){ var v2=parseFloat(t); return neg?-v2:v2; }
  return null;
}
/* ---- expression checker: compares answers like (P-2w)/2 and P/2-w at random values ---- */
function exprPrep(s){
  return String(s).toLowerCase().replace(/\s+/g,'').replace(/^[a-z]\(x\)=/i,'').replace(/^[a-z]=/i,'').replace(/[×∗·]/g,'*').replace(/÷/g,'/').replace(/[−–—]/g,'-')
    .replace(/²/g,'^2').replace(/³/g,'^3').replace(/⁴/g,'^4').replace(/⁵/g,'^5').replace(/⁶/g,'^6').replace(/⁷/g,'^7').replace(/⁸/g,'^8').replace(/⁹/g,'^9').replace(/π/g,'#').replace(/pi/gi,'#').replace(/½/g,'(1/2)');
}
function exprCompile(src){
  var s=exprPrep(src), i=0, toks=[], absDepth=0;
  while(i<s.length){
    var ch=s[i];
    if(/[0-9.]/.test(ch)){ var j=i; while(j<s.length && /[0-9.]/.test(s[j])) j++; toks.push({k:'n',v:parseFloat(s.slice(i,j))}); i=j; continue; }
    if(/[a-zA-Z#]/.test(ch)){ toks.push({k:'v',v:ch}); i++; continue; }
    if(ch==='|'){ var pv=toks[toks.length-1]; var opn=(absDepth===0)||!pv||('+-*/^(['.indexOf(pv.k)>=0); if(opn){ absDepth++; toks.push({k:'['}); } else { absDepth--; toks.push({k:']'}); } i++; continue; }
    if('+-*/^()'.indexOf(ch)>=0){ toks.push({k:ch}); i++; continue; }
    return null;
  }
  var out=[];
  for(var t=0;t<toks.length;t++){
    var a=toks[t], b=out[out.length-1];
    if(b && (b.k==='n'||b.k==='v'||b.k===')'||b.k===']') && (a.k==='n'||a.k==='v'||a.k==='('||a.k==='[')) out.push({k:'&'});
    out.push(a);
  }
  var p=0;
  function peek(){ return out[p]; }
  function parseE(){ var n=parseT(); while(peek() && (peek().k==='+'||peek().k==='-')){ var o=out[p++].k, r=parseT(); n=(function(l,r,o){return function(e){ return o==='+'?l(e)+r(e):l(e)-r(e); };})(n,r,o); } return n; }
  function parseI(){ var n=parseU(); while(peek() && peek().k==='&'){ p++; var r=parseP(); n=(function(l,r){return function(e){ return l(e)*r(e); };})(n,r); } return n; }
  function parseT(){ var n=parseI(); while(peek() && (peek().k==='*'||peek().k==='/')){ var o=out[p++].k, r=parseI(); n=(function(l,r,o){return function(e){ return o==='*'?l(e)*r(e):l(e)/r(e); };})(n,r,o); } return n; }
  function parseU(){ if(peek() && peek().k==='-'){ p++; var u=parseU(); return function(e){ return -u(e); }; } if(peek() && peek().k==='+'){ p++; return parseU(); } return parseP(); }
  function parseP(){ var b=parseA(); if(peek() && peek().k==='^'){ p++; var x=parseU(); return function(e){ return Math.pow(b(e),x(e)); }; } return b; }
  function parseA(){
    var t=out[p++]; if(!t) throw 0;
    if(t.k==='n') return function(){ return t.v; };
    if(t.k==='v') return t.v==='#' ? function(){ return Math.PI; } : function(e){ return e[t.v]; };
    if(t.k==='('){ var n=parseE(); if(!peek()||peek().k!==')') throw 0; p++; return n; }
    if(t.k==='['){ var m=parseE(); if(!peek()||peek().k!==']') throw 0; p++; return function(e){ return Math.abs(m(e)); }; }
    throw 0;
  }
  try{ var f=parseE(); if(p!==out.length) return null; return f; }catch(e){ return null; }
}
function exprEqual(input,answer){
  var f=exprCompile(input), g=exprCompile(answer); if(!f||!g) return false;
  var hits=0;
  for(var trial=0;trial<8;trial++){
    var env={}; 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').forEach(function(c){ env[c]=1.3+Math.random()*4.1; });
    var x=f(env), y=g(env);
    if(!isFinite(x)||!isFinite(y)) continue;
    if(Math.abs(x-y)>1e-7*Math.max(1,Math.abs(x),Math.abs(y))) return false;
    hits++;
  }
  return hits>=3;
}
function wordsNorm(s){ return String(s||'').toLowerCase().replace(/\band\b/g,' ').replace(/[^a-z]/g,''); }
function listNorm(s){ return (String(s||'').replace(/[−–—]/g,'-').replace(/(\d) (?=\d{3}\b)/g,'$1').match(/-?\d+(\.\d+)?/g)||[]).join(','); }
function powNorm(s){ return String(s||'').toLowerCase().replace(/\s+/g,'').replace(/[×∗*·.]/g,'x').replace(/[⁰¹²³⁴⁵⁶⁷⁸⁹]+/g,function(m){ return '^'+m.split('').map(function(c){ return '⁰¹²³⁴⁵⁶⁷⁸⁹'.indexOf(c); }).join(''); }).replace(/\^1(?!\d)/g,''); }
function fr(s){ return String(s).replace(/\{(\d+) (\d+)\/(\d+)\}/g,'<span class="mx">$1<span class="fq"><span>$2</span><span>$3</span></span></span>').replace(/\{(\d+)\/(\d+)\}/g,'<span class="fq"><span>$1</span><span>$2</span></span>'); }
function gcdI(a,b){ a=Math.abs(a); b=Math.abs(b); while(b){ var t=a%b; a=b; b=t; } return a; }
function fracParse(s){ s=String(s===undefined||s===null?'':s).trim().replace(/[\u2044\u2215\u00f7]/g,'/').replace(/\s+/g,' ').replace(/\s*\/\s*/g,'/'); var m;
  if((m=s.match(/^(-?\d+)$/))) return {v:+m[1],form:'int',n:+m[1],d:1};
  if((m=s.match(/^(-?\d+)\/(\d+)$/))){ if(+m[2]===0) return null; return {v:m[1]/m[2],form:'frac',n:+m[1],d:+m[2]}; }
  if((m=s.match(/^(-?)(\d+)(?: |\+)(\d+)\/(\d+)$/))){ if(+m[4]===0) return null; var sg=m[1]?-1:1; return {v:sg*(+m[2]+m[3]/m[4]),form:'mixed',w:sg*m[2],n:+m[3],d:+m[4]}; }
  if((m=s.match(/^-?\d*\.\d+$/))) return {v:parseFloat(s),form:'dec'};
  return null; }
function fracOK(mode,input,answer){
  if(mode==='flist'){ var A=String(input).split(/[,;]/).map(function(x){return x.trim();}).filter(Boolean), K=String(answer).split(/[,;]/).map(function(x){return x.trim();});
    if(A.length!==K.length) return false; return A.every(function(x,i){ var a=fracParse(x), k=fracParse(K[i]); return a&&k&&Math.abs(a.v-k.v)<1e-9; }); }
  var a=fracParse(input), k=fracParse(answer); if(!a||!k) return false;
  var same=Math.abs(a.v-k.v)<1e-9; if(!same) return false;
  if(mode==='fv') return true;
  if(mode==='dec') return a.form==='dec'||a.form==='int';
  if(mode==='fe') return (a.form==='frac'&&k.form==='frac'&&a.n===k.n&&a.d===k.d)||(a.form==='int'&&k.form==='int');
  if(mode==='fi') return a.form==='frac'||(a.form==='int'&&k.form==='int');
  if(mode==='fl') return a.form==='int'||(a.form==='frac'&&gcdI(a.n,a.d)===1)||(a.form==='mixed'&&a.n<a.d&&gcdI(a.n,a.d)===1);
  if(mode==='fm') return a.form==='int'||(a.form==='mixed'&&a.n>0&&a.n<a.d&&gcdI(a.n,a.d)===1);
  return false; }
function answerMatches(input,answer,accept,expr){
  if(expr==='dec'||expr==='fv'||expr==='fe'||expr==='fl'||expr==='fm'||expr==='fi'||expr==='flist'){ if(input===undefined||input===null||String(input).trim()==='') return false; return fracOK(expr,input,answer); }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  if(expr==='words'){ return [answer].concat(accept||[]).some(function(a){ if(/\d/.test(a)) return norm(a)===norm(input); var w=wordsNorm(a); return w!=='' && w===wordsNorm(input); }); }
  if(expr==='list'){ return [answer].concat(accept||[]).some(function(a){ return listNorm(a)===listNorm(input); }); }
  if(expr==='pow'){ return [answer].concat(accept||[]).some(function(a){ return powNorm(a)===powNorm(input); }); }
  if(expr==='time'){ var tn=function(x){ var t=String(x).toLowerCase().replace(/[\s.]/g,'').replace(/[:h]/g,''); if(/^\d{3}(am|pm)?$/.test(t)) t='0'+t; return t; }; return [answer].concat(accept||[]).some(function(a){ return tn(a)===tn(input); }); }
  if(expr==='glist'){ var gn=function(x){ return String(x).toUpperCase().split(/[\s,;]+/).filter(Boolean).sort().join(','); }; return [answer].concat(accept||[]).some(function(a){ return gn(a)===gn(input); }); }
  if(expr==='coord'){ var cn=function(x){ return String(x).replace(/[\u2212\u2013\u2014]/g,'-').replace(/[\s()\[\]]/g,''); }; return [answer].concat(accept||[]).some(function(a){ return cn(a)===cn(input); }); }
  if(expr==='dlist'){ var dn=function(x){ return (String(x).replace(/[−–—]/g,'-').match(/-?\d*\.?\d+/g)||[]).map(Number).join(','); }; return [answer].concat(accept||[]).some(function(a){ return dn(a)===dn(input); }); }
  if(expr==='set'){ var sn=function(x){ return (String(x).match(/-?\d+/g)||[]).map(Number).sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return sn(a)===sn(input); }); }
  if(expr==='primes'){ var pn=function(x){ var t=powNorm(x).replace(/[^0-9x^]/g,''); var out=[]; t.split('x').forEach(function(tok){ if(!tok) return; var m=tok.split('^'); var b=+m[0], e=m[1]?+m[1]:1; for(var i=0;i<e;i++) out.push(b); }); return out.sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return pn(a)===pn(input); }); }
  if(expr==='angle'){ var an=function(x){ return String(x).toUpperCase().replace(/[^A-Z]/g,''); }; var k=an(answer), v=an(input); return v===k || v===k.split('').reverse().join(''); }
  if(expr==='trans'){ var tv=function(x){ var s=String(x).toLowerCase().replace(/[−–—]/g,'-').replace(/\b(units?|squares?|and|then|steps?|to|the|a)\b/g,' ').replace(/[,;.]/g,' '); var re=/(\d+)\s*(right|left|up|down|r|l|u|d)\b/g, m, dx=0, dy=0, n=0; while((m=re.exec(s))){ var v=+m[1], d=m[2][0]; if(d==='r')dx+=v; else if(d==='l')dx-=v; else if(d==='u')dy+=v; else dy-=v; n++; } if(!n||s.replace(re,'').replace(/\s+/g,'')!=='') return null; return dx+','+dy; }; var ti=tv(input); return ti!==null && [answer].concat(accept||[]).some(function(a){ return tv(a)===ti; }); }
  if(expr==='rot'){ var rv=function(x){ var s=String(x).toLowerCase().replace(/quarter[\s-]*turn/g,'90').replace(/half[\s-]*turn/g,'180').replace(/three[\s-]*quarter[\s-]*turn/g,'270'); var m=s.match(/\b(90|180|270)\b/); if(!m) return null; var a=+m[1]; if(a===180) return '180'; var dir=/anti|counter|acw|ccw/.test(s)?-1:(/clockwise|\bcw\b/.test(s)?1:0); if(!dir) return null; return String(((a*dir)%360+360)%360); }; var ri=rv(input); return ri!==null && ri===rv(answer); }
  if(expr==='expanded'){ var f=function(x){ return String(x).replace(/\s+/g,'').replace(/,/g,''); }; return [answer].concat(accept||[]).some(function(a){ return f(a)===f(input); }); }
  if(expr===true && input!==undefined && input!==null && String(input).trim()!==''){
    if(exprEqual(input,answer)) return true;
    var alt=String(input).replace(/\/\s*(\d+(?:\.\d+)?)\s*([a-zA-Z(])/g,'/$1*$2'); if(alt!==String(input) && exprEqual(alt,answer)) return true;
    return (accept||[]).some(function(a){ return exprEqual(input,a); });
  }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  var n1=parseNum(input), n2=parseNum(answer);
  if(n1!==null && n2!==null){ if(Math.abs(n1-n2)<1e-3) return true; return (accept||[]).some(function(a){ var n3=parseNum(a); return n3!==null && Math.abs(n1-n3)<1e-3; }); }
  var cands=[answer].concat(accept||[]); var ni=norm(input);
  for(var i=0;i<cands.length;i++){ if(norm(cands[i])===ni) return true; }
  return false;
}
function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;'); }

/* ================= DATA ================= */
var AR_OPTIONS = ["Both A and R are true, and R is the correct explanation of A","Both A and R are true, but R is NOT the correct explanation of A","A is true but R is false","A is false but R is true"];
var THEORY = "<div class=\"hub\"><div class=\"hub-card\"><h3>📖 Theory notes</h3><p>Short notes for every part of the topic: the order ladder, rule tables, common mistakes and worked examples with every “=” line shown.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-jump=\"n1\">1 · What BODMAS means</button><button class=\"hub-btn\" data-jump=\"n2\">2 · Three-step expressions</button><button class=\"hub-btn\" data-jump=\"n3\">3 · Brackets within brackets and 'of'</button><button class=\"hub-btn\" data-jump=\"n4\">4 · Integers and decimals</button><button class=\"hub-btn\" data-jump=\"n5\">5 · Powers and roots (BIDMAS)</button><button class=\"hub-btn\" data-jump=\"n6\">6 · Word problems</button></div></div><div class=\"hub-card\"><h3>🎯 Step-by-step practice</h3><p>Six practice sheets, easy to hard. Multiple-choice questions alternate with fill-in questions that walk you through one step at a time.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s1\">1 · What BODMAS means</button><button class=\"hub-btn\" data-go=\"s2\">2 · Three-step expressions</button><button class=\"hub-btn\" data-go=\"s3\">3 · Brackets within brackets and 'of'</button><button class=\"hub-btn\" data-go=\"s4\">4 · Integers and decimals</button><button class=\"hub-btn\" data-go=\"s5\">5 · Powers and roots (BIDMAS)</button><button class=\"hub-btn\" data-go=\"s6\">6 · Word problems</button></div></div><div class=\"hub-card\"><h3>📝 Test</h3><p>Four short assessments mixing the whole topic, one per skill area. Take them in Quiz mode and open the report to see your results.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s7\">A · Knowing and understanding</button><button class=\"hub-btn\" data-go=\"s8\">B · Investigating patterns</button><button class=\"hub-btn\" data-go=\"s9\">C · Communicating</button><button class=\"hub-btn\" data-go=\"s10\">D · Applying mathematics in real-life contexts</button><button class=\"hub-btn primary\" data-go=\"report\">📊 View my report</button></div></div></div><section class=\"note\" id=\"nintro\"><h2>Why do we need an order?</h2><p>Look at <span class=\"mono\">6 + 4 × 3</span>. Two students work it out differently:</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Student</th><th>Working</th><th>Answer</th></tr><tr><td>Aman (left to right)</td><td class=\"mono\">6 + 4 = 10, then 10 × 3</td><td>30 ✗</td></tr><tr><td>Bela (× before +)</td><td class=\"mono\">4 × 3 = 12, then 6 + 12</td><td>18 ✓</td></tr></table></div><p>One expression must have <b>one</b> value, or nobody could check anyone else’s work. So mathematicians everywhere agree on an <b>order of operations</b>. In India and the UK it is remembered as <b>BODMAS</b>; other countries use BIDMAS or PEMDAS, which mean exactly the same thing.</p><div style=\"margin:10px 0 12px;max-width:520px\"><div style=\"font-size:13px;color:var(--ink-soft);margin-bottom:6px\">⬇ work down the ladder, one rung at a time</div><div style=\"display:flex;align-items:stretch;gap:8px;margin:0 0 6px 0px;\"><div style=\"flex:0 0 auto;min-width:54px;display:flex;align-items:center;justify-content:center;border-radius:8px;background:var(--accent-text);color:var(--card);font:700 18px/1 'Fraunces',serif;padding:8px 6px;\">B</div><div style=\"flex:1 1 auto;min-width:0;background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:6px 10px;\"><b>1st · Brackets</b><br><span style=\"font-size:14px;color:var(--ink-soft)\">( ) then { } then [ ], from the inside out</span></div></div><div style=\"display:flex;align-items:stretch;gap:8px;margin:0 0 6px 14px;\"><div style=\"flex:0 0 auto;min-width:54px;display:flex;align-items:center;justify-content:center;border-radius:8px;background:var(--accent-text);color:var(--card);font:700 18px/1 'Fraunces',serif;padding:8px 6px;\">O</div><div style=\"flex:1 1 auto;min-width:0;background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:6px 10px;\"><b>2nd · Of and Orders</b><br><span style=\"font-size:14px;color:var(--ink-soft)\">“of” ({1/2} of 18 = 9), powers such as 3² and square roots such as √49</span></div></div><div style=\"display:flex;align-items:stretch;gap:8px;margin:0 0 6px 28px;\"><div style=\"flex:0 0 auto;min-width:54px;display:flex;align-items:center;justify-content:center;border-radius:8px;background:var(--accent-text);color:var(--card);font:700 18px/1 'Fraunces',serif;padding:8px 6px;\">D M</div><div style=\"flex:1 1 auto;min-width:0;background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:6px 10px;\"><b>3rd · Divide and Multiply</b><br><span style=\"font-size:14px;color:var(--ink-soft)\">same rank: work from left to right</span></div></div><div style=\"display:flex;align-items:stretch;gap:8px;margin:0 0 6px 42px;\"><div style=\"flex:0 0 auto;min-width:54px;display:flex;align-items:center;justify-content:center;border-radius:8px;background:var(--accent-text);color:var(--card);font:700 18px/1 'Fraunces',serif;padding:8px 6px;\">A S</div><div style=\"flex:1 1 auto;min-width:0;background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:6px 10px;\"><b>4th · Add and Subtract</b><br><span style=\"font-size:14px;color:var(--ink-soft)\">same rank: work from left to right</span></div></div></div><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Sheet</th><th>Part</th><th>Learning target</th></tr><tr><td>1</td><td>What BODMAS means</td><td>I can say the order B-O-D-M-A-S, do “of” before ÷ and ×, and work ÷ × and + − from left to right.</td></tr><tr><td>2</td><td>Three-step expressions</td><td>I can evaluate expressions with two or three bracket groups, writing one “=” line per step.</td></tr><tr><td>3</td><td>Brackets within brackets and 'of'</td><td>I can remove ( ), { } and [ ] from the inside out, work out “of” before ÷ and ×, and work with negative integers.</td></tr><tr><td>4</td><td>Integers and decimals</td><td>I can apply BODMAS to expressions with negative integers and decimals, and round when asked.</td></tr><tr><td>5</td><td>Powers and roots (BIDMAS)</td><td>I can work out powers and square roots before × ÷ + −, and tell (−3)² from −3².</td></tr><tr><td>6</td><td>Word problems</td><td>I can turn a story into an expression, find each part first, then add or subtract to get the answer.</td></tr></table></div><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Assessment</th><th>Skill area</th><th>What it checks</th></tr><tr><td>A</td><td>Knowing and understanding</td><td>Working out expressions correctly.</td></tr><tr><td>B</td><td>Investigating patterns</td><td>How moving brackets changes the value; number patterns.</td></tr><tr><td>C</td><td>Communicating</td><td>Notation, explaining the order, spotting errors.</td></tr><tr><td>D</td><td>Applying mathematics in real-life contexts</td><td>Multi-step money, measure and travel problems, and estimating.</td></tr></table></div><p><b>Typing answers:</b> type whole numbers like <span class=\"mono\">-12</span> and decimals like <span class=\"mono\">4.5</span> or <span class=\"mono\">-0.75</span>. For an operation, type <span class=\"mono\">+</span>, <span class=\"mono\">-</span>, <span class=\"mono\">x</span> or <span class=\"mono\">/</span>. ✏️ opens a scratchpad for rough work.</p></section><section class=\"note\" id=\"n1\"><h2>1 · What BODMAS means</h2><p class=\"lt\"><b>Learning target:</b> I can say the order B-O-D-M-A-S, do “of” before ÷ and ×, and work ÷ × and + − from left to right.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Letter</th><th>Stands for</th><th>Examples</th></tr><tr><td><b>B</b></td><td>Brackets</td><td class=\"mono\">( )  { }  [ ]</td></tr><tr><td><b>O</b></td><td>Of (and Orders: powers, roots)</td><td class=\"mono\">{1/2} of 20, 5², √36</td></tr><tr><td><b>D</b></td><td>Division</td><td class=\"mono\">÷</td></tr><tr><td><b>M</b></td><td>Multiplication</td><td class=\"mono\">×</td></tr><tr><td><b>A</b></td><td>Addition</td><td class=\"mono\">+</td></tr><tr><td><b>S</b></td><td>Subtraction</td><td class=\"mono\">−</td></tr></table></div><h4>Same rank: the left-to-right rule</h4><p>D and M are on the <b>same rung</b> of the ladder. BODMAS does <b>not</b> mean “all divisions, then all multiplications”. When ÷ and × are next to each other, work them in the order they appear, <b>from left to right</b>. The same is true for + and −.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Expression</th><th>Right (left to right)</th><th>Wrong</th></tr><tr><td class=\"mono\">24 ÷ 4 × 2</td><td class=\"mono\">6 × 2 = 12</td><td class=\"mono\">24 ÷ 8 = 3</td></tr><tr><td class=\"mono\">8 − 3 + 2</td><td class=\"mono\">5 + 2 = 7</td><td class=\"mono\">8 − 5 = 3</td></tr><tr><td class=\"mono\">30 ÷ 5 ÷ 3</td><td class=\"mono\">6 ÷ 3 = 2</td><td class=\"mono\">30 ÷ {5/3} = 18</td></tr></table></div><h4>BODMAS, BIDMAS and PEMDAS are the same rule</h4><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Rung</th><th>BODMAS (India, UK)</th><th>BIDMAS (UK)</th><th>PEMDAS (USA)</th></tr><tr><td>1</td><td>Brackets</td><td>Brackets</td><td>Parentheses</td></tr><tr><td>2</td><td>Of (and orders)</td><td>Indices</td><td>Exponents</td></tr><tr><td>3</td><td>Division, Multiplication</td><td>Division, Multiplication</td><td>Multiplication, Division</td></tr><tr><td>4</td><td>Addition, Subtraction</td><td>Addition, Subtraction</td><td>Addition, Subtraction</td></tr></table></div><p>PEMDAS lists M before D, but they are still the same rank: left to right always decides. BIDMAS and PEMDAS have no letter for “of”; in BODMAS it sits with the orders, before ÷ and ×.</p><h4>“of” comes before ÷ and ×</h4><p><b>“of” means “a fraction (or number) of a quantity”</b>: {1/2} of 18 = {1/2} × 18 = 9, {3/4} of 20 = 15, and 3 of 4 means 3 groups of 4 = 12. Although “of” is a multiplication, it <b>binds tighter</b> than an ordinary ÷ or ×: work out every “of” straight after brackets and powers, as if it had its own bracket.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Expression</th><th>Right (“of” first)</th><th>Wrong (“of” read as × left to right)</th></tr><tr><td class=\"mono\">36 ÷ {1/3} of 12</td><td class=\"mono\">36 ÷ 4 = 9</td><td class=\"mono\">36 × 3 × 12 = 1296</td></tr><tr><td class=\"mono\">60 ÷ 5 of 3</td><td class=\"mono\">60 ÷ 15 = 4</td><td class=\"mono\">12 × 3 = 36</td></tr></table></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · × before +</div><div class=\"exl\">7 + 5 × 4<br>= 7 + 20<br>= <b>27</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · ÷ and × left to right</div><div class=\"exl\">36 ÷ 9 × 2<br>= 4 × 2<br>= <b>8</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · “of” before ÷</div><div class=\"exl\">5 + 36 ÷ {1/3} of 18<br>= 5 + 36 ÷ 6<br>= 5 + 6<br>= <b>11</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 4 · Everything together</div><div class=\"exl\">20 − 12 ÷ 4 + 2 × 3<br>= 20 − 3 + 6<br>= 17 + 6<br>= <b>23</b></div></div><div class=\"keybox\"><b>Common traps.</b> 6 + 4 × 3 is 18, not 30 (× before +). 48 ÷ {1/4} of 16 is 48 ÷ 4 = 12 (“of” before ÷). 24 ÷ 4 × 2 is 12, not 3 (left to right). 8 − 3 + 2 is 7, not 3 (left to right). The letter order D-M or A-S does not mean D always beats M.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s1\">Practise 1 →</button></div></section><section class=\"note\" id=\"n2\"><h2>2 · Three-step expressions</h2><p class=\"lt\"><b>Learning target:</b> I can evaluate expressions with two or three bracket groups, writing one “=” line per step.</p><p>Many expressions have <b>two or three bracket groups</b> joined by other operations. Work them in three steps and write each step on a new line starting with “=”.</p><div class=\"keybox\"><b>Three steps</b><br>1. Work out every bracket (each bracket on its own).<br>2. Do ÷ and × left to right.<br>3. Do + and − left to right.</div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Two brackets, then − and +</div><div class=\"exl\">(8 × 6) − (20 ÷ 5) + 7<br>= 48 − 4 + 7<br>= 44 + 7<br>= <b>51</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · A bracket divided, then +</div><div class=\"exl\">(17 + 7) ÷ 8 + (5 × 3)<br>= 24 ÷ 8 + 15<br>= 3 + 15<br>= <b>18</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Bracket ÷ bracket</div><div class=\"exl\">(45 − 9) ÷ (10 − 6) + 2<br>= 36 ÷ 4 + 2<br>= 9 + 2<br>= <b>11</b></div></div><h4>Writing it out neatly</h4><ul><li>Copy the parts you have not worked on yet, unchanged.</li><li>Change only one rung of the ladder per line.</li><li>Never write a running chain like 36 ÷ 4 = 9 + 2 = 11: the middle “=” is false. Start each line with “=”.</li></ul><div class=\"keybox\"><b>Common mistake.</b> In 27 − 2 + 5 do not add 2 + 5 first. After the brackets, + and − go <b>left to right</b>: 27 − 2 = 25, then 25 + 5 = 30.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s2\">Practise 2 →</button></div></section><section class=\"note\" id=\"n3\"><h2>3 · Brackets within brackets and 'of'</h2><p class=\"lt\"><b>Learning target:</b> I can remove ( ), { } and [ ] from the inside out, work out “of” before ÷ and ×, and work with negative integers.</p><p>When brackets sit inside other brackets, use different shapes so the eye can match them: <b>( )</b> round, <b>{ }</b> curly, <b>[ ]</b> square. Always work from the <b>innermost</b> bracket outwards.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Order</th><th>Bracket</th><th>Name</th></tr><tr><td>1st</td><td class=\"mono\">( )</td><td>round brackets (parentheses)</td></tr><tr><td>2nd</td><td class=\"mono\">{ }</td><td>curly brackets (braces)</td></tr><tr><td>3rd</td><td class=\"mono\">[ ]</td><td>square brackets</td></tr></table></div><p>Some books also use a <b>bar</b> (vinculum) as a bracket: <span class=\"mono\">10 − <span style=\"text-decoration:overline\">6 − 2</span></span> means 10 − (6 − 2) = 6. A bar is worked out first of all.</p><div class=\"ex\"><div class=\"exh\">Worked example 1 · Inside out</div><div class=\"exl\">50 − [14 + {8 − (6 − 3)}]<br>= 50 − [14 + {8 − 3}]<br>= 50 − [14 + 5]<br>= 50 − 19<br>= <b>31</b></div></div><p>Inside brackets the same ladder applies, and “of” is still done before ÷ and ×.</p><div class=\"ex\"><div class=\"exh\">Worked example 2 · “of” inside brackets</div><div class=\"exl\">{1/4} of [30 − (2 × 3)] + 4<br>= {1/4} of [30 − 6] + 4<br>= {1/4} of 24 + 4<br>= 6 + 4<br>= <b>10</b></div></div><h4>Negative integers inside brackets</h4><p>Subtracting a negative is adding: 6 − (−4) = 6 + 4. A negative × or ÷ a positive is negative; two negatives give a positive.</p><div class=\"ex\"><div class=\"exh\">Worked example 3 · Negatives</div><div class=\"exl\">−5 + [9 − (2 − 8)] × 2<br>= −5 + [9 − (−6)] × 2<br>= −5 + 15 × 2<br>= −5 + 30<br>= <b>25</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 4 · Divide by a negative</div><div class=\"exl\">[(−4) × 5 + 8] ÷ (−3)<br>= [−20 + 8] ÷ (−3)<br>= −12 ÷ (−3)<br>= <b>4</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 5 · “of” after ÷</div><div class=\"exl\">90 ÷ {2/3} of 27 + 4<br>= 90 ÷ 18 + 4<br>= 5 + 4<br>= <b>9</b></div></div><div class=\"keybox\"><b>Common mistake.</b> Finish the <b>whole</b> inner bracket before moving out. In 5 × {2 + (6 − 4)}, first 6 − 4 = 2, then {2 + 2} = 4, then 5 × 4 = 20. Writing 5 × 2 + 2 throws the curly bracket away. And never read “÷ {1/2} of” as “× 2 ×”: in 40 ÷ {1/2} of 20, first {1/2} of 20 = 10, then 40 ÷ 10 = 4.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s3\">Practise 3 →</button></div></section><section class=\"note\" id=\"n4\"><h2>4 · Integers and decimals</h2><p class=\"lt\"><b>Learning target:</b> I can apply BODMAS to expressions with negative integers and decimals, and round when asked.</p><p>BODMAS works the same way for decimals and negative numbers. Only the arithmetic inside each step changes.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Skill</th><th>Rule</th><th>Example</th></tr><tr><td>× a decimal</td><td>multiply as whole numbers, then count decimal places</td><td class=\"mono\">2.4 × 5 = 12</td></tr><tr><td>÷ a decimal</td><td>multiply both numbers by 10 (or 100) to make the divisor whole</td><td class=\"mono\">4.8 ÷ 0.6 = 48 ÷ 6 = 8</td></tr><tr><td>signs for × and ÷</td><td>same signs → +, different signs → −</td><td class=\"mono\">−18 ÷ (−3) = 6</td></tr><tr><td>adding a negative</td><td>move left on the number line</td><td class=\"mono\">6 + (−9) = −3</td></tr><tr><td>subtracting a negative</td><td>becomes adding</td><td class=\"mono\">−5 − (−8) = 3</td></tr></table></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Square brackets and a decimal</div><div class=\"exl\">[(16 ÷ 4) + (1.5 × 6)] × (3 − 2.5)<br>= [4 + 9] × 0.5<br>= 13 × 0.5<br>= <b>6.5</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · A negative start</div><div class=\"exl\">−24 ÷ (12 ÷ 3) + 9<br>= −24 ÷ 4 + 9<br>= −6 + 9<br>= <b>3</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Decimals and “of”</div><div class=\"exl\">(6.3 − 2.7) ÷ (0.8 + 0.4) + (0.5 of 8) × 2<br>= 3.6 ÷ 1.2 + 4 × 2<br>= 3 + 8<br>= <b>11</b></div></div><h4>Rounding</h4><p>If a division does not end, keep the exact value until the last step and round only the final answer, e.g. 5 ÷ 3 + 1 = 2.666… ≈ <b>2.67</b> (to 2 decimal places).</p><div class=\"keybox\"><b>Common mistake.</b> Signs do not change the order. In −6 + (−4) × 2, multiply first: (−4) × 2 = −8, then −6 + (−8) = −14. Do not add −6 and −4 first.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s4\">Practise 4 →</button></div></section><section class=\"note\" id=\"n5\"><h2>5 · Powers and roots (BIDMAS)</h2><p class=\"lt\"><b>Learning target:</b> I can work out powers and square roots before × ÷ + −, and tell (−3)² from −3².</p><p>In BIDMAS the <b>I</b> is for <b>indices</b> (powers). Powers and roots come straight after brackets and before × ÷ + −.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Notation</th><th>Meaning</th><th>Value</th></tr><tr><td class=\"mono\">4²</td><td>4 × 4</td><td>16</td></tr><tr><td class=\"mono\">2³</td><td>2 × 2 × 2</td><td>8</td></tr><tr><td class=\"mono\">√81</td><td>the positive number that squares to 81</td><td>9</td></tr><tr><td class=\"mono\">(0.3)²</td><td>0.3 × 0.3</td><td>0.09</td></tr><tr><td class=\"mono\">(−3)²</td><td>(−3) × (−3): the bracket is squared</td><td>9</td></tr><tr><td class=\"mono\">−3²</td><td>−(3 × 3): only the 3 is squared</td><td>−9</td></tr></table></div><p>A square root sign over a sum acts like a bracket: √(9 + 16) = √25 = 5, which is <b>not</b> √9 + √16 = 7.</p><div class=\"ex\"><div class=\"exh\">Worked example 1 · Root, power, then a division</div><div class=\"exl\">√64 + 2³ − (9.6 ÷ 4)<br>= 8 + 8 − 2.4<br>= 16 − 2.4<br>= <b>13.6</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Divide by a negative</div><div class=\"exl\">6² − √49 + (4.5 ÷ (−3))<br>= 36 − 7 + (−1.5)<br>= 29 − 1.5<br>= <b>27.5</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Power of a bracket</div><div class=\"exl\">[12 − (3² − 4)]² ÷ 7<br>= [12 − 5]² ÷ 7<br>= 7² ÷ 7<br>= 49 ÷ 7<br>= <b>7</b></div></div><div class=\"keybox\"><b>Common mistakes.</b> 2 × 3² = 2 × 9 = 18, not 6² = 36: the power belongs only to the 3. −4² = −16 but (−4)² = 16.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s5\">Practise 5 →</button></div></section><section class=\"note\" id=\"n6\"><h2>6 · Word problems</h2><p class=\"lt\"><b>Learning target:</b> I can turn a story into an expression, find each part first, then add or subtract to get the answer.</p><p>A word problem hides an expression. Find the parts first (often products such as “3 tickets at ₹85”), then join them with + or −.</p><div class=\"keybox\"><b>Four steps</b><br>1. Underline each quantity and price.<br>2. Write one product for each kind of item.<br>3. Join the products with + (total) or − (change, what is left).<br>4. Work it out with BODMAS and answer in words with units.</div><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Words</th><th>Operation</th></tr><tr><td>each, per, at … each, of</td><td>×</td></tr><tr><td>share equally, per box, how many groups</td><td>÷</td></tr><tr><td>total, altogether, more</td><td>+</td></tr><tr><td>left, change, fewer, discount</td><td>−</td></tr></table></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Total cost</div><div class=\"exl\">Ishaan buys 4 pencils at ₹12 each and 3 erasers at ₹8 each. Total = 4 × 12 + 3 × 8<br>= 48 + 24<br>= <b>₹72</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Change</div><div class=\"exl\">Zoya has ₹500 and buys 6 samosas at ₹25 each. Left = 500 − 6 × 25<br>= 500 − 150<br>= <b>₹350</b></div></div><div class=\"ex\"><div class=\"exh\">Worked example 3 · Share, then add</div><div class=\"exl\">₹720 is shared equally by 6 children, and each then earns ₹40 more. Each child has 720 ÷ 6 + 40 = 120 + 40<br>= <b>₹160</b></div></div><div class=\"keybox\"><b>Common mistake.</b> Writing 500 − 6 × 25 is correct; working it as (500 − 6) × 25 is not. The brackets in your head must match the story: “6 samosas at ₹25” is one product.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s6\">Practise 6 →</button></div></section><section class=\"note\" id=\"nsum\"><h2>Topic checklist</h2><ul><li>I can say the order B-O-D-M-A-S, do “of” before ÷ and ×, and work ÷ × and + − from left to right.</li><li>I can evaluate expressions with two or three bracket groups, writing one “=” line per step.</li><li>I can remove ( ), { } and [ ] from the inside out, work out “of” before ÷ and ×, and work with negative integers.</li><li>I can apply BODMAS to expressions with negative integers and decimals, and round when asked.</li><li>I can work out powers and square roots before × ÷ + −, and tell (−3)² from −3².</li><li>I can turn a story into an expression, find each part first, then add or subtract to get the answer.</li><li>I start every line of working with “=” and change only one rung per line.</li></ul><div class=\"hub-btns\" style=\"margin-top:10px\"><button class=\"hub-btn\" data-go=\"s7\">Assessment A</button><button class=\"hub-btn\" data-go=\"s8\">Assessment B</button><button class=\"hub-btn\" data-go=\"s9\">Assessment C</button><button class=\"hub-btn\" data-go=\"s10\">Assessment D</button><button class=\"hub-btn primary\" data-go=\"report\">📊 Report</button></div></section>";
var REPORT = [["s7", "A", "Knowing and understanding"], ["s8", "B", "Investigating patterns"], ["s9", "C", "Communicating"], ["s10", "D", "Applying mathematics in real-life contexts"]];
var SECTIONS = [{"id": "s1", "label": "1 · What BODMAS means", "sub": "I can say the order B-O-D-M-A-S, do “of” before ÷ and ×, and work ÷ × and + − from left to right.", "slides": [{"kind": "mcq", "text": "In BODMAS, what does the letter <b>O</b> stand for?", "opts": ["Outside: numbers out of brackets", "Order: always go left to right", "Of (and orders: powers, roots)", "Operations: any working at all"], "correct": 2, "tag": "", "sol": "In BODMAS, O stands for “of” (as in {1/2} of 18), together with orders (powers such as 3² and roots such as √25). It comes straight after Brackets and before ÷ and ×."}, {"kind": "blank", "p": "Complete the meaning of the letters in BODMAS.", "tag": "", "marks": "", "flat": [{"t": "B stands for __B1__", "a": {"B1": "brackets"}, "expr": "words", "accept": ["bracket"]}, {"t": "D stands for __B1__", "a": {"B1": "division"}, "expr": "words", "accept": ["divide", "dividing"]}, {"t": "S stands for __B1__", "a": {"B1": "subtraction"}, "expr": "words", "accept": ["subtract", "subtracting", "minus"]}], "sol": "B = Brackets: always first.\nD = Division: same rank as Multiplication.\nS = Subtraction: same rank as Addition, done last."}, {"kind": "mcq", "text": "<b>Which step comes first?</b> 18 − 2 × 5", "opts": ["Either; the answer is the same", "18 − 2", "2 × 5", "18 − 5"], "correct": 2, "tag": "", "sol": "× comes before −. 2 × 5 = 10, then 18 − 10 = 8. (Doing 18 − 2 first gives 16 × 5 = 80, which is wrong.)"}, {"kind": "blank", "p": "Find each value.", "tag": "", "marks": "", "flat": [{"t": "a) 6 + 4 × 3 = __B1__", "a": {"B1": "18"}}, {"t": "b) 20 − 12 ÷ 4 = __B1__", "a": {"B1": "17"}}, {"t": "c) 5 × 2 + 3 × 4 = __B1__", "a": {"B1": "22"}}], "sol": "4 × 3 = 12, then 6 + 12 = 18.\n12 ÷ 4 = 3, then 20 − 3 = 17.\n5 × 2 = 10 and 3 × 4 = 12, then 10 + 12 = 22."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Riya says 6 + 4 × 3 = 30. What went wrong?", "opts": ["She should do 6 × 3 first, so it is 22", "Nothing; working left to right gives 30", "She should do 6 × 4 first, so it is 27", "She added first; do 4 × 3 first, so it is 18"], "correct": 3, "tag": "", "sol": "× is on a higher rung than +, so 4 × 3 = 12 comes first: 6 + 12 = 18. Riya worked (6 + 4) × 3 = 30, as if there were brackets."}, {"kind": "blank", "p": "“of” means a fraction of (×) and is done before ÷ and ×. Find each value.", "tag": "", "marks": "", "flat": [{"t": "a) {1/3} of 27 = __B1__", "a": {"B1": "9"}}, {"t": "b) 4 + {1/2} of 16 = __B1__", "a": {"B1": "12"}}, {"t": "c) 30 − {1/5} of 25 = __B1__", "a": {"B1": "25"}}], "sol": "{1/3} × 27 = 9.\n“of” first: {1/2} × 16 = 8, then 4 + 8 = 12.\n“of” first: {1/5} × 25 = 5, then 30 − 5 = 25."}, {"kind": "mcq", "text": "What is 24 ÷ 4 × 2?", "opts": ["48", "12", "3", "6"], "correct": 1, "tag": "", "sol": "÷ and × have the same rank, so go left to right: 24 ÷ 4 = 6, then 6 × 2 = 12. (3 comes from doing 4 × 2 first; 6 forgets the × 2.)"}, {"kind": "blank", "p": "+ and − (and also ÷ and ×) are worked from left to right. Find each value.", "tag": "", "marks": "", "flat": [{"t": "a) 8 − 3 + 2 = __B1__", "a": {"B1": "7"}}, {"t": "b) 36 ÷ 6 × 3 = __B1__", "a": {"B1": "18"}}, {"t": "c) 15 − 6 − 4 = __B1__", "a": {"B1": "5"}}], "sol": "8 − 3 = 5, then 5 + 2 = 7 (not 8 − 5 = 3).\n36 ÷ 6 = 6, then 6 × 3 = 18 (not 36 ÷ 18 = 2).\n15 − 6 = 9, then 9 − 4 = 5 (not 15 − 2 = 13)."}, {"kind": "mcq", "text": "<b>Which step comes first?</b> 40 ÷ (2 + 3) × 4", "opts": ["2 + 3", "40 ÷ 2", "40 ÷ 4", "3 × 4"], "correct": 0, "tag": "", "sol": "Brackets come first: 2 + 3 = 5. Then left to right: 40 ÷ 5 = 8 and 8 × 4 = 32."}, {"kind": "blank", "p": "Work out 40 ÷ (2 + 3) × 4 one step at a time.", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (2 + 3) = __B1__", "a": {"B1": "5"}}, {"t": "Step 2 — left to right: 40 ÷ 5 = __B1__", "a": {"B1": "8"}}, {"t": "Answer: 8 × 4 = __B1__", "a": {"B1": "32"}}], "sol": "2 + 3 = 5.\n40 ÷ 5 = 8 (÷ is on the left, so it goes first).\n8 × 4 = 32."}, {"kind": "mcq", "text": "Which expression has the greatest value?", "opts": ["5 + 3 + 2", "5 × 3 − 2", "(5 + 3) × 2", "5 + 3 × 2"], "correct": 2, "tag": "", "sol": "(5 + 3) × 2 = 8 × 2 = 16. The others: 5 + 6 = 11, 15 − 2 = 13 and 5 + 3 + 2 = 10."}, {"kind": "blank", "p": "Which operation is done <b>first</b>? Type +, -, x or /.", "tag": "", "marks": "", "flat": [{"t": "a) 9 + 12 ÷ 3 → __B1__", "a": {"B1": "÷"}, "accept": ["/"]}, {"t": "b) 7 × 2 − 5 → __B1__", "a": {"B1": "×"}, "accept": ["x", "*", "X"]}, {"t": "c) (10 − 4) × 3 → __B1__", "a": {"B1": "−"}, "accept": ["-"]}], "sol": "÷ before +: 12 ÷ 3 = 4, then 9 + 4 = 13.\n× before −: 14 − 5 = 9.\nBrackets first: 10 − 4 = 6, then 6 × 3 = 18."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Arjun works out 30 − 10 + 5 as 30 − 15 = 15. What is the correct value?", "opts": ["25, because + and − go left to right", "45, because you add all three numbers", "15, because A comes before S in BODMAS", "35, because you always subtract last"], "correct": 0, "tag": "", "sol": "+ and − are on the same rung, so go left to right: 30 − 10 = 20, then 20 + 5 = 25. Arjun added 10 + 5 first."}, {"kind": "blank", "p": "Find each value. Use the order ladder.", "tag": "", "marks": "", "flat": [{"t": "a) 7 + 3 × 6 − 4 = __B1__", "a": {"B1": "21"}}, {"t": "b) 50 − 4 × 5 ÷ 2 = __B1__", "a": {"B1": "40"}}, {"t": "c) 2 × 9 ÷ 3 + 6 = __B1__", "a": {"B1": "12"}}], "sol": "3 × 6 = 18; then 7 + 18 − 4 = 25 − 4 = 21.\n4 × 5 = 20, 20 ÷ 2 = 10; then 50 − 10 = 40.\n2 × 9 = 18, 18 ÷ 3 = 6; then 6 + 6 = 12."}]}, {"id": "s2", "label": "2 · Three-step expressions", "sub": "I can evaluate expressions with two or three bracket groups, writing one “=” line per step.", "slides": [{"kind": "mcq", "text": "What is (6 × 5) − (24 ÷ 8) + 4?", "opts": ["27", "31", "23", "37"], "correct": 1, "tag": "", "sol": "= 30 − 3 + 4 = 27 + 4 = 31. (23 comes from adding 3 + 4 first; 27 forgets the + 4; 37 adds the 3.)"}, {"kind": "blank", "p": "Work out (7 × 4) − (18 ÷ 6) + 9.", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (7 × 4) = __B1__ and (18 ÷ 6) = __B2__", "a": {"B1": "28", "B2": "3"}}, {"t": "Step 2 — left to right: 28 − 3 = __B1__", "a": {"B1": "25"}}, {"t": "Answer = __B1__", "a": {"B1": "34"}}], "sol": "7 × 4 = 28 and 18 ÷ 6 = 3, so the expression becomes 28 − 3 + 9.\n= 25 + 9 (left to right: − comes first here).\n= 34."}, {"kind": "mcq", "text": "What is the correct first line of working for (15 + 9) ÷ 6 + (3 × 7)?", "opts": ["= 24 ÷ 27", "= 24 ÷ 6 + 21", "= 24 ÷ 6 + 10", "= 15 + 1.5 + 21"], "correct": 1, "tag": "", "sol": "Work out both brackets and copy everything else: (15 + 9) = 24 and (3 × 7) = 21, so = 24 ÷ 6 + 21 = 4 + 21 = 25. (“24 ÷ 27” adds before dividing.)"}, {"kind": "blank", "p": "Work out (15 + 9) ÷ 6 + (3 × 7).", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (15 + 9) = __B1__ and (3 × 7) = __B2__", "a": {"B1": "24", "B2": "21"}}, {"t": "Step 2 — divide: 24 ÷ 6 = __B1__", "a": {"B1": "4"}}, {"t": "Answer = __B1__", "a": {"B1": "25"}}], "sol": "15 + 9 = 24 and 3 × 7 = 21: = 24 ÷ 6 + 21.\n÷ before +: = 4 + 21.\n= 25."}, {"kind": "mcq", "text": "What is (20 − 8) ÷ (7 − 4) + 6?", "opts": ["18", "12", "10", "4"], "correct": 2, "tag": "", "sol": "= 12 ÷ 3 + 6 = 4 + 6 = 10. (4 forgets the + 6; 18 is 12 + 6 without dividing.)"}, {"kind": "blank", "p": "Work out (36 − 12) ÷ (11 − 5) + 8.", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (36 − 12) = __B1__ and (11 − 5) = __B2__", "a": {"B1": "24", "B2": "6"}}, {"t": "Step 2 — divide: 24 ÷ 6 = __B1__", "a": {"B1": "4"}}, {"t": "Answer = __B1__", "a": {"B1": "12"}}], "sol": "36 − 12 = 24 and 11 − 5 = 6: = 24 ÷ 6 + 8.\n= 4 + 8.\n= 12."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Kabir worked out (9 × 3) − (14 ÷ 7) + 5 as = 27 − 2 + 5 = 27 − 7 = 20. What went wrong?", "opts": ["He should do 14 ÷ 7 last; it is 27", "He did 2 + 5 first; left to right gives 30", "Nothing is wrong; the answer is 20", "He should add all three; it is 34"], "correct": 1, "tag": "", "sol": "After the brackets, − and + are on the same rung, so go left to right: 27 − 2 = 25, then 25 + 5 = 30."}, {"kind": "blank", "p": "Work out (4 × 9) + (40 ÷ 5) − 7.", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (4 × 9) = __B1__ and (40 ÷ 5) = __B2__", "a": {"B1": "36", "B2": "8"}}, {"t": "Step 2 — left to right: 36 + 8 = __B1__", "a": {"B1": "44"}}, {"t": "Answer = __B1__", "a": {"B1": "37"}}], "sol": "4 × 9 = 36 and 40 ÷ 5 = 8: = 36 + 8 − 7.\n= 44 − 7.\n= 37."}, {"kind": "mcq", "text": "What is (9 + 7) ÷ 4 × (6 − 1)?", "opts": ["20", "80", "4", "0.8"], "correct": 0, "tag": "", "sol": "= 16 ÷ 4 × 5. ÷ and × go left to right: 16 ÷ 4 = 4, then 4 × 5 = 20. (0.8 comes from 16 ÷ 20, doing × first; 80 is 16 × 5.)"}, {"kind": "blank", "p": "Work out (48 ÷ 6) × (2 + 3) − 10.", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (48 ÷ 6) = __B1__ and (2 + 3) = __B2__", "a": {"B1": "8", "B2": "5"}}, {"t": "Step 2 — multiply: 8 × 5 = __B1__", "a": {"B1": "40"}}, {"t": "Answer = __B1__", "a": {"B1": "30"}}], "sol": "48 ÷ 6 = 8 and 2 + 3 = 5: = 8 × 5 − 10.\n× before −: = 40 − 10.\n= 30."}, {"kind": "mcq", "text": "<b>Spot the error.</b> (25 − 5) ÷ 4 + (2 × 6)  Line 1: = 20 ÷ 4 + 12   Line 2: = 20 ÷ 16   Line 3: = 1.25. Which line is wrong?", "opts": ["No line is wrong; the answer is 1.25", "Line 3: 20 ÷ 16 is not 1.25", "Line 2: 20 ÷ 4 must be done before + 12", "Line 1: the brackets were worked out wrongly"], "correct": 2, "tag": "", "sol": "Line 1 is right. In Line 2 the + was done before ÷. Correct: = 20 ÷ 4 + 12 = 5 + 12 = 17."}, {"kind": "blank", "p": "Work out (14 + 6) ÷ (9 − 4) + (3 × 8).", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (14 + 6) = __B1__, (9 − 4) = __B2__, (3 × 8) = __B3__", "a": {"B1": "20", "B2": "5", "B3": "24"}}, {"t": "Step 2 — divide: 20 ÷ 5 = __B1__", "a": {"B1": "4"}}, {"t": "Answer = __B1__", "a": {"B1": "28"}}], "sol": "Three brackets: 20, 5 and 24, so = 20 ÷ 5 + 24.\n= 4 + 24.\n= 28."}, {"kind": "mcq", "text": "What is (50 − 14) ÷ (3 × 3) − 2?", "opts": ["4", "2", "34", "6"], "correct": 1, "tag": "", "sol": "= 36 ÷ 9 − 2 = 4 − 2 = 2. (4 forgets the − 2; 34 is 36 − 2 without dividing; 6 adds the 2.)"}, {"kind": "blank", "p": "Work out (8 × 7) − (63 ÷ 9) − (5 × 4).", "tag": "", "marks": "", "flat": [{"t": "Step 1 — brackets: (8 × 7) = __B1__, (63 ÷ 9) = __B2__, (5 × 4) = __B3__", "a": {"B1": "56", "B2": "7", "B3": "20"}}, {"t": "Step 2 — left to right: 56 − 7 = __B1__", "a": {"B1": "49"}}, {"t": "Answer = __B1__", "a": {"B1": "29"}}], "sol": "= 56 − 7 − 20.\n= 49 − 20 (not 56 − 13: subtract from left to right).\n= 29."}]}, {"id": "s3", "label": "3 · Brackets within brackets and 'of'", "sub": "I can remove ( ), { } and [ ] from the inside out, work out “of” before ÷ and ×, and work with negative integers.", "slides": [{"kind": "mcq", "text": "In what order do you remove the brackets in 40 − [12 + {9 − (5 − 2)}]?", "opts": ["Any order gives the same answer", "( ) first, then { }, then [ ]", "{ } first, then ( ), then [ ]", "[ ] first, then { }, then ( )"], "correct": 1, "tag": "", "sol": "Work from the inside out: (5 − 2) = 3, then {9 − 3} = 6, then [12 + 6] = 18, and 40 − 18 = 22."}, {"kind": "blank", "p": "Work out 40 − [12 + {9 − (5 − 2)}] from the inside out.", "tag": "", "marks": "", "flat": [{"t": "Round brackets: (5 − 2) = __B1__", "a": {"B1": "3"}}, {"t": "Curly brackets: {9 − 3} = __B1__", "a": {"B1": "6"}}, {"t": "Square brackets: [12 + 6] = __B1__", "a": {"B1": "18"}}, {"t": "Answer: 40 − 18 = __B1__", "a": {"B1": "22"}}], "sol": "= 40 − [12 + {9 − 3}]\n= 40 − [12 + 6]\n= 40 − 18\n= 22"}, {"kind": "mcq", "text": "Simplify 3 × [8 − (2 + 1)].", "opts": ["27", "21", "5", "15"], "correct": 3, "tag": "", "sol": "(2 + 1) = 3, then [8 − 3] = 5, then 3 × 5 = 15. (21 comes from 3 × 8 − 3, dropping the square bracket.)"}, {"kind": "blank", "p": "Find each value. Remember: “of” is done before ÷ and ×.", "tag": "", "marks": "", "flat": [{"t": "a) {1/4} of (30 − 6) = __B1__", "a": {"B1": "6"}}, {"t": "b) 2 × [{1/3} of 21] + 5 = __B1__", "a": {"B1": "19"}}, {"t": "c) 50 − {1/2} of 40 = __B1__", "a": {"B1": "30"}}], "sol": "(30 − 6) = 24, and {1/4} of 24 = 6.\n{1/3} of 21 = 7, 2 × 7 = 14, 14 + 5 = 19.\n“of” before −: {1/2} of 40 = 20, then 50 − 20 = 30."}, {"kind": "mcq", "text": "What is 48 ÷ {1/4} of 16?", "opts": ["0.75", "3072", "192", "12"], "correct": 3, "tag": "", "sol": "“of” comes before ÷: {1/4} of 16 = 4, then 48 ÷ 4 = 12. (3072 reads it as 48 ÷ {1/4} × 16 = 192 × 16; 0.75 is 48 ÷ 4 ÷ 16.)"}, {"kind": "blank", "p": "Work out 60 ÷ 3 of 4 + 5.", "tag": "", "marks": "", "flat": [{"t": "“of” first: 3 of 4 = __B1__", "a": {"B1": "12"}}, {"t": "Divide: 60 ÷ 12 = __B1__", "a": {"B1": "5"}}, {"t": "Answer = __B1__", "a": {"B1": "10"}}], "sol": "3 of 4 = 3 × 4 = 12 (“of” before ÷).\n= 60 ÷ 12 + 5 = 5 + 5\n= 10 (not 60 ÷ 3 × 4 + 5 = 85)."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Kiran works out 40 ÷ {1/2} of 20 as 40 × 2 × 20 = 1600. What is correct?", "opts": ["80, because 40 ÷ {1/2} is worked first", "4, because “of” is done before ÷", "1600, because ÷ {1/2} is the same as × 2", "1, because it is 40 ÷ 20 ÷ 2"], "correct": 1, "tag": "", "sol": "“of” binds tighter than ÷: {1/2} of 20 = 10, then 40 ÷ 10 = 4. Kiran treated “of” as an ordinary × worked left to right."}, {"kind": "blank", "p": "Work out 72 ÷ {3/4} of 8 − 2 of 5.", "tag": "", "marks": "", "flat": [{"t": "{3/4} of 8 = __B1__", "a": {"B1": "6"}}, {"t": "2 of 5 = __B1__", "a": {"B1": "10"}}, {"t": "72 ÷ 6 = __B1__", "a": {"B1": "12"}}, {"t": "Answer = __B1__", "a": {"B1": "2"}}], "sol": "Every “of” first: {3/4} × 8 = 6.\n2 × 5 = 10, so = 72 ÷ 6 − 10.\n= 12 − 10\n= 2"}, {"kind": "mcq", "text": "<b>Error analysis.</b> Meera worked out 5 × {2 + (6 − 4)} and wrote 5 × 2 + 2 = 12. What is correct?", "opts": ["20: finish the whole { } first, so 5 × 4", "14: it should be 5 × 2 + 4", "40: it should be 5 × 2 × 4", "12: her working is correct"], "correct": 0, "tag": "", "sol": "(6 − 4) = 2, then {2 + 2} = 4, then 5 × 4 = 20. Meera dropped the curly bracket, so the 2 + 2 was no longer done first."}, {"kind": "blank", "p": "Work out [(−6) × 3 + 20] ÷ (−2).", "tag": "", "marks": "", "flat": [{"t": "Inside [ ], multiply first: (−6) × 3 = __B1__", "a": {"B1": "-18"}}, {"t": "[−18 + 20] = __B1__", "a": {"B1": "2"}}, {"t": "Answer: 2 ÷ (−2) = __B1__", "a": {"B1": "-1"}}], "sol": "Different signs: −18.\n−18 + 20 = 2.\nDifferent signs: 2 ÷ (−2) = −1."}, {"kind": "mcq", "text": "What is 7 − [−3 − (4 − 9)]?", "opts": ["9", "−1", "5", "19"], "correct": 2, "tag": "", "sol": "(4 − 9) = −5. [−3 − (−5)] = −3 + 5 = 2. 7 − 2 = 5. (9 comes from a sign slip, [−3 − (−5)] = −2, giving 7 − (−2) = 9.)"}, {"kind": "blank", "p": "Work out {1/2} of [26 − (4 × 3)] + 9.", "tag": "", "marks": "", "flat": [{"t": "Round brackets: (4 × 3) = __B1__", "a": {"B1": "12"}}, {"t": "Square brackets: [26 − 12] = __B1__", "a": {"B1": "14"}}, {"t": "“of”: {1/2} of 14 = __B1__", "a": {"B1": "7"}}, {"t": "Answer = __B1__", "a": {"B1": "16"}}], "sol": "= {1/2} of [26 − 12] + 9\n= {1/2} of 14 + 9\n= 7 + 9\n= 16"}, {"kind": "mcq", "text": "Which expression equals 2?", "opts": ["24 ÷ 4 × 5 − 2", "(24 ÷ 4) × 5 − 2", "24 ÷ [4 × (5 − 2)]", "24 ÷ 4 × (5 − 2)"], "correct": 2, "tag": "", "sol": "24 ÷ [4 × 3] = 24 ÷ 12 = 2. Without the square bracket, 24 ÷ 4 × 3 = 18; the last two both give 28."}, {"kind": "blank", "p": "Work out 100 − [45 − {3 × (8 − 5)}].", "tag": "", "marks": "", "flat": [{"t": "(8 − 5) = __B1__", "a": {"B1": "3"}}, {"t": "{3 × 3} = __B1__", "a": {"B1": "9"}}, {"t": "[45 − 9] = __B1__", "a": {"B1": "36"}}, {"t": "Answer = __B1__", "a": {"B1": "64"}}], "sol": "= 100 − [45 − {3 × 3}]\n= 100 − [45 − 9]\n= 100 − 36\n= 64"}, {"kind": "mcq", "text": "What is (−12) ÷ [2 × (−3)] + {1/3} of 15?", "opts": ["−7", "3", "−3", "7"], "correct": 3, "tag": "", "sol": "[2 × (−3)] = −6. (−12) ÷ (−6) = 2 (same signs). {1/3} of 15 = 5. 2 + 5 = 7. (3 comes from −2 + 5, a sign slip.)"}, {"kind": "blank", "p": "Work out [{1/4} of 36 − (−3)] × (−2).", "tag": "", "marks": "", "flat": [{"t": "“of”: {1/4} of 36 = __B1__", "a": {"B1": "9"}}, {"t": "[9 − (−3)] = __B1__", "a": {"B1": "12"}}, {"t": "Answer = __B1__", "a": {"B1": "-24"}}], "sol": "{1/4} × 36 = 9: = [9 − (−3)] × (−2).\nSubtracting −3 is adding 3: = 12 × (−2).\n= −24."}]}, {"id": "s4", "label": "4 · Integers and decimals", "sub": "I can apply BODMAS to expressions with negative integers and decimals, and round when asked.", "slides": [{"kind": "mcq", "text": "What is 2.5 + 1.5 × 4?", "opts": ["10", "8.5", "7.5", "16"], "correct": 1, "tag": "", "sol": "× first: 1.5 × 4 = 6. Then 2.5 + 6 = 8.5. (16 comes from (2.5 + 1.5) × 4.)"}, {"kind": "blank", "p": "Work out [(21 ÷ 3) + (2.5 × 4)] × (5 − 1.8).", "tag": "", "marks": "", "flat": [{"t": "Round brackets: (21 ÷ 3) = __B1__ and (2.5 × 4) = __B2__", "a": {"B1": "7", "B2": "10"}}, {"t": "Square bracket: [7 + 10] = __B1__", "a": {"B1": "17"}}, {"t": "(5 − 1.8) = __B1__", "a": {"B1": "3.2"}}, {"t": "Answer: 17 × 3.2 = __B1__", "a": {"B1": "54.4"}}], "sol": "= [7 + 10] × (5 − 1.8)\n= 17 × (5 − 1.8)\n= 17 × 3.2\n= 54.4"}, {"kind": "mcq", "text": "What is −20 ÷ (10 ÷ 2) + 7?", "opts": ["−11", "3", "6", "−4"], "correct": 1, "tag": "", "sol": "(10 ÷ 2) = 5. −20 ÷ 5 = −4. −4 + 7 = 3. (6 comes from ignoring the bracket: −20 ÷ 10 ÷ 2 + 7; −4 forgets the + 7.)"}, {"kind": "blank", "p": "Work out (7.2 − 3.6) ÷ (0.5 + 0.4) + (0.5 of 6) × 3.", "tag": "", "marks": "", "flat": [{"t": "Brackets: (7.2 − 3.6) = __B1__, (0.5 + 0.4) = __B2__, (0.5 of 6) = __B3__", "a": {"B1": "3.6", "B2": "0.9", "B3": "3"}}, {"t": "Divide: 3.6 ÷ 0.9 = __B1__", "a": {"B1": "4"}}, {"t": "Multiply: 3 × 3 = __B1__", "a": {"B1": "9"}}, {"t": "Answer = __B1__", "a": {"B1": "13"}}], "sol": "= 3.6 ÷ 0.9 + 3 × 3\n3.6 ÷ 0.9 = 36 ÷ 9 = 4.\n3 × 3 = 9, so = 4 + 9.\n= 13"}, {"kind": "mcq", "text": "What is −3 × 4 + (−2) × (−5)?", "opts": ["2", "70", "−2", "−22"], "correct": 2, "tag": "", "sol": "Both products first: −3 × 4 = −12 and (−2) × (−5) = 10. −12 + 10 = −2. (−22 uses −10 instead of +10; 70 works left to right.)"}, {"kind": "blank", "p": "Work out 1.2 × 5 − 0.8 ÷ 0.2.", "tag": "", "marks": "", "flat": [{"t": "1.2 × 5 = __B1__", "a": {"B1": "6"}}, {"t": "0.8 ÷ 0.2 = __B1__", "a": {"B1": "4"}}, {"t": "Answer = __B1__", "a": {"B1": "2"}}], "sol": "12 × 5 = 60, one decimal place: 6.\n8 ÷ 2 = 4 (multiply both by 10).\n= 6 − 4 = 2."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Dev says (−6) + (−4) × 2 = −20. What went wrong?", "opts": ["He added first; do (−4) × 2 first, so −14", "Two negatives make a positive, so 20", "He should do (−6) × 2 first, so −16", "Nothing is wrong; the answer is −20"], "correct": 0, "tag": "", "sol": "× before +: (−4) × 2 = −8, then −6 + (−8) = −14. Dev worked (−6 + −4) × 2 = −20."}, {"kind": "blank", "p": "Work out (−15) ÷ 3 + 2.4 × (−5) − (−8).", "tag": "", "marks": "", "flat": [{"t": "(−15) ÷ 3 = __B1__", "a": {"B1": "-5"}}, {"t": "2.4 × (−5) = __B1__", "a": {"B1": "-12"}}, {"t": "Answer = __B1__", "a": {"B1": "-9"}}], "sol": "Different signs: −5.\n24 × 5 = 120 → 12, different signs: −12.\n= −5 − 12 + 8 = −17 + 8 = −9."}, {"kind": "mcq", "text": "What is 4.8 ÷ 0.6 − 1.5 × 2?", "opts": ["13", "5", "3", "11"], "correct": 1, "tag": "", "sol": "4.8 ÷ 0.6 = 8 and 1.5 × 2 = 3. 8 − 3 = 5. (13 comes from working left to right: 8 − 1.5 = 6.5, 6.5 × 2 = 13; 11 adds 8 + 3.)"}, {"kind": "blank", "p": "Work out (10 − 2.5) ÷ (1.2 + 1.8) + 7 ÷ 3. Give the answer to 2 decimal places.", "tag": "", "marks": "", "flat": [{"t": "Brackets: (10 − 2.5) = __B1__ and (1.2 + 1.8) = __B2__", "a": {"B1": "7.5", "B2": "3"}}, {"t": "7.5 ÷ 3 = __B1__", "a": {"B1": "2.5"}}, {"t": "Answer (to 2 decimal places) = __B1__", "a": {"B1": "4.83"}, "accept": ["4.833", "4.8333", "4.83333", "4.833333"]}], "sol": "= 7.5 ÷ 3 + 7 ÷ 3\n7.5 ÷ 3 = 2.5, and 7 ÷ 3 = 2.333…\n= 2.5 + 2.333… = 4.8333… ≈ 4.83"}, {"kind": "mcq", "text": "Which expression has a <b>negative</b> value?", "opts": ["2.5 × 3 − 1.5", "3 − 1.5 ÷ 2.5", "(2.5 − 1.5) × 3", "2.5 − 1.5 × 3"], "correct": 3, "tag": "", "sol": "2.5 − 4.5 = −2. The others: 1 × 3 = 3, 7.5 − 1.5 = 6, 3 − 0.6 = 2.4."}, {"kind": "blank", "p": "Work out −18 ÷ (−3) × (−2) + 0.25 × 8.", "tag": "", "marks": "", "flat": [{"t": "Left to right: −18 ÷ (−3) = __B1__", "a": {"B1": "6"}}, {"t": "6 × (−2) = __B1__", "a": {"B1": "-12"}}, {"t": "Answer = __B1__", "a": {"B1": "-10"}}], "sol": "Same signs: +6.\nDifferent signs: −12.\n0.25 × 8 = 2, so = −12 + 2 = −10."}, {"kind": "mcq", "text": "What is [(−4) × 2.5 + 16] ÷ (−0.5)?", "opts": ["−3", "−12", "−42", "12"], "correct": 1, "tag": "", "sol": "(−4) × 2.5 = −10; [−10 + 16] = 6; 6 ÷ (−0.5) = −12. (−3 multiplies by 0.5 instead of dividing; −42 ignores the square bracket.)"}, {"kind": "blank", "p": "Work out 5.6 ÷ (−0.7) + 3 × (1.5 − 4).", "tag": "", "marks": "", "flat": [{"t": "Bracket: (1.5 − 4) = __B1__", "a": {"B1": "-2.5"}}, {"t": "5.6 ÷ (−0.7) = __B1__", "a": {"B1": "-8"}}, {"t": "3 × (−2.5) = __B1__", "a": {"B1": "-7.5"}}, {"t": "Answer = __B1__", "a": {"B1": "-15.5"}}], "sol": "= 5.6 ÷ (−0.7) + 3 × (−2.5)\n56 ÷ 7 = 8, different signs: −8.\n= −8 + (−7.5)\n= −15.5"}]}, {"id": "s5", "label": "5 · Powers and roots (BIDMAS)", "sub": "I can work out powers and square roots before × ÷ + −, and tell (−3)² from −3².", "slides": [{"kind": "mcq", "text": "What is 3 + 2² × 5?", "opts": ["23", "35", "25", "100"], "correct": 0, "tag": "", "sol": "Power first: 2² = 4. Then × : 4 × 5 = 20. Then 3 + 20 = 23. (35 is (3 + 4) × 5; 100 is (2 × 5)².)"}, {"kind": "blank", "p": "Work out √49 + 3² − (8.4 ÷ 4).", "tag": "", "marks": "", "flat": [{"t": "√49 = __B1__", "a": {"B1": "7"}}, {"t": "3² = __B1__", "a": {"B1": "9"}}, {"t": "(8.4 ÷ 4) = __B1__", "a": {"B1": "2.1"}}, {"t": "Answer = __B1__", "a": {"B1": "13.9"}}], "sol": "7 × 7 = 49.\n3 × 3 = 9.\n84 ÷ 4 = 21 → 2.1 (bracket worked out first).\n= 7 + 9 − 2.1 = 16 − 2.1 = 13.9"}, {"kind": "mcq", "text": "Which statement is true?", "opts": ["(−3)² = 9 and −3² = −9", "(−3)² = 9 and −3² = 9", "(−3)² = −9 and −3² = 9", "(−3)² = −9 and −3² = −9"], "correct": 0, "tag": "", "sol": "(−3)² = (−3) × (−3) = 9. In −3² only the 3 is squared: −(3 × 3) = −9."}, {"kind": "blank", "p": "Work out 5² − √64 + (7.5 ÷ (−3)).", "tag": "", "marks": "", "flat": [{"t": "5² = __B1__", "a": {"B1": "25"}}, {"t": "√64 = __B1__", "a": {"B1": "8"}}, {"t": "(7.5 ÷ (−3)) = __B1__", "a": {"B1": "-2.5"}}, {"t": "Answer = __B1__", "a": {"B1": "14.5"}}], "sol": "5 × 5 = 25.\n8 × 8 = 64.\n75 ÷ 3 = 25 → 2.5, different signs: −2.5.\n= 25 − 8 + (−2.5) = 17 − 2.5 = 14.5"}, {"kind": "mcq", "text": "What is 2³ + 4 × 3?", "opts": ["32", "18", "20", "36"], "correct": 2, "tag": "", "sol": "2³ = 2 × 2 × 2 = 8. 4 × 3 = 12. 8 + 12 = 20. (36 is (8 + 4) × 3; 18 uses 2³ = 6.)"}, {"kind": "blank", "p": "Find each value.", "tag": "", "marks": "", "flat": [{"t": "a) (−2)⁴ = __B1__", "a": {"B1": "16"}}, {"t": "b) −2⁴ = __B1__", "a": {"B1": "-16"}}, {"t": "c) (−5)² − 5² = __B1__", "a": {"B1": "0"}}], "sol": "(−2) × (−2) × (−2) × (−2) = 16 (an even number of negatives).\nOnly the 2 is raised to the power: −(2 × 2 × 2 × 2) = −16.\n25 − 25 = 0."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Sana works out 2 × 3² as 6² = 36. What went wrong?", "opts": ["She should square both: 2² × 3² = 36", "Powers come first: 3² = 9, so 2 × 9 = 18", "Nothing; 2 × 3 = 6 and 6² = 36", "She should add instead: 2 + 9 = 11"], "correct": 1, "tag": "", "sol": "The power belongs only to the 3. O (orders) comes before M, so 3² = 9 first, then 2 × 9 = 18."}, {"kind": "blank", "p": "Work out √81 ÷ 3 + 4² × 0.5.", "tag": "", "marks": "", "flat": [{"t": "√81 = __B1__", "a": {"B1": "9"}}, {"t": "9 ÷ 3 = __B1__", "a": {"B1": "3"}}, {"t": "4² × 0.5 = __B1__", "a": {"B1": "8"}}, {"t": "Answer = __B1__", "a": {"B1": "11"}}], "sol": "9 × 9 = 81.\nDivision: 3.\n16 × 0.5 = 8.\n= 3 + 8 = 11"}, {"kind": "mcq", "text": "What is √(36 + 64)?", "opts": ["10", "50", "100", "14"], "correct": 0, "tag": "", "sol": "The root covers the whole sum, like a bracket: 36 + 64 = 100 and √100 = 10. (14 is √36 + √64 = 6 + 8, which is not the same.)"}, {"kind": "blank", "p": "Work out (1.5)² + √144 ÷ (−4).", "tag": "", "marks": "", "flat": [{"t": "(1.5)² = __B1__", "a": {"B1": "2.25"}}, {"t": "√144 = __B1__", "a": {"B1": "12"}}, {"t": "12 ÷ (−4) = __B1__", "a": {"B1": "-3"}}, {"t": "Answer = __B1__", "a": {"B1": "-0.75"}}], "sol": "1.5 × 1.5 = 2.25.\n12 × 12 = 144.\nDifferent signs: −3.\n= 2.25 + (−3) = −0.75"}, {"kind": "mcq", "text": "Which expression has the greatest value?", "opts": ["2² + 3", "2 + 3²", "(2 + 3)²", "2² + 3²"], "correct": 2, "tag": "", "sol": "(2 + 3)² = 5² = 25. The others: 4 + 9 = 13, 2 + 9 = 11, 4 + 3 = 7."}, {"kind": "blank", "p": "Work out [10 − (2² + 1)]² ÷ 5.", "tag": "", "marks": "", "flat": [{"t": "(2² + 1) = __B1__", "a": {"B1": "5"}}, {"t": "[10 − 5] = __B1__", "a": {"B1": "5"}}, {"t": "5² = __B1__", "a": {"B1": "25"}}, {"t": "Answer = __B1__", "a": {"B1": "5"}}], "sol": "Inside ( ): power first, 4 + 1 = 5.\n10 − 5 = 5.\nThe square bracket is squared: 5 × 5 = 25.\n= 25 ÷ 5 = 5"}, {"kind": "mcq", "text": "What is −4² + (−4)²?", "opts": ["16", "−32", "32", "0"], "correct": 3, "tag": "", "sol": "−4² = −16 and (−4)² = 16. −16 + 16 = 0. (32 treats both as 16; −32 treats both as −16.)"}, {"kind": "blank", "p": "Work out 6² ÷ (−9) − √25 × 0.4 + 2³.", "tag": "", "marks": "", "flat": [{"t": "6² ÷ (−9) = __B1__", "a": {"B1": "-4"}}, {"t": "√25 × 0.4 = __B1__", "a": {"B1": "2"}}, {"t": "2³ = __B1__", "a": {"B1": "8"}}, {"t": "Answer = __B1__", "a": {"B1": "2"}}], "sol": "36 ÷ (−9) = −4.\n5 × 0.4 = 2.\n2 × 2 × 2 = 8.\n= −4 − 2 + 8 = 2"}]}, {"id": "s6", "label": "6 · Word problems", "sub": "I can turn a story into an expression, find each part first, then add or subtract to get the answer.", "slides": [{"kind": "mcq", "text": "<b>Which expression matches the story?</b> A pen costs ₹15 and a notebook costs ₹40. Asha buys 3 pens and 2 notebooks. Find the total cost.", "opts": ["3 + 15 × 2 + 40", "3 × 15 + 2 × 40", "3 × (15 + 2) × 40", "(3 + 2) × (15 + 40)"], "correct": 1, "tag": "", "sol": "Pens: 3 × 15 = 45. Notebooks: 2 × 40 = 80. Total = 45 + 80 = ₹125."}, {"kind": "blank", "p": "A school buys 12 boxes of chalk at ₹35 a box and 8 dusters at ₹20 each. Find the total cost.", "tag": "", "marks": "", "flat": [{"t": "Chalk: 12 × 35 = ₹__B1__", "a": {"B1": "420"}}, {"t": "Dusters: 8 × 20 = ₹__B1__", "a": {"B1": "160"}}, {"t": "Total = ₹__B1__", "a": {"B1": "580"}}], "sol": "12 × 35 = 420.\n8 × 20 = 160.\nTotal = 12 × 35 + 8 × 20 = 420 + 160 = ₹580."}, {"kind": "mcq", "text": "<b>Which expression matches the story?</b> Ravi had ₹500. He bought 4 cinema tickets at ₹85 each. How much money does he have left?", "opts": ["500 ÷ 4 − 85", "500 − 4 × 85", "500 − 4 + 85", "(500 − 4) × 85"], "correct": 1, "tag": "", "sol": "The tickets cost 4 × 85 = ₹340. Left = 500 − 340 = ₹160. BODMAS does the × before the − automatically."}, {"kind": "blank", "p": "A water tank holds 240 litres. Each day a family uses 6 buckets of 12 litres.", "tag": "", "marks": "", "flat": [{"t": "Water used in one day = __B1__ litres", "a": {"B1": "72"}}, {"t": "Water used in two days = __B1__ litres", "a": {"B1": "144"}}, {"t": "Water left after two days = __B1__ litres", "a": {"B1": "96"}}], "sol": "6 × 12 = 72 litres.\n2 × 72 = 144 litres.\n240 − 2 × 6 × 12 = 240 − 144 = 96 litres."}, {"kind": "mcq", "text": "<b>Error analysis.</b> 5 friends share ₹600 equally, and then each gets ₹20 more from a parent. Neha writes 600 ÷ 5 + 20 = 600 ÷ 25 = 24. What went wrong?", "opts": ["She should subtract: 600 ÷ 5 − 20 = 100", "She added before dividing; 600 ÷ 5 + 20 = 140", "She should multiply: 600 × 5 + 20", "Nothing; each friend gets ₹24"], "correct": 1, "tag": "", "sol": "The expression is right, but ÷ comes before +: 600 ÷ 5 = 120, then 120 + 20 = ₹140 each. Neha did 5 + 20 first."}, {"kind": "blank", "p": "At a museum an adult ticket costs ₹250 and a child ticket costs ₹120. A family of 2 adults and 3 children pays with a ₹1000 note.", "tag": "", "marks": "", "flat": [{"t": "Adults: 2 × 250 = ₹__B1__", "a": {"B1": "500"}}, {"t": "Children: 3 × 120 = ₹__B1__", "a": {"B1": "360"}}, {"t": "Change = ₹__B1__", "a": {"B1": "140"}}], "sol": "2 × 250 = 500.\n3 × 120 = 360.\nChange = 1000 − (500 + 360) = 1000 − 860 = ₹140."}, {"kind": "mcq", "text": "<b>Which expression matches the story?</b> A baker makes 9 trays of 24 cookies and packs them in boxes of 12. How many boxes does she fill?", "opts": ["9 × 24 − 12", "9 × (24 + 12)", "9 + 24 ÷ 12", "9 × 24 ÷ 12"], "correct": 3, "tag": "", "sol": "Cookies: 9 × 24 = 216. Boxes: 216 ÷ 12 = 18. Left to right, 9 × 24 ÷ 12 does exactly that."}, {"kind": "blank", "p": "A van carries 8 crates of mangoes of 15 kg each and 5 crates of apples of 18 kg each. Find the total mass.", "tag": "", "marks": "", "flat": [{"t": "Mangoes: __B1__ kg", "a": {"B1": "120"}}, {"t": "Apples: __B1__ kg", "a": {"B1": "90"}}, {"t": "Total = __B1__ kg", "a": {"B1": "210"}}], "sol": "8 × 15 = 120 kg.\n5 × 18 = 90 kg.\n8 × 15 + 5 × 18 = 120 + 90 = 210 kg."}, {"kind": "mcq", "text": "Which story matches 200 − 3 × 45?", "opts": ["Buy 3 books, then spend ₹200 at ₹45 each", "Start with ₹200, buy 3 books at ₹45 each; money left", "Share ₹200 among 3 people, then give ₹45 more", "Spend ₹45 three times, then add ₹200 to it"], "correct": 1, "tag": "", "sol": "3 × 45 is the cost of 3 books at ₹45; taking it from ₹200 leaves 200 − 135 = ₹65."}, {"kind": "blank", "p": "48 litres of milk are poured equally into 6 cans. Then 2 more litres are added to each can.", "tag": "", "marks": "", "flat": [{"t": "Milk in each can at first = __B1__ litres", "a": {"B1": "8"}}, {"t": "Milk in each can now = __B1__ litres", "a": {"B1": "10"}}, {"t": "Milk in 4 of the cans now = __B1__ litres", "a": {"B1": "40"}}], "sol": "48 ÷ 6 = 8 litres.\n48 ÷ 6 + 2 = 8 + 2 = 10 litres.\n4 × 10 = 40 litres."}, {"kind": "mcq", "text": "Pencils are sold in packs for ₹45. Meena buys 6 packs and gets ₹30 off the whole bill. How much does she pay?", "opts": ["₹300", "₹90", "₹240", "₹270"], "correct": 2, "tag": "", "sol": "6 × 45 − 30 = 270 − 30 = ₹240. (₹90 is 6 × (45 − 30), taking ₹30 off every pack; ₹270 forgets the discount.)"}, {"kind": "blank", "p": "A cinema hall has 18 rows of 25 seats. At one show 136 seats are empty. Each ticket costs ₹150.", "tag": "", "marks": "", "flat": [{"t": "Seats in the hall = __B1__", "a": {"B1": "450"}}, {"t": "People at the show = __B1__", "a": {"B1": "314"}}, {"t": "Money from tickets = ₹__B1__", "a": {"B1": "47100"}}], "sol": "18 × 25 = 450.\n450 − 136 = 314.\n(18 × 25 − 136) × 150 = 314 × 150 = ₹47100."}, {"kind": "mcq", "text": "<b>Which expression matches the story?</b> Raj cycles 4.5 km to school and back every day for 5 days, and 12 km on Saturday. Find the total distance.", "opts": ["2 × 4.5 × 5 + 12", "4.5 × 5 + 12", "(4.5 + 12) × 2 × 5", "2 × (4.5 + 5) + 12"], "correct": 0, "tag": "", "sol": "One day there and back: 2 × 4.5 = 9 km. Five days: 9 × 5 = 45 km. Add Saturday: 45 + 12 = 57 km."}, {"kind": "blank", "p": "Rahul buys 3 kg of rice at ₹62.50 per kg and 2 litres of oil at ₹145 per litre. He pays with ₹600.", "tag": "", "marks": "", "flat": [{"t": "Rice: ₹__B1__", "a": {"B1": "187.5"}}, {"t": "Oil: ₹__B1__", "a": {"B1": "290"}}, {"t": "Change = ₹__B1__", "a": {"B1": "122.5"}}], "sol": "3 × 62.50 = 187.50.\n2 × 145 = 290.\nChange = 600 − (187.50 + 290) = 600 − 477.50 = ₹122.50."}]}, {"id": "s7", "label": "Assessment A", "sub": "Knowing and understanding", "slides": [{"kind": "mcq", "text": "What is 9 + 6 ÷ 3?", "opts": ["18", "5", "11", "15"], "correct": 2, "tag": "", "sol": "÷ before +: 6 ÷ 3 = 2, then 9 + 2 = 11. (5 is (9 + 6) ÷ 3.)"}, {"kind": "blank", "p": "Work out (5 × 6) − (20 ÷ 4) + 3.", "tag": "", "marks": "", "flat": [{"t": "Brackets: (5 × 6) = __B1__ and (20 ÷ 4) = __B2__", "a": {"B1": "30", "B2": "5"}}, {"t": "Answer = __B1__", "a": {"B1": "28"}}], "sol": "= 30 − 5 + 3\n= 25 + 3 = 28 (left to right)."}, {"kind": "mcq", "text": "<b>Which step comes first?</b> 7 + 2 × (9 − 4)", "opts": ["9 − 4", "2 × 9", "7 + 2", "2 × 4"], "correct": 0, "tag": "", "sol": "Brackets first: 9 − 4 = 5. Then 2 × 5 = 10, and 7 + 10 = 17."}, {"kind": "blank", "p": "Find each value.", "tag": "", "marks": "", "flat": [{"t": "a) 3² + 4 × 2 = __B1__", "a": {"B1": "17"}}, {"t": "b) −3² + 10 = __B1__", "a": {"B1": "1"}}], "sol": "9 + 8 = 17.\n−9 + 10 = 1 (only the 3 is squared)."}, {"kind": "mcq", "text": "What is {1/2} of 24 − 4 × 2?", "opts": ["2", "4", "20", "16"], "correct": 1, "tag": "", "sol": "“of” first: {1/2} of 24 = 12; then × : 4 × 2 = 8; then 12 − 8 = 4. (16 is (12 − 4) × 2.)"}, {"kind": "blank", "p": "Work out 60 ÷ [3 × (7 − 2)].", "tag": "", "marks": "", "flat": [{"t": "(7 − 2) = __B1__", "a": {"B1": "5"}}, {"t": "[3 × 5] = __B1__", "a": {"B1": "15"}}, {"t": "Answer = __B1__", "a": {"B1": "4"}}], "sol": "= 60 ÷ [3 × 5]\n= 60 ÷ 15\n= 4"}, {"kind": "mcq", "text": "What is −12 ÷ 4 − (−5)?", "opts": ["8", "−2", "2", "−8"], "correct": 2, "tag": "", "sol": "−12 ÷ 4 = −3. −3 − (−5) = −3 + 5 = 2. (−8 uses −3 − 5.)"}, {"kind": "blank", "p": "Work out 0.6 × 5 + 4.2 ÷ 0.7.", "tag": "", "marks": "", "flat": [{"t": "0.6 × 5 = __B1__", "a": {"B1": "3"}}, {"t": "4.2 ÷ 0.7 = __B1__", "a": {"B1": "6"}}, {"t": "Answer = __B1__", "a": {"B1": "9"}}], "sol": "6 × 5 = 30 → 3.\n42 ÷ 7 = 6.\n= 3 + 6 = 9"}, {"kind": "mcq", "text": "What is √100 − 2³?", "opts": ["4", "92", "−6", "2"], "correct": 3, "tag": "", "sol": "√100 = 10 and 2³ = 8. 10 − 8 = 2. (92 is 100 − 8, forgetting the root; 4 uses 2³ = 6.)"}, {"kind": "blank", "p": "Work out [18 − (−2)] ÷ (−4) + 1.5.", "tag": "", "marks": "", "flat": [{"t": "[18 − (−2)] = __B1__", "a": {"B1": "20"}}, {"t": "20 ÷ (−4) = __B1__", "a": {"B1": "-5"}}, {"t": "Answer = __B1__", "a": {"B1": "-3.5"}}], "sol": "Subtracting −2 is adding 2: 20.\nDifferent signs: −5.\n= −5 + 1.5 = −3.5"}]}, {"id": "s8", "label": "Assessment B", "sub": "Investigating patterns", "slides": [{"kind": "mcq", "text": "Where should brackets go so that 6 + 4 × 3 = 30?", "opts": ["(6 + 4) × 3", "(6) + 4 × 3", "6 + (4 × 3)", "6 + 4 × (3)"], "correct": 0, "tag": "", "sol": "(6 + 4) × 3 = 10 × 3 = 30. The other three all do 4 × 3 first, giving 18."}, {"kind": "blank", "p": "See how moving brackets changes 24 − 6 ÷ 3 + 1.", "tag": "", "marks": "", "flat": [{"t": "a) No brackets: 24 − 6 ÷ 3 + 1 = __B1__", "a": {"B1": "23"}}, {"t": "b) (24 − 6) ÷ 3 + 1 = __B1__", "a": {"B1": "7"}}, {"t": "c) 24 − 6 ÷ (3 + 1) = __B1__", "a": {"B1": "22.5"}}, {"t": "d) (24 − 6) ÷ (3 + 1) = __B1__", "a": {"B1": "4.5"}}], "sol": "6 ÷ 3 = 2: 24 − 2 + 1 = 23.\n18 ÷ 3 + 1 = 6 + 1 = 7.\n6 ÷ 4 = 1.5: 24 − 1.5 = 22.5.\n18 ÷ 4 = 4.5."}, {"kind": "mcq", "text": "Which placement of brackets in 2 × 5 + 3 × 4 gives 52?", "opts": ["(2 × 5 + 3) × 4", "2 × (5 + 3 × 4)", "(2 × 5) + (3 × 4)", "2 × (5 + 3) × 4"], "correct": 0, "tag": "", "sol": "(10 + 3) × 4 = 13 × 4 = 52. The others give 64, 34 and 22."}, {"kind": "blank", "p": "Look at the pattern: 1 + 2 × 3 = 7, 2 + 3 × 4 = 14, …", "tag": "", "marks": "", "flat": [{"t": "3 + 4 × 5 = __B1__", "a": {"B1": "23"}}, {"t": "4 + 5 × 6 = __B1__", "a": {"B1": "34"}}, {"t": "The 10th line: 10 + 11 × 12 = __B1__", "a": {"B1": "142"}}], "sol": "3 + 20 = 23.\n4 + 30 = 34.\n10 + 132 = 142 (always multiply first)."}, {"kind": "mcq", "text": "For which expression does removing the brackets <b>not</b> change the value?", "opts": ["(4 + 5) × 6", "(4 × 5) + 6", "(9 − 3)²", "20 ÷ (2 × 5)"], "correct": 1, "tag": "", "sol": "4 × 5 + 6 is still 26, because × is done first anyway. The others change: 54 → 34, 2 → 50, 36 → 0."}, {"kind": "blank", "p": "Put brackets into 12 − 4 × 2 + 1 in different places.", "tag": "", "marks": "", "flat": [{"t": "a) (12 − 4) × 2 + 1 = __B1__", "a": {"B1": "17"}}, {"t": "b) 12 − 4 × (2 + 1) = __B1__", "a": {"B1": "0"}}, {"t": "c) (12 − 4) × (2 + 1) = __B1__", "a": {"B1": "24"}}], "sol": "8 × 2 + 1 = 17.\n12 − 4 × 3 = 12 − 12 = 0.\n8 × 3 = 24."}, {"kind": "mcq", "text": "Take a = 5 and b = 3. Compare a² − b² with (a − b)².", "opts": ["Both are equal to 16", "5² − 3² = 16 but (5 − 3)² = 4", "Both are equal to 4", "5² − 3² = 4 but (5 − 3)² = 16"], "correct": 1, "tag": "", "sol": "5² − 3² = 25 − 9 = 16. (5 − 3)² = 2² = 4. Powers before −, unless a bracket says otherwise."}, {"kind": "blank", "p": "Find each difference of square numbers and look for a pattern.", "tag": "", "marks": "", "flat": [{"t": "2² − 1² = __B1__", "a": {"B1": "3"}}, {"t": "3² − 2² = __B1__", "a": {"B1": "5"}}, {"t": "4² − 3² = __B1__", "a": {"B1": "7"}}, {"t": "11² − 10² = __B1__", "a": {"B1": "21"}}], "sol": "4 − 1 = 3.\n9 − 4 = 5.\n16 − 9 = 7.\n121 − 100 = 21: each answer is the sum of the two numbers (11 + 10)."}, {"kind": "mcq", "text": "Which is equal to 48 ÷ 4 ÷ 2?", "opts": ["48 × 2 ÷ 4", "48 ÷ 2", "4 ÷ 2 × 48", "48 ÷ 8"], "correct": 3, "tag": "", "sol": "Left to right: 48 ÷ 4 = 12, 12 ÷ 2 = 6. Dividing by 4 and then by 2 is the same as dividing by 8: 48 ÷ 8 = 6."}, {"kind": "blank", "p": "Powers of −1 follow a pattern.", "tag": "", "marks": "", "flat": [{"t": "(−1)² = __B1__", "a": {"B1": "1"}}, {"t": "(−1)³ = __B1__", "a": {"B1": "-1"}}, {"t": "(−1)¹⁰ = __B1__", "a": {"B1": "1"}}, {"t": "(−1)¹⁵ = __B1__", "a": {"B1": "-1"}}], "sol": "(−1) × (−1) = 1.\n1 × (−1) = −1.\nAn even power of −1 is 1.\nAn odd power of −1 is −1."}]}, {"id": "s9", "label": "Assessment C", "sub": "Communicating", "slides": [{"kind": "mcq", "text": "BODMAS, BIDMAS and PEMDAS give the same order. In <b>PEMDAS</b>, what does the <b>E</b> stand for?", "opts": ["Exponents (powers)", "Expressions", "Estimates", "Equals signs"], "correct": 0, "tag": "", "sol": "PEMDAS = Parentheses, Exponents, Multiplication and Division, Addition and Subtraction. Exponents are the O (orders) of BODMAS and the I (indices) of BIDMAS."}, {"kind": "blank", "p": "Complete the sentences.", "tag": "", "marks": "", "flat": [{"t": "In BODMAS, the word “of” means __B1__, and it is done before ÷ and ×.", "a": {"B1": "multiply"}, "expr": "words", "accept": ["multiplication", "times", "multiplied", "product"]}, {"t": "÷ and × have the same rank, so they are worked from __B1__ to __B2__.", "a": {"B1": "left", "B2": "right"}, "expr": "words"}], "sol": "{1/2} of 20 = {1/2} × 20 = 10.\n24 ÷ 4 × 2: 24 ÷ 4 = 6 first, then 6 × 2 = 12."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Tom wrote 20 − 8 − 2 = 20 − 6 = 14. Which comment is right?", "opts": ["Right: take away the small numbers first", "Wrong: the answer should be 30", "Right: no brackets are needed here", "Wrong: go left to right, 12 − 2 = 10"], "correct": 3, "tag": "", "sol": "Two subtractions are worked left to right: 20 − 8 = 12, then 12 − 2 = 10. Tom worked 20 − (8 − 2)."}, {"kind": "blank", "p": "<b>Spot the error.</b> 5 + 3 × (6 − 2)   Line 1: = 5 + 3 × 4   Line 2: = 8 × 4   Line 3: = 32", "tag": "", "marks": "", "flat": [{"t": "The first wrong line is Line __B1__", "a": {"B1": "2"}}, {"t": "The correct answer is __B1__", "a": {"B1": "17"}}], "sol": "Line 1 is right. Line 2 adds 5 + 3 before multiplying.\n= 5 + 12 = 17."}, {"kind": "mcq", "text": "Which expression means “add 7 and 5, then multiply by 3”?", "opts": ["7 + (5 × 3)", "7 × 3 + 5", "7 + 5 × 3", "(7 + 5) × 3"], "correct": 3, "tag": "", "sol": "The brackets make the + happen first: (7 + 5) × 3 = 12 × 3 = 36. Without brackets, 7 + 5 × 3 = 7 + 15 = 22."}, {"kind": "blank", "p": "Find “half of 18 plus 4 times 3”.", "tag": "", "marks": "", "flat": [{"t": "{1/2} of 18 = __B1__", "a": {"B1": "9"}}, {"t": "4 × 3 = __B1__", "a": {"B1": "12"}}, {"t": "Total = __B1__", "a": {"B1": "21"}}], "sol": "{1/2} × 18 = 9.\n4 × 3 = 12.\n9 + 12 = 21."}, {"kind": "mcq", "text": "Why is 8 ÷ 2 × 4 equal to 16 and not 1?", "opts": ["Division is always done before multiplication", "÷ and × are equal rank, so go left to right", "Multiplication is always done before division", "It is 1: M comes before D in PEMDAS"], "correct": 1, "tag": "", "sol": "8 ÷ 2 = 4, then 4 × 4 = 16. Neither ÷ nor × always comes first; the one on the left is done first. (Division first “always” would fail for 8 × 4 ÷ 2.)"}, {"kind": "blank", "p": "Riya says √(9 + 16) = √9 + √16 = 7. Check her claim.", "tag": "", "marks": "", "flat": [{"t": "9 + 16 = __B1__", "a": {"B1": "25"}}, {"t": "√(9 + 16) = √25 = __B1__", "a": {"B1": "5"}}, {"t": "Is Riya right? (yes / no) __B1__", "a": {"B1": "no"}, "expr": "words"}], "sol": "The root covers the whole sum, so add first: 25.\n√25 = 5.\n5 ≠ 7, so Riya is wrong: a root of a sum is not the sum of the roots."}, {"kind": "mcq", "text": "<b>Error analysis.</b> Neel says 24 ÷ 2 of 6 = 72, because 24 ÷ 2 = 12 and 12 × 6 = 72. Which reply is correct?", "opts": ["Wrong: it is 24 ÷ 2 + 6 = 18", "Wrong: “of” first, 2 of 6 = 12, so 24 ÷ 12 = 2", "Right: ÷ and “of” are worked left to right", "Wrong: it is 24 × 2 × 6 = 288"], "correct": 1, "tag": "", "sol": "In BODMAS, O (“of”) comes before D: 2 of 6 = 12, then 24 ÷ 12 = 2. Neel treated “of” like an ordinary × in left-to-right order."}, {"kind": "blank", "p": "Write the value of each expression.", "tag": "", "marks": "", "flat": [{"t": "a) (−3)² = __B1__", "a": {"B1": "9"}}, {"t": "b) −3² = __B1__", "a": {"B1": "-9"}}, {"t": "c) (−3)² − 3² = __B1__", "a": {"B1": "0"}}], "sol": "(−3) × (−3) = 9: the bracket is squared.\n−(3 × 3) = −9: only the 3 is squared.\n9 − 9 = 0."}]}, {"id": "s10", "label": "Assessment D", "sub": "Applying mathematics in real-life contexts", "slides": [{"kind": "mcq", "text": "A family buys 4 movie tickets at ₹180 each and 2 tubs of popcorn at ₹120 each. What is the total cost?", "opts": ["₹960", "₹840", "₹1800", "₹1200"], "correct": 0, "tag": "", "sol": "4 × 180 + 2 × 120 = 720 + 240 = ₹960. (₹1800 is (4 + 2) × (180 + 120); ₹840 forgets the second popcorn.)"}, {"kind": "blank", "p": "An electricity bill charges ₹6.50 per unit plus a fixed charge of ₹150. A home uses 120 units.", "tag": "", "marks": "", "flat": [{"t": "Cost of units: 120 × 6.50 = ₹__B1__", "a": {"B1": "780"}}, {"t": "Total bill = ₹__B1__", "a": {"B1": "930"}}], "sol": "120 × 6.5 = 780.\n120 × 6.50 + 150 = 780 + 150 = ₹930."}, {"kind": "mcq", "text": "A school trip uses 4 buses with 45 seats each. Three buses are full and the fourth has 20 empty seats. How many students go?", "opts": ["180", "115", "160", "155"], "correct": 2, "tag": "", "sol": "3 × 45 + (45 − 20) = 135 + 25 = 160. (180 counts every seat; 115 is 135 − 20.)"}, {"kind": "blank", "p": "Sahil saves ₹250 a week for 8 weeks. Then he spends ₹1350 on shoes.", "tag": "", "marks": "", "flat": [{"t": "Savings: ₹__B1__", "a": {"B1": "2000"}}, {"t": "Left after buying the shoes: ₹__B1__", "a": {"B1": "650"}}], "sol": "250 × 8 = 2000.\n250 × 8 − 1350 = 2000 − 1350 = ₹650."}, {"kind": "mcq", "text": "<b>Is this reasonable?</b> Priya buys 6 notebooks at ₹48 each and a school bag for ₹395. She says the total is about ₹2700. Estimate to check.", "opts": ["Reasonable: about 6 × (50 + 400) = ₹2700", "Reasonable: about (6 + 1) × 400 = ₹2800", "Not reasonable: about 6 × 50 + 400 = ₹700", "Not reasonable: about 6 + 50 + 400 = ₹456"], "correct": 2, "tag": "", "sol": "Round ₹48 to ₹50 and ₹395 to ₹400: 6 × 50 + 400 = 700. Priya multiplied the bag by 6 too. Exact total: 6 × 48 + 395 = 288 + 395 = ₹683."}, {"kind": "blank", "p": "For a party, 3 bottles of 1.5 litres and 4 bottles of 0.75 litres of juice are poured into cups of 0.25 litre.", "tag": "", "marks": "", "flat": [{"t": "Total juice = __B1__ litres", "a": {"B1": "7.5"}}, {"t": "Number of cups filled = __B1__", "a": {"B1": "30"}}], "sol": "3 × 1.5 + 4 × 0.75 = 4.5 + 3 = 7.5 litres.\n7.5 ÷ 0.25 = 750 ÷ 25 = 30 cups."}, {"kind": "mcq", "text": "A rectangular garden is 12 m by 8 m. A square pond of side 3 m is dug in it. What area of garden is left?", "opts": ["87 m²", "90 m²", "93 m²", "75 m²"], "correct": 0, "tag": "", "sol": "12 × 8 − 3² = 96 − 9 = 87 m². (93 uses 3 instead of 3²; 90 uses 3 × 2 = 6.)"}, {"kind": "blank", "p": "A train travels for 2.5 hours at 80 km/h, then for 1.5 hours at 60 km/h.", "tag": "", "marks": "", "flat": [{"t": "First part: __B1__ km", "a": {"B1": "200"}}, {"t": "Second part: __B1__ km", "a": {"B1": "90"}}, {"t": "Total distance = __B1__ km", "a": {"B1": "290"}}], "sol": "2.5 × 80 = 200 km.\n1.5 × 60 = 90 km.\n2.5 × 80 + 1.5 × 60 = 200 + 90 = 290 km."}, {"kind": "mcq", "text": "₹2000 is shared: Ananya gets {1/4} of it and the rest is split equally among her 3 brothers. How much does each brother get?", "opts": ["₹500", "₹666.67", "₹375", "₹1500"], "correct": 0, "tag": "", "sol": "{1/4} of 2000 = 500. Rest = 2000 − 500 = 1500. Each brother: 1500 ÷ 3 = ₹500. (₹666.67 ignores Ananya’s share.)"}, {"kind": "blank", "p": "At 6 a.m. the temperature in Shimla is −4 °C. It rises by 3 °C every hour for 5 hours, then falls by 6 °C.", "tag": "", "marks": "", "flat": [{"t": "Temperature after 5 hours: −4 + 3 × 5 = __B1__ °C", "a": {"B1": "11"}}, {"t": "Final temperature = __B1__ °C", "a": {"B1": "5"}}], "sol": "× first: 3 × 5 = 15, then −4 + 15 = 11 °C.\n11 − 6 = 5 °C."}]}];
/* ================= build slide lists ================= */
var SLIDES = {}; SECTIONS.forEach(function(sec){ SLIDES[sec.id]=sec.slides; });
var TAB_DEFS = SECTIONS.map(function(sec){ return {id:sec.id, label:sec.label, sub:sec.sub}; });

/* ================= state ================= */
function blankItem(slide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    return {status:'unanswered', attempts:0, choice:undefined};
  }
  return {status:'unanswered', curStep:0, stepStates: slide.flat.map(function(){ return {status:'unanswered', attempts:0, inputs:{}}; })};
}
function defaultState(){
  var s={tabs:{}};
  TAB_DEFS.forEach(function(td){
    s.tabs[td.id] = { idx:0, maxReached:0, items: SLIDES[td.id].map(function(sl){ return blankItem(sl); }) };
  });
  return s;
}
var appState = {mode:null, learning:null, quiz:null};
var state = null;      // alias for appState[appState.mode]
var MODE = null;
var activeTab=TAB_DEFS[0].id;

function setMode(m){
  appState.mode = m;
  if(!appState[m]) appState[m] = defaultState();
  state = appState[m];
  MODE = m;
}
/* ================= student accounts (saved on this device) ================= */
var SHEET_KEY='sopaan-bodmas';
var ACC_KEY='sopaan-students-v1';
var storageOK=true;
var MEM={};
function lsGet(k){ var r=null; try{ r=localStorage.getItem(k); }catch(e){ storageOK=false; }
  if(r===null||r===undefined) r=MEM[k]||null; try{ return r?JSON.parse(r):null; }catch(e){ return null; } }
function lsSet(k,v){ var j=JSON.stringify(v); MEM[k]=j; try{ localStorage.setItem(k,j); return true; }catch(e){ storageOK=false; return false; } }
try{ localStorage.setItem('sopaan-probe','1'); localStorage.removeItem('sopaan-probe'); }catch(e){ storageOK=false; }
function hashPin(pin,salt){ var h=2166136261, str=salt+'|'+pin; for(var i=0;i<str.length;i++){ h^=str.charCodeAt(i); h=Math.imul(h,16777619)>>>0; } return h.toString(36); }
function accKey(name,roll){ return (name.trim().toLowerCase().replace(/\s+/g,' ')+'#'+roll.trim().toLowerCase()); }
function accounts(){ return lsGet(ACC_KEY)||{students:{}}; }
var student=null; // {key,name,roll}
var records=[];   // finished-tab history for this student on this sheet
function progKey(){ return SHEET_KEY+'::'+student.key; }

function loadState(){
  appState = {mode:null, learning:null, quiz:null}; records=[]; activeTab=TAB_DEFS[0].id;
  var saved=lsGet(progKey());
  if(!saved) return;
  try{
    ['learning','quiz'].forEach(function(m){
      if(saved[m] && saved[m].tabs){
        var fresh = defaultState();
        TAB_DEFS.forEach(function(td){
          if(saved[m].tabs[td.id] && saved[m].tabs[td.id].items && saved[m].tabs[td.id].items.length===SLIDES[td.id].length){
            fresh.tabs[td.id] = saved[m].tabs[td.id];
          }
        });
        appState[m] = fresh;
      }
    });
    if(saved.mode==='learning' || saved.mode==='quiz') appState.mode = saved.mode;
    if(saved.activeTab && TAB_DEFS.some(function(t){ return t.id===saved.activeTab; })) activeTab = saved.activeTab;
    if(Array.isArray(saved.records)) records = saved.records;
  }catch(e){}
}
function saveState(){
  if(!student) return;
  lsSet(progKey(), {mode:appState.mode, learning:appState.learning, quiz:appState.quiz, activeTab:activeTab, records:records, updated:Date.now()});
  var acc=accounts(); if(acc.students[student.key]){ acc.students[student.key].last=Date.now(); lsSet(ACC_KEY,acc); }
}
function addRecord(mode, tabId, correct, revealed, skipped, total){
  records.unshift({at:Date.now(), mode:mode, tab:tabId, correct:correct, revealed:revealed, skipped:skipped, total:total});
  if(records.length>60) records.length=60;
  saveState();
}
function fmtDate(ms){ try{ return new Date(ms).toLocaleString(undefined,{day:'numeric',month:'short',hour:'2-digit',minute:'2-digit'}); }catch(e){ return ''; } }

function renderWho(){
  var el=document.getElementById('whoBar');
  if(!student){ el.hidden=true; el.innerHTML=''; return; }
  el.hidden=false;
  el.innerHTML='Signed in as <b>'+esc(student.name)+'</b> · Roll '+esc(student.roll)+
    ' <button id="btnRecord">My record</button> <button id="btnSignOut">Sign out</button>';
  document.getElementById('btnRecord').addEventListener('click', renderRecord);
  document.getElementById('btnSignOut').addEventListener('click', signOut);
}
function signOut(){
  saveState(); student=null; state=null; MODE=null;
  var acc=accounts(); delete acc.current; lsSet(ACC_KEY,acc);
  document.getElementById('modePill').hidden=true;
  renderWho(); renderLogin();
}
function signIn(key){
  var acc=accounts(); var st=acc.students[key]; if(!st) return;
  student={key:key, name:st.name, roll:st.roll};
  acc.current=key; st.last=Date.now(); lsSet(ACC_KEY,acc);
  loadState(); renderWho();
  if(appState.mode==='learning' || appState.mode==='quiz'){
    setMode(appState.mode); document.getElementById('modePill').hidden=false; updateModePill();
    showChrome(true); buildTabbar(); renderSlide();
    showToast('Welcome back, '+st.name.split(' ')[0]+'. Picking up where you left off.');
  } else {
    renderModePicker();
  }
}
function renderLogin(msg){
  showChrome(false);
  var acc=accounts(); var keys=Object.keys(acc.students).sort(function(a,b){ return (acc.students[b].last||0)-(acc.students[a].last||0); });
  var h='<div class="login-card"><h2>Student sign-in</h2>'+
    '<p class="lead">Sign in to save your answers, see your record and continue where you left off next time.</p>';
  if(!storageOK) h+='<div class="login-err">This browser is blocking saved data (for example, a private window). You can still practise, but progress will not be kept after you close the page.</div>';
  if(msg) h+='<div class="login-err">'+esc(msg)+'</div>';
  if(keys.length){
    h+='<div class="known"><span class="known-label">Continue as</span>';
    keys.slice(0,6).forEach(function(k){ var st=acc.students[k];
      h+='<button class="known-btn" data-k="'+esc(k)+'"><span>'+esc(st.name)+' <small>· Roll '+esc(st.roll)+'</small></span><small>'+(st.last?'Last active '+fmtDate(st.last):'')+'</small></button>'; });
    h+='</div>';
  }
  h+='<form id="loginForm" novalidate>'+
    '<div class="fld"><label for="lgName">Full name</label><input id="lgName" autocomplete="name" required></div>'+
    '<div class="fld-row"><div class="fld"><label for="lgRoll">Roll no.</label><input id="lgRoll" required></div>'+
    '<div class="fld"><label for="lgPin">4-digit PIN</label><input id="lgPin" type="password" inputmode="numeric" maxlength="4" autocomplete="off" required></div></div>'+
    '<button class="btn btn-primary" type="submit" style="width:100%;">Sign in</button>'+
    '<p class="login-note">New here? Enter your name, roll no. and a PIN you will remember, and your profile is created. Progress is saved on this device, so use the same device and browser to continue.</p>'+
    '</form></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  wrap.querySelectorAll('.known-btn').forEach(function(b){
    b.addEventListener('click', function(){
      var st=accounts().students[b.dataset.k];
      document.getElementById('lgName').value=st.name; document.getElementById('lgRoll').value=st.roll;
      document.getElementById('lgPin').focus();
    });
  });
  document.getElementById('loginForm').addEventListener('submit', function(e){
    e.preventDefault();
    var name=document.getElementById('lgName').value.trim(), roll=document.getElementById('lgRoll').value.trim(), pin=document.getElementById('lgPin').value.trim();
    if(!name||!roll){ renderLoginErr('Enter your name and roll no.'); return; }
    if(!/^\d{4}$/.test(pin)){ renderLoginErr('Your PIN must be exactly 4 digits.'); return; }
    var k=accKey(name,roll); var acc=accounts();
    if(acc.students[k]){
      if(acc.students[k].pin!==hashPin(pin,k)){ renderLoginErr('That PIN does not match this name and roll no. Try again.'); return; }
    } else {
      acc.students[k]={name:name.replace(/\s+/g,' '), roll:roll, pin:hashPin(pin,k), created:Date.now()};
      lsSet(ACC_KEY,acc);
    }
    signIn(k);
  });
}
function renderLoginErr(m){
  var n=document.getElementById('lgName').value, r=document.getElementById('lgRoll').value;
  renderLogin(m); document.getElementById('lgName').value=n; document.getElementById('lgRoll').value=r; document.getElementById('lgPin').focus();
}
function tabSummary(m){
  var st=appState[m]; if(!st) return null;
  return TAB_DEFS.map(function(td){
    var items=st.tabs[td.id].items, n=items.length;
    var c=items.filter(function(i){return i.status==='correct';}).length;
    var done=items.filter(function(i){return i.status!=='unanswered';}).length;
    return {label:td.label, n:n, done:done, correct:c};
  });
}
function renderRecord(){
  showChrome(false);
  var tabLabel=function(id){ var t=TAB_DEFS.find(function(x){return x.id===id;}); return t?t.label:id; };
  var h='<div class="rec-card"><h2>'+esc(student.name)+'’s record</h2><p class="rec-empty" style="margin:0 0 12px;">Roll '+esc(student.roll)+' · BODMAS: Order of Operations</p>';
  ['learning','quiz'].forEach(function(m){
    var sum=tabSummary(m);
    h+='<h3>'+(m==='learning'?'📘 Learning Sheet':'📝 Quiz Mode')+'</h3>';
    if(!sum){ h+='<p class="rec-empty">Not started yet.</p>'; return; }
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>Section</th><th>Progress</th><th>Correct first time</th></tr></thead><tbody>';
    sum.forEach(function(r){ var pct=Math.round(r.done/r.n*100);
      h+='<tr><td>'+r.label+'</td><td><span class="rec-bar"><i style="width:'+pct+'%"></i></span>'+r.done+' / '+r.n+'</td><td>'+(m==='quiz'?'—':r.correct+' / '+r.n)+'</td></tr>'; });
    h+='</tbody></table></div>';
  });
  h+='</div><div class="rec-card"><h3>Finished sections</h3>';
  if(!records.length){ h+='<p class="rec-empty">No sections finished yet. Each time you finish a tab, your score is recorded here.</p>'; }
  else {
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>When</th><th>Mode</th><th>Section</th><th>Score</th></tr></thead><tbody>';
    records.forEach(function(r){ h+='<tr><td>'+fmtDate(r.at)+'</td><td>'+(r.mode==='quiz'?'Quiz':'Learning')+'</td><td>'+tabLabel(r.tab)+'</td><td>'+r.correct+' / '+r.total+(r.mode==='learning'?' <span class="rec-empty">('+r.revealed+' revealed, '+r.skipped+' skipped)</span>':'')+'</td></tr>'; });
    h+='</tbody></table></div>';
  }
  h+='</div><button class="btn btn-primary" id="btnBack">'+(MODE?'Back to practice':'Choose a mode')+'</button>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('btnBack').addEventListener('click', function(){
    if(MODE){ showChrome(true); buildTabbar(); renderSlide(); } else renderModePicker();
  });
  window.scrollTo({top:0});
}

/* ================= tab bar ================= */
function buildTabbar(){
  var el=document.getElementById('tabbar');
  el.innerHTML = '<button class="tab-btn'+(activeTab==='theory'?' active':'')+'" data-tab="theory">Theory<span class="tab-count">notes</span></button>' + TAB_DEFS.map(function(t){
    return '<button class="tab-btn'+(t.id===activeTab?' active':'')+'" data-tab="'+t.id+'">'+t.label+'<span class="tab-count">'+SLIDES[t.id].length+'</span></button>';
  }).join('') + '<button class="tab-btn'+(activeTab==='report'?' active':'')+'" data-tab="report">📊 Report<span class="tab-count">pie</span></button>';
  el.querySelectorAll('.tab-btn').forEach(function(btn){
    btn.addEventListener('click', function(){ activeTab=btn.dataset.tab; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); });
  });
}

/* ================= rendering ================= */
function canProceed(item){ return item.status==='correct'||item.status==='revealed'||item.status==='skipped'; }

function renderSlide(){
  if(!state.tabs[activeTab]){ activeTab=TAB_DEFS[0].id; }
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  if(ts.idx>=slides.length||ts.idx<0) ts.idx=0;
  var idx = ts.idx;
  var slide = slides[idx];
  var item = ts.items[idx];

  var tdef=TAB_DEFS.find(function(t){return t.id===activeTab;});
  document.getElementById('spLabel').textContent = tdef.label+' · Question '+(idx+1)+' of '+slides.length;
  document.getElementById('secSub').textContent = tdef.label+' — '+tdef.sub;
  document.getElementById('spFill').style.width = Math.round(((idx)/(Math.max(slides.length-1,1)))*100)+'%';

  var h='<div class="qcard">';
  if(slide.kind==='mcq' || slide.kind==='ar'){
    h += renderChoiceBody(slide, item, idx);
  } else {
    h += renderBlankBody(slide, item, idx);
  }
  h += '<div class="feedback" id="feedbackBox"></div>';
  if(MODE==='learning' && (item.status==='correct'||item.status==='revealed')) h += solutionHTML(slide);
  h += '</div>';

  document.getElementById('wrap').innerHTML = h;
  wireSlideEvents(slide, item, idx);
  restoreFeedback(item);
  renderNavbar(item);
  updateFab();
  saveState();
}

function solutionHTML(slide){
  if(!slide.sol) return '';
  return '<div class="solution"><div class="sol-h">Solution</div>'+slide.sol.split('\n').map(function(l){ return '<div class="sol-line">'+fr(esc(l))+'</div>'; }).join('')+'</div>';
}
var LETTERS=['a','b','c','d','e','f'];
function chosenHTML(slide,item){
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(item.choice===undefined) return '';
  var ok=item.choice===slide.correct;
  var h='<div class="chosen '+(ok?'ok':'bad')+'"><b>Your answer:</b> ('+LETTERS[item.choice]+') '+fr(esc(opts[item.choice]))+(ok?' ✓':' ✗')+'</div>';
  if(!ok) h+='<div class="chosen ok"><b>Correct answer:</b> ('+LETTERS[slide.correct]+') '+fr(esc(opts[slide.correct]))+'</div>';
  return h;
}
function renderChoiceBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div>';
  if(slide.kind==='mcq'){
    h += '<div class="qtext">'+fr(esc(slide.text))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  } else {
    h += '<div class="qtext"><span class="qtag" style="margin-left:0;margin-right:6px;">A / R</span>Assertion (A): '+fr(esc(slide.a))+'<br>Reason (R): '+fr(esc(slide.r))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  }
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(MODE==='quiz') h += '<div class="quiz-hint">Quiz mode — pick an option. The correct answer is revealed once you finish this tab.</div>';
  var locked = MODE==='learning' && (item.status==='correct' || item.status==='revealed');
  h += '<div class="options">';
  opts.forEach(function(opt,i){
    var cls='opt';
    if(locked){
      if(i===slide.correct) cls+=' is-correct';
      else if(item.choice===i) cls+=' is-wrong';
      cls+=' locked';
    }
    if(item.choice===i) cls+=' is-chosen';
    h += '<label class="'+cls+'"><input type="radio" name="choice" value="'+i+'" '+(item.choice===i?'checked':'')+' '+(locked?'disabled':'')+'> <span class="opt-l">('+LETTERS[i]+')</span> <span class="opt-t">'+fr(esc(opt))+'</span>'+(item.choice===i?'<span class="opt-tag">'+(locked?(i===slide.correct?'Your answer ✓':'Your answer ✗'):'Selected')+'</span>':'')+'</label>';
  });
  h += '</div>';
  if(locked) h += chosenHTML(slide,item);
  return h;
}

function renderBlankBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div><div class="qtext">';
  if(slide.kind==='case'){ h += '<b>'+esc(slide.title)+'.</b> '+esc(slide.body); }
  else { var pp=fr(esc(slide.p)).split('\n'); h += pp[0]+pp.slice(1).map(function(x){return '<span class="datline">'+x+'</span>';}).join(''); }
  h += (slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  if(slide.marks) h += '<div class="marks-pill">'+slide.marks+'</div>';
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';

  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  var blankCount=0; slide.flat.forEach(function(s){ blankCount+=Object.keys(s.a).length; });

  if(MODE==='quiz'){
    h += '<div class="step-badge">'+blankCount+' blank'+(blankCount===1?'':'s')+' in this question</div>';
    h += '<div class="quiz-hint">Quiz mode — fill in what you can. Correct answers are revealed once you finish this tab.</div>';
    if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
    if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
    h += '<div class="steps">';
    for(var qi=0; qi<total; qi++){
      var qstep = slide.flat[qi];
      var qss = item.stepStates[qi];
      if(qstep.partLabel) h += '<div class="subpart-label">'+esc(qstep.partLabel)+'</div>';
      if(qstep.orDivider) h += '<div class="or-divider">OR</div>';
      var qline = fr(qstep.t);
      Object.keys(qstep.a).forEach(function(bkey){
        var val = qss.inputs[bkey]||'';
        var inp='<input class="blank-input'+(['fv','fe','fl','fm','fi','dec'].indexOf(qstep.expr)>=0?' fr':(qstep.expr==='flist'||qstep.expr==='dlist'||qstep.expr==='glist')?' wide':qstep.expr===true?' expr':(qstep.expr==='words'||qstep.expr==='list'||qstep.expr==='expanded'||qstep.expr==='set'||qstep.expr==='primes'||qstep.expr==='trans'||qstep.expr==='rot'?' wide':''))+'" data-step="'+qi+'" data-bkey="'+bkey+'" value="'+esc(val)+'">';
        qline = qline.replace('__'+bkey+'__', inp);
      });
      if(qstep.fig) h += '<div class="fig">'+qstep.fig+'</div>';
      h += '<div class="step-line">'+qline+'</div>';
    }
    h += '</div>';
    return h;
  }

  if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
  if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
  var locked = item.status==='correct' || item.status==='revealed';
  var upto = locked ? total-1 : item.curStep;
  h += '<div class="step-badge">Step '+(Math.min(item.curStep,total-1)+1)+' of '+total+'</div>';
  h += '<div class="steps">';
  for(var i=0;i<=upto;i++){
    var step = slide.flat[i];
    var ss = item.stepStates[i];
    if(step.partLabel) h += '<div class="subpart-label">'+esc(step.partLabel)+'</div>';
    if(step.orDivider) h += '<div class="or-divider">OR</div>';
    var resolved = ss.status==='correct' || ss.status==='revealed';
    var line = fr(step.t);
    Object.keys(step.a).forEach(function(bkey){
      var fs = ss.inputs['fs_'+bkey];
      var cls = fs==='correct'?'is-correct':(fs==='wrong'?'is-wrong':'');
      var val = ss.inputs[bkey]||'';
      var dis = resolved ? 'disabled':'';
      var inp='<input class="blank-input '+cls+(['fv','fe','fl','fm','fi','dec'].indexOf(step.expr)>=0?' fr':(step.expr==='flist'||step.expr==='dlist'||step.expr==='glist')?' wide':step.expr===true?' expr':(step.expr==='words'||step.expr==='list'||step.expr==='expanded'||step.expr==='set'||step.expr==='primes'||step.expr==='trans'||step.expr==='rot'?' wide':''))+'" data-step="'+i+'" data-bkey="'+bkey+'" value="'+esc(val)+'" '+dis+'>';
      line = line.replace('__'+bkey+'__', inp);
    });
    if(step.fig) h += '<div class="fig">'+step.fig+'</div>';
    h += '<div class="step-line'+(resolved?' resolved':'')+'">'+line+'</div>';
    if(ss.status==='revealed'){
      var reveals=[];
      Object.keys(step.a).forEach(function(bkey){ reveals.push(bkey+' = '+esc(step.a[bkey])); });
      h += '<div class="reveal-note">correct: '+reveals.join(', ')+'</div>';
    }
  }
  h += '</div>';
  return h;
}

var __flash=null;
function restoreFeedback(item){
  var fb=document.getElementById('feedbackBox'); if(!fb) return;
  if(__flash){ fb.className='feedback show '+__flash.c; fb.innerHTML=__flash.h; __flash=null; return; }
  if(MODE==='quiz'){ fb.className='feedback'; fb.innerHTML=''; return; }
  if(item.status==='correct'){ fb.className='feedback show ok'; fb.innerHTML='<b>All done — correct!</b>'; }
  else if(item.status==='revealed'){ fb.className='feedback show reveal'; fb.innerHTML='<b>Answer revealed above.</b> Move on whenever you’re ready.'; }
  else if(item.status==='skipped'){ fb.className='feedback show retry'; fb.innerHTML='Skipped — you can answer it now and press <b>Check</b>, or come back later from the Question Palette.'; }
  else { fb.className='feedback'; fb.innerHTML=''; }
}

function slideIsAnswered(slide,item){
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return item.stepStates.some(function(ss){ return Object.keys(ss.inputs).some(function(k){ return k.indexOf('fs_')!==0 && ss.inputs[k] && String(ss.inputs[k]).trim()!==''; }); });
}
function renderNavbar(item){
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  var slide = slides[ts.idx];
  var isLast = ts.idx === slides.length-1;
  var h='';
  h += '<button class="btn" id="btnPrev" '+(ts.idx===0?'disabled':'')+'>&larr; Previous</button>';

  if(MODE==='quiz'){
    h += '<button class="btn" id="btnSkip">Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnNext">'+(isLast?'Finish':'Next →')+'</button>';
  } else {
    h += '<button class="btn" id="btnSkip" '+(item.status!=='unanswered'?'disabled':'')+'>Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnCheck" '+((item.status!=='unanswered'&&item.status!=='skipped')?'disabled':'')+'>Check</button>';
    h += '<button class="btn btn-primary" id="btnNext" '+(canProceed(item)?'':'disabled')+'>'+(isLast?'Finish':'Next →')+'</button>';
  }
  document.getElementById('navbarInner').innerHTML = h;

  document.getElementById('btnPrev').addEventListener('click', function(){ if(ts.idx>0){ ts.idx--; renderSlide(); } });

  if(MODE==='quiz'){
    document.getElementById('btnSkip').addEventListener('click', function(){
      item.status='skipped';
      advanceQuiz(ts, slides);
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      item.status = slideIsAnswered(slide,item) ? 'answered' : 'unanswered';
      advanceQuiz(ts, slides);
    });
  } else {
    document.getElementById('btnSkip').addEventListener('click', function(){
      if(item.status==='unanswered'){ item.status='skipped'; renderSlide(); }
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      if(!canProceed(item)) return;
      if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
      else { renderFinished(); }
    });
    document.getElementById('btnCheck').addEventListener('click', function(){ handleCheck(); });
  }
}
function advanceQuiz(ts, slides){
  if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
  else { renderFinished(); }
}

function wireSlideEvents(slide, item, idx){
  document.querySelectorAll('input[type="radio"][name="choice"]').forEach(function(r){
    r.addEventListener('change', function(){ item.choice=parseInt(r.value,10); saveState();
      document.querySelectorAll('.opt').forEach(function(l){ l.classList.remove('is-chosen'); var t=l.querySelector('.opt-tag'); if(t) t.remove(); });
      var lab=r.closest('.opt'); lab.classList.add('is-chosen'); var tg=document.createElement('span'); tg.className='opt-tag'; tg.textContent='Selected'; lab.appendChild(tg); });
  });
  document.querySelectorAll('.blank-input').forEach(function(inp){
    inp.addEventListener('input', function(){
      var si=parseInt(inp.dataset.step,10), bk=inp.dataset.bkey;
      item.stepStates[si].inputs[bk]=inp.value;
      saveState();
    });
  });
}

function handleCheck(){
  var ts = state.tabs[activeTab];
  var slide = SLIDES[activeTab][ts.idx];
  var item = ts.items[ts.idx];
  if(item.status==='skipped') item.status='unanswered';
  var fb = document.getElementById('feedbackBox');

  if(slide.kind==='mcq' || slide.kind==='ar'){
    if(item.choice===undefined){ fb.className='feedback show err'; fb.innerHTML='Please choose an option first.'; return; }
    var ok = item.choice===slide.correct;
    if(ok){
      item.status='correct'; playSuccess();
      fb.className='feedback show ok'; fb.innerHTML='<b>Correct!</b>';
    } else {
      item.attempts=(item.attempts||0)+1;
      if(item.attempts>=2){ item.status='revealed'; playReveal(); }
      else { playWrong(); fb.className='feedback show retry'; fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'}; }
    }
    renderSlide();
    return;
  }

  // blank / case
  var step = slide.flat[item.curStep];
  var ss = item.stepStates[item.curStep];
  var blanks = Object.keys(step.a);
  var missing = blanks.some(function(k){ return !ss.inputs[k] || String(ss.inputs[k]).trim()===''; });
  if(missing){ fb.className='feedback show err'; fb.innerHTML='Fill in every blank in this step before checking.'; return; }

  var allGood=true;
  blanks.forEach(function(k){
    var good=answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr);
    ss.inputs['fs_'+k]=good?'correct':'wrong';
    if(!good) allGood=false;
  });

  if(allGood){
    ss.status='correct'; playSuccess();
    advanceStepOrFinish(item, slide);
  } else {
    ss.attempts=(ss.attempts||0)+1;
    if(ss.attempts>=2){
      ss.status='revealed'; playReveal();
      advanceStepOrFinish(item, slide);
    } else {
      playWrong();
      fb.className='feedback show retry';
      fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'};
      renderSlide();
      return;
    }
  }
  renderSlide();
}

function advanceStepOrFinish(item, slide){
  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  if(item.curStep < total-1){
    item.curStep++;
  } else {
    var anyRevealed = item.stepStates.some(function(ss){ return ss.status==='revealed'; });
    item.status = anyRevealed ? 'revealed' : 'correct';
  }
}

/* ================= palette ================= */
var overlay=document.getElementById('overlay');
function statusOf(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  return ts.items[i].status==='unanswered' ? 'locked' : ts.items[i].status;
}
function buildPalette(){
  var slides=SLIDES[activeTab];
  document.getElementById('paletteTitle').textContent = TAB_DEFS.find(function(t){return t.id===activeTab;}).label+' · Question palette';
  var body=document.getElementById('paletteBody');
  var html='';
  for(var i=0;i<slides.length;i++){
    html += '<div class="chip" data-status="'+statusOf(activeTab,i)+'" data-idx="'+i+'">'+(i+1)+'</div>';
  }
  body.innerHTML = html;
  body.querySelectorAll('.chip').forEach(function(chip){
    chip.addEventListener('click', function(){
      var i=parseInt(chip.dataset.idx,10);
      var ts=state.tabs[activeTab];
      if(i>ts.maxReached){ showToast('Finish the earlier questions to unlock this one.'); return; }
      ts.idx=i; closePalette(); renderSlide();
    });
  });
}
function openPalette(){ buildPalette(); overlay.classList.add('show'); }
function closePalette(){ overlay.classList.remove('show'); }
var toastTimer;
function showToast(msg){
  var t=document.getElementById('toast'); t.textContent=msg; t.classList.add('show');
  clearTimeout(toastTimer); toastTimer=setTimeout(function(){ t.classList.remove('show'); },2200);
}
document.getElementById('paletteFab').addEventListener('click', openPalette);
document.getElementById('paletteClose').addEventListener('click', closePalette);
overlay.addEventListener('click', function(e){ if(e.target===overlay) closePalette(); });

function updateFab(){
  var slides=SLIDES[activeTab];
  var done=state.tabs[activeTab].items.filter(function(it){ return it.status==='correct'||it.status==='revealed'||it.status==='skipped'; }).length;
  document.getElementById('fabBadge').textContent = done+'/'+slides.length;
}

/* ================= overall progress + finish ================= */
function updateChapterProgress(){
  var total=0, done=0;
  TAB_DEFS.forEach(function(td){
    state.tabs[td.id].items.forEach(function(it){
      total++;
      if(it.status==='correct'||it.status==='revealed'||it.status==='skipped') done++;
    });
  });
  var pct = total>0 ? Math.round((done/total)*100) : 0;
  document.getElementById('cpFill').style.width=pct+'%';
  document.getElementById('cpLabel').textContent=pct+'% complete';
}
function isSlideCorrect(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice===slide.correct;
  return slide.flat.every(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).every(function(k){ return answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr); });
  });
}
function isSlideAttempted(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return slide.flat.some(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).some(function(k){ return ss.inputs[k] && String(ss.inputs[k]).trim()!==''; });
  });
}
function slideLabel(slide){
  var t = slide.kind==='case' ? slide.title : (slide.kind==='ar' ? slide.a : (slide.p||slide.text||''));
  t = String(t);
  return t.length>90 ? t.slice(0,90)+'…' : t;
}
function slideAnswerText(slide, item, correctSide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
    if(correctSide) return '('+LETTERS[slide.correct]+') '+opts[slide.correct];
    return item.choice!==undefined ? '('+LETTERS[item.choice]+') '+opts[item.choice] : '— not attempted —';
  }
  return slide.flat.map(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).map(function(k){
      return correctSide ? step.a[k] : (ss.inputs[k] || '—');
    }).join(', ');
  }).join('  |  ');
}

function renderFinished(){
  if(MODE==='quiz'){ renderQuizResults(); return; }
  var ts=state.tabs[activeTab];
  var correct=ts.items.filter(function(i){return i.status==='correct';}).length;
  var revealed=ts.items.filter(function(i){return i.status==='revealed';}).length;
  var skipped=ts.items.filter(function(i){return i.status==='skipped';}).length;
  var h='<div class="qcard done-card"><div class="big">✨</div><h2>Tab complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">You worked through all '+ts.items.length+' questions in this tab.</p>';
  if(!ts.recorded){ ts.recorded=true; addRecord('learning', activeTab, correct, revealed, skipped, ts.items.length); }
  h+='<div class="score-pills">'+
     '<span class="score-pill" style="background:var(--success-soft);color:var(--success);">'+correct+' correct</span>'+
     '<span class="score-pill" style="background:var(--danger-soft);color:var(--danger);">'+revealed+' revealed</span>'+
     '<span class="score-pill" style="background:var(--gold-soft);color:var(--retry-text);">'+skipped+' skipped</span></div>'+
     '<button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; ts.recorded=false; renderSlide(); });
  updateChapterProgress(); saveState();
}

function renderQuizResults(){
  var ts=state.tabs[activeTab];
  var slides=SLIDES[activeTab];
  var correctCount=0, attemptedCount=0;
  var rowsHtml = slides.map(function(slide,i){
    var item=ts.items[i];
    var ok=isSlideCorrect(activeTab,i);
    var att=isSlideAttempted(activeTab,i);
    if(ok) correctCount++;
    if(att) attemptedCount++;
    var tagCls = ok?'ok':(att?'bad':'na');
    var tagText = ok?'Correct':(att?'Incorrect':'Not attempted');
    var row='<div class="review-row">';
    row+='<span class="review-tag '+tagCls+'">Q'+(i+1)+' · '+tagText+'</span>';
    row+='<div class="review-q">'+fr(esc(slideLabel(slide)))+'</div>';
    row+='<div class="review-ans"><b>Your answer:</b> '+fr(esc(slideAnswerText(slide,item,false)))+'</div>';
    if(!ok) row+='<div class="review-ans" style="color:var(--success);"><b>Correct answer:</b> '+fr(esc(slideAnswerText(slide,item,true)))+'</div>';
    if(slide.sol) row+='<details class="rev-sol"><summary>Show solution</summary>'+solutionHTML(slide)+'</details>';
    row+='</div>';
    return row;
  }).join('');

  addRecord('quiz', activeTab, correctCount, 0, 0, slides.length);
  var h='<div class="qcard done-card" style="text-align:left;">';
  h+='<div style="text-align:center;"><div class="big">🏁</div><h2>Quiz complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">'+correctCount+' / '+slides.length+' correct · '+attemptedCount+' attempted</p></div>';
  h+='<div style="margin-top:16px;">'+rowsHtml+'</div>';
  h+='<div style="text-align:center;margin-top:6px;"><button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  h+='</div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; renderSlide(); });
  playSuccess();
  updateChapterProgress(); saveState();
}

/* ================= mode picker ================= */
function updateModePill(){
  var btn=document.getElementById('modePill');
  if(!btn) return;
  btn.textContent = MODE==='quiz' ? '📝 Quiz Mode · switch' : '📘 Learning Sheet · switch';
}
function showChrome(show){
  document.querySelector('.tabbar').style.display = show?'':'none';
  document.querySelector('.slide-progress').style.display = show?'':'none';
  document.querySelector('.navbar').style.display = show?'':'none';
  document.getElementById('paletteFab').style.display = show?'':'none';
}
function renderModePicker(){
  showChrome(false);
  var wrap=document.getElementById('wrap');
  wrap.innerHTML =
    '<div class="mode-pick">'+
      '<div class="mode-card" data-pick="learning"><div class="mode-icon">📘</div><h3>Learning Sheet</h3>'+
      '<p>Work through each question step by step with instant feedback — two tries, then the correct value is revealed right there so you can keep moving.</p></div>'+
      '<div class="mode-card" data-pick="quiz"><div class="mode-icon">📝</div><h3>Quiz Mode</h3>'+
      '<p>Attempt every question with no hints along the way. Your score and the full answer key are shown together once you finish the tab — just like the real exam.</p></div>'+
    '</div>';
  wrap.querySelectorAll('[data-pick]').forEach(function(card){
    card.addEventListener('click', function(){
      setMode(card.dataset.pick); activeTab='theory';
      document.getElementById('modePill').hidden=false;
      updateModePill();
      showChrome(true);
      buildTabbar();
      renderSlide();
    });
  });
}
document.getElementById('modePill').addEventListener('click', function(){
  if(!appState.mode){ return; }
  setMode(appState.mode==='quiz' ? 'learning' : 'quiz');
  updateModePill();
  showChrome(true);
  buildTabbar();
  renderSlide();
});

/* ================= boot ================= */
var _origRenderSlide = renderSlide;
renderSlide = function(){ _origRenderSlide(); updateChapterProgress(); updateModePill(); };


/* ================= theory tab ================= */
function renderTheory(){
  var wrap=document.getElementById('wrap');
  wrap.innerHTML='<div class="theory">'+fr(THEORY)+'</div>';
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Theory — notes, key ideas and worked examples';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
  wrap.querySelectorAll('[data-jump]').forEach(function(b){ b.addEventListener('click',function(){ var t=document.getElementById(b.dataset.jump); if(t) t.scrollIntoView({behavior:'smooth',block:'start'}); }); });
}
var _rsTheory = renderSlide;
renderSlide = function(){
  if(activeTab==='theory'){ renderTheory(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  document.querySelector('.slide-progress').style.display='';
  document.querySelector('.navbar').style.display='';
  document.getElementById('paletteFab').style.display='';
  _rsTheory();
};


/* ================= MYP criterion report ================= */
var CRIT_COL={A:'var(--accent-text)',B:'var(--gold)',C:'#8A6FC4',D:'#2F9AA0'};
function critStats(){
  return REPORT.map(function(r){
    var tabId=r[0], slides=SLIDES[tabId], ts=state&&state.tabs[tabId];
    var c=0,w=0,n=0;
    slides.forEach(function(sl,i){
      if(!ts||!ts.items[i]){ n++; return; }
      var it=ts.items[i];
      if(MODE==='learning'){
        if(it.status==='correct') c++; else if(it.status==='revealed') w++; else n++;
      } else {
        if(isSlideCorrect(tabId,i)) c++; else if(isSlideAttempted(tabId,i)) w++; else n++;
      }
    });
    var tot=slides.length, pct=tot?Math.round(c/tot*100):0;
    var lvl=Math.round(c/Math.max(tot,1)*8);
    return {tab:tabId,letter:r[1],name:r[2],c:c,w:w,n:n,tot:tot,pct:pct,lvl:lvl};
  });
}
function arcPath(cx,cy,r,a0,a1){
  if(a1-a0>=Math.PI*2-1e-6){ return 'M'+(cx-r)+','+cy+' a'+r+','+r+' 0 1,0 '+(2*r)+',0 a'+r+','+r+' 0 1,0 '+(-2*r)+',0 Z'; }
  var x0=cx+r*Math.sin(a0), y0=cy-r*Math.cos(a0), x1=cx+r*Math.sin(a1), y1=cy-r*Math.cos(a1);
  return 'M'+cx+','+cy+' L'+x0.toFixed(2)+','+y0.toFixed(2)+' A'+r+','+r+' 0 '+((a1-a0)>Math.PI?1:0)+',1 '+x1.toFixed(2)+','+y1.toFixed(2)+' Z';
}
function pieSVG(parts,size,hole,center,sub){
  var tot=parts.reduce(function(s,p){return s+p.v;},0), cx=size/2, cy=size/2, r=size/2-4, a=0, h='';
  if(!tot){ h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+r+'" style="fill:var(--paper-2);stroke:var(--rule)"/>'; }
  parts.forEach(function(p){ if(!p.v) return; var b=a+p.v/tot*Math.PI*2; h+='<path d="'+arcPath(cx,cy,r,a,b)+'" style="fill:'+p.col+';stroke:var(--card);stroke-width:2"><title>'+esc(p.label)+': '+p.v+'</title></path>'; a=b; });
  if(hole) h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+(r*hole)+'" style="fill:var(--card)"/>';
  if(center) h+='<text x="'+cx+'" y="'+(cy-(sub?4:0))+'" text-anchor="middle" dominant-baseline="middle" style="fill:var(--ink);font:700 '+(size>150?22:15)+'px Fraunces,serif">'+center+'</text>';
  if(sub) h+='<text x="'+cx+'" y="'+(cy+16)+'" text-anchor="middle" style="fill:var(--ink-soft);font:600 11px \'Source Sans 3\',sans-serif">'+sub+'</text>';
  return '<svg viewBox="0 0 '+size+' '+size+'" width="'+size+'" height="'+size+'" role="img">'+h+'</svg>';
}
function band(l){ return l===0?'0':(l<=2?'1–2':(l<=4?'3–4':(l<=6?'5–6':'7–8'))); }
function renderReport(){
  var st=critStats(), totC=0, totQ=0;
  st.forEach(function(s){ totC+=s.c; totQ+=s.tot; });
  var h='<div class="theory">';
  h+='<section class="note"><h2>Chapter test report</h2><p class="lt">'+(MODE==='quiz'?'<b>Quiz mode</b> results: answers are marked when you finish each test tab.':'<b>Learning mode</b> results: a question counts as correct only if you got it without the answer being revealed. Switch to Quiz mode for an exam-style score.')+'</p>';
  h+='<div class="rep-top"><div class="rep-pie">'+pieSVG(st.map(function(s){return {v:s.c,col:CRIT_COL[s.letter],label:'Criterion '+s.letter};}),200,0.55,totC+'/'+totQ,'correct')+'</div>';
  h+='<div class="rep-legend"><div class="rep-cap">Marks earned, by criterion</div>'+st.map(function(s){ return '<div class="rep-li"><span class="sw" style="background:'+CRIT_COL[s.letter]+'"></span><b>'+s.letter+'</b>&nbsp;'+esc(s.name)+'<span class="rep-n">'+s.c+' / '+s.tot+'</span></div>'; }).join('')+'</div></div></section>';
  h+='<div class="rep-grid">'+st.map(function(s){
    return '<div class="rep-card"><div class="rep-h"><span class="crit" style="background:'+CRIT_COL[s.letter]+'">'+s.letter+'</span>'+esc(s.name)+'</div>'+
      '<div class="rep-row">'+pieSVG([{v:s.c,col:'var(--success)',label:'Correct'},{v:s.w,col:'var(--danger)',label:'Incorrect'},{v:s.n,col:'var(--locked)',label:'Not attempted'}],120,0.5,s.pct+'%')+
      '<div class="rep-stats"><div><span class="sw" style="background:var(--success)"></span>Correct <b>'+s.c+'</b></div><div><span class="sw" style="background:var(--danger)"></span>Incorrect <b>'+s.w+'</b></div><div><span class="sw" style="background:var(--locked)"></span>Not attempted <b>'+s.n+'</b></div>'+
      '<div class="rep-lvl">Indicative level <b>'+s.lvl+'</b> / 8 <span>(band '+band(s.lvl)+')</span></div></div></div>'+
      '<button class="hub-btn" data-go="'+s.tab+'">Open Test '+s.letter+' →</button></div>';
  }).join('')+'</div>';
  h+='<p class="rep-note">Indicative level = fraction of questions correct × 8, rounded. It is a practice guide only; your teacher awards the real MYP criterion levels.</p></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Report — chapter test results by MYP criterion';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
}
var _rsReport = renderSlide;
renderSlide = function(){
  if(activeTab==='report'){ renderReport(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  _rsReport();
};


/* ================= skipped-question return ================= */
function firstSkippedIdx(ts){
  for(var i=0;i<ts.items.length;i++){
    var it=ts.items[i];
    if(MODE==='quiz'){ if(!isSlideAttempted(activeTab,i)) return i; }
    else if(it.status==='skipped'||it.status==='unanswered') return i;
  }
  return -1;
}
function skippedList(ts){
  var out=[]; for(var i=0;i<ts.items.length;i++){ var it=ts.items[i];
    if(MODE==='quiz' ? !isSlideAttempted(activeTab,i) : (it.status==='skipped'||it.status==='unanswered')) out.push(i); }
  return out;
}
function renderSkipGate(ts){
  var list=skippedList(ts);
  var h='<div class="qcard done-card"><div class="big">⏭️</div><h2>You skipped '+list.length+' question'+(list.length>1?'s':'')+'</h2>'+
    '<p style="color:var(--ink-soft);font-size:14px;">Answer them now, or submit the tab as it is. Answers are marked only when you submit.</p>'+
    '<div class="skip-chips">'+list.map(function(i){ return '<button class="chip skip-go" data-i="'+i+'">Q'+(i+1)+'</button>'; }).join('')+'</div>'+
    '<div class="skip-btns"><button class="btn btn-primary" id="btnGoSkipped">Answer skipped questions</button><button class="btn" id="btnSubmitAnyway">Submit anyway</button></div></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  var go=function(i){ ts.idx=i; ts.maxReached=Math.max(ts.maxReached,i); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); };
  document.getElementById('btnGoSkipped').addEventListener('click',function(){ go(list[0]); });
  document.querySelectorAll('.skip-go').forEach(function(b){ b.addEventListener('click',function(){ go(parseInt(b.dataset.i,10)); }); });
  document.getElementById('btnSubmitAnyway').addEventListener('click',function(){ ts.submitOK=true; renderFinished(); ts.submitOK=false; });
  saveState();
}
var _rfSkip = renderFinished;
renderFinished = function(){
  var ts=state.tabs[activeTab];
  if(MODE==='quiz' && !ts.submitOK && skippedList(ts).length){ renderSkipGate(ts); return; }
  _rfSkip();
  if(MODE==='learning'){
    var list=skippedList(ts);
    if(list.length){
      var card=document.querySelector('.done-card');
      if(card){ var d=document.createElement('div'); d.className='skip-btns';
        d.innerHTML='<button class="btn btn-primary" id="btnGoSkipped">Attempt skipped questions ('+list.length+')</button>';
        card.appendChild(d);
        document.getElementById('btnGoSkipped').addEventListener('click',function(){ ts.idx=list[0]; ts.recorded=false; renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); }
    }
  }
};


/* ================= approx answers (physics: within 1%) ================= */
var _amTools = answerMatches;
answerMatches = function(input,answer,accept,expr){
  if(expr==='approx'){
    var n1=parseNum(input); if(n1===null) return false;
    return [answer].concat(accept||[]).some(function(a){ var n2=parseNum(a); if(n2===null) return false; return Math.abs(n1-n2) <= Math.max(0.011*Math.abs(n2), 1e-9); });
  }
  return _amTools(input,answer,accept,expr);
};

/* ================= palette: every question already reached can be opened ================= */
statusOf = function(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  var s=ts.items[i].status;
  if(s==='unanswered') return 'skipped';
  return s;
};

/* ================= on-screen keyboard ================= */
var KB={on:true, page:'num', target:null};
try{ var kbp=localStorage.getItem('sopaan-kb-pref'); if(kbp==='off') KB.on=false; }catch(e){}
function kbSave(){ try{ localStorage.setItem('sopaan-kb-pref', KB.on?'on':'off'); }catch(e){} }
var KEYS_NUM=[['7','8','9','(',')'],['4','5','6','−','/'],['1','2','3','.',','],['0',':','%','°','⌫'],['abc','←','→','Clear','Done']];
var KEYS_FRAC=[['7','8','9','/'],['4','5','6','−'],['1','2','3','space'],['0',',','⌫','Clear'],['abc','←','→','Done']];
var KEYS_ALG=[['7','8','9','x','y','n'],['4','5','6','+','−','a'],['1','2','3','×','÷','b'],['0','.','(',')','^','²'],['/','π','|','t','⌫','Clear'],['abc','←','→','Done']];
function kbLayoutFor(inp){ try{ var ts=state.tabs[activeTab], sl=SLIDES[activeTab][ts.idx], stp=sl.flat[parseInt(inp.dataset.step,10)]; var m=stp.expr, key=String(stp.a[inp.dataset.bkey]||'');
  if(m==='words'||m==='glist'||m==='angle') return 'abc'; if(m===true||m==='pow'||m==='primes') return 'alg';
  if(['fv','fe','fl','fm','fi','flist'].indexOf(m)>=0) return 'frac'; if(m==='dec'||m==='approx'||m==='dlist'||m==='set'||m==='coord'||m==='list'||m==='time') return 'num';
  if(/^[\-−]?\d+ \d+\/\d+$/.test(key)||/^[\-−]?\d+\/\d+$/.test(key)) return 'frac'; if(/[a-df-z]/i.test(key.replace(/pi/gi,''))) return /\d/.test(key)?'alg':'abc'; return 'num'; }catch(e){ return 'num'; } }
var KEYS_CALC=[['7','8','9','x','(',')'],['4','5','6','+','−','^'],['1','2','3','×','/','²'],['0','.','e','eˣ','√','ln'],['sin','cos','tan','t','⌫','Clear'],['abc','←','→','Done']];
var KEYS_ABC=[['q','w','e','r','t','y','u','i','o','p'],['a','s','d','f','g','h','j','k','l','-'],['z','x','c','v','b','n','m',',',':','/'],['123','space','←','→','⌫','Done']];
function kbBuild(){
  var el=document.getElementById('vkb'); if(!el){ el=document.createElement('div'); el.id='vkb'; el.className='vkb'; document.body.appendChild(el);
    el.addEventListener('pointerdown',function(e){ var b=e.target.closest('button[data-k]'); e.preventDefault(); if(b) kbPress(b.dataset.k); });
    el.addEventListener('mousedown',function(e){ e.preventDefault(); }); }
  var rows={num:KEYS_NUM,frac:KEYS_FRAC,alg:KEYS_ALG,abc:KEYS_ABC,calc:KEYS_CALC}[KB.page]||KEYS_NUM;
  el.innerHTML='<div class="vkb-top"><span>⌨️ Keyboard</span><button data-k="device" class="vkb-link">Use device keyboard</button></div>'+
    rows.map(function(r){ return '<div class="vkb-row">'+r.map(function(k){
      var cls='vkb-k'+(/^(abc|123|Done|Clear|⌫|←|→|space)$/.test(k)?' fn':'')+(k==='Done'?' done':'');
      return '<button type="button" class="'+cls+'" data-k="'+k+'">'+(k==='space'?'space':k)+'</button>'; }).join('')+'</div>'; }).join('');
  return el;
}
function kbShow(inp){ KB.target=inp; KB.home=kbLayoutFor(inp); KB.page=KB.home; if(!KB.on) return; var cp=document.getElementById('calcPanel'); if(cp&&cp.classList.contains('show')) return; var el=kbBuild(); var nb=document.querySelector('.navbar'); el.style.bottom=(nb?nb.getBoundingClientRect().height:0)+'px'; el.classList.add('show'); document.body.classList.add('kb-open'); setTimeout(function(){ var r=inp.getBoundingClientRect(), top=el.getBoundingClientRect().top; if(r.bottom>top-12) window.scrollBy({top:r.bottom-top+60,behavior:'smooth'}); else if(r.top<60) window.scrollBy({top:r.top-80,behavior:'smooth'}); },30); }
function kbHide(){ var el=document.getElementById('vkb'); if(el) el.classList.remove('show'); document.body.classList.remove('kb-open'); }
function kbInsert(s){
  var t=KB.target; if(!t||t.disabled) return;
  var a=t.selectionStart==null?t.value.length:t.selectionStart, b=t.selectionEnd==null?a:t.selectionEnd;
  t.value=t.value.slice(0,a)+s+t.value.slice(b); var p=a+s.length; try{ t.setSelectionRange(p,p); }catch(e){}
  t.dispatchEvent(new Event('input',{bubbles:true}));
}
function kbPress(k){
  var t=KB.target; if(k==='device'){ KB.on=false; kbSave(); kbHide(); document.querySelectorAll('.blank-input').forEach(function(i){ i.removeAttribute('inputmode'); }); if(t){ t.blur(); setTimeout(function(){ t.focus(); },50); } showToast('Device keyboard on. Tap ⌨️ on any blank to bring the on-screen keyboard back.'); return; }
  if(k==='Done'){ kbHide(); if(t) t.blur(); return; }
  if(k==='abc'||k==='123'){ KB.page=(k==='abc')?'abc':(KB.home&&KB.home!=='abc'?KB.home:'num'); kbBuild(); return; }
  if(!t) return;
  if(k==='⌫'){ var a=t.selectionStart, b=t.selectionEnd; if(a===b&&a>0){ t.value=t.value.slice(0,a-1)+t.value.slice(b); try{t.setSelectionRange(a-1,a-1);}catch(e){} } else { t.value=t.value.slice(0,a)+t.value.slice(b); try{t.setSelectionRange(a,a);}catch(e){} } t.dispatchEvent(new Event('input',{bubbles:true})); return; }
  if(k==='Clear'){ t.value=''; t.dispatchEvent(new Event('input',{bubbles:true})); return; }
  if(k==='←'||k==='→'){ var p=(t.selectionStart||0)+(k==='←'?-1:1); p=Math.max(0,Math.min(t.value.length,p)); try{t.setSelectionRange(p,p);}catch(e){} return; }
  var map={'−':'-','space':' '}; kbInsert(map[k]!==undefined?map[k]:k);
}
function kbWire(){
  document.querySelectorAll('.blank-input').forEach(function(inp){
    if(inp.dataset.kb) return; inp.dataset.kb='1';
    if(KB.on) inp.setAttribute('inputmode','none');
    inp.addEventListener('focus',function(){ kbShow(inp); });
    var btn=document.createElement('button'); btn.type='button'; btn.className='kb-toggle'; btn.title='On-screen keyboard'; btn.textContent='⌨️'; btn.tabIndex=-1;
    btn.addEventListener('mousedown',function(e){ e.preventDefault(); });
    btn.addEventListener('click',function(){ KB.on=true; kbSave(); document.querySelectorAll('.blank-input').forEach(function(i){ i.setAttribute('inputmode','none'); }); inp.focus(); kbShow(inp); });
    if(!inp.disabled) inp.insertAdjacentElement('afterend',btn);
  });
}
document.addEventListener('focusout',function(e){ setTimeout(function(){ var a=document.activeElement; if(!a||!a.classList||!a.classList.contains('blank-input')){ if(!document.querySelector('#vkb:hover')) kbHide(); } },120); });

/* ================= calculator ================= */
var CALC={expr:'', ans:0};
function calcEval(src){
  var s=String(src).replace(/×/g,'*').replace(/÷/g,'/').replace(/−/g,'-').replace(/π/g,'(PI)').replace(/Ans/g,'('+CALC.ans+')').replace(/\^/g,'**').replace(/√\(/g,'sqrt(').replace(/²/g,'**2').replace(/E/g,'*10**');
  s=s.replace(/(^|[(*\/+\-,])-/g,'$1(-1)*');
  if(/[^0-9+\-*/().,a-z A-Z]/.test(s)) throw 0;
  var ok=s.replace(/sin|cos|tan|asin|acos|atan|sqrt|log|ln|PI|abs/g,''); if(/[a-zA-Z]/.test(ok)) throw 0;
  var f=new Function('sin','cos','tan','asin','acos','atan','sqrt','log','ln','PI','abs','return ('+s+');');
  var d=Math.PI/180;
  var v=f(function(x){return Math.sin(x*d);},function(x){return Math.cos(x*d);},function(x){return Math.tan(x*d);},
    function(x){return Math.asin(x)/d;},function(x){return Math.acos(x)/d;},function(x){return Math.atan(x)/d;},Math.sqrt,Math.log10,Math.log,Math.PI,Math.abs);
  if(typeof v!=='number'||!isFinite(v)) throw 0; return v;
}
function fmtNum(v){ if(Math.abs(v)>=1e10||(Math.abs(v)<1e-6&&v!==0)) return v.toExponential(6).replace(/\.?0+e/,'e'); return String(parseFloat(v.toPrecision(10))); }
var CKEYS=[['sin(','cos(','tan(','√(','^','²'],['asin(','acos(','atan(','(',')','π'],['7','8','9','÷','⌫','AC'],['4','5','6','×','E','Ans'],['1','2','3','−','(−)','Insert'],['0','.','=','+']];
function openCalc(){
  var p=document.getElementById('calcPanel');
  if(!p){ p=document.createElement('div'); p.id='calcPanel'; p.className='tool-panel calc';
    p.innerHTML='<div class="tp-head"><b>🧮 Calculator</b><span class="tp-note">degrees · tap a blank, then Insert</span><button class="tp-x" data-c="close">✕</button></div>'+
      '<div class="calc-disp"><div class="calc-expr" id="calcExpr"></div><div class="calc-res" id="calcRes">0</div></div>'+
      '<div class="calc-keys">'+CKEYS.map(function(r){ return r.map(function(k){ return '<button type="button" data-c="'+k+'" class="'+(k==='='?'eq':(/^(AC|⌫|Insert)$/.test(k)?'fn':''))+'">'+({'sin(':'sin','cos(':'cos','tan(':'tan','asin(':'sin⁻¹','acos(':'cos⁻¹','atan(':'tan⁻¹','√(':'√'}[k]||k)+'</button>'; }).join(''); }).join('')+'</div>';
    document.body.appendChild(p);
    p.addEventListener('mousedown',function(e){ if(e.target.closest('button')) e.preventDefault(); });
    p.addEventListener('click',function(e){ var b=e.target.closest('button[data-c]'); if(!b) return; calcKey(b.dataset.c); });
  }
  kbHide(); var nb=document.querySelector('.navbar'); p.style.bottom=((nb?nb.getBoundingClientRect().height:0)+8)+'px'; p.classList.add('show'); document.body.classList.add('calc-open'); calcShow();
}
function calcShow(res){ document.getElementById('calcExpr').textContent=CALC.expr||' '; if(res!==undefined) document.getElementById('calcRes').textContent=res; }
function calcKey(k){
  if(k==='close'){ document.getElementById('calcPanel').classList.remove('show'); document.body.classList.remove('calc-open'); return; }
  if(k==='AC'){ CALC.expr=''; calcShow('0'); return; }
  if(k==='⌫'){ CALC.expr=CALC.expr.replace(/(asin\(|acos\(|atan\(|sin\(|cos\(|tan\(|√\(|Ans|.)$/,''); calcShow(); return; }
  if(k==='='){ try{ var v=calcEval(CALC.expr); CALC.ans=v; calcShow(fmtNum(v)); }catch(e){ calcShow('Error'); } return; }
  if(k==='(−)'){ CALC.expr+='−'; calcShow(); return; }
  if(k==='Insert'){ var r=document.getElementById('calcRes').textContent; if(KB.target && !KB.target.disabled && r!=='Error'){ KB.target.value=r; KB.target.dispatchEvent(new Event('input',{bubbles:true})); showToast('Inserted '+r+' into the blank.'); } else showToast('Tap a blank first, then press Insert.'); return; }
  CALC.expr+=k; calcShow();
}

/* ================= Desmos ================= */
var DESMOS_KEY='dcb31709b452b1cf9dc26972add0fda6', desmosCalc=null, desmosLoading=false;
function openDesmos(exprs){
  var p=document.getElementById('desmosPanel');
  if(!p){ p=document.createElement('div'); p.id='desmosPanel'; p.className='tool-panel desmos';
    p.innerHTML='<div class="tp-head"><b>📈 Desmos graphing calculator</b><button class="tp-x" id="desmosReset">Reset graph</button><button class="tp-x" id="desmosClose">✕</button></div><div id="desmosBox"><div class="desmos-msg">Loading Desmos…</div></div>';
    document.body.appendChild(p);
    document.getElementById('desmosClose').addEventListener('click',function(){ p.classList.remove('show'); });
    document.getElementById('desmosReset').addEventListener('click',function(){ if(desmosCalc) desmosSet(p._exprs||[]); });
  }
  p._exprs=exprs||[]; p.classList.add('show');
  if(window.Desmos){ desmosInit(); desmosSet(p._exprs); return; }
  if(desmosLoading) return; desmosLoading=true;
  var sc=document.createElement('script'); sc.src='https://www.desmos.com/api/v1.9/calculator.js?apiKey='+DESMOS_KEY;
  sc.onload=function(){ desmosInit(); desmosSet(p._exprs); };
  sc.onerror=function(){ desmosLoading=false; document.getElementById('desmosBox').innerHTML='<div class="desmos-msg">Desmos needs an internet connection. <a href="https://www.desmos.com/calculator" target="_blank" rel="noopener">Open Desmos in a new tab</a>.</div>'; };
  document.head.appendChild(sc);
}
function desmosInit(){ if(desmosCalc) return; var box=document.getElementById('desmosBox'); box.innerHTML=''; desmosCalc=Desmos.GraphingCalculator(box,{expressionsCollapsed:false,settingsMenu:false,border:false,degreeMode:true}); }
function desmosSet(exprs){ if(!desmosCalc) return; desmosCalc.setBlank(); (exprs||[]).forEach(function(e,i){ if(typeof e==='string') desmosCalc.setExpression({id:'e'+i,latex:e}); else desmosCalc.setExpression(Object.assign({id:'e'+i},e)); }); }

/* ================= toolbar on each question ================= */
var _rsTools = renderSlide;
renderSlide = function(){
  _rsTools();
  kbHide();
  if(activeTab==='theory'||activeTab==='report') return;
  var ts=state.tabs[activeTab]; if(!ts) return;
  var slide=SLIDES[activeTab][ts.idx], card=document.querySelector('#wrap .qcard');
  if(card && slide && slide.tools && slide.tools.length && !card.querySelector('.tool-bar')){
    var bar=document.createElement('div'); bar.className='tool-bar';
    bar.innerHTML=(slide.tools.indexOf('calc')>=0?'<button type="button" class="tool-btn" data-t="calc">🧮 Calculator</button>':'')+
                  (slide.tools.indexOf('desmos')>=0?'<button type="button" class="tool-btn" data-t="desmos">📈 Desmos graph</button>':'');
    card.insertBefore(bar, card.firstChild);
    bar.addEventListener('click',function(e){ var b=e.target.closest('[data-t]'); if(!b) return; if(b.dataset.t==='calc') openCalc(); else openDesmos(slide.desmos||[]); });
  }
  kbWire();
};



/* ================= signed fractions in text: {−3/4}, {3/−4}, {−2 1/3}, {p/q} ================= */
fr = function(s){ return String(s).replace(/&lt;(\/?)(b|sup|sub|i)>/g,'<$1$2>')
  .replace(/\{([−-]?\d+) (\d+)\/(\d+)\}/g,'<span class="mx">$1<span class="fq"><span>$2</span><span>$3</span></span></span>')
  .replace(/\{([−-]?[A-Za-z0-9]+)\/([−-]?[A-Za-z0-9]+)\}/g,'<span class="fq"><span>$1</span><span>$2</span></span>'); };
/* ================= BMA badge → main page of this sheet (theory hub) ================= */
(function(){
  var c=document.querySelector('.brand .crest'); if(!c) return;
  var a=document.createElement('button'); a.type='button'; a.className='crest crest-home'; a.title='Back to the main page (theory notes)'; a.setAttribute('aria-label','Main page');
  a.innerHTML='<img src="data:image/png;base64,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" alt="Brain and Mind"><span class="crest-h">⌂</span>'; c.parentNode.replaceChild(a,c);
  a.addEventListener('click',function(){
    if(!student||!MODE){ window.scrollTo({top:0,behavior:'smooth'}); return; }
    if(typeof kbHide==='function') kbHide(); if(typeof SP!=='undefined'&&SP.on) spToggle(false);
    var cp=document.getElementById('calcPanel'); if(cp) cp.classList.remove('show'); document.body.classList.remove('calc-open');
    activeTab = (typeof THEORY!=='undefined') ? 'theory' : TAB_DEFS[0].id;
    buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'});
  });
})();

/* ================= session clock: starts at sign-in and keeps running ================= */
var CLK={t0:null};
(function(){
  var row=document.querySelector('.brand .brand-row'); if(!row) return;
  var d=document.createElement('div'); d.className='bmasw off'; d.id='swBox'; d.title='Time since you signed in';
  d.innerHTML='<span class="bmasw-ic">⏱</span><span class="bmasw-t" id="swT">00:00</span>';
  row.appendChild(d); setInterval(swTick,1000);
})();
function swFmt(s){ s=Math.floor(s||0); var h=Math.floor(s/3600), m=Math.floor(s%3600/60), x=s%60; return (h?h+':'+String(m).padStart(2,'0'):String(m).padStart(2,'0'))+':'+String(x).padStart(2,'0'); }
function swTick(){
  var box=document.getElementById('swBox'); if(!box) return;
  if(student && !CLK.t0) CLK.t0=Date.now();
  if(!student) CLK.t0=null;
  box.classList.toggle('off',!CLK.t0);
  document.getElementById('swT').textContent = CLK.t0 ? swFmt((Date.now()-CLK.t0)/1000) : '00:00';
}
function swTab(){ try{ if(!state||!state.tabs||activeTab==='theory'||activeTab==='report') return null; return state.tabs[activeTab]||null; }catch(e){ return null; } }
var _rsSW = renderSlide;
renderSlide = function(){ _rsSW(); swTick(); };
var _rfSW = renderFinished;
renderFinished = function(){ _rfSW(); var done=document.querySelector('#wrap .done-card'); if(CLK.t0 && done && !done.querySelector('.bmasw-done')){ var p=document.createElement('p'); p.className='bmasw-done'; p.textContent='⏱ Time since sign-in: '+swFmt((Date.now()-CLK.t0)/1000); var h2=done.querySelector('h2'); if(h2) h2.insertAdjacentElement('afterend',p); else done.appendChild(p); } };

/* ================= scratchpad (Khan-style, 4 pens) ================= */
var SP={on:false, draw:true, color:'ink', size:3, erase:false, store:{}, key:null, cv:null, ctx:null, down:false, last:null};
var SP_COL={ink:null, blue:'#2563EB', red:'#DC2626', green:'#16A34A'};
function spInk(){ return getComputedStyle(document.documentElement).getPropertyValue('--ink').trim()||'#211E1A'; }
function spKey(){ var ts=swTab(); return ts ? (MODE+'|'+activeTab+'|'+ts.idx) : null; }
function spBuild(){
  if(SP.cv) return;
  var cv=document.createElement('canvas'); cv.id='spCanvas'; cv.className='sp-canvas'; document.body.appendChild(cv);
  var bar=document.createElement('div'); bar.id='spBar'; bar.className='sp-bar';
  bar.innerHTML='<span class="sp-lbl">✏️ Scratchpad</span>'+
    ['ink','blue','red','green'].map(function(c){ return '<button type="button" class="sp-pen" data-c="'+c+'" title="'+(c==='ink'?'black':c)+' pen"><i style="background:'+(c==='ink'?'var(--ink)':SP_COL[c])+'"></i></button>'; }).join('')+
    '<button type="button" class="sp-tool" data-t="erase" title="Eraser">🧽</button><button type="button" class="sp-tool" data-t="size" title="Pen size">●</button><button type="button" class="sp-tool" data-t="scroll" title="Pause drawing to scroll or answer">✋</button>'+
    '<button type="button" class="sp-tool" data-t="clear" title="Clear page">🗑</button><button type="button" class="sp-tool" data-t="close" title="Close scratchpad">✕</button>';
  document.body.appendChild(bar);
  bar.addEventListener('click',function(e){ var b=e.target.closest('button'); if(!b) return;
    if(b.dataset.c){ SP.color=b.dataset.c; SP.erase=false; SP.draw=true; }
    else if(b.dataset.t==='erase'){ SP.erase=true; SP.draw=true; }
    else if(b.dataset.t==='size'){ SP.size = SP.size===3?6:(SP.size===6?1.5:3); b.textContent = SP.size===6?'⬤':(SP.size===1.5?'·':'●'); }
    else if(b.dataset.t==='scroll'){ SP.draw=!SP.draw; }
    else if(b.dataset.t==='clear'){ spClear(); }
    else if(b.dataset.t==='close'){ spToggle(false); return; }
    spUI(); });
  SP.cv=cv; SP.ctx=cv.getContext('2d');
  cv.addEventListener('pointerdown',function(e){ if(!SP.draw) return; e.preventDefault(); cv.setPointerCapture(e.pointerId); SP.down=true; SP.last=spPt(e); spDot(SP.last); });
  cv.addEventListener('pointermove',function(e){ if(!SP.down) return; e.preventDefault(); var p=spPt(e); spLine(SP.last,p); SP.last=p; });
  var up=function(){ if(SP.down){ SP.down=false; spSave(); } };
  cv.addEventListener('pointerup',up); cv.addEventListener('pointercancel',up); cv.addEventListener('pointerleave',up);
  window.addEventListener('resize',function(){ if(SP.on) spFit(true); });
}
function spPt(e){ var r=SP.cv.getBoundingClientRect(); return {x:e.clientX-r.left, y:e.clientY-r.top}; }
function spStyle(){ var c=SP.ctx; c.lineCap='round'; c.lineJoin='round'; c.globalCompositeOperation=SP.erase?'destination-out':'source-over'; c.strokeStyle=c.fillStyle=(SP.color==='ink'?spInk():SP_COL[SP.color]); c.lineWidth=SP.erase?22:SP.size; }
function spDot(p){ spStyle(); var c=SP.ctx; c.beginPath(); c.arc(p.x,p.y,(SP.erase?11:SP.size/2),0,Math.PI*2); c.fill(); }
function spLine(a,b){ spStyle(); var c=SP.ctx; c.beginPath(); c.moveTo(a.x,a.y); c.lineTo(b.x,b.y); c.stroke(); }
function spSave(){ if(SP.key&&SP.cv){ try{ SP.store[SP.key]=SP.cv.toDataURL(); }catch(e){} } }
function spClear(){ if(!SP.ctx) return; SP.ctx.save(); SP.ctx.setTransform(1,0,0,1,0,0); SP.ctx.clearRect(0,0,SP.cv.width,SP.cv.height); SP.ctx.restore(); if(SP.key) delete SP.store[SP.key]; }
function spFit(keep){
  var wrap=document.getElementById('wrap'); if(!wrap||!SP.cv) return;
  var r=wrap.getBoundingClientRect(), top=r.top+window.scrollY, h=Math.max(wrap.scrollHeight, window.innerHeight-r.top)+40, w=document.documentElement.clientWidth;
  var old=keep&&SP.key?SP.store[SP.key]:null, dpr=window.devicePixelRatio||1;
  SP.cv.style.top=top+'px'; SP.cv.style.left='0px'; SP.cv.style.width=w+'px'; SP.cv.style.height=h+'px';
  SP.cv.width=Math.round(w*dpr); SP.cv.height=Math.round(h*dpr); SP.ctx.setTransform(dpr,0,0,dpr,0,0);
  if(old){ var im=new Image(); im.onload=function(){ SP.ctx.save(); SP.ctx.setTransform(1,0,0,1,0,0); SP.ctx.drawImage(im,0,0); SP.ctx.restore(); }; im.src=old; }
}
function spLoad(){ spSave(); SP.key=spKey(); spFit(false); var d=SP.key&&SP.store[SP.key]; if(d){ var im=new Image(); im.onload=function(){ SP.ctx.save(); SP.ctx.setTransform(1,0,0,1,0,0); SP.ctx.drawImage(im,0,0); SP.ctx.restore(); }; im.src=d; } }
function spUI(){
  var bar=document.getElementById('spBar'); if(!bar) return;
  bar.querySelectorAll('.sp-pen').forEach(function(b){ b.classList.toggle('on', !SP.erase && SP.draw && b.dataset.c===SP.color); });
  bar.querySelector('[data-t=erase]').classList.toggle('on', SP.erase && SP.draw);
  bar.querySelector('[data-t=scroll]').classList.toggle('on', !SP.draw);
  SP.cv.classList.toggle('passive', !SP.draw);
  var fb=document.getElementById('spFab'); if(fb) fb.classList.toggle('on',SP.on);
}
function spToggle(on){
  spBuild(); SP.on=(on===undefined)?!SP.on:on;
  document.body.classList.toggle('sp-open',SP.on); SP.cv.style.display=SP.on?'block':'none'; document.getElementById('spBar').style.display=SP.on?'flex':'none';
  if(SP.on){ SP.draw=true; SP.erase=false; spLoad(); if(typeof kbHide==='function') kbHide(); } else { spSave(); }
  spUI();
}
(function(){ var f=document.createElement('button'); f.type='button'; f.id='spFab'; f.className='sp-fab'; f.innerHTML='✏️ <span>Scratchpad</span>'; f.title='Open a scratchpad to write your working'; f.addEventListener('click',function(){ spToggle(); }); document.body.appendChild(f); })();
var _rsSP = renderSlide;
renderSlide = function(){ _rsSP(); var fab=document.getElementById('spFab'); var q=!!swTab(); if(fab) fab.style.display=q?'':'none'; if(!q && SP.on) spToggle(false); if(SP.on) setTimeout(spLoad,30); };


/* ================= v4: attempt classification ================= */
function v4Class(item){
  if(!item) return 'un';
  if(item.status==='skipped') return 'sk';
  if(item.status==='unanswered') return 'un';
  if(item.stepStates){ // step question
    if(item.status!=='correct' && item.status!=='revealed') return 'un';
    if(item.status==='revealed' || item.stepStates.some(function(s){return s.status==='revealed';})) return 'w';
    return item.stepStates.some(function(s){return (s.attempts||0)>0;}) ? 'c2' : 'c1';
  }
  if(item.status==='revealed') return 'w';
  if(item.status==='correct') return (item.attempts||0)>0 ? 'c2' : 'c1';
  if(item.status==='wrong'||item.status==='incorrect') return 'w';
  return 'un';
}
function v4Counts(items){ var c={n:items.length,c1:0,c2:0,w:0,sk:0,un:0}; items.forEach(function(it){ c[v4Class(it)]++; }); return c; }

/* ================= v4: My record with attempt columns ================= */
renderRecord = function(){
  showChrome(false);
  var tabLabel=function(id){ var t=TAB_DEFS.find(function(x){return x.id===id;}); return t?t.label:id; };
  var title=(document.querySelector('.chapter-title')||{}).textContent||'';
  var h='<div class="rec-card"><h2>'+esc(student.name)+'’s record</h2><p class="rec-empty" style="margin:0 0 12px;">Roll '+esc(student.roll)+(title?' · '+esc(title):'')+'</p>';
  var st=appState.learning;
  h+='<h3>📘 Learning Sheet — attempts</h3><p class="v4leg"><b>1st ✓</b> correct on the first attempt · <b>2nd ✓</b> correct on the second attempt · <b>Wrong</b> still wrong after two attempts (answer revealed) · <b>Skip</b> skipped<span class="v4left"> · <b>Left</b> not done yet</span></p>';
  if(!st){ h+='<p class="rec-empty">Not started yet.</p>'; }
  else {
    var T={n:0,c1:0,c2:0,w:0,sk:0,un:0};
    h+='<div class="rec-table-wrap"><table class="rec-table v4rec"><thead><tr><th>Sheet</th><th>Qs</th><th>1st ✓</th><th>2nd ✓</th><th>Wrong</th><th>Skip</th><th class="v4left">Left</th></tr></thead><tbody>';
    TAB_DEFS.forEach(function(td){ var tt=st.tabs[td.id]; if(!tt||!tt.items||!tt.items.length) return; var c=v4Counts(tt.items);
      ['n','c1','c2','w','sk','un'].forEach(function(k){ T[k]+=c[k]; });
      h+='<tr><td>'+td.label+'</td><td>'+c.n+'</td><td class="v4g">'+c.c1+'</td><td class="v4o">'+c.c2+'</td><td class="v4r">'+c.w+'</td><td>'+c.sk+'</td><td class="v4left">'+c.un+'</td></tr>'; });
    h+='<tr class="v4tot"><td>Total</td><td>'+T.n+'</td><td class="v4g">'+T.c1+'</td><td class="v4o">'+T.c2+'</td><td class="v4r">'+T.w+'</td><td>'+T.sk+'</td><td class="v4left">'+T.un+'</td></tr>';
    h+='</tbody></table></div>';
    var doneN=T.c1+T.c2+T.w; if(doneN){ h+='<p class="v4sum">Of '+doneN+' questions answered: <b>'+Math.round(T.c1/doneN*100)+'%</b> right first time, <b>'+Math.round(T.c2/doneN*100)+'%</b> right on the second try, <b>'+Math.round(T.w/doneN*100)+'%</b> still wrong after two tries.</p>'; }
  }
  var q=appState.quiz;
  h+='<h3>📝 Quiz Mode</h3>';
  if(!q){ h+='<p class="rec-empty">Not started yet.</p>'; }
  else {
    h+='<div class="rec-table-wrap"><table class="rec-table v4rec"><thead><tr><th>Sheet</th><th>Qs</th><th>Status</th><th>Correct</th><th>Wrong</th></tr></thead><tbody>';
    TAB_DEFS.forEach(function(td){ var tt=q.tabs[td.id]; if(!tt||!tt.items||!tt.items.length) return; var n=tt.items.length, cor=0, att=0;
      tt.items.forEach(function(it){ if(it.status!=='unanswered'||it.choice!==undefined) att++; });
      var rec=records.filter(function(r){ return r.mode==='quiz'&&r.tab===td.id; })[0];
      if(rec){ cor=rec.correct; att=Math.max(att,rec.total); }
      h+='<tr><td>'+td.label+'</td><td>'+n+'</td><td>'+(rec?'Finished':att+' / '+n)+'</td><td class="v4g">'+(rec?cor:'—')+'</td><td class="v4r">'+(rec?(rec.total-cor):'—')+'</td></tr>'; });
    h+='</tbody></table></div>';
  }
  h+='</div><div class="rec-card"><h3>Finished sheets (latest first)</h3>';
  if(!records.length){ h+='<p class="rec-empty">No sheets finished yet. Each time you finish a sheet, your score is recorded here.</p>'; }
  else {
    h+='<div class="rec-table-wrap"><table class="rec-table v4rec"><thead><tr><th>When</th><th>Mode</th><th>Sheet</th><th>Score</th><th>1st ✓</th><th>2nd ✓</th><th>Wrong</th></tr></thead><tbody>';
    records.forEach(function(r){ h+='<tr><td>'+fmtDate(r.at)+'</td><td>'+(r.mode==='quiz'?'Quiz':'Learning')+'</td><td>'+tabLabel(r.tab)+'</td><td>'+r.correct+' / '+r.total+'</td><td>'+(r.c1!=null?r.c1:'—')+'</td><td>'+(r.c2!=null?r.c2:'—')+'</td><td>'+(r.w!=null?r.w:(r.mode==='quiz'?r.total-r.correct:'—'))+'</td></tr>'; });
    h+='</tbody></table></div>';
  }
  h+='</div><button class="btn btn-primary" id="btnBack">'+(MODE?'Back to practice':'Choose a mode')+'</button>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('btnBack').addEventListener('click', function(){ if(MODE){ showChrome(true); buildTabbar(); renderSlide(); } else renderModePicker(); });
  window.scrollTo({top:0});
};
var _v4add = addRecord;
addRecord = function(mode, tabId, correct, revealed, skipped, total){
  _v4add(mode, tabId, correct, revealed, skipped, total);
  try{ if(mode==='learning'&&records[0]){ var c=v4Counts(state.tabs[tabId].items); records[0].c1=c.c1; records[0].c2=c.c2; records[0].w=c.w; saveState(); } }catch(e){}
};

/* ================= v4: celebration ================= */
function v4Cracker(){
  var ctx=ensureAudio(); if(!ctx) return; try{ if(ctx.state==='suspended') ctx.resume(); }catch(e){}
  var now=ctx.currentTime;
  function pop(t, vol, dur, hp){
    var len=Math.floor(ctx.sampleRate*dur), buf=ctx.createBuffer(1,len,ctx.sampleRate), d=buf.getChannelData(0);
    for(var i=0;i<len;i++){ d[i]=(Math.random()*2-1)*Math.pow(1-i/len,3); }
    var src=ctx.createBufferSource(); src.buffer=buf;
    var f=ctx.createBiquadFilter(); f.type='highpass'; f.frequency.value=hp;
    var g=ctx.createGain(); g.gain.setValueAtTime(vol,t); g.gain.exponentialRampToValueAtTime(0.001,t+dur);
    src.connect(f); f.connect(g); g.connect(ctx.destination); src.start(t); src.stop(t+dur+0.02);
  }
  pop(now,0.9,0.35,300);                                   // big bang
  for(var k=0;k<14;k++){ pop(now+0.35+Math.random()*1.3, 0.25+Math.random()*0.35, 0.05+Math.random()*0.08, 1500+Math.random()*2500); } // crackles
  [523.25,659.25,783.99,1046.5].forEach(function(fq,i){ var o=ctx.createOscillator(), g=ctx.createGain(); o.type='triangle'; o.frequency.value=fq;
    var t=now+0.15+i*0.12; g.gain.setValueAtTime(0.0001,t); g.gain.exponentialRampToValueAtTime(0.18,t+0.03); g.gain.exponentialRampToValueAtTime(0.001,t+0.5);
    o.connect(g); g.connect(ctx.destination); o.start(t); o.stop(t+0.55); });
}
function v4Confetti(){
  var cv=document.createElement('canvas'); cv.className='v4conf'; document.body.appendChild(cv);
  var W=cv.width=window.innerWidth*(window.devicePixelRatio||1), H=cv.height=window.innerHeight*(window.devicePixelRatio||1), s=(window.devicePixelRatio||1);
  var ctx=cv.getContext('2d'), cols=['#e8b13a','#d9534f','#2e86de','#27ae60','#9b59b6','#f39c12','#1abc9c','#ff6b9a'], P=[];
  function burst(x,y,n){ for(var i=0;i<n;i++){ var a=Math.random()*Math.PI*2, v=(4+Math.random()*9)*s;
    P.push({x:x,y:y,vx:Math.cos(a)*v,vy:Math.sin(a)*v-6*s,w:(6+Math.random()*7)*s,h:(4+Math.random()*5)*s,r:Math.random()*6,vr:(Math.random()-.5)*.4,c:cols[i%cols.length],life:0,flyer:Math.random()<.25}); } }
  burst(W*0.2,H*0.35,90); burst(W*0.8,H*0.35,90); setTimeout(function(){ burst(W*0.5,H*0.25,120); },380); setTimeout(function(){ burst(W*0.35,H*0.3,70); burst(W*0.65,H*0.3,70); },800);
  var t0=performance.now();
  (function frame(t){
    ctx.clearRect(0,0,W,H);
    P.forEach(function(p){ p.vy+=0.22*s; p.vx*=0.985; p.vy*=0.985; p.x+=p.vx; p.y+=p.vy; p.r+=p.vr; p.life++;
      ctx.save(); ctx.translate(p.x,p.y); ctx.rotate(p.r); ctx.fillStyle=p.c;
      if(p.flyer){ ctx.fillRect(-p.w*1.4,-1.5*s,p.w*2.8,3*s); } else { ctx.fillRect(-p.w/2,-p.h/2,p.w,p.h*Math.abs(Math.cos(p.life/6))); }
      ctx.restore(); });
    if(t-t0<4200) requestAnimationFrame(frame); else cv.remove();
  })(t0);
}
function v4NextTab(){
  var i=TAB_DEFS.findIndex(function(t){return t.id===activeTab;});
  for(var j=i+1;j<TAB_DEFS.length;j++){ var id=TAB_DEFS[j].id; if(id!=='theory' && SLIDES[id] && SLIDES[id].length) return TAB_DEFS[j]; }
  return null;
}
function v4Celebrate(){
  var done=document.querySelector('#wrap .done-card'); if(!done || done.dataset.v4) return; done.dataset.v4='1';
  var ts=state.tabs[activeTab], c=v4Counts(ts.items), n=ts.items.length;
  var first=(student&&student.name)?String(student.name).split(' ')[0]:'';
  var pct=MODE==='quiz'?null:Math.round((c.c1+c.c2)/Math.max(n,1)*100);
  var msg = pct===null ? 'You finished this sheet!' : (pct>=90?'Outstanding work!':pct>=75?'Excellent effort!':pct>=50?'Well done — keep going!':'Great persistence — every try makes you stronger!');
  var banner=document.createElement('div'); banner.className='v4ban';
  banner.innerHTML='<div class="v4trophy">🏆</div><div class="v4h">Congratulations'+(first?', '+esc(first):'')+'!</div><div class="v4m">'+msg+'</div>'+
    (MODE==='quiz'?'':'<div class="v4pills"><span class="v4p g">✅ '+c.c1+' first attempt</span><span class="v4p o">🔁 '+c.c2+' second attempt</span><span class="v4p r">❌ '+c.w+' finally wrong</span>'+(c.sk?'<span class="v4p s">⏭ '+c.sk+' skipped</span>':'')+'</div>');
  done.insertBefore(banner, done.firstChild);
  var nx=v4NextTab(), wrapB=document.createElement('div'); wrapB.className='v4next';
  if(nx){ wrapB.innerHTML='<button class="btn btn-primary v4go" id="v4Next">Next sheet: '+nx.label+' →</button>'; }
  else if(SLIDES.report!==undefined || TAB_DEFS.some(function(t){return t.id==='report';})){ wrapB.innerHTML='<button class="btn btn-primary v4go" id="v4Next">📊 View my report →</button>'; }
  else { wrapB.innerHTML='<button class="btn btn-primary v4go" id="v4Next">⌂ Back to the home page</button>'; }
  banner.appendChild(wrapB);
  document.getElementById('v4Next').addEventListener('click',function(){
    var t=nx?nx.id:(TAB_DEFS.some(function(x){return x.id==='report';})?'report':(TAB_DEFS[0]&&TAB_DEFS[0].id));
    activeTab=t; showChrome(true); buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); });
  setTimeout(function(){ var r=banner.getBoundingClientRect(); window.scrollBy({top:r.top-110,behavior:'smooth'}); },60);
  v4Confetti(); v4Cracker();
}
var _v4rf = renderFinished;
renderFinished = function(){ _v4rf.apply(this,arguments); try{ v4Celebrate(); }catch(e){ console.log('v4',e); } };


/* ================= calculus answer mode ================= */
function calcCompile(src){
  var s=String(src).replace(/\s+/g,'').replace(/[×∗·]/g,'*').replace(/÷/g,'/').replace(/[−–—]/g,'-')
    .replace(/²/g,'^2').replace(/³/g,'^3').replace(/π/g,'#').replace(/eˣ/g,'e^x');
  if(s.indexOf('=')>=0) s=s.slice(s.lastIndexOf('=')+1);
  var FN=['sqrt','abs','ln','log','exp','sin','cos','tan','sec','csc','cot','pi'];
  var i=0,toks=[];
  while(i<s.length){
    var ch=s[i];
    if(/[0-9.]/.test(ch)){ var j=i; while(j<s.length&&/[0-9.]/.test(s[j])) j++; toks.push({k:'n',v:parseFloat(s.slice(i,j))}); i=j; continue; }
    if(ch==='√'){ toks.push({k:'f',v:'sqrt'}); i++; continue; }
    if(/[a-zA-Z#]/.test(ch)){
      var hit=null; for(var q=0;q<FN.length;q++){ if(s.substr(i,FN[q].length).toLowerCase()===FN[q]){ hit=FN[q]; break; } }
      if(hit==='pi'){ toks.push({k:'v',v:'#'}); i+=2; continue; }
      if(hit){ toks.push({k:'f',v:hit}); i+=hit.length; continue; }
      toks.push({k:'v',v:ch}); i++; continue; }
    if('+-*/^()|'.indexOf(ch)>=0){ toks.push({k:ch}); i++; continue; }
    return null;
  }
  // absolute value bars -> abs( )
  var t2=[],open=false; for(var a=0;a<toks.length;a++){ var tk=toks[a]; if(tk.k==='|'){ if(!open){ t2.push({k:'f',v:'abs'}); t2.push({k:'('}); open=true; } else { t2.push({k:')'}); open=false; } } else t2.push(tk); }
  toks=t2;
  var out=[];
  for(var t=0;t<toks.length;t++){ var A=toks[t], B=out[out.length-1];
    if(B&&(B.k==='n'||B.k==='v'||B.k===')')&&(A.k==='n'||A.k==='v'||A.k==='('||A.k==='f')) out.push({k:'&'});
    out.push(A); }
  var p=0; function pk(){ return out[p]; }
  var F={sqrt:Math.sqrt,ln:Math.log,log:function(x){return Math.log(x)/Math.LN10;},exp:Math.exp,sin:Math.sin,cos:Math.cos,tan:Math.tan,
    sec:function(x){return 1/Math.cos(x);},csc:function(x){return 1/Math.sin(x);},cot:function(x){return 1/Math.tan(x);},abs:Math.abs};
  function E(){ var n=T(); while(pk()&&(pk().k==='+'||pk().k==='-')){ var o=out[p++].k,r=T(); n=(function(l,r,o){return function(e){return o==='+'?l(e)+r(e):l(e)-r(e);};})(n,r,o);} return n; }
  function T(){ var n=I(); while(pk()&&(pk().k==='*'||pk().k==='/')){ var o=out[p++].k,r=I(); n=(function(l,r,o){return function(e){return o==='*'?l(e)*r(e):l(e)/r(e);};})(n,r,o);} return n; }
  function I(){ var n=U(); while(pk()&&pk().k==='&'){ p++; var r=P(); n=(function(l,r){return function(e){return l(e)*r(e);};})(n,r);} return n; }
  function U(){ if(pk()&&pk().k==='-'){ p++; var u=U(); return function(e){return -u(e);}; } if(pk()&&pk().k==='+'){ p++; return U(); } return P(); }
  function P(){ var b=Aa(); if(pk()&&pk().k==='^'){ p++; var x=U(); return function(e){return Math.pow(b(e),x(e));}; } return b; }
  function Aa(){ var t=out[p++]; if(!t) throw 0;
    if(t.k==='n') return function(){return t.v;};
    if(t.k==='v'){ if(t.v==='#') return function(){return Math.PI;}; if(t.v==='e') return function(){return Math.E;}; return function(e){return e[t.v];}; }
    if(t.k==='('){ var n=E(); if(!pk()||pk().k!==')') throw 0; p++; return n; }
    if(t.k==='f'){ var fn=F[t.v]; var ex=null;
      if(pk()&&pk().k==='^'){ p++; ex=Aa(); if(pk()&&pk().k==='&') p++; }            // sin^2(x)
      var arg; if(pk()&&pk().k==='('){ p++; arg=E(); if(!pk()||pk().k!==')') throw 0; p++; } else arg=P();
      return ex? function(e){ return Math.pow(fn(arg(e)),ex(e)); } : function(e){ return fn(arg(e)); }; }
    throw 0; }
  try{ var f=E(); if(p!==out.length) return null; return f; }catch(e){ return null; }
}
function calcEqual(input,answer){
  var f=calcCompile(input), g=calcCompile(answer); if(!f||!g) return false;
  var hits=0;
  for(var trial=0;trial<14;trial++){
    var env={}; 'abcdfghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').forEach(function(c){ env[c]=0.35+Math.random()*2.3; });
    var x=f(env), y=g(env); if(!isFinite(x)||!isFinite(y)) continue;
    if(Math.abs(x-y)>1e-6*Math.max(1,Math.abs(x),Math.abs(y))) return false; hits++;
  }
  return hits>=4;
}
(function(){
  var _am=answerMatches;
  answerMatches=function(input,answer,accept,expr){
    if(expr==='calc'){ if(input===undefined||input===null||String(input).trim()==='') return false;
      if(calcEqual(input,answer)) return true; return (accept||[]).some(function(a){ return calcEqual(input,a); }); }
    return _am.apply(this,arguments); };
  var _kl=kbLayoutFor;
  kbLayoutFor=function(inp){ try{ var ts=state.tabs[activeTab], sl=SLIDES[activeTab][ts.idx], stp=sl.flat[parseInt(inp.dataset.step,10)]; if(stp.expr==='calc') return 'calc'; }catch(e){} return _kl(inp); };
  var _kp=kbPress;
  kbPress=function(k){ var m={'eˣ':'e^(','√':'√(','ln':'ln(','sin':'sin(','cos':'cos(','tan':'tan('}; if(KB.page==='calc'&&m[k]){ kbInsert(m[k]); return; } return _kp(k); };
})();

renderLogin();
})();
</script>
</body>
</html>
