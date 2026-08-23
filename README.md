from pathlib import Path
import zipfile, textwrap, json

root = Path("/mnt/data/zoodo-store")
(root / "css").mkdir(parents=True, exist_ok=True)
(root / "js").mkdir(parents=True, exist_ok=True)
(root / "assets").mkdir(parents=True, exist_ok=True)

index_html = r'''<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<meta name="theme-color" content="#111827">
<meta name="description" content="Zoodo Store — smart shopping at everyday prices.">
<title>Zoodo Store — Smart Shopping</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body>
<div class="top-strip">
  <div class="container top-strip-inner">
    <span>🚚 Free delivery on selected products</span>
    <span class="top-hide">Secure checkout • Easy tracking • Fresh deals</span>
  </div>
</div>

<header class="site-header">
  <div class="container header-main">
    <button class="icon-btn mobile-menu" id="menuBtn" aria-label="Open menu">☰</button>
    <a class="brand" href="index.html"><span class="brand-mark">Z</span><span>Zoodo<span class="brand-soft">Store</span></span></a>
    <div class="search-wrap">
      <span>⌕</span>
      <input id="searchInput" type="search" placeholder="Search for products, brands and more..." autocomplete="off">
      <button id="searchBtn">Search</button>
    </div>
    <nav class="header-actions">
      <button class="header-link" id="accountBtn">👤 <span>Account</span></button>
      <button class="header-link" id="wishlistBtn">♡ <span>Wishlist</span></button>
      <button class="cart-btn" id="cartBtn">🛒 <span>Cart</span><b id="cartCount">0</b></button>
    </nav>
  </div>
  <div class="mobile-search container">
    <span>⌕</span><input id="mobileSearchInput" type="search" placeholder="Search products...">
  </div>
</header>

<nav class="primary-nav">
  <div class="container nav-scroll">
    <a class="active" href="#home">Home</a>
    <a href="#shop">Shop</a>
    <a href="#deals">🔥 Deals</a>
    <a href="#categories">Categories</a>
    <a href="#new">New Arrivals</a>
    <a href="#best">Best Sellers</a>
    <a href="#track">📦 Track Order</a>
  </div>
</nav>

<main id="home">
<section class="hero">
  <div class="container hero-grid">
    <div class="hero-copy">
      <span class="eyebrow">SMART SHOPPING • BETTER VALUE</span>
      <h1>Find more.<br><span>Pay smarter.</span></h1>
      <p>Discover useful everyday products, trending finds and great-value deals — all in one place.</p>
      <div class="hero-buttons">
        <a class="btn primary" href="#shop">Shop Now →</a>
        <a class="btn ghost" href="#categories">Explore Categories</a>
      </div>
      <div class="trust-row">
        <span>✓ Value-focused</span><span>✓ Order tracking</span><span>✓ Secure checkout</span>
      </div>
    </div>
    <div class="hero-art">
      <div class="glow"></div>
      <div class="hero-orb orb-one">🔥<small>Hot Deals</small></div>
      <div class="hero-orb orb-two">⚡<small>Trending</small></div>
      <div class="hero-card">
        <span class="mini-label">TODAY'S PICK</span>
        <div class="product-art large">🎧</div>
        <strong>Wireless Everyday Picks</strong>
        <span class="hero-price">From ₹299</span>
      </div>
    </div>
  </div>
</section>

<section class="section category-section" id="categories">
  <div class="container">
    <div class="section-head"><div><span class="eyebrow dark">SHOP BY CATEGORY</span><h2>Everything in one place</h2></div><button class="text-btn" id="allCategories">View all →</button></div>
    <div class="category-bar" id="categoryBar"></div>
  </div>
</section>

<section class="section quick-section">
  <div class="container">
    <div class="section-head compact"><div><span class="eyebrow dark">QUICK LINKS</span><h2>Shop your way</h2></div></div>
    <div class="quick-grid">
      <button class="quick-card" data-filter="deal"><span>🔥</span><div><strong>Today's Deals</strong><small>Big value picks</small></div><b>→</b></button>
      <button class="quick-card" data-filter="best"><span>🏆</span><div><strong>Best Sellers</strong><small>Customer favourites</small></div><b>→</b></button>
      <button class="quick-card" data-filter="new"><span>✨</span><div><strong>New Arrivals</strong><small>Fresh products</small></div><b>→</b></button>
      <button class="quick-card" data-filter="rating"><span>⭐</span><div><strong>Top Rated</strong><small>Highly reviewed</small></div><b>→</b></button>
    </div>
  </div>
</section>

<section class="section shortcut-section">
  <div class="container">
    <div class="section-head compact"><div><span class="eyebrow dark">SHORTCUTS</span><h2>One tap away</h2></div></div>
    <div class="shortcut-grid">
      <button data-action="orders"><span>📦</span><strong>My Orders</strong><small>View purchases</small></button>
      <button data-action="wishlist"><span>♡</span><strong>Wishlist</strong><small>Saved products</small></button>
      <button data-action="track"><span>🚚</span><strong>Track Order</strong><small>Live order status</small></button>
      <button data-action="offers"><span>🎁</span><strong>Offers</strong><small>Latest promotions</small></button>
      <button data-action="support"><span>💬</span><strong>Support</strong><small>We're here to help</small></button>
      <button data-action="account"><span>👤</span><strong>Account</strong><small>Profile & settings</small></button>
    </div>
  </div>
</section>

<section class="section products-section" id="shop">
  <div class="container">
    <div class="section-head">
      <div><span class="eyebrow dark">DISCOVER</span><h2 id="productTitle">Trending products</h2></div>
      <div class="sort-wrap"><button class="filter-btn" id="filterBtn">☷ Filters</button><select id="sortSelect"><option value="featured">Featured</option><option value="priceLow">Price: Low to High</option><option value="priceHigh">Price: High to Low</option><option value="rating">Top Rated</option></select></div>
    </div>
    <div class="tabs" id="tabs">
      <button class="tab active" data-tab="all">All</button>
      <button class="tab" data-tab="trending">Trending</button>
      <button class="tab" data-tab="deal">Deals</button>
      <button class="tab" data-tab="new">New</button>
      <button class="tab" data-tab="best">Best Sellers</button>
    </div>
    <div class="product-grid" id="productGrid"></div>
    <div class="empty-state" id="emptyState" hidden><div>🔎</div><h3>No products found</h3><p>Try another search or category.</p><button class="btn primary" id="resetBtn">Show all products</button></div>
  </div>
</section>

<section class="promo-band" id="deals">
  <div class="container promo-inner">
    <div><span class="eyebrow">ZOODO VALUE DAYS</span><h2>Fresh deals. Simple shopping.</h2><p>New products and price drops added regularly.</p></div>
    <a class="btn light" href="#shop" data-scroll-shop>Explore deals →</a>
  </div>
</section>

<section class="section info-section">
  <div class="container info-grid">
    <div class="info-card"><span>🚚</span><h3>Delivery updates</h3><p>Track your order after dispatch with your order number.</p></div>
    <div class="info-card"><span>🔒</span><h3>Secure shopping</h3><p>Your checkout information is handled through secure payment services.</p></div>
    <div class="info-card"><span>💬</span><h3>Customer support</h3><p>Need help? Contact Zoodo Store support for order assistance.</p></div>
  </div>
</section>
</main>

<footer class="footer">
  <div class="container footer-grid">
    <div><a class="brand footer-brand" href="#home"><span class="brand-mark">Z</span><span>Zoodo<span class="brand-soft">Store</span></span></a><p>Smart shopping for everyday life.</p></div>
    <div><h4>Shop</h4><a href="#categories">Categories</a><a href="#shop">All Products</a><a href="#deals">Deals</a></div>
    <div><h4>Help</h4><a href="#track">Track Order</a><a href="#" data-action="support">Support</a><a href="#">Returns</a></div>
    <div><h4>Company</h4><a href="#">About Zoodo</a><a href="#">Privacy</a><a href="#">Terms</a></div>
  </div>
  <div class="container footer-bottom">© <span id="year"></span> Zoodo Store. All rights reserved.</div>
</footer>

<div class="bottom-nav">
  <a href="#home" class="active">⌂<small>Home</small></a>
  <a href="#categories">◫<small>Categories</small></a>
  <a href="#shop">✦<small>Shop</small></a>
  <button id="bottomCart">🛒<small>Cart</small><b id="bottomCartCount">0</b></button>
  <button id="bottomAccount">●<small>Account</small></button>
</div>

<div class="modal" id="cartModal" aria-hidden="true">
  <div class="modal-box"><button class="modal-close" data-close>×</button><span class="eyebrow dark">YOUR CART</span><h2>Shopping cart</h2><div id="cartItems"></div><div class="cart-total"><span>Total</span><strong id="cartTotal">₹0</strong></div><button class="btn primary full" id="checkoutBtn">Proceed to checkout</button></div>
</div>

<div class="modal" id="accountModal" aria-hidden="true">
  <div class="modal-box small"><button class="modal-close" data-close>×</button><span class="eyebrow dark">ZOODO ACCOUNT</span><h2>Welcome back</h2><p class="muted">Account login will connect to the backend in the next phase.</p><button class="btn primary full" data-close>Continue shopping</button></div>
</div>

<div class="toast" id="toast"></div>
<script src="js/app.js"></script>
</body>
</html>
'''

