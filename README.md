# IMAGE-GALLERY

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Image Gallery</title>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: Arial, sans-serif;
      background: #f7f3ff;
      color: #30264a;
    }

    header {
      text-align: center;
      padding: 42px 20px 28px;
      background: linear-gradient(135deg, #e8ddff, #f8e9f5);
    }

    header h1 {
      font-size: clamp(28px, 5vw, 42px);
      margin-bottom: 12px;
    }

    header p {
      color: #655777;
      font-size: 16px;
      line-height: 1.6;
    }

    .toolbar {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 10px;
      padding: 24px 16px 10px;
    }

    .filter-btn {
      padding: 10px 17px;
      border: 1px solid #d9c9f5;
      border-radius: 25px;
      background: white;
      color: #594477;
      cursor: pointer;
      font-size: 14px;
      transition: 0.2s;
    }

    .filter-btn:hover,
    .filter-btn.active {
      background: #7952b3;
      color: white;
      border-color: #7952b3;
    }

    .search-area {
      text-align: center;
      padding: 18px 16px 24px;
    }

    #searchInput {
      width: min(440px, 100%);
      padding: 13px 18px;
      border: 1px solid #d8c9ef;
      border-radius: 25px;
      font-size: 15px;
      outline: none;
      background: white;
    }

    #searchInput:focus {
      border-color: #7952b3;
      box-shadow: 0 0 0 3px #7952b320;
    }

    #status {
      text-align: center;
      padding: 0 15px 15px;
      color: #76658b;
      font-size: 14px;
    }

    .gallery {
      max-width: 1250px;
      margin: auto;
      padding: 10px 24px 45px;
      display: grid;
      grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 22px;
    }

    .card {
      background: white;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 5px 20px #39215b10;
      transition: transform 0.25s, box-shadow 0.25s;
    }

    .card:hover {
      transform: translateY(-5px);
      box-shadow: 0 12px 25px #39215b20;
    }

    .image-wrap {
      height: 190px;
      background: #e9e0f4;
      position: relative;
      overflow: hidden;
    }

    .card img {
      width: 100%;
      height: 100%;
      display: block;
      object-fit: cover;
      opacity: 0;
      transition: opacity 0.3s;
      cursor: pointer;
    }

    .card img.loaded {
      opacity: 1;
    }

    .placeholder {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 12px;
      text-align: center;
      color: #79668f;
      font-size: 13px;
    }

    .placeholder .symbol {
      font-size: 30px;
    }

    .placeholder.hidden {
      display: none;
    }

    .card-info {
      padding: 15px;
    }

    .card-info h3 {
      font-size: 16px;
      margin-bottom: 8px;
      line-height: 1.4;
    }

    .category {
      display: inline-block;
      font-size: 12px;
      background: #f0e8ff;
      color: #704ba4;
      padding: 5px 10px;
      border-radius: 20px;
    }

    .view-link {
      display: inline-block;
      margin-top: 12px;
      color: #7952b3;
      font-size: 13px;
      text-decoration: none;
    }

    .view-link:hover {
      text-decoration: underline;
    }

    .empty {
      grid-column: 1 / -1;
      text-align: center;
      padding: 50px 15px;
      color: #76658b;
    }

    footer {
      text-align: center;
      padding: 25px 15px;
      background: #ece3fa;
      color: #655777;
      font-size: 13px;
    }

    @media (max-width: 1000px) {
      .gallery {
        grid-template-columns: repeat(3, minmax(0, 1fr));
      }
    }

    @media (max-width: 700px) {
      .gallery {
        grid-template-columns: repeat(2, minmax(0, 1fr));
        padding: 10px 14px 30px;
        gap: 14px;
      }

      .image-wrap {
        height: 160px;
      }
    }

    @media (max-width: 420px) {
      .gallery {
        grid-template-columns: 1fr;
      }

      .image-wrap {
        height: 220px;
      }
    }
  </style>
</head>

