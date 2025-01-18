car drift 2.0
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Drift Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            font-family: Arial, sans-serif;
        }
        canvas {
            display: block;
        }
        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div id="score">Score: 0</div>
    <canvas id="gameCanvas"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        // Game variables
        let score = 0;
        const car = {
            x: canvas.width / 2,
            y: canvas.height - 100,
            width: 50,
            height: 100,
            angle: 0,
            speed: 0,
            maxSpeed: 5,
            friction: 0.95,
            turnSpeed: 0.07,
            driftFactor: 0.85,
        };

        const keys = {
            ArrowUp: false,
            ArrowDown: false,
            ArrowLeft: false,
            ArrowRight: false,
        };

        window.addEventListener("keydown", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = true;
        });

        window.addEventListener("keyup", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = false;
        });

        function drawTrack() {
            ctx.fillStyle = "gray";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            ctx.strokeStyle = "white";
            ctx.lineWidth = 5;
            ctx.setLineDash([20, 10]);
            ctx.beginPath();
            ctx.arc(canvas.width / 2, canvas.height / 2, 200, 0, Math.PI * 2);
            ctx.stroke();
        }

        function drawCar() {
            ctx.save();
            ctx.translate(car.x, car.y);
            ctx.rotate(car.angle);
            ctx.fillStyle = "red";
            ctx.fillRect(-car.width / 2, -car.height / 2, car.width, car.height);
            ctx.restore();
        }

        function updateCar() {
            if (keys.ArrowUp) {
                car.speed = Math.min(car.speed + 0.2, car.maxSpeed);
            } else if (keys.ArrowDown) {
                car.speed = Math.max(car.speed - 0.2, -car.maxSpeed / 2);
            } else {
                car.speed *= car.friction;
            }

            if (keys.ArrowLeft) {
                car.angle -= car.turnSpeed * (car.speed / car.maxSpeed);
            }

            if (keys.ArrowRight) {
                car.angle += car.turnSpeed * (car.speed / car.maxSpeed);
            }

            const driftX = Math.sin(car.angle) * car.speed * car.driftFactor;
            const driftY = -Math.cos(car.angle) * car.speed * car.driftFactor;

            car.x += driftX;
            car.y += driftY;

            if (car.x < 0 || car.x > canvas.width || car.y < 0 || car.y > canvas.height) {
                car.x = canvas.width / 2;
                car.y = canvas.height - 100;
                car.speed = 0;
                score = 0;
            }
        }

        function updateScore() {
            score += Math.abs(car.speed);
            document.getElementById("score").textContent = `Score: ${Math.floor(score)}`;
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawTrack();
            drawCar();
            updateCar();
            updateScore();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Drift Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            font-family: Arial, sans-serif;
        }
        canvas {
            display: block;
        }
        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-size: 18px;
        }
        #trackSelector {
            position: absolute;
            top: 10px;
            right: 10px;
            z-index: 10;
        }
    </style>
