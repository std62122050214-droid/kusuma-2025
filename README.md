<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>👷Safety Patrol & 5S👷‍♀️</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Sarabun', sans-serif; background-color: #f8fafc; color: #334155; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }
        .modal-overlay { position: fixed; inset: 0; background: rgba(15, 23, 42, 0.85); z-index: 100; display: flex; align-items: center; justify-content: center; padding: 12px; backdrop-filter: blur(8px); }
        .modal-box { background: white; width: 100%; max-width: 48rem; border-radius: 1.5rem; max-height: 94vh; display: flex; flex-direction: column; overflow: hidden; }
        .input-field { border: 2px solid #e2e8f0; border-radius: 0.75rem; padding: 0.65rem 0.85rem; width: 100%; outline: none; transition: 0.2s; font-size: 16px; }
        .input-field:focus { border-color: #3b82f6; }
        .case-card { background: white; border-radius: 1.25rem; border: 1px solid #e2e8f0; transition: 0.2s; cursor: pointer; }
        .img-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); gap: 8px; }
        .preview-img { width: 100%; height: 100px; object-fit: cover; border-radius: 8px; border: 1px solid #ddd; }
    </style>
</head>
<body>

    <div id="page-home" class="min-h-screen p-4 md:p-12">
        <div class="max-w-6xl mx-auto text-center mb-10">
            <h1 class="text-2xl md:text-4xl font-extrabold text-slate-800 mb-6 uppercase">👷Safety Patrol & 5S👷‍♀️</h1>
            <div class="flex justify-center">
                <select id="yearSelect" class="font-bold text-blue-600 outline-none text-lg bg-white p-2 rounded-xl border"></select>
            </div>
        </div>
        <div id="monthGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 max-w-6xl mx-auto"></div>
    </div>

    <div id="page-list" class="hidden min-h-screen pb-24">
        <nav class="sticky top-0 z-50 bg-white/95 border-b p-4 mb-4 shadow-sm">
            <div class="max-w-5xl mx-auto flex justify-between items-center">
                <button onclick="goBackHome()" class="font-bold text-slate-500 flex items-center gap-1"><i data-lucide="arrow-left" size="18"></i> กลับ</button>
                <h2 id="currentMonthTitle" class="font-bold text-slate-800"></h2>
                <button onclick="openForm()" class="bg-blue-600 text-white px-4 py-2 rounded-lg font-bold shadow-md">+ เพิ่มข้อมูล</button>
            </div>
        </nav>
        <div id="dataItems" class="max-w-4xl mx-auto px-4 space-y-4"></div>
    </div>

    <div id="modal-form" class="modal-overlay hidden">
        <div class="modal-box">
            <div class="p-4 border-b bg-slate-900 text-white flex justify-between">
                <h3 class="font-bold">บันทึกการตรวจพบ</h3>
                <button onclick="closeModal('modal-form')"><i data-lucide="x"></i></button>
            </div>
            <form id="mainForm" class="overflow-y-auto p-6 space-y-5">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="text-xs font-bold text-slate-400 block mb-1">📅 วันที่ตรวจ</label>
                        <input type="date" id="postDate" required class="input-field">
                    </div>
                    <div>
                        <label class="text-xs font-bold text-slate-400 block mb-1">👤 ผู้รายงาน</label>
                        <input type="text" id="reporter" required class="input-field" placeholder="ชื่อผู้รายงาน">
                    </div>
                </div>
                <div>
                    <label class="text-xs font-bold text-slate-400 block mb-1">📍 แผนกที่พบ</label>
                    <select id="dept" class="input-field">
                        <option>Welding</option>
                        <option>Assembly</option>
                        <option>Manufacturing</option>
                        <option>Inventory</option>
                        <option>Quality Check</option> <option>อื่นๆ</option>
                    </select>
                </div>
                <div>
                    <label class="text-xs font-bold text-slate-400 block mb-2">🏷️ หมวดหมู่ (เลือกได้หลายข้อ)</label>
                    <div class="flex gap-4">
                        <label class="flex items-center gap-2 cursor-pointer bg-slate-50 px-3 py-2 rounded-lg border">
                            <input type="checkbox" name="category" value="Safety" class="w-4 h-4"> Safety
                        </label>
                        <label class="flex items-center gap-2 cursor-pointer bg-slate-50 px-3 py-2 rounded-lg border">
                            <input type="checkbox" name="category" value="S5" class="w-4 h-4"> 5S
                        </label>
                        <label class="flex items-center gap-2 cursor-pointer bg-slate-50 px-3 py-2 rounded-lg border">
                            <input type="checkbox" name="category" value="Other" class="w-4 h-4"> อื่นๆ
                        </label>
                    </div>
                </div>
                <div>
                    <label class="text-xs font-bold text-red-500 block mb-1">⚠️ รายละเอียดปัญหา</label>
                    <textarea id="detail" rows="3" required class="input-field" placeholder="ระบุสิ่งที่พบ..."></textarea>
                </div>
                <div>
                    <label class="text-xs font-bold text-red-500 block mb-1">📸 รูปภาพ Before (เลือกได้หลายรูป)</label>
                    <input type="file" id="imgBefore" accept="image/*" multiple class="input-field border-dashed" onchange="previewImages(this, 'beforePreview')">
                    <div id="beforePreview" class="img-grid mt-2"></div>
                </div>
                <div class="bg-emerald-50 p-5 rounded-2xl border border-emerald-100 space-y-4">
                    <label class="text-xs font-bold text-emerald-600 block mb-1">🛠️ แนวทางการแก้ไข & ผู้รับผิดชอบ</label>
                    <textarea id="solution" rows="2" class="input-field" placeholder="ระบุวิธีแก้ไข..."></textarea>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <input type="text" id="owner" required class="input-field" placeholder="ผู้รับผิดชอบ (PIC)">
                        <input type="date" id="dueDate" required class="input-field">
                    </div>
                </div>
                <button type="submit" id="submitBtn" class="w-full bg-blue-600 text-white py-4 rounded-xl font-bold shadow-lg">บันทึกข้อมูล</button>
            </form>
        </div>
    </div>

    <div id="modal-view" class="modal-overlay hidden">
        <div class="modal-box">
            <div class="p-4 border-b flex justify-between bg-slate-50">
                <button id="btnDeletePop" class="text-red-500 font-bold text-xs"><i data-lucide="trash-2"></i> ลบรายการ</button>
                <button onclick="closeModal('modal-view')"><i data-lucide="x"></i></button>
            </div>
            <div id="viewDetailContent" class="overflow-y-auto p-6 space-y-6"></div>
            <div class="p-4 border-t bg-white flex gap-2">
                <button id="btnUpdatePop" class="flex-1 bg-orange-500 text-white py-3 rounded-xl font-bold">อัปเดตงาน</button>
                <button id="btnEditPhotoPop" class="flex-1 bg-slate-100 text-slate-700 py-3 rounded-xl font-bold">แก้ไขรูปภาพ</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getDatabase, ref, push, onValue, update, remove } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

        const app = initializeApp({ databaseURL: "https://robust-helix-471007-m7-default-rtdb.asia-southeast1.firebasedatabase.app/" });
        const db = getDatabase(app);

        const months = ["มกราคม", "กุมภาพันธ์", "มีนาคม", "เมษายน", "พฤษภาคม", "มิถุนายน", "กรกฎาคม", "สิงหาคม", "กันยายน", "ตุลาคม", "พฤศจิกายน", "ธันวาคม"];
        let currentPath = "", currentId = "";

        const yearSelect = document.getElementById('yearSelect');
        for (let y = 2026; y <= 2030; y++) {
            const opt = document.createElement('option'); opt.value = y; opt.innerText = y; yearSelect.appendChild(opt);
        }

        // ตัวอย่างการแสดง Preview หลายรูป
        window.previewImages = (input, containerId) => {
            const container = document.getElementById(containerId);
            container.innerHTML = "";
            if (input.files) {
                Array.from(input.files).forEach(file => {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        const img = document.createElement('img');
                        img.src = e.target.result;
                        img.className = "preview-img";
                        container.appendChild(img);
                    };
                    reader.readAsDataURL(file);
                });
            }
        };

        function initHome() {
            const grid = document.getElementById('monthGrid'); grid.innerHTML = "";
            months.forEach((m, i) => {
                const mid = `${yearSelect.value}-${String(i + 1).padStart(2, '0')}`;
                const div = document.createElement('div');
                div.className = 'bg-white p-6 rounded-3xl border border-slate-100 cursor-pointer shadow-sm hover:shadow-md';
                div.innerHTML = `<h3 class="font-bold text-lg text-slate-700">${m}</h3><div id="stats-${mid}" class="mt-2 text-xs text-slate-400 italic">กำลังโหลด...</div>`;
                onValue(ref(db, `patrols/${mid}`), (snap) => {
                    const stats = document.getElementById(`stats-${mid}`);
                    const count = snap.val() ? Object.keys(snap.val()).length : 0;
                    stats.innerText = `พบ ${count} รายการ`;
                });
                div.onclick = () => { currentPath = mid; document.getElementById('page-home').classList.add('hidden'); document.getElementById('page-list').classList.remove('hidden'); document.getElementById('currentMonthTitle').innerText = `${m} ${yearSelect.value}`; listenData(); };
                grid.appendChild(div);
            });
            lucide.createIcons();
        }

        function listenData() {
            onValue(ref(db, `patrols/${currentPath}`), (snap) => {
                const container = document.getElementById('dataItems'); container.innerHTML = "";
                const data = snap.val();
                if (data) {
                    Object.entries(data).reverse().forEach(([id, item]) => {
                        // แก้ไขหน้าแสดงข้อมูลรวบรวม: เพิ่มชื่อผู้รายงาน (Reporter)
                        container.innerHTML += `
                            <div onclick='viewDetail("${id}", ${JSON.stringify(item)})' class="case-card p-5 border-l-[10px] ${item.category.includes('Safety') ? 'border-red-500' : 'border-blue-500'}">
                                <div class="flex justify-between text-[10px] font-bold text-slate-400 mb-2">
                                    <span>${item.category.join(' / ')} | ${item.dept}</span>
                                    <span class="${item.status === 'เสร็จสิ้น' ? 'text-emerald-500' : 'text-red-500'}">${item.status}</span>
                                </div>
                                <h3 class="font-bold text-slate-800 mb-3">${item.detail}</h3>
                                <div class="flex justify-between items-center text-xs text-slate-500 pt-3 border-t">
                                    <div class="flex gap-4">
                                        <span><b>รายงานโดย:</b> ${item.reporter}</span> <span><b>PIC:</b> ${item.owner}</span>
                                    </div>
                                    <i data-lucide="chevron-right"></i>
                                </div>
                            </div>`;
                    });
                }
                lucide.createIcons();
            });
        }

        const processMultipleImages = async (files) => {
            const promises = Array.from(files).map(file => {
                return new Promise((res) => {
                    const reader = new FileReader();
                    reader.onload = (e) => res(e.target.result); // เพื่อความง่ายในการเก็บตัวอย่าง (ปกติควรบีบอัดก่อน)
                    reader.readAsDataURL(file);
                });
            });
            return Promise.all(promises);
        };

        document.getElementById('mainForm').onsubmit = async (e) => {
            e.preventDefault();
            const btn = document.getElementById('submitBtn'); btn.disabled = true; btn.innerText = "กำลังบันทึก...";
            
            // เก็บหมวดหมู่แบบเลือกได้หลายอัน
            const categories = Array.from(document.querySelectorAll('input[name="category"]:checked')).map(cb => cb.value);
            const imagesBefore = await processMultipleImages(document.getElementById('imgBefore').files);

            const payload = {
                postDate: document.getElementById('postDate').value,
                reporter: document.getElementById('reporter').value,
                dept: document.getElementById('dept').value,
                category: categories, // เก็บเป็น Array
                owner: document.getElementById('owner').value,
                dueDate: document.getElementById('dueDate').value,
                detail: document.getElementById('detail').value,
                solution: document.getElementById('solution').value,
                imagesBefore: imagesBefore, // เก็บเป็น Array ของหลายรูป
                status: "รอดำเนินการ",
                ts: new Date().getTime()
            };

            push(ref(db, `patrols/${currentPath}`), payload).then(() => {
                closeModal('modal-form');
                e.target.reset();
                document.getElementById('beforePreview').innerHTML = "";
                btn.disabled = false; btn.innerText = "บันทึกข้อมูล";
            });
        };

        window.viewDetail = (id, item) => {
            currentId = id;
            const content = document.getElementById('viewDetailContent');
            const imgsHtml = (item.imagesBefore || []).map(src => `<img src="${src}" class="rounded-xl border w-full">`).join('');
            
            content.innerHTML = `
                <div class="grid grid-cols-2 gap-4 text-xs">
                    <div class="bg-blue-50 p-4 rounded-xl"><b>ผู้รายงาน:</b><br>${item.reporter}</div>
                    <div class="bg-emerald-50 p-4 rounded-xl"><b>ผู้รับผิดชอบ:</b><br>${item.owner}</div>
                </div>
                <div class="p-4 bg-slate-50 rounded-xl"><b>หมวดหมู่:</b> ${item.category.join(', ')} | <b>แผนก:</b> ${item.dept}</div>
                <div class="p-4 bg-red-50 rounded-xl text-red-700"><b>ปัญหา:</b><br>${item.detail}</div>
                <div class="p-4 bg-emerald-50 rounded-xl text-emerald-700"><b>การแก้ไข:</b><br>${item.solution || '-'}</div>
                <div class="space-y-2">
                    <p class="text-xs font-bold text-slate-400 uppercase">รูปภาพที่ตรวจพบ</p>
                    <div class="grid grid-cols-2 gap-2">${imgsHtml}</div>
                </div>
            `;
            document.getElementById('modal-view').classList.remove('hidden');
            lucide.createIcons();
            
            document.getElementById('btnDeletePop').onclick = () => { if(confirm('ลบรายการนี้ใช่หรือไม่?')) { remove(ref(db, `patrols/${currentPath}/${id}`)); closeModal('modal-view'); } };
            document.getElementById('btnUpdatePop').onclick = () => alert('ฟังก์ชันอัปเดตงานสำหรับรายการนี้');
        };

        window.goBackHome = () => { document.getElementById('page-list').classList.add('hidden'); document.getElementById('page-home').classList.remove('hidden'); };
        window.openForm = () => document.getElementById('modal-form').classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        yearSelect.onchange = initHome;
        initHome();
    </script>
</body>
</html>
