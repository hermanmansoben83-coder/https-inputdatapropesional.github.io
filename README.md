# https-inputdatapropesional.github.io
yes
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jemaat Eden Mansoben | GKI di Tanah Papua</title>
  <link rel="icon" href="https://z-cdn-media.chatglm.cn/files/4fabba54-2f26-438b-b40e-398b13caf196.png?auth_key=1889554295-cc7a6a9c56ee486f87310ec34f34e44a-0-f2a1c7614b95bb965c91af1bcef7ef30" type="image/png" />
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/localforage/1.10.0/localforage.min.js"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: { extend: { colors: { primary: '#1B4D3E', gold: '#D4AF37', bg: '#F4F7F6', 'deep-sky': '#0C4A6E', 'gold-accent': '#FBBF24', teal: '#0D9488', sky: '#0369A1', crimson: '#B91C1C' } } }
    }
  </script>
  <style>
    body { font-family: 'Inter', sans-serif; }
    .fade-in { animation: fadeIn 0.5s ease-in; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    .btn-delete-pro { background-color: rgba(239, 68, 68, 0.1); color: #dc2626; transition: all 0.2s; }
    .btn-delete-pro:hover { background-color: #dc2626; color: white; transform: scale(1.05); }
    #preloader { position: fixed; inset: 0; background: #0C4A6E; z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: opacity 0.5s ease; }
    .loader-circle { width: 60px; height: 60px; border: 4px solid rgba(255,255,255,0.2); border-top-color: #FBBF24; border-radius: 50%; animation: spin 1s linear infinite; }
    @keyframes spin { to { transform: rotate(360deg); } }
  </style>
</head>
<body class="bg-bg dark:bg-gray-900 text-gray-800 dark:text-gray-100 transition-colors duration-300">
  <div id="preloader"><div class="loader-circle mb-4"></div><h2 class="text-white text-xl font-bold tracking-widest">EDEN MANSOBEN</h2></div>
  <div id="root"></div>

  <script type="text/babel">
    const { useState, useEffect, useRef, createContext, useContext } = React;
    const AppContext = createContext();
    const useApp = () => useContext(AppContext);
    const STORAGE_KEY = 'eden_cms_pro_v9';
    const SESSION_KEY = 'eden_session_v9';

    const defaultData = {
      darkMode: false,
      siteSettings: {
        namaGereja: "JEMAAT EDEN MANSOBEN", organisasi: "GKI DI TANAH PAPUA", klalis: "KLASIS SUPIORI UTARA",
        alamat: "Waryesi, Supiori Timur, Kabupaten Supiori, Papua", telepon: "081243746402", whatsapp: "6281243746402",
        email: "info@edenmansoben.org",
        logo: "https://z-cdn-media.chatglm.cn/files/4fabba54-2f26-438b-b40e-398b13caf196.png?auth_key=1889554295-cc7a6a9c56ee486f87310ec34f34e44a-0-f2a1c7614b95bb965c91af1bcef7ef30",
        heroImage: "https://z-cdn-media.chatglm.cn/files/6fdf665f-8a15-42cd-a4ae-c7d2870d3b61.jpg?auth_key=1890418786-9b930747aac1472db316be621631d4a7-0-ed18eb9a9c29ad848d744f24ccb425d7",
        heroSideImage: "https://z-cdn-media.chatglm.cn/files/31d6dce0-f8c7-4745-8bca-621d982cb8ce.jpg?auth_key=1890293815-c35f59220123482a90eb3ec39872731c-0-5a117b0c0a07e257e4e12d1f8e378876"
      },
      news: [], sermons: [], schedules: [], mimbarItems: [], albums: [], mediaItems: [], waMessages: [],
      greeting: { text: "Selamat datang di website Jemaat Eden Mansoben. Marilah kita bertumbuh dalam iman dan melayani dengan kasih.", name: "Pendeta Jemaat", photoBase64: "", isVisible: true },
      aboutContent: { profil: "Profil gereja belum diisi.", sejarah: "Sejarah gereja belum diisi.", visi: "Visi gereja belum diisi.", misi: "Misi gereja belum diisi.", struktur: "Struktur organisasi belum diisi.", infoKetua: "Info ketua jemaat belum diisi." }
    };

    const AppProvider = ({ children }) => {
      const [appData, setAppData] = useState(defaultData);
      const [isAdmin, setIsAdmin] = useState(false);
      const [toast, setToast] = useState(null);
      const [isLoaded, setIsLoaded] = useState(false);

      useEffect(() => {
        const session = sessionStorage.getItem(SESSION_KEY);
        if (session) { try { const sessData = JSON.parse(atob(session)); if (sessData.expiry > Date.now()) setIsAdmin(true); else sessionStorage.removeItem(SESSION_KEY); } catch (e) { sessionStorage.removeItem(SESSION_KEY); } }
        localforage.getItem(STORAGE_KEY).then(savedData => {
          if (savedData) setAppData(prev => ({ ...prev, ...savedData }));
          setIsLoaded(true);
          const preloader = document.getElementById('preloader');
          if (preloader) { preloader.style.opacity = '0'; setTimeout(() => preloader.style.display = 'none', 500); }
        }).catch(() => setIsLoaded(true));
      }, []);

      useEffect(() => {
        if (isLoaded) {
          localforage.setItem(STORAGE_KEY, appData).catch(err => console.error(err));
          document.documentElement.classList.toggle('dark', appData.darkMode);
        }
      }, [appData, isLoaded]);

      const updateData = (key, value) => setAppData(prev => ({ ...prev, [key]: value }));
      const login = (u, p) => { if (btoa(`${u}:${p}`) === btoa('admin:iar211')) { const sess = btoa(JSON.stringify({ user: 'admin', expiry: Date.now() + 3600000 })); sessionStorage.setItem(SESSION_KEY, sess); setIsAdmin(true); return true; } return false; };
      const logout = () => { sessionStorage.removeItem(SESSION_KEY); setIsAdmin(false); };
      const showToast = (message, type = 'success') => { setToast({ message, type, id: Date.now() }); setTimeout(() => setToast(null), 3000); };

      return <AppContext.Provider value={{ ...appData, isAdmin, login, logout, updateData, setAppData, showToast }}>
        {children}
        {toast && (
          <div className="fixed top-5 right-5 z-[100] fade-in">
            <div className={`flex items-center gap-3 px-6 py-4 rounded-lg shadow-2xl text-white ${toast.type === 'success' ? 'bg-teal' : 'bg-crimson'}`}>
              <Icon name={toast.type === 'success' ? 'check-circle' : 'alert-circle'} className="w-6 h-6" />
              <span className="font-medium">{toast.message}</span>
            </div>
          </div>
        )}
      </AppContext.Provider>;
    };

    const Icon = ({ name, className }) => { useEffect(() => { if (window.lucide) lucide.createIcons(); }, [name]); return <i data-lucide={name} className={className}></i>; };
    const formatDate = (d) => { if (!d) return ""; const m = ['Jan', 'Feb', 'Mar', 'Apr', 'Mei', 'Jun', 'Jul', 'Agu', 'Sep', 'Okt', 'Nov', 'Des']; const dt = new Date(d); return isNaN(dt) ? d : `${dt.getDate()} ${m[dt.getMonth()]} ${dt.getFullYear()}`; };
    const uid = () => (window.crypto ? crypto.randomUUID() : Date.now() + Math.random().toString(36).substr(2, 9));

    const compressImage = (file, maxWidth = 1280, quality = 0.8) => {
      return new Promise((resolve, reject) => {
        const reader = new FileReader(); reader.readAsDataURL(file);
        reader.onload = (event) => {
          const img = new Image(); img.src = event.target.result;
          img.onload = () => {
            let canvas = document.createElement('canvas');
            if (img.width > maxWidth) { const scale = maxWidth / img.width; canvas.width = maxWidth; canvas.height = img.height * scale; } else { canvas.width = img.width; canvas.height = img.height; }
            let ctx = canvas.getContext('2d'); ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
            resolve(canvas.toDataURL('image/jpeg', quality));
          };
          img.onerror = reject;
        };
        reader.onerror = reject;
      });
    };

    const FileUploader = ({ onUpload, label, accept = "image/*", className = "" }) => {
      const ref = useRef(null);
      const [isUploading, setIsUploading] = useState(false);
      const handleChange = async (e) => {
        const file = e.target.files[0]; if (!file) return;
        setIsUploading(true);
        try {
          if (file.type.includes("image")) {
            if (file.size > 10 * 1024 * 1024) { alert("Ukuran gambar maksimal 10MB."); return; }
            const compressed = await compressImage(file); onUpload(compressed);
          } else if (file.type.includes("video")) {
            if (file.size > 10 * 1024 * 1024) { alert("Ukuran video lokal maksimal 10MB untuk penyimpanan permanen. Jika lebih, gunakan link YouTube."); return; }
            const reader = new FileReader();
            reader.onloadend = () => onUpload(reader.result);
            reader.readAsDataURL(file);
          }
        } catch (err) { alert("Gagal mengupload file."); } finally { setIsUploading(false); }
      };
      return (
        <div className={className}>
          <label className="block text-xs font-medium text-gray-700 dark:text-gray-300 mb-1">{label}</label>
          <button type="button" onClick={() => ref.current.click()} disabled={isUploading} className="w-full px-4 py-3 border border-dashed border-gray-300 dark:border-gray-600 rounded-lg text-sm text-gray-500 dark:text-gray-400 hover:bg-gray-50 dark:hover:bg-gray-700 flex items-center justify-center gap-2 disabled:opacity-50">
            {isUploading ? <Icon name="loader" className="w-5 h-5 animate-spin" /> : <Icon name="upload-cloud" className="w-5 h-5" />}
            {isUploading ? "Memproses..." : "Pilih File"}
          </button>
          <input ref={ref} type="file" accept={accept} className="hidden" onChange={handleChange} />
        </div>
      );
    };

    const Navbar = ({ navigate }) => {
      const { darkMode, isAdmin, updateData, siteSettings, waMessages } = useApp();
      const [isOpen, setIsOpen] = useState(false);
      const infoLinks = [{ name: "Tentang", path: "/tentang" }, { name: "Jadwal", path: "/jadwal" }, { name: "Khotbah", path: "/khotbah" }, { name: "Berita", path: "/berita" }];
      const mediaLinks = [{ name: "Galeri Foto", path: "/media/foto" }, { name: "Video", path: "/media/video" }];
      const mimbarLinks = [{ name: "Artikel", path: "/mimbar/artikel" }, { name: "Khotbah", path: "/mimbar/khotbah" }, { name: "Renungan", path: "/mimbar/renungan" }, { name: "Puisi", path: "/mimbar/puisi" }, { name: "Umum", path: "/mimbar/umum" }];
      const unreadCount = waMessages.filter(m => m.status === 'Baru').length;

      return (
        <header className="sticky top-0 z-50 w-full bg-skyLight dark:bg-gray-800 shadow-md">
          {isAdmin && unreadCount > 0 && (
            <div className="bg-gold-accent text-deep-sky py-2 text-center text-sm font-semibold">
              <Icon name="mail" className="w-4 h-4 inline-block mr-2" /> Anda memiliki {unreadCount} pesan baru. 
              <button onClick={() => navigate('/admin')} className="underline ml-2">Lihat Pesan</button>
            </div>
          )}
          <div className="hidden md:block bg-primary text-white text-xs">
            <div className="container mx-auto flex justify-between items-center py-2 px-4">
              <span>{siteSettings.organisasi} — {siteSettings.klalis}</span>
              <div className="flex items-center gap-4">
                <button onClick={() => updateData('darkMode', !darkMode)} className="flex items-center gap-1 hover:text-gold"><Icon name={darkMode ? "sun" : "moon"} className="w-3 h-3" /> {darkMode ? "Light" : "Dark"} Mode</button>
                <button onClick={() => navigate(isAdmin ? '/admin' : '/login')} className="flex items-center gap-1 hover:text-gold"><Icon name="lock" className="w-3 h-3" /> {isAdmin ? 'Dashboard' : 'Login Admin'}</button>
              </div>
            </div>
          </div>
          <nav className="container mx-auto flex justify-between items-center py-3 px-4">
            <div className="flex items-center gap-3 cursor-pointer" onClick={() => navigate('/')}>
              <img src={siteSettings.logo} className="h-12 w-12 object-contain" alt="Logo GKI" />
              <span className="font-bold text-primary dark:text-white text-lg">{siteSettings.namaGereja}</span>
            </div>
            <div className="hidden lg:flex items-center gap-6">
              <button onClick={() => navigate('/')} className="text-sm font-medium hover:text-primary dark:hover:text-gold">Beranda</button>
              <div className="relative group">
                <button className="flex items-center gap-1 text-sm font-medium hover:text-primary dark:hover:text-gold">Mimbar <Icon name="chevron-down" className="w-4 h-4" /></button>
                <div className="absolute left-0 mt-2 w-72 bg-white dark:bg-gray-800 shadow-xl rounded-lg border dark:border-gray-700 opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all duration-200 z-50 pt-1 grid grid-cols-2 gap-0">
                  {mimbarLinks.map(l => <button key={l.path} onClick={() => navigate(l.path)} className="block w-full text-left px-3 py-3 text-sm hover:bg-skyLight dark:hover:bg-gray-700 border-b dark:border-gray-700">{l.name}</button>)}
                </div>
              </div>
              <div className="relative group">
                <button className="flex items-center gap-1 text-sm font-medium hover:text-primary dark:hover:text-gold">Informasi <Icon name="chevron-down" className="w-4 h-4" /></button>
                <div className="absolute left-0 mt-2 w-56 bg-white dark:bg-gray-800 shadow-xl rounded-lg border dark:border-gray-700 opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all duration-200 z-50 pt-1">
                  {infoLinks.map(l => <button key={l.path} onClick={() => navigate(l.path)} className="block w-full text-left px-4 py-3 text-sm hover:bg-skyLight dark:hover:bg-gray-700 border-b dark:border-gray-700 last:border-0">{l.name}</button>)}
                </div>
              </div>
              <div className="relative group">
                <button className="flex items-center gap-1 text-sm font-medium hover:text-primary dark:hover:text-gold">Media <Icon name="chevron-down" className="w-4 h-4" /></button>
                <div className="absolute left-0 mt-2 w-56 bg-white dark:bg-gray-800 shadow-xl rounded-lg border dark:border-gray-700 opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all duration-200 z-50 pt-1">
                  {mediaLinks.map(l => <button key={l.path} onClick={() => navigate(l.path)} className="block w-full text-left px-4 py-3 text-sm hover:bg-skyLight dark:hover:bg-gray-700 border-b dark:border-gray-700 last:border-0">{l.name}</button>)}
                </div>
              </div>
              <button onClick={() => navigate('/kontak')} className="text-sm font-medium hover:text-primary dark:hover:text-gold">Kontak</button>
            </div>
            <button className="lg:hidden" onClick={() => setIsOpen(!isOpen)}><Icon name={isOpen ? "x" : "menu"} className="w-7 h-7 text-primary dark:text-white" /></button>
          </nav>
          {isOpen && (
            <div className="lg:hidden bg-skyLight dark:bg-gray-800 border-t dark:border-gray-700 p-4 flex flex-col gap-1">
              <button onClick={() => { navigate('/'); setIsOpen(false); }} className="text-left py-3 border-b dark:border-gray-700 font-medium">Beranda</button>
              {mimbarLinks.map(l => <button key={l.path} onClick={() => { navigate(l.path); setIsOpen(false); }} className="text-left py-3 border-b dark:border-gray-700">{l.name}</button>)}
              {infoLinks.map(l => <button key={l.path} onClick={() => { navigate(l.path); setIsOpen(false); }} className="text-left py-3 border-b dark:border-gray-700">{l.name}</button>)}
              {mediaLinks.map(l => <button key={l.path} onClick={() => { navigate(l.path); setIsOpen(false); }} className="text-left py-3 border-b dark:border-gray-700">{l.name}</button>)}
              <button onClick={() => navigate(isAdmin ? '/admin' : '/login')} className="bg-gold text-white px-4 py-2 rounded-md text-sm font-semibold mt-2">{isAdmin ? 'Dashboard' : 'Login Admin'}</button>
            </div>
          )}
        </header>
      );
    };

    const Hero = () => {
      const { siteSettings } = useApp();
      return (
        <section className="relative overflow-hidden min-h-[90vh] flex items-center bg-deep-sky">
          <div className="absolute inset-0 z-0">
            <img src={siteSettings.heroImage} className="w-full h-full object-cover" alt="Latar" />
            <div className="absolute inset-0 bg-gradient-to-r from-primary/90 via-primary/80 to-primary/40 dark:from-gray-900/90 dark:via-gray-900/80 dark:to-gray-900/40"></div>
          </div>
          <div className="container mx-auto px-4 py-16 md:py-24 grid md:grid-cols-2 gap-12 items-center fade-in relative z-10">
            <div className="text-white">
              <h1 className="text-white leading-tight mb-3 tracking-wide" style={{ fontStyle: 'italic', fontWeight: 'bold', fontSize: '36px' }}>SELAMAT DATANG DI {siteSettings.namaGereja}</h1>
              <p className="text-base font-semibold text-blue-100 mb-6">{siteSettings.organisasi} — {siteSettings.klalis}</p>
              <p className="text-lg italic text-blue-50 mb-8 border-l-4 border-gold pl-4">"Bertumbuh dalam Kristus, Melayani dengan Kasih."</p>
              <div className="flex gap-4 flex-wrap">
                <button onClick={() => window.location.hash = '/jadwal'} className="bg-gold text-primary px-6 py-3 rounded-lg font-semibold hover:bg-gold/90 flex items-center gap-2"><Icon name="calendar-days" className="w-5 h-5"/> Jadwal Ibadah</button>
                <button onClick={() => window.location.hash = '/tentang'} className="border-2 border-white text-white px-6 py-3 rounded-lg font-semibold hover:bg-white hover:text-primary">Tentang Kami</button>
              </div>
            </div>
            <div className="relative w-full max-w-sm mx-auto md:ml-auto rounded-2xl overflow-hidden shadow-2xl ring-4 ring-white/30 dark:ring-gray-700/50 bg-white/10 backdrop-blur-sm">
              <img src={siteSettings.heroSideImage} className="w-full h-auto block object-contain aspect-video" alt="Gereja" />
            </div>
          </div>
        </section>
      );
    };

    const VideoPlayer = ({ src, onClose }) => {
      const videoRef = useRef(null);
      const [playing, setPlaying] = useState(true);
      const [progress, setProgress] = useState(0);
      const [duration, setDuration] = useState(0);
      const togglePlay = () => { if (videoRef.current) { if (videoRef.current.paused) { videoRef.current.play(); setPlaying(true); } else { videoRef.current.pause(); setPlaying(false); } } };
      const handleTimeUpdate = () => { if (videoRef.current && videoRef.current.duration) setProgress((videoRef.current.currentTime / videoRef.current.duration) * 100); };
      const handleSeek = (e) => { const rect = e.currentTarget.getBoundingClientRect(); const newTime = ((e.clientX - rect.left) / rect.width) * videoRef.current.duration; if (videoRef.current) videoRef.current.currentTime = newTime; };

      return (
        <div className="fixed inset-0 bg-black/95 z-[60] flex flex-col items-center justify-center p-4 fade-in" onClick={onClose}>
          <button className="absolute top-6 right-6 text-white bg-red-600 hover:bg-red-700 w-12 h-12 rounded-full flex items-center justify-center z-10" onClick={onClose}><Icon name="x" className="w-6 h-6"/></button>
          <div className="w-full max-w-5xl" onClick={e => e.stopPropagation()}>
            <div className="bg-black rounded-lg overflow-hidden">
              <video ref={videoRef} src={src} controls={false} onTimeUpdate={handleTimeUpdate} onLoadedMetadata={(e) => setDuration(e.target.duration)} onClick={togglePlay} autoPlay className="w-full aspect-video" />
              <div className="flex items-center gap-3 p-3 bg-black/80 text-white">
                <button onClick={togglePlay}>{playing ? <Icon name="pause" className="w-6 h-6"/> : <Icon name="play" className="w-6 h-6"/>}</button>
                <div className="flex-grow h-2 bg-white/20 rounded cursor-pointer" onClick={handleSeek}><div className="h-full bg-gold-accent rounded" style={{width: `${progress}%`}}></div></div>
                <span className="text-xs">{Math.floor(progress/100 * duration)}s / {Math.floor(duration)}s</span>
              </div>
            </div>
          </div>
        </div>
      );
    };

    const MediaPage = ({ navigate, initialView = 'all' }) => {
      const { mediaItems, isAdmin, updateData, showToast } = useApp();
      const [view, setView] = useState(initialView); 
      const [search, setSearch] = useState('');
      const [filterCat, setFilterCat] = useState('All');
      const [lightbox, setLightbox] = useState(null);
      const [videoViewer, setVideoViewer] = useState(null);
      const categories = ['All', 'Ibadah', 'Pelayanan', 'PAM', 'PKB', 'PW', 'PAR', 'KSP', 'Rayon', 'Umum'];
      const publishedMedia = mediaItems.filter(m => !m.status || m.status === 'Published');
      const filteredMedia = publishedMedia.filter(m => {
        const matchType = view === 'all' ? true : (view === 'foto' ? m.type === 'photo' : m.type === 'video');
        const matchSearch = (m.title || "").toLowerCase().includes(search.toLowerCase());
        const matchCat = filterCat === 'All' || m.cat === filterCat;
        return matchType && matchSearch && matchCat;
      });
      useEffect(() => { setView(initialView); }, [initialView]);

      const handleDeleteMedia = (e, id) => { e.stopPropagation(); if (window.confirm("Hapus dokumentasi ini permanen?")) { updateData('mediaItems', mediaItems.filter(m => m.id !== id)); showToast("Media dihapus", "success"); } };

      return (
        <div className="fade-in min-h-screen pb-16 bg-deep-sky text-white">
          <div className="container mx-auto px-4 py-16">
            <h2 className="text-4xl font-bold text-white text-center mb-8">MEDIA & DOKUMENTASI</h2>
            <div className="flex justify-center gap-2 flex-wrap mb-8">
              {['all', 'foto', 'video'].map(v => <button key={v} onClick={() => setView(v)} className={`px-5 py-2 rounded-full text-sm font-semibold transition-colors ${view === v ? 'bg-gold-accent text-deep-sky' : 'bg-white/10 text-white hover:bg-white/20'}`}>{v.charAt(0).toUpperCase() + v.slice(1)}</button>)}
            </div>
            <div className="flex flex-col md:flex-row justify-between mb-8 gap-4">
              <div className="flex gap-2 flex-wrap">{categories.map(c => <button key={c} onClick={() => setFilterCat(c)} className={`px-3 py-1.5 rounded-md text-xs font-semibold ${filterCat === c ? 'bg-white text-deep-sky' : 'bg-white/10 text-white'}`}>{c}</button>)}</div>
              <input type="text" placeholder="Cari..." value={search} onChange={e => setSearch(e.target.value)} className="px-4 py-2 border border-white/20 rounded-lg bg-white/10 text-white text-sm" />
            </div>
            {filteredMedia.length === 0 ? (
              <div className="text-center text-blue-100/60 py-20">Belum ada media yang cocok.</div>
            ) : (
              <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
                {filteredMedia.map(m => (
                  <div key={m.id} className="relative overflow-hidden rounded-lg shadow-lg group h-48 border-2 border-white/20 hover:border-gold-accent transition-all cursor-pointer" onClick={() => m.type === 'photo' ? setLightbox(m) : setVideoViewer(m.url)}>
                    {isAdmin && <button onClick={(e) => handleDeleteMedia(e, m.id)} className="btn-delete-pro absolute top-2 right-2 z-30 p-1.5 rounded-full"><Icon name="trash-2" className="w-3.5 h-3.5" /></button>}
                    {m.type === 'photo' ? <img src={m.url} className="w-full h-full object-cover group-hover:scale-110 transition-transform" alt={m.title} /> : <div className="w-full h-full bg-black flex items-center justify-center relative"><img src={m.thumbnail} className="w-full h-full object-cover absolute inset-0" alt="thumb" /><div className="relative z-10 bg-black/50 p-3 rounded-full"><Icon name="play-circle" className="w-12 h-12 text-white"/></div></div>}
                  </div>
                ))}
              </div>
            )}
          </div>
          {lightbox && <div className="fixed inset-0 bg-black/90 z-50 flex flex-col items-center justify-center p-4 fade-in" onClick={() => setLightbox(null)}><img src={lightbox.url} className="max-w-full max-h-[80vh] rounded-lg shadow-2xl border-4 border-gold-accent" alt="Foto" /><button className="absolute top-6 right-6 text-white bg-gold-accent w-12 h-12 rounded-full flex items-center justify-center" onClick={() => setLightbox(null)}><Icon name="x" className="w-6 h-6"/></button></div>}
          {videoViewer && <VideoPlayer src={videoViewer} onClose={() => setVideoViewer(null)} />}
        </div>
      );
    };

    const SchedulePage = () => {
      const { schedules } = useApp();
      const [filterType, setFilterType] = useState('All');
      const published = schedules.filter(s => (!s.status || s.status === 'Aktif' || s.status === 'Published') && (filterType === 'All' || s.type === filterType));
      const sunday = published.filter(s => s.type === 'Minggu');
      const others = published.filter(s => s.type !== 'Minggu');
      const types = ['All', 'Minggu', 'Unsur', 'KSP', 'Rayon', 'Lainnya'];

      return (
        <div className="bg-bg dark:bg-gray-900 min-h-screen pb-16 fade-in">
          <div className="container mx-auto px-4 py-16">
            <h2 className="text-4xl font-bold text-primary dark:text-white text-center mb-8">Pelayanan Ibadah Jemaat</h2>
            <div className="flex justify-center gap-2 flex-wrap mb-12">
              {types.map(t => <button key={t} onClick={() => setFilterType(t)} className={`px-4 py-2 rounded-lg text-sm font-semibold transition-colors ${filterType === t ? 'bg-primary text-white' : 'bg-gray-200 dark:bg-gray-700 text-gray-700 dark:text-gray-300'}`}>{t}</button>)}
            </div>
            {published.length === 0 && <div className="text-center text-gray-500 py-20">Belum ada jadwal ibadah yang dipublikasikan untuk kategori ini.</div>}
            <div className="grid md:grid-cols-2 gap-8 mb-16">
              {sunday.map(s => (
                <div key={s.id} className="bg-white dark:bg-gray-800 rounded-2xl shadow-2xl overflow-hidden border-t-8 border-primary dark:border-gold">
                  <div className="bg-primary dark:bg-gray-900 p-6 text-center">
                    <h4 className="text-xl font-bold text-gold uppercase mb-2">{s.group}</h4>
                    <p className="text-2xl font-bold text-white">{s.date}</p>
                    <div className="flex items-center justify-center gap-4 text-gray-300 text-sm mt-2">
                      <span className="flex items-center gap-1"><Icon name="clock" className="w-4 h-4" /> {s.time}</span>
                      <span className="flex items-center gap-1"><Icon name="map-pin" className="w-4 h-4" /> {s.place}</span>
                    </div>
                  </div>
                  <div className="p-8 flex flex-col items-center text-center">
                    <div className="w-32 h-32 rounded-full bg-gray-100 border-4 border-gold flex items-center justify-center text-gray-400 overflow-hidden mb-4">
                      {s.preacherPhoto ? <img src={s.preacherPhoto} className="w-full h-full object-cover" alt={s.preacher} /> : <Icon name="user" className="w-12 h-12" />}
                    </div>
                    <p className="text-sm text-gray-500 mb-1">Pelayan Firman</p>
                    <h5 className="text-lg font-bold text-primary dark:text-white mb-6">{s.preacher}</h5>
                    <div className="border-t border-b border-gray-200 dark:border-gray-700 py-4 w-full">
                      <p className="text-sm text-gray-500 mb-2">Tema Khotbah:</p>
                      <h5 className="text-xl font-bold text-crimson italic mb-2">"{s.theme}"</h5>
                      <p className="text-sm font-semibold text-gray-700 dark:text-gray-200">{s.verse}</p>
                    </div>
                  </div>
                </div>
              ))}
            </div>
            {others.length > 0 && (
              <div className="bg-white dark:bg-gray-800 rounded-2xl shadow-xl overflow-hidden border-t-8 border-teal mb-12">
                <h3 className="text-xl font-bold text-teal p-4 bg-teal/10">Jadwal Ibadah KSP, Rayon & Unsur</h3>
                <table className="w-full text-left">
                  <thead className="bg-teal text-white text-sm"><tr><th className="p-4">Keterangan</th><th className="p-4 hidden md:table-cell">Tanggal</th><th className="p-4 hidden md:table-cell">Pelayan</th><th className="p-4">Tema</th></tr></thead>
                  <tbody>
                    {others.map(s => (
                      <tr key={s.id} className="border-t dark:border-gray-700 text-sm hover:bg-tealLight dark:hover:bg-gray-700">
                        <td className="p-4 font-bold text-teal dark:text-teal-400">{s.group} <span className="block text-xs text-gray-400 mt-1">{s.type}</span></td>
                        <td className="p-4 hidden md:table-cell text-gray-700 dark:text-gray-300">{s.date} <br/> <span className="text-xs">{s.time}</span></td>
                        <td className="p-4 hidden md:table-cell font-medium text-primary dark:text-white">{s.preacher}</td>
                        <td className="p-4 text-gray-600 dark:text-gray-400 italic">"{s.theme}" <br/> <span className="text-xs font-bold not-italic">{s.verse}</span></td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            )}
          </div>
        </div>
      );
    };

    const NewsPage = () => { const { news } = useApp(); const pub = news.filter(n => !n.status || n.status === 'Published' || n.status === 'Terbit'); return (<div className="fade-in min-h-screen pb-16 bg-deep-sky text-white"><div className="container mx-auto px-4 py-16"><h2 className="text-4xl font-bold text-white text-center mb-12">Berita & Informasi</h2><div className="grid md:grid-cols-3 gap-6">{pub.map(n => (<div key={n.id} className="bg-white/95 rounded-xl overflow-hidden shadow-lg border-t-4 border-gold-accent flex flex-col"><div className="h-48 bg-gray-300 flex items-center justify-center text-white p-4 text-center font-semibold overflow-hidden">{n.imageUrl ? <img src={n.imageUrl} className="w-full h-full object-cover" alt={n.title} /> : <span>{n.cat}</span>}</div><div className="p-6 text-gray-800 flex flex-col flex-grow"><p className="text-xs text-gray-500 mb-2">{formatDate(n.date)}</p><h3 className="text-lg font-bold text-deep-sky mb-3">{n.title}</h3><p className="text-sm text-gray-600 flex-grow mb-4 line-clamp-3">{n.excerpt}</p><span className="text-sm font-semibold text-deep-sky">Baca Selengkapnya →</span></div></div>))}{pub.length === 0 && <div className="col-span-3 text-center text-blue-100/60 py-20">Belum ada berita.</div>}</div></div></div>); };
    const SermonPage = () => { const { sermons } = useApp(); return (<div className="fade-in bg-bg dark:bg-gray-900 min-h-screen pb-16"><div className="container mx-auto px-4 py-16"><h2 className="text-4xl font-bold text-primary dark:text-white text-center mb-12">Rancangan Khotbah</h2><div className="space-y-8 max-w-4xl mx-auto">{sermons.map(s => (<div key={s.id} className="bg-white dark:bg-gray-800 rounded-2xl shadow-lg overflow-hidden border-l-8 border-gold flex flex-col md:flex-row"><div className="bg-primary dark:bg-gray-900 p-6 md:w-64 flex flex-col justify-center items-center text-center"><p className="text-sm text-gray-300 mb-2">Tema</p><h3 className="text-lg font-bold text-gold uppercase italic mb-4">"{s.theme}"</h3><p className="text-sm font-semibold text-white border-t border-white/20 pt-3">{s.verse}</p></div><div className="p-8 flex-1"><div className="ml-auto text-right mb-4"><p className="text-xs text-gray-500">Tanggal</p><p className="text-sm font-semibold text-gray-700 dark:text-gray-300">{s.date}</p></div><p className="text-gray-600 dark:text-gray-300 leading-relaxed text-sm">{s.content}</p></div></div>))}{sermons.length === 0 && <div className="text-center text-gray-500 py-20">Belum ada khotbah.</div>}</div></div></div>); };
    const MimbarPage = ({ category }) => { const { mimbarItems } = useApp(); const pub = mimbarItems.filter(item => item.cat === category && (!item.status || item.status === 'Published' || item.status === 'Terbit')); const catCap = category.charAt(0).toUpperCase() + category.slice(1); return (<div className="fade-in min-h-screen pb-16 bg-deep-sky text-white"><div className="container mx-auto px-4 py-16"><div className="max-w-3xl mx-auto text-center mb-12"><h2 className="text-4xl font-bold text-white">Konten Mimbar {catCap}</h2></div><div className="grid md:grid-cols-2 lg:grid-cols-3 gap-8">{pub.map(item => (<div key={item.id} className="bg-white/95 rounded-xl overflow-hidden shadow-lg flex flex-col">{item.imageUrl && <div className="h-56 w-full overflow-hidden bg-gray-100"><img src={item.imageUrl} className="w-full h-full object-cover" alt={item.title} /></div>}<div className="p-6 flex flex-col flex-grow text-gray-800"><h3 className="text-lg font-bold text-deep-sky mb-3">{item.title}</h3><div className="flex items-center gap-3 text-xs text-gray-500 mb-3"><span>{item.author}</span><span>{item.date}</span></div><p className="text-sm text-gray-600 flex-grow mb-4 line-clamp-3">{item.excerpt}</p></div></div>))}{pub.length === 0 && <div className="text-center text-blue-100/60 py-20">Belum ada konten.</div>}</div></div></div>); };
    const AboutPage = () => { const { aboutContent, greeting } = useApp(); return (<div className="fade-in min-h-screen pb-16 bg-deep-sky text-white"><div className="container mx-auto px-4 py-16"><div className="max-w-5xl mx-auto"><div className="text-center mb-16"><h2 className="text-4xl font-bold text-white mb-2">Tentang Kami</h2></div>{greeting.isVisible && <div className="flex flex-col md:flex-row items-center gap-10 mb-16 bg-white/5 p-8 rounded-2xl border border-white/10"><div className="w-40 h-40 rounded-full bg-white/10 border-4 border-gold-accent/50 flex items-center justify-center text-blue-100/60 overflow-hidden shrink-0">{greeting.photoBase64 ? <img src={greeting.photoBase64} className="w-full h-full object-cover" alt="Pimpinan" /> : <Icon name="user" className="w-20 h-20" />}</div><div className="text-center md:text-left"><h3 className="text-2xl font-bold text-gold-accent mb-4">{greeting.name}</h3><p className="text-blue-100/90 leading-relaxed italic">"{greeting.text}"</p></div></div>}<div className="mb-12"><h3 className="text-2xl font-bold text-gold-accent mb-4">Profil Gereja</h3><div className="bg-white/5 p-6 rounded-xl border border-white/10 text-blue-100/90 leading-relaxed whitespace-pre-wrap">{aboutContent.profil}</div></div><div className="mb-12"><h3 className="text-2xl font-bold text-gold-accent mb-4">Sejarah</h3><div className="bg-white/5 p-6 rounded-xl border border-white/10 text-blue-100/90 leading-relaxed whitespace-pre-wrap">{aboutContent.sejarah}</div></div><div className="mb-12"><h3 className="text-2xl font-bold text-gold-accent mb-4">Visi & Misi</h3><div className="bg-white/5 p-6 rounded-xl border border-white/10 text-blue-100/90 leading-relaxed whitespace-pre-wrap"><p className="font-bold mb-2">Visi:</p>{aboutContent.visi}<p className="font-bold mt-4 mb-2">Misi:</p>{aboutContent.misi}</div></div><div className="mb-12"><h3 className="text-2xl font-bold text-gold-accent mb-4">Struktur Organisasi</h3><div className="bg-white/5 p-6 rounded-xl border border-white/10 text-blue-100/90 leading-relaxed whitespace-pre-wrap">{aboutContent.struktur}</div></div><div><h3 className="text-2xl font-bold text-gold-accent mb-4">Informasi Ketua Jemaat</h3><div className="bg-white/5 p-6 rounded-xl border border-white/10 text-blue-100/90 leading-relaxed whitespace-pre-wrap">{aboutContent.infoKetua}</div></div></div></div></div>); };

    const ContactPage = () => {
      const { siteSettings, showToast, setAppData } = useApp();
      const [form, setForm] = useState({ name: '', email: '', message: '' });
      const handleSubmit = (e) => {
        e.preventDefault();
        const newMsg = { id: uid(), ...form, timestamp: Date.now(), status: 'Baru', source: 'Formulir Website' };
        setAppData(prev => ({ ...prev, waMessages: [newMsg, ...prev.waMessages] }));
        showToast("Pesan tercatat di sistem! Mengarahkan ke WhatsApp...", "success");
        const waText = `Shalom, saya ${form.name}.\nEmail: ${form.email}\n\nPesan:\n${form.message}`;
        setTimeout(() => window.open(`https://wa.me/${siteSettings.whatsapp}?text=${encodeURIComponent(waText)}`, '_blank'), 1000);
        setForm({ name: '', email: '', message: '' });
      };
      return (
        <div className="fade-in bg-skyLight dark:bg-gray-900 min-h-screen pb-16">
          <div className="container mx-auto px-4 py-16">
            <h2 className="text-4xl font-bold text-sky dark:text-white text-center mb-12">Hubungi Kami</h2>
            <div className="grid md:grid-cols-2 gap-12">
              <div>
                <div className="space-y-4 text-gray-700 dark:text-gray-300 bg-white dark:bg-gray-800 p-6 rounded-xl shadow-sm border-l-4 border-sky">
                  <p className="flex gap-3"><Icon name="map-pin" className="w-5 h-5 text-sky shrink-0"/> {siteSettings.alamat}</p>
                  <a href={`https://wa.me/${siteSettings.whatsapp}`} target="_blank" rel="noopener noreferrer" className="flex gap-3 items-center hover:text-sky"><Icon name="message-circle" className="w-5 h-5 text-sky shrink-0"/> Chat WhatsApp: <span className="font-semibold text-sky underline">{siteSettings.telepon}</span></a>
                  <p className="flex gap-3"><Icon name="mail" className="w-5 h-5 text-sky shrink-0"/> {siteSettings.email}</p>
                </div>
                <div className="mt-8 rounded-xl overflow-hidden shadow-lg border-4 border-white dark:border-gray-700"><iframe title="Lokasi" src="https://www.google.com/maps?q=-0.7516163,135.7615763&z=14&output=embed&hl=id&maptype=satellite" width="100%" height="300" style={{ border: 0 }} allowFullScreen="" loading="lazy"></iframe></div>
              </div>
              <div className="bg-white dark:bg-gray-800 p-8 rounded-xl shadow-md border-t-4 border-sky">
                <form onSubmit={handleSubmit} className="space-y-4">
                  <p className="text-gray-500 text-sm mb-4">Pesan dikirim langsung melalui WhatsApp Admin.</p>
                  <input required type="text" placeholder="Nama Lengkap" value={form.name} onChange={e => setForm({...form, name: e.target.value})} className="w-full px-4 py-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-2 focus:ring-sky focus:outline-none" />
                  <input required type="email" placeholder="Email" value={form.email} onChange={e => setForm({...form, email: e.target.value})} className="w-full px-4 py-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-2 focus:ring-sky focus:outline-none" />
                  <textarea required rows="4" placeholder="Pesan Anda" value={form.message} onChange={e => setForm({...form, message: e.target.value})} className="w-full px-4 py-3 border rounded-lg dark:bg-gray-700 dark:border-gray-600 focus:ring-2 focus:ring-sky focus:outline-none"></textarea>
                  <button className="w-full bg-sky text-white py-3 rounded-lg font-semibold hover:bg-sky/90">KIRIM PESAN VIA WHATSAPP</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      );
    };

    const AdminDashboard = ({ navigate }) => {
      const { isAdmin, updateData, news, sermons, schedules, mimbarItems, greeting, aboutContent, albums, mediaItems, siteSettings, setAppData, logout, showToast, waMessages } = useApp();
      const [view, setView] = useState('dashboard');
      const [editingItem, setEditingItem] = useState(null);
      const [schedType, setSchedType] = useState('Minggu');

      useEffect(() => { if (!isAdmin) navigate('/login'); }, [isAdmin]);
      const DeleteBtn = ({ onClick }) => <button type="button" onClick={onClick} className="btn-delete-pro inline-flex items-center gap-1 text-xs font-semibold px-3 py-2 rounded-md shadow-sm"><Icon name="trash-2" className="w-3.5 h-3.5" /> Hapus</button>;
      const EditBtn = ({ onClick }) => <button type="button" onClick={onClick} className="inline-flex items-center gap-1 text-xs font-semibold px-3 py-2 rounded-md shadow-sm bg-indigo-100 text-indigo-700 hover:bg-indigo-600 hover:text-white"><Icon name="edit" className="w-3.5 h-3.5" /> Edit</button>;
      const resetForm = () => { setEditingItem(null); setSchedType('Minggu'); };

      const submitNews = (e) => { e.preventDefault(); const fd = new FormData(e.target); const data = { id: editingItem?.id || uid(), date: fd.get('date'), title: fd.get('title'), cat: fd.get('cat'), author: fd.get('author'), location: fd.get('location'), imageUrl: fd.get('imageUrl'), excerpt: fd.get('excerpt'), status: fd.get('status') || 'Published', createdAt: editingItem?.createdAt || Date.now() }; updateData('news', editingItem ? news.map(n => n.id === editingItem.id ? data : n) : [data, ...news]); e.target.reset(); resetForm(); showToast(editingItem ? "Berita diperbarui" : "Berita ditambahkan"); };
      const submitSermon = (e) => { e.preventDefault(); const fd = new FormData(e.target); const data = { id: editingItem?.id || uid(), date: fd.get('date'), preacher: fd.get('preacher'), theme: fd.get('theme'), verse: fd.get('verse'), cat: fd.get('cat'), coverUrl: fd.get('coverUrl'), content: fd.get('content'), status: fd.get('status') || 'Published', createdAt: editingItem?.createdAt || Date.now() }; updateData('sermons', editingItem ? sermons.map(s => s.id === editingItem.id ? data : s) : [data, ...sermons]); e.target.reset(); resetForm(); showToast(editingItem ? "Khotbah diperbarui" : "Khotbah ditambahkan"); };
      const submitSchedule = (e) => { e.preventDefault(); const fd = new FormData(e.target); const type = fd.get('type'); let group = fd.get('group'); if (type === 'KSP' || type === 'Rayon') group = `${type} ${fd.get('groupNumber')}`; if (!group || !fd.get('date') || !fd.get('preacher') || !fd.get('theme')) { showToast("Harap isi kolom wajib.", "error"); return; } const data = { id: editingItem?.id || uid(), type: type, group: group, date: fd.get('date'), time: fd.get('time'), endTime: fd.get('endTime'), place: fd.get('place'), preacher: fd.get('preacher'), liturgis: fd.get('liturgis'), preacherPhoto: fd.get('preacherPhoto'), theme: fd.get('theme'), verse: fd.get('verse'), notes: fd.get('notes'), status: fd.get('status') || 'Aktif', createdAt: editingItem?.createdAt || Date.now() }; updateData('schedules', editingItem ? schedules.map(s => s.id === editingItem.id ? data : s) : [data, ...schedules]); e.target.reset(); resetForm(); showToast(editingItem ? "Jadwal diperbarui" : "Jadwal ditambahkan"); };
      const submitMimbar = (e) => { e.preventDefault(); const fd = new FormData(e.target); const data = { id: editingItem?.id || uid(), cat: fd.get('cat'), title: fd.get('title'), author: fd.get('author'), date: fd.get('date'), imageUrl: fd.get('imageUrl'), excerpt: fd.get('excerpt'), content: fd.get('content'), status: fd.get('status') || 'Published', createdAt: editingItem?.createdAt || Date.now() }; updateData('mimbarItems', editingItem ? mimbarItems.map(m => m.id === editingItem.id ? data : m) : [data, ...mimbarItems]); e.target.reset(); resetForm(); showToast(editingItem ? "Mimbar diperbarui" : "Mimbar ditambahkan"); };
      const submitVideo = (e) => { e.preventDefault(); const fd = new FormData(e.target); let url = fd.get('url') || ''; const vidFile = document.getElementById('vid-file-input'); if (vidFile && vidFile.files[0]) { const file = vidFile.files[0]; if (file.size > 10 * 1024 * 1024) { showToast("Ukuran video lokal maks 10MB. Gunakan link YouTube untuk file besar.", "error"); return; } const reader = new FileReader(); reader.onloadend = () => { url = reader.result; const data = { id: uid(), type: 'video', title: fd.get('title'), url: url, thumbnail: fd.get('thumbnail') || "", cat: fd.get('cat'), date: fd.get('date'), status: fd.get('status') || 'Published', createdAt: Date.now() }; updateData('mediaItems', [data, ...mediaItems]); e.target.reset(); showToast("Video permanen ditambahkan"); }; reader.readAsDataURL(file); } else { if (!url) { showToast("Upload file atau masukkan link YouTube", "error"); return; } if (url.includes("watch?v=")) url = url.replace("watch?v=", "embed/"); else if (url.includes("youtu.be/")) url = url.replace("youtu.be/", "www.youtube.com/embed/"); const data = { id: uid(), type: 'video', title: fd.get('title'), url: url, thumbnail: fd.get('thumbnail') || "", cat: fd.get('cat'), date: fd.get('date'), status: fd.get('status') || 'Published', createdAt: Date.now() }; updateData('mediaItems', [data, ...mediaItems]); e.target.reset(); showToast("Video ditambahkan"); } };
      const submitPhoto = (e) => { e.preventDefault(); const fd = new FormData(e.target); const data = { id: uid(), type: 'photo', title: fd.get('title'), url: fd.get('photoUrl'), cat: fd.get('cat'), date: fd.get('date'), status: fd.get('status') || 'Published', createdAt: Date.now() }; updateData('mediaItems', [data, ...mediaItems]); e.target.reset(); showToast("Foto ditambahkan"); };

      const navItems = ['dashboard', 'settings', 'greeting', 'about', 'sermon', 'news', 'schedule', 'mimbar', 'media', 'messages'];
      const icons = {dashboard:'home', settings:'settings', greeting:'message-square', about:'info', sermon:'book-open', news:'newspaper', schedule:'calendar', mimbar:'file-text', media:'image', messages:'mail'};
      const unreadCount = waMessages.filter(m => m.status === 'Baru').length;

      return (
        <div className="flex flex-col md:flex-row min-h-screen bg-skyLight dark:bg-gray-900 fade-in">
          <aside className="w-full md:w-64 bg-gradient-to-b from-sky to-blue-900 text-white p-6 shadow-xl md:h-screen md:sticky md:top-0 overflow-y-auto">
            <div className="flex items-center gap-2 mb-8"><Icon name="shield-check" className="w-8 h-8 text-amber-400" /><div><h2 className="text-lg font-bold">CMS Admin</h2><p className="text-xs text-blue-200">Eden Mansoben</p></div></div>
            <nav className="space-y-1">
              {navItems.map(v => (
                <button key={v} onClick={() => { setView(v); resetForm(); }} className={`w-full text-left px-4 py-2.5 rounded-lg flex items-center gap-3 capitalize ${view === v ? 'bg-white text-sky font-bold' : 'hover:bg-white/10'}`}>
                  <Icon name={icons[v]} className="w-5 h-5"/> {v}
                  {v === 'messages' && unreadCount > 0 && <span className="ml-auto bg-red-500 text-white text-xs px-2 py-0.5 rounded-full">{unreadCount}</span>}
                </button>
              ))}
              <button onClick={() => { logout(); navigate('/'); }} className="w-full text-left px-4 py-2.5 rounded-lg text-amber-300 hover:bg-amber-500/20 flex items-center gap-3 mt-4"><Icon name="log-out" className="w-5 h-5"/> Logout</button>
            </nav>
          </aside>
          
          <main className="flex-1 p-4 md:p-8 overflow-y-auto">
            {/* DASHBOARD VIEW */}
            {view === 'dashboard' && (
              <div>
                <h1 className="text-2xl font-bold text-sky mb-6">Dashboard</h1>
                <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
                  {[{t:"Berita", v:news.length, i:"newspaper", c:"border-sky"}, {t:"Khotbah", v:sermons.length, i:"book-open", c:"border-indigo-500"}, {t:"Jadwal", v:schedules.length, i:"calendar", c:"border-teal-500"}, {t:"Media", v:mediaItems.length, i:"image", c:"border-pink-500"}, {t:"Pesan Baru", v:unreadCount, i:"mail", c:"border-amber-500"}].map(s => (
                    <div key={s.t} className={`bg-white p-4 rounded-xl shadow-sm flex items-center justify-between border-b-4 ${s.c}`}>
                      <div><p className="text-gray-500 text-sm">{s.t}</p><h3 className="text-3xl font-bold text-gray-800">{s.v}</h3></div>
                      <div className={`${s.c.replace('border-', 'bg-')} p-3 rounded-lg text-white shadow-md`}><Icon name={s.i} className="w-6 h-6"/></div>
                    </div>
                  ))}
                </div>
                <div className="bg-white p-6 rounded-xl shadow-sm border-t-4 border-sky mb-8">
                  <h3 className="text-lg font-bold text-sky mb-4">Tombol Pintas (Quick Actions)</h3>
                  <div className="flex flex-wrap gap-4">
                    <button onClick={() => { setView('news'); resetForm(); }} className="flex items-center gap-2 bg-sky text-white px-4 py-2 rounded-lg font-semibold hover:bg-sky/90"><Icon name="plus" className="w-4 h-4"/> Tambah Berita</button>
                    <button onClick={() => { setView('sermon'); resetForm(); }} className="flex items-center gap-2 bg-indigo-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-indigo-700"><Icon name="plus" className="w-4 h-4"/> Tambah Khotbah</button>
                    <button onClick={() => { setView('schedule'); resetForm(); }} className="flex items-center gap-2 bg-amber-500 text-white px-4 py-2 rounded-lg font-semibold hover:bg-amber-600"><Icon name="plus" className="w-4 h-4"/> Tambah Jadwal</button>
                    <button onClick={() => { setView('media'); resetForm(); }} className="flex items-center gap-2 bg-pink-500 text-white px-4 py-2 rounded-lg font-semibold hover:bg-pink-600"><Icon name="plus" className="w-4 h-4"/> Tambah Media</button>
                  </div>
                </div>
                <div className="bg-white p-6 rounded-xl shadow-sm border-t-4 border-amber-500">
                  <h3 className="text-lg font-bold text-amber-600 mb-4">Backup & Restore Data</h3>
                  <div className="flex flex-wrap gap-4">
                    <button onClick={() => { const dataStr = JSON.stringify({ darkMode: false, siteSettings, news, sermons, schedules, mimbarItems, greeting, aboutContent, albums, mediaItems, waMessages }, null, 2); const blob = new Blob([dataStr], { type: "application/json" }); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = `backup-${new Date().toISOString().split('T')[0]}.json`; a.click(); URL.revokeObjectURL(url); showToast("Backup diunduh"); }} className="flex items-center gap-2 bg-amber-500 text-white px-6 py-3 rounded-lg font-semibold hover:bg-amber-600"><Icon name="download" className="w-5 h-5"/> Export Backup</button>
                    <button onClick={() => document.getElementById('restore-file').click()} className="flex items-center gap-2 bg-gray-200 text-gray-700 px-6 py-3 rounded-lg font-semibold hover:bg-gray-300"><Icon name="upload" className="w-5 h-5"/> Restore Backup</button>
                    <input id="restore-file" type="file" accept="application/json" className="hidden" onChange={(e) => { const file = e.target.files[0]; if (!file) return; const reader = new FileReader(); reader.onload = (event) => { try { const data = JSON.parse(event.target.result); if (window.confirm("Restore akan mengganti data. Lanjutkan?")) { setAppData(prev => ({ ...prev, ...data })); showToast("Data dipulihkan"); setTimeout(() => window.location.reload(), 1500); } } catch (err) { showToast("File tidak valid", "error"); } }; reader.readAsText(file); }} />
                  </div>
                </div>
              </div>
            )}

            {/* SETTINGS VIEW */}
            {view === 'settings' && (
              <div>
                <h1 className="text-2xl font-bold text-sky mb-6">Pengaturan Website</h1>
                <div className="bg-white p-8 rounded-xl shadow-sm border-t-4 border-sky max-w-3xl space-y-4">
                  <div className="grid md:grid-cols-2 gap-4">
                    <div><label className="block text-xs font-medium text-gray-700 mb-1">Nama Gereja</label><input type="text" value={siteSettings.namaGereja} onChange={e => setAppData(prev => ({...prev, siteSettings: {...prev.siteSettings, namaGereja: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                    <div><label className="block text-xs font-medium text-gray-700 mb-1">WhatsApp (62...)</label><input type="text" value={siteSettings.whatsapp} onChange={e => setAppData(prev => ({...prev, siteSettings: {...prev.siteSettings, whatsapp: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                    <div><label className="block text-xs font-medium text-gray-700 mb-1">Telepon</label><input type="text" value={siteSettings.telepon} onChange={e => setAppData(prev => ({...prev, siteSettings: {...prev.siteSettings, telepon: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                    <div><label className="block text-xs font-medium text-gray-700 mb-1">Email</label><input type="email" value={siteSettings.email} onChange={e => setAppData(prev => ({...prev, siteSettings: {...prev.siteSettings, email: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  </div>
                  <FileUploader label="Logo Gereja" onUpload={(url) => setAppData(prev => ({...prev, siteSettings: {...prev.siteSettings, logo: url}}))} />
                  <FileUploader label="Gambar Latar Beranda" onUpload={(url) => setAppData(prev => ({...prev, siteSettings: {...prev.siteSettings, heroImage: url}}))} />
                  <div className="flex gap-2 pt-4">
                    <button onClick={() => showToast('Pengaturan disimpan')} className="bg-sky text-white px-6 py-3 rounded-lg font-semibold hover:bg-sky/90">Simpan Pengaturan</button>
                    <button onClick={() => { if (window.confirm('Pulihkan pengaturan ke default? Semua data pengaturan akan kembali ke awal.')) { setAppData(prev => ({...prev, siteSettings: defaultData.siteSettings})); showToast('Pengaturan dipulihkan ke default'); } }} className="bg-gray-200 text-gray-700 px-6 py-3 rounded-lg font-semibold hover:bg-gray-300">Pulihkan Default</button>
                  </div>
                </div>
              </div>
            )}

            {/* GREETING VIEW */}
            {view === 'greeting' && (
              <div>
                <h1 className="text-2xl font-bold text-sky mb-6">Sambutan Pimpinan</h1>
                <div className="bg-white p-8 rounded-xl shadow-sm border-t-4 border-sky max-w-3xl space-y-4">
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Teks Sambutan</label><textarea rows="4" value={greeting.text} onChange={e => setAppData(prev => ({...prev, greeting: {...prev.greeting, text: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Nama Pimpinan</label><input type="text" value={greeting.name} onChange={e => setAppData(prev => ({...prev, greeting: {...prev.greeting, name: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <FileUploader label="Upload Foto Pimpinan" onUpload={(url) => setAppData(prev => ({...prev, greeting: {...prev.greeting, photoBase64: url}}))} />
                  {greeting.photoBase64 && <img src={greeting.photoBase64} className="w-24 h-24 rounded-full object-cover" alt="Preview" />}
                  <div className="flex items-center gap-2"><input type="checkbox" id="greetVisible" checked={greeting.isVisible} onChange={e => setAppData(prev => ({...prev, greeting: {...prev.greeting, isVisible: e.target.checked}}))} className="w-4 h-4" /><label htmlFor="greetVisible" className="text-sm text-gray-700">Tampilkan di halaman publik</label></div>
                  <button onClick={() => showToast('Sambutan disimpan')} className="bg-sky text-white px-6 py-3 rounded-lg font-semibold">Simpan</button>
                </div>
              </div>
            )}

            {/* ABOUT VIEW */}
            {view === 'about' && (
              <div>
                <h1 className="text-2xl font-bold text-sky mb-6">Tentang Kami</h1>
                <div className="bg-white p-8 rounded-xl shadow-sm border-t-4 border-sky max-w-3xl space-y-4">
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Profil</label><textarea rows="3" value={aboutContent.profil} onChange={e => setAppData(prev => ({...prev, aboutContent: {...prev.aboutContent, profil: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Sejarah</label><textarea rows="3" value={aboutContent.sejarah} onChange={e => setAppData(prev => ({...prev, aboutContent: {...prev.aboutContent, sejarah: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Visi</label><textarea rows="3" value={aboutContent.visi} onChange={e => setAppData(prev => ({...prev, aboutContent: {...prev.aboutContent, visi: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Misi</label><textarea rows="3" value={aboutContent.misi} onChange={e => setAppData(prev => ({...prev, aboutContent: {...prev.aboutContent, misi: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Struktur Organisasi</label><textarea rows="3" value={aboutContent.struktur} onChange={e => setAppData(prev => ({...prev, aboutContent: {...prev.aboutContent, struktur: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <div><label className="block text-xs font-medium text-gray-700 mb-1">Info Ketua Jemaat</label><textarea rows="3" value={aboutContent.infoKetua} onChange={e => setAppData(prev => ({...prev, aboutContent: {...prev.aboutContent, infoKetua: e.target.value}}))} className="w-full px-4 py-3 border rounded-lg" /></div>
                  <button onClick={() => showToast('Tentang Kami disimpan')} className="bg-sky text-white px-6 py-3 rounded-lg font-semibold">Simpan</button>
                </div>
              </div>
            )}

            {/* SERMON VIEW */}
            {view === 'sermon' && (
              <div>
                <h1 className="text-2xl font-bold text-indigo-600 mb-6">Manajemen Khotbah</h1>
                <div className="bg-white p-6 rounded-2xl shadow-sm mb-8 border-t-4 border-indigo-500">
                  <h3 className="text-lg font-bold text-indigo-600 mb-4">{editingItem ? 'Edit Khotbah' : 'Tambah Khotbah'}</h3>
                  <form onSubmit={submitSermon} key={editingItem ? editingItem.id : 'new'} className="grid lg:grid-cols-2 gap-6">
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tanggal</label><input required name="date" type="text" placeholder="09 Agustus 2026" defaultValue={editingItem?.date || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Pelayan Firman</label><input required name="preacher" type="text" defaultValue={editingItem?.preacher || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Kategori</label><input name="cat" type="text" placeholder="Umum / Minggu Pagi" defaultValue={editingItem?.cat || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Status</label><select name="status" defaultValue={editingItem?.status || "Published"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none"><option value="Published">Terbit</option><option value="Draft">Draf</option></select></div>
                    </div>
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tema</label><input required name="theme" type="text" defaultValue={editingItem?.theme || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Ayat</label><input required name="verse" type="text" defaultValue={editingItem?.verse || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" /></div>
                      <FileUploader label="Foto Sampul (Opsional)" onUpload={(url) => document.getElementById('sermon-cover').value = url} />
                      <input id="sermon-cover" name="coverUrl" type="hidden" defaultValue={editingItem?.coverUrl || ""} />
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Isi Khotbah</label><textarea required name="content" rows="6" defaultValue={editingItem?.content || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none"></textarea></div>
                      <div className="flex gap-2">
                        <button className="flex-1 bg-indigo-600 text-white py-3 rounded-lg font-semibold hover:bg-indigo-700 flex items-center justify-center gap-2"><Icon name="save" className="w-5 h-5"/> Simpan</button>
                        {editingItem && <button type="button" onClick={resetForm} className="bg-gray-200 px-4 rounded-lg">Batal</button>}
                      </div>
                    </div>
                  </form>
                </div>
                <div className="bg-white rounded-xl shadow-sm overflow-hidden border border-gray-100">
                  <table className="w-full text-left">
                    <thead className="bg-gray-100 text-gray-700 text-sm"><tr><th className="p-4">Tema</th><th className="p-4 text-right">Aksi</th></tr></thead>
                    <tbody>
                      {sermons.map(s => <tr key={s.id} className="border-t text-sm hover:bg-gray-50"><td className="p-4 font-medium text-gray-800">"{s.theme}"</td><td className="p-4 text-right flex gap-1 justify-end"><EditBtn onClick={() => setEditingItem(s)} /><DeleteBtn onClick={() => { if (window.confirm('Hapus khotbah ini?')) { updateData('sermons', sermons.filter(x => x.id !== s.id)); showToast("Khotbah dihapus"); } }} /></td></tr>)}
                      {sermons.length === 0 && <tr><td colSpan="2" className="p-8 text-center text-gray-400">Belum ada khotbah.</td></tr>}
                    </tbody>
                  </table>
                </div>
              </div>
            )}

            {/* NEWS VIEW */}
            {view === 'news' && (
              <div>
                <h1 className="text-2xl font-bold text-teal-600 mb-6">Manajemen Berita</h1>
                <div className="bg-white p-6 rounded-2xl shadow-sm mb-8 border-t-4 border-teal-500">
                  <h3 className="text-lg font-bold text-teal-600 mb-4">{editingItem ? 'Edit Berita' : 'Tambah Berita'}</h3>
                  <form onSubmit={submitNews} key={editingItem ? editingItem.id : 'new'} className="grid md:grid-cols-2 gap-5">
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tanggal</label><input required name="date" type="date" defaultValue={editingItem?.date ? new Date(editingItem.date).toISOString().substr(0,10) : ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Kategori</label><select required name="cat" defaultValue={editingItem?.cat || "Umum"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none"><option>Umum</option><option>PKB</option><option>PW</option><option>PAM</option><option>Pemuda</option></select></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Penulis</label><input name="author" type="text" defaultValue={editingItem?.author || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Lokasi</label><input name="location" type="text" defaultValue={editingItem?.location || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none" /></div>
                      <FileUploader label="Upload Gambar" onUpload={(url) => document.getElementById('news-img').value = url} />
                      <input id="news-img" name="imageUrl" type="hidden" defaultValue={editingItem?.imageUrl || ""} />
                    </div>
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Judul</label><input required name="title" type="text" defaultValue={editingItem?.title || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Isi Berita</label><textarea required name="excerpt" rows="4" defaultValue={editingItem?.excerpt || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none"></textarea></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Status</label><select name="status" defaultValue={editingItem?.status || "Published"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-teal-500 focus:outline-none"><option value="Published">Terbit</option><option value="Draft">Draf</option></select></div>
                      <div className="flex gap-2">
                        <button className="flex-1 bg-teal-500 text-white py-3 rounded-lg font-semibold hover:bg-teal-600 flex items-center justify-center gap-2"><Icon name="save" className="w-5 h-5"/> Simpan</button>
                        {editingItem && <button type="button" onClick={resetForm} className="bg-gray-200 px-4 rounded-lg">Batal</button>}
                      </div>
                    </div>
                  </form>
                </div>
                <div className="bg-white rounded-xl shadow-sm overflow-hidden border border-gray-100">
                  <table className="w-full text-left">
                    <thead className="bg-gray-100 text-gray-700 text-sm"><tr><th className="p-4">Judul</th><th className="p-4">Status</th><th className="p-4 text-right">Aksi</th></tr></thead>
                    <tbody>
                      {news.map(n => <tr key={n.id} className="border-t text-sm hover:bg-gray-50"><td className="p-4 font-medium text-gray-800">{n.title}</td><td className="p-4"><span className={`text-xs px-2 py-1 rounded ${n.status === 'Published' ? 'bg-green-100 text-green-700' : 'bg-gray-100'}`}>{n.status}</span></td><td className="p-4 text-right flex gap-1 justify-end"><EditBtn onClick={() => setEditingItem(n)} /><DeleteBtn onClick={() => { if (window.confirm('Hapus berita ini?')) { updateData('news', news.filter(x => x.id !== n.id)); showToast("Berita dihapus"); } }} /></td></tr>)}
                      {news.length === 0 && <tr><td colSpan="3" className="p-8 text-center text-gray-400">Belum ada berita.</td></tr>}
                    </tbody>
                  </table>
                </div>
              </div>
            )}

            {/* SCHEDULE VIEW */}
            {view === 'schedule' && (
              <div>
                <h1 className="text-2xl font-bold text-amber-600 mb-6">Manajemen Jadwal</h1>
                <div className="bg-white p-6 rounded-2xl shadow-sm mb-8 border-t-4 border-amber-500">
                  <h3 className="text-lg font-bold text-amber-600 mb-4">{editingItem ? 'Edit Jadwal' : 'Tambah Jadwal'}</h3>
                  <form onSubmit={submitSchedule} key={editingItem ? editingItem.id : 'new'} className="grid lg:grid-cols-2 gap-6">
                    <div className="space-y-4">
                      <div>
                        <label className="block text-xs font-medium text-gray-700 mb-1">Tipe Ibadah</label>
                        <select required name="type" value={schedType} onChange={e => setSchedType(e.target.value)} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none">
                          <option value="Minggu">Ibadah Minggu (Pagi/Malam)</option>
                          <option value="Unsur">Ibadah Unsur (PAR, PW, PKB, PAM)</option>
                          <option value="KSP">Ibadah KSP (1-13)</option>
                          <option value="Rayon">Ibadah Rayon (1-5)</option>
                          <option value="Lainnya">Ibadah Lainnya</option>
                        </select>
                      </div>
                      {schedType === 'KSP' || schedType === 'Rayon' ? (
                        <div><label className="block text-xs font-medium text-gray-700 mb-1">Nomor {schedType}</label><select required name="groupNumber" defaultValue={editingItem?.group?.split(' ')[1] || "1"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none">{schedType === 'KSP' ? [1,2,3,4,5,6,7,8,9,10,11,12,13].map(n => <option key={n} value={n}>{n}</option>) : [1,2,3,4,5].map(n => <option key={n} value={n}>{n}</option>)}</select></div>
                      ) : schedType === 'Unsur' ? (
                        <div><label className="block text-xs font-medium text-gray-700 mb-1">Keterangan</label><select required name="group" defaultValue={editingItem?.group || "PAR"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none"><option value="PAR">PAR</option><option value="PW">PW</option><option value="PKB">PKB</option><option value="PAM">PAM</option></select></div>
                      ) : (
                        <div><label className="block text-xs font-medium text-gray-700 mb-1">Keterangan</label><input required name="group" type="text" placeholder="Minggu Pagi / Ibadah Keluarga" defaultValue={editingItem?.group || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      )}
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tanggal</label><input required name="date" type="text" placeholder="09 Agustus 2026" defaultValue={editingItem?.date || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div className="grid grid-cols-2 gap-4">
                        <div><label className="block text-xs font-medium text-gray-700 mb-1">Jam Mulai</label><input required name="time" type="text" placeholder="09:00" defaultValue={editingItem?.time || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                        <div><label className="block text-xs font-medium text-gray-700 mb-1">Jam Selesai</label><input name="endTime" type="text" placeholder="11:00" defaultValue={editingItem?.endTime || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      </div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Status</label><select name="status" defaultValue={editingItem?.status || "Aktif"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none"><option value="Aktif">Aktif</option><option value="Selesai">Selesai</option><option value="Dibatalkan">Dibatalkan</option></select></div>
                    </div>
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Pelayan Firman</label><input required name="preacher" type="text" defaultValue={editingItem?.preacher || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Pemimpin Ibadah</label><input name="liturgis" type="text" defaultValue={editingItem?.liturgis || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <FileUploader label="Upload Foto Pelayan" onUpload={(url) => document.getElementById('sched-photo').value = url} />
                      <input id="sched-photo" name="preacherPhoto" type="hidden" defaultValue={editingItem?.preacherPhoto || ""} />
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tempat</label><input required name="place" type="text" defaultValue={editingItem?.place || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tema</label><input required name="theme" type="text" defaultValue={editingItem?.theme || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Ayat</label><input required name="verse" type="text" defaultValue={editingItem?.verse || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Keterangan</label><input name="notes" type="text" defaultValue={editingItem?.notes || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div className="flex gap-2">
                        <button className="flex-1 bg-amber-500 text-white py-3 rounded-lg font-semibold hover:bg-amber-600 flex items-center justify-center gap-2 mt-2"><Icon name="plus" className="w-5 h-5"/> Simpan</button>
                        {editingItem && <button type="button" onClick={resetForm} className="bg-gray-200 px-4 rounded-lg">Batal</button>}
                      </div>
                    </div>
                  </form>
                </div>
                <div className="bg-white rounded-xl shadow-sm overflow-hidden border border-gray-100">
                  <table className="w-full text-left">
                    <thead className="bg-gray-100 text-gray-700 text-sm"><tr><th className="p-4">Keterangan</th><th className="p-4">Status</th><th className="p-4 text-right">Aksi</th></tr></thead>
                    <tbody>
                      {schedules.map(s => <tr key={s.id} className="border-t text-sm hover:bg-gray-50"><td className="p-4 font-medium text-gray-800">{s.group} <span className="text-xs text-gray-400">({s.type})</span></td><td className="p-4"><span className={`text-xs px-2 py-1 rounded ${s.status === 'Aktif' ? 'bg-green-100 text-green-700' : 'bg-gray-100'}`}>{s.status}</span></td><td className="p-4 text-right flex gap-1 justify-end"><EditBtn onClick={() => { setEditingItem(s); setSchedType(s.type); }} /><DeleteBtn onClick={() => { if (window.confirm('Hapus jadwal ini?')) { updateData('schedules', schedules.filter(x => x.id !== s.id)); showToast("Jadwal dihapus"); } }} /></td></tr>)}
                      {schedules.length === 0 && <tr><td colSpan="3" className="p-8 text-center text-gray-400">Belum ada jadwal.</td></tr>}
                    </tbody>
                  </table>
                </div>
              </div>
            )}

            {/* MIMBAR VIEW */}
            {view === 'mimbar' && (
              <div>
                <h1 className="text-2xl font-bold text-amber-600 mb-6">Manajemen Mimbar</h1>
                <div className="bg-white p-6 rounded-2xl shadow-sm mb-8 border-t-4 border-amber-500">
                  <h3 className="text-lg font-bold text-amber-600 mb-4">{editingItem ? 'Edit Mimbar' : 'Tambah Mimbar'}</h3>
                  <form onSubmit={submitMimbar} key={editingItem ? editingItem.id : 'new'} className="grid lg:grid-cols-2 gap-6">
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Kategori</label><select required name="cat" defaultValue={editingItem?.cat || "artikel"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none"><option value="artikel">Artikel</option><option value="khotbah">Khotbah</option><option value="renungan">Renungan</option><option value="puisi">Puisi</option><option value="umum">Umum</option></select></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Judul</label><input required name="title" type="text" defaultValue={editingItem?.title || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Penulis</label><input required name="author" type="text" defaultValue={editingItem?.author || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tanggal</label><input required name="date" type="text" defaultValue={editingItem?.date || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none" /></div>
                      <FileUploader label="Upload Gambar/Poster" onUpload={(url) => document.getElementById('mimbar-img').value = url} />
                      <input id="mimbar-img" name="imageUrl" type="hidden" defaultValue={editingItem?.imageUrl || ""} />
                    </div>
                    <div className="space-y-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Ringkasan</label><textarea required name="excerpt" rows="4" defaultValue={editingItem?.excerpt || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none"></textarea></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Isi Lengkap Tulisan</label><textarea required name="content" rows="6" defaultValue={editingItem?.content || ""} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none"></textarea></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Status</label><select name="status" defaultValue={editingItem?.status || "Published"} className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-amber-500 focus:outline-none"><option value="Published">Terbit</option><option value="Draft">Draf</option></select></div>
                      <div className="flex gap-2">
                        <button className="flex-1 bg-amber-500 text-white py-3 rounded-lg font-semibold hover:bg-amber-600 flex items-center justify-center gap-2"><Icon name="save" className="w-5 h-5"/> Publikasi</button>
                        {editingItem && <button type="button" onClick={resetForm} className="bg-gray-200 px-4 rounded-lg">Batal</button>}
                      </div>
                    </div>
                  </form>
                </div>
                <div className="bg-white rounded-xl shadow-sm overflow-hidden border border-gray-100">
                  <table className="w-full text-left">
                    <thead className="bg-gray-100 text-gray-700 text-sm"><tr><th className="p-4">Judul</th><th className="p-4">Kategori</th><th className="p-4 text-right">Aksi</th></tr></thead>
                    <tbody>
                      {mimbarItems.map(m => <tr key={m.id} className="border-t text-sm hover:bg-gray-50"><td className="p-4 font-medium text-gray-800">{m.title}</td><td className="p-4 capitalize">{m.cat}</td><td className="p-4 text-right flex gap-1 justify-end"><EditBtn onClick={() => setEditingItem(m)} /><DeleteBtn onClick={() => { if (window.confirm('Hapus konten ini?')) { updateData('mimbarItems', mimbarItems.filter(x => x.id !== m.id)); showToast("Mimbar dihapus"); } }} /></td></tr>)}
                      {mimbarItems.length === 0 && <tr><td colSpan="3" className="p-8 text-center text-gray-400">Belum ada konten mimbar.</td></tr>}
                    </tbody>
                  </table>
                </div>
              </div>
            )}

            {/* MEDIA VIEW */}
            {view === 'media' && (
              <div>
                <h1 className="text-2xl font-bold text-pink-600 mb-6">Media & Dokumentasi</h1>
                
                <div className="bg-white p-8 rounded-2xl shadow-sm border-t-4 border-pink-500 mb-8">
                  <h3 className="text-lg font-bold text-pink-600 mb-4">Upload Foto</h3>
                  <form onSubmit={submitPhoto} className="space-y-4">
                    <div className="grid grid-cols-2 gap-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Judul</label><input required name="title" type="text" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-pink-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Kategori</label><input required name="cat" defaultValue="Umum" list="cat-list" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-pink-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tanggal</label><input required name="date" type="date" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-pink-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Status</label><select name="status" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-pink-500 focus:outline-none"><option value="Published">Tampil</option><option value="Draft">Sembunyi</option></select></div>
                    </div>
                    <FileUploader label="Upload Foto (Maks 10MB)" onUpload={(url) => document.getElementById('photo-url').value = url} />
                    <input id="photo-url" name="photoUrl" type="hidden" />
                    <button className="w-full bg-pink-600 text-white py-3 rounded-lg font-semibold hover:bg-pink-700 flex items-center justify-center gap-2"><Icon name="save" className="w-5 h-5"/> Simpan Foto</button>
                  </form>
                </div>

                <div className="bg-white p-8 rounded-2xl shadow-sm border-t-4 border-rose-500 mb-8">
                  <h3 className="text-lg font-bold text-rose-600 mb-4">Tambah Video</h3>
                  <form onSubmit={submitVideo} className="space-y-4">
                    <div className="grid grid-cols-2 gap-4">
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Judul</label><input required name="title" type="text" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-rose-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Kategori</label><input required name="cat" defaultValue="Umum" list="cat-list" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-rose-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Tanggal</label><input required name="date" type="date" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-rose-500 focus:outline-none" /></div>
                      <div><label className="block text-xs font-medium text-gray-700 mb-1">Status</label><select name="status" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-rose-500 focus:outline-none"><option value="Published">Tampil</option><option value="Draft">Sembunyi</option></select></div>
                    </div>
                    <div>
                      <label className="block text-xs font-medium text-gray-700 mb-1">Upload Video (Maks 10MB, disimpan permanen)</label>
                      <input id="vid-file-input" name="videoFile" type="file" accept="video/*" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-rose-500 focus:outline-none" />
                    </div>
                    <div><label className="block text-xs font-medium text-gray-700 mb-1">Atau Link YouTube</label><input name="url" type="text" placeholder="https://youtube.com/watch?v=..." className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-rose-500 focus:outline-none" /></div>
                    <FileUploader label="Upload Thumbnail (Opsional)" onUpload={(url) => document.getElementById('vid-thumb').value = url} />
                    <input id="vid-thumb" name="thumbnail" type="hidden" />
                    <button className="w-full bg-rose-600 text-white py-3 rounded-lg font-semibold hover:bg-rose-700 flex items-center justify-center gap-2"><Icon name="save" className="w-5 h-5"/> Simpan Video</button>
                  </form>
                </div>

                <div className="bg-white rounded-xl shadow-sm overflow-hidden border border-gray-100 mt-8">
                  <h3 className="text-lg font-bold text-gray-700 p-4 bg-gray-50">Daftar Media (Foto & Video)</h3>
                  <table className="w-full text-left">
                    <thead className="bg-gray-100 text-gray-700 text-sm"><tr><th className="p-4">Pratinjau</th><th className="p-4">Judul</th><th className="p-4">Status</th><th className="p-4 text-right">Aksi</th></tr></thead>
                    <tbody>
                      {mediaItems.map(m => (
                        <tr key={m.id} className="border-t text-sm hover:bg-gray-50">
                          <td className="p-2"><div className="w-16 h-12 bg-gray-100 rounded overflow-hidden">{m.type === 'video' ? <div className="w-full h-full flex items-center justify-center bg-gray-800"><Icon name="play" className="w-5 h-5 text-white" /></div> : <img src={m.url} className="w-full h-full object-cover" alt={m.title} />}</div></td>
                          <td className="p-4 font-medium text-gray-800">{m.title || "Tanpa Judul"}</td>
                          <td className="p-4"><span className={`text-xs px-2 py-1 rounded ${m.status === 'Published' ? 'bg-green-100 text-green-700' : 'bg-gray-100'}`}>{m.status || 'Published'}</span></td>
                          <td className="p-4 text-right"><DeleteBtn onClick={() => { if (window.confirm(`Hapus "${m.title || 'media ini'}"?`)) { updateData('mediaItems', mediaItems.filter(x => x.id !== m.id)); showToast("Media dihapus permanen", "success"); } }} /></td>
                        </tr>
                      ))}
                      {mediaItems.length === 0 && <tr><td colSpan="4" className="p-8 text-center text-gray-400">Belum ada media.</td></tr>}
                    </tbody>
                  </table>
                </div>
                <datalist id="cat-list"><option>Ibadah</option><option>Pelayanan</option><option>PAM</option><option>PKB</option><option>PW</option><option>PAR</option><option>KSP</option><option>Rayon</option><option>Umum</option></datalist>
              </div>
            )}

            {/* MESSAGES VIEW */}
            {view === 'messages' && (
              <div>
                <h1 className="text-2xl font-bold text-sky mb-6">Pesan Masuk</h1>
                <div className="bg-white rounded-xl shadow-sm overflow-hidden border border-gray-100">
                  <table className="w-full text-left">
                    <thead className="bg-gray-100 text-gray-700 text-sm"><tr><th className="p-4">Nama</th><th className="p-4">Email</th><th className="p-4">Pesan</th><th className="p-4">Status</th><th className="p-4 text-right">Aksi</th></tr></thead>
                    <tbody>
                      {waMessages.map(msg => (
                        <tr key={msg.id} className="border-t text-sm hover:bg-gray-50">
                          <td className="p-4 font-bold">{msg.name}</td>
                          <td className="p-4 text-gray-600">{msg.email}</td>
                          <td className="p-4 text-gray-600 max-w-xs truncate">{msg.message}</td>
                          <td className="p-4"><span className={`text-xs px-2 py-1 rounded ${msg.status === 'Baru' ? 'bg-red-100 text-red-700' : 'bg-gray-100'}`}>{msg.status}</span></td>
                          <td className="p-4 text-right flex gap-1 justify-end">
                            {msg.status === 'Baru' && <button onClick={() => { setAppData(prev => ({...prev, waMessages: prev.waMessages.map(m => m.id === msg.id ? {...m, status: 'Dibaca'} : m)})); showToast('Pesan ditandai dibaca'); }} className="inline-flex items-center gap-1 text-xs font-semibold px-3 py-2 rounded-md shadow-sm bg-blue-100 text-blue-700 hover:bg-blue-600 hover:text-white"><Icon name="check" className="w-3.5 h-3.5" /> Tandai Dibaca</button>}
                            <DeleteBtn onClick={() => { if(window.confirm('Hapus pesan ini?')) setAppData(prev => ({...prev, waMessages: prev.waMessages.filter(m => m.id !== msg.id)})); }} />
                          </td>
                        </tr>
                      ))}
                      {waMessages.length === 0 && <tr><td colSpan="5" className="p-8 text-center text-gray-400">Belum ada pesan masuk.</td></tr>}
                    </tbody>
                  </table>
                </div>
              </div>
            )}
          </main>
        </div>
      );
    };

    const LoginScreen = ({ navigate }) => {
      const { login, siteSettings } = useApp();
      const [error, setError] = useState('');
      const handleLogin = (e) => { e.preventDefault(); const fd = new FormData(e.target); if (login(fd.get('username'), fd.get('password'))) { navigate('/admin'); } else { setError('Username atau Password salah!'); } };
      return (
        <div className="min-h-screen flex items-center justify-center bg-skyLight dark:bg-gray-900">
          <div className="bg-white p-8 rounded-2xl shadow-lg w-full max-w-md text-center border-t-8 border-sky">
            <img src={siteSettings.logo} className="w-20 h-20 mx-auto mb-4 object-contain" alt="Logo" />
            <h2 className="text-2xl font-bold text-sky mb-2">Login Sistem</h2>
            <form onSubmit={handleLogin} className="space-y-4 text-left">
              <input type="text" name="username" required placeholder="Username" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-sky focus:outline-none" />
              <input type="password" name="password" required placeholder="Password" className="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-sky focus:outline-none" />
              {error && <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-lg text-sm text-center">{error}</div>}
              <button type="submit" className="w-full bg-sky text-white py-3 rounded-lg font-semibold hover:bg-sky/90">MASUK</button>
            </form>
            <button onClick={() => navigate('/')} className="mt-4 text-sm text-gray-500 hover:text-sky">← Kembali</button>
          </div>
        </div>
      );
    };

    const Footer = ({ navigate }) => {
      const { siteSettings } = useApp();
      return (
        <footer className="bg-primary text-white pt-16 pb-8">
          <div className="container mx-auto px-4 grid grid-cols-1 md:grid-cols-4 gap-8 mb-12">
            <div><div className="flex items-center gap-3 mb-4"><img src={siteSettings.logo} className="h-12 w-12 object-contain bg-white rounded-full p-1" alt="Logo" /><h4 className="font-bold text-lg">{siteSettings.namaGereja}</h4></div><p className="text-sm text-gray-300">{siteSettings.klalis}</p></div>
            <div><h4 className="font-bold text-gold mb-4 uppercase text-sm">Menu</h4><ul className="space-y-2 text-gray-300 text-sm"><li><button onClick={() => navigate('/')} className="hover:text-white">Beranda</button></li><li><button onClick={() => navigate('/berita')} className="hover:text-white">Berita</button></li><li><button onClick={() => navigate('/media/foto')} className="hover:text-white">Galeri Foto</button></li></ul></div>
            <div><h4 className="font-bold text-gold mb-4 uppercase text-sm">Informasi</h4><ul className="space-y-2 text-gray-300 text-sm"><li><button onClick={() => navigate('/jadwal')} className="hover:text-white">Jadwal Ibadah</button></li><li><button onClick={() => navigate('/tentang')} className="hover:text-white">Tentang Kami</button></li><li><button onClick={() => navigate('/kontak')} className="hover:text-white">Kontak</button></li></ul></div>
            <div><h4 className="font-bold text-gold mb-4 uppercase text-sm">Kontak</h4><p className="text-sm text-gray-300 mb-2">{siteSettings.alamat}</p><div className="flex gap-4 mt-2"><Icon name="message-circle" className="w-5 h-5 text-gray-300 hover:text-gold cursor-pointer"/><Icon name="facebook" className="w-5 h-5 text-gray-300 hover:text-gold cursor-pointer"/><Icon name="youtube" className="w-5 h-5 text-gray-300 hover:text-gold cursor-pointer"/></div></div>
          </div>
          <div className="border-t border-white/20 pt-8 text-center text-sm text-gray-400">© {new Date().getFullYear()} {siteSettings.namaGereja} — {siteSettings.organisasi}. All rights reserved.</div>
        </footer>
      );
    };

    const App = () => {
      const [route, setRoute] = useState(window.location.hash.slice(1) || '/');
      const { siteSettings } = useApp();
      useEffect(() => { const onHashChange = () => setRoute(window.location.hash.slice(1) || '/'); window.addEventListener('hashchange', onHashChange); lucide.createIcons(); return () => window.removeEventListener('hashchange', onHashChange); }, []);
      useEffect(() => { lucide.createIcons(); }, [route]);
      useEffect(() => { document.title = `${siteSettings.namaGereja} | GKI di Tanah Papua`; }, [siteSettings]);
      const navigate = (path) => { window.location.hash = path; setRoute(path); window.scrollTo(0, 0); };
      const mimbarMatch = route.match(/^\/mimbar\/(.*)$/);
      const mimbarCategory = mimbarMatch ? mimbarMatch[1] : null;
      const isMediaRoute = route.includes('/media');
      const mediaView = route.includes('/media/') ? route.split('/').pop() : 'all';

      if (route === '/admin' || route === '/login') {
        return route === '/admin' ? <AdminDashboard navigate={navigate} /> : <LoginScreen navigate={navigate} />;
      }
      return (
        <>
          <Navbar navigate={navigate} />
          {route === '/' && <Hero navigate={navigate} />}
          {route === '/tentang' && <AboutPage />}
          {route === '/jadwal' && <SchedulePage />}
          {route === '/khotbah' && <SermonPage />}
          {route === '/berita' && <NewsPage />}
          {mimbarCategory && <MimbarPage category={mimbarCategory} />}
          {isMediaRoute && <MediaPage navigate={navigate} initialView={mediaView === 'media' ? 'all' : mediaView} />}
          {route === '/kontak' && <ContactPage />}
          {!['/', '/tentang', '/jadwal', '/khotbah', '/berita', '/kontak'].includes(route) && !mimbarCategory && !isMediaRoute && (
            <div className="min-h-[50vh] flex flex-col items-center justify-center text-gray-500"><Icon name="alert-triangle" className="w-16 h-16 mb-4 text-amber-500" /><h2 className="text-2xl font-bold mb-2">404 - Halaman Tidak Ditemukan</h2><button onClick={() => navigate('/')} className="mt-4 text-sky hover:underline">Kembali ke Beranda</button></div>
          )}
          <Footer navigate={navigate} />
        </>
      );
    };

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<AppProvider><App /></AppProvider>);
  </script>
</body>
</html>
