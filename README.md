[Uploading index (8).html…]()
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>เมนูอาหารของเรา</title>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@500;600;700&family=Sarabun:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #F1EEE1;
    --bg-alt: #E8E3D2;
    --surface: #FFFEF9;
    --surface-soft: #FBF7EC;
    --ink: #2E3B32;
    --ink-soft: #6E7568;
    --line: #DFDAC5;
    --accent: #2F6B4F;
    --accent-dark: #204F39;
    --accent-tint: #E4EEE6;
    --gold: #C9852A;
    --gold-tint: #F6E7CC;
    --rust: #AE4E33;
    --rust-tint: #F3E0D8;
    --shadow: 0 8px 24px -12px rgba(46, 59, 50, 0.25);
    --shadow-lift: 0 16px 32px -14px rgba(46, 59, 50, 0.35);
    --radius: 18px;
  }

  * { box-sizing: border-box; }

  body {
    font-family: 'Sarabun', 'Segoe UI', Tahoma, sans-serif;
    background: var(--bg);
    background-image:
      radial-gradient(circle at 12% 8%, var(--bg-alt) 0, transparent 40%),
      radial-gradient(circle at 92% 88%, var(--bg-alt) 0, transparent 45%);
    color: var(--ink);
    margin: 0;
    padding: 40px 20px 60px;
    min-height: 100vh;
  }

  .container {
    max-width: 840px;
    margin: 0 auto;
  }

  header.page-head {
    text-align: center;
    margin-bottom: 12px;
  }

  h1 {
    font-family: 'Kanit', sans-serif;
    font-weight: 700;
    font-size: clamp(28px, 4vw, 38px);
    letter-spacing: 0.2px;
    margin: 0 0 6px;
    color: var(--accent-dark);
  }

  .subtitle {
    font-size: 15px;
    color: var(--ink-soft);
    margin: 0;
  }

  .scallop-divider {
    height: 14px;
    margin: 22px auto 34px;
    max-width: 260px;
    background-image: radial-gradient(circle at 7px 0, transparent 7px, var(--gold) 7.5px);
    background-size: 14px 14px;
    background-repeat: repeat-x;
    background-position: center top;
    opacity: 0.85;
  }

  .form-card {
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: var(--radius);
    padding: 24px 24px 22px;
    margin-bottom: 36px;
    box-shadow: var(--shadow);
  }

  .form-card h2 {
    font-family: 'Kanit', sans-serif;
    font-weight: 600;
    font-size: 17px;
    margin: 0 0 16px;
    color: var(--accent-dark);
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .field-grid {
    display: grid;
    grid-template-columns: 2fr 1fr auto auto;
    gap: 14px;
    align-items: end;
  }

  .field label {
    display: block;
    font-size: 12.5px;
    font-weight: 600;
    color: var(--ink-soft);
    margin-bottom: 6px;
    letter-spacing: 0.2px;
  }

  input[type="text"], input[type="number"] {
    width: 100%;
    padding: 11px 14px;
    border-radius: 12px;
    border: 1.5px solid var(--line);
    background: var(--surface-soft);
    color: var(--ink);
    font-family: 'Sarabun', sans-serif;
    font-size: 14.5px;
    transition: border-color 0.15s ease, box-shadow 0.15s ease;
  }

  input[type="text"]::placeholder, input[type="number"]::placeholder {
    color: #B4AF9C;
  }

  input[type="text"]:focus, input[type="number"]:focus {
    outline: none;
    border-color: var(--accent);
    box-shadow: 0 0 0 3px var(--accent-tint);
  }

  .checkbox-field {
    display: flex;
    align-items: center;
    gap: 8px;
    height: 44px;
    padding: 0 4px;
  }

  label.checkbox {
    display: flex;
    align-items: center;
    gap: 7px;
    font-size: 14px;
    color: var(--ink-soft);
    white-space: nowrap;
    cursor: pointer;
  }

  input[type="checkbox"] {
    width: 17px;
    height: 17px;
    accent-color: var(--accent);
    cursor: pointer;
  }

  button {
    padding: 12px 22px;
    height: 44px;
    border-radius: 12px;
    border: none;
    background: var(--accent);
    color: #fff;
    font-family: 'Sarabun', sans-serif;
    font-weight: 600;
    font-size: 14.5px;
    cursor: pointer;
    transition: background 0.15s ease, transform 0.1s ease;
    white-space: nowrap;
  }

  button:hover:not(:disabled) { background: var(--accent-dark); }
  button:active:not(:disabled) { transform: translateY(1px); }
  button:disabled { background: #C7C2B0; cursor: not-allowed; }
  button:focus-visible, input:focus-visible { outline: 2px solid var(--gold); outline-offset: 2px; }

  .menu-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
    gap: 18px;
  }

  .menu-card {
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: var(--radius);
    padding: 18px 18px 16px;
    position: relative;
    box-shadow: var(--shadow);
    transition: transform 0.18s ease, box-shadow 0.18s ease;
  }

  .menu-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lift);
  }

  .menu-card h3 {
    font-family: 'Kanit', sans-serif;
    font-weight: 600;
    font-size: 17px;
    margin: 0 0 12px;
    color: var(--ink);
    padding-right: 54px;
  }

  .price-tag {
    position: absolute;
    top: 14px;
    right: 14px;
    background: var(--gold-tint);
    color: var(--gold);
    font-family: 'Kanit', sans-serif;
    font-weight: 600;
    font-size: 13.5px;
    padding: 5px 10px;
    border-radius: 20px 20px 20px 4px;
  }

  .ticket-divider {
    height: 1px;
    margin: 4px 0 12px;
    background-image: repeating-linear-gradient(to right, var(--line) 0, var(--line) 5px, transparent 5px, transparent 10px);
  }

  .price-row {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 10px;
  }

  .price {
    font-family: 'Kanit', sans-serif;
    font-size: 21px;
    font-weight: 700;
    color: var(--accent-dark);
  }

  .price .thb { font-size: 13px; font-weight: 500; color: var(--ink-soft); margin-right: 2px; }

  .badge {
    display: inline-block;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
  }

  .badge.available { background: var(--accent-tint); color: var(--accent-dark); }
  .badge.unavailable { background: var(--rust-tint); color: var(--rust); }

  .status {
    text-align: center;
    color: var(--ink-soft);
    padding: 40px 20px;
    font-size: 15px;
  }

  .error {
    background: var(--rust-tint);
    color: var(--rust);
    border: 1px solid #E3B7A3;
    padding: 12px 16px;
    border-radius: 12px;
    margin-bottom: 20px;
    display: none;
    font-size: 14px;
  }

  .order-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    margin-top: 14px;
  }

  .qty-stepper {
    display: flex;
    align-items: center;
    gap: 10px;
    background: var(--surface-soft);
    border: 1.5px solid var(--line);
    border-radius: 10px;
    padding: 4px 6px;
  }

  .qty-btn {
    width: 26px;
    height: 26px;
    padding: 0;
    border-radius: 7px;
    background: transparent;
    color: var(--accent-dark);
    border: none;
    font-size: 16px;
    font-weight: 700;
    line-height: 1;
  }

  .qty-btn:hover:not(:disabled) { background: var(--accent-tint); }

  .qty-value {
    min-width: 16px;
    text-align: center;
    font-family: 'Kanit', sans-serif;
    font-weight: 600;
    font-size: 14px;
  }

  .order-btn {
    padding: 8px 16px;
    height: auto;
    font-size: 13.5px;
    border-radius: 10px;
    background: var(--gold);
  }

  .order-btn:hover:not(:disabled) { background: #B4761F; }

  .section-head {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    margin: 44px 0 16px;
  }

  .section-head h2 {
    font-family: 'Kanit', sans-serif;
    font-weight: 600;
    font-size: 19px;
    color: var(--accent-dark);
    margin: 0;
  }

  .section-head .count {
    font-size: 13px;
    color: var(--ink-soft);
  }

  .history-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .history-row {
    display: flex;
    align-items: center;
    gap: 14px;
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: 14px;
    padding: 12px 16px;
    box-shadow: var(--shadow);
  }

  .history-row .h-name {
    font-family: 'Kanit', sans-serif;
    font-weight: 600;
    font-size: 15px;
    flex: 1;
    min-width: 0;
  }

  .history-row .h-qty {
    font-size: 13px;
    color: var(--ink-soft);
    background: var(--surface-soft);
    border: 1px solid var(--line);
    border-radius: 20px;
    padding: 3px 10px;
    white-space: nowrap;
  }

  .history-row .h-total {
    font-family: 'Kanit', sans-serif;
    font-weight: 700;
    color: var(--accent-dark);
    font-size: 15px;
    white-space: nowrap;
  }

  .history-row .h-time {
    font-size: 12px;
    color: var(--ink-soft);
    white-space: nowrap;
    min-width: 90px;
    text-align: right;
  }

  @media (max-width: 560px) {
    .history-row { flex-wrap: wrap; }
    .history-row .h-time { margin-left: auto; }
  }

  @media (max-width: 620px) {
    .field-grid {
      grid-template-columns: 1fr 1fr;
    }
    .field-grid .field:first-child { grid-column: 1 / -1; }
    .checkbox-field, .field-grid > button { grid-column: auto; }
  }

  @media (prefers-reduced-motion: reduce) {
    .menu-card, button { transition: none; }
  }
</style>
</head>
<body>
<div class="container">
  <header class="page-head">
    <h1>เมนูอาหารของเรา</h1>
    <p class="subtitle">ดึงข้อมูลสดจาก Supabase</p>
  </header>
  <div class="scallop-divider"></div>

  <div id="errorBox" class="error"></div>

  <div class="form-card">
    <h2>เพิ่มเมนูใหม่</h2>
    <div class="field-grid">
      <div class="field">
        <label for="nameInput">ชื่อเมนู</label>
        <input type="text" id="nameInput" placeholder="เช่น ผัดไทย">
      </div>
      <div class="field">
        <label for="priceInput">ราคา (บาท)</label>
        <input type="number" id="priceInput" placeholder="0" min="0" step="1">
      </div>
      <div class="checkbox-field">
        <label class="checkbox">
          <input type="checkbox" id="availableInput" checked>
          มีขาย
        </label>
      </div>
      <button id="addBtn" onclick="addMenu()">เพิ่มเมนู</button>
    </div>
  </div>

  <div id="menuList" class="status">กำลังโหลดเมนู...</div>

  <div class="section-head">
    <h2>ประวัติการสั่งซื้อ</h2>
    <span class="count" id="orderCount"></span>
  </div>
  <div id="orderHistory" class="status">กำลังโหลดประวัติ...</div>
</div>

<script>
  window.addEventListener("error", function (e) {
    const box = document.getElementById("errorBox");
    box.textContent = "เกิดข้อผิดพลาด: " + e.message;
    box.style.display = "block";
  });

  // ตั้งค่า Supabase connection
  const SUPABASE_URL = "https://yhaksaikntrvwfyfxmmc.supabase.co";
  const SUPABASE_KEY = "sb_publishable_4hEmknXZe14zjyTq11XnBQ_rHLY9YEH";

  if (typeof window.supabase === "undefined") {
    document.getElementById("menuList").className = "status";
    document.getElementById("menuList").textContent = "";
    document.getElementById("errorBox").textContent =
      "โหลดไลบรารี Supabase จาก CDN ไม่สำเร็จ กรุณาตรวจสอบว่าเชื่อมต่ออินเทอร์เน็ตอยู่ แล้วลองรีเฟรชหน้านี้ใหม่ (กด Ctrl+Shift+R)";
    document.getElementById("errorBox").style.display = "block";
    throw new Error("supabase-js failed to load from CDN");
  }

  const supabaseClient = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

  // ฟังการเปลี่ยนแปลงข้อมูลแบบเรียลไทม์จากตาราง menu
  supabaseClient
    .channel("menu-changes")
    .on(
      "postgres_changes",
      { event: "*", schema: "public", table: "menu" },
      (payload) => {
        if (payload.eventType === "INSERT") {
          // กันไม่ให้ซ้ำ ถ้าแถวนี้ถูกเพิ่มไปแล้วจากฟอร์มในหน้านี้เอง
          if (!menuItems.some((item) => item.id === payload.new.id)) {
            menuItems.unshift(payload.new);
            renderMenu();
          }
        } else if (payload.eventType === "UPDATE") {
          menuItems = menuItems.map((item) =>
            item.id === payload.new.id ? payload.new : item
          );
          renderMenu();
        } else if (payload.eventType === "DELETE") {
          menuItems = menuItems.filter((item) => item.id !== payload.old.id);
          renderMenu();
        }
      }
    )
    .subscribe((status, err) => {
      console.log("Realtime status:", status, err || "");
      if (status === "SUBSCRIBED") {
        console.log("✅ เชื่อมต่อ Realtime สำเร็จ กำลังฟังการเปลี่ยนแปลงตาราง menu");
      } else if (status === "CHANNEL_ERROR" || status === "TIMED_OUT") {
        showError("เชื่อมต่อ Realtime ไม่สำเร็จ (" + status + ") — ลองตรวจสอบว่าเปิด Realtime ให้ตาราง menu แล้วหรือยัง");
      }
    });

  function showError(msg) {
    const box = document.getElementById("errorBox");
    box.textContent = msg;
    box.style.display = "block";
  }

  function clearError() {
    document.getElementById("errorBox").style.display = "none";
  }

  let menuItems = []; // เก็บรายการเมนูไว้ในหน่วยความจำ เพื่ออัปเดตหน้าจอได้ทันที
  let orderQty = {};  // จำนวนที่เลือกไว้ต่อเมนู { [menuId]: qty }

  function changeQty(id, delta) {
    const current = orderQty[id] || 1;
    const next = Math.max(1, current + delta);
    orderQty[id] = next;
    const span = document.getElementById("qty-" + id);
    if (span) span.textContent = next;
  }

  function renderMenu() {
    const listEl = document.getElementById("menuList");

    if (!menuItems || menuItems.length === 0) {
      listEl.className = "status";
      listEl.textContent = "ยังไม่มีเมนูในระบบ ลองเพิ่มเมนูแรกได้เลย!";
      return;
    }

    listEl.className = "menu-grid";
    listEl.innerHTML = menuItems.map(item => `
      <div class="menu-card">
        <span class="price-tag">${item.price != null ? "฿" + item.price : "-"}</span>
        <h3>${escapeHtml(item.name || "(ไม่มีชื่อ)")}</h3>
        <div class="ticket-divider"></div>
        <div class="price-row">
          <span class="badge ${item.is_available ? "available" : "unavailable"}">
            ${item.is_available ? "มีขาย" : "หมด"}
          </span>
        </div>
        <div class="order-row">
          <div class="qty-stepper">
            <button type="button" class="qty-btn" onclick="changeQty(${item.id}, -1)" ${!item.is_available ? "disabled" : ""}>−</button>
            <span class="qty-value" id="qty-${item.id}">${orderQty[item.id] || 1}</span>
            <button type="button" class="qty-btn" onclick="changeQty(${item.id}, 1)" ${!item.is_available ? "disabled" : ""}>+</button>
          </div>
          <button type="button" class="order-btn" id="order-btn-${item.id}" onclick="orderItem(${item.id})" ${!item.is_available ? "disabled" : ""}>สั่งเมนูนี้</button>
        </div>
      </div>
    `).join("");
  }

  async function loadMenu() {
    const listEl = document.getElementById("menuList");
    listEl.className = "status";
    listEl.textContent = "กำลังโหลดเมนู...";

    const { data, error } = await supabaseClient
      .from("menu")
      .select("*")
      .order("created_at", { ascending: false });

    if (error) {
      showError("โหลดข้อมูลไม่สำเร็จ: " + error.message);
      listEl.textContent = "";
      return;
    }
    clearError();

    menuItems = data || [];
    renderMenu();
  }

  async function addMenu() {
    const name = document.getElementById("nameInput").value.trim();
    const price = document.getElementById("priceInput").value;
    const available = document.getElementById("availableInput").checked;
    const btn = document.getElementById("addBtn");

    if (!name) {
      showError("กรุณากรอกชื่อเมนู");
      return;
    }

    btn.disabled = true;
    btn.textContent = "กำลังเพิ่ม...";

    const { data, error } = await supabaseClient
      .from("menu")
      .insert({
        name: name,
        price: price ? Number(price) : null,
        is_available: available
      })
      .select()
      .single();

    btn.disabled = false;
    btn.textContent = "เพิ่มเมนู";

    if (error) {
      showError("เพิ่มเมนูไม่สำเร็จ: " + error.message);
      return;
    }

    clearError();
    document.getElementById("nameInput").value = "";
    document.getElementById("priceInput").value = "";
    document.getElementById("availableInput").checked = true;

    // แสดงเมนูที่เพิ่งเพิ่มขึ้นบนสุดทันที ไม่ต้องรอโหลดใหม่
    if (data) {
      menuItems.unshift(data);
      renderMenu();
    } else {
      // เผื่อกรณี select() คืนค่าไม่ได้ (เช่น policy ไม่อนุญาตให้อ่านหลัง insert)
      loadMenu();
    }
  }

  function escapeHtml(str) {
    const div = document.createElement("div");
    div.textContent = str;
    return div.innerHTML;
  }

  // ------- สั่งซื้อ + ประวัติการสั่งซื้อ -------

  let orderHistoryItems = [];

  function formatTime(iso) {
    try {
      return new Date(iso).toLocaleString("th-TH", {
        day: "2-digit",
        month: "short",
        hour: "2-digit",
        minute: "2-digit"
      });
    } catch (e) {
      return "";
    }
  }

  function renderOrderHistory() {
    const el = document.getElementById("orderHistory");
    const countEl = document.getElementById("orderCount");

    if (!orderHistoryItems || orderHistoryItems.length === 0) {
      el.className = "status";
      el.textContent = "ยังไม่มีประวัติการสั่งซื้อ";
      countEl.textContent = "";
      return;
    }

    countEl.textContent = orderHistoryItems.length + " รายการ";
    el.className = "history-list";
    el.innerHTML = orderHistoryItems.map(o => `
      <div class="history-row">
        <span class="h-name">${escapeHtml(o.menu_name || "(ไม่ทราบเมนู)")}</span>
        <span class="h-qty">x${o.quantity}</span>
        <span class="h-total">฿${o.total != null ? o.total : "-"}</span>
        <span class="h-time">${formatTime(o.created_at)}</span>
      </div>
    `).join("");
  }

  async function loadOrders() {
    const { data, error } = await supabaseClient
      .from("orders")
      .select("*")
      .order("created_at", { ascending: false })
      .limit(50);

    if (error) {
      // ไม่มีตาราง orders หรือดึงไม่ได้ — แสดงข้อความแบบนุ่มนวล ไม่ขึ้น error แดงทับ
      const el = document.getElementById("orderHistory");
      el.className = "status";
      el.textContent = "ยังไม่พร้อมใช้งานประวัติการสั่งซื้อ (ตรวจสอบว่าสร้างตาราง orders และตั้งค่า RLS แล้วหรือยัง)";
      document.getElementById("orderCount").textContent = "";
      return;
    }

    orderHistoryItems = data || [];
    renderOrderHistory();
  }

  async function orderItem(menuId) {
    const item = menuItems.find((m) => m.id === menuId);
    if (!item) return;

    const qty = orderQty[menuId] || 1;
    const total = item.price != null ? Number(item.price) * qty : null;
    const btn = document.getElementById("order-btn-" + menuId);

    if (btn) {
      btn.disabled = true;
      btn.textContent = "กำลังสั่ง...";
    }

    const { data, error } = await supabaseClient
      .from("orders")
      .insert({
        menu_name: item.name,
        price: item.price,
        quantity: qty,
        total: total
      })
      .select()
      .single();

    if (btn) {
      btn.disabled = false;
      btn.textContent = "สั่งเมนูนี้";
    }

    if (error) {
      showError("สั่งเมนูไม่สำเร็จ: " + error.message);
      return;
    }

    clearError();
    orderQty[menuId] = 1;
    const span = document.getElementById("qty-" + menuId);
    if (span) span.textContent = 1;

    if (data && !orderHistoryItems.some((o) => o.id === data.id)) {
      orderHistoryItems.unshift(data);
      renderOrderHistory();
    } else if (!data) {
      loadOrders();
    }
  }

  // ฟังการเปลี่ยนแปลงข้อมูลแบบเรียลไทม์จากตาราง orders
  supabaseClient
    .channel("orders-changes")
    .on(
      "postgres_changes",
      { event: "INSERT", schema: "public", table: "orders" },
      (payload) => {
        if (!orderHistoryItems.some((o) => o.id === payload.new.id)) {
          orderHistoryItems.unshift(payload.new);
          renderOrderHistory();
        }
      }
    )
    .subscribe();

  loadMenu();
  loadOrders();
</script>
</body>
</html>