</head>
<body>
    <div id="score">Score: 0</div>
    <select id="trackSelector">
        <option value="track1">Track 1: Circular</option>
        <option value="track2">Track 2: Zigzag</option>
        <option value="track3">Track 3: Figure Eight</option>
        <option value="track4">Track 4: Spiral</option>
        <option value="track5">Track 5: Square Loop</option>
    </select>
    <canvas id="gameCanvas"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        // Game variables
        let score = 0;
        const car = {
            x: canvas.width / 2,
            y: canvas.height - 100,
            width: 50,
            height: 100,
            angle: 0,
            speed: 0,
            maxSpeed: 5,
            friction: 0.95,
            turnSpeed: 0.07,
            driftFactor: 0.85,
        };

        const keys = {
            ArrowUp: false,
            ArrowDown: false,
            ArrowLeft: false,
            ArrowRight: false,
        };

        let currentTrack = "track1";

        const tracks = {
            track1: {
                color: "gray",
                draw: function () {
                    ctx.beginPath();
                    ctx.arc(canvas.width / 2, canvas.height / 2, 200, 0, Math.PI * 2);
                    ctx.strokeStyle = "white";
                    ctx.lineWidth = 5;
                    ctx.stroke();
                },
            },
            track2: {
                color: "brown",
                draw: function () {
                    ctx.strokeStyle = "white";
                    ctx.lineWidth = 5;
                    ctx.beginPath();
                    ctx.moveTo(100, canvas.height - 100);
                    ctx.lineTo(canvas.width / 2, 100);
                    ctx.lineTo(canvas.width - 100, canvas.height - 100);
                    ctx.stroke();
                },
            },
            track3: {
                color: "black",
                draw: function () {
                    ctx.strokeStyle = "white";
                    ctx.lineWidth = 5;
                    ctx.beginPath();
                    ctx.moveTo(canvas.width / 4, canvas.height - 100);
                    ctx.lineTo(canvas.width / 2, canvas.height / 2);
                    ctx.lineTo((canvas.width * 3) / 4, canvas.height - 100);
                    ctx.moveTo(canvas.width / 2, canvas.height / 2);
                    ctx.lineTo(canvas.width / 2, 100);
                    ctx.stroke();
                },
            },
            track4: {
                color: "darkgreen",
                draw: function () {
                    ctx.strokeStyle = "white";
                    ctx.lineWidth = 5;
                    ctx.beginPath();
                    ctx.moveTo(canvas.width / 2, canvas.height / 2);
                    for (let i = 0; i < 6; i++) {
                        const radius = 50 + i * 30;
                        ctx.arc(canvas.width / 2, canvas.height / 2, radius, 0, Math.PI * 1.5, i % 2 === 0);
                    }
                    ctx.stroke();
                },
            },
            track5: {
                color: "blue",
                draw: function () {
                    ctx.strokeStyle = "white";
                    ctx.lineWidth = 5;
                    ctx.beginPath();
                    ctx.rect(canvas.width / 4, canvas.height / 4, canvas.width / 2, canvas.height / 2);
                    ctx.stroke();
                },
            },
        };

        document.getElementById("trackSelector").addEventListener("change", (e) => {
            currentTrack = e.target.value;
        });

        window.addEventListener("keydown", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = true;
        });

        window.addEventListener("keyup", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = false;
        });

        function drawTrack() {
            ctx.fillStyle = tracks[currentTrack].color;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            tracks[currentTrack].draw();
        }

        function drawCar() {
            ctx.save();
            ctx.translate(car.x, car.y);
            ctx.rotate(car.angle);
            ctx.fillStyle = "red";
            ctx.fillRect(-car.width / 2, -car.height / 2, car.width, car.height);
            ctx.restore();
        }

        function updateCar() {
            if (keys.ArrowUp) {
                car.speed = Math.min(car.speed + 0.2, car.maxSpeed);
            } else if (keys.ArrowDown) {
                car.speed = Math.max(car.speed - 0.2, -car.maxSpeed / 2);
            } else {
                car.speed *= car.friction;
            }

            if (keys.ArrowLeft) {
                car.angle -= car.turnSpeed * (car.speed / car.maxSpeed);
            }

            if (keys.ArrowRight) {
                car.angle += car.turnSpeed * (car.speed / car.maxSpeed);
            }

            const driftX = Math.sin(car.angle) * car.speed * car.driftFactor;
            const driftY = -Math.cos(car.angle) * car.speed * car.driftFactor;

            car.x += driftX;
            car.y += driftY;

            if (car.x < 0 || car.x > canvas.width || car.y < 0 || car.y > canvas.height) {
                car.x = canvas.width / 2;
                car.y = canvas.height - 100;
                car.speed = 0;
                score = 0;
            }
        }

        function updateScore() {
            score += Math.abs(car.speed);
            document.getElementById("score").textContent = `Score: ${Math.floor(score)}`;
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawTrack();
            drawCar();
            updateCar();
            updateScore();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    </script>
</body>
</html>
