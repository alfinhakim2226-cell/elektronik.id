<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Katalog Elektronik Terkini</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: {
                        primary: '#2563eb',
                        secondary: '#1e40af',
                        accent: '#f59e0b'
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f3f4f6; overflow-x: hidden; }
        .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(0,0,0,0.05); }
        .slide-fade { transition: opacity 0.5s ease-in-out; }
        .card-hover { transition: transform 0.3s ease, box-shadow 0.3s ease; }
        .card-hover:hover { transform: translateY(-5px); box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

        /* Toast Animation */
        @keyframes slideInRight {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; }
        }
        .toast-enter { animation: slideInRight 0.3s forwards; }
        .toast-exit { animation: fadeOut 0.3s forwards; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, addDoc, deleteDoc, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase Configuration fallback for standalone testing
        const defaultAppId = 'tech-store-demo-' + Math.floor(Math.random() * 1000);
        const appId = typeof __app_id !== 'undefined' ? __app_id : defaultAppId;
        
        // Setup Firebase (Using generic config if no environment config is provided)
        let firebaseConfig = {};
        if (typeof __firebase_config !== 'undefined') {
            firebaseConfig = JSON.parse(__firebase_config);
        } else {
            // Mock config for syntax completeness (will use local logic if Firebase fails in standalone)
            firebaseConfig = {
                apiKey: "mock-key", authDomain: "mock.firebaseapp.com", projectId: "mock-project"
            };
        }

        let app, db, auth;
        try {
            app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);
        } catch (e) {
            console.warn("Firebase initialization skipped (expected if no valid config). Using mock data fallback.");
        }

        // Make Firebase instances available globally for our app logic
        window.firebaseApp = { app, db, auth, appId };
    </script>
