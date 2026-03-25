# MY-<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Placement Tracker — CSE Department</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
/* ══════════════════════════════════════════
   RESET & VARIABLES
══════════════════════════════════════════ */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#080b11;--bg2:#0e1118;--bg3:#141824;
  --card:#161c2c;--card2:#1c2338;
  --border:rgba(255,255,255,0.06);--border2:rgba(255,255,255,0.11);
  --accent:#4f8ef7;--accent2:#00e5a0;--accent3:#f7b731;--red:#f74f6e;
  --txt:#e8ecf4;--txt2:#6a7590;--txt3:#3d4560;
  --r:14px;--r2:10px;--r3:8px;
  --fh:'Syne',sans-serif;--fb:'Outfit',sans-serif;--fm:'DM Mono',monospace;
}
html{scroll-behavior:smooth}
body{font-family:var(--fb);background:var(--bg);color:var(--txt);min-height:100vh;font-size:15px;line-height:1.6;-webkit-font-smoothing:antialiased}
body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(79,142,247,.025) 1px,transparent 1px),linear-gradient(90deg,rgba(79,142,247,.025) 1px,transparent 1px);background-size:52px 52px;pointer-events:none;z-index:0}
::-webkit-scrollbar{width:5px}::-webkit-scrollbar-track{background:var(--bg2)}::-webkit-scrollbar-thumb{background:var(--bg3);border-radius:3px}
input,select,button{font-family:var(--fb)}
input::placeholder{color:var(--txt3)}
select option{background:var(--bg3);color:var(--txt)}

/* ══════════════════════════════════════════
   HEADER
══════════════════════════════════════════ */
header{
  position:sticky;top:0;z-index:100;
  background:rgba(8,11,17,.85);backdrop-filter:blur(20px);
  border-bottom:1px solid var(--border);
  padding:0 2rem;display:flex;align-items:center;justify-content:space-between;height:62px;
}
.logo{font-family:var(--fh);font-weight:800;font-size:18px;letter-spacing:-.02em;color:var(--txt);display:flex;align-items:center;gap:10px}
.logo-dot{width:8px;height:8px;background:var(--accent2);border-radius:50%;animation:pulse 2.2s ease-in-out infinite;flex-shrink:0}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.6;transform:scale(.7)}}
.hdr-right{display:flex;align-items:center;gap:10px}
.hdr-badge{font-family:var(--fm);font-size:11px;color:var(--accent);background:rgba(79,142,247,.1);border:1px solid rgba(79,142,247,.2);padding:4px 12px;border-radius:20px;letter-spacing:.04em}
.hdr-batch{font-family:var(--fm);font-size:11px;color:var(--txt2);background:var(--bg3);border:1px solid var(--border2);padding:4px 12px;border-radius:20px}

/* ══════════════════════════════════════════
   LAYOUT
══════════════════════════════════════════ */
.wrapper{position:relative;z-index:1;max-width:1100px;margin:0 auto;padding:2rem 1.5rem 4rem}

/* ══════════════════════════════════════════
   TABS
══════════════════════════════════════════ */
.tabs{display:flex;gap:4px;background:var(--bg2);border:1px solid var(--border);border-radius:var(--r);padding:5px;margin-bottom:2rem;width:fit-content}
.tab-btn{font-family:var(--fb);font-size:13px;font-weight:500;padding:8px 24px;border:none;border-radius:10px;cursor:pointer;transition:all .2s;background:transparent;color:var(--txt2);letter-spacing:.01em}
.tab-btn:hover:not(.active){background:var(--bg3);color:var(--txt)}
.tab-btn.active{background:var(--accent);color:#fff;box-shadow:0 0 18px rgba(79,142,247,.28)}

/* ══════════════════════════════════════════
   PANES
══════════════════════════════════════════ */
.pane{display:none}
.pane.active{display:block;animation:fadeUp .3s ease}
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}

/* ══════════════════════════════════════════
   SECTION TITLE
══════════════════════════════════════════ */
.sec-title{font-family:var(--fm);font-size:10px;font-weight:500;letter-spacing:.14em;text-transform:uppercase;color:var(--txt3);margin-bottom:1rem;display:flex;align-items:center;gap:10px}
.sec-title::after{content:'';flex:1;height:1px;background:var(--border)}

/* ══════════════════════════════════════════
   METRICS
══════════════════════════════════════════ */
.metrics-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:1rem;margin-bottom:1.5rem}
.metric-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.25rem;position:relative;overflow:hidden;transition:transform .2s,border-color .2s;cursor:default}
.metric-card:hover{transform:translateY(-2px);border-color:var(--border2)}
.metric-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px}
.metric-card.blue::before{background:var(--accent)}.metric-card.green::before{background:var(--accent2)}.metric-card.amber::before{background:var(--accent3)}.metric-card.red::before{background:var(--red)}
.m-label{font-family:var(--fm);font-size:10px;letter-spacing:.12em;text-transform:uppercase;color:var(--txt3);margin-bottom:10px}
.m-value{font-family:var(--fh);font-size:30px;font-weight:800;line-height:1;margin-bottom:5px}
.m-value.blue{color:var(--accent)}.m-value.green{color:var(--accent2)}.m-value.amber{color:var(--accent3)}.m-value.red{color:var(--red)}
.m-sub{font-size:11px;color:var(--txt3);font-family:var(--fm)}

