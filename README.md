# Juego
Juego Breakout en CANVAS

## LECCION 1
Antes de que podamos programar la parte funcional del juego, necesitamos crear la estructura básica de la página y el lienzo que lo va a contener. Podemos hacerlo utilizando HTML y el elemento  canvas.

*En este caso vamomos a definir el documento html con el titulo "Breakout", pero se puede poner el titulo que deseé, igualmete podemos personalizar el color del fondo "background" por ahora lo vamos a poner de color negro #000000 en hexadecimal.*
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Breakout</title>
    <style>* { padding: 0; margin: 0; } canvas { background: #000000; display: block; margin: 0 auto; }</style>
</head>
<body>
<canvas id="myCanvas" width="480" height="320"></canvas>
```
*En esta leccion tambien hemos definido el tamaño del lienzo "canvas" con un ancho de 480 px y un alto de 320px, pero se puede ajustar al tamaño que requiera.*
```
<script>
// El codigo en JavaScript va a ir aqui, ya que canvas es solo el lienzo quien dibuja es javascript por lo que
es necesario llamar al elemento canvas desde el script y definir en este caso el contexto 2d. //

var canvas = document.getElementById("myCanvas");
var ctx = canvas.getContext("2d");
    
</script>

</body>
</html>
```
## LECCION 2
Ahora vamos a dibujar una pelota y a hacer que se mueva. Técnicamente, estaremos pintando la pelota en la pantalla, borrándola y luego pintándola de nuevo en una posición ligeramente diferente, cada fotograma dara la impresión de movimiento, igual que se hace en las películas (De aqui en adelante solo se mostrara el codigo en javascript ya que la parte previa que contiene el codigo en html no la volveremos a editar).
```
<script>
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
```  
*A continuacion se establecen las variables de la pelota, las variables "x" "y" definen donde estara el centro de la pelota y "dx" "dy" definen la magnitud y direccion del movimiento.*
```  
    var x = canvas.width/2;
    var y = canvas.height-30;
    var dx = 2;
    var dy = -2;
```
*Seguido a esto se crea la funcion "drawball" la cual dibujara un circulo utilizando el contexto  "arc", esta funcion tiene en sus dos primeras variables las coordenadas del centro de la circunferencia, el  tercer argumento es el radio de la esfera, y los ultimos dos son los angulos donde inicia y termina el circulo (estos  angulos se deben poner en radianes), ademas de esto se puede definir el contexto "fillStyle" para rellenar  la figura con un color o "strokeStyle" para solo colorear su contorno.*
```    
    function drawBall() {
        ctx.beginPath();
        ctx.arc(x, y, 10, 0, Math.PI*2);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }    
```
*Ahora vamos ha hacer que se dibuje cada 10 milisegundos pero que antes de esto nos limpie el lienzo con el objetivo de crear la animacion, ya que de lo contrario solo trazara una linea con el grosor del radio del circulo.*
```
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawBall();
        x += dx;
        y += dy;
    }    
    setInterval(draw, 10);
</script>
```
## LECCION 3
Continuando con el desarrollo crearemos una funcion para detectar la colisión de la pelota con la pared y si es así, cambiaremos la dirección de su movimiento en consecuencia, se debe definir una variable llamada ballRadius para facilitar los cálculos.
```
<script>
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
    var ballRadius = 10;
    var x = canvas.width/2;
    var y = canvas.height-30;
    var dx = 2;
    var dy = -2;
    function drawBall() {
        ctx.beginPath();
        ctx.arc(x, y, ballRadius, 0, Math.PI*2);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawBall();
```
*Esta es la parte del codigo que añade la deteccion de las colisiones en la funcion "Draw".*
```
        if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
            dx = -dx;
        }
        if(y + dy > canvas.height-ballRadius || y + dy < ballRadius) {
            dy = -dy;
        }
        x += dx;
        y += dy;
    }
    setInterval(draw, 10);
</script>
```
## LECCION 4
Ahora vamos a crear una paleta con la que el usuario podra interactuar con la bola.
```
<script>
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
    var ballRadius = 10;
    var x = canvas.width/2;
    var y = canvas.height-30;
    var dx = 2;
    var dy = -2;
