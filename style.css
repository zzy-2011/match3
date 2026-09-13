(() => {
  'use strict';
  const cv = document.getElementById('game'); const ctx = cv.getContext('2d');
  const W = cv.width, H = cv.height;
  const dpr = Math.max(1, Math.min(3, window.devicePixelRatio || 1));
  cv.width = W * dpr; cv.height = H * dpr; ctx.scale(dpr, dpr);
  const movesEl = document.getElementById('moves'), levelEl = document.getElementById('level');
  const overlay = document.getElementById('overlay'), ovTitle = document.getElementById('ov-title'), ovSub = document.getElementById('ov-sub');

  const LEVELS = [
    [
      '########',
      '#      #',
      '#  ..  #',
      '#  $$  #',
      '#  @   #',
      '#      #',
      '########'
    ],
    [
      '##########',
      '#        #',
      '#  ....   #',
      '#  $$$$   #',
      '#   @     #',
      '#        #',
      '##########'
    ],
    [
      '##########',
      '#        #',
      '#  ....   #',
      '#  $$$$   #',
      '#   @     #',
      '#  ####   #',
      '#        #',
      '##########'
    ]
  ];
  let lvl, grid, pr, pc, CELL, ox, oy, rows, cols, over;

  function load(i) {
    const L = LEVELS[i]; rows = L.length; cols = Math.max(...L.map(r => r.length));
    CELL = Math.floor(Math.min(W / cols, H / rows));
    ox = (W - cols * CELL) / 2; oy = (H - rows * CELL) / 2;
    grid = []; pr = pc = -1;
    for (let r = 0; r < rows; r++) { grid[r] = []; for (let c = 0; c < cols; c++) { const ch = L[r][c] || ' '; grid[r][c] = ch; if (ch === '@') { pr = r; pc = c; grid[r][c] = ' '; } } }
    lvl = i; over = false; levelEl.textContent = (i + 1) + '/' + LEVELS.length; movesEl.textContent = '0'; overlay.classList.add('hidden');
  }
  function isBox(r, c) { return grid[r][c] === '$' || grid[r][c] === '*'; }
  function isTarget(r, c) { return grid[r][c] === '.' || grid[r][c] === '*'; }
  function move(dx, dy) {
    if (over) return;
    const nr = pr + dy, nc = pc + dx;
    if (nr < 0 || nr >= rows || nc < 0 || nc >= cols || grid[nr][nc] === '#') return;
    if (isBox(nr, nc)) {
      const br = nr + dy, bc = nc + dx;
      if (br < 0 || br >= rows || bc < 0 || bc >= cols || grid[br][bc] === '#' || isBox(br, bc)) return;
      grid[br][bc] = isTarget(br, bc) ? '*' : '$';
      grid[nr][nc] = isTarget(nr, nc) ? '.' : ' ';
    }
    pr = nr; pc = nc;
    movesEl.textContent = +movesEl.textContent + 1;
    if (checkWin()) { if (lvl + 1 < LEVELS.length) { ovTitle.textContent = '过关！'; ovSub.textContent = '进入第 ' + (lvl + 2) + ' 关'; } else { ovTitle.textContent = '全部通关！'; ovSub.textContent = '你太厉害了'; } over = true; overlay.classList.remove('hidden'); }
  }
  function checkWin() { for (let r = 0; r < rows; r++) for (let c = 0; c < cols; c++) if (grid[r][c] === '$') return false; return true; }
  function draw() {
    ctx.fillStyle = '#1a1c3a'; ctx.fillRect(0, 0, W, H);
    for (let r = 0; r < rows; r++) for (let c = 0; c < cols; c++) {
      const x = ox + c * CELL, y = oy + r * CELL, ch = grid[r][c];
      if (ch === '#') { ctx.fillStyle = '#34386e'; ctx.fillRect(x, y, CELL, CELL); }
      else {
        ctx.fillStyle = '#22254a'; ctx.fillRect(x, y, CELL, CELL);
        if (ch === '.' || ch === '*') { ctx.fillStyle = '#ffd23f'; ctx.beginPath(); ctx.arc(x + CELL / 2, y + CELL / 2, CELL * 0.18, 0, Math.PI * 2); ctx.fill(); }
        if (ch === '$' || ch === '*') { ctx.fillStyle = '#ff9f43'; ctx.fillRect(x + CELL * 0.18, y + CELL * 0.18, CELL * 0.64, CELL * 0.64); }
      }
    }
    if (pr >= 0) { ctx.fillStyle = '#43d97a'; ctx.beginPath(); ctx.arc(ox + pc * CELL + CELL / 2, oy + pr * CELL + CELL / 2, CELL * 0.3, 0, Math.PI * 2); ctx.fill(); ctx.fillStyle = '#10122a'; ctx.beginPath(); ctx.arc(ox + pc * CELL + CELL / 2, oy + pr * CELL + CELL / 2, CELL * 0.12, 0, Math.PI * 2); ctx.fill(); }
  }
  const KM = { ArrowUp: [0, -1], ArrowDown: [0, 1], ArrowLeft: [-1, 0], ArrowRight: [1, 0], w: [0, -1], s: [0, 1], a: [-1, 0], d: [1, 0], W: [0, -1], S: [0, 1], A: [-1, 0], D: [1, 0] };
  window.addEventListener('keydown', e => { if (KM[e.key]) { e.preventDefault(); move(KM[e.key][0], KM[e.key][1]); } });
  let sx = 0, sy = 0;
  cv.addEventListener('touchstart', e => { const t = e.changedTouches[0]; sx = t.clientX; sy = t.clientY; }, { passive: true });
  cv.addEventListener('touchend', e => { const t = e.changedTouches[0]; const dx = t.clientX - sx, dy = t.clientY - sy; if (Math.max(Math.abs(dx), Math.abs(dy)) > 16) { if (Math.abs(dx) > Math.abs(dy)) move(dx > 0 ? 1 : -1, 0); else move(0, dy > 0 ? 1 : -1); } }, { passive: true });
  cv.addEventListener('click', () => {});
  document.getElementById('new').addEventListener('click', () => load(lvl));
  document.getElementById('ov-btn').addEventListener('click', () => { if (over && lvl + 1 < LEVELS.length && ovTitle.textContent === '过关！') load(lvl + 1); else load(lvl); });
  function loop() { draw(); requestAnimationFrame(loop); }
  load(0); requestAnimationFrame(loop);
})();