/* ══════════════════════════════════════════
   CHART CARDS
══════════════════════════════════════════ */
.charts-grid{display:grid;grid-template-columns:1fr 1fr;gap:1rem;margin-bottom:1.5rem}
.chart-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.25rem 1.25rem .75rem}
.chart-title{font-family:var(--fh);font-size:11px;font-weight:600;letter-spacing:.1em;text-transform:uppercase;color:var(--txt2);margin-bottom:10px}
.chart-wrap{position:relative;height:200px}
.legend-row{display:flex;gap:1rem;flex-wrap:wrap;margin-bottom:8px}
.legend-item{display:flex;align-items:center;gap:5px;font-size:11px;color:var(--txt2);font-family:var(--fm)}
.legend-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0}

/* ══════════════════════════════════════════
   COMPANY BARS
══════════════════════════════════════════ */
.company-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.25rem;margin-bottom:1.5rem}
.co-row{display:flex;align-items:center;gap:12px;margin-bottom:10px}
.co-row:last-child{margin-bottom:0}
.co-name{min-width:110px;font-size:13px;font-weight:500;color:var(--txt)}
.co-bg{flex:1;height:7px;background:var(--bg3);border-radius:4px;overflow:hidden}
.co-fill{height:100%;border-radius:4px;transition:width .6s cubic-bezier(.4,0,.2,1)}
.co-count{font-family:var(--fm);font-size:12px;color:var(--txt2);min-width:20px;text-align:right}

/* ══════════════════════════════════════════
   FILTERS BAR
══════════════════════════════════════════ */
.filters-bar{display:flex;gap:10px;margin-bottom:1.25rem;flex-wrap:wrap;align-items:center}
.search-wrap{position:relative;flex:1;min-width:200px}
.search-icon{position:absolute;left:12px;top:50%;transform:translateY(-50%);color:var(--txt3);pointer-events:none;font-size:14px}
.search-inp{width:100%;padding:9px 14px 9px 36px;background:var(--bg2);border:1px solid var(--border2);border-radius:var(--r2);color:var(--txt);font-size:13px;outline:none;transition:border-color .2s,box-shadow .2s}
.search-inp:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(79,142,247,.1)}
.filter-sel{padding:9px 12px;background:var(--bg2);border:1px solid var(--border2);border-radius:var(--r2);color:var(--txt);font-size:13px;cursor:pointer;outline:none;transition:border-color .2s}
.filter-sel:focus{border-color:var(--accent)}
.clear-btn{padding:9px 16px;background:transparent;border:1px solid var(--border2);border-radius:var(--r2);color:var(--txt2);font-size:13px;cursor:pointer;transition:all .15s;display:none}
.clear-btn:hover{background:var(--bg3);color:var(--txt)}
.clear-btn.show{display:block}

/* ══════════════════════════════════════════
   TABLE
══════════════════════════════════════════ */
.table-outer{border:1px solid var(--border);border-radius:var(--r);overflow:hidden}
.table-scroll{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13px}
thead tr{background:var(--bg3)}
th{padding:11px 14px;text-align:left;font-family:var(--fm);font-size:10px;font-weight:500;letter-spacing:.1em;text-transform:uppercase;color:var(--txt3);border-bottom:1px solid var(--border);white-space:nowrap;user-select:none}
th.sortable{cursor:pointer}
th.sortable:hover{color:var(--txt2)}
td{padding:11px 14px;border-bottom:1px solid var(--border);color:var(--txt);vertical-align:middle}
tbody tr:last-child td{border-bottom:none}
tbody tr:hover td{background:rgba(255,255,255,.015)}
.td-num{font-family:var(--fm);font-size:11px;color:var(--txt3)}
.td-mono{font-family:var(--fm);font-size:12px}
.td-name{font-weight:600}
.td-roll{font-family:var(--fm);font-size:10px;color:var(--txt3);letter-spacing:.04em}
.td-role{color:var(--txt2);font-size:12px}
.td-pkg{font-weight:600;color:var(--accent3);font-family:var(--fm)}
.td-dash{color:var(--txt3)}
.branch-pill{display:inline-block;padding:2px 8px;border-radius:4px;font-family:var(--fm);font-size:10px;font-weight:500;letter-spacing:.06em;background:var(--bg3);color:var(--txt2);border:1px solid var(--border2)}
.badge{display:inline-flex;align-items:center;padding:3px 10px;border-radius:20px;font-size:10px;font-weight:500;font-family:var(--fm);letter-spacing:.04em;white-space:nowrap}
.badge-placed{background:rgba(0,229,160,.1);color:var(--accent2);border:1px solid rgba(0,229,160,.2)}
.badge-not{background:rgba(247,79,110,.1);color:var(--red);border:1px solid rgba(247,79,110,.2)}
.badge-offer{background:rgba(247,183,49,.1);color:var(--accent3);border:1px solid rgba(247,183,49,.2)}
.tbl-footer{margin-top:10px;font-size:12px;color:var(--txt3);text-align:right;font-family:var(--fm)}
.tbl-footer strong{color:var(--txt2)}
.empty-row{padding:3rem 1rem;text-align:center;color:var(--txt3);font-size:14px}

