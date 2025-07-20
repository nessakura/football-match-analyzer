<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Football Match Analyzer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #2e7d32 0%, #4caf50 50%, #66bb6a 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #1b5e20, #2e7d32, #388e3c);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '⚽';
            position: absolute;
            top: -10px;
            left: 20px;
            font-size: 3em;
            opacity: 0.3;
            animation: bounce 2s infinite;
        }

        .header::after {
            content: '⚽';
            position: absolute;
            top: -10px;
            right: 20px;
            font-size: 3em;
            opacity: 0.3;
            animation: bounce 2s infinite 1s;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .content {
            padding: 30px;
        }

        .api-key-section {
            background: linear-gradient(135deg, #fff3cd, #ffeaa7);
            border: 2px solid #f39c12;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
            box-shadow: 0 5px 15px rgba(243, 156, 18, 0.1);
        }

        .api-key-section h3 {
            color: #d68910;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .api-key-section p {
            color: #856404;
            margin-bottom: 15px;
            font-size: 0.95em;
            line-height: 1.5;
        }

        .match-setup {
            background: linear-gradient(135deg, #e3f2fd, #bbdefb);
            border: 2px solid #2196f3;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 5px 15px rgba(33, 150, 243, 0.1);
        }

        .match-setup h3 {
            color: #1565c0;
            margin-bottom: 20px;
            text-align: center;
            font-size: 1.4em;
        }

        .teams-input {
            display: flex;
            align-items: center;
            gap: 15px;
            justify-content: center;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }

        .team-input {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .team-input label {
            font-weight: 600;
            color: #1565c0;
            margin-bottom: 8px;
            font-size: 1.1em;
        }

        .team-input input {
            padding: 12px 20px;
            border: 2px solid #e0e0e0;
            border-radius: 25px;
            font-size: 16px;
            text-align: center;
            min-width: 200px;
            transition: all 0.3s ease;
            font-weight: 600;
        }

        .team-input input:focus {
            outline: none;
            border-color: #2196f3;
            box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
        }

        .vs-text {
            font-size: 2em;
            font-weight: bold;
            color: #1565c0;
            margin: 0 10px;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
            font-size: 1.1em;
        }

        input[type="password"] {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s ease;
            font-family: inherit;
        }

        input[type="password"]:focus {
            outline: none;
            border-color: #4caf50;
            box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.1);
        }

        .analyze-btn {
            background: linear-gradient(135deg, #2e7d32, #4caf50);
            color: white;
            border: none;
            padding: 18px 45px;
            font-size: 1.2em;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 8px 20px rgba(76, 175, 80, 0.3);
            display: block;
            margin: 0 auto;
            min-width: 280px;
        }

        .analyze-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 12px 30px rgba(76, 175, 80, 0.4);
            background: linear-gradient(135deg, #388e3c, #66bb6a);
        }

        .analyze-btn:active {
            transform: translateY(-1px);
        }

        .analyze-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 30px;
            color: #2e7d32;
        }

        .loading.show {
            display: block;
        }

        .spinner {
            width: 50px;
            height: 50px;
            border: 4px solid #e8f5e8;
            border-top: 4px solid #4caf50;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .results-section {
            margin-top: 30px;
            padding-top: 30px;
            border-top: 3px solid #e8f5e8;
            display: none;
        }

        .analysis-display {
            background: linear-gradient(135deg, #f1f8e9, #e8f5e8);
            border: 2px solid #4caf50;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 5px 15px rgba(76, 175, 80, 0.1);
        }

        .analysis-display h3 {
            color: #2e7d32;
            margin-bottom: 20px;
            font-size: 1.5em;
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .analysis-content {
            background: white;
            padding: 25px;
            border-radius: 12px;
            margin-bottom: 20px;
            border: 1px solid #c8e6c9;
            white-space: pre-wrap;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.8;
            font-size: 16px;
            max-height: 500px;
            overflow-y: auto;
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
        }

        .prediction-result {
            background: linear-gradient(135deg, #ffecb3, #fff8e1);
            border: 3px solid #ffa000;
            border-radius: 15px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 8px 25px rgba(255, 160, 0, 0.2);
        }

        .prediction-result h4 {
            color: #e65100;
            font-size: 1.4em;
            margin-bottom: 15px;
        }

        .score-prediction {
            font-size: 2.5em;
            font-weight: bold;
            color: #bf360c;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
            margin: 10px 0;
            padding: 15px;
            background: white;
            border-radius: 10px;
            display: inline-block;
            min-width: 150px;
        }

        .error {
            background: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 10px;
            margin: 10px 0;
            border-left: 4px solid #c62828;
            font-weight: 600;
            display: none; /* Hidden by default */
        }

        .error.show {
            display: block;
        }

        @media (max-width: 768px) {
            .container {
                margin: 10px;
                border-radius: 15px;
            }
            
            .content {
                padding: 20px;
            }
            
            .header h1 {
                font-size: 2em;
            }
            
            .teams-input {
                flex-direction: column;
                gap: 20px;
            }

            .vs-text {
                font-size: 1.5em;
                margin: 10px 0;
            }

            .team-input input {
                min-width: 250px;
            }

            .analyze-btn {
                min-width: auto;
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🏆 Football Match Analyzer</h1>
            <p>วิเคราะห์ทำนายผลฟุตบอลด้วย AI และข้อมูลสถิติ</p>
        </div>

        <div class="content">
            <div class="api-key-section">
                <h3>🔑 การตั้งค่า API</h3>
                <p>กรุณาใส่ Google AI Studio API Key (Gemini) สำหรับการวิเคราะห์ทำนายผลแมตช์ (ข้อมูลจะไม่ถูกเก็บไว้)</p>
                <div class="input-group">
                    <input type="password" id="apiKey" placeholder="ใส่ Gemini API Key ของคุณ" />
                </div>
            </div>

            <div class="error" id="generalError"></div>

            <div class="match-setup">
                <h3>⚽ เลือกทีมที่ต้องการวิเคราะห์</h3>
                <div class="teams-input">
                    <div class="team-input">
                        <label>🏠 ทีมเหย้า</label>
                        <input type="text" id="homeTeam" placeholder="ชื่อทีมเหย้า" />
                    </div>
                    <div class="vs-text">VS</div>
                    <div class="team-input">
                        <label>✈️ ทีมเยือน</label>
                        <input type="text" id="awayTeam" placeholder="ชื่อทีมเยือน" />
                    </div>
                </div>

                <button class="analyze-btn" onclick="analyzeMatch()">
                    🔍 วิเคราะห์และทำนายผล
                </button>
            </div>

            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>กำลังวิเคราะห์ข้อมูลทีมและทำนายผลแมตช์...</p>
            </div>

            <div class="results-section" id="results">
                <div class="analysis-display">
                    <h3>📊 การวิเคราะห์แมตช์</h3>
                    <div class="analysis-content" id="analysisContent"></div>
                </div>

                <div class="prediction-result">
                    <h4>🎯 ผลทำนาย</h4>
                    <div class="score-prediction" id="scorePrediction">0 - 0</div>
                    <p style="margin-top: 15px; font-size: 1.1em; color: #4CAF50;">ความมั่นใจ: <span id="confidenceLevel">ไม่ระบุ</span></p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Use a constant for the API URL for better maintainability
        const GEMINI_API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent?key=';
        let isAnalyzing = false; // Flag to prevent multiple concurrent analyses

        /**
         * Initiates the match analysis process.
         * Validates inputs, shows loading, calls API, and displays results or errors.
         */
        async function analyzeMatch() {
            const apiKey = document.getElementById('apiKey').value.trim();
            const homeTeam = document.getElementById('homeTeam').value.trim();
            const awayTeam = document.getElementById('awayTeam').value.trim();

            if (!apiKey) {
                showError('กรุณาใส่ API Key ของคุณเพื่อดำเนินการต่อ');
                return;
            }

            if (!homeTeam || !awayTeam) {
                showError('กรุณาใส่ชื่อทีมทั้งทีมเหย้าและทีมเยือน');
                return;
            }

            if (isAnalyzing) return; // Prevent multiple clicks while analysis is in progress
            isAnalyzing = true;

            showLoading(true);
            hideError(); // Clear previous errors
            hideResults(); // Hide previous results

            try {
                const analysisText = await generateAnalysis(apiKey, homeTeam, awayTeam);
                const result = parseAnalysisResult(analysisText);
                
                displayResults(result.analysis, result.score, result.confidence);
                showResults(); // Show the results section
                scrollToResults(); // Scroll to the results for better UX
                
            } catch (error) {
                console.error('Error during analysis:', error);
                let errorMessage = 'เกิดข้อผิดพลาดที่ไม่คาดคิดในการวิเคราะห์';
                if (error.message.includes('API Key')) {
                    errorMessage = 'API Key ไม่ถูกต้องหรือไม่ได้รับการอนุมัติ กรุณาตรวจสอบและลองอีกครั้ง';
                } else if (error.message.includes('Rate Limit')) {
                    errorMessage = 'ถูกจำกัดการใช้งาน API ชั่วคราว กรุณารอสักครู่แล้วลองใหม่';
                } else if (error.message.includes('model did not respond')) {
                    errorMessage = 'AI ไม่ตอบสนอง กรุณาลองอีกครั้งในภายหลัง';
                } else if (error.message) {
                    errorMessage = `ข้อผิดพลาด: ${error.message}`;
                }
                showError(errorMessage);
            } finally {
                showLoading(false);
                isAnalyzing = false;
            }
        }

        /**
         * Calls the Gemini API to get the match analysis.
         * @param {string} apiKey - The Google AI Studio API Key.
         * @param {string} homeTeam - The name of the home team.
         * @param {string} awayTeam - The name of the away team.
         * @returns {Promise<string>} The raw text response from the API.
         * @throws {Error} If the API response is not OK or unexpected.
         */
        async function generateAnalysis(apiKey, homeTeam, awayTeam) {
            const prompt = `คุณเป็นนักวิเคราะห์ฟุตบอลมืออาชีพ กรุณาวิเคราะห์แมตช์ระหว่าง "${homeTeam}" (ทีมเหย้า) vs "${awayTeam}" (ทีมเยือน)

กรุณาวิเคราะห์ตามหัวข้อเหล่านี้:

1. 📈 **ฟอร์มล่าสุด**: วิเคราะห์ผลงานล่าสุด 5 นัดของทั้งสองทีม
2. 🏠 **ข้อได้เปรียบเหย้า/เยือน**: ข้อดีข้อเสียการเล่นเหย้าและเยือน
3. ⚔️ **ประวัติการพบกัน**: สถิติการพบกันครั้งล่าสุด
4. ❤️ **ความแข็งแกร่งของทีม**: จุดแข็งจุดอ่อนของแต่ละทีม
5. 🚑 **สถานะนักเตะ**: การบาดเจ็บหรือใช้ไม่ได้ของนักเตะสำคัญ (ถ้ามี)
6. 🎯 **การทำนายผล**: วิเคราะห์โอกาสชนะของแต่ละทีม

สรุปด้วยการทำนายสกอร์ที่น่าจะเป็นไปได้มากที่สุด พร้อมระดับความมั่นใจ

รูปแบบการตอบ:
[การวิเคราะห์ทั้งหมด]

**ผลทำนาย: [เลขสกอร์] (เช่น 2-1)**
**ระดับความมั่นใจ: [%]%**`;

            const response = await fetch(`${GEMINI_API_URL}${apiKey}`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    contents: [{
                        parts: [{
                            text: prompt
                        }]
                    }]
                })
            });

            if (!response.ok) {
                const errorData = await response.json(); // Try to parse JSON for more detailed error
                const errorMessage = errorData.error ? errorData.error.message : response.statusText;
                throw new Error(`HTTP error! status: ${response.status} - ${errorMessage}`);
            }

            const data = await response.json();
            
            // Robust check for the response structure
            if (!data.candidates || !data.candidates[0] || !data.candidates[0].content || !data.candidates[0].content.parts || !data.candidates[0].content.parts[0]) {
                throw new Error('ไม่สามารถวิเคราะห์ได้ อาจเกิดจาก API Key ไม่ถูกต้อง หรือโมเดลไม่ตอบสนอง');
            }

            return data.candidates[0].content.parts[0].text;
        }

        /**
         * Parses the raw analysis text from the AI into structured data.
         * @param {string} analysisText - The raw text received from the Gemini API.
         * @returns {{analysis: string, score: string, confidence: string}} Parsed analysis, score, and confidence.
         */
        function parseAnalysisResult(analysisText) {
            let score = 'N/A';
            let confidence = 'ไม่ระบุ';

            // Regex to find "ผลทำนาย: X-Y"
            const scoreMatch = analysisText.match(/ผลทำนาย:\s*([0-9]+-[0-9]+)/);
            if (scoreMatch) {
                score = scoreMatch[1];
            } else {
                // Fallback: try to find any "X-Y" pattern
                const altScoreMatch = analysisText.match(/([0-9]+-[0-9]+)/);
                if (altScoreMatch) {
                    score = altScoreMatch[1];
                }
            }

            // Regex to find "ระดับความมั่นใจ: N%"
            const confidenceMatch = analysisText.match(/ระดับความมั่นใจ:\s*(\d+)%/);
            if (confidenceMatch) {
                confidence = confidenceMatch[1] + '%';
            }

            // Clean the analysis text by removing the score and confidence lines
            let cleanedAnalysis = analysisText
                .replace(/\*\*ผลทำนาย:.*?\*\*/g, '')
                .replace(/\*\*ระดับความมั่นใจ:.*?\*\*/g, '')
                .trim();

            return {
                analysis: cleanedAnalysis,
                score: score,
                confidence: confidence
            };
        }

        /**
         * Displays the parsed analysis results on the UI.
         * @param {string} analysisText - The main analysis content.
         * @param {string} score - The predicted score.
         * @param {string} confidence - The confidence level.
         */
        function displayResults(analysisText, score, confidence) {
            const analysisContent = document.getElementById('analysisContent');
            analysisContent.innerHTML = formatAnalysisText(analysisText);

            document.getElementById('scorePrediction').textContent = score;
            document.getElementById('confidenceLevel').textContent = confidence;
        }

        /**
         * Formats the raw analysis text with HTML for better readability.
         * @param {string} text - The raw analysis text.
         * @returns {string} HTML formatted text.
         */
        function formatAnalysisText(text) {
            // Replaces **bold** with <strong> and specific color
            let formattedText = text.replace(/\*\*(.*?)\*\*/g, '<strong style="color: #2e7d32;">$1</strong>');
            // Adds line breaks and styles for numbered list items and emojis
            formattedText = formattedText
                .replace(/(\d+\.\s)/g, '<br><strong style="color: #1976d2;">$1</strong>') // New line and blue for list numbers
                .replace(/📈|🏠|⚔️|👥|🚑|🎯/g, '<span style="font-size: 1.2em; margin-right: 5px;">$&</span>') // Larger emojis
                .replace(/\n/g, '<br>'); // Convert newlines to <br> tags

            return formattedText;
        }

        /**
         * Shows or hides the loading spinner.
         * @param {boolean} show - True to show, false to hide.
         */
        function showLoading(show) {
            const loading = document.getElementById('loading');
            loading.classList.toggle('show', show);
        }

        /**
         * Displays an error message to the user.
         * @param {string} message - The error message to display.
         */
        function showError(message) {
            const errorDiv = document.getElementById('generalError');
            errorDiv.textContent = message;
            errorDiv.classList.add('show');
        }

        /**
         * Hides any currently displayed error message.
         */
        function hideError() {
            const errorDiv = document.getElementById('generalError');
            errorDiv.classList.remove('show');
        }

        /**
         * Shows the results section.
         */
        function showResults() {
            document.getElementById('results').style.display = 'block';
        }

        /**
         * Hides the results section.
         */
        function hideResults() {
            document.getElementById('results').style.display = 'none';
        }

        /**
         * Scrolls the window to the results section.
         */
        function scrollToResults() {
            const resultsSection = document.getElementById('results');
            if (resultsSection) {
                resultsSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
        }

        // Event listeners for Enter key on input fields
        document.getElementById('apiKey').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                document.getElementById('homeTeam').focus();
            }
        });

        document.getElementById('homeTeam').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                document.getElementById('awayTeam').focus();
            }
        });

        document.getElementById('awayTeam').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                analyzeMatch();
            }
        });
    </script>
</body>
</html>
