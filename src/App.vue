<script setup>
import { ref, onMounted } from 'vue'
import { Pie } from 'vue-chartjs'
import {
  Chart as ChartJS,
  ArcElement,
  Tooltip,
  Legend
} from 'chart.js'

// Register Chart.js components
ChartJS.register(ArcElement, Tooltip, Legend)

const isMenuOpen = ref(false)
const isScrolled = ref(false)

// Intersection Observer for scroll animations
onMounted(() => {
  const observerOptions = {
    threshold: 0.1,
    rootMargin: '0px 0px -50px 0px'
  }

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('animate-active')
      }
    })
  }, observerOptions)

  // Observe all elements with scroll-animation class
  const animatedElements = document.querySelectorAll('.scroll-animation')
  animatedElements.forEach(el => {
    observer.observe(el)
  })
  
  // Navbar scroll effect
  window.addEventListener('scroll', () => {
    isScrolled.value = window.scrollY > 50
  })
})

// Token allocation data for pie chart
const allocationData = {
  labels: ['Team & Advisors', 'Ecosystem Development', 'Liquidity & Market Making', 'IEO', 'Private Sale', 'Strategic Reserve', 'Strategic Partners'],
  datasets: [
    {
      data: [18, 25, 15, 10, 12, 10, 10],
      backgroundColor: [
        '#4b5563', // gray-600 - 主色调
        '#6b7280', // gray-500 - 辅助色
        '#9ca3af', // gray-400 - 成功色
        '#d1d5db', // gray-300 - 警告色
        '#e5e7eb', // gray-200 - 强调色
        '#f3f4f6', // gray-100 - 信息色
        '#f9fafb'  // gray-50 - 主色调变体
      ],
      borderWidth: 2,
      borderColor: '#ffffff',
      hoverOffset: 10, // 悬停时的偏移效果
      hoverBorderWidth: 3 // 悬停时的边框宽度
    }
  ]
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: 'right',
      labels: {
        font: {
          size: 13,
          family: 'sans-serif',
          weight: '500'
        },
        padding: 25,
        color: '#4b5563', // gray-700 - 与页面文字颜色一致
        usePointStyle: true,
        pointStyle: 'circle'
      }
    },
    tooltip: {
      backgroundColor: 'rgba(255, 255, 255, 0.95)',
      titleColor: '#111827', // gray-900 - 与页面标题颜色一致
      bodyColor: '#4b5563', // gray-600 - 主色调
      borderColor: '#f3f4f6', // gray-100 - 边框颜色
      borderWidth: 1,
      padding: 12,
      cornerRadius: 8,
      boxPadding: 5,
      callbacks: {
        label: function(context) {
          return `${context.label}: ${context.parsed}%`
        },
        labelTextColor: function(context) {
          return context.dataset.backgroundColor[context.dataIndex]
        }
      }
    }
  },
  animation: {
    animateScale: true,
    animateRotate: true,
    duration: 1500,
    easing: 'easeOutQuart'
  }
}
</script>

