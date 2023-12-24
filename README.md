

<!DOCTYPE html>

<html lang="en">
<head>
<!-- Google tag (gtag.js) 
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-JZJTWL2Y0E"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
  
    gtag('config', 'G-JZJTWL2Y0E');-->

<meta charset="utf-8"/>
<meta content="width=device-width, initial-scale=1.0" name="viewport"/>
<title>Google Sheets Website Template</title>
<link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.3/dist/tailwind.min.css" rel="stylesheet"/>
<link href="https://cdn.jsdelivr.net/npm/aos@2.3.1/dist/aos.css" rel="stylesheet"/>
<link href="https://cdnjs.cloudflare.com/ajax/libs/magic/1.0.0/magic.min.css" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.0/papaparse.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/aos@2.3.1/dist/aos.js"></script>
<style>
    #blog {
      padding-bottom: 8rem; /* Adjust this value to your liking */
    }
    #hero h1, #hero h2 {
        margin-top: 0;
        padding: 5px 10px;
        border-radius: 5px;
    }
    #hero h1.show-bg, #hero h2.show-bg {
        background-color: rgba(0, 0, 0, 0.4);
        display: inline;
    }
    #hero {
      height: 100vh;
      width: 100%;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    #header-container {
      text-align: center;
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }
    #scroll_to_blog_button {
      position: absolute;
      top: 50px;
      right: 50px;
      cursor: pointer;
      animation: bounce 3s infinite;
    }
    @keyframes bounce {
      0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
      }
      40% {
        transform: translateY(-30px);
      }
      60% {
        transform: translateY(-30px);
      }
    }
    #sticky_footer {
      transition: background-color 300ms ease;
    }
    #owner_line {
      display: none;
      text-align: center;
      margin-top: 1rem;
    }
    html {
      scroll-behavior: smooth;
    }
  </style>
