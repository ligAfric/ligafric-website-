import React, { useState } from 'react';
import { 
  Home, ShoppingBag, Truck, UserCheck, CloudSun, 
  Store, User, PlusCircle, Search, Menu, X, ArrowRight, MapPin, Phone
} from 'lucide-react';

export default function LigAfricApp() {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [isMenuOpen, setIsMenuOpen] = useState(false);

  // Mock Data
  const [products, setProducts] = useState([
    { id: 1, name: 'Fresh Yellow Maize', price: '$25 / Bag', location: 'Nakuru, Kenya', category: 'Crops', seller: 'John Doe' },
    { id: 2, name: 'Organic Tomatoes', price: '$12 / Crate', location: 'Ibadan, Nigeria', category: 'Vegetables', seller: 'Amina Bello' },
    { id: 3, name: 'Grade A Cassava', price: '$18 / Bag', location: 'Kumasi, Ghana', category: 'Tubers', seller: 'Kofi Mensah' }
  ]);

  const [newProduct, setNewProduct] = useState({ name: '', price: '', location: '', category: 'Crops' });

  const handleAddProduct = (e) => {
    e.preventDefault();
    if (!newProduct.name || !newProduct.price) return;
    setProducts([...products, { ...newProduct, id: Date.now(), seller: 'You (Farmer Profile)' }]);
    setNewProduct({ name: '', price: '', location: '', category: 'Crops' });
    setActiveTab('marketplace');
  };

  return (
    <div className="flex h-screen bg-slate-50 font-sans text-slate-800">
      
      {/* SIDEBAR NAVIGATION */}
      <aside className={`fixed inset-y-0 left-0 z-50 w-64 bg-emerald-800 text-white transform ${isMenuOpen ? 'translate-x-0' : '-translate-x-full'} md:relative md:translate-x-0 transition-transform duration-200 ease-in-out flex flex-col justify-between shadow-xl`}>
        <div>
          <div className="flex items-center justify-between p-6 border-b border-emerald-700">
            <h1 className="text-2xl font-bold tracking-wider text-emerald-200">lig<span className="text-amber-400">Afric</span></h1>
            <button onClick={() => setIsMenuOpen(false)} className="md:hidden text-emerald-200 hover:text-white">
              <X size={24} />
            </button>
          </div>

          <nav className="p-4 space-y-1">
            <NavItem icon={<Home size={20} />} label="Dashboard" active={activeTab === 'dashboard'} onClick={() => { setActiveTab('dashboard'); setIsMenuOpen(false); }} />
            <NavItem icon={<ShoppingBag size={20} />} label="Marketplace" active={activeTab === 'marketplace' || activeTab === 'add-product'} onClick={() => { setActiveTab('marketplace'); setIsMenuOpen(false); }} />
            <NavItem icon={<Truck size={20} />} label="Delivery & Logistics" active={activeTab === 'logistics'} onClick={() => { setActiveTab('logistics'); setIsMenuOpen(false); }} />
            <NavItem icon={<UserCheck size={20} />} label="Agri Experts" active={activeTab === 'experts'} onClick={() => { setActiveTab('experts'); setIsMenuOpen(false); }} />
            <NavItem icon={<CloudSun size={20} />} label="Weather Updates" active={activeTab === 'weather'} onClick={() => { setActiveTab('weather'); setIsMenuOpen(false); }} />
            <NavItem icon={<Store size={20} />} label="Input Suppliers" active={activeTab === 'suppliers'} onClick={() => { setActiveTab('suppliers'); setIsMenuOpen(false); }} />
            <NavItem icon={<User size={20} />} label="Farmer Profile" active={activeTab === 'profile'} onClick={() => { setActiveTab('profile'); setIsMenuOpen(false); }} />
          </nav>
        </div>

        <div className="p-4 border-t border-emerald-700 text-xs text-emerald-300 text-center">
          ligAfric v1.0 • Empowering African Farmers
        </div>
      </aside>

      {/* MAIN CONTENT AREA */}
      <div className="flex-1 flex flex-col overflow-hidden">
        
        {/* HEADER */}
        <header className="bg-white border-b border-slate-200 p-4 flex items-center justify-between shadow-sm">
          <div className="flex items-center gap-3">
            <button onClick={() => setIsMenuOpen(true)} className="md:hidden p-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700">
              <Menu size={24} />
            </button>
            <h2 className="text-xl font-bold capitalize text-slate-800">
              {activeTab.replace('-', ' ')}
            </h2>
          </div>
          <div className="flex items-center gap-3">
            <span className="hidden sm:inline-block text-sm text-slate-500">Welcome, <strong>Samuel Okeyo</strong></span>
            <div className="w-9 h-9 rounded-full bg-emerald-600 text-white font-bold flex items-center justify-center shadow">
              SO
            </div>
          </div>
        </header>

        {/* DYNAMIC SCREEN CONTENT */}
        <main className="flex-1 overflow-y-auto p-4 md:p-6 bg-slate-50">
          
          {/* 1. MAIN DASHBOARD */}
          {activeTab === 'dashboard' && (
            <div className="space-y-6">
              {/* Quick Stats */}
              <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <StatCard title="Active Listings" value="12" subtext="3 sold this week" color="bg-emerald-500" />
                <StatCard title="Pending Orders" value="4" subtext="2 in transit" color="bg-amber-500" />
                <StatCard title="Consultations" value="2 Active" subtext="Expert: Dr. Kwame" color="bg-blue-500" />
                <StatCard title="Local Weather" value="28°C" subtext="Sunny • Rain expected Thu" color="bg-indigo-500" />
              </div>

              {/* Quick Actions Grid */}
              <h3 className="text-lg font-semibold text-slate-800">Quick Navigation Menu</h3>
              <div className="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4">
                <QuickLink label="Browse Market" icon={<ShoppingBag size={28} className="text-emerald-600"/>} onClick={() => setActiveTab('marketplace')} />
                <QuickLink label="Sell Produce" icon={<PlusCircle size={28} className="text-amber-600"/>} onClick={() => setActiveTab('add-product')} />
                <QuickLink label="Book Logistics" icon={<Truck size={28} className="text-blue-600"/>} onClick={() => setActiveTab('logistics')} />
                <QuickLink label="Ask an Expert" icon={<UserCheck size={28} className="text-purple-600"/>} onClick={() => setActiveTab('experts')} />
                <QuickLink label="Input Store" icon={<Store size={28} className="text-emerald-600"/>} onClick={() => setActiveTab('suppliers')} />
                <QuickLink label="Weather Forecast" icon={<CloudSun size={28} className="text-indigo-600"/>} onClick={() => setActiveTab('weather')} />
              </div>
            </div>
          )}

          {/* 2. MARKETPLACE & BROWSE PRODUCTS */}
          {activeTab === 'marketplace' && (
            <div className="space-y-6">
              <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                <div className="relative w-full sm:w-96">
                  <Search className="absolute left-3 top-2.5 text-slate-400" size={18} />
                  <input type="text" placeholder="Search crops, seeds, location..." className="w-full pl-10 pr-4 py-2 bg-white border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-emerald-500" />
                </div>
                <button onClick={() => setActiveTab('add-product')} className="w-full sm:w-auto px-4 py-2 bg-emerald-600 text-white font-medium rounded-lg hover:bg-emerald-700 transition flex items-center justify-center gap-2">
                  <PlusCircle size={18} /> Add New Product
                </button>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {products.map((p) => (
                  <div key={p.id} className="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden hover:shadow-md transition">
                    <div className="h-40 bg-emerald-100 flex items-center justify-center text-emerald-800 font-bold text-lg">
                      {p.category}
                    </div>
                    <div className="p-4 space-y-2">
                      <div className="flex justify-between items-start">
                        <h4 className="font-bold text-slate-800 text-lg">{p.name}</h4>
                        <span className="text-emerald-700 font-extrabold bg-emerald-50 px-2 py-1 rounded text-sm">{p.price}</span>
                      </div>
                      <p className="text-sm text-slate-500 flex items-center gap-1"><MapPin size={14}/> {p.location}</p>
                      <p className="text-xs text-slate-400">Listed by: {p.seller}</p>
                      <button className="w-full mt-2 py-2 bg-slate-900 text-white rounded-lg text-sm font-medium hover:bg-slate-800 transition">Contact Seller</button>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* 3. ADD PRODUCT */}
          {activeTab === 'add-product' && (
            <div className="max-w-xl mx-auto bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
              <h3 className="text-xl font-bold mb-4 text-slate-800">List Produce for Sale</h3>
              <form onSubmit={handleAddProduct} className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-slate-700 mb-1">Product Title</label>
                  <input type="text" required placeholder="e.g. Fresh White Maize" className="w-full p-2.5 border border-slate-200 rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value={newProduct.name} onChange={(e) => setNewProduct({...newProduct, name: e.target.value})} />
                </div>
                <div className="grid grid-cols-2 gap-4">
                  <div>
                    <label className="block text-sm font-medium text-slate-700 mb-1">Price / Quantity</label>
                    <input type="text" required placeholder="e.g. $30 / 50kg Bag" className="w-full p-2.5 border border-slate-200 rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value={newProduct.price} onChange={(e) => setNewProduct({...newProduct, price: e.target.value})} />
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-slate-700 mb-1">Category</label>
                    <select className="w-full p-2.5 border border-slate-200 rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none bg-white" value={newProduct.category} onChange={(e) => setNewProduct({...newProduct, category: e.target.value})}>
                      <option>Crops</option>
                      <option>Vegetables</option>
                      <option>Tubers</option>
                      <option>Fruits</option>
                      <option>Livestock</option>
                    </select>
                  </div>
                </div>
                <div>
                  <label className="block text-sm font-medium text-slate-700 mb-1">Pickup Location</label>
                  <input type="text" required placeholder="e.g. Eldoret, Kenya" className="w-full p-2.5 border border-slate-200 rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value={newProduct.location} onChange={(e) => setNewProduct({...newProduct, location: e.target.value})} />
                </div>
                <div className="flex justify-end gap-3 pt-2">
                  <button type="button" onClick={() => setActiveTab('marketplace')} className="px-4 py-2 border border-slate-200 rounded-lg text-slate-600">Cancel</button>
                  <button type="submit" className="px-4 py-2 bg-emerald-600 text-white rounded-lg hover:bg-emerald-700 font-medium">Publish Listing</button>
                </div>
              </form>
            </div>
          )}

          {/* 4. FARMER PROFILE */}
          {activeTab === 'profile' && (
            <div className="max-w-2xl mx-auto bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-6">
              <div className="flex items-center gap-4 border-b border-slate-100 pb-6">
                <div className="w-20 h-20 rounded-full bg-emerald-600 text-white font-bold text-2xl flex items-center justify-center">
                  SO
                </div>
                <div>
                  <h3 className="text-xl font-bold text-slate-800">Samuel Okeyo</h3>
                  <p className="text-sm text-slate-500">Verified Maize & Poultry Farmer</p>
                  <p className="text-xs text-emerald-600 font-medium mt-1">📍 Rift Valley, Kenya • 15 Acres</p>
                </div>
              </div>
              <div className="space-y-4">
                <h4 className="font-bold text-slate-800">Farm Details</h4>
                <div className="grid grid-cols-2 gap-4 text-sm">
                  <div className="bg-slate-50 p-3 rounded-lg"><span className="text-slate-500 block">Main Crops:</span> Maize, Soybeans</div>
                  <div className="bg-slate-50 p-3 rounded-lg"><span className="text-slate-500 block">Soil Type:</span> Loam / Clay</div>
                  <div className="bg-slate-50 p-3 rounded-lg"><span className="text-slate-500 block">Phone:</span> +254 712 345 678</div>
                  <div className="bg-slate-50 p-3 rounded-lg"><span className="text-slate-500 block">Member Since:</span> Jan 2024</div>
                </div>
              </div>
            </div>
          )}

          {/* 5. DELIVERY & LOGISTICS */}
          {activeTab === 'logistics' && (
            <div className="space-y-4">
              <h3 className="font-bold text-lg text-slate-800">Logistics & Transportation Partners</h3>
              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <LogisticsCard title="AfriTruck Transport Co." routes="Nairobi ↔ Eldoret ↔ Kisumu" capacity="5 - 20 Tons (Refrigerated)" status="Available" />
                <LogisticsCard title="AgriExpress Local Haulage" routes="Intra-County & Farm Pickup" capacity="1 - 3 Tons Pickup" status="Available" />
              </div>
            </div>
          )}

          {/* 6. AGRICULTURAL EXPERTS */}
          {activeTab === 'experts' && (
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <ExpertCard name="Dr. Kwame Mensah" title="Agronomist & Soil Specialist" rating="4.9 ★" experience="12 yrs exp." />
              <ExpertCard name="Sarah Mwangi" title="Crop Disease & Pest Expert" rating="4.8 ★" experience="8 yrs exp." />
            </div>
          )}

          {/* 7. WEATHER UPDATES */}
          {activeTab === 'weather' && (
            <div className="bg-gradient-to-br from-blue-600 to-indigo-800 text-white p-6 rounded-xl shadow-lg space-y-6">
              <div>
                <h3 className="text-2xl font-bold">Local Farm Weather</h3>
                <p className="text-blue-100 text-sm">Rift Valley Region • Updated 10 mins ago</p>
              </div>
              <div className="flex items-center justify-between border-b border-blue-400/30 pb-6">
                <div>
                  <div className="text-5xl font-extrabold">28°C</div>
                  <p className="text-blue-200 mt-1">Partly Cloudy • Humidity: 62%</p>
                </div>
                <CloudSun size={64} className="text-amber-300" />
              </div>
              <div>
                <h4 className="font-bold mb-3">5-Day Agriculture Forecast</h4>
                <div className="grid grid-cols-5 gap-2 text-center text-xs">
                  <div className="bg-white/10 p-2 rounded">Wed<br/><b>28°C</b><br/>☀️</div>
                  <div className="bg-white/10 p-2 rounded">Thu<br/><b>22°C</b><br/>🌧️</div>
                  <div className="bg-white/10 p-2 rounded">Fri<br/><b>24°C</b><br/>🌦️</div>
                  <div className="bg-white/10 p-2 rounded">Sat<br/><b>27°C</b><br/>🌤️</div>
                  <div className="bg-white/10 p-2 rounded">Sun<br/><b>29°C</b><br/>☀️</div>
                </div>
              </div>
            </div>
          )}

          {/* 8. FARM INPUT SUPPLIERS */}
          {activeTab === 'suppliers' && (
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <SupplierCard name="GreenField Agro Inputs" products="Certified Hybrid Seeds, Organic Fertilizers" location="Kampala, Uganda" />
              <SupplierCard name="Sahara Farm Machinery" products="Tractors, Irrigation Equipment, Solar Pumps" location="Abuja, Nigeria" />
            </div>
          )}

        </main>
      </div>
    </div>
  );
}

// Helper Components
function NavItem({ icon, label, active, onClick }) {
  return (
    <button onClick={onClick} className={`w-full flex items-center gap-3 px-4 py-3 rounded-lg text-sm font-medium transition ${active ? 'bg-emerald-900 text-amber-400 font-bold' : 'text-emerald-100 hover:bg-emerald-700/50 hover:text-white'}`}>
      {icon}
      <span>{label}</span>
    </button>
  );
}

function StatCard({ title, value, subtext, color }) {
  return (
    <div className="bg-white p-5 rounded-xl border border-slate-200 shadow-sm relative overflow-hidden">
      <div className={`absolute top-0 left-0 w-1.5 h-full ${color}`} />
      <p className="text-xs font-semibold text-slate-400 uppercase tracking-wider">{title}</p>
      <h3 className="text-2xl font-bold text-slate-800 my-1">{value}</h3>
      <p className="text-xs text-slate-500">{subtext}</p>
    </div>
  );
}

function QuickLink({ label, icon, onClick }) {
  return (
    <button onClick={onClick} className="p-4 bg-white border border-slate-200 rounded-xl hover:shadow-md transition flex flex-col items-center justify-center gap-2 text-center group">
      <div className="p-3 rounded-full bg-slate-50 group-hover:scale-110 transition">{icon}</div>
      <span className="text-sm font-semibold text-slate-700">{label}</span>
    </button>
  );
}

function LogisticsCard({ title, routes, capacity, status }) {
  return (
    <div className="bg-white p-5 rounded-xl border border-slate-200 shadow-sm flex flex-col justify-between">
      <div>
        <h4 className="font-bold text-slate-800">{title}</h4>
        <p className="text-xs text-slate-500 mt-1"><b>Routes:</b> {routes}</p>
        <p className="text-xs text-slate-500"><b>Capacity:</b> {capacity}</p>
      </div>
      <div className="mt-4 flex items-center justify-between">
        <span className="text-xs bg-emerald-100 text-emerald-800 font-bold px-2 py-0.5 rounded">{status}</span>
        <button className="px-3 py-1.5 bg-emerald-600 text-white rounded text-xs font-medium hover:bg-emerald-700">Request Quote</button>
      </div>
    </div>
  );
}

function ExpertCard({ name, title, rating, experience }) {
  return (
    <div className="bg-white p-5 rounded-xl border border-slate-200 shadow-sm flex items-center justify-between">
      <div>
        <h4 className="font-bold text-slate-800">{name}</h4>
        <p className="text-xs text-emerald-700 font-medium">{title}</p>
        <p className="text-xs text-slate-400 mt-1">{experience} • <span className="text-amber-500 font-bold">{rating}</span></p>
      </div>
      <button className="px-3 py-2 bg-slate-900 text-white rounded-lg text-xs font-medium hover:bg-slate-800">Book Advisory</button>
    </div>
  );
}

function SupplierCard({ name, products, location }) {
  return (
    <div className="bg-white p-5 rounded-xl border border-slate-200 shadow-sm space-y-2">
      <h4 className="font-bold text-slate-800">{name}</h4>
      <p className="text-xs text-slate-600"><b>Supplies:</b> {products}</p>
      <p className="text-xs text-slate-400 flex items-center gap-1"><MapPin size={12}/> {location}</p>
      <button className="w-full mt-2 py-1.5 border border-emerald-600 text-emerald-700 rounded text-xs font-medium hover:bg-emerald-50">View Store Catalog</button>
    </div>
  );
}
