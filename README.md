# us-and-money
An easy to use expense tracker[us-and-money (1).html](https://github.com/user-attachments/files/26638413/us-and-money.1.html)

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Us & Money</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Palatino Linotype', Palatino, Georgia, serif;
    background: #faf8f4;
    color: #1a1612;
    min-height: 100vh;
    padding: 28px 16px 60px;
  }
  #app { max-width: 480px; margin: 0 auto; }
  .header { text-align: center; margin-bottom: 28px; }
  .header-eyebrow { font-size: 10px; letter-spacing: 3px; color: #a07850; text-transform: uppercase; margin-bottom: 6px; }
  .header-title { font-size: 28px; font-weight: normal; color: #1a1612; }
  .balance-card { border-radius: 14px; padding: 18px 20px; text-align: center; margin-bottom: 14px; border: 1px solid #e8e0d4; background: #fff; }
  .balance-label { font-size: 10px; letter-spacing: 2px; color: #a09080; text-transform: uppercase; margin-bottom: 8px; }
  .balance-text { font-size: 20px; font-weight: normal; }
  .balance-text.settled { color: #4a8a5a; }
  .balance-text.owe-andrea { color: #4a8a5a; }
  .balance-text.owe-ethan { color: #c05040; }
  .stats { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 10px; margin-bottom: 22px; }
  .stat-card { background: #fff; border: 1px solid #e8e0d4; border-radius: 12px; padding: 12px 8px; text-align: center; }
  .stat-label { font-size: 10px; letter-spacing: 1.5px; color: #a09080; text-transform: uppercase; margin-bottom: 6px; }
  .stat-value { font-size: 15px; font-weight: bold; color: #1a1612; }
  .tabs { display: flex; gap: 8px; margin-bottom: 18px; }
  .tab { flex: 1; padding: 10px; font-family: inherit; font-size: 14px; border-radius: 10px; border: 1px solid #d8cfc4; background: #fff; color: #888; cursor: pointer; transition: all 0.2s; }
  .tab.active { background: #b07840; color: #fff; border-color: #b07840; font-weight: bold; }
  .flash { display: none; padding: 10px 14px; border-radius: 10px; margin-bottom: 14px; font-size: 14px; }
  .flash.success { background: #eef7f0; color: #3a7a4a; border: 1px solid #b0d8ba; }
  .flash.error { background: #fdf0ee; color: #b04030; border: 1px solid #e0b8b4; }
  .form-card { background: #fff; border: 1px solid #e8e0d4; border-radius: 14px; padding: 20px; display: flex; flex-direction: column; gap: 14px; }
  .field-label { font-size: 10px; letter-spacing: 1.5px; color: #a09080; text-transform: uppercase; margin-bottom: 6px; }
  input, select { width: 100%; padding: 11px 14px; border: 1px solid #ddd; border-radius: 10px; font-family: inherit; font-size: 15px; background: #faf8f4; color: #1a1612; outline: none; transition: border-color 0.2s; appearance: none; }
  input:focus, select:focus { border-color: #b07840; }
  input::placeholder { color: #c0b8b0; }
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .btn-add { width: 100%; padding: 13px; background: #b07840; color: #fff; border: none; border-radius: 10px; font-family: inherit; font-size: 16px; font-weight: bold; cursor: pointer; transition: background 0.2s; }
  .btn-add:hover { background: #8a5e30; }
  .tx-item { background: #fff; border: 1px solid #e8e0d4; border-radius: 12px; padding: 13px 14px; margin-bottom: 9px; display: flex; justify-content: space-between; align-items: center; gap: 10px; }
  .tx-desc { font-size: 15px; color: #1a1612; margin-bottom: 3px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .tx-meta { font-size: 12px; color: #a09080; }
  .tx-amount { font-size: 16px; font-weight: bold; color: #1a1612; text-align: right; }
  .tx-each { font-size: 11px; color: #b0a898; text-align: right; }
  .tx-del { background: none; border: 1px solid #e0d8d0; color: #c8c0b8; font-size: 18px; cursor: pointer; padding: 4px 9px; flex-shrink: 0; border-radius: 8px; font-family: inherit; transition: all 0.2s; line-height: 1; }
  .tx-del:hover, .tx-del:active { background: #fdf0ee; color: #c05040; border-color: #e0b8b4; }
  .empty { text-align: center; color: #b0a898; padding: 50px 0; font-size: 15px; }
  #panel-history { display: none; }
  select option { background: #fff; }
</style>
</head>
<body>
<div id="app">
  <div class="header">
    <div class="header-eyebrow">Shared Finances</div>
    <div class="header-title">Us &amp; Money &#x1F491;</div>
  </div>
  <div class="balance-card">
    <div class="balance-label">Balance</div>
    <div class="balance-text settled" id="balance-text">All settled up!</div>
  </div>
  <div class="stats">
    <div class="stat-card">
      <div class="stat-label">Total</div>
      <div class="stat-value" id="stat-total">$0.00</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Andrea paid</div>
      <div class="stat-value" id="stat-andrea">$0.00</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Ethan paid</div>
      <div class="stat-value" id="stat-ethan">$0.00</div>
    </div>
  </div>
  <div class="tabs">
    <button class="tab active" id="tab-add" onclick="showTab('add')">+ Add expense</button>
    <button class="tab" id="tab-history" onclick="showTab('history')">History (<span id="tx-count">0</span>)</button>
  </div>
  <div class="flash" id="flash"></div>
  <div id="panel-add">
    <div class="form-card">
      <div>
        <div class="field-label">Description</div>
        <input type="text" id="inp-desc" placeholder="e.g. Electricity bill" />
      </div>
      <div>
        <div class="field-label">Amount ($)</div>
        <input type="number" id="inp-amount" placeholder="0.00" step="0.01" min="0" />
      </div>
      <div>
        <div class="field-label">Category</div>
        <select id="inp-category">
          <option>🏠 Rent</option>
          <option>🛒 Groceries</option>
          <option>💡 Utilities</option>
          <option>🍽️ Dining</option>
          <option>🚗 Transport</option>
          <option>💊 Health</option>
          <option>📦 Other</option>
        </select>
      </div>
      <div class="two-col">
        <div>
          <div class="field-label">Paid by</div>
          <select id="inp-paidby">
            <option>Andrea</option>
            <option>Ethan</option>
          </select>
        </div>
        <div>
          <div class="field-label">Split</div>
          <select disabled style="opacity:0.5;">
            <option>50 / 50</option>
          </select>
        </div>
      </div>
      <button class="btn-add" onclick="addTx()">Add Transaction</button>
    </div>
  </div>
  <div id="panel-history">
    <div id="tx-list"></div>
  </div>
</div>

<script>
var txns = [];

function fmt(n) {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(n);
}

function load() {
  try { var r = localStorage.getItem('couple-txns'); if (r) txns = JSON.parse(r); } catch(e) {}
  render();
}

function save() {
  try { localStorage.setItem('couple-txns', JSON.stringify(txns)); } catch(e) {}
}

function render() {
  var total = 0, andrea = 0, ethan = 0, bal = 0;
  txns.forEach(function(t) {
    total += t.amount;
    if (t.paidBy === 'Andrea') andrea += t.amount; else ethan += t.amount;
    bal += (t.paidBy === 'Andrea' ? 1 : -1) * (t.amount / 2);
  });
  document.getElementById('stat-total').textContent = fmt(total);
  document.getElementById('stat-andrea').textContent = fmt(andrea);
  document.getElementById('stat-ethan').textContent = fmt(ethan);
  document.getElementById('tx-count').textContent = txns.length;

  var bt = document.getElementById('balance-text');
  bt.className = 'balance-text';
  if (Math.abs(bal) < 0.01) {
    bt.textContent = 'All settled up!';
    bt.classList.add('settled');
  } else if (bal > 0) {
    bt.textContent = 'Ethan owes Andrea ' + fmt(Math.abs(bal));
    bt.classList.add('owe-andrea');
  } else {
    bt.textContent = 'Andrea owes Ethan ' + fmt(Math.abs(bal));
    bt.classList.add('owe-ethan');
  }

  var list = document.getElementById('tx-list');
  if (txns.length === 0) {
    list.innerHTML = '<div class="empty">No transactions yet — add one!</div>';
    return;
  }
  var html = '';
  for (var i = 0; i < txns.length; i++) {
    var t = txns[i];
    html += '<div class="tx-item">'
      + '<div style="flex:1;min-width:0;">'
      + '<div class="tx-desc">' + t.category + ' ' + escHtml(t.desc) + '</div>'
      + '<div class="tx-meta">' + t.date + ' &middot; ' + t.paidBy + ' paid</div>'
      + '</div>'
      + '<div style="flex-shrink:0;text-align:right;">'
      + '<div class="tx-amount">' + fmt(t.amount) + '</div>'
      + '<div class="tx-each">each: ' + fmt(t.amount / 2) + '</div>'
      + '</div>'
      + '<button class="tx-del" onclick="delTx(' + i + ')">&#x2715;</button>'
      + '</div>';
  }
  list.innerHTML = html;
}

function escHtml(str) {
  return String(str).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

function showFlash(msg, isErr) {
  var el = document.getElementById('flash');
  el.textContent = msg;
  el.className = 'flash ' + (isErr ? 'error' : 'success');
  el.style.display = 'block';
  setTimeout(function() { el.style.display = 'none'; }, 2200);
}

function addTx() {
  var desc = document.getElementById('inp-desc').value.trim();
  var amount = parseFloat(document.getElementById('inp-amount').value);
  var category = document.getElementById('inp-category').value;
  var paidBy = document.getElementById('inp-paidby').value;
  if (!desc || isNaN(amount) || amount <= 0) {
    showFlash('Please enter a description and a valid amount.', true);
    return;
  }
  txns.unshift({ id: Date.now(), desc: desc, amount: amount, category: category, paidBy: paidBy, date: new Date().toLocaleDateString() });
  save(); render();
  document.getElementById('inp-desc').value = '';
  document.getElementById('inp-amount').value = '';
  showFlash('Transaction added!', false);
}

function delTx(i) {
  txns.splice(i, 1);
  save();
  render();
  showFlash('Transaction deleted.', false);
}

function showTab(t) {
  document.getElementById('panel-add').style.display = t === 'add' ? 'block' : 'none';
  document.getElementById('panel-history').style.display = t === 'history' ? 'block' : 'none';
  document.getElementById('tab-add').className = 'tab' + (t === 'add' ? ' active' : '');
  document.getElementById('tab-history').className = 'tab' + (t === 'history' ? ' active' : '');
}

document.getElementById('inp-desc').addEventListener('keydown', function(e) { if (e.key === 'Enter') addTx(); });
document.getElementById('inp-amount').addEventListener('keydown', function(e) { if (e.key === 'Enter') addTx(); });

load();
</script>
</body>
</html>