/* ══════════════════════════════════════════
   ADD ENTRY FORM
══════════════════════════════════════════ */
.add-layout{display:grid;grid-template-columns:1fr 280px;gap:1.5rem;align-items:start}
.form-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.75rem 2rem}
.form-title{font-family:var(--fh);font-size:20px;font-weight:700;letter-spacing:-.01em;margin-bottom:4px}
.form-desc{font-size:13px;color:var(--txt2);margin-bottom:1.75rem}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:1.1rem;margin-bottom:1.5rem}
.form-group{display:flex;flex-direction:column;gap:6px}
.form-label{font-family:var(--fm);font-size:10px;letter-spacing:.12em;text-transform:uppercase;color:var(--txt3);font-weight:500}
.form-inp,.form-sel{padding:10px 13px;background:var(--bg2);border:1px solid var(--border2);border-radius:var(--r2);color:var(--txt);font-size:14px;font-family:var(--fb);outline:none;transition:border-color .2s,box-shadow .2s;width:100%}
.form-inp:focus,.form-sel:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(79,142,247,.1)}
.form-inp.has-err{border-color:var(--red)}
.form-inp.has-err:focus{box-shadow:0 0 0 3px rgba(247,79,110,.1)}
.form-inp:disabled,.form-sel:disabled{opacity:.4;cursor:not-allowed}
.err-msg{font-size:11px;color:var(--red);font-family:var(--fm)}
.form-actions{display:flex;gap:10px;align-items:center}
.btn-submit{padding:11px 28px;background:var(--accent);color:#fff;border:none;border-radius:var(--r2);font-size:14px;font-weight:600;font-family:var(--fb);cursor:pointer;letter-spacing:.01em;transition:opacity .15s,transform .15s;box-shadow:0 0 20px rgba(79,142,247,.2)}
.btn-submit:hover{opacity:.88;transform:translateY(-1px)}
.btn-submit:active{transform:scale(.98)}
.btn-reset{padding:11px 20px;background:transparent;border:1px solid var(--border2);border-radius:var(--r2);color:var(--txt2);font-size:14px;font-family:var(--fb);cursor:pointer;transition:all .15s}
.btn-reset:hover{background:var(--bg3);color:var(--txt)}
.toast{display:none;align-items:center;gap:8px;margin-top:1rem;padding:12px 16px;background:rgba(0,229,160,.08);border:1px solid rgba(0,229,160,.18);border-radius:var(--r2);color:var(--accent2);font-size:13px;font-weight:500;animation:fadeUp .3s ease}
.toast.show{display:flex}
.info-panel{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.5rem}
.info-panel-title{font-family:var(--fh);font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--txt2);margin-bottom:1rem}
.info-list{list-style:none;display:flex;flex-direction:column;gap:10px;margin-bottom:1.25rem}
.info-list li{font-size:13px;color:var(--txt2);padding-left:14px;position:relative;line-height:1.5}
.info-list li::before{content:'·';position:absolute;left:0;color:var(--accent);font-size:18px;line-height:1;top:1px}
.info-list strong{color:var(--accent3)}
.info-link{display:flex;align-items:center;gap:8px;font-size:12px;color:var(--txt3);padding-top:1rem;border-top:1px solid var(--border);font-family:var(--fm)}
.info-dot{width:6px;height:6px;border-radius:50%;background:var(--accent2);flex-shrink:0;animation:pulse 2s ease-in-out infinite}

/* ══════════════════════════════════════════
   RESPONSIVE
══════════════════════════════════════════ */
@media(max-width:900px){
  .add-layout{grid-template-columns:1fr}
  .form-grid{grid-template-columns:1fr 1fr}
}
@media(max-width:768px){
  .metrics-grid{grid-template-columns:1fr 1fr}
  .charts-grid{grid-template-columns:1fr}
  .tabs{width:100%}
  .tab-btn{flex:1;padding:8px 10px;font-size:12px}
}
@media(max-width:560px){
  .form-grid{grid-template-columns:1fr}
  .form-card{padding:1.25rem}
  header{padding:0 1rem}
  .wrapper{padding:1.25rem 1rem 3rem}
  .metrics-grid{grid-template-columns:1fr 1fr}
  .m-value{font-size:22px}
}
</style>
</head>
<body>

