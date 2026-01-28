<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hệ Thống Nhập Điểm THPT - Dạng Bảng</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            padding: 40px;
        }

        h1 {
            text-align: center;
            color: #667eea;
            margin-bottom: 10px;
            font-size: 2.2em;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .student-info {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .student-info h2 {
            color: white;
            margin-bottom: 20px;
            text-align: center;
        }

        .info-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        .form-group label {
            font-weight: 600;
            color: white;
            margin-bottom: 8px;
            font-size: 1em;
        }

        .form-group input {
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 15px;
            background: white;
            transition: all 0.3s;
        }

        .form-group input:focus {
            outline: none;
            box-shadow: 0 0 0 3px rgba(255,255,255,0.5);
            transform: translateY(-2px);
        }

        .error {
            border: 3px solid #ff4444 !important;
            animation: shake 0.5s;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }

        .error-message {
            background: #ff4444;
            color: white;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
            font-weight: 600;
            display: none;
            animation: slideDown 0.3s;
        }

        @keyframes slideDown {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .table-section {
            margin-bottom: 30px;
            overflow-x: auto;
        }

        .table-section h2 {
            color: #667eea;
            margin-bottom: 20px;
            text-align: center;
        }

        .grade-table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
            border-radius: 10px;
            overflow: hidden;
        }

        .grade-table thead {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .grade-table th {
            padding: 15px 10px;
            font-weight: 600;
            text-align: center;
            font-size: 0.95em;
        }

        .grade-table td {
            padding: 12px 8px;
            text-align: center;
            border-bottom: 1px solid #e9ecef;
        }

        .grade-table tbody tr:hover {
            background: #f8f9fa;
            transition: background 0.3s;
        }

        .grade-table tbody tr:nth-child(even) {
            background: #f8f9fa;
        }

        .grade-table tbody tr:nth-child(even):hover {
            background: #e9ecef;
        }

        .subject-name {
            font-weight: 600;
            color: #333;
            text-align: left !important;
            padding-left: 20px !important;
        }

        .grade-input {
            width: 60px;
            padding: 8px 5px;
            border: 2px solid #ddd;
            border-radius: 5px;
            text-align: center;
            font-size: 14px;
            transition: all 0.3s;
        }

        .grade-input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .average-cell {
            background: #667eea;
            color: white;
            font-weight: 700;
            font-size: 1.1em;
        }

        .btn-container {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 30px;
        }

        button {
            padding: 15px 50px;
            font-size: 18px;
            font-weight: 700;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn-calculate {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .btn-calculate:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.5);
        }

        .btn-reset {
            background: #e9ecef;
            color: #333;
        }

        .btn-reset:hover {
            background: #dee2e6;
            transform: translateY(-2px);
        }

        .result-section {
            display: none;
            background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
            padding: 40px;
            border-radius: 20px;
            color: white;
            margin-top: 40px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            animation: fadeIn 0.5s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        .result-section.show {
            display: block;
        }

        .result-header {
            text-align: center;
            font-size: 2.5em;
            margin-bottom: 25px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .result-info {
            background: rgba(255,255,255,0.25);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            backdrop-filter: blur(10px);
        }

        .result-info p {
            margin: 12px 0;
            font-size: 1.2em;
            font-weight: 600;
        }

        .result-table {
            width: 100%;
            background: white;
            color: #333;
            border-radius: 15px;
            overflow: hidden;
            margin-bottom: 25px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.15);
        }

        .result-table th,
        .result-table td {
            padding: 15px;
            text-align: center;
        }

        .result-table th {
            background: #667eea;
            color: white;
            font-weight: 700;
            font-size: 1.05em;
        }

        .result-table tr:nth-child(even) {
            background: #f8f9fa;
        }

        .result-table td:first-child {
            text-align: left;
            font-weight: 600;
            padding-left: 25px;
        }

        .result-table td:last-child {
            font-weight: 700;
            color: #667eea;
            font-size: 1.1em;
        }

        .final-result {
            text-align: center;
            padding: 30px;
            background: rgba(255,255,255,0.3);
            border-radius: 20px;
            margin-top: 25px;
            backdrop-filter: blur(10px);
        }

        .final-result h3 {
            font-size: 1.8em;
            margin-bottom: 15px;
        }

        .final-result h2 {
            font-size: 3.5em;
            text-shadow: 3px 3px 6px rgba(0,0,0,0.3);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }

            h1 {
                font-size: 1.6em;
            }

            .grade-table {
                font-size: 0.85em;
            }

            .grade-input {
                width: 50px;
                padding: 6px 3px;
            }

            .btn-container {
                flex-direction: column;
            }

            button {
                width: 100%;
            }

            .result-header {
                font-size: 1.8em;
            }

            .final-result h2 {
                font-size: 2.5em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎓 HỆ THỐNG NHẬP ĐIỂM HỌC SINH THPT</h1>
        <p class="subtitle">Quản lý và tính toán điểm trung bình tự động</p>

        <div id="errorMessage" class="error-message"></div>

        <div class="student-info">
            <h2>📋 THÔNG TIN HỌC SINH</h2>
            <div class="info-grid">
                <div class="form-group">
                    <label>👤 Họ và Tên</label>
                    <input type="text" id="studentName" placeholder="Nhập họ và tên đầy đủ">
                </div>
                <div class="form-group">
                    <label>🏫 Lớp</label>
                    <input type="text" id="studentClass" placeholder="Ví dụ: 10A1, 11B3">
                </div>
                <div class="form-group">
                    <label>🆔 Số Báo Danh</label>
                    <input type="text" id="studentId" placeholder="Nhập số báo danh">
                </div>
            </div>
        </div>

        <div class="table-section">
            <h2>📊 BẢNG NHẬP ĐIỂM 10 MÔN HỌC</h2>
            <table class="grade-table">
                <thead>
                    <tr>
                        <th style="text-align: left; padding-left: 20px;">Môn Học</th>
                        <th>TX1</th>
                        <th>TX2</th>
                        <th>TX3</th>
                        <th>TX4</th>
                        <th>Giữa Kỳ</th>
                        <th>Cuối Kỳ</th>
                        <th>TB Môn</th>
                    </tr>
                </thead>
                <tbody id="gradeTableBody"></tbody>
            </table>
        </div>

        <div class="btn-container">
            <button class="btn-calculate" onclick="calculateResults()">🔢 Tính Kết Quả</button>
            <button class="btn-reset" onclick="resetForm()">🔄 Làm Mới</button>
        </div>

        <div id="resultSection" class="result-section"></div>
    </div>

    <script>
        const subjects = [
            'Toán', 'Vật lí', 'Hóa học', 'Anh văn', 'Ngữ văn',
            'Tin học', 'Sinh học', 'Lịch sử', 'Địa lý', 'Giáo dục quốc phòng'
        ];

        function initializeTable() {
            const tbody = document.getElementById('gradeTableBody');
            subjects.forEach((subject, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="subject-name">${index + 1}. ${subject}</td>
                    <td><input type="number" class="grade-input" step="0.1" min="0" max="10" 
                        id="tx1_${index}" oninput="calculateRowAverage(${index})"></td>
                    <td><input type="number" class="grade-input" step="0.1" min="0" max="10" 
                        id="tx2_${index}" oninput="calculateRowAverage(${index})"></td>
                    <td><input type="number" class="grade-input" step="0.1" min="0" max="10" 
                        id="tx3_${index}" oninput="calculateRowAverage(${index})"></td>
                    <td><input type="number" class="grade-input" step="0.1" min="0" max="10" 
                        id="tx4_${index}" oninput="calculateRowAverage(${index})"></td>
                    <td><input type="number" class="grade-input" step="0.1" min="0" max="10" 
                        id="mid_${index}" oninput="calculateRowAverage(${index})"></td>
                    <td><input type="number" class="grade-input" step="0.1" min="0" max="10" 
                        id="final_${index}" oninput="calculateRowAverage(${index})"></td>
                    <td class="average-cell" id="avg_${index}">--</td>
                `;
                tbody.appendChild(row);
            });
        }

        function calculateRowAverage(index) {
            const tx1 = parseFloat(document.getElementById(`tx1_${index}`).value) || 0;
            const tx2 = parseFloat(document.getElementById(`tx2_${index}`).value) || 0;
            const tx3 = parseFloat(document.getElementById(`tx3_${index}`).value) || 0;
            const tx4 = parseFloat(document.getElementById(`tx4_${index}`).value) || 0;
            const mid = parseFloat(document.getElementById(`mid_${index}`).value) || 0;
            const final = parseFloat(document.getElementById(`final_${index}`).value) || 0;

            const average = (tx1 + tx2 + tx3 + tx4 + (mid * 2) + (final * 3)) / 9;
            document.getElementById(`avg_${index}`).textContent = average > 0 ? average.toFixed(2) : '--';
        }

        function validateStudentInfo() {
            const name = document.getElementById('studentName').value.trim();
            const studentClass = document.getElementById('studentClass').value.trim();
            const studentId = document.getElementById('studentId').value.trim();

            document.getElementById('studentName').classList.remove('error');
            document.getElementById('studentClass').classList.remove('error');
            document.getElementById('studentId').classList.remove('error');

            if (!name || !studentClass || !studentId) {
                const errorMsg = document.getElementById('errorMessage');
                errorMsg.textContent = '⚠️ VUI LÒNG NHẬP ĐẦY ĐỦ: HỌ TÊN, LỚP VÀ SỐ BÁO DANH!';
                errorMsg.style.display = 'block';
                
                if (!name) document.getElementById('studentName').classList.add('error');
                if (!studentClass) document.getElementById('studentClass').classList.add('error');
                if (!studentId) document.getElementById('studentId').classList.add('error');
                
                setTimeout(() => {
                    errorMsg.style.display = 'none';
                }, 3000);
                
                return false;
            }

            document.getElementById('errorMessage').style.display = 'none';
            return true;
        }

        function getAverages() {
            const averages = [];
            subjects.forEach((subject, index) => {
                const avgText = document.getElementById(`avg_${index}`).textContent;
                const score = avgText === '--' ? 0 : parseFloat(avgText);
                averages.push({
                    subject: subject,
                    score: score
                });
            });
            return averages;
        }

        function determineAcademicLevel(averages) {
            const sorted = [...averages].sort((a, b) => b.score - a.score);
            const top8 = sorted.slice(0, 8);
            const remaining2 = sorted.slice(8, 10);

            if (top8.every(s => s.score >= 9.0) && remaining2.every(s => s.score >= 6.5)) {
                return '🏆 XUẤT SẮC';
            } else if (top8.every(s => s.score >= 8.0) && remaining2.every(s => s.score >= 6.5)) {
                return '🥇 GIỎI';
            } else if (top8.every(s => s.score >= 6.5) && remaining2.every(s => s.score >= 5.0)) {
                return '🥈 KHÁ';
            } else if (top8.every(s => s.score >= 5.0) && remaining2.every(s => s.score >= 3.5)) {
                return '🥉 ĐẠT';
            } else {
                return '❌ CHƯA ĐẠT';
            }
        }

        function calculateResults() {
            if (!validateStudentInfo()) {
                return;
            }

            const name = document.getElementById('studentName').value;
            const studentClass = document.getElementById('studentClass').value;
            const studentId = document.getElementById('studentId').value;
            const averages = getAverages();
            const academicLevel = determineAcademicLevel(averages);

            let tableRows = '';
            averages.forEach((item, idx) => {
                tableRows += `
                    <tr>
                        <td>${idx + 1}. ${item.subject}</td>
                        <td><strong>${item.score.toFixed(2)}</strong></td>
                    </tr>
                `;
            });

            const resultHTML = `
                <div class="result-header">✨ KẾT QUẢ HỌC TẬP ✨</div>
                <div class="result-info">
                    <p>👤 <strong>Họ và Tên:</strong> ${name}</p>
                    <p>🏫 <strong>Lớp:</strong> ${studentClass}</p>
                    <p>🆔 <strong>Số Báo Danh:</strong> ${studentId}</p>
                </div>
                <table class="result-table">
                    <thead>
                        <tr>
                            <th style="text-align: left; padding-left: 25px;">Môn Học</th>
                            <th>Điểm Trung Bình</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${tableRows}
                    </tbody>
                </table>
                <div class="final-result">
                    <h3>🎓 HỌC LỰC</h3>
                    <h2>${academicLevel}</h2>
                </div>
            `;

            const resultSection = document.getElementById('resultSection');
            resultSection.innerHTML = resultHTML;
            resultSection.classList.add('show');
            resultSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        function resetForm() {
            document.getElementById('studentName').value = '';
            document.getElementById('studentClass').value = '';
            document.getElementById('studentId').value = '';
            
            subjects.forEach((_, index) => {
                document.getElementById(`tx1_${index}`).value = '';
                document.getElementById(`tx2_${index}`).value = '';
                document.getElementById(`tx3_${index}`).value = '';
                document.getElementById(`tx4_${index}`).value = '';
                document.getElementById(`mid_${index}`).value = '';
                document.getElementById(`final_${index}`).value = '';
                document.getElementById(`avg_${index}`).textContent = '--';
            });

            document.getElementById('resultSection').classList.remove('show');
            document.getElementById('errorMessage').style.display = 'none';
            
            document.getElementById('studentName').classList.remove('error');
            document.getElementById('studentClass').classList.remove('error');
            document.getElementById('studentId').classList.remove('error');

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        initializeTable();
    </script>
</body>
</html>