style_css = r''':root{
--ink:#111827;--muted:#667085;--line:#e7eaf0;--bg:#f6f7fb;--card:#fff;
--brand:#6d4aff;--brand2:#8d6bff;--hot:#ff5b5b;--dark:#0f172a;
--shadow:0 12px 40px rgba(15,23,42,.08);--radius:22px;
}
*{box-sizing:border-box}html{scroll-behavior:smooth}body{margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;background:var(--bg);color:var(--ink)}
button,input,select{font:inherit}button,a{cursor:pointer}a{text-decoration:none;color:inherit}.container{width:min(1180px,calc(100% - 32px));margin:auto}
.top-strip{background:#101828;color:#dce1ea;font-size:12px}.top-strip-inner{height:32px;display:flex;align-items:center;justify-content:space-between}
.site-header{background:rgba(255,255,255,.96);position:sticky;top:0;z-index:30;border-bottom:1px solid var(--line);backdrop-filter:blur(16px)}
.header-main{min-height:76px;display:flex;align-items:center;gap:22px}.brand{display:flex;align-items:center;gap:9px;font-weight:900;font-size:23px;letter-spacing:-.8px;white-space:nowrap}.brand-mark{width:36px;height:36px;border-radius:12px;background:linear-gradient(135deg,var(--brand),#b58cff);display:grid;place-items:center;color:white;box-shadow:0 8px 20px rgba(109,74,255,.28)}.brand-soft{color:#8b92a5;font-weight:700}
.search-wrap{height:46px;flex:1;display:flex;align-items:center;background:#f2f4f8;border:1px solid #e8eaf0;border-radius:14px;padding-left:14px;color:#8a91a1}.search-wrap input{border:0;outline:0;background:transparent;flex:1;padding:0 10px;color:var(--ink)}.search-wrap button{height:36px;margin-right:5px;border:0;border-radius:10px;background:var(--ink);color:#fff;padding:0 16px;font-weight:700}.header-actions{display:flex;align-items:center;gap:6px}.header-link,.cart-btn,.icon-btn{border:0;background:transparent;color:var(--ink);padding:10px 9px;border-radius:10px}.header-link:hover,.cart-btn:hover{background:#f3f4f8}.cart-btn{position:relative}.cart-btn b,.bottom-nav b{position:absolute;background:var(--hot);color:#fff;font-size:10px;min-width:17px;height:17px;border-radius:20px;display:grid;place-items:center;top:2px;right:1px}.mobile-menu,.mobile-search{display:none}
.primary-nav{background:#fff;border-bottom:1px solid var(--line)}.nav-scroll{display:flex;gap:28px;overflow:auto;scrollbar-width:none}.nav-scroll::-webkit-scrollbar,.category-bar::-webkit-scrollbar{display:none}.nav-scroll a{padding:14px 2px;color:#687083;font-size:14px;font-weight:700;white-space:nowrap;border-bottom:2px solid transparent}.nav-scroll a.active,.nav-scroll a:hover{color:var(--brand);border-color:var(--brand)}
.hero{position:relative;overflow:hidden;background:radial-gradient(circle at 80% 20%,#7659ff 0,#3d2a9c 24%,#17152d 70%);color:#fff;min-height:470px;display:flex;align-items:center}.hero:before,.hero:after{content:"";position:absolute;border-radius:50%;filter:blur(2px);opacity:.18}.hero:before{width:400px;height:400px;background:#c6b9ff;right:-100px;top:-170px}.hero:after{width:250px;height:250px;background:#64d9ff;left:-120px;bottom:-120px}.hero-grid{display:grid;grid-template-columns:1.05fr .95fr;align-items:center;gap:40px;position:relative;z-index:1}.eyebrow{display:inline-block;font-size:11px;font-weight:900;letter-spacing:1.6px;color:#d6d0ff}.eyebrow.dark{color:#7b6be0}.hero h1{font-size:clamp(46px,6vw,78px);line-height:.95;letter-spacing:-4px;margin:14px 0 20px}.hero h1 span{color:#bdb0ff}.hero-copy p{font-size:17px;line-height:1.7;color:#e5e7f1;max-width:600px}.hero-buttons{display:flex;gap:12px;margin:28px 0}.btn{display:inline-flex;align-items:center;justify-content:center;border:0;border-radius:13px;padding:13px 19px;font-weight:800;transition:.2s}.btn:hover{transform:translateY(-2px)}.btn.primary{background:var(--brand);color:#fff;box-shadow:0 10px 24px rgba(109,74,255,.25)}.hero .btn.primary{background:#fff;color:#25175d}.btn.ghost{border:1px solid rgba(255,255,255,.25);color:#fff;background:rgba(255,255,255,.08)}.btn.light{background:#fff;color:#17152d}.trust-row{display:flex;gap:18px;flex-wrap:wrap;color:#cfd3e4;font-size:12px}.hero-art{min-height:360px;position:relative;display:grid;place-items:center}.hero-card{width:min(330px,80%);background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.2);backdrop-filter:blur(20px);border-radius:28px;padding:22px;box-shadow:0 30px 80px rgba(0,0,0,.25);transform:rotate(3deg)}.mini-label{font-size:10px;font-weight:900;letter-spacing:1.5px;color:#d5d0ff}.product-art{display:grid;place-items:center;background:linear-gradient(135deg,#eee9ff,#fff);color:#31206e;border-radius:22px}.product-art.large{height:190px;font-size:88px;margin:14px 0}.hero-card strong{display:block;font-size:18px}.hero-price{display:block;margin-top:6px;color:#c9bdff;font-weight:800}.hero-orb{position:absolute;width:90px;height:90px;border-radius:50%;background:rgba(255,255,255,.13);border:1px solid rgba(255,255,255,.18);display:grid;place-items:center;font-size:28px;box-shadow:0 20px 40px rgba(0,0,0,.18)}.hero-orb small{position:absolute;font-size:9px;bottom:-17px;color:#e4e1ff;font-weight:800}.orb-one{left:5%;top:14%}.orb-two{right:4%;bottom:10%}
.section{padding:58px 0}.section-head{display:flex;align-items:end;justify-content:space-between;gap:20px;margin-bottom:24px}.section-head.compact{margin-bottom:20px}.section-head h2{font-size:29px;letter-spacing:-1.2px;margin:6px 0 0}.text-btn{border:0;background:none;color:var(--brand);font-weight:800}.category-bar{display:flex;gap:14px;overflow:auto;padding:4px 1px 12px}.category-item{min-width:118px;border:1px solid var(--line);background:#fff;border-radius:20px;padding:16px 10px;display:flex;flex-direction:column;align-items:center;gap:9px;box-shadow:0 4px 16px rgba(15,23,42,.03);transition:.2s}.category-item:hover,.category-item.active{border-color:#c9befd;transform:translateY(-2px);box-shadow:var(--shadow)}.category-icon{width:52px;height:52px;border-radius:17px;background:#f1efff;display:grid;place-items:center;font-size:24px}.category-item strong{font-size:12px}.quick-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:13px}.quick-card{border:1px solid var(--line);background:#fff;border-radius:18px;padding:18px;display:flex;align-items:center;gap:13px;text-align:left;box-shadow:0 4px 16px rgba(15,23,42,.03);transition:.2s}.quick-card:hover{transform:translateY(-3px);box-shadow:var(--shadow)}.quick-card>span{width:44px;height:44px;border-radius:14px;background:#f6f1ff;display:grid;place-items:center;font-size:21px}.quick-card div{flex:1}.quick-card strong,.quick-card small{display:block}.quick-card strong{font-size:14px}.quick-card small{color:var(--muted);font-size:11px;margin-top:4px}.quick-card b{color:#9ba1af}
.shortcut-section{padding-top:0}.shortcut-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:10px}.shortcut-grid button{border:1px solid var(--line);background:#fff;border-radius:18px;padding:18px 10px;text-align:center;transition:.2s}.shortcut-grid button:hover{border-color:#c9befd;transform:translateY(-2px)}.shortcut-grid span{display:grid;place-items:center;width:45px;height:45px;margin:0 auto 9px;border-radius:15px;background:#f3f1ff;font-size:21px}.shortcut-grid strong,.shortcut-grid small{display:block}.shortcut-grid strong{font-size:12px}.shortcut-grid small{font-size:10px;color:var(--muted);margin-top:4px}
.products-section{background:#fff}.sort-wrap{display:flex;gap:8px}.sort-wrap select,.filter-btn{border:1px solid var(--line);background:#fff;border-radius:11px;padding:10px 12px;color:#475467;font-weight:700}.tabs{display:flex;gap:8px;margin-bottom:22px;overflow:auto}.tab{border:1px solid var(--line);background:#f8f9fb;color:#667085;padding:9px 16px;border-radius:30px;font-size:12px;font-weight:800;white-space:nowrap}.tab.active{background:var(--ink);color:#fff;border-color:var(--ink)}.product-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:16px}.product-card{background:#fff;border:1px solid #eceef3;border-radius:19px;overflow:hidden;position:relative;transition:.2s}.product-card:hover{transform:translateY(-4px);box-shadow:var(--shadow)}.product-image{height:210px;background:linear-gradient(145deg,#f1f3f8,#e8eafa);display:grid;place-items:center;font-size:70px;position:relative}.discount{position:absolute;top:10px;left:10px;background:#e8f8ef;color:#117a43;border-radius:7px;padding:5px 7px;font-size:10px;font-weight:900}.wish{position:absolute;top:9px;right:9px;width:32px;height:32px;border:0;border-radius:50%;background:rgba(255,255,255,.9);font-size:17px}.product-body{padding:14px}.product-cat{font-size:9px;letter-spacing:1px;color:#8a91a1;font-weight:900;text-transform:uppercase}.product-name{font-size:14px;line-height:1.35;margin:5px 0 8px;min-height:38px}.rating{font-size:11px;color:#d89000}.price-row{display:flex;align-items:center;gap:8px;margin:8px 0}.price{font-size:18px;font-weight:900}.mrp{text-decoration:line-through;color:#98a0af;font-size:11px}.add-btn{width:100%;border:1px solid #d8d0ff;background:#f5f2ff;color:#5134ce;border-radius:10px;padding:10px;font-weight:900;font-size:12px}.add-btn:hover{background:var(--brand);color:#fff}.empty-state{text-align:center;padding:70px 10px}.empty-state>div{font-size:40px}.muted{color:var(--muted);line-height:1.7}
.promo-band{padding:42px 0;background:linear-gradient(105deg,#15132a,#35248a);color:#fff}.promo-inner{display:flex;justify-content:space-between;align-items:center;gap:30px}.promo-inner h2{font-size:30px;margin:7px 0}.promo-inner p{color:#d1d5e4;margin:0}
.info-section{padding-bottom:75px}.info-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}.info-card{background:#fff;border:1px solid var(--line);border-radius:20px;padding:23px}.info-card>span{font-size:27px}.info-card h3{font-size:15px;margin:12px 0 6px}.info-card p{font-size:12px;color:var(--muted);line-height:1.6;margin:0}
.footer{background:#101828;color:#d9dee8;padding:50px 0 80px}.footer-grid{display:grid;grid-template-columns:2fr repeat(3,1fr);gap:30px}.footer-brand{color:#fff}.footer p{color:#8f98a9;font-size:12px}.footer h4{margin:0 0 14px;color:#fff}.footer-grid>div:not(:first-child) a{display:block;color:#9ba4b5;font-size:12px;margin:9px 0}.footer-bottom{border-top:1px solid #263043;margin-top:35px;padding-top:20px;color:#7f899b;font-size:11px}
.modal{position:fixed;inset:0;background:rgba(5,8,20,.55);display:none;align-items:center;justify-content:center;padding:18px;z-index:100}.modal.open{display:flex}.modal-box{width:min(520px,100%);max-height:85vh;overflow:auto;background:#fff;border-radius:25px;padding:27px;position:relative;box-shadow:0 30px 100px rgba(0,0,0,.25)}.modal-box.small{width:min(400px,100%)}.modal-close{position:absolute;right:14px;top:12px;border:0;background:#f1f3f7;width:34px;height:34px;border-radius:50%;font-size:22px}.modal-box h2{margin:8px 0 20px}.cart-row{display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid var(--line)}.cart-thumb{width:54px;height:54px;border-radius:13px;background:#f0f2f8;display:grid;place-items:center;font-size:25px}.cart-info{flex:1}.cart-info strong,.cart-info small{display:block}.cart-info small{color:var(--muted);margin-top:4px}.remove{border:0;background:#fff;color:#d33}.cart-total{display:flex;justify-content:space-between;margin:20px 0;font-size:17px}.full{width:100%}.toast{position:fixed;left:50%;bottom:85px;transform:translate(-50%,20px);background:#101828;color:#fff;padding:12px 17px;border-radius:12px;font-size:12px;font-weight:800;opacity:0;pointer-events:none;transition:.25s;z-index:150}.toast.show{opacity:1;transform:translate(-50%,0)}
.bottom-nav{display:none}

@media(max-width:900px){
.header-main{gap:10px}.header-actions .header-link span,.cart-btn span{display:none}.search-wrap{max-width:none}.hero-grid{grid-template-columns:1fr}.hero{padding:60px 0}.hero-art{display:none}.shortcut-grid{grid-template-columns:repeat(3,1fr)}.product-grid{grid-template-columns:repeat(3,1fr)}}
@media(max-width:650px){
.container{width:min(100% - 22px,1180px)}.top-strip-inner{height:28px}.top-hide{display:none}.header-main{min-height:64px}.mobile-menu{display:block;font-size:20px}.brand{font-size:20px}.brand-mark{width:32px;height:32px}.search-wrap{display:none}.header-actions{margin-left:auto}.mobile-search{display:flex;align-items:center;gap:8px;height:45px;background:#f3f4f7;border-radius:12px;padding:0 12px;margin-bottom:9px}.mobile-search input{border:0;outline:0;background:transparent;flex:1}.primary-nav{display:none}.hero{min-height:510px;padding:48px 0}.hero h1{font-size:53px;letter-spacing:-3px}.hero-copy p{font-size:14px}.hero-buttons{flex-direction:column}.hero-buttons .btn{width:100%}.section{padding:42px 0}.section-head{align-items:start}.section-head h2{font-size:24px}.quick-grid{grid-template-columns:1fr 1fr}.shortcut-grid{grid-template-columns:repeat(3,1fr);gap:8px}.shortcut-grid button{padding:14px 5px}.product-grid{grid-template-columns:repeat(2,1fr);gap:10px}.product-image{height:170px;font-size:55px}.product-body{padding:11px}.product-name{font-size:12px}.price{font-size:16px}.sort-wrap select{display:none}.promo-inner{display:block}.promo-inner .btn{margin-top:20px}.info-grid{grid-template-columns:1fr}.footer-grid{grid-template-columns:1fr 1fr}.footer-grid>div:first-child{grid-column:1/-1}.bottom-nav{position:fixed;display:flex;bottom:0;left:0;right:0;height:64px;background:rgba(255,255,255,.97);border-top:1px solid var(--line);z-index:50;backdrop-filter:blur(18px);justify-content:space-around}.bottom-nav a,.bottom-nav button{border:0;background:none;position:relative;color:#7c8495;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:3px;font-size:18px;flex:1}.bottom-nav small{font-size:9px;font-weight:800}.bottom-nav .active{color:var(--brand)}.bottom-nav button{font-family:inherit}.bottom-nav b{top:7px;right:calc(50% - 18px)}.footer{padding-bottom:90px}}
'''