<script data-domain="a.picoapps.xyz" defer="" src="https://plausible.io/js/script.outbound-links.js"></script></head>
<body class="bg-gray-100">
<div class="magic-slideDown bg-cover bg-center" id="hero">
<div id="header-container">
<h1 class="text-white text-4xl md:text-6xl font-bold mt-10 ml-10 magic magic-wind show-bg" data-aos="fade-down" id="header"></h1>
<h2 class="text-white text-2xl md:text-4xl mt-4 ml-10 magic magic-time show-bg" data-aos="fade-up" id="subheader"></h2>
</div>
<button class="bg-blue-500 text-white px-4 py-2 rounded magic-hover magic-hover__square" id="scroll_to_blog_button" onclick="scrollToBlog()">Scroll to Content</button>
</div>
<div class="container mx-auto p-8" id="blog"></div>
<div class="footer bg-gray-800 p-4 mt-16 text-white fixed bottom-0 w-full opacity-100" id="sticky_footer">
<div class="container mx-auto flex justify-evenly" id="social_links"></div>
<div class="container mx-auto" id="owner_line"></div>
</div>
<script>
    const blogContainer = document.querySelector("#blog");
    const heroImage = document.querySelector("#hero");
    const header = document.querySelector("#header");
    const subheader = document.querySelector("#subheader");
    const socialLinks = document.querySelector("#social_links");
    const stickyFooter = document.querySelector("#sticky_footer");
    const ownerLine = document.querySelector("#owner_line");

    async function fetchCSV(url) {
      const response = await fetch(url);
      const text = await response.text();
      const parsedCSV = Papa.parse(text, { header: true });
      return parsedCSV.data;
    }

    function fetchUnsplashFeaturedImage(theme) {
      return `https://source.unsplash.com/featured/1600x900/?${encodeURIComponent(theme)}`;
    }

    function unsplashImageUrl(title) {
      return `https://source.unsplash.com/800x450/?${encodeURIComponent(title)}`;
    }

    function updateFooterTransparency() {
      if (window.innerHeight + window.pageYOffset >= document.body.offsetHeight - 10) {
        stickyFooter.classList.remove("opacity-50");
        stickyFooter.classList.add("opacity-100");
        ownerLine.style.display = "block";
      } else {
        stickyFooter.classList.remove("opacity-100");
        stickyFooter.classList.add("opacity-50");
        ownerLine.style.display = "none";
      }
    }

    function updateParallax() {
      const y = window.scrollY;
      heroImage.style.backgroundPositionY = y * -0.5 + "px";
    }

    function scrollToBlog() {
      document.getElementById("blog").scrollIntoView({ behavior: "smooth" });
    }

    function updateHeaderAndSubheaderBg() {
      // Removes the 'show-bg' class to hide transparent background
      if (!header.innerText) header.classList.remove("show-bg");
      if (!subheader.innerText) subheader.classList.remove("show-bg");
    }

    window.addEventListener("scroll", () => {
      updateFooterTransparency();
      updateParallax();
    });

    async function fetchAndDisplayContent() {
      const configData = await fetchCSV("https://docs.google.com/spreadsheets/d/e/2PACX-1vRmuxAMK2OYSxmTmthD_eR9lztCh5Opp2gEJZ8XoRySdl72RhMF4IauRg7aaRQtZ6NGETZz_uuG1VPT/pub?gid=81047960&single=true&output=csv");
      const config = configData[0];
      header.innerText = config.Header;
      subheader.innerText = config.Subheader;
      let heroImageUrl = config['hero-image'] ? config['hero-image'] : fetchUnsplashFeaturedImage(config.Theme);
		heroImage.style.backgroundImage = `url(${heroImageUrl})`;

      heroImage.style.backgroundAttachment = 'fixed';
      heroImage.style.backgroundSize = 'cover';

      const themeColor = config.Color;
      document.body.style.setProperty('--theme-color', themeColor);
      document.getElementById("scroll_to_blog_button").style.backgroundColor = themeColor;
      stickyFooter.style.backgroundColor = themeColor;

      // Add owner name
      ownerLine.innerHTML = `Made with 🧠 by ${config.Owner}`;

      Object.keys(config).forEach((key) => {
        if (key !== "Header" && key !== "Subheader" && key !== "Theme" && key !== "Color" && key !== "Owner") {
          if (config[key] !== '') {
            let url;
            switch (key.toLowerCase()) {
              case 'twitterx':
                url = `https://twitter.com/${config[key]}`;
                break;
              case 'facebook':
                url = `https://facebook.com/${config[key]}`;
                break;
              case 'reddit':
                url = `https://reddit.com/user/${config[key]}`;
                break;
              case 'email':
                url = `mailto:${config[key]}`;
                break;
              case 'github':
                url = `https://github.com/${config[key]}`;
                break;
              case 'linkedin':
                url = `https://linkedin.com/in/${config[key]}`;
                break;
              case 'instagram':
                url = `https://instagram.com/${config[key]}`;
                break;
              // ... add cases for other platforms as needed
              default:
                url = config[key];  // Fallback to using the value directly if it doesn't match a known platform
                break;
            }
            const link = `<a href="${url}" target="_blank" class="magic-hover magic-hover__square transition duration-300 ease-in-out transform hover:scale-125"><img src="https://img.icons8.com/ios-glyphs/30/ffffff/${key.toLowerCase()}.png" alt="${key}" /></a>`;
            socialLinks.insertAdjacentHTML('beforeend', link);
          }
        }
      });

      const blogData = await fetchCSV("https://docs.google.com/spreadsheets/d/e/2PACX-1vRmuxAMK2OYSxmTmthD_eR9lztCh5Opp2gEJZ8XoRySdl72RhMF4IauRg7aaRQtZ6NGETZz_uuG1VPT/pub?gid=0&single=true&output=csv");
      blogData.sort((a, b) => new Date(b.date) - new Date(a.date));

      blogData.forEach((row, index) => {
		  const blogPost = document.createElement("div");
		  blogPost.setAttribute('data-aos', 'fade-up');
		  blogPost.setAttribute('data-aos-delay', index * 100);

		  const imageUrl = row.image ? row.image : unsplashImageUrl(row.title);

		  blogPost.innerHTML = `
			<div class="bg-white rounded-lg overflow-hidden p-6 mb-8 shadow">
			  <img class="w-full h-64 object-cover mb-4 magic-hover magic-hover__image" src="${imageUrl}" alt="${row.title}">
			  <div class="text-gray-900 font-bold text-3xl mb-4 magic-twister">${row.title}</div>
			  <div class="text-gray-600">${row.content}</div>
			  <div class="mt-6">
				<span class="text-gray-600 text-sm">By ${row.author}</span>
				<span class="text-gray-600 text-sm">${row.date}</span>
			  </div>
			  <div class="mt-4">
				${row.tags.split(',').map(tag => `<span class="bg-gray-200 text-gray-700 rounded-full px-2 py-1 mr-2 text-sm">${tag.trim()}</span>`).join('')}
			  </div>
			</div>
		  `;
		  blogContainer.appendChild(blogPost);
		});

      updateHeaderAndSubheaderBg();
      AOS.init();
    }

    fetchAndDisplayContent();
    updateFooterTransparency();
  </script>

<a href="https://picoapps.xyz/remix?appUrl=eye-hand&amp;ref=InvertedBadge" rel="noopener noreferrer" style="position: fixed; bottom: 5px; left: 0px; margin-bottom:10px; margin-left:10px; z-index: 9999; background-color: black; border-radius: 5px;box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 1px 3px rgba(0,0,0,.1);" target="_blank">
<img alt="Pico Badge" src="https://picoapps.xyz/badge/badge-inverted.png" style="width: 160px;"/>
</a></body>
</html>