<!-- ══ HEADER ══ -->
<header>
  <div class="logo">
    <span class="logo-dot"></span>
    PlacementTracker
  </div>
  <div class="hdr-right">
    <span class="hdr-badge">CSE Dept</span>
    <span class="hdr-batch">Batch 2025</span>
  </div>
</header>

<!-- ══ MAIN ══ -->
<div class="wrapper">

  <!-- Tabs -->
  <div class="tabs">
    <button class="tab-btn active" onclick="showTab('dashboard',this)">Dashboard</button>
    <button class="tab-btn" onclick="showTab('students',this)">Students</button>
    <button class="tab-btn" onclick="showTab('add',this)">Add Entry</button>
  </div>

  <!-- ══ DASHBOARD PANE ══ -->
  <div class="pane active" id="pane-dashboard">
    <div class="sec-title">Overview</div>
    <div class="metrics-grid" id="metrics-grid"></div>

    <div class="sec-title">Analytics</div>
    <div class="charts-grid">
      <div class="chart-card">
        <div class="chart-title">Placement Status</div>
        <div class="legend-row" id="status-legend"></div>
        <div class="chart-wrap"><canvas id="statusChart"></canvas></div>
      </div>
      <div class="chart-card">
        <div class="chart-title">Package Distribution (LPA)</div>
        <div class="chart-wrap"><canvas id="pkgChart"></canvas></div>
      </div>
    </div>

    <div class="company-card">
      <div class="chart-title">Top Companies by Offers</div>
      <div id="company-bars"></div>
    </div>

    <div class="charts-grid">
      <div class="chart-card">
        <div class="chart-title">Branch-wise Placement %</div>
        <div class="chart-wrap"><canvas id="branchChart"></canvas></div>
      </div>
      <div class="chart-card">
        <div class="chart-title">Year-wise Placements</div>
        <div class="chart-wrap"><canvas id="yearChart"></canvas></div>
      </div>
    </div>
  </div>

  <!-- ══ STUDENTS PANE ══ -->
  <div class="pane" id="pane-students">
    <div class="filters-bar">
      <div class="search-wrap">
        <span class="search-icon">⌕</span>
        <input class="search-inp" type="text" id="search-inp" placeholder="Search name, company, roll…" oninput="filterTable()">
      </div>
      <select class="filter-sel" id="f-branch" onchange="filterTable()">
        <option value="">All Branches</option>
        <option>CSE</option><option>ECE</option><option>IT</option><option>MECH</option><option>CIVIL</option>
      </select>
      <select class="filter-sel" id="f-year" onchange="filterTable()">
        <option value="">All Years</option>
        <option>2025</option><option>2024</option><option>2023</option>
      </select>
      <select class="filter-sel" id="f-status" onchange="filterTable()">
        <option value="">All Status</option>
        <option>Placed</option><option>Not Placed</option><option>Offer in Hand</option>
      </select>
      <button class="clear-btn" id="clear-btn" onclick="clearFilters()">✕ Clear</button>
    </div>
    <div class="table-outer">
      <div class="table-scroll">
        <table>
          <thead>
            <tr>
              <th>#</th>
              <th class="sortable" onclick="sortTable('name')">Name <span id="sort-name">↑</span></th>
              <th class="sortable" onclick="sortTable('branch')">Branch <span id="sort-branch"></span></th>
              <th class="sortable" onclick="sortTable('year')">Year <span id="sort-year"></span></th>
              <th>Company</th>
              <th>Role</th>
              <th class="sortable" onclick="sortTable('pkg')">Package <span id="sort-pkg"></span></th>
              <th class="sortable" onclick="sortTable('status')">Status <span id="sort-status"></span></th>
            </tr>
          </thead>
          <tbody id="students-body"></tbody>
        </table>
      </div>
    </div>
    <div class="tbl-footer" id="tbl-footer"></div>
  </div>

  <!-- ══ ADD ENTRY PANE ══ -->
  <div class="pane" id="pane-add">
    <div class="add-layout">
      <div class="form-card">
        <div class="form-title">Add Placement Record</div>
        <div class="form-desc">Enter student details below. Starred fields are required.</div>
        <div class="form-grid">
          <div class="form-group">
            <label class="form-label">Full Name *</label>
            <input class="form-inp" type="text" id="f-name" placeholder="e.g. Aryan Mehta">
            <span class="err-msg" id="err-name"></span>
          </div>
          <div class="form-group">
            <label class="form-label">Roll No. *</label>
            <input class="form-inp" type="text" id="f-roll" placeholder="e.g. 22CS001">
            <span class="err-msg" id="err-roll"></span>
          </div>
          <div class="form-group">
            <label class="form-label">Branch *</label>
            <select class="form-sel" id="f-branch-f">
              <option value="">Select branch</option>
              <option>CSE</option><option>ECE</option><option>IT</option><option>MECH</option><option>CIVIL</option>
            </select>
            <span class="err-msg" id="err-branch"></span>
          </div>
          <div class="form-group">
            <label class="form-label">Batch Year</label>
            <select class="form-sel" id="f-year-f">
              <option>2025</option><option>2024</option><option>2023</option>
            </select>
          </div>
          <div class="form-group">
            <label class="form-label">Placement Status</label>
            <select class="form-sel" id="f-status-f" onchange="toggleFields()">
              <option>Placed</option>
              <option>Not Placed</option>
              <option>Offer in Hand</option>
            </select>
          </div>
          <div class="form-group">
            <label class="form-label">Package (LPA)</label>
            <input class="form-inp" type="number" id="f-pkg" placeholder="e.g. 12.5" min="0" step="0.5">
          </div>
          <div class="form-group" id="company-grp">
            <label class="form-label">Company *</label>
            <input class="form-inp" type="text" id="f-company" placeholder="e.g. Google, TCS…">
            <span class="err-msg" id="err-company"></span>
          </div>
          <div class="form-group" id="role-grp">
            <label class="form-label">Role / Designation</label>
            <input class="form-inp" type="text" id="f-role" placeholder="e.g. Software Engineer">
          </div>
        </div>
        <div class="form-actions">
          <button class="btn-submit" onclick="submitForm()">Add Record</button>
          <button class="btn-reset" onclick="resetForm()">Reset</button>
        </div>
        <div class="toast" id="toast">✓ Record added successfully!</div>
      </div>

      <div class="info-panel">
        <div class="info-panel-title">Instructions</div>
        <ul class="info-list">
          <li>Teams must have <strong>2–3 members</strong>. One member submits for the group.</li>
          <li>Frame a <strong>unique problem statement</strong> related to CSE dept.</li>
          <li>Registration is <strong>first-come, first-served</strong> to avoid duplicates.</li>
          <li>Carefully verify all details before submitting.</li>
          <li>Last date: <strong>4/3/2026</strong></li>
        </ul>
        <div class="info-link">
          <span class="info-dot"></span>
          CTA form link available on notice board
        </div>
      </div>
    </div>
  </div>