</head>
<body class="text-gray-800 antialiased flex flex-col min-h-screen">

    <nav class="glass-nav fixed w-full z-50 top-0 transition-all">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16 items-center">
                <div class="flex-shrink-0 flex items-center gap-2">
                    <i class="fa-solid fa-microchip text-primary text-2xl"></i>
                    <span id="nav-store-name" class="font-bold text-xl tracking-tight text-gray-900">Tech Store</span>
                </div>
                <div class="hidden md:flex space-x-8 items-center">
                    <a href="#" onclick="scrollToSection('home')" class="text-gray-600 hover:text-primary font-medium transition">Beranda</a>
                    <a href="#" onclick="scrollToSection('promo')" class="text-gray-600 hover:text-primary font-medium transition text-red-500"><i class="fa-solid fa-fire mr-1"></i> Promo</a>
                    <button onclick="openLoginModal()" class="bg-gray-100 hover:bg-gray-200 text-gray-800 px-4 py-2 rounded-full font-medium text-sm transition flex items-center gap-2">
                        <i class="fa-solid fa-user-shield"></i> Admin Login
                    </button>
                </div>
                <!-- Mobile menu button -->
                <div class="md:hidden flex items-center">
                    <button onclick="toggleMobileMenu()" class="text-gray-600 hover:text-gray-900 focus:outline-none">
                        <i class="fa-solid fa-bars text-2xl"></i>
                    </button>
                </div>
            </div>
        </div>
        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-white border-t">
            <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                <a href="#" onclick="scrollToSection('home'); toggleMobileMenu()" class="block px-3 py-2 text-base font-medium text-gray-700 hover:text-primary hover:bg-gray-50 rounded-md">Beranda</a>
                <a href="#" onclick="scrollToSection('promo'); toggleMobileMenu()" class="block px-3 py-2 text-base font-medium text-red-500 hover:text-red-600 hover:bg-gray-50 rounded-md">Promo</a>
                <button onclick="openLoginModal(); toggleMobileMenu()" class="w-full text-left block px-3 py-2 text-base font-medium text-gray-700 hover:text-primary hover:bg-gray-50 rounded-md">Admin Login</button>
            </div>
        </div>
    </nav>

    <main class="flex-grow pt-16">
        
        <!-- Slideshow Banner Section -->
        <section id="home" class="relative w-full max-w-7xl mx-auto mt-4 sm:mt-6 px-4 sm:px-6 lg:px-8">
            <div class="relative h-48 sm:h-64 md:h-80 lg:h-96 rounded-2xl overflow-hidden shadow-xl group bg-gray-900">
                <div id="slideshow-container" class="w-full h-full relative">
                    <!-- Banners injected here via JS -->
                    <div class="absolute inset-0 flex items-center justify-center text-white">
                        <i class="fa-solid fa-spinner fa-spin text-3xl"></i>
                    </div>
                </div>
                
                <!-- Slideshow Controls -->
                <button onclick="prevSlide()" class="absolute left-4 top-1/2 -translate-y-1/2 bg-black/30 hover:bg-black/50 text-white w-10 h-10 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition backdrop-blur-sm">
                    <i class="fa-solid fa-chevron-left"></i>
                </button>
                <button onclick="nextSlide()" class="absolute right-4 top-1/2 -translate-y-1/2 bg-black/30 hover:bg-black/50 text-white w-10 h-10 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition backdrop-blur-sm">
                    <i class="fa-solid fa-chevron-right"></i>
                </button>
                <div id="slideshow-indicators" class="absolute bottom-4 left-0 right-0 flex justify-center gap-2">
                    <!-- Indicators injected here -->
                </div>
            </div>
        </section>

        <!-- Promo / Catalog Section -->
        <section id="promo" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between items-end mb-8">
                <div>
                    <h2 class="text-3xl font-bold text-gray-900">Tren Teknologi <span class="text-primary">Terbaru</span></h2>
                    <p class="text-gray-500 mt-2">Temukan inovasi elektronik terkini dengan penawaran spesial.</p>
                </div>
            </div>

            <!-- Product Grid -->
            <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                <!-- Products injected via JS -->
                <div class="col-span-full text-center py-12 text-gray-400">
                    <i class="fa-solid fa-spinner fa-spin text-4xl mb-4"></i>
                    <p>Memuat katalog produk...</p>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-white border-t border-gray-200 mt-auto">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
            <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-microchip text-gray-400 text-xl"></i>
                    <span id="footer-store-name" class="font-bold text-gray-700 text-lg">Tech Store</span>
                </div>
                <div class="text-gray-500 text-sm text-center md:text-left flex flex-col items-center md:items-start">
                    <p id="footer-address"><i class="fa-solid fa-location-dot mr-1"></i> Alamat belum diatur</p>
                    <p class="mt-1">© 2026 Hak Cipta Dilindungi.</p>
                </div>
                <div class="flex space-x-4">
                    <a href="#" class="text-gray-400 hover:text-green-500 transition"><i class="fa-brands fa-whatsapp text-2xl"></i></a>
                    <a href="#" class="text-gray-400 hover:text-blue-500 transition"><i class="fa-brands fa-facebook text-2xl"></i></a>
                    <a href="#" class="text-gray-400 hover:text-pink-500 transition"><i class="fa-brands fa-instagram text-2xl"></i></a>
                </div>
            </div>
        </div>
    </footer>

    <!-- Admin Login Modal -->
    <div id="login-modal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[60] hidden flex items-center justify-center opacity-0 transition-opacity duration-300">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md p-8 transform scale-95 transition-transform duration-300 mx-4" id="login-modal-content">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold text-gray-900">Admin Login</h3>
                <button onclick="closeLoginModal()" class="text-gray-400 hover:text-gray-700 transition">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Username</label>
                    <div class="relative">
                        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                            <i class="fa-solid fa-user text-gray-400"></i>
                        </div>
                        <input type="text" id="login-username" class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary focus:border-primary outline-none transition" placeholder="Masukkan username">
                    </div>
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                    <div class="relative">
                        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                            <i class="fa-solid fa-lock text-gray-400"></i>
                        </div>
                        <input type="password" id="login-password" class="w-full pl-10 pr-10 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary focus:border-primary outline-none transition" placeholder="••••••••">
                        <button type="button" onclick="togglePassword()" class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 hover:text-gray-600">
                            <i id="eye-icon" class="fa-solid fa-eye"></i>
                        </button>
                    </div>
                </div>
                <button onclick="handleLogin()" class="w-full bg-primary hover:bg-secondary text-white font-medium py-2.5 rounded-lg transition mt-4 shadow-lg shadow-blue-500/30">
                    Masuk ke Dashboard
                </button>
            </div>
        </div>
    </div>

    <!-- Admin Dashboard Modal (Full Screen) -->
    <div id="admin-dashboard" class="fixed inset-0 bg-gray-50 z-[70] hidden overflow-y-auto">
        <!-- Dashboard Header -->
        <div class="bg-white border-b shadow-sm sticky top-0 z-10">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex justify-between items-center">
                <h2 class="text-xl font-bold text-gray-800"><i class="fa-solid fa-gauge-high text-primary mr-2"></i> Dashboard Admin</h2>
                <button onclick="closeDashboard()" class="text-gray-500 hover:text-red-500 font-medium flex items-center gap-2 bg-gray-100 hover:bg-red-50 px-4 py-2 rounded-lg transition">
                    <i class="fa-solid fa-right-from-bracket"></i> Keluar
                </button>
            </div>
        </div>
        
        <!-- Dashboard Content -->
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex flex-col lg:flex-row gap-8">
            
            <!-- Sidebar Navigation -->
            <div class="w-full lg:w-64 flex-shrink-0">
                <div class="bg-white rounded-xl shadow-sm p-4 space-y-2">
                    <button onclick="switchTab('tab-settings')" id="btn-tab-settings" class="w-full text-left px-4 py-3 rounded-lg font-medium transition flex items-center gap-3 bg-primary text-white">
                        <i class="fa-solid fa-store w-5"></i> Info Toko
                    </button>
                    <button onclick="switchTab('tab-banners')" id="btn-tab-banners" class="w-full text-left px-4 py-3 rounded-lg font-medium transition flex items-center gap-3 text-gray-600 hover:bg-gray-100">
                        <i class="fa-solid fa-images w-5"></i> Kelola Banner
                    </button>
                    <button onclick="switchTab('tab-products')" id="btn-tab-products" class="w-full text-left px-4 py-3 rounded-lg font-medium transition flex items-center gap-3 text-gray-600 hover:bg-gray-100">
                        <i class="fa-solid fa-box-open w-5"></i> Kelola Produk
                    </button>
                </div>
            </div>

            <!-- Tab Contents -->
            <div class="flex-grow">
                <!-- Tab: Settings -->
                <div id="tab-settings" class="bg-white rounded-xl shadow-sm p-6 block">
                    <h3 class="text-lg font-bold border-b pb-4 mb-4 text-gray-800">Pengaturan Informasi Toko</h3>
                    <div class="space-y-4 max-w-2xl">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Nama Toko</label>
                            <input type="text" id="admin-store-name" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Nomor WhatsApp (Contoh: 62812345678)</label>
                            <input type="text" id="admin-whatsapp" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Alamat Toko</label>
                            <textarea id="admin-address" rows="3" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none"></textarea>
                        </div>
                        <button onclick="saveStoreInfo()" class="bg-green-500 hover:bg-green-600 text-white font-medium px-6 py-2 rounded-lg transition flex items-center gap-2">
                            <i class="fa-solid fa-floppy-disk"></i> Simpan Perubahan
                        </button>
                    </div>
                </div>

                <!-- Tab: Banners -->
                <div id="tab-banners" class="bg-white rounded-xl shadow-sm p-6 hidden">
                    <div class="flex justify-between items-center border-b pb-4 mb-4">
                        <h3 class="text-lg font-bold text-gray-800">Slide Show Banners</h3>
                    </div>
                    
                    <!-- Add Banner Form -->
                    <div class="bg-gray-50 p-4 rounded-lg mb-6 border border-gray-200">
                        <h4 class="font-medium text-sm text-gray-700 mb-3"><i class="fa-solid fa-plus-circle mr-1"></i> Tambah Banner Baru</h4>
                        <div class="flex gap-4 items-start">
                            <div class="flex-grow">
                                <input type="text" id="new-banner-url" placeholder="URL Gambar Banner (Misal: https://...)" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none text-sm">
                                <p class="text-xs text-gray-500 mt-1">Rekomendasi rasio gambar 16:9 atau resolusi tinggi.</p>
                            </div>
                            <button onclick="addBanner()" class="bg-primary hover:bg-secondary text-white px-4 py-2 rounded-lg text-sm font-medium transition whitespace-nowrap">
                                Tambah
                            </button>
                        </div>
                    </div>

                    <!-- Banner List -->
                    <div id="admin-banner-list" class="space-y-3">
                        <!-- Populated by JS -->
                    </div>
                </div>

                <!-- Tab: Products -->
                <div id="tab-products" class="bg-white rounded-xl shadow-sm p-6 hidden">
                    <div class="flex justify-between items-center border-b pb-4 mb-4">
                        <h3 class="text-lg font-bold text-gray-800">Katalog Produk</h3>
                        <button onclick="openProductForm()" class="bg-primary hover:bg-secondary text-white px-4 py-2 rounded-lg text-sm font-medium transition flex items-center gap-2">
                            <i class="fa-solid fa-plus"></i> Tambah Produk
                        </button>
                    </div>

                    <!-- Product List (Table format for admin) -->
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-gray-50 text-gray-600 font-medium">
                                <tr>
                                    <th class="px-4 py-3 rounded-tl-lg">Foto</th>
                                    <th class="px-4 py-3">Nama Produk</th>
                                    <th class="px-4 py-3">Harga</th>
                                    <th class="px-4 py-3">Diskon</th>
                                    <th class="px-4 py-3 text-right rounded-tr-lg">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="admin-product-list" class="divide-y divide-gray-100">
                                <!-- Populated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Add/Edit Product Modal -->
    <div id="product-modal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[80] hidden flex items-center justify-center">
        <div class="bg-white rounded-xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto p-6 mx-4">
            <div class="flex justify-between items-center border-b pb-4 mb-4">
                <h3 id="product-modal-title" class="text-xl font-bold text-gray-800">Tambah Produk Baru</h3>
                <button onclick="closeProductForm()" class="text-gray-400 hover:text-gray-700">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <div class="space-y-4">
                <input type="hidden" id="form-product-id">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Nama Produk</label>
                        <input type="text" id="form-product-name" placeholder="Contoh: Smartphone Layar Lipat Z" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Harga (Angka saja)</label>
                        <div class="relative">
                            <span class="absolute left-3 top-2 text-gray-500">Rp</span>
                            <input type="number" id="form-product-price" placeholder="15000000" class="w-full pl-10 pr-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none">
                        </div>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Status Diskon (%) - Kosongkan jika tidak ada</label>
                        <input type="number" id="form-product-discount" placeholder="Misal: 15" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none" min="0" max="100">
                    </div>
                    <div class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-700 mb-1">URL Foto Produk</label>
                        <input type="text" id="form-product-image" placeholder="https://..." class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none">
                        <div class="mt-2 text-xs text-gray-500 flex gap-2 overflow-x-auto pb-2">
                            <span class="whitespace-nowrap">Saran gambar gratis:</span>
                            <button type="button" onclick="document.getElementById('form-product-image').value='https://images.unsplash.com/photo-1598327105666-5b89351cb31b?w=500&q=80'" class="text-blue-500 underline cursor-pointer">Kulkas</button>
                            <button type="button" onclick="document.getElementById('form-product-image').value='https://images.unsplash.com/photo-1592890288564-76628a30a657?w=500&q=80'" class="text-blue-500 underline cursor-pointer">Smartphone</button>
                            <button type="button" onclick="document.getElementById('form-product-image').value='https://images.unsplash.com/photo-1606813907291-d86efa9b94db?w=500&q=80'" class="text-blue-500 underline cursor-pointer">Konsol Game</button>
                        </div>
                    </div>
                    <div class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Deskripsi & Inovasi (Jelaskan teknologi terbarunya)</label>
                        <textarea id="form-product-desc" rows="3" placeholder="Dilengkapi dengan teknologi AI untuk menghemat listrik..." class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-primary outline-none"></textarea>
                    </div>
                </div>
                
                <div class="flex justify-end gap-3 mt-6 pt-4 border-t">
                    <button onclick="closeProductForm()" class="px-5 py-2 border rounded-lg hover:bg-gray-50 font-medium transition text-gray-700">Batal</button>
                    <button onclick="saveProduct()" class="bg-primary hover:bg-secondary text-white px-5 py-2 rounded-lg font-medium transition shadow-md">Simpan Produk</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Notification Toast Container -->
    <div id="toast-container" class="fixed bottom-4 right-4 z-[90] flex flex-col gap-2"></div>

    <script>
        // --- State Management ---
        const state = {
            storeInfo: {
                name: 'TechStore Inovasi',
                whatsapp: '6281234567890',
                address: 'Pusat Elektronik Terkini, Sukagumiwang'
            },
            banners: [],
            products: [],
            currentSlide: 0,
            slideInterval: null,
            isAdmin: false,
            // Fallback mock data structure if Firebase fails
            isUsingMockData: false
        };

        // DOM Elements setup
        const dom = {
            navStoreName: document.getElementById('nav-store-name'),
            footerStoreName: document.getElementById('footer-store-name'),
            footerAddress: document.getElementById('footer-address'),
            slideshowContainer: document.getElementById('slideshow-container'),
            slideshowIndicators: document.getElementById('slideshow-indicators'),
            productGrid: document.getElementById('product-grid'),
            toastContainer: document.getElementById('toast-container')
        };

        // --- Custom Notification (Replacing alert) ---
        function showNotification(message, type = 'success') {
            const toast = document.createElement('div');
            const bgColor = type === 'success' ? 'bg-green-500' : type === 'error' ? 'bg-red-500' : 'bg-blue-500';
            const icon = type === 'success' ? 'fa-check-circle' : type === 'error' ? 'fa-triangle-exclamation' : 'fa-info-circle';
            
            toast.className = `toast-enter ${bgColor} text-white px-4 py-3 rounded-lg shadow-lg flex items-center gap-3 max-w-sm`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-lg"></i><span class="text-sm font-medium">${message}</span>`;
            
            dom.toastContainer.appendChild(toast);
            
            setTimeout(() => {
                toast.classList.remove('toast-enter');
                toast.classList.add('toast-exit');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // --- Utility Functions ---
        const formatRupiah = (number) => {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(number);
        };
        
        function scrollToSection(id) {
            const el = document.getElementById(id);
            if (el) {
                // Adjust for fixed navbar
                const y = el.getBoundingClientRect().top + window.scrollY - 80;
                window.scrollTo({top: y, behavior: 'smooth'});
            }
        }
        
        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        }

        // --- Firebase & Data Logic ---
        async function initApp() {
            // Check if Firebase is available from the module script
            setTimeout(async () => {
                if (window.firebaseApp && window.firebaseApp.app) {
                    try {
                        await setupFirebaseAuth();
                    } catch (error) {
                        console.error("Firebase init failed, falling back to mock.", error);
                        initMockData();
                    }
                } else {
                    initMockData();
                }
            }, 500); // Small delay to let modules load
        }

        async function setupFirebaseAuth() {
            const { auth, db, appId } = window.firebaseApp;
            const { signInAnonymously, signInWithCustomToken } = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js");
            
            try {
                if (typeof __initial_auth_token !== 'undefined') {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
                console.log("Authenticated to Firebase.");
                setupFirestoreListeners(db, appId);
            } catch (error) {
                console.error("Auth error:", error);
                throw error;
            }
        }

        function setupFirestoreListeners(db, appId) {
            // Import dynamically since this is in standard script tag
            import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js").then((firestore) => {
                const { doc, onSnapshot, collection, getDocs } = firestore;

                // 1. Listen to Store Info
                const storeRef = doc(db, 'artifacts', appId, 'public', 'data', 'config', 'storeInfo');
                onSnapshot(storeRef, (docSnap) => {
                    if (docSnap.exists()) {
                        state.storeInfo = docSnap.data();
                        updateStoreUI();
                    }
                }, (err) => console.error(err));

                // 2. Listen to Banners
                const bannersCol = collection(db, 'artifacts', appId, 'public', 'data', 'banners');
                onSnapshot(bannersCol, (snapshot) => {
                    state.banners = [];
                    snapshot.forEach(doc => state.banners.push({ id: doc.id, ...doc.data() }));
                    
                    // Add default banner if empty (First load experience)
                    if (state.banners.length === 0 && state.isAdmin) {
                       // Optional: auto-seed data
                    }
                    if (state.banners.length === 0) {
                        state.banners = [
                            { id: 'default1', url: 'https://images.unsplash.com/photo-1550009158-9ebf6d250400?w=1200&q=80', active: true },
                            { id: 'default2', url: 'https://images.unsplash.com/photo-1525547719571-a2d4ac8945e2?w=1200&q=80', active: true }
                        ];
                    }
                    renderBanners();
                    renderAdminBanners();
                }, (err) => console.error(err));

                // 3. Listen to Products
                const productsCol = collection(db, 'artifacts', appId, 'public', 'data', 'products');
                onSnapshot(productsCol, (snapshot) => {
                    state.products = [];
                    snapshot.forEach(doc => state.products.push({ id: doc.id, ...doc.data() }));
                    
                    if (state.products.length === 0) {
                        // Inject Initial Prompt Requirements if DB is empty (Foldable Phone & AI Fridge)
                        seedInitialData(firestore, db, appId);
                    } else {
                        renderProducts();
                        renderAdminProducts();
                    }
                }, (err) => console.error(err));
            });
        }

        // Seed default dummy data required by prompt if firestore is empty
        async function seedInitialData(firestore, db, appId) {
            const { collection, addDoc } = firestore;
            const productsCol = collection(db, 'artifacts', appId, 'public', 'data', 'products');
            
            const seedData = [
                {
                    name: "Samsung Galaxy Z Fold 5",
                    price: 24999000,
                    discount: 10,
                    description: "Smartphone Layar Lipat revolusioner dengan engsel Flex Hinge terbaru. Multitasking tingkat dewa dalam genggaman.",
                    image: "https://images.unsplash.com/photo-1592890288564-76628a30a657?w=600&q=80"
                },
                {
                    name: "LG InstaView ThinQ AI",
                    price: 18500000,
                    discount: 0,
                    description: "Kulkas AI pintar yang bisa mendeteksi stok makanan dan menyarankan resep. Ketuk dua kali untuk melihat isi tanpa membuka pintu.",
                    image: "https://images.unsplash.com/photo-1584568694244-14fbdf83bd30?w=600&q=80"
                },
                {
                    name: "Sony PlayStation 5 Pro",
                    price: 9500000,
                    discount: 5,
                    description: "Konsol game masa depan dengan Ray Tracing 8K dan waktu loading ultra cepat menggunakan SSD custom.",
                    image: "https://images.unsplash.com/photo-1606813907291-d86efa9b94db?w=600&q=80"
                }
            ];

            try {
                for (let p of seedData) {
                    await addDoc(productsCol, p);
                }
                console.log("Seeded initial required products.");
            } catch (e) {
                console.error("Failed seeding", e);
            }
        }

        // Mock data initialization if running without Firebase Environment
        function initMockData() {
            state.isUsingMockData = true;
            state.banners = [
                { id: 'b1', url: 'https://images.unsplash.com/photo-1550009158-9ebf6d250400?w=1200&q=80' },
                { id: 'b2', url: 'https://images.unsplash.com/photo-1525547719571-a2d4ac8945e2?w=1200&q=80' }
            ];
            state.products = [
                { id: 'p1', name: "Smartphone Layar Lipat Z", price: 22000000, discount: 15, description: "Inovasi Smartphone Layar Lipat terbaru dengan ketahanan layar 2x lipat.", image: "https://images.unsplash.com/photo-1592890288564-76628a30a657?w=600&q=80" },
                { id: 'p2', name: "Smart Kulkas AI", price: 15000000, discount: 0, description: "Kulkas AI yang mengatur suhu otomatis berdasarkan kebiasaan Anda dan deteksi stok bahan makanan.", image: "https://images.unsplash.com/photo-1584568694244-14fbdf83bd30?w=600&q=80" }
            ];
            updateStoreUI();
            renderBanners();
            renderProducts();
            showNotification("Berjalan dalam mode Offline (Tanpa Database)", "info");
        }

        // --- UI Rendering Logic ---
        function updateStoreUI() {
            dom.navStoreName.textContent = state.storeInfo.name;
            dom.footerStoreName.textContent = state.storeInfo.name;
            dom.footerAddress.innerHTML = `<i class="fa-solid fa-location-dot mr-1"></i> ${state.storeInfo.address}`;
            
            // Update admin forms if they exist
            if (document.getElementById('admin-store-name')) {
                document.getElementById('admin-store-name').value = state.storeInfo.name || '';
                document.getElementById('admin-whatsapp').value = state.storeInfo.whatsapp || '';
                document.getElementById('admin-address').value = state.storeInfo.address || '';
            }
            
            document.title = state.storeInfo.name + " - Promo Elektronik";
        }

        function renderBanners() {
            if (state.banners.length === 0) return;
            
            let html = '';
            let indicatorsHtml = '';
            
            state.banners.forEach((banner, index) => {
                const isActive = index === state.currentSlide ? 'opacity-100 z-10' : 'opacity-0 z-0';
                html += `
                    <div class="absolute inset-0 transition-opacity duration-1000 ease-in-out ${isActive} slide-fade" id="slide-${index}">
                        <img src="${banner.url}" alt="Promo Banner" class="w-full h-full object-cover" onerror="this.src='https://placehold.co/1200x400/2563eb/ffffff?text=Promo+Elektronik'">
                        <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent"></div>
                    </div>
                `;
                
                const dotActive = index === state.currentSlide ? 'bg-white w-6' : 'bg-white/50 w-2 hover:bg-white/80';
                indicatorsHtml += `<button onclick="goToSlide(${index})" class="h-2 rounded-full transition-all duration-300 ${dotActive}"></button>`;
            });

            dom.slideshowContainer.innerHTML = html;
            dom.slideshowIndicators.innerHTML = indicatorsHtml;
            
            startSlideshow();
        }

        function startSlideshow() {
            clearInterval(state.slideInterval);
            if (state.banners.length > 1) {
                state.slideInterval = setInterval(nextSlide, 5000);
            }
        }
        
        function nextSlide() {
            state.currentSlide = (state.currentSlide + 1) % Math.max(1, state.banners.length);
            renderBanners();
        }
        
        function prevSlide() {
            state.currentSlide = (state.currentSlide - 1 + state.banners.length) % Math.max(1, state.banners.length);
            renderBanners();
        }
        
        function goToSlide(index) {
            state.currentSlide = index;
            renderBanners();
        }

        function generateWhatsAppLink(product) {
            const num = state.storeInfo.whatsapp || '6281234567890';
            // Clean number (remove + or leading 0 if needed, assuming valid format for now)
            const cleanNum = num.replace(/\D/g,''); 
            const formattedPrice = formatRupiah(product.price - (product.price * (product.discount || 0) / 100));
            const text = `Halo Admin ${state.storeInfo.name}, saya tertarik dengan produk:\n\n*${product.name}*\nHarga: ${formattedPrice}\n\nApakah barang ini masih tersedia?`;
            return `https://wa.me/${cleanNum}?text=${encodeURIComponent(text)}`;
        }

        function renderProducts() {
            if (state.products.length === 0) {
                dom.productGrid.innerHTML = `<div class="col-span-full text-center py-12 text-gray-500">Belum ada produk.</div>`;
                return;
            }

            let html = '';
            state.products.forEach(p => {
                const discount = parseInt(p.discount) || 0;
                const hasDiscount = discount > 0;
                const finalPrice = hasDiscount ? p.price - (p.price * discount / 100) : p.price;
                const waLink = generateWhatsAppLink(p);

                html += `
                    <div class="bg-white rounded-2xl shadow-sm border border-gray-100 overflow-hidden card-hover flex flex-col h-full group relative">
                        ${hasDiscount ? `<div class="absolute top-3 left-3 bg-red-500 text-white text-xs font-bold px-3 py-1 rounded-full z-10 animate-pulse">Diskon ${discount}%</div>` : ''}
                        
                        <div class="relative h-48 sm:h-56 overflow-hidden bg-gray-50">
                            <img src="${p.image}" alt="${p.name}" class="w-full h-full object-cover group-hover:scale-110 transition duration-500" onerror="this.src='https://placehold.co/400x400/eeeeee/999999?text=Gambar+Kosong'">
                        </div>
                        
                        <div class="p-5 flex-grow flex flex-col">
                            <h3 class="font-bold text-gray-800 text-lg mb-1 line-clamp-2">${p.name}</h3>
                            <div class="mb-3">
                                <span class="font-bold text-primary text-xl">${formatRupiah(finalPrice)}</span>
                                ${hasDiscount ? `<span class="text-sm text-gray-400 line-through ml-2">${formatRupiah(p.price)}</span>` : ''}
                            </div>
                            <p class="text-gray-500 text-sm mb-5 line-clamp-3 flex-grow">${p.description}</p>
                            
                            <a href="${waLink}" target="_blank" class="w-full bg-green-500 hover:bg-green-600 text-white font-medium py-2.5 rounded-xl transition flex items-center justify-center gap-2 mt-auto shadow-lg shadow-green-500/30">
                                <i class="fa-brands fa-whatsapp text-lg"></i> Beli via WhatsApp
                            </a>
                        </div>
                    </div>
                `;
            });
            dom.productGrid.innerHTML = html;
        }


        // Modals Logic
        function openLoginModal() {
            if (state.isAdmin) {
                openDashboard();
                return;
            }
            const modal = document.getElementById('login-modal');
            const content = document.getElementById('login-modal-content');
            modal.classList.remove('hidden');
            // Trigger reflow
            void modal.offsetWidth;
            modal.classList.remove('opacity-0');
            content.classList.remove('scale-95');
        }

        function closeLoginModal() {
            const modal = document.getElementById('login-modal');
            const content = document.getElementById('login-modal-content');
            modal.classList.add('opacity-0');
            content.classList.add('scale-95');
            setTimeout(() => {
                modal.classList.add('hidden');
            }, 300);
        }

        function togglePassword() {
            const input = document.getElementById('login-password');
            const icon = document.getElementById('eye-icon');
            if (input.type === 'password') {
                input.type = 'text';
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                input.type = 'password';
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        }

        function handleLogin() {
            const user = document.getElementById('login-username').value;
            const pass = document.getElementById('login-password').value;
            
            // Hardcoded Custom Login per prompt requirements
            if (user === 'admin' && pass === 'admin123') {
                state.isAdmin = true;
                document.getElementById('login-password').value = '';
                closeLoginModal();
                showNotification('Login berhasil! Selamat datang, Admin.');
                openDashboard();
            } else {
                showNotification('Username atau password salah!', 'error');
            }
        }


        function openDashboard() {
            if (!state.isAdmin) return;
            document.getElementById('admin-dashboard').classList.remove('hidden');
            document.body.style.overflow = 'hidden'; // Prevent background scrolling
            switchTab('tab-settings'); // Default tab
        }

        function closeDashboard() {
            document.getElementById('admin-dashboard').classList.add('hidden');
            document.body.style.overflow = 'auto';
        }

        function switchTab(tabId) {
            // Hide all tabs
            ['tab-settings', 'tab-banners', 'tab-products'].forEach(id => {
                document.getElementById(id).classList.add('hidden');
                document.getElementById('btn-' + id).classList.remove('bg-primary', 'text-white');
                document.getElementById('btn-' + id).classList.add('text-gray-600', 'hover:bg-gray-100');
            });
            
            // Show selected tab
            document.getElementById(tabId).classList.remove('hidden');
            document.getElementById('btn-' + tabId).classList.add('bg-primary', 'text-white');
            document.getElementById('btn-' + tabId).classList.remove('text-gray-600', 'hover:bg-gray-100');
            
            if (tabId === 'tab-banners') renderAdminBanners();
            if (tabId === 'tab-products') renderAdminProducts();
        }

        // --- Admin Data Handlers ---
        // 1. Settings
        async function saveStoreInfo() {
            const name = document.getElementById('admin-store-name').value.trim();
            const wa = document.getElementById('admin-whatsapp').value.trim();
            const address = document.getElementById('admin-address').value.trim();
            
            if(!name || !wa) return showNotification('Nama dan WA wajib diisi', 'error');

            const newData = { name, whatsapp: wa, address };

            if (state.isUsingMockData) {
                state.storeInfo = newData;
                updateStoreUI();
                showNotification('Tersimpan (Mode Offline)');
                return;
            }

            try {
                const { db, appId } = window.firebaseApp;
                const firestore = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js");
                const storeRef = firestore.doc(db, 'artifacts', appId, 'public', 'data', 'config', 'storeInfo');
                await firestore.setDoc(storeRef, newData, { merge: true });
                showNotification('Pengaturan Toko berhasil disimpan!');
            } catch (e) {
                console.error(e);
                showNotification('Gagal menyimpan data.', 'error');
            }
        }

        // 2. Banners
        function renderAdminBanners() {
            const list = document.getElementById('admin-banner-list');
            if(state.banners.length === 0) {
                list.innerHTML = '<p class="text-sm text-gray-500">Belum ada banner.</p>';
                return;
            }
            
            let html = '';
            state.banners.forEach(b => {
                html += `
                    <div class="flex items-center justify-between bg-white border rounded-lg p-2 gap-4">
                        <img src="${b.url}" class="h-12 w-24 object-cover rounded" onerror="this.src='https://placehold.co/100x50'">
                        <div class="flex-grow truncate text-sm text-gray-600">${b.url}</div>
                        <button onclick="deleteBanner('${b.id}')" class="text-red-500 hover:bg-red-50 p-2 rounded transition">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </div>
                `;
            });
            list.innerHTML = html;
        }

        async function addBanner() {
            const urlInput = document.getElementById('new-banner-url');
            const url = urlInput.value.trim();
            if(!url) return showNotification('Masukkan URL gambar!', 'error');

            if (state.isUsingMockData) {
                state.banners.push({ id: 'm' + Date.now(), url });
                urlInput.value = '';
                renderBanners(); renderAdminBanners();
                return showNotification('Banner ditambahkan (Offline)');
            }

            try {
                const { db, appId } = window.firebaseApp;
                const firestore = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js");
                const colRef = firestore.collection(db, 'artifacts', appId, 'public', 'data', 'banners');
                await firestore.addDoc(colRef, { url, createdAt: Date.now() });
                urlInput.value = '';
                showNotification('Banner berhasil ditambahkan');
            } catch(e) {
                console.error(e);
                showNotification('Gagal tambah banner', 'error');
            }
        }

        async function deleteBanner(id) {
            // Using custom confirm logic (omitted real confirm per prompt rules, auto-delete or custom UI. Let's auto delete for simplicity or show UI)
            if (state.isUsingMockData) {
                state.banners = state.banners.filter(b => b.id !== id);
                renderBanners(); renderAdminBanners();
                return showNotification('Banner dihapus');
            }
            
            try {
                const { db, appId } = window.firebaseApp;
                const firestore = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js");
                await firestore.deleteDoc(firestore.doc(db, 'artifacts', appId, 'public', 'data', 'banners', id));
                showNotification('Banner dihapus');
            } catch(e) {
                showNotification('Gagal hapus', 'error');
            }
        }

        // 3. Products
        function renderAdminProducts() {
            const tbody = document.getElementById('admin-product-list');
            if (state.products.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" class="px-4 py-4 text-center text-gray-500">Belum ada produk.</td></tr>';
                return;
            }

            let html = '';
            state.products.forEach(p => {
                html += `
                    <tr class="hover:bg-gray-50 transition">
                        <td class="px-4 py-3"><img src="${p.image}" class="h-12 w-12 object-cover rounded-lg border"></td>
                        <td class="px-4 py-3 font-medium text-gray-800">${p.name}</td>
                        <td class="px-4 py-3 text-primary">${formatRupiah(p.price)}</td>
                        <td class="px-4 py-3">${p.discount ? `<span class="bg-red-100 text-red-600 px-2 py-1 rounded text-xs">${p.discount}%</span>` : '-'}</td>
                        <td class="px-4 py-3 text-right">
                            <button onclick='editProduct(${JSON.stringify(p).replace(/'/g, "&#39;")})' class="text-blue-500 hover:bg-blue-50 p-2 rounded mr-1"><i class="fa-solid fa-pen"></i></button>
                            <button onclick="deleteProduct('${p.id}')" class="text-red-500 hover:bg-red-50 p-2 rounded"><i class="fa-solid fa-trash"></i></button>
                        </td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        function openProductForm() {
            document.getElementById('form-product-id').value = '';
            document.getElementById('form-product-name').value = '';
            document.getElementById('form-product-price').value = '';
            document.getElementById('form-product-discount').value = '';
            document.getElementById('form-product-image').value = '';
            document.getElementById('form-product-desc').value = '';
            
            document.getElementById('product-modal-title').textContent = 'Tambah Produk Baru';
            document.getElementById('product-modal').classList.remove('hidden');
        }

        function closeProductForm() {
            document.getElementById('product-modal').classList.add('hidden');
        }

        function editProduct(p) {
            document.getElementById('form-product-id').value = p.id;
            document.getElementById('form-product-name').value = p.name;
            document.getElementById('form-product-price').value = p.price;
            document.getElementById('form-product-discount').value = p.discount || '';
            document.getElementById('form-product-image').value = p.image;
            document.getElementById('form-product-desc').value = p.description;
            
            document.getElementById('product-modal-title').textContent = 'Edit Produk';
            document.getElementById('product-modal').classList.remove('hidden');
        }

        async function saveProduct() {
            const id = document.getElementById('form-product-id').value;
            const name = document.getElementById('form-product-name').value.trim();
            const price = parseInt(document.getElementById('form-product-price').value);
            const discount = parseInt(document.getElementById('form-product-discount').value) || 0;
            const image = document.getElementById('form-product-image').value.trim();
            const description = document.getElementById('form-product-desc').value.trim();

            if(!name || !price || !image || !description) {
                return showNotification('Semua field wajib diisi (kecuali diskon)', 'error');
            }

            const data = { name, price, discount, image, description, updatedAt: Date.now() };

            if (state.isUsingMockData) {
                if (id) {
                    const idx = state.products.findIndex(x => x.id === id);
                    if(idx !== -1) state.products[idx] = { ...data, id };
                } else {
                    state.products.push({ ...data, id: 'p' + Date.now() });
                }
                closeProductForm();
                renderProducts(); renderAdminProducts();
                return showNotification('Produk disimpan (Offline)');
            }

            try {
                const { db, appId } = window.firebaseApp;
                const firestore = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js");
                
                if (id) {
                    await firestore.updateDoc(firestore.doc(db, 'artifacts', appId, 'public', 'data', 'products', id), data);
                    showNotification('Produk diubah');
                } else {
                    const colRef = firestore.collection(db, 'artifacts', appId, 'public', 'data', 'products');
                    await firestore.addDoc(colRef, { ...data, createdAt: Date.now() });
                    showNotification('Produk ditambahkan');
                }
                closeProductForm();
            } catch(e) {
                console.error(e);
                showNotification('Gagal menyimpan produk', 'error');
            }
        }

        async function deleteProduct(id) {
            if (state.isUsingMockData) {
                state.products = state.products.filter(p => p.id !== id);
                renderProducts(); renderAdminProducts();
                return showNotification('Produk dihapus (Offline)');
            }

            try {
                const { db, appId } = window.firebaseApp;
                const firestore = await import("https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js");
                await firestore.deleteDoc(firestore.doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
                showNotification('Produk berhasil dihapus');
            } catch(e) {
                showNotification('Gagal menghapus produk', 'error');
            }
        }

        // --- Initialization ---
        document.addEventListener('DOMContentLoaded', () => {
            initApp();
        });

    </script>
</body>
</html>
