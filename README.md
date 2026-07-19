<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>modeX | Sokak Modasının Yeni Boyutu</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- FontAwesome İkonları -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    .glass {
      background: rgba(11, 15, 25, 0.75);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid rgba(255, 255, 255, 0.05);
    }
    @keyframes pulse-neon {
      0%, 100% { transform: scale(1); filter: drop-shadow(0 0 15px rgba(168, 85, 247, 0.6)); }
      50% { transform: scale(1.08); filter: drop-shadow(0 0 35px rgba(6, 182, 212, 0.8)); }
    }
    .neon-pulse { animation: pulse-neon 2s infinite ease-in-out; }
  </style>
</head>
<body class="bg-[#070a13] text-gray-100 min-h-screen font-sans overflow-hidden">

  <!-- ================= ANİMASYONLU YÜKLEME EKRANI (SPLASH) ================= -->
  <div id="splashScreen" class="fixed inset-0 bg-[#070a13] z-[9999] flex flex-col items-center justify-center transition-all duration-1000 ease-in-out">
    <div class="text-center">
      <h1 class="text-6xl md:text-8xl font-black tracking-widest bg-gradient-to-r from-purple-500 via-pink-500 to-cyan-400 bg-clip-text text-transparent neon-pulse">
        modeX
      </h1>
      <p class="text-gray-500 tracking-[0.3em] uppercase text-xs md:text-sm mt-6 animate-pulse">Tarz Yükleniyor...</p>
    </div>
  </div>

  <!-- ================= MÜŞTERİ GİRİŞ MODAL ================= -->
  <div id="authModal" class="fixed inset-0 bg-black/80 backdrop-blur-md z-[9999] hidden flex items-center justify-center p-4">
    <div class="glass w-full max-w-md p-8 rounded-3xl text-center relative z-10 space-y-5">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-black tracking-widest bg-gradient-to-r from-purple-500 via-pink-500 to-cyan-400 bg-clip-text text-transparent">modeX</h2>
        <button onclick="closeAuthModal()" class="text-gray-400 hover:text-white text-2xl">&times;</button>
      </div>
      <p class="text-gray-400 text-sm">Siparişinizi güvenle ve hızlıca WhatsApp üzerinden tamamlamak için giriş yapın.</p>
      
      <!-- Kullanıcı Giriş Formu -->
      <div class="space-y-4 text-left pt-2">
        <div>
          <label class="block text-xs font-bold tracking-widest text-gray-500 mb-1.5 uppercase">Kullanıcı Adı</label>
          <input type="text" id="customerUserInput" placeholder="Adınız veya kullanıcı adınız" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none focus:border-purple-500 text-sm">
        </div>
        <div>
          <label class="block text-xs font-bold tracking-widest text-gray-500 mb-1.5 uppercase">Şifre</label>
          <input type="password" id="customerPasswordInput" placeholder="••••••••" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none focus:border-purple-500 text-sm">
        </div>
        
        <button onclick="loginWithForm()" class="w-full py-3.5 bg-gradient-to-r from-purple-600 via-pink-600 to-cyan-500 rounded-xl font-bold hover:opacity-90 transition mt-2 text-sm shadow-lg shadow-purple-500/10">
          Giriş Yap ve Devam Et
        </button>
      </div>

      <div class="relative flex py-2 items-center">
        <div class="flex-grow border-t border-gray-800"></div>
        <span class="flex-shrink mx-4 text-gray-500 text-xs uppercase font-bold tracking-wider">Veya</span>
        <div class="flex-grow border-t border-gray-800"></div>
      </div>

      <!-- Misafir Girişi -->
      <button onclick="loginAsGuest()" class="w-full py-3.5 px-4 rounded-xl bg-[#121829] border border-gray-800 hover:border-cyan-400 text-sm font-bold transition flex items-center justify-center gap-2">
        <i class="fas fa-user-secret text-cyan-400"></i> Misafir Olarak Giriş Yap
      </button>
    </div>
  </div>

  <!-- ================= ANA İÇERİK ================= -->
  <div id="mainContent">
    
    <!-- ================= ÜST MENÜ ================= -->
    <header class="fixed top-0 left-0 w-full z-50 glass transition-all duration-500" id="mainHeader">
      <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
        <h1 onclick="window.location.hash=''" class="text-3xl font-extrabold tracking-widest bg-gradient-to-r from-purple-500 via-pink-500 to-cyan-400 bg-clip-text text-transparent cursor-pointer">
          modeX
        </h1>
        <div class="flex items-center gap-3">
          <!-- Giriş Butonları Grubu -->
          <div id="guestLoginGroup" class="flex items-center gap-2">
            <!-- Üst Menü Hızlı Misafir Giriş Butonu -->
            <button onclick="loginAsGuest()" class="text-xs bg-cyan-500/10 hover:bg-cyan-500 border border-cyan-500/30 text-cyan-400 hover:text-[#070a13] px-3 py-2 rounded-xl transition font-bold">
              <i class="fas fa-user-secret mr-1"></i> Misafir Girişi
            </button>
            <!-- Yönetici Hızlı Erişim Butonu -->
            <button id="adminHeaderTrigger" onclick="openAdminModal()" class="text-xs bg-purple-600/10 hover:bg-purple-600 border border-purple-500/30 text-purple-400 hover:text-white px-3 py-2 rounded-xl transition font-semibold">
              <i class="fas fa-user-shield mr-1"></i> Yönetici Girişi
            </button>
          </div>
          
          <button id="adminReturnBtn" onclick="document.getElementById('adminPanel').classList.remove('hidden')" class="hidden text-xs bg-pink-600/20 hover:bg-pink-600 border border-pink-500 text-pink-400 hover:text-white px-3 py-2 rounded-xl transition font-bold">
            <i class="fas fa-tools mr-1"></i> Panele Dön
          </button>

          <!-- Müşteri Durumu -->
          <span id="userBadge" class="hidden text-xs font-semibold bg-[#121829] border border-gray-800 px-4 py-2 rounded-full text-purple-400 flex items-center gap-2"></span>
          <button id="logoutBtn" onclick="logoutCustomer()" class="hidden text-xs text-red-500 hover:text-red-400 font-bold transition">Çıkış</button>
          
          <!-- Sepet Butonu -->
          <button onclick="toggleCart()" class="relative p-3 rounded-full bg-[#121829] border border-gray-800 hover:border-cyan-400 transition">
            <i class="fas fa-shopping-cart text-lg text-cyan-400"></i>
            <span id="cartCount" class="absolute -top-1 -right-1 w-5 h-5 bg-pink-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center">0</span>
          </button>
        </div>
      </div>
    </header>

    <!-- ================= KAHRAMAN BÖLÜMÜ ================= -->
    <main class="relative pt-40 pb-20 flex flex-col items-center justify-center text-center px-4 overflow-hidden min-h-screen">
      <div class="absolute top-1/4 left-1/4 w-96 h-96 bg-purple-600/10 rounded-full filter blur-[120px]"></div>
      <div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-cyan-500/10 rounded-full filter blur-[120px]"></div>

      <div class="transition-all duration-1000" id="heroContent">
        <span class="text-cyan-400 text-sm font-bold tracking-widest uppercase mb-3 block">Sınırları Zorla</span>
        <h2 class="text-5xl md:text-8xl font-black tracking-tight mb-6 leading-none uppercase">
          TARZINI <br/>
          <span class="bg-gradient-to-r from-purple-400 to-pink-500 bg-clip-text text-transparent">YENİDEN TANIMLA</span>
        </h2>
        <p class="text-gray-400 max-w-xl text-base md:text-lg mb-8 mx-auto">
          Koleksiyonu özgürce keşfet, sepetini doldur ve tek tıkla WhatsApp üzerinden siparişini tamamla! 🚀
        </p>
        <a href="#products" class="inline-block px-8 py-4 rounded-full bg-cyan-400 text-[#070a13] font-bold text-lg hover:bg-cyan-300 hover:scale-105 transition shadow-lg shadow-cyan-500/20">
          Koleksiyonu Keşfet 🔥
        </a>
      </div>
    </main>

    <!-- ================= VİTRİN BÖLÜMÜ ================= -->
    <section id="products" class="max-w-7xl mx-auto px-6 py-20">
      <div class="mb-12">
        <h3 class="text-3xl font-black tracking-wider text-purple-400">Vitrin</h3>
        <p class="text-gray-500 text-sm">Gerçek zamanlı modeX koleksiyonu</p>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8" id="productGrid"></div>
    </section>

    <!-- ================= SEPET PANELİ ================= -->
    <div id="cartPanel" class="fixed top-0 right-0 h-full w-full md:w-96 glass z-[1000] translate-x-full transition-transform duration-500 flex flex-col">
      <div class="p-6 bg-black/30 border-b border-gray-800 flex justify-between items-center">
        <h3 class="text-xl font-bold text-cyan-400">Sepetim</h3>
        <button onclick="toggleCart()" class="text-gray-400 hover:text-white text-2xl">&times;</button>
      </div>
      <div id="cartList" class="flex-1 p-6 overflow-y-auto space-y-4"></div>
      <div class="p-6 border-t border-gray-800 bg-[#070a13]">
        <div class="flex justify-between items-center mb-6">
          <span class="text-gray-400 font-bold">Toplam Tutar:</span>
          <span id="cartTotal" class="text-2xl font-black text-cyan-400">0 TL</span>
        </div>
        <button onclick="processOrderCheckout()" class="w-full py-4 bg-emerald-500 hover:bg-emerald-400 text-white font-extrabold rounded-2xl transition flex items-center justify-center gap-3 shadow-lg shadow-emerald-500/20">
          WhatsApp İle Siparişi Gönder <i class="fab fa-whatsapp text-xl"></i>
        </button>
      </div>
    </div>

    <!-- ================= DETAYLI ÜRÜN MODALI ================= -->
    <div id="productDetailModal" class="fixed inset-0 bg-black/90 backdrop-blur-md z-[999] hidden flex items-center justify-center p-4">
      <div class="glass w-full max-w-4xl rounded-3xl overflow-hidden grid grid-cols-1 md:grid-cols-2">
        <div class="p-6 flex flex-col justify-between bg-black/25">
          <div class="relative aspect-square rounded-2xl overflow-hidden bg-gray-900 flex items-center justify-center">
            <img id="detailMainImg" src="" class="w-full h-full object-cover">
          </div>
          <div id="detailThumbnails" class="flex gap-2 mt-4 overflow-x-auto py-2"></div>
        </div>
        <div class="p-8 flex flex-col justify-between">
          <div>
            <div class="flex justify-between items-start mb-6">
              <h4 id="detailTitle" class="text-3xl font-black tracking-tight">Ürün Adı</h4>
              <button onclick="closeProductDetail()" class="text-gray-400 hover:text-white text-3xl">&times;</button>
            </div>
            <p id="detailDesc" class="text-gray-400 text-sm leading-relaxed mb-4"></p>
            <p id="detailStockStatus" class="text-sm font-semibold mb-6"></p>
            <div id="detailLinkBox" class="bg-[#121829] p-3 rounded-xl border border-gray-800 mb-6 flex justify-between items-center text-xs">
              <span class="text-cyan-400 truncate mr-2" id="detailShareLink">Link</span>
              <button onclick="copyProductLink()" class="px-3 py-1 bg-purple-600 rounded-lg hover:bg-purple-500 font-bold">Kopyala</button>
            </div>
          </div>
          <div class="flex items-center justify-between gap-4 mt-6">
            <span id="detailPrice" class="text-3xl font-black text-cyan-400">0 TL</span>
            <div class="flex gap-2">
              <button id="detailAddBtn" class="px-5 py-3 bg-[#121829] hover:bg-purple-600/20 border border-purple-500/30 text-white font-bold rounded-2xl transition text-sm">
                Sepete Ekle 🛒
              </button>
              <button id="detailDirectBuyBtn" class="px-5 py-3 bg-emerald-500 hover:bg-emerald-400 text-white font-extrabold rounded-2xl transition text-sm flex items-center gap-1">
                Hemen Al <i class="fab fa-whatsapp"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <footer class="text-center py-12 border-t border-gray-900 mt-20 text-gray-600 text-sm">
      <p>&copy; 2026 modeX. Tüm Hakları Saklıdır. 🔥</p>
    </footer>
  </div>

  <!-- ================= AYRI YÖNETİCİ GİRİŞ MODALI ================= -->
  <div id="adminModal" class="fixed inset-0 bg-black/85 backdrop-blur-sm z-[9999] hidden flex items-center justify-center p-4">
    <div class="glass w-full max-w-sm p-8 rounded-3xl">
      <div class="flex justify-between items-center mb-6">
        <h4 class="text-2xl font-black tracking-wider text-purple-400 font-mono">Yönetici Paneli</h4>
        <button onclick="closeAdminModal()" class="text-gray-400 hover:text-white text-2xl">&times;</button>
      </div>
      <div class="space-y-4">
        <div>
          <label class="block text-xs font-bold tracking-widest text-gray-500 mb-2 uppercase">Kullanıcı Adı</label>
          <input type="text" id="adminUserInput" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none focus:border-purple-500 text-sm">
        </div>
        <div>
          <label class="block text-xs font-bold tracking-widest text-gray-500 mb-2 uppercase">Şifre</label>
          <input type="password" id="adminPasswordInput" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none focus:border-purple-500 text-sm">
        </div>
        <button onclick="loginAdminEncrypted()" class="w-full py-3 bg-gradient-to-r from-purple-600 to-pink-600 rounded-xl font-bold hover:opacity-90 transition mt-4">
          Güvenli Giriş Yap
        </button>
      </div>
    </div>
  </div>

  <!-- ================= KONTROL PANELİ ================= -->
  <div id="adminPanel" class="fixed inset-0 bg-[#070a13] z-[9999] hidden overflow-y-auto p-6">
    <div class="max-w-6xl mx-auto mt-20">
      <div class="flex justify-between items-center mb-12">
        <h2 class="text-3xl font-extrabold text-purple-400">Yönetici Kontrol Paneli</h2>
        <div class="flex items-center gap-3">
          <button onclick="document.getElementById('adminPanel').classList.add('hidden')" class="px-4 py-2 bg-gray-800 hover:bg-gray-700 rounded-xl font-bold transition">Siteyi Gör</button>
          <button onclick="logoutAdmin()" class="px-4 py-2 bg-red-600 hover:bg-red-500 rounded-xl font-bold transition">Çıkış Yap</button>
        </div>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
        <div class="glass p-8 rounded-3xl space-y-6 lg:col-span-1 h-fit">
          <h3 class="text-xl font-bold text-cyan-400">Yeni Ürün Ekle</h3>
          <div>
            <label class="block text-sm mb-2">Ürün Adı</label>
            <input type="text" id="newProdName" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none">
          </div>
          <div>
            <label class="block text-sm mb-2">Ürün Fiyatı</label>
            <input type="number" id="newProdPrice" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none">
          </div>
          <div>
            <label class="block text-sm mb-2">Stok Adedi</label>
            <input type="number" id="newProdStock" class="w-full bg-[#121829] border border-gray-800 rounded-xl px-4 py-3 text-white focus:outline-none" value="10">
          </div>
          <div>
            <label class="block text-sm mb-2">Ürün Açıklaması</label>
            <textarea id="newProdDesc" rows="3" class="w-full bg-[#121829] border border-gray-800 rounded-xl p-4 text-white focus:outline-none"></textarea>
          </div>
          <div>
            <label class="block text-sm mb-2">Ürün Görselleri (Çoklu Seçilebilir)</label>
            <input type="file" id="newProdPics" multiple accept="image/*" class="w-full text-sm text-gray-400 file:bg-purple-600 file:text-white file:py-2 file:px-4 file:rounded-full file:border-0 cursor-pointer">
          </div>
          <button id="uploadBtn" onclick="saveProductLocal()" class="w-full py-4 bg-cyan-400 text-black font-extrabold rounded-2xl hover:bg-cyan-300 transition">
            Yayınla 🚀
          </button>
        </div>

        <div class="glass p-8 rounded-3xl lg:col-span-2">
          <h3 class="text-xl font-bold text-cyan-400 mb-6">Mevcut Ürünleri Yönet</h3>
          <div id="adminProductList" class="space-y-4"></div>
        </div>
      </div>

      <!-- Geliştirilmiş Giriş Yapan Kullanıcılar ve Detaylı Misafir Listesi -->
      <div class="glass p-8 rounded-3xl mt-8">
        <h3 class="text-xl font-bold text-cyan-400 mb-6">Giriş Yapıp Sipariş Veren Müşteriler / Misafirler</h3>
        <div id="userList" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
      </div>
    </div>
  </div>

  <script>
    const whatsappNum = "905519283542";
    let cart = [];
    let pendingAction = null;

    // Örnek başlangıç ürünleri
    const defaultProducts = [
      {
        id: "sample-1",
        name: "Oversize Cyberpunk Hoodie",
        price: 1250,
        stock: 8,
        description: "Neon detaylı, özel kesim sokak tarzı rahat kapüşonlu sweatshirt.",
        images: ["https://images.unsplash.com/photo-1556821840-3a63f95609a7?auto=format&fit=crop&w=600&q=80"]
      },
      {
        id: "sample-2",
        name: "Cargo Techwear Pantolon",
        price: 1450,
        stock: 5,
        description: "Su geçirmez kumaş teknolojisi, çoklu cep mimarisi ogün ayarlanabilir paça bağları.",
        images: ["https://images.unsplash.com/photo-1517462964-21fdcec3f25b?auto=format&fit=crop&w=600&q=80"]
      }
    ];

    window.addEventListener('load', () => {
      setTimeout(() => {
        const splash = document.getElementById('splashScreen');
        if(splash) splash.classList.add('opacity-0', 'pointer-events-none');
        document.body.classList.remove('overflow-hidden');
        renderHeaderAuth();
        
        if (!localStorage.getItem('productsData')) {
          localStorage.setItem('productsData', JSON.stringify(defaultProducts));
        }

        loadProductsAndUsers();
        updateCartUI();
        checkHashRoute();
      }, 1500);
    });

    window.addEventListener('hashchange', checkHashRoute);

    function checkHashRoute() {
      const hash = window.location.hash;
      if (hash.startsWith('#product-')) {
        const prodId = hash.replace('#product-', '');
        openProductDetail(prodId);
      } else {
        closeProductDetail();
      }
    }

    function renderHeaderAuth() {
      const currentCustomer = sessionStorage.getItem('customerName');
      const badge = document.getElementById('userBadge');
      const logout = document.getElementById('logoutBtn');
      const guestLoginGroup = document.getElementById('guestLoginGroup');
      const adminReturn = document.getElementById('adminReturnBtn');

      if (currentCustomer) {
        badge.innerHTML = `<i class="fas fa-user text-cyan-400"></i> ${currentCustomer}`;
        badge.classList.remove('hidden');
        logout.classList.remove('hidden');
        if(guestLoginGroup) guestLoginGroup.classList.add('hidden');
      } else {
        badge.classList.add('hidden');
        logout.classList.add('hidden');
        if(guestLoginGroup) guestLoginGroup.classList.remove('hidden');
      }

      if (sessionStorage.getItem('isAdmin') === 'true') {
        if(guestLoginGroup) guestLoginGroup.classList.add('hidden');
        if(adminReturn) adminReturn.classList.remove('hidden');
      } else {
        if(adminReturn) adminReturn.classList.add('hidden');
      }
    }

    function openAuthModal(actionType, data = null) {
      pendingAction = { type: actionType, data: data };
      document.getElementById('authModal').classList.remove('hidden');
    }

    function closeAuthModal() {
      document.getElementById('authModal').classList.add('hidden');
      pendingAction = null;
    }

    function loginWithForm() {
      const username = document.getElementById('customerUserInput').value.trim();
      const pass = document.getElementById('customerPasswordInput').value.trim();
      if (!username) return alert("Lütfen kullanıcı adınızı yazın!");
      if (!pass) return alert("Lütfen şifrenizi yazın!");
      
      executeSuccessfulLogin(username, pass, false);
    }

    function loginAsGuest() {
      const randomId = Math.floor(1000 + Math.random() * 9000);
      const guestName = `Misafir-${randomId}`;
      const guestPass = `msfr-${Math.floor(100000 + Math.random() * 900000)}`; // Misafire otomatik özel şifre üretimi
      
      executeSuccessfulLogin(guestName, guestPass, true);
    }

    function openAdminModal() { document.getElementById('adminModal').classList.remove('hidden'); }
    function closeAdminModal() { document.getElementById('adminModal').classList.add('hidden'); }

    function executeSuccessfulLogin(name, password, isGuest) {
      sessionStorage.setItem('customerName', name);
      
      // Şimdiki zamanı GG.AA.YYYY - SS:DD formatına çevirme
      const now = new Date();
      const dateStr = `${String(now.getDate()).padStart(2, '0')}.${String(now.getMonth() + 1).padStart(2, '0')}.${now.getFullYear()} - ${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}`;

      const userList = JSON.parse(localStorage.getItem('activeUsersDetails') || '[]');
      
      // Eğer kullanıcı listede yoksa detaylarıyla ekle
      if (!userList.some(u => u.username === name)) {
        userList.push({
          username: name,
          password: password,
          loginTime: dateStr,
          type: isGuest ? 'guest' : 'form'
        });
        localStorage.setItem('activeUsersDetails', JSON.stringify(userList));
      }

      renderHeaderAuth();
      document.getElementById('authModal').classList.add('hidden');

      if (pendingAction) {
        if (pendingAction.type === 'directBuy') {
          const p = pendingAction.data;
          triggerWhatsAppMessage(p.name, p.price, p.id);
        } else if (pendingAction.type === 'cartCheckout') {
          triggerCartWhatsAppMessage();
        }
        pendingAction = null;
      } else {
        alert(`Hoş geldin, ${name}! Keyifli alışverişler. 🔥`);
      }
    }

    function logoutCustomer() {
      sessionStorage.removeItem('customerName');
      alert("Oturum kapatıldı.");
      location.reload();
    }

    function toggleCart() {
      document.getElementById('cartPanel').classList.toggle('translate-x-full');
    }

    function loginAdminEncrypted() {
      const u = document.getElementById('adminUserInput').value.trim().toLowerCase();
      const p = document.getElementById('adminPasswordInput').value.trim();

      if (!u || !p) return alert("Lütfen alanları doldurun!");
      const encode = str => str.split('').map(c => String.fromCharCode(c.charCodeAt(0) + 7)).reverse().join('');

      if (encode(u) === "hoh{" && encode(p) === ":98hoh{") {
        sessionStorage.setItem('isAdmin', 'true');
        alert("Yönetici girişi onaylandı! 🛠️");
        closeAdminModal();
        renderHeaderAuth();
        document.getElementById('adminPanel').classList.remove('hidden');
        loadProductsAndUsers();
      } else {
        alert("Kullanıcı adı veya şifre yanlış!");
      }
    }

    function logoutAdmin() {
      sessionStorage.removeItem('isAdmin');
      location.reload();
    }

    function loadProductsAndUsers() {
      const userListEl = document.getElementById('userList');
      if(userListEl) {
        const savedUsers = JSON.parse(localStorage.getItem('activeUsersDetails') || '[]');
        userListEl.innerHTML = savedUsers.length === 0 ? '<div class="col-span-full text-xs text-gray-600">Henüz kimse giriş yapmadı.</div>' : '';
        
        savedUsers.forEach(u => {
          let typeBadge = u.type === 'guest' ? 
            '<span class="text-xs bg-cyan-500/10 text-cyan-400 border border-cyan-500/30 px-2 py-0.5 rounded-md font-mono"><i class="fas fa-user-secret"></i> Misafir</span>' : 
            '<span class="text-xs bg-purple-500/10 text-purple-400 border border-purple-500/30 px-2 py-0.5 rounded-md font-mono"><i class="fas fa-user-check"></i> Kayıtlı Müşteri</span>';
          
          let cardHtml = `
            <div class="bg-[#121829] p-4 rounded-2xl border border-gray-800 space-y-2">
              <div class="flex justify-between items-start">
                <span class="font-bold text-sm text-white block">${u.username}</span>
                ${typeBadge}
              </div>
              <div class="text-xs text-gray-400 space-y-1">
                <p><i class="fas fa-key text-gray-600 mr-1"></i> Şifre: <span class="font-mono text-gray-200 select-all">${u.password}</span></p>
                <p><i class="fas fa-clock text-gray-600 mr-1"></i> Giriş Zamanı: <span class="text-purple-400">${u.loginTime}</span></p>
              </div>
            </div>
          `;
          userListEl.insertAdjacentHTML('beforeend', cardHtml);
        });
      }

      const grid = document.getElementById('productGrid');
      const adminList = document.getElementById('adminProductList');
      if(grid) grid.innerHTML = "";
      if(adminList) adminList.innerHTML = "";

      const products = JSON.parse(localStorage.getItem('productsData') || '[]');

      if (products.length === 0) {
        if(grid) grid.innerHTML = `<div class="col-span-full text-center text-gray-500 py-10">Koleksiyonda ürün bulunmuyor. Yönetici panelinden hemen ekleyebilirsiniz!</div>`;
        return;
      }

      products.forEach(p => {
        renderCard(p);
        renderAdminRow(p);
      });
    }

    function renderCard(p) {
      const grid = document.getElementById('productGrid');
      if(!grid) return;
      const mainImage = p.images && p.images.length > 0 ? p.images[0] : 'https://images.unsplash.com/photo-1556821840-3a63f95609a7?auto=format&fit=crop&w=600&q=80';
      const stockBadge = p.stock <= 0 ? `<span class="absolute top-4 right-4 bg-red-600 text-white text-xs px-3 py-1 rounded-full font-bold z-10">Tükendi</span>` : '';
      
      const card = `
        <div class="glass rounded-3xl overflow-hidden group hover:border-purple-500/50 transition duration-300 relative">
          ${stockBadge}
          <div onclick="window.location.hash='product-${p.id}'" class="relative overflow-hidden aspect-square cursor-pointer">
            <img src="${mainImage}" class="w-full h-full object-cover group-hover:scale-110 transition duration-500">
          </div>
          <div class="p-6">
            <h4 onclick="window.location.hash='product-${p.id}'" class="text-xl font-bold mb-2 cursor-pointer hover:text-cyan-400 transition">${p.name}</h4>
            <p class="text-gray-400 text-sm mb-4 line-clamp-2">${p.description}</p>
            <div class="flex items-center justify-between gap-2 mt-4">
              <button onclick="addToCart('${p.id}')" ${p.stock <= 0 ? 'disabled class="flex-1 py-2.5 bg-gray-800 text-gray-500 rounded-xl font-bold text-xs cursor-not-allowed"' : 'class="flex-1 py-2.5 bg-[#121829] hover:bg-purple-600/20 border border-purple-500/30 text-white rounded-xl font-bold text-xs transition"'} >
                Sepete Ekle 🛒
              </button>
              <button onclick="handleDirectBuy('${p.name}', '${p.price.toLocaleString('tr-TR')} TL', '${p.id}')" class="flex-1 py-2.5 bg-emerald-500 hover:bg-emerald-400 text-white rounded-xl font-bold text-xs transition flex items-center justify-center gap-1">
                Hemen Al <i class="fab fa-whatsapp"></i>
              </button>
            </div>
          </div>
        </div>
      `;
      grid.insertAdjacentHTML('beforeend', card);
    }

    function renderAdminRow(p) {
      const adminList = document.getElementById('adminProductList');
      if(!adminList) return;
      const mainImage = p.images && p.images.length > 0 ? p.images[0] : 'https://images.unsplash.com/photo-1556821840-3a63f95609a7?auto=format&fit=crop&w=600&q=80';
      
      const row = `
        <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 bg-[#121829] p-4 rounded-2xl border border-gray-800">
          <div class="flex items-center gap-4">
            <img src="${mainImage}" class="w-12 h-12 object-cover rounded-xl border border-gray-700">
            <div>
              <h5 class="font-bold text-sm text-gray-200">${p.name}</h5>
              <p class="text-xs text-purple-400">${p.price.toLocaleString('tr-TR')} TL</p>
            </div>
          </div>
          <div class="flex items-center gap-3 w-full sm:w-auto justify-between sm:justify-end">
            <div class="flex items-center border border-gray-700 rounded-xl bg-[#070a13] overflow-hidden">
              <button onclick="updateStockLocal('${p.id}', -1)" class="px-3 py-1.5 hover:bg-gray-800 text-gray-400 font-bold">-</button>
              <span id="admin-stock-${p.id}" class="px-3 text-sm font-mono text-cyan-400 min-w-[30px] text-center">${p.stock}</span>
              <button onclick="updateStockLocal('${p.id}', 1)" class="px-3 py-1.5 hover:bg-gray-800 text-gray-400 font-bold">+</button>
            </div>
            <button onclick="deleteProductLocal('${p.id}')" class="p-2.5 bg-red-600/10 hover:bg-red-600 border border-red-500/20 text-red-500 hover:text-white rounded-xl transition text-xs flex items-center gap-1 font-semibold">
              <i class="fas fa-trash"></i> Sil
            </button>
          </div>
        </div>
      `;
      adminList.insertAdjacentHTML('beforeend', row);
    }

    function saveProductLocal() {
      const name = document.getElementById('newProdName').value.trim();
      const price = document.getElementById('newProdPrice').value.trim();
      const stock = document.getElementById('newProdStock').value.trim();
      const desc = document.getElementById('newProdDesc').value.trim();
      const fileInput = document.getElementById('newProdPics');
      const uploadBtn = document.getElementById('uploadBtn');

      if (!name || !price) return alert("Ürün adı ve fiyatı alanları zorunludur!");
      
      uploadBtn.innerText = "Yayınlanıyor... ⏳";
      uploadBtn.disabled = true;

      const processData = (imageUrls) => {
        const products = JSON.parse(localStorage.getItem('productsData') || '[]');
        const newProduct = {
          id: 'prod-' + Date.now(),
          name,
          price: parseFloat(price),
          stock: stock ? parseInt(stock) : 10,
          description: desc || 'Bu ürün için açıklama girilmedi.',
          images: imageUrls.length > 0 ? imageUrls : ["https://images.unsplash.com/photo-1556821840-3a63f95609a7?auto=format&fit=crop&w=600&q=80"]
        };

        products.push(newProduct);
        localStorage.setItem('productsData', JSON.stringify(products));

        alert("Ürün başarıyla kaydedildi! 🎉");
        
        document.getElementById('newProdName').value = '';
        document.getElementById('newProdPrice').value = '';
        document.getElementById('newProdStock').value = '10';
        document.getElementById('newProdDesc').value = '';
        fileInput.value = '';

        uploadBtn.disabled = false;
        uploadBtn.innerText = "Yayınla 🚀";
        
        loadProductsAndUsers();
      };

      if (fileInput.files.length > 0) {
        const filePromises = Array.from(fileInput.files).map(file => {
          return new Promise((resolve) => {
            const reader = new FileReader();
            reader.onload = (e) => resolve(e.target.result);
            reader.readAsDataURL(file);
          });
        });

        Promise.all(filePromises).then(base64Images => {
          processData(base64Images);
        });
      } else {
        processData([]);
      }
    }

    function updateStockLocal(id, amount) {
      const products = JSON.parse(localStorage.getItem('productsData') || '[]');
      const prod = products.find(p => p.id === id);
      if (prod) {
        prod.stock = Math.max(0, prod.stock + amount);
        localStorage.setItem('productsData', JSON.stringify(products));
        document.getElementById(`admin-stock-${id}`).innerText = prod.stock;
        loadProductsAndUsers();
      }
    }

    function deleteProductLocal(id) {
      if (!confirm("Bu ürünü silmek istediğinize emin misiniz?")) return;
      let products = JSON.parse(localStorage.getItem('productsData') || '[]');
      products = products.filter(p => p.id !== id);
      localStorage.setItem('productsData', JSON.stringify(products));
      loadProductsAndUsers();
    }

    function addToCart(id) {
      const products = JSON.parse(localStorage.getItem('productsData') || '[]');
      const prod = products.find(p => p.id === id);
      if (!prod || prod.stock <= 0) return alert("Ürün stokta yok!");

      const cartItem = cart.find(item => item.id === id);
      if (cartItem) {
        if (cartItem.quantity >= prod.stock) return alert("Maksimum stok miktarına ulaştınız!");
        cartItem.quantity++;
      } else {
        cart.push({ id: prod.id, name: prod.name, price: prod.price, image: prod.images[0], quantity: 1 });
      }
      updateCartUI();
    }

    function updateCartUI() {
      const cartList = document.getElementById('cartList');
      const cartCount = document.getElementById('cartCount');
      const cartTotal = document.getElementById('cartTotal');
      
      if(!cartList) return;
      cartList.innerHTML = "";
      
      let total = 0;
      let count = 0;

      cart.forEach(item => {
        total += item.price * item.quantity;
        count += item.quantity;

        const cartHtml = `
          <div class="flex items-center justify-between bg-[#121829] p-3 rounded-2xl border border-gray-800">
            <div class="flex items-center gap-3">
              <img src="${item.image}" class="w-12 h-12 object-cover rounded-xl border border-gray-700">
              <div>
                <h5 class="text-xs font-bold text-gray-200 truncate max-w-[120px]">${item.name}</h5>
                <p class="text-[11px] text-cyan-400">${item.price.toLocaleString('tr-TR')} TL</p>
              </div>
            </div>
            <div class="flex items-center border border-gray-700 rounded-lg bg-[#070a13] overflow-hidden scale-90">
              <button onclick="changeQty('${item.id}', -1)" class="px-2 py-1 text-xs text-gray-400 font-bold">-</button>
              <span class="px-2 text-xs font-mono text-white">${item.quantity}</span>
              <button onclick="changeQty('${item.id}', 1)" class="px-2 py-1 text-xs text-gray-400 font-bold">+</button>
            </div>
          </div>
        `;
        cartList.insertAdjacentHTML('beforeend', cartHtml);
      });

      cartCount.innerText = count;
      cartTotal.innerText = `${total.toLocaleString('tr-TR')} TL`;
    }

    function changeQty(id, amount) {
      const item = cart.find(i => i.id === id);
      if(item) {
        item.quantity += amount;
        if(item.quantity <= 0) {
          cart = cart.filter(i => i.id !== id);
        }
        updateCartUI();
      }
    }

    function openProductDetail(id) {
      const products = JSON.parse(localStorage.getItem('productsData') || '[]');
      const p = products.find(prod => prod.id === id);
      if (!p) return;

      document.getElementById('detailTitle').innerText = p.name;
      document.getElementById('detailDesc').innerText = p.description;
      document.getElementById('detailPrice').innerText = `${p.price.toLocaleString('tr-TR')} TL`;
      
      const stockEl = document.getElementById('detailStockStatus');
      if (p.stock <= 0) {
        stockEl.className = "text-sm font-bold text-red-500";
        stockEl.innerText = "Stok Durumu: Tükendi";
      } else {
        stockEl.className = "text-sm font-bold text-emerald-400";
        stockEl.innerText = `Stok Durumu: ${p.stock} Adet Mevcut`;
      }

      const mainImg = document.getElementById('detailMainImg');
      mainImg.src = p.images[0];

      const thumbBox = document.getElementById('detailThumbnails');
      thumbBox.innerHTML = "";
      p.images.forEach(imgUrl => {
        const thumb = document.createElement('img');
        thumb.src = imgUrl;
        thumb.className = "w-16 h-16 object-cover rounded-xl border border-gray-800 cursor-pointer hover:border-purple-500 transition";
        thumb.onclick = () => mainImg.src = imgUrl;
        thumbBox.appendChild(thumb);
      });

      document.getElementById('detailShareLink').innerText = `${window.location.origin}${window.location.pathname}#product-${p.id}`;
      
      document.getElementById('detailAddBtn').onclick = () => { addToCart(p.id); };
      document.getElementById('detailDirectBuyBtn').onclick = () => { handleDirectBuy(p.name, `${p.price.toLocaleString('tr-TR')} TL`, p.id); };

      document.getElementById('productDetailModal').classList.remove('hidden');
    }

    function closeProductDetail() {
      document.getElementById('productDetailModal').classList.add('hidden');
      if(window.location.hash.startsWith('#product-')) {
        window.location.hash = '';
      }
    }

    function copyProductLink() {
      const linkText = document.getElementById('detailShareLink').innerText;
      navigator.clipboard.writeText(linkText).then(() => {
        alert("Ürün linki panoya kopyalandı! 🔗");
      });
    }

    function handleDirectBuy(name, price, id) {
      const currentCustomer = sessionStorage.getItem('customerName');
      if (!currentCustomer) {
        openAuthModal('directBuy', { name, price, id });
      } else {
        triggerWhatsAppMessage(name, price, id);
      }
    }

    function triggerWhatsAppMessage(name, price, id) {
      const customer = sessionStorage.getItem('customerName') || "Müşteri";
      const productLink = `${window.location.origin}${window.location.pathname}#product-${id}`;
      const message = `Selam, Ben ${customer}! modeX sitenden şu ürünü almak istiyorum! 🔥\n\nÜrün: ${name}\nFiyat: ${price}\n📍 Ürün Detayı: ${productLink}`;
      window.open(`https://wa.me/${whatsappNum}?text=${encodeURIComponent(message)}`, "_blank");
    }

    function processOrderCheckout() {
      if (cart.length === 0) return alert("Sepetiniz boş!");
      const currentCustomer = sessionStorage.getItem('customerName');
      
      if (!currentCustomer) {
        openAuthModal('cartCheckout');
      } else {
        triggerCartWhatsAppMessage();
      }
    }

    function triggerCartWhatsAppMessage() {
      const customer = sessionStorage.getItem('customerName') || "Müşteri";
      let message = `Selam, Ben ${customer}! modeX sitesinden siparişim var: 🔥\n\n`;
      let total = 0;
      cart.forEach((item, index) => {
        const itemTotal = item.price * item.quantity; total += itemTotal;
        message += `${index + 1}) ${item.name} (${item.quantity} Adet) - ${itemTotal.toLocaleString('tr-TR')} TL\n📍 Link: ${window.location.origin}${window.location.pathname}#product-${item.id}\n\n`;
      });
      message += `------------------------------\n💰 Toplam Tutar: ${total.toLocaleString('tr-TR')} TL`;
      window.open(`https://wa.me/${whatsappNum}?text=${encodeURIComponent(message)}`, "_blank");
    }
  </script>
</body>
</html>
