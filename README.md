html_content = '''<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>委託服務價格試算器</title>
  <style>
    :root {
      --primary: #4f46e5;
      --primary-hover: #4338ca;
      --primary-light: #eeef2b15;
      --bg-gradient: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
      --card-bg: #ffffff;
      --text-main: #0f172a;
      --text-muted: #64748b;
      --border-color: #e2e8f0;
      --accent-badge: #0284c7;
      --radius: 16px;
      --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.01);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    }

    body {
      background: #f1f5f9;
      color: var(--text-main);
      padding: 24px 16px;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
    }

    .calculator-container {
      width: 100%;
      max-width: 900px;
      background: var(--card-bg);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      border: 1px solid var(--border-color);
      overflow: hidden;
      display: grid;
      grid-template-columns: 1fr 340px;
    }

    @media (max-width: 768px) {
      .calculator-container {
        grid-template-columns: 1fr;
      }
    }

    /* 左側選項區 */
    .main-content {
      padding: 32px 28px;
    }

    .header {
      margin-bottom: 28px;
    }

    .header h1 {
      font-size: 1.5rem;
      font-weight: 700;
      color: var(--text-main);
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .header p {
      font-size: 0.875rem;
      color: var(--text-muted);
      margin-top: 6px;
    }

    .section-title {
      font-size: 1rem;
      font-weight: 600;
      color: #334155;
      margin-bottom: 14px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .section-title span.step-badge {
      background: #e0e7ff;
      color: var(--primary);
      font-size: 0.75rem;
      padding: 2px 8px;
      border-radius: 20px;
      font-weight: 700;
    }

    /* 基礎方案 GRID */
    .plans-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 12px;
      margin-bottom: 32px;
    }

    .plan-card {
      border: 2px solid var(--border-color);
      border-radius: 12px;
      padding: 16px;
      cursor: pointer;
      transition: all 0.2s ease;
      position: relative;
      background: #fafafa;
    }

    .plan-card:hover {
      border-color: #cbd5e1;
      background: #ffffff;
    }

    .plan-card.active {
      border-color: var(--primary);
      background: #f5f3ff;
      box-shadow: 0 4px 12px rgba(79, 70, 229, 0.12);
    }

    .plan-card input[type="radio"] {
      position: absolute;
      top: 14px;
      right: 14px;
      accent-color: var(--primary);
    }

    .plan-name {
      font-weight: 700;
      font-size: 0.95rem;
      color: var(--text-main);
      margin-bottom: 4px;
    }

    .plan-desc {
      font-size: 0.775rem;
      color: var(--text-muted);
      line-height: 1.3;
      margin-bottom: 10px;
    }

    .plan-price {
      font-size: 1.1rem;
      font-weight: 800;
      color: var(--primary);
    }

    /* 加購區塊 */
    .addon-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-bottom: 32px;
    }

    .addon-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      border: 1px solid var(--border-color);
      border-radius: 10px;
      padding: 12px 16px;
      transition: background 0.15s;
    }

    .addon-item:hover {
      background: #f8fafc;
    }

    .addon-info {
      display: flex;
      align-items: center;
      gap: 12px;
      flex: 1;
    }

    .addon-info input[type="checkbox"] {
      width: 18px;
      height: 18px;
      accent-color: var(--primary);
      cursor: pointer;
    }

    .addon-label {
      font-size: 0.875rem;
      font-weight: 600;
      cursor: pointer;
    }

    .addon-price {
      font-size: 0.875rem;
      color: var(--text-muted);
      font-weight: 600;
    }

    /* 數量選擇器 */
    .counter-group {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .counter-btn {
      width: 26px;
      height: 26px;
      border: 1px solid var(--border-color);
      background: #fff;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      color: #334155;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .counter-btn:hover {
      background: #e2e8f0;
    }

    .counter-value {
      font-size: 0.875rem;
      font-weight: 600;
      width: 20px;
      text-align: center;
    }

    /* 特殊選項：商業與急件 */
    .toggle-group {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .toggle-card {
      border: 1px solid var(--border-color);
      padding: 12px;
      border-radius: 10px;
      display: flex;
      align-items: center;
      gap: 10px;
      cursor: pointer;
    }

    .toggle-card input {
      accent-color: var(--primary);
      width: 16px;
      height: 16px;
    }

    .toggle-title {
      font-size: 0.825rem;
      font-weight: 600;
    }

    .toggle-sub {
      font-size: 0.7rem;
      color: var(--text-muted);
    }

    /* 右側試算側邊欄 */
    .sidebar {
      background: #f8fafc;
      border-left: 1px solid var(--border-color);
      padding: 32px 24px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }

    .summary-title {
      font-size: 1.1rem;
      font-weight: 700;
      padding-bottom: 12px;
      border-bottom: 2px dashed #cbd5e1;
      margin-bottom: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .summary-items {
      display: flex;
      flex-direction: column;
      gap: 12px;
      min-height: 180px;
      max-height: 320px;
      overflow-y: auto;
      padding-right: 4px;
    }

    .summary-row {
      display: flex;
      justify-content: space-between;
      font-size: 0.85rem;
      line-height: 1.4;
    }

    .summary-row .item-name {
      color: #475569;
    }

    .summary-row .item-val {
      font-weight: 600;
      color: #0f172a;
    }

    .total-box {
      margin-top: 24px;
      padding-top: 16px;
      border-top: 2px dashed #cbd5e1;
    }

    .total-label {
      font-size: 0.85rem;
      color: var(--text-muted);
      margin-bottom: 4px;
    }

    .total-price {
      font-size: 1.8rem;
      font-weight: 800;
      color: var(--primary);
    }

    .action-btn {
      width: 100%;
      background: var(--primary);
      color: #fff;
      border: none;
      padding: 14px;
      border-radius: 10px;
      font-weight: 700;
      font-size: 0.95rem;
      cursor: pointer;
      margin-top: 16px;
      transition: background 0.2s;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }

    .action-btn:hover {
      background: var(--primary-hover);
    }

    .secondary-btn {
      width: 100%;
      background: #ffffff;
      color: #475569;
      border: 1px solid var(--border-color);
      padding: 10px;
      border-radius: 10px;
      font-weight: 600;
      font-size: 0.85rem;
      cursor: pointer;
      margin-top: 8px;
      transition: all 0.2s;
    }

    .secondary-btn:hover {
      background: #f1f5f9;
      color: #0f172a;
    }

    .toast {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%) translateY(100px);
      background: #0f172a;
      color: #fff;
      padding: 10px 20px;
      border-radius: 30px;
      font-size: 0.85rem;
      box-shadow: 0 10px 20px rgba(0,0,0,0.2);
      transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      z-index: 1000;
    }

    .toast.show {
      transform: translateX(-50%) translateY(0);
    }
  </style>
</head>
<body>

<div class="calculator-container">
  <!-- 左側主要選擇區 -->
  <div class="main-content">
    <div class="header">
      <h1>✨ 委託服務估價試算</h1>
      <p>點選您所需的服務項目，系統將自動計算估算金額。</p>
    </div>

    <!-- 1. 基礎方案 -->
    <div class="section-title">
      1. 選擇基礎方案
      <span class="step-badge">必選</span>
    </div>
    <div class="plans-grid">
      <div class="plan-card active" onclick="selectPlan(this, 'Character', 6000, '角色立繪設計')">
        <input type="radio" name="base_plan" checked>
        <div class="plan-name">角色立繪</div>
        <div class="plan-desc">含精細全身設定與拆分，適合插畫或自主委託。</div>
        <div class="plan-price">$6,000</div>
      </div>
      <div class="plan-card" onclick="selectPlan(this, 'HalfBody', 12000, '半身 Live2D 建模')">
        <input type="radio" name="base_plan">
        <div class="plan-name">半身 Live2D</div>
        <div class="plan-desc">包含頭部 XYZ、半身物理與基本口型靈敏調整。</div>
        <div class="plan-price">$12,000</div>
      </div>
      <div class="plan-card" onclick="selectPlan(this, 'FullBody', 20000, '全身 Live2D 建模')">
        <input type="radio" name="base_plan">
        <div class="plan-name">全身 Live2D</div>
        <div class="plan-desc">完整全身動態、重力物理、豐富口型與睡眠動畫。</div>
        <div class="plan-price">$20,000</div>
      </div>
    </div>

    <!-- 2. 加購與配件 -->
    <div class="section-title">
      2. 附加項目與細節加購
      <span class="step-badge">可多選</span>
    </div>
    <div class="addon-list">
      <div class="addon-item">
        <div class="addon-info">
          <input type="checkbox" id="exp_check" onchange="updateCalc()">
          <label class="addon-label" for="exp_check">特殊表情切換 (按鍵控制)</label>
        </div>
        <div class="counter-group">
          <span class="addon-price">$500 / 個</span>
          <button class="counter-btn" onclick="changeCount('exp', -1)">-</button>
          <span class="counter-value" id="exp_count">1</span>
          <button class="counter-btn" onclick="changeCount('exp', 1)">+</button>
        </div>
      </div>

      <div class="addon-item">
        <div class="addon-info">
          <input type="checkbox" id="hair_check" onchange="updateCalc()">
          <label class="addon-label" for="hair_check">額外髮型 / 服裝切換</label>
        </div>
        <div class="addon-price">+$3,000 / 套</div>
      </div>

      <div class="addon-item">
        <div class="addon-info">
          <input type="checkbox" id="physics_check" onchange="updateCalc()">
          <label class="addon-label" for="physics_check">高階 Q 彈果凍物理 (眼/髮/胸/服裝)</label>
        </div>
        <div class="addon-price">+$2,500</div>
      </div>
    </div>

    <!-- 3. 特殊需求 (商業/急件) -->
    <div class="section-title">
      3. 授權與時程
    </div>
    <div class="toggle-group">
      <label class="toggle-card">
        <input type="checkbox" id="commercial_check" onchange="updateCalc()">
        <div>
          <div class="toggle-title">商業買斷 (x1.5)</div>
          <div class="toggle-sub">含周邊販售及廣告營利</div>
        </div>
      </label>
      <label class="toggle-card">
        <input type="checkbox" id="express_check" onchange="updateCalc()">
        <div>
          <div class="toggle-title">急件加急 (+30%)</div>
          <div class="toggle-sub">14 天內快速交件</div>
        </div>
      </label>
    </div>
  </div>

  <!-- 右側明細與結算頁 -->
  <div class="sidebar">
    <div>
      <div class="summary-title">
        <span>估價總覽</span>
        <span style="font-size:0.75rem; color:#64748b; font-weight:normal;">TWD (NT$)</span>
      </div>
      <div class="summary-items" id="summary_items">
        <!-- JS 動態渲染 -->
      </div>
    </div>

    <div>
      <div class="total-box">
        <div class="total-label">預估總金額 (Estimated Total)</div>
        <div class="total-price" id="total_price">$6,000</div>
      </div>

      <button class="action-btn" onclick="copySummary()">
        📋 複製委託估價明細
      </button>
      <button class="secondary-btn" onclick="resetForm()">
        ↺ 重置選項
      </button>
    </div>
  </div>
</div>

<div class="toast" id="toast">已複製估價明細至剪貼簿！</div>

<script>
  let currentPlan = {
    id: 'Character',
    name: '角色立繪設計',
    price: 6000
  };

  let counts = {
    exp: 1
  };

  function selectPlan(cardElement, id, price, name) {
    document.querySelectorAll('.plan-card').forEach(c => {
      c.classList.remove('active');
      c.querySelector('input').checked = false;
    });
    cardElement.classList.add('active');
    cardElement.querySelector('input').checked = true;

    currentPlan = { id, name, price };
    updateCalc();
  }

  function changeCount(type, delta) {
    if (counts[type] + delta >= 1 && counts[type] + delta <= 10) {
      counts[type] += delta;
      document.getElementById(`${type}_count`).innerText = counts[type];
      // 如果按加減，自動勾選對應核取方框
      document.getElementById(`${type}_check`).checked = true;
      updateCalc();
    }
  }

  function updateCalc() {
    let subtotal = currentPlan.price;
    let itemsHTML = `
      <div class="summary-row">
        <span class="item-name">${currentPlan.name}</span>
        <span class="item-val">$${currentPlan.price.toLocaleString()}</span>
      </div>
    `;

    // 表情加購
    const expChecked = document.getElementById('exp_check').checked;
    if (expChecked) {
      const expTotal = counts.exp * 500;
      subtotal += expTotal;
      itemsHTML += `
        <div class="summary-row">
          <span class="item-name">特殊表情 (${counts.exp}個)</span>
          <span class="item-val">+$${expTotal.toLocaleString()}</span>
        </div>
      `;
    }

    // 髮型切換
    const hairChecked = document.getElementById('hair_check').checked;
    if (hairChecked) {
      subtotal += 3000;
      itemsHTML += `
        <div class="summary-row">
          <span class="item-name">額外髮型/服裝</span>
          <span class="item-val">+$3,000</span>
        </div>
      `;
    }

    // 果凍物理
    const physicsChecked = document.getElementById('physics_check').checked;
    if (physicsChecked) {
      subtotal += 2500;
      itemsHTML += `
        <div class="summary-row">
          <span class="item-name">高階 Q 彈果凍物理</span>
          <span class="item-val">+$2,500</span>
        </div>
      `;
    }

    let finalTotal = subtotal;

    // 商業買斷 (x1.5)
    const commChecked = document.getElementById('commercial_check').checked;
    if (commChecked) {
      const commFee = subtotal * 0.5;
      finalTotal += commFee;
      itemsHTML += `
        <div class="summary-row" style="color:#d97706;">
          <span class="item-name">商業授權買斷 (x1.5)</span>
          <span class="item-val">+$${commFee.toLocaleString()}</span>
        </div>
      `;
    }

    // 急件加急 (+30%)
    const expressChecked = document.getElementById('express_check').checked;
    if (expressChecked) {
      const expressFee = subtotal * 0.3;
      finalTotal += expressFee;
      itemsHTML += `
        <div class="summary-row" style="color:#dc2626;">
          <span class="item-name">急件處理費 (+30%)</span>
          <span class="item-val">+$${expressFee.toLocaleString()}</span>
        </div>
      `;
    }

    document.getElementById('summary_items').innerHTML = itemsHTML;
    document.getElementById('total_price').innerText = `$${Math.round(finalTotal).toLocaleString()}`;
  }

  function copySummary() {
    let summaryText = `【委託估價單明細】\\n`;
    summaryText += `------------------\\n`;
    summaryText += `• 基礎方案：${currentPlan.name} ($${currentPlan.price.toLocaleString()})\\n`;

    if (document.getElementById('exp_check').checked) {
      summaryText += `• 特殊表情：${counts.exp} 個 ($${(counts.exp * 500).toLocaleString()})\\n`;
    }
    if (document.getElementById('hair_check').checked) {
      summaryText += `• 額外髮型/服裝切換 ($3,000)\\n`;
    }
    if (document.getElementById('physics_check').checked) {
      summaryText += `• 高階 Q 彈果凍物理 ($2,500)\\n`;
    }
    if (document.getElementById('commercial_check').checked) {
      summaryText += `• 商業授權買斷 (加收 50%)\\n`;
    }
    if (document.getElementById('express_check').checked) {
      summaryText += `• 急件加急處理 (加收 30%)\\n`;
    }

    summaryText += `------------------\\n`;
    summaryText += `預估總金額：${document.getElementById('total_price').innerText}\\n`;
    summaryText += `（此金額僅供參考，實際費用以雙方討論後定案為主）`;

    navigator.clipboard.writeText(summaryText).then(() => {
      showToast();
    });
  }

  function showToast() {
    const toast = document.getElementById('toast');
    toast.classList.add('show');
    setTimeout(() => {
      toast.classList.remove('show');
    }, 2500);
  }

  function resetForm() {
    document.querySelectorAll('input[type="checkbox"]').forEach(i => i.checked = false);
    counts.exp = 1;
    document.getElementById('exp_count').innerText = 1;
    const firstPlan = document.querySelector('.plan-card');
    selectPlan(firstPlan, 'Character', 6000, '角色立繪設計');
  }

  // 初始化
  updateCalc();
</script>

</body>
</html>
'''

with open('price_calculator.html', 'w', encoding='utf-8') as f:
    f.write(html_content)

print("Created price_calculator.html successfully.")
