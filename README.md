<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منصة النبراس - النسخة الاحترافية</title>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
    <style>
        :root { --bg: #0b0b0b; --red: #8b0000; --white: #f5f5f5; --card-bg: #1a1a1a; --gold: #ffcc00; --green: #28a745; --gray: #444; }
        body { margin: 0; font-family: 'Segoe UI', sans-serif; background: var(--bg); color: var(--white); text-align: right; line-height: 1.6; }
        .screen { display: none; padding: 20px; min-height: 100vh; box-sizing: border-box; }
        .active { display: block; }
        .card { background: var(--card-bg); padding: 25px; border-radius: 15px; border-right: 6px solid var(--red); margin-bottom: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); }
        .btn { padding: 14px 20px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; width: 100%; margin: 10px 0; transition: 0.3s; font-size: 16px; }
        .btn-main { background: var(--red); color: white; }
        .btn-main:hover { background: #a50000; transform: translateY(-2px); }
        .btn-sub { background: #333; color: white; }
        
        /* أزرار لوحة التحكم */
        .btn-action { padding: 8px 12px; font-size: 13px; width: auto; border-radius: 5px; cursor: pointer; border: none; font-weight: bold; margin: 2px; }
        .btn-activate { background: var(--green); color: white; }
        .btn-reject { background: var(--red); color: white; }

        input { width: 100%; padding: 12px; margin: 10px 0; background: #252525; color: white; border: 1px solid #444; border-radius: 8px; box-sizing: border-box; outline: none; }
        .name-row { display: flex; gap: 10px; }
        .name-row input { flex: 1; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; background: #111; font-size: 14px; }
        th, td { border: 1px solid #333; padding: 12px; text-align: center; }
        th { background: var(--red); color: white; }
        .option-btn { display: block; width: 100%; text-align: right; padding: 12px; margin: 8px 0; background: #2a2a2a; border: 1px solid #444; color: white; cursor: pointer; border-radius: 5px; }
        .selected { background: var(--red) !important; border-color: white; font-weight: bold; }
        .developer-credit { font-size: 22px; font-weight: bold; color: var(--gold); margin-top: 25px; text-shadow: 1px 1px 10px rgba(0,0,0,0.8); border-top: 1px solid #444; padding-top: 20px; line-height: 1.8; }
        #anti-cheat-msg { color: #ff4444; font-weight: bold; text-align: center; margin-bottom: 10px; display: none; }
        #registration-notice { color: #ff4444; font-weight: bold; text-align: center; display: none; padding: 20px; border: 2px solid #8b0000; border-radius: 10px; background: #1a0000; font-size: 18px; }
    </style>
</head>
<body>

<div id="home-screen" class="screen active" style="display:flex; flex-direction:column; justify-content:center; align-items:center; background: radial-gradient(circle, #2e0000 0%, #000 100%);">
    <div style="background:rgba(0,0,0,0.85); padding:40px; border-radius:25px; border:1px solid #8b0000; width:360px; text-align:center;">
        <h1 style="color:var(--red); margin-bottom:30px;">منصة النبراس</h1>
        <button class="btn btn-sub" onclick="showScreen('student-register')">تسجيل طالب جديد</button>
        <button class="btn btn-sub" style="background:#222" onclick="showScreen('student-login')">دخول الأبطال المسجلين</button>
        <button class="btn btn-main" onclick="adminAuth()">لوحة التحكم</button>
        <div class="developer-credit">
            المستر أشرف فكري <br>
            المطور عمار ياسر : 01281872620
        </div>
    </div>
</div>

<div id="student-register" class="screen">
    <div class="card" style="max-width: 450px; margin: auto;">
        <div id="reg-form-content">
            <h3>تسجيل جديد</h3>
            <div class="name-row">
                <input type="text" id="regStudentName" placeholder="اسم الطالب">
                <input type="text" id="regFatherName" placeholder="اسم الأب">
            </div>
            <input type="tel" id="regStudentPhone" placeholder="رقم الطالب (11 رقم)">
            <input type="tel" id="regParentPhone" placeholder="رقم ولي الأمر (11 رقم)">
            <button class="btn btn-main" onclick="handleStudentRegistration()">إنشاء حساب</button>
            <button class="btn btn-sub" onclick="showScreen('home-screen')">رجوع</button>
        </div>
        <div id="registration-notice">
            متعملش حساب تاني <br>
            حسابك إتعمل بس لسة مش إتفعل <br>
            كلم المستر يفعلة
            <button class="btn btn-sub" style="margin-top:15px; background: #444;" onclick="location.reload()">فهمت</button>
        </div>
    </div>
</div>

<div id="student-login" class="screen">
    <div class="card" style="max-width: 400px; margin: auto;">
        <h3>تسجيل الدخول</h3>
        <input type="tel" id="loginPhone" placeholder="رقم الموبايل المسجل">
        <button class="btn btn-main" onclick="handleStudentLogin()">دخول</button>
        <button class="btn btn-sub" onclick="showScreen('home-screen')">رجوع</button>
    </div>
</div>

<div id="student-dash" class="screen">
    <div class="card">
        <h2 id="stName">مرحباً بك يا بطل</h2>
        <div id="anti-cheat-msg">⚠️ تحذير: الخروج من الصفحة سيؤدي لإنهاء الامتحان فوراً!</div>
        <button id="start-btn" class="btn btn-main" onclick="loadExam()">بدء الامتحان</button>
        <button class="btn btn-sub" id="logout-btn" onclick="location.reload()">تسجيل خروج</button>
    </div>
    <div id="exam-area" style="display:none">
        <div id="questions-box"></div>
        <button class="btn btn-main" style="background:#28a745" onclick="submitExam()">إرسال الإجابات</button>
    </div>
</div>

<div id="admin-dash" class="screen">
    <div style="display:flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
        <h2>لوحة تحكم المستر أشرف</h2>
        <button class="btn btn-sub" style="width:auto" onclick="location.reload()">خروج</button>
    </div>
    
    <div class="card">
        <h3>👥 طلبات الطلاب الجدد (تفعيل أو رفض)</h3>
        <div style="overflow-x:auto">
            <table>
                <thead><tr><th>الطالب</th><th>موبايل</th><th>ولي الأمر</th><th>قرار المستر</th></tr></thead>
                <tbody id="requestsTableBody"></tbody>
            </table>
        </div>
    </div>

    <div class="card">
        <h3>📝 إضافة سؤال</h3>
        <input type="text" id="qText" placeholder="نص السؤال">
        <input type="text" id="optCorrect" placeholder="الإجابة الصحيحة">
        <input type="text" id="opt1" placeholder="خيار خطأ 1">
        <input type="text" id="opt2" placeholder="خيار خطأ 2">
        <button class="btn btn-main" onclick="addQuestion()">حفظ</button>
    </div>

    <div class="card">
        <h3>📊 نتائج الطلاب</h3>
        <div style="overflow-x:auto">
            <table>
                <thead><tr><th>الطالب</th><th>الدرجة</th><th>ملحوظة</th><th>التوقيت</th></tr></thead>
                <tbody id="resultsTable"></tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const firebaseConfig = {
      apiKey: "AIzaSyDDUR6IdJ-eQW72CN0pB0B6sqxBxZkefKg",
      authDomain: "al-nibras-in-the-arabic.firebaseapp.com",
      projectId: "al-nibras-in-the-arabic",
      storageBucket: "al-nibras-in-the-arabic.firebasestorage.app",
      messagingSenderId: "45491370927",
      appId: "1:45491370927:web:55dfc130d71b2257e1cd5a"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    let currentStudent = null, questionsData = [], studentAnswers = {};
    window.isExamRunning = false; window.isSubmitted = false;

    function showScreen(id) {
        document.querySelectorAll('.screen').forEach(s => { s.classList.remove('active'); s.style.display = 'none'; });
        const t = document.getElementById(id); t.classList.add('active');
        t.style.display = (id === 'home-screen') ? 'flex' : 'block';
    }

    // --- منطق الطالب ---
    async function handleStudentRegistration() {
        const name = document.getElementById('regStudentName').value.trim(), father = document.getElementById('regFatherName').value.trim(),
              sPhone = document.getElementById('regStudentPhone').value.trim(), pPhone = document.getElementById('regParentPhone').value.trim();
        const regex = /^(010|011|012|015)[0-9]{8}$/;
        if(!name || !father || !sPhone || !pPhone) return alert("اكمل البيانات");
        if(!regex.test(sPhone) || !regex.test(pPhone)) return alert("رقم غير صحيح");

        await db.collection("student_requests").add({ name: name + " " + father, studentPhone: sPhone, parentPhone: pPhone, status: "pending", date: new Date().toLocaleString('ar-EG') });
        document.getElementById('reg-form-content').style.display = 'none';
        document.getElementById('registration-notice').style.display = 'block';
    }

    async function handleStudentLogin() {
        const phone = document.getElementById('loginPhone').value.trim();
        const res = await db.collection("student_requests").where("studentPhone", "==", phone).where("status", "==", "active").get();
        if(!res.empty) {
            currentStudent = { id: res.docs[0].id, ...res.docs[0].data() };
            document.getElementById('stName').innerText = "البطل: " + currentStudent.name;
            showScreen('student-dash');
        } else alert("الحساب غير مفعل أو الرقم خطأ!");
    }

    // --- منطق الامتحانات ---
    document.addEventListener("visibilitychange", () => { if (document.visibilityState === 'hidden' && window.isExamRunning && !window.isSubmitted) processResult("⚠️ غش: خرج من الصفحة"); });

    async function loadExam() {
        const res = await db.collection("questions").get();
        questionsData = res.docs.map(doc => ({id: doc.id, ...doc.data()}));
        if(!questionsData.length) return alert("لا توجد امتحانات");
        window.isExamRunning = true; document.getElementById('exam-area').style.display = 'block'; document.getElementById('start-btn').style.display = 'none'; document.getElementById('anti-cheat-msg').style.display = 'block';
        const box = document.getElementById('questions-box'); box.innerHTML = "";
        questionsData.forEach((q, i) => {
            let opts = ""; q.options.forEach(opt => opts += `<button class="option-btn q-${i}" onclick="selectOpt(${i}, '${opt.replace(/'/g, "\\'")}', this)">${opt}</button>`);
            box.innerHTML += `<div class="card"><p><b>${i+1}.</b> ${q.text}</p>${opts}</div>`;
        });
    }

    function selectOpt(qi, val, btn) { studentAnswers[qi] = val; document.querySelectorAll('.q-'+qi).forEach(b => b.classList.remove('selected')); btn.classList.add('selected'); }
    function submitExam() { if(!window.isSubmitted) processResult("تسليم يدوي"); }

    async function processResult(note) {
        window.isSubmitted = true; window.isExamRunning = false;
        let score = 0; questionsData.forEach((q, i) => { if(studentAnswers[i] === q.correct) score++; });
        const total = Math.round((score / questionsData.length) * 100);
        await db.collection("results").add({ name: currentStudent.name, grade: total + "%", note: note, date: new Date().toLocaleString('ar-EG'), timestamp: firebase.firestore.FieldValue.serverTimestamp() });
        alert("انتهى الامتحان. درجتك: " + total + "%"); location.reload();
    }

    // --- لوحة تحكم المستر ---
    function adminAuth() { if(prompt("كلمة سر المستر:") === "أشرف فكري") { showScreen('admin-dash'); loadStudentRequests(); loadResults(); } }

    async function loadStudentRequests() {
        const snap = await db.collection("student_requests").where("status", "==", "pending").get();
        const tbody = document.getElementById('requestsTableBody'); tbody.innerHTML = "";
        if(snap.empty) tbody.innerHTML = "<tr><td colspan='4'>لا يوجد طلبات جديدة</td></tr>";
        snap.forEach(doc => { 
            const d = doc.data(); 
            tbody.innerHTML += `
                <tr>
                    <td>${d.name}</td>
                    <td>${d.studentPhone}</td>
                    <td>${d.parentPhone}</td>
                    <td>
                        <button class="btn-action btn-activate" onclick="activateStudent('${doc.id}')">تفعيل ✅</button>
                        <button class="btn-action btn-reject" onclick="rejectStudent('${doc.id}')">رفض ❌</button>
                    </td>
                </tr>`; 
        });
    }

    // تفعيل الطالب
    async function activateStudent(id) { 
        await db.collection("student_requests").doc(id).update({status: "active"}); 
        alert("تم التفعيل بنجاح"); 
        loadStudentRequests(); 
    }

    // رفض (حذف) الطالب
    async function rejectStudent(id) { 
        if(confirm("هل أنت متأكد من رفض وحذف هذا الطالب؟")) {
            await db.collection("student_requests").doc(id).delete(); 
            alert("تم رفض الطلب وحذفه"); 
            loadStudentRequests(); 
        }
    }

    async function addQuestion() {
        const text = document.getElementById('qText').value, correct = document.getElementById('optCorrect').value, o1 = document.getElementById('opt1').value, o2 = document.getElementById('opt2').value;
        const options = [correct, o1, o2].filter(o => o).sort(() => Math.random() - 0.5);
        await db.collection("questions").add({ text, options, correct }); alert("تم الحفظ");
        document.getElementById('qText').value = ""; document.getElementById('optCorrect').value = "";
    }

    async function loadResults() {
        const res = await db.collection("results").orderBy("timestamp", "desc").get();
        const tbody = document.getElementById('resultsTable'); tbody.innerHTML = "";
        res.forEach(doc => { const d = doc.data(); tbody.innerHTML += `<tr><td>${d.name}</td><td>${d.grade}</td><td>${d.note}</td><td>${d.date}</td></tr>`; });
    }
</script>
</body>
</html>
