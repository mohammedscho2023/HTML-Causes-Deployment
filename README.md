<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CourseCraft | HTML Interactive Course Marketplace & LMS</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: {
                        brand: { 50: '#eef2ff', 100: '#e0e7ff', 500: '#6366f1', 600: '#4f46e5', 700: '#4338ca' }
                    }
                }
            }
        }
    </script>
</head>
<body class="h-full bg-slate-50 text-slate-800 font-sans flex flex-col antialiased">

    <!-- Global Notification Banner / Toast Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- Custom Modal for Alerts/Confirmations (Avoiding window.alert/confirm) -->
    <div id="custom-modal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-sm w-full shadow-2xl border border-slate-100 p-6 space-y-4 text-center">
            <div id="custom-modal-icon" class="w-12 h-12 rounded-full bg-indigo-100 text-indigo-600 flex items-center justify-center mx-auto text-xl font-bold">
                <i class="fa-solid fa-circle-exclamation"></i>
            </div>
            <h3 id="custom-modal-title" class="font-bold text-lg text-slate-900">Notice</h3>
            <p id="custom-modal-text" class="text-sm text-slate-600"></p>
            <div id="custom-modal-actions" class="flex gap-2 justify-center pt-2">
                <button id="custom-modal-btn-ok" onclick="closeCustomModal()" class="w-full py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-sm transition">OK</button>
            </div>
        </div>
    </div>

    <!-- Top Navigation Bar -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-40 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <!-- Logo & Brand -->
            <div class="flex items-center gap-3 cursor-pointer" onclick="switchView('marketplace')">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-indigo-600 to-violet-500 flex items-center justify-center text-white shadow-md shadow-indigo-200">
                    <i class="fa-solid fa-graduation-cap text-lg"></i>
                </div>
                <div>
                    <span class="text-xl font-bold bg-gradient-to-r from-indigo-600 to-violet-600 bg-clip-text text-transparent">CourseCraft</span>
                    <span class="block text-[10px] uppercase tracking-wider font-semibold text-slate-400">HTML LMS & Marketplace</span>
                </div>
            </div>

            <!-- Navigation Links -->
            <nav class="hidden md:flex items-center gap-6 text-sm font-medium text-slate-600">
                <button onclick="switchView('marketplace')" id="nav-marketplace" class="hover:text-indigo-600 transition py-2 border-b-2 border-indigo-600 text-indigo-600 font-semibold">Explore Courses</button>
                <button onclick="switchView('dashboard')" id="nav-dashboard" class="hover:text-indigo-600 transition py-2 border-b-2 border-transparent">My Learning</button>
                <button onclick="switchView('creator')" id="nav-creator" class="hover:text-indigo-600 transition py-2 border-b-2 border-transparent">Creator Hub</button>
            </nav>

            <!-- Role Switcher & User Profile Menu -->
            <div class="flex items-center gap-3">
                <div class="bg-slate-100 p-1 rounded-xl flex items-center text-xs font-semibold">
                    <span class="px-2 text-slate-400 hidden sm:inline"><i class="fa-solid fa-user-gear mr-1"></i> Role:</span>
                    <select id="role-selector" onchange="changeUserRole(this.value)" class="bg-white border border-slate-200 text-slate-700 py-1 px-2 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 font-medium cursor-pointer">
                        <option value="student">Student</option>
                        <option value="creator">Creator / Instructor</option>
                        <option value="admin">Administrator</option>
                    </select>
                </div>
                <div class="w-9 h-9 rounded-full bg-indigo-100 text-indigo-700 font-bold flex items-center justify-center border border-indigo-200 shadow-inner" id="user-avatar-initials">
                    JS
                </div>
            </div>
        </div>
        <!-- Mobile Navigation bar -->
        <div class="md:hidden flex border-t border-slate-100 bg-slate-50 px-4 py-2 justify-around text-xs font-semibold text-slate-600">
            <button onclick="switchView('marketplace')" id="mob-nav-marketplace" class="text-indigo-600 flex items-center gap-1 py-1"><i class="fa-solid fa-store"></i> Explore</button>
            <button onclick="switchView('dashboard')" id="mob-nav-dashboard" class="flex items-center gap-1 py-1"><i class="fa-solid fa-book-open"></i> My Learning</button>
            <button onclick="switchView('creator')" id="mob-nav-creator" class="flex items-center gap-1 py-1"><i class="fa-solid fa-wand-magic-sparkles"></i> Creator</button>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-1 overflow-y-auto">

        <!-- ================= 1. MARKETPLACE VIEW ================= -->
        <section id="view-marketplace" class="py-8 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto space-y-8">
            <!-- Hero Banner -->
            <div class="relative rounded-3xl overflow-hidden bg-gradient-to-r from-indigo-900 via-slate-900 to-indigo-950 text-white p-8 sm:p-12 shadow-xl">
                <div class="absolute inset-0 opacity-10 bg-[radial-gradient(#6366f1_1px,transparent_1px)] [background-size:16px_16px]"></div>
                <div class="relative z-10 max-w-2xl space-y-4">
                    <span class="inline-flex items-center gap-1.5 py-1 px-3 rounded-full text-xs font-semibold bg-indigo-500/20 text-indigo-300 border border-indigo-500/30">
                        <i class="fa-solid fa-bolt text-[10px]"></i> Interactive HTML5 & JS Learning
                    </span>
                    <h1 class="text-3xl sm:text-5xl font-extrabold tracking-tight leading-tight">
                        Master Coding with Interactive Code Courses
                    </h1>
                    <p class="text-slate-300 text-sm sm:text-base">
                        Explore standalone HTML/JS programs crafted by expert developers. Enroll instantly via card or mobile money and run programs directly in your secure student sandbox player.
                    </p>
                    <div class="pt-2 flex flex-wrap gap-3">
                        <button onclick="document.getElementById('course-catalog').scrollIntoView({behavior: 'smooth'})" class="px-6 py-3 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-sm shadow-lg shadow-indigo-600/30 transition transform active:scale-95">
                            Browse Catalog
                        </button>
                        <button onclick="switchView('creator')" class="px-6 py-3 rounded-xl bg-white/10 hover:bg-white/20 text-white font-semibold text-sm backdrop-blur border border-white/10 transition">
                            Sell Your Course
                        </button>
                    </div>
                </div>
            </div>

            <!-- Filter & Search Bar -->
            <div id="course-catalog" class="flex flex-col sm:flex-row justify-between items-center gap-4 bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
                <div class="relative w-full sm:w-96">
                    <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                        <i class="fa-solid fa-magnifying-glass"></i>
                    </span>
                    <input type="text" id="marketplace-search" oninput="filterCourses()" placeholder="Search HTML courses or tags..." class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                </div>
                <div class="flex items-center gap-2 w-full sm:w-auto justify-end">
                    <span class="text-xs text-slate-500 font-medium">Sort By:</span>
                    <select id="marketplace-sort" onchange="filterCourses()" class="bg-slate-50 border border-slate-200 text-slate-700 py-2 px-3 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 cursor-pointer">
                        <option value="popular">Most Popular</option>
                        <option value="price-low">Price: Low to High</option>
                        <option value="price-high">Price: High to Low</option>
                    </select>
                </div>
            </div>

            <!-- Course Catalog Grid -->
            <div>
                <h2 class="text-xl font-bold text-slate-900 mb-6 flex items-center gap-2">
                    <i class="fa-solid fa-store text-indigo-600"></i> Available HTML Courses
                </h2>
                <div id="course-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Dynamic course cards injected via JS -->
                </div>
            </div>
        </section>

        <!-- ================= 2. STUDENT LEARNING DASHBOARD ================= -->
        <section id="view-dashboard" class="hidden py-8 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-6 rounded-3xl shadow-sm border border-slate-200">
                <div>
                    <h1 class="text-2xl font-bold text-slate-900">My Enrolled Courses</h1>
                    <p class="text-sm text-slate-500">Access your purchased HTML/JS programs and interactive sandboxes.</p>
                </div>
                <div class="flex gap-2">
                    <span class="px-4 py-2 rounded-xl bg-indigo-50 text-indigo-700 font-semibold text-sm border border-indigo-100" id="stats-enrolled-count">0 Enrolled</span>
                </div>
            </div>

            <div id="enrolled-course-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Dynamically populated enrolled courses -->
            </div>
        </section>

        <!-- ================= 3. CREATOR HUB / UPLOAD DASHBOARD ================= -->
        <section id="view-creator" class="hidden py-8 px-4 sm:px-6 lg:px-8 max-w-5xl mx-auto space-y-8">
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-6 pb-6 border-b border-slate-100">
                    <div>
                        <h1 class="text-2xl font-bold text-slate-900">Creator Hub & Course Publisher</h1>
                        <p class="text-sm text-slate-500">Upload standalone HTML zip bundles or paste raw HTML/JS code to sell on CourseCraft.</p>
                    </div>
                    <button onclick="resetCreatorForm()" class="px-4 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium text-xs transition">
                        <i class="fa-solid fa-rotate-right mr-1"></i> Reset Form
                    </button>
                </div>

                <!-- Course Creation Form -->
                <form id="creator-form" onsubmit="handlePublishCourse(event)" class="space-y-6">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Course Title</label>
                            <input type="text" id="form-title" required placeholder="e.g. Interactive 3D Solar System" class="w-full px-4 py-3 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Price ($ USD)</label>
                            <input type="number" id="form-price" min="0" step="0.01" required placeholder="29.00" class="w-full px-4 py-3 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Short Description</label>
                        <input type="text" id="form-desc" required placeholder="Brief summary of what students will build..." class="w-full px-4 py-3 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Cover Image URL (Optional)</label>
                            <input type="url" id="form-image" placeholder="https://images.unsplash.com/..." class="w-full px-4 py-3 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Category / Tag</label>
                            <select id="form-category" class="w-full px-4 py-3 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white cursor-pointer">
                                <option value="Canvas & Games">Canvas & Games</option>
                                <option value="Data Viz & D3">Data Viz & D3</option>
                                <option value="Web Apps & Tools">Web Apps & Tools</option>
                                <option value="Animations & UI">Animations & UI</option>
                            </select>
                        </div>
                    </div>

                    <!-- Ingestion Mode Tabs -->
                    <div class="border-t border-slate-100 pt-6">
                        <div class="flex items-center justify-between mb-4">
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600">Course Code Source (.Zip Mock or Raw HTML)</label>
                            <div class="flex bg-slate-100 p-1 rounded-xl text-xs font-medium">
                                <button type="button" onclick="setIngestTab('zip')" id="tab-btn-zip" class="px-3 py-1.5 rounded-lg bg-white shadow text-indigo-600 transition">Simulate Zip Upload</button>
                                <button type="button" onclick="setIngestTab('code')" id="tab-btn-code" class="px-3 py-1.5 rounded-lg text-slate-600 transition">Paste HTML / JS Code</button>
                            </div>
                        </div>

                        <!-- Zip Simulator Panel -->
                        <div id="ingest-panel-zip" class="space-y-3">
                            <div class="border-2 border-dashed border-slate-300 hover:border-indigo-500 transition rounded-2xl p-8 text-center bg-slate-50 cursor-pointer" onclick="document.getElementById('mock-zip-file').click()">
                                <i class="fa-solid fa-file-zipper text-4xl text-indigo-500 mb-3"></i>
                                <p class="text-sm font-semibold text-slate-700">Click to upload your .zip course package</p>
                                <p class="text-xs text-slate-400 mt-1">Must contain index.html, CSS, and JS assets</p>
                                <input type="file" id="mock-zip-file" accept=".zip" class="hidden" onchange="handleZipUpload(event)">
                            </div>
                            <div id="zip-uploaded-info" class="hidden text-xs text-emerald-600 font-semibold bg-emerald-50 p-3 rounded-xl border border-emerald-200 flex items-center justify-between">
                                <span id="zip-file-name"><i class="fa-solid fa-circle-check mr-1"></i> course-package.zip loaded successfully</span>
                                <span class="text-slate-400 cursor-pointer hover:text-red-500" onclick="clearZipUpload()"><i class="fa-solid fa-xmark"></i></span>
                            </div>
                        </div>

                        <!-- Raw HTML Code Panel -->
                        <div id="ingest-panel-code" class="hidden space-y-3">
                            <p class="text-xs text-slate-500">Paste self-contained HTML code including CSS and JavaScript tags below:</p>
                            <textarea id="form-html-code" rows="10" placeholder="<!DOCTYPE html>&#10;<html>&#10;<head><style>body { background: #111; color: #fff; font-family: sans-serif; text-align: center; padding-top: 50px; }</style></head>&#10;<body>&#10;  <h1>My Interactive Course</h1>&#10;  <button onclick='customAlert(\'Hello!\')'>Click Me</button>&#10;</body>&#10;</html>" class="w-full font-mono text-xs p-4 rounded-xl border border-slate-200 bg-slate-900 text-slate-100 focus:outline-none focus:ring-2 focus:ring-indigo-500"></textarea>
                        </div>
                    </div>

                    <div class="pt-4 flex justify-end">
                        <button type="submit" class="px-8 py-3.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-sm shadow-lg shadow-indigo-600/30 transition transform active:scale-95 flex items-center gap-2">
                            <i class="fa-solid fa-cloud-arrow-up"></i> Publish Course & List on Marketplace
                        </button>
                    </div>
                </form>
            </div>

            <!-- Instructor Analytics / Published Courses Table -->
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200 space-y-4">
                <h2 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                    <i class="fa-solid fa-chart-line text-indigo-600"></i> My Published Courses & Sales
                </h2>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm text-slate-600">
                        <thead class="bg-slate-50 text-slate-400 font-semibold uppercase text-[10px] tracking-wider border-b border-slate-200">
                            <tr>
                                <th class="py-3 px-4">Course Name</th>
                                <th class="py-3 px-4">Category</th>
                                <th class="py-3 px-4">Price</th>
                                <th class="py-3 px-4">Enrolled Students</th>
                                <th class="py-3 px-4">Revenue</th>
                                <th class="py-3 px-4 text-right">Actions</th>
                            </tr>
                        </thead>
                        <tbody id="creator-courses-table" class="divide-y divide-slate-100">
                            <!-- Injected dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

    </main>

    <!-- ================= 4. MODALS (Checkout & Course Player) ================= -->

    <!-- Checkout Modal with Support for Credit Card & Mobile Phone / Mobile Money -->
    <div id="modal-checkout" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-md w-full shadow-2xl border border-slate-100 overflow-hidden transform transition-all animate-in fade-in zoom-in-95 duration-200 max-h-[90vh] overflow-y-auto">
            <div class="bg-gradient-to-r from-indigo-600 to-violet-600 p-6 text-white flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-shield-halved text-2xl"></i>
                    <span class="font-bold text-lg">Secure Enrollment</span>
                </div>
                <button onclick="closeCheckoutModal()" class="w-8 h-8 rounded-full bg-white/10 hover:bg-white/20 flex items-center justify-center text-white transition"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="p-6 space-y-6">
                <div id="checkout-item-summary" class="bg-slate-50 p-4 rounded-2xl border border-slate-200 flex items-center gap-4">
                    <img id="checkout-modal-img" src="https://placehold.co/80x80/6366f1/ffffff?text=Course" class="w-16 h-16 rounded-xl object-cover shadow-sm">
                    <div>
                        <h4 id="checkout-modal-title" class="font-bold text-slate-800 text-sm">Course Title</h4>
                        <p class="text-xs text-slate-500 mt-0.5">Instant Access • Lifetime Sandbox</p>
                        <span id="checkout-modal-price" class="inline-block mt-2 font-extrabold text-indigo-600 text-base">$29.00</span>
                    </div>
                </div>

                <!-- Payment Method Tabs -->
                <div>
                    <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Select Payment Method</label>
                    <div class="grid grid-cols-2 gap-2 p-1 bg-slate-100 rounded-2xl text-xs font-semibold">
                        <button type="button" onclick="setPaymentMethod('card')" id="pay-tab-card" class="py-2.5 rounded-xl bg-white text-indigo-600 shadow-sm transition flex items-center justify-center gap-2"><i class="fa-solid fa-credit-card"></i> Card / Stripe</button>
                        <button type="button" onclick="setPaymentMethod('phone')" id="pay-tab-phone" class="py-2.5 rounded-xl text-slate-600 transition flex items-center justify-center gap-2"><i class="fa-solid fa-mobile-screen-button"></i> Phone / Mobile Money</button>
                    </div>
                </div>

                <!-- Card Payment Form -->
                <div id="payment-panel-card" class="space-y-4">
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1.5">Cardholder Name</label>
                        <input type="text" id="card-name" value="Alex Student" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </div>
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1.5">Card Number</label>
                        <div class="relative">
                            <input type="text" id="card-num" value="4242 •••• •••• 4242" class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 font-mono">
                            <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400"><i class="fa-solid fa-credit-card"></i></span>
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1.5">Expiry</label>
                            <input type="text" value="12/28" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 font-mono">
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1.5">CVC</label>
                            <input type="text" value="888" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 font-mono">
                        </div>
                    </div>
                </div>

                <!-- Phone / Mobile Money Payment Form -->
                <div id="payment-panel-phone" class="hidden space-y-4">
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1.5">Select Mobile Operator</label>
                        <select id="phone-operator" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white font-medium cursor-pointer">
                            <option value="Telebirr">Telebirr (Mobile Money)</option>
                            <option value="M-Pesa">M-Pesa</option>
                            <option value="Chapa Mobile">Chapa Mobile Wallet</option>
                            <option value="MTN Momo">MTN Mobile Money</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1.5">Phone Number for Payment</label>
                        <div class="relative">
                            <input type="tel" id="phone-number-input" placeholder="+251 911 223 344" class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 font-mono">
                            <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400"><i class="fa-solid fa-phone"></i></span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-1">A payment USSD prompt or SMS confirmation will be simulated instantly.</p>
                    </div>
                </div>

                <button onclick="processPayment()" class="w-full py-3.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-bold text-sm shadow-lg shadow-indigo-600/30 transition transform active:scale-95 flex items-center justify-center gap-2">
                    <i class="fa-solid fa-lock text-xs"></i> Complete Secure Enrollment
                </button>
            </div>
        </div>
    </div>

    <!-- Sandboxed Course Player Modal -->
    <div id="modal-player" class="fixed inset-0 z-50 bg-slate-950/90 backdrop-blur-md hidden flex flex-col">
        <!-- Player Header Bar -->
        <div class="bg-slate-900 border-b border-slate-800 px-6 py-4 flex items-center justify-between text-white">
            <div class="flex items-center gap-3">
                <button onclick="closeCoursePlayer()" class="w-9 h-9 rounded-xl bg-slate-800 hover:bg-slate-700 flex items-center justify-center text-slate-300 transition">
                    <i class="fa-solid fa-arrow-left"></i>
                </button>
                <div>
                    <h3 id="player-course-title" class="font-bold text-base">Course Player</h3>
                    <span class="text-xs text-emerald-400 font-mono flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> Sandboxed iframe active</span>
                </div>
            </div>
            <div class="flex items-center gap-3">
                <button onclick="togglePlayerCodeView()" id="btn-toggle-code" class="px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-semibold transition flex items-center gap-1.5">
                    <i class="fa-solid fa-code"></i> Inspect HTML Code
                </button>
                <button onclick="closeCoursePlayer()" class="w-9 h-9 rounded-xl bg-red-500/20 hover:bg-red-500/30 text-red-400 flex items-center justify-center transition">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
        </div>

        <!-- Player Body (Split view or full sandbox) -->
        <div class="flex-1 flex flex-col md:flex-row overflow-hidden">
            <!-- Sandbox iframe container -->
            <div id="player-frame-container" class="flex-1 bg-white relative flex flex-col">
                <iframe id="sandbox-iframe" class="w-full h-full border-0" sandbox="allow-scripts allow-same-origin"></iframe>
            </div>
            <!-- Code Inspector Drawer (Hidden by default) -->
            <div id="player-code-inspector" class="hidden w-full md:w-1/2 bg-slate-900 border-l border-slate-800 flex flex-col">
                <div class="bg-slate-950 px-4 py-2.5 text-xs font-bold uppercase tracking-wider text-slate-400 border-b border-slate-800 flex justify-between items-center">
                    <span>Source HTML / JS Code</span>
                    <button onclick="copyPlayerCode()" class="text-indigo-400 hover:text-indigo-300 font-medium lowercase text-xs"><i class="fa-regular fa-copy mr-1"></i> Copy Code</button>
                </div>
                <textarea id="player-code-textarea" readonly class="w-full flex-1 bg-slate-900 text-slate-200 font-mono text-xs p-4 resize-none focus:outline-none"></textarea>
            </div>
        </div>
    </div>


    <!-- ================= JAVASCRIPT APPLICATION LOGIC ================= -->
    <script>
        // Storage keys for unified persistence across phone and desktop
        const STORAGE_KEY_COURSES = 'coursecraft_all_courses_v2';
        const STORAGE_KEY_ENROLLED = 'coursecraft_enrolled_courses_v2';

        // Default Mock Courses Catalogue
        const DEFAULT_COURSES = [
            {
                id: 'c1',
                title: 'Interactive 3D Solar System with Three.js',
                desc: 'Build an orbiting 3D solar system with lighting, textures, and interactive planet click events.',
                price: 29.00,
                image: 'https://images.unsplash.com/photo-1614728894747-a83421e2b9c9?w=600&auto=format&fit=crop&q=80',
                category: 'Canvas & Games',
                students: 342,
                creator: 'Dr. Stella Orbit',
                htmlCode: `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>3D Solar System Sandbox</title>
    <style>
        body { margin: 0; background: #05050a; color: #fff; font-family: sans-serif; overflow: hidden; }
        #info { position: absolute; top: 20px; left: 20px; z-index: 10; background: rgba(0,0,0,0.6); padding: 15px; border-radius: 12px; backdrop-filter: blur(5px); border: 1px solid rgba(255,255,255,0.1); }
        h1 { margin: 0 0 5px 0; font-size: 18px; color: #818cf8; }
        p { margin: 0; font-size: 12px; color: #94a3b8; }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"><\\/script>
</head>
<body>
    <div id="info">
        <h1>Solar System Simulator</h1>
        <p>Interactive Three.js Sandbox Course</p>
    </div>
    <script>
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        const sunGeo = new THREE.SphereGeometry(2, 32, 32);
        const sunMat = new THREE.MeshBasicMaterial({ color: 0xffaa00 });
        const sun = new THREE.Mesh(sunGeo, sunMat);
        scene.add(sun);

        const earthGeo = new THREE.SphereGeometry(0.8, 32, 32);
        const earthMat = new THREE.MeshStandardMaterial({ color: 0x3b82f6, roughness: 0.4 });
        const earth = new THREE.Mesh(earthGeo, earthMat);
        scene.add(earth);

        const light = new THREE.PointLight(0xffffff, 2, 50);
        light.position.set(0, 0, 0);
        scene.add(light);
        scene.add(new THREE.AmbientLight(0x333333));

        camera.position.z = 8;

        let angle = 0;
        function animate() {
            requestAnimationFrame(animate);
            angle += 0.01;
            earth.position.x = Math.cos(angle) * 4.5;
            earth.position.z = Math.sin(angle) * 4.5;
            earth.rotation.y += 0.02;
            renderer.render(scene, camera);
        }
        animate();

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    <\\/script>
</body>
</html>`
            },
            {
                id: 'c2',
                title: 'Data Visualization Masterclass with D3.js',
                desc: 'Create real-time dynamic SVG charts, interactive tooltips, and responsive dashboards from raw JSON data.',
                price: 39.00,
                image: 'https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=600&auto=format&fit=crop&q=80',
                category: 'Data Viz & D3',
                students: 189,
                creator: 'Alex DataViz',
                htmlCode: `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>D3.js Bar Chart Masterclass</title>
    <script src="https://d3js.org/d3.v7.min.js"><\\/script>
    <style>
        body { font-family: sans-serif; background: #f8fafc; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; margin: 0; }
        .card { background: white; padding: 30px; border-radius: 20px; box-shadow: 0 10px 25px rgba(0,0,0,0.05); text-align: center; }
        .bar { fill: #6366f1; transition: fill 0.3s; rx: 4px; }
        .bar:hover { fill: #4f46e5; }
        h2 { margin-top: 0; color: #1e293b; }
    </style>
</head>
<body>
    <div class="card">
        <h2>Active User Growth (2026)</h2>
        <div id="chart"></div>
    </div>
    <script>
        const data = [35, 55, 80, 120, 160, 210, 280];
        const width = 400, height = 200, barWidth = width / data.length;

        const svg = d3.select("#chart")
          .append("svg")
          .attr("width", width)
          .attr("height", height);

        svg.selectAll("rect")
            .data(data)
            .enter()
            .append("rect")
            .attr("class", "bar")
            .attr("x", (d, i) => i * barWidth + 5)
            .attr("y", d => height - d * 0.6)
            .attr("width", barWidth - 10)
            .attr("height", d => d * 0.6);
    <\\/script>
</body>
</html>`
            },
            {
                id: 'c3',
                title: 'Retro Arcade Game Engine in Pure JS',
                desc: 'Program a fully playable brick breaker arcade game with canvas collisions, score tracking, and sound effects.',
                price: 19.00,
                image: 'https://images.unsplash.com/photo-1550745165-9bc0b252726f?w=600&auto=format&fit=crop&q=80',
                category: 'Canvas & Games',
                students: 512,
                creator: 'Retro Pixel Master',
                htmlCode: `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Retro Brick Breaker</title>
    <style>
        body { background: #0f172a; color: #fff; font-family: sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; margin: 0; }
        canvas { background: #1e293b; border-radius: 12px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        h1 { margin-bottom: 10px; font-size: 20px; color: #38bdf8; }
    </style>
</head>
<body>
    <h1>Arcade Brick Breaker</h1>
    <canvas id="gameCanvas" width="480" height="320"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        let x = canvas.width/2, y = canvas.height-30, dx = 2, dy = -2, ballRadius = 10;
        let paddleHeight = 10, paddleWidth = 75, paddleX = (canvas.width-paddleWidth)/2;
        let rightPressed = false, leftPressed = false;
        let score = 0;

        document.addEventListener("keydown", e => { if(e.key === "Right" || e.key === "ArrowRight") rightPressed = true; else if(e.key === "Left" || e.key === "ArrowLeft") leftPressed = true; });
        document.addEventListener("keyup", e => { if(e.key === "Right" || e.key === "ArrowRight") rightPressed = false; else if(e.key === "Left" || e.key === "ArrowLeft") leftPressed = false; });
        // Touch controls support for mobile
        document.addEventListener("touchmove", e => {
            let touchX = e.touches[0].clientX - canvas.getBoundingClientRect().left;
            paddleX = touchX - paddleWidth/2;
            if(paddleX < 0) paddleX = 0;
            if(paddleX > canvas.width - paddleWidth) paddleX = canvas.width - paddleWidth;
        });

        function drawBall() {
            ctx.beginPath(); ctx.arc(x, y, ballRadius, 0, Math.PI*2); ctx.fillStyle = "#38bdf8"; ctx.fill(); ctx.closePath();
        }
        function drawPaddle() {
            ctx.beginPath(); ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight); ctx.fillStyle = "#818cf8"; ctx.fill(); ctx.closePath();
        }
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBall(); drawPaddle();
            ctx.fillStyle = "#fff"; ctx.font = "14px sans-serif"; ctx.fillText("Score: " + score, 20, 20);

            if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) dx = -dx;
            if(y + dy < ballRadius) dy = -dy;
            else if(y + dy > canvas.height-ballRadius) {
                if(x > paddleX && x < paddleX + paddleWidth) { dy = -dy; score++; }
                else { document.location.reload(); }
            }

            if(rightPressed && paddleX < canvas.width-paddleWidth) paddleX += 7;
            else if(leftPressed && paddleX > 0) paddleX -= 7;

            x += dx; y += dy;
            requestAnimationFrame(draw);
        }
        draw();
    <\\/script>
</body>
</html>`
            }
        ];

        // Application State
        let currentUserRole = 'student';
        let currentActiveView = 'marketplace';
        let allCourses = [];
        let enrolledCourseIds = [];
        let selectedCourseForCheckout = null;
        let selectedPaymentMethod = 'card';
        let pendingZipData = null;

        // Initialize application on load
        window.addEventListener('DOMContentLoaded', () => {
            loadStoredData();
            renderMarketplace();
            renderDashboard();
            renderCreatorTable();
        });

        // Load & Save Storage helpers
        function loadStoredData() {
            try {
                const storedCourses = localStorage.getItem(STORAGE_KEY_COURSES);
                if (storedCourses) {
                    allCourses = JSON.parse(storedCourses);
                } else {
                    allCourses = DEFAULT_COURSES;
                    localStorage.setItem(STORAGE_KEY_COURSES, JSON.stringify(allCourses));
                }

                const storedEnrolled = localStorage.getItem(STORAGE_KEY_ENROLLED);
                if (storedEnrolled) {
                    enrolledCourseIds = JSON.parse(storedEnrolled);
                } else {
                    // Default enroll in first course for instant trial
                    enrolledCourseIds = ['c1'];
                    localStorage.setItem(STORAGE_KEY_ENROLLED, JSON.stringify(enrolledCourseIds));
                }
            } catch (e) {
                console.error("Storage error:", e);
                allCourses = DEFAULT_COURSES;
                enrolledCourseIds = ['c1'];
            }
        }

        function saveAllData() {
            localStorage.setItem(STORAGE_KEY_COURSES, JSON.stringify(allCourses));
            localStorage.setItem(STORAGE_KEY_ENROLLED, JSON.stringify(enrolledCourseIds));
        }

        // View Switcher
        function switchView(viewName) {
            currentActiveView = viewName;
            ['marketplace', 'dashboard', 'creator'].forEach(v => {
                const el = document.getElementById('view-' + v);
                if (el) el.classList.add('hidden');
                const navBtn = document.getElementById('nav-' + v);
                if (navBtn) {
                    navBtn.classList.remove('border-indigo-600', 'text-indigo-600', 'font-semibold');
                    navBtn.classList.add('border-transparent');
                }
            });

            const targetView = document.getElementById('view-' + viewName);
            if (targetView) targetView.classList.remove('hidden');

            const activeNav = document.getElementById('nav-' + viewName);
            if (activeNav) {
                activeNav.classList.remove('border-transparent');
                activeNav.classList.add('border-indigo-600', 'text-indigo-600', 'font-semibold');
            }

            if (viewName === 'marketplace') renderMarketplace();
            if (viewName === 'dashboard') renderDashboard();
            if (viewName === 'creator') renderCreatorTable();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Role Selector
        function changeUserRole(role) {
            currentUserRole = role;
            showToast('Switched role to ' + role.toUpperCase(), 'info');
        }

        // Render Marketplace Course Cards
        function renderMarketplace() {
            const grid = document.getElementById('course-grid');
            if (!grid) return;

            const searchQuery = (document.getElementById('marketplace-search')?.value || '').toLowerCase();
            const sortBy = document.getElementById('marketplace-sort')?.value || 'popular';

            let filtered = allCourses.filter(c => 
                c.title.toLowerCase().includes(searchQuery) || 
                c.desc.toLowerCase().includes(searchQuery) ||
                c.category.toLowerCase().includes(searchQuery)
            );

            if (sortBy === 'popular') {
                filtered.sort((a, b) => (b.students || 0) - (a.students || 0));
            } else if (sortBy === 'price-low') {
                filtered.sort((a, b) => a.price - b.price);
            } else if (sortBy === 'price-high') {
                filtered.sort((a, b) => b.price - a.price);
            }

            if (filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-full py-12 text-center text-slate-400"><i class="fa-solid fa-folder-open text-4xl mb-3"></i><p>No courses found matching your query.</p></div>`;
                return;
            }

            grid.innerHTML = filtered.map(course => {
                const isEnrolled = enrolledCourseIds.includes(course.id);
                const priceFormatted = course.price === 0 ? 'Free' : '$' + course.price.toFixed(2);
                return `
                    <div class="bg-white rounded-3xl overflow-hidden border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col group">
                        <div class="relative h-48 overflow-hidden bg-slate-100">
                            <img src="${course.image || 'https://placehold.co/600x400/6366f1/ffffff?text=Course'}" alt="${course.title}" class="w-full h-full object-cover group-hover:scale-105 transition duration-500" onerror="this.src='https://placehold.co/600x400/6366f1/ffffff?text=Course'">
                            <span class="absolute top-3 left-3 bg-slate-900/80 backdrop-blur text-white text-[10px] font-bold uppercase tracking-wider px-3 py-1 rounded-full">
                                ${course.category}
                            </span>
                            <span class="absolute top-3 right-3 bg-white text-slate-900 font-extrabold text-sm px-3 py-1 rounded-full shadow-md">
                                ${priceFormatted}
                            </span>
                        </div>
                        <div class="p-6 flex-1 flex flex-col justify-between space-y-4">
                            <div class="space-y-2">
                                <div class="flex items-center justify-between text-xs text-slate-400 font-medium">
                                    <span><i class="fa-solid fa-user-graduate mr-1"></i> ${course.students || 120} students</span>
                                    <span><i class="fa-solid fa-chalkboard-user mr-1"></i> ${course.creator || 'Instructor'}</span>
                                </div>
                                <h3 class="font-bold text-slate-900 text-base line-clamp-1 group-hover:text-indigo-600 transition">${course.title}</h3>
                                <p class="text-xs text-slate-500 line-clamp-2 leading-relaxed">${course.desc}</p>
                            </div>
                            <div class="pt-2 border-t border-slate-100 flex items-center justify-between gap-3">
                                ${isEnrolled ? `
                                    <button onclick="launchCoursePlayer('${course.id}')" class="w-full py-2.5 rounded-xl bg-emerald-600 hover:bg-emerald-500 text-white font-semibold text-xs transition flex items-center justify-center gap-1.5 shadow-sm">
                                        <i class="fa-solid fa-play"></i> Launch Sandbox
                                    </button>
                                ` : `
                                    <button onclick="initiateEnrollment('${course.id}')" class="w-full py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-xs transition flex items-center justify-center gap-1.5 shadow-lg shadow-indigo-600/20">
                                        <i class="fa-solid fa-cart-shopping"></i> Enroll / Buy Now
                                    </button>
                                `}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function filterCourses() {
            renderMarketplace();
        }

        // Render Student Dashboard
        function renderDashboard() {
            const grid = document.getElementById('enrolled-course-grid');
            const countBadge = document.getElementById('stats-enrolled-count');
            if (!grid) return;

            const enrolled = allCourses.filter(c => enrolledCourseIds.includes(c.id));
            if (countBadge) countBadge.textContent = enrolled.length + ' Enrolled';

            if (enrolled.length === 0) {
                grid.innerHTML = `
                    <div class="col-span-full py-16 text-center space-y-4 bg-white rounded-3xl border border-slate-200">
                        <div class="w-16 h-16 rounded-full bg-indigo-50 text-indigo-600 flex items-center justify-center mx-auto text-2xl">
                            <i class="fa-solid fa-graduation-cap"></i>
                        </div>
                        <h3 class="font-bold text-lg text-slate-800">No Enrolled Courses Yet</h3>
                        <p class="text-xs text-slate-500 max-w-sm mx-auto">Explore our catalog of interactive HTML, Three.js, and D3 courses and start learning today!</p>
                        <button onclick="switchView('marketplace')" class="px-6 py-2.5 rounded-xl bg-indigo-600 text-white text-xs font-semibold shadow-md">Explore Catalog</button>
                    </div>
                `;
                return;
            }

            grid.innerHTML = enrolled.map(course => `
                <div class="bg-white rounded-3xl overflow-hidden border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col">
                    <div class="relative h-40 overflow-hidden bg-slate-100">
                        <img src="${course.image || 'https://placehold.co/600x400/6366f1/ffffff?text=Course'}" alt="${course.title}" class="w-full h-full object-cover">
                        <span class="absolute top-3 left-3 bg-emerald-600 text-white text-[10px] font-bold uppercase tracking-wider px-3 py-1 rounded-full shadow">
                            Enrolled & Active
                        </span>
                    </div>
                    <div class="p-6 flex-1 flex flex-col justify-between space-y-4">
                        <div class="space-y-2">
                            <h3 class="font-bold text-slate-900 text-base line-clamp-1">${course.title}</h3>
                            <p class="text-xs text-slate-500 line-clamp-2">${course.desc}</p>
                        </div>
                        <div class="pt-2 border-t border-slate-100">
                            <button onclick="launchCoursePlayer('${course.id}')" class="w-full py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-xs transition flex items-center justify-center gap-1.5 shadow-md shadow-indigo-600/30">
                                <i class="fa-solid fa-play"></i> Run Sandbox Player
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Enrollment & Checkout Flow
        function initiateEnrollment(courseId) {
            const course = allCourses.find(c => c.id === courseId);
            if (!course) return;

            // If free, enroll instantly
            if (course.price === 0) {
                if (!enrolledCourseIds.includes(courseId)) {
                    enrolledCourseIds.push(courseId);
                    course.students = (course.students || 0) + 1;
                    saveAllData();
                    showToast('Successfully enrolled in ' + course.title + '!', 'success');
                    renderMarketplace();
                    renderDashboard();
                } else {
                    showCustomModal('Already Enrolled', 'You already have access to this course.');
                }
                return;
            }

            // Open secure checkout modal
            selectedCourseForCheckout = course;
            document.getElementById('checkout-modal-title').textContent = course.title;
            document.getElementById('checkout-modal-price').textContent = '$' + course.price.toFixed(2);
            document.getElementById('checkout-modal-img').src = course.image || 'https://placehold.co/80x80/6366f1/ffffff?text=Course';
            
            // Reset modal inputs
            setPaymentMethod('card');
            document.getElementById('modal-checkout').classList.remove('hidden');
        }

        function closeCheckoutModal() {
            document.getElementById('modal-checkout').classList.add('hidden');
            selectedCourseForCheckout = null;
        }

        function setPaymentMethod(method) {
            selectedPaymentMethod = method;
            const btnCard = document.getElementById('pay-tab-card');
            const btnPhone = document.getElementById('pay-tab-phone');
            const panelCard = document.getElementById('payment-panel-card');
            const panelPhone = document.getElementById('payment-panel-phone');

            if (method === 'card') {
                btnCard.className = 'py-2.5 rounded-xl bg-white text-indigo-600 shadow-sm transition flex items-center justify-center gap-2 font-semibold';
                btnPhone.className = 'py-2.5 rounded-xl text-slate-600 transition flex items-center justify-center gap-2 font-semibold';
                panelCard.classList.remove('hidden');
                panelPhone.classList.add('hidden');
            } else {
                btnPhone.className = 'py-2.5 rounded-xl bg-white text-indigo-600 shadow-sm transition flex items-center justify-center gap-2 font-semibold';
                btnCard.className = 'py-2.5 rounded-xl text-slate-600 transition flex items-center justify-center gap-2 font-semibold';
                panelPhone.classList.remove('hidden');
                panelCard.classList.add('hidden');
            }
        }

        function processPayment() {
            if (!selectedCourseForCheckout) return;

            if (selectedPaymentMethod === 'phone') {
                const phoneInput = document.getElementById('phone-number-input').value.trim();
                const operator = document.getElementById('phone-operator').value;
                if (!phoneInput) {
                    showCustomModal('Phone Required', 'Please enter a valid phone number for mobile money payment.');
                    return;
                }
                showToast(`Sending USSD prompt to ${operator} (${phoneInput})...`, 'info');
            }

            // Complete enrollment success simulation
            setTimeout(() => {
                const courseId = selectedCourseForCheckout.id;
                if (!enrolledCourseIds.includes(courseId)) {
                    enrolledCourseIds.push(courseId);
                    selectedCourseForCheckout.students = (selectedCourseForCheckout.students || 0) + 1;
                    saveAllData();
                }
                closeCheckoutModal();
                showToast(`Payment successful! Enrolled in ${selectedCourseForCheckout.title}`, 'success');
                renderMarketplace();
                renderDashboard();
                renderCreatorTable();
            }, 1000);
        }

        // Sandboxed Course Player
        function launchCoursePlayer(courseId) {
            const course = allCourses.find(c => c.id === courseId);
            if (!course) return;

            document.getElementById('player-course-title').textContent = course.title;
            document.getElementById('player-code-textarea').value = course.htmlCode || '<!-- No code available -->';

            const iframe = document.getElementById('sandbox-iframe');
            iframe.srcdoc = course.htmlCode || '<html><body><h3>No content</h3></body></html>';

            document.getElementById('modal-player').classList.remove('hidden');
        }

        function closeCoursePlayer() {
            document.getElementById('modal-player').classList.add('hidden');
            const iframe = document.getElementById('sandbox-iframe');
            iframe.srcdoc = '';
        }

        function togglePlayerCodeView() {
            const inspector = document.getElementById('player-code-inspector');
            const btn = document.getElementById('btn-toggle-code');
            if (inspector.classList.contains('hidden')) {
                inspector.classList.remove('hidden');
                btn.innerHTML = '<i class="fa-solid fa-eye"></i> Hide Code';
                btn.classList.add('bg-indigo-600', 'text-white');
            } else {
                inspector.classList.add('hidden');
                btn.innerHTML = '<i class="fa-solid fa-code"></i> Inspect HTML Code';
                btn.classList.remove('bg-indigo-600', 'text-white');
            }
        }

        function copyPlayerCode() {
            const codeArea = document.getElementById('player-code-textarea');
            codeArea.select();
            document.execCommand('copy');
            showToast('HTML code copied to clipboard!', 'success');
        }

        // Creator Hub Upload & Management
        function setIngestTab(tab) {
            const btnZip = document.getElementById('tab-btn-zip');
            const btnCode = document.getElementById('tab-btn-code');
            const panelZip = document.getElementById('ingest-panel-zip');
            const panelCode = document.getElementById('ingest-panel-code');

            if (tab === 'zip') {
                btnZip.className = 'px-3 py-1.5 rounded-lg bg-white shadow text-indigo-600 transition font-semibold';
                btnCode.className = 'px-3 py-1.5 rounded-lg text-slate-600 transition font-medium';
                panelZip.classList.remove('hidden');
                panelCode.classList.add('hidden');
            } else {
                btnCode.className = 'px-3 py-1.5 rounded-lg bg-white shadow text-indigo-600 transition font-semibold';
                btnZip.className = 'px-3 py-1.5 rounded-lg text-slate-600 transition font-medium';
                panelCode.classList.remove('hidden');
                panelZip.classList.add('hidden');
            }
        }

        function handleZipUpload(event) {
            const file = event.target.files[0];
            if (file) {
                pendingZipData = file.name;
                document.getElementById('zip-file-name').innerHTML = `<i class="fa-solid fa-circle-check mr-1"></i> ${file.name} loaded successfully`;
                document.getElementById('zip-uploaded-info').classList.remove('hidden');
                showToast('Course zip package simulated successfully!', 'success');
            }
        }

        function clearZipUpload() {
            pendingZipData = null;
            document.getElementById('mock-zip-file').value = '';
            document.getElementById('zip-uploaded-info').classList.add('hidden');
        }

        function handlePublishCourse(e) {
            e.preventDefault();
            const title = document.getElementById('form-title').value.trim();
            const price = parseFloat(document.getElementById('form-price').value) || 0;
            const desc = document.getElementById('form-desc').value.trim();
            const image = document.getElementById('form-image').value.trim() || 'https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5?w=600&auto=format&fit=crop&q=80';
            const category = document.getElementById('form-category').value;
            const rawCode = document.getElementById('form-html-code').value.trim();

            let finalHtml = rawCode;
            if (!finalHtml) {
                finalHtml = `<!DOCTYPE html><html><head><title>${title}</title><style>body { font-family: sans-serif; background: #0f172a; color: #fff; text-align: center; padding-top: 60px; }</style></head><body><h1>${title}</h1><p>${desc}</p></body></html>`;
            }

            const newCourse = {
                id: 'course_' + Date.now(),
                title,
                desc,
                price,
                image,
                category,
                students: 0,
                creator: currentUserRole === 'creator' ? 'Instructor (You)' : 'CourseCraft Creator',
                htmlCode: finalHtml
            };

            allCourses.unshift(newCourse);
            saveAllData();
            showToast('Course published successfully!', 'success');
            resetCreatorForm();
            switchView('marketplace');
        }

        function resetCreatorForm() {
            document.getElementById('creator-form').reset();
            clearZipUpload();
        }

        function renderCreatorTable() {
            const tbody = document.getElementById('creator-courses-table');
            if (!tbody) return;

            if (allCourses.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="py-6 text-center text-slate-400">No published courses yet.</td></tr>`;
                return;
            }

            tbody.innerHTML = allCourses.map(course => {
                const revenue = (course.students || 0) * course.price;
                return `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="py-3 px-4 font-bold text-slate-800 flex items-center gap-3">
                            <img src="${course.image || ''}" class="w-10 h-10 rounded-xl object-cover" onerror="this.src='https://placehold.co/40x40/6366f1/ffffff?text=C'">
                            <span class="line-clamp-1">${course.title}</span>
                        </td>
                        <td class="py-3 px-4"><span class="px-2.5 py-1 rounded-full text-xs font-semibold bg-indigo-50 text-indigo-700">${course.category}</span></td>
                        <td class="py-3 px-4 font-semibold text-slate-900">$${course.price.toFixed(2)}</td>
                        <td class="py-3 px-4">${course.students || 0} students</td>
                        <td class="py-3 px-4 font-bold text-emerald-600">$${revenue.toFixed(2)}</td>
                        <td class="py-3 px-4 text-right">
                            <button onclick="launchCoursePlayer('${course.id}')" class="px-3 py-1.5 rounded-lg bg-indigo-50 hover:bg-indigo-100 text-indigo-600 text-xs font-semibold mr-1 transition"><i class="fa-solid fa-play"></i></button>
                            <button onclick="deleteCourse('${course.id}')" class="px-3 py-1.5 rounded-lg bg-red-50 hover:bg-red-100 text-red-600 text-xs font-semibold transition"><i class="fa-solid fa-trash"></i></button>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        function deleteCourse(id) {
            allCourses = allCourses.filter(c => c.id !== id);
            saveAllData();
            showToast('Course deleted successfully.', 'info');
            renderMarketplace();
            renderDashboard();
            renderCreatorTable();
        }

        // Custom Notification Toast & Modal Helpers
        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            if (!container) return;

            const toast = document.createElement('div');
            const bgClass = type === 'success' ? 'bg-emerald-600 text-white' : (type === 'error' ? 'bg-red-600 text-white' : 'bg-slate-900 text-white');
            const icon = type === 'success' ? 'fa-circle-check' : (type === 'error' ? 'fa-circle-exclamation' : 'fa-circle-info');

            toast.className = `${bgClass} px-4 py-3 rounded-2xl shadow-xl text-xs font-semibold flex items-center gap-2 transform translate-y-2 opacity-0 transition-all duration-300 pointer-events-auto`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-sm"></i> <span>${message}</span>`;
            
            container.appendChild(toast);
            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 10);

            setTimeout(() => {
                toast.classList.add('translate-y-2', 'opacity-0');
                setTimeout(() => toast.remove(), 300);
            }, 3500);
        }

        function showCustomModal(title, text) {
            document.getElementById('custom-modal-title').textContent = title;
            document.getElementById('custom-modal-text').textContent = text;
            document.getElementById('custom-modal').classList.remove('hidden');
        }

        function closeCustomModal() {
            document.getElementById('custom-modal').classList.add('hidden');
        }
    </script>
</body>
</html>
