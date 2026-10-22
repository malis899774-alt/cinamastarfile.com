from pathlib import Path

html = r'''<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>سینما استار | پنل مدیریت</title>
<style>
:root{--bg:#101116;--panel:#191c25;--panel2:#222633;--text:#f5f6fa;--muted:#b6bbca;--gold:#f3bd42;--line:#34394a;--good:#52d6a0}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--text);font-family:Tahoma,Arial,sans-serif;line-height:1.8}button,input,select,textarea{font:inherit}button{cursor:pointer}.wrap{width:min(1150px,94%);margin:auto}
header{background:#151720;border-bottom:1px solid var(--line);position:sticky;top:0;z-index:5}.top{display:flex;justify-content:space-between;align-items:center;gap:12px;padding:14px 0;flex-wrap:wrap}.logo{font-size:1.3rem;font-weight:bold;color:var(--gold)}nav{display:flex;gap:8px;flex-wrap:wrap}nav button,.btn{border:1px solid var(--line);background:var(--panel2);color:var(--text);border-radius:9px;padding:9px 13px}nav button:hover,.btn:hover{border-color:var(--gold)}.primary{background:var(--gold);color:#191919;border-color:var(--gold);font-weight:bold}.danger{background:#4b2229;border-color:#7c3540}.muted{color:var(--muted)}.hero{padding:38px 0 25px}.hero h1{font-size:clamp(1.7rem,4vw,2.8rem);margin:0 0 8px}.hero p{color:var(--muted);margin:0}.panel{background:var(--panel);border:1px solid var(--line);border-radius:16px;padding:20px;margin:20px 0}.panel h2{margin:0 0 14px;font-size:1.35rem}.grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px}.field{display:flex;flex-direction:column;gap:5px;min-width:0}.field.full{grid-column:1/-1}label{font-size:.92rem;color:#e6e8f0}.field input,.field select,.field textarea{width:100%;min-width:0;background:#11131a;color:var(--text);border:1px solid var(--line);border-radius:9px;padding:11px;outline:none}.field input:focus,.field select:focus,.field textarea:focus{border-color:var(--gold)}input[type=file]{padding:8px}.field textarea{min-height:90px;resize:vertical}.hint{font-size:.8rem;color:var(--muted)}.actions{display:flex;gap:9px;flex-wrap:wrap;margin-top:16px}.hidden{display:none!important}.cards{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:13px}.item{border:1px solid var(--line);border-radius:13px;overflow:hidden;background:#141720;min-width:0}.item img,.item video{width:100%;max-height:220px;object-fit:cover;display:block;background:#090a0e}.item-body{padding:13px}.item h3{margin:0 0 6px}.item p{margin:4px 0;color:var(--muted);font-size:.9rem;overflow-wrap:anywhere}.pill{display:inline-block;background:#33303a;border-radius:20px;padding:2px 9px;font-size:.78rem;margin:3px 3px 3px 0}.empty{color:var(--muted);padding:10px 0}.status{color:var(--good)}.login-box{max-width:450px;margin:35px auto}.notice{padding:10px 12px;border-radius:9px;background:#262a35;color:var(--muted);margin-top:12px;font-size:.88rem}footer{padding:30px 0;text-align:center;color:var(--muted);border-top:1px solid var(--line);margin-top:35px}
@media(max-width:760px){.grid{grid-template-columns:1fr}.field.full{grid-column:auto}.cards{grid-template-columns:repeat(2,minmax(0,1fr))}.top{justify-content:center}.logo{text-align:center;width:100%}nav{justify-content:center}}@media(max-width:480px){.cards{grid-template-columns:1fr}.panel{padding:14px}.btn,nav button{padding:9px 10px;font-size:.9rem}.hero{padding-top:26px}}
</style>
</head>
<body>
<header><div class="wrap top"><div class="logo">▶ سینما استار</div><nav id="nav" class="hidden"><button data-view="home">خانه</button><button data-view="admin">پنل مدیریت</button><button data-view="schedule">برنامه پخش</button><button id="logout">خروج</button></nav></div></header>
<main class="wrap">
<section id="loginView" class="panel login-box">
<h1>ورود به سینما استار</h1><p class="muted">برای ورود کاربر عادی هر رمزی وارد کن. برای پنل مدیریت رمز مدیر لازم است.</p>
<div class="field"><label for="loginRole">نوع ورود</label><select id="loginRole"><option value="user">کاربر عادی</option><option value="admin">مدیر سایت</option></select></div>
<div class="field" style="margin-top:12px"><label for="loginName">نام نمایشی</label><input id="loginName" placeholder="مثلاً محمد علی"></div>
<div class="field" style="margin-top:12px"><label for="loginPassword">رمز عبور</label><input id="loginPassword" type="password" placeholder="رمز را وارد کن"></div>
<div class="actions"><button id="loginBtn" class="btn primary">ورود به سایت</button></div>
<div class="notice">رمز مدیر آزمایشی این نسخه: <strong>StarAdmin2026!</strong><br>این رمز داخل فایل قابل مشاهده است؛ این نسخه برای آزمایش محلی است و برای انتشار اینترنتی به ورود امن سمت سرور نیاز دارد.</div>
</section>
<section id="homeView" class="hidden">
<div class="hero"><h1>به سینما استار خوش آمدید</h1><p>دنیای فیلم و سریال‌هایی که توسط محمد علی صادقی و گروهش ساخته می‌شوند.</p></div>
<div class="panel"><h2>فیلم‌ها و سریال‌ها</h2><div id="publicItems" class="cards"></div><div id="publicEmpty" class="empty">هنوز اثری منتشر نشده است.</div></div>
<div class="panel"><h2>پیام‌های مدیر</h2><div id="publicMessages" class="cards"></div><div id="messagesEmpty" class="empty">پیامی ثبت نشده است.</div></div>
</section>
<section id="adminView" class="hidden">
<div class="hero"><h1>پنل مدیریت</h1><p>محتوا، دسته‌بندی‌ها، قسمت‌ها، برنامه پخش و پیام‌های سایت را مدیریت کن.</p></div>
<section class="panel"><h2>۱. افزودن فیلم یا سریال</h2>
<form id="contentForm">
<div class="grid">
<div class="field"><label>نوع محتوا</label><select name="kind"><option>فیلم</option><option>سریال</option></select></div>
<div class="field"><label>نام فیلم یا سریال *</label><input name="title" required placeholder="نام اثر"></div>
<div class="field"><label>دسته‌بندی (حداقل ۳ گزینه موجود است)</label><select name="category"><option>اکشن</option><option>کمدی</option><option>معمایی</option><option>ترسناک</option><option>خانوادگی</option><option>ماجراجویی</option><option>فانتزی</option><option>آموزشی</option><option>تاریخی</option><option>درام</option></select></div>
<div class="field"><label>بازیگران</label><input name="actors" placeholder="نام بازیگران، با ویرگول جدا کن"></div>
<div class="field"><label>کارگردان</label><input name="director" placeholder="نام کارگردان"></div>
<div class="field"><label>کیفیت</label><select name="quality"><option>480p</option><option selected>720p HD</option><option>1080p Full HD</option><option>4K</option><option>کیفیت نامشخص</option></select></div>
<div class="field"><label>رده سنی</label><select name="age"><option>همه سنین</option><option>۷+</option><option>۱۲+</option><option>۱۵+</option><option>۱۸+</option></select></div>
<div class="field"><label>وضعیت انتشار</label><select name="status"><option>منتشر شده</option><option>به زودی</option><option>در حال پخش</option></select></div>
<div class="field"><label>لینک تصویر جلد (اختیاری)</label><input name="imageUrl" type="url" placeholder="https://..."></div>
<div class="field"><label>افزودن عکس از گالری</label><input name="imageFile" type="file" accept="image/*"></div>
<div class="field"><label>لینک تماشا</label><input name="videoUrl" type="url" placeholder="https://..."></div>
<div class="field"><label>افزودن ویدیو پیش‌نمایش از گالری</label><input name="previewFile" type="file" accept="video/*"></div>
<div class="field full"><label>توضیحات</label><textarea name="description" placeholder="خلاصه داستان یا توضیحات اثر"></textarea></div>
</div><div class="actions"><button class="btn primary" type="submit">افزودن به سایت</button><button class="btn" type="reset">پاک کردن فرم</button></div>
<p class="hint">تصاویر و ویدیوهای انتخابی فقط در همین مرورگر ذخیره می‌شوند و اندازه هر فایل باید حداکثر ۲ مگابایت باشد. برای ذخیره‌سازی واقعی و اشتراک بین دستگاه‌ها به سرور نیاز است.</p>
</form></section>
<section class="panel"><h2>۲. مدیریت دسته‌بندی‌ها</h2><form id="categoryForm"><div class="grid"><div class="field"><label>نام دسته‌بندی *</label><input name="name" required placeholder="مثلاً علمی‌تخیلی"></div><div class="field"><label>افزودن عکس از گالری</label><input name="image" type="file" accept="image/*"></div></div><div class="actions"><button class="btn primary">افزودن دسته‌بندی</button></div></form><div id="categoryList" class="cards" style="margin-top:14px"></div></section>
<section class="panel"><h2>۳. افزودن قسمت سریال</h2><form id="episodeForm"><div class="grid">
<div class="field"><label>انتخاب سریال *</label><select name="series" id="seriesSelect" required><option value="">ابتدا سریال اضافه کن</option></select></div>
<div class="field"><label>نام قسمت *</label><input name="title" required placeholder="مثلاً صدای نیمه‌شب"></div>
<div class="field"><label>دسته‌بندی (حداقل ۳ گزینه)</label><select name="category"><option>معمایی</option><option>اکشن</option><option>کمدی</option><option>ترسناک</option><option>خانوادگی</option><option>ماجراجویی</option><option>فانتزی</option><option>درام</option></select></div>
<div class="field"><label>وضعیت انتشار</label><select name="status"><option>منتشر شده</option><option>به زودی</option><option>در حال پخش</option></select></div>
<div class="field"><label>فصل</label><input name="season" type="number" min="1" value="1"></div>
<div class="field"><label>شماره قسمت</label><input name="episode" type="number" min="1" value="1"></div>
<div class="field"><label>اجازه دانلود</label><select name="download"><option value="no">خیر</option><option value="yes">بله</option></select></div>
<div class="field"><label>روز پخش</label><select name="day"><option>شنبه</option><option>یکشنبه</option><option>دوشنبه</option><option>سه‌شنبه</option><option>چهارشنبه</option><option>پنجشنبه</option><option>جمعه</option></select></div>
<div class="field"><label>تاریخ پخش</label><input name="date" type="date"></div>
<div class="field"><label>ساعت پخش</label><input name="time" type="time"></div>
<div class="field"><label>مدت زمان (دقیقه)</label><input name="duration" type="number" min="1" placeholder="مثلاً ۱۵"></div>
<div class="field"><label>افزودن تصویر از گالری</label><input name="image" type="file" accept="image/*"></div>
<div class="field"><label>لینک ویدیو قسمت</label><input name="videoUrl" type="url" placeholder="https://..."></div>
<div class="field"><label>افزودن فایل ویدیو از گالری</label><input name="videoFile" type="file" accept="video/*"></div>
<div class="field full"><label>توضیحات قسمت</label><textarea name="description" placeholder="خلاصه این قسمت"></textarea></div>
</div><div class="actions"><button class="btn primary">افزودن قسمت</button></div></form></section>
<section class="panel"><h2>۴. برنامه پخش</h2><form id="scheduleForm"><div class="grid">
<div class="field"><label>عنوان برنامه *</label><input name="title" required placeholder="نام فیلم، سریال یا قسمت"></div>
<div class="field"><label>نوع برنامه</label><select name="kind"><option>فیلم</option><option>سریال</option><option>قسمت جدید</option><option>برنامه ویژه</option></select></div>
<div class="field"><label>تاریخ پخش</label><input name="date" type="date"></div>
<div class="field"><label>ساعت پخش</label><input name="time" type="time"></div>
<div class="field"><label>روز پخش</label><select name="day"><option>شنبه</option><option>یکشنبه</option><option>دوشنبه</option><option>سه‌شنبه</option><option>چهارشنبه</option><option>پنجشنبه</option><option>جمعه</option></select></div>
<div class="field full"><label>توضیحات</label><textarea name="description" placeholder="توضیحات برنامه پخش"></textarea></div>
</div><div class="actions"><button class="btn primary">ثبت برنامه پخش</button></div></form><div id="scheduleList" class="cards" style="margin-top:14px"></div></section>
<section class="panel"><h2>۵. پیام مدیر</h2><form id="messageForm"><div class="grid"><div class="field full"><label>متن پیام *</label><textarea name="message" required placeholder="پیام یا اطلاعیه برای کاربران"></textarea></div></div><div class="actions"><button class="btn primary">انتشار پیام</button></div></form><div id="adminMessages" class="cards" style="margin-top:14px"></div></section>
<section class="panel"><h2>محتوای ثبت‌شده</h2><div id="adminItems" class="cards"></div></section>
</section>
<section id="scheduleView" class="hidden"><div class="hero"><h1>برنامه پخش سینما استار</h1><p>زمان‌های ثبت‌شده برای فیلم‌ها و قسمت‌های جدید.</p></div><div id="publicSchedule" class="cards"></div></section>
<div id="globalNotice" class="notice hidden" role="status"></div>
</main>
<footer><div class="wrap">© <span id="year"></span> سینما استار — دنیای فیلم و سریال</div></footer>
<script>
const ADMIN_PASSWORD='StarAdmin2026!';
const DB_KEY='cinemaStarAdminV2';
let db={items:[],categories:[],episodes:[],schedule:[],messages:[]};
let role='user', userName='کاربر';
const $=s=>document.querySelector(s);
const escapeHtml=s=>String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
function loadDB(){try{const saved=JSON.parse(localStorage.getItem(DB_KEY)||'null');if(saved)db={...db,...saved}}catch(e){}}
function saveDB(){try{localStorage.setItem(DB_KEY,JSON.stringify(db));return true}catch(e){alert('فضای ذخیره‌سازی مرورگر پر شده است. فایل‌های بزرگ‌تر را حذف کن یا از لینک فایل استفاده کن.');return false}}
function fileData(file){return new Promise((resolve,reject)=>{if(!file){resolve('');return}if(file.size>2*1024*1024){reject(new Error('حجم هر فایل باید حداکثر ۲ مگابایت باشد.'));return}const r=new FileReader();r.onload=()=>resolve(r.result);r.onerror=()=>reject(new Error('خواندن فایل ناموفق بود.'));r.readAsDataURL(file)})}
function formObj(form){return Object.fromEntries(new FormData(form).entries())}
function showNotice(msg){const n=$('#globalNotice');n.textContent=msg;n.classList.remove('hidden');setTimeout(()=>n.classList.add('hidden'),4500)}
function switchView(view){['home','admin','schedule'].forEach(v=>$('#'+v+'View').classList.toggle('hidden',v!==view));}
function render(){
  const pub=db.items.filter(x=>x.status==='منتشر شده'||x.status==='در حال پخش');
  $('#publicItems').innerHTML=pub.map(x=>itemHTML(x,false)).join('');$('#publicEmpty').classList.toggle('hidden',pub.length>0);
  $('#adminItems').innerHTML=db.items.map(x=>itemHTML(x,true)).join('')||'<p class="empty">هنوز محتوایی ثبت نشده است.</p>';
  $('#categoryList').innerHTML=db.categories.map((c,i)=>`<article class="item">${c.image?`<img src="${c.image}" alt="">`:''}<div class="item-body"><h3>${escapeHtml(c.name)}</h3><button class="btn danger" onclick="deleteCategory(${i})">حذف</button></div></article>`).join('');
  const series=db.items.filter(x=>x.kind==='سریال');$('#seriesSelect').innerHTML='<option value="">انتخاب سریال</option>'+series.map(s=>`<option value="${escapeHtml(s.id)}">${escapeHtml(s.title)}</option>`).join('');
  $('#scheduleList').innerHTML=db.schedule.map((s,i)=>`<article class="item"><div class="item-body"><h3>${escapeHtml(s.title)}</h3><p>${escapeHtml(s.kind)} · ${escapeHtml(s.day||'')} · ${escapeHtml(s.date||'تاریخ تعیین نشده')} ${escapeHtml(s.time||'')}</p><p>${escapeHtml(s.description)}</p><button class="btn danger" onclick="deleteSchedule(${i})">حذف</button></div></article>`).join('');
  $('#publicSchedule').innerHTML=db.schedule.map(s=>`<article class="item"><div class="item-body"><h3>${escapeHtml(s.title)}</h3><p>${escapeHtml(s.kind)} · ${escapeHtml(s.day||'')} · ${escapeHtml(s.date||'تاریخ تعیین نشده')} ${escapeHtml(s.time||'')}</p><p>${escapeHtml(s.description)}</p></div></article>`).join('')||'<p class="empty">هنوز برنامه‌ای ثبت نشده است.</p>';
  $('#adminMessages').innerHTML=db.messages.map((m,i)=>`<article class="item"><div class="item-body"><p>${escapeHtml(m.message)}</p><button class="btn danger" onclick="deleteMessage(${i})">حذف</button></div></article>`).join('');
  $('#publicMessages').innerHTML=db.messages.map(m=>`<article class="item"><div class="item-body"><p>${escapeHtml(m.message)}</p></div></article>`).join('');$('#messagesEmpty').classList.toggle('hidden',db.messages.length>0);
}
function itemHTML(x,admin){
  const media=x.image?`<img src="${x.image}" alt="تصویر ${escapeHtml(x.title)}">`:'';
  const video=x.preview?`<video controls preload="metadata" src="${x.preview}"></video>`:'';
  return `<article class="item">${media}${video}<div class="item-body"><span class="pill">${escapeHtml(x.kind)}</span><span class="pill">${escapeHtml(x.category)}</span><span class="pill">${escapeHtml(x.status)}</span><h3>${escapeHtml(x.title)}</h3><p>${escapeHtml(x.description)}</p><p>بازیگران: ${escapeHtml(x.actors||'—')}</p><p>کارگردان: ${escapeHtml(x.director||'—')} · کیفیت: ${escapeHtml(x.quality||'—')} · رده سنی: ${escapeHtml(x.age||'—')}</p>${x.videoUrl?`<p><a class="btn" href="${escapeHtml(x.videoUrl)}" target="_blank" rel="noopener">تماشا</a></p>`:''}${admin?`<button class="btn danger" onclick="deleteItem('${x.id}')">حذف اثر</button>`:''}</div></article>`;
}
window.deleteItem=id=>{db.items=db.items.filter(x=>x.id!==id);saveDB();render();showNotice('اثر حذف شد.')};
window.deleteCategory=i=>{db.categories.splice(i,1);saveDB();render()};
window.deleteSchedule=i=>{db.schedule.splice(i,1);saveDB();render()};
window.deleteMessage=i=>{db.messages.splice(i,1);saveDB();render()};
$('#loginBtn').addEventListener('click',()=>{
 const wanted=$('#loginRole').value, pass=$('#loginPassword').value.trim();
 if(!pass){showNotice('لطفاً رمز عبور وارد کن.');return}
 if(wanted==='admin'&&pass!==ADMIN_PASSWORD){showNotice('رمز مدیر اشتباه است.');return}
 role=wanted;userName=$('#loginName').value.trim()||'کاربر';
 $('#loginView').classList.add('hidden');$('#nav').classList.remove('hidden');switchView('home');
 document.querySelector('[data-view="admin"]').classList.toggle('hidden',role!=='admin');
 render();showNotice(role==='admin'?'وارد پنل مدیریت شدی.':'خوش آمدی '+userName);
});
$('#logout').addEventListener('click',()=>{role='user';$('#nav').classList.add('hidden');switchView('home');$('#loginView').classList.remove('hidden');$('#loginPassword').value=''});
$('#nav').addEventListener('click',e=>{const b=e.target.closest('[data-view]');if(!b)return;const v=b.dataset.view;if(v==='admin'&&role!=='admin')return;switchView(v);render()});
$('#contentForm').addEventListener('submit',async e=>{
 e.preventDefault();const f=e.currentTarget,o=formObj(f);
 try{
 const image=await fileData(f.elements.imageFile.files[0]);const preview=await fileData(f.elements.previewFile.files[0]);
 if(!image&&o.imageUrl&&!/^https?:\/\//i.test(o.imageUrl)){showNotice('لینک تصویر باید با http:// یا https:// شروع شود.');return}
 db.items.unshift({id:Date.now().toString(36)+Math.random().toString(36).slice(2,6),kind:o.kind,title:o.title,category:o.category,actors:o.actors,director:o.director,quality:o.quality,age:o.age,status:o.status,image:image||o.imageUrl||'',preview,videoUrl:o.videoUrl,description:o.description});
 saveDB();f.reset();render();showNotice('محتوا با موفقیت اضافه شد.');
 }catch(err){showNotice(err.message||'خطا در افزودن محتوا')}
});
$('#categoryForm').addEventListener('submit',async e=>{e.preventDefault();const f=e.currentTarget,o=formObj(f);try{const image=await fileData(f.elements.image.files[0]);db.categories.push({name:o.name,image});saveDB();f.reset();render();showNotice('دسته‌بندی اضافه شد.')}catch(err){showNotice(err.message)}});
$('#episodeForm').addEventListener('submit',async e=>{e.preventDefault();const f=e.currentTarget,o=formObj(f);if(!o.series){showNotice('ابتدا یک سریال انتخاب کن.');return}try{const image=await fileData(f.elements.image.files[0]);const video=await fileData(f.elements.videoFile.files[0]);db.episodes.push({...o,id:Date.now().toString(36),image,video});saveDB();f.reset();render();showNotice('قسمت سریال ثبت شد.')}catch(err){showNotice(err.message)}});
$('#scheduleForm').addEventListener('submit',e=>{e.preventDefault();const o=formObj(e.currentTarget);db.schedule.push(o);saveDB();e.currentTarget.reset();render();showNotice('برنامه پخش ثبت شد.')});
$('#messageForm').addEventListener('submit',e=>{e.preventDefault();db.messages.unshift(formObj(e.currentTarget));saveDB();e.currentTarget.reset();render();showNotice('پیام منتشر شد.')});
$('#year').textContent=new Date().getFullYear();
loadDB();render();
</script>
</body>
</html>'''


