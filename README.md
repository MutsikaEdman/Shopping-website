<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nexus Business | Complete System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com');
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .page { display: none; }
        .active-page { display: block; }
        .nav-link.active { border-bottom: 4px solid #f68b1e; color: #f68b1e; }
        th { text-align: left; padding: 12px; background: #f8fafc; font-size: 10px; text-transform: uppercase; color: #94a3b8; border-bottom: 2px solid #e2e8f0; }
        td { padding: 14px 12px; border-bottom: 1px solid #f1f5f9; font-size: 13px; }
        @media print { .no-print { display: none !important; } .print-only { display: block !important; } }
        .print-only { display: none; }
    </style>
</head>
<body>

    <!-- TOP DASHBOARD -->
    <header class="bg-white shadow-sm sticky top-0 z-50 no-print">
        <div class="max-w-7xl mx-auto px-6 flex items-center justify-between h-20">
            <div class="flex items-center gap-10">
                <h1 class="text-3xl font-black italic bg-[#f68b1e] text-white px-4 py-1 rounded shadow-lg cursor-pointer" onclick="showPage('store-page')">NEXUS</h1>
                <nav class="flex gap-8 font-bold text-xs uppercase tracking-widest text-slate-400">
                    <button onclick="showPage('store-page')" class="nav-link active py-7" id="btn-store">Marketplace</button>
                    <button onclick="showPage('admin-page')" class="nav-link py-7" id="btn-admin">Admin Dashboard</button>
                </nav>
            </div>
            <div id="auth-box" class="flex items-center gap-4">
                <button onclick="showPage('login-page')" class="text-xs font-black uppercase text-slate-500 hover:text-[#f68b1e]">Sign In</button>
            </div>
        </div>
    </header>

    <!-- STORE PAGE -->
    <div id="store-page" class="page active-page max-w-7xl mx-auto px-6 py-10 no-print">
        <div class="flex flex-col md:flex-row gap-8">
            <aside class="w-full md:w-64 bg-white p-6 rounded-2xl shadow-sm h-fit border border-slate-100">
                <h3 class="text-[10px] font-black uppercase text-slate-400 mb-6 tracking-widest">Departments</h3>
                <ul class="space-y-4 text-sm font-bold text-slate-600">
                    <li class="cursor-pointer hover:text-[#f68b1e]" onclick="renderStore('all')">All Products</li>
                    <li class="cursor-pointer hover:text-[#f68b1e]" onclick="renderStore('electronics')">Electronics</li>
                    <li class="cursor-pointer hover:text-[#f68b1e]" onclick="renderStore('jewelery')">Jewelry</li>
                    <li class="cursor-pointer hover:text-[#f68b1e]" onclick="renderStore('clothing')">Clothing</li>
                </ul>
            </aside>
            <main id="store-grid" class="flex-1 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6"></main>
        </div>
    </div>

    <!-- LOGIN PAGE -->
    <div id="login-page" class="page max-w-md mx-auto mt-20 bg-white p-10 rounded-3xl shadow-2xl no-print">
        <h2 class="text-3xl font-black mb-6">Shopper Profile</h2>
        <div class="space-y-4">
            <input type="text" id="u-name" placeholder="Your Name" class="w-full p-4 border rounded-xl bg-slate-50 outline-none focus:border-[#f68b1e]">
            <select id="u-city" class="w-full p-4 border rounded-xl bg-slate-50 font-bold" onchange="updateStreets()">
                <option value="">Select City</option>
                <option value="Kampala">Kampala</option>
                <option value="Entebbe">Entebbe</option>
                <option value="Mbarara">Mbarara</option>
            </select>
            <select id="u-street" class="w-full p-4 border rounded-xl bg-slate-50 font-bold">
                <option value="">Select Street</option>
            </select>
            <button onclick="saveProfile()" class="w-full bg-[#f68b1e] text-white py-4 rounded-2xl font-black uppercase shadow-xl hover:bg-orange-600 transition">Register & Shop</button>
        </div>
    </div>

    <!-- ADMIN PANEL -->
    <div id="admin-page" class="page max-w-7xl mx-auto px-6 py-10 no-print">
        <div class="flex justify-between items-center mb-8">
            <h2 class="text-2xl font-black uppercase tracking-tight">System Control Panel</h2>
            <button onclick="openAddModal()" class="bg-slate-900 text-white px-6 py-3 rounded-xl text-[10px] font-black uppercase tracking-widest">+ Add New Good</button>
        </div>

        <div class="bg-white rounded-3xl shadow-sm border border-slate-100 overflow-hidden mb-10">
            <div class="p-6 bg-slate-50 font-black text-xs border-b">Sales & Delivery History</div>
            <div class="overflow-x-auto">
                <table class="w-full">
                    <thead>
                        <tr><th>ID</th><th>Customer</th><th>Product</th><th>Amount</th><th>Location</th><th>Terms</th><th>Status</th><th>Actions</th></tr>
                    </thead>
                    <tbody id="admin-orders-body"></tbody>
                </table>
            </div>
        </div>

        <div class="bg-white rounded-3xl shadow-sm border border-slate-100 overflow-hidden">
            <div class="p-6 bg-slate-50 font-black text-xs border-b">Inventory Management</div>
            <div class="overflow-x-auto">
                <table class="w-full">
                    <thead>
                        <tr><th>Img</th><th>Name</th><th>Brand</th><th>Price</th><th>Stock</th><th>Actions</th></tr>
                    </thead>
                    <tbody id="admin-items-body"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- PRODUCT MODAL -->
    <div id="p-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center z-50 p-4">
        <form id="product-form" class="bg-white w-full max-w-lg rounded-[2rem] p-10 shadow-2xl">
            <h3 class="text-2xl font-black mb-8" id="m-title">Product Entry</h3>
            <input type="hidden" id="e-id">
            <div class="grid grid-cols-2 gap-4">
                <input type="text" id="e-name" placeholder="Item Title" class="col-span-2 p-4 border rounded-2xl bg-slate-50 outline-none focus:border-[#f68b1e]" required>
                <input type="number" id="e-price" placeholder="Price ($)" class="p-4 border rounded-2xl bg-slate-50 outline-none focus:border-[#f68b1e]" required>
                <input type="number" id="e-qty" placeholder="Stock Qty" class="p-4 border rounded-2xl bg-slate-50 outline-none focus:border-[#f68b1e]" required>
                <input type="text" id="e-brand" placeholder="Brand" class="p-4 border rounded-2xl bg-slate-50 outline-none focus:border-[#f68b1e]" required>
                <select id="e-cat" class="col-span-2 p-4 border rounded-2xl bg-slate-50 font-bold" required>
                    <option value="electronics">Electronics</option>
                    <option value="jewelery">Jewelry</option>
                    <option value="clothing">Clothing Fashion</option>
                </select>
                <input type="text" id="e-img" placeholder="Image URL" class="col-span-2 p-4 border rounded-2xl bg-slate-50 outline-none focus:border-[#f68b1e]" required>
            </div>
            <div class="flex gap-4 mt-10">
                <button type="button" onclick="saveGood()" class="flex-1 bg-slate-900 text-white py-4 rounded-2xl font-black text-xs">SAVE TO DB</button>
                <button type="button" onclick="closeModal()" class="flex-1 bg-slate-100 text-slate-400 py-4 rounded-2xl font-black text-xs">CANCEL</button>
            </div>
        </form>
    </div>

    <script>
        const streets = { Kampala: ["Acacia Ave", "Wandegeya"], Entebbe: ["Airport Rd", "Kiwafu"], Mbarara: ["High St", "Buremba Rd"] };
        
        let products = JSON.parse(localStorage.getItem('nx_p')) || [
            { id: 101, name: "Smart TV 55'", price: 750, brand: "Sony", category: "electronics", qty: 5, img: "https://images.unsplash.com" },
            { id: 102, name: "Noise Cancel Headphones", price: 299, brand: "Bose", category: "electronics", qty: 12, img: "https://images.unsplash.com" },
            { id: 103, name: "Diamond Necklace", price: 1500, brand: "Tiffany", category: "jewelery", qty: 3, img: "https://images.unsplash.com" },
            { id: 104, name: "Leather Jacket", price: 120, brand: "ZARA", category: "clothing", qty: 20, img: "https://images.unsplash.com" },
            { id: 105, name: "Rolex Oyster", price: 8500, brand: "Rolex", category: "electronics", qty: 2, img: "https://images.unsplash.com" },
            { id: 106, name: "Designer Handbag", price: 450, brand: "Gucci", category: "clothing", qty: 8, img: "https://images.unsplash.com" },
            { id: 107, name: "Gold Earrings", price: 200, brand: "Zaveri", category: "jewelery", qty: 15, img: "https://images.unsplash.com" },
            { id: 108, name: "Wireless Earbuds", price: 150, brand: "Apple", category: "electronics", qty: 25, img: "https://images.unsplash.com" }
        ];
        
        let orders = JSON.parse(localStorage.getItem('nx_o')) || [];
        let profile = JSON.parse(localStorage.getItem('nx_u')) || null;

        function showPage(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active-page'));
            document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
            document.getElementById(id).classList.add('active-page');
            document.getElementById('btn-' + id.split('-')).classList.add('active');
            if(id === 'store-page') renderStore();
            if(id === 'admin-page') renderAdmin();
        }

        function updateStreets() {
            const city = document.getElementById('u-city').value;
            const sel = document.getElementById('u-street');
            sel.innerHTML = '<option value="">Select Street</option>';
            if(city) streets[city].forEach(s => sel.innerHTML += `<option value="${s}">${s}</option>`);
        }

        function saveProfile() {
            const name = document.getElementById('u-name').value;
            const city = document.getElementById('u-city').value;
            const street = document.getElementById('u-street').value;
            if(!name || !city || !street) return alert("Missing Details");
            profile = { name, address: `${street}, ${city}` };
            localStorage.setItem('nx_u', JSON.stringify(profile));
            location.reload();
        }

        function renderStore(cat = 'all') {
            const grid = document.getElementById('store-grid');
            const data = cat === 'all' ? products : products.filter(p => p.category === cat);
            grid.innerHTML = data.map(p => `
                <div class="bg-white p-6 rounded-3xl border border-slate-50 hover:shadow-xl transition-all group">
                    <div class="h-44 flex items-center justify-center bg-slate-50 rounded-2xl mb-4 overflow-hidden">
                        <img src="${p.img}" class="h-full object-contain group-hover:scale-110 transition duration-500">
                    </div>
                    <p class="text-[9px] font-black uppercase text-orange-500">${p.brand} (${p.qty} Left)</p>
                    <h4 class="font-bold text-sm mb-4 truncate">${p.name}</h4>
                    <div class="flex justify-between items-center">
                        <span class="text-xl font-black">$${p.price}</span>
                        <button onclick="buyNow(${p.id})" class="bg-slate-900 text-white px-5 py-2.5 rounded-xl text-xs font-black hover:bg-[#f68b1e] transition shadow-lg shadow-slate-100" ${p.qty < 1 ? 'disabled opacity-50' : ''}>BUY NOW</button>
                    </div>
                </div>
            `).join('');
        }

        function buyNow(id) {
            if(!profile) return showPage('login-page');
            const p = products.find(i => i.id === id);
            p.qty -= 1;
            orders.push({ id: 'NX-' + Date.now(), customer: profile.name, address: profile.address, product: p.name, price: p.price, date: new Date().toLocaleDateString(), status: "Pending", terms: "Pay on Delivery" });
            localStorage.setItem('nx_p', JSON.stringify(products));
            localStorage.setItem('nx_o', JSON.stringify(orders));
            alert("Order Success! Deliver to: " + profile.address);
        }

        function renderAdmin() {
            document.getElementById('admin-orders-body').innerHTML = orders.map((o, i) => `
                <tr>
                    <td class="font-black text-[10px]">${o.id}</td>
                    <td><strong>${o.customer}</strong><br><span class="text-[9px] text-slate-400">${o.address}</span></td>
                    <td class="font-bold">${o.product}</td>
                    <td class="font-black text-orange-600">$${o.price}</td>
                    <td>${o.date}</td>
                    <td class="text-[10px] uppercase font-bold text-slate-400">${o.terms}</td>
                    <td><span class="px-2 py-1 rounded text-[9px] font-bold ${o.status === 'Shipped' ? 'bg-green-100 text-green-700' : 'bg-yellow-100 text-yellow-700'}">${o.status}</span></td>
                    <td><button onclick="updateStatus(${i})" class="text-[#f68b1e] font-black text-[10px] uppercase underline">Ship</button></td>
                </tr>
            `).reverse().join('');

            document.getElementById('admin-items-body').innerHTML = products.map(p => `
                <tr>
                    <td><img src="${p.img}" class="w-10 h-10 rounded border object-cover"></td>
                    <td class="font-bold text-xs">${p.name}</td>
                    <td class="text-slate-400 text-[10px] font-bold">${p.brand}</td>
                    <td class="font-black">$${p.price}</td>
                    <td class="${p.qty < 5 ? 'text-red-500 font-bold' : ''}">${p.qty}</td>
                    <td>
                        <button onclick="editGood(${p.id})" class="text-blue-500 mr-4"><i class="fa fa-pencil"></i></button>
                        <button onclick="deleteGood(${p.id})" class="text-red-400"><i class="fa fa-trash"></i></button>
                    </td>
                </tr>
            `).join('');
        }

        function updateStatus(idx) {
            orders[orders.length - 1 - idx].status = "Shipped";
            localStorage.setItem('nx_o', JSON.stringify(orders));
            renderAdmin();
        }

        function saveGood() {
            const id = document.getElementById('e-id').value;
            const obj = {
                id: id ? Number(id) : Date.now(),
                name: document.getElementById('e-name').value,
                price: Number(document.getElementById('e-price').value),
                qty: Number(document.getElementById('e-qty').value),
                brand: document.getElementById('e-brand').value,
                category: document.getElementById('e-cat').value,
                img: document.getElementById('e-img').value
            };
            if(!obj.name || !obj.price) return alert("Fill Name and Price");

            if(id) { const i = products.findIndex(p => p.id == id); products[i] = obj; }
            else products.push(obj);

            localStorage.setItem('nx_p', JSON.stringify(products));
            
            // ERASE HISTORY: Reset form fields after save
            document.getElementById('product-form').reset();
            document.getElementById('e-id').value = ""; // Clear hidden ID too
            
            closeModal(); 
            renderAdmin();
            alert("Product Saved & Form Cleared!");
        }

        function deleteGood(id) { products = products.filter(p => p.id !== id); localStorage.setItem('nx_p', JSON.stringify(products)); renderAdmin(); }
        
        function editGood(id) {
            const p = products.find(i => i.id === id);
            document.getElementById('e-id').value = p.id;
            document.getElementById('e-name').value = p.name;
            document.getElementById('e-price').value = p.price;
            document.getElementById('e-qty').value = p.qty;
            document.getElementById('e-brand').value = p.brand;
            document.getElementById('e-cat').value = p.category;
            document.getElementById('e-img').value = p.img;
            document.getElementById('m-title').innerText = "Edit Stock Item";
            openModal();
        }

        function openAddModal() { 
            document.getElementById('product-form').reset(); // Ensure form is empty
            document.getElementById('e-id').value = "";
            document.getElementById('m-title').innerText = "New Stock Entry"; 
            openModal(); 
        }

        function openModal() { document.getElementById('p-modal').classList.remove('hidden'); }
        function closeModal() { document.getElementById('p-modal').classList.add('hidden'); }

        if(profile) document.getElementById('auth-box').innerHTML = `<div class="text-right"><p class="text-[9px] font-black uppercase text-[#f68b1e]">${profile.name}</p><p class="text-[8px] text-slate-300">Ship To: ${profile.address}</p></div><button onclick="localStorage.removeItem('nx_u'); location.reload()" class="fa fa-power-off text-slate-300 ml-4"></button>`;
        
        renderStore();
    </script>
</body>
</html>