</div><!-- /wrapper -->

<script>
/* ══════════════════════════════════════════
   DATA
══════════════════════════════════════════ */
let students = [
  {id:1,  name:"Riya Sharma",    roll:"22CS001", branch:"CSE",   year:"2025", status:"Placed",        company:"Google",     role:"Software Engineer",  pkg:42  },
  {id:2,  name:"Aditya Verma",   roll:"22CS002", branch:"CSE",   year:"2025", status:"Placed",        company:"Microsoft",  role:"SDE II",             pkg:35  },
  {id:3,  name:"Priya Nair",     roll:"22EC001", branch:"ECE",   year:"2025", status:"Placed",        company:"Qualcomm",   role:"VLSI Engineer",      pkg:22  },
  {id:4,  name:"Rohan Gupta",    roll:"22IT001", branch:"IT",    year:"2025", status:"Offer in Hand", company:"Wipro",      role:"Analyst",            pkg:7.5 },
  {id:5,  name:"Sneha Iyer",     roll:"22CS003", branch:"CSE",   year:"2025", status:"Placed",        company:"Amazon",     role:"SDE-1",              pkg:28  },
  {id:6,  name:"Karan Joshi",    roll:"22ME001", branch:"MECH",  year:"2025", status:"Not Placed",    company:"",           role:"",                   pkg:0   },
  {id:7,  name:"Ananya Das",     roll:"21CS001", branch:"CSE",   year:"2024", status:"Placed",        company:"Infosys",    role:"Systems Engineer",   pkg:9.5 },
  {id:8,  name:"Vikas Rao",      roll:"21EC001", branch:"ECE",   year:"2024", status:"Placed",        company:"TCS",        role:"Developer",          pkg:7   },
  {id:9,  name:"Meera Singh",    roll:"22CS004", branch:"CSE",   year:"2025", status:"Placed",        company:"Flipkart",   role:"SDE",                pkg:21  },
  {id:10, name:"Arjun Pillai",   roll:"22IT002", branch:"IT",    year:"2025", status:"Placed",        company:"Deloitte",   role:"Consultant",         pkg:11  },
  {id:11, name:"Tanya Khanna",   roll:"21CS002", branch:"CSE",   year:"2024", status:"Placed",        company:"Google",     role:"Product Manager",    pkg:38  },
  {id:12, name:"Nikhil Bhatt",   roll:"22CV001", branch:"CIVIL", year:"2025", status:"Not Placed",    company:"",           role:"",                   pkg:0   },
  {id:13, name:"Pooja Menon",    roll:"22CS005", branch:"CSE",   year:"2025", status:"Placed",        company:"Adobe",      role:"SDE-2",              pkg:24  },
  {id:14, name:"Siddharth K.",   roll:"20EC001", branch:"ECE",   year:"2023", status:"Placed",        company:"Samsung",    role:"FW Engineer",        pkg:14  },
  {id:15, name:"Divya Reddy",    roll:"22IT003", branch:"IT",    year:"2025", status:"Offer in Hand", company:"Cognizant",  role:"Analyst",            pkg:6.5 },
  {id:16, name:"Raj Kumar",      roll:"21ME001", branch:"MECH",  year:"2024", status:"Placed",        company:"Mahindra",   role:"Design Engineer",    pkg:8   },
  {id:17, name:"Nisha Patel",    roll:"22CS006", branch:"CSE",   year:"2025", status:"Placed",        company:"Zomato",     role:"Backend Developer",  pkg:18  },
  {id:18, name:"Amit Jain",      roll:"22CS007", branch:"CSE",   year:"2025", status:"Placed",        company:"Microsoft",  role:"SDE-1",              pkg:33  },
  {id:19, name:"Kavya Menon",    roll:"22IT004", branch:"IT",    year:"2025", status:"Placed",        company:"Accenture",  role:"Associate Dev",      pkg:8   },
  {id:20, name:"Suresh P.",      roll:"21CV001", branch:"CIVIL", year:"2024", status:"Placed",        company:"L&T",        role:"Site Engineer",      pkg:6   },
];

