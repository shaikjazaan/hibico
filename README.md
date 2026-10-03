<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HIBICO | Nature in Every Sip</title>
    
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Playfair+Display:ital,wght@0,600;0,700;0,800;1,600&display=swap" rel="stylesheet">
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Tailwind Configuration -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            // Extracting colors from the provided logo
                            primary: '#D0193B',    // Deep vibrant red from the main text
                            primaryHover: '#A6142F', // Darker shade for hovers
                            leaf: '#659E44',       // Fresh green from the leaf
                            leafHover: '#4D7D31',
                            petal: '#E42C5B',      // Pinkish red from the flower petals
                            petalLight: '#FCECEF', // Very light pink for backgrounds
                            stone: '#F9FAFB',      // Cool off-white background
                            white: '#FFFFFF',
                            textDark: '#1F2937',   // Dark gray replacing earth brown for text
                            textMuted: '#6B7280',  // Muted gray
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        serif: ['Playfair Display', 'serif'],
                    }
                }
            }
        }
    </script>

    <!-- Custom CSS for Interactivity -->
    <style>
        body { background-color: #FFFFFF; color: #1F2937; }
        
        /* Flip Card Styles */
        .flip-card {
            perspective: 1000px;
            background-color: transparent;
        }
        .flip-card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            text-align: center;
            transition: transform 0.6s cubic-bezier(0.4, 0.2, 0.2, 1);
            transform-style: preserve-3d;
        }
        .flip-card:hover .flip-card-inner {
            transform: rotateY(180deg);
        }
        .flip-card-front, .flip-card-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            -webkit-backface-visibility: hidden;
            border-radius: 0.5rem;
        }
        .flip-card-back {
            transform: rotateY(180deg);
        }

        /* Cart Drawer Animation */
        .drawer-open { transform: translateX(0%); }
        .drawer-closed { transform: translateX(100%); }
        
        .overlay-open { opacity: 1; pointer-events: auto; }
        .overlay-closed { opacity: 0; pointer-events: none; }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #F7F3E3; }
        ::-webkit-scrollbar-thumb { background: #8B4C3A; }

        /* Toast */
        .toast-show { transform: translateY(0); opacity: 1; }
        .toast-hide { transform: translateY(20px); opacity: 0; pointer-events: none; }
        
        /* Subtle glow effects adapted for new palette */
        .glow-red { text-shadow: 0 0 15px rgba(208, 25, 59, 0.3); }
        .box-shadow-soft { box-shadow: 0 10px 30px rgba(31, 41, 55, 0.05); }
        
        /* Botanical pattern overlay */
        .pattern-botanical {
            background-image: radial-gradient(#FCECEF 1.5px, transparent 1.5px);
            background-size: 24px 24px;
            opacity: 0.8;
        }
    </style>
</head>
<body class="font-sans antialiased overflow-x-hidden selection:bg-brand-primary selection:text-white">

    <!-- Navigation -->
    <nav class="fixed w-full z-40 bg-white/90 backdrop-blur-md border-b border-brand-petalLight transition-all duration-300">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <div class="flex-shrink-0 flex items-center cursor-pointer group">
                    <img src="WhatsApp Image 2026-09-23 at 12.37.07 PM.jpeg" alt="HIBICO Logo" class="h-14 w-auto object-contain transform group-hover:scale-105 transition-transform duration-300">
                </div>
                
                <div class="hidden md:flex space-x-10 items-center">
                    <a href="#shop" class="text-brand-textDark hover:text-brand-primary transition-colors text-sm font-bold uppercase tracking-widest">Shop Powder</a>
                    <a href="#benefits" class="text-brand-textDark hover:text-brand-primary transition-colors text-sm font-bold uppercase tracking-widest">Benefits</a>
                    <a href="#brew-guide" class="text-brand-textDark hover:text-brand-primary transition-colors text-sm font-bold uppercase tracking-widest">How to Use</a>
                    <a href="#ai-mixologist" class="text-brand-petal hover:text-brand-primary transition-colors text-sm font-bold uppercase tracking-widest flex items-center gap-2"><i class="fa-solid fa-wand-magic-sparkles"></i> AI Recipe</a>
                </div>

                <div class="flex items-center">
                    <button onclick="toggleCart()" class="text-brand-textDark hover:text-brand-primary transition-colors relative group p-2">
                        <i class="fa-solid fa-bag-shopping text-xl"></i>
                        <span id="nav-cart-count" class="absolute top-0 right-0 bg-brand-primary text-white text-[10px] font-bold h-4 w-4 rounded-full flex items-center justify-center transform group-hover:scale-110 transition-transform">0</span>
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Cart Overlay & Drawer -->
    <div id="cart-overlay" onclick="toggleCart()" class="fixed inset-0 bg-brand-textDark/40 backdrop-blur-sm z-50 overlay-closed transition-all duration-300"></div>
    
    <div id="cart-drawer" class="fixed top-0 right-0 w-full max-w-md h-full bg-white z-50 shadow-2xl drawer-closed transition-transform duration-500 ease-in-out flex flex-col border-l border-brand-petalLight">
        <div class="px-6 py-5 border-b border-brand-petalLight flex justify-between items-center bg-white">
            <h2 class="font-sans font-bold uppercase tracking-wider text-xl text-brand-textDark">Your Cart</h2>
            <button onclick="toggleCart()" class="text-brand-textMuted hover:text-brand-primary transition-colors p-2">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>
        </div>
        
        <div id="cart-items-container" class="flex-1 overflow-y-auto p-6 space-y-6 bg-brand-stone">
            <div id="empty-cart-msg" class="h-full flex flex-col items-center justify-center text-brand-textMuted space-y-4">
                <i class="fa-solid fa-seedling text-4xl opacity-30 text-brand-leaf"></i>
                <p class="font-medium tracking-wide uppercase text-sm">Your cart is empty.</p>
                <button onclick="toggleCart(); document.getElementById('shop').scrollIntoView();" class="text-brand-primary border-b border-brand-primary pb-1 font-bold hover:text-brand-primaryHover uppercase tracking-wider text-xs">Shop Instant Powder</button>
            </div>
        </div>
        
        <div class="border-t border-brand-petalLight p-6 bg-white">
            <div class="flex justify-between items-center mb-4">
                <span class="text-brand-textMuted font-bold uppercase tracking-wider text-sm">Subtotal</span>
                <span id="cart-total" class="font-mono text-2xl font-bold text-brand-textDark">₹0</span>
            </div>
            <p class="text-xs text-brand-textMuted mb-4 uppercase tracking-wider">Shipping calculated at checkout.</p>
            <button class="w-full bg-brand-primary text-white py-4 font-bold uppercase tracking-widest hover:bg-brand-primaryHover transition-all shadow-md flex justify-center items-center space-x-3 rounded-sm">
                <span>Checkout Securely</span>
                <i class="fa-solid fa-lock"></i>
            </button>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-8 left-1/2 transform -translate-x-1/2 bg-brand-textDark text-white px-6 py-4 shadow-2xl z-50 toast-hide transition-all duration-300 flex items-center space-x-3 font-bold border-l-4 border-brand-primary rounded-sm">
        <i class="fa-solid fa-leaf text-brand-leaf text-lg"></i>
        <span id="toast-msg" class="text-sm uppercase tracking-wide">Item added to cart</span>
    </div>

    <main>
        <!-- Hero Section -->
        <section class="relative pt-32 pb-20 lg:pt-48 lg:pb-32 overflow-hidden bg-brand-stone">
            <!-- Background Pattern -->
            <div class="absolute inset-0 pattern-botanical"></div>
            
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
                <div class="lg:grid lg:grid-cols-2 lg:gap-16 items-center">
                    
                    <div class="text-center lg:text-left mb-16 lg:mb-0">
                        <div class="inline-flex items-center px-4 py-1.5 bg-white border border-brand-petalLight text-brand-leaf text-xs font-bold tracking-widest uppercase mb-8 shadow-sm rounded-full">
                            <i class="fa-solid fa-seedling mr-2 text-brand-leaf"></i>
                            NATURE IN EVERY SIP
                        </div>
                        <h1 class="text-5xl tracking-tighter font-serif font-bold text-brand-textDark sm:text-6xl md:text-7xl mb-6 leading-[1.1]">
                            Rooted in Nature.<br>
                            <span class="text-brand-primary italic font-normal">Instant</span> Vitality.
                        </h1>
                        <p class="text-lg text-brand-textMuted sm:text-xl max-w-2xl mx-auto lg:mx-0 font-medium mb-10 leading-relaxed">
                            Experience the raw power of 100% pure hibiscus calyces, finely powdered for instant lattes, iced teas, and smoothie boosts. No bags. No waiting. Zero waste.
                        </p>
                        <div class="flex flex-col sm:flex-row justify-center lg:justify-start space-y-4 sm:space-y-0 sm:space-x-6">
                            <a href="#shop" class="px-8 py-4 bg-brand-primary text-white font-bold uppercase tracking-widest hover:bg-brand-primaryHover shadow-lg hover:-translate-y-1 transition-all duration-300 text-center text-sm rounded-sm">
                                Shop The Harvest
                            </a>
                        </div>
                    </div>
                    
                    <!-- Hero Image -->
                    <div class="relative w-full max-w-lg mx-auto lg:max-w-none group">
                        <!-- Organic shape backdrop -->
                        <div class="absolute inset-0 bg-brand-petalLight rounded-full blur-3xl transform scale-110 -translate-x-4 translate-y-8"></div>
                        <div class="relative overflow-hidden aspect-[4/5] bg-white border border-brand-petalLight rounded-t-full shadow-2xl p-2">
                            <img src="https://placehold.co/800x1000/E42C5B/FFFFFF?text=Vivid+Hibiscus+Powder" alt="Vibrant Red Hibiscus Powder" class="object-cover w-full h-full rounded-t-full transform transition-transform duration-1000 group-hover:scale-105">
                        </div>
                        
                        <!-- Floating Detail -->
                        <div class="absolute bottom-10 -left-10 bg-white p-4 border-l-4 border-brand-leaf shadow-xl flex items-center space-x-4 hidden md:flex rounded-r-lg">
                            <div class="w-12 h-12 bg-brand-stone flex items-center justify-center text-brand-leaf rounded-full">
                                <i class="fa-solid fa-leaf text-xl"></i>
                            </div>
                            <div>
                                <p class="text-[10px] text-brand-textMuted uppercase tracking-widest font-bold">Sourcing</p>
                                <p class="font-sans font-bold text-brand-textDark text-sm uppercase">Single Origin</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Product Section -->
        <section id="shop" class="py-24 bg-white text-brand-textDark relative border-t border-brand-petalLight">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center max-w-3xl mx-auto mb-20">
                    <h2 class="text-4xl font-serif font-black tracking-tight sm:text-6xl mb-4 uppercase text-brand-textDark">Botanical Powders</h2>
                    <div class="w-24 h-1 bg-brand-primary mx-auto mb-6"></div>
                    <p class="text-lg text-brand-textMuted font-medium">Finely milled from the earth for culinary perfection.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-10 max-w-5xl mx-auto">
                    
                    <!-- Product 1 -->
                    <div class="bg-brand-stone p-8 transition-all duration-300 hover:shadow-xl group border border-brand-petalLight rounded-sm relative overflow-hidden">
                        <i class="fa-solid fa-leaf absolute -bottom-6 -right-6 text-9xl text-brand-leaf/10 rotate-45 pointer-events-none transition-transform group-hover:rotate-12"></i>
                        
                        <div class="relative aspect-square overflow-hidden bg-white mb-8 border border-brand-petalLight/50 rounded-sm shadow-inner p-4">
                            <img src="https://placehold.co/800x800/D0193B/FFFFFF?text=Dried+Hibiscus+Flowers" alt="Instant Iced Hibiscus" class="w-full h-full object-cover transform transition-transform duration-700 group-hover:scale-105">
                            <div class="absolute top-6 left-6 bg-brand-primary text-white text-[10px] font-bold px-3 py-1.5 uppercase tracking-widest rounded-sm">
                                Pure Extract
                            </div>
                        </div>
                        <div class="relative z-10">
                            <h3 class="text-2xl font-black font-serif text-brand-textDark uppercase tracking-wide mb-1">Signature Pack</h3>
                            <p class="text-brand-textMuted text-sm font-bold mb-6 uppercase tracking-wider">30 Servings • Pure Hibiscus</p>
                            
                            <div class="flex justify-between items-end mb-8 border-b border-brand-petalLight pb-6">
                                <span class="text-3xl font-bold font-mono text-brand-primary">₹899</span>
                                <div class="flex flex-col items-end">
                                    <div class="flex space-x-1 text-brand-leaf text-xs mb-1">
                                        <i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i>
                                    </div>
                                    <span class="text-brand-textMuted text-xs font-bold">(412 REVIEWS)</span>
                                </div>
                            </div>
                            
                            <div class="flex space-x-4">
                                <div class="flex items-center border border-brand-petalLight bg-white w-32 rounded-sm">
                                    <button onclick="updateQty('qty-1', -1)" class="w-10 h-12 flex items-center justify-center text-brand-textDark hover:bg-brand-stone transition-colors"><i class="fa-solid fa-minus text-xs"></i></button>
                                    <input type="text" id="qty-1" value="1" readonly class="w-full h-12 text-center bg-transparent font-bold text-brand-textDark focus:outline-none font-mono">
                                    <button onclick="updateQty('qty-1', 1)" class="w-10 h-12 flex items-center justify-center text-brand-textDark hover:bg-brand-stone transition-colors"><i class="fa-solid fa-plus text-xs"></i></button>
                                </div>
                                <button onclick="addToCartTrigger('prod-1', 'Signature Pack', 899, 'https://placehold.co/400x400/D0193B/FFFFFF?text=Signature+Pack', 'qty-1')" class="flex-1 bg-brand-primary text-white font-bold uppercase tracking-widest hover:bg-brand-primaryHover transition-colors duration-300 flex items-center justify-center space-x-2 rounded-sm shadow-md">
                                    <span>Add to Cart</span>
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- Product 2 -->
                    <div class="bg-brand-stone p-8 transition-all duration-300 hover:shadow-xl group border border-brand-petalLight rounded-sm relative overflow-hidden">
                        <i class="fa-solid fa-seedling absolute -bottom-6 -right-6 text-9xl text-brand-primary/5 rotate-12 pointer-events-none transition-transform group-hover:-rotate-12"></i>
                        
                        <div class="relative aspect-square overflow-hidden bg-white mb-8 border border-brand-petalLight/50 rounded-sm shadow-inner p-4">
                            <img src="https://placehold.co/800x800/E42C5B/FFFFFF?text=Pink+Latte+Powder" alt="Pink Latte Blend" class="w-full h-full object-cover transform transition-transform duration-700 group-hover:scale-105">
                            <div class="absolute top-6 left-6 bg-brand-petal text-white text-[10px] font-bold px-3 py-1.5 uppercase tracking-widest rounded-sm">
                                Roots & Botanicals
                            </div>
                        </div>
                        <div class="relative z-10">
                            <h3 class="text-2xl font-black font-serif text-brand-textDark uppercase tracking-wide mb-1">Radiant Glow Blend</h3>
                            <p class="text-brand-textMuted text-sm font-bold mb-6 uppercase tracking-wider">60 Servings • Pure Hibiscus</p>
                            
                            <div class="flex justify-between items-end mb-8 border-b border-brand-petalLight pb-6">
                                <span class="text-3xl font-bold font-mono text-brand-primary">₹1049</span>
                                <div class="flex flex-col items-end">
                                    <div class="flex space-x-1 text-brand-leaf text-xs mb-1">
                                        <i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star-half-stroke"></i>
                                    </div>
                                    <span class="text-brand-textMuted text-xs font-bold">(189 REVIEWS)</span>
                                </div>
                            </div>
                            
                            <div class="flex space-x-4">
                                <div class="flex items-center border border-brand-petalLight bg-white w-32 rounded-sm">
                                    <button onclick="updateQty('qty-2', -1)" class="w-10 h-12 flex items-center justify-center text-brand-textDark hover:bg-brand-stone transition-colors"><i class="fa-solid fa-minus text-xs"></i></button>
                                    <input type="text" id="qty-2" value="1" readonly class="w-full h-12 text-center bg-transparent font-bold text-brand-textDark focus:outline-none font-mono">
                                    <button onclick="updateQty('qty-2', 1)" class="w-10 h-12 flex items-center justify-center text-brand-textDark hover:bg-brand-stone transition-colors"><i class="fa-solid fa-plus text-xs"></i></button>
                                </div>
                                <button onclick="addToCartTrigger('prod-2', 'Radiant Glow Blend', 1049, 'https://placehold.co/400x400/E42C5B/FFFFFF?text=Radiant+Glow', 'qty-2')" class="flex-1 bg-brand-primary text-white font-bold uppercase tracking-widest hover:bg-brand-primaryHover transition-colors duration-300 flex items-center justify-center space-x-2 rounded-sm shadow-md">
                                    <span>Add to Cart</span>
                                </button>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </section>

        <!-- Interactive Benefits Section -->
        <section id="benefits" class="py-24 bg-brand-primary text-white relative border-y border-brand-primaryHover">
            <div class="absolute inset-0 pattern-botanical opacity-20 filter invert"></div>
            
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
                <div class="text-center max-w-3xl mx-auto mb-20">
                    <h2 class="text-4xl font-serif font-black sm:text-5xl mb-4 uppercase tracking-tight text-white">The Micro-Milled Difference</h2>
                    <p class="text-brand-petalLight font-bold tracking-widest uppercase text-sm">Hover to unearth the benefits</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                    <!-- Flip Card 1 -->
                    <div class="flip-card h-80 group">
                        <div class="flip-card-inner">
                            <div class="flip-card-front bg-white border border-brand-petalLight p-8 flex flex-col items-center justify-center shadow-lg rounded-sm text-brand-textDark">
                                <div class="w-20 h-20 rounded-full bg-brand-petalLight flex items-center justify-center mb-6">
                                    <i class="fa-solid fa-droplet text-3xl text-brand-primary"></i>
                                </div>
                                <h3 class="text-xl font-bold uppercase tracking-wide font-serif text-brand-primary">Instant Dissolve</h3>
                                <p class="mt-6 text-[10px] text-brand-textMuted uppercase tracking-widest font-bold border-b border-brand-textMuted pb-1">Reveal <i class="fa-solid fa-arrow-rotate-right ml-1"></i></p>
                            </div>
                            <div class="flip-card-back bg-brand-textDark text-white border border-brand-textMuted p-8 flex flex-col items-center justify-center shadow-xl rounded-sm">
                                <h3 class="text-2xl font-black uppercase mb-4 font-serif text-brand-petalLight">Zero Steeping</h3>
                                <div class="w-12 h-1 bg-brand-primary mb-4"></div>
                                <p class="text-sm text-center font-medium leading-relaxed text-gray-300">No boiling water or tea bags required. Our fine powder dissolves effortlessly in ice-cold water, smoothies, or hot milk for immediate earthly refreshment.</p>
                            </div>
                        </div>
                    </div>

                    <!-- Flip Card 2 -->
                    <div class="flip-card h-80 group">
                        <div class="flip-card-inner">
                            <div class="flip-card-front bg-white border border-brand-petalLight p-8 flex flex-col items-center justify-center shadow-lg rounded-sm text-brand-textDark">
                                <div class="w-20 h-20 rounded-full bg-brand-petalLight flex items-center justify-center mb-6">
                                    <i class="fa-solid fa-recycle text-3xl text-brand-leaf"></i>
                                </div>
                                <h3 class="text-xl font-bold uppercase tracking-wide font-serif text-brand-leaf">100% Whole Plant</h3>
                                <p class="mt-6 text-[10px] text-brand-textMuted uppercase tracking-widest font-bold border-b border-brand-textMuted pb-1">Reveal <i class="fa-solid fa-arrow-rotate-right ml-1"></i></p>
                            </div>
                            <div class="flip-card-back bg-brand-textDark text-white border border-brand-textMuted p-8 flex flex-col items-center justify-center shadow-xl rounded-sm">
                                <h3 class="text-2xl font-black uppercase mb-4 font-serif text-brand-leaf">Zero Waste</h3>
                                <div class="w-12 h-1 bg-brand-leaf mb-4"></div>
                                <p class="text-sm text-center font-medium leading-relaxed text-gray-300">Traditional steeping throws away 50% of the plant's nutrients. By consuming the micro-milled flower, your body utilizes every bit of fiber and vitamins.</p>
                            </div>
                        </div>
                    </div>

                    <!-- Flip Card 3 -->
                    <div class="flip-card h-80 group">
                        <div class="flip-card-inner">
                            <div class="flip-card-front bg-white border border-brand-petalLight p-8 flex flex-col items-center justify-center shadow-lg rounded-sm text-brand-textDark">
                                <div class="w-20 h-20 rounded-full bg-brand-petalLight flex items-center justify-center mb-6">
                                    <i class="fa-solid fa-heart-pulse text-3xl text-brand-petal"></i>
                                </div>
                                <h3 class="text-xl font-bold uppercase tracking-wide font-serif text-brand-petal">Max Potency</h3>
                                <p class="mt-6 text-[10px] text-brand-textMuted uppercase tracking-widest font-bold border-b border-brand-textMuted pb-1">Reveal <i class="fa-solid fa-arrow-rotate-right ml-1"></i></p>
                            </div>
                            <div class="flip-card-back bg-brand-textDark text-white border border-brand-textMuted p-8 flex flex-col items-center justify-center shadow-xl rounded-sm">
                                <h3 class="text-2xl font-black uppercase mb-4 font-serif text-brand-petal">Bioavailable</h3>
                                <div class="w-12 h-1 bg-brand-petal mb-4"></div>
                                <p class="text-sm text-center font-medium leading-relaxed text-gray-300">Powdering breaks down the tough cellular walls of the calyx, making the powerful anthocyanins (the source of the deep ruby color) highly absorbable.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Interactive Brew Guide (Tabs) -->
        <section id="brew-guide" class="py-24 bg-white text-brand-textDark">
            <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center mb-16">
                    <h2 class="text-4xl font-serif font-black sm:text-5xl mb-4 uppercase text-brand-textDark">Rituals of the Root</h2>
                    <p class="text-lg text-brand-textMuted font-medium">Endless versatility. Stir, whisk, or blend.</p>
                </div>

                <!-- Tabs -->
                <div class="flex flex-wrap justify-center gap-4 mb-12">
                    <button onclick="switchTab('iced')" id="btn-iced" class="tab-btn px-8 py-4 font-bold uppercase tracking-widest text-sm transition-all duration-300 bg-brand-textDark text-white shadow-md rounded-sm">
                        Cold Infusion
                    </button>
                    <button onclick="switchTab('latte')" id="btn-latte" class="tab-btn px-8 py-4 font-bold uppercase tracking-widest text-sm transition-all duration-300 bg-brand-stone text-brand-textMuted border border-brand-petalLight hover:bg-brand-petalLight/50 hover:text-brand-textDark rounded-sm">
                        Warm Latte
                    </button>
                    <button onclick="switchTab('smoothie')" id="btn-smoothie" class="tab-btn px-8 py-4 font-bold uppercase tracking-widest text-sm transition-all duration-300 bg-brand-stone text-brand-textMuted border border-brand-petalLight hover:bg-brand-petalLight/50 hover:text-brand-textDark rounded-sm">
                        Botanical Boost
                    </button>
                </div>

                <!-- Tab Contents -->
                <div class="bg-brand-stone shadow-xl border border-brand-petalLight rounded-sm overflow-hidden">
                    
                    <!-- Iced Content -->
                    <div id="content-iced" class="tab-content flex flex-col md:flex-row min-h-[400px]">
                        <div class="md:w-1/2 relative bg-brand-petalLight">
                            <img src="https://placehold.co/800x800/FFFFFF/D0193B?text=Iced+Rosella+Tea" alt="Iced Tea" class="w-full h-full object-cover absolute inset-0 mix-blend-multiply opacity-90">
                        </div>
                        <div class="md:w-1/2 p-10 md:p-16 flex flex-col justify-center">
                            <h3 class="text-3xl font-black font-serif uppercase mb-8 border-l-4 border-brand-primary pl-4 text-brand-textDark">Instant Ruby Refresh</h3>
                            <ul class="space-y-8">
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-primary font-bold text-xs">1</span>
                                    <p class="text-brand-textDark font-medium text-lg">Empty <span class="font-bold text-brand-primary">1 sachet (2g)</span> of Signature Pack into a glass.</p>
                                </li>
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-primary font-bold text-xs">2</span>
                                    <p class="text-brand-textDark font-medium text-lg">Pour 120ml of cold filtered water directly over it.</p>
                                </li>
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-primary font-bold text-xs">3</span>
                                    <p class="text-brand-textDark font-medium text-lg">Stir vigorously. Top with ice and a slice of fresh lime. Our blend is already perfectly sweetened!</p>
                                </li>
                            </ul>
                        </div>
                    </div>

                    <!-- Latte Content -->
                    <div id="content-latte" class="tab-content hidden flex-col md:flex-row min-h-[400px]">
                        <div class="md:w-1/2 relative bg-brand-petalLight">
                            <img src="https://placehold.co/800x800/FFFFFF/E42C5B?text=Pink+Hibiscus+Latte" alt="Pink Latte" class="w-full h-full object-cover absolute inset-0 mix-blend-multiply opacity-90">
                        </div>
                        <div class="md:w-1/2 p-10 md:p-16 flex flex-col justify-center">
                            <h3 class="text-3xl font-black font-serif uppercase mb-8 border-l-4 border-brand-petal pl-4 text-brand-textDark">Earthy Glow Latte</h3>
                            <ul class="space-y-8">
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-petal font-bold text-xs">1</span>
                                    <p class="text-brand-textDark font-medium text-lg">Empty <span class="font-bold text-brand-petal">1 sachet (2g)</span> of Radiant Glow Blend into a ceramic mug.</p>
                                </li>
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-petal font-bold text-xs">2</span>
                                    <p class="text-brand-textDark font-medium text-lg">Add a splash (approx 30ml) of hot water and whisk into a smooth, dark red paste.</p>
                                </li>
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-petal font-bold text-xs">3</span>
                                    <p class="text-brand-textDark font-medium text-lg">Pour in 90ml of frothed, warm oat milk. Enjoy the creamy, naturally sweetened taste.</p>
                                </li>
                            </ul>
                        </div>
                    </div>

                    <!-- Smoothie Content -->
                    <div id="content-smoothie" class="tab-content hidden flex-col md:flex-row min-h-[400px]">
                        <div class="md:w-1/2 relative bg-brand-stone">
                            <img src="https://placehold.co/800x800/FFFFFF/1F2937?text=Hibiscus+Smoothie+Bowl" alt="Smoothie" class="w-full h-full object-cover absolute inset-0 mix-blend-multiply opacity-90">
                        </div>
                        <div class="md:w-1/2 p-10 md:p-16 flex flex-col justify-center">
                            <h3 class="text-3xl font-black font-serif uppercase mb-8 border-l-4 border-brand-leaf pl-4 text-brand-textDark">Antioxidant Booster</h3>
                            <ul class="space-y-8">
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-leaf font-bold text-xs">1</span>
                                    <p class="text-brand-textDark font-medium text-lg">Add a handful of frozen berries, half a banana, and 120ml of coconut water to your blender.</p>
                                </li>
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-leaf font-bold text-xs">2</span>
                                    <p class="text-brand-textDark font-medium text-lg">Tear open and pour in <span class="font-bold text-brand-leaf">1 sachet (2g)</span> of Signature Pack.</p>
                                </li>
                                <li class="relative pl-8">
                                    <span class="absolute left-0 top-0 w-6 h-6 rounded-full bg-brand-petalLight flex items-center justify-center text-brand-leaf font-bold text-xs">3</span>
                                    <p class="text-brand-textDark font-medium text-lg">Blend on high. Watch the deep earthy red explode through the mix, adding the perfect tartness and natural sweetness.</p>
                                </li>
                            </ul>
                        </div>
                    </div>

                </div>
            </div>
        </section>

        <!-- AI Mixologist Section -->
        <section id="ai-mixologist" class="py-24 bg-brand-petalLight/30 text-brand-textDark relative border-t border-brand-petalLight">
            <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
                <div class="text-center mb-12">
                    <div class="inline-flex items-center px-4 py-2 bg-white border border-brand-primary text-brand-primary text-xs font-bold tracking-widest uppercase mb-6 shadow-sm rounded-full">
                        <i class="fa-solid fa-wand-magic-sparkles mr-2 animate-pulse"></i> Botanical AI Recipes
                    </div>
                    <h2 class="text-4xl font-serif font-black sm:text-5xl mb-4 uppercase tracking-tight text-brand-textDark">The Alchemist</h2>
                    <p class="text-lg text-brand-textMuted font-medium max-w-2xl mx-auto">Tell us what roots, herbs, or fruits are in your kitchen. Our AI will instantly craft a custom, perfectly balanced hibiscus beverage just for you.</p>
                </div>

                <div class="bg-white border border-brand-petalLight p-6 md:p-10 shadow-lg relative overflow-hidden rounded-sm">
                    <div class="mb-6 relative z-10">
                        <label for="ai-ingredients" class="block text-xs font-bold text-brand-primary uppercase tracking-widest mb-3">What ingredients do you have?</label>
                        <textarea id="ai-ingredients" rows="3" class="w-full bg-brand-stone border border-brand-petalLight text-brand-textDark p-5 focus:outline-none focus:border-brand-primary transition-all duration-300 placeholder-brand-textMuted/50 font-medium resize-none rounded-sm" placeholder="e.g. sparkling water, fresh mint, limes, a dash of honey, maybe some ginger..."></textarea>
                    </div>
                    
                    <button id="ai-btn" onclick="generateRecipe()" class="w-full relative z-10 bg-brand-primary text-white font-black uppercase tracking-widest py-5 hover:bg-brand-primaryHover transition-all shadow-md flex items-center justify-center space-x-3 group rounded-sm">
                        <span id="ai-btn-text">Craft My Elixir</span>
                        <i id="ai-btn-icon" class="fa-solid fa-wand-magic-sparkles group-hover:rotate-12 transition-transform"></i>
                    </button>

                    <!-- Results Area -->
                    <div id="ai-result-container" class="mt-10 hidden border-t border-brand-petalLight pt-10 relative z-10 transition-all duration-500 ease-in-out opacity-0 translate-y-4">
                        <!-- Injected via JS -->
                    </div>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-brand-textDark text-white pt-20 pb-10 border-t-4 border-brand-primary">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-12 mb-16">
                
                <div class="md:col-span-2">
                    <img src="WhatsApp Image 2026-09-23 at 12.37.07 PM.jpeg" alt="HIBICO Logo" class="h-12 w-auto object-contain mb-6 filter brightness-0 invert">
                    <p class="text-gray-400 text-sm font-medium leading-relaxed max-w-sm mb-6">
                        Revolutionizing ancient botanicals through micro-milling technology. Pure, instant, and unapologetically bold.
                    </p>
                    <div class="flex space-x-5">
                        <a href="https://www.instagram.com/drinkhibico?stkn=c2czZXIybnh2ZWxp" target="_blank" rel="noopener noreferrer" class="w-10 h-10 border border-brand-stone/20 rounded-full flex items-center justify-center hover:bg-brand-primary hover:border-brand-primary transition-colors"><i class="fa-brands fa-instagram text-brand-stone"></i></a>
                        <a href="#" class="w-10 h-10 border border-brand-stone/20 rounded-full flex items-center justify-center hover:bg-brand-primary hover:border-brand-primary transition-colors"><i class="fa-brands fa-tiktok text-brand-stone"></i></a>
                    </div>
                </div>

                <div>
                    <h4 class="font-bold text-brand-petalLight uppercase tracking-widest text-xs mb-6">Explore</h4>
                    <ul class="space-y-4">
                        <li><a href="#shop" class="text-gray-400 hover:text-white transition-colors text-xs uppercase font-bold tracking-wide">Shop Powders</a></li>
                        <li><a href="#benefits" class="text-gray-400 hover:text-white transition-colors text-xs uppercase font-bold tracking-wide">Why Powder?</a></li>
                        <li><a href="#brew-guide" class="text-gray-400 hover:text-white transition-colors text-xs uppercase font-bold tracking-wide">How to Use</a></li>
                    </ul>
                </div>

                <div>
                    <h4 class="font-bold text-brand-petalLight uppercase tracking-widest text-xs mb-6">Support</h4>
                    <ul class="space-y-4">
                        <li><a href="#" class="text-gray-400 hover:text-white transition-colors text-xs uppercase font-bold tracking-wide">Shipping Policy</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition-colors text-xs uppercase font-bold tracking-wide">Contact Us</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition-colors text-xs uppercase font-bold tracking-wide">Terms of Service</a></li>
                    </ul>
                </div>
            </div>

            <div class="border-t border-gray-800 pt-8 flex flex-col md:flex-row justify-between items-center text-[10px] text-gray-500 font-bold uppercase tracking-widest">
                <p>&copy; 2026 HIBICO. Nature in Every Sip.</p>
                <p class="mt-2 md:mt-0 text-brand-petalLight">ALL PRICES IN INR (₹)</p>
            </div>
        </div>
    </footer>

    <script>
        // --- Shopping Cart Logic ---
        let cart = [];

        function toggleCart() {
            const drawer = document.getElementById('cart-drawer');
            const overlay = document.getElementById('cart-overlay');
            
            if (drawer.classList.contains('drawer-closed')) {
                // Open
                drawer.classList.remove('drawer-closed');
                drawer.classList.add('drawer-open');
                overlay.classList.remove('overlay-closed');
                overlay.classList.add('overlay-open');
                document.body.style.overflow = 'hidden'; 
            } else {
                // Close
                drawer.classList.remove('drawer-open');
                drawer.classList.add('drawer-closed');
                overlay.classList.remove('overlay-open');
                overlay.classList.add('overlay-closed');
                document.body.style.overflow = '';
            }
        }

        function updateQty(inputId, change) {
            const input = document.getElementById(inputId);
            let currentVal = parseInt(input.value);
            let newVal = currentVal + change;
            if (newVal < 1) newVal = 1;
            if (newVal > 20) newVal = 20; 
            input.value = newVal;
        }

        function addToCartTrigger(id, name, price, img, qtyInputId) {
            const qty = parseInt(document.getElementById(qtyInputId).value);
            addToCart(id, name, price, img, qty);
            
            showToast(`${qty}x ${name} ADDED`);
            
            document.getElementById(qtyInputId).value = 1;
            
            setTimeout(() => {
                if(document.getElementById('cart-drawer').classList.contains('drawer-closed')) {
                    toggleCart();
                }
            }, 600);
        }

        function addToCart(id, name, price, img, qty) {
            const existingItem = cart.find(item => item.id === id);
            
            if (existingItem) {
                existingItem.qty += qty;
            } else {
                cart.push({ id, name, price, img, qty });
            }
            updateCartUI();
        }

        function removeFromCart(id) {
            cart = cart.filter(item => item.id !== id);
            updateCartUI();
        }

        function updateCartItemQty(id, change) {
            const item = cart.find(i => i.id === id);
            if (item) {
                item.qty += change;
                if (item.qty <= 0) {
                    removeFromCart(id);
                } else {
                    updateCartUI();
                }
            }
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items-container');
            const emptyMsg = document.getElementById('empty-cart-msg');
            const navCount = document.getElementById('nav-cart-count');
            const totalEl = document.getElementById('cart-total');
            
            const totalQty = cart.reduce((sum, item) => sum + item.qty, 0);
            const totalPrice = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            
            navCount.textContent = totalQty;
            totalEl.textContent = `₹${totalPrice.toLocaleString('en-IN')}`;
            
            navCount.classList.add('scale-125', 'bg-brand-primary', 'text-white');
            setTimeout(() => navCount.classList.remove('scale-125', 'bg-brand-primary', 'text-white'), 300);

            if (cart.length === 0) {
                emptyMsg.style.display = 'flex';
                Array.from(container.children).forEach(child => {
                    if (child.id !== 'empty-cart-msg') child.remove();
                });
            } else {
                emptyMsg.style.display = 'none';
                
                Array.from(container.children).forEach(child => {
                    if (child.id !== 'empty-cart-msg') child.remove();
                });
                
                cart.forEach(item => {
                    const itemHTML = `
                        <div class="flex space-x-4 border border-brand-petalLight p-3 bg-white rounded-sm">
                            <img src="${item.img}" alt="${item.name}" class="w-20 h-20 object-cover border border-brand-stone">
                            <div class="flex-1 flex flex-col justify-between py-1">
                                <div class="flex justify-between items-start">
                                    <h4 class="font-serif font-bold text-brand-textDark text-xs uppercase tracking-wide leading-tight pr-2">${item.name}</h4>
                                    <button onclick="removeFromCart('${item.id}')" class="text-brand-textMuted hover:text-brand-primary transition-colors"><i class="fa-solid fa-trash-can text-xs"></i></button>
                                </div>
                                <div class="flex justify-between items-end">
                                    <span class="font-bold text-brand-primary font-mono">₹${(item.price * item.qty).toLocaleString('en-IN')}</span>
                                    
                                    <div class="flex items-center border border-brand-petalLight bg-brand-stone h-7 rounded-sm overflow-hidden">
                                        <button onclick="updateCartItemQty('${item.id}', -1)" class="w-6 h-full flex items-center justify-center text-brand-textDark hover:bg-brand-petalLight/50 transition-colors"><i class="fa-solid fa-minus text-[10px]"></i></button>
                                        <span class="w-6 text-center text-xs font-mono font-bold text-brand-textDark bg-white">${item.qty}</span>
                                        <button onclick="updateCartItemQty('${item.id}', 1)" class="w-6 h-full flex items-center justify-center text-brand-textDark hover:bg-brand-petalLight/50 transition-colors"><i class="fa-solid fa-plus text-[10px]"></i></button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    `;
                    container.insertAdjacentHTML('beforeend', itemHTML);
                });
            }
        }

        // --- Toast Logic ---
        let toastTimeout;
        function showToast(msg) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toast-msg');
            
            toastMsg.textContent = msg;
            
            toast.classList.remove('toast-hide');
            toast.classList.add('toast-show');
            
            clearTimeout(toastTimeout);
            toastTimeout = setTimeout(() => {
                toast.classList.remove('toast-show');
                toast.classList.add('toast-hide');
            }, 3000);
        }

        // --- Tab Logic ---
        function switchTab(tabId) {
            const buttons = document.querySelectorAll('.tab-btn');
            buttons.forEach(btn => {
                btn.classList.remove('bg-brand-textDark', 'text-white', 'shadow-md');
                btn.classList.add('bg-brand-stone', 'text-brand-textMuted', 'border-brand-petalLight');
            });
            
            const activeBtn = document.getElementById(`btn-${tabId}`);
            activeBtn.classList.remove('bg-brand-stone', 'text-brand-textMuted', 'border-brand-petalLight');
            activeBtn.classList.add('bg-brand-textDark', 'text-white', 'shadow-md');
            
            const contents = document.querySelectorAll('.tab-content');
            contents.forEach(content => {
                content.classList.add('hidden');
                content.classList.remove('flex');
            });
            
            const activeContent = document.getElementById(`content-${tabId}`);
            activeContent.classList.remove('hidden');
            activeContent.classList.add('flex');
        }

        // --- AI Mixologist Logic (Gemini API) ---
        async function fetchWithBackoff(url, options, maxRetries = 3) {
            let retries = 0;
            let delay = 1000;
            while (retries < maxRetries) {
                try {
                    const response = await fetch(url, options);
                    if (!response.ok && response.status === 429) {
                        throw new Error("Throttled");
                    }
                    return response;
                } catch (error) {
                    retries++;
                    if (retries >= maxRetries) throw error;
                    await new Promise(resolve => setTimeout(resolve, delay));
                    delay *= 2;
                }
            }
        }

        async function generateRecipe() {
            const input = document.getElementById('ai-ingredients').value.trim();
            if (!input) {
                showToast("Please enter some ingredients first!");
                return;
            }

            const btn = document.getElementById('ai-btn');
            const btnText = document.getElementById('ai-btn-text');
            const btnIcon = document.getElementById('ai-btn-icon');
            const resultContainer = document.getElementById('ai-result-container');
            
            // Set loading state
            btn.disabled = true;
            btnText.textContent = "Foraging for your recipe...";
            btnIcon.className = "fa-solid fa-circle-notch fa-spin text-xl";
            resultContainer.classList.add('hidden', 'opacity-0', 'translate-y-4');

            const systemPrompt = "You are a master botanical mixologist for 'HIBICO', an earthy, natural brand of instant hibiscus tea. Create a unique, delicious beverage recipe (mocktail, latte, iced tea, or smoothie) using the ingredients the user provides, PLUS always include '1 Sachet (2g) HIBICO Powder'. Keep in mind that 1 sachet is designed for 120ml of liquid. Note that the HIBICO sachet already contains stevia (sweetener), maltodextrin, and citric acid, so warn the user not to add too much extra sweetness or acidity. Format the output as JSON matching the schema exactly. Ensure the tone is earthy, organic, and sophisticated.";
            const userQuery = `Ingredients I have: ${input}`;
            const apiKey = ""; 
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

            const payload = {
                contents: [{ role: "user", parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: {
                            "recipeName": { type: "STRING", description: "Creative, earthy name for the drink" },
                            "description": { type: "STRING", description: "A short, appetizing 1-sentence description" },
                            "ingredients": { type: "ARRAY", items: { type: "STRING" }, description: "List of ingredients including measurements" },
                            "instructions": { type: "ARRAY", items: { type: "STRING" }, description: "Step-by-step instructions" }
                        },
                        required: ["recipeName", "description", "ingredients", "instructions"]
                    }
                }
            };

            try {
                const response = await fetchWithBackoff(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                
                const result = await response.json();
                
                if (result.candidates && result.candidates.length > 0 && result.candidates[0].content) {
                    const jsonText = result.candidates[0].content.parts[0].text;
                    const recipe = JSON.parse(jsonText);
                    displayRecipe(recipe);
                } else {
                    throw new Error("Invalid API response format");
                }
            } catch (error) {
                console.error("Gemini API Error:", error);
                showToast("Failed to forge recipe. Please try again.");
            } finally {
                btn.disabled = false;
                btnText.textContent = "Craft My Elixir";
                btnIcon.className = "fa-solid fa-wand-magic-sparkles group-hover:rotate-12 transition-transform";
            }
        }

        function displayRecipe(recipe) {
            const container = document.getElementById('ai-result-container');
            
            const ingredientsHTML = recipe.ingredients.map(ing => `
                <li class="flex items-start">
                    <i class="fa-solid fa-leaf text-brand-leaf mt-1 mr-3 text-sm"></i>
                    <span class="font-medium text-brand-textDark">${ing}</span>
                </li>
            `).join('');
            
            const instructionsHTML = recipe.instructions.map((inst, idx) => `
                <li class="mb-5">
                    <span class="text-brand-primary font-bold text-[10px] uppercase tracking-widest block mb-1">Phase 0${idx + 1}</span>
                    <span class="text-brand-textDark/80 font-medium leading-relaxed">${inst}</span>
                </li>
            `).join('');

            container.innerHTML = `
                <div class="text-center mb-10">
                    <h3 class="text-3xl font-serif font-black text-brand-textDark uppercase mb-3">${recipe.recipeName}</h3>
                    <p class="text-brand-primary italic font-serif text-lg">"${recipe.description}"</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 text-left">
                    <div class="bg-brand-stone p-8 border border-brand-petalLight rounded-sm shadow-inner">
                        <h4 class="font-bold uppercase tracking-widest text-sm mb-6 border-b border-brand-petalLight pb-3 text-brand-primary">Botanicals Required</h4>
                        <ul class="space-y-4">
                            ${ingredientsHTML}
                        </ul>
                    </div>
                    <div class="bg-white p-8 border border-brand-petalLight rounded-sm shadow-sm">
                        <h4 class="font-bold uppercase tracking-widest text-sm mb-6 border-b border-brand-petalLight pb-3 text-brand-textDark">The Ritual</h4>
                        <ul class="">
                            ${instructionsHTML}
                        </ul>
                    </div>
                </div>
                <div class="mt-10 text-center">
                     <button onclick="document.getElementById('shop').scrollIntoView({behavior: 'smooth'});" class="inline-flex items-center space-x-2 text-brand-primary border-b-2 border-brand-primary pb-1 font-bold hover:text-brand-primaryHover hover:border-brand-primaryHover transition-colors uppercase tracking-wider text-sm">
                        <span>Shop Powder to Brew This</span>
                        <i class="fa-solid fa-arrow-up"></i>
                     </button>
                </div>
            `;
            
            container.classList.remove('hidden');
            setTimeout(() => {
                container.classList.remove('opacity-0', 'translate-y-4');
                container.classList.add('opacity-100', 'translate-y-0');
            }, 50);
            
            container.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
        }
    </script>
    </body>
</html>