<template>
  <div class="font-sans leading-relaxed text-neutral-900 bg-gradient-to-br from-gray-50 to-gray-100 min-h-screen">
    <!-- Navigation -->
    <header class="fixed w-full transition-all duration-500 ease-in-out z-50" :class="isScrolled ? 'bg-white/95 backdrop-blur-md shadow-md' : 'bg-white/90 backdrop-blur-sm shadow-sm'">
      <div class="container mx-auto px-4 py-4 flex justify-between items-center">
        <div class="text-2xl font-bold text-gray-600">
          <span class="bg-gray-600 text-white px-3 py-1 rounded">PZG</span> (Pulse Zenith Grid)
        </div>
        
        <!-- Desktop Menu -->
        <nav class="hidden md:flex space-x-8">
          <a href="#home" class="font-medium text-gray-700 hover:text-gray-600 transition-colors">Home</a>
          <a href="#about" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">About PZG</a>
          <a href="#solutions" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Technology</a>
          <a href="#tokenomics" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Tokenomics</a>
          <a href="#features" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Core Advantages</a>
          <a href="#scenarios" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Use Cases</a>
          <a href="#roadmap" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Roadmap</a>
          <a href="#risks" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Risks & Compliance</a>
          <a href="#contact" class="font-medium text-gray-700 hover:text-indigo-600 transition-colors">Contact Us</a>
        </nav>
        
        <!-- Mobile Menu Button -->
        <button class="md:hidden text-gray-700 hover:text-indigo-600" @click="isMenuOpen = !isMenuOpen">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
          </svg>
        </button>
      </div>
      
      <!-- Mobile Menu -->
        <div v-if="isMenuOpen" class="md:hidden bg-white border-t">
          <div class="container mx-auto px-4 py-3 space-y-3">
            <a href="#home" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Home</a>
            <a href="#about" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">About PZG</a>
            <a href="#solutions" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Technology</a>
            <a href="#tokenomics" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Tokenomics</a>
            <a href="#features" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Core Advantages</a>
            <a href="#scenarios" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Use Cases</a>
            <a href="#roadmap" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Roadmap</a>
            <a href="#risks" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Risks & Compliance</a>
            <a href="#contact" class="block py-2 font-medium text-gray-700 hover:text-indigo-600 transition-colors">Contact Us</a>
          </div>
        </div>
    </header>
    
    <!-- Main Content -->
    <main class="pt-24 pb-16">
      <!-- Hero Section -->
      <section id="home" class="container mx-auto px-4 py-16 text-center">
        <h1 class="text-4xl md:text-6xl font-bold text-gray-900 mb-6 leading-tight scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
          Infinite Value · User-Centric Liquidity Network
        </h1>
        <p class="text-lg md:text-xl text-gray-600 mb-10 max-w-3xl mx-auto scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
          PZG is the "intelligent grid" connecting all blockchains. Leveraging decentralized technology and AI, we break down cross-chain value barriers, enabling assets to flow safely and efficiently to value-maximizing scenarios in the global multi-chain ecosystem, achieving integrated, intelligent, and democratized liquidity.
        </p>
        <div class="flex flex-col sm:flex-row justify-center gap-4 scroll-animation opacity-0 transform scale-95 transition-all duration-700 animation-delay-400">
          <button class="bg-indigo-600 hover:bg-gray-700 text-white font-semibold px-8 py-3 rounded-full transition-all hover:shadow-lg transform hover:-translate-y-1">
            View Whitepaper
          </button>
        </div>
      </section>
      
      <!-- Solutions Section -->
      <section id="solutions" class="container mx-auto px-4 py-16">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Three-Core Technical Architecture</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Building an efficient, secure, and intelligent multi-chain value planning network</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
            <div class="bg-gray-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-3 text-gray-800">Cross-Chain Communication Layer</h3>
            <ul class="text-gray-600 space-y-2">
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Decentralized Validator Network: Global node operators stake PZG to qualify</li>
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Consensus Mechanism: Enhanced PBFT, requiring more than two-thirds of weighted nodes to confirm cross-chain messages</li>
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Hybrid Security: Optimistic Validation (1-4 hour challenge period) + ZKP (for high-value/governance transactions)</li>
            </ul>
          </div>
          
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
            <div class="bg-indigo-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-3 text-indigo-800">Value Distribution Layer</h3>
            <ul class="text-gray-600 space-y-2">
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Unified Liquidity Fund: Aggregates liquidity from multi-chain DEXs and lending protocols</li>
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Dynamic Pathfinding Algorithm: Millisecond-level optimization supporting multi-hop routing</li>
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Atomic Transactions: HTLC contracts ensure transactions either complete successfully or fail entirely</li>
            </ul>
          </div>
          
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-400">
            <div class="bg-indigo-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-3 text-indigo-800">AI Revenue Distribution Layer</h3>
            <ul class="text-gray-600 space-y-2">
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Multi-Source Data Integration: Real-time on-chain data, oracles, and risk alerts</li>
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Personalized Strategies: Generates customized cross-chain strategies based on user risk preferences</li>
              <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Automatic Execution & Rebalancing: One-click execution of complex strategies with real-time monitoring and automatic adjustments</li>
            </ul>
          </div>
        </div>
        
        <div class="mt-16 bg-white rounded-xl shadow-lg p-8 scroll-animation opacity-0 transform scale-95 transition-all duration-700 animation-delay-600">
          <h3 class="text-2xl font-bold mb-6 text-center text-indigo-800">Security Mechanisms</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="flex items-start space-x-4">
              <div class="bg-gray-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Multi-Layer Security</h4>
                <p class="text-gray-600">Formal verification + multiple audits + 24/7 anomaly monitoring</p>
              </div>
            </div>
            <div class="flex items-start space-x-4">
              <div class="bg-green-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Distributed Key Management</h4>
                <p class="text-gray-600">Threshold signature technology ensures key security</p>
              </div>
            </div>
            <div class="flex items-start space-x-4">
              <div class="bg-green-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Risk Reserve Fund</h4>
                <p class="text-gray-600">DAO-managed risk reserve ensuring system stability</p>
              </div>
            </div>
            <div class="flex items-start space-x-4">
              <div class="bg-green-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Decentralized Insurance</h4>
                <p class="text-gray-600">Partnerships with Nexus Mutual, InsureDAO and other insurance protocols</p>
              </div>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Tokenomics Section -->
      <section id="tokenomics" class="container mx-auto px-4 py-16">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Tokenomics</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Core information, utilities, allocation mechanism, and economic model of PZG token</p>
        </div>
        
        <!-- Core Token Info -->
        <div class="bg-white rounded-xl shadow-lg p-8 mb-12 scroll-animation opacity-0 transition-opacity duration-700 animation-delay-200">
          <h3 class="text-2xl font-bold mb-6 text-indigo-800">Core Token Information</h3>
          <div class="overflow-x-auto">
            <table class="w-full border-collapse">
              <thead>
                <tr class="bg-gray-50">
                  <th class="border-b border-gray-200 px-4 py-3 text-left text-gray-600 font-semibold">Parameter</th>
                  <th class="border-b border-gray-200 px-4 py-3 text-left text-gray-600 font-semibold">Details</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Token Name</td>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-600">PZG (Pulse Zenith Grid)</td>
                </tr>
                <tr>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Total Supply</td>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-600">100,000,000 (Fixed)</td>
                </tr>
                <tr>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Initial Circulation</td>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-600">18,000,000 (18%)</td>
                </tr>
                <tr>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Blockchain</td>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-600">Ethereum Mainnet</td>
                </tr>
                <tr>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">IEO Exchanges</td>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-600">YXMAX+ & Top Global CEXs</td>
                </tr>
                <tr>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Contract Address</td>
                  <td class="border-b border-gray-200 px-4 py-3 text-gray-600">Available on official website/social media</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        
        <!-- Token Utilities -->
        <div class="bg-white rounded-xl shadow-lg p-8 mb-12 scroll-animation opacity-0 transition-opacity duration-700 animation-delay-400">
          <h3 class="text-2xl font-bold mb-6 text-indigo-800">Token Utilities</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="flex items-start space-x-4">
              <div class="bg-indigo-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Network Fuel</h4>
                <p class="text-gray-600">Pay for cross-chain transactions/AI strategy fees (partial burn + distribution to nodes/ecosystem treasury)</p>
              </div>
            </div>
            <div class="flex items-start space-x-4">
              <div class="bg-indigo-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Governance Rights</h4>
                <p class="text-gray-600">vePZG model - vote on network parameters, protocol updates, and treasury usage</p>
              </div>
            </div>
            <div class="flex items-start space-x-4">
              <div class="bg-indigo-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Node Staking</h4>
                <p class="text-gray-600">Stake PZG to become a validator node and earn rewards</p>
              </div>
            </div>
            <div class="flex items-start space-x-4">
              <div class="bg-indigo-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path>
                </svg>
              </div>
              <div>
                <h4 class="text-lg font-semibold mb-2 text-gray-800">Yield Enhancement</h4>
                <p class="text-gray-600">Lock PZG in "Boost Pools" to get higher strategy weights/yield shares</p>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Allocation & Release -->
        <div class="bg-white rounded-xl shadow-lg p-8 mb-12 scroll-animation opacity-0 transition-opacity duration-700 animation-delay-600">
          <h3 class="text-2xl font-bold mb-6 text-indigo-800">Allocation & Release Rules</h3>
          
          <!-- Pie Chart for Allocation -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-8">
            <div class="h-80">
              <Pie :data="allocationData" :options="chartOptions" />
            </div>
            
            <!-- Release Rules Table -->
            <div class="overflow-x-auto">
              <table class="w-full border-collapse">
                <thead>
                  <tr class="bg-indigo-50">
                    <th class="border-b border-gray-200 px-4 py-3 text-left text-gray-600 font-semibold">Category</th>
                    <th class="border-b border-gray-200 px-4 py-3 text-left text-gray-600 font-semibold">Release Rules</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Team & Advisors</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">Locked for 12 months, then linearly released over 24 months</td>
                  </tr>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Ecosystem Development</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">For R&D, ecosystem expansion, developer incentives</td>
                  </tr>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Liquidity & Market Making</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">6% released at TGE, remaining 94% linearly released over 12 months</td>
                  </tr>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">IEO</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">Fully released at TGE</td>
                  </tr>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Private Sale</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">20% released at TGE, remaining 80% linearly released over 6 months</td>
                  </tr>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Strategic Reserve</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">Managed by DAO for stability and market regulation</td>
                  </tr>
                  <tr>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-700 font-medium">Strategic Partners</td>
                    <td class="border-b border-gray-200 px-4 py-3 text-gray-600">Node ecosystem, cross-chain cooperation, incentives</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
        
        <!-- Economic Model -->
        <div class="bg-white rounded-xl shadow-lg p-8 scroll-animation opacity-0 transition-opacity duration-700 animation-delay-800">
          <h3 class="text-2xl font-bold mb-6 text-indigo-800">Economic Model</h3>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div class="bg-indigo-50 rounded-lg p-6 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-1400">
              <h4 class="text-lg font-semibold mb-3 text-indigo-800">Demand-Driven</h4>
              <p class="text-gray-600">Network growth → increased fee consumption, enhanced governance/staking demand</p>
            </div>
            <div class="bg-indigo-50 rounded-lg p-6 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-1000">
              <h4 class="text-lg font-semibold mb-3 text-indigo-800">Deflationary Mechanism</h4>
              <p class="text-gray-600">Continuous fee burning + long-term locking + staking → controlled supply</p>
            </div>
            <div class="bg-indigo-50 rounded-lg p-6 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-1200">
              <h4 class="text-lg font-semibold mb-3 text-indigo-800">Value Distribution</h4>
              <p class="text-gray-600">50% Node Staking, 30% Ecosystem Treasury, 20% Burn</p>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Features Section -->
      <section id="features" class="container mx-auto px-4 py-16 bg-white rounded-3xl shadow-xl -mt-8">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Core Advantages</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Key advantages of building a multi-chain value planning network</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-12 items-center">
          <div class="space-y-6 scroll-animation opacity-0 transform translate-x-10 transition-all duration-700 animation-delay-200">
            <div class="flex items-start space-x-4">
              <div class="bg-green-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h3 class="text-xl font-semibold mb-2 text-gray-800">Architectural Advantages</h3>
                <ul class="text-gray-600 space-y-2">
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Global Liquidity Network: One-stop access to aggregated cross-chain liquidity</li>
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Smart Multi-hop Routing: Dynamic path optimization for lower costs/faster speeds</li>
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Modular Scalability: Rapid integration of new chains through standardized modules</li>
                </ul>
              </div>
            </div>
            
            <div class="flex items-start space-x-4">
              <div class="bg-green-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h3 class="text-xl font-semibold mb-2 text-gray-800">Technical Advantages</h3>
                <ul class="text-gray-600 space-y-2">
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Trustless Security: Decentralized validator network eliminating reliance on multi-sig/centralized oracles</li>
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>AI Value Creation: Automated complex cross-chain DeFi strategies for everyday users</li>
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Full-Lifecycle Security: End-to-end protection (auditing, monitoring, risk circuit breakers)</li>
                </ul>
              </div>
            </div>
            
            <div class="flex items-start space-x-4">
              <div class="bg-green-100 p-2 rounded-full">
                <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
              </div>
              <div>
                <h3 class="text-xl font-semibold mb-2 text-gray-800">Ecosystem & Governance Advantages</h3>
                <ul class="text-gray-600 space-y-2">
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Circular Token Economy: Token utility directly linked to network growth</li>
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>DAO Governance: Community-led decisions with no team backdoors</li>
                  <li class="flex items-start"><span class="text-green-600 mr-2">•</span>Wide Adaptability: Supports DeFi, NFTs, GameFi, and RWAs</li>
                </ul>
              </div>
            </div>
          </div>
          
          <div class="relative scroll-animation opacity-0 transform -translate-x-10 transition-all duration-700 animation-delay-400">
            <div class="bg-gradient-to-br from-indigo-500 to-purple-600 rounded-2xl p-1">
              <div class="bg-white rounded-xl p-6">
                <div class="flex justify-between items-center mb-4">
                  <div class="flex space-x-2">
                    <div class="w-3 h-3 rounded-full bg-red-500"></div>
                    <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
                    <div class="w-3 h-3 rounded-full bg-green-500"></div>
                  </div>
                  <div class="text-xs text-gray-500">PZG Multi-Chain Value Network</div>
                </div>
                <div class="space-y-4">
                  <div>
                    <div class="flex justify-between text-sm font-medium mb-1">
                      <span>Multi-Chain Connections</span>
                      <span class="text-green-600">20+</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-2">
                      <div class="bg-green-600 h-2 rounded-full scroll-animation transform scale-x-0 origin-left transition-all duration-1000 animation-delay-600" style="width: 80%"></div>
                    </div>
                  </div>
                  <div>
                    <div class="flex justify-between text-sm font-medium mb-1">
                      <span>Liquidity Aggregation</span>
                      <span class="text-green-600">$500M+</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-2">
                      <div class="bg-blue-600 h-2 rounded-full scroll-animation transform scale-x-0 origin-left transition-all duration-1000 animation-delay-800" style="width: 75%"></div>
                    </div>
                  </div>
                  <div>
                    <div class="flex justify-between text-sm font-medium mb-1">
                      <span>Validator Nodes</span>
                      <span class="text-green-600">500+</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-2">
                      <div class="bg-purple-600 h-2 rounded-full scroll-animation transform scale-x-0 origin-left transition-all duration-1000 animation-delay-1000" style="width: 90%"></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="absolute -bottom-6 -right-6 bg-indigo-600 text-white p-4 rounded-xl shadow-lg">
              <div class="text-2xl font-bold">+100</div>
              <div class="text-sm">Ecosystem Partners</div>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Application Scenarios Section -->
      <section id="scenarios" class="container mx-auto px-4 py-16">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Use Cases</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Customized multi-chain value solutions for different user groups</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
            <div class="bg-blue-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-4 text-indigo-800">Individual Users</h3>
            <ul class="text-gray-600 space-y-3">
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>One-Click Multi-Chain Configuration:</strong> Automatically execute cross-chain transfers, swaps, and staking for optimal asset allocation</span>
              </li>
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>Seamless Cross-Chain Payments:</strong> Use assets from one chain to pay on another (no need to pre-stake Gas tokens)</span>
              </li>
            </ul>
          </div>
          
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
            <div class="bg-purple-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-4 text-indigo-800">Traders & Institutions</h3>
            <ul class="text-gray-600 space-y-3">
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>Automated Cross-Chain Arbitrage:</strong> Real-time interest rate monitoring + atomic execution of lending/deposit strategies</span>
              </li>
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>Institutional-Level Liquidity Provision:</strong> Cross-chain unified market making with centralized rebalancing</span>
              </li>
            </ul>
          </div>
          
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-400">
            <div class="bg-green-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-4 text-indigo-800">Developers & Project Teams</h3>
            <ul class="text-gray-600 space-y-3">
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>One-Click Multi-Chain Deployment:</strong> Cross-chain contract deployment + access to ecosystem funds for initial liquidity</span>
              </li>
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>Cross-Chain Incentive Mechanism:</strong> Cross-chain reward users without manual claiming</span>
              </li>
            </ul>
          </div>
          
          <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl transform hover:-translate-y-2 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-600">
            <div class="bg-pink-100 w-14 h-14 rounded-full flex items-center justify-center mb-6">
              <svg class="w-7 h-7 text-pink-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"></path>
              </svg>
            </div>
            <h3 class="text-xl font-bold mb-4 text-indigo-800">Emerging Sectors</h3>
            <ul class="text-gray-600 space-y-3">
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>NFT/GameFi Cross-Chain Utility:</strong> Securely transfer NFTs/game assets across chains to unlock liquidity</span>
              </li>
              <li class="flex items-start">
                <span class="text-indigo-600 mr-2 font-bold">•</span>
                <span><strong>Multi-Chain RWA Access:</strong> Compliant on-chain access to tokenized real-world assets (e.g., U.S. Treasuries)</span>
              </li>
            </ul>
          </div>
        </div>
      </section>
      
      <!-- Development Roadmap Section -->
      <section id="roadmap" class="container mx-auto px-4 py-16">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Development Roadmap</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Phased development plan for PZG multi-chain value network</p>
        </div>
        
        <div class="max-w-4xl mx-auto">
          <div class="relative">
            <!-- Timeline line -->
            <div class="absolute left-1/2 transform -translate-x-1/2 h-full w-1 bg-indigo-200 hidden md:block"></div>
            
            <!-- Phase 1 -->
            <div class="relative mb-16 md:mb-24 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
              <div class="md:flex items-center">
                <div class="md:w-1/2 md:pr-12 md:text-right mb-8 md:mb-0">
                  <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl">
                    <h3 class="text-xl font-bold mb-2 text-indigo-800">Network Launch & Core Infrastructure</h3>
                    <p class="text-indigo-600 font-semibold mb-4">Q4 2025</p>
                    <ul class="text-gray-600 space-y-2">
                      <li class="flex items-start justify-end"><span class="text-indigo-600 mr-2">•</span>Complete PZG-CMP V1 development and audit</li>
                      <li class="flex items-start justify-end"><span class="text-indigo-600 mr-2">•</span>IEO Launch</li>
                      <li class="flex items-start justify-end"><span class="text-indigo-600 mr-2">•</span>Airdrop for early contributors</li>
                    </ul>
                  </div>
                </div>
                
                <div class="absolute left-1/2 transform -translate-x-1/2 w-12 h-12 bg-indigo-600 rounded-full flex items-center justify-center z-10">
                  <span class="text-white font-bold">1</span>
                </div>
                
                <div class="md:w-1/2 md:pl-12 hidden md:block"></div>
              </div>
            </div>
            
            <!-- Phase 2 -->
            <div class="relative mb-16 md:mb-24 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
              <div class="md:flex items-center">
                <div class="md:w-1/2 md:pr-12 hidden md:block"></div>
                
                <div class="absolute left-1/2 transform -translate-x-1/2 w-12 h-12 bg-indigo-600 rounded-full flex items-center justify-center z-10">
                  <span class="text-white font-bold">2</span>
                </div>
                
                <div class="md:w-1/2 md:pl-12">
                  <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl">
                    <h3 class="text-xl font-bold mb-2 text-indigo-800">Protocol Expansion</h3>
                    <p class="text-indigo-600 font-semibold mb-4">Q1 2026</p>
                    <ul class="text-gray-600 space-y-2">
                      <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Launch V1 Mainnet and DVN</li>
                      <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Enable DAO Governance</li>
                      <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Release Beta Dashboard</li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Phase 3 -->
            <div class="relative mb-16 md:mb-24 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-400">
              <div class="md:flex items-center">
                <div class="md:w-1/2 md:pr-12 md:text-right mb-8 md:mb-0">
                  <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl">
                    <h3 class="text-xl font-bold mb-2 text-indigo-800">AI Engine Integration</h3>
                    <p class="text-indigo-600 font-semibold mb-4">Q2 2026</p>
                    <ul class="text-gray-600 space-y-2">
                      <li class="flex items-start justify-end"><span class="text-indigo-600 mr-2">•</span>Launch AI Revenue Engine Beta</li>
                      <li class="flex items-start justify-end"><span class="text-indigo-600 mr-2">•</span>Deep DeFi Protocol Integration</li>
                      <li class="flex items-start justify-end"><span class="text-indigo-600 mr-2">•</span>Mobile DApp Release</li>
                    </ul>
                  </div>
                </div>
                
                <div class="absolute left-1/2 transform -translate-x-1/2 w-12 h-12 bg-indigo-600 rounded-full flex items-center justify-center z-10">
                  <span class="text-white font-bold">3</span>
                </div>
                
                <div class="md:w-1/2 md:pl-12 hidden md:block"></div>
              </div>
            </div>
            
            <!-- Phase 4 -->
            <div class="relative scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-600">
              <div class="md:flex items-center">
                <div class="md:w-1/2 md:pr-12 hidden md:block"></div>
                
                <div class="absolute left-1/2 transform -translate-x-1/2 w-12 h-12 bg-indigo-600 rounded-full flex items-center justify-center z-10">
                  <span class="text-white font-bold">4</span>
                </div>
                
                <div class="md:w-1/2 md:pl-12">
                  <div class="bg-white rounded-xl shadow-lg p-8 transition-all hover:shadow-xl">
                    <h3 class="text-xl font-bold mb-2 text-indigo-800">Ecosystem Deepening</h3>
                    <p class="text-indigo-600 font-semibold mb-4">Q3 2026+</p>
                    <ul class="text-gray-600 space-y-2">
                      <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Cross-Chain Risk Dashboard</li>
                      <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>RWA/GameFi Support</li>
                      <li class="flex items-start"><span class="text-indigo-600 mr-2">•</span>Release PZG V2 (Native Lending, Re-staking, Full DAO Governance)</li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Risks & Compliance Section -->
      <section id="risks" class="container mx-auto px-4 py-16 bg-white rounded-3xl shadow-xl">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Risks & Compliance</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Understanding potential risks and compliance requirements of PZG Network</p>
        </div>
        
        <div class="max-w-3xl mx-auto space-y-12">
          <!-- Key Risks -->
          <div class="scroll-animation opacity-0 transform translate-x-10 transition-all duration-700">
            <h3 class="text-2xl font-bold mb-6 text-indigo-800">Key Risks</h3>
            <ul class="space-y-4 text-gray-600">
              <li class="flex items-start scroll-animation opacity-0 transform translate-y-5 transition-all duration-700 animation-delay-200">
                <span class="text-red-500 mr-3 mt-1">⚠️</span>
                <span><strong>Market Volatility:</strong> Cryptocurrency prices are significantly affected by market sentiment, regulatory policies, and macroeconomics.</span>
              </li>
              <li class="flex items-start scroll-animation opacity-0 transform translate-y-5 transition-all duration-700 animation-delay-400">
                <span class="text-red-500 mr-3 mt-1">⚠️</span>
                <span><strong>Technical Vulnerabilities:</strong> Smart contracts or cross-chain protocols may have unknown flaws.</span>
              </li>
              <li class="flex items-start scroll-animation opacity-0 transform translate-y-5 transition-all duration-700 animation-delay-600">
                <span class="text-red-500 mr-3 mt-1">⚠️</span>
                <span><strong>Integration Risks:</strong> Dependency on underlying chains or third-party protocols may cause service disruptions.</span>
              </li>
              <li class="flex items-start scroll-animation opacity-0 transform translate-y-5 transition-all duration-700 animation-delay-800">
                <span class="text-red-500 mr-3 mt-1">⚠️</span>
                <span><strong>DAO Governance Risks:</strong> There may be inefficiencies, conflicts, or voting manipulation.</span>
              </li>
              <li class="flex items-start scroll-animation opacity-0 transform translate-y-5 transition-all duration-700 animation-delay-1000">
                <span class="text-red-500 mr-3 mt-1">⚠️</span>
                <span><strong>Private Key Responsibility:</strong> Users are responsible for protecting their private keys and mnemonics.</span>
              </li>
            </ul>
          </div>
          
          <!-- Compliance Statement & Disclaimer -->
          <div class="bg-gray-50 p-8 rounded-xl scroll-animation opacity-0 transform -translate-x-10 transition-all duration-700 animation-delay-1200">
            <h3 class="text-2xl font-bold mb-4 text-indigo-800">Compliance Statement</h3>
            <p class="text-gray-600 mb-6">PZG complies with global regulatory requirements. Users must ensure compliance with local laws and regulations (securities, tax, anti-money laundering, etc.) when using the protocol. PZG does not provide legal or tax advice.</p>
            
            <h3 class="text-2xl font-bold mb-4 text-indigo-800">Disclaimer</h3>
            <p class="text-gray-600">The content of this website is for reference only and does not constitute investment advice. Using PZG indicates that users voluntarily assume all related risks. The PZG Foundation, team, and contributors shall not be liable for direct/indirect losses caused by the use of the protocol. Roadmaps and details may be adjusted by the DAO.</p>
          </div>
        </div>
      </section>
      
      <!-- About Section -->
      <section id="about" class="container mx-auto px-4 py-16">
        <div class="text-center mb-16">
          <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">About PZG</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">Building the 'Intelligent Network' of Digital Value</p>
        </div>
        
        <div class="max-w-3xl mx-auto space-y-8">
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
            <h3 class="text-2xl font-semibold text-indigo-800 mb-4">Industry Status</h3>
            <p class="text-gray-600">
              Blockchain has entered the era of 'multi-chain coexistence'. Ethereum-compatible chains, high-performance public chains, and Layer 2 solutions are thriving, but three key pain points hinder ecological growth: fragmented liquidity, complex user experience, and low capital utilization efficiency.
            </p>
          </div>
          
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
            <h3 class="text-2xl font-semibold text-indigo-800 mb-4">Market Demand</h3>
            <p class="text-gray-600">
              Beyond traditional 'point-to-point bridge 1.0' solutions, the market needs a next-generation multi-chain value planning network with: many-to-many connections (linking all key chains), intelligent routing (optimal asset transfer paths), value growth (automatic execution of value-added strategies), and decentralized security (no single chain/centralized dependency).
            </p>
          </div>
          
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-400">
            <h3 class="text-2xl font-semibold text-indigo-800 mb-4">PZG's Mission</h3>
            <p class="text-gray-600">
              Building the 'intelligent network' of digital value - eliminating cross-chain barriers through decentralized networks and AI. We ensure that any asset can flow safely and instantly to its highest value chain at any time, returning liquidity and yield sovereignty to users. PZG aims to become the foundational protocol for future blockchain interoperability and free value flow.
            </p>
          </div>
        </div>
      </section>
      
      <!-- Contact Section -->
      <section id="contact" class="container mx-auto px-4 py-16">
        <div class="max-w-4xl mx-auto bg-white rounded-3xl shadow-xl p-8 md:p-12 scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
          <div class="text-center mb-12">
            <h2 class="text-3xl md:text-4xl font-bold text-indigo-900 mb-4">Contact Us</h2>
          <p class="text-gray-600">Interested in learning more about PZG multi-chain value network? Please contact us</p>
          </div>
          
          <form class="space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
                <label class="block text-sm font-medium text-gray-700 mb-1">Name</label>
              <input type="text" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-500 hover:border-indigo-400 hover:shadow-md" placeholder="Your Name">
              </div>
              <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-400">
                <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
              <input type="email" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-500 hover:border-indigo-400 hover:shadow-md" placeholder="Your Email">
              </div>
            </div>
            <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-600">
              <label class="block text-sm font-medium text-gray-700 mb-1">Project Name</label>
              <input type="text" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-500 hover:border-indigo-400 hover:shadow-md" placeholder="Your Project Name">
            </div>
            <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-800">
              <label class="block text-sm font-medium text-gray-700 mb-1">Message</label>
              <textarea class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-500 hover:border-indigo-400 hover:shadow-md" rows="4" placeholder="Please tell us your needs"></textarea>
            </div>
            <div class="text-center scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-1000">
              <button type="submit" class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-8 py-3 rounded-full transition-all hover:shadow-lg transform hover:-translate-y-1 duration-300">
                Send Message
              </button>
            </div>
          </form>
        </div>
      </section>
    </main>
    
    <!-- Footer -->
    <footer class="bg-indigo-900 text-white py-12">
      <div class="container mx-auto px-4">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-8">
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700">
            <div class="text-2xl font-bold mb-4">
              <span class="bg-white text-indigo-600 px-3 py-1 rounded">PZG</span> (Pulse Zenith Grid)
            </div>
            <p class="text-indigo-200 mb-4">Infinite Value · User-Centric Liquidity Network</p>
            <div class="flex space-x-4">
              <a href="#" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-125 hover:rotate-6">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"></path>
                </svg>
              </a>
              <a href="#" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-125 hover:rotate-6">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"></path>
                </svg>
              </a>
              <a href="#" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-125 hover:rotate-6">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm-3.803 15.83c-.497 0-.9-.403-.9-.9s.403-.9.9-.9.9.403.9.9-.403.9-.9.9zm7.606 0c-.497 0-.9-.403-.9-.9s.403-.9.9-.9.9.403.9.9-.403.9-.9.9zm-3.803 0c-.497 0-.9-.403-.9-.9s.403-.9.9-.9.9.403.9.9-.403.9-.9.9z"></path>
                </svg>
              </a>
            </div>
          </div>
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-200">
            <h4 class="text-lg font-semibold mb-4">Official Links</h4>
            <ul class="space-y-2">
              <li><a href="#" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-105">Website</a></li>
              <li><a href="#" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-105">Whitepaper</a></li>
            </ul>
          </div>
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-400">
            <h4 class="text-lg font-semibold mb-4">Support</h4>
            <ul class="space-y-2">
              <li><a href="#" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-105">FAQ</a></li>
              <li><a href="#contact" class="text-indigo-200 hover:text-white transition-all duration-500 ease-in-out transform hover:-translate-y-2 hover:scale-105">Contact Form</a></li>
            </ul>
          </div>
          <div class="scroll-animation opacity-0 transform translate-y-10 transition-all duration-700 animation-delay-600">
            <h4 class="text-lg font-semibold mb-4 mt-8">Contact Information</h4>
            <ul class="space-y-2">
              <li class="flex items-center space-x-2">
                <svg class="w-5 h-5 text-indigo-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path>
                </svg>
                <span class="text-indigo-200">contact@pzgproject.site</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="border-t border-indigo-800 mt-8 pt-8 text-center text-indigo-300">
          <p>&copy; 2025 PZG Project. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* Additional custom styles if needed */
</style>