/* ══════════════════════════════════════════
   STATE
══════════════════════════════════════════ */
let sortKey = 'name', sortDir = 'asc';
let chartStatus = null, chartPkg = null, chartBranch = null, chartYear = null;
const CO_COLORS = ['#4f8ef7','#00e5a0','#f7b731','#f74f6e','#a78bf5','#f58c1a','#4fc3f7'];

const CHART_GRID = { color:'rgba(255,255,255,0.04)' };
const CHART_TICK = { color:'#3d4560', font:{ size:10 } };
const CHART_AXIS = { color:'rgba(255,255,255,0.07)' };

/* ══════════════════════════════════════════
   TAB ROUTING
══════════════════════════════════════════ */
function showTab(id, btn) {
  document.querySelectorAll('.pane').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('pane-' + id).classList.add('active');
  btn.classList.add('active');
  if (id === 'dashboard') renderDashboard();
  if (id === 'students')  filterTable();
}

/* ══════════════════════════════════════════
   DASHBOARD
══════════════════════════════════════════ */
function renderDashboard() {
  const total  = students.length;
  const placed = students.filter(s => s.status === 'Placed').length;
  const notP   = students.filter(s => s.status === 'Not Placed').length;
  const offer  = students.filter(s => s.status === 'Offer in Hand').length;
  const pkgs   = students.filter(s => s.pkg > 0).map(s => s.pkg);
  const avg    = pkgs.length ? pkgs.reduce((a,b) => a+b,0)/pkgs.length : 0;
  const max    = pkgs.length ? Math.max(...pkgs) : 0;
  const rate   = total ? Math.round(placed/total*100) : 0;

  // Metrics
  document.getElementById('metrics-grid').innerHTML = `
    <div class="metric-card blue">
      <div class="m-label">Total Students</div>
      <div class="m-value blue">${total}</div>
      <div class="m-sub">registered</div>
    </div>
    <div class="metric-card green">
      <div class="m-label">Placed</div>
      <div class="m-value green">${placed}</div>
      <div class="m-sub">${offer} offer in hand</div>
    </div>
    <div class="metric-card amber">
      <div class="m-label">Placement Rate</div>
      <div class="m-value amber">${rate}%</div>
      <div class="m-sub">of total students</div>
    </div>
    <div class="metric-card red">
      <div class="m-label">Avg Package</div>
      <div class="m-value red">${avg.toFixed(1)} <span style="font-size:14px;font-weight:400">LPA</span></div>
      <div class="m-sub">Highest: ${max} LPA</div>
    </div>`;

  // Legend
  document.getElementById('status-legend').innerHTML = `
    <span class="legend-item"><span class="legend-dot" style="background:#00e5a0"></span>Placed (${placed})</span>
    <span class="legend-item"><span class="legend-dot" style="background:#f74f6e"></span>Not Placed (${notP})</span>
    <span class="legend-item"><span class="legend-dot" style="background:#f7b731"></span>Offer (${offer})</span>`;

  // Status donut
  if (chartStatus) chartStatus.destroy();
  chartStatus = new Chart(document.getElementById('statusChart'), {
    type: 'doughnut',
    data: {
      labels: ['Placed','Not Placed','Offer in Hand'],
      datasets: [{ data:[placed,notP,offer], backgroundColor:['#00e5a0','#f74f6e','#f7b731'], borderWidth:0, spacing:3, hoverOffset:6 }]
    },
    options: { responsive:true, maintainAspectRatio:false, cutout:'70%', plugins:{ legend:{display:false} } }
  });

  // Package histogram
  const bands = [
    {l:'0–5',  min:0,  max:5},  {l:'5–10', min:5,  max:10},
    {l:'10–20',min:10, max:20}, {l:'20–30',min:20, max:30},
    {l:'30–50',min:30, max:50}, {l:'50+',  min:50, max:999}
  ];
  const bCounts = bands.map(b => students.filter(s => s.pkg >= b.min && s.pkg < b.max).length);
  if (chartPkg) chartPkg.destroy();
  chartPkg = new Chart(document.getElementById('pkgChart'), {
    type:'bar',
    data:{ labels:bands.map(b=>b.l), datasets:[{data:bCounts, backgroundColor:'#4f8ef7', borderRadius:5, borderSkipped:false}] },
    options:{ responsive:true, maintainAspectRatio:false,
      plugins:{legend:{display:false}},
      scales:{
        x:{ticks:CHART_TICK, grid:CHART_GRID, border:CHART_AXIS},
        y:{ticks:{...CHART_TICK,stepSize:1}, grid:CHART_GRID, border:CHART_AXIS, beginAtZero:true}
      }
    }
  });

  // Company bars
  const coMap = {};
  students.filter(s => s.company).forEach(s => coMap[s.company] = (coMap[s.company]||0)+1);
  const sorted = Object.entries(coMap).sort((a,b)=>b[1]-a[1]).slice(0,7);
  const maxCo = sorted[0]?.[1] || 1;
  document.getElementById('company-bars').innerHTML = sorted.map(([name,cnt],i) => `
    <div class="co-row">
      <span class="co-name">${name}</span>
      <div class="co-bg"><div class="co-fill" style="width:${Math.round(cnt/maxCo*100)}%;background:${CO_COLORS[i%CO_COLORS.length]}"></div></div>
      <span class="co-count">${cnt}</span>
    </div>`).join('');

  // Branch bar
  const branches = ['CSE','ECE','IT','MECH','CIVIL'];
  const branchData = branches.map(b => {
    const t = students.filter(s => s.branch===b).length;
    const p = students.filter(s => s.branch===b && s.status==='Placed').length;
    return t ? Math.round(p/t*100) : 0;
  });
  if (chartBranch) chartBranch.destroy();
  chartBranch = new Chart(document.getElementById('branchChart'), {
    type:'bar',
    data:{ labels:branches, datasets:[{data:branchData, backgroundColor:'#00e5a0', borderRadius:5, borderSkipped:false}] },
    options:{ responsive:true, maintainAspectRatio:false,
      plugins:{legend:{display:false}, tooltip:{callbacks:{label:c=>` ${c.raw}%`}}},
      scales:{
        x:{ticks:CHART_TICK, grid:CHART_GRID, border:CHART_AXIS},
        y:{ticks:{...CHART_TICK, callback:v=>v+'%'}, grid:CHART_GRID, border:CHART_AXIS, beginAtZero:true, max:100}
      }
    }
  });

  // Year line
  const years = ['2023','2024','2025'];
  const yearData = years.map(y => students.filter(s => s.year===y && s.status==='Placed').length);
  if (chartYear) chartYear.destroy();
  chartYear = new Chart(document.getElementById('yearChart'), {
    type:'line',
    data:{
      labels:years,
      datasets:[{
        label:'Placed', data:yearData,
        borderColor:'#4f8ef7', backgroundColor:'rgba(79,142,247,.12)',
        fill:true, tension:.4,
        pointBackgroundColor:'#4f8ef7', pointRadius:5, pointHoverRadius:7, borderWidth:2
      }]
    },
    options:{ responsive:true, maintainAspectRatio:false,
      plugins:{legend:{display:false}},
      scales:{
        x:{ticks:CHART_TICK, grid:CHART_GRID, border:CHART_AXIS},
        y:{ticks:{...CHART_TICK,stepSize:1}, grid:CHART_GRID, border:CHART_AXIS, beginAtZero:true}
      }
    }
  });
}