```
*A continuacion se definen las variables de la paleta, paddleHeight y paddleWidth que son las dimenciones ancho y alto de la paleta y paddleX define el lugar donde se va a dibujar en el eje x.*
``` 
    var paddleHeight = 10;
    var paddleWidth = 75;
    var paddleX = (canvas.width-paddleWidth)/2;
```
*Las variables rightPressed y leftPressed guardan informacion cuando se ha pulsado un botón añadiremos tambien dos eventos para detectar cuando se ha pulsado un botón y que asi cambie los valores de las variables rightPressed y leftPressed segun sea el caso.*
```    
    var rightPressed = false;
    var leftPressed = false;
    document.addEventListener("keydown", keyDownHandler, false);
    document.addEventListener("keyup", keyUpHandler, false);

    function keyDownHandler(e) {
        if(e.keyCode == 39) {
            rightPressed = true;
        }
        else if(e.keyCode == 37) {
            leftPressed = true;
        }
    }
    function keyUpHandler(e) {
        if(e.keyCode == 39) {
            rightPressed = false;
        }
        else if(e.keyCode == 37) {
            leftPressed = false;
        }
    }
    function drawBall() {
        ctx.beginPath();
        ctx.arc(x, y, ballRadius, 0, Math.PI*2);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
```
*Esta es la parte del codigo que define la funcion "drawPaddle" la cual dibuja la paleta, esta funcion esta conpuesta por cuatro argumentos, las dos primeras variables son las coordenadas de la esquina superior izquierda del rectangulo, el tercero es el ancho y el cuarto el alto de la paleta.*
```
    function drawPaddle() {
        ctx.beginPath();
        ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawBall();
        drawPaddle();
        if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
            dx = -dx;
        }
        if(y + dy > canvas.height-ballRadius || y + dy < ballRadius) {
            dy = -dy;
        }
```
*Igualmente se debe actualizar el codigo de la función draw para comprobar si está pulsada la flecha izquierda o la derecha cada vez que se dibuje un fotograma.*
```
        if(rightPressed && paddleX < canvas.width-paddleWidth) {
            paddleX += 7;
        }
        else if(leftPressed && paddleX > 0) {
            paddleX -= 7;
        }
        
        x += dx;
        y += dy;
    }
    setInterval(draw, 10);
</script>
```

## LECCION 5
Para implementar el final del juego vamos a escribir el codigo que identifica si se te escapa la bola y alcanza el borde inferior de la pantalla.
```
    <script>
        var canvas = document.getElementById("myCanvas");
        var ctx = canvas.getContext("2d");
        var ballRadius = 10;
        var x = canvas.width/2;
        var y = canvas.height-30;
        var dx = 2;
        var dy = -2;
        var paddleHeight = 10;
        var paddleWidth = 75;
        var paddleX = (canvas.width-paddleWidth)/2;
        var rightPressed = false;
        var leftPressed = false;
        document.addEventListener("keydown", keyDownHandler, false);
        document.addEventListener("keyup", keyUpHandler, false);
        function keyDownHandler(e) {
            if(e.keyCode == 39) {
                rightPressed = true;
            }
            else if(e.keyCode == 37) {
                leftPressed = true;
            }
        }
        function keyUpHandler(e) {
            if(e.keyCode == 39) {
                rightPressed = false;
            }
            else if(e.keyCode == 37) {
                leftPressed = false;
            }
        }
        function drawBall() {
            ctx.beginPath();
            ctx.arc(x, y, ballRadius, 0, Math.PI*2);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }
        function drawPaddle() {
            ctx.beginPath();
            ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBall();
            drawPaddle();
            if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
                dx = -dx;
            }
            if(y + dy < ballRadius) {
                dy = -dy;
            }
            
