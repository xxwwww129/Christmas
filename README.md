# Christmas
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>程序员的圣诞浪漫</title>
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/build/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/examples/js/controls/OrbitControls.js"></script>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: #000;
        }
        .info {
            position: absolute;
            top: 10px;
            left: 10px;
            color: #fff;
            font-family: Arial, sans-serif;
        }
    </style>
</head>
<body>
    <div class="info">Merry Christmas</div>
    <script>
        // 初始化场景、相机、渲染器
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // 轨道控制器
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;

        // 生成粒子圣诞树
        const particleCount = 8000;
        const geometry = new THREE.BufferGeometry();
        const positions = new Float32Array(particleCount * 3);
        const colors = new Float32Array(particleCount * 3);

        // 圣诞树形状参数
        const treeHeight = 3;
        const baseRadius = 3;
        const topRadius = 0.5;
        const segments = 32;

        // 生成圣诞树粒子
        for (let i = 0; i < particleCount; i++) {
            const t = Math.random();
            const height = t * treeHeight;
            const radius = baseRadius - (baseRadius - topRadius) * (height / treeHeight);
            const angle = Math.random() * Math.PI * 2;

            positions[i * 3] = Math.cos(angle) * radius * Math.random();
            positions[i * 3 + 1] = height - treeHeight / 2;
            positions[i * 3 + 2] = Math.sin(angle) * radius * Math.random();

            // 颜色渐变，顶部偏粉，底部偏白
            const colorFactor = (height / treeHeight) * 0.7 + 0.3;
            colors[i * 3] = 1;
            colors[i * 3 + 1] = colorFactor;
            colors[i * 3 + 2] = colorFactor;
        }

        geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
        geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));

        const material = new THREE.PointsMaterial({
            size: 0.05,
            vertexColors: true,
            transparent: true,
            opacity: 0.8
        });

        const tree = new THREE.Points(geometry, material);
        scene.add(tree);

        // 星星背景
        const starGeometry = new THREE.BufferGeometry();
        const starPositions = new Float32Array(2000 * 3);
        for (let i = 0; i < 2000; i++) {
            starPositions[i * 3] = (Math.random() - 0.5) * 20;
            starPositions[i * 3 + 1] = (Math.random() - 0.5) * 20;
            starPositions[i * 3 + 2] = (Math.random() - 0.5) * 20 + 5;
        }
        starGeometry.setAttribute('position', new THREE.BufferAttribute(starPositions, 3));
        const starMaterial = new THREE.PointsMaterial({
            size: 0.1,
            color: 0xffffff,
            transparent: true,
            opacity: Math.random() * 0.5 + 0.5
        });
        const stars = new THREE.Points(starGeometry, starMaterial);
        scene.add(stars);

        // 圣诞树顶部装饰
        const topGeometry = new THREE.SphereGeometry(0.1, 16, 16);
        const topMaterial = new THREE.MeshBasicMaterial({ color: 0xff3399 });
        const top = new THREE.Mesh(topGeometry, topMaterial);
        top.position.y = treeHeight / 2 - 0.1;
        scene.add(top);

        // 地面圆环
        const ringGeometry = new THREE.RingGeometry(1, 3, 64);
        const ringMaterial = new THREE.MeshBasicMaterial({
            color: 0xffffff,
            transparent: true,
            opacity: 0.2
        });
        const ring = new THREE.Mesh(ringGeometry, ringMaterial);
        ring.rotation.x = -Math.PI / 2;
        scene.add(ring);

        // 动画循环
        function animate() {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        }
        animate();

        // 窗口大小变化时调整
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
