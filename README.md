<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Route Filter UI</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .route-filter {
            max-width: 400px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(28, 46, 70, 0.1);
            overflow: hidden;
            position: relative;
        }

        .route-filter::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #1c2e46, #4a6fa5, #1c2e46);
            background-size: 200% 100%;
            animation: shimmer 3s ease-in-out infinite;
        }

        @keyframes shimmer {
            0%, 100% { background-position: 200% 0; }
            50% { background-position: -200% 0; }
        }

        .dest-search {
            padding: 25px;
            background: linear-gradient(135deg, #1c2e46 0%, #2d4a6b 100%);
            color: white;
            position: relative;
            overflow: hidden;
        }

        .dest-search::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translate(-50%, -50%) rotate(0deg); }
            50% { transform: translate(-50%, -50%) rotate(180deg); }
        }

        .dest-search input {
            width: 100%;
            padding: 15px 20px;
            border: none;
            border-radius: 50px;
            background: rgba(255, 255, 255, 0.15);
            color: white;
            font-size: 16px;
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
            position: relative;
            z-index: 1;
        }

        .dest-search input::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }

        .dest-search input:focus {
            outline: none;
            background: rgba(255, 255, 255, 0.25);
            box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.3);
            transform: scale(1.02);
        }

        .filter-button {
            padding: 25px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .fab-rotate {
            background: linear-gradient(135deg, #1c2e46 0%, #4a6fa5 100%);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 50px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(28, 46, 70, 0.3);
            position: relative;
            overflow: hidden;
        }

        .fab-rotate::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            transition: all 0.3s ease;
            transform: translate(-50%, -50%);
        }

        .fab-rotate:hover::before {
            width: 300px;
            height: 300px;
        }

        .fab-rotate:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(28, 46, 70, 0.4);
        }

        .toggle-container {
            display: flex;
            gap: 10px;
            margin: 15px 0;
        }

        .walk {
            flex: 1;
            padding: 15px;
            border: 2px solid #e0e6ed;
            border-radius: 15px;
            background: white;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-size: 14px;
            font-weight: 500;
            color: #64748b;
            position: relative;
            overflow: hidden;
        }

        .walk::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(28, 46, 70, 0.1), transparent);
            transition: left 0.5s ease;
        }

        .walk:hover::before {
            left: 100%;
        }

        .walk.active-toggle {
            border-color: #1c2e46;
            background: linear-gradient(135deg, #1c2e46 0%, #4a6fa5 100%);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(28, 46, 70, 0.3);
        }

        .walk i {
            font-size: 18px;
            transition: transform 0.3s ease;
        }

        .walk:hover i {
            transform: scale(1.1);
        }

        .checkbox-container {
            display: flex;
            align-items: center;
            gap: 12px;
            margin: 20px 0;
            padding: 15px;
            border-radius: 12px;
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            transition: all 0.3s ease;
        }

        .checkbox-container:hover {
            background: #f1f5f9;
            border-color: #1c2e46;
        }

        .ranked-spot-checkbox {
            width: 20px;
            height: 20px;
            accent-color: #1c2e46;
            cursor: pointer;
            border-radius: 4px;
        }

        .checkbox-container label {
            font-size: 14px;
            color: #475569;
            cursor: pointer;
            font-weight: 500;
        }

        .site-select {
            margin: 20px 0;
        }

        .site-select div {
            padding: 15px;
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-weight: 500;
            color: #475569;
        }

        .site-select div:hover {
            background: #1c2e46;
            color: white;
            transform: translateY(-1px);
            box-shadow: 0 4px 12px rgba(28, 46, 70, 0.2);
        }

        .rank-options, .rank-options-sort {
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 12px;
            margin: 10px 0;
            padding: 10px;
            animation: slideDown 0.3s ease;
        }

        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .rank-options p, .rank-options-sort p {
            padding: 12px 15px;
            cursor: pointer;
            border-radius: 8px;
            transition: all 0.3s ease;
            color: #475569;
            font-size: 14px;
            font-weight: 500;
            margin: 5px 0;
            position: relative;
        }

        .rank-options p:hover, .rank-options-sort p:hover {
            background: #1c2e46;
            color: white;
            transform: translateX(5px);
        }

        .rank-button {
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
            margin: 20px 0;
            width: 100%;
        }

        .rank-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(16, 185, 129, 0.4);
        }

        .error-message {
            color: #ef4444;
            font-size: 14px;
            text-align: center;
            padding: 10px;
            background: #fef2f2;
            border-radius: 8px;
            border: 1px solid #fecaca;
            margin-top: 10px;
        }

        .skeleton-wrapper {
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .skeleton-spot-card {
            height: 80px;
            background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
            background-size: 200% 100%;
            animation: skeleton-loading 1.5s infinite;
            border-radius: 12px;
        }

        @keyframes skeleton-loading {
            0% { background-position: 200% 0; }
            100% { background-position: -200% 0; }
        }

        .ranked-spot-item {
            margin: 15px 20px;
            padding: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 4px 12px rgba(28, 46, 70, 0.1);
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid #e2e8f0;
            position: relative;
            overflow: hidden;
        }

        .ranked-spot-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 3px;
            background: linear-gradient(90deg, #1c2e46, #4a6fa5);
            transform: scaleX(0);
            transition: transform 0.3s ease;
        }

        .ranked-spot-item:hover::before {
            transform: scaleX(1);
        }

        .ranked-spot-item:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 30px rgba(28, 46, 70, 0.15);
            border-color: #1c2e46;
        }

        .ranked-spot-item p {
            margin: 8px 0;
            font-size: 14px;
            color: #475569;
            font-weight: 500;
        }

        .ranked-spot-item p:first-child {
            font-size: 16px;
            font-weight: 600;
            color: #1c2e46;
            margin-bottom: 12px;
        }

        .spot-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin-top: 12px;
        }

        .spot-info p {
            background: #f8fafc;
            padding: 8px 12px;
            border-radius: 8px;
            text-align: center;
            border: 1px solid #e2e8f0;
        }

        @media (max-width: 480px) {
            .route-filter {
                margin: 10px;
                border-radius: 15px;
            }
            
            .toggle-container {
                flex-direction: column;
            }
            
            .spot-info {
                grid-template-columns: 1fr;
            }
        }

        .pulse {
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
    </style>
</head>
<body>
    <div class="route-filter">
        <div class="dest-search">
            <input type="text" placeholder="🎯 Search destination..." id="destinationInput">
        </div>
        
        <div class="filter-button">
            <button class="fab-rotate" onclick="rotateMap()">
                <i class="fas fa-sync-alt"></i> Rotate Map
            </button>

            <div class="toggle-container">
                <button class="walk active-toggle" id="drivingBtn" onclick="toggleTransport('driving')">
                    <i class="fas fa-car"></i>
                    <span>Driving</span>
                </button>

                <button class="walk" id="walkingBtn" onclick="toggleTransport('walking')">
                    <i class="fas fa-walking"></i>
                    <span>Walking</span>
                </button>
            </div>

            <div class="checkbox-container">
                <input type="checkbox" id="hideViewed" class="ranked-spot-checkbox" onchange="toggleHideViewed()">
                <label for="hideViewed">Hide previously shown spots</label>
            </div>

            <div class="site-select">
                <div onclick="toggleRankBy()">
                    <span>Rank spots by:</span>
                    <span id="rankByIcon">▶</span>
                </div>
            </div>

            <div class="rank-options" id="rankOptions" style="display: none;">
                <p onclick="selectRankBy('distance', 'closestToUser')">
                    <i class="fas fa-map-marker-alt"></i> Closest to you
                </p>
                <p onclick="selectRankBy('distance', 'closestToDestination')">
                    <i class="fas fa-bullseye"></i> Closest to destination
                </p>
                <p onclick="selectRankBy('price', 'cheapest')">
                    <i class="fas fa-dollar-sign"></i> All Cheapest
                </p>
            </div>

            <div class="rank-options-sort" id="sortOptions" style="display: none;">
                <p style="font-weight: 600; color: #1c2e46;">Sort By:</p>
                <p onclick="setSortBy('price')">
                    <i class="fas fa-coins"></i> Price
                </p>
                <p onclick="setSortBy('None')">
                    <i class="fas fa-times"></i> None
                </p>
            </div>

            <div class="rank-options-sort" id="priceSortOptions" style="display: none;">
                <p onclick="setSortBy('DistanceToUser')">
                    <i class="fas fa-user"></i> Closest to you
                </p>
                <p onclick="setSortBy('DistanceToDest')">
                    <i class="fas fa-flag"></i> Closest to your destination
                </p>
                <p onclick="setSortBy('None')">
                    <i class="fas fa-times"></i> None
                </p>
            </div>

            <button class="rank-button" onclick="handleRankTypeChange()">
                <i class="fas fa-sort"></i> Apply Ranking
            </button>

            <div class="error-message" id="errorMessage" style="display: none;">
                Error loading routes. Please try again.
            </div>
        </div>

        <div class="skeleton-wrapper" id="skeletonLoader" style="display: none;">
            <div class="skeleton-spot-card"></div>
            <div class="skeleton-spot-card"></div>
            <div class="skeleton-spot-card"></div>
            <div class="skeleton-spot-card"></div>
            <div class="skeleton-spot-card"></div>
        </div>

        <div id="routeList">
            <!-- Sample route items -->
            <div class="ranked-spot-item" onclick="showRoute(this)">
                <p>Lot: Downtown Plaza</p>
                <div class="spot-info">
                    <p>Distance: 850m</p>
                    <p>ETA: 6 min</p>
                    <p>Price: $2.50</p>
                    <p>Rating: ⭐ 4.8</p>
                </div>
            </div>

            <div class="ranked-spot-item" onclick="showRoute(this)">
                <p>Lot: City Center Mall</p>
                <div class="spot-info">
                    <p>Distance: 1.2km</p>
                    <p>ETA: 8 min</p>
                    <p>Price: $3.00</p>
                    <p>Rating: ⭐ 4.6</p>
                </div>
            </div>

            <div class="ranked-spot-item" onclick="showRoute(this)">
                <p>Lot: Business District</p>
                <div class="spot-info">
                    <p>Distance: 950m</p>
                    <p>ETA: 7 min</p>
                    <p>Price: $1.75</p>
                    <p>Rating: ⭐ 4.9</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        let isDriving = true;
        let hideViewed = false;
        let rankBy = false;
        let sortBy = null;
        let rankValue = null;
        let sortByValue = null;

        function rotateMap() {
            const btn = document.querySelector('.fab-rotate');
            btn.style.transform = 'rotate(360deg)';
            setTimeout(() => {
                btn.style.transform = 'rotate(0deg)';
            }, 300);
            console.log('Map rotated');
        }

        function toggleTransport(mode) {
            isDriving = mode === 'driving';
            document.getElementById('drivingBtn').classList.toggle('active-toggle', isDriving);
            document.getElementById('walkingBtn').classList.toggle('active-toggle', !isDriving);
            console.log('Transport mode:', mode);
        }

        function toggleHideViewed() {
            hideViewed = !hideViewed;
            console.log('Hide viewed:', hideViewed);
        }

        function toggleRankBy() {
            rankBy = !rankBy;
            const options = document.getElementById('rankOptions');
            const icon = document.getElementById('rankByIcon');
            
            if (rankBy) {
                options.style.display = 'block';
                icon.textContent = '▼';
            } else {
                options.style.display = 'none';
                icon.textContent = '▶';
            }
            
            setSortBy(null);
        }

        function selectRankBy(type, value) {
            sortBy = type;
            rankValue = value;
            
            const sortOptions = document.getElementById('sortOptions');
            const priceSortOptions = document.getElementById('priceSortOptions');
            
            if (type === 'distance') {
                sortOptions.style.display = 'block';
                priceSortOptions.style.display = 'none';
            } else {
                sortOptions.style.display = 'none';
                priceSortOptions.style.display = 'none';
            }
            
            console.log('Selected rank by:', type, value);
        }

        function setSortBy(value) {
            sortByValue = value;
            
            const sortOptions = document.getElementById('sortOptions');
            const priceSortOptions = document.getElementById('priceSortOptions');
            
            if (sortBy === 'price' && value !== 'None') {
                priceSortOptions.style.display = 'block';
                sortOptions.style.display = 'none';
            } else {
                priceSortOptions.style.display = 'none';
                if (sortBy === 'distance') {
                    sortOptions.style.display = 'block';
                }
            }
            
            console.log('Sort by value:', value);
        }

        function handleRankTypeChange() {
            const btn = document.querySelector('.rank-button');
            btn.classList.add('pulse');
            
            // Show loading skeleton
            document.getElementById('skeletonLoader').style.display = 'block';
            document.getElementById('routeList').style.display = 'none';
            
            setTimeout(() => {
                btn.classList.remove('pulse');
                document.getElementById('skeletonLoader').style.display = 'none';
                document.getElementById('routeList').style.display = 'block';
            }, 2000);
            
            console.log('Ranking applied');
        }

        function showRoute(element) {
            // Remove active class from all items
            document.querySelectorAll('.ranked-spot-item').forEach(item => {
                item.style.background = 'white';
                item.style.color = '#475569';
            });
            
            // Add active styling to clicked item
            element.style.background = 'linear-gradient(135deg, #1c2e46 0%, #4a6fa5 100%)';
            element.style.color = 'white';
            
            console.log('Route selected:', element.querySelector('p').textContent);
        }

        function showError(message) {
            const errorDiv = document.getElementById('errorMessage');
            errorDiv.textContent = message;
            errorDiv.style.display = 'block';
            setTimeout(() => {
                errorDiv.style.display = 'none';
            }, 3000);
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Route Filter UI initialized');
        });
    </script>
</body>
</html>