/* ══════════════════════════════════════════
   STUDENTS TABLE
══════════════════════════════════════════ */
function filterTable() {
  const q      = (document.getElementById('search-inp').value||'').toLowerCase();
  const branch = document.getElementById('f-branch').value;
  const year   = document.getElementById('f-year').value;
  const status = document.getElementById('f-status').value;

  const showClear = q||branch||year||status;
  document.getElementById('clear-btn').classList.toggle('show', !!showClear);

  let list = [...students];
  if (branch) list = list.filter(s => s.branch===branch);
  if (year)   list = list.filter(s => s.year===year);
  if (status) list = list.filter(s => s.status===status);
  if (q) list = list.filter(s =>
    s.name.toLowerCase().includes(q) ||
    s.company.toLowerCase().includes(q) ||
    s.roll.toLowerCase().includes(q)
  );

  // Sort
  list.sort((a,b) => {
    let va = sortKey==='pkg' ? +a[sortKey] : a[sortKey];
    let vb = sortKey==='pkg' ? +b[sortKey] : b[sortKey];
    if (va < vb) return sortDir==='asc' ? -1 : 1;
    if (va > vb) return sortDir==='asc' ?  1 : -1;
    return 0;
  });

  const badgeCls = { 'Placed':'badge-placed','Not Placed':'badge-not','Offer in Hand':'badge-offer' };
  const body = document.getElementById('students-body');

  if (!list.length) {
    body.innerHTML = `<tr><td colspan="8" class="empty-row">No records match your filters.</td></tr>`;
  } else {
    body.innerHTML = list.map((s,i) => `
      <tr>
        <td class="td-num">${i+1}</td>
        <td>
          <div class="td-name">${s.name}</div>
          <div class="td-roll">${s.roll}</div>
        </td>
        <td><span class="branch-pill">${s.branch}</span></td>
        <td class="td-mono">${s.year}</td>
        <td>${s.company || '<span class="td-dash">—</span>'}</td>
        <td class="td-role">${s.role || '<span class="td-dash">—</span>'}</td>
        <td>${s.pkg>0 ? `<span class="td-pkg">${s.pkg} <small style="font-size:9px;opacity:.7">LPA</small></span>` : '<span class="td-dash">—</span>'}</td>
        <td><span class="badge ${badgeCls[s.status]}">${s.status}</span></td>
      </tr>`).join('');
  }

  document.getElementById('tbl-footer').innerHTML =
    `Showing <strong>${list.length}</strong> of <strong>${students.length}</strong> records`;
}

