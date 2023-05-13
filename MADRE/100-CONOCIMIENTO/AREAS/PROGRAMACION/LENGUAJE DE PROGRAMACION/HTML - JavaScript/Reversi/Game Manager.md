```
<!DOCTYPE html>

<html>

    <head>

        <meta charset="UTF-8">

        <title>Reversi</title>

        <script>

            var canvas, context, canvasRect;

            var message, status;

            var playerNo = 0;

            var color = [["black", "黒"], ["white", "白"]];

            var board = new Array(8);

            var cnt = new Array(8), check = new Array(8);

            var x,y;

            var d = [[0,-1], [0,1], [-1,0], [1,0], [-1,-1], [1,-1], [-1,1], [1,1]];

  

            function init() {

                canvas = document.getElementById("game");//obtiene una referencia al elemento canvas del documento HTML y lo asigna a la variable canvas.

                context = canvas.getContext("2d");  //obtiene el contexto de renderizado 2D del canvas y lo asigna a la variable context.

                canvasRect = canvas.getBoundingClientRect(); //obtiene las dimensiones y la posición del canvas en la ventana del navegador y las asigna a la variable canvasRect.

  

                message = document.getElementById("message"); // obtiene una referencia al elemento HTML con ID "message" y lo asigna a la variable message.

  

                for (var i = 0; i < 8; i++) {

                    board[i] = [-1,-1,-1,-1,-1,-1,-1,-1];

                }

                drawBoard();

            }

  

            function drawBoard() { // dibuja el tablero y las fichas.

                context.fillStyle = "#006600";// Establece el color de fondo del canvas

                context.fillRect(0, 0, canvas.width, canvas.height); // Rellena el fondo con el color establecido

  

                context.beginPath();  // Inicia una nueva ruta de dibujo para las líneas del tablero

                for (var i = 0; i < 9; i++) {   // Dibuja las líneas horizontales y verticales del tablero

                    context.moveTo(i * 80, 0);

                    context.lineTo(i * 80, 640);

                    context.moveTo(0, i * 80);

                    context.lineTo(640, i * 80);

                }

                context.stroke();// Dibuja las líneas del tablero

                for (var x = 0; x < 8; x++) { // Dibuja las fichas en el tablero

                    for (var y = 0; y < 8; y++) {

                        if (board[x][y] >= 0) {

                            context.fillStyle = color[board[x][y]][0];

                            context.beginPath();

                            context.arc(40 + x * 80, 40 + y * 80, 30, 0, 2 * Math.PI); //80px entre cada posicion

                            context.fill();

                            context.stroke();

                        }

                    }

                }

            }

            function startGame() {

                for (var i = 0; i < 8; i++) {

                    board[i] = [-1, -1, -1, -1, -1, -1, -1, -1]; // El valor -1 representa una celda vacía en el tablero.

                }

                board[3][4] = 0;//Los valores 0 y 1 representan fichas negras y blancas respectivamente.

                board[4][3] = 0;

                board[3][3] = 1;

                board[4][4] = 1;

  

                drawBoard();

                status = "start";//para controlar el flujo del juego.

                playerNo = 0; //para llevar la cuenta del jugador que tiene el turno.

                message.innerText = "黒の石を置いてください。";

            }

  

            function putDiscPlayer(event)//se encarga de recibir la entrada del usuario al hacer clic en una casilla del tablero

            {                            //parámetro event es un objeto que contiene información sobre el evento de clic, como la posición del ratón en la pantalla.

                var x = Math.floor((event.clientX - canvasRect.left) / 80); // calcula la columna del tablero en la que se hizo clic dividiendo la posición horizontal del ratón por el ancho de cada celda del tablero y redondeando hacia abajo el resultado.

                var y = Math.floor((event.clientY - canvasRect.top) / 80); //calcula la fila del tablero en la que se hizo clic dividiendo la posición vertical del ratón por el alto de cada celda del tablero y redondeando hacia abajo el resultado.

                putDisc(x,y,0); //se encarga de colocar una ficha en la posición especificada del tablero y cambiar el turno al siguiente jugador.

            }

  

            function putDisc(x,y,pNo)

            {

  

                if(checkDisc(x,y) && (playerNo==pNo)) //comprueba que la casilla seleccionada por el jugador es válida, es decir, está vacía y es adyacente a una ficha del jugador opuesto.

                {                                     // comprueba que el jugador que intenta colocar una ficha es el jugador actual.  

                    board[x][y] = playerNo;// coloca la ficha del jugador actual en la casilla seleccionada

  

                    for(var i=0; i<8; i++)

                    {

                        if(cnt[i]>0) flipDisc(x+d[i][0], y+d[i][1],i); //Si hay fichas del jugador opuesto en esa dirección, se llamará a la función flipDisc para capturar esas fichas.

                    }

                    drawBoard();

                    changePlayer();

                }

            }

  

            function checkDisc(x,y)

            {

                for( var i=0; i<8; i++)

                {

                    cnt[i]=0;//Estas dos líneas establecen los valores de las matrices cnt y check en cero para el índice actual i.

                    check[i]=0;

                    if(board[x][y] == -1) checkDirDisc(x+d[i][0], y+d[i][1], i); //Si la posición actual de la matriz board en las coordenadas (x, y) es igual a -1, llama a una función llamada checkDirDisc y le pasa tres parámetros: la posición en la coordenada x desplazada por d[i][0], la posición en la coordenada y desplazada por d[i][1], y el índice actual i.

                    if(check[i]== 0) cnt[i]=0; //establece el valor de cnt[i] en cero si el valor de check[i] es cero.

                }

                var ret = false; // variable booleana ret y la inicializa en false.

                if (Math.max.apply(null, cnt) > 0) ret = true; //Comprueba si el valor máximo en la matriz cnt es mayor que cero. Si es así, establece el valor de ret en true.

                return ret; //Si es true, significa que se encontró al menos una ficha del oponente en una dirección determinada, de lo contrario, devuelve false.

            }

  

            function checkDirDisc(x,y,dir)

            {

                if((x>=0) && (x<8) && (y>=0) && (y<8)) //Comprueba si las coordenadas x e y están dentro del rango de la matriz board.

                {

                    if(board[x][y] == playerNo) //Si la casilla de la matriz board en las coordenadas (x, y) es igual al número del jugador actual playerNo, establece el valor de la matriz check en la dirección actual dir en 1.

                    {

                        check[dir] =1;

                    }

                    else if(board[x][y] == 1-playerNo) //oponente

                    {

                        cnt[dir]++; //incrementa el contador en la dirección actual dir en 1

                        checkDirDisc(x+d[dir][0], y+d[dir][1],dir); // llama a la función checkDirDisc de forma recursiva con las coordenadas desplazadas por d[dir][0] y d[dir][1] y la misma dirección dir. Esto se hace para buscar en la dirección actual hasta que se encuentra una ficha del jugador actual o hasta que se alcanza el borde del tablero.

                    }

                }

            }

            function flipDisc(x,y,dir)

            {

                if(board[x][y]== 1-playerNo)//Comprueba si la casilla de la matriz board en las coordenadas (x, y) es igual al número del oponente.

                {

                    board[x][y] = playerNo;//Si es así, establece la casilla en las coordenadas (x, y) en la matriz board en el número del jugador actual playerNo.

                    flipDisc(x+d[dir][0], y+d[dir][1],dir); // llama a la función flipDisc de forma recursiva con las coordenadas desplazadas por d[dir][0] y d[dir][1] y la misma dirección dir. Esto se hace para voltear todas las fichas del oponente en la dirección actual dir.

                }

            }

  

            function checkNoPlace()

            {

                var ret = true;

                for ( var x=0; x<8; x++)//Recorre todas las posibles posiciones en la matriz board y verifica si se puede colocar una ficha en esa posición llamando a la función checkDisc para cada posición. Si checkDisc devuelve true, establece ret en false, lo que significa que todavía hay una posición disponible para colocar una ficha.

                {

                    for (var y=0; y<8; y++)

                    {

                        if(checkDisc(x,y)) ret = false;

                    }

                }

                return ret;//Devuelve el valor de ret, que será true si no hay más posiciones para colocar una ficha y false si hay al menos una posición disponible.

            }

  

            function changePlayer()

            {

                playerNo= 1-playerNo; //Establece el valor de la variable playerNo al valor opuesto.

                if(checkNoPlace()) // comprueban si el jugador actual puede colocar un disco

                    {

                        playerNo= 1-playerNo;// si el jugador actual puede colocar un disco

                        if(checkNoPlace()) status="end";//si el otro jugador también no puede colocar un disco, se establece el estado del juego en "end"

                    }

                    message.innerText = color[playerNo][1]+"の石を置いてください。"; //actualiza el mensaje de estado para indicar de quién es el turno.

                    if(status== "end")//verifican si el juego ha terminado

                        {

                        var black = 0, white = 0;

                        for(var x=0; x<8;x++)

                            {

                                for(var y=0; y<8; y++) // muestran los resultados en el mensaje de estado.

                                {

                                    if (board[x][y] == 0) black++;

                                    if (board[x][y] == 1) white++;

                                }  

                            }

                            message.innerText = "終了! 黒: "+black+" 白: "+white;

                       }

                       else if (playerNo == 1) //turno al otro jugador

                       {

                        setTimeout(putDiscCom, 1000);//temporizador para que el ordenador coloque un disco si es el turno del ordenador.

                       }

            }

  

            function putDiscCom()//implementa la lógica para que la computadora (jugador 1) pueda seleccionar la mejor posición para colocar una ficha en el tablero.

            {

                var mx, my, max = 0;// para almacenar las coordenadas de la mejor posición para colocar la ficha

                for (var x=0; x<8; x++)//se recorre el tablero y se evalúa cada posición en busca de la mejor opción

                {

                    for(var y=0; y<8; y++)

                    {

                        if(checkDisc(x,y))//Si la posición actual es válida (puede ser usada para colocar una ficha) se calcula una puntuación para esa posición. La puntuación se determina contando cuántas fichas se pueden voltear si se coloca una ficha en la posición actual y sumándole bonificaciones si la posición está en los bordes o en las esquinas del tablero.

                        {

                            var total = 0, bonus = 5;

                            for (var i =0; i<8; i++)

                            {

                                total += cnt[i];

                            }

                            if ((x==0)&&(y!=1)&&(y!=6)) bonus+=5;//

                            if ((x==7)&&(y!=1)&&(y!=6)) bonus+=5;

                            if ((y==0)&&(x!=1)&&(x!=6)) bonus+=5;

                            if ((y==7)&&(x!=1)&&(x!=6)) bonus+=5;

                            if ((x==1)&&((y==1)||(y==6))) bonus-=5;

                            if ((x==6)&&((y==1)||(y==6))) bonus-=5;

  

                            if(max < total + bonus)//se comparan las puntuaciones de todas las posiciones evaluadas y se guarda la mejor puntuación y sus coordenadas en las variables max, mx, y my.

                            {

                                max = total + bonus;

                                mx = x;

                                my = y;

                            }

                        }

                    }

                }

                putDisc(mx,my,1);//para colocar una ficha en la posición determinada. En este caso, la ficha será colocada por la computadora, y por lo tanto, se usa 1 como segundo parámetro para indicar que la ficha es del jugador 1.

            }

        </script>

    </head>

    <body onload="init()">

        <p>リバーシ</p>

  

        <input type="button" id="start" value="スタート" onclick="startGame()">

        <span id="message"></span>

        <hr>

  

        <canvas id="game" width="640" height="640" onmousedown="putDiscPlayer(event)">

        </canvas>

    </body>

</html>
```