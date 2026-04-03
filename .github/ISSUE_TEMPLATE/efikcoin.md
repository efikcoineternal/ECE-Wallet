---
name: Efikcoin
about: Describe this issue template's purpose here.
title: ''
labels: bug, documentation, duplicate, enhancement, good first issue, help wanted,
  invalid, question, wontfix
assignees: efikcoineternal

---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>EfikCoin Eternal – Playground</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/ethers@5.7.2/dist/ethers.umd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/web3modal@1.9.8/dist/index.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@walletconnect/web3-provider@1.8.0/dist/umd/index.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <style>
        body { background: #0a0a0a; color: #e5e5e5; font-family: 'Inter', sans-serif; }
        .glass-card { background: rgba(20,20,20,0.8); backdrop-filter: blur(12px); border: 1px solid rgba(255,215,0,0.2); border-radius: 1.5rem; }
        .btn { transition: all 0.2s; cursor: pointer; font-weight: 600; }
        .btn:hover { transform: translateY(-2px); box-shadow: 0 6px 14px rgba(255,215,0,0.3); }
        .spinner {
            border: 3px solid rgba(255,255,255,0.2);
            border-radius: 50%;
            border-top: 3px solid #f59e0b;
            width: 24px;
            height: 24px;
            animation: spin 0.8s linear infinite;
            display: inline-block;
            margin-right: 8px;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .btn-loading { opacity: 0.6; pointer-events: none; }
    </style>
</head>
<body class="p-4 md:p-6 min-h-screen flex items-center justify-center">
    <div class="max-w-5xl w-full space-y-6">
        <!-- Header -->
        <div class="text-center">
            <img src="https://raw.githubusercontent.com/Efikcoinofficialecosystem/efikcoin-eternal/main/1772019882787.png" alt="ECE Logo" class="w-20 h-20 mx-auto rounded-full">
            <h1 class="text-4xl font-extrabold bg-gradient-to-r from-amber-400 to-orange-500 bg-clip-text text-transparent">
                EfikCoin Eternal
            </h1>
            <p class="text-gray-400 mt-2">Playground – Real data, demo interactions</p>
        </div>

        <!-- Connection Bar -->
        <div class="glass-card p-4 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div id="connectionIndicator" class="w-3 h-3 rounded-full bg-red-500"></div>
                <span id="connectionStatus" class="font-medium text-gray-300">Not connected</span>
                <span id="networkStatus" class="text-sm text-gray-500"></span>
            </div>
            <div class="flex gap-3">
                <button id="connectBtn" class="btn bg-amber-500 hover:bg-amber-600 text-black px-6 py-2 rounded-full text-sm font-bold shadow-lg">
                    🔌 Connect Wallet
                </button>
            </div>
        </div>

        <!-- User Info -->
        <div id="userInfo" class="glass-card p-4 hidden">
            <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                <div>
                    <p class="text-gray-400 text-sm">Connected Address</p>
                    <p id="userAddress" class="font-mono text-sm break-all bg-black/30 p-2 rounded-lg"></p>
                </div>
                <div class="text-right">
                    <p class="text-gray-400 text-sm">ECE Balance</p>
                    <p id="userBalance" class="text-2xl font-bold text-amber-400">0</p>
                </div>
                <div class="text-right">
                    <p class="text-gray-400 text-sm">BNB Balance</p>
                    <p id="bnbBalance" class="text-2xl font-bold text-amber-400">0</p>
                </div>
                <button id="refreshBtn" class="btn bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded-full text-sm">
                    🔄 Refresh
                </button>
            </div>
        </div>

        <!-- Token Stats & Price -->
        <div class="grid md:grid-cols-3 gap-4">
            <div class="glass-card p-4 text-center">
                <p class="text-gray-400 text-sm">Token Name</p>
                <p id="tokenName" class="text-xl font-bold">EfikCoin Eternal</p>
            </div>
            <div class="glass-card p-4 text-center">
                <p class="text-gray-400 text-sm">Symbol</p>
                <p id="tokenSymbol" class="text-xl font-bold">ECE</p>
            </div>
            <div class="glass-card p-4 text-center">
                <p class="text-gray-400 text-sm">Total Supply</p>
                <p id="totalSupply" class="text-xl font-bold text-amber-400">--</p>
            </div>
        </div>

        <!-- Live Price + Chart -->
        <div class="glass-card p-5">
            <div class="flex justify-between items-center flex-wrap gap-4">
                <div>
                    <p class="text-gray-400 text-sm">Live ECE Price (USD)</p>
                    <p id="price" class="text-3xl font-bold text-amber-400">--</p>
                </div>
                <div>
                    <p class="text-gray-400 text-sm">24h Change</p>
                    <p id="priceChange" class="text-lg font-bold">--</p>
                </div>
                <div>
                    <p class="text-gray-400 text-sm">Network</p>
                    <p class="text-lg font-bold">BSC Mainnet</p>
                </div>
            </div>
            <div class="mt-4">
                <iframe src="https://dexscreener.com/bsc/0x9f8c29e496ecb6c39c221458f211234dfcb233e0?embed=1" width="100%" height="300" style="border: none; border-radius: 1rem;"></iframe>
            </div>
        </div>

        <!-- Demo Features Cards -->
        <div class="grid md:grid-cols-2 gap-4">
            <div class="glass-card p-5">
                <h2 class="text-2xl font-bold text-amber-400 mb-3">🔒 Staking (Demo)</h2>
                <p class="text-gray-300 text-sm mb-3">Lock ECE for up to 365 days and earn up to 750% APR. This is a simulation – real staking will be enabled soon.</p>
                <div class="flex gap-2">
                    <input type="number" id="stakeAmountDemo" placeholder="Amount" class="bg-gray-800 border border-gray-600 rounded px-3 py-2 w-full">
                    <button id="stakeDemoBtn" class="btn bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg">Stake</button>
                </div>
                <div id="demoMessage" class="text-sm text-gray-400 mt-2"></div>
            </div>

            <div class="glass-card p-5">
                <h2 class="text-2xl font-bold text-amber-400 mb-3">📋 Airdrop / Loans (Coming)</h2>
                <p class="text-gray-300 text-sm">The full airdrop and loan systems are available in the production wallet. Here you can request a mock loan to see the flow.</p>
                <button id="mockLoanBtn" class="btn bg-purple-600 hover:bg-purple-700 text-white px-4 py-2 rounded-lg mt-2 w-full">Request Mock Loan</button>
            </div>
        </div>

        <!-- Real Contract Read Functions (safe) -->
        <div class="glass-card p-5">
            <h2 class="text-2xl font-bold text-amber-400 mb-3">📜 Contract Reader (read‑only)</h2>
            <div class="flex flex-wrap gap-2">
                <button class="btn bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded text-sm" onclick="callRead('name')">name()</button>
                <button class="btn bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded text-sm" onclick="callRead('symbol')">symbol()</button>
                <button class="btn bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded text-sm" onclick="callRead('totalSupply')">totalSupply()</button>
                <button class="btn bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded text-sm" onclick="callRead('decimals')">decimals()</button>
                <button class="btn bg-gray-700 hover:bg-gray-600 text-white px-3 py-2 rounded text-sm" onclick="callRead('owner')">owner()</button>
            </div>
            <div id="readResult" class="mt-3 text-sm text-gray-400 bg-black/30 p-2 rounded"></div>
        </div>

        <!-- Footer -->
        <div class="text-center text-gray-500 text-sm">
            <p>© 2026 EfikCoin Eternal · Built for humanity · <span class="text-amber-400">Coin is life</span></p>
            <p class="mt-1 text-xs">This is a playground. Real wallet features (stake, airdrop, loans) are fully functional in the production wallet at <a href="https://efikcoineternal.github.io/ECE-Wallet/" class="text-amber-400 underline">ECE-Wallet</a>.</p>
        </div>
    </div>

    <script>
        // ========== CONTRACT INFO ==========
        const CONTRACT_ADDRESS = "0x9F8C29E496ECB6C39c221458f211234DfCB233E0";
        const OWNER_ADDRESS = "0xc5ad5cfcf81ad63a94227334b898eafce6b27cca".toLowerCase();
        const BSC_RPC = "https://bsc-dataseed.binance.org/";
        const ABI = [
            "function name() view returns (string)",
            "function symbol() view returns (string)",
            "function decimals() view returns (uint8)",
            "function totalSupply() view returns (uint256)",
            "function balanceOf(address) view returns (uint256)",
            "function owner() view returns (address)"
        ];

        // ========== GLOBAL STATE ==========
        let provider, signer, contract, userAddress;
        let web3Modal;

        // ========== DOM ELEMENTS ==========
        const connectBtn = document.getElementById('connectBtn');
        const connectionIndicator = document.getElementById('connectionIndicator');
        const connectionStatus = document.getElementById('connectionStatus');
        const networkStatus = document.getElementById('networkStatus');
        const userInfo = document.getElementById('userInfo');
        const userAddressSpan = document.getElementById('userAddress');
        const userBalanceSpan = document.getElementById('userBalance');
        const bnbBalanceSpan = document.getElementById('bnbBalance');
        const refreshBtn = document.getElementById('refreshBtn');
        const totalSupplySpan = document.getElementById('totalSupply');
        const priceSpan = document.getElementById('price');
        const priceChangeSpan = document.getElementById('priceChange');
        const tokenNameSpan = document.getElementById('tokenName');
        const tokenSymbolSpan = document.getElementById('tokenSymbol');
        const readResultDiv = document.getElementById('readResult');

        // ========== INIT WEB3MODAL ==========
        function initWeb3Modal() {
            const providerOptions = {
                walletconnect: {
                    package: WalletConnectProvider.default,
                    options: { rpc: { 56: BSC_RPC } }
                }
            };
            web3Modal = new Web3Modal.default({ cacheProvider: true, providerOptions, theme: "dark" });
        }

        // ========== CONNECT WALLET ==========
        async function connectWallet() {
            if (!web3Modal) initWeb3Modal();
            try {
                const instance = await web3Modal.connect();
                provider = new ethers.providers.Web3Provider(instance);
                signer = provider.getSigner();
                userAddress = await signer.getAddress();

                connectionIndicator.className = 'w-3 h-3 rounded-full bg-green-500';
                connectionStatus.innerText = 'Connected';
                userInfo.classList.remove('hidden');
                userAddressSpan.innerText = userAddress;

                const network = await provider.getNetwork();
                if (network.chainId !== 56) {
                    networkStatus.innerText = '❌ Wrong network';
                } else {
                    networkStatus.innerText = '✅ BSC Mainnet';
                }

                contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, provider);
                await updateBalances();
                setupListeners(instance);
            } catch (err) {
                alert('Connection failed: ' + err.message);
            }
        }

        function setupListeners(instance) {
            instance.on('accountsChanged', (accounts) => {
                if (accounts.length === 0) disconnectWallet();
                else { userAddress = accounts[0]; updateBalances(); }
            });
            instance.on('chainChanged', () => window.location.reload());
        }

        function disconnectWallet() {
            userAddress = null; contract = null;
            connectionIndicator.className = 'w-3 h-3 rounded-full bg-red-500';
            connectionStatus.innerText = 'Not connected';
            userInfo.classList.add('hidden');
            if (web3Modal) web3Modal.clearCachedProvider();
        }

        connectBtn.addEventListener('click', connectWallet);
        refreshBtn.addEventListener('click', updateBalances);

        async function updateBalances() {
            if (!contract || !userAddress) return;
            try {
                const decimals = await contract.decimals();
                const balance = await contract.balanceOf(userAddress);
                userBalanceSpan.innerText = ethers.utils.formatUnits(balance, decimals) + ' ECE';
                const bnbBalance = await provider.getBalance(userAddress);
                bnbBalanceSpan.innerText = ethers.utils.formatEther(bnbBalance) + ' BNB';
            } catch (err) { console.error(err); }
        }

        // ========== FETCH TOKEN INFO & PRICE ==========
        async function fetchTokenInfo() {
            try {
                const provider = new ethers.providers.JsonRpcProvider(BSC_RPC);
                const contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, provider);
                const name = await contract.name();
                const symbol = await contract.symbol();
                const total = await contract.totalSupply();
                const decimals = await contract.decimals();
                tokenNameSpan.innerText = name;
                tokenSymbolSpan.innerText = symbol;
                totalSupplySpan.innerText = ethers.utils.formatUnits(total, decimals) + ' ECE';
            } catch (e) { console.error(e); }
        }

        async function fetchPrice() {
            try {
                const res = await axios.get(`https://api.dexscreener.com/latest/dex/search?q=${CONTRACT_ADDRESS}`);
                if (res.data.pairs && res.data.pairs.length) {
                    const pair = res.data.pairs[0];
                    const price = parseFloat(pair.priceUsd);
                    const change = pair.priceChange?.h24 ? parseFloat(pair.priceChange.h24).toFixed(2) : 0;
                    priceSpan.innerText = `$${price.toFixed(8)}`;
                    priceChangeSpan.innerText = `${change > 0 ? '+' : ''}${change}%`;
                    priceChangeSpan.style.color = change >= 0 ? '#4ade80' : '#f87171';
                } else {
                    priceSpan.innerText = '$0.00';
                }
            } catch (e) { console.error(e); priceSpan.innerText = '?'; }
        }

        fetchTokenInfo();
        fetchPrice();
        setInterval(fetchPrice, 30000);

        // ========== READ FUNCTIONS ==========
        window.callRead = async function(func) {
            if (!contract) { readResultDiv.innerText = 'Please connect wallet first.'; return; }
            try {
                let result;
                if (func === 'totalSupply') {
                    const decimals = await contract.decimals();
                    result = ethers.utils.formatUnits(await contract.totalSupply(), decimals) + ' ECE';
                } else if (func === 'balanceOf') {
                    const decimals = await contract.decimals();
                    const addr = prompt('Enter address:');
                    if (!addr) return;
                    const bal = await contract.balanceOf(addr);
                    result = ethers.utils.formatUnits(bal, decimals) + ' ECE';
                } else {
                    result = await contract[func]();
                }
                readResultDiv.innerHTML = `<span class="text-amber-400">${func}()</span> → ${result}`;
            } catch (err) {
                readResultDiv.innerHTML = `<span class="text-red-400">Error:</span> ${err.message}`;
            }
        };

        // ========== DEMO STAKE BUTTON ==========
        document.getElementById('stakeDemoBtn').addEventListener('click', () => {
            const amount = document.getElementById('stakeAmountDemo').value;
            if (!amount) return;
            document.getElementById('demoMessage').innerHTML = `
                <span class="spinner"></span> Demo: Staking ${amount} ECE for 90 days (125% APR).<br>
                <span class="text-green-400">✅ In production, this would call the smart contract.</span>
            `;
        });

        document.getElementById('mockLoanBtn').addEventListener('click', () => {
            document.getElementById('demoMessage').innerHTML = `
                <span class="text-amber-400">📋 Mock loan request submitted (demo).<br>
                In the full wallet, you can apply for real loans in ECE, BNB, USDT, BTC, ETH.</span>
            `;
        });

        // Auto‑connect if previously connected
        window.addEventListener('load', () => {
            initWeb3Modal();
            if (web3Modal.cachedProvider) connectWallet();
        });
    </script>
</body>
</html>
