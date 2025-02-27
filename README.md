<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>數字易經選號系統</title>
    <style>
        body {
            font-family: 'Microsoft JhengHei', sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        textarea {
            width: 100%;
            height: 100px;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #45a049;
        }
        .results {
            margin-top: 20px;
        }
        .number-card {
            background-color: #f9f9f9;
            border: 1px solid #eee;
            border-radius: 4px;
            padding: 10px;
            margin-bottom: 10px;
        }
        .number {
            font-size: 24px;
            font-weight: bold;
            text-align: center;
            margin-bottom: 10px;
        }
        .details {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }
        .hexagram {
            flex: 1;
            min-width: 70px;
            text-align: center;
            padding: 5px;
            border-radius: 4px;
        }
        .auspicious {
            background-color: #d4edda;
            color: #155724;
        }
        .inauspicious {
            background-color: #f8d7da;
            color: #721c24;
        }
        .special {
            background-color: #d1ecf1;
            color: #0c5460;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>數字易經手機選號系統</h1>
        
        <div class="form-group">
            <label for="numbers">請輸入可選擇的手機號碼（每行一個號碼）：</label>
            <textarea id="numbers" placeholder="例如：&#10;0912345678&#10;0987654321&#10;0922334455"></textarea>
        </div>
        
        <div class="form-group">
            <button id="analyze">分析號碼</button>
        </div>
        
        <div class="results" id="results">
            <!-- 結果將在這裡顯示 -->
        </div>
    </div>

    <script>
        document.getElementById('analyze').addEventListener('click', function() {
            const numbersInput = document.getElementById('numbers').value;
            const numbers = numbersInput.split('\n').filter(n => n.trim() !== '');
            
            const results = analyzeNumbers(numbers);
            displayResults(results);
        });
        
        function analyzeNumbers(numbers) {
            const auspiciousResults = [];
            
            // 吉卦對照表
            const auspiciousPairs = {
                // 生意
                '13': '生意', '31': '生意', '68': '生意', '86': '生意', 
                '49': '生意', '94': '生意', '27': '生意', '72': '生意',
                // 開創
                '14': '開創', '41': '開創', '67': '開創', '76': '開創', 
                '39': '開創', '93': '開創', '28': '開創', '82': '開創',
                // 累積
                '19': '累積', '91': '累積', '78': '累積', '87': '累積', 
                '34': '累積', '43': '累積', '26': '累積', '62': '累積',
                // 專精
                '11': '專精', '22': '專精', '33': '專精', '44': '專精', 
                '66': '專精', '77': '專精', '88': '專精', '99': '專精',
                '00': '專精'
            };
            
            // 兇卦對照表
            const inauspiciousPairs = {
                // 整合
                '18': '整合', '81': '整合', '79': '整合', '97': '整合', 
                '36': '整合', '63': '整合', '24': '整合', '42': '整合',
                // 名聲
                '16': '名聲', '61': '名聲', '47': '名聲', '74': '名聲', 
                '38': '名聲', '83': '名聲', '29': '名聲', '92': '名聲',
                // 掌控
                '17': '掌控', '71': '掌控', '89': '掌控', '98': '掌控', 
                '46': '掌控', '64': '掌控', '23': '掌控', '32': '掌控',
                // 顧問
                '12': '顧問', '21': '顧問', '69': '顧問', '96': '顧問', 
                '48': '顧問', '84': '顧問', '37': '顧問', '73': '顧問'
            };
            
            for (const number of numbers) {
                // 確保號碼格式正確
                if (number.length !== 10 || !number.startsWith('09')) {
                    continue;
                }
                
                // 去掉前綴09
                const digits = number.slice(2);
                
                const analysis = {
                    number: number,
                    pairs: [],
                    allAuspicious: true
                };
                
                // 分析每對數字
                for (let i = 0; i < digits.length - 1; i++) {
                    // 處理特殊規則
                    let firstDigit = digits[i];
                    let secondDigit = digits[i+1];
                    
                    // 規則3: 遇5跳過
                    if (firstDigit === '5') {
                        continue;
                    }
                    
                    // 規則4: 尾數為5視為0
                    if (i === digits.length - 2 && secondDigit === '5') {
                        secondDigit = '0';
                    }
                    
                    // 規則2: 0視為前後數字，形成專精卦
                    if (firstDigit === '0' || secondDigit === '0') {
                        analysis.pairs.push({
                            position: `${i}-${i+1}`,
                            pair: `${firstDigit}${secondDigit}`,
                            hexagram: '專精',
                            type: 'special'
                        });
                        continue;
                    }
                    
                    // 判斷卦象
                    const pair = `${firstDigit}${secondDigit}`;
                    
                    if (auspiciousPairs[pair]) {
                        analysis.pairs.push({
                            position: `${i}-${i+1}`,
                            pair: pair,
                            hexagram: auspiciousPairs[pair],
                            type: 'auspicious'
                        });
                    } else if (inauspiciousPairs[pair]) {
                        analysis.pairs.push({
                            position: `${i}-${i+1}`,
                            pair: pair,
                            hexagram: inauspiciousPairs[pair],
                            type: 'inauspicious'
                        });
                        analysis.allAuspicious = false;
                    }
                }
                
                if (analysis.allAuspicious) {
                    auspiciousResults.push(analysis);
                }
            }
            
            return auspiciousResults;
        }
        
        function displayResults(results) {
            const resultsContainer = document.getElementById('results');
            resultsContainer.innerHTML = '';
            
            if (results.length === 0) {
                resultsContainer.innerHTML = '<p>沒有找到全吉卦的號碼。</p>';
                return;
            }
            
            resultsContainer.innerHTML = `<h2>全吉卦號碼（共找到 ${results.length} 組）</h2>`;
            
            for (const result of results) {
                const numberCard = document.createElement('div');
                numberCard.className = 'number-card';
                
                const numberDiv = document.createElement('div');
                numberDiv.className = 'number';
                numberDiv.textContent = result.number;
                
                const detailsDiv = document.createElement('div');
                detailsDiv.className = 'details';
                
                for (const pair of result.pairs) {
                    const hexagramDiv = document.createElement('div');
                    hexagramDiv.className = `hexagram ${pair.type}`;
                    hexagramDiv.textContent = `${pair.pair}: ${pair.hexagram}`;
                    detailsDiv.appendChild(hexagramDiv);
                }
                
                numberCard.appendChild(numberDiv);
                numberCard.appendChild(detailsDiv);
                resultsContainer.appendChild(numberCard);
            }
        }
    </script>
</body>
</html>
