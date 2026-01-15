<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>고등학교 2학년 공학일반 성적 분석 대시보드</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', 'Malgun Gothic', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            margin-bottom: 30px;
            text-align: center;
        }

        h1 {
            color: #667eea;
            font-size: 2.5em;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .subtitle {
            color: #666;
            font-size: 1.1em;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15);
        }

        .stat-label {
            color: #888;
            font-size: 0.9em;
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .stat-value {
            color: #667eea;
            font-size: 2.2em;
            font-weight: 700;
        }

        .charts-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
            gap: 30px;
            margin-bottom: 30px;
        }

        .chart-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }

        .chart-title {
            color: #333;
            font-size: 1.5em;
            margin-bottom: 20px;
            font-weight: 600;
        }

        .table-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            margin-bottom: 30px;
        }

        .search-box {
            width: 100%;
            padding: 15px;
            border: 2px solid #667eea;
            border-radius: 10px;
            font-size: 1em;
            margin-bottom: 20px;
            transition: all 0.3s ease;
        }

        .search-box:focus {
            outline: none;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px;
            text-align: left;
            font-weight: 600;
            cursor: pointer;
            user-select: none;
            position: sticky;
            top: 0;
        }

        th:hover {
            background: linear-gradient(135deg, #5568d3 0%, #65408b 100%);
        }

        td {
            padding: 12px 15px;
            border-bottom: 1px solid #eee;
        }

        tr:hover {
            background: #f8f9ff;
        }

        .rank-badge {
            display: inline-block;
            padding: 5px 12px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 0.9em;
        }

        .rank-1 {
            background: linear-gradient(135deg, #FFD700, #FFA500);
            color: white;
        }

        .rank-2 {
            background: linear-gradient(135deg, #C0C0C0, #808080);
            color: white;
        }

        .rank-3 {
            background: linear-gradient(135deg, #CD7F32, #8B4513);
            color: white;
        }

        .grade-badge {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 12px;
            font-weight: 600;
            font-size: 0.85em;
        }

        .grade-1,
        .grade-2 {
            background: #4CAF50;
            color: white;
        }

        .grade-3,
        .grade-4 {
            background: #2196F3;
            color: white;
        }

        .grade-5,
        .grade-6 {
            background: #FF9800;
            color: white;
        }

        .grade-7,
        .grade-8,
        .grade-9 {
            background: #f44336;
            color: white;
        }

        @media (max-width: 768px) {
            .charts-grid {
                grid-template-columns: 1fr;
            }

            h1 {
                font-size: 1.8em;
            }

            .stat-value {
                font-size: 1.8em;
            }
        }
    </style>
</head>

<body>
    <div class="container">
        <header>
            <h1>📊 고등학교 2학년 공학일반 성적 분석</h1>
            <p class="subtitle">Interactive Grade Dashboard</p>
        </header>

        <div class="stats-grid">
            <div class="stat-card">
                <div class="stat-label">총 학생 수</div>
                <div class="stat-value">120</div>
            </div>
            <div class="stat-card">
                <div class="stat-label">평균 총점</div>
                <div class="stat-value">79.0</div>
            </div>
            <div class="stat-card">
                <div class="stat-label">기말 평균</div>
                <div class="stat-value">79.7</div>
            </div>
            <div class="stat-card">
                <div class="stat-label">수행평가 평균</div>
                <div class="stat-value">78.0</div>
            </div>
        </div>

        <div class="charts-grid">
            <div class="chart-card">
                <h2 class="chart-title">📈 반별 평균 점수</h2>
                <canvas id="classChart"></canvas>
            </div>

            <div class="chart-card">
                <h2 class="chart-title">🎯 등급 분포</h2>
                <canvas id="gradeChart"></canvas>
            </div>

            <div class="chart-card">
                <h2 class="chart-title">📊 과목별 평균 점수</h2>
                <canvas id="subjectChart"></canvas>
            </div>

            <div class="chart-card">
                <h2 class="chart-title">🏆 반별 성적 비교 (레이더)</h2>
                <canvas id="radarChart"></canvas>
            </div>
        </div>

        <div class="table-card">
            <h2 class="chart-title">🏅 상위 10명</h2>
            <table id="topTable">
                <thead>
                    <tr>
                        <th>순위</th>
                        <th>학번</th>
                        <th>반</th>
                        <th>번호</th>
                        <th>이름</th>
                        <th>기말</th>
                        <th>수행1</th>
                        <th>수행2</th>
                        <th>수행3</th>
                        <th>총점</th>
                    </tr>
                </thead>
                <tbody id="topTableBody"></tbody>
            </table>
        </div>

        <div class="table-card">
            <h2 class="chart-title">🔍 전체 학생 검색</h2>
            <input type="text" class="search-box" id="searchBox" placeholder="학생 이름 또는 학번으로 검색...">
            <table id="allTable">
                <thead>
                    <tr>
                        <th onclick="sortTable(0)">학번 ▼</th>
                        <th onclick="sortTable(1)">반 ▼</th>
                        <th onclick="sortTable(2)">번호 ▼</th>
                        <th onclick="sortTable(3)">이름 ▼</th>
                        <th onclick="sortTable(4)">기말 ▼</th>
                        <th onclick="sortTable(5)">수행1 ▼</th>
                        <th onclick="sortTable(6)">수행2 ▼</th>
                        <th onclick="sortTable(7)">수행3 ▼</th>
                        <th onclick="sortTable(8)">총점 ▼</th>
                        <th onclick="sortTable(9)">등급 ▼</th>
                    </tr>
                </thead>
                <tbody id="allTableBody"></tbody>
            </table>
        </div>
    </div>

    <script>
        // 데이터 임베딩
        const statsData = { "total_students": 120, "avg_total": 79.04933333333334, "avg_final": 79.725, "avg_perf1": 77.44166666666666, "avg_perf2": 77.63333333333334, "avg_perf3": 79.03333333333333, "class_stats": [{ "class": 1, "count": 30, "avg_final": 78.6, "avg_perf1": 77.23333333333333, "avg_perf2": 79.8, "avg_perf3": 79.03333333333333, "avg_total": 78.63533333333334, "max_total": 91.0, "min_total": 65.93 }, { "class": 2, "count": 30, "avg_final": 79.8, "avg_perf1": 76.93333333333334, "avg_perf2": 75.93333333333334, "avg_perf3": 77.66666666666667, "avg_total": 78.61733333333333, "max_total": 90.2, "min_total": 61.4 }, { "class": 3, "count": 30, "avg_final": 82.83333333333333, "avg_perf1": 78.3, "avg_perf2": 77.33333333333333, "avg_perf3": 78.46666666666667, "avg_total": 80.91433333333333, "max_total": 95.87, "min_total": 66.27 }, { "class": 4, "count": 30, "avg_final": 77.66666666666667, "avg_perf1": 77.3, "avg_perf2": 77.46666666666667, "avg_perf3": 80.96666666666667, "avg_total": 78.03033333333333, "max_total": 92.93, "min_total": 58.93 }], "grade_distribution": [4, 9, 14, 21, 24, 20, 14, 9, 5], "top_students": [{ "rank": 1, "student_id": "20260320", "class": 3, "number": 20, "name": "박현준", "final": 98, "perf1": 94, "perf2": 98, "perf3": 86, "total": 95.87 }, { "rank": 2, "student_id": "20260324", "class": 3, "number": 24, "name": "최수현", "final": 99, "perf1": 97, "perf2": 91, "perf3": 77, "total": 94.73 }, { "rank": 3, "student_id": "20260328", "class": 3, "number": 28, "name": "황나은", "final": 97, "perf1": 84, "perf2": 94, "perf3": 88, "total": 93.67 }, { "rank": 4, "student_id": "20260405", "class": 4, "number": 5, "name": "박민재", "final": 98, "perf1": 88, "perf2": 79, "perf3": 89, "total": 92.93 }, { "rank": 5, "student_id": "20260421", "class": 4, "number": 21, "name": "이현우", "final": 99, "perf1": 78, "perf2": 87, "perf3": 81, "total": 92.2 }, { "rank": 6, "student_id": "20260126", "class": 1, "number": 26, "name": "신지우", "final": 99, "perf1": 88, "perf2": 82, "perf3": 67, "total": 91.0 }, { "rank": 7, "student_id": "20260205", "class": 2, "number": 5, "name": "정지훈", "final": 97, "perf1": 77, "perf2": 88, "perf3": 75, "total": 90.2 }, { "rank": 8, "student_id": "20260224", "class": 2, "number": 24, "name": "최도윤", "final": 96, "perf1": 65, "perf2": 81, "perf3": 98, "total": 90.13 }, { "rank": 9, "student_id": "20260423", "class": 4, "number": 23, "name": "오민준", "final": 96, "perf1": 94, "perf2": 74, "perf3": 75, "total": 90.0 }, { "rank": 10, "student_id": "20260209", "class": 2, "number": 9, "name": "이승우", "final": 97, "perf1": 72, "perf2": 73, "perf3": 93, "total": 89.93 }], "all_students": [{ "student_id": "20260320", "class": 3, "number": 20, "name": "박현준", "final": 98, "perf1": 94, "perf2": 98, "perf3": 86, "total": 95.87, "grade": 1 }, { "student_id": "20260324", "class": 3, "number": 24, "name": "최수현", "final": 99, "perf1": 97, "perf2": 91, "perf3": 77, "total": 94.73, "grade": 1 }, { "student_id": "20260328", "class": 3, "number": 28, "name": "황나은", "final": 97, "perf1": 84, "perf2": 94, "perf3": 88, "total": 93.67, "grade": 1 }, { "student_id": "20260405", "class": 4, "number": 5, "name": "박민재", "final": 98, "perf1": 88, "perf2": 79, "perf3": 89, "total": 92.93, "grade": 1 }, { "student_id": "20260421", "class": 4, "number": 21, "name": "이현우", "final": 99, "perf1": 78, "perf2": 87, "perf3": 81, "total": 92.2, "grade": 2 }, { "student_id": "20260126", "class": 1, "number": 26, "name": "신지우", "final": 99, "perf1": 88, "perf2": 82, "perf3": 67, "total": 91.0, "grade": 2 }, { "student_id": "20260205", "class": 2, "number": 5, "name": "정지훈", "final": 97, "perf1": 77, "perf2": 88, "perf3": 75, "total": 90.2, "grade": 2 }, { "student_id": "20260224", "class": 2, "number": 24, "name": "최도윤", "final": 96, "perf1": 65, "perf2": 81, "perf3": 98, "total": 90.13, "grade": 2 }, { "student_id": "20260423", "class": 4, "number": 23, "name": "오민준", "final": 96, "perf1": 94, "perf2": 74, "perf3": 75, "total": 90.0, "grade": 2 }, { "student_id": "20260209", "class": 2, "number": 9, "name": "이승우", "final": 97, "perf1": 72, "perf2": 73, "perf3": 93, "total": 89.93, "grade": 2 }, { "student_id": "20260406", "class": 4, "number": 6, "name": "정채은", "final": 94, "perf1": 90, "perf2": 72, "perf3": 86, "total": 89.47, "grade": 2 }, { "student_id": "20260207", "class": 2, "number": 7, "name": "정하윤", "final": 95, "perf1": 80, "perf2": 69, "perf3": 94, "total": 89.4, "grade": 2 }, { "student_id": "20260305", "class": 3, "number": 5, "name": "윤지민", "final": 91, "perf1": 84, "perf2": 77, "perf3": 100, "total": 89.4, "grade": 2 }, { "student_id": "20260302", "class": 3, "number": 2, "name": "서하준", "final": 95, "perf1": 91, "perf2": 75, "perf3": 69, "total": 88.33, "grade": 3 }, { "student_id": "20260119", "class": 1, "number": 19, "name": "송승현", "final": 96, "perf1": 89, "perf2": 72, "perf3": 69, "total": 88.27, "grade": 3 }, { "student_id": "20260313", "class": 3, "number": 13, "name": "조하준", "final": 92, "perf1": 98, "perf2": 77, "perf3": 71, "total": 88.0, "grade": 3 }, { "student_id": "20260109", "class": 1, "number": 9, "name": "장승우", "final": 91, "perf1": 87, "perf2": 73, "perf3": 90, "total": 87.93, "grade": 3 }, { "student_id": "20260129", "class": 1, "number": 29, "name": "최현준", "final": 86, "perf1": 99, "perf2": 93, "perf3": 80, "total": 87.87, "grade": 3 }, { "student_id": "20260127", "class": 1, "number": 27, "name": "송소율", "final": 92, "perf1": 86, "perf2": 79, "perf3": 79, "total": 87.73, "grade": 3 }, { "student_id": "20260309", "class": 3, "number": 9, "name": "한지우", "final": 93, "perf1": 94, "perf2": 65, "perf3": 78, "total": 87.4, "grade": 3 }, { "student_id": "20260101", "class": 1, "number": 1, "name": "박지후", "final": 83, "perf1": 85, "perf2": 95, "perf3": 100, "total": 87.13, "grade": 3 }, { "student_id": "20260308", "class": 3, "number": 8, "name": "임하은", "final": 100, "perf1": 64, "perf2": 61, "perf3": 77, "total": 86.93, "grade": 3 }, { "student_id": "20260311", "class": 3, "number": 11, "name": "신채은", "final": 95, "perf1": 84, "perf2": 71, "perf3": 69, "total": 86.87, "grade": 3 }, { "student_id": "20260102", "class": 1, "number": 2, "name": "박민재", "final": 95, "perf1": 63, "perf2": 71, "perf3": 82, "total": 85.8, "grade": 3 }, { "student_id": "20260407", "class": 4, "number": 7, "name": "윤승현", "final": 85, "perf1": 88, "perf2": 82, "perf3": 90, "total": 85.67, "grade": 3 }, { "student_id": "20260427", "class": 4, "number": 27, "name": "윤지민", "final": 91, "perf1": 76, "perf2": 82, "perf3": 73, "total": 85.4, "grade": 3 }, { "student_id": "20260229", "class": 2, "number": 29, "name": "강하준", "final": 79, "perf1": 96, "perf2": 89, "perf3": 97, "total": 85.0, "grade": 3 }, { "student_id": "20260326", "class": 3, "number": 26, "name": "서지호", "final": 90, "perf1": 78, "perf2": 67, "perf3": 86, "total": 84.8, "grade": 4 }, { "student_id": "20260419", "class": 4, "number": 19, "name": "조승현", "final": 84, "perf1": 94, "perf2": 86, "perf3": 78, "total": 84.8, "grade": 4 }, { "student_id": "20260329", "class": 3, "number": 29, "name": "강도윤", "final": 90, "perf1": 64, "perf2": 72, "perf3": 92, "total": 84.4, "grade": 4 }, { "student_id": "20260118", "class": 1, "number": 18, "name": "박채원", "final": 87, "perf1": 76, "perf2": 92, "perf3": 72, "total": 84.2, "grade": 4 }, { "student_id": "20260204", "class": 2, "number": 4, "name": "오수아", "final": 81, "perf1": 89, "perf2": 92, "perf3": 86, "total": 84.2, "grade": 4 }, { "student_id": "20260128", "class": 1, "number": 28, "name": "조예진", "final": 91, "perf1": 70, "perf2": 61, "perf3": 89, "total": 83.93, "grade": 4 }, { "student_id": "20260125", "class": 1, "number": 25, "name": "조현우", "final": 86, "perf1": 74, "perf2": 80, "perf3": 84, "total": 83.33, "grade": 4 }, { "student_id": "20260415", "class": 4, "number": 15, "name": "서민재", "final": 92, "perf1": 60, "perf2": 75, "perf3": 76, "total": 83.33, "grade": 4 }, { "student_id": "20260114", "class": 1, "number": 14, "name": "임서윤", "final": 85, "perf1": 63, "perf2": 77, "perf3": 100, "total": 83.0, "grade": 4 }, { "student_id": "20260410", "class": 4, "number": 10, "name": "강지후", "final": 80, "perf1": 88, "perf2": 83, "perf3": 90, "total": 82.8, "grade": 4 }, { "student_id": "20260220", "class": 2, "number": 20, "name": "한민서", "final": 89, "perf1": 78, "perf2": 70, "perf3": 72, "total": 82.73, "grade": 4 }, { "student_id": "20260228", "class": 2, "number": 28, "name": "조승현", "final": 90, "perf1": 60, "perf2": 80, "perf3": 75, "total": 82.67, "grade": 4 }, { "student_id": "20260216", "class": 2, "number": 16, "name": "김지훈", "final": 85, "perf1": 87, "perf2": 87, "perf3": 63, "total": 82.6, "grade": 4 }, { "student_id": "20260221", "class": 2, "number": 21, "name": "서예준", "final": 89, "perf1": 69, "perf2": 67, "perf3": 79, "total": 82.07, "grade": 4 }, { "student_id": "20260110", "class": 1, "number": 10, "name": "한민재", "final": 92, "perf1": 68, "perf2": 60, "perf3": 72, "total": 81.87, "grade": 4 }, { "student_id": "20260212", "class": 2, "number": 12, "name": "윤승현", "final": 83, "perf1": 89, "perf2": 77, "perf3": 73, "total": 81.67, "grade": 4 }, { "student_id": "20260227", "class": 2, "number": 27, "name": "정예준", "final": 81, "perf1": 89, "perf2": 70, "perf3": 89, "total": 81.67, "grade": 4 }, { "student_id": "20260303", "class": 3, "number": 3, "name": "신가은", "final": 89, "perf1": 61, "perf2": 64, "perf3": 87, "total": 81.67, "grade": 4 }, { "student_id": "20260201", "class": 2, "number": 1, "name": "오나은", "final": 89, "perf1": 60, "perf2": 74, "perf3": 77, "total": 81.53, "grade": 4 }, { "student_id": "20260107", "class": 1, "number": 7, "name": "한하은", "final": 82, "perf1": 60, "perf2": 96, "perf3": 86, "total": 81.47, "grade": 4 }, { "student_id": "20260316", "class": 3, "number": 16, "name": "서민준", "final": 80, "perf1": 90, "perf2": 100, "perf3": 60, "total": 81.33, "grade": 4 }, { "student_id": "20260325", "class": 3, "number": 25, "name": "권하윤", "final": 90, "perf1": 60, "perf2": 71, "perf3": 73, "total": 81.2, "grade": 5 }, { "student_id": "20260420", "class": 4, "number": 20, "name": "윤나은", "final": 83, "perf1": 87, "perf2": 67, "perf3": 81, "total": 81.13, "grade": 5 }, { "student_id": "20260215", "class": 2, "number": 15, "name": "오유나", "final": 79, "perf1": 74, "perf2": 77, "perf3": 96, "total": 80.33, "grade": 5 }, { "student_id": "20260321", "class": 3, "number": 21, "name": "임지훈", "final": 78, "perf1": 81, "perf2": 74, "perf3": 94, "total": 80.0, "grade": 5 }, { "student_id": "20260327", "class": 3, "number": 27, "name": "서서현", "final": 80, "perf1": 77, "perf2": 91, "perf3": 71, "total": 79.87, "grade": 5 }, { "student_id": "20260208", "class": 2, "number": 8, "name": "임현준", "final": 80, "perf1": 93, "perf2": 75, "perf3": 70, "total": 79.73, "grade": 5 }, { "student_id": "20260318", "class": 3, "number": 18, "name": "이민서", "final": 81, "perf1": 73, "perf2": 72, "perf3": 88, "total": 79.67, "grade": 5 }, { "student_id": "20260413", "class": 4, "number": 13, "name": "이채원", "final": 84, "perf1": 69, "perf2": 76, "perf3": 74, "total": 79.6, "grade": 5 }, { "student_id": "20260424", "class": 4, "number": 24, "name": "윤서준", "final": 86, "perf1": 71, "perf2": 64, "perf3": 75, "total": 79.6, "grade": 5 }, { "student_id": "20260322", "class": 3, "number": 22, "name": "장다은", "final": 88, "perf1": 69, "perf2": 69, "perf3": 60, "total": 79.2, "grade": 5 }, { "student_id": "20260105", "class": 1, "number": 5, "name": "신예준", "final": 81, "perf1": 79, "perf2": 76, "perf3": 71, "total": 78.73, "grade": 5 }, { "student_id": "20260401", "class": 4, "number": 1, "name": "송하은", "final": 76, "perf1": 72, "perf2": 85, "perf3": 88, "total": 78.27, "grade": 5 }, { "student_id": "20260213", "class": 2, "number": 13, "name": "한민준", "final": 89, "perf1": 60, "perf2": 63, "perf3": 63, "total": 78.2, "grade": 5 }, { "student_id": "20260225", "class": 2, "number": 25, "name": "안승우", "final": 80, "perf1": 69, "perf2": 88, "perf3": 69, "total": 78.13, "grade": 5 }, { "student_id": "20260222", "class": 2, "number": 22, "name": "정채은", "final": 73, "perf1": 92, "perf2": 96, "perf3": 69, "total": 78.07, "grade": 5 }, { "student_id": "20260304", "class": 3, "number": 4, "name": "신예진", "final": 66, "perf1": 100, "perf2": 95, "perf3": 89, "total": 77.47, "grade": 5 }, { "student_id": "20260319", "class": 3, "number": 19, "name": "박민재", "final": 78, "perf1": 87, "perf2": 65, "perf3": 78, "total": 77.47, "grade": 5 }, { "student_id": "20260116", "class": 1, "number": 16, "name": "서지민", "final": 77, "perf1": 97, "perf2": 77, "perf3": 60, "total": 77.4, "grade": 5 }, { "student_id": "20260409", "class": 4, "number": 9, "name": "최하윤", "final": 78, "perf1": 78, "perf2": 62, "perf3": 88, "total": 77.2, "grade": 5 }, { "student_id": "20260414", "class": 4, "number": 14, "name": "한예진", "final": 75, "perf1": 71, "perf2": 72, "perf3": 98, "total": 77.13, "grade": 5 }, { "student_id": "20260112", "class": 1, "number": 12, "name": "정민서", "final": 84, "perf1": 66, "perf2": 74, "perf3": 60, "total": 77.07, "grade": 5 }, { "student_id": "20260307", "class": 3, "number": 7, "name": "박승우", "final": 74, "perf1": 72, "perf2": 86, "perf3": 87, "total": 77.07, "grade": 5 }, { "student_id": "20260315", "class": 3, "number": 15, "name": "황하준", "final": 77, "perf1": 80, "perf2": 72, "perf3": 79, "total": 77.0, "grade": 5 }, { "student_id": "20260301", "class": 3, "number": 1, "name": "안서연", "final": 77, "perf1": 69, "perf2": 85, "perf3": 76, "total": 76.87, "grade": 5 }, { "student_id": "20260104", "class": 1, "number": 4, "name": "황승우", "final": 71, "perf1": 91, "perf2": 95, "perf3": 70, "total": 76.73, "grade": 6 }, { "student_id": "20260403", "class": 4, "number": 3, "name": "강나은", "final": 77, "perf1": 75, "perf2": 76, "perf3": 78, "total": 76.73, "grade": 6 }, { "student_id": "20260123", "class": 1, "number": 23, "name": "이지민", "final": 73, "perf1": 80, "perf2": 95, "perf3": 71, "total": 76.6, "grade": 6 }, { "student_id": "20260219", "class": 2, "number": 19, "name": "안지호", "final": 76, "perf1": 79, "perf2": 74, "perf3": 79, "total": 76.53, "grade": 6 }, { "student_id": "20260412", "class": 4, "number": 12, "name": "조도윤", "final": 72, "perf1": 77, "perf2": 90, "perf3": 82, "total": 76.4, "grade": 6 }, { "student_id": "20260404", "class": 4, "number": 4, "name": "신서윤", "final": 78, "perf1": 88, "perf2": 65, "perf3": 67, "total": 76.13, "grade": 6 }, { "student_id": "20260429", "class": 4, "number": 29, "name": "박지후", "final": 74, "perf1": 78, "perf2": 71, "perf3": 88, "total": 76.0, "grade": 6 }, { "student_id": "20260214", "class": 2, "number": 14, "name": "한은우", "final": 71, "perf1": 100, "perf2": 89, "perf3": 60, "total": 75.8, "grade": 6 }, { "student_id": "20260218", "class": 2, "number": 18, "name": "장예은", "final": 71, "perf1": 97, "perf2": 80, "perf3": 70, "total": 75.53, "grade": 6 }, { "student_id": "20260210", "class": 2, "number": 10, "name": "강은우", "final": 78, "perf1": 70, "perf2": 81, "perf3": 64, "total": 75.47, "grade": 6 }, { "student_id": "20260402", "class": 4, "number": 2, "name": "김유나", "final": 71, "perf1": 72, "perf2": 88, "perf3": 86, "total": 75.4, "grade": 6 }, { "student_id": "20260226", "class": 2, "number": 26, "name": "이채은", "final": 83, "perf1": 60, "perf2": 61, "perf3": 70, "total": 75.27, "grade": 6 }, { "student_id": "20260408", "class": 4, "number": 8, "name": "이소율", "final": 72, "perf1": 74, "perf2": 81, "perf3": 85, "total": 75.2, "grade": 6 }, { "student_id": "20260130", "class": 1, "number": 30, "name": "한하윤", "final": 76, "perf1": 64, "perf2": 72, "perf3": 85, "total": 75.07, "grade": 6 }, { "student_id": "20260202", "class": 2, "number": 2, "name": "신지민", "final": 74, "perf1": 74, "perf2": 75, "perf3": 81, "total": 75.07, "grade": 6 }, { "student_id": "20260314", "class": 3, "number": 14, "name": "임지훈", "final": 74, "perf1": 60, "perf2": 80, "perf3": 88, "total": 74.8, "grade": 6 }, { "student_id": "20260124", "class": 1, "number": 24, "name": "임채원", "final": 82, "perf1": 60, "perf2": 60, "perf3": 71, "total": 74.67, "grade": 6 }, { "student_id": "20260411", "class": 4, "number": 11, "name": "안예준", "final": 64, "perf1": 98, "perf2": 88, "perf3": 86, "total": 74.67, "grade": 6 }, { "student_id": "20260310", "class": 3, "number": 10, "name": "안서준", "final": 75, "perf1": 76, "perf2": 62, "perf3": 83, "total": 74.47, "grade": 6 }, { "student_id": "20260122", "class": 1, "number": 22, "name": "이서준", "final": 78, "perf1": 61, "perf2": 64, "perf3": 80, "total": 74.13, "grade": 6 }, { "student_id": "20260108", "class": 1, "number": 8, "name": "박채은", "final": 61, "perf1": 100, "perf2": 88, "perf3": 91, "total": 73.8, "grade": 7 }, { "student_id": "20260223", "class": 2, "number": 23, "name": "안하윤", "final": 78, "perf1": 67, "perf2": 65, "perf3": 70, "total": 73.73, "grade": 7 }, { "student_id": "20260428", "class": 4, "number": 28, "name": "황지우", "final": 78, "perf1": 63, "perf2": 79, "perf3": 60, "total": 73.73, "grade": 7 }, { "student_id": "20260323", "class": 3, "number": 23, "name": "이예진", "final": 71, "perf1": 89, "perf2": 84, "perf3": 60, "total": 73.67, "grade": 7 }, { "student_id": "20260103", "class": 1, "number": 3, "name": "안예진", "final": 66, "perf1": 90, "perf2": 80, "perf3": 85, "total": 73.6, "grade": 7 }, { "student_id": "20260417", "class": 4, "number": 17, "name": "서하은", "final": 72, "perf1": 60, "perf2": 72, "perf3": 93, "total": 73.2, "grade": 7 }, { "student_id": "20260426", "class": 4, "number": 26, "name": "한서연", "final": 70, "perf1": 79, "perf2": 70, "perf3": 84, "total": 73.07, "grade": 7 }, { "student_id": "20260422", "class": 4, "number": 22, "name": "김지후", "final": 65, "perf1": 60, "perf2": 93, "perf3": 100, "total": 72.73, "grade": 7 }, { "student_id": "20260330", "class": 3, "number": 30, "name": "조시우", "final": 68, "perf1": 82, "perf2": 77, "perf3": 80, "total": 72.67, "grade": 7 }, { "student_id": "20260106", "class": 1, "number": 6, "name": "신유나", "final": 65, "perf1": 85, "perf2": 77, "perf3": 84, "total": 71.8, "grade": 7 }, { "student_id": "20260117", "class": 1, "number": 17, "name": "정승우", "final": 67, "perf1": 84, "perf2": 71, "perf3": 79, "total": 71.4, "grade": 7 }, { "student_id": "20260203", "class": 2, "number": 3, "name": "오서준", "final": 63, "perf1": 83, "perf2": 77, "perf3": 84, "total": 70.33, "grade": 7 }, { "student_id": "20260113", "class": 1, "number": 13, "name": "권태양", "final": 59, "perf1": 85, "perf2": 91, "perf3": 85, "total": 70.2, "grade": 7 }, { "student_id": "20260121", "class": 1, "number": 21, "name": "박가은", "final": 64, "perf1": 69, "perf2": 77, "perf3": 90, "total": 69.87, "grade": 7 }, { "student_id": "20260312", "class": 3, "number": 12, "name": "조준서", "final": 69, "perf1": 60, "perf2": 83, "perf3": 70, "total": 69.8, "grade": 8 }, { "student_id": "20260120", "class": 1, "number": 20, "name": "이시우", "final": 66, "perf1": 69, "perf2": 85, "perf3": 72, "total": 69.73, "grade": 8 }, { "student_id": "20260217", "class": 2, "number": 17, "name": "서도윤", "final": 72, "perf1": 60, "perf2": 68, "perf3": 70, "total": 69.6, "grade": 8 }, { "student_id": "20260115", "class": 1, "number": 15, "name": "정예준", "final": 64, "perf1": 69, "perf2": 90, "perf3": 69, "total": 68.8, "grade": 8 }, { "student_id": "20260418", "class": 4, "number": 18, "name": "송도윤", "final": 67, "perf1": 60, "perf2": 86, "perf3": 68, "total": 68.73, "grade": 8 }, { "student_id": "20260306", "class": 3, "number": 6, "name": "이승현", "final": 64, "perf1": 71, "perf2": 66, "perf3": 74, "total": 66.53, "grade": 8 }, { "student_id": "20260230", "class": 2, "number": 30, "name": "조유나", "final": 63, "perf1": 67, "perf2": 67, "perf3": 80, "total": 66.33, "grade": 8 }, { "student_id": "20260317", "class": 3, "number": 17, "name": "서소율", "final": 66, "perf1": 60, "perf2": 76, "perf3": 64, "total": 66.27, "grade": 8 }, { "student_id": "20260111", "class": 1, "number": 11, "name": "서지후", "final": 59, "perf1": 60, "perf2": 91, "perf3": 78, "total": 65.93, "grade": 8 }, { "student_id": "20260430", "class": 4, "number": 30, "name": "조수현", "final": 62, "perf1": 73, "perf2": 79, "perf3": 62, "total": 65.73, "grade": 9 }, { "student_id": "20260211", "class": 2, "number": 11, "name": "서예진", "final": 56, "perf1": 88, "perf2": 65, "perf3": 84, "total": 65.2, "grade": 9 }, { "student_id": "20260425", "class": 4, "number": 25, "name": "한지호", "final": 55, "perf1": 82, "perf2": 68, "perf3": 88, "total": 64.73, "grade": 9 }, { "student_id": "20260206", "class": 2, "number": 6, "name": "정현준", "final": 57, "perf1": 64, "perf2": 60, "perf3": 80, "total": 61.4, "grade": 9 }, { "student_id": "20260416", "class": 4, "number": 16, "name": "최민서", "final": 52, "perf1": 76, "perf2": 72, "perf3": 60, "total": 58.93, "grade": 9 }] };

        // 반별 평균 점수 차트
        const classCtx = document.getElementById('classChart').getContext('2d');
        new Chart(classCtx, {
            type: 'bar',
            data: {
                labels: statsData.class_stats.map(c => c.class + '반'),
                datasets: [
                    {
                        label: '기말',
                        data: statsData.class_stats.map(c => c.avg_final),
                        backgroundColor: 'rgba(102, 126, 234, 0.8)',
                    },
                    {
                        label: '수행1',
                        data: statsData.class_stats.map(c => c.avg_perf1),
                        backgroundColor: 'rgba(118, 75, 162, 0.8)',
                    },
                    {
                        label: '수행2',
                        data: statsData.class_stats.map(c => c.avg_perf2),
                        backgroundColor: 'rgba(237, 100, 166, 0.8)',
                    },
                    {
                        label: '수행3',
                        data: statsData.class_stats.map(c => c.avg_perf3),
                        backgroundColor: 'rgba(255, 154, 162, 0.8)',
                    }
                ]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'top',
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        max: 100
                    }
                }
            }
        });

        // 등급 분포 차트
        const gradeCtx = document.getElementById('gradeChart').getContext('2d');
        new Chart(gradeCtx, {
            type: 'doughnut',
            data: {
                labels: ['1등급', '2등급', '3등급', '4등급', '5등급', '6등급', '7등급', '8등급', '9등급'],
                datasets: [{
                    data: statsData.grade_distribution,
                    backgroundColor: [
                        '#4CAF50',
                        '#66BB6A',
                        '#2196F3',
                        '#42A5F5',
                        '#FF9800',
                        '#FFA726',
                        '#f44336',
                        '#EF5350',
                        '#E53935'
                    ]
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'right',
                    }
                }
            }
        });

        // 과목별 평균 점수 차트
        const subjectCtx = document.getElementById('subjectChart').getContext('2d');
        new Chart(subjectCtx, {
            type: 'bar',
            data: {
                labels: ['기말고사', '수행평가1', '수행평가2', '수행평가3'],
                datasets: [{
                    label: '평균 점수',
                    data: [
                        statsData.avg_final,
                        statsData.avg_perf1,
                        statsData.avg_perf2,
                        statsData.avg_perf3
                    ],
                    backgroundColor: [
                        'rgba(102, 126, 234, 0.8)',
                        'rgba(118, 75, 162, 0.8)',
                        'rgba(237, 100, 166, 0.8)',
                        'rgba(255, 154, 162, 0.8)'
                    ]
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        display: false
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        max: 100
                    }
                }
            }
        });

        // 레이더 차트
        const radarCtx = document.getElementById('radarChart').getContext('2d');
        new Chart(radarCtx, {
            type: 'radar',
            data: {
                labels: ['기말', '수행1', '수행2', '수행3', '총점'],
                datasets: statsData.class_stats.map((c, idx) => ({
                    label: c.class + '반',
                    data: [c.avg_final, c.avg_perf1, c.avg_perf2, c.avg_perf3, c.avg_total],
                    borderColor: `hsl(${idx * 90}, 70%, 50%)`,
                    backgroundColor: `hsla(${idx * 90}, 70%, 50%, 0.2)`
                }))
            },
            options: {
                responsive: true,
                scales: {
                    r: {
                        beginAtZero: true,
                        max: 100
                    }
                }
            }
        });

        // 상위 10명 테이블 렌더링
        const topTableBody = document.getElementById('topTableBody');
        statsData.top_students.forEach(student => {
            const row = document.createElement('tr');
            const rankClass = student.rank <= 3 ? `rank-${student.rank}` : '';
            row.innerHTML = `
                <td><span class="rank-badge ${rankClass}">${student.rank}</span></td>
                <td>${student.student_id}</td>
                <td>${student.class}</td>
                <td>${student.number}</td>
                <td>${student.name}</td>
                <td>${student.final}</td>
                <td>${student.perf1}</td>
                <td>${student.perf2}</td>
                <td>${student.perf3}</td>
                <td><strong>${student.total.toFixed(2)}</strong></td>
            `;
            topTableBody.appendChild(row);
        });

        // 전체 학생 테이블 렌더링
        const allTableBody = document.getElementById('allTableBody');
        let allStudentsData = statsData.all_students;

        function renderAllStudents(students) {
            allTableBody.innerHTML = '';
            students.forEach(student => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${student.student_id}</td>
                    <td>${student.class}</td>
                    <td>${student.number}</td>
                    <td>${student.name}</td>
                    <td>${student.final}</td>
                    <td>${student.perf1}</td>
                    <td>${student.perf2}</td>
                    <td>${student.perf3}</td>
                    <td><strong>${student.total.toFixed(2)}</strong></td>
                    <td><span class="grade-badge grade-${student.grade}">${student.grade}등급</span></td>
                `;
                allTableBody.appendChild(row);
            });
        }

        renderAllStudents(allStudentsData);

        // 검색 기능
        const searchBox = document.getElementById('searchBox');
        searchBox.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filtered = allStudentsData.filter(student =>
                student.name.toLowerCase().includes(searchTerm) ||
                student.student_id.toLowerCase().includes(searchTerm)
            );
            renderAllStudents(filtered);
        });

        // 테이블 정렬 기능
        let sortDirection = true;
        function sortTable(columnIndex) {
            const keys = ['student_id', 'class', 'number', 'name', 'final', 'perf1', 'perf2', 'perf3', 'total', 'grade'];
            const key = keys[columnIndex];

            allStudentsData.sort((a, b) => {
                let aVal = a[key];
                let bVal = b[key];

                if (typeof aVal === 'string') {
                    return sortDirection ? aVal.localeCompare(bVal) : bVal.localeCompare(aVal);
                } else {
                    return sortDirection ? aVal - bVal : bVal - aVal;
                }
            });

            sortDirection = !sortDirection;
            renderAllStudents(allStudentsData);
        }
    </script>
</body>

</html>