```
*En esta seccion vamos a permitir que la pelota solo rebote en tres partes: izquierda, arriba y derecha, cuando alcance  la pared inferior debera comprobar si el centro de la bola se encuentra entre los extremos de la paleta, de no ser asi sera el fin del juego y se activara un mensaje de alerta.*
```

            else if(y + dy > canvas.height-ballRadius) {
                if(x > paddleX && x < paddleX + paddleWidth) {
                    dy = -dy;
                }
                else {
                    clearInterval(game);
                    alert("GAME OVER");
                    document.location.reload();
                }
            }
            if(rightPressed && paddleX < canvas.width-paddleWidth) {
                paddleX += 7;
            }
            else if(leftPressed && paddleX > 0) {
                paddleX -= 7;
            }
            x += dx;
            y += dy;
        }
        var game = setInterval(draw, 10);
    </script>
```
## LECCION 6
Ahora vamos a dibujar los ladrillos

```
<script>
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
    var ballRadius = 10;
    var x = canvas.width/2;
    var y = canvas.height-30;
    var dx = 2;
    var dy = -2;
    var paddleHeight = 10;
    var paddleWidth = 75;
    var paddleX = (canvas.width-paddleWidth)/2;
    var rightPressed = false;
    var leftPressed = false;
    
```
*Primero se preparan las variables que contienen la información sobre los ladrillos, como su ancho y alto, filas y columnas.*
```
    var brickRowCount = 5;
    var brickColumnCount = 3;
    var brickWidth = 75;
    var brickHeight = 20;
    var brickPadding = 10;
    var brickOffsetTop = 30;
    var brickOffsetLeft = 30;
    var bricks = [];    
    for(var c=0; c<brickColumnCount; c++) {
        bricks[c] = [];
        for(var r=0; r<brickRowCount; r++) {
            bricks[c][r] = { x: 0, y: 0 };
        }
    }
    
    document.addEventListener("keydown", keyDownHandler, false);
    document.addEventListener("keyup", keyUpHandler, false);
    function keyDownHandler(e) {
        if(e.keyCode == 39) {
            rightPressed = true;
        }
        else if(e.keyCode == 37) {
            leftPressed = true;
        }
    }
    function keyUpHandler(e) {
        if(e.keyCode == 39) {
            rightPressed = false;
        }
        else if(e.keyCode == 37) {
            leftPressed = false;
        }
    }
    function drawBall() {
        ctx.beginPath();
        ctx.arc(x, y, ballRadius, 0, Math.PI*2);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
    function drawPaddle() {
        ctx.beginPath();
        ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
    
```
*Ahora vamos a crear una función para recorrer todos los bloques de la matriz y dibujarlos en la pantalla.*
```
    function drawBricks() {
        for(var c=0; c<brickColumnCount; c++) {
            for(var r=0; r<brickRowCount; r++) {
                var brickX = (r*(brickWidth+brickPadding))+brickOffsetLeft;
                var brickY = (c*(brickHeight+brickPadding))+brickOffsetTop;
                bricks[c][r].x = brickX;
                bricks[c][r].y = brickY;
                ctx.beginPath();
                ctx.rect(brickX, brickY, brickWidth, brickHeight);
                ctx.fillStyle = "#0095DD";
                ctx.fill();
                ctx.closePath();
            }
        }
    }
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawBricks();
        drawBall();
        drawPaddle();
        if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
            dx = -dx;
        }
        if(y + dy < ballRadius) {
            dy = -dy;
        }
        else if(y + dy > canvas.height-ballRadius) {
            if(x > paddleX && x < paddleX + paddleWidth) {
                dy = -dy;
            }
            else {
                alert("GAME OVER");
                document.location.reload();
            }
        }
        if(rightPressed && paddleX < canvas.width-paddleWidth) {
            paddleX += 7;
        }
        else if(leftPressed && paddleX > 0) {
            paddleX -= 7;
        }
        x += dx;
        y += dy;
    }
    setInterval(draw, 10);
