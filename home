<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PRAZA - Premium Property Marketplace</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@200;400;600;800&display=swap');
        
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #020617;
            overflow-x: hidden;
        }

        .glass {
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .property-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(59, 130, 246, 0.15);
        }

        .payment-option:hover {
            background: rgba(59, 130, 246, 0.1);
            border-color: #3b82f6;
        }

        .payment-option.active {
            background: rgba(59, 130, 246, 0.2);
            border-color: #3b82f6;
            box-shadow: 0 0 15px rgba(59, 130, 246, 0.3);
        }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #020617; }
        ::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 10px; }

        #three-canvas {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: -1;
        }

        .loader {
            border: 3px solid rgba(255, 255, 255, 0.1);
            border-top: 3px solid #3b82f6;
            border-radius: 50%;
            width: 24px; height: 24px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .animate-fade-in { animation: fadeIn 0.3s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
    </style>
</head>
<body class="text-slate-100">

    <div id="three-canvas"></div>

    <!-- Global Notification -->
    <div id="toast" class="fixed top-6 right-6 z-[200] px-6 py-4 rounded-2xl glass border-blue-500/30 text-blue-200 hidden animate-fade-in flex items-center gap-3">
        <i data-lucide="shield-check" class="w-5 h-5"></i>
        <span id="toast-msg">Action Successful</span>
    </div>

    <div id="app" class="max-w-7xl mx-auto px-6 py-8">
        
        <!-- Auth Section -->
        <div id="auth-section" class="flex flex-col items-center justify-center min-h-[90vh]">
            <div class="w-full max-w-md glass p-10 rounded-[2.5rem] shadow-2xl">
                <div class="text-center mb-10">
                    <div class="inline-flex p-4 bg-blue-600 rounded-3xl mb-4 shadow-lg shadow-blue-600/30">
                        <i data-lucide="home" class="w-8 h-8 text-white"></i>
                    </div>
                    <h1 class="text-4xl font-black tracking-tighter mb-2">PRAZA</h1>
                    <p class="text-slate-400">Premium Estate Marketplace</p>
                </div>
                <form id="auth-form" class="space-y-4">
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase px-2">Email Address</label>
                        <input type="email" id="email" value="demo@praza.com" class="w-full bg-white/5 border border-white/10 rounded-2xl px-5 py-4 outline-none focus:ring-2 focus:ring-blue-500 transition-all">
                    </div>
                    <div class="space-y-1">
                        <label class="text-xs font-bold text-slate-500 uppercase px-2">Password</label>
                        <input type="password" id="password" value="password" class="w-full bg-white/5 border border-white/10 rounded-2xl px-5 py-4 outline-none focus:ring-2 focus:ring-blue-500 transition-all">
                    </div>
                    <button type="submit" class="w-full bg-blue-600 hover:bg-blue-500 text-white py-4 rounded-2xl font-bold shadow-xl shadow-blue-600/20 transition-all active:scale-95">
                        Enter Marketplace
                    </button>
                </form>
                <p id="auth-error" class="text-red-400 text-xs mt-4 text-center hidden"></p>
            </div>
        </div>

        <!-- Dashboard Section -->
        <div id="dashboard-section" class="hidden">
            <header class="flex flex-wrap items-center justify-between gap-6 mb-12 glass p-6 rounded-[2rem] shadow-xl">
                <div class="flex items-center gap-4">
                    <div class="p-3 bg-blue-600 rounded-2xl shadow-lg">
                        <i data-lucide="home" class="w-6 h-6 text-white"></i>
                    </div>
                    <div>
                        <h1 class="text-2xl font-bold tracking-tight">PRAZA</h1>
                        <p class="text-xs text-blue-400 font-bold uppercase tracking-widest">Property Intelligence</p>
                    </div>
                </div>

                <div class="flex items-center gap-3">
                    <button id="view-explore" class="p-3 rounded-xl bg-white/10 text-white"><i data-lucide="search" class="w-5 h-5"></i></button>
                    <button id="view-bookings" class="p-3 rounded-xl text-slate-400 hover:text-white relative">
                        <i data-lucide="calendar" class="w-5 h-5"></i>
                        <span id="booking-badge" class="absolute -top-1 -right-1 bg-blue-500 text-[10px] w-4 h-4 flex items-center justify-center rounded-full hidden">0</span>
                    </button>
                    <div class="w-px h-8 bg-white/10 mx-2"></div>
                    <button id="logout-btn" class="p-2 bg-red-500/20 text-red-400 rounded-full hover:bg-red-500 hover:text-white transition-all"><i data-lucide="log-out" class="w-4 h-4"></i></button>
                </div>
            </header>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-10">
                <aside class="lg:col-span-3 space-y-6">
                    <div class="glass p-8 rounded-[2rem] shadow-xl">
                        <h3 class="text-xs font-black text-slate-500 uppercase mb-6 tracking-widest flex items-center gap-2">
                            <i data-lucide="filter" class="w-4 h-4"></i> Refine Search
                        </h3>
                        <div class="space-y-6">
                            <input type="text" id="search-input" placeholder="City or title..." class="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-sm focus:ring-2 focus:ring-blue-500 outline-none">
                            <div class="flex flex-wrap gap-2">
                                <button class="filter-btn px-4 py-2 rounded-xl bg-blue-600 text-[10px] font-black uppercase tracking-wider" data-cat="All">All</button>
                                <button class="filter-btn px-4 py-2 rounded-xl bg-white/5 text-[10px] font-black uppercase tracking-wider text-slate-400" data-cat="Home">Home</button>
                                <button class="filter-btn px-4 py-2 rounded-xl bg-white/5 text-[10px] font-black uppercase tracking-wider text-slate-400" data-cat="Villa">Villa</button>
                            </div>
                        </div>
                    </div>
                    <button id="add-property-btn" class="w-full bg-white text-slate-950 p-6 rounded-[2rem] font-black shadow-2xl hover:bg-blue-600 hover:text-white transition-all flex items-center justify-between group">
                        <span>SELL PROPERTY</span>
                        <i data-lucide="plus" class="w-6 h-6 group-hover:rotate-90 transition-transform"></i>
                    </button>
                </aside>

                <main id="main-content" class="lg:col-span-9">
                    <div id="property-grid" class="grid grid-cols-1 md:grid-cols-2 gap-8">
                        <!-- Properties -->
                    </div>
                </main>
            </div>
        </div>
    </div>

    <!-- Booking Modal (Payment Integration) -->
    <div id="booking-modal" class="fixed inset-0 z-[150] flex items-center justify-center p-6 bg-slate-950/80 backdrop-blur-xl hidden">
        <div class="bg-slate-900 w-full max-w-xl rounded-[3rem] border border-white/10 shadow-3xl overflow-hidden animate-fade-in">
            <div class="p-8 border-b border-white/5 flex justify-between items-center">
                <div>
                    <h2 class="text-2xl font-black">Book a Virtual Tour</h2>
                    <p class="text-slate-500 text-sm" id="booking-prop-name">Property Name</p>
                </div>
                <button onclick="closeModal('booking-modal')" class="p-2 hover:bg-white/10 rounded-full"><i data-lucide="x" class="w-6 h-6"></i></button>
            </div>
            
            <div class="p-8 space-y-8">
                <div class="grid grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label class="text-[10px] font-black text-slate-500 uppercase tracking-widest px-2">Select Date</label>
                        <input type="date" id="tour-date" class="w-full bg-white/5 border border-white/10 rounded-2xl px-5 py-4 outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="space-y-2">
                        <label class="text-[10px] font-black text-slate-500 uppercase tracking-widest px-2">Time Slot</label>
                        <select id="tour-time" class="w-full bg-white/5 border border-white/10 rounded-2xl px-5 py-4 outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="10:00 AM">10:00 AM</option>
                            <option value="02:00 PM">02:00 PM</option>
                            <option value="06:00 PM">06:00 PM</option>
                        </select>
                    </div>
                </div>

                <div class="space-y-4">
                    <label class="text-[10px] font-black text-slate-500 uppercase tracking-widest px-2">Payment Gateway</label>
                    <div class="grid grid-cols-3 gap-3">
                        <button onclick="selectPayment('upi')" class="payment-option p-4 rounded-2xl border border-white/10 flex flex-col items-center gap-2 transition-all active" id="pay-upi">
                            <i data-lucide="smartphone" class="w-6 h-6"></i>
                            <span class="text-[10px] font-bold">UPI</span>
                        </button>
                        <button onclick="selectPayment('card')" class="payment-option p-4 rounded-2xl border border-white/10 flex flex-col items-center gap-2 transition-all" id="pay-card">
                            <i data-lucide="credit-card" class="w-6 h-6"></i>
                            <span class="text-[10px] font-bold">CARD</span>
                        </button>
                        <button onclick="selectPayment('net')" class="payment-option p-4 rounded-2xl border border-white/10 flex flex-col items-center gap-2 transition-all" id="pay-net">
                            <i data-lucide="building-2" class="w-6 h-6"></i>
                            <span class="text-[10px] font-bold">BANKING</span>
                        </button>
                    </div>
                </div>

                <div class="bg-blue-600/10 p-4 rounded-2xl border border-blue-500/20 flex justify-between items-center">
                    <span class="text-xs font-bold text-blue-300">Consultation Fee</span>
                    <span class="text-xl font-black text-white">₹499.00</span>
                </div>

                <button id="confirm-booking-btn" class="w-full bg-white text-slate-950 py-5 rounded-2xl font-black hover:bg-blue-600 hover:text-white transition-all flex items-center justify-center gap-3">
                    <i data-lucide="shield-check" class="w-5 h-5"></i>
                    SECURE CHECKOUT
                </button>
            </div>
        </div>
    </div>

    <!-- Firebase Scripts -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, doc, updateDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";

        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'praza-pay-001';

        // Background Particles
        function initThree() {
            const container = document.getElementById('three-canvas');
            const scene = new THREE.Scene();
            const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            container.appendChild(renderer.domElement);

            const geo = new THREE.BufferGeometry();
            const count = 3000;
            const pos = new Float32Array(count * 3);
            for(let i=0; i<count*3; i++) pos[i] = (Math.random() - 0.5) * 15;
            geo.setAttribute('position', new THREE.BufferAttribute(pos, 3));
            const mat = new THREE.PointsMaterial({ size: 0.03, color: 0x60a5fa, transparent: true, opacity: 0.4 });
            const points = new THREE.Points(geo, mat);
            scene.add(points);
            camera.position.z = 8;

            let mouseX = 0, mouseY = 0;
            window.addEventListener('mousemove', (e) => {
                mouseX = (e.clientX - window.innerWidth/2) * 0.005;
                mouseY = (e.clientY - window.innerHeight/2) * 0.005;
            });

            function animate() {
                requestAnimationFrame(animate);
                points.rotation.y += 0.001;
                points.position.x += (mouseX - points.position.x) * 0.05;
                points.position.y += (-mouseY - points.position.y) * 0.05;
                renderer.render(scene, camera);
            }
            animate();
            window.addEventListener('resize', () => {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
            });
        }

        // App Logic
        const authSection = document.getElementById('auth-section');
        const dashboardSection = document.getElementById('dashboard-section');
        const propertyGrid = document.getElementById('property-grid');
        const toast = document.getElementById('toast');
        const bookingModal = document.getElementById('booking-modal');

        let selectedProperty = null;
        let selectedPaymentMethod = 'upi';

        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.selectPayment = (method) => {
            selectedPaymentMethod = method;
            document.querySelectorAll('.payment-option').forEach(el => el.classList.remove('active'));
            document.getElementById(`pay-${method}`).classList.add('active');
        };

        const showToast = (msg) => {
            document.getElementById('toast-msg').innerText = msg;
            toast.classList.remove('hidden');
            setTimeout(() => toast.classList.add('hidden'), 4000);
        };

        onAuthStateChanged(auth, (user) => {
            if (user) {
                authSection.classList.add('hidden');
                dashboardSection.classList.remove('hidden');
                loadProperties();
                loadBookings(user);
            } else {
                authSection.classList.remove('hidden');
                dashboardSection.classList.add('hidden');
            }
            lucide.createIcons();
        });

        document.getElementById('auth-form').onsubmit = async (e) => {
            e.preventDefault();
            const btn = e.target.querySelector('button');
            btn.innerHTML = '<div class="loader mx-auto"></div>';
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (err) {
                btn.innerText = 'Error - Try Again';
            }
        };

        document.getElementById('logout-btn').onclick = () => signOut(auth);

        function loadProperties() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'properties'));
            onSnapshot(q, (snapshot) => {
                propertyGrid.innerHTML = '';
                snapshot.forEach(doc => {
                    const p = doc.data();
                    const id = doc.id;
                    const card = document.createElement('div');
                    card.className = "glass rounded-[2.5rem] overflow-hidden property-card transition-all";
                    card.innerHTML = `
                        <div class="h-56 bg-gradient-to-br from-slate-800 to-slate-900 flex items-center justify-center relative">
                            <span class="absolute top-4 left-4 px-3 py-1 bg-blue-500/20 border border-blue-500/30 rounded-full text-[10px] font-black uppercase tracking-widest text-blue-400">${p.category || 'Luxury'}</span>
                            <div class="text-center">
                                <div class="text-3xl font-black tracking-tighter">₹${p.price || 0} Lacs</div>
                            </div>
                        </div>
                        <div class="p-8">
                            <div class="flex justify-between items-start mb-4">
                                <div>
                                    <h3 class="text-xl font-bold">${p.title || 'Elite Estate'}</h3>
                                    <p class="text-slate-400 text-sm mt-1 flex items-center gap-1"><i data-lucide="map-pin" class="w-3 h-3 text-red-500"></i> ${p.location}</p>
                                </div>
                                <div class="p-2 glass rounded-xl"><i data-lucide="heart" class="w-4 h-4 text-slate-500"></i></div>
                            </div>
                            <button id="book-btn-${id}" class="w-full py-4 bg-white text-slate-950 hover:bg-blue-600 hover:text-white rounded-2xl text-sm font-black transition-all flex items-center justify-center gap-2">
                                <i data-lucide="calendar" class="w-4 h-4"></i> BOOK TOUR
                            </button>
                        </div>
                    `;
                    propertyGrid.appendChild(card);
                    document.getElementById(`book-btn-${id}`).onclick = () => openBooking(id, p.title);
                });
                lucide.createIcons();
            });
        }

        function openBooking(id, title) {
            selectedProperty = { id, title };
            document.getElementById('booking-prop-name').innerText = title;
            bookingModal.classList.remove('hidden');
        }

        document.getElementById('confirm-booking-btn').onclick = async () => {
            const btn = document.getElementById('confirm-booking-btn');
            const date = document.getElementById('tour-date').value;
            const time = document.getElementById('tour-time').value;

            if (!date) { showToast("Please select a date"); return; }

            btn.innerHTML = '<div class="loader mx-auto"></div>';
            btn.disabled = true;

            // Simulate Gateway Latency
            setTimeout(async () => {
                try {
                    const bookingData = {
                        propertyId: selectedProperty.id,
                        propertyTitle: selectedProperty.title,
                        tourDate: date,
                        tourTime: time,
                        paymentMethod: selectedPaymentMethod,
                        amount: 499,
                        status: 'Confirmed',
                        bookedAt: serverTimestamp()
                    };

                    await addDoc(collection(db, 'artifacts', appId, 'users', auth.currentUser.uid, 'bookings'), bookingData);
                    
                    showToast("Transaction Successful! Tour Confirmed.");
                    bookingModal.classList.add('hidden');
                    btn.innerHTML = '<i data-lucide="shield-check" class="w-5 h-5"></i> SECURE CHECKOUT';
                    btn.disabled = false;
                    lucide.createIcons();
                } catch (err) {
                    showToast("Payment Failed. Try again.");
                    btn.innerText = 'RETRY PAYMENT';
                    btn.disabled = false;
                }
            }, 2000);
        };

        function loadBookings(user) {
            const q = query(collection(db, 'artifacts', appId, 'users', user.uid, 'bookings'));
            onSnapshot(q, (snapshot) => {
                const count = snapshot.size;
                const badge = document.getElementById('booking-badge');
                if (count > 0) {
                    badge.innerText = count;
                    badge.classList.remove('hidden');
                } else {
                    badge.classList.add('hidden');
                }
            });
        }

        initThree();
        lucide.createIcons();
    </script>
</body>
</html>