app_js = r'''const products = [
 {id:1,name:"360° Adjustable Phone Stand",cat:"Mobile Accessories",icon:"📱",price:299,mrp:599,rating:4.6,tags:["trending","deal","best"],discount:50},
 {id:2,name:"Smart Kitchen Storage Organizer",cat:"Home & Kitchen",icon:"🏠",price:349,mrp:699,rating:4.7,tags:["trending","new"],discount:50},
 {id:3,name:"Universal Car Phone Holder",cat:"Car & Bike",icon:"🚗",price:399,mrp:799,rating:4.5,tags:["deal","best"],discount:50},
 {id:4,name:"Resistance Band Fitness Set",cat:"Fitness & Lifestyle",icon:"💪",price:499,mrp:899,rating:4.8,tags:["best","trending"],discount:44},
 {id:5,name:"Makeup & Beauty Organizer",cat:"Beauty & Personal Care",icon:"💄",price:449,mrp:899,rating:4.4,tags:["new","deal"],discount:50},
 {id:6,name:"Minimal Everyday Wallet",cat:"Fashion Accessories",icon:"👛",price:299,mrp:599,rating:4.3,tags:["best"],discount:50},
 {id:7,name:"Mini Household Tool Kit",cat:"Tools & Utility",icon:"🔧",price:549,mrp:999,rating:4.7,tags:["trending","deal"],discount:45},
 {id:8,name:"Pet Grooming Brush",cat:"Pet Products",icon:"🐾",price:249,mrp:499,rating:4.6,tags:["new","best"],discount:50},
 {id:9,name:"Cable Management Kit",cat:"Mobile Accessories",icon:"🔌",price:199,mrp:399,rating:4.2,tags:["deal"],discount:50},
 {id:10,name:"Multipurpose Cleaning Tool",cat:"Home & Kitchen",icon:"🧽",price:229,mrp:499,rating:4.5,tags:["trending","new"],discount:54},
 {id:11,name:"Car Cleaning Utility Set",cat:"Car & Bike",icon:"✨",price:399,mrp:799,rating:4.4,tags:["best"],discount:50},
 {id:12,name:"Portable Water Bottle",cat:"Fitness & Lifestyle",icon:"🥤",price:279,mrp:499,rating:4.1,tags:["new"],discount:44}
];

const categories = [
 ["🏠","Home & Kitchen"],["📱","Mobile Accessories"],["🚗","Car & Bike"],["💪","Fitness & Lifestyle"],
 ["💄","Beauty & Personal Care"],["👕","Fashion Accessories"],["🔧","Tools & Utility"],["🐾","Pet Products"]
];

let cart = JSON.parse(localStorage.getItem("zoodoCart")||"[]");
let currentTab="all", currentCategory="", searchTerm="";

const $ = s => document.querySelector(s);
const $$ = s => [...document.querySelectorAll(s)];

function money(n){return "₹"+n.toLocaleString("en-IN")}
function saveCart(){localStorage.setItem("zoodoCart",JSON.stringify(cart));updateCartCount()}
function updateCartCount(){
 const n=cart.reduce((a,x)=>a+x.qty,0);
 $("#cartCount").textContent=n; $("#bottomCartCount").textContent=n;
}
function toast(msg){const t=$("#toast");t.textContent=msg;t.classList.add("show");clearTimeout(window.__toast);window.__toast=setTimeout(()=>t.classList.remove("show"),2200)}

function renderCategories(){
 $("#categoryBar").innerHTML=categories.map(([icon,name])=>`<button class="category-item ${currentCategory===name?"active":""}" data-category="${name}"><span class="category-icon">${icon}</span><strong>${name}</strong></button>`).join("");
}
function filtered(){
 let arr=[...products];
 if(currentCategory) arr=arr.filter(p=>p.cat===currentCategory);
 if(currentTab!=="all") arr=arr.filter(p=>p.tags.includes(currentTab));
 if(searchTerm) arr=arr.filter(p=>(p.name+" "+p.cat).toLowerCase().includes(searchTerm.toLowerCase()));
 const sort=$("#sortSelect").value;
 if(sort==="priceLow") arr.sort((a,b)=>a.price-b.price);
 if(sort==="priceHigh") arr.sort((a,b)=>b.price-a.price);
 if(sort==="rating") arr.sort((a,b)=>b.rating-a.rating);
 return arr;
}
function renderProducts(){
 const arr=filtered(), grid=$("#productGrid"), empty=$("#emptyState");
 $("#productTitle").textContent=currentCategory || (currentTab==="all"?"Trending products":currentTab==="deal"?"Today's deals":currentTab==="best"?"Best sellers":currentTab==="new"?"New arrivals":"Trending products");
 grid.innerHTML=arr.map(p=>`
 <article class="product-card">
   <div class="product-image"><span class="discount">${p.discount}% OFF</span><button class="wish" data-wish="${p.id}" aria-label="Wishlist">♡</button><span>${p.icon}</span></div>
   <div class="product-body">
    <span class="product-cat">${p.cat}</span>
    <div class="product-name">${p.name}</div>
    <div class="rating">★ ${p.rating} <span style="color:#98a0af">• Popular</span></div>
    <div class="price-row"><span class="price">${money(p.price)}</span><span class="mrp">${money(p.mrp)}</span></div>
    <button class="add-btn" data-add="${p.id}">Add to Cart</button>
   </div>
 </article>`).join("");
 empty.hidden=arr.length!==0;
}
function openModal(id){$(id).classList.add("open");$(id).setAttribute("aria-hidden","false")}
function closeModals(){$$(".modal").forEach(m=>{m.classList.remove("open");m.setAttribute("aria-hidden","true")})}
function renderCart(){
 const box=$("#cartItems");
 if(!cart.length){box.innerHTML='<div class="empty-state" style="padding:30px 5px"><div>🛒</div><h3>Your cart is empty</h3><p>Browse products and add your favourites.</p></div>';$("#cartTotal").textContent="₹0";return}
 box.innerHTML=cart.map(item=>`<div class="cart-row"><div class="cart-thumb">${item.icon}</div><div class="cart-info"><strong>${item.name}</strong><small>${money(item.price)} × ${item.qty}</small></div><button class="remove" data-remove="${item.id}">Remove</button></div>`).join("");
 $("#cartTotal").textContent=money(cart.reduce((a,x)=>a+x.price*x.qty,0));
}

document.addEventListener("click",e=>{
 const add=e.target.closest("[data-add]");
 if(add){const id=+add.dataset.add,p=products.find(x=>x.id===id),old=cart.find(x=>x.id===id);old?old.qty++:cart.push({...p,qty:1});saveCart();toast("Added to cart");return}
 const remove=e.target.closest("[data-remove]");
 if(remove){cart=cart.filter(x=>x.id!==+remove.dataset.remove);saveCart();renderCart();return}
 const cat=e.target.closest("[data-category]");
 if(cat){currentCategory=cat.dataset.category;currentTab="all";$$(".tab").forEach(x=>x.classList.toggle("active",x.dataset.tab==="all"));renderCategories();renderProducts();$("#shop").scrollIntoView({behavior:"smooth"});return}
 const tab=e.target.closest(".tab");
 if(tab){currentTab=tab.dataset.tab;currentCategory="";$$(".tab").forEach(x=>x.classList.toggle("active",x===tab));renderCategories();renderProducts();return}
 const quick=e.target.closest("[data-filter]");
 if(quick){currentCategory="";currentTab=quick.dataset.filter;$$(".tab").forEach(x=>x.classList.toggle("active",x.dataset.tab===currentTab));renderCategories();renderProducts();$("#shop").scrollIntoView({behavior:"smooth"});return}
 const action=e.target.closest("[data-action]");
 if(action){
   const a=action.dataset.action;
   if(a==="wishlist") toast("Wishlist will be connected to your account backend.");
   else if(a==="account") openModal("#accountModal");
   else if(a==="track") toast("Enter your order number in the tracking page.");
   else if(a==="orders") toast("Your orders will appear after login.");
   else if(a==="offers") {currentTab="deal";currentCategory="";$$(".tab").forEach(x=>x.classList.toggle("active",x.dataset.tab==="deal"));renderProducts();$("#shop").scrollIntoView({behavior:"smooth"});}
   else toast("Support centre will be connected in the next phase.");
 }
});
$("#cartBtn").onclick=()=>{renderCart();openModal("#cartModal")};
$("#bottomCart").onclick=()=>{renderCart();openModal("#cartModal")};
$("#accountBtn").onclick=()=>openModal("#accountModal");
$("#bottomAccount").onclick=()=>openModal("#accountModal");
$$("[data-close]").forEach(x=>x.onclick=closeModals);
$$(".modal").forEach(m=>m.addEventListener("click",e=>{if(e.target===m)closeModals()}));
$("#sortSelect").onchange=renderProducts;
$("#filterBtn").onclick=()=>toast("Filters panel will be connected to the product database.");
$("#searchBtn").onclick=()=>{searchTerm=$("#searchInput").value.trim();renderProducts();$("#shop").scrollIntoView({behavior:"smooth"})};
$("#searchInput").addEventListener("keydown",e=>{if(e.key==="Enter")$("#searchBtn").click()});
$("#mobileSearchInput").addEventListener("input",e=>{searchTerm=e.target.value.trim();renderProducts()});
$("#allCategories").onclick=()=>{currentCategory="";currentTab="all";renderCategories();renderProducts();$("#shop").scrollIntoView({behavior:"smooth"})};
$("#resetBtn").onclick=()=>{searchTerm="";currentCategory="";currentTab="all";$("#searchInput").value="";$("#mobileSearchInput").value="";$$(".tab").forEach(x=>x.classList.toggle("active",x.dataset.tab==="all"));renderCategories();renderProducts()};
$("#checkoutBtn").onclick=()=>toast(cart.length?"Checkout backend will be connected next.":"Your cart is empty.");
$("#menuBtn").onclick=()=>document.querySelector(".primary-nav").style.display="block";
$("#year").textContent=new Date().getFullYear();
renderCategories();renderProducts();updateCartCount();
'''

