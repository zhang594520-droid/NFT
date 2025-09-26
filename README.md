<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NFT复投计算器</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#3B82F6',
                        secondary: '#10B981',
                        accent: '#F59E0B',
                        dark: '#1E293B',
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .scrollbar-thin {
                scrollbar-width: thin;
            }
            .scrollbar-hide::-webkit-scrollbar {
                display: none;
            }
        }
    </style>
</head>
<body class="bg-gray-50 font-sans text-dark">
    <!-- 顶部导航 -->
    <header class="bg-primary text-white shadow-md">
        <div class="container mx-auto px-4 py-4 flex justify-between items-center">
            <h1 class="text-xl md:text-2xl font-bold">
                <i class="fa fa-calculator mr-2"></i>NFT复投计算器
            </h1>
            <button id="infoBtn" class="p-2 rounded-full hover:bg-primary-700 transition">
                <i class="fa fa-info-circle"></i>
            </button>
        </div>
    </header>

    <!-- 联系方式区域 -->
    <div class="bg-amber-50 border border-amber-200 py-4">
        <div class="container mx-auto px-4 max-w-4xl">
            <div class="space-y-2 text-center text-gray-800">
                <p class="font-medium"><i class="fa fa-weixin text-green-600 mr-1"></i> 买卖PIFI认准微信张帅：<span class="text-red-600">zhang709617619</span></p>
                <p class="font-medium"><i class="fa fa-wallet text-purple-600 mr-1"></i> 接收唯一钱包地址：<span class="text-purple-800 text-sm md:text-base break-all">0xBd0C6Aa5CB43Bd1026CaaA1F0E5B111cd6cF6904</span></p>
                <p class="text-sm italic"><i class="fa fa-lightbulb-o text-amber-500 mr-1"></i> 我只是个黄牛，搬砖赚点烟钱！同时让买的少花钱，让卖的多卖钱，仅此而已</p>
            </div>
        </div>
    </div>

    <!-- 主内容区 -->
    <main class="container mx-auto px-4 py-6 max-w-4xl">
        <!-- 输入面板 -->
        <div class="bg-white rounded-lg shadow-md p-5 mb-6">
            <h2 class="text-lg font-semibold mb-4 border-b pb-2">计算参数设置</h2>
            
            <div class="mb-4">
                <label class="block text-gray-700 mb-2 font-medium">计算模式</label>
                <div class="flex flex-wrap gap-4">
                    <label class="inline-flex items-center">
                        <input type="radio" name="mode" value="1" checked class="form-radio text-primary">
                        <span class="ml-2">输入初始NFT数量</span>
                    </label>
                    <label class="inline-flex items-center">
                        <input type="radio" name="mode" value="2" class="form-radio text-primary">
                        <span class="ml-2">输入每日产币量</span>
                    </label>
                </div>
            </div>

            <div id="nftInputGroup" class="mb-4">
                <label for="initialNft" class="block text-gray-700 mb-2 font-medium">初始NFT数量</label>
                <input type="number" id="initialNft" min="1" value="140" 
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
            </div>

            <div id="pifiInputGroup" class="mb-4 hidden">
                <label for="dailyPifi" class="block text-gray-700 mb-2 font-medium">每日产币量</label>
                <input type="number" id="dailyPifi" min="0.1" step="0.1" value="112" 
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
            </div>

            <div class="mb-4">
                <label for="totalDays" class="block text-gray-700 mb-2 font-medium">计算天数</label>
                <input type="number" id="totalDays" min="1" max="1000" value="365" 
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
            </div>

            <button id="calculateBtn" class="w-full bg-secondary hover:bg-secondary/90 text-white font-semibold py-3 px-4 rounded-md transition flex items-center justify-center">
                <i class="fa fa-calculator mr-2"></i>开始计算
            </button>
        </div>

        <!-- 计算结果 -->
        <div id="resultSection" class="hidden">
            <!-- 结果摘要 -->
            <div class="bg-white rounded-lg shadow-md p-5 mb-6">
                <h2 class="text-lg font-semibold mb-4 border-b pb-2">计算结果摘要</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="flex items-center">
                        <div class="bg-blue-100 p-3 rounded-full mr-3">
                            <i class="fa fa-cubes text-primary text-xl"></i>
                        </div>
                        <div>
                            <div class="text-sm text-gray-500">初始NFT数量</div>
                            <div id="summaryInitial" class="text-xl font-semibold">140张</div>
                        </div>
                    </div>
                    <div class="flex items-center">
                        <div class="bg-green-100 p-3 rounded-full mr-3">
                            <i class="fa fa-calendar text-secondary text-xl"></i>
                        </div>
                        <div>
                            <div class="text-sm text-gray-500">计算天数</div>
                            <div id="summaryDays" class="text-xl font-semibold">365天</div>
                        </div>
                    </div>
                    <div class="flex items-center">
                        <div class="bg-purple-100 p-3 rounded-full mr-3">
                            <i class="fa fa-line-chart text-purple-600 text-xl"></i>
                        </div>
                        <div>
                            <div class="text-sm text-gray-500">最终持有数量</div>
                            <div id="summaryFinal" class="text-xl font-semibold">0张</div>
                        </div>
                    </div>
                    <div class="flex items-center">
                        <div class="bg-amber-100 p-3 rounded-full mr-3">
                            <i class="fa fa-refresh text-accent text-xl"></i>
                        </div>
                        <div>
                            <div class="text-sm text-gray-500">累计复投数量</div>
                            <div id="summaryReinvest" class="text-xl font-semibold">0张</div>
                        </div>
                    </div>
                    <div class="flex items-center">
                        <div class="bg-red-100 p-3 rounded-full mr-3">
                            <i class="fa fa-times-circle text-red-500 text-xl"></i>
                        </div>
                        <div>
                            <div class="text-sm text-gray-500">累计消失数量</div>
                            <div id="summaryExpired" class="text-xl font-semibold">0张</div>
                        </div>
                    </div>
                    <div class="flex items-center">
                        <div class="bg-indigo-100 p-3 rounded-full mr-3">
                            <i class="fa fa-coins text-indigo-600 text-xl"></i>
                        </div>
                        <div>
                            <div class="text-sm text-gray-500">剩余可复投PIFI</div>
                            <div id="summaryRemaining" class="text-xl font-semibold">0</div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 详细数据表格 -->
            <div class="bg-white rounded-lg shadow-md p-5 mb-6">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-lg font-semibold">每日详细数据</h2>
                    <div class="flex gap-2">
                        <button id="showLess" class="px-3 py-1 bg-gray-200 hover:bg-gray-300 rounded text-sm transition">
                            精简显示
                        </button>
                        <button id="showMore" class="px-3 py-1 bg-gray-200 hover:bg-gray-300 rounded text-sm transition hidden">
                            显示全部
                        </button>
                    </div>
                </div>
                
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200 text-sm">
                        <thead class="bg-gray-100">
                            <tr>
                                <th class="px-3 py-3 text-left font-semibold">天数</th>
                                <th class="px-3 py-3 text-left font-semibold">持有量</th>
                                <th class="px-3 py-3 text-left font-semibold">当天消失</th>
                                <th class="px-3 py-3 text-left font-semibold">当天产币</th>
                                <th class="px-3 py-3 text-left font-semibold">累计产币</th>
                                <th class="px-3 py-3 text-left font-semibold">累计可复投</th>
                                <th class="px-3 py-3 text-left font-semibold">复投记录</th>
                            </tr>
                        </thead>
                        <tbody id="detailTable" class="bg-white divide-y divide-gray-200">
                            <!-- 表格内容将通过JS动态生成 -->
                        </tbody>
                    </table>
                </div>
                
                <div id="moreDataIndicator" class="text-center py-3 text-gray-500 hidden">
                    <i class="fa fa-ellipsis-h mr-1"></i> 中间数据已省略 <i class="fa fa-ellipsis-h ml-1"></i>
                </div>
            </div>
        </div>
    </main>

    <!-- 信息弹窗 -->
    <div id="infoModal" class="fixed inset-0 bg-black/50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg max-w-md w-full mx-4 p-6 shadow-xl">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-bold text-primary">计算规则说明</h3>
                <button id="closeInfo" class="text-gray-500 hover:text-gray-700">
                    <i class="fa fa-times text-xl"></i>
                </button>
            </div>
            <div class="space-y-3 text-sm">
                <p><strong>1. 产币规则：</strong>按30天周期计算，1-3天0.8PIFI/张，4-6天0.9PIFI/张...28-30天1.7PIFI/张</p>
                <p><strong>2. 复投规则：</strong>每3天可复投一次，100PIFI可复投1张，复投后所有NFT均从0.8PIFI/天重新开始计算</p>
                <p><strong>3. 消失规则：</strong>累计总产币量达到300的倍数时，对应数量的NFT消失（累计300消失1张，累计600消失2张...）</p>
                <p><strong>使用说明：</strong>选择计算模式，输入相应参数，点击"开始计算"即可获得结果，在手机上也可正常使用</p>
            </div>
            <div class="mt-6">
                <button id="gotItBtn" class="w-full bg-primary hover:bg-primary/90 text-white font-medium py-2 px-4 rounded transition">
                    明白了
                </button>
            </div>
        </div>
    </div>

    <!-- 加载中提示 -->
    <div id="loadingModal" class="fixed inset-0 bg-black/50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg p-6 flex flex-col items-center">
            <div class="animate-spin rounded-full h-12 w-12 border-t-2 border-b-2 border-primary mb-4"></div>
            <p class="text-gray-700">计算中，请稍候...</p>
        </div>
    </div>

    <script>
        // 产币周期表
        const earnCycle = {
            1:0.8, 2:0.8, 3:0.8, 4:0.9, 5:0.9, 6:0.9,
            7:1.0, 8:1.0, 9:1.0, 10:1.1, 11:1.1, 12:1.1,
            13:1.2, 14:1.2, 15:1.2, 16:1.3, 17:1.3, 18:1.3,
            19:1.4, 20:1.4, 21:1.4, 22:1.5, 23:1.5, 24:1.5,
            25:1.6, 26:1.6, 27:1.6, 28:1.7, 29:1.7, 30:1.7
        };

        // DOM元素
        const modeRadios = document.querySelectorAll('input[name="mode"]');
        const nftInputGroup = document.getElementById('nftInputGroup');
        const pifiInputGroup = document.getElementById('pifiInputGroup');
        const initialNftInput = document.getElementById('initialNft');
        const dailyPifiInput = document.getElementById('dailyPifi');
        const totalDaysInput = document.getElementById('totalDays');
        const calculateBtn = document.getElementById('calculateBtn');
        const resultSection = document.getElementById('resultSection');
        const detailTable = document.getElementById('detailTable');
        const summaryInitial = document.getElementById('summaryInitial');
        const summaryDays = document.getElementById('summaryDays');
        const summaryFinal = document.getElementById('summaryFinal');
        const summaryReinvest = document.getElementById('summaryReinvest');
        const summaryExpired = document.getElementById('summaryExpired');
        const summaryRemaining = document.getElementById('summaryRemaining');
        const infoBtn = document.getElementById('infoBtn');
        const infoModal = document.getElementById('infoModal');
        const closeInfo = document.getElementById('closeInfo');
        const gotItBtn = document.getElementById('gotItBtn');
        const loadingModal = document.getElementById('loadingModal');
        const showLessBtn = document.getElementById('showLess');
        const showMoreBtn = document.getElementById('showMore');
        const moreDataIndicator = document.getElementById('moreDataIndicator');

        // 模式切换
        modeRadios.forEach(radio => {
            radio.addEventListener('change', () => {
                if (radio.value === '1') {
                    nftInputGroup.classList.remove('hidden');
                    pifiInputGroup.classList.add('hidden');
                } else {
                    nftInputGroup.classList.add('hidden');
                    pifiInputGroup.classList.remove('hidden');
                }
            });
        });

        // 信息弹窗控制
        infoBtn.addEventListener('click', () => {
            infoModal.classList.remove('hidden');
        });

        [closeInfo, gotItBtn].forEach(btn => {
            btn.addEventListener('click', () => {
                infoModal.classList.add('hidden');
            });
        });

        // 计算按钮点击事件
        calculateBtn.addEventListener('click', calculate);

        // 显示/隐藏更多数据
        let showingAllData = true;
        let allRows = [];
        
        showLessBtn.addEventListener('click', () => {
            if (allRows.length <= 30) return;
            
            showingAllData = false;
            showLessBtn.classList.add('hidden');
            showMoreBtn.classList.remove('hidden');
            moreDataIndicator.classList.remove('hidden');
            
            // 只显示前10行和后10行
            const rowsToShow = [
                ...allRows.slice(0, 10),
                ...allRows.slice(-10)
            ];
            
            renderTableRows(rowsToShow);
        });
        
        showMoreBtn.addEventListener('click', () => {
            showingAllData = true;
            showLessBtn.classList.remove('hidden');
            showMoreBtn.classList.add('hidden');
            moreDataIndicator.classList.add('hidden');
            renderTableRows(allRows);
        });

        // 计算函数 - 核心逻辑：累计产币达到300消失对应数量NFT
        function calculate() {
            // 显示加载中
            loadingModal.classList.remove('hidden');
            
            // 为了在UI上有加载效果，使用setTimeout
            setTimeout(() => {
                try {
                    // 获取输入值
                    let initialNft, totalDays;
                    
                    if (document.querySelector('input[name="mode"]:checked').value === '1') {
                        initialNft = parseInt(initialNftInput.value);
                        if (isNaN(initialNft) || initialNft <= 0) {
                            alert('请输入有效的初始NFT数量');
                            loadingModal.classList.add('hidden');
                            return;
                        }
                    } else {
                        const dailyPifi = parseFloat(dailyPifiInput.value);
                        if (isNaN(dailyPifi) || dailyPifi <= 0) {
                            alert('请输入有效的每日产币量');
                            loadingModal.classList.add('hidden');
                            return;
                        }
                        // 按周期1的0.8计算初始数量
                        initialNft = Math.round(dailyPifi / 0.8);
                    }
                    
                    totalDays = parseInt(totalDaysInput.value);
                    if (isNaN(totalDays) || totalDays <= 0 || totalDays > 1000) {
                        alert('请输入1-1000之间的天数');
                        loadingModal.classList.add('hidden');
                        return;
                    }

                    // 初始化数据
                    let nftCycles = Array(initialNft).fill(1); // 所有NFT初始周期为1
                    let currentHold = initialNft;
                    let totalPifi = 0.0;          // 可复投的PIFI
                    let totalEarned = 0.0;        // 累计总产币量（用于计算消失）
                    let lastReinvestDay = 0;
                    let totalReinvest = 0;
                    let totalExpired = 0;
                    let dailyData = [];

                    // 计算每一天的数据
                    for (let day = 1; day <= totalDays; day++) {
                        let todayEarn = 0.0;
                        let newCycles = [];
                        let todayExpired = 0;
                        let reinvestLog = "无";

                        // 计算当天产币
                        for (let cycle of nftCycles) {
                            todayEarn += earnCycle[cycle];
                            // 更新周期
                            const nextCycle = cycle < 30 ? cycle + 1 : 1;
                            newCycles.push(nextCycle);
                        }

                        // 累加产币数据
                        todayEarn = parseFloat(todayEarn.toFixed(1));
                        totalEarned += todayEarn;
                        totalPifi += todayEarn;
                        totalPifi = parseFloat(totalPifi.toFixed(1));

                        // 核心逻辑：累计产币达到300的倍数时计算消失数量
                        const totalPossibleExpired = Math.floor(totalEarned / 300);
                        todayExpired = totalPossibleExpired - totalExpired;
                        
                        if (todayExpired > 0) {
                            todayExpired = Math.min(todayExpired, currentHold);
                            newCycles = newCycles.slice(0, newCycles.length - todayExpired);
                            currentHold -= todayExpired;
                            totalExpired += todayExpired;
                        }

                        // 复投逻辑（复投后所有NFT重置为周期1）
                        if (totalPifi >= 100 && (day - lastReinvestDay) >= 3) {
                            const reinvestNum = Math.floor(totalPifi / 100);
                            totalPifi -= reinvestNum * 100;
                            currentHold += reinvestNum;
                            
                            // 所有NFT重置为周期1
                            newCycles = Array(currentHold).fill(1);
                            reinvestLog = `复投${reinvestNum}张（全部重置为0.8）`;
                            lastReinvestDay = day;
                            totalReinvest += reinvestNum;
                        }

                        // 更新周期列表
                        nftCycles = newCycles;

                        // 保存当天数据
                        dailyData.push({
                            day,
                            currentHold,
                            todayExpired,
                            todayEarn,
                            totalEarned: parseFloat(totalEarned.toFixed(1)),
                            totalPifi,
                            reinvestLog
                        });
                    }

                    // 更新摘要信息
                    summaryInitial.textContent = `${initialNft}张`;
                    summaryDays.textContent = `${totalDays}天`;
                    summaryFinal.textContent = `${currentHold}张`;
                    summaryReinvest.textContent = `${totalReinvest}张`;
                    summaryExpired.textContent = `${totalExpired}张`;
                    summaryRemaining.textContent = `${totalPifi.toFixed(1)}`;

                    // 生成表格数据
                    allRows = dailyData.map(data => createTableRow(data));
                    
                    // 根据数据量决定是否默认显示精简版
                    if (allRows.length > 30) {
                        showLessBtn.click();
                    } else {
                        showingAllData = true;
                        showLessBtn.classList.add('hidden');
                        showMoreBtn.classList.add('hidden');
                        moreDataIndicator.classList.add('hidden');
                        renderTableRows(allRows);
                    }

                    // 显示结果区域
                    resultSection.classList.remove('hidden');
                    
                    // 滚动到结果区域
                    resultSection.scrollIntoView({ behavior: 'smooth' });
                } catch (error) {
                    console.error('计算出错:', error);
                    alert('计算过程中出现错误，请重试');
                } finally {
                    // 隐藏加载中
                    loadingModal.classList.add('hidden');
                }
            }, 500);
        }

        // 创建表格行
        function createTableRow(data) {
            const row = document.createElement('tr');
            row.className = data.reinvestLog !== "无" ? "bg-green-50" : "";
            
            row.innerHTML = `
                <td class="px-3 py-2">${data.day}</td>
                <td class="px-3 py-2">${data.currentHold}</td>
                <td class="px-3 py-2 ${data.todayExpired > 0 ? 'text-red-600 font-medium' : ''}">${data.todayExpired}</td>
                <td class="px-3 py-2">${data.todayEarn}</td>
                <td class="px-3 py-2">${data.totalEarned}</td>
                <td class="px-3 py-2">${data.totalPifi}</td>
                <td class="px-3 py-2 ${data.reinvestLog !== "无" ? 'text-green-700 font-medium' : ''}">${data.reinvestLog}</td>
            `;
            
            return row;
        }

        // 渲染表格行
        function renderTableRows(rows) {
            // 清空表格
            detailTable.innerHTML = '';
            
            // 添加行
            rows.forEach(row => {
                detailTable.appendChild(row);
            });
        }
    </script>
</body>
</html>
    
