
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>سینما استار</title>
  <style>
    body {
      margin: 0;
      padding: 20px;
      background: #0b1020;
      color: white;
      font-family: Tahoma, sans-serif;
    }
    header, section {
      max-width: 900px;
      margin: 20px auto;
      padding: 20px;
      background: #151d32;
      border-radius: 15px;
    }
    button, input, select, textarea {
      box-sizing: border-box;
      width: 100%;
      margin: 7px 0;
      padding: 12px;
      border-radius: 8px;
      font: inherit;
    }
    button {
      background: #ffc857;
      border: 0;
      cursor: pointer;
    }
    input, select, textarea {
      background: #0b1020;
      color: white;
      border: 1px solid #405071;
    }
    .film {
      padding: 12px;
      margin-top: 12px;
      border: 1px solid #405071;
      border-radius: 10px;
    }
    img {
      max-width: 100%;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <header>
    <h1>🎬 سینما استار</h1>
    <p>دنیای فیلم و سریال‌های ساخته‌شده توسط محمد علی</p>
    <button onclick="showFilms()">نمایش فیلم‌ها</button>
    <button onclick="showAdmin()">پنل مدیریت</button>
  </header>

  <section id="films">
    <h2>فیلم‌ها و سریال‌ها</h2>
    <div id="filmList">هنوز اثری اضافه نشده است.</div>
  </section>

  <section id="admin" hidden>
    <h2>افزودن فیلم یا سریال</h2>
    <form id="filmForm">
      <input name="title" placeholder="نام فیلم یا سریال" required>
      <select name="kind">
        <option>فیلم</option>
        <option>سریال</option>
      </select>
      <select name="category">
        <option>اکشن</option>
        <option>کمدی</option>
        <option>معمایی</option>
        <option>خانوادگی</option>
      </select>
      <input name="date" type="date" aria-label="تاریخ پخش">
      <input name="time" type="time" aria-label="ساعت پخش">
      <input name="image" type="file" accept="image/*">
      <input name="url" type="url" placeholder="لینک تماشای اثر">
      <textarea name="description" placeholder="توضیحات اثر"></textarea>
      <button type="submit">ذخیره اثر</button>
    </form>
  </section>

  <script>
    const key = "cinemaStarFilms";

    function getFilms() {
      try {
        return JSON.parse(localStorage.getItem(key)) || [];
      } catch {
        return [];
      }
    }

    function showFilms() {
      document.getElementById("admin").hidden = true;
      document.getElementById("films").hidden = false;
    }

    function showAdmin() {
      document.getElementById("admin").hidden = false;
      document.getElementById("films").hidden = true;
    }

    function renderFilms() {
      const films = getFilms();
      const list = document.getElementById("filmList");

      list.innerHTML = "";

      if (!films.length) {
        list.textContent = "هنوز اثری اضافه نشده است.";
        return;
      }

      films.forEach(film => {
        const card = document.createElement("article");
        card.className = "film";

        const title = document.createElement("h3");
        title.textContent = film.title;
        card.appendChild(title);

        const info = document.createElement("p");
        info.textContent =
          film.kind + " | " + film.category +
          " | " + (film.date || "تاریخ اعلام نشده") +
          " " + (film.time || "");
        card.appendChild(info);

        const description = document.createElement("p");
        description.textContent = film.description;
        card.appendChild(description);

        if (film.image) {
          const img = document.createElement("img");
          img.src = film.image;
          img.alt = film.title;
          card.appendChild(img);
        }

        if (film.url) {
          const link = document.createElement("a");
          link.href = film.url;
          link.target = "_blank";
          link.rel = "noopener noreferrer";
          link.textContent = "تماشای اثر";
          card.appendChild(link);
        }

        list.appendChild(card);
      });
    }

    document.getElementById("filmForm")
      .addEventListener("submit", async event => {
        event.preventDefault();

        const form = event.currentTarget;
        const data = new FormData(form);
        const file = form.elements.image.files[0];

        let image = "";

        if (file) {
          image = await new Promise((resolve, reject) => {
            const reader = new FileReader();
            reader.onload = () => resolve(reader.result);
            reader.onerror = reject;
            reader.readAsDataURL(file);
          });
        }

        const films = getFilms();

        films.push({
          title: data.get("title"),
          kind: data.get("kind"),
          category: data.get("category"),
          date: data.get("date"),
          time: data.get("time"),
          image,
          url: data.get("url"),
          description: data.get("description")
        });

        try {
          localStorage.setItem(key, JSON.stringify(films));
          form.reset();
          renderFilms();
          showFilms();
          alert("اثر ذخیره شد!");
        } catch {
          alert("فضای ذخیره‌سازی مرورگر کافی نیست.");
        }
      });

    renderFilms();
  </script>
</body>
</html>
