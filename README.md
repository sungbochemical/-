<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>아미노산 이성질체 뷰어</title>
    <style>
        body { font-family: sans-serif; }
        #search-container { margin-bottom: 20px; }
        #search-input { padding: 10px; width: 300px; }
        #search-button { padding: 10px 20px; }
        #model-container { width: 400px; height: 300px; border: 1px solid #ccc; }
    </style>
</head>
<body>
    <h1>아미노산 이성질체 뷰어</h1>

    <div id="search-container">
        <input type="text" id="search-input" placeholder="원하는 아미노산을 검색하세요">
        <button id="search-button">검색</button>
    </div>

    <div id="model-container">
        </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const searchButton = document.getElementById('search-button');
            const searchInput = document.getElementById('search-input');
            const modelContainer = document.getElementById('model-container');
            let scene, camera, renderer;

            function init3D() {
                scene = new THREE.Scene();
                camera = new THREE.PerspectiveCamera(75, modelContainer.offsetWidth / modelContainer.offsetHeight, 0.1, 1000);
                renderer = new THREE.WebGLRenderer();
                renderer.setSize(modelContainer.offsetWidth, modelContainer.offsetHeight);
                modelContainer.appendChild(renderer.domElement);
                camera.position.z = 5;

                const ambientLight = new THREE.AmbientLight(0x404040);
                scene.add(ambientLight);
                const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
                directionalLight.position.set(0, 1, 1).normalize();
                scene.add(directionalLight);

                animate();
            }

            function loadAndDisplayModel(aminoAcid) {
                // 여기에서 aminoAcid에 해당하는 3D 모델 데이터를 로드하고 scene에 추가하는 로직을 구현해야 합니다.
                // 예를 들어, 서버에서 JSON 형태의 모델 데이터를 받아와 three.js의 Geometry, Material 등을 사용하여 모델을 만들 수 있습니다.

                // 임시로 간단한 구체를 표시합니다.
                const geometry = new THREE.SphereGeometry(1, 32, 32);
                const material = new THREE.MeshPhongMaterial({ color: 0x00ff00 });
                const sphere = new THREE.Mesh(geometry, material);
                scene.add(sphere);
            }

            function animate() {
                requestAnimationFrame(animate);
                // 모델에 애니메이션 효과를 줄 수도 있습니다. (예: 회전)
                // if (scene.children.length > 1) { // AmbientLight, DirectionalLight 제외
                //     scene.children[2].rotation.x += 0.01;
                //     scene.children[2].rotation.y += 0.01;
                // }
                renderer.render(scene, camera);
            }

            searchButton.addEventListener('click', () => {
                const searchTerm = searchInput.value.trim();
                if (searchTerm) {
                    // 기존 모델 제거
                    while (scene.children.length > 0) {
                        scene.remove(scene.children[0]);
                    }
                    // 조명 다시 추가
                    const ambientLight = new THREE.AmbientLight(0x404040);
                    scene.add(ambientLight);
                    const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
                    directionalLight.position.set(0, 1, 1).normalize();
                    scene.add(directionalLight);

                    loadAndDisplayModel(searchTerm);
                } else {
                    alert('검색어를 입력해주세요.');
                }
            });

            init3D(); // 페이지 로딩 시 3D 환경 초기화
        });
    </script>
</body>
</html>