<body>
  <header>
    <h1>India Through My Lens 🇮🇳</h1>
    <p>Explore the beauty, culture, traditions and wonders of India.</p>
  </header>

  <nav class="toolbar" id="filters" aria-label="Gallery categories"></nav>

  <div class="search-area">
    <input
      id="searchInput"
      type="search"
      placeholder="Search photos..."
      aria-label="Search photos"
    >
  </div>

  <p id="status" aria-live="polite">Preparing your gallery...</p>

  <main class="gallery" id="gallery"></main>

  <footer>
    Made with ❤️ to celebrate the incredible diversity of India.
    Images are sourced from Wikimedia Commons.
  </footer>

  <script>
    const collections = [
      {
        category: "Landmarks",
        items: [
          ["Taj Mahal", "Taj Mahal Agra India"],
          ["Red Fort", "Red Fort Delhi India"],
          ["India Gate", "India Gate New Delhi India"],
          ["Qutub Minar", "Qutub Minar Delhi India"]
        ]
      },
      {
        category: "Nature",
        items: [
          ["Kerala Backwaters", "Kerala backwaters India"],
          ["Himalayas", "Himalayas mountain India"],
          ["Valley of Flowers", "Valley of Flowers Uttarakhand India"],
          ["Western Ghats", "Western Ghats landscape India"]
        ]
      },
      {
        category: "Festivals",
        items: [
          ["Diwali Celebration", "Diwali festival India lights"],
          ["Holi Festival", "Holi festival India colours"],
          ["Durga Puja", "Durga Puja Kolkata India"],
          ["Ganesh Chaturthi", "Ganesh Chaturthi India festival"]
        ]
      },
      {
        category: "Food",
        items: [
          ["Hyderabadi Biryani", "Hyderabadi biryani India"],
          ["Masala Dosa", "Masala dosa South India"],
          ["Indian Thali", "Indian thali traditional food"],
          ["Gulab Jamun", "Gulab jamun Indian dessert"]
        ]
      },
      {
        category: "Architecture",
        items: [
          ["Hawa Mahal", "Hawa Mahal Jaipur India"],
          ["Mysore Palace", "Mysore Palace Karnataka India"],
          ["Golden Temple", "Golden Temple Amritsar India"],
          ["Meenakshi Temple", "Meenakshi Amman Temple Madurai India"]
        ]
      },
      {
        category: "Wildlife",
        items: [
          ["Bengal Tiger", "Bengal tiger India wildlife"],
          ["Indian Elephant", "Asian elephant India wildlife"],
          ["Peacock", "Indian peacock national bird"],
          ["Asiatic Lion", "Asiatic lion Gir India"]
        ]
      },
      {
        category: "Beaches",
        items: [
          ["Goa Beach", "Goa beach India"],
          ["Marina Beach", "Marina Beach Chennai India"],
          ["Varkala Beach", "Varkala beach Kerala India"],
          ["Andaman Islands", "Andaman Islands beach India"]
        ]
      },
      {
        category: "Culture",
        items: [
          ["Bharatanatyam", "Bharatanatyam classical dance India"],
          ["Kathakali", "Kathakali dance Kerala India"],
          ["Indian Classical Music", "Indian classical music performance"],
          ["Traditional Indian Art", "Indian traditional folk art"]
        ]
      }
    ];

    const allItems = collections.flatMap(group =>
      group.items.map(([title, query]) => ({
        title,
        query,
        category: group.category
      }))
    );

    const gallery = document.getElementById("gallery");
    const filters = document.getElementById("filters");
    const searchInput = document.getElementById("searchInput");
    const status = document.getElementById("status");

    let activeCategory = "All";
    let searchTerm = "";
    const imageCache = new Map();

    function createFilters() {
      const categories = ["All", ...collections.map(group => group.category)];

      filters.innerHTML = categories.map(category => `
        <button
          class="filter-btn ${category === activeCategory ? "active" : ""}"
          data-category="${category}"
        >${category}</button>
      `).join("");

      filters.querySelectorAll("button").forEach(button => {
        button.addEventListener("click", () => {
          activeCategory = button.dataset.category;
          createFilters();
          renderGallery();
        });
      });
    }

    function createCard(item, index) {
      const card = document.createElement("article");
      card.className = "card";

      const imageWrap = document.createElement("div");
      imageWrap.className = "image-wrap";

      const placeholder = document.createElement("div");
      placeholder.className = "placeholder";
      placeholder.innerHTML = `
        <span class="symbol">📷</span>
        <span>Loading photo...</span>
      `;

      const img = document.createElement("img");
      img.alt = item.title;
      img.loading = index < 8 ? "eager" : "lazy";
      img.decoding = "async";

      img.addEventListener("load", () => {
        img.classList.add("loaded");
        placeholder.classList.add("hidden");
      });

      img.addEventListener("error", () => {
        img.classList.remove("loaded");
        placeholder.classList.remove("hidden");
        placeholder.innerHTML = `
          <span class="symbol">🖼️</span>
          <span>Photo unavailable</span>
        `;
      });

      imageWrap.append(img, placeholder);

      const info = document.createElement("div");
      info.className = "card-info";

      const title = document.createElement("h3");
      title.textContent = item.title;

      const category = document.createElement("span");
      category.className = "category";
      category.textContent = item.category;

      const link = document.createElement("a");
      link.className = "view-link";
      link.textContent = "View image source ↗";
      link.href = "https://commons.wikimedia.org/wiki/Special:MediaSearch?type=image&search="
        + encodeURIComponent(item.query);
      link.target = "_blank";
      link.rel = "noopener noreferrer";

      info.append(title, category, document.createElement("br"), link);
      card.append(imageWrap, info);

      loadImage(item, img, placeholder);

      return card;
    }

    async function loadImage(item, img, placeholder) {
      const cacheKey = item.query;

      if (imageCache.has(cacheKey)) {
        img.src = imageCache.get(cacheKey);
        return;
      }

      try {
        const apiUrl = new URL("https://commons.wikimedia.org/w/api.php");
        apiUrl.search = new URLSearchParams({
          action: "query",
          generator: "search",
          gsrsearch: item.query,
          gsrnamespace: "6",
          gsrlimit: "8",
          prop: "imageinfo",
          iiprop: "url",
          iiurlwidth: "800",
          format: "json",
          origin: "*"
        });

        const response = await fetch(apiUrl);

        if (!response.ok) {
          throw new Error("Could not contact Wikimedia Commons");
        }

        const data = await response.json();
        const pages = Object.values(data.query?.pages || {});

        const usable = pages.find(page => {
          const info = page.imageinfo?.[0];
          return info && (info.thumburl || info.url);
        });

        if (!usable) {
          throw new Error("No image found");
        }

        const imageUrl = usable.imageinfo[0].thumburl
          || usable.imageinfo[0].url;

        imageCache.set(cacheKey, imageUrl);
        img.src = imageUrl;
      } catch (error) {
        placeholder.classList.remove("hidden");
        placeholder.innerHTML = `
          <span class="symbol">🌐</span>
          <span>Could not load photo. Check your internet connection.</span>
        `;
      }
    }

    function renderGallery() {
      const visibleItems = allItems.filter(item => {
        const matchesCategory =
          activeCategory === "All" || item.category === activeCategory;

        const matchesSearch =
          item.title.toLowerCase().includes(searchTerm) ||
          item.category.toLowerCase().includes(searchTerm);

        return matchesCategory && matchesSearch;
      });

      gallery.innerHTML = "";

      if (!visibleItems.length) {
        gallery.innerHTML =
          '<div class="empty">No photos found. Try another search.</div>';
        status.textContent = "0 photos found";
        return;
      }

      visibleItems.forEach((item, index) => {
        gallery.appendChild(createCard(item, index));
      });

      status.textContent =
        `Showing ${visibleItems.length} of ${allItems.length} photos`;
    }

    searchInput.addEventListener("input", event => {
      searchTerm = event.target.value.trim().toLowerCase();
      renderGallery();
    });

    createFilters();
    renderGallery();
  </script>
</body>
</html>