function sortTable(key) {
  if (sortKey === key) { sortDir = sortDir==='asc' ? 'desc' : 'asc'; }
  else { sortKey = key; sortDir = 'asc'; }
  ['name','branch','year','pkg','status'].forEach(k => {
    const el = document.getElementById('sort-'+k);
    if (el) el.textContent = k===sortKey ? (sortDir==='asc'?'↑':'↓') : '';
  });
  filterTable();
}

function clearFilters() {
  document.getElementById('search-inp').value = '';
  document.getElementById('f-branch').value   = '';
  document.getElementById('f-year').value     = '';
  document.getElementById('f-status').value   = '';
  document.getElementById('clear-btn').classList.remove('show');
  filterTable();
}

/* ══════════════════════════════════════════
   ADD ENTRY FORM
══════════════════════════════════════════ */
function toggleFields() {
  const isNotPlaced = document.getElementById('f-status-f').value === 'Not Placed';
  const companyGrp  = document.getElementById('company-grp');
  const roleGrp     = document.getElementById('role-grp');
  const pkgInp      = document.getElementById('f-pkg');
  const companyInp  = document.getElementById('f-company');
  const roleInp     = document.getElementById('f-role');
  companyGrp.style.opacity = isNotPlaced ? '.4' : '1';
  roleGrp.style.opacity    = isNotPlaced ? '.4' : '1';
  companyInp.disabled = isNotPlaced;
  roleInp.disabled    = isNotPlaced;
  pkgInp.disabled     = isNotPlaced;
  if (isNotPlaced) { companyInp.value=''; roleInp.value=''; pkgInp.value=''; }
}

function clearErr(id) {
  const el = document.getElementById('err-'+id);
  if (el) el.textContent = '';
  const inp = document.getElementById('f-'+id) || document.getElementById('f-'+id+'-f');
  if (inp) inp.classList.remove('has-err');
}

function setErr(id, msg) {
  const el = document.getElementById('err-'+id);
  if (el) el.textContent = msg;
  const inp = document.getElementById('f-'+id) || document.getElementById('f-'+id+'-f');
  if (inp) inp.classList.add('has-err');
}

function submitForm() {
  // Clear errors
  ['name','roll','branch','company'].forEach(clearErr);
  let valid = true;

  const name    = document.getElementById('f-name').value.trim();
  const roll    = document.getElementById('f-roll').value.trim();
  const branch  = document.getElementById('f-branch-f').value;
  const year    = document.getElementById('f-year-f').value;
  const status  = document.getElementById('f-status-f').value;
  const pkg     = parseFloat(document.getElementById('f-pkg').value) || 0;
  const company = document.getElementById('f-company').value.trim();
  const role    = document.getElementById('f-role').value.trim();

  if (!name)   { setErr('name',   'Name is required'); valid=false; }
  if (!roll)   { setErr('roll',   'Roll No. is required'); valid=false; }
  if (!branch) { setErr('branch', 'Select a branch'); valid=false; }
  if (status !== 'Not Placed' && !company) { setErr('company','Company name is required'); valid=false; }

  if (!valid) return;

  students.unshift({ id:Date.now(), name, roll, branch, year, status, company, role, pkg });

  // Toast
  const toast = document.getElementById('toast');
  toast.textContent = `✓ "${name}" added successfully!`;
  toast.classList.add('show');
  setTimeout(() => toast.classList.remove('show'), 3500);

  resetForm();
}

function resetForm() {
  ['f-name','f-roll','f-pkg','f-company','f-role'].forEach(id => document.getElementById(id).value='');
  document.getElementById('f-branch-f').value = '';
  document.getElementById('f-year-f').value   = '2025';
  document.getElementById('f-status-f').value = 'Placed';
  ['name','roll','branch','company'].forEach(clearErr);
  toggleFields();
}

/* ══════════════════════════════════════════
   INIT
══════════════════════════════════════════ */
renderDashboard();
filterTable();
</script>
</body>
</html>
