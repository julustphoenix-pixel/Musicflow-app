<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Music - Mon App Musique & Vidéo</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 400px;
            margin: 0 auto;
        }
        
        .header {
            text-align: center;
            margin-bottom: 20px;
        }
        
        .header h1 {
            font-size: 28px;
            margin-bottom: 5px;
        }

        .tabs {
            display: flex;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 5px;
            margin-bottom: 20px;
            backdrop-filter: blur(10px);
        }
        
        .tab {
            flex: 1;
            text-align: center;
            padding: 12px;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
        }
        
        .tab.active {
            background: #ff6b6b;
            box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .player {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .media-container {
            width: 100%;
            height: 200px;
            border-radius: 15px;
            margin: 0 auto 20px;
            overflow: hidden;
            background: #000;
            position: relative;
        }
        
        #real-audio-player, #real-video-player-display {
            width: 100%;
            height: 100%;
            display: none;
        }
        
        .media-placeholder {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 48px;
            background: linear-gradient(45deg, #ff6b6b, #ffa726);
        }
        
        .media-info {
            text-align: center;
            margin-bottom: 25px;
        }
        
        .media-info h2 {
            font-size: 20px;
            margin-bottom: 5px;
        }
        
        .media-info p {
            color: rgba(255, 255, 255, 0.7);
            font-size: 14px;
        }
        
        .progress-container {
            margin-bottom: 20px;
        }
        
        .progress-bar {
            width: 100%;
            height: 6px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 3px;
            overflow: hidden;
            margin-bottom: 5px;
            cursor: pointer;
        }
        
        .progress {
            width: 0%;
            height: 100%;
            background: #ff6b6b;
            border-radius: 3px;
            transition: width 0.1s;
        }
        
        .time {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: rgba(255, 255, 255, 0.7);
        }
        
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .control-btn {
            background: none;
            border: none;
            color: white;
            font-size: 24px;
            cursor: pointer;
            padding: 10px;
            border-radius: 50%;
            transition: all 0.3s;
        }
        
        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
        }
        
        .play-btn {
            background: #ff6b6b;
            width: 60px;
            height: 60px;
            font-size: 28px;
        }
        
        .play-btn:hover {
            background: #ff5252;
            transform: scale(1.1);
        }
        
        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .volume-slider {
            flex: 1;
            height: 4px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 2px;
            outline: none;
            cursor: pointer;
        }
        
        .file-input {
            display: none;
        }
        
        .upload-btn {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            cursor: pointer;
            margin: 10px 0;
            width: 100%;
        }
        
        .library {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
        }
        
        .library h3 {
            margin-bottom: 15px;
            font-size: 18px;
        }
        
        .library-item {
            display: flex;
            align-items: center;
            padding: 12px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            margin-bottom: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .library-item:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: translateX(5px);
        }
        
        .library-item.active {
            background: rgba(255, 107, 107, 0.2);
            border-left: 3px solid #ff6b6b;
        }
        
        .item-thumbnail {
            width: 40px;
            height: 40px;
            background: linear-gradient(45deg, #45b7d1, #4ecdc4);
            border-radius: 5px;
            margin-right: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
        }
        
        .item-details {
            flex: 1;
        }
        
        .item-details h4 {
            font-size: 14px;
            margin-bottom: 2px;
        }
        
        .item-details p {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.6);
        }
        
        .item-duration {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.5);
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎵 My Music 🎬</h1>
            <p>Mon application musique personnelle</p>
        </div>
        
        <div class="tabs">
            <div class="tab active" onclick="switchTab('music')">🎵 Musique</div>
            <div class="tab" onclick="switchTab('video')">🎬 Vidéo</div>
        </div>
        
        <audio id="real-audio-player" controls></audio>
        <video id="real-video-player-display" controls></video>
        
        <div id="music-tab" class="tab-content active">
            <div class="player">
                <div class="media-container">
                    <div class="media-placeholder" id="music-placeholder">
                        🎵<br>
                        <small style="font-size: 14px;">Lecteur Audio</small>
                    </div>
                </div>
                
                <div class="media-info">
                    <h2 id="music-title">Aucun fichier audio</h2>
                    <p id="music-artist">Importe ta musique</p>
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="music-progress-bar">
                        <div class="progress" id="music-progress"></div>
                    </div>
                    <div class="time">
                        <span id="music-current-time">0:00</span>
                        <span id="music-duration">0:00</span>
                    </div>
                </div>
                
                <div class="controls">
                    <button class="control-btn" onclick="previousAudio()">⏮</button>
                    <button class="control-btn play-btn" onclick="toggleAudioPlay()" id="audio-play-btn">▶️</button>
                    <button class="control-btn" onclick="nextAudio()">⏭</button>
                </div>
                
                <div class="volume-container">
                    <span>🔊</span>
                    <input type="range" min="0" max="100" value="50" class="volume-slider" id="audio-volume">
                </div>
                
                <button class="upload-btn" onclick="document.getElementById('audio-upload').click()">
                    📁 Importer musique
                </button>
                <input type="file" id="audio-upload" class="file-input" accept="audio/*" onchange="handleAudioUpload(this.files)">
            </div>
            
            <div class="library">
                <h3>📋 Mes Musiques</h3>
                <div id="audio-library"></div>
            </div>
        </div>
        
        <div id="video-tab" class="tab-content">
            <div class="player">
                <div class="media-container">
                    <div class="media-placeholder" id="video-placeholder">
                        🎬<br>
                        <small style="font-size: 14px;">Lecteur Vidéo</small>
                    </div>
                    <video id="real-video-player-display" style="width: 100%; height: 100%; display: none;"></video>
                </div>
                
                <div class="media-info">
                    <h2 id="video-title">Aucun fichier vidéo</h2>
                    <p id="video-description">Importe ta vidéo</p>
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="video-progress-bar">
                        <div class="progress" id="video-progress"></div>
                    </div>
                    <div class="time">
                        <span id="video-current-time">0:00</span>
                        <span id="video-duration">0:00</span>
                    </div>
                </div>
                
                <div class="controls">
                    <button class="control-btn" onclick="previousVideo()">⏮</button>
                    <button class="control-btn play-btn" onclick="toggleVideoPlay()" id="video-play-btn">▶️</button>
                    <button class="control-btn" onclick="nextVideo()">⏭</button>
                </div>
                
                <div class="volume-container">
                    <span>🔊</span>
                    <input type="range" min="0" max="100" value="50" class="volume-slider" id="video-volume">
                </div>
                
                <button class="upload-btn" onclick="document.getElementById('video-upload').click()">
                    📁 Importer vidéo
                </button>
                <input type="file" id="video-upload" class="file-input" accept="video/*" onchange="handleVideoUpload(this.files)">
            </div>
            
            <div class="library">
                <h3>🎥 Mes Vidéos</h3>
                <div id="video-library"></div>
            </div>
        </div>
    </div>

    <script>
        let currentTab = 'music';
        let audioFiles = [];
        let videoFiles = [];
        let currentAudioIndex = 0;
        let currentVideoIndex = 0;
        
        const audioPlayer = document.getElementById('real-audio-player');
        const videoPlayer = document.getElementById('real-video-player-display');
        
        function switchTab(tabName) {
            currentTab = tabName;
            
            document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
            
            document.querySelector(`.tab:nth-child(${tabName === 'music' ? 1 : 2})`).classList.add('active');
            document.getElementById(`${tabName}-tab`).classList.add('active');
            
            showNotification(`Onglet ${tabName === 'music' ? 'Musique 🎵' : 'Vidéo 🎬'} activé`);
        }
        
        function showNotification(message) {
            console.log('🔔 ' + message);
        }
        
        function formatTime(seconds) {
            if (isNaN(seconds)) return '0:00';
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs.toString().padStart(2, '0')}`;
        }
        
        function handleAudioUpload(files) {
            if (files.length > 0) {
                const file = files[0];
                const url = URL.createObjectURL(file);
                
                audioFiles.push({
                    name: file.name.replace(/\.[^/.]+$/, ""),
                    url: url,
                    file: file,
                    duration: '0:00'
                });
                
                renderAudioLibrary();
                showNotification(`🎵 "${file.name}" importé !`);
                
                if (audioFiles.length === 1) {
                    playAudio(0);
                }
            }
        }
        
        function renderAudioLibrary() {
            const library = document.getElementById('audio-library');
            library.innerHTML = '';
            
            audioFiles.forEach((audio, index) => {
                const item = document.createElement('div');
                item.className = `library-item ${index === currentAudioIndex ? 'active' : ''}`;
                item.innerHTML = `
                    <div class="item-thumbnail">🎵</div>
                    <div class="item-details">
                        <h4>${audio.name}</h4>
                        <p>Fichier audio</p>
                    </div>
                    <div class="item-duration">${audio.duration}</div>
                `;
                item.onclick = () => playAudio(index);
                library.appendChild(item);
            });
        }
        
        function playAudio(index) {
            if (audioFiles.length === 0) return;
            
            currentAudioIndex = index;
            const audio = audioFiles[index];
            
            audioPlayer.src = audio.url;
            document.getElementById('music-title').textContent = audio.name;
            document.getElementById('music-artist').textContent = 'Fichier audio';
            
            document.getElementById('music-placeholder').style.display = 'none';
            
            renderAudioLibrary();
            
            audioPlayer.onloadedmetadata = function() {
                audio.duration = formatTime(audioPlayer.duration);
                document.getElementById('music-duration').textContent = audio.duration;
                renderAudioLibrary();
            };
            
            audioPlayer.play();
            document.getElementById('audio-play-btn').textContent = '⏸️';
            updateAudioProgress();
            
            showNotification(`🎵 Lecture de: ${audio.name}`);
        }
        
        function toggleAudioPlay() {
            if (audioFiles.length === 0) return;
            
            if (audioPlayer.paused) {
                audioPlayer.play();
                document.getElementById('audio-play-btn').textContent = '⏸️';
            } else {
                audioPlayer.pause();
                document.getElementById('audio-play-btn').textContent = '▶️';
            }
        }
        
        function updateAudioProgress() {
            audioPlayer.ontimeupdate = function() {
                const progress = (audioPlayer.currentTime / audioPlayer.duration) * 100;
                document.getElementById('music-progress').style.width = progress + '%';
                document.getElementById('music-current-time').textContent = formatTime(audioPlayer.currentTime);
            };
        }
        
        function previousAudio() {
            if (audioFiles.length === 0) return;
            const newIndex = (currentAudioIndex - 1 + audioFiles.length) % audioFiles.length;
            playAudio(newIndex);
        }
        
        function nextAudio() {
            if (audioFiles.length === 0) return;
            const newIndex = (currentAudioIndex + 1) % audioFiles.length;
            playAudio(newIndex);
        }
        
        function handleVideoUpload(files) {
            if (files.length > 0) {
                const file = files[0];
                const url = URL.createObjectURL(file);
                
                videoFiles.push({
                    name: file.name.replace(/\.[^/.]+$/, ""),
                    url: url,
                    file: file,
                    duration: '0:00'
                });
                
                renderVideoLibrary();
                showNotification(`🎬 "${file.name}" importé !`);
                
                if (videoFiles.length === 1) {
                    playVideo(0);
                }
            }
        }
        
        function renderVideoLibrary() {
            const library = document.getElementById('video-library');
            library.innerHTML = '';
            
            videoFiles.forEach((video, index) => {
                const item = document.createElement('div');
                item.className = `library-item ${index === currentVideoIndex ? 'active' : ''}`;
                item.innerHTML = `
                    <div class="item-thumbnail">🎬</div>
                    <div class="item-details">
                        <h4>${video.name}</h4>
                        <p>Fichier vidéo</p>
                    </div>
                    <div class="item-duration">${video.duration}</div>
                `;
                item.onclick = () => playVideo(index);
                library.appendChild(item);
            });
        }
        
        function playVideo(index) {
            if (videoFiles.length === 0) return;
            
            currentVideoIndex = index;
            const video = videoFiles[index];
            
            videoPlayer.src = video.url;
            document.getElementById('video-title').textContent = video.name;
            document.getElementById('video-description').textContent = 'Fichier vidéo';
            
            document.getElementById('video-placeholder').style.display = 'none';
            videoPlayer.style.display = 'block';
            
            renderVideoLibrary();
            
            videoPlayer.onloadedmetadata = function() {
                video.duration = formatTime(videoPlayer.duration);
                document.getElementById('video-duration').textContent = video.duration;
                renderVideoLibrary();
            };
            
            videoPlayer.play();
            document.getElementById('video-play-btn').textContent = '⏸️';
            updateVideoProgress();
            
            showNotification(`🎬 Lecture de: ${video.name}`);
        }
        
        function toggleVideoPlay() {
            if (videoFiles.length === 0) return;
            
            if (videoPlayer.paused) {
                videoPlayer.play();
                document.getElementById('video-play-btn').textContent = '⏸️';
            } else {
                videoPlayer.pause();
                document.getElementById('video-play-btn').textContent = '▶️';
            }
        }
        
        function updateVideoProgress() {
            videoPlayer.ontimeupdate = function() {
                const progress = (videoPlayer.currentTime / videoPlayer.duration) * 100;
                document.getElementById('video-progress').style.width = progress + '%';
                document.getElementById('video-current-time').textContent = formatTime(videoPlayer.currentTime);
            };
        }
        
        function previousVideo() {
            if (videoFiles.length === 0) return;
            const newIndex = (currentVideoIndex - 1 + videoFiles.length) % videoFiles.length;
            playVideo(newIndex);
        }
        
        function nextVideo() {
            if (videoFiles.length === 0) return;
            const newIndex = (currentVideoIndex + 1) % videoFiles.length;
            playVideo(newIndex);
        }
        
        document.getElementById('audio-volume').addEventListener('input', function(e) {
            audioPlayer.volume = e.target.value / 100;
        });
        
        document.getElementById('video-volume').addEventListener('input', function(e) {
            videoPlayer.volume = e.target.value / 100;
        });
        
        document.getElementById('music-progress-bar').addEventListener('click', function(e) {
            if (audioPlayer.duration) {
                const rect = this.getBoundingClientRect();
                const percent = (e.clientX - rect.left) / rect.width;
                audioPlayer.currentTime = percent * audioPlayer.duration;
            }
        });
        
        document.getElementById('video-progress-bar').addEventListener('click', function(e) {
            if (videoPlayer.duration) {
                const rect = this.getBoundingClientRect();
                const percent = (e.clientX - rect.left) / rect.width;
                videoPlayer.currentTime = percent * videoPlayer.duration;
            }
        });
        
        function init() {
            renderAudioLibrary();
            renderVideoLibrary();
            showNotification('🚀 App My Music prête ! Importe tes fichiers audio et vidéo.');
        }
        
        init();
    </script>
</body>
</html>