</script>
```
## LECCION 7
Lo siguiente sera detectar colisiones para que la bola pueda rebotar en los ladrillos y romperlos.

```<script>
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
    var ballRadius = 10;
    var x = canvas.width/2;
    var y = canvas.height-30;
    var dx = 2;
    var dy = -2;
    var paddleHeight = 10;
    var paddleWidth = 75;
    var paddleX = (canvas.width-paddleWidth)/2;
    var rightPressed = false;
    var leftPressed = false;
    var brickRowCount = 5;
    var brickColumnCount = 3;
    var brickWidth = 75;
    var brickHeight = 20;
    var brickPadding = 10;
    var brickOffsetTop = 30;
    var brickOffsetLeft = 30;
```
*Para empezar añadiremos una propiedad status a cada ladrillo ya que mas adelante esto servira para evaluar si lo tenemos que dibujar o no.*
```
    var bricks = [];
    for(var c=0; c<brickColumnCount; c++) {
        bricks[c] = [];
        for(var r=0; r<brickRowCount; r++) {
            bricks[c][r] = { x: 0, y: 0, status: 1 };
        }
    }
    document.addEventListener("keydown", keyDownHandler, false);
    document.addEventListener("keyup", keyUpHandler, false);
    function keyDownHandler(e) {
        if(e.keyCode == 39) {
            rightPressed = true;
        }
        else if(e.keyCode == 37) {
            leftPressed = true;
        }
    }
    function keyUpHandler(e) {
        if(e.keyCode == 39) {
            rightPressed = false;
        }
        else if(e.keyCode == 37) {
            leftPressed = false;
        }
    }
```
*Antes de dezaparcer los ladrillos cuando se golpean creamos la función que detecta la colision de la pelota con los ladrillos, en un bucle que recorrerá todos los ladrillos y comparará la posición de cada uno con la posición de la bola, cada vez que se dibuje un fotograma, si el centro de la bola está dentro de las coordenadas de uno de los ladrillos, cambiara la dirección de la bola.*
```
    function collisionDetection() {
        for(var c=0; c<brickColumnCount; c++) {
            for(var r=0; r<brickRowCount; r++) {
                var b = bricks[c][r];
                if(b.status == 1) {
                    if(x > b.x && x < b.x+brickWidth && y > b.y && y < b.y+brickHeight) {
                        dy = -dy;
                        b.status = 0;
                    }
                }
            }
        }
    }
    function drawBall() {
        ctx.beginPath();
        ctx.arc(x, y, ballRadius, 0, Math.PI*2);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
    function drawPaddle() {
        ctx.beginPath();
        ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
        ctx.fillStyle = "#0095DD";
        ctx.fill();
        ctx.closePath();
    }
 ```
 *Se actualiza la función drawBricks() para que quede así:*
 ```
    function drawBricks() {
        for(var c=0; c<brickColumnCount; c++) {
            for(var r=0; r<brickRowCount; r++) {
                if(bricks[c][r].status == 1) {
                    var brickX = (r*(brickWidth+brickPadding))+brickOffsetLeft;
                    var brickY = (c*(brickHeight+brickPadding))+brickOffsetTop;
                    bricks[c][r].x = brickX;
                    bricks[c][r].y = brickY;
                    ctx.beginPath();
                    ctx.rect(brickX, brickY, brickWidth, brickHeight);
                    ctx.fillStyle = "#0095DD";
                    ctx.fill();
                    ctx.closePath();
                }
            }
        }
    }
```
*Por ultimo se activar la función de detección de colisiones collisionDetection() llamandola desde la función draw()*
```
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawBricks();
        drawBall();
        drawPaddle();
        collisionDetection();
        if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
            dx = -dx;
        }
        if(y + dy < ballRadius) {
            dy = -dy;
        }
        else if(y + dy > canvas.height-ballRadius) {
            if(x > paddleX && x < paddleX + paddleWidth) {
                dy = -dy;
            }
            else {
                alert("GAME OVER");
                document.location.reload();
            }
        }
        if(rightPressed && paddleX < canvas.width-paddleWidth) {
            paddleX += 7;
        }
        else if(leftPressed && paddleX > 0) {
            paddleX -= 7;
        }
        x += dx;
        y += dy;
    }
    setInterval(draw, 10);
</script>
````