readme = r'''# Zoodo Store

A mobile-first marketplace-style frontend starter for Zoodo Store.

## Included
- Responsive homepage
- Background/hero section
- Top navigation
- Category bar
- Quick links
- Shortcuts/icon grid
- Product tabs
- Search
- Sorting
- Product cards
- Local cart
- Responsive mobile bottom navigation
- Starter account/cart modals

## Deploy to Cloudflare Pages
1. Push this folder to a GitHub repository named `zoodo-store`.
2. In Cloudflare: Workers & Pages → Create application → Pages → Connect to Git.
3. Select the `zoodo-store` repository.
4. Framework: None.
5. Build command: leave blank.
6. Output directory: `/`.
7. Deploy.

## Next backend phase
Connect Cloudflare Workers + D1 for:
- Admin authentication
- Product CRUD
- Supplier management
- Orders
- Customers
- Payments
- Tracking
- Reviews
- Analytics
'''

(root/"index.html").write_text(index_html, encoding="utf-8")
(root/"css/style.css").write_text(style_css, encoding="utf-8")
(root/"js/app.js").write_text(app_js, encoding="utf-8")
(root/"README.md").write_text(readme, encoding="utf-8")

zip_path=Path("/mnt/data/Zoodo-Store-Frontend.zip")
with zipfile.ZipFile(zip_path,"w",zipfile.ZIP_DEFLATED) as z:
    for f in root.rglob("*"):
        if f.is_file():
            z.write(f, f.relative_to(root.parent))

print(f"Created: {zip_path}")
print("Files:", [str(f.relative_to(root)) for f in root.rglob("*") if f.is_file()])
