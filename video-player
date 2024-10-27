<div id="fullscreen-container" onclick="toggleFullScreen()">
    <!-- Adicionando evento de clique no container -->
    <div id="status-indicator"></div> <!-- Indicador de status -->
    <video id="meuVideo" playsinline preload="auto"></video> <!-- Removido o atributo controls -->
    <div id="button-container">
        <button id="playButton" class="button">Play</button>
        <a href="https://www.africamono.it.ao/p/live-channel-2424.html" class="button">Live Channels</a>
    </div>
</div>

<style>
    #fullscreen-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center; /* Centraliza o conteúdo verticalmente */
        width: 100%;
        max-width: 100%;
        padding: 5px;
        border-radius: 8px;
        box-shadow: 0 0px 0px rgba(0, 0, 0, 0.3);
        position: relative;
    }

    video {
        width: 100%;
        height: auto; /* Mantém a altura automática */
        aspect-ratio: 16 / 9; /* Define a proporção 16:9 */
        background-color: #000;
        object-fit: cover; /* Faz o vídeo preencher a área sem distorção */
    }

    #button-container {
        margin-top: 8px;
        width: 100%;
        display: flex;
        justify-content: center; /* Centraliza os botões horizontalmente */
        flex-wrap: wrap; /* Permite que os botões quebrem para uma nova linha se necessário */
    }

    .button {
        padding: 5px 20px;
        font-size: 16px;
        cursor: pointer;
        border: none;
        border-radius: 5px; /* Arredondamento dos botões */
        background-color: #ff4c4c;
        color: white;
        transition: background-color 0.3s;
        margin: 5px; /* Espaçamento entre os botões */
    }

    .button:hover {
        background-color: #e43e3e;
    }

    @media (max-width: 480px) {
        video {
            max-height: 30vh;
        }
        .button {
            font-size: 14px;
            padding: 8px 16px;
        }
    }

    #status-indicator {
        position: absolute;
        top: -30px;
        left: 10px;
        font-size: 14px;
        font-style: italic;
        font-weight: normal;
        display: none;
    }
</style>

<script>
    const videoSources = [
        'https://zggg98sgwbg1gh.bitchute.com/OkD3fEQ7n2eI/WJZQIxw4r9bn.mp4',
        'https://seed305.bitchute.com/OkD3fEQ7n2eI/M0tlZveG084G.mp4',
        'https://seed122.bitchute.com/OkD3fEQ7n2eI/YMNuSONlLHMC.mp4'
    ];

    const videoDurations = [2000, 2000, 2000]; // Durações em segundos para cada vídeo
    const videoPlayer = document.getElementById('meuVideo');
    const playButton = document.getElementById('playButton');
    const statusIndicator = document.getElementById('status-indicator');
    const streamingUrl = 'https://edge1-us-newyork.picarto.tv/stream/golive%2bafricamono.mp4';

    let isStreaming = false;
    let currentIndex = 0;

    function getSyncedVideoAndTime() {
        const now = Math.floor(Date.now() / 1000);
        const totalDuration = videoDurations.reduce((a, b) => a + b, 0);
        const syncedTime = now % totalDuration;

        let accumulatedTime = 0;
        for (let i = 0; i < videoSources.length; i++) {
            if (syncedTime < accumulatedTime + videoDurations[i]) {
                return { index: i, startTime: syncedTime - accumulatedTime };
            }
            accumulatedTime += videoDurations[i];
        }
    }

    function changeVideo(index, startTime) {
        videoPlayer.src = videoSources[index];
        videoPlayer.currentTime = startTime;
        videoPlayer.play().catch(error => {
            console.error('Error playing video:', error);
        });
        updateStatusIndicator(false);
    }

    function autoplayNextVideo() {
        currentIndex = (currentIndex + 1) % videoSources.length;
        const { startTime } = getSyncedVideoAndTime();
        changeVideo(currentIndex, startTime);
    }

    videoPlayer.addEventListener('ended', autoplayNextVideo);

    window.addEventListener('load', function () {
        const { index, startTime } = getSyncedVideoAndTime();
        changeVideo(index, startTime);
    });

    function startStreaming() {
        if (!isStreaming) {
            videoPlayer.pause();
            videoPlayer.src = streamingUrl;
            videoPlayer.play().catch(error => {
                console.error('Error playing stream:', error);
            });
            isStreaming = true;
            updateStatusIndicator(true);
        }
    }

    function stopStreaming() {
        if (isStreaming) {
            const { index, startTime } = getSyncedVideoAndTime();
            videoPlayer.pause();
            changeVideo(index, startTime);
            isStreaming = false;
            updateStatusIndicator(false);
        }
    }

    function updateStatusIndicator(isLive) {
        statusIndicator.style.display = 'block';
        statusIndicator.textContent = isLive ? 'Live' : 'Recorded Programs';
    }

    function checkStreaming() {
        const xhr = new XMLHttpRequest();
        xhr.open('HEAD', streamingUrl, true);
        xhr.onload = function () {
            if (xhr.status >= 200 && xhr.status < 400) {
                startStreaming();
            } else {
                stopStreaming();
            }
        };
        xhr.onerror = function () {
            stopStreaming();
        };
        xhr.send();
    }

    setInterval(checkStreaming, 1000);

    playButton.addEventListener('click', function () {
        if (videoPlayer.paused) {
            videoPlayer.play().then(() => {
                toggleFullScreen();
            }).catch(error => {
                console.error('Error starting video:', error);
            });
        }
    });

    videoPlayer.addEventListener('dblclick', function () {
        toggleFullScreen();
    });

    function toggleFullScreen() {
        if (!document.fullscreenElement) {
            document.getElementById('fullscreen-container').requestFullscreen().catch(err => {
                console.error(`Erro ao habilitar tela cheia: ${err.message}`);
            });
        } else {
            document.exitFullscreen();
        }
    }
</script>
